---
title: Tutorial – Criar e gerenciar regras no aplicativo Azure IoT Central
description: Este tutorial mostra a você como as regras do Azure IoT Central permitem monitorar os dispositivos quase em tempo real e invocar ações automaticamente, como enviar um email, quando a regra é disparada.
author: dominicbetts
ms.author: dobett
ms.date: 04/06/2020
ms.topic: tutorial
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: 555da74da65f3b1897a276cf819a263334cfa053
ms.sourcegitcommit: 25490467e43cbc3139a0df60125687e2b1c73c09
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80999052"
---
# <a name="tutorial-create-a-rule-and-set-up-notifications-in-your-azure-iot-central-application"></a>Tutorial: Criar uma regra e configurar notificações no aplicativo Azure IoT Central

*Este artigo aplica-se a operadores, construtores e administradores.*

É possível usar a Azure IoT Central para monitorar remotamente os dispositivos conectados. As regras do Azure IoT Central permitem monitorar os dispositivos quase em tempo real e invocar ações automaticamente, como enviar um email. Este artigo explica como criar regras para monitorar a telemetria enviada pelos seus dispositivos.

Os dispositivos usam a telemetria para enviar dados numéricos do dispositivo. Uma regra é disparada quando a telemetria do dispositivo selecionado ultrapassa um limite especificado.

Neste tutorial, você cria uma regra para enviar um email quando a temperatura em um dispositivo de sensor ambiental simulado exceder 70&deg;F.

Neste tutorial, você aprenderá como:

> [!div class="checklist"]
>
> * Criar uma regra
> * Adicionar uma ação de e-mail

## <a name="prerequisites"></a>Pré-requisitos

Antes de começar, conclua os inícios rápidos [Criar um aplicativo do Azure IoT Central](./quick-deploy-iot-central.md) e [Adicionar um dispositivo simulado ao seu aplicativo do IoT Central](./quick-create-simulated-device.md) para criar o modelo de dispositivo **MXChip IoT DevKit** com o qual trabalhar.

## <a name="create-a-rule"></a>Criar uma regra

Para criar uma regra de telemetria, o modelo do dispositivo deve incluir pelo menos um valor de telemetria. Este tutorial usa um dispositivo **MXChip IoT DevKit** simulado que envia telemetria de temperatura e umidade. Você adicionou esse modelo de dispositivo e criou um dispositivo simulado no início rápido [Adicionar um dispositivo simulado ao seu aplicativo de IoT Central](./quick-create-simulated-device.md). A regra monitora a temperatura relatada pelo dispositivo e envia um email quando ultrapassa os 70 graus.

1. No painel esquerdo, selecione **Regras**.

1. Se você ainda não tiver criado nenhuma regra, verá a seguinte tela:

    ![Não há regras](media/tutorial-create-telemetry-rules/rules-landing-page1.png)

1. Selecione **+** para adicionar uma nova regra.

1. Insira o nome _Monitor de temperatura_ para identificar a regra e pressione Enter.

1. Selecione o modelo de dispositivo **MXChip IoT DevKit**. Por padrão, a regra aplica-se automaticamente a todos os dispositivos associados ao modelo de dispositivo. Para filtrar um subconjunto dos dispositivos, selecione **+ Filtro** e use as propriedades do dispositivo para identificar os dispositivos. Para desabilitar a regra, alterne o botão **Habilitada/Desabilitada** no cabeçalho da regra:

    ![Filtros e habilitação](media/tutorial-create-telemetry-rules/device-filters.png)

### <a name="configure-the-rule-conditions"></a>Configurar as condições de regra

As condições definem os critérios monitorados pela regra. Neste tutorial, você configurará a regra a ser acionada quando a temperatura exceder 70&deg; F.

1. Selecione **Temperatura** no menu suspenso **Telemetria**.

1. Em seguida, escolha **É maior que** como o **Operador** e insira _70_ como o **Valor**.

    ![Condição](media/tutorial-create-telemetry-rules/condition-filled-out1.png)

1. Opcionalmente, você pode definir uma **Agregação de tempo**. Ao selecionar uma agregação de tempo, você deve selecionar também um tipo de agregação, como média ou soma do menu suspenso de agregação.

    * Sem agregação, a regra é disparada para cada ponto de dados de telemetria que atenda à condição. Por exemplo, se você configurar a regra para disparar quando temperatura estiver acima de 70, então ela será disparada quase instantaneamente quando a temperatura do dispositivo exceder esse valor.
    * Com a agregação, a regra será disparada se o valor de agregação dos pontos de dados de telemetria na janela de tempo atender à condição. Por exemplo, se você configurar a regra para disparar quando a temperatura estiver acima de 70 e com uma agregação de tempo média de 10 minutos, então a regra será disparada quando o dispositivo relatar uma temperatura média maior que 70, calculada durante um intervalo de 10 minutos.

     ![Condição de agregação](media/tutorial-create-telemetry-rules/aggregate-condition-filled-out1.png)

Você pode adicionar várias condições a uma regra selecionando **+ Condição**. Quando várias condições são especificadas, todas as condições devem ser atendidas para que a regra seja disparada. Cada condição é unida por uma cláusula `AND` implícita. Se você estiver usando a agregação de tempo com várias condições, todos os valores de telemetria deverão ser agregados.

### <a name="configure-actions"></a>Configurar ações

Após definir a condição, configure as ações a serem executadas quando a regra for acionada. As ações são invocadas quando todas as condições especificadas na regra são avaliadas como true.

1. Selecione **+ Email** na seção **Ações**.

1. Insira _Aviso de temperatura_ como o nome de exibição para a ação, seu endereço de email no campo **Para** e _Você deve verificar o dispositivo!_ como uma observação que deve ser exibida no corpo do email.

    > [!NOTE]
    > Os emails serão enviados apenas aos usuários que foram adicionados ao aplicativo e fizeram logon pelo menos uma vez. Saiba mais sobre o [gerenciamento de usuários](howto-administer.md) no Azure IoT Central.

   ![Configurar Ação](media/tutorial-create-telemetry-rules/configure-action1.png)

1. Para salvar a ação, escolha **Concluído**. Você pode adicionar várias ações a uma regra.

1. Para salvar a regra, escolha **Salvar**. A regra entra em ação em poucos minutos e começa a monitorar a telemetria enviada ao aplicativo. Quando a condição especificada na regra é atendida, a regra aciona a ação de e-mail configurada.

Após alguns instantes, você receberá uma mensagem de email quando a regra for acionada:

![Email de exemplo](media/tutorial-create-telemetry-rules/email.png)

## <a name="delete-a-rule"></a>Excluir uma regra

Se você não precisar mais de uma regra, exclua-a, abrindo a regra e escolhendo **Excluir**.

## <a name="enable-or-disable-a-rule"></a>Habilitar ou desabilitar uma regra

Escolha a regra que você deseja habilitar ou desabilitar. Alterne o botão **Habilitado/Desabilitado** na regra para habilitá-la ou desabilitá-la em todos os dispositivos no escopo.

## <a name="enable-or-disable-a-rule-for-specific-devices"></a>Habilitar ou desabilitar uma regra para dispositivos específicos

Escolha a regra que você deseja personalizar. Use um ou mais filtros na seção **Dispositivos de destino** para restringir o escopo da regra aos dispositivos que você deseja monitorar.

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você aprendeu a:

* Criar uma regra baseada em telemetria
* Adicionar uma ação

Agora que você definiu uma regra baseada em limite, a próxima etapa sugerida é aprender a:

> [!div class="nextstepaction"]
> [Configurar a exportação contínua de dados](./howto-export-data.md).
