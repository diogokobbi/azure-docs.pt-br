---
title: Migrar para o monitor de conexão (versão prévia) do Monitor de Desempenho de Rede
titleSuffix: Azure Network Watcher
description: Saiba como migrar para o monitor de conexão (versão prévia) do Monitor de Desempenho de Rede.
services: network-watcher
documentationcenter: na
author: vinynigam
ms.service: network-watcher
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/20/2020
ms.author: vinigam
ms.openlocfilehash: dcbb82c1315e6150ddcfadbb52b2976447329b87
ms.sourcegitcommit: bf1340bb706cf31bb002128e272b8322f37d53dd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/03/2020
ms.locfileid: "89441826"
---
# <a name="migrate-to-connection-monitor-preview-from-network-performance-monitor"></a>Migrar para o monitor de conexão (versão prévia) do Monitor de Desempenho de Rede

Você pode migrar testes de Monitor de Desempenho de Rede (NPM) para um monitor de conexão novo e aprimorado (versão prévia) com um único clique e sem tempo de inatividade. Para saber mais sobre os benefícios, consulte [Monitor de conexão (versão prévia)](https://docs.microsoft.com/azure/network-watcher/connection-monitor-preview).

>[!NOTE]
> Somente testes do monitor de conectividade de serviço podem ser migrados para o monitor de conexão (versão prévia).
>

## <a name="key-points-to-note"></a>Pontos-chave a serem observados

A migração ajuda a produzir os seguintes resultados:

* Os agentes locais e as configurações de firewall funcionam como estão. Nenhuma alteração é necessária. Os agentes de Log Analytics instalados em máquinas virtuais do Azure precisam ser substituídos pela extensão do observador de rede.
* Os testes existentes são mapeados para o monitor de conexão (visualização) > grupo de teste > formato de teste. Ao selecionar **Editar**, você pode exibir e modificar as propriedades do novo monitor de conexão (versão prévia), baixar um modelo para fazer alterações e enviar o modelo por meio de Azure Resource Manager.
* Os agentes enviam dados para o espaço de trabalho Log Analytics e as métricas.
* Monitoramento de dados:
   * **Dados em log Analytics**: antes da migração, os dados permanecem no espaço de trabalho no qual o NPM está configurado na tabela NetworkMonitoring. Após a migração, os dados vão para a tabela NetworkMonitoring e ConnectionMonitor_CL tabela no mesmo espaço de trabalho. Depois que os testes são desabilitados no NPM, os dados são armazenados apenas na tabela ConnectionMonitor_CL.
   * **Alertas, painéis e integrações baseados em log**: você deve editar manualmente as consultas com base na nova tabela de ConnectionMonitor_CL. Para recriar os alertas em métricas, consulte [monitoramento de conectividade de rede com o monitor de conexão (versão prévia)](https://docs.microsoft.com/azure/network-watcher/connection-monitor-preview#metrics-in-azure-monitor).
    
## <a name="prerequisites"></a>Pré-requisitos

* Verifique se o observador de rede está habilitado na sua assinatura e na região do espaço de trabalho Log Analytics.
* As máquinas virtuais do Azure com agentes de Log Analytics instalados devem ser habilitadas com a extensão do observador de rede.

## <a name="migrate-the-tests"></a>Migrar os testes

Para migrar os testes do Monitor de Desempenho de Rede para o monitor de conexão (versão prévia), faça o seguinte:

1. Em observador de rede, selecione **Monitor de conexão**e, em seguida, selecione a guia **migrar testes de NPM** . 

    ![Captura de tela mostrando o painel "migrar testes do NPM" no observador de rede | Monitor de conexão (versão prévia).](./media/connection-monitor-2-preview/migrate-npm-to-cm-preview.png)
    
1. Nas listas suspensas, selecione sua assinatura e espaço de trabalho e, em seguida, selecione o recurso NPM que você deseja migrar. No momento, você pode migrar testes somente do monitor de conectividade de serviço.  
1. Selecione **importar** para migrar os testes.

Após o início da migração, as seguintes alterações ocorrem: 
* Um novo recurso de monitor de conexão é criado.
   * Um monitor de conexão por região e assinatura é criado. Para testes com agentes locais, o novo nome do monitor de conexão é formatado como `<workspaceName>_"on-premises"` . Para testes com agentes do Azure, o novo nome do monitor de conexão é formatado como `<workspaceName>_<Azure_region_name>` .
   * Os dados de monitoramento agora são armazenados no mesmo espaço de trabalho Log Analytics no qual o NPM está habilitado, em uma nova tabela chamada Connectionmonitor_CL. 
   * O nome do teste é postergado como o nome do grupo de teste. A descrição do teste não é migrada.
   * Os pontos de extremidade de origem e destino são criados e usados no novo grupo de teste. Para agentes locais, os pontos de extremidade são formatados como `<workspaceName>_"endpoint"_<FQDN of on-premises machine>` . Para o Azure, se os testes de migração contiverem agentes que não estão em execução, você precisará habilitar os agentes e migrar novamente.
   * A porta de destino e o intervalo de investigação são movidos para uma configuração de teste chamada *TC_ \<testname> * e *TC_ \<testname> _AppThresholds*. O protocolo é definido com base nos valores de porta. Os limites de êxito e outras propriedades opcionais são deixados em branco.
* NPM não está desabilitado, portanto, os testes migrados podem continuar a enviar dados para as tabelas NetworkMonitoring e ConnectionMonitor_CL. Essa abordagem garante que os alertas baseados em log existentes e as integrações não sejam afetados.
* O monitor de conexão criado recentemente é visível no monitor de conexão (versão prévia).

Após a migração, certifique-se de:
* Desabilite manualmente os testes no NPM. Até fazer isso, você continuará a ser cobrado por eles. 
* Enquanto estiver desabilitando o NPM, recrie os alertas na tabela ConnectionMonitor_CL ou use métricas. 
* Migre todas as integrações externas para a tabela ConnectionMonitor_CL. Exemplos de integrações externas são painéis em Power BI e Grafana e integrações com sistemas SIEM (gerenciamento de eventos e informações de segurança).


## <a name="next-steps"></a>Próximas etapas

Para saber mais sobre o monitor de conexão (versão prévia), consulte:
* [Migrar do monitor de conexão para o monitor de conexão (versão prévia)](migrate-to-connection-monitor-preview-from-connection-monitor.md)
* [Criar monitor de conexão (versão prévia) usando o portal do Azure](https://docs.microsoft.com/azure/network-watcher/connection-monitor-preview-create-using-portal)
