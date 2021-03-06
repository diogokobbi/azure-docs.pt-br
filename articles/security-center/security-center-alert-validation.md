---
title: Validação de alerta (arquivo de teste EICAR) na central de segurança do Azure | Microsoft Docs
description: Este documento ajuda a validar os alertas de segurança na Central de Segurança do Azure.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: f8f17a55-e672-4d86-8ba9-6c3ce2e71a57
ms.service: security-center
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/22/2020
ms.author: memildin
ms.openlocfilehash: 0c777c2461047f21e8fb413faec6d5f1140fc75f
ms.sourcegitcommit: 3792cf7efc12e357f0e3b65638ea7673651db6e1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/29/2020
ms.locfileid: "91438808"
---
# <a name="alert-validation-in-azure-security-center"></a>Validação de alerta na central de segurança do Azure
Este documento ensina você a verificar se o sistema está configurado corretamente para os alertas da Central de Segurança do Azure.

## <a name="what-are-security-alerts"></a>O que são alertas de segurança?
Alertas são as notificações que a Central de Segurança gera quando detecta ameaças em seus recursos. Ele prioriza e lista os alertas junto com as informações necessárias para investigar rapidamente o problema. A Central de Segurança também fornece recomendações sobre como corrigir um ataque.
Para obter mais informações, consulte [alertas de segurança na central de segurança](security-center-alerts-overview.md) e [Gerenciando e respondendo a alertas de segurança](security-center-managing-and-responding-alerts.md)

## <a name="alert-validation"></a>Validação de alerta

* [Windows](#validate-windows)
* [Linux](#validate-linux)
* [Kubernetes](#validate-kubernetes)

## <a name="validate-alerts-on-windows-vms"></a>Validar alertas em VMs do Windows <a name="validate-windows"></a>

Depois que o agente da central de segurança estiver instalado no computador, siga estas etapas do computador no qual você deseja ser o recurso atacado do alerta:

1. Copie um executável (por exemplo **calc.exe**) na área de trabalho do computador ou em outro diretório de sua conveniência e renomeie-o como **ASC_AlertTest_662jfi039N.exe**.
1. Abra o prompt de comando e execute esse arquivo com um argumento (apenas um nome de argumento falso), como: ```ASC_AlertTest_662jfi039N.exe -foo```
1. Aguarde 5 a 10 minutos e abra Alertas da Central de Segurança. Um alerta deve aparecer.

> [!NOTE]
> Ao revisar este alerta de teste para o Windows, verifique se a auditoria de argumentos de campo **habilitada** é **verdadeira**. Se for **false**, você precisará habilitar a auditoria de argumentos de linha de comando. Para habilitá-lo, use o seguinte comando:
>
>```reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\policies\system\Audit" /f /v "ProcessCreationIncludeCmdLine_Enabled"```

## <a name="validate-alerts-on-linux-vms"></a>Validar alertas em VMs do Linux <a name="validate-linux"></a>

Depois que o agente da central de segurança estiver instalado no computador, siga estas etapas do computador no qual você deseja ser o recurso atacado do alerta:
1. Copie um executável para um local conveniente e renomeie-o para **./asc_alerttest_662jfi039n**, por exemplo:

    ```cp /bin/echo ./asc_alerttest_662jfi039n```

1. Abra o prompt de comando e execute este arquivo:

    ```./asc_alerttest_662jfi039n testing eicar pipe```

1. Aguarde 5 a 10 minutos e abra Alertas da Central de Segurança. Um alerta deve aparecer.


## <a name="validate-alerts-on-kubernetes"></a>Validar alertas no kubernetes <a name="validate-kubernetes"></a>

Se você integrou o serviço kubernetes do Azure com a central de segurança, poderá testar se os alertas estão funcionando com o seguinte comando kubectl:

```kubectl get pods --namespace=asc-alerttest-662jfi039n```

Para obter mais informações sobre como defender seus nós e clusters do kubernetes, consulte [introdução ao Azure defender para kubernetes](defender-for-kubernetes-introduction.md)

## <a name="next-steps"></a>Próximas etapas
Este artigo apresentou a você o processo de validação de alertas. Agora que você está familiarizado com esse tipo de validação, experimente os seguintes artigos:

* [Validando Azure Key Vault detecção de ameaças na central de segurança do Azure](https://techcommunity.microsoft.com/t5/azure-security-center/validating-azure-key-vault-threat-detection-in-azure-security/ba-p/1220336)
* [Gerenciando e respondendo a alertas de segurança na central de segurança do Azure](security-center-managing-and-responding-alerts.md) -saiba como gerenciar alertas e responder a incidentes de segurança na central de segurança.
* [Monitoramento da integridade de segurança na Central de Segurança do Azure](security-center-monitoring.md): saiba como monitorar a integridade dos recursos do Azure.
* [Noções básicas sobre alertas de segurança na central de segurança do Azure](security-center-alerts-type.md) -saiba mais sobre os diferentes tipos de alertas de segurança.