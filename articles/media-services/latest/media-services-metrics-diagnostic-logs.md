---
title: Métricas dos serviços de mídia e logs de diagnóstico com Azure Monitor
titleSuffix: Azure Media Services
description: Saiba como monitorar as métricas dos serviços de mídia do Azure e os logs de diagnóstico por meio de Azure Monitor.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/31/2020
ms.author: inhenkel
ms.openlocfilehash: c03950d64c9ead17dfa5c07ef70ab2b7ee0e90bb
ms.sourcegitcommit: 58d3b3314df4ba3cabd4d4a6016b22fa5264f05a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89296644"
---
# <a name="monitor-media-services-metrics-and-diagnostic-logs-via-azure-monitor"></a>Monitorar as métricas dos serviços de mídia e os logs de diagnóstico por meio de Azure Monitor

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

[Azure monitor](../../azure-monitor/overview.md) permite que você monitore as métricas e os logs de diagnóstico que ajudam a entender como seus aplicativos estão sendo executados. Todos os dados coletados pelo Azure Monitor se adaptam a um dos dois tipos fundamentais: métricas e logs. Você pode monitorar os logs de diagnóstico dos serviços de mídia e criar alertas e notificações para as métricas e os logs coletados. Você pode visualizar e analisar os dados de métricas usando o [Metrics Explorer](../../azure-monitor/platform/metrics-getting-started.md). Você pode enviar logs para o [armazenamento do Azure](https://azure.microsoft.com/services/storage/), transmiti-los para os [hubs de eventos do Azure](https://azure.microsoft.com/services/event-hubs/), exportá-los para [log Analytics](https://azure.microsoft.com/services/log-analytics/)ou usar serviços de terceiros.

Para obter uma visão geral detalhada, consulte [Azure monitor métricas](../../azure-monitor/platform/data-platform.md) e [Azure monitor logs de diagnóstico](../../azure-monitor/platform/platform-logs-overview.md).

Este tópico discute as [métricas dos serviços](#media-services-metrics) de mídia e [os logs de diagnóstico dos serviços de mídia](#media-services-diagnostic-logs)com suporte.

## <a name="media-services-metrics"></a>Métricas dos serviços de mídia

As métrica são coletadas em intervalos regulares independentemente da mudança no valor. Elas são úteis para alertar, pois podem ser utilizadas com frequência, e um alerta pode ser acionado rapidamente com uma lógica relativamente simples. Para obter informações sobre como criar alertas de métrica, consulte [criar, exibir e gerenciar alertas de métrica usando Azure monitor](../../azure-monitor/platform/alerts-metric.md).

Os serviços de mídia oferecem suporte a métricas de monitoramento para os seguintes recursos:

* Conta
* Ponto de Extremidade de Streaming

### <a name="account"></a>Conta

Você pode monitorar as seguintes métricas de conta.

|Nome da métrica|Nome de exibição|Descrição|
|---|---|---|
|AssetCount|Contagem de ativos|Ativos em sua conta.|
|AssetQuota|Cota de ativos|Cota de ativos em sua conta.|
|AssetQuotaUsedPercentage|Percentual de cota de ativos usado|A porcentagem da cota de ativos já usada.|
|ContentKeyPolicyCount|Contagem de Política de Chave de Conteúdo|Políticas de chave de conteúdo em sua conta.|
|ContentKeyPolicyQuota|Cota de Política de Chave de Conteúdo|Cota de políticas de chave de conteúdo em sua conta.|
|ContentKeyPolicyQuotaUsedPercentage|Percentual de Cota Usada da Política de Chave de Conteúdo|A porcentagem da cota de política de chave de conteúdo já usada.|
|StreamingPolicyCount|Contagem de Política de Streaming|Políticas de streaming em sua conta.|
|StreamingPolicyQuota|Cota de Política de Streaming|Cota de políticas de streaming em sua conta.|
|StreamingPolicyQuotaUsedPercentage|Percentual de cota usada da Política de Streaming|A porcentagem da cota da política de streaming já usada.|

Você também deve examinar as [cotas de conta e os limites](limits-quotas-constraints.md).

### <a name="streaming-endpoint"></a>Ponto de Extremidade de Streaming

As métricas de [pontos de extremidade de streaming](/rest/api/media/streamingendpoints) dos serviços de mídia a seguir têm suporte:

|Nome da métrica|Nome de exibição|Descrição|
|---|---|---|
|Requests|Requests|Fornece o número total de solicitações HTTP atendidas pelo ponto de extremidade de streaming.|
|Saída|Saída|Total de bytes de saída por minuto por ponto de extremidade de streaming.|
|SuccessE2ELatency|Latência de ponta a ponta bem-sucedida|Duração de tempo de quando o ponto de extremidade de streaming recebeu a solicitação para quando o último byte da resposta foi enviado.|

### <a name="why-would-i-want-to-use-metrics"></a>Por que desejo usar métricas?

Aqui estão exemplos de como o monitoramento das métricas dos serviços de mídia pode ajudá-lo a entender como seus aplicativos estão sendo executados. Algumas perguntas que podem ser abordadas com as métricas dos serviços de mídia são:

* Como fazer monitorar meu ponto de extremidade de streaming padrão para saber quando eu exceda os limites?
* Como fazer saber se tenho unidades de escala de ponto de extremidade de streaming Premium suficientes?
* Como posso definir um alerta para saber quando escalar verticalmente meus pontos de extremidade de streaming?
* Como fazer definir um alerta para saber quando a egresso máxima configurada na conta foi atingida?
* Como posso ver a divisão das solicitações que falharam e o que está causando a falha?
* Como posso ver quantas solicitações HLS ou DASH estão sendo extraídas do empacotador?
* Como fazer definir um alerta para saber quando o valor do limite de n º de solicitações com falha foi atingido?

### <a name="example"></a>Exemplo

Consulte [como monitorar as métricas dos serviços de mídia](media-services-metrics-howto.md).

## <a name="media-services-diagnostic-logs"></a>Logs de diagnóstico dos serviços de mídia

Os logs de diagnóstico fornecem dados avançados e frequentes sobre a operação de um recurso do Azure. Para obter mais informações, consulte [como coletar e consumir dados de log de seus recursos do Azure](../../azure-monitor/platform/platform-logs-overview.md).

Os serviços de mídia oferecem suporte aos seguintes logs de diagnóstico:

* Entrega de chave

### <a name="key-delivery"></a>Entrega de chave

|Nome|Descrição|
|---|---|
|Solicitação do serviço de distribuição de chaves|Logs que mostram as informações de solicitação de serviço de entrega de chave. Para obter mais informações, consulte [esquemas](media-services-diagnostic-logs-schema.md).|

### <a name="why-would-i-want-to-use-diagnostics-logs"></a>Por que desejo usar os logs de diagnóstico?

Algumas coisas que você pode examinar com os logs de diagnóstico de distribuição de chaves são:

* Consulte o número de licenças entregues pelo tipo de DRM.
* Consulte o número de licenças entregues pela política.
* Consulte os erros por DRM ou tipo de política.
* Consulte o número de solicitações de licença não autorizadas de clientes.

### <a name="example"></a>Exemplo

Consulte [como monitorar os logs de diagnóstico do serviço de mídia](media-services-diagnostic-logs-howto.md).

## <a name="next-steps"></a>Próximas etapas

* [Como coletar e consumir dados de log de seus recursos do Azure](../../azure-monitor/platform/platform-logs-overview.md)
* [Criar, exibir e gerenciar alertas de métrica usando Azure Monitor](../../azure-monitor/platform/alerts-metric.md)
* [Como monitorar as métricas dos serviços de mídia](media-services-metrics-howto.md)
* [Como monitorar os logs de diagnóstico do serviço de mídia](media-services-diagnostic-logs-howto.md)
