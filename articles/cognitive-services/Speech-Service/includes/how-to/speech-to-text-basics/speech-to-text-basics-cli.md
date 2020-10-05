---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 09/08/2020
ms.author: trbye
ms.openlocfilehash: 905eacc3751b3d5d6c66a2fdb0e1391a747ab895
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91327122"
---
Konuşma hizmetinin temel özelliklerinden biri de insan konuşmanızı tanıyabilme ve (genellikle konuşma-metin olarak adlandırılır). Bu hızlı başlangıçta, yüksek kaliteli konuşmadan metne dönüştürme gerçekleştirmek için uygulama ve ürünlerinize yönelik konuşma CLı 'sını nasıl kullanacağınızı öğreneceksiniz.

[!INCLUDE [SPX Setup](../../spx-setup.md)]

## <a name="speech-to-text-from-microphone"></a>Mikrofondan konuşmaya metin

BILGISAYAR mikrofonunuzu takın ve açın ve mikrofonu da kullanabilir olabilecek tüm uygulamaları kapatın. Bazı bilgisayarlarda yerleşik bir mikrofon vardır, diğerleri ise Bluetooth cihazının yapılandırılması gerekir.

Artık mikrofona konuşmayı tanımak için konuşma CLı 'sını çalıştırmaya hazırsınız demektir. Komut satırından, konuşma CLı ikili dosyasını içeren dizine geçin ve aşağıdaki komutu çalıştırın.

```bash
spx recognize --microphone
```

> [!NOTE]
> Konuşma CLı varsayılan olarak Ingilizce 'Dir. [Konuşma-metin tablosundan](../../../../language-support.md)farklı bir dil seçebilirsiniz.
> Örneğin, `--source de-DE` Almanya konuşmanızı tanımak için ekleyin.

Mikrofona konuşun ve kelimelerinizin gerçek zamanlı olarak metne göre dökümünü görürsünüz. Konuşma CLı, bir süre sessizden sonra durur veya CTRL-C tuşlarına basın.

## <a name="speech-to-text-from-audio-file"></a>Ses dosyasından konuşmayı metne dönüştürme

Konuşma CLı, konuşmayı birçok dosya biçiminde ve doğal dillerde tanıyabilir. Bu örnekte, Ingilizce konuşma içeren herhangi bir WAV dosyasını (16kHz veya 8kHz, 16 bit ve mono PCM) kullanabilirsiniz. Ya da bir hızlı örnek isterseniz, <a href="https://github.com/Azure-Samples/cognitive-services-speech-sdk/blob/master/samples/csharp/sharedcontent/console/whatstheweatherlike.wav" download="whatstheweatherlike" target="_blank">whatsthedalgalı gibi. wav <span class="docon docon-download x-hidden-focus"></span> </a> dosyasını INDIRIN ve konuşma CLI ikili dosyası ile aynı dizine kopyalayın.

Artık, aşağıdaki komutu çalıştırarak ses dosyasında bulunan konuşmayı tanımak için konuşma CLı 'yı çalıştırmaya hazırsınız demektir.

```bash
spx recognize --file whatstheweatherlike.wav
```

> [!NOTE]
> Konuşma CLı varsayılan olarak Ingilizce 'Dir. [Konuşma-metin tablosundan](../../../../language-support.md)farklı bir dil seçebilirsiniz.
> Örneğin, `--source de-DE` Almanya konuşmanızı tanımak için ekleyin.

Konuşma CLı, ekranda konuşmanın bir metin dökümünü gösterir.