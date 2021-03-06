---
title: Fornecer detalhes de contato de segurança na Central de Segurança do Azure | Microsoft Docs
description: Este documento mostra como fornecer detalhes de contato de segurança na Central de Segurança do Azure.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 26b5dcb4-ce3f-4f22-8d56-d2bf743cfc90
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/11/2020
ms.author: memildin
ms.openlocfilehash: 9fbd63e1b46b837350be720fadf68777927f9bff
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90904751"
---
# <a name="set-up-email-notifications-for-security-alerts"></a>Configurar notificações por email para alertas de segurança 

Para garantir que as pessoas certas em sua organização sejam notificadas sobre alertas de segurança em seu ambiente, insira seus endereços de email na página de configurações de **notificações de email** .

Ao configurar suas notificações, você pode configurar os emails a serem enviados a indivíduos específicos ou a qualquer pessoa com uma função específica do Azure para uma assinatura. 

Para evitar o alerta fadiga, a central de segurança limita o volume de emails enviados. Para cada assinatura, a central de segurança envia:

- um máximo de **quatro** emails por dia para alertas de **alta severidade**
- um máximo de **dois** emails por dia para alertas de **severidade média**
- um máximo de **um** email por dia para alertas de **baixa severidade**


:::image type="content" source="./media/security-center-provide-security-contacts/email-notification-settings.png" alt-text="Configurar os detalhes do contato que receberá emails sobre alertas de segurança." :::

## <a name="availability"></a>Disponibilidade

|Aspecto|Detalhes|
|----|:----|
|Estado da versão:|GA (em disponibilidade geral)|
|Refere|Gratuita|
|Funções e permissões necessárias:|**Administrador de Segurança**<br>**Proprietário da assinatura** |
|Nuvens:|![Sim](./media/icons/yes-icon.png) Nuvens comerciais<br>![Sim](./media/icons/yes-icon.png) US Gov (parcial)<br>![No](./media/icons/no-icon.png) China gov, outros gov|
|||


## <a name="set-up-email-notifications-for-alerts"></a>Configurar notificações por email para alertas <a name="email"></a>

Você pode enviar notificações por email para indivíduos ou para todos os usuários com funções específicas do Azure.

1. Na área de **configurações & de preços** da central de segurança, na assinatura relevante e selecione **notificações por email**.

1. Defina os destinatários para suas notificações:

    - Na lista suspensa, selecione entre as funções disponíveis.
    - E/ou insira endereços de email específicos separados por vírgulas. Não há nenhum limite para o número de endereços de email que você pode inserir.

1. Para aplicar as informações de contato de segurança à sua assinatura, selecione **salvar**.


## <a name="see-also"></a>Confira também
Para saber mais sobre alertas de segurança, consulte o seguinte:

* [Alertas de segurança-um guia de referência](alerts-reference.md) – saiba mais sobre os alertas de segurança que você pode ver no módulo de proteção contra ameaças da central de segurança do Azure
* [Gerenciar e responder a alertas de segurança na central de segurança do Azure](security-center-managing-and-responding-alerts.md) – saiba como gerenciar e responder a alertas de segurança
* [Automação de fluxo de trabalho](workflow-automation.md) -automatizar respostas para alertas com lógica de notificação personalizada