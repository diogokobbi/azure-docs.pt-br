---
title: Script do PowerShell para criar um bloqueio de recurso para um keyspace e uma tabela da API do Cassandra do Azure Cosmos
description: Criar um bloqueio de recurso para um keyspace e uma tabela da API do Cassandra do Azure Cosmos
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: sample
ms.date: 06/12/2020
ms.openlocfilehash: be0ba84b323f235d15761ed5bf85a380f48276a2
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87505427"
---
# <a name="create-a-resource-lock-for-azure-cosmos-cassandra-api-keyspace-and-table-using-azure-powershell"></a>Criar um bloqueio de recurso para um keyspace e uma tabela da API do Cassandra do Azure Cosmos usando o Azure PowerShell

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../../../includes/sample-powershell-install-no-ssh.md)]

> [!IMPORTANT]
> Os bloqueios de recursos não funcionam para as alterações feitas por usuários que se conectam usando qualquer SDK do Cassandra, o Shell do CQL ou o portal do Azure, a menos que a conta do Cosmos DB seja bloqueada primeiro com a propriedade `disableKeyBasedMetadataWriteAccess` habilitada. Para saber mais sobre como habilitar essa propriedade, confira [Como impedir alterações por meio dos SDKs](../../../role-based-access-control.md#prevent-sdk-changes).

## <a name="sample-script"></a>Exemplo de script

[!code-powershell[main](../../../../../powershell_scripts/cosmosdb/cassandra/ps-cassandra-lock.ps1 "Create, list, and remove resource locks")]

## <a name="clean-up-deployment"></a>Limpar a implantação

Após a execução do script de exemplo, o comando a seguir pode ser usado para remover o grupo de recursos e todos os recursos associados a ele.

```powershell
Remove-AzResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a>Explicação sobre o script

Este script usa os comandos a seguir. Cada comando da tabela é vinculado à documentação específica do comando.

| Comando | Observações |
|---|---|
|**Recurso do Azure**| |
| [New-AzResourceLock](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcelock) | Cria um bloqueio de recurso. |
| [Get-AzResourceLock](https://docs.microsoft.com/powershell/module/az.resources/get-azresourcelock) | Obtém um bloqueio de recurso ou lista os bloqueios de recurso. |
| [Remove-AzResourceLock](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcelock) | Remove um bloqueio de recurso. |
|||

## <a name="next-steps"></a>Próximas etapas

Para obter mais informações sobre o Azure PowerShell, confira a [Documentação do Azure PowerShell](https://docs.microsoft.com/powershell/).

Exemplos adicionais de scripts do PowerShell do Banco de Dados Cosmos do Azure podem ser encontrados nos [Scripts do PowerShell do Banco de Dados Cosmos do Azure](../../../powershell-samples.md).