---
title: Azure Ölçüm Gezgini 'ni kullanmaya başlama
description: Azure Ölçüm Gezgini ile ilk ölçüm grafiğinizi oluşturmayı öğrenin.
author: vgorbenko
services: azure-monitor
ms.topic: conceptual
ms.date: 02/25/2019
ms.author: vitalyg
ms.subservice: metrics
ms.openlocfilehash: e31eb8deb3102ab03809f01e33b1e6548113dae4
ms.sourcegitcommit: 78ecfbc831405e8d0f932c9aafcdf59589f81978
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2021
ms.locfileid: "98736398"
---
# <a name="getting-started-with-azure-metrics-explorer"></a>Azure Ölçüm Gezgini'ni kullanmaya başlama

## <a name="where-do-i-start"></a>Nereden başlayabilirim?
Azure Izleyici Ölçüm Gezgini, grafikleri çizdirme, eğilimleri görsel olarak ilişkilendirme ve ölçüm değerlerinde ani artışları ve DIP 'leri araştırmanıza olanak tanıyan Microsoft Azure portal bir bileşenidir. Kaynaklarınızın sistem durumunu ve kullanımını araştırmak için ölçüm gezginini kullanın. Aşağıdaki sırada başlatın:

1. [Bir kaynak ve ölçüm seçin](#create-your-first-metric-chart) ve temel bir grafik görürsünüz. Daha sonra araştırmanıza uygun [bir zaman aralığı seçin](#select-a-time-range) .

1. [Boyut filtrelerini uygulamayı ve bölmeyi](#apply-dimension-filters-and-splitting)deneyin. Filtreler ve bölme, ölçüm segmentlerinin genel ölçüm değerine katkıda bulunduğunu analiz etmenize ve olası aykırı değerleri tespit etmenize olanak tanır.

1. Panoya sabitlemeden önce grafiği özelleştirmek için [Gelişmiş ayarları](#advanced-chart-settings) kullanın. Ölçüm değeri bir eşiğin altına çıktığında veya altına düştüğünde bildirimleri almak için [uyarıları yapılandırın](alerts-metric-overview.md) .

## <a name="create-your-first-metric-chart"></a>İlk ölçüm grafiğinizi oluşturma

Bir ölçüm grafiği oluşturmak için, kaynak, kaynak grubu, abonelik veya Azure Izleyici görünümünden **ölçümler** sekmesini açın ve şu adımları izleyin:

1. Kaynak kapsamı seçicisini açmak için "kapsam seçin" düğmesine tıklayın. Bu, ölçümlerini görmek istediğiniz kaynakları seçmenizi sağlar. Kaynak menüsünden Ölçüm Gezgini 'ni açtıysanız kaynak zaten doldurulmalıdır. Birden çok kaynak arasında ölçümleri görüntülemeyi öğrenmek için [Bu makaleyi okuyun](./metrics-dynamic-scope.md).
    > ![Bir kaynak seçin](./media/metrics-getting-started/scope-picker.png)

2. Bazı kaynaklar için bir ad alanı seçmelisiniz. Ad alanı, ölçümleri kolayca bulabilmeniz için, ölçümleri düzenlemenin bir yoludur. Örneğin, depolama hesaplarının dosya, tablo, blob 'Lar ve sıra ölçümlerini depolamak için ayrı ad alanları vardır. Birçok kaynak türünün yalnızca bir ad alanı vardır.

3. Kullanılabilir ölçümler listesinden bir ölçüm seçin.

    > ![Ölçüm seçin](./media/metrics-getting-started/metrics-dropdown.png)

4. İsteğe bağlı olarak, [ölçüm toplamayı değiştirebilirsiniz](metrics-charts.md#aggregation). Örneğin, grafiğinizin ölçüm minimum, maksimum veya Ortalama değerlerini göstermesini isteyebilirsiniz.

> [!TIP]
> Aynı grafikte birden çok ölçüm çizilmek istiyorsanız **ölçüm Ekle** düğmesini kullanın ve bu adımları tekrarlayın. Tek bir görünümdeki birden çok grafik için üstteki **Grafik Ekle** düğmesini seçin.

## <a name="select-a-time-range"></a>Bir zaman aralığı seçin

> [!WARNING]
> [Azure'daki ölçümlerin çoğu 93 gün boyunca depolanır](data-platform-metrics.md#retention-of-metrics). Ancak, herhangi bir grafik üzerinde en fazla 30 günlük veri girebilirsiniz. Bu sınırlama [günlük tabanlı ölçümler](../app/pre-aggregated-metrics-log-metrics.md#log-based-metrics) için geçerli değildir.

Varsayılan olarak, grafik en son 24 saat ölçüm verilerini gösterir. Zaman aralığını değiştirmek, grafiğinizde yakınlaştırmak veya uzaklaştırmak için **zaman Seçicisi** bölmesini kullanın. 

![Zaman aralığı Bölmesini Değiştir](./media/metrics-getting-started/time.png)

> [!TIP]
> Grafiğin ilgi çekici bir alanını (ani veya bir DIP) araştırmak için **zaman fırçası** 'nı kullanın. Fare işaretçisini alanın başlangıcına koyun, sol fare düğmesine tıklayın ve basılı tutun, alanın diğer tarafına sürükleyin ve sonra düğmeyi bırakın. Grafik bu zaman aralığında yakınlaşacaktır. 

## <a name="apply-dimension-filters-and-splitting"></a>Boyut filtrelerini uygulama ve bölme

[Filtreleme](metrics-charts.md#filters) ve [bölme](metrics-charts.md#apply-splitting) boyutları olan ölçümler için güçlü tanılama araçlarıdır. Bu özellikler çeşitli ölçüm segmentlerinin ("boyut değerleri") ölçümün genel değerini nasıl etkilediğini gösterir ve olası aykırı değerleri tanımlamanızı sağlar.

- **Filtreleme** , grafiğe hangi boyut değerlerinin ekleneceğini seçmenizi sağlar. Örneğin, *sunucu yanıt süresi* ölçümünü grafik oluştururken başarılı istekleri göstermek isteyebilirsiniz. Filtreyi *istek boyutunun başarısına* uygulamanız gerekir. 

- Grafiğe, grafiğin her bir boyutun değeri için ayrı satırlar görüntüleyip görüntülemediğini veya değerleri tek bir satıra toplamayacağını **denetler.** Örneğin, tüm sunucu örneklerinde ortalama yanıt süresi için bir satır görebilir veya her bir sunucu için ayrı satırlara bakabilirsiniz. Ayrı satırları görmek için *sunucu örneği* boyutunda bölme uygulamanız gerekir.

Filtreleme ve bölme uygulanmış [grafiklerin örneklerine](metric-chart-samples.md) bakın. Makale, grafikleri yapılandırmak için kullanılan adımları gösterir.

## <a name="advanced-chart-settings"></a>Gelişmiş grafik ayarları

Grafik stilini, başlığı özelleştirebilir ve gelişmiş grafik ayarlarını değiştirebilirsiniz. Özelleştirme ile işiniz bittiğinde çalışmanızı kaydetmek için bir panoya sabitleyin. Ayrıca, ölçüm uyarılarını yapılandırabilirsiniz. Azure Izleyici Ölçüm Gezgini 'nin bu ve diğer gelişmiş özellikleri hakkında bilgi edinmek için [ürün belgelerini](metrics-charts.md) izleyin.

## <a name="next-steps"></a>Sonraki adımlar

* [Ölçüm Gezgini gelişmiş özellikleri hakkında bilgi edinin](metrics-charts.md)
* [Ölçüm Gezgini birden çok kaynağı görüntüleme](metrics-dynamic-scope.md)
* [Ölçüm Gezgini sorunlarını giderme](metrics-troubleshoot.md)
* [Azure hizmetleri için kullanılabilir ölçümlerin listesine bakın](metrics-supported.md)
* [Yapılandırılmış grafik örneklerine bakın](metric-chart-samples.md)