---
title: O que é o Azure Load Balancer?
titleSuffix: Azure Load Balancer
description: Visão geral dos recursos do Balanceador de Carga do Azure, arquitetura e implementação. Saiba como o Load Balancer funciona e como usá-lo na nuvem.
services: load-balancer
documentationcenter: na
author: asudbring
ms.service: load-balancer
Customer intent: As an IT administrator, I want to learn more about the Azure Load Balancer service and what I can use it for.
ms.devlang: na
ms.topic: overview
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 1/14/2020
ms.author: allensu
ms.openlocfilehash: 602a36964d4a6d5f3660499570327db3479c980e
ms.sourcegitcommit: 58d3b3314df4ba3cabd4d4a6016b22fa5264f05a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89299194"
---
# <a name="what-is-azure-load-balancer"></a>O que é o Azure Load Balancer?

*Balanceamento de carga* refere-se à distribuição uniforme de carga ou tráfego de rede de entrada para um grupo de recursos ou servidores de back-end. 

O Azure Load Balancer opera na camada quatro do modelo OSI (Interconexão de Sistemas Abertos). É o ponto de contato único para clientes. O Load Balancer distribui fluxos de entrada que chegam nas instâncias de pool de front-end a back-end do balanceador de carga. Esses fluxos ocorrem de acordo com as investigações de integridade e regras de balanceamento de carga especificadas. As instâncias do pool de back-end podem ser Máquinas Virtuais ou instâncias do Azure em um conjunto de dimensionamento de máquinas virtuais.

Um **[balanceador de carga público](./components.md#frontend-ip-configurations)** pode fornecer conexões de saída para VMs (máquinas virtuais) dentro de sua rede virtual. Essas conexões são realizadas por meio da tradução dos endereços IP privados para endereços IP públicos. Os balanceadores de carga públicos são usados para balancear a carga do tráfego de Internet para suas VMs.

Um **[balanceador de carga interno (ou privado)](./components.md#frontend-ip-configurations)** é usado quando IPs privados são necessários apenas no front-end. Os balanceadores de carga internos são usados para balancear a carga do tráfego dentro de uma rede virtual. Um front-end do balanceador de carga pode ser acessado de uma rede local em um cenário híbrido.

<p align="center">
  <img src="./media/load-balancer-overview/load-balancer.svg" alt="Figure depicts both public and internal load balancers directing traffic to port 80 on multiple servers on a Web tier and port 443 on multiple servers on a business tier." width="512" title="Azure Load Balancer">
</p>

*Figura: Balanceando aplicativos de várias camadas usando o Load Balancer interno e público*

Para obter mais informações sobre os componentes individuais do balanceador de carga, confira [Componentes do Azure Load Balancer](./components.md).

## <a name="why-use-azure-load-balancer"></a>Por que usar o Azure Load Balancer?
Com o Standard Load Balancer, você pode dimensionar seus aplicativos e criar serviços com alta disponibilidade. O Load Balancer dá suporte a cenários de entrada e saída. O Load Balancer fornece baixa latência e alta taxa de transferência e pode ser escalado verticalmente em milhões de fluxos para aplicativos TCP e UDP.

Os principais cenários em que você pode usar o Standard Load Balancer incluem:

- Balancear carga do tráfego **[interno](https://docs.microsoft.com/azure/load-balancer/tutorial-load-balancer-standard-internal-portal)** e **[externo](https://docs.microsoft.com/azure/load-balancer/tutorial-load-balancer-standard-manage-portal)** para máquinas virtuais do Azure.

- Aumentar a disponibilidade distribuindo recursos **[dentro](https://docs.microsoft.com/azure/load-balancer/tutorial-load-balancer-standard-public-zonal-portal)** e **[entre](https://docs.microsoft.com/azure/load-balancer/tutorial-load-balancer-standard-public-zone-redundant-portal)** zonas.

- Configurar a **[conectividade de saída ](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections)** para máquinas virtuais do Azure.

- Usar **[investigações de integridade](https://docs.microsoft.com/azure/load-balancer/load-balancer-custom-probe-overview)** para monitorar recursos com balanceamento de carga.

- Empregar o **[encaminhamento de porta](https://docs.microsoft.com/azure/load-balancer/tutorial-load-balancer-port-forwarding-portal)** para acessar máquinas virtuais em uma rede virtual por uma porta e endereço IP público.

- Habilitar o suporte para o **[balanceamento de carga](https://docs.microsoft.com/azure/virtual-network/virtual-network-ipv4-ipv6-dual-stack-standard-load-balancer-powershell)** do **[IPv6](https://docs.microsoft.com/azure/virtual-network/ipv6-overview)** .

- O Standard Load Balancer fornece métricas multidimensionais por meio do [Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/overview).  Essas métricas podem ser filtradas, agrupadas e divididas para uma determinada dimensão.  Elas fornecem informações atuais e históricas de desempenho e integridade do seu serviço.  Também há suporte para a Integridade de Recursos. Leia **[Diagnóstico do Standard Load Balancer](load-balancer-standard-diagnostics.md)** para obter mais detalhes.

- Balancear carga de serviços em **[várias portas, vários endereços IP ou ambos](https://docs.microsoft.com/azure/load-balancer/load-balancer-multivip-overview)** .

- Mover recursos **[internos](https://docs.microsoft.com/azure/load-balancer/move-across-regions-internal-load-balancer-portal)** e **[externos](https://docs.microsoft.com/azure/load-balancer/move-across-regions-external-load-balancer-portal)** do Load Balancer entre regiões do Azure.

- Balancear carga do fluxo TCP e UDP em todas as portas simultaneamente usando **[portas de HA](https://docs.microsoft.com/azure/load-balancer/load-balancer-ha-ports-overview)** .

### <a name="secure-by-default"></a><a name="securebydefault"></a>Segurança por padrão

O Standard Load Balancer se baseia no modelo de segurança de rede de confiança zero em seu núcleo. O Standard Load Balancer é seguro por padrão e faz parte de sua rede virtual. A rede virtual é uma rede privada e isolada.  Isso significa que os Standard Load Balancers e os endereços IP públicos padrão são fechados para fluxos de entrada, a menos que sejam abertos por Grupos de Segurança de Rede. Os NSGs são usados para permitir explicitamente o tráfego permitido.  Se você não tiver um NSG em uma sub-rede ou NIC de seu recurso de máquina virtual, o tráfego não terá permissão para acessar o recurso. Para saber mais sobre NSGs e como aplicá-los para seu cenário, consulte [Grupos de segurança de rede no Azure](../virtual-network/security-overview.md).
Por padrão, o Load Balancer Básico é aberto para a Internet. Além disso, o Load Balancer não armazena os dados do cliente.

## <a name="pricing-and-sla"></a>Preço e SLA

Para obter informações sobre o preço do Standard Load Balancer, confira [Preço do Load Balancer](https://azure.microsoft.com/pricing/details/load-balancer/).
O Azure Load Balancer Basic é oferecido gratuitamente.
Confira [SLA para Load Balancer](https://aka.ms/lbsla). O Load Balancer Básico não tem SLA.

## <a name="whats-new"></a>Novidades

Assine o RSS feed e veja as atualizações mais recentes dos recursos do Azure Load Balancer na página [Atualizações do Azure](https://azure.microsoft.com/updates/?category=networking&query=load%20balancer).

## <a name="next-steps"></a>Próximas etapas
Confira [Atualizar um Load Balancer Básico](upgrade-basic-standard.md) para atualizar o Load Balancer Básico para o Standard Load Balancer.

Confira [Criar um Standard Load Balancer público](quickstart-load-balancer-standard-public-portal.md) para começar a usar um Load Balancer.

Para obter mais informações sobre as limitações e componentes do Azure Load Balancer, confira [Componentes do Azure Load Balancer](./components.md) e [Conceitos do Azure Load Balancer](./concepts.md)

Para obter uma comparação das opções de balanceamento de carga do Azure, confira [Visão geral das opções de balanceamento de carga no Azure](https://docs.microsoft.com/azure/architecture/guide/technology-choices/load-balancing-overview).
