---
title: Monitorar o desempenho em VMs do Azure-informações do Aplicativo Azure
description: Monitoramento do desempenho de aplicativos para a VM do Azure e conjuntos de dimensionamento de máquinas virtuais do Azure. Tempo de resposta e carregamento do gráfico, informações de dependência e definir alertas sobre o desempenho.
ms.topic: conceptual
ms.date: 08/26/2019
ms.openlocfilehash: 8b025c5196d65234a632bd1f939bc1116b72dce0
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87024625"
---
# <a name="deploy-the-azure-monitor-application-insights-agent-on-azure-virtual-machines-and-azure-virtual-machine-scale-sets"></a>Implantar o agente de Application Insights de Azure Monitor em máquinas virtuais do Azure e conjuntos de dimensionamento de máquinas virtuais do Azure

A habilitação do monitoramento em seus aplicativos Web baseados em .NET executados em [máquinas virtuais do Azure](https://azure.microsoft.com/services/virtual-machines/) e [conjuntos de dimensionamento de máquinas virtuais do Azure](../../virtual-machine-scale-sets/index.yml) agora está mais fácil do que nunca. Obtenha todos os benefícios do uso de Application Insights sem modificar seu código.

Este artigo orienta você pela habilitação do monitoramento de Application Insights usando o agente Application Insights e fornece diretrizes preliminares para automatizar o processo para implantações em larga escala.

> [!IMPORTANT]
> No momento, o agente de informações do Aplicativo Azure para .NET está em visualização pública.
> Esta versão de visualização é fornecida sem um contrato de nível de serviço e não é recomendável para cargas de trabalho de produção. Alguns recursos podem não ter suporte e alguns podem ter recursos restritos.
> Para obter mais informações, consulte [Termos de Uso Complementares de Versões Prévias do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="enable-application-insights"></a>Habilitar o Application Insights

Há duas maneiras de habilitar o monitoramento de aplicativos para máquinas virtuais do Azure e conjuntos de dimensionamento de máquinas virtuais do Azure aplicativos hospedados:

* Sem **código** por meio do agente de Application insights
    * Esse método é o mais fácil de habilitar e nenhuma configuração avançada é necessária. Ele é geralmente chamado de monitoramento de "tempo de execução".

    * Para máquinas virtuais do Azure e conjuntos de dimensionamento de máquinas virtuais do Azure, é recomendável habilitar, no mínimo, esse nível de monitoramento. Depois disso, com base em seu cenário específico, você pode avaliar se a instrumentação manual é necessária.

    * O agente de Application Insights coleta automaticamente os mesmos sinais de dependência prontos para uso que o SDK do .NET. Consulte [coleção de dependência automática](./auto-collect-dependencies.md#net) para saber mais.
        > [!NOTE]
        > Atualmente, há suporte apenas para aplicativos hospedados no IIS do .net. Use um SDK para instrumentar aplicativos ASP.NET Core, Java e Node.js hospedados em máquinas virtuais do Azure e conjuntos de dimensionamento de máquinas virtuais.

* **Baseado em código** via SDK

    * Essa abordagem é muito mais personalizável, mas requer [a adição de uma dependência nos pacotes NuGet do SDK Application insights](./asp-net.md). Esse método, também significa que você precisa gerenciar as atualizações para a versão mais recente dos pacotes por conta própria.

    * Se você precisar fazer chamadas à API personalizada para acompanhar eventos/dependências não capturadas por padrão com o monitoramento baseado em agente, você precisará usar esse método. Confira o [artigo API para eventos e métricas personalizados](./api-custom-events-metrics.md) para saber mais.

> [!NOTE]
> Se o monitoramento baseado em agente e a instrumentação baseada em SDK manual forem detectados, somente as configurações de instrumentação manual serão respeitadas. Isso é para evitar que dados duplicados sejam enviados. Para saber mais sobre isso, confira a [seção solução de problemas](#troubleshooting) abaixo.

## <a name="manage-application-insights-agent-for-net-applications-on-azure-virtual-machines-using-powershell"></a>Gerenciar o agente de Application Insights para aplicativos .NET em máquinas virtuais do Azure usando o PowerShell

> [!NOTE]
> Antes de instalar o agente de Application Insights, você precisará de uma cadeia de conexão. [Crie um novo recurso Application insights](./create-new-resource.md) ou copie a cadeia de conexão de um recurso existente do Application insights.

> [!NOTE]
> Novo no PowerShell? Confira o [Guia de introdução](/powershell/azure/get-started-azureps?view=azps-2.5.0).

Instalar ou atualizar o agente de Application Insights como uma extensão para máquinas virtuais do Azure
```powershell
$publicCfgJsonString = '
{
  "redfieldConfiguration": {
    "instrumentationKeyMap": {
      "filters": [
        {
          "appFilter": ".*",
          "machineFilter": ".*",
          "virtualPathFilter": ".*",
          "instrumentationSettings" : {
            "connectionString": "InstrumentationKey=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
          }
        }
      ]
    }
  }
}
';
$privateCfgJsonString = '{}';

Set-AzVMExtension -ResourceGroupName "<myVmResourceGroup>" -VMName "<myVmName>" -Location "<myVmLocation>" -Name "ApplicationMonitoring" -Publisher "Microsoft.Azure.Diagnostics" -Type "ApplicationMonitoringWindows" -Version "2.8" -SettingString $publicCfgJsonString -ProtectedSettingString $privateCfgJsonString
```

> [!NOTE]
> Você pode instalar ou atualizar o agente de Application Insights como uma extensão em várias máquinas virtuais em escala usando um loop do PowerShell.

Desinstalar a extensão do agente de Application Insights da máquina virtual do Azure
```powershell
Remove-AzVMExtension -ResourceGroupName "<myVmResourceGroup>" -VMName "<myVmName>" -Name "ApplicationMonitoring"
```

Consultar o status de extensão do agente Application Insights para a máquina virtual do Azure
```powershell
Get-AzVMExtension -ResourceGroupName "<myVmResourceGroup>" -VMName "<myVmName>" -Name ApplicationMonitoring -Status
```

Obter lista de extensões instaladas para a máquina virtual do Azure
```powershell
Get-AzResource -ResourceId "/subscriptions/<mySubscriptionId>/resourceGroups/<myVmResourceGroup>/providers/Microsoft.Compute/virtualMachines/<myVmName>/extensions"

# Name              : ApplicationMonitoring
# ResourceGroupName : <myVmResourceGroup>
# ResourceType      : Microsoft.Compute/virtualMachines/extensions
# Location          : southcentralus
# ResourceId        : /subscriptions/<mySubscriptionId>/resourceGroups/<myVmResourceGroup>/providers/Microsoft.Compute/virtualMachines/<myVmName>/extensions/ApplicationMonitoring
```
Você também pode exibir as extensões instaladas na [folha máquina virtual do Azure](../../virtual-machines/extensions/overview.md) no Portal.

> [!NOTE]
> Verifique a instalação clicando em Live Metrics Stream dentro do recurso de Application Insights associado à cadeia de conexão usada para implantar a extensão do agente de Application Insights. Se você estiver enviando dados de várias máquinas virtuais, selecione as máquinas virtuais do Azure de destino em nome do servidor. Pode levar até um minuto para que os dados comecem a fluir.

## <a name="manage-application-insights-agent-for-net-applications-on-azure-virtual-machine-scale-sets-using-powershell"></a>Gerenciar Application Insights agente para aplicativos .NET em conjuntos de dimensionamento de máquinas virtuais do Azure usando o PowerShell

Instalar ou atualizar o agente de Application Insights como uma extensão para o conjunto de dimensionamento de máquinas virtuais do Azure
```powershell
$publicCfgHashtable =
@{
  "redfieldConfiguration"= @{
    "instrumentationKeyMap"= @{
      "filters"= @(
        @{
          "appFilter"= ".*";
          "machineFilter"= ".*";
          "virtualPathFilter": ".*",
          "instrumentationSettings" : {
            "connectionString": "InstrumentationKey=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx" # Application Insights connection string, create new Application Insights resource if you don't have one. https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/microsoft.insights%2Fcomponents
          }
        }
      )
    }
  }
};
$privateCfgHashtable = @{};

$vmss = Get-AzVmss -ResourceGroupName "<myResourceGroup>" -VMScaleSetName "<myVmssName>"

Add-AzVmssExtension -VirtualMachineScaleSet $vmss -Name "ApplicationMonitoring" -Publisher "Microsoft.Azure.Diagnostics" -Type "ApplicationMonitoringWindows" -TypeHandlerVersion "2.8" -Setting $publicCfgHashtable -ProtectedSetting $privateCfgHashtable

Update-AzVmss -ResourceGroupName $vmss.ResourceGroupName -Name $vmss.Name -VirtualMachineScaleSet $vmss

# Note: depending on your update policy, you might need to run Update-AzVmssInstance for each instance
```

Desinstalar a extensão de monitoramento de aplicativos dos conjuntos de dimensionamento de máquinas virtuais do Azure
```powershell
$vmss = Get-AzVmss -ResourceGroupName "<myResourceGroup>" -VMScaleSetName "<myVmssName>"

Remove-AzVmssExtension -VirtualMachineScaleSet $vmss -Name "ApplicationMonitoring"

Update-AzVmss -ResourceGroupName $vmss.ResourceGroupName -Name $vmss.Name -VirtualMachineScaleSet $vmss

# Note: depending on your update policy, you might need to run Update-AzVmssInstance for each instance
```

Consultar o status da extensão de monitoramento de aplicativo para conjuntos de dimensionamento de máquinas virtuais do Azure
```powershell
# Not supported by extensions framework
```

Obter lista de extensões instaladas para conjuntos de dimensionamento de máquinas virtuais do Azure
```powershell
Get-AzResource -ResourceId /subscriptions/<mySubscriptionId>/resourceGroups/<myResourceGroup>/providers/Microsoft.Compute/virtualMachineScaleSets/<myVmssName>/extensions

# Name              : ApplicationMonitoringWindows
# ResourceGroupName : <myResourceGroup>
# ResourceType      : Microsoft.Compute/virtualMachineScaleSets/extensions
# Location          :
# ResourceId        : /subscriptions/<mySubscriptionId>/resourceGroups/<myResourceGroup>/providers/Microsoft.Compute/virtualMachineScaleSets/<myVmssName>/extensions/ApplicationMonitoringWindows
```

## <a name="troubleshooting"></a>Solução de problemas

Encontre dicas de solução de problemas para Application Insights extensão do agente de monitoramento para aplicativos .NET em execução em máquinas virtuais do Azure e conjuntos de dimensionamento de máquinas virtuais.

> [!NOTE]
> Os aplicativos .NET Core, Java e Node.js só têm suporte em máquinas virtuais do Azure e em conjuntos de dimensionamento de máquinas virtuais do Azure por meio de instrumentação manual baseada em SDK e, portanto, as etapas a seguir não se aplicam a esses cenários.

A saída de execução da extensão é registrada nos arquivos localizados nos seguintes diretórios:
```Windows
C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.ApplicationMonitoringWindows\<version>\
```

## <a name="next-steps"></a>Próximas etapas
* Saiba como [implantar um aplicativo em um conjunto de dimensionamento de máquinas virtuais do Azure](../../virtual-machine-scale-sets/virtual-machine-scale-sets-deploy-app.md).
* [Configure testes da Web de disponibilidade](monitor-web-app-availability.md) para serem alertados se o ponto de extremidade estiver inativo.
