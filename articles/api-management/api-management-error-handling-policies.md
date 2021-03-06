---
title: Tratamento de erro em políticas de Gerenciamento de API do Azure | Microsoft Docs
description: Saiba como responder às condições de erro que podem ocorrer durante o processamento de solicitações no Gerenciamento de API do Azure.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: 3c777964-02b2-4f55-8731-8c3bd3c0ae27
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 01/10/2020
ms.author: apimpm
ms.openlocfilehash: bddb4ea3759d19d1e122739fb69cf9bf96c66635
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/11/2020
ms.locfileid: "86243538"
---
# <a name="error-handling-in-api-management-policies"></a>Tratamento de erro em políticas de Gerenciamento de API

Ao fornecer um objeto `ProxyError`, o Gerenciamento de API do Azure permite que os editores respondam a condições de erro que podem ocorrer durante o processamento de solicitações. O objeto `ProxyError` é acessado por meio da propriedade [context.LastError](api-management-policy-expressions.md#ContextVariables) e pode ser usado por políticas na seção da política `on-error`. Este artigo fornece uma referência para os recursos de tratamento de erro no Gerenciamento de API do Azure.

## <a name="error-handling-in-api-management"></a>Tratamento de erro no Gerenciamento de API

As políticas no Gerenciamento de API do Azure são divididas nas seções `inbound`, `backend`, `outbound` e `on-error` conforme mostrado no exemplo a seguir.

```xml
<policies>
  <inbound>
    <!-- statements to be applied to the request go here -->
  </inbound>
  <backend>
    <!-- statements to be applied before the request is
         forwarded to the backend service go here -->
    </backend>
    <outbound>
      <!-- statements to be applied to the response go here -->
    </outbound>
    <on-error>
        <!-- statements to be applied if there is an error
             condition go here -->
  </on-error>
</policies>
```

Durante o processamento de uma solicitação, as etapas internas são executadas junto com as políticas, que estão no escopo da solicitação. Se ocorre um erro, o processamento imediatamente salta para a seção da política `on-error`.
A seção de política `on-error` pode ser usada em qualquer escopo. Os editores de API podem configurar um comportamento personalizado, como registrar o erro para hubs de eventos ou criar uma nova resposta para retornar ao chamador.

> [!NOTE]
> A seção `on-error` não está presente nas políticas por padrão. Para adicionar a seção `on-error` a uma política, navegue até a política desejada no editor de políticas e adicione-a. Para informações sobre como configurar políticas, consulte [Políticas do Gerenciamento de API](./api-management-howto-policies.md).
>
> Se não houver nenhuma seção `on-error`, os chamadores receberão mensagens de resposta HTTP 400 ou 500 se ocorrer uma condição de erro.

### <a name="policies-allowed-in-on-error"></a>Políticas permitidas em caso de erro

As políticas a seguir podem ser usadas na seção da política `on-error`.

-   [escolha](api-management-advanced-policies.md#choose)
-   [definir variável](api-management-advanced-policies.md#set-variable)
-   [find-and-replace](api-management-transformation-policies.md#Findandreplacestringinbody)
-   [return-response](api-management-advanced-policies.md#ReturnResponse)
-   [set-header](api-management-transformation-policies.md#SetHTTPheader)
-   [set-method](api-management-advanced-policies.md#SetRequestMethod)
-   [set-status](api-management-advanced-policies.md#SetStatus)
-   [send-request](api-management-advanced-policies.md#SendRequest)
-   [solicitação Send-unidirecional](api-management-advanced-policies.md#SendOneWayRequest)
-   [log-to-eventhub](api-management-advanced-policies.md#log-to-eventhub)
-   [json-to-xml](api-management-transformation-policies.md#ConvertJSONtoXML)
-   [xml-to-json](api-management-transformation-policies.md#ConvertXMLtoJSON)
-   [limit-concurrency](api-management-advanced-policies.md#LimitConcurrency)
-   [mock-response](api-management-advanced-policies.md#mock-response)
-   [Repita](api-management-advanced-policies.md#Retry)
-   [rastreou](api-management-advanced-policies.md#Trace)

## <a name="lasterror"></a>lastError

Quando ocorre um erro e o controle salta para a `on-error` seção de política, o erro é armazenado no [contexto. Propriedade LastError](api-management-policy-expressions.md#ContextVariables) , que pode ser acessada por políticas na `on-error` seção. LastError tem as propriedades a seguir.

| Nome       | Type   | Descrição                                                                                               | Obrigatório |
| ---------- | ------ | --------------------------------------------------------------------------------------------------------- | -------- |
| `Source`   | string | Indica o elemento em que ocorreu o erro. Pode ser a política ou um nome de etapa de pipeline interno.      | Sim      |
| `Reason`   | string | Código de erro amigável para computadores, que pode ser usado no tratamento de erro.                                       | Não       |
| `Message`  | string | Descrição de erro legível por humanos.                                                                         | Sim      |
| `Scope`    | string | Nome do escopo em que ocorreu o erro e pode ser um dos "global", "produto", "api" ou "operação" | Não       |
| `Section`  | string | Nome da seção em que ocorreu o erro. Valores possíveis: "inbound", "backend", "outbound" ou "on-error".      | Não       |
| `Path`     | string | Especifica a política aninhada, por exemplo "choose[3]/when[2]".                                                 | Não       |
| `PolicyId` | string | O valor do atributo `id`, se especificado pelo cliente, na política em que ocorreu o erro             | Não       |

> [!TIP]
> É possível acessar o código de status por meio de context.Response.StatusCode.

> [!NOTE]
> Todas as políticas têm um atributo `id` opcional que pode ser adicionado ao elemento raiz da política. Se esse atributo estiver presente em uma política quando ocorrer uma condição de erro, o valor do atributo poderá ser recuperado usando a propriedade `context.LastError.PolicyId`.

## <a name="predefined-errors-for-built-in-steps"></a>Erros predefinidos para etapas internas

Os erros a seguir são predefinidos para condições de erro que podem ocorrer durante a avaliação das etapas de processamento interno.

| Origem        | Condição                                 | Motivo                  | Mensagem                                                                                                                |
| ------------- | ----------------------------------------- | ----------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| configuração | O Uri não corresponde a nenhuma API ou Operação | OperationNotFound       | Não é possível corresponder a solicitação recebida a uma operação.                                                                      |
| autorização | Chave de assinatura não fornecida             | SubscriptionKeyNotFound | Acesso negado devido à ausência da chave de assinatura. Certifique-se de incluir a chave de assinatura ao fazer solicitações para esta API. |
| autorização | O valor chave e assinatura é inválido         | SubscriptionKeyInvalid  | Acesso negado devido à chave de assinatura inválida. Certifique-se de fornecer uma chave válida para uma assinatura ativa.            |
| vários | A conexão downstream (de um cliente para um gateway de gerenciamento de API) foi anulada pelo cliente enquanto a solicitação estava pendente | ClientConnectionFailure | vários |
| vários | A conexão upstream (de um gateway de gerenciamento de API para um serviço de back-end) não foi estabelecida ou foi anulada pelo back-end | BackendConnectionFailure | vários |
| vários | Ocorreu uma exceção de tempo de execução durante a avaliação de uma expressão específica | ExpressionValueEvaluationFailure | vários |

## <a name="predefined-errors-for-policies"></a>Erros predefinidos para políticas

Os erros a seguir são predefinidos para condições de erro que podem ocorrer durante a avaliação da política.

| Origem       | Condição                                                       | Motivo                    | Mensagem                                                                                                                              |
| ------------ | --------------------------------------------------------------- | ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| rate-limit   | Limite de taxa excedido                                             | RateLimitExceeded         | O limite da taxa foi excedido                                                                                                               |
| quota        | Cota excedida                                                  | QuotaExceeded             | Fora da cota do volume de chamada. A cota será reposta em xx:xx:xx. – ou – Sem cota de largura de banda. A cota será reposta em xx:xx:xx. |
| jsonp        | O valor do parâmetro de retorno de chamada é inválido (contém caracteres errados) | CallbackParameterInvalid  | O valor do parâmetro de retorno de chamada {callback-parameter-name} não é um identificador JavaScript válido.                                          |
| ip-filter    | Falha ao analisar o IP do chamador da solicitação                          | FailedToParseCallerIP     | Falha ao estabelecer o endereço IP para o chamador. Acesso negado                                                                        |
| ip-filter    | O IP do chamador não está na lista de permissões                                | CallerIpNotAllowed        | O endereço IP do chamador {ip-address} não é permitido. Acesso negado                                                                        |
| ip-filter    | O IP do chamador está na lista de bloqueios                                    | CallerIpBlocked           | O endereço IP do chamador está bloqueado. Acesso negado                                                                                         |
| check-header | O cabeçalho necessário não é apresentado ou o valor está ausente               | HeaderNotFound            | O cabeçalho {header-name} não foi encontrado na solicitação. Acesso negado                                                                    |
| check-header | O cabeçalho necessário não é apresentado ou o valor está ausente               | HeaderValueNotAllowed     | O valor do cabeçalho {header-name} de {header-value} não é permitido. Acesso negado                                                          |
| validate-jwt | O token Jwt está ausente na solicitação                                 | TokenNotFound             | JWT não encontrado na solicitação. Acesso negado                                                                                         |
| validate-jwt | Falha na validação da assinatura                                     | TokenSignatureInvalid     | <mensagem da biblioteca jwt\>. Acesso negado                                                                                          |
| validate-jwt | Público-alvo inválido                                                | TokenAudienceNotAllowed   | <mensagem da biblioteca jwt\>. Acesso negado                                                                                          |
| validate-jwt | Emissor inválido                                                  | TokenIssuerNotAllowed     | <mensagem da biblioteca jwt\>. Acesso negado                                                                                          |
| validate-jwt | Token expirado                                                   | TokenExpired              | <mensagem da biblioteca jwt\>. Acesso negado                                                                                          |
| validate-jwt | A chave de assinatura não foi resolvida pela ID                            | TokenSignatureKeyNotFound | <mensagem da biblioteca jwt\>. Acesso negado                                                                                          |
| validate-jwt | As declarações necessárias estão ausentes no token                          | TokenClaimNotFound        | O token JWT não tem as seguintes declarações: <c1\>, <c2\>, … Acesso negado                                                            |
| validate-jwt | Incompatibilidade de valores de declaração                                           | TokenClaimValueNotAllowed | O valor da declaração {claim-name} de {claim-value} não é permitido. Acesso negado                                                             |
| validate-jwt | Outras falhas de validação                                       | JwtInvalid                | <mensagem da biblioteca jwt\>                                                                                                          |
| solicitação de encaminhamento ou envio-solicitação | O código e os cabeçalhos de status de resposta HTTP não foram recebidos do back-end dentro do tempo limite configurado | Tempo limite | vários |

## <a name="example"></a>Exemplo

Configurando uma política de API para:

```xml
<policies>
    <inbound>
        <base />
    </inbound>
    <backend>
        <base />
    </backend>
    <outbound>
        <base />
    </outbound>
    <on-error>
        <set-header name="ErrorSource" exists-action="override">
            <value>@(context.LastError.Source)</value>
        </set-header>
        <set-header name="ErrorReason" exists-action="override">
            <value>@(context.LastError.Reason)</value>
        </set-header>
        <set-header name="ErrorMessage" exists-action="override">
            <value>@(context.LastError.Message)</value>
        </set-header>
        <set-header name="ErrorScope" exists-action="override">
            <value>@(context.LastError.Scope)</value>
        </set-header>
        <set-header name="ErrorSection" exists-action="override">
            <value>@(context.LastError.Section)</value>
        </set-header>
        <set-header name="ErrorPath" exists-action="override">
            <value>@(context.LastError.Path)</value>
        </set-header>
        <set-header name="ErrorPolicyId" exists-action="override">
            <value>@(context.LastError.PolicyId)</value>
        </set-header>
        <set-header name="ErrorStatusCode" exists-action="override">
            <value>@(context.Response.StatusCode.ToString())</value>
        </set-header>
        <base />
    </on-error>
</policies>
```

e enviar uma solicitação não autorizada resultará na resposta a seguir:

![Resposta de erro não autorizada](media/api-management-error-handling-policies/error-response.png)

## <a name="next-steps"></a>Próximas etapas

Para obter mais informações sobre como trabalhar com políticas, consulte:

-   [Políticas no Gerenciamento de API](api-management-howto-policies.md)
-   [Transformar APIs](transform-api.md)
-   [Referência de Política](./api-management-policies.md) para uma lista completa das instruções de política e suas configurações
-   [Exemplos de política](policy-samples.md)
