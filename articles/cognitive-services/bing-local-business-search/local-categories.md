---
title: Bing yerel Iş Arama API 'SI için kategorileri ara
titleSuffix: Azure Cognitive Services
description: Bing yerel Iş Arama API uç noktası için arama kategorilerini belirtme hakkında bilgi edinmek için bu makaleyi kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-local-business
ms.topic: conceptual
ms.date: 11/01/2018
ms.author: rosh
ms.openlocfilehash: 19b769d1fff431f95c20e607c17747f2ff483d2f
ms.sourcegitcommit: d60976768dec91724d94430fb6fc9498fdc1db37
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/02/2020
ms.locfileid: "96487193"
---
# <a name="search-categories-for-the-bing-local-business-search-api"></a>Bing yerel Iş Arama API 'SI için kategorileri ara

> [!WARNING]
> Bing Arama API'leri bilişsel hizmetlerden Bing Arama hizmetlere taşınıyor. **30 ekim 2020 ' den** itibaren, [burada](/bing/search-apis/bing-web-search/create-bing-search-service-resource)belgelenen işlem sonrasında Bing arama yeni örneklerin sağlanması gerekir.
> Bilişsel hizmetler kullanılarak sağlanan Bing Arama API'leri, sonraki üç yıl boyunca veya Kurumsal Anlaşma sonuna kadar, hangisi önce gerçekleşene kadar desteklenecektir.
> Geçiş yönergeleri için bkz. [Bing arama Services](/bing/search-apis/bing-web-search/create-bing-search-service-resource).

Bing yerel Iş Arama API 'SI, bir kullanıcının konumunu kapatan sonuçlara göre çok sayıda kategoride yerel iş varlıkları aramanızı sağlar. Bu aramaları, ve parametreleriyle birlikte aramalara dahil edebilirsiniz `localCircularView` `localMapView` [parameters](specify-geographic-search.md).


## <a name="toplevel-categories"></a>TopLevel kategorileri 

Aşağıdaki türler, aramanın ana kategorilerini tanımlar.  Parametreye atanan virgülle ayrılmış bir liste kullanılarak birden fazla kategori belirtilebilir `localCategories` .  
- EatDrink 
- SeeDo 
- Ba 
- HotelsAndMotels 
- Banksandalacaklı tunları 
- Park 
- Hastaneler 

## <a name="sub-categories"></a>Alt Kategoriler
Alt kategoriler ile aynı şekilde geçirilir `localCategories` . Alt Kategoriler daha belirgin kategorileridir. Bu, bir kategori C ve alt kategorilerinden birini, aynı virgülle ayrılmış listede belirtirseniz, yalnızca C 'yi belirttiklerinde aynı sonuçları elde edersiniz.

### <a name="eat-drink"></a>Kurutink yiyecek

> BreweriesAndBrewPubs, Coctasillounges, AfricanRestaurants, AmericanRestaurants, Bagels, BarbecueRestaurants, Taverns, Sportsçubuklar, çubuklar, BarsGrillsAndPubs, BuffetRestaurants | BelgianRestaurants, Britishrestoranlar, Caferestaurler, Caribbeanrestoranlar, Chineserestaurlar, CoffeeAndTea, Delicatessens, DeliveryService, dinleyiciler, Discountmağazalar, Donmanlar, Fastgıda, Frenchrestoran, FrozenYogurt, Germanrestoranlar, süper pazarlar, Greekrestoranlar, Grocers, Hawaii JapaneseRestaurants, Juıces, KoreanRestaurants, LiquorStores, Mexicanrestoranlar, MiddleEasternRestaurants, Pizza, PolishRestaurants, PortugueseRestaurants, Pretzze, restoranlar, Russianandukrainilanrestoran, Sandwiches, deniz Yondrestoranlar, Spanlarrestoranlar, Steakhouserestaurler, Sushirestoranlar, kalkış, Thairestoranlar, Türklarrestoran, Vegetarianandveganrestoranlar, VietnameseRestaurants

### <a name="see-do"></a>Bkz. do

> Amusementpark, Atizler, Carnisal, Casınos, Landmarksandhıtoricalsites, Mini Aturegolfkurslar, Movielandters, Museums, Park, Sılerseeingtur, Touristınformation, Buros

### <a name="shop"></a>Ba

> Antisorgtores, Bookmağazaların, Cdandrecordmağazaların, Childrensclothingmağazaların, CigarAndTobaccoShops, Comicbookmağazaların, DepartmentStores, Discountmağazaların, FleaMarketsAndBazaars, Mobilyanitugeri Yüklemeler, HomeImprovementStores, JewelryAndWatchesStores, KitchenwareStores, LiquorStores, MallsAndShoppingCenters, Mensclothingmağazalar, Musicmağazaları, Outletmağazaları, Pettat, Petsupplymağazalar, SchoolAndOfficeSupplyStores, ShoeStores, SportingGoodsStores, ToyAndGameStores, Catminandtooltmağazaları, Womensclothingmağazaları


## <a name="examples-of-local-categories-search"></a>Yerel Kategori arama örnekleri

Aşağıdaki örnekler, parametresine göre sonuçları alır `localCategories` :

`https://api.cognitive.microsoft.com/localbusinesses/v7.0/search?&q=&mkt=en-US&localcategories=HotelsAndMotels`

`https://api.cognitive.microsoft.com/localbusinesses/v7.0/search?&q=&mkt=en-US&localcategories=EatDrink`

`https://api.cognitive.microsoft.com/localbusinesses/v7.0/search?&q=&mkt=en-US&localcategories=Shop`

`https://api.cognitive.microsoft.com/localbusinesses/v7.0/search?&q=&mkt=en-US&localcategories=Hospitals`

Aşağıdaki sorgu, Bing yerel Iş Arama API 'sinden ilk üç ' hospte ' sonuç sayısını kısıtlar:

`https://api.cognitive.microsoft.com/localbusinesses/v7.0/search?&q=&mkt=en-US&localCategories=Hospitals&count=3&offset=0`

Aşağıdaki örnek JSON yanıtı, daha büyük Seattle alanında üç hastaneler içerir:

```json
BingAPIs-TraceId: 68AFB51807C6485CAB8AAF20E232EFFF
BingAPIs-SessionId: F89E7B8539B34BF58AAF811485E83B20
X-MSEdge-ClientID: 1C44E64DBFAA6BCA1270EADDBE7D6A22
BingAPIs-Market: en-US
X-MSEdge-Ref: Ref A: 68AFB51807C6485CAB8AAF20E232EFFF Ref B: CO1EDGE0108 Ref C: 2018-10-22T18:52:28Z

{
   "_type": "SearchResponse",
   "queryContext": {
      "originalQuery": ""
   },
   "places": {
      "readLink": "https:\/\/www.bingapis.com\/api\/v7\/places\/search?q=",
      "totalEstimatedMatches": 0,
      "value": [
         {
            "_type": "LocalBusiness",
            "id": "https:\/\/www.bingapis.com\/api\/v7\/#Places.0",
            "name": "Overlake Hospital Medical Center",
            "url": "http:\/\/www.overlakehospital.org\/",
            "entityPresentationInfo": {
               "entityScenario": "ListItem",
               "entityTypeHints": [
                  "Place",
                  "LocalBusiness"
               ]
            },
            "geo": {
               "latitude": 47.6204986572266,
               "longitude": -122.185829162598
            },
            "routablePoint": {
               "latitude": 47.6204986572266,
               "longitude": -122.185668945312
            },
            "address": {
               "streetAddress": "1035 116th Ave NE",
               "addressLocality": "Bellevue",
               "addressRegion": "WA",
               "postalCode": "98004",
               "addressCountry": "US",
               "neighborhood": "",
               "text": "1035 116th Ave NE, Bellevue, WA, 98004"
            },
            "telephone": "(425) 688-5000"
         },
         {
            "_type": "LocalBusiness",
            "id": "https:\/\/www.bingapis.com\/api\/v7\/#Places.1",
            "name": "Virginia Mason University Village Medical Center",
            "url": "https:\/\/virginiamason.org\/Seattle",
            "entityPresentationInfo": {
               "entityScenario": "ListItem",
               "entityTypeHints": [
                  "Place",
                  "LocalBusiness"
               ]
            },
            "geo": {
               "latitude": 47.6095390319824,
               "longitude": -122.327941894531
            },
            "routablePoint": {
               "latitude": 47.6093978881836,
               "longitude": -122.328277587891
            },
            "address": {
               "streetAddress": "1100 9th Ave",
               "addressLocality": "Seattle",
               "addressRegion": "WA",
               "postalCode": "98101",
               "addressCountry": "US",
               "neighborhood": "University District",
               "text": "1100 9th Ave, Seattle, WA, 98101"
            },
            "telephone": "(206) 223-6600"
         },
         {
            "_type": "LocalBusiness",
            "id": "https:\/\/www.bingapis.com\/api\/v7\/#Places.2",
            "name": "Seattle Children’s Hospital",
            "url": "http:\/\/www.seattlechildrens.org\/",
            "entityPresentationInfo": {
               "entityScenario": "ListItem",
               "entityTypeHints": [
                  "Place",
                  "LocalBusiness"
               ]
            },
            "geo": {
               "latitude": 47.6625061035156,
               "longitude": -122.283760070801
            },
            "routablePoint": {
               "latitude": 47.6631507873535,
               "longitude": -122.284614562988
            },
            "address": {
               "streetAddress": "4800 Sand Point Way NE",
               "addressLocality": "Seattle",
               "addressRegion": "WA",
               "postalCode": "98105",
               "addressCountry": "US",
               "neighborhood": "Laurelhurst",
               "text": "4800 Sand Point Way NE, Seattle, WA, 98105"
            },
            "telephone": "(206) 987-2000"
         }
      ],
      "searchAction": {

      }
   }
}
```

## <a name="next-steps"></a>Sonraki adımlar
- [Coğrafi arama sınırları](specify-geographic-search.md)
- [Sorgu ve yanıt](local-search-query-response.md)
- [C 'de hızlı başlangıç #](quickstarts/local-quickstart.md)