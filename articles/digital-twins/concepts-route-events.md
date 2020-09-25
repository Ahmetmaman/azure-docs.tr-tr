---
title: Olay yolları
titleSuffix: Azure Digital Twins
description: Azure dijital TWINS içindeki olayları ve diğer Azure hizmetlerini nasıl yönlendirildiğini öğrenin.
author: baanders
ms.author: baanders
ms.date: 3/12/2020
ms.topic: conceptual
ms.service: digital-twins
ms.openlocfilehash: 96da89fa8d7e4783afa11807534bbaeba52b79fe
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91334268"
---
# <a name="route-events-within-and-outside-of-azure-digital-twins"></a>Azure dijital TWINS 'in içindeki ve dışındaki olayları yönlendirme

Azure dijital TWINS, hizmet dışındaki tüketicilere veri göndermek için **olay yollarını** kullanır. 

Önizleme süresince Azure dijital TWINS verileri göndermek için iki önemli durum vardır:
* Azure dijital TWINS grafiğindeki bir ikizi 'den diğerine veri gönderme. Örneğin, bir dijital ikizi üzerinde bir özellik değiştiğinde, başka bir dijital ikizi 'e göre bildirim almak ve güncelleştirmek isteyebilirsiniz.
* Ek depolama veya işleme ( *veri*çıkışı olarak da bilinir) için aşağı akış veri hizmetlerine veri gönderme. Örneğin,
  - Bir barındırma, toplu analizler için el ile ilgili olayların zaman serisi verilerini kaydetmek üzere Azure dijital TWINS olay verilerini [Time Series Insights (TSI)](../time-series-insights/time-series-insights-update-overview.md)' A göndermek isteyebilir.
  - Zaten [Azure Maps](../azure-maps/about-azure-maps.md) kullanan bir işletme, çözümlerini geliştirmek Için Azure dijital TWINS 'i kullanmak isteyebilir. Azure dijital TWINS 'i ayarladıktan sonra Azure haritasını hızlı bir şekilde etkinleştirebilir, Azure Map varlıklarını ikizi grafiğinde [dijital TWINS](concepts-twins-graph.md) olarak Azure dijital TWINS 'e taşıyın veya Azure haritalarını ve Azure dijital TWINS verilerini birlikte kullanarak güçlü sorgular çalıştırın.

Olay yolları bu senaryoların her ikisi için kullanılır.

## <a name="about-event-routes"></a>Olay rotaları hakkında

Bir olay yolu, Azure dijital TWINS 'deki dijital TWINS 'den, aboneliklerinizdeki özel tanımlı uç noktalara olay verileri göndermenizi sağlar. Şu anda uç noktalar için üç Azure hizmeti desteklenmektedir: [Olay Hub](../event-hubs/event-hubs-about.md)'ı, [Event Grid](../event-grid/overview.md)ve [Service Bus](../service-bus-messaging/service-bus-messaging-overview.md). Bu Azure hizmetlerinin her biri, diğer hizmetlere bağlanabilir ve Middleman olarak görev yapabilir ve ihtiyacınız olan herhangi bir işleme için örneğin, TSI veya Azure haritaları gibi son hedeflere veri gönderiyor.

Aşağıdaki diyagramda, Azure dijital TWINS ile daha büyük bir IoT çözümü aracılığıyla olay verilerinin akışı gösterilmektedir:

:::image type="content" source="media/concepts-route-events/routing-workflow.png" alt-text="Azure dijital TWINS verileri uç noktalar aracılığıyla birçok aşağı akış hizmetine yönlendirme" border="false":::

Olay yolları için tipik aşağı akış hedefleri, TSI, Azure Maps, Storage ve Analytics çözümleri gibi kaynaklardır.

### <a name="event-routes-for-internal-digital-twin-events"></a>İç dijital ikizi olayları için olay yolları

Geçerli önizleme sürümü sırasında olay yolları, ikizi Graph içindeki olayları işlemek ve dijital ikizi 'den dijital ikizi 'e veri göndermek için de kullanılır. Bu, [Azure işlevleri](../azure-functions/functions-overview.md)gibi işlem kaynaklarına Event Grid aracılığıyla olay yolları bağlanarak yapılır. Bu işlevler daha sonra TWINS 'in olayları alıp nasıl yanıtlaması tanımlar. 

Bir işlem kaynağı, olay rotası aracılığıyla aldığı bir olaya göre ikizi grafiğini değiştirmek istediğinde, onun zaman içinde hangi ikizi üzerinde değişiklik yapılmasını istediğini bilmesini sağlamak yararlı olur. 

Alternatif olarak, olay iletisi iletiyi gönderen kaynak ikizi KIMLIĞINI de içerir, bu nedenle işlem kaynağı istenen işlem için bir hedef ikizi bulmak üzere sorguları veya çapraz geçiş ilişkilerini kullanabilir. 

İşlem kaynağının ayrıca güvenlik ve erişim izinlerini bağımsız olarak kurması gerekir.

Dijital ikizi olaylarını işlemek için bir Azure işlevi ayarlama sürecinde gezinmek için bkz. [*nasıl yapılır: verileri işlemek için bir Azure Işlevi ayarlama*](how-to-create-azure-function.md).

## <a name="create-an-endpoint"></a>Uç nokta oluşturma

Bir olay yolu tanımlamak için, geliştiriciler önce uç noktaları tanımlamalıdır. **Uç nokta** , Azure dijital TWINS 'in dışında bir yol bağlantısını destekleyen bir hedefdir. Geçerli önizleme sürümündeki desteklenen hedefler şunlardır:
* Event Grid özel konular
* Olay Hub'ı
* Service Bus

Uç noktalar denetim düzlemi API 'Leri kullanılarak ayarlanır ( [Azure dijital TWINS CLI](how-to-use-cli.md)tarafından desteklenir veya Azure Portal aracılığıyla yapılır. Bir uç nokta tanımı şunları sağlar:
* Uç noktanın adı
* Uç nokta türü (Event Grid, Olay Hub 'ı veya Service Bus)
* Kimlik doğrulaması için birincil bağlantı dizesi ve ikincil bağlantı dizesi 
* Uç noktanın konu yolu (örneğin, *Your-topic.westus2.eventgrid.Azure.net* )

Denetim düzleminde kullanılabilen uç nokta API 'Leri şunlardır:
* Uç nokta oluştur
* Uç noktaların listesini al
* Uç noktayı ada göre al
* Uç noktayı ada göre Sil

## <a name="create-an-event-route"></a>Olay yolu oluşturma
 
Olay yolları bir istemci uygulamasında oluşturulur. Bunu yapmanın bir yolu `CreateEventRoute` [.net (C#) SDK](how-to-use-apis-sdks.md) çağrıdır: 

```csharp
EventRoute er = new EventRoute("endpointName");
er.Filter("true"); //Filter allows all messages
await client.CreateEventRoute("routeName", er);
```

1. İlk olarak, bir `EventRoute` nesne oluşturulur ve Oluşturucu bir uç noktanın adını alır. Bu `endpointName` alan, bir olay hub 'ı, Event Grid veya Service Bus gibi bir uç noktayı tanımlar. Bu uç noktaların aboneliğinizde oluşturulması ve bu kayıt çağrısını yapmadan önce denetim düzlemi API 'Leri kullanılarak Azure Digital TWINS 'e eklenmesi gerekir.

2. Olay Yönlendirme nesnesi Ayrıca, bu yolu izleyen olay türlerini kısıtlamak için kullanılabilecek bir [**filtre**](./how-to-manage-routes-apis-cli.md#filter-events) alanına sahiptir. Bir filtresi `true` , ek filtre olmadan rotayı etkinleştirilir (bir filtre `false` yolu devre dışı bırakır). 

3. Bu olay yolu nesnesi daha sonra `CreateEventRoute` yol için bir adla birlikte öğesine geçirilir.

> [!TIP]
> Tüm SDK işlevleri, zaman uyumlu ve zaman uyumsuz sürümlerde gelir.

Rotalar [Azure Digital TWINS CLI](how-to-use-cli.md)kullanılarak da oluşturulabilir.

### <a name="types-of-event-messages"></a>Olay iletilerinin türleri

IoT Hub ve Azure dijital TWINS 'de farklı olay türleri aşağıda açıklandığı gibi farklı türlerde bildirim iletileri üretir.

[!INCLUDE [digital-twins-notifications.md](../../includes/digital-twins-notifications.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bkz. bir olay yolunu ayarlama ve yönetme:
* [*Nasıl yapılır: uç noktaları ve yolları yönetme*](how-to-manage-routes-apis-cli.md)

Ya da Azure Digital TWINS içindeki olayları yönlendirmek için bkz. Azure Işlevleri 'ni kullanma:
* [*Nasıl yapılır: verileri işlemek için bir Azure işlevi ayarlama*](how-to-create-azure-function.md)