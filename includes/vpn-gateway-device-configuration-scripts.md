---
title: incluir arquivo
description: incluir arquivo
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 02/07/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: e84a77629026bb8885a48b8ed928699825f07115
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "77111211"
---
| **Fornecedor** | **Família de dispositivos** | **Versão do Firmware** |
| --- | --- | --- |
|Cisco | ISR| IOS 15.1 (versão prévia)|
|Cisco | ASA | ASA (*) RouteBased (IKEv2 não BGP) para ASA abaixo de 9.8 |
|Cisco | ASA | ASA RouteBased (IKEv2 - não BGP) para ASA 9.8+ |
|Juniper | SRX_GA | 12.x|
|Juniper | SSG_GA | ScreenOS 6.2.x|
|Juniper | JSeries_GA | JunOS 12.x|
|Juniper | SRX | JunOS 12.x RouteBased BGP |
|Ubiquiti| EdgeRouter| EdgeOS v1.10x RouteBased VTI|
|Ubiquiti| EdgeRouter| EdgeOS v1.10x RouteBased VTI|

> [!NOTE]
> ( * ) Obrigatório: NarrowAzureTrafficSelectors (habilitar a opção UsePolicyBasedTrafficSelectors) e CustomAzurePolicies (IKE/IPsec)
>
