---
title: Azure Container Instances modelleri dağıtma
titleSuffix: Azure Machine Learning
description: Azure Container Instances kullanarak Azure Machine Learning modellerinizi Web hizmeti olarak dağıtmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.custom: how-to
ms.author: jordane
author: jpe316
ms.reviewer: larryfr
ms.date: 06/12/2020
ms.openlocfilehash: fc20d59ca016e7c6be8dae9f50a84e4927449467
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91660743"
---
# <a name="deploy-a-model-to-azure-container-instances"></a>Modeli Azure Container Instances’a dağıtma


Azure Container Instances (ACI) üzerinde bir modeli Web hizmeti olarak dağıtmak için Azure Machine Learning nasıl kullanacağınızı öğrenin. Aşağıdaki koşullardan biri doğru ise Azure Container Instances kullanın:

- Modelinizi hızlıca dağıtmanız ve doğrulamanız gerekir. Zaman içinde acı kapsayıcıları oluşturmanız gerekmez. Dağıtım sürecinin bir parçası olarak oluşturulur.
- Geliştirme aşamasındaki bir modeli test edersiniz. 

ACI 'nin kota ve bölge kullanılabilirliği hakkında daha fazla bilgi için bkz. [Azure Container Instances Için kotalar ve bölge kullanılabilirliği](https://docs.microsoft.com/azure/container-instances/container-instances-quotas) .

> [!IMPORTANT]
> Web hizmetine dağıtılmadan önce yerel olarak hata ayıklaması önerilir, daha fazla bilgi için bkz. [yerel olarak hata ayıklama](https://docs.microsoft.com/azure/machine-learning/how-to-troubleshoot-deployment#debug-locally)
>
> Azure Machine Learning - [Yerel Not Defterine Dağıtma](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/deployment/deploy-to-local) konusuna da bakabilirsiniz

## <a name="prerequisites"></a>Ön koşullar

- Azure Machine Learning çalışma alanı. Daha fazla bilgi için bkz. [Azure Machine Learning çalışma alanı oluşturma](how-to-manage-workspace.md).

- Bir Machine Learning modeli, çalışma alanınıza kaydedildi. Kayıtlı bir modeliniz yoksa, bkz. [modellerin nasıl ve nereye dağıtılacağı](how-to-deploy-and-where.md).

- [Machine Learning hizmeti Için Azure CLI uzantısı](reference-azure-machine-learning-cli.md), [Azure Machine Learning Python SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py&preserve-view=true)veya [Azure Machine Learning Visual Studio Code uzantısı](tutorial-setup-vscode-extension.md).

- Bu makaledeki __Python__ kod parçacıkları aşağıdaki değişkenlerin ayarlandığı varsayılır:

    * `ws` -Çalışma alanınıza ayarlayın.
    * `model` -Kayıtlı modelinize ayarlanır.
    * `inference_config` -Modelin çıkarım yapılandırmasına ayarlayın.

    Bu değişkenleri ayarlama hakkında daha fazla bilgi için bkz. [modellerin nasıl ve ne şekilde dağıtılacağı](how-to-deploy-and-where.md).

- Bu makaledeki __CLI__ kod parçacıkları bir belge oluşturduğunuzu varsayar `inferenceconfig.json` . Bu belgeyi oluşturma hakkında daha fazla bilgi için bkz. [modellerin nasıl ve nereye dağıtılacağı](how-to-deploy-and-where.md).

## <a name="limitations"></a>Sınırlamalar

* Bir sanal ağda Azure Container Instances kullanırken, sanal ağın Azure Machine Learning çalışma alanınız ile aynı kaynak grubunda olması gerekir.
* Sanal ağ içinde Azure Container Instances kullanırken, çalışma alanınızın Azure Container Registry (ACR) Sanal ağda de olamaz.

Daha fazla bilgi için bkz. [sanal ağlarla ının güvenliğini sağlama](how-to-secure-inferencing-vnet.md#enable-azure-container-instances-aci).

## <a name="deploy-to-aci"></a>ACI'ye dağıtma

Azure Container Instances bir modeli dağıtmak için, gereken işlem kaynaklarını açıklayan bir __dağıtım yapılandırması__ oluşturun. Örneğin, çekirdek ve bellek sayısı. Ayrıca, modeli ve Web hizmetini barındırmak için gereken ortamı açıklayan bir __çıkarım yapılandırmasına__ihtiyacınız vardır. Çıkarım yapılandırmasını oluşturma hakkında daha fazla bilgi için bkz. [modellerin nasıl ve ne şekilde dağıtılacağı](how-to-deploy-and-where.md).

> [!NOTE]
> * ACI yalnızca boyutu 1 GB 'ın altında olan küçük modeller için uygundur. 
> * Geliştirme ve test daha büyük modeller için tek düğümlü AKS kullanılması önerilir.
> * Dağıtılacak model sayısı, dağıtım başına 1.000 modellerle sınırlıdır (kapsayıcı başına). 

### <a name="using-the-sdk"></a>SDK’yı kullanarak

```python
from azureml.core.webservice import AciWebservice, Webservice
from azureml.core.model import Model

deployment_config = AciWebservice.deploy_configuration(cpu_cores = 1, memory_gb = 1)
service = Model.deploy(ws, "aciservice", [model], inference_config, deployment_config)
service.wait_for_deployment(show_output = True)
print(service.state)
```

Bu örnekte kullanılan sınıflar, Yöntemler ve parametreler hakkında daha fazla bilgi için, aşağıdaki başvuru belgelerine bakın:

* [AciWebservice.deploy_configuration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aciwebservice?view=azure-ml-py&preserve-view=true#&preserve-view=truedeploy-configuration-cpu-cores-none--memory-gb-none--tags-none--properties-none--description-none--location-none--auth-enabled-none--ssl-enabled-none--enable-app-insights-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--ssl-cname-none--dns-name-label-none--primary-key-none--secondary-key-none--collect-model-data-none--cmk-vault-base-url-none--cmk-key-name-none--cmk-key-version-none-)
* [Model. deploy](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py&preserve-view=true#&preserve-view=truedeploy-workspace--name--models--inference-config-none--deployment-config-none--deployment-target-none--overwrite-false-)
* [Webservice.wait_for_deployment](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice%28class%29?view=azure-ml-py&preserve-view=true#&preserve-view=truewait-for-deployment-show-output-false-)

### <a name="using-the-cli"></a>CLı 'yi kullanma

CLı kullanarak dağıtmak için aşağıdaki komutu kullanın. `mymodel:1`Kayıt, kayıtlı modelin adı ve sürümü ile değiştirin. `myservice`Bu hizmete verilecek adla değiştirin:

```azurecli-interactive
az ml model deploy -m mymodel:1 -n myservice -ic inferenceconfig.json -dc deploymentconfig.json
```

[!INCLUDE [deploymentconfig](../../includes/machine-learning-service-aci-deploy-config.md)]

Daha fazla bilgi için, [az ml model dağıtım](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/model?view=azure-cli-latest&preserve-view=true#ext-azure-cli-ml-az-ml-model-deploy) başvurusuna bakın. 

## <a name="using-vs-code"></a>VS Code'u kullanma

Bkz. [vs Code modellerinizi dağıtma](tutorial-train-deploy-image-classification-model-vscode.md#deploy-the-model).

> [!IMPORTANT]
> Önceden test etmek için bir ACI kapsayıcısı oluşturmanız gerekmez. ACI kapsayıcıları gerektiğinde oluşturulur.


## <a name="next-steps"></a>Sonraki adımlar

* [Özel bir Docker görüntüsü kullanarak model dağıtma](how-to-deploy-custom-docker-image.md)
* [Dağıtım sorunlarını giderme](how-to-troubleshoot-deployment.md)
* [Web hizmetini güncelleştirme](how-to-deploy-update-web-service.md)
* [TLS kullanarak Azure Machine Learning aracılığıyla web hizmetinin güvenliğini sağlama](how-to-secure-web-service.md)
* [Web hizmeti olarak dağıtılan bir ML modelini kullanma](how-to-consume-web-service.md)
* [Application Insights Azure Machine Learning modellerinizi izleyin](how-to-enable-app-insights.md)
* [Üretimde modeller için veri toplama](how-to-enable-data-collection.md)
