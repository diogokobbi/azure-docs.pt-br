---
title: Série NC-máquinas virtuais do Azure
description: Especificações para as VMs da série NC.
author: vikancha-MSFT
ms.service: virtual-machines
ms.subservice: sizes
ms.topic: article
ms.date: 02/03/2020
ms.author: jushiman
ms.openlocfilehash: 9df68d76c9a06a8391c301fa13b6a0d1ac6529a2
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84726643"
---
# <a name="nc-series"></a>Série NC

As VMs da série NC são alimentadas pela placa [NVIDIA Tesla K80](https://www.nvidia.com/content/dam/en-zz/Solutions/Data-Center/tesla-product-literature/Tesla-K80-BoardSpec-07317-001-v05.pdf) e pelo processador Intel Xeon E5-2690 v3 (Haswell). Os usuários podem processar os dados mais rapidamente aproveitando o CUDA para aplicativos de exploração de energia, simulações de falhas, renderização de traçados de raio, aprendizado profundo e muito mais. A configuração NC24r fornece um adaptador de rede de alta taxa de transferência e baixa latência, otimizado para cargas de trabalho de computação paralela firmemente acopladas.

Armazenamento Premium:  Sem suporte

Cache de Armazenamento Premium:  Sem suporte

Migração ao Vivo: Sem suporte

Atualizações de preservação de memória: Sem suporte

| Tamanho | vCPU | Memória: GiB | Armazenamento temporário (SSD) GiB | GPU | Memória da GPU: GiB | Discos de dados máximos | Máximo de NICs |
|---|---|---|---|---|---|---|---|
| Standard_NC6    | 6  | 56  | 340  | 1 | 12 | 24 | 1 |
| Standard_NC12   | 12 | 112 | 680  | 2 | 24 | 48 | 2 |
| Standard_NC24   | 24 | 224 | 1440 | 4 | 48 | 64 | 4 |
| Standard_NC24r* | 24 | 224 | 1440 | 4 | 48 | 64 | 4 |

1 GPU = metade de um cartão K80.

*Compatível com RDMA

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../includes/virtual-machines-common-sizes-table-defs.md)]

## <a name="supported-operating-systems-and-drivers"></a>Sistemas operacionais e drivers com suporte

Para aproveitar os recursos de GPU das VMs da série N do Azure, os drivers NVIDIA GPU devem ser instalados.

A [Extensão de Driver de GPU NVIDIA](./extensions/hpccompute-gpu-windows.md) instala drivers CUDA ou GRID NVIDIA apropriados em VMs da série N. Instale ou gerencie a extensão usando o portal do Azure ou ferramentas, como Azure PowerShell ou modelos do Azure Resource Manager. Confira a [documentação da Extensão de Driver de GPU NVIDIA](./extensions/hpccompute-gpu-windows.md) para saber quais são os sistemas operacionais compatíveis e as etapas de implantação. Para obter informações gerais sobre extensões de VM, confira [Recursos e extensões de máquina virtual do Azure](./extensions/overview.md).

Se você optar por instalar manualmente os drivers NVIDIA GPU, consulte [configuração do driver GPU da série n para Windows](./windows/n-series-driver-setup.md) ou [instalação do driver de GPU da série n para Linux](./linux/n-series-driver-setup.md) para sistemas operacionais, Drivers, instalação e etapas de verificação com suporte.

## <a name="other-sizes"></a>Outros tamanhos

- [Propósito geral](sizes-general.md)
- [Memória otimizada](sizes-memory.md)
- [Armazenamento otimizado](sizes-storage.md)
- [GPU otimizada](sizes-gpu.md)
- [Computação de alto desempenho](sizes-hpc.md)
- [Gerações anteriores](sizes-previous-gen.md)

## <a name="next-steps"></a>Próximas etapas

Saiba mais sobre como as [ACUs (unidade de computação do Azure)](acu.md) podem ajudar você a comparar o desempenho de computação entre SKUs do Azure.
