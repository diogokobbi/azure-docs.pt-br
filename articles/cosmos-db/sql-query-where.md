---
title: Cláusula WHERE em Azure Cosmos DB
description: Saiba mais sobre a cláusula WHERE do SQL para Azure Cosmos DB
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/06/2020
ms.author: tisande
ms.openlocfilehash: ceffb203ccc2cca1ff6e1c53644cde955c2e0acb
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86523495"
---
# <a name="where-clause-in-azure-cosmos-db"></a>Cláusula WHERE em Azure Cosmos DB

A cláusula WHERE opcional ( `WHERE <filter_condition>` ) especifica a (s) condição (ões) que os itens JSON de origem devem satisfazer para que a consulta as inclua nos resultados. Um item JSON deve avaliar as condições especificadas a serem `true` consideradas para o resultado. A camada de índice usa a cláusula WHERE para determinar o menor subconjunto de itens de origem que podem fazer parte do resultado.
  
## <a name="syntax"></a>Sintaxe
  
```sql  
WHERE <filter_condition>  
<filter_condition> ::= <scalar_expression>  
  
```  
  
## <a name="arguments"></a>Argumentos

- `<filter_condition>`  
  
   Especifica a condição a ser atendida para que os documentos sejam retornados.  
  
- `<scalar_expression>`  
  
   Expressão que representa o valor a ser calculado. Consulte [expressões escalares](sql-query-scalar-expressions.md) para obter detalhes.  
  
## <a name="remarks"></a>Comentários
  
  Para que o documento seja retornado, uma expressão especificada como condição de filtro deve ser avaliada como verdadeira. Somente o valor booliano `true` atenderá à condição, qualquer outro valor: indefinido, nulo, falso, número, matriz ou objeto não atenderá à condição.

  Se você incluir sua chave de partição na `WHERE` cláusula como parte de um filtro de igualdade, sua consulta filtrará automaticamente somente as partições relevantes.

## <a name="examples"></a>Exemplos

A consulta a seguir solicita itens que contêm uma `id` propriedade cujo valor é `AndersenFamily` . Ele exclui qualquer item que não tenha uma `id` propriedade ou cujo valor não corresponda `AndersenFamily` .

```sql
    SELECT f.address
    FROM Families f
    WHERE f.id = "AndersenFamily"
```

Os resultados são:

```json
    [{
      "address": {
        "state": "WA",
        "county": "King",
        "city": "Seattle"
      }
    }]
```

### <a name="scalar-expressions-in-the-where-clause"></a>Expressões escalares na cláusula WHERE

O exemplo anterior mostrou uma consulta de igualdade simples. A API do SQL também dá suporte a várias [expressões escalares](sql-query-scalar-expressions.md). As expressões mais usadas são as binárias e unárias. Referências de propriedade do objeto JSON fonte também são expressões válidas.

Você pode usar os seguintes operadores binários com suporte:  

|**Tipo de operador**  | **Valores** |
|---------|---------|
|Aritmético | +,-,*,/,% |
|Bit a bit    | \|, &, ^, <<, >>, >>> (deslocamento à direita com preenchimento com zero) |
|Lógica    | AND, OR, NOT      |
|Comparação | =, !=, &lt;, &gt;, &lt;=, &gt;=, <> |
|Cadeia de caracteres     |  \|\| (concatenar) |

As consultas a seguir usam operadores binários:

```sql
    SELECT *
    FROM Families.children[0] c
    WHERE c.grade % 2 = 1     -- matching grades == 5, 1

    SELECT *
    FROM Families.children[0] c
    WHERE c.grade ^ 4 = 1    -- matching grades == 5

    SELECT *
    FROM Families.children[0] c
    WHERE c.grade >= 5    -- matching grades == 5
```

Você também pode usar os operadores unários +,-, ~, e não em consultas, conforme mostrado nos exemplos a seguir:

```sql
    SELECT *
    FROM Families.children[0] c
    WHERE NOT(c.grade = 5)  -- matching grades == 1

    SELECT *
    FROM Families.children[0] c
    WHERE (-c.grade = -5)  -- matching grades == 5
```

Você também pode usar referências de propriedade em consultas. Por exemplo, `SELECT * FROM Families f WHERE f.isRegistered` retorna o item JSON que contém a propriedade `isRegistered` com valor igual a `true` . Qualquer outro valor, como,,,, `false` `null` `Undefined` `<number>` `<string>` , `<object>` ou `<array>` , exclui o item do resultado. Além disso, você pode usar a `IS_DEFINED` função de verificação de tipo para consultar com base na presença ou ausência de uma determinada propriedade JSON. Por exemplo, `SELECT * FROM Families f WHERE NOT IS_DEFINED(f.isRegistered)` retorna qualquer item JSON que não tenha um valor para `isRegistered` .

## <a name="next-steps"></a>Próximas etapas

- [Guia de Introdução](sql-query-getting-started.md)
- [Palavra-chave IN](sql-query-keywords.md#in)
- [Cláusula FROM](sql-query-from.md)
