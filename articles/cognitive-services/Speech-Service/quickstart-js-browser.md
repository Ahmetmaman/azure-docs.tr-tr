---
title: 'Hızlı Başlangıç: JavaScript (tarayıcı) - konuşma Hizmetleri konuşma tanıma'
titleSuffix: Azure Cognitive Services
description: Konuşma Tanıma Hizmeti SDK’sını kullanarak tarayıcıda JavaScript’te konuşma tanımayı öğrenme
services: cognitive-services
author: fmegen
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: quickstart
ms.date: 11/06/2018
ms.author: fmegen
ms.openlocfilehash: b6add6ac94e45789033cfe9913ec64e69f394f45
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2018
ms.locfileid: "53715146"
---
# <a name="quickstart-recognize-speech-in-javascript-in-a-browser-using-the-speech-service-sdk"></a>Hızlı Başlangıç: Konuşma hizmeti SDK'sını kullanarak bir tarayıcıda JavaScript dilinde konuşma tanıma

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Bu makalede, konuşmayı metne dönüştürmek için Bilişsel Hizmetler Konuşma SDK’sının JavaScript bağlamasını kullanarak bir web sitesi oluşturmayı öğreneceksiniz.
Uygulama üzerindeki Microsoft Bilişsel hizmetler konuşma SDK bağlıdır ([indirme sürümü 1.2.0](https://aka.ms/csspeech/jsbrowserpackage)).

## <a name="prerequisites"></a>Önkoşullar

* Konuşma tanıma hizmeti için bir abonelik anahtarı. Bkz. [Konuşma Tanıma Hizmetini ücretsiz olarak deneyin](get-started.md).
* Çalışan bir mikrofon ile bir PC veya Mac.
* Bir metin düzenleyici.
* Chrome ya da Microsoft Edge’in güncel bir sürümü.
* İsteğe bağlı olarak, PHP betiklerini barındırmayı destekleyen bir web sunucusu.

## <a name="create-a-new-website-folder"></a>Yeni bir Web sitesi klasörü oluşturma

Yeni, boş bir klasör oluşturun. Örneği bir web sunucusunda barındırmak istiyorsanız, web sunucusunun klasöre erişebildiğinden emin olun.

## <a name="unpack-the-speech-sdk-for-javascript-into-that-folder"></a>JavaScript için Konuşma SDK’sını bu klasöre çıkarın

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

Konuşma SDK’sını [.zip paketi](https://aka.ms/csspeech/jsbrowserpackage) olarak indirin ve yeni oluşturulan klasöre çıkarın. Bu açılmış iki dosyada sonuçları `microsoft.cognitiveservices.speech.sdk.bundle.js` ve `microsoft.cognitiveservices.speech.sdk.bundle.js.map`.
İkinci dosyası isteğe bağlıdır ve SDK'sı koda hata ayıklama için yararlıdır.

## <a name="create-an-indexhtml-page"></a>Bir index.html sayfası oluşturma

Klasörde `index.html` adlı yeni bir dosya oluşturun ve bu dosyayı bir metin düzenleyiciyle açın.

1. Aşağıdaki HTML çatısını oluşturun:

  ```html
  <html>
  <head>
      <title>Microsoft Cognitive Service Speech SDK JavaScript Quickstart</title>
  </head>
  <body>
    <!-- UI code goes here -->

    <!-- SDK reference goes here -->

    <!-- Optional authorization token request goes here -->

    <!-- Sample code goes here -->
  </body>
  </html>
  ```

1. Aşağıda kullanıcı arabirimi kodunu dosyanızda ilk açıklamanın altına ekleyin:

  [!code-html[](~/samples-cognitive-services-speech-sdk/quickstart/js-browser/index.html#uidiv)]

1. Konuşma SDK’sına bir başvuru ekleme

  [!code-html[](~/samples-cognitive-services-speech-sdk/quickstart/js-browser/index.html#speechsdkref)]

1. Yukarı düğme tanıma, tanıma işleminin sonucu ve UI kod tarafından tanımlanan abonelikle ilişkili alanları için işleyiciler bağlayabilirsiniz:

  [!code-html[](~/samples-cognitive-services-speech-sdk/quickstart/js-browser/index.html#quickstartcode)]

## <a name="create-the-token-source-optional"></a>Belirteç kaynağı oluşturma (isteğe bağlı)

Web sayfasını bir web sunucusunda barındırmak istiyorsanız, tanıtım uygulamanız için isteğe bağlı olarak bir belirteç kaynağı sağlayabilirsiniz.
Bu şekilde, abonelik anahtarınız hiçbir zaman sunucunuzdan çıkmaz ve kullanıcılar bir yetkilendirme kodu girmeden konuşma özelliklerini kullanabilir.

1. `token.php` adlı yeni bir dosya oluşturun. Bu örnekte web sunucunuzun PHP betik oluşturma dilini desteklediği varsayılmaktadır. Aşağıdaki kodu girin:

  [!code-php[](~/samples-cognitive-services-speech-sdk/quickstart/js-browser/token.php)]

1. `index.html` dosyasını düzenleyin ve aşağıdaki kodu dosyanıza ekleyin:

  [!code-html[](~/samples-cognitive-services-speech-sdk/quickstart/js-browser/index.html#authorizationfunction)]

> [!NOTE]
> Yetkilendirme belirteçlerinin sınırlı bir kullanım ömrü vardır.
> Bu basit örnekte yetkinlendirme belirteçlerinin otomatik olarak yenilenmesi gösterilmez. Kullanıcı olarak, sayfayı el ile yeniden yükleyebilir veya yenilemek için F5’e basabilirsiniz.

## <a name="build-and-run-the-sample-locally"></a>Örneği yerel olarak derleyin ve çalıştırın

Uygulamayı başlatmak için, index.html dosyasına çift tıklayın veya index.html dosyasını sık kullandığınız web tarayıcısıyla açın. Abonelik anahtarınızı ve [bölgenizi](regions.md) girmenize ve mikrofonu kullanarak bir tanıma tetiklemenize olanak sağlayan basit bir GUI sunar.

## <a name="build-and-run-the-sample-via-a-web-server"></a>Web sunucusu aracılığıyla örnek derleme ve çalıştırma

Uygulamanızı başlatmak için, sık kullandığınız bir web tarayıcısını açın klasörü barındırdığınız ortak URL’ye işaret edin, [bölgenizi](regions.md) girin ve mikrofonu kullanarak bir tanıma tetikleyin. Yapılandırılırsa, belirteç kaynağınızdan bir belirteç alır.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [GitHub üzerinde JavaScript örneklerini keşfedin](https://aka.ms/csspeech/samples)
