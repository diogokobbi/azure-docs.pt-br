---
title: Planejando uma implantação de Sincronização de Arquivos do Azure | Microsoft Docs
description: Planejar uma implantação com o Sincronização de Arquivos do Azure, um serviço que permite armazenar em cache vários compartilhamentos de arquivos do Azure em um Windows Server ou VM de nuvem local.
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 01/15/2020
ms.author: rogarana
ms.subservice: files
ms.custom: references_regions
ms.openlocfilehash: 876a96f579bff8d30e454e927054a951734f44ba
ms.sourcegitcommit: bf1340bb706cf31bb002128e272b8322f37d53dd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/03/2020
ms.locfileid: "89441092"
---
# <a name="planning-for-an-azure-file-sync-deployment"></a>Planejando uma implantação da Sincronização de Arquivos do Azure

:::row:::
    :::column:::
        [![Entrevista e demonstração apresentando a Sincronização de Arquivos do Azure – clique para reproduzir!](./media/storage-sync-files-planning/azure-file-sync-interview-video-snapshot.png)](https://www.youtube.com/watch?v=nfWLO7F52-s)
    :::column-end:::
    :::column:::
        A Sincronização de Arquivos do Azure é um serviço que permite armazenar em cache vários compartilhamentos de arquivos do Azure em um Windows Server local ou uma VM na nuvem. 
        
        Este artigo apresenta conceitos e recursos da Sincronização de Arquivos do Azure. Quando estiver familiarizado com a Sincronização de Arquivos do Azure, siga o [guia de implantação da Sincronização de Arquivos do Azure](storage-sync-files-deployment-guide.md) para experimentar esse serviço.        
    :::column-end:::
:::row-end:::

Os arquivos serão armazenados na nuvem em [compartilhamentos de arquivos do Azure](storage-files-introduction.md). Os compartilhamentos de arquivos do Azure podem ser usados de duas maneiras: ao montar diretamente esses compartilhamentos de arquivos do Azure sem servidor (SMB) ou armazenar em cache os compartilhamentos de arquivos do Azure localmente usando a Sincronização de Arquivos do Azure. A opção de implantação escolhida altera os aspectos que você precisa considerar ao planejar sua implantação. 

- **Montagem direta de um compartilhamento de arquivo do Azure**: Como os Arquivos do Azure fornecem acesso SMB, você pode montar compartilhamentos de arquivo do Azure localmente ou na nuvem usando o cliente SMB padrão disponível no Windows, no macOS e no Linux. Como os compartilhamentos de arquivos do Azure são sem servidor, a implantação para cenários de produção não requer o gerenciamento de um servidor de arquivos nem de um dispositivo NAS. Isso significa que você não precisa aplicar patches de software nem trocar discos físicos. 

- **Armazenar em cache o compartilhamento de arquivo do Azure local com a Sincronização de Arquivos do Azure**: A Sincronização de Arquivos do Azure permite centralizar os compartilhamentos de arquivos da sua organização no serviço Arquivos do Azure sem abrir mão da flexibilidade, do desempenho e da compatibilidade de um servidor de arquivos local. A Sincronização de Arquivos do Azure transforma um Windows Server local (ou em nuvem) em um cache rápido do compartilhamento de arquivo do Azure. 

## <a name="management-concepts"></a>Conceitos de gerenciamento
Uma implantação da Sincronização de Arquivos do Azure tem três objetos de gerenciamento fundamentais:

- **Compartilhamento de arquivos do Azure**: Um compartilhamento de arquivo do Azure é um compartilhamento de arquivo em nuvem sem servidor, que fornece o *ponto de extremidade na nuvem* de uma relação de sincronização da Sincronização de Arquivos do Azure. Os arquivos em um compartilhamento de arquivo do Azure podem ser acessados diretamente com o protocolo SMB ou FileREST, embora recomendemos que você acesse os arquivos principalmente por meio do cache do Windows Server quando o compartilhamento de arquivo do Azure está sendo usado com a Sincronização de Arquivos do Azure. Isso ocorre porque o serviço Arquivos do Azure atualmente não tem um mecanismo de detecção de alteração eficiente, como o Windows Server tem, portanto, as alterações feitas diretamente no compartilhamento de arquivo do Azure levarão tempo para serem propagadas de volta para os pontos de extremidade do servidor.
- **Ponto de extremidade do servidor**: O caminho no Windows Server que está sendo sincronizado com um compartilhamento de arquivo do Azure. Isso pode ser uma pasta específica em um volume ou a raiz do volume. Vários pontos de extremidade do servidor poderão existir no mesmo volume se seus namespaces não forem sobrepostos.
- **Grupo de sincronização**: O objeto que define a relação de sincronização entre um **ponto de extremidade na nuvem** ou compartilhamento de arquivo do Azure e um ponto de extremidade do servidor. Os pontos de extremidade em um grupo de sincronização são mantidos em sincronização entre si. Se, por exemplo, você tiver dois conjuntos distintos de arquivos que deseja gerenciar com a Sincronização de Arquivos do Azure, crie dois grupos de sincronização e adicione pontos de extremidade diferentes a cada grupo de sincronização.

### <a name="azure-file-share-management-concepts"></a>Conceitos de gerenciamento de compartilhamento de arquivo do Azure
[!INCLUDE [storage-files-file-share-management-concepts](../../../includes/storage-files-file-share-management-concepts.md)]

### <a name="azure-file-sync-management-concepts"></a>Conceitos de gerenciamento de Sincronização de Arquivos do Azure
Os grupos de sincronização são implantados em **Serviços de Sincronização de Armazenamento**, que são objetos de nível superior que registram servidores para uso com a Sincronização de Arquivos do Azure e contêm as relações do grupo de sincronização. O recurso Serviço de Sincronização de Armazenamento é um par do recurso de conta de armazenamento e pode, de maneira semelhante, ser implantado em grupos de recursos do Azure. Um Serviço de Sincronização de Armazenamento pode criar grupos de sincronização que contêm compartilhamentos de arquivo do Azure entre várias contas de armazenamento e vários Windows Servers registrados.

Para criar um grupo de sincronização em um Serviço de Sincronização de Armazenamento, primeiro você deve registrar um Windows Server no Serviço de Sincronização de Armazenamento. Isso cria o objeto **servidor registrado** que representa uma relação de confiança entre seu servidor ou cluster e o Serviço de Sincronização de Armazenamento. Para registrar um Serviço de Sincronização de Armazenamento, você deve primeiro instalar o agente da Sincronização de Arquivos do Azure no servidor. Um servidor individual ou um cluster pode ser registrado apenas em um Serviço de Sincronização de Armazenamento por vez.

Um grupo de sincronização contém um ponto de extremidade na nuvem ou um compartilhamento de arquivo do Azure e pelo menos um ponto de extremidade do servidor. O objeto de ponto de extremidade do servidor contém as configurações que definem a funcionalidade de **camada de nuvem**, que fornece a funcionalidade de cache da Sincronização de Arquivos do Azure. Para sincronizar com um compartilhamento de arquivo do Azure, a conta de armazenamento que contém o compartilhamento de arquivo do Azure deve estar na mesma região do Azure que o Serviço de Sincronização de Armazenamento.

### <a name="management-guidance"></a>Diretrizes de gerenciamento
Ao implantar a Sincronização de Arquivos do Azure, recomendamos:

- Relação de um para um entre implantação de compartilhamentos de arquivo do Azure e compartilhamentos de arquivo do Windows. O objeto de ponto de extremidade do servidor oferece um excelente grau de flexibilidade na forma como você configura a topologia de sincronização no lado do servidor da relação de sincronização. Para simplificar o gerenciamento, faça com que o caminho do ponto de extremidade do servidor corresponda ao caminho do compartilhamento de arquivo do Windows. 

- Use o mínimo possível de Serviços de Sincronização de Armazenamento. Isso simplificará o gerenciamento quando você tiver grupos de sincronização que contenham vários pontos de extremidade de servidor, já que um Windows Server só pode ser registrado em um Serviço de Sincronização de Armazenamento por vez. 

- Prestar atenção às limitações de IOPS de uma conta de armazenamento ao implantar compartilhamentos de arquivo do Azure. O ideal é que você mapeie os compartilhamentos de arquivos em uma relação de um para um com contas de armazenamento. No entanto, isso nem sempre é possível devido a vários limites e restrições, tanto da sua organização quanto do Azure. Quando não for possível ter apenas um compartilhamento de arquivo implantado em uma conta de armazenamento, considere quais compartilhamentos serão altamente ativos e quais compartilhamentos serão menos ativos para garantir que os compartilhamentos de arquivos mais ativos não sejam colocados na mesma conta de armazenamento.

## <a name="windows-file-server-considerations"></a>Considerações sobre servidor de arquivos do Windows
Para habilitar a funcionalidade de sincronização no Windows Server, você deve instalar o agente Sincronização de Arquivos do Azure para baixar. O agente de Sincronização de Arquivos do Azure fornece dois componentes principais: `FileSyncSvc.exe`, o serviço Windows em segundo plano que é responsável por monitorar alterações nos pontos de extremidade do servidor e iniciar sessões de sincronização e `StorageSync.sys`, um filtro do sistema de arquivos que habilita a camada de nuvem e a rápida recuperação de desastre.  

### <a name="operating-system-requirements"></a>Requisitos do sistema operacional
A Sincronização de Arquivos do Azure é compatível com as seguintes versões do Windows Server:

| Versão | SKUs com suporte | Opções de implantação com suporte |
|---------|----------------|------------------------------|
| Windows Server 2019 | Datacenter, Standard e IoT | Completo e Core |
| Windows Server 2016 | Datacenter, Standard e Storage Server | Completo e Core |
| Windows Server 2012 R2 | Datacenter, Standard e Storage Server | Completo e Core |

Versões futuras do Windows Server serão adicionadas à medida que forem liberadas.

> [!Important]  
> Recomendamos manter todos os servidores usados com a Sincronização de Arquivos do Azure atualizados com as últimas atualizações do Windows Update. 

### <a name="minimum-system-resources"></a>Recursos mínimos do sistema
O serviço Sincronização de Arquivos do Azure requer um servidor, físico ou virtual, com pelo menos uma CPU e um mínimo de 2 GiB de memória.

> [!Important]  
> Se o servidor estiver sendo executado em uma máquina virtual com memória dinâmica habilitada, a VM deverá ser configurada com um mínimo de 2048 MiB de memória.

Para a maioria das cargas de trabalho de produção, não recomendamos a configuração de um servidor de sincronização da Sincronização de Arquivos do Azure somente com os requisitos mínimos. Confira [Recursos do sistema recomendados](#recommended-system-resources) para obter mais informações.

### <a name="recommended-system-resources"></a>Recursos de sistema recomendados
Assim como qualquer recurso de servidor ou aplicativo, os requisitos de recursos de sistema para a Sincronização de Arquivos do Azure são determinados pela escala da implantação; implantações maiores em um servidor exigem mais recursos do sistema. Na Sincronização de Arquivos do Azure, a escala é determinada pelo número de objetos entre os pontos de extremidade do servidor e a variação no conjunto de dados. Um servidor pode ter pontos de extremidade de servidor em vários grupos de sincronização e o número de objetos listados nas contas de tabela a seguir para o namespace completo ao qual um servidor está anexado. 

Por exemplo: ponto de extremidade do servidor A com 10 milhões de objetos + ponto de extremidade do servidor B com 10 milhões de objetos = 20 milhões de objetos. Para essa implantação de exemplo, recomendamos 8 CPUs, 16 GiB de memória para o estado estável e (se possível) 48 GiB de memória para a migração inicial.
 
Os dados de namespace são armazenados na memória por questões de desempenho. Por causa disso, namespaces maiores exigem mais memória para manter o bom desempenho e mais variação requer mais CPU para processar. 
 
Na tabela a seguir, fornecemos o tamanho do namespace, bem como uma conversão para capacidade para compartilhamentos de arquivos de uso geral típicos, em que o tamanho médio do arquivo é 512 KiB. Se os tamanhos do arquivo forem menores, considere adicionar mais memória para a mesma quantidade de capacidade. Baseie sua configuração de memória no tamanho do namespace.

| Tamanho do namespace – arquivos e diretórios (milhões)  | Capacidade típica (TiB)  | Núcleos de CPU  | Memória recomendada (GiB) |
|---------|---------|---------|---------|
| 3        | 1.4     | 2        | 8 (sincronização inicial)/ 2 (variação típica)      |
| 5        | 2.3     | 2        | 16 (sincronização inicial)/ 4 (variação típica)    |
| 10       | 4.7     | 4        | 32 (sincronização inicial)/ 8 (variação típica)   |
| 30       | 14.0    | 8        | 48 (sincronização inicial)/ 16 (variação típica)   |
| 50       | 23,3    | 16       | 64 (sincronização inicial)/ 32 (variação típica)  |
| 100*     | 46,6    | 32       | 128 (sincronização inicial)/ 32 (variação típica)  |

\*A sincronização de mais de 100 milhões arquivos e diretórios não é recomendada neste momento. Esse é um limite flexível com base em limites testados. Para obter mais informações, confira [Metas de desempenho e escalabilidade de Arquivos do Azure](storage-files-scale-targets.md#azure-file-sync-scale-targets).

> [!TIP]
> A sincronização inicial de um namespace é uma operação intensiva e é recomendável alocar mais memória até que a sincronização inicial seja concluída. Isso não é necessário, mas pode acelerar a sincronização inicial. 
> 
> A variação típica é de 0,5% da alteração de namespace por dia. Para níveis mais altos de variação, considere adicionar mais CPU. 

- Um volume anexado localmente formatado com o sistema de arquivos NTFS.

### <a name="evaluation-cmdlet"></a>Cmdlet de avaliação
Antes de implantar a Sincronização de Arquivos do Azure, você precisa avaliar se ela é compatível com seu sistema usando o cmdlet de avaliação da Sincronização de Arquivos do Azure. Esse cmdlet verifica se há possíveis problemas com seu sistema de arquivos e conjunto de dados, como caracteres sem suporte ou uma versão do sistema operacional não compatível. As verificações abrangem a maioria dos recursos mencionados abaixo, mas não todos eles. É recomendável que você leia o restante desta seção com cuidado para garantir que sua implantação seja perfeita. 

O cmdlet de avaliação pode ser instalado por meio do módulo Az PowerShell, que pode ser instalado seguindo as instruções aqui: [Instale e configure o Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-Az-ps).

#### <a name="usage"></a>Uso  
Você pode invocar a ferramenta de avaliação de algumas maneiras diferentes: você pode executar verificações de sistema, verificações de conjunto de dados ou ambas. Para executar verificações de sistema e de conjunto de dados: 

```powershell
Invoke-AzStorageSyncCompatibilityCheck -Path <path>
```

Para testar apenas o conjunto de dados:
```powershell
Invoke-AzStorageSyncCompatibilityCheck -Path <path> -SkipSystemChecks
```
 
Para testar somente os requisitos do sistema:
```powershell
Invoke-AzStorageSyncCompatibilityCheck -ComputerName <computer name> -SkipNamespaceChecks
```
 
Para exibir os resultados em CSV:
```powershell
$validation = Invoke-AzStorageSyncCompatibilityCheck C:\DATA
$validation.Results | Select-Object -Property Type, Path, Level, Description, Result | Export-Csv -Path C:\results.csv -Encoding utf8
```

### <a name="file-system-compatibility"></a>Compatibilidade do sistema de arquivos
A Sincronização de Arquivos do Azure só é compatível com volumes NTFS anexados diretamente. O armazenamento com conexão direta (ou DAS) no Windows Server significa que o sistema operacional do Windows Server é proprietário do sistema de arquivos. O DAS pode ser fornecido anexando discos físicos ao servidor de arquivos, anexando discos virtuais a uma VM do servidor de arquivos (como uma VM hospedada pelo Hyper-V) ou até mesmo por meio de ISCSI.

Somente volumes NTFS são compatíveis; ReFS, FAT, FAT32 e outros sistemas de arquivos não são compatíveis.

A seguinte tabela mostra o estado de interoperabilidade dos recursos do sistema de arquivos NTFS: 

| Recurso | Status de suporte | Observações |
|---------|----------------|-------|
| ACLs (listas de controle de acesso) | suporte completo | As listas de controle de acesso condicional de estilo do Windows são preservadas pela Sincronização de Arquivos do Azure e são impostas pelo Windows Server em pontos de extremidade de servidor. As ACLs também podem ser impostas ao montar diretamente o compartilhamento de arquivo do Azure, no entanto, isso requer configuração adicional. Confira a [Seção identidade](#identity) para obter mais informações. |
| Links físicos | Ignorado | |
| Links simbólicos | Ignorado | |
| Pontos de montagem | Suporte parcial | Pontos de montagem podem ser a raiz de um Ponto de Extremidade de Servidor, mas serão ignorados se estiverem contidos no namespace de um ponto de extremidade de servidor. |
| Junções | Ignorado | Por exemplo, as pastas DFSRoots e DfrsrPrivate do Sistema de Arquivos Distribuído. |
| Pontos de nova análise | Ignorado | |
| Compactação NTFS | suporte completo | |
| Arquivos esparsos | suporte completo | Os arquivos esparsos são sincronizados (não são bloqueados), mas são sincronizados com a nuvem como um arquivo completo. Se o conteúdo do arquivo for alterado na nuvem (ou em outro servidor), o arquivo não será mais esparso quando a alteração for baixada. |
| ADS (Fluxos de Dados Alternativos) | Preservados, mas não sincronizados | Por exemplo, as marcas de classificação criadas pela Infraestrutura de classificação de arquivos não são sincronizadas. As marcas de classificação em arquivos existentes em cada um dos pontos de extremidade do servidor são mantidas. |

<a id="files-skipped"></a>A Sincronização de Arquivos do Azure também ignorará determinados arquivos temporários e pastas do sistema:

| Arquivo/pasta | Observação |
|-|-|
| pagefile.sys | Arquivo específico do sistema |
| Desktop.ini | Arquivo específico do sistema |
| thumbs.db | Arquivo temporário para miniaturas |
| ehthumbs.db | Arquivo temporário para miniaturas de mídia |
| ~$\*.\* | Arquivo temporário do Office |
| \*.tmp | Arquivo temporário |
| \*.laccdb | Arquivo de bloqueio do banco de dados do Access|
| 635D02A9D91C401B97884B82B3BCDAEA.* | Arquivo de sincronização interno|
| \\Informações de Volume do Sistema | Pasta específica do volume |
| $RECYCLE.BIN| Pasta |
| \\SyncShareState | Pasta para sincronização |

### <a name="failover-clustering"></a>Clustering de failover
O clustering de failover do Windows Server tem suporte pela Sincronização de Arquivo do Azure para a opção de implantação “Servidor de Arquivos de uso geral”. Não há suporte para o Clustering de Failover em “SOFS (Servidor de Arquivos de Escalabilidade Horizontal) para dados de aplicativos” ou CSVs (Volumes Compartilhados Clusterizados).

> [!Note]  
> O agente de Sincronização de Arquivos do Azure deve ser instalado em cada nó em um Cluster de Failover para a sincronização funcionar corretamente.

### <a name="data-deduplication"></a>Eliminação de duplicação de dados
**Windows Server 2016 e Windows Server 2019**   
A eliminação de duplicação de dados tem suporte em volumes com a camada de nuvem habilitada no Windows Server 2016 e Windows Server 2019. Habilitar a Eliminação de Duplicação de Dados em um volume com camada de nuvem habilitada permite armazenar em cache mais arquivos localmente sem ter que provisionar mais armazenamento. 

Quando a Eliminação de Duplicação de Dados é habilitada em um volume com camada de nuvem habilitada, os arquivos otimizados para eliminação de duplicação na localização do ponto de extremidade do servidor serão colocados em camadas semelhantes a um arquivo normal com base nas configurações de política de camada de nuvem. Depois que os arquivos otimizados para eliminação de duplicação estiverem em camadas, o trabalho de coleta de lixo da Eliminação de Duplicação de Dados será executado automaticamente para recuperar espaço em disco removendo partes desnecessárias que não são mais referenciadas por outros arquivos no volume.

Observe que a economia em volume se aplica somente ao servidor; seus dados no compartilhamento de arquivo do Azure não passarão pela eliminação de duplicação.

> [!Note]  
> Para dar suporte à Eliminação de Duplicação de Dados em volumes com a camada de nuvem habilitada no Windows Server 2019, o Windows Update [KB4520062](https://support.microsoft.com/help/4520062) deve ser instalado e o agente da Sincronização de Arquivos do Azure versão 9.0.0.0 ou mais recente é necessário.

**Windows Server 2012 R2**  
A Sincronização de Arquivos do Azure não é compatível com a Eliminação de Duplicação de Dados e a camada de nuvem no mesmo volume no Windows Server 2012 R2. Se a Eliminação de Duplicação de Dados estiver habilitada em um volume, a camada de nuvem deverá ser desabilitada. 

**Observações**
- Se a Eliminação de Duplicação de Dados for instalada antes da instalação do agente da Sincronização de Arquivos do Azure, uma reinicialização será necessária para dar suporte à Eliminação de Duplicação de Dados e à camada de nuvem no mesmo volume.
- Se a Eliminação de Duplicação de Dados estiver habilitada em um volume depois que a camada de nuvem estiver habilitada, o trabalho de otimização de eliminação de duplicação inicial otimizará os arquivos do volume que ainda não estiverem em camadas e terá o seguinte impacto na camada de nuvem:
    - A política de espaço livre continuará a colocar arquivos em camada de acordo com o espaço livre no volume usando o mapa de calor.
    - A política de data ignorará a camada de arquivos que, de outra forma, poderia ter sido qualificada para camada devido ao trabalho de otimização da eliminação de duplicação que acessa os arquivos.
- Para trabalhos de otimização de eliminação de duplicação em andamento, a camada de nuvem com a política de data será atrasada pela configuração [MinimumFileAgeDays](https://docs.microsoft.com/powershell/module/deduplication/set-dedupvolume?view=win10-ps) da Eliminação de Duplicação de Dados, se o arquivo ainda não estiver em camada. 
    - Exemplo: Se a configuração MinimumFileAgeDays for de sete dias e a política de data da camada de nuvem for de 30 dias, a política de data colocará os arquivos em camada após 37 dias.
    - Observação: Quando um arquivo é colocado em camadas pela Sincronização de Arquivos do Azure, o trabalho de otimização da eliminação de duplicação ignorará o arquivo.
- Se um servidor que executa o Windows Server 2012 R2 com o agente da Sincronização de Arquivos do Azure instalado for atualizado para o Windows Server 2016 ou o Windows Server 2019, as seguintes etapas deverão ser executadas para dar suporte à Eliminação de Duplicação de Dados e à camada de nuvem no mesmo volume:  
    - Desinstale o agente da Sincronização de Arquivos do Azure para Windows Server 2012 R2 e reinicie o servidor.
    - Baixe o agente da Sincronização de Arquivos do Azure para a nova versão do sistema operacional do servidor (Windows Server 2016 ou Windows Server 2019).
    - Instale o agente de Sincronização de Arquivos do Azure e reinicie o servidor.  
    
    Observação: As definições de configuração de Sincronização de Arquivos do Azure no servidor são mantidas quando o agente é desinstalado e reinstalado.

### <a name="distributed-file-system-dfs"></a>DFS (Sistema de Arquivos Distribuído)
A Sincronização de Arquivos do Azure fornece suporte para interoperabilidade com Namespaces de DFS (DFS-N) e Replicação do DFS (DFS-R).

**DFS-N (Namespaces de DFS)** : A Sincronização de Arquivos do Azure é totalmente suportada em servidores DFS-N. É possível instalar o agente Sincronização de arquivos do Azure em um ou mais membros DFS-N para sincronizar dados entre os pontos de extremidade de servidor e o ponto de extremidade da nuvem. Para obter mais informações, consulte [visão geral de Namespaces de DFS](https://docs.microsoft.com/windows-server/storage/dfs-namespaces/dfs-overview).
 
**DFS-R (Replicação do DFS)** : Quando tanto DFS-R como a Sincronização de arquivos do Azure forem soluções de replicação, na maioria dos casos é recomendável substituir DFS-R pela Sincronização de Arquivos do Azure. No entanto, existem vários cenários em que você deseja usar DFS-R e Sincronização de arquivos do Azure em conjunto:

- Você está migrando de uma implantação de DFS-R para uma implantação de Sincronização de arquivos do Azure. Para obter mais informações, consulte [Migrar uma implantação de DFS-R (Replicação do DFS) para Sincronização de arquivos do Azure](storage-sync-files-deployment-guide.md#migrate-a-dfs-replication-dfs-r-deployment-to-azure-file-sync).
- Nem todo servidor local que precisa de uma cópia dos dados do seu arquivo pode ser conectado diretamente à Internet.
- Os servidor de ramificação consolidam dados em um servidor de hub único, para o qual você gostaria de usar a Sincronização de arquivos do Azure.

Para que a Sincronização de Arquivos do Azure e o DFS-R trabalharem lado a lado:

1. A camada de nuvem da Sincronização de arquivos do Azure deve ser desabilitada em volumes com pastas replicadas DFS-R.
2. Os pontos de extremidade de servidor não devem ser configurados em pastas de replicação somente leitura do DFS-R.

Para obter mais informações, consulte [Visão geral da Replicação do DFS](https://technet.microsoft.com/library/jj127250).

### <a name="sysprep"></a>Sysprep
Não há compatibilidade no uso do sysprep em um servidor que tenha o agente de Sincronização de Arquivos do Azure instalado e isso pode levar a resultados inesperados. A instalação do agente e o registro do servidor devem ocorrer depois da implantação da imagem do servidor e da conclusão da mini-instalação do sysprep.

### <a name="windows-search"></a>Windows Search
Se a opção de camadas em nuvem estiver habilitada em um ponto de extremidade do servidor, os arquivos que estão em camadas serão ignorados e não serão indexados pelo Windows Search. Arquivos sem camadas são indexados corretamente.

### <a name="other-hierarchical-storage-management-hsm-solutions"></a>Outras soluções de HSM (Gerenciamento de Armazenamento Hierárquico)
Nenhuma outra solução de HSM deve ser usada com a Sincronização de Arquivos do Azure.

## <a name="identity"></a>Identidade
A Sincronização de Arquivos do Azure trabalha com sua identidade padrão baseada em AD sem qualquer configuração especial além da configuração da sincronização. Quando você está usando a Sincronização de Arquivos do Azure, a expectativa geral é que a maioria dos acessos percorra os servidores de cache de Sincronização de Arquivos do Azure, em vez de usar o compartilhamento de arquivo do Azure. Como os pontos de extremidade do servidor estão localizados no Windows Server e o Windows Server é compatível com ACLs de estilo do AD e do Windows há muito tempo, não é necessário nada além de garantir que os servidores de arquivo do Windows registrados com o Serviço de Sincronização de Armazenamento sejam ingressados no domínio. A Sincronização de Arquivos do Azure armazenará ACLs nos arquivos do compartilhamento de arquivo do Azure e os replicará para todos os pontos de extremidade do servidor.

Embora as alterações feitas diretamente no compartilhamento de arquivo do Azure demorem mais para serem sincronizadas com os pontos de extremidade do servidor no grupo de sincronização, também é interessante garantir a imposição das suas permissões do AD no compartilhamento de arquivo diretamente na nuvem. Para fazer isso, você precisa ingressar no domínio em sua conta de armazenamento no AD local, assim como os servidores de arquivos do Windows são ingressados no domínio. Para saber mais sobre o ingresso no domínio com sua conta de armazenamento em um Active Directory de propriedade do cliente, confira [Visão geral sobre Active Directory no serviço Arquivos do Azure](storage-files-active-directory-overview.md).

> [!Important]  
> Não é necessário ingressar sua conta de armazenamento no domínio do Active Directory para implantar a Sincronização de Arquivos do Azure com êxito. Essa é uma etapa estritamente opcional que permite que o compartilhamento de arquivo do Azure imponha ACLs locais quando os usuários montam o compartilhamento de arquivo do Azure diretamente.

## <a name="networking"></a>Rede
O agente de Sincronização de Arquivos do Azure se comunica com o Serviço de Sincronização de Armazenamento e o compartilhamento de arquivo do Azure usando o protocolo REST e o protocolo FileREST da Sincronização de Arquivos do Azure e ambos sempre usam HTTPS pela porta 443. O SMB nunca é usado para carregar nem baixar dados entre o Windows Server e o compartilhamento de arquivo do Azure. Como a maioria das organizações permite o tráfego HTTPS pela porta 443, como um requisito para visitar a maioria dos sites, normalmente a configuração de rede especial não é necessária para implantar a Sincronização de Arquivos do Azure.

Com base na política da sua organização ou em requisitos regulatórios específicos, você pode exigir comunicação mais restritiva com o Azure e, portanto, a Sincronização de Arquivos do Azure oferece vários mecanismos para você configurar a rede. Com base em seus requisitos, você pode:

- Passar tráfego de sincronização e upload/download de arquivos por túnel em seu ExpressRoute ou na VPN do Azure. 
- Fazer uso de Arquivos do Azure e recursos de Rede do Azure, como pontos de extremidade de serviço e pontos de extremidade privados.
- Configurar a Sincronização de Arquivos do Azure para compatibilidade com o proxy em seu ambiente.
- Restringir a atividade de rede da Sincronização de Arquivos do Azure.

Para saber mais sobre Sincronização de Arquivos do Azure e rede, confira [sincronização de arquivos do Azure considerações de rede](storage-sync-files-networking-overview.md).

## <a name="encryption"></a>Criptografia
Ao usar a Sincronização de Arquivos do Azure, há três camadas diferentes de criptografia a serem consideradas: criptografia no armazenamento em repouso do Windows Server, criptografia em trânsito entre o agente de Sincronização de Arquivos do Azure e o Azure e a criptografia em repouso dos dados no compartilhamento de arquivo do Azure. 

### <a name="windows-server-encryption-at-rest"></a>Criptografia em repouso do Windows Server 
Há duas estratégias para criptografar dados no Windows Server que funcionam geralmente com a Sincronização de Arquivos do Azure: criptografia subjacente do sistema de arquivos de modo que o sistema de arquivos e todos os dados gravados nele sejam criptografados e criptografia no próprio formato de arquivo. Esses métodos não são mutuamente exclusivos; eles podem ser usados juntos, se desejado, uma vez que a finalidade da criptografia é diferente.

Para fornecer criptografia subjacente ao sistema de arquivos, o Windows Server oferece a caixa de entrada do BitLocker. O BitLocker é totalmente transparente para a Sincronização de Arquivos do Azure. O principal motivo para usar um mecanismo de criptografia como o BitLocker é impedir a exfiltração física de dados do seu datacenter local por alguém que queira roubar os discos e para impedir o sideload de um sistema operacional não autorizado para executar leituras/gravações não autorizadas em seus dados. Para saber mais sobre o BitLocker, confira [Visão geral do BitLocker](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-overview).

Os produtos de terceiros que funcionam de maneira semelhante ao BitLocker, no que ficam subjacente ao volume NTFS, devem funcionar de maneira totalmente transparente com a Sincronização de Arquivos do Azure. 

O outro método principal para criptografar dados é criptografar o fluxo de dados do arquivo quando o aplicativo salva o arquivo. Alguns aplicativos podem fazer isso nativamente, no entanto, geralmente esse não é o caso. Um exemplo de um método para criptografar o fluxo de dados do arquivo é a AIP (Proteção de Informações do Azure)/o Azure RMS (Azure Rights Management Services)/o RMS do Active Directory. O principal motivo para usar um mecanismo de criptografia como o AIP/RMS é impedir a exfiltração dos dados do seu compartilhamento de arquivo por pessoas que os copiam para locais alternativos, como em uma unidade flash ou os enviam por email para uma pessoa não autorizada. Quando o fluxo de dados de um arquivo é criptografado como parte do formato de arquivo, esse arquivo continuará a ser criptografado no compartilhamento de arquivo do Azure. 

A Sincronização de Arquivos do Azure não interopera com o EFS (sistema de arquivos criptografados) de NTFS nem com soluções de criptografia de terceiros que ficam acima do sistema de arquivos, mas abaixo do fluxo de dados do arquivo. 

### <a name="encryption-in-transit"></a>Criptografia em trânsito

> [!NOTE]
> O serviço Sincronização de Arquivos do Azure removerá o suporte para TLS 1.0 e 1.1 em 1º de agosto de 2020. Todas as versões de agente da Sincronização de Arquivos do Azure compatíveis já usam o TLS 1.2 por padrão. O uso de uma versão anterior do TLS poderia ocorrer se o TLS 1.2 fosse desabilitado no servidor ou um proxy fosse usado. Se você estiver usando um proxy, recomendamos que verifique a configuração do proxy. As regiões do serviço Sincronização de Arquivos do Azure adicionadas após 01/05/2020 serão compatíveis apenas com o TLS 1.2 e a compatibilidade com TLS 1.0 e 1.1 será removida das regiões existentes em 1º de agosto de 2020.  Para obter mais informações, confira o [guia de solução de problemas](storage-sync-files-troubleshoot.md#tls-12-required-for-azure-file-sync).

O agente de Sincronização de Arquivos do Azure se comunica com o Serviço de Sincronização de Armazenamento e o compartilhamento de arquivo do Azure usando o protocolo REST e o protocolo FileREST da Sincronização de Arquivos do Azure e ambos sempre usam HTTPS pela porta 443. A Sincronização de Arquivos do Azure não envia solicitações não criptografadas por HTTP. 

As contas de armazenamento do Azure contêm uma opção para exigir criptografia em trânsito, que é habilitada por padrão. Mesmo que a opção no nível da conta de armazenamento esteja desabilitada, o que significa que as conexões não criptografadas com os compartilhamentos de arquivo do Azure são possíveis, a Sincronização de Arquivos do Azure ainda usará canais criptografados para acessar seu compartilhamento de arquivo.

O principal motivo para desabilitar a criptografia em trânsito para a conta de armazenamento é a compatibilidade com um aplicativo herdado que deva ser executado em um sistema operacional mais antigo, como o Windows Server 2008 R2 ou a distribuição mais antiga do Linux, que converse com um compartilhamento de arquivo do Azure diretamente. Se o aplicativo herdado se comunicar com o cache do Windows Server do compartilhamento de arquivo, mudar essa configuração não terá nenhum efeito. 

É altamente recomendável garantir que a criptografia de dados em trânsito esteja habilitada.

Para obter mais informações sobre criptografia em trânsito, consulte [Exigir transferência segura no armazenamento do Azure](../common/storage-require-secure-transfer.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

### <a name="azure-file-share-encryption-at-rest"></a>Criptografia de compartilhamento de arquivo do Azure em repouso
[!INCLUDE [storage-files-encryption-at-rest](../../../includes/storage-files-encryption-at-rest.md)]

## <a name="storage-tiers"></a>Camadas de armazenamento
[!INCLUDE [storage-files-tiers-overview](../../../includes/storage-files-tiers-overview.md)]

### <a name="enable-standard-file-shares-to-span-up-to-100-tib"></a>Habilitar compartilhamentos de arquivo padrão para abranger até 100 TiB
[!INCLUDE [storage-files-tiers-enable-large-shares](../../../includes/storage-files-tiers-enable-large-shares.md)]

#### <a name="regional-availability"></a>Disponibilidade regional
[!INCLUDE [storage-files-tiers-large-file-share-availability](../../../includes/storage-files-tiers-large-file-share-availability.md)]

## <a name="azure-file-sync-region-availability"></a>Disponibilidade de região da sincronização de arquivos do Azure
A Sincronização de Arquivos do Azure está disponível nas seguintes regiões:

| Nuvem do Azure | Região geográfica | Região do Azure | Código da região |
|-------------|-------------------|--------------|-------------|
| Público | Ásia | Leste da Ásia | `eastasia` |
| Público | Ásia | Sudeste Asiático | `southeastasia` |
| Público | Austrália | Leste da Austrália | `australiaeast` |
| Público | Austrália | Sudeste da Austrália | `australiasoutheast` |
| Público | Brasil | Sul do Brasil | `brazilsouth` |
| Público | Canada | Canadá Central | `canadacentral` |
| Público | Canada | Leste do Canadá | `canadaeast` |
| Público | Europa | Norte da Europa | `northeurope` |
| Público | Europa | Europa Ocidental | `westeurope` |
| Público | França | França Central | `francecentral` |
| Público | França | Sul da França* | `francesouth` |
| Público | Índia | Índia Central | `centralindia` |
| Público | Índia | Sul da Índia | `southindia` |
| Público | Japão | Leste do Japão | `japaneast` |
| Público | Japão | Oeste do Japão | `japanwest` |
| Público | Coreia do Sul | Coreia Central | `koreacentral` |
| Público | Coreia do Sul | Sul da Coreia | `koreasouth` |
| Público | África do Sul | Norte da África do Sul | `southafricanorth` |
| Público | África do Sul | Oeste da África do Sul* | `southafricawest` |
| Público | EAU | EAU Central* | `uaecentral` |
| Público | EAU | Norte dos EAU | `uaenorth` |
| Público | Reino Unido | Sul do Reino Unido | `uksouth` |
| Público | Reino Unido | Oeste do Reino Unido | `ukwest` |
| Público | EUA | Centro dos EUA | `centralus` |
| Público | EUA | Leste dos EUA | `eastus` |
| Público | EUA | Leste dos EUA 2 | `eastus2` |
| Público | EUA | Centro-Norte dos EUA | `northcentralus` |
| Público | EUA | Centro-Sul dos Estados Unidos | `southcentralus` |
| Público | EUA | Centro-Oeste dos EUA | `westcentralus` |
| Público | EUA | Oeste dos EUA | `westus` |
| Público | EUA | Oeste dos EUA 2 | `westus2` |
| Gov dos EUA | EUA | Governo dos EUA do Arizona | `usgovarizona` |
| Gov dos EUA | EUA | Governo dos EUA do Texas | `usgovtexas` |
| Gov dos EUA | EUA | Gov. dos EUA – Virgínia | `usgovvirginia` |

A Sincronização de Arquivos do Azure é compatível apenas com um compartilhamento de arquivo do Azure que esteja na mesma região que o Serviço de Sincronização de Armazenamento.

Para as regiões marcadas com asteriscos, você deve contatar o Suporte do Azure para solicitar acesso ao Armazenamento do Azure nessas regiões. O processo é descrito [neste documento](https://azure.microsoft.com/global-infrastructure/geographies/).

## <a name="redundancy"></a>Redundância
[!INCLUDE [storage-files-redundancy-overview](../../../includes/storage-files-redundancy-overview.md)]

> [!Important]  
> O armazenamento com redundância geográfica e redundância de zona geográfica tem a capacidade de fazer failover manual do armazenamento na região secundária. É recomendável que você não faça isso fora de um desastre quando estiver usando a Sincronização de Arquivos do Azure devido à maior probabilidade de perda de dados. No caso de um desastre em que você gostaria de iniciar um failover manual do armazenamento, será necessário abrir um caso de suporte com a Microsoft para fazer com que a Sincronização de Arquivos do Azure retome a sincronização com o ponto de extremidade secundário.

## <a name="migration"></a>Migração
Se você tiver um servidor de arquivos do Windows existente, a Sincronização de Arquivos do Azure poderá ser instalada diretamente no local, sem a necessidade de mover dados para um novo servidor. Se você está planejando migrar para um novo servidor de arquivos do Windows como parte da adoção da Sincronização de Arquivos do Azure, há várias abordagens possíveis para migrar dados:

- Crie pontos de extremidade de servidor para o compartilhamento de arquivo antigo e o novo compartilhamento de arquivo e permita que a Sincronização de Arquivos do Azure sincronize os dados entre os pontos de extremidade do servidor. A vantagem dessa abordagem é que ela facilita muito exceder a assinatura do armazenamento em seu novo servidor de arquivos, pois a Sincronização de Arquivos do Azure tem reconhecimento de camada de nuvem. Quando estiver pronto, você poderá transferir os usuários finais para o compartilhamento de arquivo no novo servidor e remover o ponto de extremidade do servidor do compartilhamento de arquivo antigo.

- Crie um ponto de extremidade de servidor somente no novo servidor de arquivos e copie dados do compartilhamento de arquivo antigo usando `robocopy`. Dependendo da topologia dos compartilhamentos de arquivo no novo servidor (quantos compartilhamentos você tem em cada volume, o espaço disponível de cada volume etc.), talvez seja necessário provisionar temporariamente um armazenamento adicional, pois espera-se que a `robocopy` de seu servidor antigo para o novo servidor no datacenter local seja concluída mais rapidamente que a migração de dados para o Azure feita pela Sincronização de Arquivos do Azure.

Também é possível usar o Data Box para migrar dados para uma implantação da Sincronização de Arquivos do Azure. Na maioria das vezes, quando os clientes usam o Data Box para ingerir dados, eles fazem isso porque acham que aumentarão a velocidade da implantação ou porque isso ajudará com cenários de largura de banda restrita. Embora seja verdade que usar um Data Box para ingerir dados em sua implantação da Sincronização de Arquivos do Azure diminuirá a utilização da largura de banda, é provável que seja mais rápido para a maioria dos cenários buscar um carregamento de dados online por meio de um dos métodos descritos acima. Para saber mais sobre como usar o Data Box para ingerir dados em sua implantação da Sincronização de Arquivos do Azure, confira [Migrar dados para a Sincronização de Arquivos do Azure com o Azure Data Box](storage-sync-offline-data-transfer.md).

Um erro comum que os clientes fazem ao migrar dados para a nova implantação da Sincronização de Arquivos do Azure é copiar dados diretamente no compartilhamento de arquivo do Azure, em vez de fazê-lo em seus servidores de arquivos do Windows. Embora a Sincronização de Arquivos do Azure identifique todos os novos arquivos no compartilhamento de arquivo do Azure e sincronize-os de volta em seus compartilhamentos de arquivo do Windows, isso costuma ser consideravelmente mais lento do que carregar dados por meio do servidor de arquivos do Windows. Ao usar as ferramentas de cópia do Azure, como AzCopy, é importante usar a versão mais recente. Verifique a [tabela de ferramentas de cópia de arquivo](storage-files-migration-overview.md#file-copy-tools) para obter uma visão geral das ferramentas de cópia do Azure para garantir que você possa copiar todos os metadados importantes de um arquivo, como carimbos de data/hora e ACLs.

## <a name="antivirus"></a>Antivírus
Como o antivírus funciona examinando arquivos em busca de código mal-intencionado conhecido, um produto antivírus pode causar a recall de arquivos em camadas, resultando em encargos de egresso altos. Nas versões 4.0 e acima do agente de Sincronização de Arquivo do Azure, arquivos em camadas têm o conjunto FILE_ATTRIBUTE_RECALL_ON_DATA_ACCESS de atributo seguro do Windows. Recomendamos consultar o fornecedor do software para saber como configurar a solução para ignorar a leitura de arquivos com esse conjunto de atributos (muitos fazem isso automaticamente). 

As soluções antivírus internas da Microsoft, o Windows Defender e o System Center Endpoint Protection (SCEP), ignoram automaticamente a leitura de arquivos que possuem esse atributo definido. Nós os testamos e identificamos um problema menor: quando você adiciona um servidor a um grupo de sincronização existente, os arquivos com menos de 800 bytes são recuperados (feitos o download) no novo servidor. Esses arquivos permanecerão no novo servidor e não serão colocados em camadas, pois não atendem ao requisito de tamanho em camadas (> 64kb).

> [!Note]  
> Os fornecedores de antivírus podem verificar a compatibilidade entre seus produtos e a Sincronização de Arquivos do Azure usando o [Conjunto de testes de compatibilidade de antivírus da Sincronização de Arquivos do Azure](https://www.microsoft.com/download/details.aspx?id=58322), que está disponível para download no Centro de Download da Microsoft.

## <a name="backup"></a>Backup 
Se a disposição em camadas da nuvem estiver habilitada, as soluções que fazem backup diretamente do ponto de extremidade do servidor ou de uma VM na qual o ponto de extremidade do servidor está localizado não devem ser usadas. A camada de nuvem faz com que apenas um subconjunto de seus dados seja armazenado no ponto de extremidade do servidor, com o DataSet completo que reside em seu compartilhamento de arquivos do Azure. Dependendo da solução de backup usada, os arquivos em camadas serão ignorados e não será feito backup (porque têm o conjunto de atributos FILE_ATTRIBUTE_RECALL_ON_DATA_ACCESS) ou serão rechamados para o disco, resultando em encargos de egresso altos. É recomendável usar uma solução de backup de nuvem para fazer backup do compartilhamento de arquivos do Azure diretamente. Para obter mais informações, consulte [sobre o backup do compartilhamento de arquivos do Azure](https://docs.microsoft.com/azure/backup/azure-file-share-backup-overview?toc=/azure/storage/files/toc.json) ou contate seu provedor de backup para ver se eles dão suporte ao backup de compartilhamentos de arquivos do Azure.

Se você preferir usar uma solução de backup local, os backups devem ser executados em um servidor no grupo de sincronização que tenha a camada de nuvem desabilitada. Ao executar uma restauração, use as opções de restauração no nível do volume ou no nível do arquivo. Os arquivos restaurados usando a opção de restauração no nível do arquivo serão sincronizados com todos os pontos de extremidade no grupo de sincronização e os arquivos existentes serão substituídos pela versão restaurada do backup.  As restaurações no nível de volume não substituirão as versões de arquivo mais recentes no compartilhamento de arquivos do Azure ou em outros pontos de extremidade do servidor.

> [!Note]  
> A restauração bare-metal (BMR) pode causar resultados inesperados e não é atualmente suportada.

> [!Note]  
> Com a versão 9 do agente de Sincronização de Arquivos do Azure, os instantâneos do VSS (incluindo a guia Versões Anteriores) não são compatíveis com volumes com camada de nuvem habilitada. No entanto, você deve habilitar a compatibilidade de versão anterior por meio do PowerShell. [Saiba como](storage-sync-files-deployment-guide.md#self-service-restore-through-previous-versions-and-vss-volume-shadow-copy-service).

## <a name="azure-file-sync-agent-update-policy"></a>Política de atualização do agente de Sincronização de Arquivo do Azure
[!INCLUDE [storage-sync-files-agent-update-policy](../../../includes/storage-sync-files-agent-update-policy.md)]

## <a name="next-steps"></a>Próximas etapas
* [Considere as configurações de firewall e proxy](storage-sync-files-firewall-and-proxy.md)
* [Planejando uma implantação de Arquivos do Azure](storage-files-planning.md)
* [Implantar os Arquivos do Azure](storage-files-deployment-guide.md)
* [Implantar a Sincronização de Arquivos do Azure](storage-sync-files-deployment-guide.md)
* [Monitorar a Sincronização de Arquivos do Azure](storage-sync-files-monitoring.md)
