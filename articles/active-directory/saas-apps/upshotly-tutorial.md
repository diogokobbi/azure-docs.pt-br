---
title: 'Tutorial: Integração do SSO (logon único) do Azure Active Directory ao Upshotly | Microsoft Docs'
description: Saiba como configurar o logon único entre o Azure Active Directory e o Upshotly.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 1/7/2020
ms.author: jeedes
ms.openlocfilehash: 1dbcd2bd997872820e87c9d8c2e239c4324e9ce6
ms.sourcegitcommit: 023d10b4127f50f301995d44f2b4499cbcffb8fc
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88532771"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-upshotly"></a>Tutorial: Integração do SSO (logon único) do Azure Active Directory ao Upshotly

Neste tutorial, você aprenderá como integrar o Upshotly ao Azure AD (Azure Active Directory). Ao integrar o Upshotly ao Azure AD, você poderá:

* Controlar quem tem acesso ao Upshotly no Azure AD.
* Permitir que os usuários sejam conectados automaticamente ao Upshotly com as contas do Azure AD deles.
* Gerenciar suas contas em um local central: o portal do Azure.

Para saber mais sobre a integração de aplicativos SaaS ao Azure AD, confira [O que é o acesso de aplicativos e o logon único com o Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Pré-requisitos

Para começar, você precisará dos seguintes itens:

* Uma assinatura do Azure AD. Caso você não tenha uma assinatura, obtenha uma [conta gratuita](https://azure.microsoft.com/free/).
* Assinatura habilitada para SSO (logon único) do Upshotly.

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, você configurará e testará o SSO do Azure AD em um ambiente de teste.

* O Upshotly é compatível com SSO iniciado por **SP e IDP**

## <a name="adding-upshotly-from-the-gallery"></a>Adicionar o Upshotly da galeria

Para configurar a integração do Upshotly ao Azure AD, você precisa adicionar o Upshotly da galeria à sua lista de aplicativos SaaS gerenciados.

1. Entre no [portal do Azure](https://portal.azure.com) usando uma conta corporativa ou de estudante ou uma conta pessoal da Microsoft.
1. No painel de navegação esquerdo, escolha o serviço **Azure Active Directory**.
1. Navegue até **Aplicativos Empresariais** e, em seguida, escolha **Todos os Aplicativos**.
1. Para adicionar um novo aplicativo, escolha **Novo aplicativo**.
1. Na seção **Adicionar da galeria**, digite **Upshotly** na caixa de pesquisa.
1. Selecione **Upshotly** no painel de resultados e, em seguida, adicione o aplicativo. Aguarde alguns segundos enquanto o aplicativo é adicionado ao seu locatário.

## <a name="configure-and-test-azure-ad-single-sign-on-for-upshotly"></a>Configurar e testar o logon único do Azure AD para o Upshotly

Configure e teste o SSO do Azure AD com o Upshotly usando uma usuária de teste chamada **B.Fernandes**. Para que o SSO funcione, é necessário estabelecer uma relação de vínculo entre um usuário do Azure AD e o usuário relacionado do Upshotly.

Para configurar e testar o SSO do Azure AD com o Upshotly, conclua os seguintes blocos de construção:

1. **[Configurar o SSO do Azure AD](#configure-azure-ad-sso)** – para permitir que os usuários usem esse recurso.
    * **[Criar um usuário de teste do Azure AD](#create-an-azure-ad-test-user)** para testar o logon único do Azure AD com B.Fernandes.
    * **[Atribuir o usuário de teste do Azure AD](#assign-the-azure-ad-test-user)** – para permitir que B.Fernandes use o logon único do Azure AD.
1. **[Configurar o SSO do Upshotly](#configure-upshotly-sso)** – para configurar o logon único no lado do aplicativo.
    * **[Criar um usuário de teste do Upshotly](#create-upshotly-test-user)** – para ter um equivalente de B.Fernandes no Upshotly que esteja vinculado à representação de usuário do Azure AD.
1. **[Testar o SSO](#test-sso)** – para verificar se a configuração funciona.

## <a name="configure-azure-ad-sso"></a>Configurar o SSO do Azure AD

Siga estas etapas para habilitar o SSO do Azure AD no portal do Azure.

1. No [portal do Azure](https://portal.azure.com/), na página de integração de aplicativos do **Upshotly**, localize a seção **Gerenciar** e selecione **logon único**.
1. Na página **Selecionar um método de logon único**, escolha **SAML**.
1. Na página **Configurar o logon único com o SAML**, clique no ícone de edição/caneta da **Configuração Básica do SAML** para editar as configurações.

   ![Editar a Configuração Básica de SAML](common/edit-urls.png)

1. Na seção **Configuração Básica de SAML**, se desejar configurar o aplicativo no modo iniciado por **IDP**, o aplicativo será pré-configurado e as URLs necessárias já serão preenchidas previamente com o Azure. É necessário que o usuário salve a configuração, clicando no botão **Salvar**.

1. Clique em **Definir URLs adicionais** e execute o passo seguinte se quiser configurar a aplicação no modo **SP** iniciado:

    Na caixa de texto **URL de logon**, digite um URL usando o seguinte padrão: `https://app.upshotly.com/api/sso/login/<companyID>`

    > [!NOTE]
    > O valor da URL de logon não é real. Atualize esses valores com a URL de Logon real. Você obterá o valor **companyID**, que será explicado posteriormente no tutorial. Entre em contato com a [equipe de suporte ao cliente do Upshotly](mailto:support@upshotly.com) para obter consultas. Você também pode consultar os padrões exibidos na seção **Configuração Básica de SAML** no portal do Azure.

1. Na página **Configurar o logon único com o SAML**, na seção **Certificado de Autenticação SAML**, localize **XML de Metadados de Federação** e selecione **Baixar** para baixar o certificado e salvá-lo no computador.

    ![O link de download do Certificado](common/metadataxml.png)

1. Na seção **Configurar o Upshotly**, copie as URLs apropriadas de acordo com suas necessidades.

    ![Copiar URLs de configuração](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Criar um usuário de teste do Azure AD

Nesta seção, você criará um usuário de teste no portal do Azure chamado B.Fernandes.

1. No painel esquerdo do portal do Azure, escolha **Azure Active Directory**, **Usuários** e, em seguida, **Todos os usuários**.
1. Selecione **Novo usuário** na parte superior da tela.
1. Nas propriedades do **Usuário**, siga estas etapas:
   1. No campo **Nome**, insira `B.Simon`.  
   1. No campo **Nome de usuário**, insira username@companydomain.extension. Por exemplo, `B.Simon@contoso.com`.
   1. Marque a caixa de seleção **Mostrar senha** e, em seguida, anote o valor exibido na caixa **Senha**.
   1. Clique em **Criar**.

### <a name="assign-the-azure-ad-test-user"></a>Atribuir o usuário de teste do Azure AD

Nesta seção, você permitirá que B.Fernandes use o logon único do Azure permitindo acesso ao Upshotly.

1. No portal do Azure, selecione **Aplicativos empresariais** e, em seguida, selecione **Todos os aplicativos**.
1. Na lista de aplicativos, selecione **Upshotly**.
1. Na página de visão geral do aplicativo, localize a seção **Gerenciar** e escolha **Usuários e grupos**.

   ![O link “Usuários e grupos”](common/users-groups-blade.png)

1. Escolha **Adicionar usuário** e, em seguida, **Usuários e grupos** na caixa de diálogo **Adicionar Atribuição**.

    ![O link Adicionar Usuário](common/add-assign-user.png)

1. Na caixa de diálogo **Usuários e grupos**, selecione **B.Fernandes** na lista Usuários e clique no botão **Selecionar** na parte inferior da tela.
1. Se você estiver esperando um valor de função na declaração SAML, na caixa de diálogo **Selecionar Função**, escolha a função apropriada para o usuário da lista e, em seguida, clique no botão **Escolher** na parte inferior da tela.
1. Na caixa de diálogo **Adicionar atribuição**, clique no botão **Atribuir**.

## <a name="configure-upshotly-sso"></a>Configurar o SSO do Upshotly

1. Em uma janela diferente do navegador da Web, entre no site da empresa Upshotly como administrador.

1. Clique no **Perfil do Usuário** e navegue até **Administrador > SSO** e siga estas etapas:

    ![Configuração do Upshotly](./media/upshotly-tutorial/config1.png)

    a. Copie o valor **ID da Empresa** e use esse valor **ID da Empresa** para substituir o valor **ID da Empresa** presente na **URL de Logon** na seção **Configuração Básica de SAML** no portal do Azure.

    b. Abra o **XML de metadados de federação** baixado do portal do Azure no Bloco de Notas, copie o conteúdo do XML de metadados e cole-o na caixa de texto **Metadados XML**.

### <a name="create-upshotly-test-user"></a>Criar usuário de teste do Upshotly

Nesta seção, você criará uma usuária chamada B.Fernandes no Upshotly Edge Cloud. Trabalhe com a [equipe de suporte ao cliente do Upshotly](mailto:support@upshotly.com) para adicionar os usuários à plataforma do Upshotly Edge Cloud. Os usuários devem ser criados e ativados antes de usar o logon único.

## <a name="test-sso"></a>Testar o SSO 

Nesta seção, você testará sua configuração de logon único do Azure AD usando o Painel de Acesso.

Ao clicar no bloco do Upshotly no Painel de Acesso, você deverá ser conectado automaticamente ao Upshotly para o qual você configurou o SSO. Para saber mais sobre o Painel de Acesso, veja [Introdução ao Painel de Acesso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionais

- [ Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [O que é o acesso a aplicativos e logon único com o Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [O que é o acesso condicional no Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Experimente o Upshotly com o Azure AD](https://aad.portal.azure.com/)