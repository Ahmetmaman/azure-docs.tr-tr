---
title: Bulut (Node.js) - Azure IOT hub'a bağlanma Raspberry Pi web simülatörü Raspberry Pi'yi simülasyonu | Microsoft Docs
description: Raspberry Pi, Azure bulutuna veri göndermek için Azure IOT Hub ile Raspberry Pi web simülatörü bağlanın.
author: rangv
manager: ''
keywords: raspberry pi simülatör, azure IOT raspberry pi, raspberry pi IOT hub, bulut verilerini raspberry pi göndermek, buluta raspberry pi
ms.service: iot-hub
services: iot-hub
ms.devlang: nodejs
ms.topic: conceptual
ms.date: 04/11/2018
ms.author: rangv
ms.openlocfilehash: 2dd9b14ebd7e64a1073ab773b2f1ac8d8c05ac0a
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2018
ms.locfileid: "39185256"
---
# <a name="connect-raspberry-pi-online-simulator-to-azure-iot-hub-nodejs"></a>Raspberry Pi çevrimiçi simülatör (Node.js) Azure IOT hub'a bağlama

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

Bu öğreticide, Raspberry Pi çevrimiçi simülatör ile çalışmanın temel bilgileri öğrenerek başlayın. Daha sonra PI simülatör'ü kullanarak buluta sorunsuz bir şekilde bağlanmak nasıl öğrenin [Azure IOT hub'ı](about-iot-hub.md). 

Fiziksel cihazlar varsa [Azure IOT hub'a bağlanma Raspberry Pi'yi](iot-hub-raspberry-pi-kit-node-get-started.md) kullanmaya başlamak için. 

<p>
<div id="diag" style="width:100%; text-align:center">
<a href="https://azure-samples.github.io/raspberry-pi-web-simulator/#getstarted" target="_blank">
<img src="media/iot-hub-raspberry-pi-web-simulator/3_banner.png" alt="Connect Raspberry Pi web simulator to Azure IoT Hub" width="400">
</div>
<p>
<div id="button" style="width:100%; text-align:center">
<a href="https://azure-samples.github.io/raspberry-pi-web-simulator/#Getstarted" target="_blank">
<img src="media/iot-hub-raspberry-pi-web-simulator/6_button_default.png" alt="Start Raspberry Pi simulator" width="400" onmouseover="this.src='media/iot-hub-raspberry-pi-web-simulator/5_button_click.png';" onmouseout="this.src='media/iot-hub-raspberry-pi-web-simulator/6_button_default.png';">
</div>

## <a name="what-you-do"></a>Neler

* Raspberry Pi çevrimiçi simülatör ile ilgili temel bilgileri öğrenin.
* IOT hub oluşturun.
* Bir cihaz IOT hub'ına PI sayısı için kaydedin.
* IOT hub'ınıza sanal sensör verilerini göndermeyi Pi üzerinde bir örnek uygulamayı çalıştırın.

Benzetimli Raspberry Pi'yi oluşturduğunuz IOT hub'a bağlayın. Ardından simülatör ile algılayıcı verilerini oluşturmak için örnek uygulamayı çalıştırın. Son olarak, IOT hub'ınıza sensör verilerini gönderin.

## <a name="what-you-learn"></a>Öğrenecekleriniz

* Azure IOT hub oluşturma ve yeni cihaz bağlantı dizesini almak nasıl. Azure hesabınız yoksa, [ücretsiz Azure deneme hesabı oluşturma](https://azure.microsoft.com/free/) yalnızca birkaç dakika içinde.
* Raspberry Pi çevrimiçi simülatör ile çalışmayı öğrenin.
* IOT hub'ınıza sensör verilerini gönderme işlemini.

## <a name="overview-of-raspberry-pi-web-simulator"></a>Raspberry Pi web simülatörü'ne genel bakış

Raspberry Pi çevrimiçi simülatör başlatmak için düğmeye tıklayın.

> [!div class="button"]
<a href="https://azure-samples.github.io/raspberry-pi-web-simulator/#GetStarted" target="_blank">Raspberry Pi simülatörü başlatın</a>

Web benzetici üç alan vardır.
1. Derleme - varsayılan bağlantı hattının bir PI BME280 algılayıcı ve bir LED ile bağlanan alanıdır. Önizleme sürümünde alan kilitli özelleştirme bu nedenle şu anda bunu yapamazsınız.
2. Alan - bir çevrimiçi kod düzenleyicisine, kod ile Raspberry Pi kodlama. Varsayılan örnek uygulamayı BME280 algılayıcıdan algılayıcı verilerini toplamak için yardımcı olur ve Azure IOT Hub'ınıza gönderir. Uygulamayı gerçek PI cihazlarla tamamen uyumludur. 
3. Tümleşik bir konsol penceresi - kodunuzu çıktısını gösterir. Bu pencerenin en üstünde üç düğme bulunur.
   * **Çalıştırma** -kodlama alanında uygulamayı çalıştırın.
   * **Sıfırlama** -varsayılan örnek uygulamaya kodlama alan sıfırlayın.
   * **Katlama/genişletme** -sağ tarafta, konsol penceresinde Katlama/genişletmek bir düğme vardır.

> [!NOTE] 
Raspberry Pi web simülatörü artık Önizleme sürümünde kullanılabilir. Sesinizi de duymak istiyoruz [Gitter odası](https://gitter.im/Microsoft/raspberry-pi-web-simulator). Kaynak kodu public [Github](https://github.com/Azure-Samples/raspberry-pi-web-simulator).

![Pi çevrimiçi simülatör'ne genel bakış](media/iot-hub-raspberry-pi-web-simulator/0_overview.png)

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]


## <a name="run-a-sample-application-on-pi-web-simulator"></a>Pi web simülatörü hakkında bir örnek uygulamayı çalıştırma

1. Alan kodlama, varsayılan örnek uygulama üzerinde çalıştığından emin olun. Satır 15 yer tutucuyu, Azure IOT hub cihaz bağlantı dizesiyle değiştirin.
   ![Cihaz bağlantı dizesini değiştirin](media/iot-hub-raspberry-pi-web-simulator/1_connectionstring.png)

2. Tıklayın **çalıştırma** veya türü `npm start` uygulamayı çalıştırın.


Algılayıcı verilerini ve IOT hub'ınıza gönderdiği iletileri gösterir aşağıdaki çıktıyı görmeniz gerekir ![çıkış - IOT hub'ınıza Raspberry Pi'dan gönderilen algılayıcı verileri](media/iot-hub-raspberry-pi-web-simulator/2_run_application.png)


## <a name="next-steps"></a>Sonraki adımlar

Algılayıcı verilerini toplamak ve IOT hub'ına göndermek için örnek bir uygulama çalıştırdınız.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
