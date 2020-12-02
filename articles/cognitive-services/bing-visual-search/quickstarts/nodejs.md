---
title: 'Hızlı başlangıç: REST API ve Node.js kullanarak görüntü öngörülerini alın Bing Görsel Arama'
titleSuffix: Azure Cognitive Services
description: Bing Görsel Arama API'si ve Node.js kullanarak bir görüntüyü karşıya yüklemeyi ve sonra görüntüyle ilgili öngörüleri almayı öğrenin.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: quickstart
ms.date: 05/22/2020
ms.author: scottwhi
ms.custom: devx-track-js
ms.openlocfilehash: 94a642886b626eb84da3a2d02684b5dd170dcbb1
ms.sourcegitcommit: d60976768dec91724d94430fb6fc9498fdc1db37
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/02/2020
ms.locfileid: "96499076"
---
# <a name="quickstart-get-image-insights-using-the-bing-visual-search-rest-api-and-nodejs"></a>Hızlı başlangıç: Bing Görsel Arama REST API ve Node.js kullanarak görüntü öngörüleri alın

> [!WARNING]
> Bing Arama API'leri bilişsel hizmetlerden Bing Arama hizmetlere taşınıyor. **30 ekim 2020 ' den** itibaren, [burada](/bing/search-apis/bing-web-search/create-bing-search-service-resource)belgelenen işlem sonrasında Bing arama yeni örneklerin sağlanması gerekir.
> Bilişsel hizmetler kullanılarak sağlanan Bing Arama API'leri, sonraki üç yıl boyunca veya Kurumsal Anlaşma sonuna kadar, hangisi önce gerçekleşene kadar desteklenecektir.
> Geçiş yönergeleri için bkz. [Bing arama Services](/bing/search-apis/bing-web-search/create-bing-search-service-resource).

Bing Görsel Arama API'si ilk çağrısını yapmak için bu hızlı başlangıcı kullanın. Bu basit JavaScript uygulaması, API 'ye bir görüntü yükler ve onunla ilgili olarak döndürülen bilgileri görüntüler. Bu uygulama JavaScript 'e yazılsa da, API çoğu programlama dili ile uyumlu olan yeniden yazılmış bir Web hizmetidir.

## <a name="prerequisites"></a>Önkoşullar

* [Node.js](https://nodejs.org/en/download/)
* JavaScript için Istek modülü. `npm install request`Modülünü yüklemek için komutunu kullanabilirsiniz.
* Form veri modülü. `npm install form-data`Modülünü yüklemek için komutunu kullanabilirsiniz. 

[!INCLUDE [cognitive-services-bing-visual-search-signup-requirements](../../../../includes/cognitive-services-bing-visual-search-signup-requirements.md)]

## <a name="initialize-the-application"></a>Uygulamayı başlatma

1. En sevdiğiniz IDE veya düzenleyicide bir JavaScript dosyası oluşturun ve aşağıdaki gereksinimleri ayarlayın:

    ```javascript
    var request = require('request');
    var FormData = require('form-data');
    var fs = require('fs');
    ```

2. API uç noktanız, abonelik anahtarınız ve görüntünüzün yolu için değişkenler oluşturun. Değer için `baseUri` aşağıdaki kodda genel uç noktasını kullanabilir veya kaynağınız için Azure Portal görüntülenmiş [özel alt etki alanı](../../../cognitive-services/cognitive-services-custom-subdomains.md) uç noktasını kullanabilirsiniz.

    ```javascript
    var baseUri = 'https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch';
    var subscriptionKey = 'your-api-key';
    var imagePath = "path-to-your-image";
    ```

3. `requestCallback()`API 'den yanıtı yazdırmak için adlı bir işlev oluşturun.

    ```javascript
    function requestCallback(err, res, body) {
        console.log(JSON.stringify(JSON.parse(body), null, '  '))
    }
    ```

## <a name="construct-and-send-the-search-request"></a>Arama isteğini oluşturun ve gönderin

1. Yerel bir görüntüyü karşıya yüklediğinizde, form verileri `Content-Disposition` üstbilgiyi içermelidir. `name`Parametresini "Image" olarak ayarlayın ve bu `filename` parametreyi görüntünüzün dosya adına ayarlayın. Formun içeriği görüntünün ikili verilerini içerir. Karşıya yükleyebileceğiniz en büyük görüntü boyutu 1 MB 'tır.

   ```
   --boundary_1234-abcd
   Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"

   ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

   --boundary_1234-abcd--
   ```

2. İle yeni bir `FormData` nesne oluşturun `FormData()` ve kullanarak görüntü yolunu ekleyin `fs.createReadStream()` .
    
    ```javascript
    var form = new FormData();
    form.append("image", fs.createReadStream(imagePath));
    ```

3. Görüntüyü karşıya yüklemek için istek kitaplığını kullanın ve `requestCallback()` yanıtı yazdırmak için çağrısı yapın. Abonelik anahtarınızı istek üstbilgisine ekleyin.

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
> [Görsel Arama tek sayfalı Web uygulaması oluşturma](../tutorial-bing-visual-search-single-page-app.md)