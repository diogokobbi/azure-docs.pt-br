---
title: incluir arquivo
titleSuffix: Azure
description: incluir arquivo
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: include
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: 000971878e24c46892aaef1fa0c65237a4219883
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "81678588"
---
Depois que o recurso de **emparelhamento** for implantado com êxito, você poderá exibi-lo seguindo estas etapas.

1. Vá para **grupos de recursos**e selecione o grupo de recursos que você selecionou quando criou o recurso de **emparelhamento** . Use a caixa de **filtro** se você tiver muitos grupos de recursos.

    > [!div class="mx-imgBorder"]
    > ![Grupos de recursos](../media/setup-direct-get-resourcegroup.png)

1. Selecione o recurso de **emparelhamento** que você criou.

    > [!div class="mx-imgBorder"]
    > ![Exibição de recursos de emparelhamento](../media/setup-direct-get-open.png)

1. A página **visão geral** mostra informações de alto nível, como mostrado aqui.

    > [!div class="mx-imgBorder"]
    > ![Painel Visão geral do recurso de emparelhamento](../media/setup-exchange-get-overview.png)

1. À esquerda, selecione **informações de ASN** para exibir as informações enviadas quando você criou o PeerAsn.

    > [!div class="mx-imgBorder"]
    > ![Informações de ASN do recurso de emparelhamento](../media/setup-direct-get-asninfo.png)

1. À esquerda, selecione **conexões**. Na parte superior da tela, você verá um resumo das conexões de emparelhamento entre o ASN e a Microsoft, em diferentes instalações dentro do metro. Você também pode acessar o resumo de conexões na página **visão geral** selecionando **conexões** no painel central, conforme mostrado.

    > [!div class="mx-imgBorder"]
    > ![Conexões de recursos de emparelhamento](../media/setup-exchange-get-connectionssummary.png)

    * O **estado da conexão** corresponde ao estado da configuração da conexão de emparelhamento. Os Estados exibidos neste campo seguem o diagrama de estado mostrado no [passo a passos de emparelhamento do Exchange](../walkthrough-exchange-all.md).
    * Estado de **sessão IPv4** e **estado de sessão IPv6** correspondem aos Estados de sessão BGP IPv4 e IPv6, respectivamente.  
    * Quando você seleciona uma linha na parte superior da tela, a seção de **conexão** na parte inferior mostra detalhes de cada conexão. Selecione as setas para expandir a **configuração**, o **endereço IPv4**e o **endereço IPv6**.

    > [!div class="mx-imgBorder"]
    > ![Exibição de recursos de emparelhamento](../media/setup-exchange-get-connectionsipv4.png)
