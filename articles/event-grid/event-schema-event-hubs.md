---
title: Azure Event Hubs Event Grid kaynak olarak
description: Azure Event Grid olan olay hub 'ları olayları için sunulan özellikleri açıklar
ms.topic: conceptual
ms.date: 07/07/2020
ms.openlocfilehash: 960aa1fe7184e1d02d28fdc135907119fee8f123
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "86113692"
---
# <a name="azure-event-hubs-as-an-event-grid-source"></a>Azure Event Hubs Event Grid kaynak olarak

Bu makalede, Olay Hub 'ları olayları için özellikler ve şema sağlanmaktadır.Olay şemalarına giriş için bkz. [Azure Event Grid olay şeması](event-schema.md).

## <a name="event-grid-event-schema"></a>Event Grid olay şeması

### <a name="available-event-types"></a>Kullanılabilir olay türleri

Event Hubs yakalama dosyası oluşturulduğunda **Microsoft. EventHub. CaptureFileCreated** olay türünü yayar.

### <a name="example-event"></a>Örnek olay

Bu örnek olay, yakalama özelliği bir dosya depoladığında harekete geçirilen bir olay hub 'ı olayının şemasını gösterir: 

```json
[
    {
        "topic": "/subscriptions/<guid>/resourcegroups/rgDataMigrationSample/providers/Microsoft.EventHub/namespaces/tfdatamigratens",
        "subject": "eventhubs/hubdatamigration",
        "eventType": "Microsoft.EventHub.CaptureFileCreated",
        "eventTime": "2017-08-31T19:12:46.0498024Z",
        "id": "14e87d03-6fbf-4bb2-9a21-92bd1281f247",
        "data": {
            "fileUrl": "https://tf0831datamigrate.blob.core.windows.net/windturbinecapture/tfdatamigratens/hubdatamigration/1/2017/08/31/19/11/45.avro",
            "fileType": "AzureBlockBlob",
            "partitionId": "1",
            "sizeInBytes": 249168,
            "eventCount": 1500,
            "firstSequenceNumber": 2400,
            "lastSequenceNumber": 3899,
            "firstEnqueueTime": "2017-08-31T19:12:14.674Z",
            "lastEnqueueTime": "2017-08-31T19:12:44.309Z"
        },
        "dataVersion": "",
        "metadataVersion": "1"
    }
]
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
| veriler | object | Olay Hub 'ı olay verileri. |
| dataVersion | string | Veri nesnesinin şema sürümü. Şema sürümünü yayımcı tanımlar. |
| metadataVersion | string | Olay meta verilerinin şema sürümü. Event Grid en üst düzey özelliklerin şemasını tanımlar. Event Grid bu değeri sağlar. |

Veri nesnesi aşağıdaki özelliklere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| Dosya URL 'si | string | Yakalama dosyasının yolu. |
| fileType | string | Yakalama dosyasının dosya türü. |
| PartitionID | string | Parça KIMLIĞI. |
| sizeInBytes | tamsayı | Dosya boyutu. |
| eventCount | tamsayı | Dosyadaki olay sayısı. |
| firstSequenceNumber | tamsayı | Kuyruktaki en küçük sıra numarası. |
| lastSequenceNumber | tamsayı | Kuyruktaki son sıra numarası. |
| firstEnqueueTime | string | Sıradan ilk kez. |
| lastEnqueueTime | string | Sıradaki son zaman. |

## <a name="tutorials-and-how-tos"></a>Öğreticiler ve nasıl yapılır kılavuzları

|Başlık  |Açıklama  |
|---------|---------|
| [Öğretici: veri ambarına büyük veri akışı](event-grid-event-hubs-integration.md) | Event Hubs bir yakalama dosyası oluşturduğunda, Event Grid bir işlev uygulamasına bir olay gönderir. Uygulama yakalama dosyasını alır ve verileri bir veri ambarına geçirir. |

## <a name="next-steps"></a>Sonraki adımlar

* Azure Event Grid giriş için bkz. [Event Grid nedir?](overview.md)
* Azure Event Grid aboneliği oluşturma hakkında daha fazla bilgi için bkz. [Event Grid abonelik şeması](subscription-creation-schema.md).
* Olay Hub 'ları olaylarını işleme hakkında daha fazla bilgi için bkz. [veri ambarına büyük verileri akışa](event-grid-event-hubs-integration.md)alma.