---
title: Configurar um indexador de BLOB
titleSuffix: Azure Cognitive Search
description: Configure um indexador de blob do Azure para automatizar a indexação de conteúdo de BLOB para operações de pesquisa de texto completo no Azure Pesquisa Cognitiva.
manager: nitinme
author: MarkHeff
ms.author: maheff
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 09/23/2020
ms.openlocfilehash: e3419711c9a7358914f85574f6dbd5af29def1cf
ms.sourcegitcommit: dc68a2c11bae2e9d57310d39fbed76628233fd7f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/28/2020
ms.locfileid: "91403596"
---
# <a name="how-to-configure-a-blob-indexer-in-azure-cognitive-search"></a>Como configurar um indexador de blob no Azure Pesquisa Cognitiva

Este artigo mostra como usar o Azure Pesquisa Cognitiva para indexar documentos baseados em texto (como PDFs, Microsoft Office documentos e vários outros formatos comuns) armazenados no armazenamento de BLOBs do Azure. Primeiro, ele explica as noções básicas de configuração de um indexador de blob. Em seguida, ele explora mais profundamente os comportamentos e cenários que você pode encontrar.

<a name="SupportedFormats"></a>

## <a name="supported-formats"></a>Formatos com suporte

O indexador de blob pode extrair o texto dos seguintes formatos de documento:

[!INCLUDE [search-blob-data-sources](../../includes/search-blob-data-sources.md)]

## <a name="set-up-blob-indexing"></a>Configurar a indexação de BLOB

Você pode configurar um indexador do Armazenamento de Blobs do Azure usando:

* [Azure portal](https://ms.portal.azure.com)
* [API REST](/rest/api/searchservice/Indexer-operations) do Azure pesquisa cognitiva
* SDK do [.net](/dotnet/api/overview/azure/search) pesquisa cognitiva do Azure

> [!NOTE]
> Alguns recursos (por exemplo, mapeamentos de campo) ainda não estão disponíveis no portal e precisam ser usados por meio de programação.
>

Aqui, demonstraremos o fluxo usando a API REST.

### <a name="step-1-create-a-data-source"></a>Etapa 1: Criar uma fonte de dados

Uma fonte de dados especifica quais dados indexar, as credenciais necessárias para acessar os dados e as políticas que identificam com eficiência as alterações nos dados (linhas novas, modificadas ou excluídas). Uma fonte de dados pode ser usada por vários indexadores no mesmo serviço de pesquisa.

Para a indexação de blobs, a fonte de dados deve ter as seguintes propriedades requeridas:

* **name** é o nome exclusivo da fonte de dados dentro de seu serviço de pesquisa.
* **type** deve ser `azureblob`.
* * * as credenciais fornecem a cadeia de conexão da conta de armazenamento como o `credentials.connectionString` parâmetro. Consulte [Como especificar credenciais](#Credentials) abaixo para obter detalhes.
* **container** especifica um contêiner em sua conta de armazenamento. Por padrão, todos os blobs no contêiner são recuperáveis. Se você quiser apenas blobs de índice em um diretório virtual específico, especifique o diretório usando o parâmetro opcional **query**.

Para criar uma fonte de dados:

```http
    POST https://[service name].search.windows.net/datasources?api-version=2020-06-30
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>;" },
        "container" : { "name" : "my-container", "query" : "<optional-virtual-directory-name>" }
    }
```

Para obter mais informações sobre Criar a API da Fonte de Dados, consulte [Criar Fonte de Dados](/rest/api/searchservice/create-data-source).

<a name="Credentials"></a>

#### <a name="how-to-specify-credentials"></a>Como especificar credenciais

Você pode fornecer as credenciais para o contêiner de blobs de uma das seguintes maneiras:

* **Cadeia de conexão de identidade gerenciada**: `ResourceId=/subscriptions/<your subscription ID>/resourceGroups/<your resource group name>/providers/Microsoft.Storage/storageAccounts/<your storage account name>/;` 

  Essa cadeia de conexão não requer uma chave de conta, mas você deve seguir as instruções para [Configurar uma conexão com uma conta de armazenamento do Azure usando uma identidade gerenciada](search-howto-managed-identities-storage.md).

* **Cadeia de conexão da conta de armazenamento de acesso completo**: `DefaultEndpointsProtocol=https;AccountName=<your storage account>;AccountKey=<your account key>`

  É possível obter a cadeia de conexão no portal do Azure navegando para a folha da conta de armazenamento > Configurações > Chaves (para contas de armazenamento Clássicas) ou Configurações > Chaves de acesso (para contas de armazenamento do Azure Resource Manager).

* Cadeia de conexão de SAS ( **assinatura de acesso compartilhado** ) da conta de armazenamento:`BlobEndpoint=https://<your account>.blob.core.windows.net/;SharedAccessSignature=?sv=2016-05-31&sig=<the signature>&spr=https&se=<the validity end time>&srt=co&ss=b&sp=rl`

  As SAS devem ter as permissões de lista e leitura nos contêineres e objetos (blobs neste caso).

* **Assinatura de acesso compartilhado do contêiner**: `ContainerSharedAccessUri=https://<your storage account>.blob.core.windows.net/<container name>?sv=2016-05-31&sr=c&sig=<the signature>&se=<the validity end time>&sp=rl`

  As SAS devem ter a lista e permissões de leitura no contêiner.

Para obter mais informações sobre assinaturas de acesso compartilhado de armazenamento, consulte [usando assinaturas de acesso compartilhado](../storage/common/storage-sas-overview.md).

> [!NOTE]
> Se você usar credenciais SAS, você precisará atualizar as credenciais de fonte de dados periodicamente com assinaturas renovadas para impedir sua expiração. Se as credenciais SAS expirarem, o indexador irá falhar com uma mensagem de erro semelhante à `Credentials provided in the connection string are invalid or have expired.`.  

### <a name="step-2-create-an-index"></a>Etapa 2: Criar um índice

O índice especifica os campos em um documento, atributos e outras construções que modelam a experiência de pesquisa.

Veja como criar um índice com um campo `content` pesquisável para armazenar o texto extraído dos blobs:

```http
    POST https://[service name].search.windows.net/indexes?api-version=2020-06-30
    Content-Type: application/json
    api-key: [admin key]

    {
          "name" : "my-target-index",
          "fields": [
            { "name": "id", "type": "Edm.String", "key": true, "searchable": false },
            { "name": "content", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": false, "facetable": false }
          ]
    }
```

Para obter mais informações, consulte [criar índice (API REST)](/rest/api/searchservice/create-index).

### <a name="step-3-create-an-indexer"></a>Etapa 3: Criar um indexador

Um indexador conecta uma fonte de dados a um índice de pesquisa de destino e fornece um agendamento para automatizar a atualização de dados.

Uma vez que o índice e a fonte de dados forem criados, será possível criar o indexador:

```http
    POST https://[service name].search.windows.net/indexers?api-version=2020-06-30
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "blob-indexer",
      "dataSourceName" : "blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" }
    }
```

Esse indexador será executado a cada duas horas (o intervalo de agendamento é definido como "PT2H"). Para executar um indexador a cada 30 minutos, defina o intervalo para "PT30M". O intervalo mais curto com suporte é de 5 minutos. O agendamento é opcional – se ele for omitido, um indexador será executado apenas uma vez quando for criado. No entanto, você pode executar um indexador sob demanda a qualquer momento.   

Para obter mais informações, consulte [criar indexador (API REST)](/rest/api/searchservice/create-indexer). Para obter mais informações sobre como definir as agendas do indexador, confira [Como agendar indexadores para o Azure Cognitive Search](search-howto-schedule-indexers.md).

<a name="how-azure-search-indexes-blobs"></a>

## <a name="how-blobs-are-indexed"></a>Como os BLOBs são indexados

Dependendo da [configuração do indexador](#PartsOfBlobToIndex), o indexador de blobs pode indexar somente metadados de armazenamento (algo útil quando você está preocupado apenas com os metadados e não precisa indexar o conteúdo dos blobs), metadados de armazenamento e conteúdo ou conteúdo textual e de metadados. Por padrão, o indexador extrai os metadados e o conteúdo.

> [!NOTE]
> Por padrão, os blobs com conteúdo estruturado, como JSON, CSV e indexados como uma única parte de texto. Se você quiser indexar BLOBs JSON e CSV de forma estruturada, consulte [indexando BLOBs JSON](search-howto-index-json-blobs.md) e [indexando BLOBs CSV](search-howto-index-csv-blobs.md) para obter mais informações.
>
> Um documento composto ou incorporado (como um arquivo ZIP, um documento do Word com emails inseridos do Outlook contendo anexos ou um. O arquivo MSG com anexos) também é indexado como um único documento. Por exemplo, todas as imagens extraídas dos anexos de um. O arquivo MSG será retornado no campo normalized_images.

* O conteúdo textual do documento é extraído para um campo de cadeia de caracteres chamado `content`.

  > [!NOTE]
  > O Azure Pesquisa Cognitiva limita a quantidade de texto que ele extrai, dependendo do tipo de preço: 32.000 caracteres para a camada gratuita, 64.000 para Basic, 4 milhões para Standard, 8 milhões para Standard S2 e 16 milhões para o Standard S3. Um aviso é incluído na resposta de status do indexador para documentos truncados.  

* As propriedades de metadados especificadas pelo usuário e presentes no blob, se houver alguma, são extraídas literalmente. Observe que isso requer que um campo seja definido no índice com o mesmo nome da chave de metadados do blob. Por exemplo, se o blob tiver uma chave de metadados de `Sensitivity` com valor `High` , você deverá definir um campo chamado `Sensitivity` no índice de pesquisa e ele será preenchido com o valor `High` .

* As propriedades de metadados de blob padrão são extraídas para os seguintes campos:

  * **metadata\_storage\_name** (Edm.String): o nome do arquivo do blob. Por exemplo, se você tiver um blob /my-container/my-folder/subfolder/resume.pdf, o valor desse campo será `resume.pdf`.

  * **metadata\_storage\_path** (Edm.String): o URI completo do blob, incluindo a conta de armazenamento. Por exemplo, `https://myaccount.blob.core.windows.net/my-container/my-folder/subfolder/resume.pdf`

  * **metadata\_storage\_content\_type** (Edm.String): o tipo de conteúdo, conforme especificado pelo código usado para carregar o blob. Por exemplo, `application/octet-stream`.

  * **metadata\_storage\_last\_modified** (Edm.DateTimeOffset): último carimbo de data/hora modificado para o blob. O Azure Pesquisa Cognitiva usa esse carimbo de data/hora para identificar BLOBs alterados, para evitar a reindexação de tudo após a indexação inicial.

  * **metadata\_storage\_size** (Edm.Int64): tamanho do blob em bytes.

  * **metadata\_storage\_content\_md5** (Edm.String): hash MD5 do conteúdo do blob, se estiver disponível.

  * ** \_ \_ \_ token SAS de armazenamento de metadados** (EDM. String)-um token SAS temporário que pode ser usado por [habilidades personalizadas](cognitive-search-custom-skill-interface.md) para obter acesso ao blob. Esse token não deve ser armazenado para uso posterior, pois ele pode expirar.

* As propriedades de metadados específicas a cada formato de documento são extraídas nos campos listados [aqui](#ContentSpecificMetadata).

Não é necessário definir os campos para todas as propriedades acima em seu índice de pesquisa, basta capturar as propriedades necessárias para seu aplicativo.

> [!NOTE]
> Geralmente, os nomes de campo no índice existente serão diferentes dos nomes de campo gerados durante a extração do documento. Você pode usar **mapeamentos de campo** para mapear os nomes de propriedade fornecidos pelo Azure pesquisa cognitiva para os nomes de campo no índice de pesquisa. Você verá um exemplo de uso de mapeamento de campo abaixo.
>

<a name="DocumentKeys"></a>

### <a name="defining-document-keys-and-field-mappings"></a>Definir chaves de documento e mapeamentos de campo

No Azure Pesquisa Cognitiva, a chave do documento identifica exclusivamente um documento. Cada índice de pesquisa deve ter exatamente um campo de chave do tipo Edm.String. O campo de chave é necessário para cada documento adicionado ao índice (é, na verdade, o único campo obrigatório).  

Você deve considerar cuidadosamente qual campo extraído deve ser mapeado para o campo de chave de seu índice. Os candidatos são:

* **metadata\_storage\_name**: este pode ser um candidato conveniente, mas observe que 1) talvez os nomes não sejam exclusivos, pois você pode ter blobs com o mesmo nome em pastas diferentes, e 2) o nome pode conter caracteres inválidos em chaves de documento, por exemplo, traços. Você pode lidar com caracteres inválidos usando a `base64Encode` [função de mapeamento de campo](search-indexer-field-mappings.md#base64EncodeFunction). Se fizer isso, lembre-se de codificar as chaves de documento ao transmiti-las em chamadas à API, como Pesquisa. (Por exemplo, em .NET você pode usar o método [UrlTokenEncode](/dotnet/api/system.web.httpserverutility.urltokenencode) para essa finalidade).

* **metadata\_storage\_path**: o uso do caminho completo garante a exclusividade, mas o caminho contém definitivamente caracteres `/` que são [inválidos em uma chave de documento](/rest/api/searchservice/naming-rules).  Como foi mencionado acima, você tem a opção de codificar as chaves usando a  [função](search-indexer-field-mappings.md#base64EncodeFunction)`base64Encode`.

* Uma terceira opção é adicionar uma propriedade de metadados personalizada aos BLOBs. No entanto, essa opção requer que o processo de carregamento de blob adicione essa propriedade de metadados a todos os BLOBs. Como a chave é uma propriedade obrigatória, todos os blobs que não tiverem essa propriedade apresentarão falha na indexação.

> [!IMPORTANT]
> Se não houver nenhum mapeamento explícito para o campo de chave no índice, o Azure Pesquisa Cognitiva usará automaticamente `metadata_storage_path` como os valores de chave key e base-64 codificações (a segunda opção acima).
>
>

Para este exemplo, vamos escolher o campo `metadata_storage_name` como a chave do documento. Vamos supor também que o índice tenha um campo de chave chamado `key` e um campo `fileSize` para armazenamento do tamanho do documento. Para conectar as coisas conforme o desejado, especifique os seguintes mapeamentos de campo ao criar ou atualizar o indexador:

```http
    "fieldMappings" : [
      { "sourceFieldName" : "metadata_storage_name", "targetFieldName" : "key", "mappingFunction" : { "name" : "base64Encode" } },
      { "sourceFieldName" : "metadata_storage_size", "targetFieldName" : "fileSize" }
    ]
```

Para unir tudo isso, veja como você pode adicionar mapeamentos de campo e habilitar a codificação das chaves em base-64 para um indexador existente:

```http
    PUT https://[service name].search.windows.net/indexers/blob-indexer?api-version=2020-06-30
    Content-Type: application/json
    api-key: [admin key]

    {
      "dataSourceName" : " blob-datasource ",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "fieldMappings" : [
        { "sourceFieldName" : "metadata_storage_name", "targetFieldName" : "key", "mappingFunction" : { "name" : "base64Encode" } },
        { "sourceFieldName" : "metadata_storage_size", "targetFieldName" : "fileSize" }
      ]
    }
```

Para obter mais informações, consulte [mapeamentos de campo e transformações](search-indexer-field-mappings.md).

#### <a name="what-if-you-need-to-encode-a-field-to-use-it-as-a-key-but-you-also-want-to-search-it"></a>E se você precisar codificar um campo para usá-lo como uma chave, mas também deseja pesquisá-lo?

Há ocasiões em que você precisa usar uma versão codificada de um campo como metadata_storage_path como a chave, mas também precisa que esse campo seja pesquisável (sem codificação). Para resolver esse problema, você pode mapeá-lo em dois campos; um que será usado para a chave e outro que será usado para fins de pesquisa. No exemplo abaixo, o campo de *chave* contém o caminho codificado, enquanto o campo de *caminho* não está codificado e será usado como o campo pesquisável no índice.

```http
    PUT https://[service name].search.windows.net/indexers/blob-indexer?api-version=2020-06-30
    Content-Type: application/json
    api-key: [admin key]

    {
      "dataSourceName" : " blob-datasource ",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "fieldMappings" : [
        { "sourceFieldName" : "metadata_storage_path", "targetFieldName" : "key", "mappingFunction" : { "name" : "base64Encode" } },
        { "sourceFieldName" : "metadata_storage_path", "targetFieldName" : "path" }
      ]
    }
```
<a name="WhichBlobsAreIndexed"></a>

## <a name="index-by-file-type"></a>Indexar por tipo de arquivo

É possível controlar quais blobs são indexados e quais são ignorados.

### <a name="include-blobs-having-specific-file-extensions"></a>Incluir BLOBs com extensões de arquivo específicas

Você pode indexar apenas os blobs com as extensões do nome de arquivo especificadas usando o parâmetro de configuração do indexador `indexedFileNameExtensions`. O valor é uma cadeia de caracteres que contém uma lista separada por vírgulas das extensões de arquivo (com um ponto à esquerda). Por exemplo, para indexar apenas blobs de .PDF e .DOCX, faça o seguinte:

```http
    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2020-06-30
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "indexedFileNameExtensions" : ".pdf,.docx" } }
    }
```

### <a name="exclude-blobs-having-specific-file-extensions"></a>Excluir BLOBs com extensões de arquivo específicas

Você pode excluir os blobs com extensões do nome de arquivo específicas da indexação usando o parâmetro de configuração `excludedFileNameExtensions`. O valor é uma cadeia de caracteres que contém uma lista separada por vírgulas das extensões de arquivo (com um ponto à esquerda). Por exemplo, para indexar todos os blobs, exceto aqueles com as extensões .PNG e .JPEG, faça o seguinte:

```http
    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2020-06-30
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "excludedFileNameExtensions" : ".png,.jpeg" } }
    }
```

Se ambos `indexedFileNameExtensions` os `excludedFileNameExtensions` parâmetros e estiverem presentes, o Azure pesquisa cognitiva primeiro examinará `indexedFileNameExtensions` , em seguida, em `excludedFileNameExtensions` . Isso significa que, se a mesma extensão de arquivo estiver presente nas duas listas, ela será excluída da indexação.

<a name="PartsOfBlobToIndex"></a>

## <a name="index-parts-of-a-blob"></a>Partes de índice de um blob

Você pode controlar quais partes dos blobs são indexadas usando o parâmetro de configuração `dataToExtract`. Ele pode usar os seguintes valores:

* `storageMetadata` – especifica que somente [as propriedades de blob padrão e os metadados especificados pelo usuário](../storage/blobs/storage-blob-container-properties-metadata.md) são indexados.
* `allMetadata` – especifica que os metadados de armazenamento e os [metadados específicos do tipo de conteúdo](#ContentSpecificMetadata) extraídos do conteúdo do blob são indexados.
* `contentAndMetadata` – especifica que todos os metadados e conteúdo textual extraídos do blob são indexados. Esse é o valor padrão.

Por exemplo, para indexar apenas os metadados de armazenamento, use:

```http
    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2020-06-30
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "dataToExtract" : "storageMetadata" } }
    }
```

### <a name="using-blob-metadata-to-control-how-blobs-are-indexed"></a>Usando metadados de blob para controlar como os blobs são indexados

Os parâmetros de configuração descritos acima se aplicam a todos os blobs. Às vezes, você pode desejar controlar como *blobs individuais* são indexados. Faça isso adicionando as seguintes propriedades de metadados de blob e valores:

| Property name | Valor da propriedade | Explicação |
| --- | --- | --- |
| AzureSearch_Skip |"true" |Instrui o indexador de blobs a ignorar o blob por completo. Não há nenhuma tentativa de extração de metadados nem de conteúdo. Isso é útil quando um blob específico falha repetidamente e interrompe o processo de indexação. |
| AzureSearch_SkipContent |"true" |Isso é equivalente à configuração `"dataToExtract" : "allMetadata"` descrita [acima](#PartsOfBlobToIndex), no escopo de um blob específico. |

## <a name="index-from-multiple-sources"></a>Índice de várias fontes

Talvez você queira "montar" documentos de várias fontes em seu índice. Por exemplo, convém mesclar texto de blobs com outros metadados armazenados no Cosmos DB. Você pode até usar a API de indexação por push junto a vários indexadores para criar documentos de pesquisa de várias partes.

Para que isso funcione, todos os indexadores e outros componentes precisam concordar com a chave de documento. Para obter detalhes adicionais sobre este tópico, consulte [indexar várias fontes de dados do Azure](./tutorial-multiple-data-sources.md) ou esta postagem de blog, [combinar documentos com outros dados no Azure pesquisa cognitiva](https://blog.lytzen.name/2017/01/combine-documents-with-other-data-in.html).

## <a name="index-large-datasets"></a>Indexar conjuntos de grandes volumes

A indexação de blobs pode ser um processo demorado. Nos casos em que você tem milhões de blobs para indexar, é possível acelerar a indexação particionando seus dados e usando vários indexadores para processar os dados em paralelo. Veja como você pode configurar isso:

* Particione seus dados em vários contêineres de blob ou pastas virtuais

* Configure várias fontes de dados do Azure Pesquisa Cognitiva, uma por contêiner ou pasta. Para apontar para uma pasta de blobs, use o parâmetro `query`:

    ```json
    {
        "name" : "blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "<your storage connection string>" },
        "container" : { "name" : "my-container", "query" : "my-folder" }
    }
    ```

* Crie um indexador correspondente para cada fonte de dados. Todos os indexadores podem apontar para o mesmo índice de pesquisa de destino.  

* Uma unidade de pesquisa em seu serviço pode executar um indexador a qualquer momento. Criar vários indexadores conforme descrito acima será útil somente se eles realmente forem executados em paralelo. Para executar vários indexadores em paralelo, escale horizontalmente seu serviço de pesquisa criando um número apropriado de partições e réplicas. Por exemplo, se o serviço de pesquisa tiver seis unidades de pesquisa (por exemplo, 2 partições x 3 réplicas), seis indexadores poderão ser executados simultaneamente, resultando em um aumento de seis vezes a taxa de transferência da indexação. Para saber mais sobre dimensionamento e planejamento de capacidade, confira [ajustar a capacidade de um serviço de pesquisa cognitiva do Azure](search-capacity-planning.md).

<a name="DealingWithErrors"></a>

## <a name="handle-errors"></a>Tratar erros

Por padrão, o indexador de blobs é interrompido assim que encontra um blob com um tipo de conteúdo sem suporte (por exemplo, uma imagem). Evidentemente, você pode usar o parâmetro `excludedFileNameExtensions` para ignorar alguns tipos de conteúdo. No entanto, talvez você precise indexar blobs sem conhecer todos os possíveis tipos de conteúdo com antecedência. Para continuar a indexação quando um tipo de conteúdo sem suporte é encontrado, defina o parâmetro de configuração `failOnUnsupportedContentType` como `false`:

```http
    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2020-06-30
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "failOnUnsupportedContentType" : false } }
    }
```

Para alguns BLOBs, o Azure Pesquisa Cognitiva não pode determinar o tipo de conteúdo ou não pode processar um documento de tipo de conteúdo com suporte de outra forma. Para ignorar este modo de falha, defina o parâmetro de configuração `failOnUnprocessableDocument` como false:

```http
      "parameters" : { "configuration" : { "failOnUnprocessableDocument" : false } }
```

O Azure Pesquisa Cognitiva limita o tamanho dos BLOBs que são indexados. Esses limites são documentados em [limites de serviço no Azure pesquisa cognitiva](./search-limits-quotas-capacity.md). Por padrão, os blobs superdimensionados são tratados como erros. No entanto, você ainda pode indexar os metadados de armazenamento de blobs superdimensionados se você definir o parâmetro de configuração `indexStorageMetadataOnlyForOversizedDocuments` como true:

```http
    "parameters" : { "configuration" : { "indexStorageMetadataOnlyForOversizedDocuments" : true } }
```

Você também poderá continuar a indexação se ocorrem erros a qualquer momento do processamento, ao analisar blobs ou ao adicionar documentos a um índice. Para ignorar um número específico de erros, defina os parâmetros de configuração `maxFailedItems` e `maxFailedItemsPerBatch` como os valores desejados. Por exemplo:

```http
    {
      ... other parts of indexer definition
      "parameters" : { "maxFailedItems" : 10, "maxFailedItemsPerBatch" : 10 }
    }
```

<a name="ContentSpecificMetadata"></a>

## <a name="content-type-specific-metadata-properties"></a>Propriedades de metadados específicas ao tipo de conteúdo

A tabela a seguir resume o processamento feito para cada formato de documento e descreve as propriedades de metadados extraídas pelo Pesquisa Cognitiva do Azure.

| Formato de documento/tipo de conteúdo | Metadados extraídos | Detalhes do processamento |
| --- | --- | --- |
| HTML (texto/HTML) |`metadata_content_encoding`<br/>`metadata_content_type`<br/>`metadata_language`<br/>`metadata_description`<br/>`metadata_keywords`<br/>`metadata_title` |Remoção da marcação HTML e extração do texto |
| PDF (aplicativo/PDF) |`metadata_content_type`<br/>`metadata_language`<br/>`metadata_author`<br/>`metadata_title` |Extração do texto, incluindo documentos incorporados (excluindo imagens) |
| DOCX (application/vnd.openxmlformats-officedocument.wordprocessingml.document) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` |Extração de texto, incluindo documentos incorporados |
| DOC (application/msword) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` |Extração de texto, incluindo documentos incorporados |
| DOCM (aplicativo/vnd.ms-word.document. macroenabled. 12) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` |Extração de texto, incluindo documentos incorporados |
| XML do WORD (application/vnd. ms-word2006ml) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` |Remoção da marcação XML e extração do texto |
| WORD 2003 XML (application/vnd. ms-WordML) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date` |Remoção da marcação XML e extração do texto |
| XLSX (application/vnd.openxmlformats-officedocument.spreadsheetml.sheet) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified` |Extração de texto, incluindo documentos incorporados |
| XLS (application/vnd.ms-excel) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified` |Extração de texto, incluindo documentos incorporados |
| XLSM (application/vnd. MS-Excel. Sheet. macroenabled. 12) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified` |Extração de texto, incluindo documentos incorporados |
| PPTX (application/vnd.openxmlformats-officedocument.presentationml.presentation) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_slide_count`<br/>`metadata_title` |Extração de texto, incluindo documentos incorporados |
| PPT (application/vnd.ms-powerpoint) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_slide_count`<br/>`metadata_title` |Extração de texto, incluindo documentos incorporados |
| PPTM (application/vnd. ms-PowerPoint. Presentation. macroenabled. 12) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_slide_count`<br/>`metadata_title` |Extração de texto, incluindo documentos incorporados |
| MSG (application/vnd.ms-outlook) |`metadata_content_type`<br/>`metadata_message_from`<br/>`metadata_message_from_email`<br/>`metadata_message_to`<br/>`metadata_message_to_email`<br/>`metadata_message_cc`<br/>`metadata_message_cc_email`<br/>`metadata_message_bcc`<br/>`metadata_message_bcc_email`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_subject` |Extração de texto, incluindo texto extraído de anexos. `metadata_message_to_email``metadata_message_cc_email`e `metadata_message_bcc_email` são coleções de cadeias de caracteres, o restante dos campos são cadeias.|
| ODT (application/vnd. Oasis. OpenDocument. Text) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` |Extração de texto, incluindo documentos incorporados |
| ODS (application/vnd. Oasis. OpenDocument. Spreadsheet) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified` |Extração de texto, incluindo documentos incorporados |
| ODP (application/vnd. Oasis. OpenDocument. Presentation) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`title` |Extração de texto, incluindo documentos incorporados |
| ZIP (application/zip) |`metadata_content_type` |Extração do texto de todos os documentos no arquivo |
| GZ (aplicativo/gzip) |`metadata_content_type` |Extração do texto de todos os documentos no arquivo |
| EPUB (Application/ePub + zip) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_title`<br/>`metadata_description`<br/>`metadata_language`<br/>`metadata_keywords`<br/>`metadata_identifier`<br/>`metadata_publisher` |Extração do texto de todos os documentos no arquivo |
| XML (application/xml) |`metadata_content_type`<br/>`metadata_content_encoding`<br/> |Remoção da marcação XML e extração do texto |
| JSON (application/json) |`metadata_content_type`<br/>`metadata_content_encoding` |Extrair texto<br/>OBSERVAÇÃO: se você precisar extrair vários campos de documento de um blob JSON, consulte [Como indexar blobs JSON](search-howto-index-json-blobs.md) para obter detalhes |
| EML (message/rfc822) |`metadata_content_type`<br/>`metadata_message_from`<br/>`metadata_message_to`<br/>`metadata_message_cc`<br/>`metadata_creation_date`<br/>`metadata_subject` |Extração do texto, incluindo anexos |
| RTF (aplicativo/rtf) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_page_count`<br/>`metadata_word_count`<br/> | Extrair texto|
| Texto sem formatação (text/plain) |`metadata_content_type`<br/>`metadata_content_encoding`<br/> | Extrair texto|

## <a name="see-also"></a>Confira também

* [Indexadores na Pesquisa Cognitiva do Azure](search-indexer-overview.md)
* [Entender os BLOBs usando o ia](search-blob-ai-integration.md)
* [Visão geral da indexação de BLOB](search-blob-storage-integration.md)