---
title: Azure IOT Hub cihaz akışları C# Hızlı Başlangıç (Önizleme) | Microsoft Docs
description: Bu hızlı başlangıçta, iki örnek çalışacak C# , IOT hub'ı aracılığıyla kurulan bir cihaz akışını aracılığıyla iletişim kuran uygulamaları.
author: rezasherafat
manager: briz
ms.service: iot-hub
services: iot-hub
ms.devlang: csharp
ms.topic: quickstart
ms.custom: mvc
ms.date: 01/15/2019
ms.author: rezas
ms.openlocfilehash: 45f0b7deb0e14a398c4f1220e66239c3727e46e1
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57444235"
---
# <a name="quickstart-communicate-to-device-applications-in-c-via-iot-hub-device-streams-preview"></a>Hızlı Başlangıç: Cihaz uygulamaları kullanıcılara C# aracılığıyla IOT Hub cihaz akışları (Önizleme)

[!INCLUDE [iot-hub-quickstarts-3-selector](../../includes/iot-hub-quickstarts-3-selector.md)]

[IOT Hub cihaz akışları](./iot-hub-device-streams-overview.md) güvenli ve güvenlik duvarı uyumlu bir şekilde iletişim kurmak hizmet ve cihaz uygulamalarınıza izin verin. Bu hızlı başlangıçta iki içerir C# sürekli veri (Yankı) göndermek için cihaz akışları yararlanan programlar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıçta çalıştırdığınız iki örnek uygulama, C# kullanılarak yazılır. Geliştirme makinenizde .NET Core SDK 2.1.0 veya üzeri bir sürüm olması gerekir.

[.NET](https://www.microsoft.com/net/download/all)’ten birden fazla platform için .NET Core SDK’sını indirebilirsiniz.

Aşağıdaki komutu kullanarak geliştirme makinenizde geçerli C# sürümünü doğrulayabilirsiniz:

```
dotnet --version
```

https://github.com/Azure-Samples/azure-iot-samples-csharp/archive/master.zip adresinden örnek C# projesini indirin ve ZIP arşivini ayıklayın. Hem cihaz hem de hizmet tarafında ihtiyacınız.

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub-device-streams.md)]

## <a name="register-a-device"></a>Cihaz kaydetme

Bir cihazın bağlanabilmesi için IoT hub’ınıza kaydedilmesi gerekir. Bu hızlı başlangıçta Azure Cloud Shell kullanarak bir simülasyon cihazı kaydedeceksiniz.

1. Aşağıdaki komutları Azure Cloud Shell'de çalıştırarak IoT Hub CLI uzantısını ekleyin ve cihaz kimliğini oluşturun. 

   **YourIoTHubName**: Aşağıda bu yer tutucu IOT hub'ınız için seçtiğiniz adıyla değiştirin.

   **Cihazım**: Bu, kayıtlı bir cihaz için verilen addır. Cihazım gösterildiği gibi kullanın. Cihazınız için farklı bir ad seçerseniz bu makalenin geri kalan bölümünde aynı adı kullanmanız ve örnek uygulamaları çalıştırmadan önce bunlarda da cihaz adını güncelleştirmeniz gerekir.

    ```azurecli-interactive
    az extension add --name azure-cli-iot-ext
    az iot hub device-identity create --hub-name YourIoTHubName --device-id MyDevice
    ```

2. Yeni kaydettiğiniz cihazın _cihaz bağlantı dizesini_ almak için aşağıdaki komutları Azure Cloud Shell'de çalıştırın:

   **YourIoTHubName**: Aşağıda bu yer tutucu IOT hub'ınız için seçtiğiniz adıyla değiştirin.

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name YourIoTHubName --device-id MyDevice --output table
    ```

    Aşağıdaki gibi görünen cihaz bağlantı dizesini not edin:

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyDevice;SharedAccessKey={YourSharedAccessKey}`

    Bu değeri hızlı başlangıcın ilerleyen bölümlerinde kullanacaksınız.

3. Ayrıca gerekir _hizmet bağlantı dizesini_ IOT hub'ınıza bağlanmak ve bir cihaz akışını kurmak Hizmet tarafı uygulamasını etkinleştirmek için IOT hub'ından. Aşağıdaki komut, IOT hub'ınız için bu değeri alır:

   **YourIoTHubName**: Aşağıda bu yer tutucu IOT hub'ınız için seçtiğiniz adıyla değiştirin.

    ```azurecli-interactive
    az iot hub show-connection-string --policy-name service --hub-name YourIoTHubName
    ```

    Şuna benzer döndürülen değeri not edin:

   `"HostName={YourIoTHubName}.azure-devices.net;SharedAccessKeyName=service;SharedAccessKey={YourSharedAccessKey}"`

## <a name="communicate-between-device-and-service-via-device-streams"></a>Cihaz ve hizmet aracılığıyla cihaz akışları arasında iletişim

### <a name="run-the-service-side-application"></a>Hizmet tarafı uygulamayı çalıştırın

Gidin `iot-hub/Quickstarts/device-streams-echo/service` sıkıştırması proje klasörünüzde. Aşağıdaki bilgiler yararlı gerekir:

| Parametre adı | Parametre değeri |
|----------------|-----------------|
| `ServiceConnectionString` | IOT hub'ınızın hizmeti bağlantı dizesini belirtin. |
| `DeviceId` | Örneğin, Cihazım daha önce oluşturulan cihaz kimliği sağlayın. |

Derleyin ve kodun aşağıdaki gibi çalıştırın:

```
cd ./iot-hub/Quickstarts/device-streams-echo/service/

# Build the application
dotnet build

# Run the application
# In Linux/MacOS
dotnet run "<ServiceConnectionString>" "<MyDevice>"

# In Windows
dotnet run <ServiceConnectionString> <MyDevice>
```

> [!NOTE]
> Aygıt tarafı uygulamayı zamanında yanıt vermezse, bir zaman aşımı meydana gelir.

### <a name="run-the-device-side-application"></a>Aygıt tarafı uygulamayı çalıştırın

Gidin `iot-hub/Quickstarts/device-streams-echo/device` sıkıştırması proje klasörünüzde dizin. Aşağıdaki bilgiler yararlı gerekir:

| Parametre adı | Parametre değeri |
|----------------|-----------------|
| `DeviceConnectionString` | IOT hub'ınızın cihaz bağlantı dizesini belirtin. |

Derleyin ve kodun aşağıdaki gibi çalıştırın:

```
cd ./iot-hub/Quickstarts/device-streams-echo/device/

# Build the application
dotnet build

# Run the application
# In Linux/MacOS
dotnet run "<DeviceConnectionString>"

# In Windows
dotnet run <DeviceConnectionString>
```

Son adımın sonunda, hizmet tarafı program cihazınıza ve kurulan sonra bir akışı başlatacak bir dize arabelleğine akış üzerinden hizmete gönderin. Bu örnekte, hizmet tarafı programı yalnızca geri başarılı çift yönlü iletişim iki uygulama arasındaki gösteren bir cihaza aynı verileri görüntülemektedir. Aşağıdaki şekle bakın.

Konsol çıktısı cihaz tarafında: ![alternatif metin](./media/quickstart-device-streams-echo-csharp/device-console-output.png "konsol çıktısı cihaz tarafında")

Konsol çıktısı hizmet tarafında: ![alternatif metin](./media/quickstart-device-streams-echo-csharp/service-console-output.png "konsol çıktısı hizmet tarafında")

Akış üzerinden gönderilen trafik, IOT hub'ı yerine doğrudan gönderilen tünel. Bu sağlar [yararlar](./iot-hub-device-streams-overview.md#benefits).

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources-device-streams.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir IOT hub'ı ayarladınız, kayıtlı bir cihaz, cihaz akışı arasında kurulan C# cihaz ve hizmet tarafında, uygulamalar ve akış uygulamaları arasında sürekli veri göndermek için kullanılır.

Cihaz akışları hakkında daha fazla bilgi edinmek için aşağıdaki bağlantıları kullanın:

> [!div class="nextstepaction"]
> [Cihaz akışları genel bakış](./iot-hub-device-streams-overview.md)
