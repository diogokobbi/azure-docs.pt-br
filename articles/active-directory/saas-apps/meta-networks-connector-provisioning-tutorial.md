---
title: 'Tutorial: configurar o conector de metaredes para o provisionamento automático de usuário com o Azure Active Directory | Microsoft Docs'
description: Saiba como configurar Azure Active Directory para provisionar e desprovisionar automaticamente contas de usuário para o conector de metaredes.
services: active-directory
author: zchia
writer: zchia
manager: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: article
ms.date: 10/01/2019
ms.author: Zhchia
ms.openlocfilehash: bcddaec1660082c2d3fed42e0c10cbdde987693c
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91273510"
---
# <a name="tutorial-configure-meta-networks-connector-for-automatic-user-provisioning"></a>Tutorial: configurar o conector de metaredes para provisionamento automático de usuário

O objetivo deste tutorial é demonstrar as etapas a serem executadas no conector de metaredes e Azure Active Directory (AD do Azure) para configurar o Azure AD para provisionar e desprovisionar automaticamente usuários e/ou grupos no conector de metaredes.

> [!NOTE]
> Este tutorial descreve um conector compilado na parte superior do Serviço de Provisionamento de Usuário do Microsoft Azure AD. Para detalhes importantes sobre o que esse serviço faz, como funciona e as perguntas frequentes, consulte [Automatizar o provisionamento e desprovisionamento de usuários para aplicativos SaaS com o Azure Active Directory](../app-provisioning/user-provisioning.md).
>
> Atualmente, esse conector está em versão prévia pública. Para obter mais informações sobre os Termos de uso gerais do Microsoft Azure para a versão prévia de recursos, confira [Termos de uso adicionais para versões prévias do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Pré-requisitos

O cenário descrito neste tutorial pressupõe que você já tem os seguintes pré-requisitos:

* Um locatário do Azure AD
* [Um locatário do conector de metaredes](https://www.metanetworks.com/)
* Uma conta de usuário no conector de metaredes com permissões de administrador.

## <a name="assigning-users-to-meta-networks-connector"></a>Atribuindo usuários ao conector de metaredes

O Azure Active Directory usa um conceito chamado *atribuições* para determinar quais usuários devem receber acesso aos aplicativos selecionados. No contexto do provisionamento automático de usuário, somente os usuários e/ou grupos que foram atribuídos a um aplicativo no Azure AD são sincronizados.

Antes de configurar e habilitar o provisionamento automático de usuário, você deve decidir quais usuários e/ou grupos no Azure AD precisam de acesso ao conector de metaredes. Depois de decidir, você pode atribuir esses usuários e/ou grupos ao conector de metaredes seguindo estas instruções:
* [Atribuir um usuário ou um grupo a um aplicativo empresarial](../manage-apps/assign-user-or-group-access-portal.md)

## <a name="important-tips-for-assigning-users-to-meta-networks-connector"></a>Dicas importantes para atribuir usuários ao conector meta Networks

* É recomendável que um único usuário do Azure AD seja atribuído ao conector meta Networks para testar a configuração automática de provisionamento de usuário. Outros usuários e/ou grupos podem ser atribuídos mais tarde.

* Ao atribuir um usuário ao conector de metaredes, você deve selecionar qualquer função específica do aplicativo válida (se disponível) na caixa de diálogo de atribuição. Usuários com a função **Acesso padrão** são excluídos do provisionamento.

## <a name="setup-meta-networks-connector-for-provisioning"></a>Conector de metaredes de instalação para provisionamento

1. Entre no console do [administrador do conector de Metaredes](https://login.metanetworks.com/login/) usando o nome da sua organização. Navegue até **administração > chaves de API**.

    ![Console de administração do conector de metaredes](media/meta-networks-connector-provisioning-tutorial/apikey.png)

2.  Clique no sinal de adição no lado superior direito da tela para criar uma nova chave de **API**.

    ![Conector de metaredes e ícone](media/meta-networks-connector-provisioning-tutorial/plusicon.png)

3.  Defina o **nome da chave de API** e a **Descrição da chave de API**.

    ![Token de criação de conector de metaredes](media/meta-networks-connector-provisioning-tutorial/keyname.png)

4.  Ative os privilégios de **gravação** para **grupos** e **usuários**.

    ![Privilégios do conector de metaredes](media/meta-networks-connector-provisioning-tutorial/privileges.png)

5.  Clique em **Adicionar**. Copie o **segredo** e salve-o, pois essa será a única vez que você pode exibi-lo. Esse valor será inserido no campo token secreto na guia provisionamento do aplicativo conector de metaredes na portal do Azure.

    ![Token de criação de conector de metaredes](media/meta-networks-connector-provisioning-tutorial/token.png)

6.  Adicione um IdP navegando até **administração > configurações > IdP > criar novo**.

    ![Conector de metaredes adicionar IdP](media/meta-networks-connector-provisioning-tutorial/newidp.png)

7.  Na página de **configuração do IDP** , você pode **nomear** sua configuração IDP e escolher um **ícone**.

    ![Nome do IdP do conector de redes meta](media/meta-networks-connector-provisioning-tutorial/idpname.png)

    ![Ícone de IdP do conector de metaredes](media/meta-networks-connector-provisioning-tutorial/icon.png)

8.  Em **Configurar scim** , selecione o nome da chave de API criado nas etapas anteriores. Clique em **Save**.

    ![Conector de metaredes configurar SCIM](media/meta-networks-connector-provisioning-tutorial/configure.png)

9.  Navegue até **administração > configurações > guia IDP**. Clique no nome da configuração IdP criada nas etapas anteriores para exibir a **ID do IDP**. Essa **ID** é adicionada ao final da **URL do locatário** ao inserir o valor no campo **URL do locatário** na guia provisionamento do aplicativo conector de metaredes na portal do Azure.

    ![ID IdP do conector de redes meta](media/meta-networks-connector-provisioning-tutorial/idpid.png)

## <a name="add-meta-networks-connector-from-the-gallery"></a>Adicionar conector de metaredes da Galeria

Antes de configurar o conector de metaredes para o provisionamento automático de usuário com o Azure AD, você precisa adicionar o conector de metaredes da Galeria de aplicativos do Azure AD à sua lista de aplicativos SaaS gerenciados.

**Para adicionar o conector de metaredes da Galeria de aplicativos do Azure AD, execute as seguintes etapas:**

1. No **[portal do Azure](https://portal.azure.com)**, no painel de navegação à esquerda, selecione **Azure Active Directory**.

    ![O botão Azure Active Directory](common/select-azuread.png)

2. Vá para **Aplicativos da empresa**, em seguida, selecione **Todos os aplicativos**.

    ![A folha Aplicativos empresariais](common/enterprise-applications.png)

3. Para adicionar um novo aplicativo, selecione o botão **novo aplicativo** na parte superior do painel.

    ![O botão Novo aplicativo](common/add-new-app.png)

4. Na caixa de pesquisa, insira **conector de Metaredes**, selecione **conector de metaredes** no painel de resultados e, em seguida, clique no botão **Adicionar** para adicionar o aplicativo.

    ![Meta redes conector na lista de resultados](common/search-new-app.png)

## <a name="configuring-automatic-user-provisioning-to-meta-networks-connector"></a>Configurando o provisionamento automático de usuário para o conector de metaredes 

Esta seção orienta você pelas etapas para configurar o serviço de provisionamento do Azure AD para criar, atualizar e desabilitar usuários e/ou grupos no conector de metaredes com base em atribuições de usuário e/ou grupo no Azure AD.

> [!TIP]
> Você também pode optar por habilitar o logon único baseado em SAML para o conector de metaredes, seguindo as instruções fornecidas no [tutorial de logon único do conector de Metaredes](https://docs.microsoft.com/azure/active-directory/saas-apps/metanetworksconnector-tutorial). O logon único pode ser configurado independentemente do provisionamento automático de usuário, embora esses dois recursos se complementem uns aos outros

### <a name="to-configure-automatic-user-provisioning-for-meta-networks-connector-in-azure-ad"></a>Para configurar o provisionamento automático de usuário para o conector meta Networks no Azure AD:

1. Entre no [portal do Azure](https://portal.azure.com). Selecione **Aplicativos Empresariais** e **Todos os Aplicativos**.

    ![Folha de aplicativos empresariais](common/enterprise-applications.png)

2. Na lista de aplicativos, selecione **Meta redes conector**.

    ![O link de Meta redes conector na lista de aplicativos](common/all-applications.png)

3. Selecione a guia **Provisionamento**.

    ![Captura de tela das opções de gerenciamento com a opção de provisionamento chamada out.](common/provisioning.png)

4. Defina o **Modo de Provisionamento** como **Automático**.

    ![Captura de tela da lista suspensa modo de provisionamento com a opção automática chamada out.](common/provisioning-automatic.png)

5. Na seção **credenciais de administrador** , insira `https://api.metanetworks.com/v1/scim/<IdP ID>` a **URL de locatário**. Insira o valor do **token de autenticação scim** recuperado anteriormente no **token secreto**. Clique em **testar conexão** para garantir que o Azure ad possa se conectar ao conector de metaredes. Se a conexão falhar, verifique se a conta do conector de metaredes tem permissões de administrador e tente novamente.

    ![URL do locatário + token](common/provisioning-testconnection-tenanturltoken.png)

6. No campo **Notificação por Email**, insira o endereço de email de uma pessoa ou grupo que deverá receber as notificações de erro de provisionamento e selecione a caixa de seleção - **Enviar uma notificação por email quando ocorrer uma falha**.

    ![Email de notificação](common/provisioning-notification-email.png)

7. Clique em **Save** (Salvar).

8. Na seção **mapeamentos** , selecione **sincronizar Azure Active Directory usuários para conector de metaredes**.

    ![Mapeamentos de usuário do conector de metaredes](media/meta-networks-connector-provisioning-tutorial/usermappings.png)

9. Examine os atributos de usuário que são sincronizados do Azure AD para o conector de metaredes na seção **mapeamento de atributos** . Os atributos selecionados como propriedades **correspondentes** são usados para corresponder as contas de usuário no conector meta Networks para operações de atualização. Selecione o botão **Salvar** para confirmar as alterações.

    ![Atributos de usuário do conector de metaredes](media/meta-networks-connector-provisioning-tutorial/userattributes.png)

10. Na seção **mapeamentos** , selecione **sincronizar grupos de Azure Active Directory para conector de metaredes**.

    ![Mapeamentos do grupo de conectores de metaredes](media/meta-networks-connector-provisioning-tutorial/groupmappings.png)

11. Examine os atributos de grupo que são sincronizados do Azure AD para o conector de metaredes na seção **mapeamento de atributos** . Os atributos selecionados como propriedades **correspondentes** são usados para corresponder os grupos no conector de metaredes para operações de atualização. Selecione o botão **Salvar** para confirmar as alterações.

    ![Atributos do grupo de conectores de metaredes](media/meta-networks-connector-provisioning-tutorial/groupattributes.png)

12. Para configurar filtros de escopo, consulte as seguintes instruções fornecidas no [tutorial do Filtro de Escopo](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).

13. Para habilitar o serviço de provisionamento do Azure AD para o conector de metaredes, altere o **status de provisionamento** para **ativado** na seção **configurações** .

    ![Status do provisionamento ativado](common/provisioning-toggle-on.png)

14. Defina os usuários e/ou grupos que você deseja provisionar para o conector de metaredes escolhendo os valores desejados no **escopo** na seção **configurações** .

    ![Escopo de provisionamento](common/provisioning-scope.png)

15. Quando estiver pronto para provisionar, clique em **Salvar**.

    ![Salvando a configuração de provisionamento](common/provisioning-configuration-save.png)

Essa operação inicia a sincronização inicial de todos os usuários e/ou grupos definidos no **Escopo** na seção **Configurações**. Observe que a sincronização inicial levará mais tempo do que as sincronizações subsequentes, que ocorrem aproximadamente a cada 40 minutos, desde que o serviço de provisionamento do Microsoft Azure Active Directory esteja em execução. Você pode usar a seção **detalhes de sincronização** para monitorar o progresso e seguir os links para o relatório de atividade de provisionamento, que descreve todas as ações executadas pelo serviço de provisionamento do Azure AD no conector de metaredes.

Para saber mais sobre como ler os logs de provisionamento do Azure AD, consulte [Relatórios sobre o provisionamento automático de contas de usuário](../app-provisioning/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Gerenciamento do provisionamento de conta de usuário para Aplicativos Empresariais](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Próximas etapas

* [Saiba como fazer revisão de logs e obter relatórios sobre atividade de provisionamento](../app-provisioning/check-status-user-account-provisioning.md)

