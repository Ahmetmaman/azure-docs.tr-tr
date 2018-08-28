---
title: (.NET/.NET) Azure IOT Hub cihaz ikizlerini kullanmaya başlama | Microsoft Docs
description: Azure IOT Hub cihaz ikizlerini etiketler ekleyin ve ardından IOT Hub sorgu kullanma Sanal cihaz uygulaması ve Azure IOT hizmeti SDK'sını etiketleri ekler ve IOT Hub sorgu çalışan bir hizmet uygulaması uygulamak .NET için Azure IOT cihaz SDK'sı .NET için kullanın.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.devlang: csharp
ms.topic: conceptual
ms.date: 05/15/2017
ms.author: dobett
ms.openlocfilehash: 340453448d38db66558e59edb845f2caf4454cf9
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "43104159"
---
# <a name="get-started-with-device-twins-netnet"></a>(.NET/.NET) cihaz ikizlerini kullanmaya başlama
[!INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

Bu öğreticinin sonunda, bu bir .NET konsol uygulamanız olacaktır:

* **CreateDeviceIdentity**, bir cihaz kimliği ve sanal cihaz uygulamanızı bağlamak için ilişkili güvenlik anahtarı oluşturan bir .NET uygulaması.

* **AddTagsAndQuery**, etiketleri ekler ve cihaz ikizlerini sorgular bir .NET arka uç uygulaması.

* **ReportConnectivity**, bir cihaza benzetim yapan daha önce oluşturulan cihaz kimliğiyle IOT hub'ınıza bağlanır ve kendi bağlantı koşulu raporları bir .NET cihaz uygulamasını.

> [!NOTE]
> Makaleyi [Azure IOT SDK'ları](iot-hub-devguide-sdks.md) hem cihaz hem de arka uç uygulamaları oluşturmak için kullanabileceğiniz Azure IOT SDK'ları hakkında bilgi sağlar.
> 

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Visual Studio 2017.
* Etkin bir Azure hesabı. (Hesabınız yoksa, oluşturabileceğiniz bir [ücretsiz bir hesap](http://azure.microsoft.com/pricing/free-trial/) yalnızca birkaç dakika içinde.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

## <a name="create-the-service-app"></a>Hizmet uygulaması oluşturma

Bu bölümde, konum meta verileri ile ilişkili cihaz ikizi ekler (C# kullanarak) bir .NET konsol uygulaması oluşturma **myDeviceId**. Ardından, IOT hub'ı ABD, ardından bir hücresel bağlantı bildirilen olanları içinde bulunan aygıtları seçme içinde depolanan cihaz ikizlerini sorgular.

1. Visual Studio'da **Konsol Uygulaması** proje şablonunu kullanarak geçerli çözüme bir Visual C# Windows Klasik Masaüstü projesi ekleyin. Projeyi adlandırın **AddTagsAndQuery**.
   
    ![Yeni Visual C# Windows Klasik Masaüstü projesi](./media/iot-hub-csharp-csharp-twin-getstarted/createnetapp.png)

2. Çözüm Gezgini'nde sağ **AddTagsAndQuery** proje ve ardından **NuGet paketlerini Yönet...** .

3. İçinde **NuGet Paket Yöneticisi** penceresinde **Gözat** araması **Microsoft.Azure.Devices**. Seçin **yükleme** yüklemek için **Microsoft.Azure.Devices** paketini ve kullanım koşullarını kabul edin. Bu yordam indirir, yükler ve bir başvuru ekler [Azure IOT hizmeti SDK'sını](https://www.nuget.org/packages/Microsoft.Azure.Devices/) NuGet paketi ve bağımlılıkları.
   
    ![NuGet Paket Yöneticisi penceresi](./media/iot-hub-csharp-csharp-twin-getstarted/servicesdknuget.png)

4. Aşağıdaki `using` deyimlerini **Program.cs** dosyasının üst kısmına ekleyin:

    ```csharp  
    using Microsoft.Azure.Devices;
    ```

5. **Program** sınıfına aşağıdaki alanları ekleyin. Yer tutucu değerini, önceki bölümde hub için oluşturduğunuz IoT Hub bağlantı dizesiyle değiştirin.

    ```csharp  
    static RegistryManager registryManager;
    static string connectionString = "{iot hub connection string}";
    ```

6. **Program** sınıfına aşağıdaki yöntemi ekleyin:

    ```csharp  
    public static async Task AddTagsAndQuery()
    {
        var twin = await registryManager.GetTwinAsync("myDeviceId");
        var patch =
            @"{
                tags: {
                    location: {
                        region: 'US',
                        plant: 'Redmond43'
                    }
                }
            }";
        await registryManager.UpdateTwinAsync(twin.DeviceId, patch, twin.ETag);
   
        var query = registryManager.CreateQuery(
          "SELECT * FROM devices WHERE tags.location.plant = 'Redmond43'", 100);
        var twinsInRedmond43 = await query.GetNextAsTwinAsync();
        Console.WriteLine("Devices in Redmond43: {0}", 
          string.Join(", ", twinsInRedmond43.Select(t => t.DeviceId)));
   
        query = registryManager.CreateQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43' AND properties.reported.connectivity.type = 'cellular'", 100);
        var twinsInRedmond43UsingCellular = await query.GetNextAsTwinAsync();
        Console.WriteLine("Devices in Redmond43 using cellular network: {0}", 
          string.Join(", ", twinsInRedmond43UsingCellular.Select(t => t.DeviceId)));
    }
    ```
   
    **RegistryManager** sınıfı hizmetinden cihaz ikizlerini ile etkileşim kurmak için gereken tüm yöntemleri gösterir. Önceki kodun ilk başlatır **registryManager** nesnesi ve ardından cihaz ikizi alır **myDeviceId**ve son olarak, etiketler ile istenen konuma bilgileri güncelleştirir.
   
    Güncelleştirdikten sonra iki sorguları yürüten: yalnızca cihaz ikizlerini bulunan cihazların ilk seçer **Redmond43** tesis ve ikincisi de hücresel ağ üzerinden bağlı cihazları seçmek için sorguyu iyileştirir.
   
    Unutmayın, oluşturduğunda, önceki kod **sorgu** nesne, döndürülen belgeler sayısı üst sınırını belirtir. **Sorgu** nesne içeren bir **HasMoreResults** çağırmak için kullanabileceğiniz bir boolean özelliği **GetNextAsTwinAsync** birden çok kez tüm sonuçları almak için yöntemleri. Bir yöntemi çağıran **GetNextAsJson** değil cihaz çiftleri, örneğin, sonuçları için toplama sorguların sonuçlarını kullanılabilir.

7. Son olarak, **Main** yöntemine aşağıdaki satırları ekleyin:

    ```csharp  
    registryManager = RegistryManager.CreateFromConnectionString(connectionString);
    AddTagsAndQuery().Wait();
    Console.WriteLine("Press Enter to exit.");
    Console.ReadLine();
    ```

8. Çözüm Gezgini'nde açın **başlangıç projelerini Ayarla...**  emin **eylem** için **AddTagsAndQuery** proje **Başlat**. Çözümü derleyin.

9. Sağ tıklayarak bu uygulamayı çalıştırmak **AddTagsAndQuery** proje ve seçerek **hata ayıklama**çizgidir **yeni örnek Başlat**. Bulunan tüm cihazlar için bir cihaz sormaya sorgu sonuçları görmeniz gerekir **Redmond43** ve sonuçları bir hücresel ağ kullanan cihazlar için sınırlar sorgu için yok.
   
    ![Sorgu Sonuçları penceresinde](./media/iot-hub-csharp-csharp-twin-getstarted/addtagapp.png)

Sonraki bölümde, bağlantı bilgilerini raporlar ve önceki bölümde sorgusunun sonucunu değiştiren bir cihaz uygulaması oluşturun.

## <a name="create-the-device-app"></a>Cihaz uygulaması oluşturma

Bu bölümde oluşturduğunuz hub'ınıza bağlanan bir .NET konsol uygulaması **myDeviceId**ve sonra da bildirilen özelliklerini hücresel ağ üzerinden bağlı bilgileri içerecek şekilde güncelleştirir.

1. Visual Studio'da **Konsol Uygulaması** proje şablonunu kullanarak geçerli çözüme bir Visual C# Windows Klasik Masaüstü projesi ekleyin. Projeyi adlandırın **ReportConnectivity**.
   
    ![Yeni Windows Visual C# Klasik cihaz uygulaması](./media/iot-hub-csharp-csharp-twin-getstarted/createdeviceapp.png)
    
2. Çözüm Gezgini'nde sağ **ReportConnectivity** proje ve ardından **NuGet paketlerini Yönet...** .

3. İçinde **NuGet Paket Yöneticisi** penceresinde **Gözat** araması **Microsoft.Azure.Devices.Client**. Seçin **yükleme** yüklemek için **Microsoft.Azure.Devices.Client** paketini ve kullanım koşullarını kabul edin. Bu yordam indirir, yükler ve bir başvuru ekler [Azure IOT cihaz SDK'sını](https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/) NuGet paketi ve bağımlılıkları.
   
    ![NuGet Paket Yöneticisi penceresi istemci uygulaması](./media/iot-hub-csharp-csharp-twin-getstarted/clientsdknuget.png)

4. Aşağıdaki `using` deyimlerini **Program.cs** dosyasının üst kısmına ekleyin:

    ```csharp  
    using Microsoft.Azure.Devices.Client;
    using Microsoft.Azure.Devices.Shared;
    using Newtonsoft.Json;
    ```

5. **Program** sınıfına aşağıdaki alanları ekleyin. Yer tutucu değerini önceki bölümde not ettiğiniz cihaz bağlantı dizesiyle değiştirin.

    ```csharp  
    static string DeviceConnectionString = "HostName=<yourIotHubName>.azure-devices.net;
      DeviceId=<yourIotDeviceName>;SharedAccessKey=<yourIotDeviceAccessKey>";
    static DeviceClient Client = null;
    ```

6. **Program** sınıfına aşağıdaki yöntemi ekleyin:

    ```csharp
    public static async void InitClient()
    {
        try
        {
            Console.WriteLine("Connecting to hub");
            Client = DeviceClient.CreateFromConnectionString(DeviceConnectionString, 
              TransportType.Mqtt);
            Console.WriteLine("Retrieving twin");
            await Client.GetTwinAsync();
        }
        catch (Exception ex)
        {
            Console.WriteLine();
            Console.WriteLine("Error in sample: {0}", ex.Message);
        }
    }
    ```

    **İstemci** nesne ihtiyaç duyduğunuz etkileşim kurmak için cihaz ikizlerini CİHAZDAN ile tüm yöntemleri sunar. Yukarıda gösterilen kod başlatır **istemci** nesnesi ve ardından cihaz ikizi alır **myDeviceId**.

7. **Program** sınıfına aşağıdaki yöntemi ekleyin:

    ```csharp  
    public static async void ReportConnectivity()
    {
        try
        {
            Console.WriteLine("Sending connectivity data as reported property");
            
            TwinCollection reportedProperties, connectivity;
            reportedProperties = new TwinCollection();
            connectivity = new TwinCollection();
            connectivity["type"] = "cellular";
            reportedProperties["connectivity"] = connectivity;
            await Client.UpdateReportedPropertiesAsync(reportedProperties);
        }
        catch (Exception ex)
        {
            Console.WriteLine();
            Console.WriteLine("Error in sample: {0}", ex.Message);
        }
    }
    ```

   Kod güncelleştirmeleri yukarıda **myDeviceId**bağlantı bilgileri özelliğiyle bildirilen.

8. Son olarak, **Main** yöntemine aşağıdaki satırları ekleyin:

    ```csharp
    try
    {
        InitClient();
        ReportConnectivity();
    }
    catch (Exception ex)
    {
        Console.WriteLine();
        Console.WriteLine("Error in sample: {0}", ex.Message);
    }
    Console.WriteLine("Press Enter to exit.");
    Console.ReadLine();
    ```

9. Çözüm Gezgini'nde açın **başlangıç projelerini Ayarla...**  emin **eylem** için **ReportConnectivity** proje **Başlat**. Çözümü derleyin.

10. Sağ tıklayarak bu uygulamayı çalıştırmak **ReportConnectivity** proje ve seçerek **hata ayıklama**çizgidir **yeni örnek Başlat**. İkiz bilgi alma ve sonra bağlantı olarak göndererek görmelisiniz bir *özelliği bildirilen*.
   
    ![Rapor bağlantısı cihaz uygulamasını çalıştırın](./media/iot-hub-csharp-csharp-twin-getstarted/rundeviceapp.png)
       
11. Cihaz bağlantı bilgilerini bildirilen, her iki sorgularda görüntülenmesi gerekir. .NET çalışan **AddTagsAndQuery** uygulama sorguları yeniden çalıştırın. Bu süre **myDeviceId** iki sorgu sonuçlarında görüntülenmesi gerekir.
   
    ![Başarıyla raporlanan cihaz bağlantısı](./media/iot-hub-csharp-csharp-twin-getstarted/tagappsuccess.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure portalında yeni bir IoT hub'ı yapılandırdınız ve ardından IoT hub'ının kimlik kayıt defterinde bir cihaz kimliği oluşturdunuz. Cihaz meta verilerini etiketler bir arka uç uygulamasından eklenen ve bir sanal cihaz uygulaması rapor cihaz bağlantı bilgileri cihaz ikizinde söyleyebiliriz. Ayrıca bu bilgiler IOT Hub'ı SQL benzeri sorgu dili kullanarak sorgulamayı öğrendiniz.

Bilgi edinmek için aşağıdaki kaynakları kullanın. nasıl yapılır:

* ile cihazlardan telemetri gönderme [telemetri gönderir bir CİHAZDAN bir IOT hub'ına](quickstart-send-telemetry-dotnet.md) öğretici

* ile cihaz ikizinin istenen özellikleri kullanarak cihazları yapılandırma [kullanmak istediğiniz cihazları yapılandırmak için Özellikler](tutorial-device-twins.md) öğretici

* İle etkileşimli olarak (örneğin, bir kullanıcı tarafından denetlenen uygulamasından fan özelliğini) cihazları denetleme [doğrudan yöntemler kullanma](quickstart-control-device-node.md) öğretici.
