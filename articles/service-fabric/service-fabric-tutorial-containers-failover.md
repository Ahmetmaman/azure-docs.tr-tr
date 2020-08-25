---
title: Kapsayıcılar uygulaması için yük devretme ve ölçeklendirme
description: Bu öğreticide, Azure Service Fabric kapsayıcıları uygulamasında yük devretmenin nasıl ele alındığını öğrenirsiniz.  Ayrıca, bir kümede çalışan kapsayıcıları ve hizmetleri ölçeklendirme işleminden de bahsedilir.
author: suhuruli
ms.topic: tutorial
ms.date: 07/22/2019
ms.author: suhuruli
ms.custom: mvc
ms.openlocfilehash: bef1c42712c881296c5ab7b8096deb50fe94ee55
ms.sourcegitcommit: c5021f2095e25750eb34fd0b866adf5d81d56c3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/25/2020
ms.locfileid: "75614103"
---
# <a name="tutorial-demonstrate-fail-over-and-scaling-of-container-services-with-service-fabric"></a>Öğretici: Service Fabric ile kapsayıcı hizmetlerinin yük devretme ve ölçeklendirme işlemlerini gösterir

Bu öğretici, bir serinin üçüncü bölümüdür. Bu öğreticide, Service Fabric kapsayıcı uygulamalarında yük devretmenin nasıl ele alındığını öğrenirsiniz. Ayrıca, kapsayıcı ölçeklendirme işleminden de bahsedilir. Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Service Fabric kümesindeki bir kapsayıcıda yük devretme ile ilgili daha fazla bilgi edinin
> * Uygulamadaki web ön uç kapsayıcılarını ölçeklendirme

## <a name="prerequisites"></a>Önkoşullar

[Kısım 2](service-fabric-tutorial-package-containers.md)’deki uygulama, etkin bir Service Fabric kümesinde çalışıyor.

## <a name="fail-over-a-container-in-a-cluster"></a>Kümedeki bir kapsayıcıya yük devretme

Service Fabric, kapsayıcı örneklerinizin bir arıza durumunda kümedeki diğer düğümlere otomatik olarak taşınmasını sağlar. Bir düğümü kapsayıcılar için el ile boşaltabilir ve kümedeki diğer düğümlere taşıyabilirsiniz. Hizmetlerinizi ölçeklendirmek için kullanabileceğiniz birden fazla yöntem vardır. Bu örnekte Service Fabric Explorer'ı kullanacağız.

Ön uç kapsayıcısında yük devretmek için aşağıdaki adımları uygulayın:

1. Kümenizde Service Fabric Explorer'ı açın. Örneğin: `http://lin4hjim3l4.westus.cloudapp.azure.com:19080`.
2. Ağaç görünümünde **fabric:/TestContainer/azurevotefront** düğümüne tıklayın ve bölüm düğümünü (GUID ile gösterilir) genişletin. Ağaç görünümünde kapsayıcının üzerinde çalıştığı düğümleri gösteren düğüm adına dikkat edin. Örneğin: `_nodetype_1`
3. Ağaç görünümünde **Düğümler** düğümünü genişletin. Kapsayıcıyı çalıştıran düğümün yanındaki üç noktaya tıklayın.
4. İlgili düğümü yeniden başlatmak için **Yeniden Başlat**'ı seçin ve yeniden başlatma eylemini onaylayın. Yeniden başlatma durumunda kapsayıcıdan kümedeki başka bir düğüme yük devretme gerçekleştirilir.

![noderestart][noderestart]

Düğüm adının (ön uç kapsayıcılarının çalıştığı yeri belirtir), kapsayıcıdaki başka bir düğümle nasıl değiştiğini göz önünde bulundurun. Bir süre sonra uygulamaya yeniden gözatabilir ve uygulamanın başka bir düğümde çalışır durumda olduğunu görebilirsiniz.

## <a name="scale-containers-and-services-in-a-cluster"></a>Bir kümedeki kapsayıcıları ve hizmetleri ölçeklendirme

Hizmet yükünü karşılamak için bir kümedeki Service Fabric kapsayıcıları ölçeklendirilebilir. Kümede çalıştırılan örnek sayısını değiştirerek bir kapsayıcıyı ölçeklendirebilirsiniz.

Web ön ucunu ölçeklendirmek için aşağıdaki adımları gerçekleştirin:

1. Kümenizde Service Fabric Explorer'ı açın. Örneğin: `http://lin4hjim3l4.westus.cloudapp.azure.com:19080`.
2. Ağaç görünümünde **fabric:/TestContainer/azurevotefront** düğümünün yanındaki üç noktaya tıklayın ve **Hizmeti Ölçeklendir**'i seçin.

![sfxscale][sfxscale]

Şimdi web ön ucundaki örnek sayısını ölçeklendirebilirsiniz.

1. Rakamı **2** olarak değiştirin ve **Hizmeti Ölçeklendir**'e tıklayın.
1. Ağaç görünümünde **fabric:/TestContainer/azurevotefront** düğümüne tıklayın ve bölüm düğümünü (GUID ile gösterilir) genişletin.

![sfxscaledone][sfxscaledone]

Hizmetin artık iki örneği olduğunu görebilirsiniz. Ağaç görünümünde örneklerin üzerinde çalıştığı düğümleri görürsünüz.

Bu basit yönetim görevi sayesinde ön uç hizmetinizde kullanıcı yükünü işleyecek kaynak sayısını iki katına çıkarmış olduk. Bir hizmetin güvenilir bir şekilde çalışması için birden fazla örneğe ihtiyaç duymadığını anlamanız önemlidir. Bir hizmet başarısız olursa Service Fabric kümede yeni bir hizmet örneği çalışmasını sağlar.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, kapsayıcıda yük devremenin yanı sıra uygulama ölçeklendirme işlemleri gösterilmiştir. Aşağıdaki adımlar tamamlandı:

> [!div class="checklist"]
> * Service Fabric kümesindeki bir kapsayıcıda yük devretme ile ilgili daha fazla bilgi edinin
> * Uygulamadaki web ön uç kapsayıcılarını ölçeklendirme

Bu öğretici serisinde, şunların nasıl yapıldığını öğrendiniz:
> [!div class="checklist"]
> * Kapsayıcı görüntüleri oluşturma
> * Kapsayıcı görüntülerini Azure Container Registry’ye gönderme
> * Yeoman kullanan Service Fabric için Paket Kapsayıcıları
> * Kapsayıcılara Sahip bir Service Fabric Uygulaması Derleme ve Çalıştırma
> * Service Fabric’de yük devretme ve ölçeklendirme nasıl işlenir?

[noderestart]: ./media/service-fabric-tutorial-containers-failover/containersfailovertutorialnoderestart.png
[sfxscale]: ./media/service-fabric-tutorial-containers-failover/containersfailovertutorialscale.png
[sfxscaledone]: ./media/service-fabric-tutorial-containers-failover/containersfailovertutorialscaledone.png