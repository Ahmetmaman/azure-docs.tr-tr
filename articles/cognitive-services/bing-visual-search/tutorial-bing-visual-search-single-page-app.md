---
title: " Bing görsel arama - tek sayfa Web uygulaması derleme"
titleSuffix: Azure Cognitive Services
description: Bing görsel arama API'sine bir tek sayfalı Web uygulamasıyla tümleştirmeyi öğrenin.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: article
ms.date: 10/04/2017
ms.author: aahi
ms.openlocfilehash: cf8525d4cc829805532210bf09e9ea9da240405d
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55857758"
---
# <a name="create-a-visual-search-single-page-web-app"></a>Görsel arama tek sayfa web uygulaması oluşturma 

Bing Görsel Arama API'si, Bing.com/images sayfasında gösterilen resim ayrıntılarına benzer bir deneyim sağlar. Görsel Arama ile bir görüntü belirtebilir ve görüntü hakkında görsel olarak benzer görüntüler, alışveriş kaynakları ve görüntüyü içeren web sayfaları gibi içgörüler alabilirsiniz. 

Bu makalede, Bing resim arama API'si için bir tek sayfalı web uygulamasını genişletmek açıklanmaktadır. Bu öğreticiyi görüntüleyin ya da burada kullanılan kaynak kodu alma hakkında bilgi için bkz: [Öğreticisi: Bing resim arama API'si için bir tek sayfalı uygulama oluşturma](../Bing-Image-Search/tutorial-bing-image-search-single-page-app.md). 

(Bing görsel arama API'sine kullanacak şekilde genişlettikten sonra), bu uygulama için tam kaynak kodunu edinilebilir [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/Tutorials/Bing-Visual-Search/BingVisualSearchApp.html).

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [cognitive-services-bing-visual-search-signup-requirements](../../../includes/cognitive-services-bing-visual-search-signup-requirements.md)]

## <a name="call-the-bing-visual-search-api-and-handle-the-response"></a>Bing görsel arama API'sine çağrı ve yanıt işleme

Bing resim arama Öğreticisi düzenleyin ve sonuna aşağıdaki kodu ekleyin `<script>` öğesi (ve kapatmadan önce `</script>` etiketi). Aşağıdaki kod, görsel arama API'si yanıtı işler, sonuçları yinelenir ve bunları görüntüler.

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

Aşağıdaki kodu çağırmak için bir olay dinleyicisi kullanarak bir arama isteği API'sine gönderir `handleVisualSearchResponse()`.


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

Aşağıdaki kodu ekleyin `searchItemsRenderer` nesne. Bu kod, tıklandığında `bingVisualSearch` işlevini çağıran bir **benzerlerini bulma** bağlantısı ekler. İşlev imageInsightsToken değerini bağımsız değişken olarak alır.

``` javascript
html.push("<a href='javascript:bingVisualSearch(\"" + item.imageInsightsToken + "\");'>find similar</a><br>");
```

## <a name="display-similar-images"></a>Benzer resimler görüntüleme

Aşağıdaki HTML kodunu 601. satıra ekleyin. Bu işaretleme kodu Bing Görsel Arama API'si çağrısının sonuçlarını görüntülemek için kullanılan bir öğe ekler.

``` html
<div id="insights">
    <h3><a href="#" onclick="return toggleDisplay('_insights')">Similar</a></h3>
    <div id="_insights" style="display: none"></div>
</div>
```

Yeni JavaScript kodu ve HTML öğeleri yerleştirildikten sonra arama sonuçları bir **benzerlerini bul** bağlantısıyla görüntülenir. **Benzerleri** bölümünü seçtiğinize benzer görüntülerle doldurmak için bağlantıya tıklayın. Görüntüleri göstermek için **Benzerleri** bölümünü genişletmeniz gerekebilir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Kırpma ve bir görüntüyü karşıya yükleme](tutorial-visual-search-crop-area-results.md)
