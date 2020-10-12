---
title: Azure IoT Hub sistem durumunu izleyin | Microsoft Docs
description: IoT Hub izlemek ve sorunları hızla tanılamak için Azure Izleyici ve Azure Kaynak Durumu kullanın
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/21/2020
ms.author: robinsh
ms.custom:
- amqp
- 'Role: Cloud Development'
- 'Role: Technical Support'
- devx-track-csharp
ms.openlocfilehash: 100f87b8a13fb424706c3b5ec13268cd3ba42bbe
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "89438415"
---
# <a name="monitor-the-health-of-azure-iot-hub-and-diagnose-problems-quickly"></a>Azure IoT Hub durumunu izleyin ve sorunları hızla tanılayın

Azure IoT Hub uygulayan işletmeler, kaynaklarından güvenilir performans bekler. Operasyonlarınızın yakından bir izlemesini korumanıza yardımcı olmak için IoT Hub [Azure izleyici](../azure-monitor/index.yml) ve [Azure Kaynak durumu](../service-health/resource-health-overview.md)ile tamamen tümleşiktir. Bu iki hizmet, IoT çözümlerinizi sağlıklı bir durumda tutmanız için ihtiyacınız olan verileri size sağlamak üzere çalışır.

Azure Izleyici, tüm Azure hizmetlerinizin tek bir izleme ve günlüğe kaydetme kaynağıdır. Azure Izleyici 'nin oluşturduğu tanılama günlüklerini özel işleme için Azure Izleyici günlüklerine, Event Hubs veya Azure depolama 'ya gönderebilirsiniz. Azure Izleyici 'nin ölçümleri ve Tanılama ayarları, kaynaklarınızın performansına ilişkin görünürlük sağlar. [Azure izleyici](#use-azure-monitor) 'yi IoT Hub 'ınızla nasıl kullanacağınızı öğrenmek için bu makaleyi okumaya devam edin. 

> [!IMPORTANT]
> Azure Izleyici tanılama günlüklerini kullanarak IoT Hub hizmeti tarafından yayılan olayların güvenilir veya sıralı olması garanti edilmez. Bazı olaylar kaybolmuş veya sıra dışında teslim edilebilir. Tanılama günlükleri de gerçek zamanlı olarak değildir ve olayların seçtiğiniz hedefe eklenmesi birkaç dakika sürebilir.

Azure Kaynak Durumu, kaynaklarınızı etkileyen bir Azure sorunu tanılamanıza ve destek almanıza yardımcı olur. Bir Pano, IoT Hub 'larınızın her biri için geçerli ve geçmiş sistem durumu sağlar. IoT Hub 'ınızla [Azure Kaynak durumu kullanmayı](#use-azure-resource-health) öğrenmek için bu makalenin altındaki bölümüne ilerleyin. 

IoT Hub, IoT kaynaklarınızın durumunu anlamak için kullanabileceğiniz kendi ölçümlerini da sağlar. Daha fazla bilgi için bkz. [IoT Hub ölçümleri anlama](iot-hub-metrics.md).

## <a name="use-azure-monitor"></a>Azure İzleyici’yi kullanma

Azure Izleyici, Azure kaynakları için tanılama bilgileri sağlar. Bu, IoT Hub 'ınız içinde gerçekleşen işlemleri izleyebilmeniz anlamına gelir.

Azure Monitor 'un izleyen belirli ölçümler ve olaylar hakkında daha fazla bilgi edinmek için bkz. Azure [izleyici Ile desteklenen ölçümler](../azure-monitor/platform/metrics-supported.md) ve [Azure tanılama günlükleri için desteklenen hizmetler, şemalar ve Kategoriler](../azure-monitor/platform/diagnostic-logs-schema.md).

[!INCLUDE [iot-hub-diagnostics-settings](../../includes/iot-hub-diagnostics-settings.md)]

### <a name="understand-the-logs"></a>Günlükleri anlama

Azure Izleyici, IoT Hub oluşan farklı işlemleri izler. Her kategorinin, bu kategorideki olayların nasıl raporlandığını tanımlayan bir şeması vardır.

#### <a name="connections"></a>Bağlantılar

Bağlantılar kategorisi, cihaz bağlantısını ve bir IoT Hub 'ından olayları ve bağlantı kesmeyi izler. Bu kategori, cihazların bağlantısını kaybettiyseniz yetkisiz bağlantı girişimlerini belirlemek ve veya uyarma için faydalıdır.

> [!NOTE]
> Cihazların güvenilir bağlantı durumu için [cihaz sinyalini](iot-hub-devguide-identity-registry.md#device-heartbeat)denetleyin.

```json
{
   "records":
   [
        {
            "time": " UTC timestamp",
            "resourceId": "Resource Id",
            "operationName": "deviceConnect",
            "category": "Connections",
            "level": "Information",
            "properties": "{\"deviceId\":\"<deviceId>\",\"sdkVersion\":\"<sdkVersion>\",\"protocol\":\"<protocol>\",\"authType\":\"{\\\"scope\\\":\\\"device\\\",\\\"type\\\":\\\"sas\\\",\\\"issuer\\\":\\\"iothub\\\",\\\"acceptingIpFilterRule\\\":null}\",\"maskedIpAddress\":\"<maskedIpAddress>\"}",
            "location": "Resource location"
        }
    ]
}
```

#### <a name="cloud-to-device-commands"></a>Buluttan cihaza komutlar

Buluttan cihaza komutlar kategorisi, IoT Hub 'ında oluşan ve buluttan cihaza ileti işlem hattı ile ilgili hataları izler. Bu kategori, şuradan oluşan hataları içerir:

* Buluttan cihaza iletileri gönderme (yetkisiz gönderen hatalar gibi),
* Buluttan cihaza iletileri alma (örneğin, teslim sayısı aşıldı hataları) ve
* Buluttan cihaza ileti geri bildirimi alma (geri bildirim süre sonu hataları gibi).

Bu kategori, buluttan cihaza ileti başarıyla teslim edildiğinde, ancak cihaz tarafından yanlış işlenirse hataları yakalamaz.

```json
{
    "records":
    [
        {
            "time": " UTC timestamp",
            "resourceId": "Resource Id",
            "operationName": "messageExpired",
            "category": "C2DCommands",
            "level": "Error",
            "resultType": "Event status",
            "resultDescription": "MessageDescription",
            "properties": "{\"deviceId\":\"<deviceId>\",\"messageId\":\"<messageId>\",\"messageSizeInBytes\":\"<messageSize>\",\"protocol\":\"Amqp\",\"deliveryAcknowledgement\":\"<None, NegativeOnly, PositiveOnly, Full>\",\"deliveryCount\":\"0\",\"expiryTime\":\"<timestamp>\",\"timeInSystem\":\"<timeInSystem>\",\"ttl\":<ttl>, \"EventProcessedUtcTime\":\"<UTC timestamp>\",\"EventEnqueuedUtcTime\":\"<UTC timestamp>\", \"maskedIpAddress\": \"<maskedIpAddress>\", \"statusCode\": \"4XX\"}",
            "location": "Resource location"
        }
    ]
}
```

#### <a name="device-identity-operations"></a>Cihaz kimliği işlemleri

Cihaz kimliği işlemler kategorisi, IoT Hub 'ının kimlik kayıt defterinde bir girişi oluşturmaya, güncelleştirmeye veya silmeye çalıştığınızda oluşan hataları izler. Bu kategoriyi izlemek, senaryoları sağlamak için faydalıdır.

```json
{
    "records":
    [
        {
            "time": "UTC timestamp",
            "resourceId": "Resource Id",
            "operationName": "get",
            "category": "DeviceIdentityOperations",
            "level": "Error",
            "resultType": "Event status",
            "resultDescription": "MessageDescription",
            "properties": "{\"maskedIpAddress\":\"<maskedIpAddress>\",\"deviceId\":\"<deviceId>\", \"statusCode\":\"4XX\"}",
            "location": "Resource location"
        }
    ]
}
```

#### <a name="routes"></a>Yollar

[İleti yönlendirme](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-d2c) kategorisi, IoT Hub tarafından algılanan ileti yolu değerlendirmesi ve uç nokta durumu sırasında oluşan hataları izler. Bu kategori, şunlar gibi olayları içerir:

* Bir kural "tanımsız" olarak değerlendirilir,
* IoT Hub bir uç noktayı ölü olarak işaretler veya
* Bir uç noktadan alınan hatalar.

Bu kategori, "cihaz telemetrisi" kategorisi altında bildirilen iletiler hakkındaki belirli hataları (cihaz azaltma hataları gibi) içermez.

```json
{
    "records":
    [
        {
            "time":"2019-12-12T03:25:14Z",
            "resourceId":"/SUBSCRIPTIONS/91R34780-3DEC-123A-BE2A-213B5500DFF0/RESOURCEGROUPS/ANON-TEST/PROVIDERS/MICROSOFT.DEVICES/IOTHUBS/ANONHUB1",
            "operationName":"endpointUnhealthy",
            "category":"Routes",
            "level":"Error",
            "resultType":"403004",
            "resultDescription":"DeviceMaximumQueueDepthExceeded",
            "properties":"{\"deviceId\":null,\"endpointName\":\"anon-sb-1\",\"messageId\":null,\"details\":\"DeviceMaximumQueueDepthExceeded\",\"routeName\":null,\"statusCode\":\"403\"}",
            "location":"westus"
        }
    ]
}
```

Yönlendirme tanılama günlükleri hakkında daha fazla ayrıntı aşağıda verilmiştir:

* [Yönlendirme tanılama günlüğü hata kodlarının listesi](troubleshoot-message-routing.md#diagnostics-error-codes)
* [Yönlendirme tanılama günlükleri operationNames listesi](troubleshoot-message-routing.md#diagnostics-operation-names)

#### <a name="device-telemetry"></a>Cihaz telemetrisi

Cihaz telemetri kategorisi, IoT Hub 'ında oluşan hataları izler ve telemetri işlem hattı ile ilgilidir. Bu kategori telemetri olayları gönderilirken (daraltma gibi) ve telemetri olaylarının (yetkisiz okuyucu gibi) alınması sırasında oluşan hataları içerir. Bu kategori, cihazın kendisinde çalışan kodun neden olduğu hataları yakalayamaz.

```json
{
    "records":
    [
        {
            "time": "UTC timestamp",
            "resourceId": "Resource Id",
            "operationName": "ingress",
            "category": "DeviceTelemetry",
            "level": "Error",
            "resultType": "Event status",
            "resultDescription": "MessageDescription",
            "properties": "{\"deviceId\":\"<deviceId>\",\"batching\":\"0\",\"messageSizeInBytes\":\"<messageSizeInBytes>\",\"EventProcessedUtcTime\":\"<UTC timestamp>\",\"EventEnqueuedUtcTime\":\"<UTC timestamp>\",\"partitionId\":\"1\"}", 
            "location": "Resource location"
        }
    ]
}
```

#### <a name="file-upload-operations"></a>Dosya karşıya yükleme işlemleri

Karşıya dosya yükleme kategorisi, IoT Hub 'ında oluşan ve karşıya dosya yükleme işlevselliğiyle ilgili hataları izler. Bu kategori şunları içerir:

* Bir cihazın, tamamlanmış bir karşıya yükleme merkezine bildirim göndermeden önce ne zaman dolacağını belirten SAS URI 'SI ile oluşan hatalar.

* Cihaz tarafından raporlanan karşıya yüklemeler başarısız oldu.

* IoT Hub bildirim iletisi oluşturma sırasında depolamada bir dosya bulunamadığında oluşan hatalar.

Bu kategori, cihaz bir dosyayı depoya yüklerken doğrudan oluşan hataları yakalayamaz.

```json
{
    "records":
    [
        {
            "time": "UTC timestamp",
            "resourceId": "Resource Id",
            "operationName": "ingress",
            "category": "FileUploadOperations",
            "level": "Error",
            "resultType": "Event status",
            "resultDescription": "MessageDescription",
            "durationMs": "1",
            "properties": "{\"deviceId\":\"<deviceId>\",\"protocol\":\"<protocol>\",\"authType\":\"{\\\"scope\\\":\\\"device\\\",\\\"type\\\":\\\"sas\\\",\\\"issuer\\\":\\\"iothub\\\",\\\"acceptingIpFilterRule\\\":null}\",\"blobUri\":\"http//bloburi.com\"}",
            "location": "Resource location"
        }
    ]
}
```

#### <a name="cloud-to-device-twin-operations"></a>Buluttan cihaza ikizi işlemleri

Buluttan cihaza ikizi Operations kategorisi, cihaz ikizlerinin hizmet tarafından başlatılan olaylarını izler. Bu işlemler, Get ikizi, etiketleri güncelleştirebilir veya değiştirebilir ve istenen özellikleri güncelleştirebilir veya değiştirebilir.

```json
{
    "records":
    [
        {
            "time": "UTC timestamp",
            "resourceId": "Resource Id",
            "operationName": "read",
            "category": "C2DTwinOperations",
            "level": "Information",
            "durationMs": "1",
            "properties": "{\"deviceId\":\"<deviceId>\",\"sdkVersion\":\"<sdkVersion>\",\"messageSize\":\"<messageSize>\"}",
            "location": "Resource location"
        }
    ]
}
```

#### <a name="device-to-cloud-twin-operations"></a>Cihazdan buluta ikizi işlemleri

Cihazdan buluta ikizi Operations kategorisi cihaz ikizlerinin cihaz tarafından başlatılan olaylarını izler. Bu işlemler Get ikizi, bildirilen özellikleri güncelleştirebilir ve istenen özelliklere abone olabilir.

```json
{
    "records":
    [
        {
            "time": "UTC timestamp",
            "resourceId": "Resource Id",
            "operationName": "update",
            "category": "D2CTwinOperations",
            "level": "Information",
            "durationMs": "1",
            "properties": "{\"deviceId\":\"<deviceId>\",\"protocol\":\"<protocol>\",\"authenticationType\":\"{\\\"scope\\\":\\\"device\\\",\\\"type\\\":\\\"sas\\\",\\\"issuer\\\":\\\"iothub\\\",\\\"acceptingIpFilterRule\\\":null}\"}",
            "location": "Resource location"
        }
    ]
}
```

#### <a name="twin-queries"></a>İkizi sorguları

İkizi, bulutta başlatılan cihaz TWINS 'e yönelik sorgu istekleri hakkında rapor raporlar.

```json
{
    "records":
    [
        {
            "time": "UTC timestamp",
            "resourceId": "Resource Id",
            "operationName": "query",
            "category": "TwinQueries",
            "level": "Information",
            "durationMs": "1",
            "properties": "{\"query\":\"<twin query>\",\"sdkVersion\":\"<sdkVersion>\",\"messageSize\":\"<messageSize>\",\"pageSize\":\"<pageSize>\", \"continuation\":\"<true, false>\", \"resultSize\":\"<resultSize>\"}",
            "location": "Resource location"
        }
    ]
}
```

#### <a name="jobs-operations"></a>İş işlemleri

İş istekleri kategorisi, cihaz iklerini güncelleştirmek veya birden çok cihazda doğrudan yöntemleri çağırmak için iş istekleri hakkında rapor sağlar. Bu istekler bulutta başlatılır.

```json
{
    "records":
    [
        {
            "time": "UTC timestamp",
            "resourceId": "Resource Id",
            "operationName": "jobCompleted",
            "category": "JobsOperations",
            "level": "Information",
            "durationMs": "1",
            "properties": "{\"jobId\":\"<jobId>\", \"sdkVersion\": \"<sdkVersion>\",\"messageSize\": <messageSize>,\"filter\":\"DeviceId IN ['1414ded9-b445-414d-89b9-e48e8c6285d5']\",\"startTimeUtc\":\"Wednesday, September 13, 2017\",\"duration\":\"0\"}",
            "location": "Resource location"
        }
    ]
}
```

#### <a name="direct-methods"></a>Doğrudan Yöntemler

Doğrudan Yöntemler kategorisi, bireysel cihazlara gönderilen istek-yanıt etkileşimlerini izler. Bu istekler bulutta başlatılır.

```json
{
    "records":
    [
        {
            "time": "UTC timestamp",
            "resourceId": "Resource Id",
            "operationName": "send",
            "category": "DirectMethods",
            "level": "Information",
            "durationMs": "1",
            "properties": "{\"deviceId\":<messageSize>, \"RequestSize\": 1, \"ResponseSize\": 1, \"sdkVersion\": \"2017-07-11\"}",
            "location": "Resource location"
        }
    ]
}
```

#### <a name="distributed-tracing-preview"></a>Dağıtılmış Izleme (Önizleme)

Dağıtılmış izleme kategorisi, izleme bağlam üstbilgisini taşıyan iletiler için bağıntı kimliklerini izler. Bu günlükleri tam olarak etkinleştirmek için, istemci tarafı kodu, [IoT Hub dağıtılmış izleme (Önizleme) ile uçtan uca olan IoT uygulamalarını çözümleyin ve tanılayın](iot-hub-distributed-tracing.md).

`correlationId` [W3C Trace bağlam](https://github.com/w3c/trace-context) teklifine, ve ' a ve ' a sahip olduğunu unutmayın `trace-id` `span-id` .

##### <a name="iot-hub-d2c-device-to-cloud-logs"></a>IoT Hub D2C (cihazdan buluta) günlükleri

IoT Hub, geçerli izleme özellikleri içeren bir ileti IoT Hub geldiğinde bu günlüğü kaydeder.

```json
{
    "records":
    [
        {
            "time": "UTC timestamp",
            "resourceId": "Resource Id",
            "operationName": "DiagnosticIoTHubD2C",
            "category": "DistributedTracing",
            "correlationId": "00-8cd869a412459a25f5b4f31311223344-0144d2590aacd909-01",
            "level": "Information",
            "resultType": "Success",
            "resultDescription":"Receive message success",
            "durationMs": "",
            "properties": "{\"messageSize\": 1, \"deviceId\":\"<deviceId>\", \"callerLocalTimeUtc\": : \"2017-02-22T03:27:28.633Z\", \"calleeLocalTimeUtc\": \"2017-02-22T03:27:28.687Z\"}",
            "location": "Resource location"
        }
    ]
}
```

Burada, `durationMs` IoT Hub saatinin cihaz saatiyle eşitlenmiş olmaması ve bu nedenle bir süre hesaplamasının yanıltıcı olması için hesaplanmaz. `properties`Cihazdan buluta gecikme süresini yakalamak için bölümündeki zaman damgalarını kullanarak mantık yazma öneririz.

| Özellik | Tür | Açıklama |
|--------------------|-----------------------------------------------|------------------------------------------------------------------------------------------------|
| **messageSize** | Tamsayı | Cihazdan buluta iletinin bayt cinsinden boyutu |
| **deviceId** | ASCII 7 bit alfasayısal karakter dizesi | Cihazın kimliği |
| **callerLocalTimeUtc** | UTC zaman damgası | Cihazın yerel saat tarafından raporlanan Oluşturulma saati |
| **calleeLocalTimeUtc** | UTC zaman damgası | IoT Hub ağ geçidinde ileti alma zamanı IoT Hub hizmet tarafı saatine göre bildirildi |

##### <a name="iot-hub-ingress-logs"></a>Giriş günlüklerini IoT Hub

IoT Hub, geçerli izleme özellikleri içeren ileti iç veya yerleşik Olay Hub 'ına yazdığında bu günlüğü kaydeder.

```json
{
    "records":
    [
        {
            "time": "UTC timestamp",
            "resourceId": "Resource Id",
            "operationName": "DiagnosticIoTHubIngress",
            "category": "DistributedTracing",
            "correlationId": "00-8cd869a412459a25f5b4f31311223344-349810a9bbd28730-01",
            "level": "Information",
            "resultType": "Success",
            "resultDescription":"Ingress message success",
            "durationMs": "10",
            "properties": "{\"isRoutingEnabled\": \"true\", \"parentSpanId\":\"0144d2590aacd909\"}",
            "location": "Resource location"
        }
    ]
}
```

`properties`Bölümünde, bu günlük ileti girişi hakkında ek bilgiler içerir.

| Özellik | Tür | Açıklama |
|--------------------|-----------------------------------------------|------------------------------------------------------------------------------------------------|
| **isRoutingEnabled** | Dize | True veya false, IoT Hub ileti yönlendirmenin etkin olup olmadığını gösterir |
| **Parentspanıd** | Dize | Bu durumda D2C ileti izlemesi olacak üst iletinin [yayılma kimliği](https://w3c.github.io/trace-context/#parent-id) |

##### <a name="iot-hub-egress-logs"></a>Çıkış günlüklerini IoT Hub

IoT Hub [yönlendirme](iot-hub-devguide-messages-d2c.md) etkinken ve ileti bir [uç noktaya](iot-hub-devguide-endpoints.md)yazıldığında bu günlüğü kaydeder. Yönlendirme etkinleştirilmemişse IoT Hub bu günlüğü kaydetmez.

```json
{
    "records":
    [
        {
            "time": "UTC timestamp",
            "resourceId": "Resource Id",
            "operationName": "DiagnosticIoTHubEgress",
            "category": "DistributedTracing",
            "correlationId": "00-8cd869a412459a25f5b4f31311223344-98ac3578922acd26-01",
            "level": "Information",
            "resultType": "Success",
            "resultDescription":"Egress message success",
            "durationMs": "10",
            "properties": "{\"endpointType\": \"EventHub\", \"endpointName\": \"myEventHub\", \"parentSpanId\":\"349810a9bbd28730\"}",
            "location": "Resource location"
        }
    ]
}
```

`properties`Bölümünde, bu günlük ileti girişi hakkında ek bilgiler içerir.

| Özellik | Tür | Açıklama |
|--------------------|-----------------------------------------------|------------------------------------------------------------------------------------------------|
| **Uçnoktaadı** | Dize | Yönlendirme uç noktasının adı |
| **endpointType** | Dize | Yönlendirme uç noktasının türü |
| **Parentspanıd** | Dize | Bu durumda IoT Hub giriş iletisi izlemesi olacak üst iletinin [yayılma kimliği](https://w3c.github.io/trace-context/#parent-id) |

#### <a name="configurations"></a>Yapılandırmalar

IoT Hub yapılandırma günlükleri otomatik cihaz yönetimi özellik kümesi için olayları ve hataları izler.

```json
{
    "records":
    [
         {
             "time": "2019-09-24T17:21:52Z",
             "resourceId": "Resource Id",
             "operationName": "ReadManyConfigurations",
             "category": "Configurations",
             "resultType": "",
             "resultDescription": "",
             "level": "Information",
             "durationMs": "17",
             "properties": "{\"configurationId\":\"\",\"sdkVersion\":\"2018-06-30\",\"messageSize\":\"0\",\"statusCode\":null}",
             "location": "southcentralus"
         }
    ]
}
```

### <a name="device-streams-preview"></a>Cihaz akışları (Önizleme)

Cihaz akışları kategorisi, bireysel cihazlara gönderilen istek-yanıt etkileşimlerini izler.

```json
{
    "records":
    [
         {
             "time": "2019-09-19T11:12:04Z",
             "resourceId": "Resource Id",
             "operationName": "invoke",
             "category": "DeviceStreams",
             "resultType": "",
             "resultDescription": "",    
             "level": "Information",
             "durationMs": "74",
             "properties": "{\"deviceId\":\"myDevice\",\"moduleId\":\"myModule\",\"sdkVersion\":\"2019-05-01-preview\",\"requestSize\":\"3\",\"responseSize\":\"5\",\"statusCode\":null,\"requestName\":\"myRequest\",\"direction\":\"c2d\"}",
             "location": "Central US"
         }
    ]
}
```

### <a name="sdk-version"></a>SDK sürümü

Bazı işlemler nesnesine bir `sdkVersion` özellik döndürür `properties` . Bu işlemler için, bir cihaz veya arka uç uygulaması Azure IoT SDK 'Lardan birini kullanırken, bu özellik kullanılan SDK, SDK sürümü ve SDK 'nın çalıştığı platform hakkındaki bilgileri içerir. Aşağıdaki örnek, `sdkVersion` `deviceConnect` Node.js cihaz SDK 'sı kullanılırken bir işlem için yayılan özelliği gösterir: `"azure-iot-device/1.17.1 (node v10.16.0; Windows_NT 10.0.18363; x64)"` . .NET (C#) SDK 'Sı için yayılan değere bir örnek aşağıda verilmiştir: `".NET/1.21.2 (.NET Framework 4.8.4200.0; Microsoft Windows 10.0.17763 WindowsProduct:0x00000004; X86)"` .

Aşağıdaki tabloda farklı Azure IoT SDK 'Ları için kullanılan SDK adı gösterilmektedir:

| SdkVersion özelliğinde SDK adı | Dil |
|----------|----------|
| .NET | .NET (C#) |
| Microsoft. Azure. Devices | .NET (C#) hizmeti SDK 'Sı |
| Microsoft. Azure. Devices. Client | .NET (C#) cihaz SDK 'Sı |
| ıothubclient | C veya Python v1 (kullanım dışı) cihaz SDK 'Sı |
| iothubserviceclient | C veya Python v1 (kullanım dışı) hizmet SDK 'Sı |
| Azure-IoT-Device-ıothub-Kopyala | Python cihaz SDK 'Sı |
| azure-iot-device | Node.js cihaz SDK 'Sı |
| azure-iothub | Node.js hizmeti SDK 'Sı |
| com. Microsoft. Azure. ıothub-Java-Client | Java cihaz SDK 'Sı |
| com. Microsoft. Azure. ıothub. Service. SDK | Java hizmeti SDK 'Sı |
| com. Microsoft. Azure. SDK. IoT. IoT-Device-Client | Java cihaz SDK 'Sı |
| com. Microsoft. Azure. SDK. IoT. IoT-Service-Client | Java hizmeti SDK 'Sı |
| C | Gömülü C |
| C + (Osbasitleştirilmiş = Azure RTOS) | Azure RTOS |

Tanılama günlüklerine yönelik sorgular gerçekleştirdiğinizde SDK sürüm özelliğini ayıklayabilirsiniz. Aşağıdaki sorgu, bağlantı olayları tarafından döndürülen özelliklerden SDK sürüm özelliğini (ve cihaz KIMLIĞI) ayıklar. Bu iki özellik, olay saati ve cihazın bağlandığı IoT Hub 'ın kaynak KIMLIĞI ile birlikte sonuçlara yazılır.

```kusto
// SDK version of devices
// List of devices and their SDK versions that connect to IoT Hub
AzureDiagnostics
| where ResourceProvider == "MICROSOFT.DEVICES" and ResourceType == "IOTHUBS"
| where Category == "Connections"
| extend parsed_json = parse_json(properties_s) 
| extend SDKVersion = tostring(parsed_json.sdkVersion) , DeviceId = tostring(parsed_json.deviceId)
| distinct DeviceId, SDKVersion, TimeGenerated, _ResourceId
```

### <a name="read-logs-from-azure-event-hubs"></a>Azure Event Hubs günlüklerini okuyun

Tanılama ayarları aracılığıyla olay günlüğü ayarladıktan sonra, içerdikleri bilgilere göre işlem yapabilmeniz için günlükleri okuyan uygulamalar oluşturabilirsiniz. Bu örnek kod, bir olay hub 'ından günlükleri alır:

```csharp
class Program
{ 
    static string connectionString = "{your AMS eventhub endpoint connection string}";
    static string monitoringEndpointName = "{your AMS event hub endpoint name}";
    static EventHubClient eventHubClient;
    //This is the Diagnostic Settings schema
    class AzureMonitorDiagnosticLog
    {
        string time { get; set; }
        string resourceId { get; set; }
        string operationName { get; set; }
        string category { get; set; }
        string level { get; set; }
        string resultType { get; set; }
        string resultDescription { get; set; }
        string durationMs { get; set; }
        string callerIpAddress { get; set; }
        string correlationId { get; set; }
        string identity { get; set; }
        string location { get; set; }
        Dictionary<string, string> properties { get; set; }
    };

    static void Main(string[] args)
    {
        Console.WriteLine("Monitoring. Press Enter key to exit.\n");
        eventHubClient = EventHubClient.CreateFromConnectionString(connectionString, monitoringEndpointName);
        var d2cPartitions = eventHubClient.GetRuntimeInformationAsync().PartitionIds;
        CancellationTokenSource cts = new CancellationTokenSource();
        var tasks = new List<Task>();
        foreach (string partition in d2cPartitions)
        {
            tasks.Add(ReceiveMessagesFromDeviceAsync(partition, cts.Token));
        }
        Console.ReadLine();
        Console.WriteLine("Exiting...");
        cts.Cancel();
        Task.WaitAll(tasks.ToArray());
    }

    private static async Task ReceiveMessagesFromDeviceAsync(string partition, CancellationToken ct)
    {
        var eventHubReceiver = eventHubClient.GetDefaultConsumerGroup().CreateReceiver(partition, DateTime.UtcNow);
        while (true)
        {
            if (ct.IsCancellationRequested)
            {
                await eventHubReceiver.CloseAsync();
                break;
            }
            EventData eventData = await eventHubReceiver.ReceiveAsync(new TimeSpan(0,0,10));
            if (eventData != null)
            {
                string data = Encoding.UTF8.GetString(eventData.GetBytes());
                Console.WriteLine("Message received. Partition: {0} Data: '{1}'", partition, data);
                var deserializer = new JavaScriptSerializer();
                //deserialize json data to azure monitor object
                AzureMonitorDiagnosticLog message = new JavaScriptSerializer().Deserialize<AzureMonitorDiagnosticLog>(result);
            }
        }
    }
}
```

## <a name="use-azure-resource-health"></a>Azure Kaynak Durumu kullan

IoT Hub 'ınızın çalışır duruma yapılıp yapılmayacağını izlemek için Azure Kaynak Durumu kullanın. Ayrıca, bir bölgesel kesintide IoT Hub 'ınızın sistem durumunu etkileyip etkilemediğini de öğrenebilirsiniz. Azure IoT Hub sistem durumu hakkında belirli ayrıntıları anlamak için [Azure Izleyici kullanmanızı](#use-azure-monitor)öneririz.

Azure IoT Hub, bölgesel düzeyde sistem durumunu gösterir. Bir bölgesel kesinti IoT Hub 'ınızı etkileirse sistem durumu **bilinmiyor**olarak görünür. Daha fazla bilgi edinmek için bkz. [Azure Kaynak durumu 'Nda kaynak türleri ve durum denetimleri](../service-health/resource-health-checks-resource-types.md).

IoT Hub 'larınızın sistem durumunu denetlemek için şu adımları izleyin:

1. [Azure portalında](https://portal.azure.com) oturum açın.

2. **Hizmet sistem durumu**  >  **kaynak sistem durumu**' na gidin.

3. Aşağı açılan kutularda, aboneliğinizi seçin ve kaynak türü olarak **IoT Hub** ' yi seçin.

Sistem durumu verilerini yorumlama hakkında daha fazla bilgi edinmek için bkz. [Azure Kaynak durumu genel bakış](../service-health/resource-health-overview.md).

## <a name="next-steps"></a>Sonraki adımlar

* [IoT Hub ölçümlerini anlayın](iot-hub-metrics.md)
* [IoT Hub 'ınızı ve posta kutunuzu bağlama Azure Logic Apps IoT uzaktan izleme ve bildirimler](iot-hub-monitoring-notifications-with-azure-logic-apps.md)
