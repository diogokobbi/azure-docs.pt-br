---
title: 'SDK do Java v4 do Azure Cosmos DB para a API do SQL: notas de versão e recursos'
description: Saiba tudo sobre o SDK do Java v4 do Azure Cosmos DB para API e SDK do SQL, incluindo datas de lançamento, datas de desativação e alterações feitas entre cada versão do SDK do Java Assíncrono do Azure Cosmos DB SQL.
author: anfeldma-ms
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: java
ms.topic: reference
ms.date: 08/12/2020
ms.author: anfeldma
ms.custom: devx-track-java
ms.openlocfilehash: 92f1b722e39083463fd7fa57fdf8508c2c4084cd
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91326636"
---
# <a name="azure-cosmos-db-java-sdk-v4-for-core-sql-api-release-notes-and-resources"></a>SDK do Java v4 do Azure Cosmos DB para API do Core (SQL): notas de versão e recursos
> [!div class="op_single_selector"]
> * [SDK v3 do .NET](sql-api-sdk-dotnet-standard.md)
> * [SDK do .NET v2](sql-api-sdk-dotnet.md)
> * [SDK v2 do .NET Core](sql-api-sdk-dotnet-core.md)
> * [SDK v2 do feed de alterações do .NET](sql-api-sdk-dotnet-changefeed.md)
> * [Node.js](sql-api-sdk-node.md)
> * [SDK do Java v4](sql-api-sdk-java-v4.md)
> * [SDK do Java Assíncrono v2](sql-api-sdk-async-java.md)
> * [SDK do Java Síncrono v2](sql-api-sdk-java.md)
> * [Spring data v2](sql-api-sdk-java-spring-v2.md)
> * [Spring data v3](sql-api-sdk-java-spring-v3.md)
> * [Conector do Spark](sql-api-sdk-java-spark.md)
> * [Python](sql-api-sdk-python.md)
> * [REST](/rest/api/cosmos-db/)
> * [Provedor de recursos REST](/rest/api/cosmos-db-resource-provider/)
> * [SQL](sql-api-query-reference.md)
> * [Executor em massa-.NET v2](sql-api-sdk-bulk-executor-dot-net.md)
> * [Executor em massa – Java](sql-api-sdk-bulk-executor-java.md)

O SDK do Java v4 do Azure Cosmos DB para Core (SQL) combina uma API assíncrona e uma API síncrona em um artefato Maven. O SDK da v4 oferece desempenho aprimorado, novos recursos de API e suporte a Async com base no Project Reactor e na [biblioteca Netty](https://netty.io/). Os usuários podem esperar um desempenho aprimorado com o SKD do Java v4 do Azure Cosmos DB em comparação com o [SDK do Java Assíncrono v2 do Azure Cosmos DB](sql-api-sdk-async-java.md) e o [SDK do Java Síncrono v2 do Azure Cosmos DB](sql-api-sdk-java.md).

> [!IMPORTANT]  
> Essas notas de versão são apenas para o SDK do Java v4 do Azure Cosmos DB. Se você estiver usando uma versão mais antiga do que a v4, confira o guia [Migrar para o SDK do Java v4 do Azure Cosmos DB](migrate-java-v4-sdk.md) para obter ajuda na atualização da versão 4.
>
> Aqui estão três etapas para começar rápido!
> 1. Instale o [runtime mínimo com suporte para Java, JDK 8](/java/azure/jdk/?view=azure-java-stable) para poder usar o SDK.
> 2. Trabalhe no [Guia de Início Rápido do SDK do Java v4 do Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/create-sql-api-java), que fornece acesso ao artefato Maven, e percorre as solicitações básicas do Azure Cosmos DB.
> 3. Leia as [dicas de desempenho](performance-tips-java-sdk-v4-sql.md) e os guias de [solução de problemas](troubleshoot-java-sdk-v4-sql.md) do SDK do Java v4 do Azure Cosmos DB para otimizar o SDK para seu aplicativo.
>
> Os [workshops e laboratórios do Azure Cosmos DB](https://aka.ms/cosmosworkshop) são outro ótimo recurso para aprender como usar o SDK do Java v4 do Azure Cosmos DB!
>

## <a name="helpful-content"></a>Conteúdo útil

| Conteúdo | Link |
|---|---|
|**Baixe o SDK**| [Maven](https://mvnrepository.com/artifact/com.azure/azure-cosmos) |
|**Documentação da API** | [Documentação de referência de API Java](https://docs.microsoft.com/java/api/overview/azure/cosmosdb/client?view=azure-java-stable) |
|**Contribuir para o SDK** | [SDK do Azure para repositório central do Java no GitHub](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/cosmos/azure-cosmos) | 
|**Introdução** | [Início Rápido: Criar um aplicativo Java para gerenciar os dados de API de SQL do Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/create-sql-api-java) <br> [Repositório GitHub com código de início rápido](https://github.com/Azure-Samples/azure-cosmos-java-getting-started) | 
|**Amostras de código básico** | [Azure Cosmos DB: exemplos de Java para API do SQL](sql-api-java-sdk-samples.md) <br> [Repositório GitHub com código de exemplo](https://github.com/Azure-Samples/azure-cosmos-java-sql-api-samples)|
|**Aplicativo de console com feed de alterações**| [Feed de alterações-exemplo de Java SDK v4](create-sql-api-java-changefeed.md) <br> [Repositório GitHub com código de exemplo](https://github.com/Azure-Samples/azure-cosmos-java-sql-app-example)| 
|**Exemplo de aplicativo Web**| [Compilar um aplicativo Web com o SDK do Java v4](sql-api-java-application.md) <br> [Repositório GitHub com código de exemplo](https://github.com/Azure-Samples/azure-cosmos-java-sql-api-todo-app)|
| **Dicas de desempenho**| [Dicas de desempenho para o SDK do Java v4](performance-tips-java-sdk-v4-sql.md)| 
| **Solução de problemas** | [Solucionar problemas do SDK do Java v4](troubleshoot-java-sdk-v4-sql.md) |
| **Migrar para v4 de um SDK mais antigo** | [Migrar para o SDK do Java V4](migrate-java-v4-sdk.md) |
| **runtime mínimo com suporte**|[JDK 8](/java/azure/jdk/?view=azure-java-stable) | 
| **Workshops e laboratórios do Azure Cosmos DB** |[Home page dos workshops do Cosmos DB](https://aka.ms/cosmosworkshop)

[!INCLUDE[Release notes](~/azure-sdk-for-java-cosmos-db/sdk/cosmos/azure-cosmos/CHANGELOG.md)]

## <a name="faq"></a>Perguntas frequentes
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)] 

## <a name="next-steps"></a>Próximas etapas
Para saber mais sobre o Cosmos DB, consulte a página de serviço do [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/).