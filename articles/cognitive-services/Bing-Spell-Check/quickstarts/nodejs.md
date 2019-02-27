---
title: "Hızlı Başlangıç: Bing yazım denetimi REST API'si ve Node.js ile yazım denetimi"
titlesuffix: Azure Cognitive Services
description: Bing yazım denetimi REST API'si yazım ve dilbilgisi denetimini kullanmaya başlayın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-spell-check
ms.topic: quickstart
ms.date: 02/20/2019
ms.author: aahi
ms.openlocfilehash: 8e3379a086eb09745142f4e3997ed195eb4d1de5
ms.sourcegitcommit: 24906eb0a6621dfa470cb052a800c4d4fae02787
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2019
ms.locfileid: "56885916"
---
# <a name="quickstart-check-spelling-with-the-bing-spell-check-rest-api-and-nodejs"></a>Hızlı Başlangıç: Bing yazım denetimi REST API'si ve Node.js ile yazım denetimi

Bu hızlı başlangıçta, Bing yazım denetimi REST API'si, ilk çağrı yapmak için kullanın. Tarafından önerilen düzeltmeler ve ardından bu basit Python uygulaması API'sine bir istek gönderir ve bir kelimelerin tanıyamadık, bir liste döndürür. Bu uygulama Python ile yazılmış olmakla birlikte API, çoğu programlama diliyle uyumlu bir RESTful Web hizmetidir. Bu uygulama için kaynak kodu kullanılabilir [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/nodejs/Search/BingSpellCheckv7.js).

## <a name="prerequisites"></a>Önkoşullar

* [Node.js 6](https://nodejs.org/en/download/) veya üzeri.

[!INCLUDE [cognitive-services-bing-spell-check-signup-requirements](../../../../includes/cognitive-services-bing-spell-check-signup-requirements.md)]


## <a name="create-and-initialize-a-project"></a>Proje oluşturma ve başlatma

1. Yeni bir JavaScript dosyası, sık kullandığınız IDE veya düzenleyici oluşturun. Katılık ayarlayın ve https gerektirir. Ardından değişkenlerinin API uç noktanın ana bilgisayar, yol ve abonelik anahtarınızı oluşturun.

    ```javascript
    'use strict';
    let https = require ('https');
    
    let host = 'api.cognitive.microsoft.com';
    let path = '/bing/v7.0/spellcheck';
    let key = 'ENTER KEY HERE';
    ```

2. Pazarınızın, yazım denetimi modu ve metnin kontrol etmek istediğiniz değişkenleri oluşturun. Bir dize ekler oluşturup `?mkt=` parametresi, pazarlama ve `&mode=` modunuzu için.

    ```javascript
    let mkt = "en-US";
    let mode = "proof";
    let text = "Hollo, wrld!";
    let query_string = "?mkt=" + mkt + "&mode=" + mode;
    ```

## <a name="create-the-request-parameters"></a>İstek parametreleri oluşturma

İstek parametrelerinizi ile yeni bir nesne oluşturarak oluşturma bir `POST` yöntemi. Bitiş noktası yolu ve sorgu dizesi eklenerek yolunuza ekleyin. Abonelik anahtarınızı ekleme `Ocp-Apim-Subscription-Key` başlığı.

```javascript
let request_params = {
   method : 'POST',
   hostname : host,
   path : path + query_string,
   headers : {
   'Content-Type' : 'application/x-www-form-urlencoded',
   'Content-Length' : text.length + 5,
      'Ocp-Apim-Subscription-Key' : key,
   }
};
```

## <a name="create-a-response-handler"></a>Yanıt işleyici oluşturma

Çağrılan bir işlev oluşturma `response_handler` API'sinden bir JSON yanıtı alıp yazdırabilirsiniz. Yanıt gövdesi için bir değişken oluşturun. Yanıt ekleme yaparken bir `data` bayrağı alındığında, kullanarak `response.on()`. Olduğunda bir `end` bayrağı alındığında, JSON gövde konsola yazdırma.

```javascript
let response_handler = function (response) {
    let body = '';
    response.on ('data', function (d) {
        body += d;
    });
    response.on ('end', function () {
        console.log (body);
    });
    response.on ('error', function (e) {
        console.log ('Error: ' + e.message);
    });
};
```

## <a name="send-the-request"></a>İsteği Gönder

Kullanarak API çağrısı `https.request()` istek parametrelerini ve yanıt işleyici. API için metninizi yazın ve istek daha sonra bitmelidir.

```javascript
let req = https.request (request_params, response_handler);
req.write ("text=" + text);
req.end ();
```

## <a name="example-json-response"></a>Örnek JSON yanıtı

Başarılı yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür: 

```json
{
   "_type": "SpellCheck",
   "flaggedTokens": [
      {
         "offset": 0,
         "token": "Hollo",
         "type": "UnknownToken",
         "suggestions": [
            {
               "suggestion": "Hello",
               "score": 0.9115257530801
            },
            {
               "suggestion": "Hollow",
               "score": 0.858039839213461
            },
            {
               "suggestion": "Hallo",
               "score": 0.597385084464481
            }
         ]
      },
      {
         "offset": 7,
         "token": "wrld",
         "type": "UnknownToken",
         "suggestions": [
            {
               "suggestion": "world",
               "score": 0.9115257530801
            }
         ]
      }
   ]
}
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bir tek sayfalı web uygulaması oluşturma](../tutorials/spellcheck.md)

- [Bing yazım denetimi API'si nedir?](../overview.md)
- [Bing Yazım Denetimi API’si v7 Başvurusu](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v7-reference)
