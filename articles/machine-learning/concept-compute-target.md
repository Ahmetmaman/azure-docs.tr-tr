---
title: İşlem hedefleri nelerdir?
titleSuffix: Azure Machine Learning
description: Azure Machine Learning modelinize eğmesini veya dağıtmayı istediğinizi tanımlayın.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sgilley
author: sdgilley
ms.date: 07/27/2020
ms.openlocfilehash: 3e1832d656ea2bad772a686e52a357c1daf0ded5
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90883201"
---
#  <a name="what-are-compute-targets-in-azure-machine-learning"></a>Azure Machine Learning işlem hedefleri nelerdir? 

**İşlem hedefi** , eğitim betiğinizi çalıştırdığınız veya hizmet dağıtımınızı barındırabileceğiniz, belirlenen bir işlem kaynağıdır/ortamıdır. Bu konum, yerel makineniz veya bulut tabanlı bir işlem kaynağıdır. İşlem hedeflerini kullanmak, kodunuzu değiştirmek zorunda kalmadan daha sonra işlem ortamınızı değiştirmenizi kolaylaştırır.  

Tipik bir model geliştirme yaşam döngüsünde şunları yapabilirsiniz:
1. Daha az miktarda veri geliştirme ve deneme ile başlayın. Bu aşamada, işlem hedefi olarak yerel ortamınızı (yerel bilgisayar veya bulut tabanlı VM) öneririz. 
2. Daha büyük verilere kadar ölçeklendirin ve bu [eğitim işlem hedeflerinden](#train)birini kullanarak dağıtılmış eğitim yapın.  
3. Modeliniz hazırlandıktan sonra, bu [dağıtım işlem hedeflerinin](#deploy)birine sahip bir Web barındırma ortamına veya IoT cihazına dağıtın.

İşlem hedeflerinizin kullanacağı işlem kaynakları bir [çalışma alanına](concept-workspace.md)eklenir. Yerel makine dışında işlem kaynakları, çalışma alanının kullanıcıları tarafından paylaşılır.

## <a name="training-compute-targets"></a><a name="train"></a> Eğitim işlem hedefleri

Azure Machine Learning, farklı işlem kaynakları arasında farklı destek içerir.  Ayrıca, çeşitli senaryolar için destek farklılık gösterebilse de kendi işlem kaynağınızı ekleyebilirsiniz.

[!INCLUDE [aml-compute-target-train](../../includes/aml-compute-target-train.md)]

[Model eğitimi için bir işlem hedefi kullanma](how-to-set-up-training-targets.md)hakkında daha fazla bilgi edinin.

## <a name="deployment-targets"></a><a name="deploy"></a>Dağıtım hedefleri

Aşağıdaki işlem kaynakları, model dağıtımınızı barındırmak için kullanılabilir.

[!INCLUDE [aml-compute-target-deploy](../../includes/aml-compute-target-deploy.md)]

[Modelinizin bir işlem hedefine dağıtımını nerede ve nasıl dağıtacağınızı](how-to-deploy-and-where.md)öğrenin.

<a name="amlcompute"></a>
## <a name="azure-machine-learning-compute-managed"></a>Azure Machine Learning işlem (yönetilen)

Yönetilen bir işlem kaynağı Azure Machine Learning tarafından oluşturulur ve yönetilir. Bu işlem makine öğrenimi iş yükleri için iyileştirilmiştir. Azure Machine Learning işlem kümeleri ve [işlem örnekleri](concept-compute-instance.md) yalnızca yönetilen hesaplalardır. 

Azure Machine Learning işlem örnekleri veya işlem kümeleri oluşturabilirsiniz:
* [Azure Machine Learning Studio](how-to-create-attach-compute-studio.md)
* Azure portal
* Python SDK [ComputeInstance](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.computeinstance(class)?view=azure-ml-py&preserve-view=true) ve [amlcompute](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.amlcompute(class)?view=azure-ml-py&preserve-view=true) sınıfları
* [R SDK](https://azure.github.io/azureml-sdk-for-r/reference/index.html#section-compute-targets) (Önizleme)
* Kaynak Yöneticisi şablonu. Örnek bir şablon için [create Azure Machine Learning işlem şablonuna](https://github.com/Azure/azure-quickstart-templates/tree/master/101-machine-learning-compute-create-amlcompute)bakın.
* [Azure CLI için](reference-azure-machine-learning-cli.md#resource-management)Machine Learning uzantısı.  

Bu işlem kaynakları oluşturulduğunda, diğer tür bilgi işlem hedeflerinin aksine, çalışma alanınızın otomatik olarak bir parçasıdır.


|Özellik  |İşlem kümesi  |İşlem örneği  |
|---------|---------|---------|
|Tek veya çok düğümlü küme     |    **&check;**       |         |
|Her çalıştırma gönderdiğinizde otomatik ölçekler     |     **&check;**      |         |
|Otomatik küme yönetimi ve iş zamanlaması     |   **&check;**        |     **&check;**      |
|Hem CPU hem de GPU kaynakları için destek     |  **&check;**         |    **&check;**       |


> [!NOTE]
> Bir işlem kümesi boşta olduğunda, otomatik olarak 0 düğüme ölçeklenirken, kullanımda olmadığında ödeme yapmayın.  Ancak, bir işlem *örneği*her zaman açık olur ve otomatik ölçeklendirme yapmaz.  Ek maliyetten kaçınmak için, bunu kullanmadığınız durumlarda [işlem örneğini durdurmanız](concept-compute-instance.md#managing-a-compute-instance) gerekir. 

### <a name="supported-vm-series-and-sizes"></a>Desteklenen VM Serisi ve boyutları

Azure Machine Learning yönetilen bir işlem kaynağı için bir düğüm boyutu seçtiğinizde, Azure 'da kullanılabilir olan VM boyutları arasından seçim yapabilirsiniz. Azure, farklı iş yükleri için Linux ve Windows için bir dizi boyut sunar. Farklı [VM türleri ve boyutları](https://docs.microsoft.com/azure/virtual-machines/linux/sizes)hakkında daha fazla bilgi edinmek için buraya bakın.

VM boyutu seçmenin bazı özel durumları ve sınırlamaları vardır:
* Bazı VM serileri Azure Machine Learning desteklenmez.
* Bazı VM dizileri kısıtlıdır. Kısıtlı bir seriyi kullanmak için desteğe başvurun ve seri için bir kota artışı isteyin. Desteğe başvurma hakkında daha fazla bilgi için bkz. [Azure destek seçenekleri](https://azure.microsoft.com/support/options/)

Desteklenen seriler ve kısıtlamalar hakkında daha fazla bilgi edinmek için aşağıdaki tabloya bakın. 

| **Desteklenen VM Serisi**  | **Kısıtlamalar** |
|------------|------------|
| D | Yok |
| Dv2 | Yok |  
| DSv2 | Yok |  
| FSv2 | Yok | 
| HBv2 | Onay gerekiyor |  
| HCS | Onay gerekiyor |  
| M | Onay gerekiyor |
| NC | Yok |    
| NCsv2 | Onay gerekiyor |
| NCsv3 | Onay gerekiyor |  
| Üstündeki | Onay gerekiyor |
| NDv2 | Onay gerekiyor |
| NV | Yok |
| NVv3 | Onay gerekiyor | 


Azure Machine Learning bu VM serisini desteklese de, tüm Azure bölgelerinde bulunmayabilir. Burada kullanılabilir VM serisine bakabilirsiniz: [bölgeye göre kullanılabilir ürünler](https://azure.microsoft.com/global-infrastructure/services/?products=virtual-machines).

## <a name="unmanaged-compute"></a>Yönetilmeyen işlem

Yönetilmeyen bir işlem hedefi Azure Machine Learning *tarafından yönetilmiyor.* Bu tür bir işlem hedefini Azure Machine Learning dışında oluşturun ve ardından çalışma alanınıza ekleyin. Yönetilmeyen işlem kaynakları, makine öğrenimi iş yüklerinin performansını korumanız veya iyileştirebilmeniz için ek adımlar gerektirebilir.

## <a name="next-steps"></a>Sonraki adımlar

Şunları nasıl yapacağınızı öğrenin:
* [Modelinizi eğitme bir işlem hedefi kullanın](how-to-set-up-training-targets.md)
* [Modelinizi bir işlem hedefine dağıtın](how-to-deploy-and-where.md)
