---
title: Configurar a autenticação do Azure AD
description: Saiba como configurar a autenticação do Azure Active Directory como um provedor de identidade para seu Serviço de Aplicativo ou aplicativo Azure Functions.
ms.assetid: 6ec6a46c-bce4-47aa-b8a3-e133baef22eb
ms.topic: article
ms.date: 04/14/2020
ms.custom: seodec18, fasttrack-edit, has-adal-ref
ms.openlocfilehash: 4022ca97f88e2f06d3b4c1eeb103bb60d1856eda
ms.sourcegitcommit: 648c8d250106a5fca9076a46581f3105c23d7265
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "88962138"
---
# <a name="configure-your-app-service-or-azure-functions-app-to-use-azure-ad-login"></a>Configurar seu aplicativo do Serviço de Aplicativo ou do Azure Functions aplicativo para usar o logon do Azure AD

[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Este artigo mostra como configurar o Serviço de Aplicativo do Azure ou o Azure Functions para usar o Azure Active Directory (Azure AD) como um provedor de autenticação.

> [!NOTE]
> O fluxo de configurações expressas configura um registro de aplicativo do AAD V1. Se você quiser usar o [Azure Active Directory v 2.0](../active-directory/develop/v2-overview.md) (incluindo [MSAL](../active-directory/develop/msal-overview.md)), siga as [instruções de configuração avançada](#advanced).

Siga estas práticas recomendadas ao configurar seu aplicativo e a autenticação:

- Dê a cada aplicativo do Serviço de Aplicativo suas próprias permissões e consentimento.
- Configure cada aplicativo do Serviço de Aplicativo com seu próprio registro.
- Evite o compartilhamento de permissão entre ambientes usando registros de aplicativo separados para slots de implantação separados. Essa prática pode ajudar a evitar problemas que afetam o aplicativo de produção durante o teste do novo código.

> [!NOTE]
> No momento, este recurso não está disponível no plano de Consumo em Linux para Azure Functions

## <a name="configure-with-express-settings"></a><a name="express"> </a>Configurar com configurações expressas

> [!NOTE]
> A opção **Expressa** não está disponível para nuvens governamentais.

1. No [Azure portal], pesquise e selecione **Serviços de Aplicativos** e, em seguida, selecione o nome do seu aplicativo.
2. No painel de navegação à esquerda, selecione **Autenticação / Autorização** > **Ativada**.
3. Selecione **Azure Active Directory** > **Expresso**.

   Se você quiser escolher um registro de aplicativo existente, em vez disso:

   1. Escolha **Selecionar aplicativo do AD existente** e, em seguida, clique em **Aplicativo do Azure AD**.
   2. Escolha um registro de aplicativo existente e clique em **OK**.

3. Selecione **OK** para registrar o aplicativo do Serviço de Aplicativo no Azure Active Directory. Um novo registro de aplicativo é criado.

    ![Configurações expressas no Azure Active Directory](./media/configure-authentication-provider-aad/express-settings.png)

4. (Opcional) Por padrão, o Serviço de Aplicativo fornece autenticação, mas não restringe o acesso autorizado ao conteúdo do site e às APIs. Você deve autorizar os usuários no código do aplicativo. Para restringir o acesso somente para usuários autenticados pelo Azure Active Directory, defina a **Ação a ser executada quando a solicitação não for autenticada** como **Logon com o Azure Active Directory**. Quando você define essa funcionalidade, seu aplicativo requer que todas as solicitações sejam autenticadas. Ele também redireciona todas as solicitações não autenticadas para o Azure Active Directory para autenticação.

    > [!CAUTION]
    > Restringir o acesso dessa maneira se aplica a todas as chamadas para seu aplicativo, o que pode não ser desejável para aplicativos que têm uma página inicial publicamente disponível, como em muitos aplicativos de página única. Para tais aplicativos, **Permitir solicitações anônimas (nenhuma ação)** pode ser preferível, com o aplicativo iniciando o logon manualmente. Para obter mais informações, consulte [Fluxo de autenticação](overview-authentication-authorization.md#authentication-flow).
5. Clique em **Salvar**.

## <a name="configure-with-advanced-settings"></a><a name="advanced"> </a>Configurar com configurações avançadas

Você pode definir as configurações do aplicativo manualmente se quiser usar um registro de aplicativo de um locatário do Azure AD diferente. Para concluir esta configuração personalizada:

1. Criar um registro no Azure AD.
2. Forneça alguns dos detalhes de registro para o Serviço de Aplicativo.

### <a name="create-an-app-registration-in-azure-ad-for-your-app-service-app"></a><a name="register"> </a>Criar um registro de aplicativo no Azure AD para seu aplicativo do Serviço de Aplicativo

Você precisará das seguintes informações quando configurar seu aplicativo do Serviço de Aplicativo:

- ID do Cliente
- ID do locatário
- Segredo do cliente (opcional)
- URI da ID de Aplicativo

Execute as seguintes etapas:

1. Entre no [Azure portal], pesquise e selecione **Serviços de Aplicativos** e, em seguida, selecione o nome do seu aplicativo. Anote a **URL** do seu aplicativo. Você a usará para configurar o registro do aplicativo do Azure Active Directory.
1. Selecione **Azure Active Directory** > **Registros do aplicativo** > **Novo registro**.
1. Na página **Registrar um aplicativo**, insira um **Nome** para o registro do seu aplicativo.
1. Em **URI de redirecionamento**, selecione **Web** e digite `<app-url>/.auth/login/aad/callback`. Por exemplo, `https://contoso.azurewebsites.net/.auth/login/aad/callback`.
1. Selecione **Criar**.
1. Depois que o registro do aplicativo for criado, copie a **ID do aplicativo (cliente)** e a **ID do Directory (locatário)** para mais tarde.
1. Selecione **Autenticação**. Em **Concessão implícita**, habilite **Tokens de ID** para permitir as entradas de usuário do OpenID Connect a partir do Serviço de Aplicativo.
1. (Opcional) Selecione **Identidade visual**. Em **URL da página inicial**, insira a URL do seu aplicativo do Serviço de Aplicativo e selecione **Salvar**.
1. Selecione **Expor uma API** > **Configurar**. Para um aplicativo de locatário único, cole a URL do seu aplicativo do Serviço de Aplicativo e selecione **Salvar** e para um aplicativo multilocatário, cole a URL que se baseia em um dos domínios verificados do locatário e, em seguida, selecione **Salvar**.

   > [!NOTE]
   > Esse valor é o **URI da ID de aplicativo** do registro do aplicativo. Se o seu aplicativo Web exigir acesso a uma API na nuvem, você precisará do **URI da ID de aplicativo** do aplicativo Web quando configurar o recurso de Serviço de Aplicativo de nuvem. Você pode usar isso, por exemplo, se desejar que o serviço de nuvem conceda explicitamente acesso ao aplicativo Web.

1. Selecione **Adicionar um escopo**.
   1. Em **Nome do escopo**, digite *user_impersonation*.
   1. Nas caixas de texto, insira o nome do escopo de consentimento e a descrição que você deseja que os usuários vejam na página de consentimento. Por exemplo, digite *Acessar meu aplicativo*.
   1. Selecione **Adicionar escopo**.
1. (Opcional) Para criar um segredo do cliente, selecione **Certificados e segredos** > **Novo segredo do cliente** > **Adicionar**. Copie o valor do segredo do cliente mostrado na página. Não será mostrado novamente.
1. (Opcional) Para adicionar várias **URLs de resposta**, selecione **Autenticação**.

### <a name="enable-azure-active-directory-in-your-app-service-app"></a><a name="secrets"> </a>Habilitar o Azure Active Directory no aplicativo do Serviço de Aplicativo

1. No [Azure portal], pesquise e selecione **Serviços de Aplicativos** e, em seguida, selecione o nome do seu aplicativo.
1. No painel à esquerda, em **Configurações**, selecione **Autenticação / Autorização** > **Ativada**.
1. (Opcional) Por padrão, a autenticação do Serviço de Aplicativo permite o acesso não autenticado ao seu aplicativo. Para impor a autenticação do usuário, configure **Ação a ser tomada quando a solicitação não for autenticada** para **Logon com o Azure Active Directory**.
1. Em **Provedores de Autenticação**, selecione **Azure Active Directory**.
1. No **Modo de gerenciamento**, selecione **Avançado** e configure a autenticação do Serviço de Aplicativo de acordo com a tabela a seguir:

    |Campo|Descrição|
    |-|-|
    |ID do Cliente| Use a **ID do aplicativo (cliente)** do registro do aplicativo. |
    |URL do emissor| Use `<authentication-endpoint>/<tenant-id>/v2.0` e substitua *\<authentication-endpoint>* pelo ponto de [extremidade de autenticação para seu ambiente de nuvem](../active-directory/develop/authentication-national-cloud.md#azure-ad-authentication-endpoints) (por exemplo, " https://login.microsoft.com " para o Azure global), também substituindo *\<tenant-id>* pela ID do **diretório (locatário)** na qual o registro do aplicativo foi criado. Esse valor é usado para redirecionar os usuários para o locatário correto do Azure AD, bem como para baixar os metadados apropriados para determinar as chaves de assinatura de token apropriadas e o valor de declaração do emissor do token, por exemplo. Para aplicativos que usam o Azure AD v1 e para aplicativos Azure Functions, omita `/v2.0` na URL.|
    |Segredo do cliente (opcional)| Use o segredo do cliente gerado no registro do aplicativo.|
    |Audiências de token permitidas| Se este for um aplicativo de nuvem ou de servidor e você quiser permitir tokens de autenticação de um aplicativo Web, adicione o **URI da ID do aplicativo** do aplicativo Web aqui. A **ID do cliente** configurada é *sempre* implicitamente considerada uma audiência permitida. |

2. Selecione **OK** e, em seguida, **Salvar**.

Agora você está pronto para usar o Azure Active Directory para autenticação no aplicativo do Serviço de Aplicativo.

## <a name="configure-a-native-client-application"></a>Configurar um aplicativo de cliente nativo

Você pode registrar clientes nativos para permitir a autenticação para a API da Web hospedada em seu aplicativo usando uma biblioteca de cliente, como a **Biblioteca de Autenticação do Active Directory**.

1. No [Azure portal], selecione **Active Directory** > **Registros de aplicativo** > **Novo registro**.
1. Na página **Registrar um aplicativo**, insira um **Nome** para o registro do seu aplicativo.
1. Em **URI de redirecionamento**, selecione **Cliente público (móvel e área de trabalho)** e digite a URL `<app-url>/.auth/login/aad/callback`. Por exemplo, `https://contoso.azurewebsites.net/.auth/login/aad/callback`.

    > [!NOTE]
    > Para um aplicativo da Microsoft Store, use o [SID de pacote](/previous-versions/azure/app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library#package-sid) como o URI.
1. Selecione **Criar**.
1. Depois que o registro do aplicativo for criado, copie o valor da **ID do aplicativo (cliente)** .
1. Selecione **Permissões de API** > **Adicionar uma permissão** > **Minhas APIs**.
1. Selecione o registro de aplicativo que você criou anteriormente para seu aplicativo do Serviço de Aplicativo. Se você não vir o registro do aplicativo, certifique-se de ter adicionado o escopo **user_impersonation** em [Criar um registro de aplicativo no Azure AD para o aplicativo do Serviço de Aplicativo](#register).
1. Em **Permissões delegadas**, selecione **user_impersonation** e, em seguida, **Adicionar permissões**.

Você já configurou um aplicativo de cliente nativo que pode acessar seu aplicativo do Serviço de Aplicativo em nome de um usuário.

## <a name="configure-a-daemon-client-application-for-service-to-service-calls"></a>Configurar um aplicativo cliente daemon para chamadas de serviço a serviço

Seu aplicativo pode adquirir um token para chamar uma API Web hospedada no Serviço de Aplicativo ou no aplicativo de Funções em seu nome (não em nome de um usuário). Esse cenário é útil para aplicativos daemon não interativos que executam tarefas sem um usuário conectado. Ele usa a concessão das [credenciais de cliente](../active-directory/azuread-dev/v1-oauth2-client-creds-grant-flow.md) padrão do OAuth 2.0.

1. No [Azure portal], selecione **Active Directory** > **Registros de aplicativo** > **Novo registro**.
1. Na página **Registrar um aplicativo**, insira um **Nome** para o registro do aplicativo daemon.
1. Em um aplicativo daemon, você não precisa de um URI de redirecionamento para que possa mantê-lo vazio.
1. Selecione **Criar**.
1. Depois que o registro do aplicativo for criado, copie o valor da **ID do aplicativo (cliente)** .
1. Selecione **Certificados e segredos** > **Novo segredo do cliente** > **Adicionar**. Copie o valor do segredo do cliente mostrado na página. Não será mostrado novamente.

Agora você pode [solicitar um token de acesso usando a ID e o segredo do cliente](../active-directory/azuread-dev/v1-oauth2-client-creds-grant-flow.md#first-case-access-token-request-with-a-shared-secret), definindo o parâmetro `resource` como o **URI da ID do aplicativo** de destino. O token de acesso resultante pode ser apresentado ao aplicativo de destino usando o [cabeçalho de autorização OAuth 2.0](../active-directory/azuread-dev/v1-oauth2-client-creds-grant-flow.md#use-the-access-token-to-access-the-secured-resource) padrão, e a autenticação/autorização do Serviço de Aplicativo validará e usará o token como de costume para indicar que o chamador (um aplicativo neste caso, não um usuário) está autenticado.

No momento, isso permite que _qualquer_ aplicativo cliente no seu locatário do Azure AD solicite um token de acesso e faça a autenticação no aplicativo de destino. Se você também quiser impor _autorização_ para permitir apenas determinados aplicativos cliente, deverá executar algumas configurações adicionais.

1. [Defina uma função de aplicativo](../active-directory/develop/howto-add-app-roles-in-azure-ad-apps.md) no manifesto do registro do aplicativo que representa o Serviço de Aplicativo ou o aplicativo de Funções que você deseja proteger.
1. No registro do aplicativo representando o cliente que precisa ser autorizado, selecione **Permissões da API** > **Adicionar uma permissão** > **Minhas APIs**.
1. Selecione o registro de aplicativo que você criou anteriormente. Se você não vir o registro do aplicativo, verifique se [adicionou uma Função de Aplicativo](../active-directory/develop/howto-add-app-roles-in-azure-ad-apps.md).
1. Em **Permissões de aplicativo**, selecione a Função de Aplicativo que você criou anteriormente e, em seguida, selecione **Adicionar permissões**.
1. Clique em **Conceder consentimento de administrador** para autorizar o aplicativo cliente a solicitar a permissão.
1. Semelhante ao cenário anterior (antes de qualquer função ter sido adicionada), agora você pode [solicitar um token de acesso](../active-directory/azuread-dev/v1-oauth2-client-creds-grant-flow.md#first-case-access-token-request-with-a-shared-secret) para o mesmo `resource` de destino, e o token de acesso incluirá uma declaração de `roles` contendo as Funções de Aplicativo autorizadas para o aplicativo cliente.
1. No Serviço de Aplicativo de destino ou no código do aplicativo de Funções, agora você pode confirmar que as funções esperadas estão presentes no token (isso não é executado pela autenticação/autorização do Serviço de Aplicativo). Para mais informações, consulte [Acessar declarações de usuário](app-service-authentication-how-to.md#access-user-claims).

Você já configurou um aplicativo cliente daemon que pode acessar seu aplicativo do Serviço de Aplicativo usando uma identidade própria.

## <a name="next-steps"></a><a name="related-content"> </a>Próximas etapas

[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]
* [Tutorial: Autenticar e autorizar usuários de ponta a ponta no Serviço de Aplicativo do Azure](tutorial-auth-aad.md)
<!-- URLs. -->

[Azure portal]: https://portal.azure.com/