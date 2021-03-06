---
title: O que são pipelines Azure Machine Learning
description: Saiba como os pipelines do ML (Machine Learning) ajudam a criar, otimizar e gerenciar fluxos de trabalho de aprendizado de máquina.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: laobri
author: lobrien
ms.date: 08/17/2020
ms.custom: devx-track-python
ms.openlocfilehash: b0217766c92ddcd1907eca2c6702d91b02e06c03
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90893647"
---
# <a name="what-are-azure-machine-learning-pipelines"></a>O que são pipelines Azure Machine Learning?

Neste artigo, você aprende como Azure Machine Learning pipelines ajudam a criar, otimizar e gerenciar fluxos de trabalho de aprendizado de máquina. Esses fluxos de trabalho têm vários benefícios: 

+ Simplicidade
+ Velocidade
+ Repetição
+ Flexibilidade
+ Controle de versão e acompanhamento
+ Modularidade 
+ Garantia de qualidade
+ Controle de custo

Esses benefícios se tornam significativos assim que o seu projeto de aprendizado de máquina passa além da exploração pura e da iteração. Até mesmo pipelines simples de uma etapa podem ser valiosos. Os projetos de aprendizado de máquina geralmente estão em um estado complexo, e pode ser um alívio para fazer o realização preciso de um único fluxo de trabalho um processo trivial.

<a name="compare"></a>
### <a name="which-azure-pipeline-technology-should-i-use"></a>Qual tecnologia de pipeline do Azure devo usar?

A nuvem do Azure fornece vários outros pipelines, cada um com uma finalidade diferente. A tabela a seguir lista os pipelines diferentes e para que eles são usados:

| Cenário | Persona primário | Oferta do Azure | Oferta de OSS | Pipe canônico | Pontos fortes | 
| -------- | --------------- | -------------- | ------------ | -------------- | --------- | 
| Orquestração de modelo (Machine Learning) | Cientista de dados | Pipelines do Azure Machine Learning | Pipelines do Kubeflow | Modelo de > de dados | Distribuição, cache, primeiro código, reutilização | 
| Orquestração de dados (preparação de dados) | Engenheiro de dados | [Pipelines do Azure Data Factory](https://docs.microsoft.com/azure/data-factory/concepts-pipelines-activities) | Fluxo de ar do Apache | Dados > dados | Movimentação fortemente tipada, atividades centradas em dados |
| Código & orquestração de aplicativo (CI/CD) | Desenvolvedor de aplicativos/Ops | [Pipelines do Azure DevOps](https://azure.microsoft.com/services/devops/pipelines/) | Jenkins | Código + modelo – aplicativo/serviço > | Mais suporte a atividades abertas e flexíveis, filas de aprovação, fases com a retenção | 

## <a name="what-can-azure-ml-pipelines-do"></a>O que os pipelines do Azure ML podem fazer?

Um pipeline de Azure Machine Learning é um fluxo de trabalho executável independente de uma tarefa de aprendizado de máquina completa. As subtarefas são encapsuladas como uma série de etapas no pipeline. Um pipeline Azure Machine Learning pode ser tão simples quanto um que chama um script Python, portanto, _pode_ fazer praticamente qualquer coisa. Os pipelines _devem_ se concentrar em tarefas de aprendizado de máquina, como:

+ Preparação de dados, incluindo importação, validação e limpeza, mudanças irreversíveis e transformação, normalização e preparo
+ Configuração de treinamento, incluindo argumentos de parametrização, filePaths e configurações de log/relatório
+ Treinamento e validação de forma eficiente e repetida. A eficiência pode vir de especificar subconjuntos de dados específicos, diferentes recursos de computação de hardware, processamento distribuído e monitoramento de progresso
+ Implantação, incluindo controle de versão, dimensionamento, provisionamento e controle de acesso

As etapas independentes permitem que vários cientistas de dados funcionem no mesmo pipeline ao mesmo tempo sem sobrecarregar recursos de computação. Etapas separadas também facilitam o uso de diferentes tipos/tamanhos de computação para cada etapa.

Depois que o pipeline é criado, muitas vezes, há mais ajustes finos no loop de treinamento do pipeline. Quando você executa novamente um pipeline, a execução salta para as etapas que precisam ser executadas novamente, como um script de treinamento atualizado. As etapas que não precisam ser executadas novamente são ignoradas. 

Com pipelines, você pode optar por usar hardware diferente para tarefas diferentes. O Azure coordena os vários [destinos de computação](concept-azure-machine-learning-architecture.md) que você usa, de modo que seus dados intermediários fluem diretamente para destinos de computação downstream.

Você pode [acompanhar as métricas de seus experimentos de pipeline](https://docs.microsoft.com/azure/machine-learning/how-to-track-experiments) diretamente no portal do Azure ou na [página de aterrissagem do espaço de trabalho (versão prévia)](https://ml.azure.com). Depois que um pipeline tiver sido publicado, você poderá configurar um ponto de extremidade REST, que permite executar novamente o pipeline de qualquer plataforma ou pilha.

Em suma, todas as tarefas complexas do ciclo de vida do Machine Learning podem ser ajudado com pipelines. Outras tecnologias de pipeline do Azure têm seus próprios pontos fortes. [Azure data Factory pipelines](https://docs.microsoft.com/azure/data-factory/concepts-pipelines-activities) os excels em trabalhando com dados e [Azure pipelines](https://azure.microsoft.com/services/devops/pipelines/) é a ferramenta certa para integração e implantação contínuas. Mas se o seu foco for aprendizado de máquina, Azure Machine Learning pipelines provavelmente serão a melhor opção para suas necessidades de fluxo de trabalho. 

### <a name="analyzing-dependencies"></a>Analisando dependências

Muitos ecossistemas de programação têm ferramentas que orquestram dependências de recursos, biblioteca ou compilação. Em geral, essas ferramentas usam carimbos de data/hora de arquivo para calcular dependências. Quando um arquivo é alterado, apenas ele e seus dependentes são atualizados (baixados, recompilados ou empacotados). Os pipelines do Azure ML estendem esse conceito. Como as ferramentas de Build tradicionais, os pipelines calculam as dependências entre as etapas e só executam os recálculos necessários. 

No entanto, a análise de dependência em pipelines do Azure ML é mais sofisticada do que carimbos de data/hora simples. Cada etapa pode ser executada em um ambiente de hardware e software diferente. A preparação de dados pode ser um processo demorado, mas não precisa ser executada em hardware com GPUs poderosas, determinadas etapas podem exigir software específico do sistema operacional, talvez você queira usar o treinamento distribuído e assim por diante. 

Azure Machine Learning orquestra automaticamente todas as dependências entre as etapas do pipeline. Essa orquestração pode incluir a rotação de imagens do Docker para cima e para baixo, anexando e desanexando recursos de computação e movendo dados entre as etapas de maneira consistente e automática.

### <a name="coordinating-the-steps-involved"></a>Coordenando as etapas envolvidas

Quando você cria e executa um `Pipeline` objeto, ocorrem as seguintes etapas de alto nível:

+ Para cada etapa, o serviço calcula os requisitos para:
    + Recursos de computação de hardware
    + Recursos do sistema operacional (imagem (ns) do Docker)
    + Recursos de software (dependências de Conda/virtualenv)
    + Entradas de dados 
+ O serviço determina as dependências entre etapas, resultando em um grafo de execução dinâmica
+ Quando cada nó no grafo de execução é executado:
    + O serviço configura o ambiente de hardware e software necessário (talvez reutilizando os recursos existentes)
    + A etapa é executada, fornecendo registro em log e informações de monitoramento para seu `Experiment` objeto recipiente
    + Quando a etapa for concluída, suas saídas serão preparadas como entradas para a próxima etapa e/ou gravadas no armazenamento
    + Os recursos que não são mais necessários são finalizados e desanexados

![Etapas de pipeline](./media/concept-ml-pipelines/run_an_experiment_as_a_pipeline.png)

## <a name="building-pipelines-with-the-python-sdk"></a>Criando pipelines com o SDK do Python

No [SDK do Azure Machine Learning Python](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py&preserve-view=true), um pipeline é um objeto Python definido no `azureml.pipeline.core` módulo. Um objeto de [pipeline](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipeline%28class%29?view=azure-ml-py&preserve-view=true) contém uma sequência ordenada de um ou mais objetos [PipelineStep](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.builder.pipelinestep?view=azure-ml-py&preserve-view=true) . A `PipelineStep` classe é abstrata e as etapas reais serão de subclasses, como [EstimatorStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.estimatorstep?view=azure-ml-py&preserve-view=true), [PythonScriptStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.pythonscriptstep?view=azure-ml-py&preserve-view=true)ou [DataTransferStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.datatransferstep?view=azure-ml-py&preserve-view=true). A classe [ModuleStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.modulestep?view=azure-ml-py&preserve-view=true) mantém uma sequência reutilizável de etapas que podem ser compartilhadas entre pipelines. Um `Pipeline` é executado como parte de um `Experiment` .

Um pipeline do Azure ML está associado a um espaço de trabalho Azure Machine Learning e uma etapa de pipeline está associada a um destino de computação disponível nesse espaço de trabalho. Para obter mais informações, consulte [criar e gerenciar espaços de trabalho do Azure Machine Learning no portal do Azure](https://docs.microsoft.com/azure/machine-learning/how-to-manage-workspace) ou [quais são os destinos de computação no Azure Machine Learning?](https://docs.microsoft.com/azure/machine-learning/concept-compute-target).

### <a name="a-simple-python-pipeline"></a>Um pipeline Python simples

Este trecho de código mostra os objetos e as chamadas necessárias para criar e executar um `Pipeline` :

```python
ws = Workspace.from_config() 
blob_store = Datastore(ws, "workspaceblobstore")
compute_target = ws.compute_targets["STANDARD_NC6"]
experiment = Experiment(ws, 'MyExperiment') 

input_data = Dataset.File.from_files(
    DataPath(datastore, '20newsgroups/20news.pkl'))

output_data = PipelineData("output_data", datastore=blob_store)

input_named = input_data.as_named_input('input')

steps = [ PythonScriptStep(
    script_name="train.py",
    arguments=["--input", input_named.as_download(), "--output", output_data],
    inputs=[input_data],
    outputs=[output_data],
    compute_target=compute_target,
    source_directory="myfolder"
) ]

pipeline = Pipeline(workspace=ws, steps=steps)

pipeline_run = experiment.submit(pipeline)
pipeline_run.wait_for_completion()
```

O trecho de código começa com objetos Azure Machine Learning comuns, a `Workspace` , a `Datastore` , um [ComputeTarget](https://docs.microsoft.com/python/api/azureml-core/azureml.core.computetarget?view=azure-ml-py&preserve-view=true)e um `Experiment` . Em seguida, o código cria os objetos a serem guardados `input_data` e `output_data` . A matriz `steps` contém um único elemento, um `PythonScriptStep` que usará os objetos de dados e será executado no `compute_target` . Em seguida, o código instancia o `Pipeline` objeto em si, passando o espaço de trabalho e a matriz de etapas. A chamada para `experiment.submit(pipeline)` começa a execução do pipeline do Azure ml. A chamada para `wait_for_completion()` blocos até que o pipeline seja concluído. 

Para saber mais sobre como conectar seu pipeline a seus dados, confira os artigos [acesso a dados em Azure Machine Learning](concept-data.md) e [movendo dados para e entre as etapas de pipeline do ml (Python)](how-to-move-data-in-out-of-pipelines.md). 

## <a name="building-pipelines-with-the-designer"></a>Criando pipelines com o designer

Os desenvolvedores que preferem uma superfície de Design Visual podem usar o designer de Azure Machine Learning para criar pipelines. Você pode acessar essa ferramenta por meio da seleção de **Designer** na home page do seu espaço de trabalho.  O designer permite que você arraste e solte as etapas na superfície de design. 

Quando você cria pipelines visualmente, as entradas e saídas de uma etapa são exibidas visivelmente. Você pode arrastar e soltar conexões de dados, permitindo que você entenda rapidamente e modifique o fluxo de armazenamento de seu pipeline.

![Exemplo do Azure Machine Learning Designer](./media/concept-designer/designer-drag-and-drop.gif)

## <a name="key-advantages"></a>Principais vantagens

As principais vantagens de usar pipelines para seus fluxos de trabalho de aprendizado de máquina são:

|Principal vantagem|Descrição|
|:-------:|-----------|
|**Execuções&nbsp;autônomas**|Agende as etapas para execução em paralelo ou em sequência de maneira confiável e autônoma. A preparação e a modelagem de dados podem durar dias ou semanas, e os pipelines permitem que você se concentre em outras tarefas enquanto o processo está em execução. |
|**Computação heterogênea**|Use vários pipelines que são coordenados de forma confiável entre os recursos de computação heterogêneos e escalonáveis e os locais de armazenamento. Faça uso eficiente dos recursos de computação disponíveis executando etapas de pipeline individuais em diferentes destinos de computação, como o HDInsight, VMs de ciência de dados de GPU e databricks.|
|**Reutilização**|Crie modelos de pipeline para cenários específicos, como retreinamento e pontuação de lote. Disparar pipelines publicados de sistemas externos por meio de chamadas REST simples.|
|**Acompanhamento e controle de versão**|Em vez de acompanhar manualmente os caminhos de dados e resultados durante a iteração, use o SDK de pipelines para fornecer explicitamente o nome e a versão de fontes de dados, entradas e saídas. Você também pode gerenciar os scripts e os dados separadamente para aumentar a produtividade.|
| **Modularidade** | Separar áreas de preocupações e isolar alterações permite que o software evolua a uma taxa mais rápida com maior qualidade. | 
|**Colaboração**|Os pipelines permitem que os cientistas de dados colaborem em todas as áreas do processo de design do Machine Learning, ao mesmo tempo em que podem trabalhar simultaneamente em etapas de pipeline.|

## <a name="next-steps"></a>Próximas etapas

Os pipelines do Azure ML são um recurso poderoso que começa a fornecer valor nos primeiros estágios de desenvolvimento. O valor aumenta conforme a equipe e o projeto crescem. Este artigo explicou como pipelines são especificados com o SDK Azure Machine Learning Python e orquestrados no Azure. Você viu um código-fonte simples e foi introduzido em algumas das `PipelineStep` classes disponíveis. Você deve ter uma noção de quando usar pipelines do Azure ML e como o Azure os executa. 


+ Saiba como [criar seu primeiro pipeline](how-to-create-your-first-pipeline.md).

+ Saiba como [executar previsões em lote em dados grandes](tutorial-pipeline-batch-scoring-classification.md ).

+ Consulte os documentos de referência do SDK para [pipeline Core](https://docs.microsoft.com/python/api/azureml-pipeline-core/?view=azure-ml-py&preserve-view=true) e [as etapas de pipeline](https://docs.microsoft.com/python/api/azureml-pipeline-steps/?view=azure-ml-py&preserve-view=true).

+ Experimente o exemplo Jupyter notebooks mostrando [Azure Machine Learning pipelines](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/machine-learning-pipelines). Saiba como [executar blocos de anotações para explorar esse serviço](samples-notebooks.md).
