---
title: Nova experiência de registro de aplicativo do portal do Azure
titleSuffix: Microsoft identity platform
description: Uma introdução à nova experiência de registro de aplicativo no portal do Azure
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: how-to
ms.date: 11/8/2019
ms.author: marsma
ms.reviewer: lenalepa, alamaral
ms.custom: aaddev
ms.openlocfilehash: 13242a41e1d10b0df031bf10fd646d9ec3cf47c3
ms.sourcegitcommit: bf1340bb706cf31bb002128e272b8322f37d53dd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/03/2020
ms.locfileid: "89437761"
---
# <a name="the-new-azure-portal-app-registration-experience"></a>A nova experiência de registro de aplicativo portal do Azure

Há muitas melhorias na nova experiência de [registros de aplicativo](https://go.microsoft.com/fwlink/?linkid=2083908) no portal do Azure. Se você estiver mais familiarizado com o apps.dev.microsoft.com (portal de registro de aplicativos) para registrar ou gerenciar aplicativos, mencionado aqui como a "experiência antiga", este guia ajudará você a começar a usar a nova experiência.

## <a name="whats-not-changing"></a>O que não está mudando?

- Seus aplicativos e configurações relacionadas podem ser encontrados no estado em que se encontram na nova experiência. Você não precisa registrar os aplicativos novamente e os usuários de seus aplicativos não precisarão entrar novamente.

    > [!NOTE]
    > Você deve entrar com a conta usada para registrar aplicativos para encontrá-los no portal do Azure. Recomendamos que você verifique o usuário conectado na portal do Azure corresponde ao usuário que entrou no portal de registro de aplicativo, comparando o endereço de email do seu perfil.
    >
    > Em alguns casos, especialmente quando você entra usando contas pessoais da Microsoft (por exemplo, Outlook, Live, Xbox etc.) com um endereço de email do Azure AD, descobrimos que quando você vai para o portal do Azure da experiência antiga, ele entra em uma conta diferente com o mesmo email em seu locatário do Azure AD. Se você ainda acredita que seus aplicativos estão ausentes, saia e entre com a conta certa.

- Os aplicativos do Live SDK criados com contas pessoais da Microsoft ainda não têm suporte no portal do Azure e continuarão a permanecer na experiência antiga em um futuro próximo.

## <a name="key-changes"></a>Principais alterações

-   Na experiência antiga, os aplicativos eram, por padrão, registrados como aplicativos *convergidos* – aplicativos que dão suporte a todas as contas organizacionais (multilocatário), bem como contas pessoais da Microsoft. Isso não pode ser modificado com a experiência antiga, dificultando a criação de aplicativos que têm suporte apenas para contas organizacionais (multilocatário ou locatário único).
    A nova experiência permite que você registre aplicativos que dão suporte a todas essas opções. [Saiba mais sobre os tipos de aplicativos](active-directory-v2-registration-portal.md).

-   Na nova experiência, se sua conta Microsoft pessoal também estiver em um locatário do Azure AD, você verá três guias – todos os aplicativos no locatário, aplicativos de propriedade no locatário, bem como aplicativos de sua conta pessoal. Portanto, se você acreditar que os aplicativos registrados em seus conta Microsoft pessoais estão ausentes, verifique os **aplicativos na guia conta pessoal** .

-   Na nova experiência, você pode alternar facilmente entre locatários navegando até seu perfil e escolhendo alternar diretório.

## <a name="list-of-applications"></a>Lista de aplicativos

-   A nova lista de aplicativo mostra os aplicativos que foram registrados por meio da experiência de registros do aplicativo herdado no portal do Azure (aplicativos que entram somente em contas do Azure AD), bem como aplicativos registrados no [portal de registro do aplicativo](https://apps.dev.microsoft.com/) (aplicativos que se conectam às contas do Azure AD e pessoal da Microsoft).

-   A nova lista de aplicativos tem duas colunas adicionais: **criado em** coluna e **certificados & coluna segredos** que mostra o status (atual, expirando em breve ou expirado) de credenciais que foram registradas no aplicativo.

## <a name="new-app-registration"></a>Registro de novo aplicativo

Na experiência antiga, para registrar um aplicativo, você só precisa fornecer um nome. Os aplicativos que foram criados foram registrados como aplicativos *convergidos* – aplicativos que dão suporte a todos os diretórios organizacionais (multilocatário), bem como contas pessoais da Microsoft.  Isso não pode ser modificado com a experiência antiga, dificultando a criação de aplicativos que têm suporte apenas para contas organizacionais (de um ou de vários locatários). [Saiba mais sobre os tipos de conta com suporte](v2-supported-account-types.md)

Na nova experiência, você deve fornecer um nome para o aplicativo e escolher os tipos de conta com suporte. Opcionalmente, você pode fornecer um URI de redirecionamento.
Se você fornecer um URI de redirecionamento, será necessário especificar se ele é Web/público (nativo/móvel e área de trabalho). Para obter mais informações sobre como registrar um aplicativo usando a nova experiência de registros de aplicativo, consulte este guia de [início rápido](quickstart-register-app.md).

## <a name="app-management-page"></a>Página de gerenciamento de aplicativo

A antiga experiência tinha uma única página de gerenciamento de aplicativo para aplicativos com as seguintes seções: Propriedades, segredos do aplicativo, plataformas, proprietários, Microsoft Graph permissões, perfil e opções avançadas.

A nova experiência no portal do Azure apresenta esses recursos em páginas separadas. Aqui está onde você pode encontrar a funcionalidade equivalente:

- Propriedades-o nome e a ID do aplicativo estão na página Visão geral.
- Os segredos do aplicativo estão na página certificados & segredos
- A configuração de plataformas está na página de autenticação
- Microsoft Graph permissões está na página permissões de API junto com outras permissões
- O perfil está na página de identidade visual
- Opção avançada – o suporte ao Live SDK está na página de autenticação.

## <a name="application-secretscertificates--secrets"></a>Segredos/certificados do aplicativo & segredos

Na nova experiência, os **segredos do aplicativo** foram renomeados para **certificados & segredos**. Além disso, **as chaves públicas** são conhecidas como **certificados** e **as senhas** são chamadas de **segredos do cliente**. Optamos por não trazer essa funcionalidade na nova experiência por motivos de segurança, portanto, você não pode mais gerar um novo par de chaves.

## <a name="platformsauthentication-reply-urlsredirect-uris"></a>Plataformas/autenticação: URLs de resposta/URIs de redirecionamento
Na antiga experiência, uma seção aplicativo tinha plataformas para Web, nativa e API Web para configurar URLs de redirecionamento, URL de logout e fluxo implícito.

Na nova experiência, as URLs de resposta podem ser encontradas em uma \' seção de autenticação do aplicativo s. Além disso, eles são chamados de URIs de redirecionamento e o formato para URIs de redirecionamento foi alterado. Eles devem ser associados a um tipo de aplicativo (cliente Web ou público-celular e desktop). [Saiba mais](quickstart-register-app.md#add-a-redirect-uri)

As APIs da Web são configuradas em expor uma página de API.

> [!NOTE]
> Experimente a nova experiência de configurações de Autenticação, na qual você pode definir configurações para seu aplicativo com base na plataforma ou no dispositivo que você deseja ter como destino. [Saiba mais](quickstart-register-app.md#configure-platform-settings)

## <a name="microsoft-graph-permissionsapi-permissions"></a>Permissões de Microsoft Graph/permissões de API

-   Ao selecionar uma API na experiência antiga, você pode escolher apenas Microsoft Graph APIs. Na nova experiência, você pode escolher entre muitas APIs da Microsoft, incluindo Microsoft Graph, APIs da sua organização e suas APIs, isso é apresentado em três guias: APIs da Microsoft, APIs que minha organização usa ou minhas APIs. A barra de pesquisa em APIs minha organização usa pesquisa de guias por meio de entidades de serviço no locatário.

    > [!NOTE]
    > Você não verá essa guia se seu aplicativo não estiver associado a um locatário. Para obter mais informações sobre como solicitar permissões usando a nova experiência, consulte [este guia de início rápido](https://github.com/MicrosoftDocs/azure-docs/blob/master/articles/active-directory/develop/quickstart-configure-app-access-web-apis.md).

-   A antiga experiência não tinha um botão **conceder permissões** . Na nova experiência, há uma seção conceder consentimento com um botão **conceder consentimento do administrador** na seção permissões de API de um aplicativo. Somente um administrador pode conceder consentimento e esse botão é habilitado somente para administradores. Quando um administrador seleciona o botão **conceder consentimento do administrador** , o consentimento do administrador é concedido a todas as permissões solicitadas.

## <a name="profile"></a>Perfil
Na antiga experiência, o perfil tinha o logotipo, a URL da Home Page, a URL dos termos de serviço e a configuração da URL da política de privacidade. Na nova experiência, eles podem ser encontrados na página de identidade visual.

## <a name="application-manifest"></a>Manifesto do aplicativo
Na nova experiência, a página de manifesto permite que você edite e atualize os atributos do aplicativo. Para obter mais informações, veja [Manifesto do aplicativo](reference-app-manifest.md).

## <a name="new-ui"></a>Nova interface do usuário
Há uma nova interface do usuário para propriedades que, anteriormente, só poderia ser definida usando o editor de manifesto ou a API, ou não existia.

-   O oauth2AllowImplicitFlow (fluxo de concessão implícita) pode ser encontrado na página de autenticação. Ao contrário da experiência antiga, você pode habilitar tokens de acesso ou tokens de ID, ou ambos.

-   Os escopos definidos por essa API (oauth2Permissions) e preAuthorizedApplications (aplicativos cliente autorizados) podem ser configurados por meio da página expor uma API. Para obter mais informações sobre como configurar um aplicativo para ser uma API da Web e expor permissões/escopos, consulte este guia de [início rápido](quickstart-configure-app-expose-web-apis.md).

-   O domínio do Publicador (que é exibido para os usuários no [ \' prompt de consentimento do aplicativo s](application-consent-experience.md)) pode ser encontrado na página de identidade visual. Para obter mais informações sobre como configurar um domínio do Publicador, consulte [este "como](howto-configure-publisher-domain.md)".

## <a name="limitations"></a>Limitações

A nova experiência tem as seguintes limitações:

-   A nova experiência ainda não oferece suporte a Registros de aplicativo para locatários Azure AD B2C.

-   A nova experiência ainda não dá suporte a aplicativos de SDK ao vivo criados com contas pessoais da Microsoft.

-   Não há suporte para a alteração do valor de contas com suporte na interface do usuário. Você precisa usar o manifesto do aplicativo, a menos que você \' alterne novamente entre o locatário único do Azure AD e o multilocatário.

   > [!NOTE]
   > Se você for um usuário de conta Microsoft pessoal no locatário do Azure AD e o administrador do locatário tiver restringido o acesso ao portal do Azure, você poderá obter um acesso negado. No entanto, se você vir o atalho digitando Registros de aplicativo na barra de pesquisa ou fixando-o, você poderá acessar a nova experiência.

## <a name="next-steps"></a>Próximas etapas

Para começar a usar a nova experiência de registro de aplicativo, consulte [início rápido: registrar um aplicativo com a plataforma de identidade da Microsoft](quickstart-register-app.md).
