---
title: Negar acesso à rede pública-portal do Azure-banco de dados do Azure para MySQL
description: Saiba como configurar o acesso de rede pública Deny usando o portal do Azure para o banco de dados do Azure para MySQL
author: kummanish
ms.author: manishku
ms.service: mysql
ms.topic: how-to
ms.date: 03/10/2020
ms.openlocfilehash: eb5c4e4c4dfb73b2f7c9dc9f2629296790790885
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90896145"
---
# <a name="deny-public-network-access-in-azure-database-for-mysql-using-azure-portal"></a>Negar acesso à rede pública no banco de dados do Azure para MySQL usando portal do Azure

Este artigo descreve como você pode configurar um servidor de banco de dados do Azure para MySQL para negar todas as configurações públicas e permitir somente conexões por meio de pontos de extremidade privados para aprimorar ainda mais a segurança da rede.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este guia de instruções, você precisa:

* Um [banco de dados do Azure para MySQL](quickstart-create-mysql-server-database-using-azure-portal.md)

## <a name="set-deny-public-network-access"></a>Definir acesso de rede pública de negação

Siga estas etapas para definir o servidor MySQL negar acesso à rede pública:

1. Na [portal do Azure](https://portal.azure.com/), selecione seu banco de dados do Azure para servidor MySQL existente.

1. Na página MySQL Server, em **configurações**, clique em **segurança de conexão** para abrir a página configuração de segurança de conexão.

1. Em **negar acesso à rede pública**, selecione **Sim** para habilitar o acesso de negação pública para o servidor MySQL.

    :::image type="content" source="./media/howto-deny-public-network-access/setting-deny-public-network-access.PNG" alt-text="Banco de dados do Azure para MySQL negar acesso à rede":::

1. Clique em **Salvar** para salvar as alterações.

1. Uma notificação confirmará que a configuração de segurança de conexão foi habilitada com êxito.

    :::image type="content" source="./media/howto-deny-public-network-access/setting-deny-public-network-access-success.png" alt-text="Banco de dados do Azure para MySQL negar acesso à rede de êxito":::

## <a name="next-steps"></a>Próximas etapas

Saiba mais sobre [como criar alertas sobre métricas](howto-alert-on-metric.md).