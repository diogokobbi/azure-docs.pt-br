---
title: Tutorial – Implantar o Cluster de vSphere no Azure
description: Saiba como implantar um Cluster de vSphere no Azure usando a Solução VMware no Azure
ms.topic: tutorial
ms.date: 09/07/2020
ms.openlocfilehash: 2aa9d64dfa143e77b0edcc0c32a853645803ef67
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90985943"
---
# <a name="tutorial-deploy-an-azure-vmware-solution-private-cloud-in-azure"></a>Tutorial: Implantar uma nuvem privada da Solução VMware no Azure

A Solução VMware no Azure permite implantar um cluster de vSphere no Azure. A implantação inicial mínima é de três hosts. Mais hosts podem ser adicionados, um de cada vez, até atingir um máximo de 16 hosts por cluster. 

Como a Solução VMware no Azure não permite que você gerencie sua nuvem privada com o vCenter local na inicialização, são necessárias uma configuração adicional de uma instância do vCenter local e uma conexão com ela, uma rede virtual e mais. Esses procedimentos e os pré-requisitos relacionados são abordados neste tutorial.

Neste tutorial, você aprenderá como:

> [!div class="checklist"]
> * Criar uma nuvem privada da Solução VMware no Azure
> * Verificar a nuvem privada implantada

## <a name="prerequisites"></a>Pré-requisitos

- Uma conta do Azure com uma assinatura ativa. [Crie uma conta gratuitamente](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Direitos administrativos apropriados e permissão para criar uma nuvem privada.
- Verifique se você tem a rede adequada configurada, conforme descrito no [Tutorial: Lista de verificação de rede](tutorial-network-checklist.md).

## <a name="register-the-resource-provider"></a>Registre o provedor de recursos

[!INCLUDE [register-resource-provider-steps](includes/register-resource-provider-steps.md)]


## <a name="create-a-private-cloud"></a>Criar uma nuvem privada

Você pode criar uma nuvem privada da Solução VMware no Azure usando o [portal do Azure](#azure-portal) ou a [CLI do Azure](#azure-cli).

### <a name="azure-portal"></a>Portal do Azure

[!INCLUDE [create-avs-private-cloud-azure-portal](includes/create-avs-private-cloud-azure-portal-steps.md)]

### <a name="azure-cli"></a>CLI do Azure

Em vez de usar o portal do Azure para criar uma nuvem privada da Solução VMware no Azure, você pode usar a CLI do Azure usando o Azure Cloud Shell. É um shell de interativo gratuito com ferramentas comuns do Azure pré-instaladas e configuradas para usar com sua conta. 

#### <a name="open-azure-cloud-shell"></a>Abrir o Azure Cloud Shell

Para abrir o Cloud Shell, selecione **Experimentar** no canto superior direito de um bloco de código. Você também pode iniciar o Cloud Shell em uma guia separada do navegador indo até [https://shell.azure.com/bash](https://shell.azure.com/bash). Selecione **Copiar** para copiar os blocos de código, cole o código no Cloud Shell e depois pressione **Enter** para executá-lo.

#### <a name="create-a-resource-group"></a>Criar um grupo de recursos

Crie um grupo de recursos com o comando [az group create](/cli/azure/group). Um grupo de recursos do Azure é um contêiner lógico no qual os recursos do Azure são implantados e gerenciados. O exemplo a seguir cria um grupo de recursos chamado *myResourceGroup* na localização *eastus*:

```
azurecli-interactive
az group create --name myResourceGroup --location eastus
```

#### <a name="create-a-private-cloud"></a>Criar uma nuvem privada

Forneça um nome do grupo de recursos, um nome para a nuvem privada, uma localização e o tamanho do cluster.

| Propriedade  | Descrição  |
| --------- | ------------ |
| **-g** (Nome do Grupo de Recursos)     | O nome do grupo de recursos com os recursos de sua nuvem privada.        |
| **-n** (Nome da Nuvem Privada)     | O nome da sua nuvem privada da Solução VMware no Azure.        |
| **--location**     | A localização usada para sua nuvem privada.         |
| **--cluster-size**     | O tamanho do cluster. O valor mínimo é 3.         |
| **--network-block**     | O bloco de rede de endereço IP do CIDR a ser usado para sua nuvem privada. O bloco de endereço não deve se sobrepor aos blocos de endereços usados em outras redes virtuais que estão em sua assinatura e redes locais.        |
| **--sku** | O valor de SKU: AV36 |

```
azurecli-interactive
az vmware private-cloud create -g myResourceGroup -n myPrivateCloudName --location eastus --cluster-size 3 --network-block xx.xx.xx.xx/22 --sku AV36
```

## <a name="delete-an-azure-vmware-solution-private-cloud"></a>Excluir uma nuvem privada da Solução VMware no Azure

Se tiver uma nuvem privada da Solução VMware no Azure que não é mais necessária, você poderá excluí-la. Uma nuvem privada da Solução VMware no Azure inclui um domínio de rede isolado, um ou mais clusters do vSphere provisionados em nós de servidor dedicados e, normalmente, muitas máquinas virtuais. Quando uma nuvem privada é excluída, todas as máquinas virtuais, os dados delas e os clusters são excluídos. Os nós dedicados de bare-metal são apagados com segurança e retornados ao pool gratuito. O domínio de rede provisionado o cliente é excluído.  

> [!CAUTION]
> A exclusão da nuvem privada é uma operação irreversível. Após a nuvem privada ser excluída, os dados não poderão ser recuperados, pois todos os componentes e as cargas de trabalho em execução serão terminados e todos os dados e configurações da nuvem privada serão destruídos, incluindo os endereços IP públicos.

### <a name="prerequisites"></a>Pré-requisitos

Depois que uma nuvem privada for excluída, não será possível recuperar as máquinas virtuais e os dados delas. Se os dados da máquina virtual forem necessários mais tarde, o administrador precisará primeiro fazer backup de todos os dados antes de excluir a nuvem privada.

### <a name="steps-to-delete-an-azure-vmware-solution-private-cloud"></a>Etapas para excluir uma nuvem privada da Solução VMware no Azure

1. Acesse a página Soluções VMware no Azure no portal do Azure.

2. Selecione a nuvem privada a ser excluída.
 
3. Insira o nome da nuvem privada e selecione **Sim**. Em algumas horas, o processo de exclusão será concluído.  

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você aprendeu a:

> [!div class="checklist"]
> * Criar uma nuvem privada da Solução VMware no Azure
> * Verificar a nuvem privada implantada
> * Excluir uma nuvem privada da Solução VMware no Azure

Continue para o próximo tutorial para saber como criar uma rede virtual para uso com sua nuvem privada como parte da configuração do gerenciamento local para seus clusters de nuvem privada.

> [!div class="nextstepaction"]
> [Criar uma rede virtual](tutorial-configure-networking.md)
