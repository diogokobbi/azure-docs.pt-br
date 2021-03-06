---
title: Atualizar serviços Web
titleSuffix: Azure Machine Learning
description: Saiba como atualizar um serviço Web que já está implantado no Azure Machine Learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: gopalv
author: gvashishtha
ms.date: 07/31/2020
ms.openlocfilehash: 5f2def8d41252b2267f2de736dc93825ac767540
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91328319"
---
# <a name="update-a-deployed-web-service"></a>Atualizar um serviço Web implantado

Neste artigo, você aprenderá a atualizar um serviço Web que foi implantado com o Azure Machine Learning.

## <a name="prerequisites"></a>Pré-requisitos

Este tutorial pressupõe que você já tenha implantado um serviço Web com Azure Machine Learning. Se você precisar aprender como implantar um serviço Web, [siga estas etapas](how-to-deploy-and-where.md).

## <a name="update-web-service"></a>Atualizar serviço Web

Para atualizar um serviço Web, use o `update` método. Você pode atualizar o serviço Web para usar um novo modelo, um novo script de entrada ou novas dependências que podem ser especificadas em uma configuração de inferência. Para obter mais informações, consulte a documentação do [WebService. Update](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.webservice.webservice?view=azure-ml-py&preserve-view=true#&preserve-view=trueupdate--args-).

Consulte [método de atualização do serviço AKs.](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.akswebservice?view=azure-ml-py&preserve-view=true#&preserve-view=trueupdate-image-none--autoscale-enabled-none--autoscale-min-replicas-none--autoscale-max-replicas-none--autoscale-refresh-seconds-none--autoscale-target-utilization-none--collect-model-data-none--auth-enabled-none--cpu-cores-none--memory-gb-none--enable-app-insights-none--scoring-timeout-ms-none--replica-max-concurrent-requests-none--max-request-wait-time-none--num-replicas-none--tags-none--properties-none--description-none--models-none--inference-config-none--gpu-cores-none--period-seconds-none--initial-delay-seconds-none--timeout-seconds-none--success-threshold-none--failure-threshold-none--namespace-none--token-auth-enabled-none-)

Consulte [método de atualização do serviço ACI.](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aci.aciwebservice?view=azure-ml-py&preserve-view=true#&preserve-view=trueupdate-image-none--tags-none--properties-none--description-none--auth-enabled-none--ssl-enabled-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--ssl-cname-none--enable-app-insights-none--models-none--inference-config-none-)

> [!IMPORTANT]
> Ao criar uma nova versão de um modelo, você deve atualizar manualmente cada serviço que deseja usá-lo.
>
> Você não pode usar o SDK para atualizar um serviço Web publicado no designer de Azure Machine Learning.

**Usar o SDK**

O código a seguir mostra como usar o SDK para atualizar o modelo, o ambiente e o script de entrada para um serviço Web:

```python
from azureml.core import Environment
from azureml.core.webservice import Webservice
from azureml.core.model import Model, InferenceConfig

# Register new model.
new_model = Model.register(model_path="outputs/sklearn_mnist_model.pkl",
                           model_name="sklearn_mnist",
                           tags={"key": "0.1"},
                           description="test",
                           workspace=ws)

# Use version 3 of the environment.
deploy_env = Environment.get(workspace=ws,name="myenv",version="3")
inference_config = InferenceConfig(entry_script="score.py",
                                   environment=deploy_env)

service_name = 'myservice'
# Retrieve existing service.
service = Webservice(name=service_name, workspace=ws)



# Update to new model(s).
service.update(models=[new_model], inference_config=inference_config)
service.wait_for_deployment(show_output=True)
print(service.state)
print(service.get_logs())
```

**Usando a CLI**

Você também pode atualizar um serviço Web usando a CLI do ML. O exemplo a seguir demonstra como registrar um novo modelo e, em seguida, atualizar um serviço Web para usar o novo modelo:

```azurecli
az ml model register -n sklearn_mnist  --asset-path outputs/sklearn_mnist_model.pkl  --experiment-name myexperiment --output-metadata-file modelinfo.json
az ml service update -n myservice --model-metadata-file modelinfo.json
```

> [!TIP]
> Neste exemplo, um documento JSON é usado para passar as informações do modelo do comando de registro para o comando Update.
>
> Para atualizar o serviço para usar um novo script ou ambiente de entrada, crie um [arquivo de configuração de inferência](/azure/machine-learning/reference-azure-machine-learning-cli#inference-configuration-schema) e especifique-o com o `ic` parâmetro.

Para obter mais informações, consulte a documentação de [atualização do serviço AZ ml](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/service?view=azure-cli-latest&preserve-view=true#ext-azure-cli-ml-az-ml-service-update) .

## <a name="next-steps"></a>Próximas etapas

* [Solucionar problemas de uma implantação com falha](how-to-troubleshoot-deployment.md)
* [Implantar no Serviço de Kubernetes do Azure](how-to-deploy-azure-kubernetes-service.md)
* [Criar aplicativos cliente para consumir serviços Web](how-to-consume-web-service.md)
* [Como implantar um modelo usando uma imagem personalizada do Docker](how-to-deploy-custom-docker-image.md)
* [Use o TLS para proteger um serviço Web por meio do Azure Machine Learning](how-to-secure-web-service.md)
* [Monitore seus modelos de Azure Machine Learning com Application Insights](how-to-enable-app-insights.md)
* [Coletar dados para modelos em produção](how-to-enable-data-collection.md)
* [Criar alertas de eventos e gatilhos para implantações de modelo](how-to-use-event-grid.md)
