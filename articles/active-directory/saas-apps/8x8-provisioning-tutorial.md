---
title: 'Tutorial: Configurar o 8x8 para o provisionamento automático de usuários com o Azure Active Directory | Microsoft Docs'
description: Saiba como provisionar e descontinuar automaticamente as contas de usuário do Azure AD para o 8x8.
services: active-directory
author: zchia
writer: zchia
manager: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: article
ms.date: 05/15/2020
ms.author: Zhchia
ms.openlocfilehash: 850db53cbf88968a413d72527d76463e17aa8f5d
ms.sourcegitcommit: d95cab0514dd0956c13b9d64d98fdae2bc3569a0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91361697"
---
# <a name="tutorial-configure-8x8-for-automatic-user-provisioning"></a>Tutorial: Configurar o 8x8 para provisionamento automático de usuários

Este tutorial descreve as etapas que você precisa executar tanto no 8x8 Configuration Manager quanto no Azure AD (Azure Active Directory) para configurar o provisionamento automático de usuário. Quando configurado, o Azure AD provisiona e desprovisiona automaticamente usuários e grupos para o [8x8](https://www.8x8.com) usando o serviço de provisionamento do Azure AD. Para detalhes importantes sobre o que esse serviço faz, como funciona e as perguntas frequentes, consulte [Automatizar o provisionamento e desprovisionamento de usuários para aplicativos SaaS com o Azure Active Directory](../manage-apps/user-provisioning.md). 

## <a name="capabilities-supported"></a>Funcionalidades com suporte
> [!div class="checklist"]
> * Criar usuários no 8x8
> * Remover usuários no 8x8 quando eles não precisarem mais de acesso
> * Manter os atributos de usuário sincronizados entre o Azure AD e o 8x8
> * [Logon único](https://docs.microsoft.com/azure/active-directory/saas-apps/8x8virtualoffice-tutorial) para o 8x8 (recomendado)

## <a name="prerequisites"></a>Pré-requisitos

O cenário descrito neste tutorial pressupõe que você já tem os seguintes pré-requisitos:

* [Um locatário do Azure AD](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant) 
* Uma conta de usuário no Azure AD com [permissão](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) para configurar o provisionamento (por exemplo, Administrador de Aplicativo, Administrador de aplicativos de nuvem, Proprietário de Aplicativo ou Administrador global).
* Uma assinatura da série 8x8 X de qualquer nível.
* Uma conta de usuário do 8x8 com permissão de administrador no [Configuration Manager](https://vo-cm.8x8.com).
* O [Logon único com o Azure AD](https://docs.microsoft.com/azure/active-directory/saas-apps/8x8virtualoffice-tutorial) já foi configurado.

## <a name="step-1-plan-your-provisioning-deployment"></a>Etapa 1. Planeje a implantação do provisionamento
1. Saiba mais sobre [como funciona o serviço de provisionamento](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning).
2. Determine quem estará no [escopo de provisionamento](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts).
3. Determine quais dados serão [mapeados entre o Azure AD e o 8x8](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes).

## <a name="step-2-configure-8x8-to-support-provisioning-with-azure-ad"></a>Etapa 2. Configurar o 8x8 para dar suporte ao provisionamento com o Azure AD

Esta seção orienta você pelas etapas para configurar o 8x8 para dar suporte ao provisionamento com o Azure AD.

### <a name="to-configure-a-user-provisioning-access-token-in-8x8-configuration-manager"></a>Para configurar um token de acesso de provisionamento de usuário no 8x8 Configuration Manager:

1. Entre no [Configuration Manager](https://vo-cm.8x8.com). Selecione **Gerenciamento de Identidades**.

   ![Gerenciamento de Identidades](./media/8x8-provisioning-tutorial/8x8-identity-management.png)

2. Clique no link **Mostrar informações de provisionamento de usuário** para gerar um token.

   ![Mostrar provisionamento do usuário](./media/8x8-provisioning-tutorial/8x8-show-user-provisioning.png)

3. Copie a **URL do 8x8** e os valores do **Token da API do 8x8**. Esses valores serão inseridos nos campos **URL do Locatário** e **Token de Segredo**, respectivamente, na guia Provisionamento do aplicativo 8x8 no portal do Azure.

   ![Copiar URL e token](./media/8x8-provisioning-tutorial/8x8-copy-url-token.png)

## <a name="step-3-add-8x8-from-the-azure-ad-application-gallery"></a>Etapa 3. Adicionar o 8x8 da galeria de aplicativos do Azure AD

Adicione o 8x8 por meio da galeria de aplicativos do Azure AD para começar a gerenciar o provisionamento dele. Se você já tiver configurado o 8x8 para SSO, poderá usar o mesmo aplicativo. No entanto, recomendamos que você crie um aplicativo diferente ao testar a integração no início. Saiba mais sobre como adicionar um aplicativo da galeria [aqui](https://docs.microsoft.com/azure/active-directory/manage-apps/add-gallery-app).

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>Etapa 4. Defina quem estará no escopo de provisionamento

No Azure AD, é possível definir quem estará no escopo de provisionamento com base na atribuição ao aplicativo ou nos atributos do usuário/grupo. Se você optar por definir quem estará no escopo de provisionamento com base na atribuição, poderá usar as [etapas](../manage-apps/assign-user-or-group-access-portal.md) a seguir para atribuir usuários e grupos ao aplicativo. Essa é a opção mais simples e é usada pela maioria das pessoas.

Se você optar por definir quem estará no escopo de provisionamento com base somente em atributos do usuário ou do grupo, poderá usar um filtro de escopo, conforme descrito [aqui](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts). 

* Ao atribuir usuários e grupos ao 8x8, é preciso selecionar uma função diferente do **Acesso Padrão**. Os usuários com a função Acesso Padrão são excluídos do provisionamento e serão marcados como "Não qualificado efetivamente" nos logs de provisionamento. Se a única função disponível no aplicativo for a de acesso padrão, você poderá [atualizar o manifesto do aplicativo](https://docs.microsoft.com/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps) para adicionar outras funções. 

* Comece pequeno. Teste com um pequeno conjunto de usuários e grupos antes de implementar para todos. Quando o escopo de provisionamento é definido para usuários e grupos atribuídos, é possível controlar isso atribuindo um ou dois usuários ou grupos ao aplicativo. Quando o escopo é definido para todos os usuários e grupos, é possível especificar um [atributo com base no filtro de escopo](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts). 

## <a name="step-5-configure-automatic-user-provisioning-to-8x8"></a>Etapa 5. Configurar o provisionamento automático de usuário para o 8x8 

Nesta seção, você verá orientações para seguir as etapas de configuração do serviço de provisionamento do Azure AD para criar, atualizar e desabilitar usuários e/ou grupos no 8x8 com base em atribuições de usuário e/ou grupo no Azure AD.

### <a name="to-configure-automatic-user-provisioning-for-8x8-in-azure-ad"></a>Para configurar o provisionamento automático de usuário para o 8x8 no Azure AD:

1. Entre no [portal do Azure](https://portal.azure.com). Selecione **Aplicativos Empresariais** e **Todos os Aplicativos**.

    ![Folha de aplicativos empresariais](./media/8x8-provisioning-tutorial/enterprise-applications.png)

    ![Folha Todos os aplicativos](./media/8x8-provisioning-tutorial/all-applications.png)

2. Na lista de aplicativos, selecione **8x8**.

    ![O link do 8x8 na lista de Aplicativos](common/all-applications.png)

3. Selecione a guia **Provisionamento**. Clique em **Introdução**.

    ![Captura de tela das opções de gerenciamento com a opção de provisionamento chamada out.](common/provisioning.png)

   ![Folha de introdução](./media/8x8-provisioning-tutorial/get-started.png)

4. Defina o **Modo de Provisionamento** como **Automático**.

    ![Captura de tela da lista suspensa modo de provisionamento com a opção automática chamada out.](common/provisioning-automatic.png)

5. Na seção **Credenciais de Administrador**, copie a URL do **8x8** do Configuration Manager para a **URL do locatário**. Copie o **Token da API do 8x8** do Configuration Manager para **Token Secreto**. Clique em **Testar Conexão** para verificar se o Azure AD pode se conectar ao 8x8. Se a conexão falhar, verifique se sua conta 8x8 tem permissões de administrador e tente novamente.

    ![Captura de tela mostra a caixa de diálogo credenciais de administrador, em que é possível inserir seu locatário U R L e token secreto.](./media/8x8-provisioning-tutorial/provisioning.png)

6. No campo **Notificação por Email**, insira o endereço de email de uma pessoa ou grupo que deverá receber as notificações de erro de provisionamento e marque a caixa de seleção **Enviar uma notificação por email quando ocorrer uma falha**.

    ![Email de notificação](common/provisioning-notification-email.png)

7. Clique em **Salvar**.

8. Na seção **Mapeamentos**, selecione **Provisionar usuários do Azure Active Directory**.

9. Examine os atributos de usuário que serão sincronizados do Azure AD com o 8x8 na seção **Mapeamento de Atributos**. Os atributos selecionados como propriedades **Correspondentes** serão usados para fazer a correspondência entre as contas de usuário no 8x8 para operações de atualização. Se você optar por alterar o [atributo de destino correspondente](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes), precisará garantir que a API do 8x8 seja compatível com a filtragem de usuários com base nesse atributo. Selecione o botão **Salvar** para confirmar as alterações.

   |Atributo|Type|Observações|
   |---|---|---|
   |userName|String|Define o nome de usuário e a ID da Federação|
   |externalId|String||
   |ativo|Boolean||
   |título|String||
   |emails[type eq "work"].value|String||
   |name.givenName|String||
   |name.familyName|String||
   |phoneNumbers[type eq "mobile"].value|String|Número de contato pessoal|
   |phoneNumbers[type eq "work"].value|String|Número de contato pessoal|
   |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:department|String||
   |urn:ietf:params:scim:schemas:extension:8x8:1.1:User:site|String|Não pode ser atualizado após a criação do usuário|
   |localidade|String|Não é mapeado por padrão|
   |timezone|String|Não é mapeado por padrão|

10. Para configurar filtros de escopo, consulte as seguintes instruções fornecidas no [tutorial do Filtro de Escopo](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

11. Para habilitar o serviço de provisionamento do Azure AD para o 8x8, altere o **Status de Provisionamento** para **Ativado** na seção **Configurações**.

    ![Status do provisionamento ativado](common/provisioning-toggle-on.png)

12. Defina os usuários e/ou grupos que você gostaria de provisionar no 8x8 escolhendo os valores desejados em **Escopo** na seção **Configurações**.

    ![Escopo de provisionamento](common/provisioning-scope.png)

13. Quando estiver pronto para provisionar, clique em **Salvar**.

    ![Salvando a configuração de provisionamento](common/provisioning-configuration-save.png)

Essa operação começa o ciclo de sincronização inicial de todos os usuários e grupos definidos no **Escopo** na seção **Configurações**. O ciclo inicial leva mais tempo do que as sincronizações subsequentes, que ocorrem aproximadamente a cada 40 minutos, desde que o serviço de provisionamento do Azure AD esteja em execução. 

## <a name="step-6-monitor-your-deployment"></a>Etapa 6. Monitorar a implantação
Depois de configurar o provisionamento, use os seguintes recursos para monitorar a implantação:

1. Use os [logs de provisionamento](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-provisioning-logs) para determinar quais usuários foram provisionados com êxito ou não.
2. Confira a [barra de progresso](https://docs.microsoft.com/azure/active-directory/app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user) para ver o status do ciclo de provisionamento e saber como fechá-la para concluir.
3. Se a configuração de provisionamento parecer estar em um estado não íntegro, o aplicativo entrará em quarentena. Saiba mais sobre os estados de quarentena [aqui](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-quarantine-status).

## <a name="additional-resources"></a>Recursos adicionais

* [Gerenciamento do provisionamento de conta de usuário para Aplicativos Empresariais](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Próximas etapas

* [Saiba como fazer revisão de logs e obter relatórios sobre atividade de provisionamento](../manage-apps/check-status-user-account-provisioning.md)
