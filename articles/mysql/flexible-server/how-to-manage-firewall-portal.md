---
title: Gerenciar regras de firewall-portal do Azure-banco de dados do Azure para MySQL-servidor flexível
description: Criar e gerenciar regras de firewall para o banco de dados do Azure para MySQL-servidor flexível usando o portal do Azure
author: ambhatna
ms.author: ambhatna
ms.service: mysql
ms.topic: how-to
ms.date: 9/21/2020
ms.openlocfilehash: 132319575147c2ff1075881b1f1faec8bc5029f4
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90933743"
---
# <a name="create-and-manage-firewall-rules-for-azure-database-for-mysql---flexible-server-using-the-azure-portal"></a>Criar e gerenciar regras de firewall para o banco de dados do Azure para MySQL-servidor flexível usando o portal do Azure

> [!IMPORTANT]
> O banco de dados do Azure para MySQL servidor flexível está atualmente em visualização pública.

O Servidor Flexível do Banco de Dados do Azure para MySQL oferece suporte a dois tipos de métodos de conectividade de rede mutuamente exclusivos, para se conectar ao seu servidor flexível. As duas opções são:

1. Acesso público (endereços IP permitidos)
2. Acesso privado (Integração VNet)

Neste artigo, nos concentraremos na criação do servidor MySQL com **acesso público (endereços IP permitidos)** usando portal do Azure e fornecerei uma visão geral do gerenciamento de regras de firewall após a criação de um servidor flexível. Com *acesso público (endereços IP permitidos)*, as conexões com o servidor MySQL são restritas somente a endereços IP permitidos. Os endereços IP do cliente precisam ser permitidos em regras de firewall. Para saber mais sobre isso, consulte [acesso público (endereços IP permitidos)](./concepts-networking.md#public-access-allowed-ip-addresses). As regras de firewall podem ser definidas no momento da criação do servidor (recomendado), mas também podem ser adicionadas posteriormente. Neste artigo, forneceremos uma visão geral de como criar e gerenciar regras de firewall usando o acesso público (endereços IP permitidos).

## <a name="create-a-firewall-rule-when-creating-a-server"></a>Criar uma regra de firewall ao criar um servidor

1. Selecione **Criar um recurso** (+) no canto superior esquerdo do portal.
2. Selecione **Bancos de Dados** > **Banco de Dados do Azure para MySQL**. Você também pode inserir **MySQL** na caixa de pesquisa para localizar o serviço.
3. Selecione **Servidor flexível** como a opção de implantação.
4. Preencha o formulário **básico** .
5. Vá para a guia **rede** para configurar o modo como você deseja se conectar ao servidor.
6. No **método de conectividade**, selecione *acesso público (endereços IP permitidos)*. Para criar as **regras de firewall**, especifique o nome da regra de firewall e o endereço IP único ou um intervalo de endereços. Se você quiser limitar a regra a um único endereço IP, digite o mesmo endereço no campo para endereço IP inicial e endereço IP final. Abrir o firewall permite que administradores, usuários e aplicativos acessem qualquer banco de dados no servidor MySQL ao qual tenham credenciais válidas.
   > [!Note]
   > O banco de dados do Azure para MySQL servidor flexível cria um firewall no nível do servidor. Ele impede que os aplicativos e as ferramentas externas se conectem ao servidor e aos bancos de dados no servidor, a menos que uma regra seja criada para abrir o firewall em endereços IP específicos.

7. Selecione **Examinar + criar** para examinar a configuração do servidor flexível.
8.  Selecione **Criar** para provisionar o servidor. O provisionamento pode levar alguns minutos.

## <a name="create-a-firewall-rule-after-server-is-created"></a>Criar uma regra de firewall após a criação do servidor

1. Na [portal do Azure](https://portal.azure.com/), selecione o servidor flexível do banco de dados do Azure para MySQL no qual você deseja adicionar regras de firewall.
2. Na página servidor flexível, no título **configurações** , clique em **rede** para abrir a página rede para o servidor flexível.

   <!--:::image type="content" source="./media/howto-manage-firewall-portal/1-connection-security.png" alt-text="Azure portal - click Connection Security":::-->

3. Clique em **Adicionar endereço IP do cliente atual** nas regras de firewall. Isso cria automaticamente uma regra de firewall com o endereço IP público do seu computador, como visto pelo sistema do Azure.

   <!--:::image type="content" source="./media/howto-manage-firewall-portal/2-add-my-ip.png" alt-text="Azure portal - click Add My IP":::-->

4. Verifique seu endereço IP antes de salvar a configuração. Em algumas situações, o endereço IP observado pelo Portal do Azure é diferente do endereço IP usado ao acessar a Internet e os servidores do Azure. Portanto, talvez seja necessário alterar o endereço IP inicial e o endereço IP final para que a regra funcione conforme o esperado.

   Você pode usar um mecanismo de pesquisa ou outra ferramenta online para verificar seu próprio endereço IP. Por exemplo, pesquise "qual é meu IP".

   <!--:::image type="content" source="./media/howto-manage-firewall-portal/3-what-is-my-ip.png" alt-text="Bing search for What is my IP":::-->

5. Adicionar outros intervalos de endereço. Nas regras de firewall para o servidor flexível do banco de dados do Azure para MySQL, você pode especificar um único endereço IP ou um intervalo de endereços. Se você quiser limitar a regra a um único endereço IP, digite o mesmo endereço no campo para endereço IP inicial e endereço IP final. Abrir o firewall permite que administradores, usuários e aplicativos acessem qualquer banco de dados no servidor MySQL ao qual tenham credenciais válidas.

   <!--:::image type="content" source="./media/howto-manage-firewall-portal/4-specify-addresses.png" alt-text="Azure portal - firewall rules":::-->

6. Clique em **salvar** na barra de ferramentas para salvar esta regra de firewall. Aguarde a confirmação de que a atualização das regras de firewall foi bem-sucedida.

   <!--:::image type="content" source="./media/howto-manage-firewall-portal/5-save-firewall-rule.png" alt-text="Azure portal - click Save":::-->

## <a name="connect-from-azure"></a>Conectar do Azure

Talvez você queira habilitar recursos ou aplicativos implantados no Azure para se conectarem ao seu servidor flexível. Isso inclui aplicativos Web hospedados no serviço Azure App, em execução em uma VM do Azure, em um gateway de gerenciamento de dados Azure Data Factory e muito mais.

Quando um aplicativo no Azure tenta se conectar ao seu servidor, o Firewall verifica se as conexões do Azure são permitidas. Você pode habilitar essa configuração selecionando a opção **permitir acesso público dos serviços e recursos do Azure no Azure para este servidor** no portal na guia **rede** e clicar em **salvar**.

Os recursos não precisam estar na mesma rede virtual (VNet) ou grupo de recursos para que a regra de firewall habilite essas conexões. Se a tentativa de conexão não for permitida, a solicitação não alcançará o servidor flexível do banco de dados do Azure para MySQL.

> [!IMPORTANT]
> Esta opção configura o firewall para permitir todas as conexões do Azure, incluindo as conexões das assinaturas de outros clientes. Ao selecionar essa opção, verifique se as permissões de logon e de usuário limitam o acesso somente a usuários autorizados.
>
> É recomendável escolher o **acesso privado (integração VNet)** para acessar com segurança o servidor flexível.
>

## <a name="manage-existing-firewall-rules-through-the-azure-portal"></a>Gerenciar regras de firewall existentes por meio do portal do Azure

Repita as etapas a seguir para gerenciar as regras de firewall.

- Para adicionar o computador atual, clique em + **Adicionar endereço IP do cliente atual** nas regras de firewall. Clique em **Salvar** para salvar as alterações.
- Para adicionar mais endereços IP, digite o Nome da Regra, o Endereço IP Inicial e o Endereço IP Final. Clique em **Salvar** para salvar as alterações.
- Para modificar uma regra existente, clique em qualquer um dos campos na regra e modifique. Clique em **Salvar** para salvar as alterações.
- Para excluir uma regra existente, clique nas reticências [...] e clique em **Excluir** para remover a regra. Clique em **Salvar** para salvar as alterações.

## <a name="next-steps"></a>Próximas etapas
- Saiba mais sobre a [rede no banco de dados do Azure para MySQL servidor flexível](./concepts-networking.md)
- Saiba mais sobre [as regras de firewall de servidor flexíveis do banco de dados do Azure para MySQL](./concepts-networking.md#public-access-allowed-ip-addresses)
- [Crie e gerencie as regras de firewall do banco de dados do Azure para MySQL usando CLI do Azure](./how-to-manage-firewall-cli.md).