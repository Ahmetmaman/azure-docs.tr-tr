---
title: Arama sonuçlarını filtreleme-Bing Web Araması API'si
titleSuffix: Azure Cognitive Services
description: "' ResponseFilter ' sorgu parametresini kullanarak, Bing 'in yanıt (örneğin, görüntüler, videolar ve Haberler) içerdiği yanıt türlerini filtreleyebilirsiniz."
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.assetid: 8B837DC2-70F1-41C7-9496-11EDFD1A888D
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: conceptual
ms.date: 07/08/2019
ms.author: scottwhi
ms.openlocfilehash: 571314009b6f58e5c2ab6aac02cfebc82c53f42f
ms.sourcegitcommit: 9eda79ea41c60d58a4ceab63d424d6866b38b82d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2020
ms.locfileid: "96351870"
---
# <a name="filtering-the-answers-that-the-search-response-includes"></a>Arama yanıtının içerdiği yanıtları filtreleme  

> [!WARNING]
> Bing Arama API'leri bilişsel hizmetlerden Bing Arama hizmetlere taşınıyor. **30 ekim 2020 ' den** itibaren, [burada](/bing/search-apis/bing-web-search/create-bing-search-service-resource)belgelenen işlem sonrasında Bing arama yeni örneklerin sağlanması gerekir.
> Bilişsel hizmetler kullanılarak sağlanan Bing Arama API'leri, sonraki üç yıl boyunca veya Kurumsal Anlaşma sonuna kadar, hangisi önce gerçekleşene kadar desteklenecektir.
> Geçiş yönergeleri için bkz. [Bing arama Services](/bing/search-apis/bing-web-search/create-bing-search-service-resource).

Web 'i sorguladığınızda, Bing arama için bulduğu tüm ilgili içeriği döndürür. Örneğin, arama sorgusu "karmaşık + Dinghies" ise, yanıt aşağıdaki yanıtları içerebilir:

```json
{
    "_type" : "SearchResponse",
    "webPages" : {
        "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=3A43C...",
        "totalEstimatedMatches" : 262000,
        "value" : [...]
    },
    "images" : {
        "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#Images",
        "readLink" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/images\/search?q=sail...",
        "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=3A43CA5CA6464E5D...",
        "isFamilyFriendly" : true,
        "value" : [...]
    },
    "rankingResponse" : {
        "mainline" : {
            "items" : [...]
        }
    }
}    
```

## <a name="query-parameters"></a>Sorgu parametreleri

Bing tarafından döndürülen yanıtları filtrelemek için API 'yi çağırırken aşağıdaki sorgu parametrelerini kullanın.  

### <a name="responsefilter"></a>ResponseFilter

Bir yanıtın virgülle ayrılmış listesi olan [Responsefilter](/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference#responsefilter) sorgu parametresini kullanarak, Bing 'in yanıt (örneğin, görüntüler, videolar ve Haberler) içerdiği yanıt türlerini filtreleyebilirsiniz. Bir yanıt, Bing buna ait ilgili içeriği bulursa yanıta dahil edilir. 

Görüntüler gibi yanıtlardan belirli yanıtları dışlamak için, `-` Yanıt türüne bir karakter ekleyin. Örneğin:

```
&responseFilter=-images,-videos
```

Aşağıda, `responseFilter` yelbilerin görüntülerini, Videoları ve haberleri istemek için nasıl kullanılacağı gösterilmektedir. Sorgu dizesini kodlarken, virgüller% 2C olarak değişir.  

```  
GET https://api.cognitive.microsoft.com/bing/v7.0/search?q=sailing+dinghies&responseFilter=images%2Cvideos%2Cnews&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-Search-ClientIP: 999.999.999.999  
X-Search-Location:  47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```  

Aşağıda, bir önceki sorgunun yanıtı gösterilmektedir. Bing ilgili video ve haber sonuçları bulmadığından, yanıt bunları içermez.

```json
{
    "_type" : "SearchResponse",
    "images" : {
        "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#Images",
        "readLink" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/images\/search?q=sail...",
        "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=3AD78B183C56456C...",
        "isFamilyFriendly" : true,
        "value" : [...]
    },
    "rankingResponse" : {
        "mainline" : {
            "items" : [{
                "answerType" : "Images",
                "value" : {
                    "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#Images"
                }
            }]
        }
    }
}
```

Bing, önceki yanıtta video ve haber sonuçları döndürmese de, video ve haber içeriği yok demektir. Yalnızca sayfanın bu sayfada yer almamasıdır. Bununla birlikte, daha fazla sonuç [elde ederseniz,](./paging-search-results.md) sonraki sayfalar büyük olasılıkla bunları içerebilir. Ayrıca, [VIDEO arama API](../bing-video-search/overview.md) ve [Haber Arama API](../bing-news-search/search-the-web.md) uç noktalarını doğrudan çağırırsanız, yanıt muhtemelen sonuçlar içerebilir.

`responseFilter`Tek BIR API 'den sonuçları almak için kullanmanız önerilmez. Tek bir Bing API 'den içerik istiyorsanız, bu API 'YI doğrudan çağırın. Örneğin, yalnızca görüntüleri almak için Resim Arama API uç noktasına `https://api.cognitive.microsoft.com/bing/v7.0/images/search` veya diğer [görüntü](/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#endpoints) uç noktalarından birine bir istek gönderin. Tek bir API 'nin çağrılması yalnızca performans nedenleriyle değil önemlidir, ancak içeriğe özgü API 'Ler daha zengin sonuçlar sunar. Örneğin, sonuçları filtrelemek için Web Araması API 'SI tarafından kullanılamayan filtreler kullanabilirsiniz.  

### <a name="site"></a>Site

Belirli bir etki alanındaki arama sonuçlarını almak için sorgu `site:` dizesine sorgu parametresini ekleyin.  

```
https://api.cognitive.microsoft.com/bing/v7.0/search?q=sailing+dinghies+site:contososailing.com&mkt=en-us
```

> [!NOTE]
> Sorguya bağlı olarak, `site:` sorgu işlecini kullanırsanız, [Güvenli Arama](/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference#safesearch) ayarından bağımsız olarak, yanıtın yetişkinlere yönelik içerik içerebileceği ihtimaline sahip olabilirsiniz. `site:` işlecini yalnızca sitenin içeriği hakkında bilgi sahibiyseniz ve senaryonuz, yetişkinlere yönelik içeriğin mevcut olma ihtimalini destekliyorsa kullanın.

### <a name="freshness"></a>Güncellik

Web yanıt sonuçlarını, Bing 'in belirli bir dönemde bulduğu web sayfalarıyla sınırlamak için, [yenilik](/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference#freshness) sorgu parametresini şu büyük/küçük harf duyarsız değerlerden birine ayarlayın:

* `Day` — Son 24 saat içinde Bing tarafından bulunan Web sayfalarını döndürün
* `Week` — Son 7 gün içinde Bing tarafından bulunan Web sayfalarını döndürün
* `Month` — Son 30 gün içinde bulunan Web sayfalarını geri döndür

Bu parametreyi Ayrıca formunda özel bir tarih aralığına da ayarlayabilirsiniz `YYYY-MM-DD..YYYY-MM-DD` . 

`https://<host>/bing/v7.0/search?q=ipad+updates&freshness=2019-02-01..2019-05-30`

Sonuçları tek bir tarihle sınırlamak için, yenilik parametresini belirli bir tarih olarak ayarlayın:

`https://<host>/bing/v7.0/search?q=ipad+updates&freshness=2019-02-04`

Sonuçlar, Bing filtre ölçütlerinizle eşleşen Web sayfası sayısı istediğiniz Web sayfası sayısından (veya Bing tarafından döndürülen varsayılan sayı) daha az olduğunda, belirtilen dönem dışında kalan Web sayfalarını içerebilir.

## <a name="limiting-the-number-of-answers-in-the-response"></a>Yanıttaki yanıt sayısını sınırlandırma

Bing, JSON yanıtında birden çok yanıt türü döndürebilir. Örneğin, *yelkenler + Dinghies*'yi sorgulayıp Bing,,, `webpages` `images` ve döndürebilir `videos` `relatedSearches` .

```json
{
    "_type" : "SearchResponse",
    "queryContext" : {
        "originalQuery" : "sailing dinghies"
    },
    "webPages" : {...},
    "images" : {...},
    "relatedSearches" : {...},
    "videos" : {...},
    "rankingResponse" : {...}
}
```

Bing 'in döndürdüğü yanıt sayısını en yüksek iki yanıt (Web sayfası ve resim) ile sınırlamak için [answerCount](/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference#answercount) sorgu parametresini 2 olarak ayarlayın.

```  
GET https://api.cognitive.microsoft.com/bing/v7.0/search?q=sailing+dinghies&answerCount=2&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-Search-ClientIP: 999.999.999.999  
X-Search-Location:  47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```  

Yanıt yalnızca ve içerir `webPages` `images` .

```json
{
    "_type" : "SearchResponse",
    "queryContext" : {
        "originalQuery" : "sailing dinghies"
    },
    "webPages" : {...},
    "images" : {...},
    "rankingResponse" : {...}
}
```

`responseFilter`Sorgu parametresini önceki sorguya ekler ve bunları Web sayfası ve haberlere ayarlarsanız, yanıt yalnızca Web sayfalarını içerir çünkü Haberler derecelendirilir.

```json
{
    "_type" : "SearchResponse",
    "queryContext" : {
        "originalQuery" : "sailing dinghies"
    },
    "webPages" : {...},
    "rankingResponse" : {...}
}
```

## <a name="promoting-answers-that-are-not-ranked"></a>Derecelendirildi yanıtları yükseltme

Bing 'in bir sorgu için döndürdüğü en üst dereceli yanıtlar, Web sayfaları, görüntüler, videolar ve Relatedaramalarıdır. bu yanıtlar, yanıt bu yanıtları içerir. [AnswerCount](/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference#answercount) öğesini iki (2) olarak ayarlarsanız Bing, en üst iki dereceli yanıtı döndürür: Web sayfaları ve görüntüler. Bing 'in yanıta görüntü ve video içermesini istiyorsanız, [Yükselt](/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference#promote) sorgu parametresini belirtin ve bunları görüntüler ve videolar olarak ayarlayın.

```  
GET https://api.cognitive.microsoft.com/bing/v7.0/search?q=sailing+dinghies&answerCount=2&promote=images%2Cvideos&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-Search-ClientIP: 999.999.999.999  
X-Search-Location:  47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```  

Yukarıdaki isteğin yanıtı aşağıda verilmiştir. Bing, en iyi iki yanıtı, Web sayfalarını ve resimleri geri döndürür ve videoları yanıta yükseltir.

```json
{
    "_type" : "SearchResponse",
    "queryContext" : {
        "originalQuery" : "sailiing dinghies"
    },
    "webPages" : {...},
    "images" : {...},
    "videos" : {...},
    "rankingResponse" : {...}
}
```

`promote`Haberler olarak ayarlarsanız, bir derecelendirilmiş yanıt olmadığından, yanıt &mdash; yalnızca derecelendirilmiş yanıtları yükseltebileceğiniz için haber yanıtı dahil değildir.

Yükseltmek istediğiniz yanıtlar sınıra göre sayılmaz `answerCount` . Örneğin, derecelendirilen yanıtlar Haberler, Resimler ve videolar ise ve `answerCount` 1 ve `promote` haberlere ayarlarsanız, yanıt Haberler ve görüntüler içerir. Ya da, derecelendirilen yanıtlar videolar, görüntüler ve Haberler ise, yanıt Videoları ve haberleri içerir.

`promote`Yalnızca `answerCount` sorgu parametresini belirtirseniz kullanabilirsiniz.