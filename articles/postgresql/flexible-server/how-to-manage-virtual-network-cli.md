---
title: Gerenciar redes virtuais-CLI do Azure-banco de dados do Azure para PostgreSQL-servidor flexível
description: Criar e gerenciar redes virtuais para o banco de dados do Azure para PostgreSQL – servidor flexível usando o CLI do Azure
author: ambhatna
ms.author: ambhatna
ms.service: postgresql
ms.topic: how-to
ms.date: 09/22/2020
ms.openlocfilehash: 727eb4cd7e7c3de090e1573cb5358ef23118385e
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90932822"
---
# <a name="create-and-manage-virtual-networks-for-azure-database-for-postgresql---flexible-server-using-the-azure-cli"></a>Criar e gerenciar redes virtuais para o banco de dados do Azure para PostgreSQL – servidor flexível usando o CLI do Azure

> [!IMPORTANT]
> O Servidor Flexível do Banco de Dados do Azure para PostgreSQL está em versão prévia

Banco de dados do Azure para PostgreSQL-o servidor flexível dá suporte a dois tipos de métodos de conectividade de rede mutuamente exclusivos para se conectar ao seu servidor flexível. As duas opções são:

* Acesso público (endereços IP permitidos)
* Acesso privado (Integração VNet)

Neste artigo, vamos nos concentrar na criação do servidor PostgreSQL com **acesso privado (integração VNet)** usando CLI do Azure. Com o *acesso privado (integração VNet)*, você pode implantar seu servidor flexível em sua própria [rede virtual do Azure](../../virtual-network/virtual-networks-overview.md). As redes virtuais do Azure fornecem comunicação de rede privada e segura. No acesso privado, as conexões com o servidor PostgreSQL são restritas somente dentro de sua rede virtual. Para saber mais sobre isso, consulte [acesso privado (integração VNet)](./concepts-networking.md#private-access-vnet-integration).

No banco de dados do Azure para PostgreSQL – servidor flexível, você só pode implantar o servidor em uma rede virtual e uma sub-rede durante a criação do servidor. Depois que o servidor flexível for implantado em uma rede virtual e sub-rede, você não poderá movê-lo para outra rede virtual, sub-rede ou *acesso público (endereços IP permitidos)*.

## <a name="launch-azure-cloud-shell"></a>Iniciar o Azure Cloud Shell

O [Azure Cloud Shell](../../cloud-shell/overview.md) é um shell gratuito e interativo que poderá ser usado para executar as etapas deste artigo. Ele tem ferramentas do Azure instaladas e configuradas para usar com sua conta.

Para abrir o Cloud Shell, basta selecionar **Experimentar** no canto superior direito de um bloco de código. Você também pode abrir o Cloud Shell em uma guia separada do navegador indo até [https://shell.azure.com/bash](https://shell.azure.com/bash). Selecione **Copiar** para copiar os blocos de código, cole-o no Cloud Shell e selecione **Enter** para executá-lo.

Caso prefira instalar e usar a CLI localmente, este início rápido exigirá a CLI do Azure versão 2.0 ou posterior. Execute `az --version` para encontrar a versão. Se você precisa instalar ou atualizar, consulte [Instalar a CLI do Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="prerequisites"></a>Pré-requisitos

Você precisará entrar na sua conta usando o comando [az login](https://docs.microsoft.com/cli/azure/reference-index?view=azure-cli-latest#az-login). Observe a propriedade **ID** , que se refere à **ID da assinatura** da sua conta do Azure.

```azurecli-interactive
az login
```

Selecione a assinatura específica em sua conta usando o comando [az account set](https://docs.microsoft.com/cli/azure/account?view=azure-cli-latest#az-account-set). Anote o valor de **ID** da saída de **logon AZ** para usar como o argumento valor para **assinatura** no comando. Se tiver várias assinaturas, escolha a que for adequada para cobrança do recurso. Para obter todas as suas assinaturas, use [az account list](https://docs.microsoft.com/cli/azure/account?view=azure-cli-latest#az-account-list).

```azurecli
az account set --subscription <subscription id>
```

## <a name="create-azure-database-for-postgresql---flexible-server-using-cli"></a>Criar banco de dados do Azure para PostgreSQL – servidor flexível usando a CLI
Você pode usar o `az postgres flexible-server` comando para criar o servidor flexível com *acesso privado (integração VNet)*. Esse comando usa o acesso privado (integração VNet) como o método de conectividade padrão. Uma rede virtual e uma sub-rede serão criadas para você se nenhuma for fornecida. Você também pode fornecer a rede virtual e a sub-rede já existentes usando a ID de sub-rede. <!-- You can provide the **vnet**,**subnet**,**vnet-address-prefix** or**subnet-address-prefix** to customize the virtual network and subnet.--> Há várias opções para criar um servidor flexível usando a CLI, conforme mostrado nos exemplos abaixo.

>[!Important]
> O uso desse comando delegará a sub-rede a **Microsoft. DBforPostgreSQL/flexibleServers**. Essa delegação significa que somente os servidores flexíveis do banco de dados do Azure para PostgreSQL podem usar essa sub-rede. Nenhum outro tipo de recurso do Azure pode estar na sub-rede delegada.
>
Consulte a documentação de referência do CLI do Azure <!--FIXME --> para obter uma lista completa de parâmetros configuráveis da CLI. Por exemplo, nos comandos abaixo, você pode opcionalmente especificar o grupo de recursos.

- Criar um servidor flexível usando a rede virtual padrão, a sub-rede com o prefixo de endereço padrão
    ```azurecli-interactive
    az postgres flexible-server create
    ```
<!--- Create a flexible server using already existing virtual network and subnet
    ```azurecli-interactive
    az postgres flexible-server create --vnet myVnet --subnet mySubnet
    ```-->
- Crie um servidor flexível usando a rede virtual já existente, a sub-rede e o usando a ID de sub-rede. A sub-rede fornecida não deve ter nenhum outro recurso implantado nela e essa sub-rede será delegada para **Microsoft. DBforPostgreSQL/flexibleServers**, se ainda não tiver sido delegada.
    ```azurecli-interactive
    az postgres flexible-server create --subnet /subscriptions/{SubID}/resourceGroups/{ResourceGroup}/providers/Microsoft.Network/virtualNetworks/{VNetName}/subnets/{SubnetName}
    ```
    > [!Note]
    > A rede virtual e a sub-rede devem estar na mesma região e assinatura que o servidor flexível.
<!--  
- Create a flexible server using new virtual network, subnet with non-default address prefix
    ```azurecli-interactive
    az postgres flexible-server create --vnet myVnet --vnet-address-prefix 10.0.0.0/24 --subnet mySubnet --subnet-address-prefix 10.0.0.0/24
    ```-->
Consulte a documentação de referência do CLI do Azure <!--FIXME --> para obter uma lista completa de parâmetros configuráveis da CLI.

## <a name="next-steps"></a>Próximas etapas
- Saiba mais sobre [rede no banco de dados do Azure para PostgreSQL – servidor flexível](./concepts-networking.md).
- [Crie e gerencie o banco de dados do Azure para PostgreSQL – rede virtual de servidor flexível usando o portal do Azure](./how-to-manage-virtual-network-portal.md).
- Saiba mais sobre o [banco de dados do Azure para PostgreSQL – rede virtual de servidor flexível](./concepts-networking.md#private-access-vnet-integration).