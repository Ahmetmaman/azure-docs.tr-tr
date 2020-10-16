---
title: "Hızlı başlangıç: JavaScript için konuşma SDK 'Sı (tarayıcı) Platform Kurulumu-konuşma hizmeti"
titleSuffix: Azure Cognitive Services
description: Bunu konuşma hizmeti SDK 'Sı ile JavaScript (tarayıcı) kullanmaya yönelik platformunuzu ayarlamak için bu kılavuzu kullanın.
services: cognitive-services
author: markamos
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 10/15/2020
ms.author: erhopf
ms.custom: devx-track-js
ms.openlocfilehash: ec73394e184e3c16fbc4eebf2a1090c9e52ddee4
ms.sourcegitcommit: 93329b2fcdb9b4091dbd632ee031801f74beb05b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2020
ms.locfileid: "92097057"
---
Bu kılavuzda, bir Web sayfasıyla kullanılmak üzere JavaScript için [konuşma SDK 'sının](~/articles/cognitive-services/speech-service/speech-sdk.md) nasıl yükleneceği gösterilmektedir.

[!INCLUDE [License Notice](~/includes/cognitive-services-speech-service-license-notice.md)]

## <a name="create-a-new-website-folder"></a>Yeni bir Web sitesi klasörü oluşturma

Yeni, boş bir klasör oluşturun. Örneği bir web sunucusunda barındırmak istiyorsanız, web sunucusunun klasöre erişebildiğinden emin olun.

## <a name="unpack-the-speech-sdk-for-javascript-into-that-folder"></a>JavaScript için Konuşma SDK’sını bu klasöre çıkarın

Konuşma SDK’sını [.zip paketi](https://aka.ms/csspeech/jsbrowserpackage) olarak indirin ve yeni oluşturulan klasöre çıkarın. Bu, beş dosyanın dağıtılmasına neden olur:
* `microsoft.cognitiveservices.speech.sdk.bundle.js` Konuşma SDK 'sının okunabilir bir sürümü.
* `microsoft.cognitiveservices.speech.sdk.bundle.js.map` SDK kodunda hata ayıklamak için kullanılan bir eşleme dosyası.
* `microsoft.cognitiveservices.speech.sdk.bundle.d.ts` TypeScript ile kullanılacak nesne tanımları
* `microsoft.cognitiveservices.speech.sdk.bundle-min.js` Konuşma SDK 'sının küçültülmüş bir sürümü.
* `speech-processor.js` Bazı tarayıcılarda performansı artırmak için kod.

## <a name="create-an-indexhtml-page"></a>Bir index.html sayfası oluşturma

Klasörde `index.html` adlı yeni bir dosya oluşturun ve bu dosyayı bir metin düzenleyiciyle açın.

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [windows](../quickstart-list.md)]