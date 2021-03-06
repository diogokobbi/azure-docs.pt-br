---
title: Criar um sugestor
titleSuffix: Azure Cognitive Search
description: Habilite ações de consulta de tipo antecipado no Azure Pesquisa Cognitiva criando sugestores e solicitações formular que invoquem os termos de consulta autocompletados ou autosugeridos.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 04/21/2020
ms.custom: devx-track-csharp
ms.openlocfilehash: e439f7d2b0232a2e1c36517f24723e4e16f7e6bb
ms.sourcegitcommit: f5580dd1d1799de15646e195f0120b9f9255617b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/29/2020
ms.locfileid: "91537592"
---
# <a name="create-a-suggester-to-enable-autocomplete-and-suggested-results-in-a-query"></a>Criar um Sugestor para habilitar o preenchimento automático e os resultados sugeridos em uma consulta

No Azure Pesquisa Cognitiva, "Pesquisar conforme o tipo" é habilitado por meio de uma construção de **sugestão** adicionada a um [índice de pesquisa](search-what-is-an-index.md). Um Sugestor dá suporte a duas experiências: *preenchimento automático*, que conclui uma entrada parcial para uma consulta de termo inteiro e *sugestões* que os convites clicam para uma correspondência específica. O preenchimento automático produz uma consulta. As sugestões produzem um documento correspondente.

A captura de tela a seguir de [criar seu primeiro aplicativo em C#](tutorial-csharp-type-ahead-and-suggestions.md) ilustra ambos. O preenchimento automático prevê um prazo potencial, concluindo "TW" com "in". Sugestões são resultados de mini pesquisa, onde um campo como nome de Hotel representa um documento de pesquisa de Hotel correspondente do índice. Para obter sugestões, você pode retonar qualquer campo que forneça informações descritivas.

![Comparação visual de consultas autocompletas e sugeridas](./media/index-add-suggesters/hotel-app-suggestions-autocomplete.png "Comparação visual de consultas autocompletas e sugeridas")

Você pode usar esses recursos separadamente ou juntos. Para implementar esses comportamentos no Azure Pesquisa Cognitiva, há um componente de índice e consulta. 

+ No índice, adicione um Sugestor a um índice. Você pode usar o portal, a [API REST](/rest/api/searchservice/create-index)ou o [SDK do .net](/dotnet/api/microsoft.azure.search.models.suggester). O restante deste artigo se concentra na criação de um Sugestor.

+ Na solicitação de consulta, chame uma das [APIs listadas abaixo](#how-to-use-a-suggester).

O suporte de pesquisa conforme o tipo é habilitado em uma base por campo para campos de cadeia de caracteres. Você pode implementar ambos os comportamentos do typeahead na mesma solução de pesquisa se desejar uma experiência semelhante à indicada na captura de tela. Ambas as solicitações direcionam a coleção de *documentos* de índice e respostas específicas são retornadas depois que um usuário tiver fornecido pelo menos uma cadeia de caracteres de entrada de três caracteres.

## <a name="what-is-a-suggester"></a>O que é um Sugestor?

Um Sugestor é uma estrutura de dados interna que dá suporte a comportamentos de pesquisa conforme o tipo, armazenando prefixos para correspondência em consultas parciais. Assim como ocorre com os termos com token, os prefixos são armazenados em índices invertidos, um para cada campo especificado em uma coleção de campos de sugestão.

## <a name="define-a-suggester"></a>Definir um Sugestor

Para criar um Sugestor, adicione um a um [esquema de índice](/rest/api/searchservice/create-index) e [Defina cada propriedade](#property-reference). O melhor momento para criar um Sugestor é quando você também está definindo o campo que o usará.

+ Usar somente campos de cadeia de caracteres

+ Use o analisador Lucene padrão padrão ( `"analyzer": null` ) ou um [analisador de linguagem](index-add-language-analyzers.md) (por exemplo, `"analyzer": "en.Microsoft"` ) no campo

### <a name="choose-fields"></a>Selecionar campos

Embora um Sugestor tenha várias propriedades, ele é basicamente uma coleção de campos de cadeia de caracteres para os quais você está habilitando uma experiência de pesquisa conforme o tipo. Há um Sugestor para cada índice, portanto, a lista de sugestores deve incluir todos os campos que contribuem com conteúdo para sugestões e preenchimento automático.

O preenchimento automático beneficia-se de um pool maior de campos para desenhar, pois o conteúdo adicional tem mais potencial de conclusão do termo.

As sugestões, por outro lado, produzem resultados melhores quando a opção de seu campo é seletiva. Lembre-se de que a sugestão é um proxy para um documento de pesquisa, para que você queira campos que melhor representem um único resultado. Os nomes, títulos ou outros campos exclusivos que distinguem entre várias correspondências funcionam melhor. Se os campos consistem em valores repetitivos, as sugestões consistem em resultados idênticos e um usuário não saberá em qual deles clicar.

Para atender a experiências de pesquisa conforme o uso, adicione todos os campos necessários para preenchimento automático, mas, em seguida, use **$Select**, **$Top**, **$Filter**e **searchFields** para controlar os resultados das sugestões.

### <a name="choose-analyzers"></a>Escolher analisadores

Sua escolha de um analisador determina como os campos são indexados e subsequentemente prefixados. Por exemplo, para uma cadeia de caracteres hifenizada como "sensível ao contexto", o uso de um analisador de linguagem resultará nessas combinações de token: "contexto", "confidencial", "contextual". Se você usou o analisador Lucene padrão, a cadeia de caracteres hifenizada não existiria. 

Ao avaliar os analisadores, considere usar a [API de análise de texto](/rest/api/searchservice/test-analyzer) para obter informações sobre como os termos são indexados e subsequentemente prefixados. Depois de criar um índice, você pode tentar vários analisadores em uma cadeia de caracteres para exibir a saída do token.

Os campos que usam [analisadores personalizados](index-add-custom-analyzers.md) ou [analisadores predefinidos](index-add-custom-analyzers.md#predefined-analyzers-reference) (com exceção do Lucene padrão) são explicitamente despermitidos para evitar resultados fracos.

> [!NOTE]
> Se você precisar contornar a restrição do analisador, por exemplo, se precisar de uma palavra-chave ou ngram Analyzer para determinados cenários de consulta, use dois campos separados para o mesmo conteúdo. Isso permitirá que um dos campos tenha um Sugestor, enquanto o outro pode ser configurado com uma configuração personalizada do analisador.

### <a name="when-to-create-a-suggester"></a>Quando criar um Sugestor

O melhor momento para criar um Sugestor é quando você também está criando a própria definição de campo.

Se você tentar criar um Sugestor usando campos preexistentes, a API não permitirá isso. Os prefixos são gerados durante a indexação, quando termos parciais em duas ou mais combinações de caracteres são indexados ao longo de termos inteiros. Considerando que os campos existentes já foram indexados, você precisará recompilar o índice se quiser adicioná-los a um Sugestor. Para obter mais informações, consulte [como recriar um índice de pesquisa cognitiva do Azure](search-howto-reindex.md).

## <a name="create-using-rest"></a>Criar usando REST

Na API REST, adicione sugestores por meio de [CREATE INDEX](/rest/api/searchservice/create-index) ou [Update index](/rest/api/searchservice/update-index). 

  ```json
  {
    "name": "hotels-sample-index",
    "fields": [
      . . .
          {
              "name": "HotelName",
              "type": "Edm.String",
              "facetable": false,
              "filterable": false,
              "key": false,
              "retrievable": true,
              "searchable": true,
              "sortable": false,
              "analyzer": "en.microsoft",
              "indexAnalyzer": null,
              "searchAnalyzer": null,
              "synonymMaps": [],
              "fields": []
          },
    ],
    "suggesters": [
      {
        "name": "sg",
        "searchMode": "analyzingInfixMatching",
        "sourceFields": ["HotelName"]
      }
    ],
    "scoringProfiles": [
      . . .
    ]
  }
  ```

## <a name="create-using-net"></a>Criar usando o .NET

Em C#, defina um [objeto de sugestão](/dotnet/api/microsoft.azure.search.models.suggester). `Suggesters` é uma coleção, mas só pode pegar um item. 

```csharp
private static void CreateHotelsIndex(SearchServiceClient serviceClient)
{
    var definition = new Index()
    {
        Name = "hotels-sample-index",
        Fields = FieldBuilder.BuildForType<Hotel>(),
        Suggesters = new List<Suggester>() {new Suggester()
            {
                Name = "sg",
                SourceFields = new string[] { "HotelName", "Category" }
            }}
    };

    serviceClient.Indexes.Create(definition);

}
```

## <a name="property-reference"></a>Referência de propriedade

|Propriedade      |Descrição      |
|--------------|-----------------|
|`name`        |O nome do sugestor.|
|`searchMode`  |A estratégia usada para pesquisar frases candidatas. O único modo com suporte no momento é `analyzingInfixMatching` , que corresponde atualmente ao início de um termo.|
|`sourceFields`|Uma lista de um ou mais campos que são a fonte do conteúdo para obter sugestões. Os campos devem ser do tipo `Edm.String` e `Collection(Edm.String)` . Se um analisador for especificado no campo, ele deverá ser um analisador Nomeado [desta lista](/dotnet/api/microsoft.azure.search.models.analyzername) (não um analisador personalizado).<p/> Como prática recomendada, especifique somente os campos que se prestam a uma resposta esperada e apropriada, seja uma cadeia de caracteres completa em uma barra de pesquisa ou uma lista suspensa.<p/>Um nome de Hotel é um bom candidato porque tem precisão. Campos detalhados, como descrições e comentários, são muito densos. Da mesma forma, campos repetitivos, como categorias e marcas, são menos eficazes. Nos exemplos, incluímos "category" de qualquer forma para demonstrar que você pode incluir vários campos. |

<a name="how-to-use-a-suggester"></a>

## <a name="use-a-suggester"></a>Usar um Sugestor

Um Sugestor é usado em uma consulta. Depois que um Sugestor for criado, chame uma das seguintes APIs para uma experiência de pesquisa conforme o tipo:

+ [API REST de sugestões](/rest/api/searchservice/suggestions) 
+ [API REST de preenchimento automático](/rest/api/searchservice/autocomplete) 
+ [Método SuggestWithHttpMessagesAsync] (/dotnet/api/microsoft.azure.search.idocumentsoperations.suggestwithhttpmessagesasync?
+ [Método AutocompleteWithHttpMessagesAsync](/dotnet/api/microsoft.azure.search.idocumentsoperations.autocompletewithhttpmessagesasync)

Em um aplicativo de pesquisa, o código do cliente deve aproveitar uma biblioteca, como o [jQuery interface do usuário](https://jqueryui.com/autocomplete/) , para coletar a consulta parcial e fornecer a correspondência. Para obter mais informações sobre essa tarefa, consulte [Adicionar resultados automáticos ou sugeridos ao código do cliente](search-autocomplete-tutorial.md).

O uso da API é ilustrado na chamada a seguir para a API REST de preenchimento automático. Há dois argumentos neste exemplo. Primeiro, assim como acontece com todas as consultas, a operação é contra a coleção de documentos de um índice e a consulta inclui um parâmetro de **pesquisa** que, nesse caso, fornece a consulta parcial. Em segundo lugar, você deve adicionar **suggesterName** à solicitação. Se um Sugestor não estiver definido no índice, uma chamada para preenchimento automático ou sugestões falhará.

```http
POST /indexes/myxboxgames/docs/autocomplete?search&api-version=2020-06-30
{
  "search": "minecraf",
  "suggesterName": "sg"
}
```

## <a name="sample-code"></a>Código de exemplo

+ [Crie seu primeiro aplicativo no C# (lição 3 – Adicionar pesquisa conforme o tipo)](tutorial-csharp-type-ahead-and-suggestions.md) demonstra uma construção de sugestão, consultas sugeridas, preenchimento automático e navegação facetada. Este exemplo de código é executado em um serviço de Pesquisa Cognitiva do Azure de área restrita e usa um índice de hotéis pré-carregado para que tudo o que você precisa fazer seja pressionar F5 para executar o aplicativo. Nenhuma assinatura ou entrada é necessária.

+ [DotNetHowToAutocomplete](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToAutocomplete) é um exemplo mais antigo que contém o código C# e Java. Ele também demonstra uma construção de sugestão, consultas sugeridas, preenchimento automático e navegação facetada. Este exemplo de código usa os dados de exemplo hospedados do [NYCJobs](https://github.com/Azure-Samples/search-dotnet-asp-net-mvc-jobs) . 

## <a name="next-steps"></a>Próximas etapas

Recomendamos o seguinte artigo para saber mais sobre como as solicitações são formuladas.

> [!div class="nextstepaction"]
> [Adicionar preenchimento automático e sugestões ao código do cliente](search-autocomplete-tutorial.md)