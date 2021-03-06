---
title: Dimensionar e proteger rapidamente um aplicativo Web usando o Azure Front Door e o Firewall do Aplicativo Web do Azure (WAF) | Microsoft Docs
description: Este tutorial explicará como usar o Firewall do Aplicativo Web com seu Azure Front Door Service
services: frontdoor
documentationcenter: ''
author: duongau
ms.service: frontdoor
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/14/2020
ms.author: duau
ms.openlocfilehash: 1958481193b66c8cec2cb6a1ac6648a6900d70ac
ms.sourcegitcommit: 03662d76a816e98cfc85462cbe9705f6890ed638
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/15/2020
ms.locfileid: "90531195"
---
# <a name="tutorial-quickly-scale-and-protect-a-web-application-using-azure-front-door-and-azure-web-application-firewall-waf"></a>Tutorial: Dimensionar e proteger rapidamente um aplicativo Web usando o Azure Front Door e o Firewall do Aplicativo Web do Azure (WAF)

Nas últimas semanas, muitos aplicativos Web experimentaram um aumento rápido de tráfego relacionado à COVID-19. Além disso, esses aplicativos Web também estão observando um aumento no tráfego mal-intencionado, incluindo ataques de negação de serviço. Uma maneira eficaz de lidar com essas necessidades, escalar horizontalmente para aumentos de tráfego e proteger contra ataques, é configurar o Azure Front Door com o WAF do Azure como uma camada de aceleração, cache e segurança na frente do seu aplicativo Web. Este artigo fornece orientação sobre como obter rapidamente essa Azure Front Door com a configuração de WAF do Azure para qualquer aplicativo Web em execução dentro ou fora do Azure. 

Usaremos a CLI do Azure para configurar o WAF neste tutorial, mas todas essas etapas também têm suporte completo no portal do Azure, Azure PowerShell, ARM do Azure e nas APIs REST do Azure. 

Neste tutorial, você aprenderá como:
> [!div class="checklist"]
> - Criar um Front Door.
> - Criar uma política de WAF do Azure.
> - Configurar conjuntos de regras para a política de WAF.
> - Associar a política de WAF ao Front Door
> - Configurar domínio personalizado

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Pré-requisitos

As instruções neste blog usam a CLI (interface de linha de comando) do Azure. Exiba este guia de [introdução à CLI do Azure](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest).

*Dica: uma maneira fácil e rápida de começar a usar a CLI do Azure é com o [Bash no Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/quickstart)*

Verifique se a extensão de front door foi adicionada à sua CLI do Azure

```azurecli-interactive 
az extension add --name front-door
```

Observação: Para obter mais detalhes sobre os comandos listados abaixo, consulte a [referência de CLI do Azure para Front Door](https://docs.microsoft.com/cli/azure/ext/front-door/?view=azure-cli-latest).

## <a name="create-an-azure-front-door-afd-resource"></a>Criar um recurso do Azure Front Door (AFD)

```azurecli-interactive 
az network front-door create --backend-address <>  --accepted-protocols <> --name <> --resource-group <>
```

**--backend-address**: O endereço de back-end é o nome de domínio totalmente qualificado (FQDN) do aplicativo que você deseja proteger. Por exemplo, myapplication.contoso.com

**--accepted-protocols**: Os protocolos aceitos especificam quais protocolos você deseja que o AFD dê suporte para seu aplicativo Web. Um exemplo seria --accepted-protocols Http Https.

**--name**: Especifique um nome para o recurso do AFD

**--resource-group**: O grupo de recursos no qual você deseja posicionar este recurso do AFD.  Para saber mais sobre grupos de recursos, visite Gerenciar grupos de recursos no Azure

Na resposta que você obtiver ao executar com êxito esse comando, procure a chave “hostName” e anote o valor a ser usado em uma etapa posterior. O hostName é o nome DNS do recurso do AFD que você criou

## <a name="create-an-azure-waf-profile-to-use-with-azure-front-door-resources"></a>Criar um perfil de WAF do Azure para usar com os recursos do Azure Front Door

```azurecli-interactive 
az network front-door waf-policy create --name <>  --resource-group <>  --disabled false --mode Prevention
```

--name Especifique um nome para a política do WAF do Azure

--resource-group O grupo de recursos no qual você deseja posicionar este recurso de WAF. 

O código da CLI acima criará uma política de WAF que está habilitada e está no modo de prevenção. 

Observação: você também pode querer criar o WAF no modo de Detecção e observar como ele está detectando e registrando em log as solicitações mal-intencionadas (e não o bloqueio) antes de decidir alterar para o modo de Proteção.

Na resposta que você obtiver ao executar com êxito esse comando, procure a chave “ID” e anote o valor a ser usado em uma etapa posterior. O campo ID deve estar no formato

/subscriptions/**subscription id**/resourcegroups/**resource group name**/providers/Microsoft.Network/frontdoorwebapplicationfirewallpolicies/**WAF policy name**

## <a name="add-managed-rulesets-to-this-waf-policy"></a>Adicionar conjuntos de regras gerenciados a esta política de WAF

Em uma política de WAF, você pode adicionar conjuntos de regras gerenciados que são um conjunto de regras criadas e gerenciadas pela Microsoft e fornece proteção pronta para uso em classes inteiras de ameaças. Neste exemplo, estamos adicionando dois conjuntos de regras desse tipo: (1) um conjunto de regras padrão, que protege contra ameaças comuns da Web e (2) um conjunto de regras de proteção contra bots, que protege contra bots mal-intencionados

(1) Adicionar o conjunto de regras padrão

```azurecli-interactive 
az network front-door waf-policy managed-rules add --policy-name <> --resource-group <> --type DefaultRuleSet --version 1.0
```

(2) Adicionar o conjunto de regras do gerenciador de bots

```azurecli-interactive 
az network front-door waf-policy managed-rules add --policy-name <> --resource-group <> --type Microsoft_BotManagerRuleSet --version 1.0
```

--policy-name O nome que você deu para o recurso de WAF do Azure

--resource-group O grupo de recursos no qual você posicionou este recurso de WAF.

## <a name="associate-the-waf-policy-with-the-afd-resource"></a>Associar a política de WAF ao recurso do AFD

Nesta etapa, vamos associar a política de WAF que criamos com o recurso do AFD que está na frente do seu aplicativo Web.

```azurecli-interactive 
az network front-door update --name <> --resource-group <> --set frontendEndpoints[0].webApplicationFirewallPolicyLink='{"id":"<>"}'
```

--name O nome que você especificou para o recurso do AFD

--resource-group O grupo de recursos no qual você colocou o recurso do Azure Front Door.

--set É aqui que você atualiza o atributo WebApplicationFirewallPolicyLink para o frontendEndpoint associado ao recurso do AFD com a política de WAF criada recentemente. A ID da política de WAF pode ser encontrada a partir da resposta obtida na etapa nº 2 acima

Observação: o exemplo acima é para o caso de você não estar usando um domínio personalizado, se você estiver

Se você não estiver usando domínios personalizados para acessar seus aplicativos Web, poderá ignorar a etapa nº 5. Nesse caso, você estará fornecendo aos seus usuários finais o nome de host obtido na etapa nº 1 para navegar até seu aplicativo Web

## <a name="configure-custom-domain-for-your-web-application"></a>Configurar domínios personalizados para seu aplicativo Web

Inicialmente, o nome de domínio personalizado do seu aplicativo Web (aquele que os clientes usam para fazer referência ao seu aplicativo, por exemplo, www.contoso.com) estava apontando para o local onde você o estava executando antes de o AFD ser introduzido. Após essa alteração de arquitetura adicionando AFD + WAF para proteger o aplicativo, a entrada DNS correspondente a esse domínio personalizado agora deve apontar para este recurso do AFD. Isso pode ser feito remapeando essa entrada no servidor DNS para o nome de host do AFD que você anotou na etapa nº 1.

As etapas específicas para atualizar seus registros DNS dependerão do seu provedor de serviços DNS, mas se você estiver usando o DNS do Azure para hospedar seu nome DNS, consulte a documentação com as [etapas para atualizar um registro DNS](https://docs.microsoft.com/azure/dns/dns-operations-recordsets-cli) e apontar para o nome do host do AFD. 

Uma coisa importante a ser observada aqui é que, se você precisar que seus usuários naveguem até seu site usando o Apex da zona, por exemplo, contoso.com, será necessário usar o DNS do Azure e o [tipo de registro de ALIAS](https://docs.microsoft.com/azure/dns/dns-alias) para hospedar seu nome DNS. 

Além disso, você também precisa atualizar sua configuração do AFD para [adicionar esse domínio personalizado](https://docs.microsoft.com/azure/frontdoor/front-door-custom-domain) a ele para que o AFD entenda esse mapeamento.

Por fim, se você estiver usando um domínio personalizado para acessar seu aplicativo Web e quiser habilitar o protocolo HTTPS, você precisará ter os [certificados para a configuração de domínio personalizado no AFD](https://docs.microsoft.com/azure/frontdoor/front-door-custom-domain-https). 

## <a name="lock-down-your-web-application"></a>Bloquear seu aplicativo Web

Uma melhor prática opcional a ser seguida é garantir que apenas as bordas do AFD possam se comunicar com seu aplicativo Web. Essa ação garantirá que ninguém possa ignorar as proteções do AFD e acessar seus aplicativos diretamente. Você pode realizar esse bloqueio visitando a [seção de perguntas frequentes do AFD](https://docs.microsoft.com/azure/frontdoor/front-door-faq) e referindo-se à pergunta sobre o bloqueio de back-ends para acesso somente pelo AFD.

## <a name="clean-up-resources"></a>Limpar os recursos

Quando não precisar mais dos recursos neste tutorial, use o comando [az group delete](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest#az-group-delete) para remover o grupo de recursos, o Front Door e a política de WAF.

```azurecli-interactive
  az group delete \
    --name <>
```
--name O nome do grupo de recursos para todos os recursos implantados neste tutorial.

## <a name="next-steps"></a>Próximas etapas

Para saber como solucionar problemas do seu Front Door, prossiga para os guias de instruções.

> [!div class="nextstepaction"]
> [Solução de problemas comuns de roteamentos](front-door-troubleshoot-routing.md)
