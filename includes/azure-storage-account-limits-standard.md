---
title: incluir arquivo
description: incluir arquivo
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 07/08/2020
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 509568b143c9fbbf236139ca83cb55b0ef39beb0
ms.sourcegitcommit: 5cace04239f5efef4c1eed78144191a8b7d7fee8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86145944"
---
A tabela a seguir descreve os limites padrão das contas de armazenamento de uso geral v1 e v2, de Armazenamento de Blobs e de Armazenamento de Blobs de Blocos do Azure. O limite de *entrada* refere-se a todos os dados enviados a uma conta de armazenamento. O limite de *saída* refere-se a todos os dados recebidos de uma conta de armazenamento.

| Recurso | Limite |
| --- | --- |
| Número de contas de armazenamento por região por assinatura, incluindo contas de armazenamento Standard e Premium.| 250 |
| Capacidade máxima da conta de armazenamento | 5 PiB <sup>1</sup>|
| Número máximo de contêineres de blob, blobs, compartilhamentos de arquivo, tabelas, filas, entidades ou mensagens por conta de armazenamento | Sem limite |
| Taxa<sup>1</sup> máxima de solicitação por conta de armazenamento | 20 mil solicitações por segundo |
| Entrada máxima<sup>1</sup> por conta de armazenamento (regiões dos EUA e Europa) | 10 Gbps |
| Entrada máxima<sup>1</sup> por conta de armazenamento (regiões fora dos EUA e da Europa) | 5 Gbps se o RA-GRS/GRS estiver habilitado, 10 Gbps para o LRS/ZRS<sup>2</sup> |
| Saída máxima para contas de armazenamento de Blobs e de uso geral v2 (todas as regiões) | 50 Gbps |
| Saída máxima para contas de armazenamento de uso geral v1 (regiões dos EUA) | 20 Gbps se o RA-GRS/GRS estiver habilitado, 30 Gbps para o LRS/ZRS<sup>2</sup> |
| Saída máxima para contas de armazenamento de uso geral v1 (regiões fora dos EUA) | 10 Gbps se o RA-GRS/GRS estiver habilitado, 15 Gbps para o LRS/ZRS<sup>2</sup> |
| Número máximo de regras de rede virtual por conta de armazenamento | 200 |
| Número máximo de regras de endereço IP por conta de armazenamento | 200 |

<sup>1</sup> As contas padrão do Armazenamento do Azure dão suporte a limites mais altos de capacidade e de entrada por solicitação. Para solicitar um aumento nos limites de conta, contate o [Suporte do Azure](https://azure.microsoft.com/support/faq/).

<sup>2</sup> Se sua conta de armazenamento tiver acesso de leitura habilitado com RA-GRS (armazenamento com redundância geográfica) ou RA-GZRS (armazenamento com redundância de zona geográfica), os destinos de saída do local secundário serão idênticos aos do local principal. Para obter mais informações, consulte [Replicação do Armazenamento do Azure](../articles/storage/common/storage-redundancy.md).

> [!NOTE]
> A Microsoft recomenda o uso de contas de armazenamento de uso geral v2 na maioria dos cenários. É possível atualizar facilmente uma conta do Armazenamento de Blobs do Azure ou de uso geral v1 para uma conta de uso geral v2 sem tempo de inatividade e sem precisar copiar dados. Para obter mais informações, confira [Atualizar para uma conta de armazenamento de uso geral v2](../articles/storage/common/storage-account-upgrade.md).

Se as necessidades do seu aplicativo excederem as metas de escalabilidade de uma conta única de armazenamento, você pode preparar seu aplicativo para usar várias contas de armazenamento. Em seguida, particione seus objetos de dados nessas contas de armazenamento. Para obter informações sobre o preço por volume, confira [Preço do Armazenamento do Azure](https://azure.microsoft.com/pricing/details/storage/).

Todas as contas de armazenamento são executadas em uma topologia de rede simples, independentemente de quando foram criadas. Para obter mais informações sobre a arquitetura de rede simples do armazenamento do Azure e sobre escalabilidade, confira [Armazenamento do Microsoft Azure: um serviço de armazenamento em nuvem altamente disponível com coerência forte](https://docs.microsoft.com/archive/blogs/hanuk/windows-azures-flat-network-storage-to-enable-higher-scalability-targets). 
