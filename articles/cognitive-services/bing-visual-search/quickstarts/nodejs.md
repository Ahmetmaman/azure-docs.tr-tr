---
title: 'Quickstart: REST API ve Node.js kullanarak görüntü öngörüleri alın - Bing Görsel Arama'
titleSuffix: Azure Cognitive Services
description: Bing Görsel Arama API'sine nasıl görüntü yükleyip bu konuda bilgi edineceklerini öğrenin.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: quickstart
ms.date: 12/17/2019
ms.author: scottwhi
ms.openlocfilehash: 373d6fa5402ba703cbebe88ad562974ba97f3391
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75379717"
---
# <a name="quickstart-get-image-insights-using-the-bing-visual-search-rest-api-and-nodejs"></a>Hızlı başlangıç: Bing Visual Search REST API ve Node.js'yi kullanarak görüntü öngörüleri alın

Bing Görsel Arama API'sine ilk aramanızı yapmak ve arama sonuçlarını görüntülemek için bu hızlı başlangıcı kullanın. Bu basit JavaScript uygulaması API'ye bir resim yükler ve bu uygulamayla ilgili döndürülen bilgileri görüntüler. Bu uygulama JavaScript'te yazılı olsa da, API çoğu programlama diliyle uyumlu bir RESTful Web hizmetidir.

## <a name="prerequisites"></a>Ön koşullar

* [Node.js](https://nodejs.org/en/download/)
* JavaScript için İstek modülü. Modülü yüklemek `npm install request` için komutu kullanabilirsiniz.
* Form-veri modülü. Modülü yüklemek `npm install form-data` için komutu kullanabilirsiniz. 

[!INCLUDE [cognitive-services-bing-visual-search-signup-requirements](../../../../includes/cognitive-services-bing-visual-search-signup-requirements.md)]

## <a name="initialize-the-application"></a>Uygulamayı başlatma

1. En sevdiğiniz IDE veya düzenleyicide bir JavaScript dosyası oluşturun ve aşağıdaki gereksinimleri ayarlayın:

    ```javascript
    var request = require('request');
    var FormData = require('form-data');
    var fs = require('fs');
    ```

2. API bitiş noktanız, abonelik anahtarınız ve resminize giden yol için değişkenler oluşturun. `baseUri`aşağıdaki genel bitiş noktası veya kaynağınız için Azure portalında görüntülenen [özel alt etki alanı](../../../cognitive-services/cognitive-services-custom-subdomains.md) bitiş noktası olabilir:

    ```javascript
    var baseUri = 'https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch';
    var subscriptionKey = 'your-api-key';
    var imagePath = "path-to-your-image";
    ```

3. YANıTı API'den yazdırmak için adlandırılmış `requestCallback()` bir işlev oluşturun:

    ```javascript
    function requestCallback(err, res, body) {
        console.log(JSON.stringify(JSON.parse(body), null, '  '))
    }
    ```

## <a name="construct-and-send-the-search-request"></a>Arama isteğini oluşturma ve gönderme

Yerel bir resim yüklerken, form verilerinin üstbilgiiç `Content-Disposition` içermesi gerekir. Parametresini `name` "görüntü" olarak ayarlamanız `filename` gerekir ve parametre herhangi bir dize olarak ayarlanabilir. Formun içeriği görüntünün ikili verilerini içerir. Karşıya yükleyebileceğiniz resim boyutu üst sınırı 1 MB'tır.

```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

1. Kullanarak yeni bir **FormData** nesnesi `FormData()`oluşturun ve resim `fs.createReadStream()`yolunuzu ona ekleyin:
    
    ```javascript
    var form = new FormData();
    form.append("image", fs.createReadStream(imagePath));
    ```

2. Görüntüyü yüklemek için istek kitaplığını `requestCallback()` kullanın ve yanıtı yazdırmak için arayın. Abonelik anahtarınızı istek üstbilgisine eklediğinizden emin olun:

    ```javascript
    form.getLength(function(err, length){
      if (err) {
        return requestCallback(err);
      }
      var r = request.post(baseUri, requestCallback);
      r._form = form; 
      r.setHeader('Ocp-Apim-Subscription-Key', subscriptionKey);
    });
    ```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Görsel Arama tek sayfalık web uygulaması oluşturma](../tutorial-bing-visual-search-single-page-app.md)
