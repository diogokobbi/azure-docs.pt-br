---
title: Usar segredos de Azure Key Vault em atividades de pipeline
description: Saiba como buscar credenciais armazenadas do Azure Key Vault e usá-las durante data factory execuções de pipeline.
services: data-factory
author: ChrisLound
manager: anandsub
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 10/31/2019
ms.author: chlound
ms.openlocfilehash: 5a662119d9ccf95eac23785c5fe9a787da882531
ms.sourcegitcommit: f5580dd1d1799de15646e195f0120b9f9255617b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/29/2020
ms.locfileid: "91537388"
---
# <a name="use-azure-key-vault-secrets-in-pipeline-activities"></a>Usar segredos de Azure Key Vault em atividades de pipeline

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Você pode armazenar credenciais ou valores secretos em um Azure Key Vault e usá-los durante a execução do pipeline para passar para suas atividades.

## <a name="prerequisites"></a>Pré-requisitos

Esse recurso depende da identidade gerenciada data factory.  Saiba como ele funciona da [identidade gerenciada para data Factory](https://docs.microsoft.com/azure/data-factory/data-factory-service-identity) e certifique-se de que sua data Factory tenha uma associada.

## <a name="steps"></a>Etapas

1. Abra as propriedades do seu data factory e copie o valor da ID do aplicativo de identidade gerenciada.

    ![Valor de identidade gerenciada](media/how-to-use-azure-key-vault-secrets-pipeline-activities/managedidentity.png)

2. Abra as políticas de acesso do cofre de chaves e adicione as permissões de identidade gerenciadas para obter e listar segredos.

    ![Captura de tela que mostra a página "políticas de acesso" com a ação "Adicionar política de acesso" realçada.](media/how-to-use-azure-key-vault-secrets-pipeline-activities/akvaccesspolicies.png)

    ![Políticas de acesso ao cofre de chaves](media/how-to-use-azure-key-vault-secrets-pipeline-activities/akvaccesspolicies-2.png)

    Clique em **Adicionar**e em **salvar**.

3. Navegue até seu segredo de Key Vault e copie o identificador secreto.

    ![Identificador de segredo](media/how-to-use-azure-key-vault-secrets-pipeline-activities/secretidentifier.png)

    Anote seu URI secreto que você deseja obter durante a execução do pipeline de data factory.

4. Em seu pipeline de Data Factory, adicione uma nova atividade da Web e configure-a da seguinte maneira.  

    |Propriedade  |Valor  |
    |---------|---------|
    |Saída segura     |Verdadeiro         |
    |URL     |[Seu valor de URI secreto]? API-Version = 7.0         |
    |Método     |GET         |
    |Autenticação     |MSI         |
    |Recurso        |https://vault.azure.net       |

    ![Atividade da Web](media/how-to-use-azure-key-vault-secrets-pipeline-activities/webactivity.png)

    > [!IMPORTANT]
    > Você deve adicionar **? API-Version = 7.0** ao final do seu URI secreto.  

    > [!CAUTION]
    > Defina a opção de saída segura como true para impedir que o valor secreto seja registrado em texto sem formatação.  Qualquer outra atividade que consumir esse valor deve ter a opção de Entrada segura definida como verdadeira.

5. Para usar o valor em outra atividade, use a seguinte expressão de código ** @activity (' web1 '). Output. Value**.

    ![Expressão de código](media/how-to-use-azure-key-vault-secrets-pipeline-activities/usewebactivity.png)

## <a name="next-steps"></a>Próximas etapas

Para saber como usar Azure Key Vault para armazenar credenciais para armazenamentos de dados e computações, consulte [armazenar credenciais no Azure Key Vault](https://docs.microsoft.com/azure/data-factory/store-credentials-in-key-vault)
