---
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 03/18/2019
ms.author: rogarana
ms.openlocfilehash: c3028ed7629c41eece354dd2554ede9249bac4f8
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "80334965"
---
Você pode anexar um número de discos de dados a uma máquina virtual do Azure. Com base nas metas de escalabilidade e desempenho dos discos de dados de uma VM, você pode determinar o número e o tipo de disco necessários para atender aos requisitos de desempenho e capacidade.

> [!IMPORTANT]
> Para obter o desempenho ideal, limite a quantidade de discos altamente utilizados anexados à máquina virtual para evitar possíveis limitações. Se todos os discos anexados não forem altamente utilizados ao mesmo tempo, a máquina virtual poderá dar suporte a um número maior de discos.

**Para Azure Managed disks:**

A tabela a seguir ilustra os limites padrão e máximo do número de recursos por região por assinatura. Não há nenhum limite para o número de Managed Disks, instantâneos e imagens por grupo de recursos.  

> | Recurso | Limite |
> | --- | --- |
> | Standard Managed Disks | 50.000 |
> | SSD Standard discos gerenciados | 50.000 |
> | Discos gerenciados Premium | 50.000 |
> | Instantâneos de Standard_LRS | 50.000 |
> | Instantâneos de Standard_ZRS | 50.000 |
> | Imagem gerenciada | 50.000 |

* **Para contas de armazenamento padrão:** Uma conta de armazenamento Standard tem uma taxa de solicitação total máxima de 20.000 IOPS. O total de IOPS em todos os discos de máquina virtual em uma conta de armazenamento Standard não deve exceder esse limite.
  
    Você pode calcular aproximadamente o número de discos altamente utilizados com suporte em uma única conta de armazenamento Standard com base no limite da taxa de solicitação. Por exemplo, para uma VM de camada básica, o número máximo de discos altamente utilizados é de cerca de 66, que é de 20.000/300 IOPS por disco. O número máximo de discos altamente utilizados para uma VM de camada Standard é de cerca de 40, que é de 20.000/500 IOPS por disco. 

* **Para contas de armazenamento Premium:** Uma conta de armazenamento Premium tem uma taxa de transferência total máxima de 50 Gbps. A taxa de transferência total de todos os discos da VM não deve exceder esse limite.

