---
title: Bing Visual arama API için JavaScript hızlı başlangıç | Microsoft Docs
titleSuffix: Bing Web Search APIs - Cognitive Services
description: Bing Visual arama API görüntüyü karşıya yükleme ve görüntü ile ilgili Öngörüler ulaşırsınız gösterilmektedir.
services: cognitive-services
author: swhite-msft
manager: rosh
ms.service: cognitive-services
ms.technology: bing-visual-search
ms.topic: article
ms.date: 5/16/2018
ms.author: scottwhi
ms.openlocfilehash: dd28c829d8d24980a746244dc6aca880d2d69224
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355091"
---
# <a name="your-first-bing-visual-search-query-in-javascript"></a>JavaScript, ilk Bing Visual arama sorgusu

Bing Visual arama API sağlayan bir görüntü ile ilgili bilgileri döndürür. Belirteç, ya da görüntüyü karşıya yükleyerek bir Öngörüler resmin URL'sini kullanarak görüntü sağlayabilir. Bu seçenekler hakkında daha fazla bilgi için bkz: [Bing Visual arama API nedir?](../overview.md) Bu makalede, görüntüyü karşıya gösterilmektedir. Görüntüyü karşıya burada iyi bilinen bir yer işareti resmini alın ve ilgili bilgileri dönmek mobil senaryolarda yararlı olabilir. Örneğin, Öngörüler yer işareti hakkında trivia içerebilir. 

Yerel görüntü yüklerseniz, aşağıdaki form verilerini POST gövdesinde içermelidir gösterir. Form verileri içerik düzeni üstbilgisini eklemeniz gerekir. Kendi `name` parametresini ayarlayın, görüntü"için" ve `filename` parametresi için herhangi bir dize ayarlanmış olabilir. Form içeriğini ikili görüntünün olur. Karşıya yükleme en büyük görüntü boyutu 1 MB'tır. 

```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

Bu makalede bir Bing Visual arama API isteği gönderir ve JSON arama sonuçlarını görüntüleyen basit bir konsol uygulaması içerir. Bu uygulama, JavaScript'te yazılmış olsa da, HTTP isteği yapmak ve JSON ayrıştırma programlama dili ile uyumlu bir RESTful Web hizmeti API'dir. 

## <a name="prerequisites"></a>Önkoşullar

Gereksinim duyduğunuz [Node.js 6](https://nodejs.org/en/download/) bu kodu çalıştırmak için.

Bu Hızlı Başlangıç için kullandığınız bir [ücretsiz deneme sürümü](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) abonelik anahtar veya Ücretli abonelik anahtarı.

## <a name="running-the-application"></a>Uygulamayı çalıştırma

Node.js içinde FormData kullanarak ileti göndermek nasıl gösterir.

Bu uygulamayı çalıştırmak için aşağıdaki adımları izleyin:

1. Projeniz için bir klasör oluşturun (veya sık kullanılan IDE veya düzenleyicisi kullanın).
2. Bir komut istemi veya terminal, az önce oluşturduğunuz klasöre gidin.
3. İstek modüllerini yükleyin:  
  ```  
  npm install request  
  ```  
3. Form verileri modüllerini yükleyin:  
  ```  
  npm install form-data  
  ```  
4. GetVisualInsights.js adlı bir dosya oluşturun ve aşağıdaki kodu ekleyin.
5. Değiştir `subscriptionKey` abonelik anahtarınızı değerle.
6. Değiştir `imagePath` karşıya yüklemek için resminin yolunu içeren değer.
7. Programını çalıştırın.  
  ```
  node GetVisualInsights.js
  ```

```javascript
var request = require('request');
var FormData = require('form-data');
var fs = require('fs');

var baseUri = 'https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch';
var subscriptionKey = '<yoursubscriptionkeygoeshere>';
var imagePath = "<pathtoyourimagegoeshere>";

var form = new FormData();
form.append("image", fs.createReadStream(imagePath));

form.getLength(function(err, length){
  if (err) {
    return requestCallback(err);
  }

  var r = request.post(baseUri, requestCallback);
  r._form = form; 
  r.setHeader('Ocp-Apim-Subscription-Key', subscriptionKey);
});

function requestCallback(err, res, body) {
    console.log(JSON.stringify(JSON.parse(body), null, '  '))
}
```


## <a name="next-steps"></a>Sonraki adımlar

[Öngörüler belirteci kullanarak bir görüntü ile ilgili Öngörüler alın](../use-insights-token.md)  
[Bing Visual arama tek sayfa uygulaması Öğreticisi](../tutorial-bing-visual-search-single-page-app.md)  
[Bing Visual arama genel bakış](../overview.md)  
[Deneyin](https://aka.ms/bingvisualsearchtryforfree)  
[Ücretsiz deneme erişim anahtarı alma](https://azure.microsoft.com/try/cognitive-services/?api=bing-visual-search-api)  
[Bing Visual arama API Başvurusu](https://aka.ms/bingvisualsearchreferencedoc)
