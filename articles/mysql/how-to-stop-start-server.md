---
title: Stop/Start-portal do Azure-banco de dados do Azure para servidor MySQL
description: Este artigo descreve como parar/iniciar operações no banco de dados do Azure para MySQL.
author: kummanish
ms.author: manishku
ms.service: mysql
ms.topic: how-to
ms.date: 09/21/2020
ms.openlocfilehash: f09b6d48e8a98b0995c882769d6c978996324dad
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91340989"
---
# <a name="stopstart-an-azure-database-for-mysql"></a>Parar/iniciar um banco de dados do Azure para MySQL

> [!IMPORTANT]
> A funcionalidade de parar/iniciar para o banco de dados do Azure para MySQL está atualmente em visualização pública.

Este artigo fornece um procedimento passo a passo para executar a interrupção e o início do servidor flexível.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este guia de instruções, você precisa:

-   Você deve ter um servidor flexível do banco de dados do Azure para MySQL.

> [!NOTE]
> Consulte a limitação do uso de [parar/iniciar](concepts-servers.md#limitations-of-stopstart-operation)

## <a name="how-to-stopstart-the-azure-database-for-mysql-using-azure-portal"></a>Como parar/iniciar o banco de dados do Azure para MySQL usando portal do Azure

### <a name="stop-a-running-server"></a>Parar um servidor em execução

1.  Na [portal do Azure](https://portal.azure.com/), escolha o servidor MySQL que você deseja parar.

2.  Na página **visão geral** , clique no botão **parar** na barra de ferramentas.

    :::image type="content" source="./media/howto-stop-start-server/mysql-stop-server.png" alt-text="Servidor de parada do banco de dados do Azure para MySQL":::

    > [!NOTE]
    > Depois que o servidor for interrompido, as outras operações de gerenciamento não estarão disponíveis para o servidor flexível.

### <a name="start-a-stopped-server"></a>Iniciar um servidor interrompido

1.  Na [portal do Azure](https://portal.azure.com/), escolha o servidor flexível que você deseja iniciar.

2.  Na página **visão geral** , clique no botão **Iniciar** na barra de ferramentas.

    :::image type="content" source="./media/howto-stop-start-server/mysql-start-server.png" alt-text="Servidor inicial do banco de dados do Azure para MySQL":::

    > [!NOTE]
    > Depois que o servidor é iniciado, todas as operações de gerenciamento agora estão disponíveis para o servidor flexível.

## <a name="how-to-stopstart-the-azure-database-for-mysql-using-cli"></a>Como parar/iniciar o banco de dados do Azure para MySQL usando a CLI

### <a name="stop-a-running-server"></a>Parar um servidor em execução

1.  Na [portal do Azure](https://portal.azure.com/), escolha o servidor MySQL que você deseja parar.

2.  Na página **visão geral** , clique no botão **parar** na barra de ferramentas.

    ```azurecli-interactive
    az mysql server stop --name <server-name> -g <resource-group-name>
    ```
    > [!NOTE]
    > Depois que o servidor for interrompido, as outras operações de gerenciamento não estarão disponíveis para o servidor flexível.

### <a name="start-a-stopped-server"></a>Iniciar um servidor interrompido

1.  Na [portal do Azure](https://portal.azure.com/), escolha o servidor flexível que você deseja iniciar.

2.  Na página **visão geral** , clique no botão **Iniciar** na barra de ferramentas.

    ```azurecli-interactive
    az mysql server start --name <server-name> -g <resource-group-name>
    ```
    > [!NOTE]
    > Depois que o servidor é iniciado, todas as operações de gerenciamento agora estão disponíveis para o servidor flexível.

## <a name="next-steps"></a>Próximas etapas
Saiba mais sobre [como criar alertas sobre métricas](howto-alert-on-metric.md).
