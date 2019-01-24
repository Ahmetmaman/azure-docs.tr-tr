---
title: Yönetme, kaydetme, dağıtma ve ML modelleri izleyin
titleSuffix: Azure Machine Learning service
description: Azure Machine Learning hizmetini dağıtmak, yönetmek ve sürekli olarak geliştirmek için Modellerinizi izleme için kullanmayı öğrenin. Yerel makinenizde veya diğer kaynaklardan Azure Machine Learning hizmeti ile eğitilmiş modeller dağıtabilirsiniz.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
author: chris-lauren
ms.author: clauren
ms.date: 09/24/2018
ms.custom: seodec18
ms.openlocfilehash: 44f61d7b90018b76b1903a04d219dcf0226f95e0
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54852329"
---
# <a name="manage-deploy-and-monitor-models-with-azure-machine-learning-service"></a>Yönetin, dağıtın ve modeller Azure Machine Learning hizmeti ile izleme

Bu makalede, Azure Machine Learning hizmeti dağıtma, yönetme ve sürekli olarak geliştirmek için Modellerinizi izlemek için nasıl kullanılacağını öğrenebilirsiniz. Azure Machine Learning ile yerel makinenizde veya diğer kaynaklardan eğitilmiş modeller dağıtabilirsiniz. 

Tam dağıtım iş akışı aşağıdaki diyagramda gösterilmektedir: [ ![Azure Machine Learning için dağıtım iş akışı](media/concept-model-management-and-deployment/deployment-pipeline.png) ](media/concept-model-management-and-deployment/deployment-pipeline.png#lightbox)

Dağıtım iş akışı, aşağıdaki adımları içerir:
1. **Modeli kaydetmeyi** , Azure Machine Learning hizmeti çalışma alanında barındırılan bir kayıt defterinde
1. **Bir görüntüyü kaydedin** , bir model Puanlama betiğine ve taşınabilir bir kapsayıcıda bağımlılıkları ile eşleşmesini 
1. **Dağıtma** bulutta veya uç cihazlarında bir web hizmeti olarak görüntüsü
1. **İzleme ve veri toplama**
1. **Güncelleştirme** yeni görüntüyü kullanarak bir dağıtım.

Her adım, bağımsız olarak veya tek dağıtım komutun bir parçası olarak gerçekleştirilebilir. Ayrıca, dağıtımı ile tümleştirebilirsiniz bir **CI/CD iş akışı** Bu grafikte gösterildiği gibi.

[ !['Azure Machine Learning sürekli tümleştirme/sürekli dağıtım (CI/CD) döngüsü'](media/concept-model-management-and-deployment/model-ci-cd.png) ](media/concept-model-management-and-deployment/model-ci-cd.png#lightbox)

## <a name="step-1-register-model"></a>1. Adım: Modeli kaydetme

Model kaydı, depolamanızı ve sürüm Modellerinizi çalışma alanınızda Azure bulutunda sağlar. Model kayıt defterini düzenlemek ve eğitilen Modellerinizi izlemek kolaylaştırır.
 
Kayıtlı modelleri, ada ve sürüme göre tanımlanır. Mevcut bir aynı ada sahip bir model her kaydettirdiğinizde, kayıt defteri sürüm artırır. Ek meta veri etiketleri aramak modellerinde kullanılabilir kayıt sırasında de sağlayabilirsiniz. Azure Machine Learning hizmetini kullanarak Python 3 yüklenen herhangi bir model destekler. 

Görüntü tarafından kullanılmakta olan modeller nelze odstranit.

Daha fazla bilgi için kayıt modeli bölümüne bakın. [modelleri dağıtma](how-to-deploy-and-where.md#registermodel).

Bir model pickle biçiminde depolanan kaydetme ilişkin bir örnek için bkz [Öğreticisi: Bir görüntü sınıflandırma modeli eğitme](tutorial-deploy-models-with-aml.md).

ONNX modelleri kullanma hakkında daha fazla bilgi için bkz. [ONNX ve Azure Machine Learning](how-to-build-deploy-onnx.md) belge.

## <a name="step-2-register-image"></a>2. Adım: Görüntüyü kaydedin

Model kullanmak için gereken tüm bileşenler birlikte güvenilir model dağıtımı için görüntüler sağlar. Görüntü, aşağıdaki öğeleri içerir:

* Model
* Puanlama altyapısı
* Puanlama dosyası veya uygulama
* Modeli Puanlama için gereken tüm bağımlılıkları

Görüntü ayrıca günlüğe kaydetme ve izleme için SDK bileşenleri içerir. SDK'sı günlükleri verilerini ince ayar yapma veya giriş ve çıkış modeli de dahil olmak üzere modelinizi yeniden eğitme için kullanılabilir.

Azure Machine Learning en popüler çerçeveleri destekler, ancak genel pıp'in yüklü olan herhangi bir çerçeveyi çalışabilir.

Bu nedenle çalışma alanınızı oluştururken diğer birçok diğer Azure kaynakları bu çalışma alanı tarafından kullanıldı.
Görüntüyü oluşturmak için kullanılan tüm nesneler, çalışma alanınızda Azure depolama hesabında depolanır. Ek meta veri etiketleri, görüntü oluştururken sağlayabilir. Meta veri etiketleri de görüntü kayıt depolanır ve görüntünüzü bulmak için sorgulanabilir.

Daha fazla bilgi için bkz. configure ve görüntü bölümünü kaydetmek [modelleri dağıtma](how-to-deploy-and-where.md#configureimage).

## <a name="step-3-deploy-image"></a>3. Adım: Görüntüsü dağıtma

Buluta veya uç cihazlarında kayıtlı görüntülerini dağıtabilirsiniz. Dağıtım işlemi modeliniz izlemek için Yük Dengeleme ve otomatik ölçeklendirme gereken tüm kaynakları oluşturur. Dağıtılan hizmetlere erişmek için sertifika tabanlı kimlik doğrulama ile güvenli hale getirilebilir dağıtım sırasında güvenlik varlıklar sağlayarak. Ayrıca, daha yeni bir görüntüyü kullanmak için mevcut bir dağıtımı yükseltebilirsiniz.

Web hizmeti dağıtımları da aranabilir. Örneğin, belirli bir model veya görüntü tüm dağıtımları için arama yapabilirsiniz.

[ ![Çıkarım hedefleri](media/concept-model-management-and-deployment/inferencing-targets.png) ](media/concept-model-management-and-deployment/inferencing-targets.png#lightbox)

Aşağıdaki dağıtım hedefleri bulutta görüntülerinizi dağıtabilirsiniz:

* Azure Container Örneği
* Azure Kubernetes Service
* Azure FPGA makineler
* Azure IOT Edge cihazları

Hizmetinizi dağıtılan gibi çıkarım isteği otomatik olarak yük dengeli ve isteğe bağlı herhangi bir ani artış karşılamak için küme Ölçeklendirildi. [Hizmetinizi hakkında telemetri tutulabilir](how-to-enable-app-insights.md) çalışma alanınızla ilişkili Azure Application Insights hizmetine.

Daha fazla bilgi için dağıtma bölümüne bakın. [modelleri dağıtma](how-to-deploy-and-where.md#deploy).

## <a name="step-4-monitor-models-and-collect-data"></a>4. Adım: İzleyici modeller ve veri toplama

Giriş, çıkış ve diğer ilgili veri modelinizden izleyebilmek model günlük ve veri yakalama için bir SDK'sı kullanılabilir. Veriler, çalışma alanınız için Azure depolama hesabındaki bir blob olarak depolanır.

Modelinizi SDK'yı kullanmak için SDK, Puanlama betik veya uygulamanızı almanız gerekir. Ardından SDK parametre sonuçları ya da giriş ayrıntıları gibi verileri günlüğe kaydetmek için de kullanabilirsiniz.

Görüntüyü dağıtmak her zaman model verisi toplamayı etkinleştir karar verirseniz, kimlik bilgileri, kişisel blob mağazası gibi verilerini yakalamak için gerekli ayrıntıları otomatik olarak sağlanır.

> [!Important]
> Microsoft modelinizden topladığı verileri görmez. Çalışma alanınız için Azure depolama hesabına doğrudan bir veri gönderilmedi.

Daha fazla bilgi için [model verileri toplamayı etkinleştirme](how-to-enable-data-collection.md).

## <a name="step-5-update-the-deployment"></a>5. Adım: Güncelleştirme dağıtımı

Modelinizi güncelleştirmeleri otomatik olarak kayıtlı değil. Benzer şekilde, yeni bir görüntü kayıt otomatik olarak görüntü önceki bir sürümünden oluşturulan dağıtımlar güncelleştirmez. Bunun yerine, el ile modeli kaydedin, görüntüyü kaydetmeniz ve ardından modeli güncelleştirmek gerekir. Daha fazla bilgi için güncelleştirme bölümünü [modelleri dağıtma](how-to-deploy-and-where.md#update).

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinin [nasıl ve nerede modelleri dağıtma](how-to-deploy-and-where.md) Azure Machine Learning hizmeti ile.

İstemci uygulamaları oluşturmayı öğrenin ve Hizmetleri [bir web hizmeti olarak modeli kullanma](how-to-consume-web-service.md).
