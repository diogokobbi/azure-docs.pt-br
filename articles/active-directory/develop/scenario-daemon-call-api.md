---
title: Chamar uma API da Web de um aplicativo daemon-plataforma Microsoft Identity | Azure
description: Saiba como criar um aplicativo daemon que chama uma API da Web.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 10/30/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 576eaf2ad9350651e4400d980e6fedce236dfa57
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91257598"
---
# <a name="daemon-app-that-calls-web-apis---call-a-web-api-from-the-app"></a>Aplicativo daemon que chama APIs Web – chama uma API Web do aplicativo

Os aplicativos do daemon .NET podem chamar uma API da Web. Os aplicativos do daemon .NET também podem chamar várias APIs Web previamente aprovadas.

## <a name="calling-a-web-api-from-a-daemon-application"></a>Chamando uma API Web a partir de um aplicativo daemon

Veja como usar o token para chamar uma API:

# <a name="net"></a>[.NET](#tab/dotnet)

[!INCLUDE [Call web API in .NET](../../../includes/active-directory-develop-scenarios-call-apis-dotnet.md)]

# <a name="python"></a>[Python](#tab/python)

```Python
endpoint = "url to the API"
http_headers = {'Authorization': 'Bearer ' + result['access_token'],
                'Accept': 'application/json',
                'Content-Type': 'application/json'}
data = requests.get(endpoint, headers=http_headers, stream=False).json()
```

# <a name="java"></a>[Java](#tab/java)

```Java
HttpURLConnection conn = (HttpURLConnection) url.openConnection();

// Set the appropriate header fields in the request header.
conn.setRequestProperty("Authorization", "Bearer " + accessToken);
conn.setRequestProperty("Accept", "application/json");

String response = HttpClientHelper.getResponseStringFromConn(conn);

int responseCode = conn.getResponseCode();
if(responseCode != HttpURLConnection.HTTP_OK) {
    throw new IOException(response);
}

JSONObject responseObject = HttpClientHelper.processResponse(responseCode, response);
```

---

## <a name="calling-several-apis"></a>Chamando várias APIs

Para aplicativos daemon, as APIs da Web que você chama precisam ser aprovadas previamente. Não há nenhum consentimento incremental com aplicativos de daemon. (Não há interação do usuário.) O administrador de locatários precisa fornecer consentimento antecipadamente para o aplicativo e todas as permissões de API. Se você quiser chamar várias APIs, precisará adquirir um token para cada recurso, sempre chamando `AcquireTokenForClient` . MSAL usará o cache de token de aplicativo para evitar chamadas de serviço desnecessárias.

## <a name="next-steps"></a>Próximas etapas

# <a name="net"></a>[.NET](#tab/dotnet)

> [!div class="nextstepaction"]
> [Aplicativo de daemon – mover para produção](./scenario-daemon-production.md?tabs=dotnet)

# <a name="python"></a>[Python](#tab/python)

> [!div class="nextstepaction"]
> [Aplicativo de daemon – mover para produção](./scenario-daemon-production.md?tabs=python)

# <a name="java"></a>[Java](#tab/java)

> [!div class="nextstepaction"]
> [Aplicativo de daemon – mover para produção](./scenario-daemon-production.md?tabs=java)

---
