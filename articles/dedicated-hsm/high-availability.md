---
title: Alta disponibilidade - HSM Dedicado do Azure | Microsoft Docs
description: Saiba mais sobre as considerações básicas para a alta disponibilidade do HSM dedicada do Azure. Este artigo inclui um exemplo.
services: dedicated-hsm
author: msmbaldwin
manager: rkarlin
ms.custom: mvc, seodec18
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/27/2019
ms.author: mbaldwin
ms.openlocfilehash: 8f8fa2f12825fe88218fe7033a1721cb49fc7335
ms.sourcegitcommit: 9ce0350a74a3d32f4a9459b414616ca1401b415a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/13/2020
ms.locfileid: "88189850"
---
# <a name="azure-dedicated-hsm-high-availability"></a>Alta disponibilidade do HSM Dedicado do Azure

O HSM Dedicado do Azure é sustentado pelos datacenters altamente disponíveis da Microsoft. No entanto, qualquer datacenter altamente disponível é vulnerável a falhas localizadas e, em circunstâncias extremas, a falhas no nível regional. A Microsoft implanta dispositivos HSM em diferentes datacenters em uma região para garantir que o provisionamento de vários dispositivos não leve esses dispositivos a compartilhar um único rack. Um nível adicional de alta disponibilidade pode ser obtido emparelhando esses HSMs entre os datacenters em uma região usando o recurso de grupo Gemalto HA. Também é possível emparelhar dispositivos entre regiões para resolver o failover regional em uma situação de recuperação de desastre. Com essa configuração de alta disponibilidade em várias camadas, qualquer falha no dispositivo será automaticamente endereçada para manter os aplicativos funcionando. Todos os datacenters também têm dispositivos e componentes sobressalentes no local para que qualquer dispositivo com falha possa ser substituído em tempo hábil.

## <a name="high-availability-example"></a>Exemplo de alta disponibilidade

Informações sobre como configurar dispositivos HSM para alta disponibilidade no nível do software estão no 'Guia de Administração do HSM da Rede Luna da Gemalto'. Este documento está disponível na  [página HSM do Gemalto](https://safenet.gemalto.com/data-encryption/hardware-security-modules-hsms/safenet-network-hsm/).

O diagrama a seguir mostra uma arquitetura altamente disponível. Ele usa vários dispositivos na região e vários dispositivos emparelhados em uma região separada. Essa arquitetura usa um mínimo de quatro dispositivos HSM e componentes de rede virtual.

![Diagrama de alta disponibilidade](media/high-availability/high-availability.png)

## <a name="next-steps"></a>Próximas etapas

Recomenda-se que todos os principais conceitos do serviço, como alta disponibilidade e segurança, sejam bem compreendidos antes do provisionamento do dispositivo e do design ou implantação do aplicativo.
Mais tópicos de nível de conceito:

* [Arquitetura de implantação](deployment-architecture.md)
* [Segurança física](physical-security.md)
* [Rede](networking.md)
* [Capacidade de suporte](supportability.md)
* [Monitoring](monitoring.md)

Para obter detalhes específicos sobre a configuração de dispositivos HSM para alta disponibilidade, consulte o Portal de suporte ao cliente da Gemalto para os Guias do administrador e consulte a seção 6.
