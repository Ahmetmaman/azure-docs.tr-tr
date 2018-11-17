---
title: Azure İzleyici ile performans VM'ler (Önizleme) için grafik nasıl | Microsoft Docs
description: Performans, Azure İzleyici sanal makineler için otomatik olarak Windows ve Linux sistemleri üzerindeki uygulama bileşenlerini bulur ve hizmetler arasındaki iletişimi eşleyen bir özelliğidir. Bu makalede, çeşitli senaryoları içinde kullanma hakkında ayrıntılar sağlar.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: ''
ms.service: azure-monitor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/25/2018
ms.author: magoedte
ms.openlocfilehash: f45cf56e0ebaa4a85bd4ece8fd4c727435db0652
ms.sourcegitcommit: a4e4e0236197544569a0a7e34c1c20d071774dd6
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51715930"
---
# <a name="how-to-chart-performance-with-azure-monitor-for-vms-preview"></a>Nasıl yapılır Azure İzleyici ile grafik performansı için Vm'leri (Önizleme)
VM'ler için Azure İzleyici, birkaç ana performans göstergelerini (KPI'lar) bir sanal makineye ne kadar iyi belirlemenize yardımcı olmak için şu gerçekleştiriyor hedefleyen bir dizi performans grafiklerini içerir. Performans sorunlarını, anormallikleri belirlemek ya da seçili ölçüme göre kaynak kullanımını görüntülemek için her bir makine listeleyen bir perspektif geçiş grafikleri bir süre boyunca kaynak kullanımını gösterir. Performans ile işlem yapılırken dikkate alınması gereken çok sayıda öğe olsa da, işletim sisteminde işlemci, bellek, ağ bağdaştırıcıları ve diskleri bildirilen olarak sanal makineler için Azure İzleyici odaklanmıştır. Performans sistem durumu izleme özelliğini tamamlar ve bir olası sistem bileşeni hatası, destek ayarlama ve iyileştirme verimlilik elde etmek için gösteren sorunları ortaya veya kapasite planlamasını desteklemek yardımcı olur.  

## <a name="multi-vm-perspective-from-azure-monitor"></a>Azure İzleyicisi'nden çoklu VM perspektifi
Azure İzleyici'den performans özelliği, ortamınızda ya da aboneliklerinizdeki kaynak grupları arasında dağıtılan tüm izlenen Vm'leri birden çok sanal makine bir görünümünü sağlar.  Azure İzleyici'den erişmek için aşağıdaki adımları gerçekleştirin. 

1. Azure portalında **İzleyici**. 
2. Seçin **sanal makineler (Önizleme)** içinde **çözümleri** bölümü.
3. Seçin **performans** sekmesi.

![VM performansı en iyi N listesi görünümü](./media/vminsights-performance/vminsights-performance-aggview-01.png)

Üzerinde **üst N grafikleri** sekmesinde çözümden ile etkin çalışma alanını seçin, birden fazla Log Analytics çalışma alanınız varsa, **çalışma** sayfanın üstündeki Seçici. **Grubu** Seçici, abonelikler, kaynak grupları döndürecektir [bilgisayar grupları](../../log-analytics/log-analytics-computer-groups.md)ve bilgisayarların daha fazla filtrelemek için kullanabileceğiniz seçili çalışma alanına ilgili sanal makine ölçek kümeleri Sonuçlar bu sayfadaki ve diğer sayfalara grafiklerde sunulur. Seçiminiz yalnızca performans özelliğini uygular ve durumu ya da harita taşımaz.  

Varsayılan olarak, son 24 saat grafikleri göster. Kullanarak **TimeRange** Seçici, sorgulayabilir nasıl performans geçmişte baktığı göstermek için 30 günlük geçmiş zaman aralıkları için.   

Sayfada gösterilen beş kapasite kullanımı grafikleri şunlardır:

* CPU kullanımı % - en yüksek ortalama işlemci kullanımını en iyi beş makinelerle gösterir 
* Kullanılabilir bellek - kullanılabilir bellek düşük ortalama miktarını en çok beş makinelerle gösterir 
* Mantıksal Disk alanı yüzdesi - en yüksek ortalama disk alanına sahip en çok beş makine tüm birimlerin % kullanılan gösterir 
* Bayt gönderilen oranı - gönderilen bayt sayısı en yüksek ortalama ilk beş makinelerle gösterir 
* Gönderilen bayt sayısı en yüksek ortalama ilk beş makinelerle bayt alma oranı - gösterir 

Beş grafikleri herhangi biri sağ üst köşesinde tıklayarak açılır **en iyi N listesi** görünümü.  Burada, bir liste görünümü ve hangi makinenin en popüler tek bir VM tarafından bu performans ölçümü için kaynak kullanımını görürsünüz.  

![Seçili performans ölçüm için üst N liste görünümü](./media/vminsights-performance/vminsights-performance-topnlist-01.png)

Sanal makinede tıkladığınızda **özellikleri** bölmesinde sistem bilgileri Azure VM, vb. özellikler, işletim sistemi tarafından bildirilen gibi seçili öğenin özelliklerini göstermek için sağ taraftaki genişletilir. Altındaki seçeneklerden birine tıklayarak **hızlı bağlantılar** bölümüne yönlendirilirsiniz bu özellik için seçili sanal makineden doğrudan.  

![Sanal makine özellikleri bölmesi](./media/vminsights-performance/vminsights-properties-pane-01.png)

Geçiş **toplanan grafikleri** performans ölçümleri görüntülemek için sekmesinde filtrelenmiş ortalama veya yüzdebirliklerini önlemler alınmış.  

![VM performans toplama görünümü](./media/vminsights-performance/vminsights-performance-aggview-02.png)

Aşağıdaki kapasite kullanımı grafikleri sağlanır:

* CPU kullanımı % - varsayılan olarak ortalama ve en iyi 95. yüzdebirlik gösteriliyor 
* Kullanılabilir bellek - ortalama, en çok 5 ve 10 yüzdebirlik gösteren Varsayılanları 
* Mantıksal Disk alanı yüzdesi - varsayılan olarak ortalama ve 95. yüzdebirlik gösteriliyor 
* Bayt gönderilen oranı - varsayılan olarak gönderilen gösteren ortalama bayt sayısı 
* Bayt alma oranı - alınan ortalama bayt gösteren Varsayılanları

Seçerek zaman aralığı içinde grafiklerin ayrıntı değiştirebilirsiniz **ortalama**, **Min**, **Max**, **50**,  **90'ıncı**, ve **95** yüzdebirlik Seçici içinde.   

Liste görünümünde tek bir VM tarafından kaynak kullanımı ve en yüksek kullanımı ile hangi makinenin popüler olduğunu görmek için **en iyi N listesi** sekmesi.  **En iyi N listesi** sayfası gösterir en çok kullanılan göre ölçüm için 95. yüzdebirlik göre sıralanmış ilk 20 makineler *CPU kullanımı %*.  Daha fazla makine seçerek gördüğünüz **yük daha fazla**, ve sonuçları ilk 500 makineler göstermek için genişletin. 

>[!NOTE]
>Listenin 500'den fazla makineler aynı anda gösteremez.  
>

![Üst N liste sayfası örneği](./media/vminsights-performance/vminsights-performance-topnlist-01.png)

Listede belirli bir sanal makinede sonuçları filtrelemek için bilgisayar adını girin. **adına göre Ara** metin.  

Bunun yerine bir farklı performans ölçüm kullanımı görünümü, **ölçüm** açılan listesini seçin **kullanılabilir bellek**, **kullanılan Mantıksal Disk %**,  **Alınan Bayt/sn ağ**, veya **ağ gönderilen bayt/sn** ve bu ölçümü için kapsamlı kullanımını göstermek için listeyi güncelleştirir.  

Açılır listeden bir sanal makine **özellikleri** panelinde sağ tarafında sayfasının ve buradan seçebilirsiniz **Performans ayrıntı**.  **Sanal makine ayrıntıları** sayfası açılır ve bu VM, doğrudan Azure VM'den VM Insights performans erişirken bir deneyime benzer kapsamlıdır.  

## <a name="view-performance-directly-from-an-azure-vm"></a>Bir Azure VM'den doğrudan performans görünümü
Bir sanal makineden doğrudan erişmek için aşağıdakileri gerçekleştirin.

1. Azure portalında **sanal makineler**. 
2. Listeden VM'yi seçin ve **izleme** bölümü seçin **Insights (Önizleme)**.  
3. Seçin **performans** sekmesi. 

Bu sayfa yalnızca performans kullanımı grafikleri, ancak her mantıksal diski bulunan kendi kapasitesine kullanımı için ayrıca bir tablo gösteren içerir ve her ölçünün ortalama toplam sayısı.  

Aşağıdaki kapasite kullanımı grafikleri sağlanır:

* CPU kullanımı % - varsayılan olarak ortalama ve en iyi 95. yüzdebirlik gösteriliyor 
* Kullanılabilir bellek - ortalama, en çok 5 ve 10 yüzdebirlik gösteren Varsayılanları 
* Mantıksal Disk alanı yüzdesi - varsayılan olarak ortalama ve 95. yüzdebirlik gösteriliyor 
* Mantıksal Disk IOPS - ortalama ve 95. yüzdebirlik gösteren Varsayılanları
* Mantıksal Disk MB/sn - ortalama ve 95. yüzdebirlik gösteren Varsayılanları
* Mantıksal Disk kullanılan en fazla % - ortalama ve 95. yüzdebirlik gösteren Varsayılanları
* Bayt gönderilen oranı - varsayılan olarak gönderilen gösteren ortalama bayt sayısı 
* Bayt alma oranı - alınan ortalama bayt gösteren Varsayılanları

![VM'den VM ınsights performans doğrudan görüntüleyin](./media/vminsights-performance/vminsights-performance-directvm-01.png)

## <a name="alerting-and-alert-management"></a>Uyarı ve uyarı Yönetimi 
Önceden yapılandırılmış uyarı kuralları performans ölçümleri özellikli VM'ler için Azure İzleyici bir parçası olarak dahil değildir. Varken [sistem durumu uyarılarını](vminsights-health.md#alerting-and-alert-management) yüksek CPU kullanımı, düşük bellek, disk g/ç, yetersiz disk alanı, vs. gibi Azure vm'nizdeki algılanan performans sorunları karşılık gelen bu sistem durumu uyarıları yalnızca tüm Vm'lere uygulanır Azure İzleyici için özellikli VM'ler için aynı Log Analytics çalışma alanına bağlı. 

Ancak biz yalnızca toplamak ve Log Analytics çalışma alanında ihtiyaç duyduğunuz performans ölçümleri bir kısmını depolamak. İzleme stratejinizi analiz veya, uyarı etkili bir şekilde kapasite veya sanal makine durumunu değerlendirmek için diğer performans Ölçümleriyle içerir veya belirtme esnekliğini kendi ölçütleri veya mantıksal uyarı ihtiyacınız gerektiriyorsa, şunları yapabilirsiniz: Yapılandırma [bu performans sayaçlarını toplamayı](../../log-analytics/log-analytics-data-sources-performance-counters.md?toc=/azure/azure-monitor/toc.json) Log analytics'te ve tanımlama [günlük uyarıları](../../monitoring-and-diagnostics/alert-log.md?toc=/azure/azure-monitor/toc.json). Diğer veri türlerine sahip karmaşık bir analiz gerçekleştirin ve eğilim analizi, ölçümleri diğer yandan desteklemek için daha uzun bekletme süresi sağlamak log Analytics sağlar, ancak basit ve gerçek zamanlı senaryoları destekleme özelliğine sahip olan. Tarafından toplanan [Azure tanılama Aracısı](../../virtual-machines/windows/monitor.md) ve daha düşük gecikme süresi ve daha düşük bir maliyetle uyarılar oluşturmanıza olanak sağlayan Azure İzleyici ölçümleri deposu depolanır.

Özeti gözden [ölçüm ve günlükleri Azure İzleyici ile koleksiyonunu](../../monitoring/monitoring-data-collection.md?toc=/azure/azure-monitor/toc.json) daha fazla temel farklılıklar ve dikkat edilecek diğer noktalar koleksiyonunu bu ek Ölçümler ve uyarı kurallarını yapılandırmadan önce anlaşılması.  

## <a name="next-steps"></a>Sonraki adımlar
Sistem durumu özelliği kullanmayı öğrenmek için bkz [görünümü VM sistem durumu için Azure İzleyici](vminsights-health.md), veya bulunan Uygulama bağımlılıklarını görüntülemek için bkz. [Vm'leri harita görünümü Azure İzleyici](vminsights-maps.md). 