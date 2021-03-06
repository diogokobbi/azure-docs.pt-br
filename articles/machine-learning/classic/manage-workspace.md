---
title: 'ML Studio (clássico): Gerenciar espaços de trabalho – Azure'
description: Gerenciar o acesso a espaços de trabalho Azure Machine Learning Studio (clássico) e implantar e gerenciar serviços Web de API Machine Learning
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: how-to
author: likebupt
ms.author: keli19
ms.custom: previous-author=heatherbshapiro, previous-ms.author=hshapiro
ms.date: 02/27/2017
ms.openlocfilehash: 50607093dd71184740e972a3c8572630df609453
ms.sourcegitcommit: d95cab0514dd0956c13b9d64d98fdae2bc3569a0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91361132"
---
# <a name="manage-an-azure-machine-learning-studio-classic-workspace"></a>Gerenciar um espaço de trabalho Azure Machine Learning Studio (clássico)

**aplica-se a:** ![ Aplica-se a. ](../../../includes/media/aml-applies-to-skus/yes.png) Machine Learning Studio (clássico) não ![ se aplica a.](../../../includes/media/aml-applies-to-skus/no.png)[ Azure Machine Learning](../compare-azure-ml-to-studio-classic.md)  


> [!NOTE]
> Para obter informações sobre como gerenciar serviços Web no portal de serviços Web do Machine Learning, veja [Gerenciar um serviço Web usando o portal de serviços Web do Azure Machine Learning](manage-new-webservice.md).
> 
> 

Você pode gerenciar os espaços de trabalho do Machine Learning Studio (clássico) no portal do Azure.



## <a name="use-the-azure-portal"></a>Use o Portal do Azure

Para gerenciar um espaço de trabalho do Studio (clássico) no portal do Azure:

1. Entre no [Portal do Azure](https://portal.azure.com/) usando uma conta de administrador de assinatura do Azure.
2. Na caixa de pesquisa na parte superior da página, insira "espaços de trabalho do Machine Learning Studio (clássico)" e, em seguida, selecione os **espaços de trabalho do Machine Learning Studio (clássico)**.
3. Clique no workspace que você deseja gerenciar.

Além das informações de gerenciamento de recursos padrão e das opções disponíveis, você pode:

- Exibir **Propriedades** - essa página exibe as informações do workspace e dos recursos e você pode alterar o assinatura e o grupo de recursos aos quais esse workspace está conectado.
- **Ressincronizar as Chaves de Armazenamento** - o workspace mantém chaves para a conta de armazenamento. Se a conta de armazenamento alterar as chaves, clique em **Ressincronizar chaves** para sincronizar as chaves com o workspace.

Para gerenciar os serviços Web associados a este espaço de trabalho do estúdio (clássico), use o Machine Learning Portal de serviços Web. Consulte [Gerenciar um serviço Web usando o portal de Serviços Web do Azure Machine Learning](manage-new-webservice.md) para obter informações completas.

> [!NOTE]
> Para implantar ou gerenciar Novos serviços Web, você deverá ser atribuído a uma função de colaborador ou de administrador na assinatura na qual o serviço Web é implantado. Se você convidar outro usuário para um espaço de trabalho do Machine Learning Studio (clássico), deverá atribuí-lo a uma função de colaborador ou de administrador na assinatura para poder implantar ou gerenciar serviços Web. 
> 
>Para obter mais informações sobre como definir permissões de acesso, confira [Gerenciar o acesso usando o RBAC e o portal do Azure](../../role-based-access-control/role-assignments-portal.md).

## <a name="next-steps"></a>Próximas etapas
* Saiba mais sobre [implantar o Machine Learning com modelos do Azure Resource Manager](deploy-with-resource-manager-template.md). 
