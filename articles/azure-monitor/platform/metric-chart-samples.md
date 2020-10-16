---
title: Azure Izleyici ölçüm grafiği örneği
description: Azure Izleyici verilerinizi görselleştirmeyi öğrenin.
author: vgorbenko
services: azure-monitor
ms.topic: conceptual
ms.date: 01/29/2019
ms.author: vitalyg
ms.subservice: metrics
ms.openlocfilehash: 9b2ab664f319de07fd70bd1a22b1ba6d64ac208f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "87320264"
---
# <a name="metric-chart-examples"></a>Ölçüm grafiği örnekleri 

Azure platformu, çoğu boyut içeren, binlerce [ölçüm üzerinde](./metrics-supported.md)sunulur. [Boyut filtrelerini](./metrics-charts.md)kullanarak, [bölme](./metrics-charts.md), grafik türünü denetleme ve grafik ayarlarını ayarlama yoluyla, altyapınızın ve uygulamalarınızın sistem durumuna ilişkin Öngörüler sağlayan güçlü tanılama görünümleri ve panolar oluşturabilirsiniz. Bu makalede, [Ölçüm Gezgini](./metrics-charts.md) kullanarak oluşturabileceğiniz grafiklerin bazı örnekleri gösterilmektedir ve bu grafiklerin her birini yapılandırmak için gereken adımları açıklar.

Harika grafik örneklerinizi dünya ile paylaşmak ister misiniz? GitHub 'da bu sayfaya katkıda bulunun ve kendi grafik örneklerinizi burada paylaşabilirsiniz!

## <a name="website-cpu-utilization-by-server-instances"></a>Sunucu örneklerine göre Web sitesi CPU kullanımı

Bu grafik, bir App Service için CPU 'nun kabul edilebilir aralık dahilinde olup olmadığını gösterir ve yükün doğru bir şekilde dağıtılıp dağıtılmadığını anlamak için örneğe göre böler. Uygulamanın 6 ' dan önce tek bir sunucu örneğinde çalıştığını ve sonra başka bir örnek ekleyerek ölçeği artırılacağını görebilirsiniz.

![Sunucu örneğine göre ortalama CPU yüzdesi satır grafiği](./media/metric-chart-samples/cpu-by-instance.png)

### <a name="how-to-configure-this-chart"></a>Bu grafik nasıl yapılandırılır?

App Service kaynağını seçin ve **CPU yüzdesi** ölçüsünü bulun. Ardından **bölme Uygula** ' ya tıklayın ve **örnek** boyutunu seçin.

## <a name="application-availability-by-region"></a>Bölgeye göre uygulama kullanılabilirliği

Hangi coğrafi konumların sorun yaşadığını belirlemek için uygulamanızın bölgeye göre kullanılabilirliğini görüntüleyin. Bu grafik Application Insights kullanılabilirlik ölçümünü gösterir. İzlenen uygulamanın Doğu ABD veri merkezinde kullanılabilirliğiyle ilgili bir sorun olmadığını görebilirsiniz, ancak Batı ABD ve Doğu Asya kısmi bir kullanılabilirlik sorunu yaşar.

![Konumlara göre ortalama kullanılabilirlik grafiği](./media/metric-chart-samples/availability-run-location.png)

### <a name="how-to-configure-this-chart"></a>Bu grafik nasıl yapılandırılır?

Önce Web siteniz için [Application Insights kullanılabilirlik](../app/monitor-web-app-availability.md) izlemeyi açmanız gerekir. Bundan sonra Application Insights kaynağını seçin ve kullanılabilirlik ölçümünü seçin. **Çalışma konumu** boyutunda bölme uygulayın.

## <a name="volume-of-storage-account-transactions-by-api-name"></a>API adına göre depolama hesabı işlemleri hacmi

Depolama hesabı kaynağınız fazla miktarda işlem hacimde yaşıyor. Fazla yükün hangi API 'nin sorumlu olduğunu belirlemek için işlem ölçümünü kullanabilirsiniz. Aşağıdaki grafiğin filtrelemede aynı boyut (API adı) ile yapılandırıldığını ve görünümü yalnızca ilgilendiğiniz API çağrılarına daraltmak için böltiğine dikkat edin:

![API işlemlerinin çubuk grafiği](./media/metric-chart-samples/transactions-by-api.png)

### <a name="how-to-configure-this-chart"></a>Bu grafik nasıl yapılandırılır?

Ölçüm seçicide depolama hesabınızı ve **işlem** ölçümünü seçin. Grafik türünü **çubuk grafik**olarak değiştirin. **Bölmeyi Uygula** ' ya tıklayın ve boyut **API 'si adını**seçin. Sonra **Filtre Ekle** ' ye tıklayın ve **API adı** boyutunu bir kez daha seçin. Filtre iletişim kutusunda, grafiğe çizmek istediğiniz API 'Leri seçin.

## <a name="next-steps"></a>Sonraki adımlar

* Azure Izleyici [çalışma kitapları](./workbooks-overview.md) hakkında bilgi edinin
* [Ölçüm Gezgini](metrics-charts.md) hakkında daha fazla bilgi edinin

