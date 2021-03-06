---
title: Análise de sentimento – LUIS
titleSuffix: Azure Cognitive Services
description: Se a análise de sentimento estiver configurada, a resposta JSON do LUIS incluirá a análise de sentimento.
services: cognitive-services
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 07/01/2020
ms.openlocfilehash: 7c3c0f603b9e7d83e40f43ab00d31f4ca63f2b4f
ms.sourcegitcommit: f5580dd1d1799de15646e195f0120b9f9255617b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/29/2020
ms.locfileid: "91535365"
---
# <a name="sentiment-analysis"></a>Análise de sentimento
Se a análise de sentimento estiver configurada, a resposta JSON do LUIS incluirá a análise de sentimento. Saiba mais sobre a análise de sentimento na documentação [Análise de Texto](https://docs.microsoft.com/azure/cognitive-services/text-analytics/).

LUIS usa Análise de Texto v2. 

## <a name="resolution-for-sentiment"></a>Resolução de sentimentos

Dados de sentimento são uma pontuação entre 1 e 0 indicando o sentimento positivo (mais próximo de 1) ou negativo (mais próximo de 0) dos dados.

#### <a name="english-language"></a>[Idioma inglês](#tab/english)

Quando a cultura for `en-us`, a resposta será:

```JSON
"sentimentAnalysis": {
  "label": "positive",
  "score": 0.9163064
}
```

#### <a name="other-languages"></a>[Outros idiomas](#tab/other-languages)

Para todas as outras culturas, a resposta é:

```JSON
"sentimentAnalysis": {
  "score": 0.9163064
}
```
* * *

## <a name="next-steps"></a>Próximas etapas

Saiba mais sobre o [ponto de extremidade de previsão V3](luis-migration-api-v3.md).

