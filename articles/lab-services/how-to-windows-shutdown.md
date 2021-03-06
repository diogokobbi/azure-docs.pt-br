---
title: Guia para controlar o comportamento de desligamento do Windows no Azure Lab Services | Microsoft Docs
description: Etapas para desligar automaticamente uma máquina virtual do Windows ociosa e remover o comando de desligamento do Windows.
ms.topic: article
ms.date: 09/29/2020
ms.openlocfilehash: c6021131787dde4fe23ec4caad107bda2e20158a
ms.sourcegitcommit: f5580dd1d1799de15646e195f0120b9f9255617b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/29/2020
ms.locfileid: "91541553"
---
# <a name="guide-to-controlling-windows-shutdown-behavior"></a>Guia para controlar o comportamento de desligamento do Windows

O Azure Lab Services fornece vários controles de custo para garantir que as VMs (máquinas virtuais) do Windows não estejam em execução inesperadamente:
 - [Definir um agendamento](https://docs.microsoft.com/azure/lab-services/classroom-labs/tutorial-setup-classroom-lab#set-a-schedule-for-the-lab)
 - [Definir cotas para usuários](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-configure-student-usage#set-quotas-for-users)
 - [Habilitar desligamento automático ao desconectar](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-enable-shutdown-disconnect)

Mesmo com esses controles de custo, há situações em que uma VM Windows pode continuar a ser executada inesperadamente; e, como resultado, deduzir da cota do aluno:

- **A janela RDP é deixada aberta**
  
    Quando um aluno se conecta à sua VM usando o RDP, ele pode acidentalmente deixar a janela RDP aberta.  Desde que a janela RDP permaneça aberta, a configuração de **desligamento automático na desconexão** nunca entrará em vigor, pois ela só é disparada depois que a sessão RDP é desconectada.

- **O comando de desligamento do Windows é usado para desligar a VM**
  
    Um aluno pode usar o comando de desligamento do Windows ou outros mecanismos de desligamento fornecidos no Windows para desativar a VM em vez de usar [Azure Lab Services botão ' parar '](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-use-classroom-lab#start-or-stop-the-vm).  Quando isso acontece, da perspectiva do Azure Lab Services, a VM ainda está sendo usada.
    
Para ajudá-lo a evitar que essas situações ocorram, este guia fornece etapas para desligar automaticamente uma VM ociosa do Windows e remover o comando de desligamento do Windows do menu **Iniciar** .  

> [!NOTE]
> Uma VM também pode deduzir inesperadamente da cota quando o aluno inicia sua VM, mas nunca se conecta a ela usando o RDP.  Este guia *não* atende atualmente a esse cenário.  Em vez disso, os alunos devem ser lembrados para conectar-se imediatamente à sua VM usando o RDP depois de iniciá-lo; ou, eles devem parar a VM.

## <a name="remove-windows-shutdown-command-from-start-menu"></a>Remover o comando de desligamento do Windows do menu iniciar

As configurações de **política de grupo local** do Windows também permitem que você remova o comando de desligamento do menu **Iniciar** .

Para remover o comando de desligamento, você pode se conectar à VM de modelo e executar o script do PowerShell abaixo.

```powershell
Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\Explorer" -Name "HidePowerOptions" -Value 1 -Force
```

Ou, você pode optar por seguir estas etapas manuais usando a VM de modelo:

1. Pressione a tecla Windows, digite **gpedit**e, em seguida, selecione **Editar política de grupo (painel de controle)**.

1. Vá para **configuração do computador > Modelos Administrativos > menu iniciar e barra de tarefas**.  

    ![Editor de política de grupo local](./media/how-to-windows-shutdown/group-policy-shutdown.png)

1. Clique com o botão direito do mouse em **remover e impeça o acesso aos comandos desligar, reiniciar, suspender e hibernar**e clique em **Editar**.

1. Selecione a configuração **habilitado** e clique em **OK**:
 
   ![Configuração de desligamento](./media/how-to-windows-shutdown/edit-shutdown.png)

1. Observe que o comando de desligamento não aparece mais no menu **Iniciar** do Windows; somente o comando **Disconnect** é exibido.

    ![Comando de desligamento](./media/how-to-windows-shutdown/start-menu.png)

## <a name="next-steps"></a>Próximas etapas
Consulte o artigo sobre como preparar uma VM de modelo do Windows: [guia para configurar um computador de modelo do Windows no Azure Lab Services](how-to-prepare-windows-template.md)
