---
title: 'Tutorial: Integração do Azure Active Directory ao Schoox | Microsoft Docs'
description: Saiba como configurar o logon único entre o Azure Active Directory e o Schoox.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 02/14/2019
ms.author: jeedes
ms.openlocfilehash: 5dc88caae33cc085b1f66b37d90be76873f779f1
ms.sourcegitcommit: 023d10b4127f50f301995d44f2b4499cbcffb8fc
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88547826"
---
# <a name="tutorial-azure-active-directory-integration-with-schoox"></a>Tutorial: Integração do Azure Active Directory com o Schoox

Neste tutorial, você aprenderá a integrar o Schoox ao Azure AD (Azure Active Directory).
A integração do Schoox ao Azure AD oferece os seguintes benefícios:

* Você pode controlar no Azure AD quem terá acesso ao Schoox.
* Você pode permitir que os usuários sejam conectados automaticamente ao Schoox (logon único) com suas contas do Azure AD.
* Você pode gerenciar suas contas em um único local central – o portal do Azure.

Para conhecer mais detalhadamente a integração de aplicativos de SaaS ao AD do Azure, consulte [O que é o acesso a aplicativos e logon único com o Active Directory do Azure](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Se você não tiver uma assinatura do Azure, [crie uma conta gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="prerequisites"></a>Prerequisites

Para configurar a integração do Azure AD com o Schoox, você precisará dos seguintes itens:

* Uma assinatura do Azure AD. Se não tiver um ambiente do Azure AD, você pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/)
* Assinatura habilitada para logon único do Schoox

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, você configurará e testará o logon único do Azure AD em um ambiente de teste.

* O Schoox dá suporte ao SSO iniciado por **SP e IDP**

## <a name="adding-schoox-from-the-gallery"></a>Adicionando o Schoox da galeria

Para configurar a integração do Schoox ao Azure AD, você precisará adicionar o Schoox da galeria à sua lista de aplicativos SaaS gerenciados.

**Para adicionar o Schoox da galeria, execute as seguintes etapas:**

1. No **[Portal do Azure](https://portal.azure.com)** , no painel navegação à esquerda, clique no ícone **Azure Active Directory**.

    ![O botão Azure Active Directory](common/select-azuread.png)

2. Navegue até **Aplicativos Empresariais** e, em seguida, selecione a opção **Todos os Aplicativos**.

    ![A folha Aplicativos empresariais](common/enterprise-applications.png)

3. Clique no botão **Novo aplicativo** na parte superior da caixa de diálogo para adicionar o novo aplicativo.

    ![O botão Novo aplicativo](common/add-new-app.png)

4. Na caixa de pesquisa, digite **Schoox**, selecione **Schoox** no painel de resultados e, em seguida, clique no botão **Adicionar** para adicionar o aplicativo.

     ![Schoox na lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar logon único do Azure AD

Nesta seção, você configurará e testará o logon único do Azure AD com o Schoox, com base em um usuário de teste chamado **Brenda Fernandes**.
Para que o logon único funcione, é necessário estabelecer uma relação de vínculo entre um usuário do Azure AD e o usuário relacionado do Schoox.

Para configurar e testar o logon único do Azure AD com o Schoox, você precisa concluir os seguintes blocos de construção:

1. **[Configurar o logon único do Azure AD](#configure-azure-ad-single-sign-on)** – para habilitar seus usuários a usar esse recurso.
2. **[Configurar o logon único do Schoox](#configure-schoox-single-sign-on)** – para definir as configurações de logon único no lado do aplicativo.
3. **[Criar um usuário de teste do Azure AD](#create-an-azure-ad-test-user)** – para testar o logon único do Azure AD com Brenda Fernandes.
4. **[Atribuir o usuário de teste do Azure AD](#assign-the-azure-ad-test-user)** – para permitir que Brenda Fernandes use o logon único do Azure AD.
5. **[Criar um usuário de teste do Schoox](#create-schoox-test-user)** – para ter um equivalente de Brenda Fernandes no Schoox que esteja vinculado à representação de usuário do Azure AD.
6. **[Teste o logon único](#test-single-sign-on)** – para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o logon único do Azure AD

Nesta seção, você habilitará o logon único do Azure AD no portal do Azure.

Para configurar o logon único do Azure AD com o Schoox, execute as seguintes etapas:

1. No [portal do Azure](https://portal.azure.com/), na página de integração de aplicativos do **Schoox**, selecione **Logon único**.

    ![Link Configurar logon único](common/select-sso.png)

2. Na caixa de diálogo **Selecionar um método de logon único**, selecione o modo **SAML/WS-Fed** para habilitar o logon único.

    ![Modo de seleção de logon único](common/select-saml-option.png)

3. Na página **Configurar Logon Único com SAML**, clique no ícone **Editar** para abrir a caixa de diálogo **Configuração Básica do SAML**.

    ![Editar a Configuração Básica de SAML](common/edit-urls.png)

4. Na seção **Configuração Básica do SAML**, execute as etapas a seguir caso deseje configurar o aplicativo no modo iniciado por **IDP**:

    ![Informações de logon único de Domínio e URLs do Schoox](common/idp-identifier.png)

    Na caixa de texto **Identificador**, digite uma URL: `https://saml.schoox.com/saml/adfsmetadata`

5. Clique em **Definir URLs adicionais** e execute o passo seguinte se quiser configurar a aplicação no modo **SP** iniciado:

    ![image](common/both-preintegrated-signon.png)

    Na caixa de texto **URL de logon**, digite um URL usando o seguinte padrão: `https://saml.schoox.com/saml/login?idpUrl=<entityID>`

    > [!NOTE]
    > `<entityID>` é a ID de Entidade SAML copiada da seção de Referência Rápida, descrita posteriormente no tutorial.

6. Na página **Configurar logon único com SAML**, na seção **Certificado de autenticação SAML**, clique em **Baixar** para baixar o **XML de metadados de federação** usando as opções fornecidas de acordo com seus requisitos e salve-o no computador.

    ![O link de download do Certificado](common/metadataxml.png)

7. Na seção **Configurar o Schoox**, copie as URLs apropriadas de acordo com suas necessidades.

    ![Copiar URLs de configuração](common/copy-configuration-urls.png)

    a. URL de logon

    b. Identificador do Azure Ad

    c. URL de logoff

### <a name="configure-schoox-single-sign-on"></a>Configurar o logon único do Schoox

Para configurar o logon único no lado do **Schoox**, é necessário enviar o **XML de Metadados de Federação** baixado e as URLs apropriadas copiadas do portal do Azure para a [equipe de suporte do Schoox](https://www.schoox.com/help/). Eles definem essa configuração para ter a conexão de SSO de SAML definida corretamente em ambos os lados.

### <a name="create-an-azure-ad-test-user"></a>Criar um usuário de teste do Azure AD

O objetivo desta seção é criar um usuário de teste no Portal do Azure chamado Brenda Fernandes.

1. No Portal do Azure, no painel esquerdo, selecione **Azure Active Directory**, selecione **Usuários** e, em seguida, **Todos os usuários**.

    ![Os links “Usuários e grupos” e “Todos os usuários”](common/users.png)

2. Selecione **Novo usuário** na parte superior da tela.

    ![Botão Novo usuário](common/new-user.png)

3. Nas Propriedades do usuário, execute as etapas a seguir.

    ![A caixa de diálogo Usuário](common/user-properties.png)

    a. No campo **Nome**, insira **BrendaFernandes**.
  
    b. No campo **Nome de usuário**, digite **brendafernandes\@domíniodaempresa.extensão**  
    Por exemplo, BrittaSimon@contoso.com

    c. Marque a caixa de seleção **Mostrar senha** e, em seguida, anote o valor exibido na caixa Senha.

    d. Clique em **Criar**.

### <a name="assign-the-azure-ad-test-user"></a>Atribuir o usuário de teste do Azure AD

Nesta seção, você permitirá que Brenda Fernandes use o logon único do Azure concedendo-lhe acesso ao Schoox.

1. No portal do Azure, selecione **Aplicativos Empresariais**, **Todos os aplicativos** e, em seguida, **Schoox**.

    ![Folha de aplicativos empresariais](common/enterprise-applications.png)

2. Na lista de aplicativos, escolha **Schoox**.

    ![O link do Schoox na lista de Aplicativos](common/all-applications.png)

3. No menu à esquerda, selecione **Usuários e grupos**.

    ![O link “Usuários e grupos”](common/users-groups-blade.png)

4. Escolha o botão **Adicionar usuário** e, em seguida, escolha **Usuários e grupos** na caixa de diálogo **Adicionar Atribuição**.

    ![O painel Adicionar Atribuição](common/add-assign-user.png)

5. Na caixa de diálogo **Usuários e grupos**, selecione **Brenda Fernandes** na lista Usuários e, em seguida, clique no botão **Selecionar** na parte inferior da tela.

6. Se você estiver esperando um valor de função na declaração SAML, na caixa de diálogo **Selecionar Função**, escolha a função de usuário apropriada para o usuário na lista e, em seguida, clique no botão **Selecionar** na parte inferior da tela.

7. Na caixa de diálogo **Adicionar atribuição**, clique no botão **Atribuir**.

### <a name="create-schoox-test-user"></a>Criar um usuário de teste do Schoox

Nesta seção, você criará uma usuária chamada Brenda Fernandes no Schoox. Trabalhe com a [equipe de suporte do Schoox](https://www.schoox.com/help/) para adicionar os usuários à plataforma Schoox. Os usuários devem ser criados e ativados antes de usar o logon único.

### <a name="test-single-sign-on"></a>Testar logon único

Nesta seção, você testará sua configuração de logon único do Azure AD usando o Painel de Acesso.

Ao clicar no bloco do Schoox no Painel de Acesso, você deverá ser conectado automaticamente ao Schoox, para o qual você configurou o SSO. Para saber mais sobre o Painel de Acesso, veja [Introdução ao Painel de Acesso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionais

- [Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [O que é o acesso condicional no Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
