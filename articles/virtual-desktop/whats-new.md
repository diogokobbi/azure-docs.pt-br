---
title: O que há de novo na Área de Trabalho Virtual do Windows? - Azure
description: Novos recursos e atualizações de produto para a Área de Trabalho Virtual do Windows.
author: Heidilohr
ms.topic: overview
ms.date: 09/02/2020
ms.author: helohr
ms.reviewer: thhickli; darank
manager: lizross
ms.custom: references_regions
ms.openlocfilehash: d698470f450f6fe903ab68334764e0918d659d7f
ms.sourcegitcommit: 5ed504a9ddfbd69d4f2d256ec431e634eb38813e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89319807"
---
# <a name="whats-new-in-windows-virtual-desktop"></a>O que há de novo na Área de Trabalho Virtual do Windows?

A Área de Trabalho Virtual do Windows é atualizada regularmente. Neste artigo, você descobrirá:

- As últimas atualizações
- Novos recursos
- Melhorias aos recursos existentes
- Correções de bug

Este artigo é atualizado mensalmente. Volte a consultá-lo com frequência para acompanhar as atualizações.

## <a name="august-2020"></a>Agosto de 2020

Estas foram as alterações de agosto de 2020:

- Aprimoramos o desempenho para reduzir a latência de conexão nas seguintes regiões do Azure: 

    - Reino Unido
    - França
    - Noruega
    - Coreia do Sul

   Você pode usar o [avaliador de experiência](https://azure.microsoft.com/services/virtual-desktop/assessment/) para ter uma ideia geral de como essas alterações afetarão os usuários.

- O Cliente de Área de Trabalho Remota da Microsoft Store (v10.2.1522 e posterior) já está em disponibilidade geral. Esta versão do Cliente de Área de Trabalho Remota da Microsoft Store é compatível com a Área de Trabalho Virtual do Windows. Também introduzimos fluxos de interface do usuário atualizados para proporcionar experiências aprimoradas para os usuários. Essa atualização inclui os modos de Fluent Design, claro e escuro e muitas outras alterações interessantes. Também reescrevemos o cliente para que ele use o mesmo mecanismo subjacente do protocolo RDP dos clientes iOS, macOS e Android. Isso nos permite fornecer novos recursos com maior velocidade em todas as plataformas. [Baixe o cliente](https://www.microsoft.com/p/microsoft-remote-desktop/9wzdncrfj3ps?rtc=1&activetab=pivot:overviewtab) e experimente-o.

- Corrigimos um problema no cliente do Teams Desktop (versão 1.3.00.21759) em que o cliente só mostrava o fuso horário UTC no chat, nos canais e no calendário. O cliente atualizado agora mostra o fuso horário da sessão remota.

- O Assistente do Azure agora faz parte da Área de Trabalho Virtual do Windows. Ao acessar a Área de Trabalho Virtual do Windows por meio do portal do Azure, você poderá ver recomendações para otimizar o ambiente da Área de Trabalho Virtual do Windows. Saiba mais em [Assistente do Azure](azure-advisor.md).

- A CLI do Azure agora dá suporte à Área de Trabalho Virtual do Windows (`az desktopvirtualization`) para ajudar você a automatizar suas implantações da Área de Trabalho Virtual do Windows. Confira [desktopvirtualization](/cli/azure/ext/desktopvirtualization/?view=azure-cli-latest) para obter uma lista de comandos de extensão.

- Atualizamos nossos modelos de implantação para torná-los totalmente compatíveis com as interfaces do Azure Resource Manager na Área de Trabalho Virtual do Windows. Encontre os modelos no [GitHub](https://github.com/Azure/RDS-Templates/tree/master/ARM-wvd-templates).

- O portal de US Gov da Área de Trabalho Virtual do Windows já está em versão prévia pública. Para saber mais, confira [nossos comunicados](https://azure.microsoft.com/updates/windows-virtual-desktop-is-now-available-in-the-azure-government-cloud-in-preview/).

## <a name="july-2020"></a>Julho de 2020  

Foi em julho que a integração da Área de Trabalho Virtual do Windows ao Azure Resource Manager entrou em disponibilidade geral.

Aqui está o que mudou nesta nova versão: 

- A "versão Fall 2019" agora é conhecida como "Área de Trabalho Virtual do Windows (Clássica)", enquanto a "versão Spring 2020" agora é apenas "Área de Trabalho Virtual do Windows". Para obter mais informações, confira [esta postagem no blog](https://azure.microsoft.com/blog/new-windows-virtual-desktop-capabilities-now-generally-available/). 

Para saber mais sobre os novos recursos, confira [esta postagem no blog](https://techcommunity.microsoft.com/t5/itops-talk-blog/windows-virtual-desktop-spring-update-enters-public-preview/ba-p/1340245). 

### <a name="autoscaling-tool-update"></a>Atualização da ferramenta de dimensionamento automático

A versão mais recente da ferramenta de dimensionamento automático que estava em versão prévia agora está em disponibilidade geral. Essa ferramenta usa uma conta de automação do Azure e o Aplicativo Lógico do Azure para desligar e reiniciar automaticamente as VMs (máquinas virtuais) do host da sessão em um pool de host, reduzindo os custos de infraestrutura. Saiba mais em [Dimensionar hosts da sessão usando a Automação do Azure](set-up-scaling-script.md).

### <a name="azure-portal"></a>Portal do Azure

Agora você pode fazer o seguinte com o portal do Azure na Área de Trabalho Virtual do Windows: 

- Atribuir usuários diretamente a hosts da sessão de área de trabalho pessoal  
- Alterar a configuração do ambiente de validação para pools de hosts 

### <a name="diagnostics"></a>Diagnósticos

Lançamos algumas novas consultas predefinidas para o workspace do Log Analytics. Para acessar as consultas, acesse **Logs** e, em **Categoria**, selecione **Área de Trabalho Virtual do Windows**. Saiba mais em [Usar o Log Analytics para o recurso de diagnóstico](diagnostics-log-analytics.md).

### <a name="update-for-remote-desktop-client-for-android"></a>Atualização para o cliente da Área de Trabalho Remota para Android

O [cliente da Área de Trabalho Remota para Android](https://play.google.com/store/apps/details?id=com.microsoft.rdc.androidx) agora dá suporte a conexões de área de trabalho virtual do Windows. Da versão 10.0.7 em diante, o cliente Android apresenta uma nova interface do usuário para melhorar a experiência dos usuários. O cliente também se integra ao Microsoft Authenticator em dispositivos Android para habilitar o acesso condicional ao assinar workspaces da Área de Trabalho Virtual do Windows.  

A versão anterior do cliente da Área de Trabalho Remota agora é chamada de "Área de Trabalho Remota 8". Todas as conexões existentes que você tiver na versão anterior do cliente serão transferidas diretamente para o novo cliente. O novo cliente foi reescrito para o mesmo mecanismo de núcleo de RDP subjacente que os clientes iOS e macOS, com uma versão mais rápida de novos recursos em todas as plataformas. 

### <a name="teams-update"></a>Atualização do Teams

Fizemos melhorias ao Microsoft Teams para Área de Trabalho Virtual do Windows. Mais importante, a Área de Trabalho Virtual do Windows agora dá suporte à otimização de áudio e vídeo para o cliente de Área de Trabalho do Windows. O redirecionamento melhora a latência criando caminhos diretos entre os usuários quando eles usam áudio ou vídeo em chamadas e reuniões. Menos distância significa menos saltos, tornando as chamadas mais tranquilas. Saiba mais em [Usar o Teams a Área de Trabalho Virtual do Windows](teams-on-wvd.md).

## <a name="june-2020"></a>Junho de 2020

No mês passado, introduzimos a Área de Trabalho Virtual do Windows com integração do Azure Resource Manager em versão prévia. Essa atualização tem muitos recursos novos incríveis que adoraríamos apresentar. Aqui estão as novidades dessa versão da Área de Trabalho Virtual do Windows.

### <a name="windows-virtual-desktop-is-now-integrated-with-azure-resource-manager"></a>A Área de Trabalho Virtual do Windows agora está integrada ao Azure Resource Manager

A Área de Trabalho Virtual do Windows agora está integrada ao Azure Resource Manager. Na atualização mais recente, todos os objetos da Área de Trabalho Virtual do Windows agora são recursos do Azure Resource Manager. Essa atualização também é integrada ao Azure RBAC (Controle de Acesso Baseado em Função do Azure). Confira [O que é o Azure Resource Manager?](../azure-resource-manager/management/overview.md) para saber mais.

Isto é o que essa alteração faz para você:

- A Área de Trabalho Virtual do Windows agora está integrada ao portal do Azure. Isso significa que você pode gerenciar tudo diretamente no portal, sem o PowerShell, aplicativos Web ou ferramentas de terceiros. Para começar, confira nosso tutorial em [Criar um pool de host com o portal do Azure](create-host-pools-azure-marketplace.md).

- Antes dessa atualização, você só podia publicar RemoteApps e Desktops para usuários individuais. Com o Azure Resource Manager, agora você pode publicar recursos em grupos do Azure Active Directory.

- A versão anterior da Área de Trabalho Virtual do Windows tinha quatro funções de administrador internas que você podia atribuir a um locatário ou a um pool de host. Essas funções agora estão no [Azure RBAC (controle de acesso baseado em função do Azure)](../role-based-access-control/overview.md). Você pode aplicar essas funções a cada objeto do Azure Resource Manager da Área de Trabalho Virtual do Windows, o que lhe permite ter um modelo de delegação completo e avançado.

- Nessa atualização, você não precisa mais executar o Azure Marketplace nem o modelo do GitHub repetidamente para expandir um pool de host. Para expandir um pool de host, basta acessá-lo no portal do Azure e selecionar **+ Adicionar** para implantar hosts de sessão adicionais.

- A implantação do pool de host agora está totalmente integrada à [Galeria de Imagens Compartilhadas do Azure](../virtual-machines/windows/shared-image-galleries.md). A Galeria de Imagens Compartilhadas é um serviço do Azure separado que armazena definições de imagem de VM (máquina virtual), incluindo o controle de versão de imagem. Você também pode usar a replicação global para copiar e enviar suas imagens para outras regiões do Azure para implantação local.

- As funções de monitoramento que costumavam ser feitas por meio do PowerShell ou do aplicativo Web do Serviço de Diagnóstico agora foram movidas para Log Analytics no portal do Azure. Agora, você também tem duas opções para visualizar seus relatórios. Você pode executar consultas do Kusto e usar Pastas de Trabalho para criar relatórios visuais.

- Não é mais necessário o consentimento completo do Azure Active Directory (Azure AD) para usar a Área de Trabalho Virtual do Windows. Nessa atualização, o locatário do Azure AD na sua assinatura do Azure autentica os usuários e fornece controles RBAC do Azure para seus administradores.


### <a name="powershell-support"></a>Suporte ao PowerShell

Adicionamos novos cmdlets AzWvd ao módulo AZ do Azure PowerShell com essa atualização. Esse novo módulo tem suporte no PowerShell Core, que é executado no .NET Core.

Para instalar o módulo, siga as instruções em [Configurar o módulo do PowerShell para a Área de Trabalho Virtual do Windows](powershell-module.md).

Você também pode ver uma lista de comandos disponíveis na [referência do PowerShell AzWvd](/powershell/module/az.desktopvirtualization/?view=azps-4.2.0#desktopvirtualization).

Para obter mais informações sobre os novos recursos, confira [nossa postagem no blog](https://techcommunity.microsoft.com/t5/itops-talk-blog/windows-virtual-desktop-spring-update-enters-public-preview/ba-p/1340245).

### <a name="additional-gateways"></a>Gateways adicionais

Adicionamos um cluster de gateway na África do Sul para reduzir a latência de conexão.

### <a name="microsoft-teams-on-windows-virtual-desktop-preview"></a>Microsoft Teams na Área de Trabalho Virtual do Windows (versão prévia)

Fizemos algumas melhorias ao Microsoft Teams para Área de Trabalho Virtual do Windows. O mais importante é que a Área de Trabalho Virtual do Windows agora dá suporte ao redirecionamento audiovisual para chamadas. O redirecionamento melhora a latência criando caminhos diretos entre os usuários quando eles chamam o uso de áudio ou vídeo. Menos distância significa menos saltos, tornando as chamadas mais tranquilas.

Para saber mais, confira [nossa postagem no blog](https://azure.microsoft.com/updates/windows-virtual-desktop-media-optimization-for-microsoft-teams-is-now-available-in-public-preview/).

## <a name="client-updates"></a>Atualizações do cliente

Confira estes artigos para saber mais sobre as atualizações para nossos clientes para Área de Trabalho Virtual do Windows e Serviços de Área de Trabalho Remota:

- [Windows](/windows-server/remote/remote-desktop-services/clients/windowsdesktop-whatsnew)
- [macOS](/windows-server/remote/remote-desktop-services/clients/mac-whatsnew)
- [iOS](/windows-server/remote/remote-desktop-services/clients/ios-whatsnew)
- [Android](/windows-server/remote/remote-desktop-services/clients/android-whatsnew)
- [Web](/windows-server/remote/remote-desktop-services/clients/web-client-whatsnew)

## <a name="next-steps"></a>Próximas etapas

Saiba mais sobre os planos futuros no [roteiro da Área de Trabalho Virtual do Windows no Microsoft 365](https://www.microsoft.com/microsoft-365/roadmap?filters=Windows%20Virtual%20Desktop).

