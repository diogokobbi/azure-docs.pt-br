---
title: Lista de URL segura para área de trabalho virtual do Windows-Azure
description: Uma lista de URLs que você deve desbloquear para garantir que sua implantação de área de trabalho virtual do Windows funcione conforme o esperado.
author: Heidilohr
ms.topic: conceptual
ms.date: 08/12/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: f9f68d3734cd7de83a2ddd376caefa410c619d61
ms.sourcegitcommit: 58d3b3314df4ba3cabd4d4a6016b22fa5264f05a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89291102"
---
# <a name="safe-url-list"></a>Lista de URL segura

Você precisará desbloquear determinadas URLs para que sua implantação de desktop virtual do Windows funcione corretamente. Este artigo lista essas URLs para que você saiba quais são seguras.

## <a name="virtual-machines"></a>Máquinas virtuais

As máquinas virtuais do Azure criadas para a Área de Trabalho Virtual do Windows precisam ter acesso às seguintes URLs:

|Endereço|Porta TCP de saída|Finalidade|Marca de serviço|
|---|---|---|---|
|*.wvd.microsoft.com|443|Tráfego de serviço|WindowsVirtualDesktop|
|mrsglobalsteus2prod.blob.core.windows.net|443|Atualizações do agente e da pilha de SXS|AzureCloud|
|*.core.windows.net|443|Tráfego de agente|AzureCloud|
|*.servicebus.windows.net|443|Tráfego de agente|AzureCloud|
|gcs.prod.monitoring.core.windows.net|443|Tráfego de agente|AzureCloud|
|catalogartifact.azureedge.net|443|Azure Marketplace|AzureCloud|
|kms.core.windows.net|1688|Ativação do Windows|Internet|
|wvdportalstorageblob.blob.core.windows.net|443|Suporte do portal do Azure|AzureCloud|
| 169.254.169.254 | 80 | [Ponto de extremidade do serviço de metadados de instância do Azure](../virtual-machines/windows/instance-metadata-service.md) | N/D |
| 168.63.129.16 | 80 | [Monitoramento de integridade do host de sessão](../virtual-network/security-overview.md#azure-platform-considerations) | N/D |

>[!IMPORTANT]
>Agora, a Área de Trabalho Virtual do Windows dá suporte à marca FQDN. Para obter mais informações, confira [Usar o Firewall do Azure para proteger implantações da Área de Trabalho Virtual do Windows](../firewall/protect-windows-virtual-desktop.md).
>
>Recomendamos que você use marcas de FQDN ou marcas de serviço, em vez de URLs, para evitar problemas de serviço. As URLs e as marcas listadas correspondem apenas aos sites e aos recursos da Área de Trabalho Virtual do Windows. Não incluem URLs para outros serviços, como Azure Active Directory.

A seguinte tabela lista as URLs opcionais às quais suas máquinas virtuais do Azure podem ter acesso:

|Endereço|Porta TCP de saída|Finalidade|Marca de serviço|
|---|---|---|---|
|*.microsoftonline.com|443|Autenticação no Microsoft Online Services|Nenhum|
|*.events.data.microsoft.com|443|Serviço de telemetria|Nenhum|
|www.msftconnecttest.com|443|Detecta se o sistema operacional está conectado à Internet|Nenhum|
|*.prod.do.dsp.mp.microsoft.com|443|Windows Update|Nenhum|
|login.windows.net|443|Entrar nos Serviços Online da Microsoft, Microsoft 365|Nenhum|
|*.sfx.ms|443|Atualizações do software cliente do OneDrive|Nenhum|
|*.digicert.com|443|Verificação de revogação do certificado|Nenhum|

>[!NOTE]
>A área de trabalho virtual do Windows atualmente não tem uma lista de intervalos de endereços IP que você pode desbloquear para permitir o tráfego de rede. Há suporte apenas para desbloquear URLs específicas no momento.
>
>Para obter uma lista de URLs seguras relacionadas ao Office, incluindo URLs necessárias relacionadas ao Azure Active Directory, consulte [URLs e intervalos de endereços IP do office 365](/office365/enterprise/urls-and-ip-address-ranges).
>
>Você precisa usar o caractere curinga (*) para URLs que envolvem tráfego de serviço. Se preferir não usar * para o tráfego relacionado ao agente, encontre as URLs sem curinga da seguinte forma:
>
>1. Registre suas máquinas virtuais no pool de hosts da Área de Trabalho Virtual do Windows.
>2. Abra o **Visualizador de eventos**, vá para logs do **Windows**  >  **Application**  >  **WVD-Agent** e procure a ID de evento 3701.
>3. Desbloqueie as URLs que você encontrar na ID de evento 3701. As URLs em ID de evento 3701 são específicas da região. Você precisará repetir o processo de desbloqueio com as URLs relevantes para cada região em que você deseja implantar suas máquinas virtuais.

## <a name="remote-desktop-clients"></a>Clientes de área de trabalho remota

Qualquer cliente Área de Trabalho Remota que você usar deve ter acesso às seguintes URLs:

|Endereço|Porta TCP de saída|Finalidade|Cliente(s)|
|---|---|---|---|
|*.wvd.microsoft.com|443|Tráfego de serviço|Todos|
|*.servicebus.windows.net|443|Solucionar problemas de dados|Todos|
|go.microsoft.com|443|FWLinks da Microsoft|Todos|
|aka.ms|443|Redutor de URL da Microsoft|Todos|
|docs.microsoft.com|443|Documentação|Todos|
|privacy.microsoft.com|443|Política de privacidade|Todos|
|query.prod.cms.rt.microsoft.com|443|Atualizações do cliente|Área de Trabalho do Windows|

>[!IMPORTANT]
>Abrir essas URLs é essencial para ter uma experiência do cliente confiável. Não há suporte ao bloqueio do acesso a essas URLs e isso afetará a funcionalidade do serviço.
>
>Essas URLs correspondem apenas aos sites e recursos do cliente. Essa lista não inclui URLs para outros serviços, como Azure Active Directory. Azure Active Directory URLs podem ser encontradas na ID 56 nas [URLs do Office 365 e nos intervalos de endereços IP](/office365/enterprise/urls-and-ip-address-ranges#microsoft-365-common-and-office-online).
