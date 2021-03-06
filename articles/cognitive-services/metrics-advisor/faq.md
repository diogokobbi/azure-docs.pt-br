---
title: Perguntas frequentes do assistente de métricas
titleSuffix: Azure Cognitive Services
description: Perguntas frequentes sobre o serviço do Orientador de métricas.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: metrics-advisor
ms.topic: conceptual
ms.date: 09/10/2020
ms.author: aahi
ms.openlocfilehash: 0fde9a0f46073a2f3a24962ea58431581455f474
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90933192"
---
# <a name="metrics-advisor-frequently-asked-questions"></a>Perguntas frequentes do assistente de métricas

### <a name="what-is-the-cost-of-my-instance"></a>Qual é o custo da minha instância?

Atualmente, não há um custo para usar sua instância durante a visualização.

### <a name="why-is-the-demo-website-readonly"></a>Por que o site de demonstração é ReadOnly?

O [site de demonstração](https://anomaly-detector.azurewebsites.net/) está disponível publicamente. Essa instância é feita somente leitura para evitar o carregamento acidental de quaisquer dados.

### <a name="why-cant-i-create-the-resource-the-pricing-tier-is-unavailable-and-it-says-you-have-already-created-1-s0-for-this-subscription"></a>Por que não posso criar o recurso? O "tipo de preço" não está disponível e diz "você já criou 1 S0 para esta assinatura"?

:::image type="content" source="media/pricing.png" alt-text="Mensagem quando um recurso F0 já existe":::

Durante a visualização pública, apenas uma instância do assistente de métricas pode ser criada em uma assinatura, em uma região.

Se você já tiver uma instância criada na mesma região usando a mesma assinatura, poderá tentar uma região diferente ou uma assinatura diferente para criar uma nova instância. Você também pode excluir uma instância existente para criar uma nova.

Se você já tiver excluído a instância existente, mas ainda vir o erro, aguarde cerca de 20 minutos após a exclusão do recurso antes de criar uma nova instância.

## <a name="basic-concepts"></a>Conceitos básicos

### <a name="what-is-multi-dimensional-time-series-data"></a>O que são dados multidimensionais de série temporal?

Consulte a definição de [métrica multidimensional](glossary.md#multi-dimensional-metric)  no Glossário.

### <a name="how-much-data-is-needed-for-metrics-advisor-to-start-anomaly-detection"></a>Quantos dados são necessários para o revisor de métricas iniciar a detecção de anomalias?

No mínimo, um ponto de dados pode disparar a detecção de anomalias. No entanto, isso não traz a melhor precisão. O serviço assumirá uma janela de pontos de dados anteriores usando o valor especificado como a regra de "lacuna de preenchimento" durante a criação do feed de dados.

É recomendável ter alguns dados antes do carimbo de data/hora em que você deseja a detecção.
Com base na granularidade de seus dados, a quantidade de dados recomendada varia conforme a seguir.

| Granularidade | Valor de dados recomendado para detecção |
| ----------- | ------------------------------------- |
| Menos de 5 minutos | 4 dias de dados |
| 5 minutos a 1 dia | 28 dias de dados |
| Mais de 1 dia, para 31 dias | 4 anos de dados |
| Mais de 31 dias | 48 anos de dados |

### <a name="why-metrics-advisor-doesnt-detect-anomalies-from-historical-data"></a>Por que o Metrics Advisor não detecta anomalias de dados históricos?

O assistente de métricas foi projetado para detectar dados de transmissão ao vivo. Há uma limitação do comprimento máximo dos dados históricos que o serviço examinará e executará a detecção de anomalias. Isso significa que somente os pontos de dados após um determinado carimbo de data/hora terão resultados de detecção de anomalias. Esse carimbo de data/hora mais antigo depende da granularidade dos seus dados.

Com base na granularidade dos seus dados, os comprimentos dos dados históricos que terão resultados de detecção de anomalias são os seguintes.

| Granularidade | Comprimento máximo de dados históricos para detecção de anomalias |
| ----------- | ------------------------------------- |
| Menos de 5 minutos | Tempo integrado-13 horas |
| 5 minutos a menos de 1 hora | Tempo integrado-4 dias  |
| 1 hora para menos de 1 dia | Tempo integrado-14 dias  |
| 1 dia | Tempo integrado-28 dias  |
| Mais de 1 dia, menos de 31 dias | Tempo integrado-2 anos  |
| Mais de 31 dias | Tempo integrado-24 anos   |

### <a name="more-concepts-and-technical-terms"></a>Mais conceitos e termos técnicos

Acesse o [Glossário](glossary.md) para ler mais.

## <a name="how-do-i-detect-such-kinds-of-anomalies"></a>Como fazer detectar esses tipos de anomalias? 

### <a name="how-do-i-detect-spikes--dips-as-anomalies"></a>Como fazer detectar picos & DIPs como anomalias?

Se você tiver limites rígidos predefinidos, poderá, na verdade, definir manualmente "limite rígido" nas [configurações de detecção de anomalias](how-tos/configure-metrics.md#anomaly-detection-methods).
Se não houver limites, você poderá usar a "detecção inteligente", que é alimentada pelo ia. Veja [ajustar a configuração de detecção](how-tos/configure-metrics.md#tune-the-detecting-configuration) para obter detalhes.

### <a name="how-do-i-detect-inconformity-with-regular-seasonal-patterns-as-anomalies"></a>Como fazer detectar a inconformidade com padrões regulares (sazonais) como anomalias?

"Detecção inteligente" é capaz de aprender o padrão de seus dados, incluindo padrões sazonais. Em seguida, ele detecta os pontos de dados que não estão em conformidade com os padrões regulares como anomalias. Veja [ajustar a configuração de detecção](how-tos/configure-metrics.md#tune-the-detecting-configuration) para obter detalhes.

### <a name="how-do-i-detect-flat-lines-as-anomalies"></a>Como fazer detectar linhas simples como anomalias?

Se seus dados normalmente estiverem muito instáveis e flutuar muito e você quiser ser alertado quando ficar muito estável ou se se tornar uma linha simples, o "limite de alteração" poderá ser configurado para detectar esses pontos de dados quando a alteração for muito pequena.
Veja [as configurações de detecção de anomalias](how-tos/configure-metrics.md#anomaly-detection-methods) para obter detalhes.

## <a name="next-steps"></a>Próximas etapas
- [Visão geral do Assistente de Métricas](overview.md)
- [Experimente o site de demonstração](quickstarts/explore-demo.md)
- [Usar o portal da Web](quickstarts/web-portal.md)