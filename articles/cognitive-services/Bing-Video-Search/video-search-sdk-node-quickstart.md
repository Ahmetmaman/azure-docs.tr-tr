---
title: Video arama SDK düğümü hızlı başlangıç | Microsoft Docs
description: Video arama SDK konsol uygulaması kurulumu.
titleSuffix: Azure cognitive services
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-video-search
ms.topic: article
ms.date: 02/12/2018
ms.author: v-gedod
ms.openlocfilehash: 5718c750288e0a5605db3296d2911cca5e03375c
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "35355750"
---
# <a name="video-search-sdk-node-quickstart"></a>Video arama SDK düğümü hızlı başlangıç

Bing Video arama SDK video sorgular ve ayrıştırma sonuçları için REST API işlevselliğini içerir. 

[Kaynak kodu düğümü Bing Video arama SDK örnekleri için](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/videoSearch.js) Git hub'da kullanılabilir.

## <a name="application-dependencies"></a>Uygulama bağımlılıkları

Bing Video arama SDK'yı kullanarak bir konsol uygulaması ayarlamak için çalıştırın `npm install azure-cognitiveservices-videosearch` geliştirme ortamınızda.

## <a name="video-search-client"></a>Video arama istemci
Alma bir [Bilişsel hizmetler erişim tuşu](https://azure.microsoft.com/try/cognitive-services/) altında *arama*. Bir örneğini oluşturmak `CognitiveServicesCredentials`:
```
const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;
let credentials = new CognitiveServicesCredentials('YOUR-ACCESS-KEY');
```
Ardından, istemci örneği:
```
const VideoSearchAPIClient = require('azure-cognitiveservices-videosearch');
let client = new VideoSearchAPIClient(credentials);
```
Arama sonuçları için.
```
client.videosOperations.search('Interstellar Trailer').then((result) => {
    console.log(result.value);
}).catch((err) => {
    throw err;
});

```

<!-- Remove until the response can be replace with a sanitized version.
The code prints `result.value` items to the console without parsing any text. The results will be:
- _type: 'VideoObjectElementType'

![Video results](media/video-search-sdk-node-results.png)
-->

## <a name="next-steps"></a>Sonraki adımlar

[Bilişsel hizmetler Node.js SDK'sı örneği](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples)
