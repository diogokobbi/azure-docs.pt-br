---
title: Requisitos de identidade para o design de identidade de nuvem híbrida do Azure | Microsoft Docs
description: Identifique as necessidades de negócios da empresa que levarão você definir os requisitos para o design de identidade híbrida.
documentationcenter: ''
services: active-directory
author: billmath
manager: daveba
editor: ''
ms.assetid: de690978-84ef-41ad-9dfe-785722d343a1
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/29/2019
ms.subservice: hybrid
ms.author: billmath
ms.custom: seohack1
ms.collection: M365-identity-device-management
ms.openlocfilehash: 074c203a0a5688855d4f7607a877b25ce6ee6ad7
ms.sourcegitcommit: f8d2ae6f91be1ab0bc91ee45c379811905185d07
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89660566"
---
# <a name="determine-identity-requirements-for-your-hybrid-identity-solution"></a>Determinar requisitos de identidade para sua solução de identidade híbrida
A primeira etapa na criação de uma solução de identidade híbrida é determinar os requisitos da organização de negócios que aproveitará essa solução.  A identidade híbrida começa como uma função de suporte (ela dá suporte para todas as outras soluções de nuvem fornecendo autenticação) e prossegue fornecendo recursos novos e interessantes que desbloqueiam novas cargas de trabalho para os usuários.  Essas cargas de trabalho ou os serviços que você deseja adotar para seus usuários determinarão os requisitos de design de identidade híbrida.  Esses serviços e cargas de trabalho precisam aproveitar a identidade híbrida no local e na nuvem.  

Você precisa abordar esses aspectos principais do negócio para entender o que é um requisito agora e quais são os planos da empresa para o futuro. Se você não tiver a visualização da estratégia de longo prazo para o design de identidade híbrida, a probabilidade é a de que sua solução não seja escalonável conforme o crescimento e a mudança nas necessidades dos negócios. O diagrama a seguir mostra um exemplo de uma arquitetura de identidade híbrida e as cargas de trabalho que estão sendo desbloqueadas para os usuários. Isso é apenas um exemplo de todos os recursos novos que podem ser desbloqueados e entregues com uma estratégia de identidade híbrida sólida. 

Alguns componentes que fazem parte da arquitetura de identidade híbrida da arquitetura de identidade híbrida ![](./media/plan-hybrid-identity-design-considerations/hybrid-identity-architechture.png)

## <a name="determine-business-needs"></a>Determinar as necessidades de negócios
Cada empresa terá requisitos diferentes, mesmo que essas empresas façam parte do mesmo setor; os requisitos reais dos negócios podem variar. Você ainda pode aproveitar as práticas recomendadas do setor, mas acaba sendo as necessidades da empresa que levarão você a definir os requisitos para o design de identidade híbrida. 

Não deixe de responder às seguintes perguntas para identificar suas necessidades de negócios:

* Sua empresa está procurando reduzir os custos operacionais de TI?
* Sua empresa está procurando proteger ativos de nuvem (aplicativos SaaS, infraestrutura)?
* Sua empresa está procurando modernizar sua TI?
  * Seus usuários são mais exigentes e exigem da TI que esta crie exceções na sua rede de perímetro para permitir outro tipo de tráfego a fim de acessar recursos diferentes?
  * Sua empresa tem aplicativos herdados que precisavam ser publicados para esses usuários modernos, mas não são fáceis de reescrever?
  * Sua empresa precisa realizar todas essas tarefas e colocá-las sob controle ao mesmo tempo?
* Sua empresa está procurando proteger as identidades dos usuários e reduzir riscos trazendo novas ferramentas que utilizam a experiência de segurança do Microsoft Azure no local?
* Sua empresa está tentando se livrar das temidas contas "externas" no local e movê-las para a nuvem onde não são mais uma ameaça inativa dentro do seu ambiente local?

## <a name="analyze-on-premises-identity-infrastructure"></a>Analisar a infraestrutura de identidades no local
Agora que você tem uma ideia sobre os requisitos de negócios da empresa, precisa avaliar sua infraestrutura de identidade no local. Essa avaliação é importante para definir os requisitos técnicos para integrar sua solução de identidade atual ao sistema de gerenciamento de identidade de nuvem. Certifique-se de responder às seguintes perguntas:

* Que solução de autenticação e autorização sua empresa usa no local? 
* Sua empresa possui atualmente algum serviço de sincronização local?
* Sua empresa usa qualquer IdP (provedor de identidade de terceiros)?

Você também precisa estar ciente dos serviços de nuvem que sua empresa pode ter. É muito importante executar uma avaliação para entender a integração atual com os modelos SaaS, IaaS ou PaaS em seu ambiente. Certifique-se de responder às seguintes perguntas durante essa avaliação:

* Sua empresa tem alguma integração com um provedor de serviços de nuvem?
* Em caso afirmativo, quais serviços estão sendo usados?
* Atualmente, essa integração está em produção ou é um piloto?

> [!NOTE]
> O Cloud Discovery analisa os logs de tráfego no catálogo de aplicativos de nuvem do Microsoft Cloud App Security de mais de 16.000 nuvem aplicativos que são classificados e pontuados com base nos fatores de risco mais de 70, para fornecer visibilidade contínua do nuvem uso, TI sombra e o risco Sombra poses IT em sua organização. Para começar, consulte [configurar o Cloud Discovery](/cloud-app-security/set-up-cloud-discovery).
> 
> 

## <a name="evaluate-identity-integration-requirements"></a>Avaliar os requisitos de integração de identidade
Em seguida, você precisa avaliar os requisitos de integração de identidade. Essa avaliação é importante para definir os requisitos técnicos de como os usuários serão autenticados, qual será a aparência da presença da organização na nuvem, como a organização permitirá a autorização e como será a experiência do usuário. Certifique-se de responder às seguintes perguntas:

* Sua organização usará federação, autenticação padrão ou ambos?
* A federação é um requisito?  Por causa do seguinte:
  * SSO baseado em Kerberos
  * Sua empresa tem um aplicativo local (seja criado internamente ou de terceiros) que usa SAML ou recursos de federação semelhantes.
  * MFA usando cartões inteligentes. RSA SecurID, etc.
  * Regras de acesso para cliente que abordam as perguntas a seguir:
    1. Posso bloquear todo o acesso externo a Microsoft 365 com base no endereço IP do cliente?
    2. Posso bloquear todo o acesso externo a Microsoft 365, exceto o Exchange ActiveSync?
    3. Posso bloquear todo o acesso externo a Microsoft 365, exceto para aplicativos baseados em navegador (OWA, SPO)
    4. Posso bloquear todo o acesso externo a Microsoft 365 para membros de grupos do AD designados
* Preocupações de auditoria/segurança
* Investimento já existente na autenticação federada
* Qual nome a nossa organização usará para nosso domínio na nuvem?
* A organização tem um domínio personalizado?
  1. Esse domínio é público e verificável facilmente usando DNS?
  2. Se não for, você tem um domínio público que possa ser usado para registrar um UPN alternativo no AD?
* Os identificadores de usuário são consistentes para a representação de nuvem? 
* A empresa possui aplicativos que requerem integração com serviços de nuvem?
* A organização possui vários domínios e todos eles usarão a autenticação federada ou padrão?

## <a name="evaluate-applications-that-run-in-your-environment"></a>Avaliar aplicativos executados em seu ambiente
Agora que você tem uma ideia sobre suas instalações e infraestrutura de nuvem, precisa avaliar os aplicativos executados nesses ambientes. Essa avaliação é importante para definir os requisitos técnicos para integrar esses aplicativos ao sistema de gerenciamento de identidades na nuvem. Certifique-se de responder às seguintes perguntas:

* Onde nossos aplicativos residirão?
* Os usuários acessarão os aplicativos no local?  Na nuvem? Ou ambos?
* Há planos para levar as cargas de trabalho de aplicativo existentes e movê-las para a nuvem?
* Há planos para desenvolver novos aplicativos que residirão no local ou na nuvem que usará autenticação de nuvem?

## <a name="evaluate-user-requirements"></a>Avaliar os requisitos do usuário
Você também precisa avaliar os requisitos do usuário. Essa avaliação é importante para definir as etapas que serão necessárias para a integração e para ajudar os usuários com a transição para a nuvem. Certifique-se de responder às seguintes perguntas:

* Os usuários acessarão aplicativos no local?
* Os usuários acessarão aplicativos na nuvem?
* Como os usuários normalmente entram em seu ambiente local?
* Como os usuários entrarão na nuvem?

> [!NOTE]
> Faça anotações de cada resposta e entenda o raciocínio por trás da resposta. [Determinar os requisitos de resposta a incidentes](plan-hybrid-identity-design-considerations-incident-response-requirements.md) apresentará as opções disponíveis e os prós e contras de cada opção.  Ao responder essas perguntas, você selecionará a opção que melhor se ajusta às necessidades da sua empresa.
> 
> 

## <a name="next-steps"></a>Próximas etapas
[Determinar os requisitos de sincronização de diretório](plan-hybrid-identity-design-considerations-directory-sync-requirements.md)

## <a name="see-also"></a>Confira também
[Visão geral sobre as considerações de design](plan-hybrid-identity-design-considerations-overview.md)

