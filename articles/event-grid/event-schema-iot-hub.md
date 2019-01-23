---
title: IOT hub'ın Azure Event Grid şeması | Microsoft Docs
description: Olay şeması biçimi ve IOT Hub'ının özellikler için başvuru sayfası
services: iot-hub
documentationcenter: ''
author: kgremban
manager: timlt
editor: ''
ms.service: event-grid
ms.topic: reference
ms.date: 01/17/2019
ms.author: kgremban
ms.openlocfilehash: df1c0f8256b49e23b720df47c513fba8c62677b5
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54475212"
---
# <a name="azure-event-grid-event-schema-for-iot-hub"></a>IOT hub'ın Azure Event Grid olay şeması

Bu makalede, Azure IOT Hub olaylarını şema ve özellikleri sağlar. Olay şemaları için bir giriş için bkz [Azure Event Grid olay şeması](event-schema.md). 

Örnek betikler ve öğreticiler listesi için bkz: [IOT Hub olay kaynağı](event-sources.md#iot-hub).

## <a name="available-event-types"></a>Kullanılabilir olay türleri

Azure IOT Hub aşağıdaki olay türlerini gösterir:

| Olay türü | Açıklama |
| ---------- | ----------- |
| Microsoft.Devices.DeviceCreated | IOT hub'a bir cihaz kaydedildiğinde yayımladı. |
| Microsoft.Devices.DeviceDeleted | Bir cihaz IOT hub'ından silindiğinde yayımladı. | 
| Microsoft.Devices.DeviceConnected | Bir cihaz IOT hub'a bağlandığında yayımladı. |
| Microsoft.Devices.DeviceDisconnected | Bir cihaz IOT hub'ından kesildiğinde yayımladı. | 

## <a name="example-event"></a>Örnek olay

Şema DeviceConnected ve DeviceDisconnected olayları için aynı yapıya sahip. Bu örnek olay, bir cihaz IOT hub'a bağlı olduğunda gerçekleşen bir olay şeması gösterir:

```json
[{
  "id": "f6bbf8f4-d365-520d-a878-17bf7238abd8", 
  "topic": "/SUBSCRIPTIONS/<subscription ID>/RESOURCEGROUPS/<resource group name>/PROVIDERS/MICROSOFT.DEVICES/IOTHUBS/<hub name>", 
  "subject": "devices/LogicAppTestDevice", 
  "eventType": "Microsoft.Devices.DeviceConnected", 
  "eventTime": "2018-06-02T19:17:44.4383997Z", 
  "data": {
    "deviceConnectionStateEventInfo": {
      "sequenceNumber":
        "000000000000000001D4132452F67CE200000002000000000000000000000001"
    },
    "hubName": "egtesthub1",
    "deviceId": "LogicAppTestDevice",
    "moduleId" : "DeviceModuleID"
  }, 
  "dataVersion": "1", 
  "metadataVersion": "1" 
}]
```

Şema DeviceCreated ve DeviceDeleted olayları için aynı yapıya sahip. Bu örnek olay, bir IOT hub'a bir cihaz kaydedildiğinde gerçekleşen bir olay şeması gösterir:

```json
[{
  "id": "56afc886-767b-d359-d59e-0da7877166b2",
  "topic": "/SUBSCRIPTIONS/<subscription ID>/RESOURCEGROUPS/<resource group name>/PROVIDERS/MICROSOFT.DEVICES/IOTHUBS/<hub name>",
  "subject": "devices/LogicAppTestDevice",
  "eventType": "Microsoft.Devices.DeviceCreated",
  "eventTime": "2018-01-02T19:17:44.4383997Z",
  "data": {
    "twin": {
      "deviceId": "LogicAppTestDevice",
      "etag": "AAAAAAAAAAE=",
      "deviceEtag": "null",
      "status": "enabled",
      "statusUpdateTime": "0001-01-01T00:00:00",
      "connectionState": "Disconnected",
      "lastActivityTime": "0001-01-01T00:00:00",
      "cloudToDeviceMessageCount": 0,
      "authenticationType": "sas",
      "x509Thumbprint": {
        "primaryThumbprint": null,
        "secondaryThumbprint": null
      },
      "version": 2,
      "properties": {
        "desired": {
          "$metadata": {
            "$lastUpdated": "2018-01-02T19:17:44.4383997Z"
          },
          "$version": 1
        },
        "reported": {
          "$metadata": {
            "$lastUpdated": "2018-01-02T19:17:44.4383997Z"
          },
          "$version": 1
        }
      }
    },
    "hubName": "egtesthub1",
    "deviceId": "LogicAppTestDevice"
  },
  "dataVersion": "1",
  "metadataVersion": "1"
}]
```

### <a name="event-properties"></a>Olay Özellikleri

Tüm olaylar aynı üst düzey veri içerir: 

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| id | dize | Olayın benzersiz tanımlayıcısı. |
| konu başlığı | dize | Olay kaynağı tam kaynak yolu. Bu alan, yazılabilir değil. Event Grid, bu değeri sağlar. |
| konu | dize | Yayımcı tarafından tanımlanan olay konu yolu. |
| olay türü | dize | Bu olay kaynağı için kayıtlı olay türlerinden biri. |
| eventTime | dize | Olayın oluşturulduğu zamandan, sağlayıcının UTC saatini temel alan. |
| veriler | object | IOT Hub olay verileri.  |
| dataVersion | dize | Veri nesnesinin şema sürümü. Yayımcı, şema sürümü tanımlar. |
| metadataVersion | dize | Olay meta verilerinin şema sürümü. Event Grid, şemanın en üst düzey özellikleri tanımlar. Event Grid, bu değeri sağlar. |

Tüm IOT Hub olaylarını veri nesnesinin aşağıdaki özellikleri içerir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| HubName | dize | IOT cihaz nerede oluşturulduğu veya silindiği Hub adı. |
| deviceId | dize | Cihazın benzersiz tanımlayıcısı. Bu büyük küçük harfe duyarlı dize en fazla 128 karakter uzunluğunda olabilir ve ASCII 7 bit alfasayısal karakterlerin yanı sıra şu özel karakterleri destekler: `- : . + % _ # * ? ! ( ) , = @ ; $ '`. |

Veri nesnesi, içeriğini, her olay yayımcısı için farklıdır. İçin **cihaz bağlı** ve **cihazın bağlantısı** IOT Hub olayları, veri nesnesi, aşağıdaki özellikleri içerir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| Modül kimliği | dize | Modülün benzersiz tanımlayıcısı. Çıkış Modülü cihazlar için yalnızca alandır. Bu büyük küçük harfe duyarlı dize en fazla 128 karakter uzunluğunda olabilir ve ASCII 7 bit alfasayısal karakterlerin yanı sıra şu özel karakterleri destekler: `- : . + % _ # * ? ! ( ) , = @ ; $ '`. |
| deviceConnectionStateEventInfo | object | Cihaz bağlantı durumu olay bilgilerini
| sequenceNumber | dize | Yardımcı olan bağlı cihaz veya cihaz sırasını belirten bir sayı olayları bağlantısı kesildi. En son olay, bir sıra numarası olacaktır, önceki olay daha yüksek. Bu sayı tarafından 1'den fazla değiştirebilir, ancak kesin olarak artmaktadır. Bkz: [sıra numarası kullanmayı](../iot-hub/iot-hub-how-to-order-connection-state-events.md). |

Veri nesnesi, içeriğini, her olay yayımcısı için farklıdır. İçin **cihaz oluşturulan** ve **cihaz silinmiş** IOT Hub olayları, veri nesnesi, aşağıdaki özellikleri içerir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| ikiz | object | Uygulama cihaz meta verilerini, bulut represenation olan cihaz ikizi hakkında bilgi sağlar. | 
| cihaz kimliği | dize | Cihaz ikizi benzersiz tanımlayıcısı. | 
| Etag | dize | Cihaz ikizi güncelleştirmeleri tutarlılık sağlamaya yönelik bir doğrulayıcı. Her bir etag cihaz ikizi benzersiz olması garanti edilir. |  
| deviceEtag| dize | Bir cihaz kayıt defterine güncelleştirmelerin tutarlılık sağlamaya yönelik bir doğrulayıcı. Her deviceEtag cihaz kayıt defteri başına benzersiz olması garanti edilir. |
| durum | dize | Olup cihaz ikizinde etkin veya devre dışı. | 
| statusUpdateTime | dize | Son cihaz ikizi durumu ISO8601 zaman damgası güncelleştirin. |
| connectionState | dize | Olup cihaz bağlı bağlantısı kesildi veya. | 
| lastActivityTime | dize | Son Etkinlik ISO8601 zaman damgası. | 
| cloudToDeviceMessageCount | integer | Bu cihaza gönderilen cihaz iletileri buluta sayısı. | 
| authenticationType | dize | Bu cihaz için kullanılan kimlik doğrulaması türü: ya da `SAS`, `SelfSigned`, veya `CertificateAuthority`. |
| X509Thumbprint | dize | Parmak izi, x509 için benzersiz bir değer belirli bir sertifika bir sertifika deposunda bulmak için kullanılan sertifika. Parmak izi SHA1 algoritması kullanılarak dinamik olarak oluşturulur ve fiziksel olarak sertifika mevcut değil. | 
| primaryThumbprint | dize | Birincil parmak izi x509 için sertifika. |
| secondaryThumbprint | dize | İkincil parmak izi x509 için sertifika. | 
| version | integer | Bir her artırılır tamsayı saat cihaz ikizi güncelleştirilir. |
| İstenen | object | Yalnızca uygulama arka ucu tarafından yazılmış ve cihaz tarafından okunur özellikleri kısmı. | 
| bildirilen | object | Yalnızca cihaz tarafından yazılmış ve uygulama arka ucu tarafından okunur özellikleri kısmı. |
| lastUpdated | dize | ISO8601 zaman damgası son cihaz ikizi özelliğinin güncelleştirin. | 

## <a name="next-steps"></a>Sonraki adımlar

* Azure Event grid'e giriş için bkz [Event Grid nedir?](overview.md)
* IOT Hub ve Event Grid birlikte nasıl çalıştığı hakkında bilgi edinmek için [tetikleyici eylemlere Event Grid kullanarak IOT Hub olaylarına tepki](../iot-hub/iot-hub-event-grid.md).