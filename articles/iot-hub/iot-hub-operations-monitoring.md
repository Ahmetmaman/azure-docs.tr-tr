---
title: Azure IOT Hub işlemlerini izleme | Microsoft Docs
description: Azure IOT Hub işlemlerini gerçek zamanlı IOT hub'ınızdaki işlemlerin durumunu izlemek için izleme kullanma
author: nberdy
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 10/10/2017
ms.author: nberdy
ms.openlocfilehash: 0f4d5105b7266ba24fc5efa9af887b4458c05d5e
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2018
ms.locfileid: "39186205"
---
# <a name="iot-hub-operations-monitoring"></a>IOT Hub işlemlerini izleme

IOT Hub işlemlerini izleme, gerçek zamanlı IOT hub'ınızdaki işlemlerin durumunu izlemenize olanak sağlar. IOT Hub olaylarını birkaç işlem kategorisi izler. Olay işleme için IOT hub'ınızın bir uç nokta için bir veya daha fazla kategorilerden göndermeye içine seçebilirsiniz. Hatalar için verileri izlemek veya veri modellerini daha karmaşık bir işlem ayarlayın.

>[!NOTE]
>IOT Hub işlemlerini izleme kullanım dışıdır ve 10 Mart 2019 üzerinde IOT Hub'ından kaldırılacak. İşlemleri ve IOT Hub'ın sistem durumu izleme için bkz: [Azure IOT Hub durumunu izleyin ve sorunları hızla tanılayın][lnk-monitor]. Kullanımdan kaldırma zaman çizelgesini hakkında daha fazla bilgi için bkz: [Azure IOT çözümlerinizi Azure İzleyici ve Azure kaynak durumu izleme][lnk-blog-announcement].

IOT hub'ı altı olayların kategorilerini izler:

* Cihaz kimlik işlemleri
* Cihaz telemetrisi
* Bulut-cihaz iletilerini
* Bağlantılar
* Dosya yüklemeleri
* İleti yönlendirme

> [!IMPORTANT]
> IOT Hub işlemlerini izleme güvenilir ya da sıralı olay teslimini garanti etmez. IOT hub'ı temel altyapıyı bağlı olarak bazı olaylar kaybolabilir veya düzensiz teslim. Başarısız bağlantı denemesi veya belirli cihazlar için yüksek frekanslı bağlantı kesilmesi gibi hata sinyalleri göre uyarılar oluşturmak için izleme işlemleri kullanın. Cihaz durumu için tutarlı bir deposu oluşturmak için olay izleme işlemleri dayanarak doğrulamamalısınız, örneğin izleme bir depolama bağlı veya bir cihaz durumu bağlantı kesildi. 

## <a name="how-to-enable-operations-monitoring"></a>İşlemleri izleme olanağı tanıma

1. IOT hub oluşturun. IOT hub'ı oluşturma hakkında yönergeler bulabilirsiniz [Başlarken] [ lnk-get-started] Kılavuzu.

1. IOT hub'ın dikey penceresini açın. Burada, tıklayın **işlem izleme**.

    ![Yapılandırma portalında izleme erişim işlemleri][1]

1. İzleyin ve ardından istediğiniz izleme kategorileri seçin **Kaydet**. Olayları listelenen Event Hub ile uyumlu uç noktasından okumak için kullanılabilir **izleme ayarlarını**. IOT Hub uç nokta adı verilen `messages/operationsmonitoringevents`.

    ![IOT hub'ınızda izleme işlemlerini yapılandırma][2]

> [!NOTE]
> Seçme **ayrıntılı** için izleme **bağlantıları** kategori ek tanılama iletileri oluşturmak için IOT Hub neden olur. Diğer tüm kategorileri için **ayrıntılı** değişiklikleri IOT hub'ı bilgi miktarını ayarlama her hata iletisi içerir.

## <a name="event-categories-and-how-to-use-them"></a>Olay kategorisi ve bunları kullanma

Her işlem kategorisi parçaları izleme olayları bu kategorideki nasıl yapılandırılmıştır tanımlayan bir şema farklı türde bir IOT hub'ı ve her izleme kategorisi etkileşimi sahip.

### <a name="device-identity-operations"></a>Cihaz kimlik işlemleri

Cihaz kimlik işlem kategorisi oluşturmak, güncelleştirmek veya IOT hub'ınızın kimlik kayıt defterinde bir girdiyi silmek açmaya çalıştığında oluşan hatalar izler. Bu kategori izleme senaryoları sağlamak için kullanışlıdır.

```json
{
    "time": "UTC timestamp",
    "operationName": "create",
    "category": "DeviceIdentityOperations",
    "level": "Error",
    "statusCode": 4XX,
    "statusDescription": "MessageDescription",
    "deviceId": "device-ID",
    "durationMs": 1234,
    "userAgent": "userAgent",
    "sharedAccessPolicy": "accessPolicy"
}
```

### <a name="device-telemetry"></a>Cihaz telemetrisi

Cihaz telemetrisi kategorisi, IOT hub ve telemetri ardışık düzene ilgili hataları izler. Bu kategori (azaltma gibi) telemetri olayları gönderirken oluşan hataları içerir ve telemetri olaylar (örneğin, yetkisiz okuyucusu) alma. Bu kategori, cihaz üzerinde çalışan kod tarafından neden olduğu hata yakalayamaz.

```json
{
    "messageSizeInBytes": 1234,
    "batching": 0,
    "protocol": "Amqp",
    "authType": "{\"scope\":\"device\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
    "time": "UTC timestamp",
    "operationName": "ingress",
    "category": "DeviceTelemetry",
    "level": "Error",
    "statusCode": 4XX,
    "statusType": 4XX001,
    "statusDescription": "MessageDescription",
    "deviceId": "device-ID",
    "EventProcessedUtcTime": "UTC timestamp",
    "PartitionId": 1,
    "EventEnqueuedUtcTime": "UTC timestamp"
}
```

### <a name="cloud-to-device-commands"></a>Bulut-cihaz komutları

Bulut-cihaz komutlarını kategorisi, IOT hub ve bulut-cihaz ileti işlem hattına ilgili hataları izler. Bu kategori, (örneğin, yetkisiz gönderen) bulut buluttan cihaza iletileri gönderme (örneğin, teslimat sayısı aşıldı) bulut-cihaz iletilerini alma ve (geri bildirim süresi gibi) bulut-cihaz ileti geri bildirim alan olduğunda oluşan hataları içerir. Bu kategori, bulut buluttan cihaza iletinin başarıyla teslim edildi, yanlış bir bulut-cihaz iletiyi işleyen bir CİHAZDAN hataları yakalamaz.

```json
{
    "messageSizeInBytes": 1234,
    "authType": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
    "deliveryAcknowledgement": 0,
    "protocol": "Amqp",
    "time": " UTC timestamp",
    "operationName": "ingress",
    "category": "C2DCommands",
    "level": "Error",
    "statusCode": 4XX,
    "statusType": 4XX001,
    "statusDescription": "MessageDescription",
    "deviceId": "device-ID",
    "EventProcessedUtcTime": "UTC timestamp",
    "PartitionId": 1,
    "EventEnqueuedUtcTime": "UTC timestamp"
}
```

### <a name="connections"></a>Bağlantılar

Bağlantıları kategorisi cihazları bağlayın veya bir IOT hub'ından kesin oluşan hataları izler. Bu kategori izleme, yetkisiz bağlantı girişimleri tanımlama ve zamanlar zayıf bağlantıya alanlarında cihazlar için bir bağlantı kesildiğinde izlemek için yararlıdır.

```json
{
    "durationMs": 1234,
    "authType": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
    "protocol": "Amqp",
    "time": " UTC timestamp",
    "operationName": "deviceConnect",
    "category": "Connections",
    "level": "Error",
    "statusCode": 4XX,
    "statusType": 4XX001,
    "statusDescription": "MessageDescription",
    "deviceId": "device-ID"
}
```

### <a name="file-uploads"></a>Dosya yüklemeleri

Dosya karşıya yükleme kategorisi, IOT hub ve dosya karşıya yükleme işlevselliği ile ilgili hataları izler. Bu kategori içerir:

* Ne zaman süresi dolmadan önce tamamlanan bir karşıya yükleme hub'a bir cihaz bildirir gibi SAS URI'si ile oluşan hatalar.
* Cihaz tarafından bildirilen karşıya yükleme başarısız oldu.
* Bir dosya depolama alanında, IOT hub'ı bildirim iletisi oluşturulurken bulunmadığında, oluşan hataları.

Bu kategori, cihazın depolama için bir dosya yüklenirken doğrudan ortaya çıkan hataları yakalayamaz.

```json
{
    "authType": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
    "protocol": "HTTP",
    "time": " UTC timestamp",
    "operationName": "ingress",
    "category": "fileUpload",
    "level": "Error",
    "statusCode": 4XX,
    "statusType": 4XX001,
    "statusDescription": "MessageDescription",
    "deviceId": "device-ID",
    "blobUri": "http//bloburi.com",
    "durationMs": 1234
}
```

### <a name="message-routing"></a>İleti yönlendirme

İleti yönlendirme kategorisi ileti yönlendirme değerlendirme ve IOT Hub tarafından algılanan uç nokta sistem durumu sırasında oluşan hataları izler. Bu kategori, bir kural "undefined" olarak değerlendirir, ne zaman IOT hub'ı bir uç nokta atılacak ve bir uç noktasından alınan hatalar olarak işaretler gibi olayları içerir. Bu kategori, "cihaz telemetrisi" kategorisi altında bildirilen iletilerini kendileri (örneğin, cihaz) azaltma hataları, ilgili belirli hataları içermez.

```json
{
    "messageSizeInBytes": 1234,
    "time": "UTC timestamp",
    "operationName": "ingress",
    "category": "routes",
    "level": "Error",
    "deviceId": "device-ID",
    "messageId": "ID of message",
    "routeName": "myroute",
    "endpointName": "myendpoint",
    "details": "ExternalEndpointDisabled"
}
```

## <a name="view-events"></a>Etkinlikleri görüntüleme

Kullanabileceğiniz *iothub-explorer* IOT hub'ınızı izleme olayları oluşturduğunu hızlı bir şekilde test etmek için aracı. Aracı yüklemek için yönergeleri görmek [iothub-explorer] [ lnk-iothub-explorer] GitHub deposu.

1. Emin **bağlantıları** izleme kategorisi ayarlanır **ayrıntılı** portalında.

1. Bir komut isteminde, izleme uç noktasından okumak için aşağıdaki komutu çalıştırın:

    ```
    iothub-explorer monitor-ops --login {your iothubowner connection string}
    ```

1. Başka bir komut istemi'nde, CİHAZDAN buluta iletiler gönderen bir cihazın benzetimini yapmak için aşağıdaki komutu çalıştırın:

    ```
    iothub-explorer simulate-device {your device name} --send "My test message" --login {your iothubowner connection string}
    ```

1. Sanal cihaz IOT hub'ınıza bağlanır gibi ilk komut istemi izleme olayları gösterir.

## <a name="connect-to-the-monitoring-endpoint"></a>İzleme uç noktasına bağlanma

IOT hub'ınızı izleme uç noktada bir Event Hub ile uyumlu uç noktadır. İzleme iletileri Bu uç noktasından okumak için Event Hubs ile çalışan herhangi bir mekanizma kullanabilirsiniz. Aşağıdaki örnek, bir yüksek işleme dağıtımına uygun olmayan temel bir okuyucu oluşturur. Event Hubs'dan iletilerin nasıl işleneceği hakkında daha fazla bilgi için [Event Hubs ile Çalışmaya Başlama][lnk-eventhubs-tutorial] öğreticisine bakın.

İzleme uç noktaya bağlanmak için bir bağlantı dizesi ve uç nokta adı gerekir. Aşağıdaki adımlar Portalı'nda gerekli değerleri nasıl gösterir:

1. Portalda, IOT hub'ı kaynak dikey pencerenize gidin.

1. Seçin **işlem izleme**ve Not **Event Hub ile uyumlu adı** ve **Event Hub ile uyumlu uç nokta** değerleri:

    ![Event Hub ile uyumlu uç nokta değerleri][img-endpoints]

1. Seçin **paylaşılan erişim ilkeleri**, ardından **hizmet**. Not **birincil anahtar** değeri:

    ![Hizmet paylaşılan erişim ilkesi birincil anahtarı][img-service-key]

Aşağıdaki C# kod örneği, Visual Studio'dan alınmış **Windows Klasik Masaüstü** C# konsol uygulaması. Proje **WindowsAzure.ServiceBus** NuGet paketi yüklü.

* Bağlantı dizesi yer tutucusunu kullanan bir bağlantı dizesiyle değiştirin **Event Hub ile uyumlu uç nokta** ve hizmet **birincil anahtar** daha önce aşağıdaki örnekte gösterildiği gibi değerleri:

    ```cs
    "Endpoint={your Event Hub-compatible endpoint};SharedAccessKeyName=service;SharedAccessKey={your service primary key value}"
    ```

* İzleme uç noktası adı tutucusuyla değiştirin **Event Hub ile uyumlu adı** daha önce değer.

```cs
class Program
{
    static string connectionString = "{your monitoring endpoint connection string}";
    static string monitoringEndpointName = "{your monitoring endpoint name}";
    static EventHubClient eventHubClient;

    static void Main(string[] args)
    {
        Console.WriteLine("Monitoring. Press Enter key to exit.\n");

        eventHubClient = EventHubClient.CreateFromConnectionString(connectionString, monitoringEndpointName);
        var d2cPartitions = eventHubClient.GetRuntimeInformation().PartitionIds;
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
            }
        }
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla IOT Hub'ın özelliklerini keşfetmek için bkz:

* [IOT Hub Geliştirici Kılavuzu][lnk-devguide]
* [Azure IOT Edge ile sınır cihazlarına Al dağıtma][lnk-iotedge]

<!-- Links and images -->
[1]: media/iot-hub-operations-monitoring/enable-OM-1.png
[2]: media/iot-hub-operations-monitoring/enable-OM-2.png
[img-endpoints]: media/iot-hub-operations-monitoring/monitoring-endpoint.png
[img-service-key]: media/iot-hub-operations-monitoring/service-key.png

[lnk-blog-announcement]: https://azure.microsoft.com/blog/monitor-your-azure-iot-solutions-with-azure-monitor-and-azure-resource-health
[lnk-monitor]: iot-hub-monitor-resource-health.md
[lnk-get-started]: quickstart-send-telemetry-dotnet.md
[lnk-diagnostic-metrics]: iot-hub-metrics.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-dr]: iot-hub-ha-dr.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md
[lnk-iothub-explorer]: https://github.com/azure/iothub-explorer
[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
