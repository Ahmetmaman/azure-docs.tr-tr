---
title: Akış alma olay kaynakları-Azure Time Series Insights Gen2 | Microsoft Docs
description: Azure Time Series Insights Gen2 ' e akış verileri hakkında bilgi edinin.
author: lyrana
ms.author: lyhughes
manager: deepakpalled
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 01/19/2021
ms.openlocfilehash: 7b7e29b6e2ebb3b229045df439848264540b59b1
ms.sourcegitcommit: afb9e9d0b0c7e37166b9d1de6b71cd0e2fb9abf5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/14/2021
ms.locfileid: "103461632"
---
# <a name="azure-time-series-insights-gen2-event-sources"></a>Azure Time Series Insights Gen2 olay kaynakları

 Azure Time Series Insights Gen2 ortamınız en fazla iki akış olay kaynağına sahip olabilir. Giriş olarak iki tür Azure kaynağı desteklenir:

- [Azure IoT Hub](../iot-hub/about-iot-hub.md)
- [Azure Event Hubs](../event-hubs/event-hubs-about.md)

Olayların UTF-8 kodlu JSON olarak gönderilmesi gerekir.

## <a name="create-or-edit-event-sources"></a>Olay kaynakları oluşturma veya düzenleme

Olay kaynak kaynağınız, Azure Time Series Insights Gen2 ortamınız veya farklı bir abonelikle aynı Azure aboneliğinde bulunabilir. Ortamınızın olay kaynaklarını oluşturmak, düzenlemek veya kaldırmak için [Azure Portal](./tutorial-set-up-environment.md#create-an-azure-time-series-insights-gen2-environment), [Azure CLI](https://github.com/Azure/azure-cli-extensions/tree/master/src/timeseriesinsights), [ARM şablonları](time-series-insights-manage-resources-using-azure-resource-manager-template.md)ve [REST API](/rest/api/time-series-insights/management(gen1/gen2)/eventsources) kullanabilirsiniz.

Bir olay kaynağını bağladığınızda, Azure Time Series Insights Gen2 ortamınız, en eski olayla başlayarak IoT veya Olay Hub 'ınızda depolanmış olan tüm olayları okur.

> [!IMPORTANT]
>
> - Azure Time Series Insights Gen2 ortamınıza bir olay kaynağı eklerken yüksek gecikme süresine karşılaşabilirsiniz.
> - Olay kaynağı gecikmesi, IoT Hub veya Olay Hub 'ınızdaki olay sayısına bağlıdır.
> - Olay kaynak verilerinin ilk kez alındıktan sonra yüksek gecikme süresi alt tarafı olur. Devam eden yüksek gecikme süresi yaşarsanız Azure portal aracılığıyla bir destek bileti gönderebilirsiniz.

## <a name="streaming-ingestion-best-practices"></a>Akış alma en iyi uygulamaları

- Olay kaynağınızdaki verileri tüketmek için Azure Time Series Insights Gen2 ortamınız için her zaman benzersiz bir tüketici grubu oluşturun. Tüketici gruplarını yeniden kullanmak rastgele kesilen kesilebilir ve veri kaybına yol açabilir.

- Azure Time Series Insights Gen2 ortamınızı ve IoT Hub ve/veya Event Hubs aynı Azure bölgesinde yapılandırın. Bir olay kaynağını ayrı bir bölgede yapılandırmak mümkün olsa da, bu senaryo desteklenmez ve yüksek kullanılabilirlik garantisi vermeyiz.

- Ortamınızın [işleme hızı sınırının](./concepts-streaming-ingress-throughput-limits.md) ötesine geçmeyin ve bölüm sayısı sınırı yoktur.

- Ortamınız verileri işlerken sorunlarla karşılaşıyorsa bildirim almak için bir gecikme [uyarısı](./time-series-insights-environment-mitigate-latency.md#monitor-latency-and-throttling-with-alerts) yapılandırın. Önerilen uyarı koşulları için aşağıdaki [üretim iş yükleri](./concepts-streaming-ingestion-event-sources.md#production-workloads) bölümüne bakın.

- Yalnızca neredeyse gerçek zamanlı ve en son veriler için akış alımı kullanın, geçmiş verileri akışa alma desteklenmez.

- Özelliklerin nasıl kaçırılması gerektiğini ve JSON [verilerinin düzleştirilmiş ve nasıl depolandığını anlayın.](./concepts-json-flattening-escaping-rules.md)

- Olay kaynağı bağlantı dizeleri sağlarken en az ayrıcalık ilkesini izleyin. Event Hubs için, yalnızca *gönderme* talebiyle bir paylaşılan erişim ilkesi yapılandırın ve IoT Hub için yalnızca *hizmet bağlantı* iznini kullanın.

## <a name="production-workloads"></a>Üretim iş yükleri

Yukarıdaki en iyi yöntemlere ek olarak, iş açısından kritik iş yükleri için aşağıdakileri uygulamanızı öneririz.

- IoT Hub veya Olay Hub 'ı veri saklama süresini en fazla 7 güne yükseltin.

- Azure portal ortam uyarıları oluşturun. Platform [ölçümlerini](./how-to-monitor-tsi-reference.md#metrics) temel alan uyarılar, uçtan uca ardışık düzen davranışını doğrulamanızı sağlar. Uyarı oluşturma ve yönetme yönergelerine [buradan](./time-series-insights-environment-mitigate-latency.md#monitor-latency-and-throttling-with-alerts)ulaşabilirsiniz. Önerilen uyarı koşulları:

  - Inressreceived Iletizamanı gecikme 5 dakikadan fazla
  - Alınan baytlar 0 ' dır
- IoT Hub veya Olay Hub 'ı bölümleriniz arasında alma yük dengelenme süresini koruyun.

### <a name="historical-data-ingestion"></a>Geçmiş veri alımı

Geçmiş verileri içeri aktarmak için akış işlem hattının kullanılması şu anda Azure Time Series Insights Gen2 sürümünde desteklenmemektedir. Eski verileri ortamınıza aktarmanız gerekiyorsa aşağıdaki yönergeleri izleyin:

- Canlı ve geçmiş verileri paralel olarak akışmayın. Verilerin dışına inmek, sorgu performansının düşmesine neden olur.
- En iyi performansı elde etmek için geçmiş verileri zaman düzenli olarak alma.
- Aşağıdaki alma işlemi hız sınırları dahilinde kalır.
- Veriler, sıcak mağaza saklama süresinden daha eskiyse, sıcak mağazayı devre dışı bırakın.

## <a name="event-source-timestamp"></a>Olay kaynağı zaman damgası

Bir olay kaynağını yapılandırırken zaman damgası KIMLIĞI özelliği sağlamanız istenir. Zaman damgası özelliği, olayları zamana göre izlemek için kullanılır. [sorgu API 'lerinde](/rest/api/time-series-insights/dataaccessgen2/query/execute) ve Azure Time Series Insights Explorer 'da dizi çizmek için $event. $TS olarak kullanılacak süredir. Oluşturma zamanında hiçbir özellik sağlanmazsa veya bir olayda Timestamp özelliği eksikse, olayın IoT Hub veya Olay Hub 'Ları sıraya alınan süre varsayılan olarak kullanılır. Zaman damgası özellik değerleri UTC olarak depolanır.

Genel olarak, kullanıcılar zaman damgası özelliğini özelleştirmeyi ve algılayıcı ya da etiketin varsayılan Merkez sıraya alma süresini kullanmak yerine okumayı oluşturduğu zamanı kullanması için kabul edecektir. Bu özellikle, cihazlarda aralıklı bağlantı kaybı olduğunda ve bir toplu işlem gecikmeli ileti Azure Time Series Insights Gen2 'e iletildiğinde gereklidir.

Özel zaman Damgalarınız iç içe geçmiş bir JSON nesnesi veya bir dizi içindeyse, [düzleştirme ve kaçış adlandırma kurallarımızın ardından](concepts-json-flattening-escaping-rules.md)doğru özellik adını sağlamanız gerekir. Örneğin, [burada](concepts-json-flattening-escaping-rules.md#example-a) gösterilen JSON yükünün olay kaynağı zaman damgası olarak girilmelidir `"values.time"` .

### <a name="time-zone-offsets"></a>Saat dilimi uzaklıkları

Tarih damgalarının ISO 8601 biçiminde gönderilmesi ve UTC 'de depolanacak olması gerekir. Bir saat dilimi boşluğu sağlanırsa, fark uygulanır ve ardından UTC biçiminde saklanan ve döndürülen zaman değişir. Fark düzgün biçimlendirilmediyse, yok sayılır. Çözümünüzün orijinal uzaklığa sahip olmadığı durumlarda, korunan ve uygulamanızın bir sorgu yanıtında başvuracağından emin olmak için, fark verilerini ek bir ayrı olay özelliğine gönderebilirsiniz.

Saat dilimi boşluğu, aşağıdakilerden biri olarak biçimlendirilmelidir:

± HHMMZ</br>
± HH: MM</br>
± HH: MMZ</br>

## <a name="next-steps"></a>Sonraki adımlar

- Olayların nasıl depolanacağını anlamak için [JSON düzleştirme ve kaçış kurallarını](./concepts-json-flattening-escaping-rules.md) okuyun.

- Ortamınızın [verimlilik sınırlamalarını](./concepts-streaming-ingress-throughput-limits.md) anlayın
