---
title: Proteger o Azure Data Lake Analytics para vários usuários
description: Saiba como configurar vários usuários para executar trabalhos no Azure Data Lake Analytics.
ms.service: data-lake-analytics
services: data-lake-analytics
ms.reviewer: jasonh
ms.topic: how-to
ms.date: 05/30/2018
ms.openlocfilehash: 9006a22c588a7f1456585d40da0b4345145c6d05
ms.sourcegitcommit: 0e8a4671aa3f5a9a54231fea48bcfb432a1e528c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87132476"
---
# <a name="configure-user-access-to-job-information-to-job-information-in-azure-data-lake-analytics"></a>Configurar o acesso do usuário para informações de trabalho no Azure Data Lake Analytics 

No Azure Data Lake Analytics, é possível usar várias contas de usuário ou entidades de serviço para executar trabalhos. 

Para que esses mesmos usuários vejam informações detalhadas sobre o trabalho, eles precisam ler o conteúdo das pastas de trabalho. As pastas de trabalho estão localizadas no diretório `/system/`. 

Se as permissões necessárias não forem configuradas, o usuário poderá ver um erro: `Graph data not available - You don't have permissions to access the graph data.` 

## <a name="configure-user-access-to-job-information"></a>Configurar o acesso do usuário a informações do trabalho

Você pode usar o **Assistente Adicionar Usuário** para configurar as ACLs nas pastas. Para saber mais, confira [Adicionar um novo usuário](data-lake-analytics-manage-use-portal.md#add-a-new-user).

Se você precisar de mais controle granular ou de script de permissões, proteja as pastas da seguinte maneira:

1. Conceda permissões de **execução** (por meio de uma ACL de acesso) na pasta raiz:
   - /
   
2. Conceda permissões de **execução** e **leitura** (por meio de uma ACL de acesso e uma ACL padrão) nas pastas que contêm as pastas de trabalho. Por exemplo, para um trabalho específico executado no dia 25 de maio de 2018, estas pastas precisam ser acessadas:
   - /system
   - /system/jobservice
   - /system/jobservice/jobs
   - /system/jobservice/jobs/Usql
   - /system/jobservice/jobs/Usql/2018
   - /system/jobservice/jobs/Usql/2018/05
   - /system/jobservice/jobs/Usql/2018/05/25
   - /system/jobservice/jobs/Usql/2018/05/25/11
   - /system/jobservice/jobs/Usql/2018/05/25/11/01
   - /system/jobservice/jobs/Usql/2018/05/25/11/01/b074bd7a-1448-d879-9d75-f562b101bd3d

## <a name="next-steps"></a>Próximas etapas
[Adicionar um novo usuário](data-lake-analytics-manage-use-portal.md#add-a-new-user)
