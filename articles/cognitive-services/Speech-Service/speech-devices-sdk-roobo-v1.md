---
title: Konuşma cihazları SDK Roobo akıllı ses geliştirme seti v1-konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Konuşma cihazları SDK 'Sı ile çalışmaya başlama için Önkoşullar ve yönergeler, Roobo Smart Audio Dev Kit v1.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: 73eb1225ffc5fd01f9a27ca99ad2b059d45a36cf
ms.sourcegitcommit: 10d00006fec1f4b69289ce18fdd0452c3458eca5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2020
ms.locfileid: "95015299"
---
# <a name="device-roobo-smart-audio-dev-kit"></a>Cihaz: Roobo akıllı ses geliştirme seti

Bu makalede, Roobo Smart Audio Dev Kit için cihaza özgü bilgiler sağlanmaktadır.

## <a name="set-up-the-development-kit"></a>Geliştirme setini ayarlama

1. Geliştirme setinde iki mikro USB bağlayıcısı vardır. Sol bağlayıcı, geliştirme setini güçlendirin ve aşağıdaki görüntüde güç olarak vurgulanır. Bu, doğru bir denetimdir ve görüntüde hata ayıklama olarak işaretlenir.

    ![Geliştirme setini bağlama](media/speech-devices-sdk/qsg-1.png)

1. Güç bağlantı noktasını bir BILGISAYARA veya güç bağdaştırıcısına bağlamak için mikro USB kablosu kullanarak geliştirme setini güçlendirin. Üst panonun altında yeşil bir güç göstergesi açılır.

1. Geliştirme setini denetlemek için, ikinci bir mikro USB kablosu kullanarak hata ayıklama bağlantı noktasını bir bilgisayara bağlayın. Güvenilir iletişimler sağlamak için yüksek kaliteli bir kablonun kullanılması önemlidir.

1. Geliştirme setinizi dairesel veya doğrusal yapılandırma için yönlendirmek.

    |Geliştirme Seti yapılandırması|Yön|
    |-----------------------------|------------|
    |Çember|Yukarı bakan mikrofonlarla yukarı doğru|
    |Doğrusal|Tarafında, mikrofonlarla ilgili olarak (aşağıdaki görüntüde gösterildiği gibi)|

    ![Doğrusal geliştirme seti yönü](media/speech-devices-sdk/qsg-2.png)

1. Sertifikaları yükleyip ses cihazının izinlerini ayarlayın. Komut Istemi penceresinde aşağıdaki komutları yazın:

   ```powershell
   adb push C:\SDSDK\Android-Sample-Release\scripts\roobo_setup.sh /data/
   adb shell
   cd /data/
   chmod 777 roobo_setup.sh
   ./roobo_setup.sh
   exit
   ```

    > [!NOTE]
    > Bu komutlar, `adb.exe` Android Studio yüklemesinin bir parçası olan Android Debug Bridge kullanır. Bu araç, C:\Users \[ Kullanıcı adı] \AppData\Local\Android\Sdk\platform-Tools. içinde bulunur Bu dizini, çağrılmasını daha kolay hale getirmek için yolunuza ekleyebilirsiniz `adb` . Aksi takdirde, öğesini çağıran her komutta adb.exe yüklemenizin tam yolunu belirtmeniz gerekir `adb` .
    >
    > Bir hata görürseniz, `no devices/emulators found` USB kablonuzun bağlı olduğunu ve yüksek kaliteli bir kablo olduğunu denetleyin. `adb devices`Bilgisayarınızın, cihaz listesini döndürdüğü için geliştirme seti ile konuştuğunu denetlemek için kullanabilirsiniz.
    >
    > [!TIP]
    > Geliştirme Seti mikrofonlarla çalıştığınızdan emin olmak için BILGISAYARıNıZıN mikrofonunu ve hoparlörünü kapatın. Bu şekilde, yanlışlıkla cihazı BILGISAYARDAN sesle tetikleymezsiniz.

1. Geliştirme setine bir konuşmacı eklemek istiyorsanız, bunu ses hattına bağlayabilirsiniz. 3,5 mm analog bir eklentiyle iyi kalitede bir konuşmacı seçmeniz gerekir.

    ![Vysor ses](media/speech-devices-sdk/qsg-14.png)

## <a name="development-information"></a>Geliştirme bilgileri

Daha fazla geliştirme bilgisi için, [Roobo geliştirme kılavuzuna](http://dwn.roo.bo/server_upload/ddk/ROOBO%20Dev%20Kit-User%20Guide.pdf)bakın.

## <a name="audio"></a>Ses

Roobo, Flash belleğine tüm sesleri yakalayan bir araç sağlar. Ses sorunlarını gidermenize yardımcı olabilir. Her geliştirme seti yapılandırması için aracın bir sürümü sağlanır. [Roobo sitesinde](http://ddk.roobo.com/)cihazınızı seçin ve ardından sayfanın altındaki **roobo araçları** bağlantısını seçin.

## <a name="next-steps"></a>Sonraki adımlar

* [Android örnek uygulamasını çalıştırma](./speech-devices-sdk-quickstart.md?pivots=platform-android%253fpivots%253dplatform-android)