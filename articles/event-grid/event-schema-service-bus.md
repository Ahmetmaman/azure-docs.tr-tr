---
title: Event Grid kaynak olarak Azure Service Bus
description: Azure Event Grid Service Bus olaylar için belirtilen özellikleri açıklar
ms.topic: conceptual
ms.date: 07/07/2020
ms.openlocfilehash: 81293321b3a8fb989023a231c905996b4059bd81
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "86121143"
---
# <a name="azure-service-bus-as-an-event-grid-source"></a>Event Grid kaynak olarak Azure Service Bus

Bu makalede Service Bus olayları için özellikler ve şema sağlanmaktadır.Olay şemalarına giriş için bkz. [Azure Event Grid olay şeması](event-schema.md).

## <a name="event-grid-event-schema"></a>Event Grid olay şeması

### <a name="available-event-types"></a>Kullanılabilir olay türleri

Service Bus aşağıdaki olay türlerini yayar:

| Olay türü | Açıklama |
| ---------- | ----------- |
| Microsoft. ServiceBus. ActiveMessagesAvailableWithNoListeners | Bir kuyrukta veya abonelikte etkin iletiler olduğunda ve hiçbir alıcı dinlemeden oluşturulur. |
| Microsoft. ServiceBus. DeadletterMessagesAvailableWithNoListener | Sahipsiz bir kuyrukta etkin iletiler olduğunda ve etkin dinleyici olmadığında tetiklenir. |

### <a name="example-event"></a>Örnek olay

Aşağıdaki örnekte, hiçbir dinleyici olayı olmayan etkin iletilerin şeması gösterilmektedir:

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourcegroups/{your-rg}/providers/Microsoft.ServiceBus/namespaces/{your-service-bus-namespace}",
  "subject": "topics/{your-service-bus-topic}/subscriptions/{your-service-bus-subscription}",
  "eventType": "Microsoft.ServiceBus.ActiveMessagesAvailableWithNoListeners",
  "eventTime": "2018-02-14T05:12:53.4133526Z",
  "id": "dede87b0-3656-419c-acaf-70c95ddc60f5",
  "data": {
    "namespaceName": "YOUR SERVICE BUS NAMESPACE WILL SHOW HERE",
    "requestUri": "https://{your-service-bus-namespace}.servicebus.windows.net/{your-topic}/subscriptions/{your-service-bus-subscription}/messages/head",
    "entityType": "subscriber",
    "queueName": "QUEUE NAME IF QUEUE",
    "topicName": "TOPIC NAME IF TOPIC",
    "subscriptionName": "SUBSCRIPTION NAME"
  },
  "dataVersion": "1",
  "metadataVersion": "1"
}]
```

Geçerliliği kalmamış bir sıra olayının şeması benzerdir:

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourcegroups/{your-rg}/providers/Microsoft.ServiceBus/namespaces/{your-service-bus-namespace}",
  "subject": "topics/{your-service-bus-topic}/subscriptions/{your-service-bus-subscription}",
  "eventType": "Microsoft.ServiceBus.DeadletterMessagesAvailableWithNoListener",
  "eventTime": "2018-02-14T05:12:53.4133526Z",
  "id": "dede87b0-3656-419c-acaf-70c95ddc60f5",
  "data": {
    "namespaceName": "YOUR SERVICE BUS NAMESPACE WILL SHOW HERE",
    "requestUri": "https://{your-service-bus-namespace}.servicebus.windows.net/{your-topic}/subscriptions/{your-service-bus-subscription}/$deadletterqueue/messages/head",
    "entityType": "subscriber",
    "queueName": "QUEUE NAME IF QUEUE",
    "topicName": "TOPIC NAME IF TOPIC",
    "subscriptionName": "SUBSCRIPTION NAME"
  },
  "dataVersion": "1",
  "metadataVersion": "1"
}]
```

### <a name="event-properties"></a>Olay özellikleri

Bir olay aşağıdaki en üst düzey verilere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| konu başlığı | string | Olay kaynağının tam kaynak yolu. Bu alan yazılabilir değil. Event Grid bu değeri sağlar. |
| subject | string | Olay konusunun yayımcı tarafından tanımlanan yolu. |
| eventType | string | Bu olay kaynağı için kayıtlı olay türlerinden biri. |
| eventTime | string | Etkinliğin UTC saatine göre oluşturulduğu zaman. |
| kimlik | string | Etkinliğin benzersiz tanımlayıcısı. |
| veriler | object | BLOB depolama olay verileri. |
| dataVersion | string | Veri nesnesinin şema sürümü. Şema sürümünü yayımcı tanımlar. |
| metadataVersion | string | Olay meta verilerinin şema sürümü. Event Grid en üst düzey özelliklerin şemasını tanımlar. Event Grid bu değeri sağlar. |

Veri nesnesi aşağıdaki özelliklere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| Uz | string | Kaynağın mevcut olduğu ad alanı Service Bus. |
| requestUri | string | Olayı yayan belirli bir sıranın veya aboneliğin URI 'SI. |
| entityType | string | Olayların (kuyruk veya abonelik) yayan Service Bus varlık türü. |
| Adı | string | Bir kuyruğa abone olunurken etkin iletileri olan kuyruk. Konular/abonelikler kullanılıyorsa null değeri. |
| topicName | string | Etkin iletilerle Service Bus aboneliğin üyesi olan konu. Kuyruk kullanılıyorsa null değeri. |
| subscriptionName | string | Etkin iletilerle Service Bus aboneliği. Kuyruk kullanılıyorsa null değeri. |

## <a name="tutorials-and-how-tos"></a>Öğreticiler ve nasıl yapılır kılavuzları
|Başlık  |Açıklama  |
|---------|---------|
| [Öğretici: Azure Event Grid tümleştirme örneklerine Azure Service Bus](../service-bus-messaging/service-bus-to-event-grid-integration-example.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Event Grid, Service Bus konudan işlev uygulaması ve mantıksal uygulama 'a ileti gönderir. |
| [Event Grid tümleştirmeye Azure Service Bus](../service-bus-messaging/service-bus-to-event-grid-integration-concept.md) | Service Bus Event Grid tümleştirilmesine genel bakış. |

## <a name="next-steps"></a>Sonraki adımlar

* Azure Event Grid giriş için bkz. [Event Grid nedir?](overview.md)
* Azure Event Grid aboneliği oluşturma hakkında daha fazla bilgi için bkz. [Event Grid abonelik şeması](subscription-creation-schema.md).
* Service Bus ile Azure Event Grid kullanma hakkında ayrıntılı bilgi için bkz. [Service Bus Event Grid tümleştirmeye genel bakış](../service-bus-messaging/service-bus-to-event-grid-integration-concept.md).
* [İşlevlerle veya Logic Apps Service Bus olayları almayı](../service-bus-messaging/service-bus-to-event-grid-integration-example.md?toc=%2fazure%2fevent-grid%2ftoc.json)deneyin.
