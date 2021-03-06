---
title: Conceitos do Azure Load Balancer
description: Visão geral dos conceitos do Azure Load Balancer
services: load-balancer
documentationcenter: na
author: asudbring
ms.service: load-balancer
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/13/2020
ms.author: allensu
ms.openlocfilehash: 9765f685f2fccc9332a2f07d907aac415aa2c57f
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91333912"
---
# <a name="azure-load-balancer-concepts"></a>Conceitos do Azure Load Balancer

O balanceador de carga fornece várias funcionalidades para os aplicativos UDP e TCP. 

## <a name="load-balancing-algorithm"></a>Algoritmo de balanceamento de carga
Você pode criar uma regra de balanceamento de carga para distribuir o tráfego do front-end para um pool de back-end. O Azure Load Balancer usa um algoritmo de hash para distribuição de fluxos de entrada (não bytes). O balanceador de carga reescreve os cabeçalhos de fluxos para instâncias de pool de back-end. Um servidor fica disponível para receber novos fluxos quando a investigação de integridade indica que o ponto de extremidade de back-end é íntegro.

Por padrão, o balanceador de carga usa um hash de cinco tuplas.

O hash inclui:

- **Endereço IP de origem**
- **Porta de origem**
- **Endereço IP de destino**
- **Porta de destino**
- **Número do protocolo IP para mapear os fluxos para servidores disponíveis**

A afinidade com um endereço IP de origem é criada usando um hash de duas ou três tuplas. Pacotes do mesmo fluxo chegam na mesma instância por trás do front-end com balanceamento de carga. 

A porta de origem é alterada quando um cliente inicia um novo fluxo do mesmo IP de origem. Dessa forma, o hash de cinco tuplas podem fazer com que o tráfego vá para um ponto de extremidade de back-end diferente.
Para saber mais, confira [Configurar o modo de distribuição para o Azure Load Balancer](./load-balancer-distribution-mode.md).

A imagem a seguir mostra a distribuição baseada em hash:

![Distribuição baseada em hash](./media/load-balancer-overview/load-balancer-distribution.png)

*Figura: distribuição baseada em hash*

## <a name="application-independence-and-transparency"></a>Independência e transparência de aplicativo

O balanceador de carga não interage diretamente com o TCP ou UDP, nem com a camada de aplicativo. Qualquer cenário de aplicativo TCP ou UDP pode ser compatível. O balanceador de carga não fecha nem origina fluxos e nem interage com o conteúdo do fluxo. O balanceador de carga não fornece a funcionalidade de gateway de camada de aplicativo. Os handshakes de protocolo sempre ocorrem diretamente entre o cliente e a instância do pool de back-end. Uma resposta para um fluxo de entrada sempre é uma resposta de uma máquina virtual. Quando o fluxo chega na máquina virtual, o endereço IP de origem original também é preservado.

* Cada ponto de extremidade é respondido por uma VM. Por exemplo, um handshake TCP ocorre entre o cliente e a VM de back-end selecionada. Uma resposta a uma solicitação feita a um front-end é uma resposta gerada por uma VM de back-end. Ao validar com êxito a conectividade com um front-end, você está validando a conectividade total com pelo menos uma máquina virtual de back-end.
* O conteúdo do aplicativo é transparente para o balanceador de carga. Qualquer aplicativo UDP ou TCP pode ser compatível.
* Como o balanceador de carga não interage com o conteúdo de TCP e fornece o descarregamento de TLS, você pode criar cenários criptografados abrangentes. O uso do balanceador de carga apresenta uma grande expansão para aplicativos TLS encerrando a conexão TLS na VM em si. Por exemplo, a capacidade de chaveamento de sua sessão TLS é limitada apenas pelo tipo e pelo número de VMs adicionadas ao pool de back-end.

## <a name="outbound-connections"></a>Conexões de saída 

Os fluxos do pool de back-end para IPs públicos são mapeados para o front-end. O Azure traduz as conexões de saída para o endereço IP de front-end público por meio da regra de saída de balanceamento de carga. Essa configuração tem as vantagens a seguir. Facilidade de upgrade e recuperação de desastre dos serviços, uma vez que o front-end pode ser mapeado dinamicamente para outra instância do serviço. Gerenciamento mais fácil da ACL (lista de controle de acesso). As ACLs expressas como IPs de front-end não são alteradas à medida que os serviços são escalados verticalmente para cima ou para baixo, ou quando são reimplantados. Mover conexões de saída para um número menor de endereços IP do que computadores reduz a carga de implementar listas seguras de destinatários. Para saber mais sobre a conversão de endereços de rede de origem (SNAT) e Azure Load Balancer, confira [SNAT e Azure Load Balancer](load-balancer-outbound-connections.md).

## <a name="availability-zones"></a>Zonas de Disponibilidades 

O Standard Load Balancer é compatível com recursos adicionais em regiões em que as Zonas de Disponibilidade estão disponíveis. As configurações de Zonas de Disponibilidade estão disponíveis para ambos os tipos de Standard Load Balancer: público e interno. Um front-end com redundância de zona sobrevive a falha da zona usando uma infraestrutura dedicada em todas as zonas simultaneamente. Além disso, você pode garantir um front-end para uma zona específica. Um front-end zonal é atendido por infraestrutura dedicada em uma zona. O balanceamento de carga entre zonas está disponível para o pool de back-end. Qualquer recurso de máquina virtual em uma rede virtual pode fazer parte de um pool de back-end. O balanceador de carga básico não é compatível com zonas. Examine a [discussão detalhada das habilidades relacionadas às Zonas de Disponibilidade](load-balancer-standard-availability-zones.md) e a [Visão Geral de Zonas de Disponibilidade](../availability-zones/az-overview.md) para saber mais.

## <a name="ha-ports"></a>Portas de HA

É possível configurar as regras de balanceamento de carga da porta de HA para dimensionar seu aplicativo e ser altamente confiável. O balanceamento de carga por fluxo em portas de curta duração do IP de front-end do balanceador de carga interno é fornecido por essas regras. O recurso é útil quando é impraticável ou indesejável especificar portas individuais. Uma regra de portas de HA permite que você crie cenários ativo-passivo ou ativo-ativo n+1. Esses cenários são para soluções de virtualização de rede e qualquer aplicativo que exija grandes intervalos de portas de entrada. Uma investigação de integridade pode ser usada para determinar quais back-ends devem receber novos fluxos.  Você pode usar um Grupo de Segurança de Rede para emular um cenário de intervalo de porta. O balanceador de carga Básico não é compatível com portas de HA. Revisão [da discussão detalhada de Portas de alta disponibilidade](load-balancer-ha-ports-overview.md).

## <a name="multiple-frontends"></a>Vários front-ends 

O balanceador de carga é compatível com várias regras com vários front-ends.  O Standard Load Balancer expande essa funcionalidade para cenários de saída. As regras de saída são o inverso de uma regra de entrada. A regra de saída cria uma associação para conexões de saída. O Standard Load Balancer usa todos os front-ends associados a um recurso de máquina virtual por meio de uma regra de balanceamento de carga. Além disso, um parâmetro na regra de balanceamento de carga permite que você suprima uma regra desse tipo para fins de conectividade de saída, o que permite a seleção de front-ends específicos, incluindo nenhum. Para comparação, o Basic Load Balancer seleciona um front-end aleatoriamente. Não há habilidade de controlar qual front-end foi selecionado.

## <a name="floating-ip"></a>IP flutuante

Alguns cenários de aplicativos preferem ou exigem a utilização da mesma porta por várias instâncias do aplicativo em uma VM no pool de back-end. Exemplos comuns de reutilização de porta incluem clusters de alta disponibilidade, dispositivos virtuais de rede e exposição de vários pontos de extremidade TLS sem nova criptografia. Se você quiser reutilizar a porta de back-end em várias regras, habilite o IP Flutuante na definição da regra.

**IP flutuante** é a terminologia do Azure para uma parte do que é conhecido como DSR (Retorno de Servidor Direto). O DSR consiste em duas partes: 

- Topologia de fluxo
- Um esquema de mapeamento de endereço IP

Em um nível de plataforma, o Azure Load Balancer sempre opera em uma topologia de fluxo de DSR, independentemente de o IP Flutuante estar habilitado ou não. Isso significa que a parte de saída de um fluxo é sempre reescrita corretamente para fluir diretamente de volta para a origem.
Com o IP Flutuante, o Azure expõe um esquema de mapeamento de endereço IP de balanceamento de carga tradicional para facilitar o uso (o IP de instâncias de VM). A habilitação do IP Flutuante altera o mapeamento de endereço IP para o IP de front-end do balanceador de carga para possibilitar flexibilidade adicional. Saiba mais [aqui](load-balancer-multivip-overview.md).


## <a name="limitations"></a><a name = "limitations"></a>Limitações

- No momento, não há suporte para o IP flutuante em configurações de IP secundárias para cenários de balanceamento de carga interno ou de balanceamento de carga público.

- Uma regra de balanceador de carga não pode abranger duas redes virtuais.  Os front-ends e as instâncias de back-end deles devem estar localizados na mesma rede virtual.  

- Funções de Trabalho da Web sem uma rede virtual e outros serviços da plataforma Microsoft podem ser acessados de instâncias atrás de apenas um balanceador de carga Standard interno. Não confie nessa acessibilidade, pois o respectivo serviço em si ou a plataforma subjacente a pode ser alterada sem aviso prévio. Se for necessária conectividade de saída ao usar um balanceador de carga interno padrão, a [conectividade de saída](load-balancer-outbound-connections.md) deverá ser configurada.

- O balanceador de carga fornece balanceamento de carga e encaminhamento de porta para protocolos TCP e UDP específicos. As regras de balanceamento de carga e as regras NAT de entrada são compatíveis com TCP e UDP, mas não com outros protocolos IP, incluindo ICMP.

- O fluxo de saída de uma VM de back-end para um front-end de um Load Balancer interno falhará.

- Não há suporte para o encaminhamento de fragmentos de IP nas regras de balanceamento de carga. A fragmentação de IP de pacotes UDP e TCP não é permitida em regras de balanceamento de carga. As regras de balanceamento de carga das portas de HA podem ser usadas para encaminhar fragmentos IP existentes. Para saber mais, confira [Visão geral das portas de alta disponibilidade](load-balancer-ha-ports-overview.md).

## <a name="next-steps"></a>Próximas etapas

- Confira [Criar um Standard Load Balancer público](quickstart-load-balancer-standard-public-portal.md) para começar a usar um Load Balancer: crie VMs com uma extensão de IIS personalizada instalada e faça o balanceamento de carga do aplicativo Web entre as VMs.
- Aprenda sobre [Conexões de saída do Azure Load Balancer](load-balancer-outbound-connections.md).
- Saiba mais sobre o [Azure Load Balancer](load-balancer-overview.md).
- Saiba mais sobre [Investigações de Integridade](load-balancer-custom-probe-overview.md).
- Saiba mais sobre o [Diagnóstico do Load Balancer Standard](load-balancer-standard-diagnostics.md).
- Saiba mais sobre [Grupos de Segurança de Rede](../virtual-network/security-overview.md).
