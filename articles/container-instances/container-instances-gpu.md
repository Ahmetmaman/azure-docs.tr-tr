---
title: GPU özellikli Azure container ınstances'a dağıtma
description: GPU kaynakları üzerinde çalıştırılacak Azure container ınstances'a dağıtmayı öğrenin.
services: container-instances
author: dlepow
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 11/29/2018
ms.author: danlep
ms.openlocfilehash: adb893a9d37219409f81b2fb402f2d4afd36aa34
ms.sourcegitcommit: 8b41b86841456deea26b0941e8ae3fcdb2d5c1e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2019
ms.locfileid: "57338867"
---
# <a name="deploy-container-instances-that-use-gpu-resources"></a>GPU kaynakları kullanan container Instances'ı dağıtma

Azure Container Instances hakkında belirli bilgi işlem açısından yoğun iş yüklerini çalıştırmak için dağıtma, [kapsayıcı grupları](container-instances-container-groups.md) ile *GPU kaynakları*. Grubundaki kapsayıcı örnekleri, NVIDIA Tesla Gpu'lar bir veya daha fazla CUDA gibi kapsayıcı iş yükleri ve uygulamalar derin öğrenme erişebilirsiniz.

Bu makalede gösterilen şekilde kullanarak bir kapsayıcı grubu dağıttığınızda GPU kaynaklar ekleyebilirsiniz bir [YAML dosyası](container-instances-multi-container-yaml.md) veya [Resource Manager şablonu](container-instances-multi-container-group.md).

> [!IMPORTANT]
> Bu özellik şu anda önizlemededir ve bazı [sınırlamalar uygulanır](#preview-limitations). Önizlemeler, [ek kullanım koşullarını][terms-of-use] kabul etmeniz şartıyla kullanımınıza sunulur. Bu özelliğin bazı yönleri genel kullanıma açılmadan önce değişebilir.

## <a name="preview-limitations"></a>Önizleme sınırlamaları

Önizleme aşamasında olan, GPU kaynakları kapsayıcı grupları kullanılırken aşağıdaki sınırlamalar geçerlidir. 

**Desteklenen bölgeler**:

* Doğu ABD (myresourcegroup)
* Batı ABD 2 (westus2)
* Güney Orta ABD (southcentralus)
* Batı Avrupa (westeurope)
* Kuzey Avrupa (northeurope)
* Doğu Asya (ping'in ekran)
* Orta Hindistan (centralindia)

Destek zaman içinde için ek bölgeler eklenecektir.

**Desteklenen işletim sistemi türleri**: Yalnızca Linux

**Ek sınırlamalar**: GPU kaynakları kullanılamaz, bir kapsayıcı grubuna dağıtırken bir [sanal ağ](container-instances-vnet.md).

## <a name="about-gpu-resources"></a>GPU kaynaklar hakkında

### <a name="count-and-sku"></a>Sayısı ve SKU

GPU'ları bir kapsayıcı örneği içinde kullanmak için belirtin bir *GPU kaynak* aşağıdaki bilgilerle:

* **Sayısı** -Gpu'lar sayısı: **1**, **2**, veya **4**.
* **SKU** -GPU SKU: **K80**, **P100**, veya **V100**. Her SKU tek bir NVIDIA Tesla GPU aşağıdaki Azure GPU etkin sanal makine aileleri eşler:

  | SKU | VM ailesi |
  | --- | --- |
  | K80 | [NC](../virtual-machines/linux/sizes-gpu.md#nc-series) |
  | P100 | [NCv2](../virtual-machines/linux/sizes-gpu.md#ncv2-series) |
  | V100 | [NCv3](../virtual-machines/linux/sizes-gpu.md#ncv3-series) |

### <a name="cpu-and-memory"></a>CPU ve bellek

GPU kaynakları dağıtırken, CPU ve bellek kaynakları aşağıdaki tabloda gösterilen en yüksek değerleri en fazla iş yükü için uygun olarak ayarlayın. Bu GPU kaynaklar olmadan container Instances, CPU ve bellek sınırları daha büyük değerler.  

| GPU SKU | GPU sayısı | CPU |  Bellek (GB) |
| --- | --- | --- | --- |
| K80 | 1 | 6 | 56 |
| K80 | 2 | 12 | 112 |
| K80 | 4 | 24 | 224 |
| P100 | 1 | 6 | 112 |
| P100 | 2 | 12 | 224 |
| P100 | 4 | 24 | 448 |
| V100 | 1 | 6 | 112 |
| V100 | 2 | 12 | 224 |
| V100 | 4 | 24 | 448 |

### <a name="things-to-know"></a>Bilmeniz gerekenler

* **Dağıtım süresini** -GPU kaynakları içeren bir kapsayıcı grubunun oluşturulması alır kadar **8-10 dakika**. Bu, sağlamak ve Azure'daki bir GPU VM yapılandırmak için ek süre kaynaklanır. 

* **Fiyatlandırma** - GPU kaynaklar olmadan kapsayıcı grupları, Azure faturaları üzerinde kullanılan kaynaklar için benzer *süresi* GPU kaynaklarla bir kapsayıcı grubunun. Süre kapsayıcı grubunun kapsayıcınızın ilk kapsayıcınızın görüntüsünü çekmek için süreye göre hesaplanır. Kapsayıcı grubu dağıtma süresini içermez.

  Fiyatlandırma, kapsayıcı gruplarının kapsayıcı grubu için GPU kaynakları yüksektir. Bkz: [fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/container-instances/).

* **CUDA sürücüleri** - Container Instances GPU kaynaklarla NVIDIA CUDA sürücüleriyle önceden sağlanmış ve kapsayıcı görüntüleri kullanabilmeniz için kapsayıcı çalışma zamanı, geliştirilen için CUDA iş yükleri.

  Bu aşamada CUDA 9.0 destekliyoruz. Örneğin, temel görüntü Docker dosyanız için aşağıdaki kullanabilirsiniz:
  * [nVidia/Cuda:9.0-Base-ubuntu16.04](https://hub.docker.com/r/nvidia/cuda/)
  * [tensorflow/tensorflow: 1.12.0-gpu-py3](https://hub.docker.com/r/tensorflow/tensorflow)
    
## <a name="yaml-example"></a>YAML örneği

GPU kaynakları eklemek için bir yol olan bir kapsayıcı grubu kullanarak dağıtmak için bir [YAML dosyası](container-instances-multi-container-yaml.md). Adlı yeni bir dosyaya aşağıdaki YAML'ye kopyalayın *gpu dağıtma aci.yaml*, ardından dosyayı kaydedin. Adlı bir kapsayıcı grubu bu YAML oluşturur *gpucontainergroup* K80 GPU ile bir kapsayıcı örneği belirtme. Örneği, örnek CUDA vektör toplama uygulaması çalışır. İş yükü çalıştırmak için yeterli kaynak isteklerdir.

```YAML
additional_properties: {}
apiVersion: '2018-10-01'
name: gpucontainergroup
properties:
  containers:
  - name: gpucontainer
    properties:
      image: k8s-gcrio.azureedge.net/cuda-vector-add:v0.1
      resources:
        requests:
          cpu: 1.0
          memoryInGB: 1.5
          gpu:
            count: 1
            sku: K80
  osType: Linux
  restartPolicy: OnFailure
```

Kapsayıcı grubu dağıtma [az kapsayıcı oluşturma] [ az-container-create] YAML dosyası adını belirterek komutu `--file` parametresi. Bir kaynak grubu ve kapsayıcı grubu için bir konum adı gibi sağlamanız gereken *eastus* , GPU kaynakları destekler.  

```azurecli
az container create --resource-group myResourceGroup --file gpu-deploy-aci.yaml --location eastus
```

Dağıtımın tamamlanması birkaç dakika sürer. Ardından, kapsayıcı başlar ve CUDA vektör toplama işlemi çalıştırır. Çalıştırma [az kapsayıcı günlüklerini] [ az-container-logs] günlük çıktısını görmek için komutu:

```azurecli
az container logs --resource-group myResourceGroup --name gpucontainergroup --container-name gpucontainer
```

Çıkış:

```Console
[Vector addition of 50000 elements]
Copy input data from the host memory to the CUDA device
CUDA kernel launch with 196 blocks of 256 threads
Copy output data from the CUDA device to the host memory
Test PASSED
Done
```

## <a name="resource-manager-template-example"></a>Resource Manager şablonu örneği

Bir kapsayıcı grubu GPU kaynakları dağıtmak için başka bir yolu kullanmaktır bir [Resource Manager şablonu](container-instances-multi-container-group.md). Adlı bir dosya oluşturarak başlayın `gpudeploy.json`, içine aşağıdaki JSON kopyalayın. Bu örnek, bir kapsayıcı örneği çalıştıran bir V100 GPU dağıtır bir [TensorFlow](https://www.tensorflow.org/versions/r1.1/get_started/mnist/beginners) eğitim işini karşı [MNIST dataset](http://yann.lecun.com/exdb/mnist/). İş yükü çalıştırmak için yeterli kaynak isteklerdir.

```JSON
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "containerGroupName": {
        "type": "string",
        "defaultValue": "gpucontainergrouprm",
        "metadata": {
          "description": "Container Group name."
        }
      }
    },
    "variables": {
      "containername": "gpucontainer",
      "containerimage": "microsoft/samples-tf-mnist-demo:gpu"
    },
    "resources": [
      {
        "name": "[parameters('containerGroupName')]",
        "type": "Microsoft.ContainerInstance/containerGroups",
        "apiVersion": "2018-10-01",
        "location": "[resourceGroup().location]",
        "properties": {
            "containers": [
            {
              "name": "[variables('containername')]",
              "properties": {
                "image": "[variables('containerimage')]",
                "resources": {
                  "requests": {
                    "cpu": 4.0,
                    "memoryInGb": 12.0,
                    "gpu": {
                        "count": 1,
                        "sku": "V100"
                  }
                }
              }
            }
          }
        ],
        "osType": "Linux",
        "restartPolicy": "OnFailure"
        }
      }
    ]
}
```

Şablon ile dağıtım [az grubu dağıtım oluşturma] [ az-group-deployment-create] komutu. Gibi bir bölgede oluşturulan bir kaynak grubu adı sağlamanız gereken *eastus* , GPU kaynakları destekler.

```azurecli-interactive
az group deployment create --resource-group myResourceGroup --template-file gpudeploy.json
```

Dağıtımın tamamlanması birkaç dakika sürer. Ardından, kapsayıcıyı başlar ve TensorFlow işi çalıştırır. Çalıştırma [az kapsayıcı günlüklerini] [ az-container-logs] günlük çıktısını görmek için komutu:

```azurecli
az container logs --resource-group myResourceGroup --name gpucontainergroup --container-name gpucontainer
```

Çıkış:

```Console
2018-10-25 18:31:10.155010: I tensorflow/core/platform/cpu_feature_guard.cc:137] Your CPU supports instructions that this TensorFlow binary was not compiled to use: SSE4.1 SSE4.2 AVX AVX2 FMA
2018-10-25 18:31:10.305937: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1030] Found device 0 with properties:
name: Tesla K80 major: 3 minor: 7 memoryClockRate(GHz): 0.8235
pciBusID: ccb6:00:00.0
totalMemory: 11.92GiB freeMemory: 11.85GiB
2018-10-25 18:31:10.305981: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1120] Creating TensorFlow device (/device:GPU:0) -> (device: 0, name: Tesla K80, pci bus id: ccb6:00:00.0, compute capability: 3.7)
2018-10-25 18:31:14.941723: I tensorflow/stream_executor/dso_loader.cc:139] successfully opened CUDA library libcupti.so.8.0 locally
Successfully downloaded train-images-idx3-ubyte.gz 9912422 bytes.
Extracting /tmp/tensorflow/input_data/train-images-idx3-ubyte.gz
Successfully downloaded train-labels-idx1-ubyte.gz 28881 bytes.
Extracting /tmp/tensorflow/input_data/train-labels-idx1-ubyte.gz
Successfully downloaded t10k-images-idx3-ubyte.gz 1648877 bytes.
Extracting /tmp/tensorflow/input_data/t10k-images-idx3-ubyte.gz
Successfully downloaded t10k-labels-idx1-ubyte.gz 4542 bytes.
Extracting /tmp/tensorflow/input_data/t10k-labels-idx1-ubyte.gz
Accuracy at step 0: 0.097
Accuracy at step 10: 0.6993
Accuracy at step 20: 0.8208
Accuracy at step 30: 0.8594
...
Accuracy at step 990: 0.969
Adding run metadata for 999
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

GPU kaynakları kullanan pahalı olabileceği için uzun süre boyunca kapsayıcılarınızı beklenmedik bir şekilde çalıştırma emin olun. Azure portalında kapsayıcılarınızı izleyecek veya bir kapsayıcı grubu durumunu [az container show] [ az-container-show] komutu. Örneğin:

```azurecli
az container show --resource-group myResourceGroup --name gpucontainergroup --output table
```

İşiniz bittiğinde kapsayıcı örnekleri ile çalışma, oluşturduğunuz ve aşağıdaki komutlarla silin:

```azurecli
az container delete --resource-group myResourceGroup --name gpucontainergroup -y
az container delete --resource-group myResourceGroup --name gpucontainergrouprm -y
```

## <a name="next-steps"></a>Sonraki adımlar

* Kullanarak bir kapsayıcı grubu dağıtma hakkında daha fazla bilgi bir [YAML dosyası](container-instances-multi-container-yaml.md) veya [Resource Manager şablonu](container-instances-multi-container-group.md).
* Daha fazla bilgi edinin [GPU için iyileştirilmiş sanal makine boyutlarını](../virtual-machines/linux/sizes-gpu.md) azure'da.


<!-- IMAGES -->
[aci-vnet-01]: ./media/container-instances-vnet/aci-vnet-01.png

<!-- LINKS - External -->
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container#az-container-create
[az-container-show]: /cli/azure/container#az-container-show
[az-container-logs]: /cli/azure/container#az-container-logs
[az-container-show]: /cli/azure/container#az-container-show
[az-group-deployment-create]: /cli/azure/group/deployment#az-group-deployment-create
