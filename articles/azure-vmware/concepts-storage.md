---
title: Conceitos-armazenamento
description: Saiba mais sobre os principais recursos de armazenamento nas nuvens privadas da solução Azure VMware.
ms.topic: conceptual
ms.date: 05/04/2020
ms.openlocfilehash: 02378040061080d3c9abbfafb26180c9d22e9073
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91316810"
---
#  <a name="azure-vmware-solution-storage-concepts"></a>Conceitos de armazenamento da solução VMware do Azure

As nuvens privadas da solução Azure VMware fornecem armazenamento nativo em todo o cluster com a vSAN do VMware. Todo o armazenamento local de cada host em um cluster é usado em um armazenamento de dados vSAN, e a criptografia em repouso está disponível e habilitada por padrão. Você pode usar os recursos de armazenamento do Azure para estender os recursos de armazenamento de suas nuvens privadas.

## <a name="vsan-clusters"></a>clusters vSAN

O armazenamento local em cada host de cluster é usado como parte de um repositório de armazenamento vSAN. Todos os diskgroups usam uma camada de cache NVMe de 1,6 TB com a capacidade bruta, por host, baseada em SSD de 15,4 TB. O tamanho da camada de capacidade bruta de um cluster é a capacidade por host vezes o número de hosts. Por exemplo, um cluster de quatro hosts fornecerá capacidade bruta de 61,6 TB na camada de capacidade vSAN.

O armazenamento local em hosts de cluster é usado no repositório de armazenamento vSAN em todo o cluster. Todos os repositórios de armazenamento são criados como parte de uma implantação de nuvem privada e estão disponíveis para uso imediato. O usuário cloudadmin e todos os usuários no grupo CloudAdmin podem gerenciar repositórios de armazenamento com estes privilégios de vSAN:
- Datastore.AllocateSpace
- Datastore.Browse
- Datastore.Config
- Datastore. Excluirfile
- Repositório de armazenamento. filemanagement
- Datastore. UpdateVirtualMachineMetadata

## <a name="data-at-rest-encryption"></a>Criptografia de dados em repouso

Por padrão, os armazenamentos de dados vSAN usam a criptografia em repouso. A solução de criptografia é baseada em KMS e dá suporte a operações de vCenter para gerenciamento de chaves. As chaves são armazenadas criptografadas, encapsuladas por uma chave mestra de Azure Key Vault baseada em HSM. Quando um host é removido de um cluster por qualquer motivo, os dados no SSDs são invalidados imediatamente.

## <a name="scaling"></a>Dimensionamento

A capacidade de armazenamento de cluster nativo é dimensionada adicionando hosts a um cluster. Para clusters que usam hosts, a capacidade bruta em todo o cluster é aumentada em 15,4 TB com cada host adicional. Os clusters criados com hosts GP têm sua capacidade bruta aumentada em 7,7 TB com cada host adicional. Em ambos os tipos de clusters, os hosts demoram cerca de 10 minutos para serem adicionados a um cluster. Consulte o [tutorial dimensionar nuvem privada][tutorial-scale-private-cloud] para obter instruções sobre como dimensionar clusters.

## <a name="azure-storage-integration"></a>Integração do armazenamento do Azure

Você pode usar os serviços de armazenamento do Azure em cargas de trabalho em execução na sua nuvem privada. Os serviços de armazenamento do Azure incluem contas de armazenamento, armazenamento de tabelas e armazenamento de BLOBs. A conexão de cargas de trabalho para os serviços de armazenamento do Azure não atravessa a Internet. Essa conectividade fornece segurança adicional e permite que você use serviços de armazenamento do Azure baseados em SLA em suas cargas de trabalho de nuvem privada.

## <a name="next-steps"></a>Próximas etapas

A próxima etapa é aprender sobre os [conceitos de identidade de nuvem privada][concepts-identity].

<!-- LINKS - external-->

<!-- LINKS - internal -->
[tutorial-scale-private-cloud]: ./tutorial-scale-private-cloud.md
[concepts-identity]: ./concepts-identity.md
