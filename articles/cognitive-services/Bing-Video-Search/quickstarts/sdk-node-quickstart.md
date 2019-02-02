---
title: "Hızlı Başlangıç: Bing Video arama SDK'sı için Node.js kullanarak video arayın"
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, Node.js için Bing Video arama SDK'sını kullanarak video arama istekleri göndermek için kullanın
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.subservice: bing-video-search
ms.topic: quickstart
ms.date: 01/31/2019
ms.author: rosh
ms.openlocfilehash: ffb04634b41d14e151c5a14d052d0d9f70b34d70
ms.sourcegitcommit: ba035bfe9fab85dd1e6134a98af1ad7cf6891033
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2019
ms.locfileid: "55569560"
---
# <a name="quickstart-perform-a-video-search-with-the-bing-video-search-sdk-for-nodejs"></a>Hızlı Başlangıç: Node.js için bir video arama Bing Video arama SDK ile gerçekleştirme

Haberler için Bing Video arama SDK ile Node.js için aramaya başlamak için bu Hızlı Başlangıç'ı kullanın. Bing Video arama çoğu programlama dilleri ile uyumlu bir REST API olsa da SDK hizmeti uygulamalarınızla tümleştirmek için kolay bir yol sağlar. Bu örnek için kaynak kodu bulunabilir [GitHub](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/videoSearch.js). Bu, daha fazla ek açıklamalar ve özellikleri içerir.

## <a name="prerequisites"></a>Önkoşullar

- [Node.js](https://www.nodejs.org/)

Bing Video arama SDK'sını kullanarak bir konsol uygulaması ayarlamak için:
* Çalıştırma `npm install ms-rest-azure` geliştirme ortamınızda.
* Çalıştırma `npm install azure-cognitiveservices-videosearch` geliştirme ortamınızda.

[!INCLUDE [cognitive-services-bing-video-search-signup-requirements](../../../../includes/cognitive-services-bing-video-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Uygulamayı oluşturma ve başlatma

1. Yeni bir JavaScript dosyası, sık kullandığınız IDE veya düzenleyici oluşturma ve ekleme bir `require()` Bing Video arama SDK bildirimi ve `CognitiveServicesCredentials` modülü. Abonelik anahtarınız için bir değişken oluşturun. 
    
    ```javascript
    const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;
    const VideoSearchAPIClient = require('azure-cognitiveservices-videosearch');
    ```

2. Bir örneğini oluşturmak `CognitiveServicesCredentials` anahtarınızı. Ardından video arama istemcisi örneği oluşturmak için kullanın.

    ```javascript
    let credentials = new CognitiveServicesCredentials('YOUR-ACCESS-KEY');
    let client = new VideoSearchAPIClient(credentials);
    ```

## <a name="send-the-search-request"></a>Arama isteği gönder

1. Kullanım `client.videosOperations.search()` Bing Video arama API'si için bir arama isteği gönderin. Arama sonuçları döndürdüğünde kullanın `.then()` sonucu oturum.
    
    ```javascript
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

> [!div class="nextstepaction"]
> [Tek sayfalı bir web uygulaması oluşturma](../tutorial-bing-video-search-single-page-app.md)

## <a name="see-also"></a>Ayrıca bkz. 

* [Bing Video arama API'si nedir?](../overview.md)
* [Bilişsel Hizmetler .NET SDK örnekleri](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7)