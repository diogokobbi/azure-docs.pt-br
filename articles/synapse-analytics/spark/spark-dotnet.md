---
title: Usar .NET para Apache Spark
description: Saiba como usar o .NET e o Apache Spark para fazer processamento em lotes, streaming em tempo real, aprendizado de máquina e gravar consultas ad hoc nos notebooks do Azure Synapse Analytics.
author: mamccrea
services: synapse-analytics
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: spark
ms.date: 05/01/2020
ms.author: mamccrea
ms.reviewer: jrasnick
ms.openlocfilehash: b445787c44efca4eb6bdee3e61eb39735e5c6b14
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91259893"
---
# <a name="use-net-for-apache-spark-with-azure-synapse-analytics"></a>Use o .NET para Apache Spark com Azure Synapse Analytics

O [.NET para Apache Spark](https://dot.net/spark) fornece suporte .NET gratuito, open-source e multiplataforma para o Spark. 

Ele fornece associações .NET para o Spark, que permite que você acesse as APIs do Spark por meio de C# e F #. Com o .NET para Apache Spark, você também pode gravar e executar funções definidas pelo usuário para o Spark escrito em .NET. As APIs do .NET para Spark permitem que acessar todos os aspectos dos DataFrames do Spark que ajudam a analisar seus dados, incluindo o Spark SQL, Delta Lake e o fluxo estruturado.

Você pode analisar dados com o .NET para Apache Spark por meio de definições de trabalho do lote Spark ou com notebooks interativos do Azure Synapse Analytics. Neste artigo, você aprenderá a usar o .NET para Apache Spark com o Azure Synapse usando as duas técnicas.

## <a name="submit-batch-jobs-using-the-spark-job-definition"></a>Enviar trabalhos em lotes usando a definição de trabalho do Spark

Visite o tutorial para saber como usar o Azure Synapse Analytics para [criar definições de trabalho do Apache Spark para pools do Synapse Spark](apache-spark-job-definitions.md). Se você não tiver empacotado seu aplicativo para enviar ao Azure Synapse, conclua as etapas a seguir.

1. Execute os comandos a seguir para publicar seu aplicativo. Certifique-se de substituir *mySparkApp* pelo caminho para seu aplicativo.

   **No Windows:**

   ```dotnetcli
   cd mySparkApp
   dotnet publish -c Release -f netcoreapp3.1 -r ubuntu.16.04-x64
   ```

   **No Linux:**

   ```bash
   zip -r publish.zip
   ```

## <a name="net-for-apache-spark-in-azure-synapse-analytics-notebooks"></a>.NET para Apache Spark nos notebooks do Azure Synapse Analytics 

Os notebooks são uma ótima opção para criar um protótipo do seu .NET para pipelines e cenários do Apache Spark. Você pode começar a trabalhar com, entender, filtrar, exibir e visualizar seus dados com rapidez e eficiência. 

Engenheiros de dados, cientistas de dados, analistas de negócios e engenheiros de aprendizado de máquina são capazes de colaborar em um documento interativo e compartilhado. Você vê resultados imediatos na exploração de dados e pode visualizar seus dados no mesmo notebook.

### <a name="how-to-use-net-for-apache-spark-notebooks"></a>Como usar o .NET para notebooks do Apache Spark

Ao criar um novo notebook, você escolhe um kernel de linguagem no qual deseja expressar sua lógica de negócios. O suporte a kernel está disponível para várias linguagens, incluindo C#.

Para usar o .NET para Apache Spark no seu notebook do Azure Synapse Analytics, selecione **.NET Spark ( C# )** como o seu kernel e anexe o notebook a um pool do Spark existente.

O notebook .NET Spark se baseia nas experiências interativas do .NET e fornece experiências interativas de C# com a capacidade de usar o .NET para o Spark pronto para uso com a variável de sessão do Spark `spark` já predefinida.

### <a name="net-for-apache-spark-c-kernel-features"></a>Recursos do kernel C# do .NET para Apache Spark

Os recursos a seguir estão disponíveis quando você usa o .NET para Apache Spark no notebook do Azure Synapse Analytics:

* HTML Declarativo: Gere a saída de suas células usando a sintaxe HTML, como cabeçalhos, listas com marcadores e até mesmo exibindo imagens.
* Instruções simples de C# (como atribuições, impressão no console, lançamento de exceções e assim por diante).
* Blocos de código C# de várias linhas (como instruções If, loops foreach, definições de classe e assim por diante).
* Acesso à biblioteca C# padrão (como Sistema, LINQ, Enumeráveis e assim por diante).
* Suporte para [recursos de linguagem C# 8.0](/dotnet/csharp/whats-new/csharp-8?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json).
* "Spark" como uma variável predefinida para dar acesso à sua sessão do Apache Spark.
* Suporte para definir [funções definidas pelo usuário do .NET que podem ser executadas no Apache Spark ](https://github.com/dotnet/spark/blob/master/examples/Microsoft.Spark.CSharp.Examples/Sql).
* Suporte para visualizar a saída de seus trabalhos do Spark usando diferentes gráficos (como linha, barra ou histograma) e layouts (como único, sobreposto e assim por diante) usando a biblioteca `XPlot.Plotly`.
* Capacidade de incluir pacotes NuGet em seu notebook C#.

## <a name="next-steps"></a>Próximas etapas

* [Documentação do .NET para Apache Spark](/dotnet/spark?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)
* [Azure Synapse Analytics](https://docs.microsoft.com/azure/synapse-analytics)
* [.NET Interativo](https://devblogs.microsoft.com/dotnet/creating-interactive-net-documentation/)
