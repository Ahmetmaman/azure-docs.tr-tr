---
title: Analytics Portalı'nda Azure Log Analytics ile çalışmaya başlama | Microsoft Docs
description: Bu makale, Log Analytics'te sorgu yazmak için Analytics portalı kullanmaya yönelik bir öğretici sağlar.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/06/2018
ms.author: bwren
ms.component: na
ms.openlocfilehash: 6f6916b27aa251bc0a0c25be060378c11faab607
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2018
ms.locfileid: "39635143"
---
# <a name="get-started-with-the-analytics-portal"></a>Analytics portalı ile çalışmaya başlama

Bu öğreticide, Azure Log Analytics sorguları yazma için Analytics portalı kullanmayı öğreneceksiniz. Size nasıl yardımcı olacak için:

- Basit Sorgu yazma
- Verilerinizin şemasını anlama
- Filtre, sıralama ve Grup sonuçları
- Bir zaman aralığı uygulayın
- Grafikler oluşturun
- Kaydet ve sorguları
- Verme ve sorguları paylaşma


## <a name="meet-the-analytics-portal"></a>Analytics portalını karşılamak
Analytics portalını, yazma ve Azure Log Analytics sorgularını yürütmek için kullanılan bir web aracıdır. 

![Giriş sayfası](media/get-started-analytics-portal/homepage.png)

Giriş sayfasında son ve kaydedilmiş sorgular ve örnekler gibi yararlı kaynaklar için kolay erişim sunar. Kendi sorgular yazmaya başlamak için yeni bir sekme açın.

## <a name="basic-queries"></a>Temel sorgular
Sorgular, arama terimleri, eğilimleri belirlemenize, biçimlerini çözümleme ve verilerinizi temel alan birçok öngörüden sağlamak için kullanılabilir. Temel bir sorgu başlatın:

```OQL
Event | search "error"
```

Bu sorgu arar _olay_ herhangi bir özelliği "error" terimini içeren kayıtlar için tablo.

Sorgular, bir tablo adı ile başlatabilir veya **arama** komutu. Yukarıdaki örnekte tablo adı ile başlayan _olay_, sorgunun kapsamını tanımlar. Dikey çubuk (|) karakterini komutları, bu nedenle ayıran birincisini giriş aşağıdaki komutun çıkışı. Herhangi bir sayıda komutları için tek bir sorgu ekleyebilirsiniz.

Aynı sorgu yazmak için başka bir yolu şu şekilde olur:

```OQL
search in (Event) "error"
```

Bu örnekte, **arama** kapsamı _olay_ tablo ve bu tablodaki tüm kayıtları "error" terimini arayan aranır.

## <a name="running-a-query"></a>Bir sorgu çalıştırma
Tıklayarak bir sorgu çalıştırın **çalıştırma** düğme veya tuşlarına basarak **Shift + Enter**. Çalıştırılacak kod ve döndürülen verileri belirleyen aşağıdaki ayrıntıları göz önünde bulundurun:

- Satır sonları: tek bir kesme sorgunuzu daha anlaşılır hale getirir. Birden çok satır sonları ayrı sorgulara bölün.
- İmleç: imlecinizi sorgusunda çalıştırmak üzere bir yere yerleştirin. Geçerli sorgu boş bir satır bulunana kadar kodu olarak kabul edilir.
- Zaman aralığı - bir zaman aralığı _son 24 saat_ varsayılan olarak ayarlanır. Farklı bir aralık kullanmak için Saat Seçici kullanın veya açık bir zaman Ekle sorgunuz için Aralık filtresi.


## <a name="understand-the-schema"></a>Şemayı anlamak
Şema, görsel olarak mantıksal bir kategori altında gruplandırılmış bir tablo koleksiyonudur. Çeşitli kategorileri izleme çözümleri şunlardır. _LogManagement_ kategorisi, Windows ve Syslog olayları, performans verilerini ve istemci sinyal gibi sık kullanılan verileri içerir.

![Şema](media/get-started-analytics-portal/schema.png)

Her tabloda sütun adının yanındaki simge tarafından belirtildiği gibi farklı veri türleriyle sütunlardaki verileri düzenlenir. Örneğin, _olay_ ekran görüntüsünde gösterilen tablo içeren sütunlar gibi _bilgisayar_ metin olduğu _EventCategory_ bir sayı olan ve _ TimeGenerated_ olduğu tarih/saat.

## <a name="filter-the-results"></a>Sonuçları filtreleme
Her şeyi alınırken Start _olay_ tablo.

```OQL
Event
```

Analytics portalını sonuçlarına göre otomatik olarak kapsamları:

- Zaman aralığı: varsayılan olarak, sorgular son 24 saat sınırlıdır.
- Sonuç sayısı: sonucu olan sınırlı en fazla 10.000 kaydeder.

Bu sorgu çok genel ve kullanışlı olması için çok fazla sonuç döndürür. Tablo öğeleri aracılığıyla ya da açıkça sorguya bir filtre eklemeden sonuçlarını filtreleyebilirsiniz. Sorgu filtre yeni filtre uygulanmış bir sonuç ayarlamak ve bu nedenle daha doğru sonuçlar üretebilir döndürürken Tablo öğeleri aracılığıyla sonuçları filtrelemek mevcut sonuç kümesi için geçerlidir.

### <a name="add-a-filter-to-the-query"></a>Sorguya bir filtre ekleyin
Her kaydın sol ok yoktur. Belirli bir kaydın ayrıntılarını açmak için bu oka tıklayın.

Bir sütun adı için yukarıda getirin "+" ve "-" görüntülenecek simge. Yalnızca aynı değere sahip kayıtları döndürecek bir filtre eklemek için "+" işaretine tıklayın. Tıklayın "-" Bu değere sahip kayıtları hariç tutacak ve ardından **çalıştırma** sorguyu yeniden çalıştırmayı.

![Sorgu Filtresi Ekle](media/get-started-analytics-portal/add-filter.png)

### <a name="filter-through-the-table-elements"></a>Tablo öğelerini filtreleyin
Artık bir önem derecesi ile olayları odaklanalım _hata_. Bu adlı bir sütun belirtilen _EventLevelName_. Bu sütun görmek için sağa kaydırmak gerekir.

Sütun başlığının yanında bulunan filtre simgesine tıklayın ve açılır pencerede'ı seçin, değerleri _ile başlayan_ metin _hata_:

![Filtre](media/get-started-analytics-portal/filter.png)


## <a name="sort-and-group-results"></a>Sırala ve Grupla sonuçları
Sonuçları yalnızca son 24 saat içinde oluşturulan SQL Server hata olaylarını içerecek şekilde daraltılmıştır. Ancak, sonuçları herhangi bir yolla sıralı değildir. Sonuçları gibi belirli bir sütuna göre sıralamak için _zaman damgası_ Örneğin, sütun başlığına tıklayın. Tek bir tıklamayla, ikinci tıklama azalan düzende sıralama sırasında artan düzende sıralar.

![Sıralama sütunu](media/get-started-analytics-portal/sort-column.png)

Sonuçları düzenlemek için başka bir grup tarafından yoludur. Sonuçları belirli bir sütuna yalnızca sütun üst bilgisinin diğer sütunları yukarıda Sürükle göre grup. Alt grupları oluşturmak için diğer sütunları de üst çubuğuna sürükleyin.

![Gruplar](media/get-started-analytics-portal/groups.png)

## <a name="select-columns-to-display"></a>Görüntülenecek sütunları seçin
Sonuçlar tablosu, genellikle çok sayıda sütun içerir. Geri dönen sütunlar bazıları varsayılan olarak görüntülenmez veya bazı görüntülenen sütunları kaldırmak isteyebileceğiniz bulabilirsiniz. Gösterilecek sütunları seçmek için sütunları düğmesine tıklayın:

![Sütun seçme](media/get-started-analytics-portal/select-columns.png)


## <a name="select-a-time-range"></a>Bir zaman aralığı seçin
Analytics portalını varsayılan olarak, geçerli _son 24 saat_ zaman aralığı. Farklı bir aralık kullanmak için Saat Seçici başka bir değer seçin ve **çalıştırma**. Önceden oluşturulmuş değerlere ek olarak, kullandığınız _özel zaman aralığı_ sorgunuz için mutlak bir aralık seçmek için seçenek.

![Saat Seçici](media/get-started-analytics-portal/time-picker.png)

Özel bir zaman aralığı seçerken, seçilen değerleri, yerel saat diliminizi farklı olabilir, UTC biçimindedir.

Sorgu için bir filtre açıkça içeriyorsa _TimeGenerated_Seçici başlık gösterilir saati _sorguda Ayarla_. Bir çakışmayı önlemek üzere el ile seçim devre dışı bırakılır.


## <a name="charts"></a>Grafikler
Ayrıca bir tablodaki sonuçları döndürmek, sorgu sonuçları visual biçimlerde sunulabilir. Örnek olarak aşağıdaki sorguyu kullanın:

```OQL
Event 
| where EventLevelName == "Error" 
| where TimeGenerated > ago(1d) 
| summarize count() by Source 
```

Varsayılan olarak, bir tablodaki sonuçlar görüntülenir. Tıklayın _grafik_ sonuçları bir grafik görünümde görmek için:

![Çubuk grafik](media/get-started-analytics-portal/bar-chart.png)

Sonuçlar, yığılmış çubuk grafik olarak görüntülenir. Tıklayın _yığılmış sütun_ seçip _pasta_ sonuçları başka bir görünümünü göstermek için:

![Pasta grafiği](media/get-started-analytics-portal/pie-chart.png)

Görünümün x gibi farklı özellikleri ve y eksenleri veya gruplama ve bölme tercihleri değiştirilebilir el ile denetim çubuğundan.

İşleme işlecini kullanarak sorgunun kendisi, tercih ettiğiniz görünümü de ayarlayabilirsiniz.

### <a name="smart-diagnostics"></a>Akıllı tanılama
Bir zaman grafiğini üzerinde ani bir değişiklik veya verilerinizi adımda ise bir vurgulanan satırın noktasında görebilirsiniz. Gösterir _akıllı tanılama_ ani değişiklik filtre özelliklerinin belirlemiştir. Noktası üzerindeki filtre daha fazla ayrıntı alalım ve filtrelenmiş sürümü görmek için tıklayın. Bu değişikliğin nedenini belirlemenize yardımcı olabilir:

![Akıllı tanılama](media/get-started-analytics-portal/smart-diagnostics.png)

## <a name="pin-to-dashboard"></a>Panoya sabitle
Bir diyagram veya tabloya bir paylaşılan Azure panolarınızı sabitlemek için Raptiye simgesine tıklayın.

![Panoya sabitle](media/get-started-analytics-portal/pin-dashboard.png)

Panoya sabitleme belirli basitleştirme grafiğe uygulanır:

- Tablo satırları ve sütunları: bir tabloyu panoya sabitlemek için dört veya daha az sütun olması gerekir. Yalnızca üst yedi satırlar görüntülenir.
- Kısıtlama süresi: sorguları için son 14 gün otomatik olarak sınırlı.
- Depo sayısı kısıtlaması: çok sayıda ayrı bir depo olan grafik görüntülerseniz, daha az doldurulmuş depo tek bir otomatik olarak gruplandırılır _başkalarının_ depo.

## <a name="save-queries"></a>Sorguları Kaydet
Yararlı bir sorgu oluşturduktan sonra kaydedin veya başkalarıyla paylaşmak isteyebilirsiniz. **Kaydet** üst çubuğunda simgedir.

Tüm sorgu sayfası ya da tek bir sorgu işlevi olarak kaydedebilirsiniz. Diğer sorgular tarafından başvurulabilen sorguları işlevlerdir. Bir işlev olarak bir sorguyu kaydetmek için bu sorguyu diğer sorgular tarafından başvurulduğunda çağırmak için kullanılan ad bir işlev diğer adı sağlamanız gerekir.

![İşlev Kaydet](media/get-started-analytics-portal/save-function.png)

Log Analytics sorgu her zaman bir seçilen çalışma alanına kaydedilir ve bu çalışma alanının diğer kullanıcılarla paylaşılan.

## <a name="load-queries"></a>Sorguları
Sorgu Gezgini simgesine sağ üst alandır. Bu, tüm kaydedilmiş sorgular kategoriye göre listeler. Ayrıca, belirli sorguları hızla gelecekte bulmak için sık kullanılan olarak işaretlemek sağlar. Kaydedilmiş bir sorgu için geçerli pencere eklemek için çift tıklayın.

![Sorgu gezgini](media/get-started-analytics-portal/query-explorer.png)

## <a name="export-and-share-as-link"></a>Dışarı aktarma ve bağlantı olarak paylaşın
Analytics portalını birkaç verme yöntemleri destekler:

- Excel: sonuçları bir CSV dosyası olarak kaydedin.
- Power BI: sonuçları bir power BI dışarı aktarın. Bkz: [alma Azure Log Analytics verilerini Power bı'a](../log-analytics-powerbi.md) Ayrıntılar için.
- Bir bağlantı paylaşabilirsiniz: sorgu daha sonra gönderilen ve aynı çalışma alanına erişimi olan diğer kullanıcılar tarafından yürütülen bir bağlantı olarak paylaşılabilir.

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinin [Log Analytics sorguları yazma](get-started-queries.md).
