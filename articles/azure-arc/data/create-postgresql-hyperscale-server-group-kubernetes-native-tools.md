---
title: Criar um grupo de servidores de hiperescala PostgreSQL usando ferramentas de kubernetes
description: Criar um grupo de servidores de hiperescala PostgreSQL usando ferramentas de kubernetes
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: twright-msft
ms.author: twright
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
ms.openlocfilehash: bbf41cf48f4891814fa0c2baa750783f98d8574b
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91341223"
---
# <a name="create-a-postgresql-hyperscale-server-group-using-kubernetes-tools"></a>Criar um grupo de servidores de hiperescala PostgreSQL usando ferramentas de kubernetes

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="prerequisites"></a>Pré-requisitos

Você já deve ter criado um [controlador de dados de arco do Azure](./create-data-controller.md).

Para criar um grupo de servidores de hiperescala PostgreSQL usando as ferramentas do kubernetes, você precisará ter as ferramentas do kubernetes instaladas.  Os exemplos neste artigo usarão `kubectl` , mas abordagens semelhantes podem ser usadas com outras ferramentas de kubernetes como, por exemplo, o painel do kubernetes, `oc` ou `helm` se você estiver familiarizado com essas ferramentas e kubernetes YAML/JSON.

[Instalar a ferramenta kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)

## <a name="overview"></a>Visão geral

Para criar um grupo de servidores de hiperescala PostgreSQL, você precisa criar um segredo kubernetes para armazenar o logon e a senha do administrador do postgres com segurança e um recurso personalizado do grupo de servidores de hiperescala do PostgreSQL com base nas definições de recurso personalizado PostgreSQL-12 ou PostgreSQL-11.

## <a name="create-a-yaml-file"></a>Criar um arquivo YAML

Você pode usar o arquivo [YAML do modelo](https://raw.githubusercontent.com/microsoft/azure_arc/master/arc_data_services/deploy/yaml/postgresql.yaml) como um ponto de partida para criar seu próprio arquivo YAML do grupo de servidores de hiperescala do PostgreSQL personalizado.  Baixe esse arquivo em seu computador local e abra-o em um editor de texto.  É útil usar um editor de texto como [vs Code](https://code.visualstudio.com/download) que dão suporte a realce e reutilização de sintaxe para arquivos YAML.

Este é um exemplo de arquivo YAML:

```yaml
apiVersion: v1
data:
  password: <your base64 encoded password>
kind: Secret
metadata:
  name: example-login-secret
type: Opaque
---
apiVersion: arcdata.microsoft.com/v1alpha1
kind: postgresql-12
metadata:
  generation: 1
  name: example
spec:
  engine:
    extensions:
    - name: citus
  scale:
    shards: 3
  scheduling:
    default:
      resources:
        limits:
          cpu: "4"
          memory: 4Gi
        requests:
          cpu: "1"
          memory: 2Gi
  service:
    type: LoadBalancer
  storage:
    backups:
      className: default
      size: 5Gi
    data:
      className: default
      size: 5Gi
    logs:
      className: default
      size: 1Gi
```

### <a name="customizing-the-login-and-password"></a>Personalizando o logon e a senha.
Um segredo kubernetes é armazenado como uma cadeia de caracteres codificada em base64-um para o nome de usuário e outro para a senha.  Você precisará codificar em base64 um logon e uma senha de administrador e colocá-los no local do espaço reservado em `data.password` e `data.username` .  Não inclua os `<` símbolos e `>` fornecidos no modelo.

Você pode usar uma ferramenta online para codificar o nome de usuário e a senha desejados ou pode usar ferramentas de CLI internas, dependendo da sua plataforma.

PowerShell

```console
[Convert]::ToBase64String([System.Text.Encoding]::Unicode.GetBytes('<your string to encode here>'))

#Example
#[Convert]::ToBase64String([System.Text.Encoding]::Unicode.GetBytes('example'))

```

Linux/macOS

```console
echo '<your string to encode here>' | base64

#Example
# echo 'example' | base64
```

### <a name="customizing-the-name"></a>Personalizando o nome

O modelo tem um valor de ' example ' para o atributo Name.  Você pode alterar isso, mas deve ser caracteres que sigam os padrões de nomenclatura do DNS.  Você também deve alterar o nome do segredo para corresponder.  Por exemplo, se você alterar o nome do grupo de servidores de hiperescala PostgreSQL para ' postgres1 ', deverá alterar o nome do segredo de ' example-logon-Secret ' para ' postgres1-login-Secret '

### <a name="customizing-the-engine-version"></a>Personalizando a versão do mecanismo

Você pode alterar a versão do mecanismo para ser PostgreSQL-11 ou PostgreSQL-12 editando o `kind` atributo.

### <a name="customizing-the-resource-requirements"></a>Personalizando os requisitos de recursos

Você pode alterar os requisitos de recursos-os limites de RAM e de núcleo e as solicitações-conforme necessário.  

> [!NOTE]
> Você pode aprender mais sobre a [governança de recursos do kubernetes](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/#resource-units-in-kubernetes).

Requisitos para limites e solicitações de recursos:
- O valor do limite de núcleos é **necessário** para fins de cobrança.
- O restante das solicitações e dos limites de recursos são opcionais.
- O limite e a solicitação de núcleos devem ser um valor inteiro positivo, se especificado.
- O mínimo de 1 núcleo é necessário para a solicitação de núcleos, se especificado.
- O formato do valor da memória segue a notação kubernetes.  

### <a name="customizing-service-type"></a>Personalizando o tipo de serviço

O tipo de serviço pode ser alterado para NodePort, se desejado.  Um número de porta aleatório será atribuído.

### <a name="customizing-storage"></a>Personalizando o armazenamento

Você pode personalizar as classes de armazenamento para armazenamento para corresponder ao seu ambiente.  Se você não tiver certeza de quais classes de armazenamento estão disponíveis, poderá executar o comando `kubectl get storageclass` para exibi-las.  O modelo tem um valor padrão de ' default '.  Isso significa que há uma classe de armazenamento _chamada_ ' default ', não que haja uma classe de armazenamento que _seja_ o padrão.  Opcionalmente, você também pode alterar o tamanho do seu armazenamento.  Você pode ler mais sobre a [configuração de armazenamento](./storage-configuration.md).

## <a name="creating-the-postgresql-hyperscale-server-group"></a>Criando o grupo de servidores de hiperescala PostgreSQL

Agora que você personalizou o arquivo de YAML do grupo de servidores de hiperescala do PostgreSQL, você pode criar o grupo de servidores de hiperescala PostgreSQL executando o seguinte comando:

```console
kubectl create -n <your target namespace> -f <path to your yaml file>

#Example
#kubectl create -n arc -f C:\arc-data-services\postgres.yaml
```


## <a name="monitoring-the-creation-status"></a>Monitorando o status de criação

A criação do grupo de servidores de hiperescala do PostgreSQL levará alguns minutos para ser concluída. Você pode monitorar o progresso em outra janela do terminal com os seguintes comandos:

> [!NOTE]
>  Os comandos de exemplo a seguir pressupõem que você criou um grupo de servidores de hiperescala PostgreSQL chamado ' postgres1 ' e o namespace kubernetes com o nome ' Arc '.  Se você usou um nome de grupo de servidores de hiperescala do namespace/PostgreSQL diferente, poderá substituir ' Arc ' e ' postgres1 ' por seus nomes.

```console
kubectl get postgresql-12/postgres1 --namespace arc
```

```console
kubectl get pods --namespace arc
```

Você também pode verificar o status de criação de qualquer Pod específico executando um comando como abaixo.  Isso é especialmente útil para solucionar problemas.

```console
kubectl describe po/<pod name> --namespace arc

#Example:
#kubectl describe po/postgres1-0 --namespace arc
```

## <a name="troubleshooting-creation-problems"></a>Solucionando problemas de criação

Se você encontrar qualquer problemas com a criação, consulte o [Guia de solução de problemas](troubleshoot-guide.md).