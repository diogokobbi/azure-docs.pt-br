---
title: Como preparar um aplicativo para implantação no Azure Spring Cloud
description: Saiba como preparar um aplicativo para implantação no Azure Spring Cloud.
author: bmitchell287
ms.service: spring-cloud
ms.topic: how-to
ms.date: 09/08/2020
ms.author: brendm
ms.custom: devx-track-java
zone_pivot_groups: programming-languages-spring-cloud
ms.openlocfilehash: ff0582e3c4f654ed2a7f5efdc9ce8fd7a226595a
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90906832"
---
# <a name="prepare-an-application-for-deployment-in-azure-spring-cloud"></a>Preparar um aplicativo para implantação no Azure Spring Cloud

::: zone pivot="programming-language-csharp"
O Azure Spring Cloud fornece serviços robustos para hospedar, monitorar, dimensionar e atualizar um aplicativo Steeltoe. Este artigo mostra como preparar um aplicativo Steeltoe existente para implantação no Azure Spring Cloud. 

Este artigo explica as dependências, a configuração e o código necessários para executar um aplicativo Steeltoe do .NET Core no Azure Spring Cloud. Para obter informações sobre como implantar um aplicativo no Azure Spring Cloud, consulte [implantar seu primeiro aplicativo do Azure Spring Cloud](spring-cloud-quickstart.md).

>[!Note]
> O suporte do Steeltoe para o Azure Spring Cloud é oferecido atualmente como uma visualização pública. As ofertas de visualização pública permitem que os clientes experimentem os novos recursos antes do lançamento oficial.  Os serviços e recursos de visualização pública não são destinados ao uso em produção.  Para obter mais informações sobre o suporte durante as visualizações, consulte as [perguntas frequentes](https://azure.microsoft.com/support/faq/) ou arquivo a [solicitação de suporte](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request).

##  <a name="supported-versions"></a>Versões com suporte

O Azure Spring Cloud dá suporte a:

* .NET Core 3.1
* Steeltoe 2,4

## <a name="dependencies"></a>Dependências

Instale o pacote [Microsoft. Azure. SpringCloud. Client](https://www.nuget.org/packages/Microsoft.Azure.SpringCloud.Client/) .

## <a name="update-programcs"></a>Atualizar Program.cs

No `Program.Main` método, chame o `UseAzureSpringCloudService` método:

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseStartup<Startup>();
        })
        .UseAzureSpringCloudService();
```

## <a name="enable-eureka-server-service-discovery"></a>Habilitar descoberta de serviço do servidor Eureka

Na fonte de configuração que será usada quando o aplicativo for executado no Azure Spring Cloud, defina `spring.application.name` com o mesmo nome que o aplicativo de nuvem Spring do Azure no qual o projeto será implantado.

Por exemplo, se você implantar um projeto do .NET chamado `EurekaDataProvider` para um aplicativo de nuvem Spring do Azure chamado `planet-weather-provider` o *appSettings.jsno* arquivo deverá incluir o seguinte JSON:

```json
"spring": {
  "application": {
    "name": "planet-weather-provider"
  }
}
```

## <a name="use-service-discovery"></a>Usar descoberta de serviço

Para chamar um serviço usando a descoberta de serviço do servidor Eureka, faça solicitações HTTP para `http://<app_name>` onde `app_name` é o valor do `spring.application.name` aplicativo de destino. Por exemplo, o código a seguir chama o `planet-weather-provider` serviço:

```csharp
using (var client = new HttpClient(discoveryHandler, false))
{
    var responses = await Task.WhenAll(
        client.GetAsync("http://planet-weather-provider/weatherforecast/mercury"),
        client.GetAsync("http://planet-weather-provider/weatherforecast/saturn"));
    var weathers = await Task.WhenAll(from res in responses select res.Content.ReadAsStringAsync());
    return new[]
    {
        new KeyValuePair<string, string>("Mercury", weathers[0]),
        new KeyValuePair<string, string>("Saturn", weathers[1]),
    };
}
```
::: zone-end

::: zone pivot="programming-language-java"
Este tópico mostra como preparar um aplicativo Java Spring existente para implantação no Azure Spring Cloud. Se configurado corretamente, o Azure Spring Cloud fornecerá serviços robustos para monitorar, dimensionar e atualizar seu aplicativo Java Spring Cloud.

Antes de executar este exemplo, experimente o [guia de início rápido básico](spring-cloud-quickstart.md).

Outros exemplos explicam como implantar um aplicativo no Azure Spring Cloud quando o arquivo POM é configurado. 
* [Iniciar seu primeiro aplicativo](spring-cloud-quickstart.md)
* [Criar e executar microsserviços](spring-cloud-quickstart-sample-app-introduction.md)

Este artigo explica as dependências necessárias e como adicioná-las ao arquivo POM.

## <a name="java-runtime-version"></a>Versão do Java Runtime

Somente aplicativos Spring/Java podem ser executados no Azure Spring Cloud.

O Azure Spring Cloud dá suporte ao Java 8 e ao Java 11. O ambiente de hospedagem contém a última versão do Azul Zulu OpenJDK para Azure. Para obter mais informações sobre o Azul Zulu OpenJDK para Azure, confira [Instalar o JDK](https://docs.microsoft.com/azure/developer/java/fundamentals/java-jdk-install).

## <a name="spring-boot-and-spring-cloud-versions"></a>Versões Spring Boot e Spring Cloud

Para preparar um aplicativo Spring boot existente para implantação no Azure Spring Cloud, inclua as dependências Spring Boot e Spring Cloud no arquivo POM do aplicativo, conforme mostrado nas seções a seguir.

O Azure Spring Cloud é compatível apenas com aplicativos Spring boot versão 2.1 ou 2.2. A seguinte tabela lista as combinações do Spring Boot e do Spring Cloud compatíveis:

Versão do Spring Boot | Versão do Spring Cloud
---|---
2.1 | Greenwich.RELEASE
2.2 | Hoxton.SR8
2.3 | Hoxton.SR8

### <a name="dependencies-for-spring-boot-version-21"></a>Dependências do Spring Boot versão 2.1

Para o Spring Boot versão 2.1, adicione as dependências a seguir ao arquivo POM do aplicativo.

```xml
    <!-- Spring Boot dependencies -->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.1.12.RELEASE</version>
    </parent>

    <!-- Spring Cloud dependencies -->
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>Greenwich.RELEASE</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>
```

### <a name="dependencies-for-spring-boot-version-22"></a>Dependências do Spring Boot versão 2.2

Para o Spring Boot versão 2.2, adicione as dependências a seguir ao arquivo POM do aplicativo.

```xml
    <!-- Spring Boot dependencies -->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.2.4.RELEASE</version>
    </parent>

    <!-- Spring Cloud dependencies -->
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>Hoxton.SR8</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>
```
### <a name="dependencies-for-spring-boot-version-23"></a>Dependências para o Spring boot versão 2,3

Para o Spring boot versão 2,3, adicione as seguintes dependências ao arquivo POM do aplicativo.

```xml
    <!-- Spring Boot dependencies -->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.3.0.RELEASE</version>
    </parent>

    <!-- Spring Cloud dependencies -->
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>Hoxton.SR8</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>
```
## <a name="azure-spring-cloud-client-dependency"></a>Dependência de cliente do Azure Spring Cloud

O Azure Spring Cloud hospeda e gerencia os componentes do Spring Cloud. Os componentes incluem o Registro de Serviço do Spring Cloud e o Servidor de Configuração do Spring Cloud. É recomendável usar o Spring boot 2,2 ou 2,3. Para o Spring boot 2,1, você precisará incluir a biblioteca de cliente do Azure Spring Cloud em suas dependências para permitir a comunicação com sua instância do serviço de nuvem do Azure Spring.

A tabela a seguir lista as versões corretas do Azure Spring Cloud para o aplicativo que usa o Spring Boot e o Spring Cloud.

Versão do Spring Boot | Versão do Spring Cloud | Versão inicial do cliente do Azure Spring Cloud
---|---|---
2.1.x | Greenwich.RELEASE | 2.1.2
2.2. x | Hoxton.SR8 | Não necessária
2.3. x | Hoxton.SR8 | Não necessária

Inclua o seguinte dependenciy no arquivo de pom.xml se você estiver usando o Spring boot 2,1.

```xml
<dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>spring-cloud-starter-azure-spring-cloud-client</artifactId>
        <version>2.1.2</version>
</dependency>
```

## <a name="other-recommended-dependencies-to-enable-azure-spring-cloud-features"></a>Outras dependências recomendadas para habilitar os recursos do Azure Spring Cloud

Para habilitar os recursos internos do Azure Spring Cloud do registro de serviço para o rastreamento distribuído, você também precisará incluir as seguintes dependências em seu aplicativo. Você poderá descartar algumas dessas dependências se não precisar de recursos correspondentes para os aplicativos específicos.

### <a name="service-registry"></a>Registro do Serviço

Para usar o serviço gerenciado Registro de Serviço do Azure, inclua a dependência `spring-cloud-starter-netflix-eureka-client` no arquivo pom.xml, conforme mostrado aqui:

```xml
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
    </dependency>
```

O ponto de extremidade do servidor do Registro de Serviço é injetado automaticamente como variáveis de ambiente com o aplicativo. Os aplicativos podem se registrar por conta própria no servidor do Registro de Serviço e descobrir outros microsserviços dependentes.

#### <a name="enablediscoveryclient-annotation"></a>Anotação EnableDiscoveryClient

Adicione a anotação a seguir ao código-fonte do aplicativo.
```java
@EnableDiscoveryClient
```
Por exemplo, confira o aplicativo piggymetrics de exemplos anteriores:
```java
package com.piggymetrics.gateway;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;
import org.springframework.cloud.netflix.zuul.EnableZuulProxy;

@SpringBootApplication
@EnableDiscoveryClient
@EnableZuulProxy

public class GatewayApplication {
    public static void main(String[] args) {
        SpringApplication.run(GatewayApplication.class, args);
    }
}
```

### <a name="distributed-configuration"></a>Configuração Distribuída

Para habilitar a Configuração Distribuída, inclua a seguinte dependência `spring-cloud-config-client` na seção de dependências do arquivo pom.xml:

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-config-client</artifactId>
</dependency>
```

> [!WARNING]
> Não especifique `spring.cloud.config.enabled=false` na configuração de inicialização. Caso contrário, o aplicativo deixará de funcionar com o Servidor de Configuração.

### <a name="metrics"></a>Métricas

Inclua a dependência `spring-boot-starter-actuator` na seção de dependências do arquivo pom.xml, conforme mostrado aqui:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

 As métricas são extraídas periodicamente dos pontos de extremidade JMX. Visualize as métricas usando o portal do Azure.

 > [!WARNING]
 > Especifique `spring.jmx.enabled=true` na sua propriedade de configuração. Caso contrário, as métricas não podem ser visualizadas em portal do Azure.

### <a name="distributed-tracing"></a>Rastreamento distribuído

Inclua as seguintes dependências `spring-cloud-starter-sleuth` e `spring-cloud-starter-zipkin` na seção de dependências do arquivo pom.xml:

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-sleuth</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-zipkin</artifactId>
</dependency>
```

 Você também precisa habilitar uma instância do Azure Application Insights para trabalhar com sua instância de serviço do Azure Spring Cloud. Para obter informações sobre como usar Application Insights com o Azure Spring Cloud, consulte a [documentação sobre rastreamento distribuído](spring-cloud-tutorial-distributed-tracing.md).

## <a name="see-also"></a>Confira também
* [Analisar as métricas e os logs do aplicativo](https://docs.microsoft.com/azure/spring-cloud/diagnostic-services)
* [Configurar seu Servidor de Configuração](https://docs.microsoft.com/azure/spring-cloud/spring-cloud-tutorial-config-server)
* [Usar o rastreamento distribuído com o Azure Spring Cloud](https://docs.microsoft.com/azure/spring-cloud/spring-cloud-tutorial-distributed-tracing)
* [Guia de Início Rápido do Spring](https://spring.io/quickstart)
* [Documentação do Spring Boot](https://spring.io/projects/spring-boot)

## <a name="next-steps"></a>Próximas etapas

Neste tópico, você aprendeu a configurar o aplicativo Spring Java para implantação no Azure Spring Cloud. Para saber como configurar uma instância de servidor de configuração, consulte [Configurar uma instância de servidor de configuração](spring-cloud-tutorial-config-server.md).

Mais exemplos estão disponíveis no GitHub: [Exemplos do Azure Spring Cloud](https://github.com/Azure-Samples/Azure-Spring-Cloud-Samples).
::: zone-end
