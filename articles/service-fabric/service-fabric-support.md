---
title: Saiba mais sobre as opções de suporte do Azure Service Fabric
description: Versões de cluster do Azure Service Fabric com suporte e links para transmitir tíquetes de suporte
author: pkcsf
ms.topic: troubleshooting
ms.date: 8/24/2018
ms.author: pkc
ms.openlocfilehash: ae49a59c2629d9f9461d298ada555d314c0c9f22
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/11/2020
ms.locfileid: "86256961"
---
# <a name="azure-service-fabric-support-options"></a>Opções de suporte do Azure Service Fabric

Para fornecer o suporte apropriado para os clusters do Service Fabric nos quais suas cargas de trabalho do aplicativo são executadas, organizamos várias opções para você. Dependendo do nível de suporte necessário e da severidade do problema, escolha as opções ideais. 

## <a name="report-production-issues-or-request-paid-support-for-azure"></a>Relatar problemas de produção ou solicitar suporte pago para o Azure

Para relatar problemas no seu cluster do Service Fabric implantado no Azure, abra um tíquete de suporte [no portal do Azure](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) ou no [portal de suporte da Microsoft](https://support.microsoft.com/oas/default.aspx?prid=16146).

Saiba mais sobre:
 
- [Suporte da Microsoft para o Azure](https://azure.microsoft.com/support/plans/?b=16.44).
- [Suporte premier da Microsoft](https://support.microsoft.com/en-us/premier).

> [!Note]
> Clusters em execução em uma camada de confiabilidade bronze ou em cluster de nó único permitirão executar apenas cargas de trabalho de teste. No caso de problemas com um cluster em execução em confiabilidade bronze ou cluster de nó único, a equipe de suporte da Microsoft ajudará você a mitigar o problema, mas não executará uma análise da causa raiz. Confira [as características de confiabilidade do cluster](./service-fabric-cluster-capacity.md#reliability-characteristics-of-the-cluster) para obter mais detalhes.
>
> Para saber mais sobre o que é necessário para um cluster pronto para produção, confira a [lista de verificação de preparação para produção](./service-fabric-production-readiness-checklist.md).

<a id="getlivesitesupportonprem"></a>

## <a name="report-production-issues-or-request-paid-support-for-standalone-service-fabric-clusters"></a>Relatar problemas de produção ou solicitar o suporte pago para clusters independentes do Service Fabric

Para relatar problemas no seu cluster do Service Fabric implantado localmente ou em outras nuvens, abra um tíquete para obter suporte profissional no [portal de suporte da Microsoft](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview).

Saiba mais sobre:

- [Suporte Profissional da Microsoft para o local](https://support.microsoft.com/en-us/gp/offerprophone?wa=wsignin1.0).
- [Suporte premier da Microsoft](https://support.microsoft.com/en-us/premier).

## <a name="report-azure-service-fabric-issues"></a>Relatar problemas do Azure Service Fabric

Organizamos um repositório GitHub para relatar problemas do Service Fabric.  Também estamos monitorando de forma ativa os fóruns a seguir.

### <a name="github-repo"></a>Repositório GitHub 

Relate problemas do Azure Service Fabric no [repositório Git de problemas do Service Fabric](https://github.com/Azure/service-fabric-issues). Esse repositório destina-se a relatar e acompanhar problemas com o Azure Service Fabric e a ser usado para fazer pequenas solicitações de recursos. **Não use o repositório para relatar problemas de site ativo**.

### <a name="stackoverflow-and-msdn-forums"></a>StackOverflow e fóruns do MSDN

A [marcação Service Fabric no StackOverflow][stackoverflow] e o [fórum do Service Fabric no MSDN][msdn-forum] devem ser usados para fazer perguntas sobre o funcionamento da plataforma e como determinadas tarefas podem ser realizadas com ela.

### <a name="azure-feedback-forum"></a>Fórum de Comentários do Azure

O [Fórum de Comentários do Azure sobre o Service Fabric][uservoice-forum] é o melhor local para enviar grandes ideias de recursos para o produto, pois examinamos as solicitações mais populares como parte de nosso planejamento de médio a longo prazo. Incentivamos você a conseguir suporte para suas sugestões na comunidade.

## <a name="service-fabric-preview-versions---unsupported-for-production-use"></a>Versões prévias do Service Fabric – sem suporte para uso em produção

De tempos em tempos, lançamos versões com recursos significativos sobre os quais queremos comentários e que são lançadas como versões prévias. Essas versões prévias devem ser usadas apenas para fins de teste. O cluster de produção sempre deve estar executando uma versão do Service Fabric estável e com suporte. Uma versão prévia sempre começa com um número de versão principal e secundária de 255. Por exemplo, se você vir uma versão 255.255.5703.949 do Service Fabric, essa versão só deverá ser usada em clusters de teste e é uma versão prévia. Esses lançamentos de versões prévias também serão comunicados no [Blog da equipe do Service Fabric](https://techcommunity.microsoft.com/t5/azure-service-fabric/bg-p/Service-Fabric) e terão detalhes sobre os recursos incluídos.
Não há nenhuma opção de suporte pago para essas versões prévias. Use uma das opções listadas em [Relatar problemas do Azure Service Fabric](#report-azure-service-fabric-issues) para fazer perguntas ou fornecer comentários.

## <a name="next-steps"></a>Próximas etapas

[Versões do Service Fabric com suporte](service-fabric-versions.md)

<!--references-->
[Microsoft Q&A question page]: /answers/topics/azure-service-fabric.html
[stackoverflow]: https://stackoverflow.com/questions/tagged/azure-service-fabric
[uservoice-forum]: https://feedback.azure.com/forums/293901-service-fabric
[acom-docs]: https://aka.ms/servicefabricdocs
[sample-repos]: https://aka.ms/servicefabricsamples
[msdn-forum]: https://social.msdn.microsoft.com/forums/azure/en-US/home?category=windowsazureplatform
