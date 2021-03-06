---
title: Linha de base de segurança do Azure para Azure Monitor
description: A linha de base de segurança Azure Monitor fornece diretrizes de procedimento e recursos para implementar as recomendações de segurança especificadas no benchmark de segurança do Azure.
author: msmbaldwin
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/01/2020
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: fd68f720f372ee61f7c441ea83bd365bc2a6f36a
ms.sourcegitcommit: 3fc3457b5a6d5773323237f6a06ccfb6955bfb2d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/11/2020
ms.locfileid: "90032734"
---
# <a name="azure-security-baseline-for-azure-monitor"></a>Linha de base de segurança do Azure para Azure Monitor

Essa linha de base de segurança aplica diretrizes do [benchmark de segurança do Azure](../security/benchmarks/overview.md) para Azure monitor. O Azure Security Benchmark fornece recomendações sobre como você pode proteger suas soluções de nuvem no Azure. O conteúdo é agrupado pelos **controles de segurança** definidos pelo benchmark de segurança do Azure e pelas diretrizes relacionadas aplicáveis ao Azure monitor. **Controles** não aplicáveis a Azure monitor foram excluídos. Para ver como Azure Monitor é completamente mapeado para o benchmark de segurança do Azure, consulte o [arquivo completo de mapeamento de linha de base de segurança Azure monitor](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines).

Azure Monitor faz parte dos serviços principais do Azure e o serviço de Azure Monitor não pode ser implantado como um serviço separadamente. Azure Monitor componentes podem ser implantados com seus recursos, e isso pode afetar a postura de segurança desses recursos.

## <a name="network-security"></a>Segurança de rede

*Para obter mais informações, consulte o [benchmark de segurança do Azure: segurança de rede](/azure/security/benchmarks/security-control-network-security).*

### <a name="11-protect-azure-resources-within-virtual-networks"></a>1,1: proteger os recursos do Azure em redes virtuais

**Orientação**: habilitar o link privado do Azure para permitir o acesso aos serviços SaaS do Azure (por exemplo, Azure monitor) e serviços hospedados de cliente/parceiro do Azure por meio de um ponto de extremidade privado em sua rede virtual. O tráfego entre a rede virtual e o serviço percorre a rede de backbone da Microsoft, eliminando a exposição da Internet pública.

Para permitir que o tráfego alcance Azure Monitor, use as marcas de serviço "AzureMonitor" para permitir o tráfego de entrada e saída por meio de grupos de segurança de rede. Para permitir que o tráfego de teste de monitoramento de disponibilidade alcance Azure Monitor, use a marca de serviço "ApplicationInsightsAvailability" para todo o tráfego de entrada por meio de grupos de segurança de rede.

As regras de rede virtual permitem que Azure Monitor só aceitem comunicações que são enviadas de sub-redes selecionadas dentro de uma rede virtual.

Use Log Analytics gateway para enviar dados a um espaço de trabalho do Log Analytics no Azure Monitor em nome dos computadores que não podem se conectar diretamente à Internet, impedindo a necessidade de computadores conectados à Internet. 

- [Como configurar o link privado para Azure Monitor](platform/private-link-security.md)

- [Conectar computadores sem acesso à Internet usando o gateway de Log Analytics no Azure Monitor](platform/gateway.md)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-virtual-networks-subnets-and-network-interfaces"></a>1,2: monitorar e registrar a configuração e o tráfego de redes virtuais, sub-redes e interfaces de rede

**Diretrizes**: Azure monitor é um serviço principal e não oferece suporte à implantação diretamente em uma rede virtual, sua infraestrutura subjacente é tratada pela Microsoft. No entanto, para recursos que fazem conexões de rede com a oferta de Azure Monitor, proteja sua rede com um grupo de segurança de rede. Habilite logs de fluxo do grupo de segurança de rede e envie logs para uma conta de armazenamento para auditoria de tráfego. Você também pode enviar logs de fluxo para um espaço de trabalho Log Analytics e usar Análise de Tráfego para fornecer informações sobre o fluxo de tráfego em sua nuvem do Azure. Algumas vantagens da Análise de Tráfego são a capacidade de visualizar a atividade de rede e identificar pontos de acesso, identificar ameaças à segurança, compreender os padrões de fluxo de tráfego e identificar configurações incorretas de rede de pinpoint.

Ao usar Azure Monitor com o link privado, você obtém acesso ao log de rede, como ' dados processados pelo ponto de extremidade privado (IN/OUT) '.

- [Requisitos de rede para agentes de Azure Monitor](platform/log-analytics-agent.md#network-requirements)

- [Conectar computadores sem acesso à Internet usando o gateway de Log Analytics no Azure Monitor](platform/gateway.md)

- [Como habilitar logs de fluxo do grupo de segurança de rede](../network-watcher/network-watcher-nsg-flow-logging-portal.md)

- [Como habilitar e usar Análise de Tráfego](../network-watcher/traffic-analytics.md)

- [Entender a segurança de rede fornecida pela central de segurança do Azure](../security-center/security-center-network-recommendations.md)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1.8: Minimizar a complexidade e a sobrecarga administrativa de regras de segurança de rede

**Orientação**: para permitir que o tráfego alcance Azure monitor, use as marcas de serviço "AzureMonitor" para permitir o tráfego de entrada e de saída por meio de grupos de segurança de rede. Para permitir que o tráfego de teste de monitoramento de disponibilidade alcance Azure Monitor, use a marca de serviço "ApplicationInsightsAvailability" para todo o tráfego de entrada por meio de grupos de segurança de rede. A Microsoft gerencia os prefixos de endereço englobados pela marca de serviço e atualiza automaticamente a marca de serviço em caso de alteração de endereços.

- [Entender e usar marcas de serviço](../virtual-network/service-tags-overview.md) 

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

### <a name="110-document-traffic-configuration-rules"></a>1.10: documentar regras de configuração de tráfego

**Diretrizes**: Azure monitor faz parte dos serviços principais do Azure e não pode ser implantada como um serviço separadamente. Azure Monitor componentes, incluindo o agente de Azure Monitor, e o SDK do Application Insights podem ser implantados com seus recursos, e isso pode afetar a postura de segurança desses recursos.

- [Requisitos de rede para agentes de Azure Monitor](platform/log-analytics-agent.md#network-requirements)

- [Conectar computadores sem acesso à Internet usando o gateway de Log Analytics no Azure Monitor](platform/gateway.md) 

- [Consulte Introdução ao Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview#get-started)

- [Como configurar testes da Web de disponibilidade](app/monitor-web-app-availability.md)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1.11: usar ferramentas automatizadas para monitorar as configurações de recursos de rede e detectar alterações

**Diretrizes**: Use o log de atividades do Azure para monitorar as configurações de recursos e detectar alterações nos recursos de rede relacionados a Azure monitor. Crie alertas dentro de Azure Monitor que serão disparados quando ocorrerem alterações nesses recursos de rede críticos.

- [Como exibir e recuperar eventos do log de atividades do Azure](/azure/azure-monitor/platform/activity-log-view)

- [Como criar alertas no Azure Monitor](platform/alerts-activity-log.md)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

## <a name="logging-and-monitoring"></a>Log e monitoramento

*Para obter mais informações, consulte o [benchmark de segurança do Azure: registro em log e monitoramento](/azure/security/benchmarks/security-control-logging-monitoring).*

### <a name="22-configure-central-security-log-management"></a>2.2: configurar o gerenciamento central de log de segurança

**Diretrizes**: o Azure monitor usa logs de atividade para registrar alterações em seus recursos. Você pode exportar esses logs para o armazenamento do Azure, Hub de eventos ou um espaço de trabalho Log Analytics. Os logs de ingestão por meio de Azure Monitor para agregar dados de segurança gerados por dispositivos de ponto de extremidade, recursos de rede e outros sistemas de segurança. Em Azure Monitor, você pode consultar e executar análises em relação aos dados, usar contas de armazenamento do Azure para qualquer armazenamento de longo prazo/arquivamento de logs.

Como alternativa, você pode habilitar e integrar dados ao Azure Sentinel ou a um SIEM de terceiros.

- [Como coletar logs e métricas de plataforma com Azure Monitor](platform/diagnostic-settings.md)

- [Como coletar logs de host interno da máquina virtual do Azure com Azure Monitor](learn/quick-collect-azurevm.md)

- [Como integrar o Azure Sentinel](../sentinel/quickstart-onboard.md)

- [Introdução à integração do Azure Monitor e ao SIEM de terceiros](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

### <a name="23-enable-audit-logging-for-azure-resources"></a>2.3: habilitar o registro em log de auditoria para recursos do Azure

**Diretrizes**: Azure monitor usa logs de atividade, o log de atividades é habilitado automaticamente e registra as operações executadas em recursos de Azure monitor, como: quem iniciou a operação, quando a operação ocorreu, o status da operação e outras informações úteis de auditoria. 

- [Como coletar logs e métricas de plataforma com Azure Monitor](platform/diagnostic-settings.md)

- [Entender o registro em log e diferentes tipos de log no Azure](platform/platform-logs-overview.md)

**Monitoramento da Central de Segurança do Azure**: Não disponível no momento

**Responsabilidade**: Cliente

### <a name="25-configure-security-log-storage-retention"></a>2.5: Configurar a retenção de armazenamento do log de segurança

**Diretrizes**: em Azure monitor, defina seu período de retenção de espaço de trabalho de log Analytics de acordo com os regulamentos de conformidade de sua organização. Use contas de armazenamento do Azure para qualquer armazenamento de longo prazo/arquivamento de seus logs.

- [Alterar o período de retenção de dados em Log Analytics](platform/manage-cost-storage.md#change-the-data-retention-period)

- [Como configurar a política de retenção para logs de conta de armazenamento do Azure](../storage/common/storage-monitor-storage-account.md#configure-logging)

**Monitoramento da Central de Segurança do Azure**: Não disponível no momento

**Responsabilidade**: Cliente

### <a name="26-monitor-and-review-logs"></a>2,6: monitorar e examinar os logs

**Diretrizes**: analise e monitore os logs em busca de comportamento anormal e Examine regularmente os resultados. Use Azure Monitor e um espaço de trabalho Log Analytics para examinar os logs e executar consultas nos dados de log.

Como alternativa, você pode habilitar o e os dados integrados para o Azure Sentinel ou um SIEM de terceiros.

- [Como integrar o Azure Sentinel](../sentinel/quickstart-onboard.md)

- [Introdução às consultas de Log Analytics](log-query/get-started-portal.md)

- [Como realizar consultas personalizadas no Azure Monitor](log-query/get-started-queries.md)

**Monitoramento da Central de Segurança do Azure**: Não disponível no momento

**Responsabilidade**: Cliente

### <a name="27-enable-alerts-for-anomalous-activities"></a>2,7: habilitar alertas para atividades anômalas

**Orientação**: Use a central de segurança do Azure com log Analytics espaço de trabalho para monitoramento e alertas em atividade anômala encontrada em eventos e logs de segurança. Como alternativa, você pode habilitar o e os dados integrados para o Azure Sentinel.

- [Como integrar o Azure Sentinel](../sentinel/quickstart-onboard.md)

- [Como gerenciar alertas na central de segurança do Azure](../security-center/security-center-managing-and-responding-alerts.md)

- [Como alertar sobre dados de log do log Analytics](learn/tutorial-response.md)

**Monitoramento da Central de Segurança do Azure**: Não disponível no momento

**Responsabilidade**: Cliente

## <a name="identity-and-access-control"></a>Identidade e controle de acesso

*Para obter mais informações, consulte o [benchmark de segurança do Azure: identidade e controle de acesso](/azure/security/benchmarks/security-control-identity-access-control).*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3.1: manter um inventário de contas administrativas

**Diretrizes**: o Azure RBAC (controle de acesso baseado em função) do Azure permite que você gerencie o acesso aos recursos do Azure por meio de atribuições de função. Você pode atribuir essas funções a usuários, grupos de entidades de serviço e identidades gerenciadas. Há funções internas predefinidas para determinados recursos, e essas funções podem ser inventariadas ou consultadas por meio de ferramentas como CLI do Azure, Azure PowerShell ou portal do Azure.

- [Como obter uma função de diretório no Azure AD com o PowerShell](https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrole?view=azureadps-2.0)

- [Como obter membros de uma função de diretório no Azure AD com o PowerShell](https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrolemember?view=azureadps-2.0)

**Monitoramento da Central de Segurança do Azure**: Não disponível no momento

**Responsabilidade**: Cliente

### <a name="33-use-dedicated-administrative-accounts"></a>3.3: Usar contas administrativas dedicadas

**Diretriz**: Crie procedimentos operacionais padrão em relação ao uso de contas administrativas dedicadas. Use a identidade e o gerenciamento de acesso da Central de Segurança do Azure para monitorar a quantidade de contas administrativas.

Você também pode habilitar um acesso just-in-time/apenas o suficiente usando Azure AD Privileged Identity Management funções privilegiadas para serviços da Microsoft e Azure Resource Manager. 

- [Visão geral de Azure AD Privileged Identity Management](../active-directory/privileged-identity-management/pim-configure.md)

**Monitoramento da Central de Segurança do Azure**: Não disponível no momento

**Responsabilidade**: Cliente

### <a name="34-use-azure-active-directory-single-sign-on-sso"></a>3,4: usar o logon único (SSO) do Azure Active Directory

**Diretrizes**: sempre que possível, use Azure Active Directory SSO em vez de configurar credenciais autônomas individuais por serviço. Use as recomendações de gerenciamento de acesso e identidade da central de segurança do Azure.

- [Entender o SSO com o Azure AD](../active-directory/manage-apps/what-is-single-sign-on.md)

**Monitoramento da Central de Segurança do Azure**: Não disponível no momento

**Responsabilidade**: Cliente

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3,5: usar a autenticação multifator para todo o acesso baseado em Azure Active Directory

**Orientação**: habilitar a MFA do Azure AD e seguir as recomendações de acesso e identidade da central de segurança do Azure.

- [Como habilitar a MFA no Azure](../active-directory/authentication/howto-mfa-getstarted.md)

- [Como monitorar identidade e acesso na Central de Segurança do Azure](../security-center/security-center-identity-access.md)

**Monitoramento da Central de Segurança do Azure**: Sim

**Responsabilidade**: Cliente

### <a name="36-use-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3.6: usar computadores dedicados (estações de trabalho com acesso privilegiado) para todas as tarefas administrativas

**Diretrizes**: Use uma estação de trabalho segura e gerenciada pelo Azure (também conhecida como uma estação de trabalho com acesso privilegiado ou Paw) para tarefas administrativas que exigem privilégios elevados.

- [Entenda as estações de trabalho seguras e gerenciadas pelo Azure](../active-directory/devices/concept-azure-managed-workstation.md)

- [Como habilitar o Azure AD MFA](../active-directory/authentication/howto-mfa-getstarted.md)

**Monitoramento da Central de Segurança do Azure**: Não disponível no momento

**Responsabilidade**: Cliente

### <a name="37-log-and-alert-on-suspicious-activities-from-administrative-accounts"></a>3,7: registrar em log e alertar sobre atividades suspeitas de contas administrativas

**Diretrizes**: Use Azure Active Directory relatórios de segurança e monitoramento para detectar quando uma atividade suspeita ou insegura ocorre no ambiente. Use a Central de Segurança do Azure para monitorar a atividade de identidade e acesso. 

- [Como identificar usuários do Azure AD sinalizados em relação a atividades arriscadas](/azure/active-directory/reports-monitoring/concept-user-at-risk)

- [Como monitorar a atividade de identidade e acesso dos usuários na Central de Segurança do Azure](../security-center/security-center-identity-access.md)

**Monitoramento da Central de Segurança do Azure**: Não disponível no momento

**Responsabilidade**: Cliente

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3.8: Gerenciar recursos do Azure provenientes somente de locais aprovados

**Diretrizes**: Use localizações nomeadas de acesso condicional para permitir o acesso somente de agrupamentos lógicos de intervalos de endereços IP ou de países/regiões específicos.

- [Como configurar localizações nomeadas no Azure](../active-directory/reports-monitoring/quickstart-configure-named-locations.md)

**Monitoramento da Central de Segurança do Azure**: Não disponível no momento

**Responsabilidade**: Cliente

### <a name="39-use-azure-active-directory"></a>3.9: Use o Azure Active Directory Domain Services

**Diretrizes**: Use Azure Active Directory (AD do Azure) como o sistema de autenticação e autorização central. O Azure AD protege os dados usando criptografia forte para dados em repouso e em trânsito. O Azure Active Directory também inclui sais, hashes e armazena com segurança as credenciais do usuário.

- [Como criar e configurar uma instância do Azure AD](../active-directory/fundamentals/active-directory-access-create-new-tenant.md)

**Monitoramento da Central de Segurança do Azure**: Não disponível no momento

**Responsabilidade**: Cliente

### <a name="310-regularly-review-and-reconcile-user-access"></a>3.10: revisar e reconciliar regularmente o acesso do usuário

**Diretrizes**: O Azure AD fornece logs para ajudar a descobrir contas obsoletas. Além disso, use as revisões de acesso de identidade do Azure para gerenciar com eficiência as associações de grupo, o acesso aos aplicativos empresariais e as atribuições de função. O acesso de usuários pode ser examinado regularmente para garantir que somente os usuários corretos tenham acesso contínuo. 

- [Entender os relatórios do Azure AD](/azure/active-directory/reports-monitoring/)

- [Como usar as revisões de acesso de identidade do Azure](../active-directory/governance/access-reviews-overview.md)

**Monitoramento da Central de Segurança do Azure**: Não disponível no momento

**Responsabilidade**: Cliente

### <a name="311-monitor-attempts-to-access-deactivated-credentials"></a>3,11: monitorar tentativas de acessar credenciais desativadas

**Diretrizes**: Você tem acesso à atividade de entrada do Azure AD, às fontes de log de eventos de auditoria e de risco, que permitem a integração a qualquer ferramenta de Monitoramento/SIEM. Você pode simplificar esse processo criando configurações de diagnóstico para contas de usuário do Azure Active Directory e enviando os logs de auditoria e os logs de entrada para um workspace do Log Analytics. Você pode configurar os alertas desejados no workspace do Log Analytics.

- [Como integrar os logs de atividades do Azure ao Azure Monitor](/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics)

**Monitoramento da central de segurança do Azure**: desdefinição. Forneça um valor no item de trabalho.

**Responsabilidade**: Cliente

### <a name="312-alert-on-account-sign-in-behavior-deviation"></a>3,12: alerta sobre o desvio do comportamento de entrada da conta

**Diretrizes**: Use os recursos de proteção de identidade e proteção contra riscos do Azure AD para configurar respostas automatizadas para ações suspeitas detectadas relacionadas a identidades de usuários. Você também pode ingerir dados no Azure Sentinel para uma investigação mais aprofundada.

- [Como exibir entradas suspeitas do Azure Active Directory](/azure/active-directory/reports-monitoring/concept-risky-sign-ins)

- [Como configurar e habilitar políticas de risco de proteção de identidade](../active-directory/identity-protection/howto-identity-protection-configure-risk-policies.md)

- [Como integrar o Azure Sentinel](../sentinel/quickstart-onboard.md)

**Monitoramento da Central de Segurança do Azure**: Não disponível no momento

**Responsabilidade**: Cliente

## <a name="data-protection"></a>Proteção de dados

*Para obter mais informações, consulte o [benchmark de segurança do Azure: proteção de dados](/azure/security/benchmarks/security-control-data-protection).*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4.1: manter um inventário de informações confidenciais

**Orientação**: use marcas quando possível para auxiliar no rastreamento de Azure monitor recursos que armazenam ou processam informações confidenciais, como você log Analytics espaços de trabalho.

- [Como criar e usar marcas](/azure/azure-resource-manager/resource-group-using-tags)

- [Gerenciar acesso a dados de log e workspaces no Azure Monitor](platform/manage-access.md)

**Monitoramento da Central de Segurança do Azure**: Não disponível no momento

**Responsabilidade**: Cliente

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4.2: isolar sistemas que armazenam ou processam informações confidenciais

**Diretrizes**: implemente o isolamento usando assinaturas e grupos de gerenciamento separados para domínios de segurança individuais, como tipo de ambiente e nível de sensibilidade de dados. Você pode restringir o nível de acesso ao seu Azure Monitor e aos recursos relacionados que seus aplicativos e ambientes empresariais exigem. Você pode controlar o acesso a Azure Monitor por meio de Azure Active Directory controle de acesso baseado em função.

- [Como criar assinaturas adicionais do Azure](/azure/billing/billing-create-subscription)

- [Como criar Grupos de Gerenciamento](/azure/governance/management-groups/create)

- [Como criar e usar marcas](/azure/azure-resource-manager/resource-group-using-tags)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4.4: criptografar todas as informações confidenciais em trânsito

**Diretrizes**: Azure monitor negocia o TLS 1,2 por padrão. Verifique se todos os clientes que se conectam aos recursos do Azure são capazes de negociar o TLS 1,2 ou superior. 

Application Insights e Log Analytics continuam a permitir que os dados TLS 1,1 e TLS 1,0 sejam ingeridos. Os dados podem ser restritos ao TLS 1,2 Configurando no lado do cliente.

- [Como enviar dados com segurança usando o TLS 1,2](platform/data-security.md#sending-data-securely-using-tls-12)

**Monitoramento da Central de Segurança do Azure**: Não disponível no momento

**Responsabilidade**: Compartilhado

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4.5: usar uma ferramenta de descoberta ativa para identificar dados confidenciais

**Diretrizes**: os recursos de identificação de dados, classificação e prevenção de perda ainda não estão disponíveis para Azure monitor. Se necessário, implemente uma solução de terceiros para fins de conformidade.
Para a plataforma subjacente que é gerenciada pela Microsoft, a Microsoft trata todo o conteúdo do cliente como confidencial e faz todo o esforço possível para proteger contra perda e exposição de dados do cliente. Para garantir que os dados do cliente no Azure permaneçam seguros, a Microsoft implementou e mantém um conjunto de recursos e controles robustos de proteção de dados.

- [Entender a proteção de dados do cliente no Azure](../security/fundamentals/protection-customer-data.md)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Compartilhado

### <a name="46-use-role-based-access-control-to-control-access-to-resources"></a>4,6: usar o controle de acesso baseado em função para controlar o acesso aos recursos

**Orientação**: Use o RBAC (controle de acesso baseado em função) do Azure para gerenciar o acesso a Azure monitor.

- [Funções, permissões e segurança no Azure Monitor](platform/roles-permissions-security.md)

- [Como configurar o RBAC do Azure](../role-based-access-control/role-assignments-portal.md)

**Monitoramento da Central de Segurança do Azure**: Não disponível no momento

**Responsabilidade**: Cliente

### <a name="48-encrypt-sensitive-information-at-rest"></a>4.8: Criptografar informações confidenciais em repouso

**Diretrizes**: Azure monitor garante que todos os dados e consultas salvas sejam criptografadas em repouso usando chaves gerenciadas pela Microsoft (MMK). O Azure Monitor também fornece uma opção de criptografia usando sua própria chave armazenada em seu Azure Key Vault e acessada pelo armazenamento usando a autenticação de identidade gerenciada atribuída pelo sistema. Essa chave gerenciada pelo cliente (CMK) pode ser protegida por HSM de software ou hardware.

- [Azure Monitor chaves gerenciadas pelo cliente](platform/customer-managed-keys.md)

- [Segurança de dados do Log Analytics](platform/data-security.md)

- [Coleta, retenção e armazenamento de dados no Application Insights](app/data-retention-privacy.md)

- [Entender a criptografia em repouso no Azure](../security/fundamentals/encryption-atrest.md) 

**Monitoramento da Central de Segurança do Azure**: Sim

**Responsabilidade**: Cliente

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4.9: registrar e alertar sobre alterações em recursos críticos do Azure

**Diretrizes**: Use Azure monitor com o log de atividades do Azure para criar alertas para quando as alterações ocorrerem em Azure monitor e recursos relacionados.

- [Como criar alertas para eventos do log de atividades do Azure](platform/alerts-activity-log.md)

**Monitoramento da Central de Segurança do Azure**: Sim

**Responsabilidade**: Cliente

## <a name="vulnerability-management"></a>Gerenciamento de vulnerabilidades

*Para obter mais informações, consulte o [benchmark de segurança do Azure: gerenciamento de vulnerabilidade](/azure/security/benchmarks/security-control-vulnerability-management).*

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5.5: usar um processo de avaliação de risco para priorizar a correção das vulnerabilidades descobertas

**Diretrizes**: Use um programa de Pontuação de risco comum (por exemplo, sistema comum de Pontuação de vulnerabilidade) ou as classificações de risco padrão fornecidas pela ferramenta de verificação de terceiros.

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

## <a name="inventory-and-asset-management"></a>Inventário e gerenciamento de ativos

*Para obter mais informações, consulte o [benchmark de segurança do Azure: inventário e gerenciamento de ativos](/azure/security/benchmarks/security-control-inventory-asset-management).*

### <a name="61-use-automated-asset-discovery-solution"></a>6,1: usar solução de descoberta de ativos automatizada

**Diretrizes**: Use CLI do Azure para consultar e descobrir Azure monitor recursos em suas assinaturas. Configure permissões apropriadas (leitura) no seu locatário e enumere todas as assinaturas do Azure, bem como os recursos em suas assinaturas.

- [CLI do Azure Monitor](https://docs.microsoft.com/cli/azure/monitor?view=azure-cli-latest)

- [Como exibir suas assinaturas do Azure](https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription?view=azps-3.0.0)

- [Entender o RBAC do Azure](../role-based-access-control/overview.md)

- [Funções, permissões e segurança no Azure Monitor](platform/roles-permissions-security.md)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

### <a name="62-maintain-asset-metadata"></a>6.2: manter metadados de ativo

**Diretrizes**: aplique marcas para Azure monitor recursos que fornecem metadados para organizá-los logicamente em uma taxonomia.

- [Como criar e usar marcas](/azure/azure-resource-manager/resource-group-using-tags)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

### <a name="63-delete-unauthorized-azure-resources"></a>6.3: excluir recursos do Azure não autorizados

**Diretrizes**: use marcação, grupos de gerenciamento e assinaturas separadas, quando apropriado, para organizar e controlar Azure monitor recursos relacionados. Reconcilie o inventário regularmente e garanta que os recursos não autorizados sejam excluídos da assinatura em tempo hábil.

- [Como criar assinaturas adicionais do Azure](/azure/billing/billing-create-subscription)

- [Como criar Grupos de Gerenciamento](/azure/governance/management-groups/create)

- [Como criar e usar marcas](/azure/azure-resource-manager/resource-group-using-tags)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

### <a name="64-define-and-maintain-inventory-of-approved-azure-resources"></a>6,4: definir e manter o inventário de recursos aprovados do Azure

**Diretrizes**: Crie um inventário dos recursos aprovados do Azure e do software aprovado para recursos de computação de acordo com suas necessidades organizacionais.

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

### <a name="65-monitor-for-unapproved-azure-resources"></a>6.5: monitorar recursos do Azure não aprovados

**Diretrizes**: Use Azure Policy para colocar restrições no tipo de recursos que podem ser criados em suas assinaturas.

Use o grafo de recursos do Azure para consultar e descobrir recursos em suas assinaturas.  Verifique se todos os recursos do Azure presentes no ambiente foram aprovados.

- [Como configurar e gerenciar o Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Como criar consultas com o Gerenciador de gráficos de recursos do Azure](../governance/resource-graph/first-query-portal.md)

**Monitoramento da Central de Segurança do Azure**: Sim

**Responsabilidade**: Cliente

### <a name="67-remove-unapproved-azure-resources-and-software-applications"></a>6.7: Remover recursos e aplicativos de software não aprovados do Azure

**Diretrizes**: reconcilie o inventário regularmente e garanta que Azure monitor recursos relacionados não autorizados sejam excluídos da assinatura em tempo hábil.  

- [Excluir o espaço de trabalho Log Analytics do Azure](platform/delete-workspace.md)

**Monitoramento da Central de Segurança do Azure**: Sim

**Responsabilidade**: Cliente

### <a name="69-use-only-approved-azure-services"></a>6.9: Usar somente serviços do Azure aprovados

**Diretrizes**: Use Azure Policy para restringir quais Azure monitor recursos relacionados que você pode provisionar em seu ambiente. 

- [Como configurar e gerenciar o Azure Policy](../governance/policy/tutorials/create-and-manage.md) 

- [Como negar um tipo de recurso específico com o Azure Policy](/azure/governance/policy/samples/not-allowed-resource-types)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

### <a name="611-limit-users-ability-to-interact-with-azure-resource-manager"></a>6,11: limitar a capacidade dos usuários de interagir com Azure Resource Manager

**Orientação**: Use o acesso condicional do Azure para limitar a capacidade dos usuários de interagir com o Gerenciador de recursos do Azure Configurando "bloquear acesso" para o aplicativo de "gerenciamento de Microsoft Azure".

- [Como configurar o acesso condicional para bloquear o acesso ao Gerenciador de recursos do Azure](../role-based-access-control/conditional-access-azure-management.md)

**Monitoramento da Central de Segurança do Azure**: Não disponível no momento

**Responsabilidade**: Cliente

## <a name="secure-configuration"></a>Configuração segura

*Para obter mais informações, consulte o [benchmark de segurança do Azure: configuração segura](/azure/security/benchmarks/security-control-secure-configuration).*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7.1: estabelecer configurações seguras para todos os recursos do Azure

**Diretrizes**: Use definições de Azure Policy personalizadas para auditar ou impor a configuração de seus Azure monitor recursos relacionados. Você também pode usar definições de Azure Policy internas como disponíveis.

Além disso, Azure Resource Manager tem a capacidade de exportar o modelo no JavaScript Object Notation (JSON), que deve ser revisado para garantir que as configurações atendam/excedam os requisitos de segurança para sua organização.

Você também pode usar recomendações da central de segurança do Azure como uma linha de base de configuração segura para seus recursos do Azure.

Se estiver usando recursos do APM de streaming ao vivo, torne o canal seguro com uma chave de API secreta além da chave de instrumentação.

- [Secure APM Live Metrics Stream](app/live-stream.md#secure-the-control-channel)

- [Como exibir os aliases disponíveis do Azure Policy](https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-3.3.0)

- [Tutorial: Criar e gerenciar políticas para impor a conformidade](../governance/policy/tutorials/create-and-manage.md)

- [Exportação única e de vários recursos para um modelo no portal do Azure](../azure-resource-manager/templates/export-template-portal.md)

- [Recomendações de segurança – um guia de referência](../security-center/recommendations-reference.md)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

### <a name="73-maintain-secure-azure-resource-configurations"></a>7.3: Manter configurações seguras de recursos do Azure

**Orientação**: Use a política do Azure [negar] e [implantar se não existir] para impor configurações seguras em seus Azure monitor recursos relacionados.  Além disso, você pode usar modelos de Azure Resource Manager para manter a configuração de segurança de seus Azure Monitor recursos relacionados exigidos pela sua organização.

- [Entender Azure Policy efeitos](../governance/policy/concepts/effects.md)

- [Criar e gerenciar políticas para impor a conformidade](../governance/policy/tutorials/create-and-manage.md)

- [Visão geral de modelos de Azure Resource Manager](../azure-resource-manager/templates/overview.md)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

### <a name="75-securely-store-configuration-of-azure-resources"></a>7.5: armazenar configuração de recursos do Azure com segurança

**Diretrizes**: Use o Azure DevOps para armazenar e gerenciar com segurança seu código, como políticas personalizadas do Azure e modelos de Azure Resource Manager. Para acessar os recursos que você gerencia no Azure DevOps, você pode conceder ou negar permissões a usuários específicos, grupos de segurança internos ou grupos definidos no Azure Active Directory (AD do Azure), se integrados ao Azure DevOps, ou Active Directory se integrado ao TFS.

- [Como armazenar código no Azure DevOps](https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops)

- [Sobre permissões e grupos no Azure DevOps](/azure/devops/organizations/security/about-permissions)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

### <a name="77-deploy-configuration-management-tools-for-azure-resources"></a>7,7: implantar as ferramentas de gerenciamento de configuração para recursos do Azure

**Orientação**: definir e implementar configurações de segurança padrão para Azure monitor recursos relacionados usando o Azure Policy. Use definições de Azure Policy personalizadas para auditar ou impor a configuração de segurança do seu Azure Monitor recursos relacionados. Você também pode fazer uso de definições de política internas relacionadas aos seus recursos específicos.

- [Como configurar e gerenciar o Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Aliases de Azure Policy](../governance/policy/concepts/definition-structure.md#aliases)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

### <a name="79-implement-automated-configuration-monitoring-for-azure-resources"></a>7,9: implementar o monitoramento automatizado de configuração para recursos do Azure

**Orientação**: Use a central de segurança do Azure para executar verificações de linha de base para seus Azure monitor recursos relacionados.  Além disso, use Azure Policy para alertar e auditar configurações de recursos do Azure.

- [Como corrigir recomendações na central de segurança do Azure](../security-center/security-center-remediate-recommendations.md)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

### <a name="711-manage-azure-secrets-securely"></a>7.11: Gerenciar segredos do Azure com segurança

**Diretrizes**: Use identidade de serviço gerenciada em conjunto com Azure Key Vault para simplificar e proteger o gerenciamento de segredos para recursos relacionados ao Azure monitor com suporte.

- [Como integrar com identidades gerenciadas do Azure](../azure-app-configuration/howto-integrate-azure-managed-service-identity.md)

- [Serviços compatíveis com identidades gerenciadas para recursos do Azure](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md)

- [Como criar um Key Vault](/azure/key-vault/quick-create-portal)

- [Como fornecer Key Vault autenticação com uma identidade gerenciada](/azure/key-vault/managed-identity)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

### <a name="712-manage-identities-securely-and-automatically"></a>7.12: gerenciar identidades de maneira segura e automática

**Diretrizes**: Use identidades gerenciadas para fornecer aos serviços do Azure uma identidade gerenciada automaticamente no Azure AD. Identidades gerenciadas permitem que você se autentique em qualquer serviço que ofereça suporte à autenticação do Azure AD, incluindo Azure Monitor recursos, sem credenciais em seu código. 

- [Como configurar identidades gerenciadas para recursos do Azure](../active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm.md)

**Monitoramento da Central de Segurança do Azure**: Não disponível no momento

**Responsabilidade**: Cliente

### <a name="713-eliminate-unintended-credential-exposure"></a>7.13: eliminar a exposição involuntária de credenciais

**Diretriz**: implemente o verificador de credenciais para identificar credenciais no código. O verificador de credenciais também encorajará a migração de credenciais descobertas para locais mais seguros, como o Azure Key Vault.

- [Como configurar o verificador de credenciais](https://secdevtools.azurewebsites.net/helpcredscan.html)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

## <a name="malware-defense"></a>Defesa contra malware

*Para obter mais informações, consulte o [benchmark de segurança do Azure: defesa contra malware](/azure/security/benchmarks/security-control-malware-defense).*

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8.2: arquivos de pré-verificação a serem carregados para recursos não computados do Azure

**Diretrizes**: o antimalware da Microsoft está habilitado no host subjacente que dá suporte aos serviços do Azure (por exemplo, Azure monitor recursos relacionados), no entanto, ele não é executado no seu conteúdo. 

Examine previamente todos os arquivos que estão sendo carregados para Azure Monitor recursos relacionados aplicáveis, como Log Analytics espaço de trabalho.

Use a detecção de ameaças da central de segurança do Azure para serviços de dados para detectar malwares carregados em contas de armazenamento. 

- [Entender o antimalware da Microsoft para serviços de nuvem e máquinas virtuais do Azure](../security/fundamentals/antimalware.md)

- [Entender a detecção de ameaças da central de segurança do Azure para serviços de dados](/azure/security-center/security-center-alerts-data-services)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

## <a name="data-recovery"></a>Recuperação de dados

*Para obter mais informações, consulte o [benchmark de segurança do Azure: recuperação de dados](/azure/security/benchmarks/security-control-data-recovery).*

### <a name="91-ensure-regular-automated-back-ups"></a>9,1: garantir back-ups automatizados regulares

**Diretrizes**: Use Azure Resource Manager para exportar o Azure monitor e os recursos relacionados em um modelo de JavaScript Object Notation (JSON) que pode ser usado como backup para Azure monitor e configurações relacionadas.  Use a automação do Azure para executar os scripts de backup automaticamente. 

- [Gerenciar Log Analytics espaço de trabalho usando modelos de Azure Resource Manager](/azure/azure-monitor/platform/template-workspace-configuration)

- [Exportação única e de vários recursos para um modelo no portal do Azure](../azure-resource-manager/templates/export-template-portal.md)

- [Sobre a Automação do Azure](../automation/automation-intro.md)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9,2: executar backups completos do sistema e fazer backup de qualquer chave gerenciada pelo cliente

**Diretrizes**: Use Azure Resource Manager para exportar o Azure monitor e os recursos relacionados em um modelo de JavaScript Object Notation (JSON) que pode ser usado como backup para Azure monitor e configurações relacionadas.  Fazer backup de chaves gerenciadas pelo cliente dentro de Azure Key Vault se Azure Monitor recursos relacionados estiverem usando chaves gerenciadas pelo cliente, 

- [Gerenciar Log Analytics espaço de trabalho usando modelos de Azure Resource Manager](/azure/azure-monitor/platform/template-workspace-configuration)

- [Exportação única e de vários recursos para um modelo no portal do Azure](../azure-resource-manager/templates/export-template-portal.md)

- [Como fazer backup de chaves do cofre de chaves no Azure](https://docs.microsoft.com/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey?view=azurermps-6.13.0)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9,3: validar todos os backups, incluindo chaves gerenciadas pelo cliente

**Orientação**: garanta a capacidade de executar a restauração periodicamente usando Azure Resource Manager arquivos de modelo com suporte.  Teste a restauração de chaves de backup gerenciadas pelo cliente.

- [Gerenciar Log Analytics espaço de trabalho usando modelos de Azure Resource Manager](/azure/azure-monitor/platform/template-workspace-configuration)

- [Como restaurar chaves do cofre de chaves no Azure](https://docs.microsoft.com/powershell/module/azurerm.keyvault/restore-azurekeyvaultkey?view=azurermps-6.13.0)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9,4: garantir a proteção de backups e chaves gerenciadas pelo cliente

**Diretrizes**: Use o DevOps do Azure para armazenar e gerenciar com segurança seu código, como políticas personalizadas do azure, Azure Resource Manager modelos. Para proteger os recursos que você gerencia no Azure DevOps, você pode conceder ou negar permissões a usuários específicos, grupos de segurança internos ou grupos definidos no Azure Active Directory (AD do Azure), se integrados ao Azure DevOps, ou Active Directory se integrado ao TFS.   Use o controle de acesso baseado em função para proteger chaves gerenciadas pelo cliente. 

Além disso, habilite a proteção de exclusão e limpeza reversível no Key Vault para proteger as chaves contra exclusão acidental ou mal-intencionada. Se o armazenamento do Azure for usado para armazenar Azure Resource Manager backups de modelo, habilite a exclusão reversível para salvar e recuperar seus dados quando BLOBs ou instantâneos de blob forem excluídos. 

- [Como armazenar código no Azure DevOps](https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops)

- [Sobre permissões e grupos no Azure DevOps](/azure/devops/organizations/security/about-permissions)

- [Como habilitar a exclusão reversível e limpar a proteção no Key Vault](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete?tabs=azure-portal)

- [Exclusão reversível para blobs do Armazenamento do Azure ](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete?tabs=azure-portal)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

## <a name="incident-response"></a>Resposta a incidentes

*Para obter mais informações, consulte o [benchmark de segurança do Azure: resposta a incidentes](/azure/security/benchmarks/security-control-incident-response).*

### <a name="101-create-an-incident-response-guide"></a>10.1: criar um guia de resposta a incidentes

**Diretriz**: crie um guia de resposta a incidentes para sua organização. Verifique se há planos de resposta a incidentes escritos que definem todas as funções de pessoal, bem como as fases de tratamento/gerenciamento de incidentes, desde a detecção até a revisão após o incidente.

- [Orientação sobre como criar seu processo de resposta a incidentes de segurança](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

- [Anatomia de um incidente do Microsoft Security Response Center](https://msrc-blog.microsoft.com/2019/06/27/inside-the-msrc-anatomy-of-a-ssirp-incident/)

- [Aproveitar o Guia de tratamento de incidentes de segurança de computador do NIST para ajudar na criação de seu plano de resposta a incidentes](https://csrc.nist.gov/publications/detail/sp/800-61/rev-2/final)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10.2: criar um procedimento de pontuação e priorização de incidentes

**Diretriz**: a Central de Segurança atribui uma severidade a cada alerta para ajudar você a priorizar quais alertas devem ser investigados primeiro. A severidade se baseia na confiança que a Central de Segurança tem na constatação ou na análise usada para emitir o alerta, bem como no nível de confiança de que houve uma ação mal-intencionada por trás da atividade que levou ao alerta.

Além disso, marque claramente as assinaturas (por exemplo, produção, não produção) usando marcas e crie um sistema de nomeação para identificar claramente e categorizar os recursos do Azure, em especial aqueles que processam dados confidenciais.  É sua responsabilidade priorizar a correção de alertas com base na criticalidade dos recursos do Azure e do ambiente em que o incidente ocorreu.

- [Alertas na Central de Segurança do Azure](../security-center/security-center-alerts-overview.md)

- [Usar marcas para organizar seus recursos do Azure](/azure/azure-resource-manager/resource-group-using-tags)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

### <a name="103-test-security-response-procedures"></a>10.3: Testar procedimentos de resposta de segurança

**Diretriz**: Conduza regularmente exercícios para testar os recursos de resposta a incidentes de seus sistemas para ajudar a proteger seus recursos do Azure. Identifique pontos fracos e lacunas e revise o plano conforme necessário.

- [Publicação do NIST - Guia para testar, treinar e exercitar programas para planos de TI e capacidades](https://csrc.nist.gov/publications/detail/sp/800-84/final)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10.4: Fornecer detalhes de contato do incidente de segurança e configurar notificações de alerta para incidentes de segurança

**Diretriz**: As informações de contato do incidente serão usadas pela Microsoft para contatá-lo se o MSRC (Microsoft Security Response Center) descobrir que seus dados foram acessados por uma pessoa não autorizada ou ilegal. Examine os incidentes após o fato para garantir que os problemas sejam resolvidos.

- [Como definir o contato de segurança da Central de Segurança do Azure](../security-center/security-center-provide-security-contact-details.md)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10.5: Incorporar alertas de segurança em seu sistema de resposta a incidentes

**Diretriz**: Exporte seus alertas e recomendações da Central de Segurança do Azure usando o recurso de exportação contínua para ajudar a identificar riscos para os recursos do Azure. A exportação contínua permite exportar alertas e recomendações de forma manual ou contínua. Você pode usar o conector de dados da Central de Segurança do Azure para transmitir os alertas do Azure Sentinel. 

- [Como configurar a exportação contínua](../security-center/continuous-export.md) 

- [Como transmitir alertas para o Azure Sentinel](../sentinel/connect-azure-security-center.md)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

### <a name="106-automate-the-response-to-security-alerts"></a>10.6: automatizar a resposta a alertas de segurança

**Diretrizes**: Use o recurso de automação de fluxo de trabalho na central de segurança do Azure para disparar automaticamente respostas por meio de "aplicativos lógicos" em alertas de segurança e recomendações para proteger os recursos do Azure.

- [Como configurar a automação de fluxo de trabalho e os Aplicativos Lógicos](../security-center/workflow-automation.md)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

## <a name="penetration-tests-and-red-team-exercises"></a>Testes de penetração e exercícios de Red Team

*Para obter mais informações, consulte o [benchmark de segurança do Azure: testes de penetração e exercícios de equipe vermelho](/azure/security/benchmarks/security-control-penetration-tests-red-team-exercises).*

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-remediation-of-all-critical-security-findings"></a>11,1: realize testes de penetração regulares de seus recursos do Azure e garanta a correção de todas as descobertas de segurança críticas

**Diretrizes**: siga as regras de envolvimento da Microsoft para garantir que seus testes de penetração não sejam violações das políticas da Microsoft. Use a estratégia da Microsoft e a execução de equipes vermelhas e testes de penetração de sites ativos em infraestrutura de nuvem, serviços e aplicativos gerenciados pela Microsoft.

- [Regras de teste de penetração do Engagement](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1)

- [Microsoft Cloud o agrupamento vermelho](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Compartilhado

## <a name="next-steps"></a>Próximas etapas

- Confira o [Azure Security Benchmark](/azure/security/benchmarks/overview)
- Saiba mais sobre a [Linhas de base de segurança do Azure](/azure/security/benchmarks/security-baselines-overview)
