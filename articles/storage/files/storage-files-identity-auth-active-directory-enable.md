---
title: Visão geral-AD DS autenticação local para compartilhamentos de arquivos do Azure
description: Saiba mais sobre a autenticação do Active Directory Domain Services (AD DS) para compartilhamentos de arquivos do Azure. Este artigo passa por cenários de suporte, disponibilidade e explica como as permissões funcionam entre o AD DS e o Azure Active Directory.
author: roygara
ms.service: storage
ms.subservice: files
ms.topic: how-to
ms.date: 09/13/2020
ms.author: rogarana
ms.openlocfilehash: f64cad731998fefb2cfa694314e42f0dfb629eb4
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91322063"
---
# <a name="overview---on-premises-active-directory-domain-services-authentication-over-smb-for-azure-file-shares"></a>Visão geral-local Active Directory Domain Services autenticação sobre SMB para compartilhamentos de arquivos do Azure

[Arquivos](storage-files-introduction.md)   do Azure oferece suporte à autenticação baseada em identidade sobre o protocolo SMB por meio de dois tipos de serviços de domínio: Active Directory Domain Services local (AD DS) e Azure Active Directory Domain Services (Azure AD DS). É altamente recomendável que você examine a [seção como funciona](https://docs.microsoft.com/azure/storage/files/storage-files-active-directory-overview#how-it-works) para selecionar o serviço de domínio certo para autenticação. A configuração é diferente dependendo do serviço de domínio que você escolher. Estas séries de artigos se concentram em habilitar e configurar AD DS locais para autenticação com compartilhamentos de arquivos do Azure.

Se você for novo nos compartilhamentos de arquivos do Azure, é recomendável ler nosso [Guia de planejamento](storage-files-planning.md) antes de ler a série de artigos a seguir.

## <a name="supported-scenarios-and-restrictions"></a>Cenários e restrições com suporte

- AD DS identidades usadas para o arquivos do Azure no local, a autenticação do AD DS deve ser sincronizada com o Azure AD. A sincronização de hash de senha é opcional. 
- Dá suporte a compartilhamentos de arquivos do Azure gerenciados pelo Sincronização de Arquivos do Azure.
- Dá suporte à autenticação Kerberos com o AD com a criptografia RC4-HMAC e [AES 256](https://docs.microsoft.com/azure/storage/files/storage-troubleshoot-windows-file-connection-problems#azure-files-on-premises-ad-ds-authentication-support-for-aes-256-kerberos-encryption). A criptografia Kerberos AES 128 ainda não tem suporte.
- Dá suporte à experiência de logon único.
- Somente com suporte em clientes em execução em versões do sistema operacional mais recentes do que o Windows 7 ou o Windows Server 2008 R2.
- Somente com suporte na floresta do AD na qual a conta de armazenamento está registrada. Você só pode acessar compartilhamentos de arquivos do Azure com as credenciais de AD DS de uma única floresta por padrão. Se você precisar acessar o compartilhamento de arquivos do Azure de uma floresta diferente, verifique se você tem a relação de confiança de floresta apropriada configurada, consulte as [perguntas frequentes](storage-files-faq.md#ad-ds--azure-ad-ds-authentication) para obter detalhes.
- O não oferece suporte à autenticação em relação a contas de computador criadas no AD DS.
- O não oferece suporte à autenticação em compartilhamentos de arquivos NFS (Network File System).

Quando você habilita AD DS para compartilhamentos de arquivos do Azure via SMB, seus computadores ingressados AD DS podem montar compartilhamentos de arquivos do Azure usando suas credenciais de AD DS existentes. Esse recurso pode ser habilitado com um ambiente de AD DS hospedado em computadores locais ou hospedado no Azure.

> [!NOTE]
> Para ajudá-lo a configurar a autenticação do AD dos arquivos do Azure para alguns casos de uso comuns, publicamos dois vídeos com orientações passo a passo para os seguintes cenários:
> - [Substituindo servidores de arquivos locais por arquivos do Azure (incluindo a instalação no link privado para arquivos e autenticação do AD)](https://sec.ch9.ms/ch9/3358/0addac01-3606-4e30-ad7b-f195f3ab3358/ITOpsTalkAzureFiles_high.mp4)
> - [Usando os arquivos do Azure como o contêiner de perfil para a área de trabalho virtual do Windows (incluindo a configuração na autenticação do AD e FsLogix)](https://www.youtube.com/embed/9S5A1IJqfOQ)

## <a name="prerequisites"></a>Pré-requisitos 

Antes de habilitar a autenticação AD DS para compartilhamentos de arquivos do Azure, verifique se você concluiu os seguintes pré-requisitos: 

- Selecione ou crie seu [ambiente de AD DS](https://docs.microsoft.com/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview) e sincronize-o com o [Azure AD](../../active-directory/hybrid/how-to-connect-install-roadmap.md) com o Azure ad Connect. 

    Você pode habilitar o recurso em um ambiente de AD DS local novo ou existente. As identidades usadas para o acesso devem ser sincronizadas com o Azure AD. O locatário do Azure AD e o compartilhamento de arquivos que você está acessando devem ser associados à mesma assinatura.

- Domínio-ingresse em um computador local ou em uma VM do Azure para AD DS local. Para obter informações sobre como ingressar no domínio, consulte [ingressar um computador em um domínio](https://docs.microsoft.com/windows-server/identity/ad-fs/deployment/join-a-computer-to-a-domain).

    Se seu computador não estiver ingressado no domínio de um AD DS, você ainda poderá aproveitar as credenciais do AD para autenticação se o seu computador tiver uma linha de visão do controlador de domínio do AD.

- Selecione ou crie uma conta de armazenamento do Azure.  Para obter um desempenho ideal, recomendamos que você implante a conta de armazenamento na mesma região que o cliente do qual você planeja acessar o compartilhamento. Em seguida, [monte o compartilhamento de arquivos do Azure](storage-how-to-use-files-windows.md) com sua chave de conta de armazenamento. A montagem com a chave da conta de armazenamento verifica a conectividade.

    Certifique-se de que a conta de armazenamento que contém seus compartilhamentos de arquivos ainda não esteja configurada para autenticação de AD DS do Azure. Se a autenticação de AD DS do Azure for habilitada na conta de armazenamento, ela precisará ser desabilitada antes da alteração para usar o AD DS local. Isso implica que as ACLs existentes configuradas no ambiente de AD DS do Azure precisarão ser reconfiguradas para a imposição de permissão adequada.

    Se você tiver problemas ao conectar-se aos arquivos do Azure, consulte [a ferramenta de solução de problemas que publicamos para erros de montagem de arquivos do Azure no Windows](https://gallery.technet.microsoft.com/Troubleshooting-tool-for-a9fa1fe5). Também fornecemos [orientações](https://docs.microsoft.com/azure/storage/files/storage-files-faq#on-premises-access) para contornar cenários quando a porta 445 é bloqueada. 

- Faça qualquer configuração de rede relevante antes de habilitar e configurar AD DS autenticação para seus compartilhamentos de arquivos do Azure. Consulte [considerações de rede de arquivos do Azure](storage-files-networking-overview.md) para obter mais informações.

## <a name="regional-availability"></a>Disponibilidade regional

A autenticação de arquivos do Azure com o AD DS está disponível em [todas as regiões do Azure Public e gov](https://azure.microsoft.com/global-infrastructure/locations/).

## <a name="overview"></a>Visão geral

Se você planeja habilitar qualquer configuração de rede em seu compartilhamento de arquivos, recomendamos que leia o artigo [considerações de rede](https://docs.microsoft.com/azure/storage/files/storage-files-networking-overview) e conclua a configuração relacionada antes de habilitar a autenticação de AD DS.

Habilitar a autenticação de AD DS para seus compartilhamentos de arquivos do Azure permite que você se autentique em seus compartilhamentos de arquivos do Azure com suas credenciais de AD DS local. Além disso, ele permite que você gerencie melhor suas permissões para permitir o controle de acesso granular. Isso requer a sincronização de identidades do AD DS local para o Azure AD com o AD Connect. Você controla o acesso de nível de compartilhamento com identidades sincronizadas ao Azure AD enquanto gerencia o acesso de nível de arquivo/compartilhamento com credenciais de AD DS local.

Em seguida, siga as etapas abaixo para configurar os arquivos do Azure para autenticação AD DS: 

1. [Parte um: habilitar a autenticação de AD DS em sua conta de armazenamento](storage-files-identity-ad-ds-enable.md)

1. [Parte dois: atribuir permissões de acesso para um compartilhamento para a identidade do Azure AD (um usuário, grupo ou entidade de serviço) que está em sincronia com a identidade do AD de destino](storage-files-identity-ad-ds-assign-permissions.md)

1. [Parte três: configurar ACLs do Windows sobre SMB para diretórios e arquivos](storage-files-identity-ad-ds-configure-permissions.md)
 
1. [Parte quatro: montar um compartilhamento de arquivos do Azure para uma VM ingressada em seu AD DS](storage-files-identity-ad-ds-mount-file-share.md)

1. [Atualize a senha da sua identidade de conta de armazenamento no AD DS](storage-files-identity-ad-ds-update-password.md)

O diagrama a seguir ilustra o fluxo de trabalho de ponta a ponta para habilitar a autenticação do Azure AD sobre SMB para compartilhamentos de arquivos do Azure. 

![Diagrama de fluxo de trabalho do AD de arquivos](media/storage-files-active-directory-domain-services-enable/diagram-files-ad.png)

As identidades usadas para acessar os compartilhamentos de arquivos do Azure devem ser sincronizadas com o Azure AD para impor permissões de arquivo de nível de compartilhamento por meio do modelo de [controle de acesso baseado em função (RBAC do Azure)](../../role-based-access-control/overview.md) . As [DACLs de estilo do Windows](https://docs.microsoft.com/previous-versions/technet-magazine/cc161041(v=msdn.10)?redirectedfrom=MSDN) em arquivos/diretórios transferidos de servidores de arquivos existentes serão preservadas e impostas. Isso oferece integração direta com o ambiente de AD DS empresarial. Ao substituir servidores de arquivos locais por compartilhamentos de arquivos do Azure, os usuários existentes podem acessar compartilhamentos de arquivos do Azure de seus clientes atuais com uma experiência de logon único, sem nenhuma alteração nas credenciais em uso.  

## <a name="next-steps"></a>Próximas etapas

Para habilitar a autenticação de AD DS local para o compartilhamento de arquivos do Azure, vá para o próximo artigo:

[Parte um: habilitar a autenticação de AD DS para sua conta](storage-files-identity-ad-ds-enable.md)