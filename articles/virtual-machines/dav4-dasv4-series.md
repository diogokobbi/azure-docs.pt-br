---
title: Séries Dav4 e Dasv4
description: Especificações para as VMs da série Dav4 e Dasv4.
author: migerdes
ms.service: virtual-machines
ms.subservice: sizes
ms.topic: conceptual
ms.date: 02/03/2020
ms.author: jushiman
ms.openlocfilehash: 2548f002b2006b5d241c323ae880704bbe95301e
ms.sourcegitcommit: 271601d3eeeb9422e36353d32d57bd6e331f4d7b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88649917"
---
# <a name="dav4-and-dasv4-series"></a>Séries Dav4 e Dasv4

As séries Dav4 e Dasv4 são novos tamanhos que utilizam o processador 2.35 GHz EPYC<sup>TM</sup> 7452 da AMD em uma configuração multi-threaded com até 256 MB de cache L3, que dedica 8 MB desse cache L3 a cada 8 núcleos aumentando as opções do cliente para executar suas cargas de trabalho de uso geral. As séries Dav4 e Dasv4 têm as mesmas configurações de memória e disco que as séries Dsv3 e D.

## <a name="dav4-series"></a>Série Dav4

ACU: 230-260

Armazenamento Premium: sem suporte

Armazenamento em cache Premium: sem suporte

Migração ao Vivo: Com suporte

Atualizações de preservação de memória: Com suporte

Os tamanhos da série Dav4 são baseados no processador AMD EPYC<sup>TM</sup> 7452 de 2.35 GHz que pode alcançar uma frequência máxima aumentada de 3.35 GHz. Os tamanhos da série Dav4 oferecem uma combinação de vCPU, memória e armazenamento temporário para a maioria das cargas de trabalho de produção. O armazenamento do disco de dados é faturado separadamente das máquinas virtuais. Para usar o SSD Premium, use os tamanhos de Dasv4. Os medidores de cobrança e preço para os tamanhos de Dasv4 são os mesmos que os da série Dav4.

| Tamanho | vCPU | Memória: GiB | Armazenamento temporário (SSD) GiB | Discos de dados máximos | Taxa de transferência máxima de armazenamento temporário: IOPS / MBps de leitura / MBps de gravação | Máximo de NICs | Largura de banda de rede esperada (Mbps) |
|-----|-----|-----|-----|-----|-----|-----|-----|
| Standard_D2a_v4 |  2  | 8  | 50  | 4  | 3000 / 46 / 23   | 2 | 1000 |
| Standard_D4a_v4 |  4  | 16 | 100 | 8  | 6000 / 93 / 46   | 2 | 2000 |
| Standard_D8a_v4 |  8  | 32 | 200 | 16 | 12000 / 187 / 93 | 4 | 4000 |
| Standard_D16a_v4|  16 | 64 | 400 |32  | 24000 / 375 / 187 |8 | 8000 |
| Standard_D32a_v4|  32 | 128| 800 | 32 | 48000 / 750 / 375 |8 | 16000 |
| Standard_D48a_v4| 48 | 192| 1200 | 32 | 96000/1000/500 | 8 | 24.000 |
| Standard_D64a_v4| 64 | 256 | 1600 | 32 | 96000/1000/500 | 8 | 30000 |
| Standard_D96a_v4| 96 | 384 | 2400 | 32 | 96000/1000/500 | 8 | 30000 |

## <a name="dasv4-series"></a>Série Dasv4

ACU: 230-260

Armazenamento Premium: com suporte

Cache de armazenamento Premium: com suporte

Migração ao Vivo: Com suporte

Atualizações de preservação de memória: Com suporte

Os tamanhos da série Dasv4 são baseados no processador AMD EPYC<sup>TM</sup> 7452 de 2.35 GHz que pode alcançar uma frequência máxima aumentada de 3.35 GHz e usar SSD Premium. Os tamanhos da série Dasv4 oferecem uma combinação de vCPU, memória e armazenamento temporário para a maioria das cargas de trabalho de produção.

| Tamanho | vCPU | Memória: GiB | Armazenamento temporário (SSD) GiB | Discos de dados máximos | Taxa de transferência máxima de armazenamento temporário: IOPS / MBps (tamanho do cache em GiB) | Taxa de transferência máxima do disco não armazenado em cache: IOPS / MBps | Máximo de NICs | Largura de banda de rede esperada (Mbps) |
|-----|-----|-----|-----|-----|-----|-----|-----|-----|
| Standard_D2as_v4|2|8|16|4|4000/32 (50)|3200/48|2 | 1000 |
| Standard_D4as_v4|4|16|32|8|8000/64 (100)|6400/96|2 | 2000 |
| Standard_D8as_v4|8|32|64|16|16000/128 (200)|12800/192|4 | 4000 |
| Standard_D16as_v4|16|64|128|32|32000/255 (400)|25600/384|8 | 8000 |
| Standard_D32as_v4|32|128|256|32|64000/510 (800)|51200/768|8 | 16000 |
| Standard_D48as_v4|48|192|384|32|96000/1020 (1200)|76800/1148|8 | 24.000 |
| Standard_D64as_v4|64|256|512|32|128000/1020 (1600)|80000/1200|8 | 30000 | 
| Standard_D96as_v4|96|384|768|32|192000/1020 (2400)|80000/1200|8 | 30000 |

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../includes/virtual-machines-common-sizes-table-defs.md)]

## <a name="other-sizes-and-information"></a>Outros tamanhos e informações

- [Propósito geral](sizes-general.md)
- [Memória otimizada](sizes-memory.md)
- [Armazenamento otimizado](sizes-storage.md)
- [GPU otimizada](sizes-gpu.md)
- [Computação de alto desempenho](sizes-hpc.md)
- [Gerações anteriores](sizes-previous-gen.md)

Calculadora de preços: [calculadora de preços](https://azure.microsoft.com/pricing/calculator/)

Mais informações sobre tipos de discos: [tipos de disco](./disks-types.md#ultra-disk)

## <a name="next-steps"></a>Próximas etapas

Saiba mais sobre como as [ACUs (unidade de computação do Azure)](acu.md) podem ajudar você a comparar o desempenho de computação entre SKUs do Azure.