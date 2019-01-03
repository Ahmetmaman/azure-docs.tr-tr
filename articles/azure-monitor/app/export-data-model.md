---
title: Azure Application Insights veri modeli | Microsoft Docs
description: JSON içinde sürekli dışarı aktarmayı öğesinden dışarı aktarılan ve filtre olarak kullanılan özellikleri tanımlar.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: cabad41c-0518-4669-887f-3087aef865ea
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 03/21/2016
ms.author: mbullwin
ms.openlocfilehash: a3cab6af86a18e23199437c91b6d07102e783cd1
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/02/2019
ms.locfileid: "53971282"
---
# <a name="application-insights-export-data-model"></a>Application Insights dışa aktarma veri modeli
Bu tabloda gönderilen telemetri özelliklerinin [Application Insights](../../application-insights/app-insights-overview.md) SDK'ları portalı.
Bu özellikler, veri çıkışı görürsünüz [sürekli dışarı aktarma](export-telemetry.md).
Ayrıca özellik filtrelerini görünürler [ölçüm Gezgini'nde](../../application-insights/app-insights-metrics-explorer.md) ve [tanılama araması](../../azure-monitor/app/diagnostic-search.md).

Dikkat edilecek noktalar:

* `[0]` Bu tablolara bir dizin eklemek için sahip olduğu yolu noktasında gösterir; ancak her zaman değil 0.
* Süreler içinde onda bir mikrosaniye ölçeğinde, bu nedenle 10000000 biri olan == 1 saniye.
* Tarihler ve saatler UTC olan ve ISO biçiminde verilir `yyyy-MM-DDThh:mm:ss.sssZ`


## <a name="example"></a>Örnek
    // A server report about an HTTP request
    {
    "request": [
      {
        "urlData": { // derived from 'url'
          "host": "contoso.org",
          "base": "/",
          "hashTag": ""
        },
        "responseCode": 200, // Sent to client
        "success": true, // Default == responseCode<400
        // Request id becomes the operation id of child events
        "id": "fCOhCdCnZ9I=",  
        "name": "GET Home/Index",
        "count": 1, // 100% / sampling rate
        "durationMetric": {
          "value": 1046804.0, // 10000000 == 1 second
          // Currently the following fields are redundant:
          "count": 1.0,
          "min": 1046804.0,
          "max": 1046804.0,
          "stdDev": 0.0,
          "sampledValue": 1046804.0
        },
        "url": "/"
      }
    ],
    "internal": {
      "data": {
        "id": "7f156650-ef4c-11e5-8453-3f984b167d05",
        "documentVersion": "1.61"
      }
    },
    "context": {
      "device": { // client browser
        "type": "PC",
        "screenResolution": { },
        "roleInstance": "WFWEB14B.fabrikam.net"
      },
      "application": { },
      "location": { // derived from client ip
        "continent": "North America",
        "country": "United States",
        // last octagon is anonymized to 0 at portal:
        "clientip": "168.62.177.0",
        "province": "",
        "city": ""
      },
      "data": {
        "isSynthetic": true, // we identified source as a bot
        // percentage of generated data sent to portal:
        "samplingRate": 100.0,
        "eventTime": "2016-03-21T10:05:45.7334717Z" // UTC
      },
      "user": {
        "isAuthenticated": false,
        "anonId": "us-tx-sn1-azr", // bot agent id
        "anonAcquisitionDate": "0001-01-01T00:00:00Z",
        "authAcquisitionDate": "0001-01-01T00:00:00Z",
        "accountAcquisitionDate": "0001-01-01T00:00:00Z"
      },
      "operation": {
        "id": "fCOhCdCnZ9I=",
        "parentId": "fCOhCdCnZ9I=",
        "name": "GET Home/Index"
      },
      "cloud": { },
      "serverDevice": { },
      "custom": { // set by custom fields of track calls
        "dimensions": [ ],
        "metrics": [ ]
      },
      "session": {
        "id": "65504c10-44a6-489e-b9dc-94184eb00d86",
        "isFirst": true
      }
    }
  }

## <a name="context"></a>Bağlam
Tüm telemetri türlerini içerik bölümü tarafından yayımlanır. Tüm bu alanlar her veri noktası ile aktarılır.

| Yol | Tür | Notlar |
| --- | --- | --- |
| Context.Custom.Dimensions [0] |Nesne] |Anahtar-değer çiftleri özel özellikler parametresiyle ayarlayın. Anahtar uzunluğu en fazla 100, en fazla uzunluğu 1024 değerleri. 100'den fazla benzersiz değerleri özelliği aranabilir ancak segmentlere ayırma için kullanılamaz. İkey başına en fazla 200 anahtarları. |
| Context.Custom.Metrics [0] |Nesne] |Anahtar-değer çiftleri özel ölçümleri parametresi ve TrackMetrics göre ayarlayın. Anahtar en büyük uzunluğunu 100, sayısal değerler olabilir. |
| context.data.eventTime |dize |UTC |
| context.data.isSynthetic |boole |İstek bir bot veya web testi görünür. |
| context.data.samplingRate |number |Portala gönderilen SDK'sı tarafından oluşturulan telemetri yüzdesi. 0,0-100.0 aralığı. |
| Context.Device |object |İstemci cihazı |
| Context.Device.browser |dize |IE, Chrome... |
| context.device.browserVersion |dize |Chrome 48,0... |
| context.device.deviceModel |dize | |
| context.device.deviceName |dize | |
| Context.Device.id |dize | |
| Context.Device.Locale |dize |en-GB, de-DE... |
| Context.Device.Network |dize | |
| context.device.oemName |dize | |
| context.device.osVersion |dize |Konak işletim sistemi |
| context.device.roleInstance |dize |Sunucu ana bilgisayar kimliği |
| context.device.roleName |dize | |
| Context.Device.Type |dize |PC, tarayıcı... |
| Context.location |object |Clientıp türetilmiş. |
| Context.location.City |dize |Biliniyorsa clientıp türetilmiş |
| Context.location.clientip |dize |Son bir sekizgen 0 olarak anonimleştirilmiştir. |
| Context.location.continent |dize | |
| Context.location.country |dize | |
| Context.location.Province |dize |Eyalet veya il |
| Context.Operation.id |dize |Aynı işlem kimliğine sahip öğeler, portalda ilişkili öğeleri olarak gösterilir. Genellikle istek kimliği. |
| Context.Operation.Name |dize |URL veya istek adı |
| context.operation.parentId |dize |İç içe geçmiş ilgili öğeleri sağlar. |
| Context.Session.id |dize |İşlemlerin aynı kaynak grubunun kimliği. Bir işlem olmadan 30 dakikalık bir süre sonuna bir oturumu bildirir. |
| context.session.isFirst |boole | |
| context.user.accountAcquisitionDate |dize | |
| context.user.anonAcquisitionDate |dize | |
| context.user.anonId |dize | |
| context.user.authAcquisitionDate |dize |[Kimliği doğrulanmış kullanıcı](../../azure-monitor/app/api-custom-events-metrics.md#authenticated-users) |
| context.user.isAuthenticated |boole | |
| internal.data.documentVersion |dize | |
| internal.Data.id |dize | Bir öğe alınan zaman Application Insights'a atanan benzersiz kimliği |

## <a name="events"></a>Olaylar
Tarafından oluşturulan özel olaylar [TrackEvent()](../../azure-monitor/app/api-custom-events-metrics.md#trackevent).

| Yol | Tür | Notlar |
| --- | --- | --- |
| [0] olay sayısı |integer |100 / ([örnekleme](../../application-insights/app-insights-sampling.md) oranı). Örneğin, 4 =&gt; % 25. |
| [0] olay adı |dize |Olay adı.  Maks. uzunluk 250. |
| Olay [0] URL'si |dize | |
| Olay [0] urlData.base |dize | |
| Olay [0] urlData.host |dize | |

## <a name="exceptions"></a>Özel durumlar
Raporları [özel durumları](../../azure-monitor/app/asp-net-exceptions.md) sunucunun ve tarayıcı.

| Yol | Tür | Notlar |
| --- | --- | --- |
| basicException [0] derlemesi |dize | |
| [0] basicException sayısı |integer |100 / ([örnekleme](../../application-insights/app-insights-sampling.md) oranı). Örneğin, 4 =&gt; % 25. |
| [0] basicException exceptionGroup |dize | |
| exceptionType basicException [0] |dize | |
| [0] basicException failedUserCodeMethod |dize | |
| [0] basicException failedUserCodeAssembly |dize | |
| [0] basicException handledAt |dize | |
| [0] basicException hasFullStack |boole | |
| basicException [0] kimliği |dize | |
| [0] basicException yöntemi |dize | |
| [0] basicException iletisi |dize |Özel durum iletisi. En çok 10k. |
| [0] basicException outerExceptionMessage |dize | |
| [0] basicException outerExceptionThrownAtAssembly |dize | |
| [0] basicException outerExceptionThrownAtMethod |dize | |
| [0] basicException outerExceptionType |dize | |
| [0] basicException outerId |dize | |
| [0] basicException parsedStack [0] derlemesi |dize | |
| [0] basicException parsedStack [0] dosya adı |dize | |
| [0] basicException parsedStack [0] düzeyi |integer | |
| basicException [0] [0] parsedStack satır |integer | |
| basicException [0] [0] parsedStack yöntemi |dize | |
| [0] basicException yığını |dize |En çok 10k |
| typeName basicException [0] |dize | |

## <a name="trace-messages"></a>İzleme İletileri
Tarafından gönderilen [TrackTrace](../../azure-monitor/app/api-custom-events-metrics.md#tracktrace)ve bunun [günlük bağdaştırıcıları](../../azure-monitor/app/asp-net-trace-logs.md).

| Yol | Tür | Notlar |
| --- | --- | --- |
| ileti [0] GünlükçüAdı |dize | |
| [0] ileti parametreleri |dize | |
| ileti [0] ham |dize |Günlük iletisi, uzunluğu 10k. |
| ileti [0] Err |dize | |

## <a name="remote-dependency"></a>Uzak bağımlılık
TrackDependency tarafından gönderilir. Rapor performansını ve kullanımını için kullanılan [bağımlılıklara yapılan çağrıları](../../azure-monitor/app/asp-net-dependencies.md) sunucu ve tarayıcıda AJAX çağrıları.

| Yol | Tür | Notlar |
| --- | --- | --- |
| [0] remoteDependency zaman uyumsuz |boole | |
| [0] remoteDependency baseName |dize | |
| [0] remoteDependency commandName |dize |Örneğin "home/Index" |
| [0] remoteDependency sayısı |integer |100 / ([örnekleme](../../application-insights/app-insights-sampling.md) oranı). Örneğin, 4 =&gt; % 25. |
| [0] remoteDependency dependencyTypeName |dize |HTTP SQL... |
| [0] remoteDependency durationMetric.value |number |Bağımlılık yanıt tamamlanmasından çağrı süresi |
| remoteDependency [0] kimliği |dize | |
| [0] remoteDependency adı |dize |URL. Maks. uzunluk 250. |
| [0] remoteDependency resultCode |dize |HTTP bağımlılık |
| [0] remoteDependency başarılı |boole | |
| remoteDependency [0] türü |dize |HTTP Sql... |
| remoteDependency [0] URL'si |dize |En çok 2000 |
| [0] remoteDependency urlData.base |dize |En çok 2000 |
| [0] remoteDependency urlData.hashTag |dize | |
| [0] remoteDependency urlData.host |dize |Maks. uzunluk 200 |

## <a name="requests"></a>İstekler
Tarafından gönderilen [TrackRequest](../../azure-monitor/app/api-custom-events-metrics.md#trackrequest). Standart modülleri bu sunucuda ölçülen raporları sunucu yanıt süresi için kullanın.

| Yol | Tür | Notlar |
| --- | --- | --- |
| [0] istek sayısı |integer |100 / ([örnekleme](../../application-insights/app-insights-sampling.md) oranı). Örneğin: 4 =&gt; % 25. |
| İstek [0] durationMetric.value |number |İstekten gelen yanıt süresi. 1e7 1s == |
| [0] istek kimliği |dize |İşlem kimliği |
| [0] istek adı |dize |GET/POST + temel URL'si.  Maks. uzunluk 250 |
| [0] istek yanıt kodu |integer |İstemciye gönderilen HTTP yanıtı |
| [0] istek başarılı |boole |Varsayılan == (yanıt kodu &lt; 400) |
| [0] istek URL'si |dize |Konak dahil değil |
| İstek [0] urlData.base |dize | |
| İstek [0] urlData.hashTag |dize | |
| İstek [0] urlData.host |dize | |

## <a name="page-view-performance"></a>Sayfa görüntüleme performansı
Tarayıcı tarafından gönderilir. Bir sayfadan (zaman uyumsuz AJAX çağrıları) tamamını görüntülemek için isteği başlatmaktan kullanıcı işlemek için süreyi ölçer.

Bağlam değerleri, istemci işletim sistemi ve tarayıcı sürümü gösterir.

| Yol | Tür | Notlar |
| --- | --- | --- |
| [0] clientPerformance clientProcess.value |integer |Son sayfa görüntüleme için HTML alma süresi. |
| [0] clientPerformance adı |dize | |
| [0] clientPerformance networkConnection.value |integer |Bir ağ bağlantısı kurmak için geçen süre. |
| [0] clientPerformance receiveRequest.value |integer |Son HTML yanıt almak için istek gönderme süresi. |
| [0] clientPerformance sendRequest.value |integer |Saati HTTP isteği göndermek için gerçekleştirilecek. |
| [0] clientPerformance total.value |integer |Sayfa görüntüleme için istek gönderebilirsiniz başlayarak başlangıç saati'ı tıklatın. |
| clientPerformance [0] URL'si |dize |Bu isteğin URL'si |
| [0] clientPerformance urlData.base |dize | |
| [0] clientPerformance urlData.hashTag |dize | |
| [0] clientPerformance urlData.host |dize | |
| [0] clientPerformance urlData.protocol |dize | |

## <a name="page-views"></a>Sayfa Görüntülemeleri
TrackPageView() tarafından gönderilen veya [stopTrackPage](../../azure-monitor/app/api-custom-events-metrics.md#page-views)

| Yol | Tür | Notlar |
| --- | --- | --- |
| [0] görüntüleme sayısı |integer |100 / ([örnekleme](../../application-insights/app-insights-sampling.md) oranı). Örneğin, 4 =&gt; % 25. |
| [0] durationMetric.value görüntüleyin |integer |Değeri trackPageView() veya startTrackPage() - isteğe bağlı olarak ayarlanmış stopTrackPage(). Aynı clientPerformance değerleri. |
| [0] Görünüm adı |dize |Sayfa başlığı.  Maks. uzunluk 250 |
| Görünüm [0] URL'si |dize | |
| [0] urlData.base görüntüleyin |dize | |
| [0] urlData.hashTag görüntüleyin |dize | |
| [0] urlData.host görüntüleyin |dize | |

## <a name="availability"></a>Kullanılabilirlik
Raporları [kullanılabilirlik web testleri](../../azure-monitor/app/monitor-web-app-availability.md).

| Yol | Tür | Notlar |
| --- | --- | --- |
| Kullanılabilirlik [0] availabilityMetric.name |dize |availability |
| Kullanılabilirlik [0] availabilityMetric.value |number |1.0 ya da 0.0 |
| Kullanılabilirlik [0] sayısı |integer |100 / ([örnekleme](../../application-insights/app-insights-sampling.md) oranı). Örneğin, 4 =&gt; % 25. |
| Kullanılabilirlik [0] dataSizeMetric.name |dize | |
| Kullanılabilirlik [0] dataSizeMetric.value |integer | |
| Kullanılabilirlik [0] durationMetric.name |dize | |
| Kullanılabilirlik [0] durationMetric.value |number |Test süresi. 1e7 1s == |
| [0] kullanılabilirlik iletisi |dize |Tanılama hatası |
| [0] kullanılabilirlik sonucu |dize |Geçer/başarısız |
| Kullanılabilirlik [0] runLocation |dize |Http isteği coğrafi kaynağı |
| Kullanılabilirlik [0] testName |dize | |
| Kullanılabilirlik [0] Testrunıd |dize | |
| Kullanılabilirlik [0] testTimestamp |dize | |

## <a name="metrics"></a>Ölçümler
Trackmetric() işlevine tarafından oluşturulur.

Ölçüm değeri context.custom.metrics[0 içinde bulunan]

Örneğin:

    {
     "metric": [ ],
     "context": {
     ...
     "custom": {
        "dimensions": [
          { "ProcessId": "4068" }
        ],
        "metrics": [
          {
            "dispatchRate": {
              "value": 0.001295,
              "count": 1.0,
              "min": 0.001295,
              "max": 0.001295,
              "stdDev": 0.0,
              "sampledValue": 0.001295,
              "sum": 0.001295
            }
          }
         } ] }
    }

## <a name="about-metric-values"></a>Ölçüm değerleri hakkında
Ölçüm raporları ve başka bir yerde, ölçüm değerleri bir standart nesne yapısını raporlanır. Örneğin:

      "durationMetric": {
        "name": "contoso.org",
        "type": "Aggregation",
        "value": 468.71603053650279,
        "count": 1.0,
        "min": 468.71603053650279,
        "max": 468.71603053650279,
        "stdDev": 0.0,
        "sampledValue": 468.71603053650279
      }

Şu anda - yine de bu değişebilir gelecekte - tüm değerlerin standart SDK modüllerden bildirilen `count==1` ve yalnızca `name` ve `value` alanları kullanışlıdır. Burada, olacak farklı yalnızca kendi TrackMetric çağrıları, yazma olur diğer parametreler kümesi'ndeki.

Diğer alanları amacı, portala trafiğini azaltmak için SDK, toplanacak ölçümleri izin vermektir. Örneğin, her bir ölçüm raporu göndermeden önce birkaç art arda gelen okumalar ortalama. Daha sonra min, max, standart sapma ve (sum veya average) toplam değer hesaplamak ve rapor tarafından temsil edilen okumalar sayısına sayınızın.

Yukarıdaki tablolarda, biz nadiren kullanılan alanların sayısı, min, max, stdDev ve sampledValue atlanmış.

Önceden toplama ölçümleri yerine kullanabileceğiniz [örnekleme](../../application-insights/app-insights-sampling.md) telemetri hacmini azaltmak gerekiyorsa.

### <a name="durations"></a>Süreleri
Aksi takdirde belirtilenler dışında süreleri 10000000.0 1 saniye anlamına onda mikrosaniye ölçeğinde, biri gösterilir.

## <a name="see-also"></a>Ayrıca bkz.
* [Application Insights](../../application-insights/app-insights-overview.md)
* [Sürekli dışarı aktarma](export-telemetry.md)
* [Kod örnekleri](export-telemetry.md#code-samples)
