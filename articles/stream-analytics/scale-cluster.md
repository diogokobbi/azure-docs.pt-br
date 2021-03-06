---
title: Redimensionar um cluster do Azure Stream Analytics
description: Saiba como aumentar ou diminuir o tamanho de um cluster do Azure Stream Analytics.
author: sidramadoss
ms.author: sidram
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: overview
ms.custom: mvc
ms.date: 09/22/2020
ms.openlocfilehash: 0763e56de6c72a36d39b17d153db6fc4d7dd821a
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90943320"
---
# <a name="resize-an-azure-stream-analytics-cluster"></a>Redimensionar um cluster do Azure Stream Analytics

A capacidade de um cluster do Stream Analytics é medida em SUs (Unidades de Streaming). Vários trabalhos poderão ser executados em paralelo no mesmo cluster, desde que a soma de SUs atribuídas a todos os trabalhos em execução não exceda a capacidade do cluster.

A capacidade do cluster poderá ser ampliada ou reduzida para corresponder ao tamanho das cargas de trabalho de streaming. Dimensionar um cluster leva tempo e ele não foi projetado para operações de dimensionamento frequentes. Recomendamos planejar e provisionar um cluster com o número preciso de SUs que você planeja consumir.

## <a name="change-the-scale-of-your-cluster"></a>Alterar a escala de um cluster

1. No portal do Azure, localize e selecione seu cluster do Stream Analytics.

1. Na seção **Visão Geral**, selecione **Escala**. Será possível conferir quantas SUs foram atribuídas ao seu cluster. Use o seletor para aumentar ou diminuir as SUs, conforme necessário.

   ![dimensionar um cluster](./media/scale-cluster/scale-cluster.png)

A operação de dimensionamento não afetará os trabalhos que estiverem em execução.

## <a name="next-steps"></a>Próximas etapas

Agora você já sabe como aumentar ou diminuir os clusters do Stream Analytics. Em seguida, será possível aprender sobre o gerenciamento de pontos de extremidade privados e o dimensionamento automático de seus trabalhos:

* [Gerenciar pontos de extremidade privados em um cluster do Azure Stream Analytics](private-endpoints.md)
* [Gerenciar trabalhos em um cluster do Azure Stream Analytics](manage-jobs-cluster.md)
