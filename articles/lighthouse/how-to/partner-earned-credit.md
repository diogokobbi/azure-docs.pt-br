---
title: Vincular sua ID de parceiro para habilitar o crédito ganho de parceiro em recursos delegados
description: Saiba como associar sua ID de parceiro para receber o PEC (crédito ganho) do parceiro nos recursos do cliente que você gerencia por meio do Azure Lighthouse.
ms.date: 09/04/2020
ms.topic: how-to
ms.openlocfilehash: 0a9e7f51e90b38bad24eada5a665ca60bf43452f
ms.sourcegitcommit: de2750163a601aae0c28506ba32be067e0068c0c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/04/2020
ms.locfileid: "89493303"
---
# <a name="link-your-partner-id-to-enable-partner-earned-credit-on-delegated-resources"></a>Vincular sua ID de parceiro para habilitar o crédito ganho de parceiro em recursos delegados

Se você for membro da [Microsoft Partner Network](https://partner.microsoft.com/), poderá vincular sua ID de parceiro com as credenciais usadas para gerenciar recursos de cliente delegados. Isso permite que você controle seu impacto nos compromissos do cliente e receba [crédito ganho do parceiro para serviços gerenciados (PEC)](/partner-center/partner-earned-credit).

Se você integrar [clientes com ofertas de serviço gerenciado no Azure Marketplace](publish-managed-services-offers.md), esse link ocorrerá automaticamente, usando a ID de MPN associada à conta do Partner Center usada para publicar as ofertas. Nenhuma ação adicional é necessária para receber o PEC para esses clientes.

Se você integrar [clientes usando modelos de gerenciamento de recursos do Azure](onboard-customer.md), será necessário tomar medidas para criar esse link. Isso é feito [vinculando sua ID do MPN](../../cost-management-billing/manage/link-partner-id.md) a pelo menos uma conta de usuário no seu locatário de gerenciamento que tem acesso a cada uma de suas assinaturas integradas.

## <a name="associate-your-partner-id-when-you-onboard-new-customers"></a>Associe sua ID de parceiro ao integrar novos clientes

Ao integrar clientes por meio de modelos do Azure Resource Manager, use o processo a seguir para vincular sua ID de parceiro e habilitar o crédito ganho do parceiro. Você precisará saber sua [ID de parceiro do MPN](/partner-center/partner-center-account-setup#locate-your-mpn-id) para concluir estas etapas.

Para simplificar, é recomendável criar uma conta de entidade de serviço em seu locatário, associando-a à sua ID do MPN e, em seguida, concedendo a ela acesso a todos os clientes que você integrar com uma [função interna do Azure qualificada para o PEC](https://docs.microsoft.com/partner-center/azure-roles-perms-pec).

1. [Crie uma conta de entidade de serviço](../../active-directory/develop/howto-authenticate-service-principal-powershell.md) no seu locatário de gerenciamento. Para este exemplo, vamos nomear essa conta de automação do PEC de entidade de serviço.
1. Usando essa conta de entidade de serviço, [vincule à sua ID de parceiro](../../cost-management-billing/manage/link-partner-id.md#link-to-a-partner-id) em seu locatário de gerenciamento. Você só precisa fazer isso uma vez.
1. Ao integrar [um cliente usando modelos de Azure Resource Manager](onboard-customer.md), não se esqueça de incluir uma autorização que inclua a conta de automação do PEC como um usuário com uma [função interna do Azure qualificada para o PEC](https://docs.microsoft.com/partner-center/azure-roles-perms-pec).

Seguindo essas etapas, todos os locatários de clientes que você gerenciar serão associados à sua ID de parceiro, permitindo que você receba o PEC para esses clientes. A conta de automação do PEC não precisa autenticar ou executar nenhuma ação no locatário do cliente.

## <a name="add-your-partner-id-to-previously-onboarded-customers"></a>Adicione sua ID de parceiro aos clientes integrados anteriormente

Se você já tiver integrado um cliente, talvez não queira executar outra implantação para adicionar a entidade de serviço da conta de automação do PEC. Em vez disso, você pode associar a ID do MPN a uma conta de usuário que já tem acesso ao trabalho no locatário desse cliente. Verifique se a conta recebeu uma [função interna do Azure qualificada para o PEC](https://docs.microsoft.com/partner-center/azure-roles-perms-pec).

Depois que a conta tiver sido [vinculada à sua ID de parceiro](../../cost-management-billing/manage/link-partner-id.md#link-to-a-partner-id) no seu locatário de gerenciamento, você poderá receber o PEC para esse cliente.

## <a name="confirm-partner-earned-credit"></a>Confirmar crédito de parceiro ganho

Você pode [exibir os detalhes do PEC no portal do Azure](/partner-center/partner-earned-credit-explanation#azure-cost-management) e confirmar quais custos receberam o benefício do PEC.

Se você seguiu as etapas acima e não vê a associação, abra uma solicitação de suporte no portal do Azure.

## <a name="next-steps"></a>Próximas etapas

- Saiba mais sobre como unir o [Microsoft Partner Network](/partner-center/mpn-overview).
- Saiba [como o PEC é calculado e pago](/partner-center/partner-earned-credit-explanation).
- Integrar [clientes](onboard-customer.md) ao Azure Lighthouse.
