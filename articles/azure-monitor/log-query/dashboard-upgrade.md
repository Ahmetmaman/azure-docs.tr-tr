---
title: Log Analytics Pano görselleştirmelerinizi yükseltme
description: Log Analytics Pano görselleştirmelerinizi, güçlü Öngörüler sağlayabilen sorgularla nasıl yükselteceğinizi öğrenin.
ms.subservice: logs
ms.topic: article
author: rboucher
ms.author: robb
ms.date: 07/01/2020
ms.openlocfilehash: a029dcbebf6dfe7a2b6cb517641c824a5937ca95
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "90988249"
---
# <a name="upgrading-your-log-analytics-dashboard-visualizations"></a>Log Analytics Pano görselleştirmelerinizi yükseltme

Şubat 2020 ' de, geliştirilmiş bir görselleştirme teknolojisini sunuyoruz. Sorgu sonuçlarını görselleştirme ve hızlı ve güçlü Öngörüler elde etme yeteneğinizi geliştirme ve geliştirme 

Bu [Azure güncelleştirmesinde](https://azure.microsoft.com/updates/azure-monitor-log-analytics-upgraded-results-visualization/)bu yükseltme hakkında daha fazla bilgi edinebilirsiniz. 

Bu yeni görselleştirme teknolojisi, sorgu sonuç kümesi etrafında yeni ve geliştirilmiş deneyimlerin bir yoludur. 

## <a name="dashboards-in-azure"></a>Azure 'daki panolar

Azure panoları, tüm Azure Surface alanının durumunu görselleştirebileceğiniz bir yoldur. Bunlar, Azure 'un durumu için tek bir cam bölmesi sağlamak ve sık görülen eylemlere yönelik çeşitli kısayollara izin vermek için tasarlanmıştır. 

Daha fazla bilgi için bkz. [Azure panoları](../../azure-portal/azure-portal-dashboards.md)


## <a name="upgrading-log-analytics-dashboard-parts"></a>Log Analytics Pano parçalarını yükseltme

Yeni görselleştirme teknolojisi, eski uygulamayla ilgili bazı yaygın sorunları ele alarak sabitlenmiş Log Analytics parçalara yönelik bazı yeni yetenekler sunar: 

- **Aynı kullanılabilir türler** -Log Analytics bulunan tüm Görselleştirme türleri panolar üzerinde sabitlenmiş parçalar olarak kullanılabilir.

- **Tutarlı görünüm** , sabitlenmiş parçalar için görselleştirme görünümü ve Log Analytics artık neredeyse aynıdır. Farklar, görselin veri içeriklerinde hafif farklar gerektiren iyileştirmelere neden olur. Bu farklılıklara ilişkin daha fazla bilgi için bu belgenin önemli noktaları bölümüne bakın.

- **Araç ipuçları ve Etiketler** – yeni sabitlenmiş Log Analytics görselleştirmeler araç ipuçlarını destekler. Pasta ve halka grafikler artık etiketleri destekliyor.

- **Etkileşimli göstergeler** : görselleştirme göstergesine tıkladığınızda, Log Analytics sabitlenmiş görselden boyutların eklenmesine/kaldırılmasına izin verir.

## <a name="stage-1---opt-in-upgrade-message"></a>1. aşama-katılım yükseltme iletisi

Log Analytics sabitlenmiş bir bölüm yükseltilebilmişse, panolarda Log Analytics sabitlenmiş parçalar üzerinde, kullanıcıların görselleştirmelerini yükseltmesine izin veren yeni bir *katılım* bildirimi görüntülenir. Panoda seçili görselleştirmeleri yükseltmek için yeni görselleştirmelere deneyim vermek istiyorsanız.

 
![Kutusuna](media/dashboard-upgrade/update-message-1.png)
 
![Kutusuna](media/dashboard-upgrade/update-message-2.png)

> [!WARNING]
> Pano yayımlandıktan sonra, yükseltme geri alınamaz. Ancak, yeniden yayımlamadan panodan uzağa giderseniz değişiklikler atılır.  

Tıklandığında görselleştirme yeni teknolojiye güncelleştirilecektir. Görselleştirmedeki hafif farklılıklar, Log Analytics bir görünüm ile hizalanmanız durumunda ortaya çıkabilir.

Görselleştirmeler yükseltildikten sonra, değişikliğin etkili olması için panonuzu yeniden yayımlamanız gerekir.

![Kutusuna](media/dashboard-upgrade/update-message-3.png)

## <a name="stage-2---migration-of-all-dashboards"></a>2. Aşama-tüm panoların geçirilmesi

Bir ilk kabul etme süresi bittikten sonra, Log Analytics ekibi sistemdeki tüm panoları yükseltir. Tüm Azure panoları hizalamak, ekibin Pano genelinde daha fazla görselleştirme ve deneyim geliştirmesi sağlamasına olanak sağlar.

## <a name="considerations"></a>Dikkat edilmesi gerekenler

Panoya sabitlenmiş Log Analytics görselleştirmeler, en iyi deneyim için tasarlanan bazı özel davranışlara sahiptir. Bir görselleştirmeyi panoya sabitleyerek aşağıdaki tasarım konularını gözden geçirin.

### <a name="query-time-scope---30-day-limit"></a>Sorgu süresi kapsamı-30 günlük sınır

Panolar birden çok sorgudan gelen birden çok görselleştirme içerebildiğinden, sabitlenmiş tek sorgunun zaman kapsamı 30 günle sınırlıdır. Tek bir sorgu yalnızca 30 güne eşit veya daha küçük bir zaman aralığı üzerinde çalışabilir. Bu sınırlama, makul bir pano yükleme süresi sağlamaktır.

### <a name="query-data-values---25-values-and-other-grouping"></a>Sorgu verileri değerleri-25 değer ve diğer gruplandırma

Panolar görsel açıdan yoğun ve karmaşık olabilir. Bir panoyu görüntülerken bilişsel yükü azaltmak için, görüntüyü 25 farklı veri türüyle sınırlayarak görselleştirmeleri iyileştirdik. 25 ' ten fazla olduğunda Log Analytics verileri iyileştirir. Bu, çoğu veriyi ayrı olarak en fazla 25 tür gösterir ve ardından kalan değerleri "diğer" bir değere gruplandırır. Aşağıdaki grafikte böyle bir durum gösterilmektedir.  

![Kutusuna](media/dashboard-upgrade/values-25-limit.png)

### <a name="dashboard-refresh-on-load"></a>Yükleme sırasında Pano yenileme

Panolar yük üzerine yenilenir. Pano sabitlenmiş Log Analytics görselleştirmelerle ilgili tüm sorgular yürütülür ve yüklendikten sonra Pano yenilenir. Pano sayfası açık kalırsa, Panodaki veriler her 60 dakikada bir yenilenir.

## <a name="next-steps"></a>Sonraki adımlar

[Log Analytics panoları oluşturma ve paylaşma](../learn/tutorial-logs-dashboards.md)
