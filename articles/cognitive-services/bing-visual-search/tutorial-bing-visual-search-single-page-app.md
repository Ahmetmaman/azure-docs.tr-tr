---
title: " Tek sayfalı Web uygulaması oluşturma-Bing Görsel Arama"
titleSuffix: Azure Cognitive Services
description: Bing Görsel Arama API'si tek sayfalı bir Web uygulamasıyla tümleştirmeyi öğrenin.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: tutorial
ms.date: 03/27/2020
ms.author: aahi
ms.custom: devx-track-js
ms.openlocfilehash: b1ca88cd654b7bf373bee60b1e9b7a2e7a129fa2
ms.sourcegitcommit: d60976768dec91724d94430fb6fc9498fdc1db37
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/02/2020
ms.locfileid: "96499059"
---
# <a name="tutorial-create-a-visual-search-single-page-web-app"></a>Öğretici: Görsel Arama tek sayfalı Web uygulaması oluşturma

> [!WARNING]
> Bing Arama API'leri bilişsel hizmetlerden Bing Arama hizmetlere taşınıyor. **30 ekim 2020 ' den** itibaren, [burada](/bing/search-apis/bing-web-search/create-bing-search-service-resource)belgelenen işlem sonrasında Bing arama yeni örneklerin sağlanması gerekir.
> Bilişsel hizmetler kullanılarak sağlanan Bing Arama API'leri, sonraki üç yıl boyunca veya Kurumsal Anlaşma sonuna kadar, hangisi önce gerçekleşene kadar desteklenecektir.
> Geçiş yönergeleri için bkz. [Bing arama Services](/bing/search-apis/bing-web-search/create-bing-search-service-resource).

Bing Görsel Arama API'si bir görüntü için öngörüleri döndürür. Bir görüntüyü karşıya yükleyebilir veya bir URL sağlayabilirsiniz. Öngörüler görsel olarak benzer görüntüler, alışveriş kaynakları, görüntü içeren Web sayfaları ve daha fazlasını içerir. Bing Görsel Arama API'si tarafından döndürülen Öngörüler, Bing.com/images 'de gösterilenlere benzerdir.

Bu öğreticide, Bing Resim Arama API'si için tek sayfalı bir Web uygulamasının nasıl genişletileceği açıklanmaktadır. Bu öğreticiyi görüntülemek veya burada kullanılan kaynak kodu almak için bkz. [öğretici: Bing resim arama API'si için tek sayfalı uygulama oluşturma](../Bing-Image-Search/tutorial-bing-image-search-single-page-app.md).

Bu uygulama için tam kaynak kodu (Bing Görsel Arama API'si kullanmak üzere genişlettikten sonra), [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/Tutorials/Bing-Visual-Search/BingVisualSearchApp.html)' da kullanılabilir.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [cognitive-services-bing-visual-search-signup-requirements](../../../includes/cognitive-services-bing-visual-search-signup-requirements.md)]

## <a name="call-the-bing-visual-search-api-and-handle-the-response"></a>Bing Görsel Arama API'si çağırın ve yanıtı işleyin

Bing Resim Arama öğreticisini düzenleyin ve aşağıdaki kodu `<script>` öğenin sonuna (ve kapanış `</script>` etiketinden önce) ekleyin. Aşağıdaki kod, API 'den bir görsel arama yanıtı işler, sonuçlar boyunca yinelenir ve bunları görüntüler:

``` javascript
function handleVisualSearchResponse(){
    if(this.status !== 200){
        console.log(this.responseText);
        return;
    }
    let json = this.responseText;
    let response = JSON.parse(json);
    for (let i =0; i < response.tags.length; i++) {
        let tag = response.tags[i];
        if (tag.displayName === '') {
            for (let j = 0; j < tag.actions.length; j++) {
                let action = tag.actions[j];
                if (action.actionType === 'VisualSearch') {
                    let html = '';
                    for (let k = 0; k < action.data.value.length; k++) {
                        let item = action.data.value[k];
                        let height = 120;
                        let width = Math.max(Math.round(height * item.thumbnail.width / item.thumbnail.height), 120);
                        html += "<img src='"+ item.thumbnailUrl + "&h=" + height + "&w=" + width + "' height=" + height + " width=" + width + "'>";
                    }
                    showDiv("insights", html);
                    window.scrollTo({top: document.getElementById("insights").getBoundingClientRect().top, behavior: "smooth"});
                }
            }
        }
    }
}
```

Aşağıdaki kod, çağırmak için bir olay dinleyicisi kullanarak API 'ye bir arama isteği gönderir `handleVisualSearchResponse()` :

```javascript
function bingVisualSearch(insightsToken){
    let visualSearchBaseURL = 'https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch',
        boundary = 'boundary_ABC123DEF456',
        startBoundary = '--' + boundary,
        endBoundary = '--' + boundary + '--',
        newLine = "\r\n",
        bodyHeader = 'Content-Disposition: form-data; name="knowledgeRequest"' + newLine + newLine;

    let postBody = {
        imageInfo: {
            imageInsightsToken: insightsToken
        }
    }
    let requestBody = startBoundary + newLine;
    requestBody += bodyHeader;
    requestBody += JSON.stringify(postBody) + newLine + newLine;
    requestBody += endBoundary + newLine;

    let request = new XMLHttpRequest();

    try {
        request.open("POST", visualSearchBaseURL);
    } 
    catch (e) {
        renderErrorMessage("Error performing visual search: " + e.message);
    }
    request.setRequestHeader("Ocp-Apim-Subscription-Key", getSubscriptionKey());
    request.setRequestHeader("Content-Type", "multipart/form-data; boundary=" + boundary);
    request.addEventListener("load", handleVisualSearchResponse);
    request.send(requestBody);
}
```

## <a name="capture-insights-token"></a>İçgörü elde etme belirteci

Nesnesine aşağıdaki kodu ekleyin `searchItemsRenderer` . Bu kod, tıklandığında `bingVisualSearch` işlevini çağıran bir **benzerlerini bulma** bağlantısı ekler. İşlevi `imageInsightsToken` bir bağımsız değişken olarak alır.

``` javascript
html.push("<a href='javascript:bingVisualSearch(\"" + item.imageInsightsToken + "\");'>find similar</a><br>");
```

## <a name="display-similar-images"></a>Benzer resimler görüntüleme

Aşağıdaki HTML kodunu 601. satıra ekleyin. Bu biçimlendirme kodu, Bing Görsel Arama API'si çağrısının sonuçlarını göstermek için bir öğesi ekler:

``` html
<div id="insights">
    <h3><a href="#" onclick="return toggleDisplay('_insights')">Similar</a></h3>
    <div id="_insights" style="display: none"></div>
</div>
```

Yeni JavaScript kodu ve HTML öğeleri yerleştirildikten sonra arama sonuçları bir **benzerlerini bul** bağlantısıyla görüntülenir. **Benzerleri** bölümünü seçtiğinize benzer görüntülerle doldurmak için bağlantıya tıklayın. Görüntüleri göstermek için **Benzerleri** bölümünü genişletmeniz gerekebilir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: C için Bing Görsel Arama SDK ile görüntü kırpma #](tutorial-visual-search-crop-area-results.md)