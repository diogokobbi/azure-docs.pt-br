---
title: Monitorar seu dispositivo pro Edge Azure Stack | Microsoft Docs
description: Descreve como usar o portal do Azure e a interface do usuário da Web local para monitorar seu Azure Stack Edge pro.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 04/15/2019
ms.author: alkohli
ms.openlocfilehash: cd75eb0f7de602979f2233a873c01ef742471e37
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90904407"
---
# <a name="monitor-your-azure-stack-edge-pro"></a>Monitore seu Azure Stack Edge pro

Este artigo descreve como monitorar seu Azure Stack Edge pro. Para monitorar seu dispositivo, use o portal do Azure ou a IU da Web local. Use o portal do Azure para exibir eventos do dispositivo, configurar e gerenciar alertas e exibir métricas. Use a IU da Web local em seu dispositivo físico para exibir o status de hardware dos vários componentes do dispositivo.

Neste artigo, você aprenderá como:

> [!div class="checklist"]
>
> * Exibir eventos do dispositivo e os alertas correspondentes
> * Exibir o status de hardware dos componentes do dispositivo
> * Exibir métricas de capacidade e transação do dispositivo
> * Configurar e gerenciar alertas

## <a name="view-device-events"></a>Exibir eventos do dispositivo

[!INCLUDE [Supported OS for clients connected to device](../../includes/data-box-edge-gateway-view-device-events.md)]

## <a name="view-hardware-status"></a>Exibir o status de hardware

Execute as etapas a seguir na IU da Web local para exibir o status de hardware dos componentes do dispositivo.

1. Conecte-se à IU da Web local do dispositivo.
2. Acesse **Manutenção > Status do hardware**. Exiba a integridade dos vários componentes do dispositivo.

    ![Exibir o status de hardware](media/azure-stack-edge-monitor/view-hardware-status.png)

## <a name="view-metrics"></a>Métricas de exibição

[!INCLUDE [Supported OS for clients connected to device](../../includes/data-box-edge-gateway-view-metrics.md)]

### <a name="metrics-on-your-device"></a>Métricas em seu dispositivo

Esta seção descreve as métricas de monitoramento em seu dispositivo. As métricas podem ser:

* Métricas de capacidade. As métricas de capacidade estão relacionadas à capacidade do dispositivo.

* Métricas de transação. As métricas de transação estão relacionadas a operações de leitura e gravação no Armazenamento do Azure.

* Métricas de computação de borda. As métricas de computação de borda estão relacionadas ao uso da computação de borda em seu dispositivo.

Uma lista completa das métricas é mostrada na tabela a seguir:

|Métricas de capacidade                     |Descrição  |
|-------------------------------------|-------------|
|**Capacidade disponível**               | Refere-se ao tamanho dos dados que podem ser gravados no dispositivo. Em outras palavras, essa é a capacidade que pode ser disponibilizada no dispositivo. <br></br>Libere a capacidade do dispositivo excluindo a cópia local dos arquivos que têm uma cópia no dispositivo, bem como na nuvem.        |
|**Capacidade total**                   | Refere-se ao total de bytes no dispositivo no qual os dados serão gravados. Isso também é chamado de tamanho total do cache local. <br></br> Agora você pode aumentar a capacidade de um dispositivo virtual existente adicionando um disco de dados. Adicione um disco de dados por meio do gerenciamento do hipervisor à VM e, em seguida, reinicie a VM. O pool de armazenamento local do dispositivo de gateway será expandido para acomodar o disco de dados recém-adicionado. <br></br>Para obter mais informações, acesse [Adicionar um disco rígido a uma máquina virtual do Hyper-V](https://www.youtube.com/watch?v=EWdqUw9tTe4). |

|Métricas de transação              | Descrição         |
|-------------------------------------|---------|
|**Bytes carregados na nuvem (dispositivo)**    | Soma de todos os bytes carregados em todos os compartilhamentos no dispositivo        |
|**Bytes carregados na nuvem (compartilhamento)**     | Bytes carregados por compartilhamento. Isso pode ser: <br></br> Méd., que é a (Soma de todos os bytes carregados por compartilhamento/Número de compartilhamentos)  <br></br>Máx., que é o número máximo de bytes carregados de um compartilhamento <br></br>Mín., que é o número mínimo de bytes carregados de um compartilhamento      |
|**Taxa de transferência de downloads na nuvem (compartilhamento)**| Bytes baixados por compartilhamento. Isso pode ser: <br></br> Méd., que é a (Soma de todos os bytes lidos ou baixados em um compartilhamento/Número de compartilhamentos) <br></br> Máx., que é o número máximo de bytes baixados de um compartilhamento<br></br> e Mín., que é o número mínimo de bytes baixados de um compartilhamento  |
|**Taxa de transferência de leituras na nuvem**            | Soma de todos os bytes lidos na nuvem em todos os compartilhamentos no dispositivo     |
|**Taxa de transferência de uploads na nuvem**          | Soma de todos os bytes gravados na nuvem em todos os compartilhamentos no dispositivo     |
|**Taxa de transferência de uploads na nuvem (compartilhamento)**  | Soma de todos os bytes gravados na nuvem de um compartilhamento/nº de compartilhamentos é média, máx. e mín. por compartilhamento      |
|**Taxa de transferência da leituras (rede)**           | Inclui a taxa de transferência de rede do sistema para todos os bytes lidos na nuvem. Essa exibição pode incluir dados que não são restritos aos compartilhamentos. <br></br>A separação mostrará o tráfego em todos os adaptadores de rede no dispositivo. Isso inclui adaptadores que não estão conectados nem habilitados.      |
|**Taxa de transferência de gravações (rede)**       | Inclui a taxa de transferência de rede do sistema para todos os bytes gravados na nuvem. Essa exibição pode incluir dados que não são restritos aos compartilhamentos. <br></br>A separação mostrará o tráfego em todos os adaptadores de rede no dispositivo. Isso inclui adaptadores que não estão conectados nem habilitados.          |

| Métricas de computação de borda              | Descrição         |
|-------------------------------------|---------|
|**Computação de borda – uso de memória**      |           |
|**Computação de borda – percentual de CPU**    |         |

## <a name="manage-alerts"></a>Gerenciar alertas

[!INCLUDE [Supported OS for clients connected to device](../../includes/data-box-edge-gateway-manage-alerts.md)]

## <a name="next-steps"></a>Próximas etapas

Saiba como [Gerenciar a largura de banda](azure-stack-edge-manage-bandwidth-schedules.md).
