---
title: Azure IOT Hub cihazı akışları Node.js Hızlı Başlangıç (Önizleme) | Microsoft Docs
description: Bu hızlı başlangıçta, bir IOT cihazı ile bir cihaz akış iletişim kuran bir Node.js Hizmet tarafı uygulamalar çalışır.
author: rezasherafat
manager: briz
ms.service: iot-hub
services: iot-hub
ms.devlang: nodejs
ms.topic: quickstart
ms.custom: mvc
ms.date: 03/14/2019
ms.author: rezas
ms.openlocfilehash: 1e7efe28918cafb3fa9547c144be3360768d549c
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2019
ms.locfileid: "58079904"
---
# <a name="quickstart-communicate-to-a-device-application-in-nodejs-via-iot-hub-device-streams-preview"></a>Hızlı Başlangıç: Node.js IOT Hub cihaz akışları (Önizleme) ile bir cihaz uygulaması için iletişim

[!INCLUDE [iot-hub-quickstarts-3-selector](../../includes/iot-hub-quickstarts-3-selector.md)]

Microsoft Azure IOT Hub cihaz akışları olarak şu anda destekleyen bir [önizleme özelliği](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

[IOT Hub cihaz akışları](./iot-hub-device-streams-overview.md) güvenli ve güvenlik duvarı uyumlu bir şekilde iletişim kurmak hizmet ve cihaz uygulamalarınıza izin verin. Genel Önizleme süresince Node.js SDK'sı yalnızca hizmet tarafında cihaz akışlarını destekler. Sonuç olarak, bu hızlı başlangıçta, yalnızca hizmet tarafı uygulamayı çalıştırmak için yönergeleri kapsar. Kullanılabilir eşlik eden bir aygıt tarafı uygulama çalıştırmalısınız [C hızlı](./quickstart-device-streams-echo-c.md) veya [ C# hızlı](./quickstart-device-streams-echo-csharp.md) Kılavuzlar.

Bu hızlı başlangıçta Hizmet tarafı Node.js uygulaması aşağıdaki işlevlere sahiptir:

* IOT cihaz bir cihaz akış oluşturur.

* Komut satırından giriş okur ve geri echo cihaz uygulamaya gönderir.

Kod, veri göndermek ve almak için nasıl kullanılacağını yanı sıra, bir cihaz akış başlatma işlemi gösterilecektir.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.


## <a name="prerequisites"></a>Önkoşullar

Cihaz akışları şu anda önizlemesidir yalnızca IOT hub'ları aşağıdaki bölgelerde oluşturulan için desteklenir:

  - **Orta ABD**
  - **Orta ABD EUAP**

Bu hızlı başlangıçta Hizmet tarafı uygulamayı çalıştırmak için geliştirme makinenizi Node.js v4.x.x veya sonraki bir sürümü gerekir.

Node.js için birden çok platformdan indirebileceğiniz [Node.js.org](https://nodejs.org).

Aşağıdaki komutu kullanarak geliştirme makinenizde geçerli Node.js sürümünü doğrulayabilirsiniz:

```
node --version
```

Örnek Node.js projesini önceden indirmediyseniz https://github.com/Azure-Samples/azure-iot-samples-node/archive/streams-preview.zip adresinden indirip ZIP arşivini ayıklayın.


## <a name="create-an-iot-hub"></a>IoT hub oluşturma

Önceki tamamladıysanız [hızlı başlangıç: Bir IOT hub'ına bir CİHAZDAN telemetri gönderme](quickstart-send-telemetry-node.md), bu adımı atlayabilirsiniz.

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub-device-streams.md)]


## <a name="register-a-device"></a>Cihaz kaydetme

Önceki tamamladıysanız [hızlı başlangıç: Bir IOT hub'ına bir CİHAZDAN telemetri gönderme](quickstart-send-telemetry-node.md), bu adımı atlayabilirsiniz.

Bir cihazın bağlanabilmesi için IoT hub’ınıza kaydedilmesi gerekir. Bu hızlı başlangıçta Azure Cloud Shell kullanarak bir simülasyon cihazı kaydedeceksiniz.

1. Aşağıdaki komutları Azure Cloud Shell'de çalıştırarak IoT Hub CLI uzantısını ekleyin ve cihaz kimliğini oluşturun. 

   **YourIoTHubName**: Aşağıda bu yer tutucu IOT hub'ınız için seçtiğiniz adı ile değiştirin.

   **Cihazım**: Bu, kayıtlı bir cihaz için verilen addır. Cihazım gösterildiği gibi kullanın. Cihazınız için farklı bir ad seçerseniz bu makalenin geri kalan bölümünde aynı adı kullanmanız ve örnek uygulamaları çalıştırmadan önce bunlarda da cihaz adını güncelleştirmeniz gerekir.

    ```azurecli-interactive
    az extension add --name azure-cli-iot-ext
    az iot hub device-identity create --hub-name YourIoTHubName --device-id MyDevice
    ```

2. Arka uç uygulamasının IoT hub’ınıza bağlanmasına ve iletileri almasına olanak sağlamak için bir _hizmet bağlantı dizesi_ de gerekir. Aşağıdaki komut, IoT hub'ınız için hizmeti bağlantı dizesini alır:

    **YourIoTHubName**: Aşağıda bu yer tutucu IOT hub'ınız için seçtiğiniz adı ile değiştirin.

    ```azurecli-interactive
    az iot hub show-connection-string --policy-name service --name YourIoTHubName
    ```

    Şuna benzer döndürülen değeri not edin:

   `"HostName={YourIoTHubName}.azure-devices.net;SharedAccessKeyName=service;SharedAccessKey={YourSharedAccessKey}"`


## <a name="communicate-between-device-and-service-via-device-streams"></a>Cihaz ve hizmet aracılığıyla cihaz akışları arasında iletişim

### <a name="run-the-device-side-application"></a>Aygıt tarafı uygulamayı çalıştırın

Daha önce belirtildiği gibi IOT Hub Node.js SDK'sı hizmet tarafında yalnızca cihaz akışlarını destekler. Aygıt tarafı uygulama için bulunan eşlik eden cihaz programları kullanın [C hızlı](./quickstart-device-streams-echo-c.md) veya [ C# hızlı](./quickstart-device-streams-echo-csharp.md) Kılavuzlar. Aygıt tarafı uygulamayı sonraki adıma devam etmeden önce çalıştığından emin olun.


### <a name="run-the-service-side-application"></a>Hizmet tarafı uygulamayı çalıştırın

Aygıt tarafı uygulamayı çalıştıran varsayıldığında, node.js'de Hizmet tarafı uygulamayı çalıştırmak için aşağıdaki adımları izleyin:

- Ortam değişkenleri olarak, cihaz kimliği ve hizmet kimlik bilgilerini sağlayın.
  ```
  # In Linux
  export IOTHUB_CONNECTION_STRING="<provide_your_service_connection_string>"
  export STREAMING_TARGET_DEVICE="MyDevice"

  # In Windows
  SET IOTHUB_CONNECTION_STRING=<provide_your_service_connection_string>
  SET STREAMING_TARGET_DEVICE=MyDevice
  ```
  Değişiklik `MyDevice` cihazınız için seçtiğiniz cihaz kimliği.

- Gidin `Quickstarts/device-streams-service` , sıkıştırması, proje klasörü ve düğüm kullanarak örneği çalıştırın.
  ```
  cd azure-iot-samples-node-streams-preview/iot-hub/Quickstarts/device-streams-service
  
  # Install the preview service SDK, and other dependencies
  npm install azure-iothub@streams-preview
  npm install

  node echo.js
  ```

Son adımın sonunda, hizmet tarafı program cihazınıza ve kurulan sonra bir akışı başlatacak bir dize arabelleğine akış üzerinden hizmete gönderin. Bu örnekte, hizmet tarafı programı yalnızca terminal üzerinde stdin okur ve ardından bunu geri echo cihaza gönderir. Bu, iki uygulama arasındaki başarılı çift yönlü iletişim gösterir.

Konsol çıktısı hizmet tarafında: ![alternatif metin](./media/quickstart-device-streams-echo-nodejs/service-console-output.PNG "konsol çıktısı hizmet tarafında")


Ardından, tuşlarına basarak programı sonlandırabilirsiniz tekrar girin.


## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources-device-streams.md)]


## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir IOT hub'ı ayarladınız, kayıtlı bir cihaz, bir cihaz akışını cihazını ve hizmetini tarafında uygulamalar arasında kurulan ve akış uygulamaları arasında sürekli veri göndermek için kullanılan.

Cihaz akışları hakkında daha fazla bilgi edinmek için aşağıdaki bağlantıları kullanın:

> [!div class="nextstepaction"]
> [Cihaz akışları genel bakış](./iot-hub-device-streams-overview.md)
