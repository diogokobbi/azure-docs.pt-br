---
title: Instalar a atualização no dispositivo Azure Stack GPU pro Edge | Microsoft Docs
description: Descreve como aplicar atualizações usando o portal do Azure e a interface do usuário da Web local para o dispositivo Azure Stack Edge pro GPU e o cluster kubernetes no dispositivo
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 09/29/2020
ms.author: alkohli
ms.openlocfilehash: 7a534f794f7ab5323ad46ebc555e42b2514e94e2
ms.sourcegitcommit: f5580dd1d1799de15646e195f0120b9f9255617b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/29/2020
ms.locfileid: "91542335"
---
# <a name="update-your-azure-stack-edge-pro-gpu"></a>Atualizar sua GPU do Azure Stack Edge pro 

Este artigo descreve as etapas necessárias para instalar a atualização no Azure Stack Edge pro com GPU por meio da interface do usuário da Web local e por meio do portal do Azure. Aplique as atualizações de software ou hotfixes para manter seu Azure Stack dispositivo do Edge pro e o cluster kubernetes associado no dispositivo atualizado. 

O procedimento descrito neste artigo foi executado usando uma versão diferente do software, mas o processo permanece o mesmo para a versão atual do software.

> [!IMPORTANT]
> - A atualização **2009** corresponde à versão do **2.1.1364.2110** software em seu dispositivo. Para obter informações sobre essa atualização, acesse [notas de versão](azure-stack-edge-gpu-2009-release-notes.md).
>
> - Tenha em mente que instalar uma atualização ou um hotfix reinicia seu dispositivo. Essa atualização exige que você aplique duas atualizações sequencialmente. Primeiro, aplique as atualizações de software do dispositivo e, em seguida, as atualizações do kubernetes. Considerando que o Azure Stack Edge pro é um dispositivo de nó único, qualquer e/s em andamento será interrompida e o dispositivo apresentará um tempo de inatividade de até 30 minutos para a atualização de software do dispositivo.

Para instalar atualizações em seu dispositivo, primeiro você precisa configurar o local do servidor de atualização. Depois que o servidor de atualização estiver configurado, você poderá aplicar as atualizações por meio da interface do usuário do portal do Azure ou da interface do usuário da Web local.

Cada uma dessas etapas é descrita nas seções a seguir.

## <a name="configure-update-server"></a>Configurar servidor de atualização

1. Na interface do usuário da Web local, vá para **configuração**  >  **servidor de atualização**. 
   
    ![Configurar atualizações](./media/azure-stack-edge-gpu-install-update/configure-update-server-1.png)

2. Em **Selecionar tipo de servidor de atualização**, na lista suspensa, escolha do servidor de Microsoft Update (padrão) ou Windows Server Update Services.  
   
    Se estiver atualizando do Windows Server Update Services, especifique o URI do servidor. O servidor nesse URI implantará as atualizações em todos os dispositivos conectados a esse servidor.

    ![Configurar atualizações](./media/azure-stack-edge-gpu-install-update/configure-update-server-2.png)
    
    O servidor do WSUS é usado para gerenciar e distribuir atualizações por meio de um console de gerenciamento. Um servidor do WSUS também pode ser a fonte de atualização de outros servidores do WSUS na organização. O servidor WSUS que atua como fonte de atualização é chamado de servidor upstream. Em uma implementação do WSUS, pelo menos um servidor do WSUS na rede precisa conseguir se conectar ao Microsoft Update para obter as informações de atualizações disponíveis. Como administrador, você pode determinar, com base na segurança e na configuração da rede, quantos outros servidores WSUS se conectam diretamente ao Microsoft Update.
    
    Para obter mais informações, acesse [Windows Server Update Services (WSUS)](https://docs.microsoft.com/windows-server/administration/windows-server-update-services/get-started/windows-server-update-services-wsus)

## <a name="use-the-azure-portal"></a>Use o Portal do Azure

Recomendamos que você instale as atualizações por meio do portal do Azure. O dispositivo verifica automaticamente se há atualizações uma vez por dia. Depois que as atualizações estiverem disponíveis, você verá uma notificação no Portal. Em seguida, você pode baixar e instalar as atualizações. 

> [!NOTE]
> Verifique se o dispositivo está íntegro e se o status aparece como **online** antes de prosseguir com a instalação das atualizações.

1. Quando as atualizações estiverem disponíveis para seu dispositivo, você verá uma notificação. Selecione a notificação ou na barra de comandos superior, **Atualizar dispositivo**. Isso permitirá que você aplique atualizações de software do dispositivo.

    ![Versão do software após a atualização](./media/azure-stack-edge-gpu-install-update/portal-update-1.png)

2. Na folha **atualizações do dispositivo** , verifique se você analisou os termos de licença associados aos novos recursos nas notas de versão.

    Você pode optar por **baixar e instalar** as atualizações ou apenas **baixar** as atualizações. Em seguida, você pode optar por instalar essas atualizações mais tarde.

    ![Versão do software após a atualização](./media/azure-stack-edge-gpu-install-update/portal-update-2a.png)    

    Se você quiser baixar e instalar as atualizações, verifique a opção que as atualizações instalam automaticamente após a conclusão do download.

    ![Versão do software após a atualização](./media/azure-stack-edge-gpu-install-update/portal-update-2b.png)

3. O download das atualizações é iniciado. Você verá uma notificação informando que o download está em andamento.

    ![Versão do software após a atualização](./media/azure-stack-edge-gpu-install-update/portal-update-3.png)

    Uma faixa de notificação também é exibida no portal do Azure. Isso indica o progresso do download. 

    ![Versão do software após a atualização](./media/azure-stack-edge-gpu-install-update/portal-update-4.png)

    Você pode selecionar essa notificação ou selecionar **Atualizar dispositivo** para ver o status detalhado da atualização.

    ![Versão do software após a atualização](./media/azure-stack-edge-gpu-install-update/portal-update-5.png)   


4. Depois que o download for concluído, a faixa de notificação será atualizada para indicar a conclusão. Se você optar por baixar e instalar as atualizações, a instalação será iniciada automaticamente.

    ![Versão do software após a atualização](./media/azure-stack-edge-gpu-install-update/portal-update-6.png)

    Se você optar por baixar apenas as atualizações, selecione a notificação para abrir a folha **atualizações do dispositivo** . Selecione **Instalar**.
  
    ![Versão do software após a atualização](./media/azure-stack-edge-gpu-install-update/portal-update-7.png)

5. Você verá uma notificação de que a instalação está em andamento.

    ![Versão do software após a atualização](./media/azure-stack-edge-gpu-install-update/portal-update-8.png)

    O portal também exibe um alerta informativo para indicar que a instalação está em andamento. O dispositivo fica offline e está no modo de manutenção.
    
    ![Versão do software após a atualização](./media/azure-stack-edge-gpu-install-update/portal-update-9.png)

6. Como esse é um dispositivo de 1 nó, o dispositivo será reiniciado depois que as atualizações forem instaladas. O alerta crítico durante a reinicialização indicará que a pulsação do dispositivo será perdida.

    ![Versão do software após a atualização](./media/azure-stack-edge-gpu-install-update/portal-update-10.png)

    Selecione o alerta para ver o evento de dispositivo correspondente.
    
    ![Versão do software após a atualização](./media/azure-stack-edge-gpu-install-update/portal-update-11.png)


7. Após a reinicialização, o dispositivo é colocado novamente no modo de manutenção e um alerta informativo é exibido para indicar que.

    Se você selecionar o **dispositivo de atualização** na barra de comandos superior, poderá ver o progresso das atualizações.   

8. O status do dispositivo é atualizado para **online** depois que as atualizações são instaladas. 

    ![Versão do software após a atualização](./media/azure-stack-edge-gpu-install-update/portal-update-14.png)

    Na barra de comandos superior, selecione **atualizações de dispositivo**. Verifique se a atualização foi instalada com êxito e se a versão do software do dispositivo reflete isso.

    ![Versão do software após a atualização](./media/azure-stack-edge-gpu-install-update/portal-update-15.png)

9. Você verá novamente uma notificação de que as atualizações estão disponíveis. Essas são as atualizações do kubernetes. Selecione a notificação ou selecione **Atualizar dispositivo** na barra de comandos superior.

    ![Versão do software após a atualização](./media/azure-stack-edge-gpu-install-update/portal-update-16.png)

10. Baixe as atualizações do kubernetes. Você pode ver que o tamanho do pacote é diferente quando comparado ao pacote de atualização anterior.

    ![Versão do software após a atualização](./media/azure-stack-edge-gpu-install-update/portal-update-17.png)

    O processo de instalação é idêntico ao das atualizações de dispositivo. Primeiro, as atualizações são baixadas.

    ![Versão do software após a atualização](./media/azure-stack-edge-gpu-install-update/portal-update-18.png)    
    
11. Depois que as atualizações forem baixadas, você poderá instalar as atualizações. 

    ![Versão do software após a atualização](./media/azure-stack-edge-gpu-install-update/portal-update-19.png)

    À medida que as atualizações são instaladas, o dispositivo é colocado no modo de manutenção. O dispositivo não é reiniciado para as atualizações do kubernetes. 

    Depois que as atualizações do kubernetes forem instaladas com êxito, a notificação de faixa desaparece, pois nenhuma atualização adicional é necessária. Seu dispositivo agora tem a versão mais recente do software do dispositivo e do kubernetes.

    ![Versão do software após a atualização](./media/azure-stack-edge-gpu-install-update/portal-update-20.png)


## <a name="use-the-local-web-ui"></a>Usar a interface do usuário da Web local

Há duas etapas ao usar a interface do usuário da Web local:

* Baixe a atualização ou o hotfix
* Instale a atualização ou o hotfix

Cada uma dessas etapas é descrita mais detalhadamente nas seções a seguir.


### <a name="download-the-update-or-the-hotfix"></a>Baixe a atualização ou o hotfix

Execute as etapas a seguir para baixar a atualização. Você pode baixar a atualização do local fornecido pela Microsoft ou do catálogo Microsoft Update.


Execute as etapas a seguir para baixar a atualização do catálogo Microsoft Update.

1. Inicie o navegador e navegue até [https://catalog.update.microsoft.com](https://catalog.update.microsoft.com) .

    ![Pesquisar o catálogo](./media/azure-stack-edge-gpu-install-update/download-update-1.png)

2. Na caixa de pesquisa do catálogo Microsoft Update, insira o número da base de dados de conhecimento (KB) do hotfix ou os termos da atualização que você deseja baixar. Por exemplo, digite **Azure Stack Edge pro**e clique em **Pesquisar**.
   
    A listagem de atualização aparece como **Azure Stack Edge Pro 2006**.
   
    ![Pesquisar o catálogo](./media/azure-stack-edge-gpu-install-update/download-update-2b.png)

4. Selecione **Baixar**. Há dois arquivos a serem baixados com *SoftwareUpdatePackage.exe* e *Kubernetes_Package.exe* sufixos que correspondem às atualizações de software do dispositivo e às atualizações do kubernetes, respectivamente. Baixe os arquivos em uma pasta no sistema local. Você também pode copiar a pasta para um compartilhamento de rede que é acessível do dispositivo.

### <a name="install-the-update-or-the-hotfix"></a>Instale a atualização ou o hotfix

Antes da instalação de atualização ou hotfix, certifique-se de que:

 - Você tem a atualização ou hotfix baixado localmente em seu host ou acessível por meio de um compartilhamento de rede.
 - O status do dispositivo é íntegro, conforme mostrado na página **visão geral** da interface do usuário da Web local.

   ![atualizar dispositivo](./media/azure-stack-edge-gpu-install-update/local-ui-update-1.png) 

Esse procedimento leva cerca de 20 minutos para ser concluído. Execute as etapas a seguir para instalar a atualização ou o hotfix.

1. Na interface do usuário da Web local, vá para **manutenção**  >  **atualização de software**. Anote a versão do software que você está executando. 
   
   ![atualizar dispositivo](./media/azure-stack-edge-gpu-install-update/local-ui-update-2.png)

2. Forneça o caminho para o arquivo de atualização. Você também pode navegar até o arquivo de instalação de atualização, se colocado em um compartilhamento de rede. Selecione o arquivo de atualização de software com o sufixo *SoftwareUpdatePackage.exe* .

   ![atualizar dispositivo](./media/azure-stack-edge-gpu-install-update/local-ui-update-3a.png)

3. Selecione **Aplicar**. 

   ![atualizar dispositivo](./media/azure-stack-edge-gpu-install-update/local-ui-update-4.png)

4. Quando a confirmação for solicitada, selecione **Sim** para continuar. Considerando que o dispositivo é um dispositivo de nó único, depois que a atualização é aplicada, o dispositivo é reiniciado e há um tempo de inatividade. 
   
   ![atualizar dispositivo](./media/azure-stack-edge-gpu-install-update/local-ui-update-5.png)

5. A atualização será iniciada. Depois que for atualizado com êxito, o dispositivo será reiniciado. A interface do usuário local não estará acessível durante esse tempo.
   
6. Depois que a reinicialização for concluída, você será levado à página **Entrar** . Para verificar se o software do dispositivo foi atualizado, na interface do usuário da Web local, vá para **manutenção**  >  **atualização de software**. A versão de software exibida neste exemplo é **2.0.1257.1591**.

   ![atualizar dispositivo](./media/azure-stack-edge-gpu-install-update/local-ui-update-6.png) 

7. Agora, você atualizará a versão do software kubernetes. Repita as etapas acima. Forneça um caminho para o arquivo de atualização kubernetes com o sufixo *Kubernetes_Package.exe* .  

   <!--![update device](./media/azure-stack-edge-gpu-install-update/local-ui-update-7.png)--> 

8. Selecione **Aplicar**. 

   ![atualizar dispositivo](./media/azure-stack-edge-gpu-install-update/local-ui-update-8.png)

9. Quando a confirmação for solicitada, selecione **Sim** para continuar. 

10. Depois que a atualização do kubernetes for instalada com êxito, não haverá alteração no software exibido na **Maintenance**  >  **atualização de software**de manutenção. 


## <a name="next-steps"></a>Próximas etapas

Saiba mais sobre como [administrar seu Azure Stack Edge pro](azure-stack-edge-manage-access-power-connectivity-mode.md).
