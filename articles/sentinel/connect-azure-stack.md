---
title: Integre suas máquinas virtuais Azure Stack ao Azure sentinela | Microsoft Docs
description: Este artigo mostra como provisionar a extensão de máquina virtual de gerenciamento de Azure Monitor, atualização e configuração em Azure Stack máquinas virtuais e começar a monitorá-las com o sentinela.
services: sentinel
documentationcenter: na
author: yelevin
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/23/2019
ms.author: yelevin
ms.openlocfilehash: 8114a5e6db7b82b846d221471f41dbdf418ddd9d
ms.sourcegitcommit: 419cf179f9597936378ed5098ef77437dbf16295
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89001987"
---
# <a name="connect-azure-stack-virtual-machines-to-azure-sentinel"></a>Conectar Azure Stack máquinas virtuais ao Azure Sentinel




Com o Azure Sentinel, você pode monitorar suas VMs em execução no Azure e Azure Stack em um único local. Para integrar seus computadores Azure Stack ao Azure Sentinel, primeiro você precisa adicionar a extensão da máquina virtual às suas máquinas virtuais Azure Stack existentes. 

Depois de conectar Azure Stack computadores, escolha um de uma galeria de painéis que insights de superfície com base em seus dados. Esses painéis podem ser facilmente personalizados para suas necessidades.



## <a name="add-the-virtual-machine-extension"></a>Adicionar a extensão da máquina virtual 

Adicione a extensão de máquina virtual de **Gerenciamento de Azure monitor, atualização e configuração** para as máquinas virtuais em execução no seu Azure Stack. 

1. Em uma nova guia do navegador, faça logon em seu [portal de Azure Stack](https://docs.microsoft.com/azure-stack/user/azure-stack-use-portal#access-the-portal).
2. Vá para a página **máquinas virtuais** , selecione a máquina virtual que você deseja proteger com o Azure Sentinel. Para obter informações sobre como criar uma máquina virtual em Azure Stack, consulte [criar uma VM do Windows Server com o portal do Azure Stack](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-quick-windows-portal) ou [criar uma VM do servidor Linux usando o portal do Azure Stack](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-quick-linux-portal).
3. Selecione **Extensões**. A lista de extensões da máquina virtual instaladas nesta máquina virtual é mostrada.
4. Clique na guia **Adicionar**. A folha de menu **Novo recurso** é aberta e mostra a lista de extensões da máquina virtual disponíveis. 
5. Selecione a extensão de **Gerenciamento de Azure monitor, atualização e configuração** e clique em **criar**. A janela instalar configuração de **extensão** é aberta.

   ![Configurações de Azure Monitor, atualização e gerenciamento de configuração](./media/connect-azure-stack/azure-monitor-extension-fix.png)  

   >[!NOTE]
   > Se você não vir a extensão de **Azure monitor, atualização e gerenciamento de configuração** listada em seu Marketplace, entre em contato com seu operador de Azure Stack para disponibilizá-lo.

6. No menu do Azure Sentinel, selecione **configurações do espaço de trabalho** seguido por **avançado**e copie a **ID do espaço** de trabalho e a **chave do espaço de trabalho (chave primária)**. 
1. Na janela Azure Stack **instalar extensão** , Cole-os nos campos indicados e clique em **OK**.
1. Depois que a instalação da extensão for concluída, seu status será exibido como **provisionamento bem-sucedido**. Pode levar até uma hora para a máquina virtual aparecer no portal do Azure Sentinel.

Para obter mais informações sobre como instalar e configurar o agente para Windows, consulte [conectar computadores Windows](../azure-monitor/platform/agent-windows.md#install-agent-using-setup-wizard).

Para solução de problemas de agente do Linux, confira [Troubleshoot Azure Log Analytics Linux Agent](../azure-monitor/platform/agent-linux-troubleshoot.md) (Solucionar problemas do agente do Linux do Azure Log Analytics).

No portal do Azure Sentinel no Azure, em **máquinas virtuais**, você tem uma visão geral de todas as VMs e computadores junto com seu status. 

## <a name="clean-up-resources"></a>Limpar os recursos
Quando não for mais necessário, será possível remover a extensão da máquina virtual por meio do portal do Azure Stack.

Para remover a extensão:

1. Abra o **portal do Azure Stack**.
2. Acesse a página **Máquinas virtuais**, selecione a máquina virtual da qual você deseja remover a extensão.
3. Selecione **Extensões**, selecione a extensão **Microsoft.EnterpriseCloud.Monitoring**.
4. Clique em **desinstalar**e confirme sua seleção.

## <a name="next-steps"></a>Próximas etapas

Para saber mais sobre o Azure Sentinel, consulte os seguintes artigos:
- Saiba como [obter visibilidade dos seus dados e possíveis ameaças](quickstart-get-visibility.md).
- Comece a [detectar ameaças com o Azure Sentinel](tutorial-detect-threats-built-in.md).
- Transmita dados de [dispositivos com Formato de Erro Comuns](connect-common-event-format.md) para o Azure Sentinel.
