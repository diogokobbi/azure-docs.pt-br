---
author: IngridAtMicrosoft
ms.service: media-services
ms.topic: include
ms.date: 08/17/2020
ms.author: inhenkel
ms.custom: CLI
ms.openlocfilehash: 5c0341087cdd348e973da5faaa1f90081780c9c8
ms.sourcegitcommit: 37afde27ac137ab2e675b2b0492559287822fded
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88602226"
---
<!--Create a media services account -->

O comando da CLI do Azure a seguir cria uma nova conta dos Serviços de Mídia. Você pode substituir os seguintes valores: `amsaccount` `storageaccountforams` (deve corresponder ao valor que você forneceu para sua conta de armazenamento) e `amsResourceGroup` (deve corresponder ao valor que você deu para o grupo de recursos).  

```azurecli
az ams account create --name amsaccount -g amsResourceGroup --storage-account storageaccountforams -l westus2
```