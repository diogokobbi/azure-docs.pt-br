---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 09/16/2020
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 404a3a41519d329d0c6f317b876e8edc42062071
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90980428"
---
## <a name="azure-security-benchmark"></a>Azure Security Benchmark

O [Azure Security Benchmark](../../../../articles/security/benchmarks/overview.md) fornece recomendações sobre como você pode proteger suas soluções de nuvem no Azure. Para ver como esse serviço é mapeado completamente para o Azure Security Benchmark, confira os [arquivos de mapeamento do Azure Security Benchmark](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines).

Para examinar como os internos do Azure Policy disponíveis para todos os serviços do Azure são mapeados para esse padrão de conformidade, confira [Conformidade Regulatória do Azure Policy – Azure Security Benchmark](../../../../articles/governance/policy/samples/azure-security-benchmark.md).

|Domínio |ID de Controle |Título do controle |Política<br /><sub>(Portal do Azure)</sub> |Versão da política<br /><sub>(GitHub)</sub>  |
|---|---|---|---|---|
|Segurança de rede |1,1 |proteger recursos usando grupos de segurança de rede ou o Firewall do Azure em sua Rede Virtual |[O Barramento de Serviço deve usar um ponto de extremidade de serviço de rede virtual](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F235359c5-7c52-4b82-9055-01c75cf9f60e) |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Network/VirtualNetworkServiceEndpoint_ServiceBus_AuditIfNotExists.json) |
|Registro em log e monitoramento |2.3 |habilitar o registro em log de auditoria para recursos do Azure |[Os logs de diagnóstico no Barramento de Serviço devem ser habilitados](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Ff8d36e2f-389b-4ee4-898d-21aeb69a0f45) |[3.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Service%20Bus/ServiceBus_AuditDiagnosticLog_Audit.json) |

## <a name="hipaa-hitrust-92"></a>HIPAA HITRUST 9.2

Para examinar como as iniciativas internas disponíveis do Azure Policy de todos os serviços do Azure são mapeadas para esse padrão de conformidade, confira [Conformidade Regulatória do Azure Policy – HIPAA HITRUST 9.2](../../../../articles/governance/policy/samples/hipaa-hitrust-9-2.md).
Para obter mais informações sobre esse padrão de conformidade, confira [HIPAA HITRUST 9.2](https://www.hhs.gov/hipaa/for-professionals/security/laws-regulations/index.html).

|Domínio |ID do controle |Título do controle |Política<br /><sub>(Portal do Azure)</sub> |Versão da política<br /><sub>(GitHub)</sub>  |
|---|---|---|---|---|
|Diferenciação em redes |0805.01m1Organizational.12 – 01.m |Os gateways de segurança da organização (por exemplo, firewalls) impõem políticas de segurança e são configurados para filtrar o tráfego entre domínios, bloquear acesso não autorizado e são usados para manter a diferenciação entre segmentos de rede internos com fio, internos sem fio e externos (por exemplo, a Internet), incluindo DMZs e imposição de políticas de controle de acesso para cada um dos domínios. |[O Barramento de Serviço deve usar um ponto de extremidade de serviço de rede virtual](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F235359c5-7c52-4b82-9055-01c75cf9f60e) |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Network/VirtualNetworkServiceEndpoint_ServiceBus_AuditIfNotExists.json) |
|Diferenciação em redes |0806.01m2Organizational.12356 – 01.m |A rede de organizações é lógica e fisicamente segmentada com um perímetro de segurança definido e um conjunto graduado de controles, incluindo sub-redes de componentes do sistema publicamente acessíveis que são logicamente separados da rede interna, com base nos requisitos organizacionais; e o tráfego é controlado com base na funcionalidade necessária e na classificação dos dados/sistemas com base em uma avaliação de risco e nos respectivos requisitos de segurança. |[O Barramento de Serviço deve usar um ponto de extremidade de serviço de rede virtual](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F235359c5-7c52-4b82-9055-01c75cf9f60e) |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Network/VirtualNetworkServiceEndpoint_ServiceBus_AuditIfNotExists.json) |
|Diferenciação em redes |0894.01m2Organizational.7 – 01.m |As redes são separadas das redes de nível de produção durante a migração de servidores físicos, aplicativos ou dados para servidores virtualizados. |[O Barramento de Serviço deve usar um ponto de extremidade de serviço de rede virtual](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F235359c5-7c52-4b82-9055-01c75cf9f60e) |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Network/VirtualNetworkServiceEndpoint_ServiceBus_AuditIfNotExists.json) |
|Log de auditoria |1208.09aa3System.1 – 09.aa |Os logs de auditoria são mantidos para atividades de gerenciamento, inicialização/desligamento/erros de sistema e aplicativo, alterações de arquivo e alterações de política de segurança. |[Os logs de diagnóstico no Barramento de Serviço devem ser habilitados](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Ff8d36e2f-389b-4ee4-898d-21aeb69a0f45) |[3.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Service%20Bus/ServiceBus_AuditDiagnosticLog_Audit.json) |
|Controles de rede |0860.09m1Organizational.9 – 09.m |A organização gerencia formalmente os equipamentos na rede, incluindo equipamentos em áreas de usuário. |[O Barramento de Serviço deve usar um ponto de extremidade de serviço de rede virtual](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F235359c5-7c52-4b82-9055-01c75cf9f60e) |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Network/VirtualNetworkServiceEndpoint_ServiceBus_AuditIfNotExists.json) |

