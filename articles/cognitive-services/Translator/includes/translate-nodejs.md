---
author: erhopf
ms.service: cognitive-services
ms.topic: include
ms.date: 08/06/2019
ms.author: erhopf
ms.openlocfilehash: 432084b2b94d233eb58bc2cb1eeea04e88d9776c
ms.sourcegitcommit: 5d6c8231eba03b78277328619b027d6852d57520
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2019
ms.locfileid: "68968688"
---
## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıç şunları gerektirir:

* [Node 8.12.x veya üzeri](https://nodejs.org/en/)
* Translator Metin Çevirisi için Azure abonelik anahtarı

## <a name="create-a-project-and-import-required-modules"></a>Bir proje oluşturun ve gerekli modülleri içeri aktarın

En sevdiğiniz IDE veya düzenleyiciyi kullanarak yeni bir proje oluşturun veya masaüstünüzde adlı `translate-text.js` bir dosya içeren yeni bir klasör oluşturun. Sonra bu kod parçacığını projenize/dosyanıza kopyalayın:

```javascript
const request = require('request');
const uuidv4 = require('uuid/v4');
```

> [!NOTE]
> Bu modülleri kullanmadıysanız, programınızı çalıştırmadan önce bunları yüklemeniz gerekir. Bu paketleri yüklemek için şunu çalıştırın: `npm install request uuidv4`.

Bu modüller HTTP isteği ve `'X-ClientTraceId'` üst bilgisi için benzersiz tanıtıcı oluşturmak için gereklidir.

## <a name="set-the-subscription-key"></a>Abonelik anahtarını ayarlama

Bu kod, `TRANSLATOR_TEXT_KEY` ortam değişkeninden Translator Metin Çevirisi abonelik anahtarınızı okumaya çalışır. Ortam değişkenlerini bilmiyorsanız, `subscriptionKey` öğesini dize olarak ayarlayabilir ve koşul deyimini açıklama satırı yapabilirsiniz.

Bu kodu projenize kopyalayın:

```javascript
/* Checks to see if the subscription key is available
as an environment variable. If you are setting your subscription key as a
string, then comment these lines out.

If you want to set your subscription key as a string, replace the value for
the Ocp-Apim-Subscription-Key header as a string. */
const subscriptionKey = process.env.TRANSLATOR_TEXT_KEY;
if (!subscriptionKey) {
  throw new Error('Environment variable for your subscription key is not set.')
};
```

## <a name="configure-the-request"></a>İsteği yapılandırma

İstek modülü aracılığıyla kullanıma sunulan `request()` yöntemi HTTP yöntemi, URL, istek parametreleri, üst bilgileri ve JSON gövdesi bileşenlerini `options` nesnesi olarak geçirmemizi sağlar. Bu kod parçacığında isteği yapılandıracağız:

>[!NOTE]
> Uç noktalar, rotalar ve istek parametreleri hakkında daha fazla bilgi için bkz [. Translator metin çevirisi API'si 3,0: Çeviri](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-translate)yapın.

```javascript
let options = {
    method: 'POST',
    baseUrl: 'https://api.cognitive.microsofttranslator.com/',
    url: 'translate',
    qs: {
      'api-version': '3.0',
      'to': ['de', 'it']
    },
    headers: {
      'Ocp-Apim-Subscription-Key': subscriptionKey,
      'Content-type': 'application/json',
      'X-ClientTraceId': uuidv4().toString()
    },
    body: [{
          'text': 'Hello World!'
    }],
    json: true,
};
```

Bir isteğin kimliğini doğrulamanın en kolay yolu, abonelik anahtarınızda bir `Ocp-Apim-Subscription-Key` üst bilgisi olarak geçirmektir. Bu örnekte bu yöntem kullanılır. Alternatif olarak, abonelik anahtarınızı bir erişim belirteciyle değiştirebilir ve isteğinizi doğrulamak için erişim belirtecini bir `Authorization` üst bilgisi olarak geçirebilirsiniz.

Bilişsel hizmetler çoklu hizmet aboneliği kullanıyorsanız, istek üst bilgilerine de dahil `Ocp-Apim-Subscription-Region` etmeniz gerekir.

Daha fazla bilgi için bkz. [Kimlik doğrulaması](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication).

## <a name="make-the-request-and-print-the-response"></a>İstekte bulunma ve yanıtı yazdırma

Ardından, `request()` yöntemini kullanarak isteği oluşturacağız. Bu istek ilk bağımsız değişken olarak bir önceki bölümde oluşturduğumuz `options` nesnesini alır ve düzeltilmiş JSON yanıtını yazdırır.

```javascript
request(options, function(err, res, body){
    console.log(JSON.stringify(body, null, 4));
});
```

>[!NOTE]
> Bu örnekte HTTP isteğini `options` nesnesinde tanımlıyoruz. Ancak istek modülü `.post` ve `.get` gibi kullanışlı yöntemleri de destekler. Daha fazla bilgi için bkz. [kullanışlı yöntemler](https://github.com/request/request#convenience-methods).

## <a name="put-it-all-together"></a>Hepsini bir araya getirin

İşte bu kadar! Translator Metin Çevirisi API'sini çağıran ve bir JSON yanıtı döndüren basit bir program oluşturdunuz. Artık programınızı çalıştırmak zamanı geldi:

```console
node translate-text.js
```

Kodunuzu bizimkiyle karşılaştırmak isterseniz, tam örnek kodu [GitHub](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-NodeJS)’da bulabilirsiniz.

## <a name="sample-response"></a>Örnek yanıt

```json
[
    {
        "detectedLanguage": {
            "language": "en",
            "score": 1.0
        },
        "translations": [
            {
                "text": "Hallo Welt!",
                "to": "de"
            },
            {
                "text": "Salve, mondo!",
                "to": "it"
            }
        ]
    }
]
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Abonelik anahtarınızı programınıza sabit kodladıysanız, bu hızlı başlangıcı tamamladıktan sonra abonelik anahtarını kaldırdığınızdan emin olun.

## <a name="next-steps"></a>Sonraki adımlar

Translator Metin Çevirisi API'si ile yapabileceğiniz her şeyi anlamak için API başvurusuna göz atın.

> [!div class="nextstepaction"]
> [API başvurusu](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference)
