---
title: Azure Izleme günlükleri için kullanımı ve maliyetleri yönetme | Microsoft Docs
description: Azure Izleyici 'de Log Analytics çalışma alanınızın fiyatlandırma planını değiştirme ve veri hacmini ve bekletme ilkesini yönetme hakkında bilgi edinin.
services: azure-monitor
documentationcenter: azure-monitor
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 07/29/2019
ms.author: magoedte
ms.subservice: ''
ms.openlocfilehash: 5e325f7766e7b0d9764949eb3fbf9753d65db8b3
ms.sourcegitcommit: 08d3a5827065d04a2dc62371e605d4d89cf6564f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2019
ms.locfileid: "68619386"
---
# <a name="manage-usage-and-costs-with-azure-monitor-logs"></a>Azure Izleyici günlükleriyle kullanımı ve maliyetleri yönetme

> [!NOTE]
> Bu makalede, Log Analytics çalışma alanınızın veri saklama süresini ayarlayarak Azure Izleyici 'de maliyetlerinizi nasıl denetleyeceğinizi açıklanmaktadır.  İlgili bilgiler için aşağıdaki makaleye bakın.
> - [Kullanım ve Tahmini maliyetler izleme](usage-estimated-costs.md) çoklu Azure İzleme özelliklerini farklı fiyatlandırma modelleri için tahmini maliyetleri ve kullanım görüntülemeyi açıklar. Ayrıca, uygulamanızın fiyatlandırma modelinin değiştirilmesi nasıl açıklar.

Azure Izleyici günlükleri, kuruluşunuzda bulunan veya Azure 'da dağıtılan herhangi bir kaynakta çok büyük miktarlarda veri toplamayı, dizinlemesini ve depolamayı, ölçeklendirmek ve desteklemek üzere tasarlanmıştır.  Bu, kuruluşunuz için birincil bir sürücü olabilir, ancak hesaplıdır sonuçta temel alınan sürücüsüdür. Bu uçta, bir Log Analytics çalışma alanının maliyetinin yalnızca toplanan verilerin hacmine dayanmadığını anlamak önemlidir, Ayrıca, seçilen plana de bağlıdır ve bağlı kaynaklarınızdan oluşturulan verileri depolamayı ne kadar tercih edersiniz.  

Bu makalede nasıl proaktif bir şekilde veri hacmi ve depolama büyüme izleyebilir ve bu ilişkili maliyetleri denetleyebilirsiniz sınırlarını tanımlamak inceleyin. 

Veri maliyetine aşağıdaki faktörlere bağlı olarak önemli ölçüde olabilir: 

- Oluşturulan ve çalışma alanına alınan veri hacmi 
    - Etkin yönetim çözümü sayısı
    - İzlenen sistem sayısı
    - İzlenen her kaynaktan toplanan verilerin türü 
- Verilerinizi tutmaya karar verirken geçen sürenin uzunluğu 

## <a name="understand-your-workspaces-usage-and-estimated-cost"></a>Çalışma alanınızın kullanımını ve tahmini maliyetini anlayın

Azure Izleyici günlükleri, en son kullanım desenlerine göre maliyetlerin büyük olasılıkla ne olduğunu anlamak kolaylaşır. Bunu yapmak için, veri kullanımını gözden geçirmek ve çözümlemek üzere **Log Analytics kullanımı ve tahmini maliyetleri** kullanın. Her çözüm tarafından toplanan veri miktarını gösterir, ne kadar veri tutulur ve maliyetlerinizi tahmini temel alınan veri miktarına ve herhangi ek bir saklama dahil edilen miktarın üzerinde.

![Kullanım ve tahmini maliyetler](media/manage-cost-storage/usage-estimated-cost-dashboard-01.png)

Verilerinizi daha ayrıntılı incelemek için üstteki simgeye tıklayın ya da grafikler, sağ **kullanım ve Tahmini maliyetler** sayfası. Artık daha fazla kullanım ayrıntılarını incelemek için bu sorgu ile çalışabilirsiniz.  

![Günlükleri görüntüle](media/manage-cost-storage/logs.png)

**Kullanım ve tahmini maliyetler** sayfasında, veri hacminin ayı için gözden geçirebilirsiniz. Bu, Log Analytics çalışma alanınızda saklanır ve alınan tüm verileri içerir.  Kaynak, bilgisayar ve teklife göre veri hacmi eğilimlerini hakkında bilgi içeren Kullanım panosunu görüntülemek için sayfanın üst kısmından **kullanım ayrıntıları** ' na tıklayın. Görüntüleme ve günlük üst limit ayarlayabilir veya bekletme süresini değiştirmek için tıklayın **veri hacmi Yönetimi**.
 
Log Analytics ücretleri Azure faturanızı eklenir. Azure faturalandırma bölümünde Azure portal'ın veya fatura ayrıntılarını görebilirsiniz [Azure fatura portalı](https://account.windowsazure.com/Subscriptions).  

## <a name="daily-cap"></a>Günlük sınır

Günlük üst sınır yapılandırın ve çalışma alanınız için günlük alımı sınırlayabilirsiniz, ancak hedef günlük limite ulaşılmadan olmamalıdır dikkatli kullanın.  Aksi takdirde, diğer Azure Hizmetleri ve çözümleri olan işlevselliği güncel verileri çalışma alanında kullanılabilir olan bağımlı etkileyebilir günün geri kalanında verileri kaybedersiniz.  Sonuç olarak, BT Hizmetleri destekleyen kaynakların sistem durumu koşullarını etkilendiğinde yeteneğinizi inceleyin ve almak için sizi uyarır.  Günlük üst sınır, yönetilen kaynaklarınızdan alınan veri hacminde beklenmeyen artışı yönetmek için bir yol olarak veya çalışma alanınız için plansız ücretleri sınırlamak istediğinizde kullanılmak üzere tasarlanmıştır.  

Günlük sınıra ulaşıldığında, Faturalanabilir veri türlerinin günlük geri kalanı için durdurur. Seçili Log Analytics çalışma alanı için sayfanın üstündeki bir uyarı başlık görünür ve bir işlemi olay gönderilir *işlemi* altında tablo **LogManagement** kategorisi. Veri toplama sürdürür altında sıfırlama zaman tanımlandıktan sonra *günlük sınır ayarlanacak*. Günlük veri sınırına ulaşıldığında bildirmek için yapılandırılmış. Bu işlem olayı temel alan bir uyarı kuralı tanımlayan öneririz. 

> [!NOTE]
> Günlük sınır Azure Güvenlik Merkezi 'ndeki verilerin toplanmasını durdurmaz.

### <a name="identify-what-daily-data-limit-to-define"></a>Tanımlamak için hangi günlük veri sınırınızın tanımlayın

Gözden geçirme [Log Analytics kullanımı ve Tahmini maliyetler](usage-estimated-costs.md) veri alma eğilimi ve tanımlamak için günlük hacim üst sınırını ne olduğunu anlamak için. Sınıra ulaşıldıktan sonra kaynaklarınızı izleyin mümkün olmayacaktır beri dikkatlice değerlendirilmelidir. 

### <a name="manage-the-maximum-daily-data-volume"></a>Maksimum günlük veri hacmini yönetme

Aşağıdaki adımlarda, Log Analytics çalışma alanının günlük olarak kullanacağı veri hacmini yönetmek için bir sınırın nasıl yapılandırılacağı açıklanır.  

1. Çalışma alanınızın sayfasında, soldaki bölmeden **Kullanım ve tahmini maliyetler**’i seçin.
2. Üzerinde **kullanım ve Tahmini maliyetler** sayfasında seçilen çalışma alanı için **veri hacmi Yönetimi** sayfanın üst. 
3. Günlük üst sınır olan **OFF** varsayılan olarak – tıklayın **ON** etkinleştirin ve ardından veri birimi sınırı GB/gün.

    ![Log Analytics veri sınırı yapılandırma](media/manage-cost-storage/set-daily-volume-cap-01.png)

### <a name="alert-when-daily-cap-reached"></a>Günlük sınıra ulaşıldığında uyar

Veri sınırı eşiğine karşılandığında size görsel bir ipucu Azure portalında mevcut olsa da bu davranış mutlaka Acil dikkat gerektiren işletimsel sorunları nasıl yönettiğiniz için hizalayın değil.  Bir uyarı bildirimine almak, Azure İzleyici'de yeni bir uyarı kuralı oluşturabilirsiniz.  Daha fazla bilgi edinmek için bkz. [Uyarılar oluşturma, görüntüleme ve yönetme](alerts-metric.md).

Başlamanıza yardımcı olmak için uyarı için önerilen ayarları şunlardır:

- Hedef: Log Analytics kaynağınızı seçin
- Ölçütleri: 
   - Sinyal adı: Özel günlük araması
   - Arama sorgusu: İşlem | Burada ayrıntıların ' fazla kota ' olması
   - Temel: Sonuç sayısı
   - Koşul: Büyüktür
   - Eşik: 0
   - Dönemini 5 (dakika)
   - Lemiyor 5 (dakika)
- Uyarı kuralı adı: Günlük veri sınırına ulaşıldı
- Önem derecesi: Uyarı (sev 1)

Uyarı tanımlanır ve sınıra ulaşıldıktan sonra bir uyarı tetiklenir ve eylem grubunda tanımlanan yanıt gerçekleştirir. Aracılığıyla e-posta veya metin iletileriyle ekibinizi bilgilendirin veya Web kancaları, Otomasyon runbook'ları kullanarak işlemleri otomatik hale getirmek veya [dış bir ITSM çözümüyle tümleştirme](itsmc-overview.md#create-itsm-work-items-from-azure-alerts). 

## <a name="change-the-data-retention-period"></a>Veri saklama süresini değiştirme

Aşağıdaki adımları ne kadar günlük verileri çalışma alanınızda tarafından tutulur yapılandırma açıklanmaktadır.
 
1. Çalışma alanınızın sayfasında, soldaki bölmeden **Kullanım ve tahmini maliyetler**’i seçin.
2. **Kullanım ve tahmini maliyetler** sayfasının üst kısmındaki **Veri hacmi yönetimi**'ni seçin.
3. Bölmede artırın veya gün sayısını azaltın, ardından kaydırıcıyı **Tamam**.  Kullanıyorsanız *ücretsiz* katmanı, veri bekletme süresini değiştirmek mümkün olmayacaktır ve bu ayarı denetlemek için ücretli katmana yükseltmeniz gerekir.

    ![Çalışma alanı verilerini bekletme ayarını değiştir](media/manage-cost-storage/manage-cost-change-retention-01.png)
    
Saklama, `dataRetention` parametresini kullanarak [ARM aracılığıyla da ayarlanabilir](https://docs.microsoft.com/azure/azure-monitor/platform/template-workspace-configuration#configure-a-log-analytics-workspace) . Ayrıca, veri bekletmesini 30 güne ayarlarsanız, bu, uyumluluk ile ilgili senaryolar için yararlı olabilecek `immediatePurgeDataOn30Days` parametresini kullanarak eski verilerin hemen temizliğini tetikleyebilirsiniz. Bu işlevsellik yalnızca ARM aracılığıyla sunulur. 

## <a name="legacy-pricing-tiers"></a>Eski fiyatlandırma katmanları

2 Nisan 2018 tarihinden önce Log Analytics bir çalışma alanına veya Application Insights kaynağına sahip olan abonelikler, 1 Şubat 2019 ' den önce başlatılan bir Kurumsal Anlaşma bağlı olmaya devam eder, eski fiyatlandırma katmanlarını kullanmak için erişime sahip olmaya devam edecektir: **Ücretsiz**, **tek başına (GB başına)** ve **düğüm başına (OMS)** .  Ücretsiz fiyatlandırma katmanındaki çalışma alanlarında, günlük veri alımı 500 MB ile sınırlıdır (Azure Güvenlik Merkezi tarafından toplanan güvenlik verileri türleri hariç) ve veri saklama süresi 7 gün ile sınırlıdır. Ücretsiz fiyatlandırma katmanı yalnızca değerlendirme amaçlarıyla tasarlanmıştır. Tek başına veya düğüm başına fiyatlandırma katmanlarında çalışma alanları, Kullanıcı tarafından yapılandırılabilen ve 2 yıla kadar saklama sağlar. 

2016 Nisan 'dan önce oluşturulan çalışma alanları, 30 ve 365 günün sabit veri bekletmesini içeren orijinal **Standart** ve **Premium** fiyatlandırma katmanlarına de erişebilir. Yeni çalışma alanları **Standart** veya **Premium** fiyatlandırma katmanlarında oluşturulamaz ve bir çalışma alanı bu katmanlardan taşınmışsa, geri taşınamaz. 

Fiyatlandırma Katmanı sınırlamalarıyla ilgili daha fazla ayrıntıya [buradan](https://docs.microsoft.com/azure/azure-subscription-service-limits#log-analytics-workspaces)ulaşabilirsiniz.

> [!NOTE]
> System Center için OMS E1 Suite, OMS E2 Suite veya OMS eklentisi satın alma işleminden gelen yetkilendirmeleri kullanmak için *düğüm başına* fiyatlandırma katmanını Log Analytics seçin.


## <a name="changing-pricing-tier"></a>Fiyatlandırma katmanını değiştirme

Log Analytics çalışma alanınızın eski fiyatlandırma katmanlarına erişimi varsa, eski fiyatlandırma katmanları arasında geçiş yapmak için:

1. Azure portalında Log Analytics abonelikleri bölmesinde, bir çalışma alanı seçin.

2. Çalışma Alanı bölmesinden altında **genel**seçin **fiyatlandırma katmanı**.  

3. Altında **fiyatlandırma katmanı**, bir fiyatlandırma katmanı seçin ve ardından **seçin**.  
    ![Seçili fiyatlandırma planı](media/manage-cost-storage/workspace-pricing-tier-info.png)

Ayrıca, `ServiceTier` parametresini kullanarak [fiyatlandırma katmanını ARM aracılığıyla da ayarlayabilirsiniz](https://docs.microsoft.com/azure/azure-monitor/platform/template-workspace-configuration#configure-a-log-analytics-workspace) . 

## <a name="troubleshooting-why-log-analytics-is-no-longer-collecting-data"></a>Log Analytics neden artık veri toplamadığına ilişkin sorun giderme

Eski ücretsiz fiyatlandırma katmanınız varsa ve günde 500 MB 'tan fazla veri gönderdikten sonra, veri toplama günün geri kalanı için duraklar. Günlük sınıra ulaşılması Log Analytics Veri toplamayı durdurur ya da veri eksik gibi görünüyor yaygın bir nedenidir.  Log Analytics'e veri toplamayı başlatır ve durdurur ' % s'olay türü işlemi oluşturur. Günlük sınıra ve eksik verilere ulaşıp ulaşılmayacağını denetlemek için aramada aşağıdaki sorguyu çalıştırın: 

```kusto
Operation | where OperationCategory == 'Data Collection Status'
```

Veri toplama durdurulduğunda, OperationStatus **uyarısı**olur. Veri toplama başladığında, OperationStatus **başarılı**olur. Aşağıdaki tabloda veri toplamayı durdurur nedenleri açıklanır ve veri koleksiyonu devam bir önerilen eylem:  

|Neden koleksiyonu durdurur| Çözüm| 
|-----------------------|---------|
|Eski ücretsiz fiyatlandırma katmanının günlük sınırına ulaşıldı |Koleksiyon otomatik olarak yeniden başlatmak için sonraki güne kadar bekleyin veya Ücretli fiyatlandırma katmanı olarak değiştirme.|
|Çalışma alanınızın günlük tepesine ulaşıldı|Koleksiyonun otomatik olarak yeniden başlatılmasını bekleyin veya en fazla günlük veri birimini yönetme bölümünde açıklanan günlük veri birimi sınırını artırın. Günlük üst sınır sıfırlama zamanı, **veri hacmi yönetimi** sayfasında gösterilir. |
|Azure aboneliği askıya alınma durumuna nedeniyle oluşturulur.<br> Ücretsiz deneme sürümü sona erdi<br> Azure pass süresi doldu<br> Aylık harcama sınırına (örneğin bir MSDN veya Visual Studio abonelik üzerinde)|Ücretli aboneliğe dönüştürme<br> Sınırı kaldırın veya sınır sıfırlar kadar bekleyin|

Veri toplama durdurulduğunda uyarılmak için, veri toplama durdurulduğunda bildirim almak üzere *günlük veri Cap uyarısı oluşturma* bölümünde açıklanan adımları kullanın. Uyarı kuralı için bir e-posta, Web kancası veya Runbook eylemi yapılandırmak üzere [eylem grubu oluşturma](action-groups.md) bölümünde açıklanan adımları kullanın. 

## <a name="troubleshooting-why-usage-is-higher-than-expected"></a>Kullanımın neden beklenenden daha yüksek olduğuyla ilgili sorunları giderme

Yüksek kullanımın nedeni aşağıdakilerden biri veya her ikisidir:
- Log Analytics çalışma alanına beklenenden daha fazla düğüm gönderilemedi
- Log Analytics çalışma alanına gönderilmekte olan beklenenden daha fazla veri

## <a name="understanding-nodes-sending-data"></a>Veri gönderen düğümleri anlama

Son ayın her gününde sinyal raporlayan bilgisayar sayısını anlamak için şunu kullanın

```kusto
Heartbeat | where TimeGenerated > startofday(ago(31d))
| summarize dcount(Computer) by bin(TimeGenerated, 1d)    
| render timechart
```

Düğüm olarak faturalandırılacak bilgisayarların listesini almak için, çalışma alanı, eski düğüm başına fiyatlandırma katmanındaysa, **faturalandırılan veri türlerini** gönderen düğümleri arayın (bazı veri türleri ücretsizdir). Bunu yapmak için, [özelliğini](log-standard-properties.md#_isbillable) kullanın `_IsBillable` ve tam etki alanı adının en solundaki alanı kullanın. Bu, faturalandırılan verileri içeren bilgisayarların listesini döndürür:

```kusto
union withsource = tt * 
| where _IsBillable == true 
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| where computerName != ""
| summarize TotalVolumeBytes=sum(_BilledSize) by computerName
```

Görülen faturalandırılabilir düğümlerin sayısı şu şekilde tahmin edilebilir: 

```kusto
union withsource = tt * 
| where _IsBillable == true 
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| where computerName != ""
| billableNodes=dcount(computerName)
```

> [!NOTE]
> Veri türlerindeki `union withsource = tt *` taramaların yürütülmesi pahalı olduğundan bu sorguları dikkatli bir şekilde kullanın. Bu sorgu, bilgisayar başına bilgilerin kullanım veri türüyle sorgulanması için eski yolu değiştirir.  

Gerçekte faturalandırılacaklarının daha doğru bir şekilde hesaplanması, faturalanan veri türlerini Gönderen saat başına bilgisayar sayısını almak olacaktır. (Eski düğüm başına fiyatlandırma katmanındaki çalışma alanları için, Log Analytics saatlik olarak faturalandırılması gereken düğüm sayısını hesaplar.) 

```kusto
union withsource = tt * 
| where _IsBillable == true 
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| where computerName != ""
| summarize billableNodes=dcount(computerName) by bin(TimeGenerated, 1h) | sort by TimeGenerated asc
```

## <a name="understanding-ingested-data-volume"></a>Alınan veri birimini anlama

Üzerinde **kullanım ve Tahmini maliyetler** sayfasında *çözüm başına veri alımı* grafik, toplam gönderilen veri hacmini ve ne kadar her çözüm tarafından gönderilen verilerin gösterir. Bu sayede olup genel veri kullanımı (veya belirli bir çözüm tarafından kullanım) artıyor mu gibi eğilimleri belirlemek sabit kaldığını veya azaldığını. Bu oluşturmak için kullanılan sorgu

```kusto
Usage | where TimeGenerated > startofday(ago(31d))| where IsBillable == true
| summarize TotalVolumeGB = sum(Quantity) / 1024 by bin(TimeGenerated, 1d), Solution| render barchart
```

Unutmayın yan tümcesi "nerede IsBillable = true" veri türleri için ücretsizdir alımı belirli çözümlerinden filtreler. 

Bkz: veri eğilimlerini IIS günlükler nedeniyle verileri incelemek isterseniz, örneğin belirli veri türleri için daha fazla sınırlandıramazsınız gidebilir:

```kusto
Usage | where TimeGenerated > startofday(ago(31d))| where IsBillable == true
| where DataType == "W3CIISLog"
| summarize TotalVolumeGB = sum(Quantity) / 1024 by bin(TimeGenerated, 1d), Solution| render barchart
```

### <a name="data-volume-by-computer"></a>Bilgisayara göre veri hacmi

Bilgisayar başına alınan faturalandırılabilir olayların **boyutunu** görmek için, boyutu bayt cinsinden sağlayan `_BilledSize` [özelliğini](log-standard-properties.md#_billedsize)kullanın:

```kusto
union withsource = tt * 
| where _IsBillable == true 
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| summarize Bytes=sum(_BilledSize) by  computerName | sort by Bytes nulls last
```

Özelliği, alınan verilerin ücretlendirip ödemeyeceğini belirtir. [](log-standard-properties.md#_isbillable) `_IsBillable`

Bilgisayar başına alınan **faturalandırılabilir** olay sayısını görmek için şunu kullanın: 

```kusto
union withsource = tt * 
| where _IsBillable == true 
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| summarize eventCount=count() by computerName  | sort by count_ nulls last
```

Sayıları Faturalandırılabilir veri türleri için belirli bir bilgisayar için veri gönderdiğini görmek istiyorsanız, bu seçeneği kullanın:

```kusto
union withsource = tt *
| where Computer == "computer name"
| where _IsBillable == true 
| summarize count() by tt | sort by count_ nulls last
```

### <a name="data-volume-by-azure-resource-resource-group-or-subscription"></a>Azure kaynağına, kaynak grubuna veya aboneliğe göre veri hacmi

Azure 'da barındırılan düğümlerdeki veriler için, __bilgisayar başına__alınan faturalandırılabilir olayların **boyutunu** alabilir, kaynağın tam yolunu sağlayan _resourceıd [özelliğini](log-standard-properties.md#_resourceid)kullanın:

```kusto
union withsource = tt * 
| where _IsBillable == true 
| summarize Bytes=sum(_BilledSize) by _ResourceId | sort by Bytes nulls last
```

Azure 'da barındırılan düğümlerdeki veriler için, __Azure aboneliği başına__alınan faturalandırılabilir olayların `_ResourceId` **boyutunu** alabilir, özelliği şu şekilde ayrıştırabilirsiniz:

```kusto
union withsource = tt * 
| where _IsBillable == true 
| parse tolower(_ResourceId) with "/subscriptions/" subscriptionId "/resourcegroups/" 
    resourceGroup "/providers/" provider "/" resourceType "/" resourceName   
| summarize Bytes=sum(_BilledSize) by subscriptionId | sort by Bytes nulls last
```

' A değiştirmek `subscriptionId` , Azure Kaynak grubu ile faturalandırılabilen veri hacmini gösterir. `resourceGroup` 


> [!NOTE]
> Kullanım verileri türünün bazı alanları şemada hala kullanım dışı bırakılmıştır ve değerleri artık doldurulmayacaktır. Bunlar **bilgisayar** alımıyla ilgili alanları yanı sıra (**TotalBatches**, **BatchesWithinSla**, **BatchesOutsideSla**,  **BatchesCapped** ve **AverageProcessingTimeMs**.

### <a name="querying-for-common-data-types"></a>Ortak veri türleri sorgulanıyor

Belirli veri türü için veri kaynağına daha ayrıntılı incelemek için bazı yararlı örnek sorgular şunlardır:

+ **Güvenlik** çözümü
  - `SecurityEvent | summarize AggregatedValue = count() by EventID`
+ **Günlük Yönetimi** çözümü
  - `Usage | where Solution == "LogManagement" and iff(isnotnull(toint(IsBillable)), IsBillable == true, IsBillable == "true") == true | summarize AggregatedValue = count() by DataType`
+ **Perf** veri türü
  - `Perf | summarize AggregatedValue = count() by CounterPath`
  - `Perf | summarize AggregatedValue = count() by CounterName`
+ **Event** veri türü
  - `Event | summarize AggregatedValue = count() by EventID`
  - `Event | summarize AggregatedValue = count() by EventLog, EventLevelName`
+ **Syslog** veri türü
  - `Syslog | summarize AggregatedValue = count() by Facility, SeverityLevel`
  - `Syslog | summarize AggregatedValue = count() by ProcessName`
+ **AzureDiagnostics** veri türü
  - `AzureDiagnostics | summarize AggregatedValue = count() by ResourceProvider, ResourceId`

### <a name="tips-for-reducing-data-volume"></a>Veri hacmini azaltmak için ipuçları

Toplanan günlük hacmini azaltmak için bazı öneriler şunlardır:

| Yüksek veri hacminin kaynağı | Veri hacmi nasıl azaltılır |
| -------------------------- | ------------------------- |
| Güvenlik olayları            | [Yaygın veya en az güvenlik olaylarını](https://docs.microsoft.com/azure/security-center/security-center-enable-data-collection#data-collection-tier) seçin <br> Güvenlik denetimi ilkesini yalnızca gerekli olayları toplayacak şekilde değiştirin. Özellikle, şunlarla ilgili olayları toplamak gerekip gerekmediğini gözden geçirin: <br> - [filtre platformu denetimi](https://technet.microsoft.com/library/dd772749(WS.10).aspx) <br> - [kayıt defteri denetimi](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd941614(v%3dws.10))<br> - [dosya sistemi denetimi](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd772661(v%3dws.10))<br> - [çekirdek nesnesi denetimi](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd941615(v%3dws.10))<br> - [tanıtıcı değiştirme denetimi](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd772626(v%3dws.10))<br> - çıkarılabilir depolama birimi denetimi |
| Performans sayaçları       | [Performans sayacı yapılandırmasını](data-sources-performance-counters.md) şöyle değiştirin: <br> - Koleksiyonun sıklığını azaltın <br> - Performans sayaçlarının sayısını azaltın |
| Olay günlükleri                 | [Olay günlüğü yapılandırmasını](data-sources-windows-events.md) şöyle değiştirin: <br> - Toplanan olay günlüklerinin sayısını azaltın <br> - Yalnızca gerekli olay düzeylerini toplayın. Örneğin, *Bilgi* düzeyindeki olayları toplamayın |
| Syslog                     | [Syslog yapılandırmasını](data-sources-syslog.md) şu şekilde değiştirin: <br> - Toplanan tesislerin sayısını azaltın <br> - Yalnızca gerekli olay düzeylerini toplayın. Örneği *Bilgi* ve *Hata Ayıklama* düzeyindeki olayları toplamayın |
| AzureDiagnostics           | Aşağıdaki amaçlarla kaynak günlüğü koleksiyonunu değiştirin: <br> - Log Analytics’e günlük gönderen kaynak sayısını azaltma <br> - Yalnızca gerekli günlükleri toplama |
| Çözüm ihtiyacı olmayan bilgisayarlardan toplanan çözüm verileri | Yalnızca gerekli bilgisayar gruplarından veri toplamak için [çözüm hedefleme](../insights/solution-targeting.md) özelliğini kullanın. |

### <a name="getting-security-and-automation-node-counts"></a>Güvenlik ve otomasyon düğüm sayılarını alma

"Fiyatlandırma katmanı düğümde (OMS)" olduğunuz sonra düğüm ve çözüm sayısına göre kullanacağınız Insights sayısı ve Analytics düğümleri için faturalandırılır gösterilecek tabloda ücretlendirilir **kullanım ve tahmini maliyet**sayfası.  

Güvenlik düğümleri sayısını görmek için sorguyu kullanabilirsiniz:

```kusto
union
(
    Heartbeat
    | where (Solutions has 'security' or Solutions has 'antimalware' or Solutions has 'securitycenter')
    | project Computer
),
(
    ProtectionStatus
    | where Computer !in~
    (
        (
            Heartbeat
            | project Computer
        )
    )
    | project Computer
)
| distinct Computer
| project lowComputer = tolower(Computer)
| distinct lowComputer
| count
```

Farklı bir Otomasyon düğüm sayısını görmek için sorguyu kullanın:

```kusto
 ConfigurationData 
 | where (ConfigDataType == "WindowsServices" or ConfigDataType == "Software" or ConfigDataType =="Daemons") 
 | extend lowComputer = tolower(Computer) | summarize by lowComputer 
 | join (
     Heartbeat 
       | where SCAgentChannel == "Direct"
       | extend lowComputer = tolower(Computer) | summarize by lowComputer, ComputerEnvironment
 ) on lowComputer
 | summarize count() by ComputerEnvironment | sort by ComputerEnvironment asc
```

## <a name="create-an-alert-when-data-collection-is-high"></a>Veri toplama işlemi yüksekse uyarı oluştur

Bu bölümde, aşağıdaki durumlarda nasıl uyarı oluşturulacağı açıklanır:
- Veri hacmi belirtilen bir miktarı aştığında.
- Veri hacminin belirtilen bir miktarı aşacağı tahmin edildiğinde.

Azure Uyarıları, arama sorguları kullanan [günlük uyarılarını](alerts-unified-log.md) destekler. 

Aşağıdaki sorgu, son 24 saatte 100 GB'den fazla veri toplandığında bir sonuç verir:

```kusto
union withsource = $table Usage 
| where QuantityUnit == "MBytes" and iff(isnotnull(toint(IsBillable)), IsBillable == true, IsBillable == "true") == true 
| extend Type = $table | summarize DataGB = sum((Quantity / 1024)) by Type 
| where DataGB > 100
```

Aşağıdaki sorgu, ne zaman bir günde 100 GB'den fazla veri toplanacağını tahmin etmek için basit bir formül kullanır: 

```kusto
union withsource = $table Usage 
| where QuantityUnit == "MBytes" and iff(isnotnull(toint(IsBillable)), IsBillable == true, IsBillable == "true") == true 
| extend Type = $table 
| summarize EstimatedGB = sum(((Quantity * 8) / 1024)) by Type 
| where EstimatedGB > 100
```

Farklı bir veri hacminde uyarıda bulunmak için, sorgulardaki 100 değerini uyarılmak istediğiniz GB sayısıyla değiştirin.

Toplanan veri beklenen miktarı aştığında size bildirilmesini sağlamak için, [yeni günlük uyarısı oluşturma](alerts-metric.md) başlığı altında açıklanan adımları kullanın.

İlk sorgu için, yani 24 saat içinde 100 GB'den fazla veri toplandığında uyarı oluştururken şu ayarları yapın:  

- **Uyarı koşulunu tanımlama** adımında Log Analytics çalışma alanınızı kaynak hedefi olarak belirtin.
- **Uyarı ölçütleri** alanında aşağıdakileri belirtin:
   - **Sinyal Adı** bölümünde **Özel günlük araması**'nı seçin
   - **Arama sorgusu**: `union withsource = $table Usage | where QuantityUnit == "MBytes" and iff(isnotnull(toint(IsBillable)), IsBillable == true, IsBillable == "true") == true | extend Type = $table | summarize DataGB = sum((Quantity / 1024)) by Type | where DataGB > 100`
   - **Uyarı mantığı**, **Temeli** *bir dizi sonuçtur* ve **Koşul**, *Büyüktür* bir **Eşik değeri**, *0*
   - Kullanım verileri saatte bir güncelleştirildiğinden **Süre** *1440* dakika, **Uyarı sıklığı** ise *60* dakikada bir olarak belirlenmiştir.
- **Uyarı ayrıntılarını tanımlama** adımında aşağıdakileri belirtin:
   - **Ad**: *24 saat içinde 100 GB'den büyük veri hacmi*
   - **Önem derecesi**: *Uyarı*

Günlük uyarısı ölçütlerle eşleştiğinde bilgilendirme yapılması için var olan bir [Eylem Grubunu](action-groups.md) kullanın veya yeni bir tane oluşturun.

İkinci sorgu için, yani 24 saat içinde 100 GB'den fazla veri olacağı tahmin edildiğinde uyarı oluştururken şu ayarları yapın:

- **Uyarı koşulunu tanımlama** adımında Log Analytics çalışma alanınızı kaynak hedefi olarak belirtin.
- **Uyarı ölçütleri** alanında aşağıdakileri belirtin:
   - **Sinyal Adı** bölümünde **Özel günlük araması**'nı seçin
   - **Arama sorgusu**: `union withsource = $table Usage | where QuantityUnit == "MBytes" and iff(isnotnull(toint(IsBillable)), IsBillable == true, IsBillable == "true") == true | extend Type = $table | summarize EstimatedGB = sum(((Quantity * 8) / 1024)) by Type | where EstimatedGB > 100`
   - **Uyarı mantığı**, **Temeli** *bir dizi sonuçtur* ve **Koşul**, *Büyüktür* bir **Eşik değeri**, *0*
   - Kullanım verileri saatte bir güncelleştirildiğinden **Süre** *180* dakika, **Uyarı sıklığı** ise *60* dakikada bir olarak belirlenmiştir.
- **Uyarı ayrıntılarını tanımlama** adımında aşağıdakileri belirtin:
   - **Ad**: *24 saat içinde veri hacminin 100 GB'den büyük olacağı tahmin ediliyor*
   - **Önem derecesi**: *Uyarı*

Günlük uyarısı ölçütlerle eşleştiğinde bilgilendirme yapılması için var olan bir [Eylem Grubunu](action-groups.md) kullanın veya yeni bir tane oluşturun.

Uyarı aldığınızda, kullanımın neden beklenenden fazla olduğu konusundaki sorunları gidermek için aşağıdaki bölümde yer alan adımları kullanın.

## <a name="limits-summary"></a>Limit Özeti

Bazıları Log Analytics fiyatlandırma katmanına bağlı olan bazı ek Log Analytics limitleri vardır. Bunlar [burada](https://docs.microsoft.com/azure/azure-subscription-service-limits#log-analytics-workspaces)belgelenmiştir.


## <a name="next-steps"></a>Sonraki adımlar

- Arama dilinin nasıl kullanılacağını öğrenmek için bkz. [Azure Izleyici günlüklerinde günlük aramaları](../log-query/log-query-overview.md) . Kullanım verilerinde başka analizler yapmak için arama sorgularını kullanabilirsiniz.
- Bir arama ölçütü karşılandığında size bildirilmesini sağlamak için, [yeni günlük uyarısı oluşturma](alerts-metric.md) başlığı altında açıklanan adımları kullanın.
- Yalnızca gerekli bilgisayar gruplarından veri toplamak için [çözüm hedefleme](../insights/solution-targeting.md) özelliğini kullanın.
- Etkin bir olay toplama ilkesini yapılandırmak için [Azure Güvenlik Merkezi filtreleme ilkesini](../../security-center/security-center-enable-data-collection.md)gözden geçirin.
- [Performans sayacı yapılandırmasını](data-sources-performance-counters.md) değiştirin.
- Olay toplama ayarlarınızda değişiklik yapmak için, [olay günlüğü yapılandırması](data-sources-windows-events.md) konusunu gözden geçirin.
- Syslog koleksiyonu ayarlarınızda değişiklik yapmak için, [syslog yapılandırması](data-sources-syslog.md) konusunu gözden geçirin.