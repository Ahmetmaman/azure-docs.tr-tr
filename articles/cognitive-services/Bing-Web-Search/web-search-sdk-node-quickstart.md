---
title: "Hızlı başlangıç: Node.js için Bing Web Araması SDK'sını kullanma"
titleSuffix: Azure Cognitive Services
description: Bing Web Araması SDK'sı, Bing Web Araması özelliklerini Node.js uygulamanızla tümleştirmeyi kolaylaştırır. Bu hızlı başlangıçta istemci başlatmayı, istek göndermeyi ve yanıtı yazdırmayı öğreneceksiniz.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: quickstart
ms.date: 08/16/2018
ms.author: erhopf
ms.openlocfilehash: b27c11a69c7d16f38c2448d380b611940078c501
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46121938"
---
# <a name="quickstart-use-the-bing-web-search-sdk-for-nodejs"></a>Hızlı başlangıç: Node.js için Bing Web Araması SDK'sını kullanma

Bing Web Araması SDK'sı, Bing Web Araması özelliklerini Node.js uygulamanızla tümleştirmeyi kolaylaştırır. Bu hızlı başlangıçta istemci başlatmayı, istek göndermeyi ve yanıtı yazdırmayı öğreneceksiniz.

Kodu hemen görmek istiyor musunuz? GitHub'daki [Node.js için Bing Web Araması SDK'sı örneklerini](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples) inceleyebilirsiniz.

[!INCLUDE [bing-web-search-quickstart-signup](../../../includes/bing-web-search-quickstart-signup.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı çalıştırmak için aşağıdakilere ihtiyacınız olacaktır:

* [Node.js 6](https://nodejs.org/en/download/) veya üzeri
* Abonelik anahtarı  

## <a name="set-up-your-development-environment"></a>Geliştirme ortamınızı kurma

Node.js projemiz için geliştirme ortamını ayarlayarak başlayalım.

1. Projeniz için yeni bir dizin oluşturun:

    ```console
    mkdir YOUR_PROJECT
    ```

2. Yeni bir paket dosyası oluşturun:

    ```console
    cd YOUR_PROJECT
    npm init
    ```

3. Şimdi birkaç Azure modülü yükleyip `package.json` içine ekleyelim:

    ```console
    npm install --save azure-cognitiveservices-websearch
    npm install --save ms-rest-azure
    ```

## <a name="create-a-project-and-declare-required-modules"></a>Bir proje oluşturun ve gerekli modülleri bildirin

`package.json` ile aynı dizinde favori IDE ortamınızı veya düzenleyiciyi kullanarak yeni bir Node.js projesi oluşturun. Örneğin: `sample.js`.

Şimdi bu kodu projenize kopyalayın. Önceki bölümde yüklenen modülleri yükler.

```javascript
const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;
const WebSearchAPIClient = require('azure-cognitiveservices-websearch');
```

## <a name="instantiate-the-client"></a>İstemciyi başlatma

Bu kod `azure-cognitiveservices-websearch` modünü kullanarak bir istemci başlatır. Devam etmeden önce Azure hesabınız için geçerli bir abonelik anahtarı girdiğinizden emin olun.

```javascript
let credentials = new CognitiveServicesCredentials('YOUR-ACCESS-KEY');
let webSearchApiClient = new WebSearchAPIClient(credentials);
```

## <a name="make-a-request-and-print-the-results"></a>İstekte bulunma ve sonuçları yazdırma

İstemciyi kullanarak Bing Web Araması'na bir arama sorgusu gönderin. Yanıt `properties` dizisindeki öğeler için sonuç içerirse konsolda `result.value` yazılır.

```javascript
webSearchApiClient.web.search('seahawks').then((result) => {
    let properties = ["images", "webPages", "news", "videos"];
    for (let i = 0; i < properties.length; i++) {
        if (result[properties[i]]) {
            console.log(result[properties[i]].value);
        } else {
            console.log(`No ${properties[i]} data`);
        }
    }
}).catch((err) => {
    throw err;
})
```

## <a name="run-the-program"></a>Programı çalıştırma

Son adım programınızı çalıştırmaktır!

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu projeyi tamamladıktan sonra abonelik anahtarınızı program kodundan kaldırmayı unutmayın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Cognitive Services Node.js SDK'sı örnekleri](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples)

## <a name="see-also"></a>Ayrıca bkz.

* [Azure Node SDK başvurusu](https://docs.microsoft.com/javascript/api/azure-cognitiveservices-websearch/)
