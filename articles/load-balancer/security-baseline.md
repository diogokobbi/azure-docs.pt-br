---
title: Linha de base de segurança do Azure para Azure Load Balancer
description: A linha de base de segurança Azure Load Balancer fornece diretrizes de procedimento e recursos para implementar as recomendações de segurança especificadas no benchmark de segurança do Azure.
author: msmbaldwin
ms.service: load-balancer
ms.topic: conceptual
ms.date: 09/28/2020
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: e62da791e8c60f884855fba16315a03fe22cecb5
ms.sourcegitcommit: 3792cf7efc12e357f0e3b65638ea7673651db6e1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/29/2020
ms.locfileid: "91450682"
---
# <a name="azure-security-baseline-for-azure-load-balancer"></a>Linha de base de segurança do Azure para Azure Load Balancer

A linha de base de segurança do Azure para Microsoft Azure Load Balancer contém recomendações que o ajudarão a melhorar a postura de segurança de sua implantação. A linha de base para esse serviço é extraída do [Azure Security Benchmark versão 1.0](https://docs.microsoft.com/azure/security/benchmarks/overview), que fornece recomendações sobre como proteger suas soluções de nuvem no Azure com nossas diretrizes de melhores práticas. Para obter mais informações, consulte [Visão geral sobre linhas de base de segurança do Azure](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview).

## <a name="network-security"></a>Segurança de rede

*Para obter mais informações, consulte o [benchmark de segurança do Azure: segurança de rede](https://docs.microsoft.com/azure/security/benchmarks/security-control-network-security).*

### <a name="11-protect-azure-resources-within-virtual-networks"></a>1,1: proteger os recursos do Azure em redes virtuais

**Diretrizes**: Use balanceadores de carga internos do Azure para permitir somente o tráfego para recursos de back-end de determinadas redes virtuais ou redes virtuais emparelhadas sem exposição à Internet. Implemente um Load Balancer externo com SNAT (conversão de endereços de rede de origem) para mascarar os endereços IP de recursos de back-end para proteção contra exposição direta da Internet.

O Azure oferece dois tipos de ofertas de Load Balancer, Standard e Basic. Use o Standard Load Balancer para todas as cargas de trabalho de produção. Implemente grupos de segurança de rede e somente permita o acesso às portas confiáveis e aos intervalos de endereços IP do seu aplicativo. Nos casos em que não há nenhum grupo de segurança de rede atribuído à sub-rede de back-end ou NIC das máquinas virtuais de back-end, o tráfego não será permitido a esses recursos do balanceador de carga. Com os balanceadores de carga padrão, forneça regras de saída para definir o NAT de saída com um grupo de segurança de rede. Examine essas regras de saída para ajustar o comportamento de suas conexões de saída. 

O uso de um Standard Load Balancer é recomendado para suas cargas de trabalho de produção e, normalmente, o Load Balancer básico é usado apenas para teste, já que o tipo básico é aberto para conexões da Internet por padrão e não requer grupos de segurança de rede para a operação. 

- [Conexões de saída no Azure](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections#outboundrule)

- [Atualizar Load Balancer públicos do Azure](https://docs.microsoft.com/azure/load-balancer/upgrade-basic-standard)

**Monitoramento da Central de Segurança do Azure**: Sim

**Responsabilidade**: Cliente

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-virtual-networks-subnets-and-nics"></a>1,2: monitorar e registrar a configuração e o tráfego de redes virtuais, sub-redes e NICs

**Diretrizes**: a Load Balancer é um serviço de passagem, pois depende das regras de grupos de segurança de rede aplicadas aos recursos de back-end e às regras de saída configuradas para controlar o acesso à Internet.

Examine as regras de saída configuradas para seu Standard Load Balancer por meio da folha regras de saída do seu Load Balancer e da folha regras de balanceamento de carga em que você pode ter regras de saída implícitas habilitadas.

Monitore a contagem de suas conexões de saída para controlar com que frequência seus recursos estão chegando à Internet. 

Use a central de segurança e siga as recomendações de proteção de rede para ajudar a proteger os recursos de rede do Azure.

Siga as recomendações de segurança para seus recursos de back-end e habilite os logs de fluxo do grupo de segurança de rede e envie os logs para uma conta de armazenamento do Azure para auditoria.

Além disso, envie os logs de fluxo para um espaço de trabalho Log Analytics e, em seguida, use Análise de Tráfego para fornecer informações sobre os padrões de tráfego em sua nuvem do Azure. As vantagens do Análise de Tráfego incluem a capacidade de visualizar a atividade da rede, identificar pontos de acesso e ameaças à segurança, compreender os padrões de fluxo de tráfego e identificar as configurações incorretas da rede.

- [Como habilitar logs de fluxo do grupo de segurança de rede](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal)

- [Como habilitar e usar Análise de Tráfego](https://docs.microsoft.com/azure/network-watcher/traffic-analytics)

- [Entender a segurança de rede fornecida pela central de segurança do Azure](https://docs.microsoft.com/azure/security-center/security-center-network-recommendations)

- [Como fazer verificar minhas estatísticas de conexão de saída](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-diagnostics#how-do-i-check-my-outbound-connection-statistics)

**Monitoramento da Central de Segurança do Azure**: Sim

**Responsabilidade**: Cliente

### <a name="13-protect-critical-web-applications"></a>1.3: proteger aplicativos Web críticos

**Orientação**: defina explicitamente a conectividade com a Internet e os IPs de origem válidos por meio de regras de saída e grupos de segurança de rede com seu Load Balancer para usar a inteligência contra ameaças da Microsoft para proteger seus aplicativos Web.

- [Integrar o Firewall do Azure](https://docs.microsoft.com/azure/firewall/integrate-lb)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1.4: Rejeitar comunicações com endereços IP maliciosos conhecidos

**Orientação**: habilitar a proteção padrão de DDoS (negação de serviço distribuído) do Azure em sua rede virtual do Azure para proteger contra ataques de DDoS. 

Implante o Firewall do Azure em cada um dos limites de rede da organização com a filtragem baseada em inteligência contra ameaças habilitada e configurada para "alertar e negar" para tráfego de rede mal-intencionado.

 

Use a proteção contra ameaças da central de segurança para detectar comunicações com endereços IP mal-intencionados conhecidos. 

O Standard Load Balancer é projetado para ser seguro por padrão e parte de uma rede virtual privada e isolada. Ele é fechado para fluxos de entrada, a menos que sejam abertos por grupos de segurança de rede para permitir explicitamente o tráfego permitido e para não permitir endereços IP mal-intencionados conhecidos. A menos que um grupo de segurança de rede em uma sub-rede ou NIC de seu recurso de máquina virtual exista por trás do Load Balancer, o tráfego não tem permissão para acessar esse recurso. 

Implante o Firewall do Azure em cada um dos limites de rede da organização com a filtragem baseada em inteligência contra ameaças habilitada e configurada para "alertar e negar" para tráfego de rede mal-intencionado para evitar ataques de endereços IP mal-intencionados. 

Implemente diretrizes para integrar o Firewall do Azure com seu Load Balancer.

Use os recursos da proteção contra ameaças da central de segurança para detectar comunicações com endereços IP mal-intencionados conhecidos. 

A central de segurança (camada Standard) fornece acesso à máquina virtual just-in-time e configura os endereços IP de origem permitidos para permitir o acesso somente de intervalos/endereços IP aprovados.
 

Use o recurso de proteção de rede adaptável da central de segurança para recomendar configurações de grupo de segurança de rede que limitam portas e IPs de origem com base no tráfego real e na inteligência contra ameaças.
 

- [Gerenciar a Proteção contra DDoS do Azure Standard usando o portal do Azure](https://docs.microsoft.com/azure/virtual-network/manage-ddos`protection)

- [Filtragem baseada em inteligência contra ameaças do Firewall do Azure](https://docs.microsoft.com/azure/firewall/threat-intel)

- [Proteção contra ameaças na Central de Segurança do Azure](https://docs.microsoft.com/azure/security-center/threat-protection)

- [Proteja suas portas de gerenciamento com acesso just-in-time](https://docs.microsoft.com/azure/security-center/security-center-just-in-time)

- [Proteção de rede adaptável na central de segurança do Azure](https://docs.microsoft.com/azure/security-center/security-center-adaptive-network-hardening)

- [Integre o Firewall do Azure com seu Load Balancer](https://docs.microsoft.com/azure/firewall/overview)

**Monitoramento da Central de Segurança do Azure**: Sim

**Responsabilidade**: Cliente

### <a name="15-record-network-packets"></a>1,5: gravar pacotes de rede

**Orientação**: habilitar a captura de pacotes do observador de rede para investigar atividades anormais.

- [Como criar uma instância do observador de rede](https://docs.microsoft.com/azure/network-watcher/network-watcher-create)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1,6: implantar os sistemas de detecção de intrusão/prevenção de invasão baseado em rede (IDS/IPS)

**Diretrizes**: implemente uma oferta do Azure Marketplace que dá suporte à funcionalidade de IDS/IPS com recursos de inspeção de carga para o ambiente do seu Load Balancer. 

Use a inteligência contra ameaças do firewall do Azure se a inspeção de conteúdo não for um requisito. A filtragem baseada em inteligência de ameaças do firewall do Azure é usada para alertar e/ou bloquear o tráfego de e para domínios e endereços IP mal-intencionados conhecidos. Os endereços IP e os domínios são originados do feed de inteligência de ameaças da Microsoft.

Implante a solução de firewall de sua escolha em cada um dos limites de rede da sua organização para detectar e/ou bloquear o tráfego mal-intencionado.

- [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/?term=Firewall)

- [Como implantar o Firewall do Azure](https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal)

- [Como configurar alertas com o Firewall do Azure](https://docs.microsoft.com/azure/firewall/threat-intel)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

### <a name="17-manage-traffic-to-web-applications"></a>1.7: gerenciar o tráfego para aplicativos Web

**Orientação**: defina explicitamente a conectividade com a Internet e os IPs de origem válidos por meio de regras de saída e grupos de segurança de rede com seu Load Balancer para usar os recursos de inteligência contra ameaças da Microsoft para proteger seus aplicativos Web.

- [Integrar o Firewall do Azure](https://docs.microsoft.com/azure/firewall/integrate-lb)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1.8: Minimizar a complexidade e a sobrecarga administrativa de regras de segurança de rede

**Orientação**: use marcas de serviço no lugar de endereços IP específicos ao criar regras de segurança. Especifique o nome da marca de serviço no campo de origem ou de destino de uma regra para permitir ou negar o tráfego para o serviço correspondente. 

A Microsoft gerencia os prefixos de endereço englobados pela marca de serviço e atualiza automaticamente a marca de serviço em caso de alteração de endereços. 

Por padrão, cada grupo de segurança de rede inclui a marca de serviço AzureLoadBalancer para permitir o tráfego de investigação de integridade. 

Consulte a documentação do Azure para todas as marcas de serviço disponíveis para uso em regras de grupo de segurança de rede.

- [Marcas de serviço disponíveis](https://docs.microsoft.com/azure/virtual-network/service-tags-overview#available-service-tags)

**Monitoramento da Central de Segurança do Azure**: Sim

**Responsabilidade**: Cliente

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1.9: manter configurações de segurança padrão para dispositivos de rede

**Diretrizes**: defina e implemente configurações de segurança padrão para recursos de rede com Azure Policy.

Use os planos gráficos do Azure para simplificar implantações do Azure em larga escala empacotando artefatos de ambiente-chave, como modelos do Azure Resource Manager, controles RBAC do Azure e políticas, em uma única definição de Blueprint. 

Aplique o plano gráfico às novas assinaturas e ajuste o controle e o gerenciamento, por meio da versão.

- [Como configurar e gerenciar o Azure Policy](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

- [Exemplos de Azure Policy para rede](https://docs.microsoft.com/azure/governance/policy/samples/built-in-policies#network)

- [Como criar um blueprint do Azure](https://docs.microsoft.com/azure/governance/blueprints/create-blueprint-portal)

**Monitoramento da Central de Segurança do Azure**: Sim

**Responsabilidade**: Cliente

### <a name="110-document-traffic-configuration-rules"></a>1.10: documentar regras de configuração de tráfego

**Diretrizes**: use marcas de recurso para grupos de segurança de rede e outros recursos relacionados à segurança de rede e ao fluxo de tráfego. 

Use o campo "Descrição" para documentar as regras que permitem o tráfego de/para uma rede para regras de grupo de segurança de rede individuais.

Implemente qualquer uma das definições de Azure Policy internas relacionadas à marcação, como "exigir marca e seu valor", o que garante que todos os recursos sejam criados com marcas e notifiquem quaisquer recursos não marcados existentes.

Use Azure PowerShell ou CLI do Azure para pesquisar ou executar ações em recursos com base em suas marcas.

- [Como criar e usar marcas](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

- [Como criar uma rede virtual do Azure](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)

- [Como filtrar o tráfego de rede com regras de grupo de segurança de rede](https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic)

**Monitoramento da Central de Segurança do Azure**: Sim

**Responsabilidade**: Cliente

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1.11: usar ferramentas automatizadas para monitorar as configurações de recursos de rede e detectar alterações

**Orientação**: Use o log de atividades do Azure para monitorar as configurações de recursos e detectar alterações nos recursos do Azure. 

Crie alertas no Azure Monitor para notificá-lo quando os recursos críticos forem alterados.

- [Como exibir e recuperar eventos do log de atividades do Azure](https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-view)

- [Como criar alertas no Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log)

**Monitoramento da Central de Segurança do Azure**: Sim

**Responsabilidade**: Cliente

## <a name="logging-and-monitoring"></a>Log e monitoramento

*Para obter mais informações, consulte o [benchmark de segurança do Azure: registro em log e monitoramento](https://docs.microsoft.com/azure/security/benchmarks/security-control-logging-monitoring).*

### <a name="22-configure-central-security-log-management"></a>2.2: configurar o gerenciamento central de log de segurança

**Diretrizes**: examine as alterações em suas regras de saída e grupos de segurança de rede para seus balanceadores de carga exibindo o log de atividades em suas assinaturas. 

Exiba os logs de host internos para garantir que seus recursos de back-end sejam seguros.

Exporte esses logs para Log Analytics ou outra plataforma de armazenamento. No Azure Monitor, use espaços de trabalho do Log Analytics para consultar e executar análises e usar contas de armazenamento do Azure para armazenamento de longo prazo e arquivamento.

Habilite e integre esses dados ao Azure Sentinel ou a um SIEM de terceiros com base em seus requisitos de negócios organizacionais.

- [Como integrar o Azure Sentinel](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

- [Como coletar logs e métricas de plataforma com Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings)

- [Como coletar logs de host interno da máquina virtual do Azure com Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/learn/quick-collect-azurevm)

- [Introdução à integração do Azure Monitor e ao SIEM de terceiros](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/)

- [Logs de atividade da plataforma](https://docs.microsoft.com/azure/azure-monitor/platform/activity-log)

**Monitoramento da Central de Segurança do Azure**: Sim

**Responsabilidade**: Cliente

### <a name="23-enable-audit-logging-for-azure-resources"></a>2.3: habilitar o registro em log de auditoria para recursos do Azure

**Diretrizes**: revise as informações de log e auditoria do plano de gerenciamento e do controle capturadas com os logs de atividade para o Load Balancer básico. Essas configurações de captura são habilitadas por padrão. 

Use logs de atividade para monitorar ações em recursos para exibir todas as atividades e seu status. 

Determine quais operações foram executadas nos recursos em sua assinatura com os logs de atividade: 

- quem iniciou a operação
- quando a operação ocorreu
- o status da operação

- os valores de outras propriedades que podem ajudar você a pesquisar a operação

Recupere informações do log de atividades por meio do Azure PowerShell, a CLI (interface de linha de comando) do Azure, a API REST do Azure ou o portal do Azure. 

Implemente o diagnóstico multidimensional para os recursos de configurações de Standard Load Balancer com Azure Monitor.  Isso inclui as métricas disponíveis para segurança que incluem informações sobre conexões SNAT (conversão de endereço de rede de origem), portas. Métricas adicionais em pacotes SYN (Synchronize) e contadores de pacotes também estão disponíveis. 

Recuperar métricas multidimensionais programaticamente por meio de APIs e gravá-las em uma conta de armazenamento por meio da opção ' todas as métricas '.

Use o pacote de conteúdo dos logs de auditoria do Azure com o Microsoft Power BI para analisar seus dados com painéis pré-configurados ou para personalizar as exibições com base em seus requisitos.

Transmita logs para um hub de eventos ou um espaço de trabalho Log Analytics. Eles também podem ser extraídos do armazenamento de BLOBs do Azure e exibidos em diferentes ferramentas, como Excel e Power BI. 

Habilite e integre dados ao Azure Sentinel ou a um SIEM de terceiros com base em seus requisitos de negócios.

- [Leia este artigo com instruções passo a passo para cada método detalhado nas operações de auditoria com o Gerenciador de recursos](https://docs.microsoft.com/azure/azure-resource-manager/management/view-activity-logs)

- [Logs do Azure Monitor para o Basic Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-monitor-log)

- [Exibir logs de atividade para monitorar ações em recursos](https://docs.microsoft.com/azure/azure-resource-manager/management/view-activity-logs)

- [Recuperar as métricas multidimensionais programaticamente por meio de APIs](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-diagnostics#retrieve-multi-dimensional-metrics-programmatically-via-apis)

- [Introdução à integração do Azure Monitor e ao SIEM de terceiros](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools)

**Monitoramento da Central de Segurança do Azure**: Sim

**Responsabilidade**: Cliente

### <a name="25-configure-security-log-storage-retention"></a>2.5: Configurar a retenção de armazenamento do log de segurança

**Orientação**: o log de atividades é habilitado por padrão e é preservado por 90 dias no repositório de logs de eventos do Azure. Defina seu período de retenção de Log Analytics espaço de trabalho de acordo com os regulamentos de conformidade de sua organização em Azure Monitor. Use contas de armazenamento do Azure para armazenamento de longo prazo e arquivamento.

- [Exibir logs de atividades para monitorar ações no artigo de recursos](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-audit)

- [Alterar o período de retenção de dados em Log Analytics](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period)

- [Como configurar a política de retenção para logs de conta de armazenamento do Azure](https://docs.microsoft.com/azure/storage/common/storage-monitor-storage-account#configure-logging)

**Monitoramento da Central de Segurança do Azure**: Sim

**Responsabilidade**: Cliente

### <a name="26-monitor-and-review-logs"></a>2.6: monitorar e revisar logs

**Orientação**: monitorar, gerenciar e solucionar problemas Standard Load Balancer recursos usando a página Load Balancer no portal do Azure e a página Resource Health em Azure monitor. As métricas disponíveis para segurança incluem informações sobre conexões SNAT (conversão de endereço de rede de origem), portas. Além disso, as métricas em pacotes SYN (sincronizar) e contadores de pacotes também estão disponíveis. 

Use Azure Monitor para examinar o status da investigação de integridade do ponto de extremidade com métricas multidimensionais para balanceadores de carga padrão, externos e internos. Reúna essas métricas programaticamente por meio de APIs e gravadas em uma conta de armazenamento por meio da opção ' todas as métricas '.

Os logs de Azure Monitor não estão disponíveis para balanceadores de carga básicos internos. 

Use Azure Monitor para ver o status de investigação de integridade resumido por pool de back-end para o Load Balancer externo básico, como, o número de instâncias em seu pool de back-end não está recebendo solicitações do Load Balancer devido a falhas de investigação de integridade. 

Implemente o log com o Azure Operational insights para fornecer estatísticas sobre o status de integridade do balanceador de carga. 

Use os logs de atividade para exibir alertas e monitorar ações em recursos e seu status em suas assinaturas do Azure. Os logs de atividade são habilitados por padrão e podem ser exibidos no portal do Azure. 

Use o Microsoft Power BI com o pacote de conteúdo dos logs de auditoria do Azure e analise seus dados com painéis pré-configurados. Personalize exibições para atender às suas necessidades com base na necessidade de negócios. 

Transmita logs para um hub de eventos ou um espaço de trabalho Log Analytics. Eles também podem ser extraídos do armazenamento de BLOBs do Azure e exibidos em diferentes ferramentas, como Excel e Power BI. Você pode habilitar e integrar dados ao Azure Sentinel ou a um SIEM de terceiros.

- [Investigações de integridade do Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-custom-probe-overview)

- [API REST do Azure Monitor](https://docs.microsoft.com/rest/api/monitor)

- [Como recuperar métricas por meio da API REST](https://docs.microsoft.com/rest/api/monitor/metrics/list)

- [Diagnóstico de Standard Load Balancer com métricas, alertas e integridade de recursos](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-diagnostics)

- [Logs do Azure Monitor para o Basic Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-monitor-log)

- [Exibir suas métricas do balanceador de carga no portal do Azure](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-diagnostics#view-your-load-balancer-metrics-in-the-azure-portal)

**Monitoramento da Central de Segurança do Azure**: Sim

**Responsabilidade**: Cliente

### <a name="27-enable-alerts-for-anomalous-activities"></a>2,7: habilitar alertas para atividades anômalas

**Diretrizes**: Use a central de segurança com log Analytics espaço de trabalho para monitoramento e alertas sobre atividades anormais relacionadas a Load Balancer em eventos e logs de segurança.

Habilitar e integrar dados ao Azure Sentinel ou a uma ferramenta SIEM de terceiros.

- [Como integrar o Azure Sentinel](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

- [Como gerenciar alertas na central de segurança do Azure](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts)

- [Como alertar sobre dados de log do log Analytics](https://docs.microsoft.com/azure/azure-monitor/learn/tutorial-response)

**Monitoramento da Central de Segurança do Azure**: Sim

**Responsabilidade**: Cliente

### <a name="28-centralize-anti-malware-logging"></a>2.8: centralizar o registro em log de antimalware

**Orientação**: não aplicável a Azure Load Balancer. Essa recomendação destina-se a recursos de computação.

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

### <a name="29-enable-dns-query-logging"></a>2.9: habilitar o registro em log de consulta DNS

**Diretrizes**: não aplicável como Azure Load Balancer é um serviço de rede principal que não faz consultas DNS.

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

### <a name="210-enable-command-line-audit-logging"></a>2.10: habilitar o registro em log de auditoria de linha de comando

**Diretrizes**: não aplicável a Azure Load Balancer conforme essa recomendação se aplica aos recursos de computação.

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

## <a name="identity-and-access-control"></a>Identidade e controle de acesso

*Para obter mais informações, consulte o [benchmark de segurança do Azure: identidade e controle de acesso](https://docs.microsoft.com/azure/security/benchmarks/security-control-identity-access-control).*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3.1: manter um inventário de contas administrativas

**Diretrizes**: o Azure RBAC (controle de acesso baseado em função) do Azure permite que você gerencie o acesso aos recursos do Azure, como o Load Balancer por meio de atribuições de função. Atribua essas funções a usuários, grupos de entidades de serviço e identidades gerenciadas. 

Funções predefinidas e internas de inventário para determinados recursos com ferramentas como CLI do Azure, Azure PowerShell ou portal do Azure.

- [Como obter uma função de diretório no Azure AD com o PowerShell](https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrole?view=azureadps-2.0)

- [Como obter membros de uma função de diretório no Azure AD com o PowerShell](https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrolemember?view=azureadps-2.0)

**Monitoramento da Central de Segurança do Azure**: Sim

**Responsabilidade**: Cliente

## <a name="data-protection"></a>Proteção de dados

*Para obter mais informações, consulte o [benchmark de segurança do Azure: proteção de dados](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-protection).*

### <a name="46-use-azure-rbac-to-manage-access-to-resources"></a>4,6: usar o RBAC do Azure para gerenciar o acesso aos recursos

**Orientação**: Use o RBAC do Azure para controlar o acesso aos seus recursos de Load Balancer.

- [Como configurar o RBAC no Azure](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal)

**Monitoramento da Central de Segurança do Azure**: Sim

**Responsabilidade**: Cliente

### <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4.7: usar a prevenção contra perda de dados baseada em host para impor controle de acesso

**Diretrizes**: Load Balancer é um serviço de passagem que não armazena dados do cliente. Ele faz parte da plataforma subjacente que é gerenciada pela Microsoft. 

A Microsoft trata todo o conteúdo do cliente como confidencial e vai para uma grande quantidade de comprimentos para se proteger contra a perda e a exposição de dados do cliente. 

Para garantir que os dados do cliente no Azure permaneçam seguros, a Microsoft implementou e mantém um conjunto de recursos e controles robustos de proteção de dados. 

- [Entender a proteção de dados do cliente no Azure](https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Compartilhado

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4.9: Registrar e alertar sobre alterações em recursos críticos do Azure

**Diretrizes**: Use Azure monitor com o log de atividades do Azure para criar alertas quando as alterações ocorrerem para recursos críticos do Azure, como balanceadores de carga usados para cargas de trabalho de produção importantes.

- [Como criar alertas para eventos do log de atividades do Azure](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log)

**Monitoramento da Central de Segurança do Azure**: Sim

**Responsabilidade**: Cliente

## <a name="inventory-and-asset-management"></a>Inventário e gerenciamento de ativos

*Para obter mais informações, consulte o [benchmark de segurança do Azure: inventário e gerenciamento de ativos](https://docs.microsoft.com/azure/security/benchmarks/security-control-inventory-asset-management).*

### <a name="61-use-automated-asset-discovery-solution"></a>6,1: usar solução de descoberta de ativos automatizada

**Orientação**: Use o grafo de recursos do Azure para consultar e descobrir todos os recursos (como computação, armazenamento, rede, portas, protocolos e assim por diante) em suas assinaturas. Azure Resource Manager é recomendável para criar e usar os recursos atuais. 

Verifique as permissões apropriadas (leitura) no seu locatário e enumere todas as assinaturas e recursos do Azure em suas assinaturas.

- [Como criar consultas com o Gerenciador de gráficos de recursos do Azure](https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal)

- [Como exibir suas assinaturas do Azure](https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription?view=azps-3.0.0)

- [Entender o RBAC do Azure](https://docs.microsoft.com/azure/role-based-access-control/overview)

**Monitoramento da Central de Segurança do Azure**: Sim

**Responsabilidade**: Cliente

### <a name="62-maintain-asset-metadata"></a>6.2: manter metadados de ativo

**Diretrizes**: aplique marcas aos recursos do Azure com metadados para organizar logicamente de acordo com uma taxonomia.

- [Como criar e usar marcas](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

### <a name="63-delete-unauthorized-azure-resources"></a>6.3: excluir recursos do Azure não autorizados

**Orientação**: use marcação, grupos de gerenciamento e assinaturas separadas, quando apropriado, para organizar e acompanhar ativos. 

Reconcilie o inventário regularmente e garanta que os recursos não autorizados sejam excluídos de suas assinaturas em tempo hábil.

- [Como criar assinaturas adicionais do Azure](https://docs.microsoft.com/azure/billing/billing-create-subscription)

- [Como criar grupos de gerenciamento](https://docs.microsoft.com/azure/governance/management-groups/create)

- [Como criar e usar marcas](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Monitoramento da Central de Segurança do Azure**: Sim

**Responsabilidade**: Cliente

### <a name="64-define-and-maintain-an-inventory-of-approved-azure-resources"></a>6,4: definir e manter um inventário de recursos aprovados do Azure

**Diretrizes**: Crie uma lista de recursos aprovados do Azure de acordo com suas necessidades organizacionais, que você pode aproveitar como um mecanismo de lista de permissões. Isso permitirá que sua organização integre quaisquer serviços do Azure recentemente disponíveis depois que eles forem formalmente revisados e aprovados pelos processos de avaliação de segurança típicos da sua organização.

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

### <a name="65-monitor-for-unapproved-azure-resources"></a>6.5: monitorar recursos do Azure não aprovados

**Diretrizes**: Use Azure Policy para colocar restrições no tipo de recursos que podem ser criados em suas assinaturas.

Consulte e descubra recursos com o grafo de recursos do Azure nas assinaturas de propriedade. 

Verifique se todos os recursos do Azure presentes no ambiente foram aprovados.

- [Como configurar e gerenciar o Azure Policy](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

- [Como criar consultas com o Gerenciador de gráficos de recursos do Azure](https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

### <a name="611-limit-users-ability-to-interact-with-azure-resource-manager"></a>6,11: limitar a capacidade dos usuários de interagir com Azure Resource Manager

**Orientação**: Use o acesso condicional do Azure ad para limitar a capacidade dos usuários de interagir com Azure Resource Manager Configurando "bloquear acesso" para o aplicativo de "gerenciamento de Microsoft Azure".

- [Como configurar o acesso condicional para bloquear o acesso ao Gerenciador de recursos do Azure](https://docs.microsoft.com/azure/role-based-access-control/conditional-access-azure-management)

**Monitoramento da Central de Segurança do Azure**: Sim

**Responsabilidade**: Cliente

### <a name="613-physically-or-logically-segregate-high-risk-applications"></a>6.13: Separar física ou logicamente os aplicativos de alto risco

**Orientação**: software necessário para operações de negócios, mas pode incorrer em um risco maior para a organização, deve ser isolado em sua própria máquina virtual e/ou rede virtual e é suficientemente protegido com um firewall do Azure ou um grupo de segurança de rede.

- [Como criar uma rede virtual](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)

- [Como criar um grupo de segurança de rede com uma configuração de segurança](https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

## <a name="secure-configuration"></a>Configuração segura

*Para obter mais informações, consulte o [benchmark de segurança do Azure: configuração segura](https://docs.microsoft.com/azure/security/benchmarks/security-control-secure-configuration).*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7.1: estabelecer configurações seguras para todos os recursos do Azure

**Orientação**: use aliases de Azure Policy para criar políticas personalizadas para auditar ou impor a configuração dos recursos do Azure. Use definições de Azure Policy internas.

Azure Resource Manager tem a capacidade de exportar o modelo no JavaScript Object Notation (JSON), que deve ser revisado para garantir que as configurações atendam aos requisitos de segurança da sua organização.

Exporte modelos de Azure Resource Manager em formatos de JavaScript Object Notation (JSON) e examine-os periodicamente para garantir que as configurações atendam aos requisitos de segurança organizacional. 

Implemente as recomendações da central de segurança como uma linha de base de configuração segura para os recursos do Azure. 

- [Como exibir os aliases de Azure Policy disponíveis](https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-3.3.0)

- [Tutorial: Criar e gerenciar políticas para impor a conformidade](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

- [Exportação única e de vários recursos para um modelo no portal do Azure](https://docs.microsoft.com/azure/azure-resource-manager/templates/export-template-portal)

- [Recomendações de segurança – um guia de referência](https://docs.microsoft.com/azure/security-center/recommendations-reference)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

### <a name="73-maintain-secure-azure-resource-configurations"></a>7.3: Manter configurações seguras de recursos do Azure

**Diretriz**: use o Azure Policy [negar] e [implantar se não existir] para impor configurações seguras em seus recursos do Azure.  Além disso, você pode usar modelos de Azure Resource Manager para manter a configuração de segurança dos recursos do Azure exigidos por sua organização. 

- [Entender Azure Policy efeitos](https://docs.microsoft.com/azure/governance/policy/concepts/effects)

- [Criar e gerenciar políticas para impor a conformidade](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

- [Visão geral de modelos de Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/templates/overview)

**Monitoramento da Central de Segurança do Azure**: Sim

**Responsabilidade**: Cliente

### <a name="75-securely-store-configuration-of-azure-resources"></a>7.5: armazenar configuração de recursos do Azure com segurança

**Diretrizes**: Use o Azure DevOps para armazenar e gerenciar com segurança seu código, como definições de Azure Policy personalizadas, modelos de Azure Resource Manager e scripts de configuração de estado desejado. 

Conceda ou negue permissões a usuários específicos, grupos de segurança internos ou grupos definidos no Azure Active Directory (Azure AD) se ele estiver integrado com o Azure DevOps ou no Active Directory se estiver integrado com o TFS.

- [Como armazenar código no Azure DevOps](https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops)

- [Sobre permissões e grupos no Azure DevOps](https://docs.microsoft.com/azure/devops/organizations/security/about-permissions)

**Monitoramento da Central de Segurança do Azure**: Sim

**Responsabilidade**: Cliente

### <a name="77-deploy-configuration-management-tools-for-azure-resources"></a>7,7: implantar as ferramentas de gerenciamento de configuração para recursos do Azure

**Orientação**: definir e implementar configurações de segurança padrão para recursos do Azure usando Azure Policy.  Use aliases de Azure Policy para criar políticas personalizadas para auditar ou impor a configuração de rede dos recursos do Azure. Implemente definições de políticas internas relacionadas aos seus recursos específicos do Azure Load Balancer.  Além disso, use a automação do Azure para implantar alterações de configuração. para seus recursos de Azure Load Balancer.

- [Como configurar e gerenciar o Azure Policy](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

- [Como usar aliases](https://docs.microsoft.com/azure/governance/policy/concepts/definition-structure#aliases)

**Monitoramento da Central de Segurança do Azure**: Sim

**Responsabilidade**: Cliente

### <a name="79-implement-automated-configuration-monitoring-for-azure-resources"></a>7,9: implementar o monitoramento automatizado de configuração para recursos do Azure

**Diretrizes**: Use a central de segurança para executar verificações de linha de base para seus recursos do Azure e Azure Policy para alertar e auditar configurações de recursos.

- [Como corrigir recomendações na central de segurança do Azure](https://docs.microsoft.com/azure/security-center/security-center-remediate-recommendations)

**Monitoramento da Central de Segurança do Azure**: Sim

**Responsabilidade**: Cliente

## <a name="incident-response"></a>Resposta a incidentes

*Para obter mais informações, consulte o [benchmark de segurança do Azure: resposta a incidentes](https://docs.microsoft.com/azure/security/benchmarks/security-control-incident-response).*

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10.2: criar um procedimento de pontuação e priorização de incidentes

**Diretriz**: a Central de Segurança atribui uma severidade a cada alerta para ajudar você a priorizar quais alertas devem ser investigados primeiro. 

A gravidade se baseia em quão confiante a central de segurança está na localização ou na análise usada para emitir o alerta, bem como o nível de confiança de que houve uma intenção mal-intencionada por trás da atividade que levou ao alerta.

Marque as assinaturas usando marcas e crie um sistema de nomeação para identificar e categorizar os recursos do Azure, especialmente aqueles que processam dados confidenciais.  

É sua responsabilidade priorizar a correção de alertas com base na criticalidade dos recursos do Azure e do ambiente em que o incidente ocorreu.

- [Alertas na Central de Segurança do Azure](https://docs.microsoft.com/azure/security-center/security-center-alerts-overview)

- [Usar marcas para organizar seus recursos do Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Monitoramento da Central de Segurança do Azure**: Sim

**Responsabilidade**: Cliente

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10.5: incorporar alertas de segurança em seu sistema de resposta a incidentes

**Diretrizes**: exporte seus alertas e recomendações da central de segurança usando o recurso de exportação contínua para ajudar a identificar os riscos para os recursos do Azure. 

Use o recurso de exportação contínua na central de segurança que permite exportar alertas e recomendações de forma manual ou contínua, de modo contínuo. 

Utilize o conector de dados da central de segurança para transmitir os alertas para o Azure Sentinel.

- [Como configurar a exportação contínua](https://docs.microsoft.com/azure/security-center/continuous-export)

- [Como transmitir alertas para o Azure Sentinel](https://docs.microsoft.com/azure/sentinel/connect-azure-security-center)

**Monitoramento da Central de Segurança do Azure**: Sim

**Responsabilidade**: Cliente

### <a name="106-automate-the-response-to-security-alerts"></a>10.6: automatizar a resposta a alertas de segurança

**Diretrizes**: Use o recurso de automação de fluxo de trabalho na central de segurança para disparar automaticamente respostas a alertas de segurança e recomendações para proteger os recursos do Azure.

- [Como configurar a automação do fluxo de trabalho na segurança Enter](https://docs.microsoft.com/azure/security-center/workflow-automation)

**Monitoramento da Central de Segurança do Azure**: Sim

**Responsabilidade**: Cliente

## <a name="penetration-tests-and-red-team-exercises"></a>Testes de penetração e exercícios de Red Team

*Para obter mais informações, consulte o [benchmark de segurança do Azure: testes de penetração e exercícios de equipe vermelho](https://docs.microsoft.com/azure/security/benchmarks/security-control-penetration-tests-red-team-exercises).*

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-remediation-of-all-critical-security-findings"></a>11,1: realize testes de penetração regulares de seus recursos do Azure e garanta a correção de todas as descobertas de segurança críticas

**Diretrizes**: siga as regras de teste de penetração Microsoft Cloud do Engagement para garantir que seus testes de penetração não estejam violando as políticas da Microsoft. Use a estratégia da Microsoft e a execução de equipes vermelhas e testes de penetração de sites ativos em infraestrutura de nuvem, serviços e aplicativos gerenciados pela Microsoft. 

- [Regras de teste de penetração do Engagement](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1) 

- [Microsoft Cloud o agrupamento vermelho](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Compartilhado

## <a name="next-steps"></a>Próximas etapas

- Confira o [Azure Security Benchmark](/azure/security/benchmarks/overview)
- Saiba mais sobre a [Linhas de base de segurança do Azure](/azure/security/benchmarks/security-baselines-overview)
