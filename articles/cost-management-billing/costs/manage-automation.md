---
title: Otomasyon ile Azure maliyetlerini yönetme
description: Bu makalede, otomasyon ile Azure maliyetlerini nasıl yönetebileceğiniz açıklanmaktadır.
author: bandersmsft
ms.author: banders
ms.date: 03/08/2021
ms.topic: conceptual
ms.service: cost-management-billing
ms.subservice: cost-management
ms.reviewer: adwise
ms.openlocfilehash: a54b8243b5a680168b2e5806dd58c0fa4109728f
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "104670282"
---
# <a name="manage-costs-with-automation"></a>Otomasyon ile maliyetleri yönetme

Maliyet verilerini alıp yönetmek amacıyla özel bir çözüm kümesi oluşturmak için Maliyet Yönetimi otomasyonunu kullanabilirsiniz. Bu makalede, Maliyet Yönetimi otomasyonuna yönelik yaygın senaryolar ve içinde bulunduğunuz duruma göre kullanabileceğiniz seçenekler ele alınmaktadır. API’leri kullanarak uygulama geliştirmek istiyorsanız, bu süreci hızlandırmaya yardımcı olmak için sunulan yaygın API isteği örneklerinden faydalanabilirsiniz.

## <a name="automate-cost-data-retrieval-for-offline-analysis"></a>Çevrimdışı analiz için maliyet verilerini almayı otomatikleştirme

Azure maliyet verilerinizi diğer veri kümeleriyle birleştirmek için indirmeniz gerekebilir. Alternatif olarak, maliyet verilerinizi kendi sistemlerinize tümleştirmeniz gerekebilir. İlgili veri miktarına bağlı olarak farklı seçenekler mevcuttur. Her durumda API’leri ve araçları kullanmak için uygun kapsamda Maliyet Yönetimi izinlerine sahip olmanız gerekir. Daha fazla bilgi için bkz. [Verilere erişim atama](./assign-access-acm-data.md).

## <a name="suggestions-for-handling-large-datasets"></a>Büyük veri kümelerini işlemeye yönelik öneriler

Kuruluşunuzun Azure’ın kullanıldığı çok sayıda kaynak ve abonelik varsa, kullanım ayrıntılarına ilişkin oldukça fazla veri elde edersiniz. Excel genellikle böyle büyük dosyaları yükleyemez. Bu durumda aşağıdaki seçenekleri öneririz:

**Power BI**

Power BI, büyük miktarda verileri alıp işlemek için kullanılır. Kurumsal Anlaşma müşterisiyseniz faturalama hesabınızın maliyetlerini analiz etmek için Power BI şablon uygulamasını kullanabilirsiniz. Raporda, müşteriler tarafından kullanılan önemli görünümler yer alır. Daha fazla bilgi için bkz. [Power BI şablon uygulamasıyla Azure maliyetlerini analiz etme](./analyze-cost-data-azure-cost-management-power-bi-template-app.md).

**Power BI veri bağlayıcısı**

Verilerinizi günlük olarak analiz etmek istiyorsanız ayrıntılı analiz amacıyla verileri almak için [Power BI veri bağlayıcısını](/power-bi/connect-data/desktop-connect-azure-cost-management) kullanmanızı öneririz. Daha fazla maliyet yansıdıkça bağlayıcı, oluşturduğunuz raporları güncel tutar.

**Maliyet Yönetimi dışarı aktarmaları**

Verileri günlük olarak analiz etmeniz gerekmeyebilir. Böyle durumlarda, veri dışarı aktarmalarını bir Azure Depolama hesabına yönlendirilecek biçimde zamanlamak için Maliyet Yönetimi’nin [Dışarı aktarmalar](./tutorial-export-acm-data.md) özelliğini kullanmanız faydalı olabilir. Daha sonra, verileri gerektiği şekilde Power BI’a yükleyebilir veya dosya yeterince küçükse Excel’de analiz edebilirsiniz. Dışarı aktarmaları Azure portalında bulabilirsiniz veya [Dışarı Aktarmalar API’sini](/rest/api/cost-management/exports) kullanarak dışarı aktarmaları yapılandırabilirsiniz.

**Kullanım Ayrıntıları API’si**

Maliyet verileriniz küçük bir kümeyse [Kullanım Ayrıntıları API’sini](/rest/api/consumption/usageDetails) kullanmanız faydalı olabilir. Maliyet verileriniz büyük bir kümeyse, bir dönem için talep edeceğiniz veri miktarını mümkün olan en düşük düzeyde tutmanız gerekir. Bunu yapmak için kısa bir zaman aralığı belirtin veya isteğinizde filtre kullanın. Örneğin, üç yıllık maliyet verilerine ihtiyaç duyduğunuz bir senaryoda, tek bir çağrı yerine farklı zaman aralıkları için birden çok çağrı yaptığınızda API kullanmanız daha faydalı olabilir. Buradan, daha ayrıntılı analiz etmek amacıyla verileri Excel’e yükleyebilirsiniz.

## <a name="automate-retrieval-with-usage-details-api"></a>Kullanım Ayrıntıları API’si ile alımı otomatikleştirme

[Kullanım Ayrıntıları API’si](/rest/api/consumption/usageDetails) Azure faturanıza karşılık gelen ham, toplu olmayan maliyet verilerini almanın kolay bir yolunu sunar. Kuruluşunuz için programlı bir veri alma çözümüne ihtiyaç duyuyorsanız API’den faydalanabilirsiniz. Daha küçük maliyet verisi kümelerini analiz etmek istiyorsanız API’yi kullanmanız faydalı olabilir. Ancak daha büyük veri kümeleriniz varsa önceden açıklanan diğer çözümleri kullanmanız gerekir. Kullanım Ayrıntılarındaki veriler günlük olarak ölçüm başına sağlanır. Bu veriler, aylık faturanız hesaplanırken kullanılır. Bu API’lerin genel kullanılabilirlik (GA) sürümü, `2019-10-01` sürümüdür. Rezervasyon ve Azure Market satın alımlarına yönelik önizleme sürümüne erişmek için API’lerle `2019-04-01-preview` sürümünü kullanın.

Düzenli olarak büyük miktarlarda dışarı aktarılan veri almak istiyorsanız, bkz. [dışarı aktarmalar ile büyük maliyet veri kümelerini yinelenen olarak alma](ingest-azure-usage-at-scale.md).

### <a name="usage-details-api-suggestions"></a>Kullanım Ayrıntıları API’si önerileri

**İstek zamanlaması**

Kullanım Ayrıntıları API’sine bir günde _birden fazla istek yapmamanızı_ öneririz. Maliyet verilerinin ne sıklıkta yenilendiği ve yuvarlamanın nasıl ele alındığı hakkında daha fazla bilgi için bkz. [Maliyet yönetimi verilerini anlama](./understand-cost-mgt-data.md).

**Filtre uygulamadan en üst düzey kapsamları hedefleme**

İhtiyaç duyduğunuz tüm verileri mümkün olan en geniş kapsamda almak için API’yi kullanın. Filtreleme, gruplandırma veya toplu analiz yapmadan önce tüm gerekli verilerin alınmasını bekleyin. API, büyük miktarda toplu olmayan ham maliyet verilerini sunmak amacıyla özel olarak iyileştirilmiştir. Maliyet Yönetimi’nde mevcut olan kapsamlar hakkında daha fazla bilgi edinmek için bkz. [Kapsamları anlama ve bunlarla çalışma](./understand-work-scopes.md). Bir kapsam için gereken verileri indirdiğinizde, verileri filtrelerle ve PivotTable’larla daha ayrıntılı bir şekilde analiz etmek için Excel’i kullanın.

### <a name="notes-about-pricing"></a>Fiyatlandırma hakkında notlar

Kullanımı ve ücretleri fiyat listeniz veya faturanızla mutabık kılmak istiyorsanız aşağıdaki bilgileri dikkate alın.

Fiyat Listesi fiyat davranışı - Fiyat listesinde gösterilen fiyatlar, Azure'dan aldığınız fiyatlardır. Bunlar belirli bir ölçü birimine göre ölçeklendirilir. Ne yazık ki söz konusu ölçü birimi her zaman gerçek kaynak kullanımının ve ücretlerinin ifade edildiği ölçü birimiyle eşleşmez.

Kullanım Ayrıntıları fiyat davranışı - Kullanım dosyalarında ölçeklendirilmiş bilgiler gösterilir ve bunlar fiyat listesiyle tam olarak eşleşmeyebilir. Özellikle:

- Birim Fiyatı - Fiyat, ücretlerin Azure kaynakları tarafından ifade edildiği ölçü birimiyle eşleşmesi için ölçeklendirilir. Ölçeklendirme yapılırsa fiyat artık Fiyat Listesindeki fiyatla eşleşmeyecektir.
- Ölçü Birimi - Ücretlerin Azure kaynakları tarafından ifade edildiği ölçü birimini temsil eder.
- Geçerli Fiyat / Kaynak Ücreti - Fiyat, indirimler hesaba katıldıktan sonra birim başına ödeyeceğiniz gerçek rakamı temsil eder. Ücretleri mutabık kılmak için Miktara Uygulanan Fiyat * Miktar hesaplamaları yaparken kullanılacak olan fiyat budur. Fiyatta aşağıdaki senaryolar ve dosyalarda da yer alan ölçeklendirilmiş birim fiyatı hesaba katılır. Sonuç olarak bu, ölçeklendirilmiş birim fiyatından farklı olabilir.
  - Katmanlı fiyatlandırma - Örneğin: İlk 100 birim için 10 ABD doları, sonraki 100 birim için 8 ABD doları.
  - Dahil edilen miktar - Örneğin: İlk 100 birim ücretsiz ve sonra birim başına 10 ABD doları.
  - Rezervasyonlar
  - Hesaplamada yapılan yuvarlama – Yuvarlama yapılırken tüketilen miktar, katman/dahil edilen miktar fiyatlandırması ve ölçeklendirilmiş birim fiyatı hesaba katılır.

## <a name="example-usage-details-api-requests"></a>Örnek Kullanım Ayrıntıları API’si istekleri

Aşağıdaki örnek istekler, karşılaşabileceğiniz yaygın senaryoları ele almak amacıyla Microsoft müşterileri tarafından kullanılır.

### <a name="get-usage-details-for-a-scope-during-specific-date-range"></a>Belirli bir tarih aralığı süresince bir kapsama ait Kullanım Verilerini alma

İstek tarafından döndürülen veriler, faturalama sistemi tarafından kullanım verilerinin alındığı tarihe karşılık gelir. Bu, birden çok faturadaki maliyetleri içerebilir. Kullanılacak çağrı abonelik türünüze göre değişiklik gösterir.

Kurumsal Anlaşması (EA) veya kullandıkça öde aboneliği olan eski müşteriler için aşağıdaki çağrıyı kullanın:

```http
GET https://management.azure.com/{scope}/providers/Microsoft.Consumption/usageDetails?$filter=properties%2FusageStart%20ge%20'2020-02-01'%20and%20properties%2FusageEnd%20le%20'2020-02-29'&$top=1000&api-version=2019-10-01
```

Microsoft Müşteri Sözleşmesi olan modern müşteriler için aşağıdaki çağrıyı kullanın:

```http
GET https://management.azure.com/{scope}/providers/Microsoft.Consumption/usageDetails?startDate=2020-08-01&endDate=&2020-08-05$top=1000&api-version=2019-10-01
```

### <a name="get-amortized-cost-details"></a>Amorti edilmiş maliyet ayrıntılarını alma

Gerçek maliyetlerin, yansıtılan satın alma işlemlerini göstermesini istiyorsanız aşağıdaki istekte *ölçüm* değerini `ActualCost` ile değiştirin. Amorti edilmiş ve gerçek maliyetleri kullanmak için `2019-04-01-preview` sürümünü kullanmanız gerekir. Geçerli API sürümü, yeni tür/ölçüm özniteliği ve değişen özellik adları dışında `2019-10-01` sürümüyle aynı şekilde çalışır. Microsoft Müşteri Sözleşmeniz varsa aşağıdaki örnekte filtreleriniz `startDate` ve `endDate` olur.

```http
GET https://management.azure.com/{scope}/providers/Microsoft.Consumption/usageDetails?metric=AmortizedCost&$filter=properties/usageStart+ge+'2019-04-01'+AND+properties/usageEnd+le+'2019-04-30'&api-version=2019-04-01-preview
```

## <a name="automate-alerts-and-actions-with-budgets"></a>Uyarıları ve eylemleri bütçelerle otomatikleştirin

Buluttaki yatırımınızın değerini en üst düzeye çıkarmak için dikkate alınması gereken iki kritik bileşen vardır. Bunlardan ilki otomatik bütçe oluşturulmasıdır. Diğeriyse bütçe uyarılarına karşılık maliyet temelli düzenlemeler yapılandırmaktır. Azure bütçe oluşturmayı otomatikleştirmenin farklı yöntemleri vardır. Yapılandırılmış uyarı eşikleri geçildiğinde, çeşitli uyarı yanıtları meydana gelir.

Aşağıdaki bölümlerde, kullanılabilir seçenekler ele alınır ve bütçe otomasyonunu kullanmaya başlamanız için örnek API istekleri sağlanır.

### <a name="how-costs-are-evaluated-against-your-budget-threshold"></a>Maliyetler bütçe eşiğinize karşı nasıl değerlendirilir?

Maliyetler bütçe eşiğinize karşı günde bir kez değerlendirilir. Yeni bir bütçe oluşturduğunuzda veya bütçenizin sıfırlanma gününde, değerlendirme yapılmamış olabileceğinden eşikle karşılaştırılan maliyetler sıfır/null olur.

Azure, maliyetlerinizin eşiği aştığını algıladığında, algılandığı saatte bir bildirim tetiklenir.

### <a name="view-your-current-cost"></a>Geçerli maliyetinizi görüntüleme

Geçerli maliyetlerinizi görüntülemek için [Sorgu API’sini](/rest/api/cost-management/query) kullanarak bir GET çağrısı yapmanız gerekir.

Bütçeler API’sine yapılan bir GET çağrısı, Maliyet Analizi’nde gösterilen geçerli maliyetleri döndürmez. Bunun yerine, çağrı son değerlendirilen maliyetinizi döndürür.

### <a name="automate-budget-creation"></a>Bütçe oluşturmayı otomatikleştirme

[Bütçeler API’sini](/rest/api/consumption/budgets) kullanarak bütçe oluşturmayı otomatikleştirebilirsiniz. Ayrıca, [bütçe şablonuyla](quick-create-budget-template.md) bir bütçe oluşturabilirsiniz. Şablonlar, maliyet denetiminizin düzgün yapılandırılıp zorlanmasını sağlarken Azure dağıtımlarınızı standart hale getirmenize yönelik kolay bir yöntem sağlar.

### <a name="supported-locales-for-budget-alert-emails"></a>Bütçe uyarısı e-postaları için desteklenen yerel ayarlar

Bütçeler sayesinde, maliyetleriniz belirli bir eşiği açtığında uyarı alırsınız. Bütçe başına en fazla beş e-posta alıcısı ayarlayabilirsiniz. Bütçe eşiği aşıldıktan sonraki 24 saat içinde alıcılara e-posta gönderilir. Ancak, alıcınızın farklı bir dilde e-posta alması gerekebilir. Bütçe API’siyle aşağıdaki dil kültür kodlarını kullanabilirsiniz. Kültür kodunu, aşağıdaki örneğe benzer şekilde `locale` parametresiyle ayarlayın.

```json
{
  "eTag": "\"1d681a8fc67f77a\"",
  "properties": {
    "timePeriod": {
      "startDate": "2020-07-24T00:00:00Z",
      "endDate": "2022-07-23T00:00:00Z"
    },
    "timeGrain": "BillingMonth",
    "amount": 1,
    "currentSpend": {
      "amount": 0,
      "unit": "USD"
    },
    "category": "Cost",
    "notifications": {
      "actual_GreaterThan_10_Percent": {
        "enabled": true,
        "operator": "GreaterThan",
        "threshold": 20,
        "locale": "en-us",
        "contactEmails": [
          "user@contoso.com"
        ],
        "contactRoles": [],
        "contactGroups": [],
        "thresholdType": "Actual"
      }
    }
  }
}

```

Kültür koduyla desteklenen diller:

| Kültür kodu| Dil |
| --- | --- |
| tr-tr | İngilizce (ABD) |
| ja-jp | Japonca (Japonya) |
| zh-cn | Çince (Yalın, Çin) |
| de-de | Almanca (Almanya) |
| es-es | İspanyolca (İspanya, Uluslararası) |
| fr-fr | Fransızca (Fransa) |
| it-it | İtalyanca (İtalya) |
| ko-kr | Korece (Kore) |
| pt-br | Portekizce (Brezilya) |
| ru-ru | Rusça (Rusya) |
| zh-tw | Çince (Geleneksel, Tayvan) |
| cs-cz | Çekçe (Çek Cumhuriyeti) |
| pl-pl | Lehçe (Polonya) |
| tr-tr | Türkçe (Türkiye) |
| da-dk | Danca (Danimarka) |
| en-GB | İngilizce (İngiltere) |
| hu-hu | Macarca (Macaristan) |
| nb-no | Norveççe (Bokmal) (Norveç) |
| nl-nl | Felemenkçe (Hollanda) |
| pt-pt | Portekizce (Portekiz) |
| sv-se | İsveççe (İsviçre) |

### <a name="common-budgets-api-configurations"></a>Yaygın Bütçeler API’si yapılandırmaları

Azure ortamınızda bir bütçeyi yapılandırmanın pek çok yolu vardır. İlk olarak senaryonuzu ele alın, daha sonra bu senaryoyu gerçekleştirmeye yarayan yapılandırma seçeneklerini belirleyin. Aşağıdaki seçenekleri gözden geçirin:

- **Zaman Birimi**: Bütçenizin maliyetleri tahakkuk ettirip değerlendirmek için kullandığı yinelenme süresini temsil eder. En yaygın seçenekler Aylık, Üç Aylık ve Yıllıktır.
- **Zaman Aralığı**: Bütçenizin ne kadar süreliğine geçerli olduğunu gösterir. Bütçe yalnızca geçerli olduğu durumlarda sizi etkin bir şekilde izleyip uyarır.
- **Bildirimler**
  - İletişim E-postaları: Bütçe maliyetleri tahakkuk ettirdiğinde ve tanımlı eşikleri aştığında e-posta adresleri uyarılar alır.
  - Kişi Rolleri: Belirtilen kapsamda bir Azure rolüne sahip olan tüm kullanıcılar bu seçenek sayesinde e-posta uyarıları alır. Örneğin Abonelik Sahipleri, abonelik kapsamında oluşturulan bir bütçe için uyarı alabilirler.
  - Kişi Grupları: Bir uyarı eşiği aşıldığında, bütçe yapılandırılmış eylem gruplarını çağırır.
- **Maliyet boyut filtreleri**: Maliyet Analizi’nde yapabileceğiniz filtrenin aynısı, Sorgu API’si de bütçenizde yapılabilir. Bütçe ile izlediğiniz maliyetlerin aralığını daraltmak için bu filtreyi kullanın.

İhtiyaçlarınızı karşılayan bütçe oluşturma seçeneklerini belirledikten sonra, API’yi kullanarak bütçeyi oluşturun. Aşağıdaki örnek, yaygın bir bütçe yapılandırmasıyla başlamanıza yardımcı olur.

**Birden çok kaynak ve etikete göre filtrelenen bir bütçe oluşturma**

İstek URL’si: `PUT https://management.azure.com/subscriptions/{SubscriptionId} /providers/Microsoft.Consumption/budgets/{BudgetName}/?api-version=2019-10-01`

```json
{
  "eTag": "\"1d34d016a593709\"",
  "properties": {
    "category": "Cost",
    "amount": 100.65,
    "timeGrain": "Monthly",
    "timePeriod": {
      "startDate": "2017-10-01T00:00:00Z",
      "endDate": "2018-10-31T00:00:00Z"
    },
    "filter": {
      "and": [
        {
          "dimensions": {
            "name": "ResourceId",
            "operator": "In",
            "values": [
              "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{meterName}",
              "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{meterName}"
            ]
          }
        },
        {
          "tags": {
            "name": "category",
            "operator": "In",
            "values": [
              "Dev",
              "Prod"
            ]
          }
        },
        {
          "tags": {
            "name": "department",
            "operator": "In",
            "values": [
              "engineering",
              "sales"
            ]
          }
        }
      ]
    },
    "notifications": {
      "Actual_GreaterThan_80_Percent": {
        "enabled": true,
        "operator": "GreaterThan",
        "threshold": 80,
        "contactEmails": [
          "user1@contoso.com",
          "user2@contoso.com"
        ],
        "contactRoles": [
          "Contributor",
          "Reader"
        ],
        "contactGroups": [
          "/subscriptions/{subscriptionID}/resourceGroups/{resourceGroupName}/providers/microsoft.insights/actionGroups/{actionGroupName}
        ],
        "thresholdType": "Actual"
      }
    }
  }
}
```

### <a name="configure-cost-based-orchestration-for-budget-alerts"></a>Bütçe uyarıları için maliyet temelli düzenleme yapılandırma

Azure Eylem Grupları’nı kullanarak otomatikleştirilmiş eylemleri başlatmak için bütçeleri yapılandırabilirsiniz. Bütçeleri kullanarak eylemleri otomatikleştirme hakkında daha fazla bilgi için bkz. [Azure Bütçeler ile otomasyon](../manage/cost-management-budget-scenario.md).

## <a name="data-latency-and-rate-limits"></a>Veri gecikme süresi ve hız sınırları

API’leri günde en fazla bir kez çağırmanızı öneririz. Maliyet Yönetimi verileri, Azure kaynak sağlayıcılarından yeni kullanım verileri alındıkça dört saatte bir yenilenir. Daha sık çağrı yapmak daha fazla veri sağlamaz. Bunun yerine yalnızca yükün artmasına neden olur. Verilerin değişme sıklığı ve veri gecikme süresinin işlenme yöntemi hakkında daha fazla bilgi edinmek için bkz. [Maliyet Yönetimi verilerini anlama](understand-cost-mgt-data.md).

### <a name="error-code-429---call-count-has-exceeded-rate-limits"></a>Hata kodu 429 - Çağrı sayısı, hız sınırını aştı

Tüm Maliyet Yönetimi abonelerine tutarlı bir deneyim sunmak amacıyla Maliyet Yönetimi API’leri hız sınırına sahiptir. Sınıra ulaştığınızda `429: Too many requests` HTTP durum kodunu alırsınız. API’lerimizin geçerli aktarım hızı aşağıdaki gibidir:

- Dakikada 30 çağrı - Kapsam, kullanıcı veya uygulama başına yapılır.
- Dakikada 200 çağrı - Kiracı, kullanıcı veya uygulama başına yapılır.

## <a name="next-steps"></a>Sonraki adımlar

- [Power BI şablon uygulamasıyla Azure maliyetlerini analiz etme](./analyze-cost-data-azure-cost-management-power-bi-template-app.md).
- Dışarı Aktarmalar ile [dışarı aktarılmış verileri oluşturup yönetin](./tutorial-export-acm-data.md).
- [Kullanım Ayrıntıları API’si](/rest/api/consumption/usageDetails) hakkında daha fazla bilgi edinin.
