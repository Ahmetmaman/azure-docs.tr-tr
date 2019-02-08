---
title: Bing varlık arama API'si ile varlıkları Ara
titlesuffix: Azure Cognitive Services
description: Bing varlık arama API'si ayıklayın ve varlıkları ve yerde arama kullanmasını arama sorgularından.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-entity-search
ms.topic: overview
ms.date: 02/01/2019
ms.author: aahi
ms.openlocfilehash: e78cc86a91e229b5d4d4aead5709da2f694ecac1
ms.sourcegitcommit: e51e940e1a0d4f6c3439ebe6674a7d0e92cdc152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55895372"
---
# <a name="searching-for-entities-with-the-bing-entity-api"></a>Bing varlık API'si ile varlıkları arama

## <a name="suggest-search-terms-with-the-bing-autosuggest-api"></a>Bing otomatik öneri API'si ile arama terimlerini önerin

Kullanıcıların arama terimlerini gireceği bir arama kutusu sağlıyorsanız deneyimi geliştirmek için [Bing Otomatik Öneri API'sini](../../bing-autosuggest/get-suggested-search-terms.md) kullanın. API, kullanıcı yazarken kısmi arama terimlerine dayalı önerilen sorgu dizelerini yönetin.

Kullanıcı arama terimini girdikten sonra [q](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#query) sorgu parametresini ayarlamadan önce terimi URL ile kodlayın. Örneğin kullanıcı *Marcus Appel* yazarsa `q` içinde *Marcus+Appel* veya *Marcus%20Appel* olarak kullanın.

Arama teriminde yazım hatası varsa arama yanıtında [QueryContext](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#querycontext) nesnesi bulunur. Nesne, özgün terimi ve Bing'in arama için kullandığı düzeltilmiş halini döndürür.

```json
"queryContext": {
    "originalQuery": "hollo wrld",
    "alteredQuery": "hello world",
    "alterationOverrideQuery": "+hollo wrld",
    "adultIntent": false
}
```

## <a name="the-bing-entity-search-api-response"></a>Bing varlık arama API'si yanıtı

API yanıtını içeren bir [SearchResponse](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#searchresponse) nesne. Bing ilgili bir varlık veya yer bulduğunda nesne `entities` alanını, `places` alanını veya ikisini birden içerir. Aksi takdirde yanıt nesnesi iki alanı da içermez.
> [!NOTE]
> Varlık yanıtları birden fazla pazarı destekler ancak Places yanıtı yalnızca ABD'deki İşletme konumlarını destekler. 

`entities` alanı bir [EntityAnswer](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference) nesnesidir ve [Entity](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#entity) nesnelerinin listesini içerir (`value` alanına bakın). Listede tek bir baskın varlık, birden fazla kesinleştirme varlığı veya ikisi birden bulunabilir. 

Bing istek karşılayan yalnızca bir varlık olarak düşündüğü baskın varlık döndürülür (varlık için istek karşılayan bir belirsizlik olmaz yoktur). İsteği birden fazla varlık karşılıyorsa listede birden fazla kesinleştirme varlığı bulunur. Örneğin istekte bir film serisinin genel adı kullanılıyorsa listede muhtemelen kesinleştirme varlıkları bulunacaktır. Ancak istekte serideki filmlerden birinin adı belirtiliyorsa listede muhtemelen tek bir baskın varlık olacaktır.

Varlıklar şarkıcılar, oyuncular, atletler, modeller gibi tanınmış kişileri, Mount Rainier veya Lincoln Memorial gibi önemli yerleri ve muz, goldendoodle, kitap veya film adı gibi nesneleri içerir. [entityPresentationInfo](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#entitypresentationinfo) alanı, varlık türünü tanımlayan ipuçları içerir. Örneğin varlığın kişi, film, hayvan veya önemli nokta olduğunu gösterebilir. Olası türlerin listesi için bkz. [Varlık Türleri](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#entity-types)

```json
"entityPresentationInfo": {
    "entityScenario": "DominantEntity",
    "entityTypeHints": ["Attraction"],
    "entityTypeDisplayHint": "Mountain"
}, ...
```

Aşağıda baskın varlık ve kesinleştirme varlığı içeren bir yanıt gösterilmiştir.

```json
{
    "_type": "SearchResponse",
    "queryContext": {
        "originalQuery": "Mount Rainier"
    },
    "entities": {
        "value": [{
            "contractualRules": [{
                "_type": "ContractualRules/LicenseAttribution",
                "targetPropertyName": "description",
                "mustBeCloseToContent": true,
                "license": {
                    "name": "CC-BY-SA",
                    "url": "http://creativecommons.org/licenses/by-sa/3.0/"
                },
                "licenseNotice": "Text under CC-BY-SA license"
            },
            {
                "_type": "ContractualRules/LinkAttribution",
                "targetPropertyName": "description",
                "mustBeCloseToContent": true,
                "text": "contoso.com",
                "url": "http://contoso.com/mount_rainier"
            },
            {
                "_type": "ContractualRules/MediaAttribution",
                "targetPropertyName": "image",
                "mustBeCloseToContent": true,
                "url": "http://contoso.com/mount-rainier"
            }],
            "webSearchUrl": "https://www.bing.com/search?q=Mount%20Rainier...",
            "name": "Mount Rainier",
            "url": "http://www.northwindtraders.com/",
            "image": {
                "name": "Mount Rainier",
                "thumbnailUrl": "https://www.bing.com/th?id=A4ae343983daa4...",
                "provider": [{
                    "_type": "Organization",
                    "url": "http://contoso.com/mount_rainier"
                }],
                "hostPageUrl": "http://contoso.com/commons/7/72/mount_rain...",
                "width": 110,
                "height": 110
            },
            "description": "Mount Rainier is 14,411 ft tall and the highest mountain...",
            "entityPresentationInfo": {
                "entityScenario": "DominantEntity",
                "entityTypeHints": ["Attraction"]
            },
            "bingId": "38b9431e-cf91-93be-0584-c42a3ecbfdc7"
        },
        {
            "contractualRules": [{
                "_type": "ContractualRules/MediaAttribution",
                "targetPropertyName": "image",
                "mustBeCloseToContent": true,
                "url": "http://contoso.com/mount_rainier_national_park"
            }],
            "webSearchUrl": "https://www.bing.com/search?q=Mount%20Rainier%20National...",
            "name": "Mount Rainier National Park",
            "url": "http://worldwideimporters.com/",
            "image": {
                "name": "Mount Rainier National Park",
                "thumbnailUrl": "https://www.bing.com/th?id=A91bdc5a1b648a695a39...",
                "provider": [{
                    "_type": "Organization",
                    "url": "http://contoso.com/mount_rainier_national_park"
                }],
                "hostPageUrl": "http://contoso.com/en/7/7a...",
                "width": 50,
                "height": 50
            },
            "description": "Mount Rainier National Park is a United States National Park...",
            "entityPresentationInfo": {
                "entityScenario": "DisambiguationItem",
                "entityTypeHints": ["Organization"]
            },
            "bingId": "29d4b681-227a-3924-7bb1-8a54e8666b8c"
        }]
    }
}
```

Varlıkta `name`, `description` ve `image` alanı vardır. Bu alanları kullanıcı deneyiminizde görüntülediğinizde atıfta bulunmanız gerekir. `contractualRules` alanında uygulamanız gereken özniteliklerin listesi bulunur. Atıfta bulunulacak alanı sözleşme kuralı belirler. Atıfta bulunma hakkında daha fazla bilgi için bkz. [Atıfta bulunma](#data-attribution).

```json
"contractualRules": [{
    "_type": "ContractualRules/LicenseAttribution",
    "targetPropertyName": "description",
    "mustBeCloseToContent": true,
    "license": {
        "name": "CC-BY-SA",
        "url": "http://creativecommons.org/licenses/by-sa/3.0/"
    },
    "licenseNotice": "Text under CC-BY-SA license"
},
{
    "_type": "ContractualRules/LinkAttribution",
    "targetPropertyName": "description",
    "mustBeCloseToContent": true,
    "text": "contoso.com",
    "url": "http://contoso.com/wiki/Mount_Rainier"
},
{
    "_type": "ContractualRules/MediaAttribution",
    "targetPropertyName": "image",
    "mustBeCloseToContent": true,
    "url": "http://contoso.com/wiki/Mount_Rainier"
}], ...
```

Varlık bilgilerini (ad, açıklama ve görüntü) görüntülediğinizde `webSearchUrl` alanındaki URL'yi kullanarak varlığı içeren Bing arama sonuçları sayfasına da bağlantı vermeniz gerekir.

## <a name="find-places"></a>Yerler bulun

`places` Alan bir [LocalEntityAnswer](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference) listesini içeren nesne [yerde](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#place) nesneleri (bkz [varlık türleri](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#entity-types) daha fazla bilgi için). Listede isteği karşılayan bir veya daha fazla yerel varlık bulunur.

Yerler restoranlar, oteller veya yerel işletmeler olabilir. [entityPresentationInfo](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#entitypresentationinfo) alanı, yerel varlık türünü tanımlayan ipuçları içerir. Listede Place, LocalBusiness, Restaurant gibi ipuçları bulunur. Dizideki ardışık ipuçları varlık türünü daraltır. Olası türlerin listesi için bkz. [Varlık Türleri](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#entity-types)

```json
"entityPresentationInfo": {
    "entityScenario": "ListItem",
    "entityTypeHints": ["Place",
    "LocalBusiness",
    "Restaurant"]
}, ...
```
> [!NOTE]
> Varlık yanıtları birden fazla pazarı destekler ancak Places yanıtı yalnızca ABD'deki İşletme konumlarını destekler. 

*Yakınımdaki restoranlar* gibi konuma dayalı varlık sorguları doğru sonuçları sunmak için kullanıcının konumuna ihtiyaç duyar. İsteklerinizde her zaman kullanıcının konumunu belirtmek için X-Search-Location ve X-MSEdge-ClientIP üst bilgileri kullanılmalıdır. Bing, kullanıcının konumunun sorgu için faydalı olacağını düşündüğünde [QueryContext](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#querycontext) öğesinin `askUserForLocation` alanını **true** olarak ayarlar. 

```json
{
    "_type": "SearchResponse",
    "queryContext": {
        "originalQuery": "Sinful Bakery and Cafe",
        "askUserForLocation": true
    },
    ...
}
```

Yer sonucunda yerin adı, adresi, telefon numarası ve varlık web sitesinin URL'si bulunur. Varlık bilgilerini görüntülediğinizde `webSearchUrl` alanındaki URL'yi kullanarak varlığı içeren Bing arama sonuçları sayfasına da bağlantı vermeniz gerekir.

```json
"places": {
    "value": [{
        "_type": "Restaurant",
        "webSearchUrl": "https://www.bing.com/search?q=Sinful%20Bakery...",
        "name": "Liberty's Delightful Sinful Bakery & Cafe",
        "url": "http://libertysdelightfulsinfulbakeryandcafe.com/",
        "entityPresentationInfo": {
            "entityScenario": "ListItem",
            "entityTypeHints": ["Place",
            "LocalBusiness",
            "Restaurant"]
        },
        "address": {
            "addressLocality": "Seattle",
            "addressRegion": "WA",
            "postalCode": "98112",
            "addressCountry": "US",
            "neighborhood": "Madison Park"
        },
        "telephone": "(800) 555-1212"
    }]
}
```

> [!NOTE]
> Varlıklar API'sinden alınan veriler sizin tarafınızdan veya sizin adınıza hareket eden üçüncü şahıslar tarafından Microsoft harici bir hizmeti veya özelliği test etme, geliştirme, eğitme, dağıtma veya kullanıma sunma amacıyla kullanılamaz, saklanamaz, depolanamaz, önbelleğe alınamaz, paylaşılamaz veya dağıtılamaz.  

## <a name="data-attribution"></a>Veri atfı

Bing Varlık API'si yanıtları, üçüncü taraflara ait bilgiler içerir. Kullanıcı deneyiminizde kullanılan Creative Commons lisansına uygun hareket etme gibi kullanımınızın gerektirdiği durumlardan sorumlu olursunuz.

Yanıt ya da sonuç `contractualRules`, `attributions` veya `provider` alanlarını içeriyorsa veriler için atıfta bulunmanız gerekir. Yanıtta bu alanlardan biri yoksa atıfta bulunmanıza gerek yoktur. Yanıtta `contractualRules` alanı ile `attributions` ve/veya `provider` alanı varsa verilere atıfta bulunmak için sözleşme kurallarını kullanmanız gerekir.

Aşağıdaki örnek MediaAttribution sözleşme kuralına ve `provider` alanına sahip olan bir Image nesnesine sahip olan varlığı göstermektedir. MediaAttribution kuralı görüntüyü kuralın hedefi olarak tanımlamaktadır. Bu nedenle görüntünün `provider` alanını yoksayabilir ve atıfta bulunmak için MediaAttribution kuralını kullanabilirsiniz.  

```json
"value": [{
    "contractualRules": [
        ...
        {
            "_type": "ContractualRules/MediaAttribution",
            "targetPropertyName": "image",
            "mustBeCloseToContent": true,
            "url": "http://contoso.com/mount_rainier"
        }
    ],
    ...
    "image": {
        "name": "Mount Rainier",
        "thumbnailUrl": "https://www.bing.com/th?id=A46378861201...",
        "provider": [{
            "_type": "Organization",
            "url": "http://contoso.com/mount_rainier"
        }],
        "hostPageUrl": "http://www.graphicdesigninstitute.com/Uploaded...",
        "width": 110,
        "height": 110
    },
    ...
}]
```

Sözleşme kuralında `targetPropertyName` alanı varsa kural yalnızca hedeflenen alan için geçerli olur. Aksi takdirde, kural `contractualRules` alanını içeren üst nesneye uygulanır.

Aşağıdaki örnekte `LinkAttribution` kuralı `targetPropertyName` alanını içerdiğinden kural `description` alanına uygulanır. Belirli alanlara uygulanan kurallar için hedeflenen verilerin hemen altına, sağlayıcının web sitesine bağlantı içeren bir satır eklemeniz gerekir. Örneğin açıklamaya atıfta bulunmak için açıklama metninin hemen altına, sağlayıcının web sitesindeki verilere bağlantı içeren bir satır ekleyin. Burada contoso.com sitesine bağlantı oluşturmanız gerekir.

```json
"entities": {
    "value": [{
            ...
            "description": "Marcus Appel is a former American....",
            ...
            "contractualRules": [{
                    "_type": "ContractualRules/LinkAttribution",
                    "targetPropertyName": "description",
                    "mustBeCloseToContent": true,
                    "text": "contoso.com",
                    "url": "http://contoso.com/cr?IG=B8AD73..."
                 },
            ...
  
```

### <a name="license-attribution"></a>Lisans atfı

Sözleşme kuralları listesinde [LicenseAttribution](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#licenseattribution) kuralı varsa bildirimi lisansın geçerli olduğu içeriğin hemen altındaki satırda görüntülemeniz gerekir. `LicenseAttribution` kuralı lisansın geçerli olduğu özelliği tanımlamak için `targetPropertyName` alanını kullanır.

Aşağıda `LicenseAttribution` kuralını içeren bir örnek gösterilmektedir.

![Lisans atfı](../media/cognitive-services-bing-entities-api/licenseattribution.png)

Görüntülediğiniz lisans bildirimi, lisans hakkında bilgi içeren web sitesine bağlantı içermelidir. Genellikle lisansın adı bir köprü haline getirilir. Örneğin bildirim **Metin CC-BY-SA lisansı kapsamındadır** şeklindeyse ve CC-BY-SA lisansın adıysa CC-BY-SA bölümünü köprü haline getirmeniz gerekir.

### <a name="link-and-text-attribution"></a>Bağlantı ve metin atfı

[LinkAttribution](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#linkattribution) ve [TextAttribution](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#textattribution) kuralları genellikle veri sağlayıcısını tanımlamak için kullanılır. `targetPropertyName` alanı kuralın uygulanacağı alanı tanımlar.

Sağlayıcılara atıfta bulunmak için atıfların geçerli olduğu içeriğin (hedeflenen alan gibi) hemen altına bir satır ekleyin. Satırın, verilerin kaynağının sağlayıcılar olduğunu gösterecek şekilde düzenlenmesi gerekir. Örneğin "Veri sağlayıcısı: contoso.com". `LinkAttribution` kuralları için sağlayıcının web sitesine köprü oluşturmanız gerekir.

Aşağıda `LinkAttribution` ve `TextAttribution` kurallarını içeren bir örnek gösterilmektedir.

![Bağlantı metni atfı](../media/cognitive-services-bing-entities-api/linktextattribution.png)

### <a name="media-attribution"></a>Medya atfı

Varlıkta görüntü varsa ve bunu görüntülerseniz, sağlayıcının web sitesine tıklanabilir bağlantı sağlamanız gerekir. Varlık bir [MediaAttribution](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#mediaattribution) kuralı içeriyorsa tıklama bağlantısını oluşturmak için kural URL'sini kullanın. Aksi takdirde tıklama bağlantısını oluşturmak için görüntünün `provider` alanındaki URL'yi kullanın.

Aşağıdaki örnekte bir görüntünün `provider` alanı ve sözleşme kuralları gösterilmektedir. Örnekte sözleşme kuralı olduğu için görüntünün `provider` alanını yoksayıp `MediaAttribution` kuralını uygulamanız gerekir.

![Medya atfı](../media/cognitive-services-bing-entities-api/mediaattribution.png)

### <a name="search-or-search-like-experience"></a>Arama veya arama benzeri deneyim

Bing Web Araması API'sinde olduğu gibi Bing Varlık Arama API'si de yalnızca doğrudan kullanıcı sorgusu veya araması sonucunda ya da bir uygulama veya deneyimde bulunan ve kullanıcının arama isteği gibi yorumlanabilecek bir eylem sonucunda kullanılabilir. Örnek olması amacıyla aşağıda bazı kabul edilebilir arama veya arama benzeri deneyimler sunulmuştur.

- Kullanıcı bir uygulamadaki arama kutusuna doğrudan sorgu girer
- Kullanıcı belirli bir metni veya görüntüyü seçer ve "daha fazla bilgi" ya da "ek bilgi" ister
- Kullanıcı arama botuna belirli bir konuyla ilgili soru sorar
- Görsel arama türündeki bir senaryoda kullanıcı belirli bir nesne veya varlık üzerinde durur

Deneyiminizin arama benzeri deneyim sınıfına girip girmediğinden emin değilseniz Microsoft'a danışmanız önerilir.

## <a name="throttling-requests"></a>İstekleri azaltma

[!INCLUDE [cognitive-services-bing-throttling-requests](../../../../includes/cognitive-services-bing-throttling-requests.md)]

## <a name="next-steps"></a>Sonraki adımlar

* Deneyin bir [hızlı](../quickstarts/csharp.md) Bing varlık arama API'si sahip varlıklar için aramaya başlamak için.
