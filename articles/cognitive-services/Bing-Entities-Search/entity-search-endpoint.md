---
title: Bing Varlık Arama API'si uç noktası
titleSuffix: Azure Cognitive Services
description: Bing Varlık Arama API'si bir sorguya göre Web 'den varlıkları döndüren bir uç nokta vardır. Bu arama sonuçları JSON içinde döndürülür.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-entity-search
ms.topic: conceptual
ms.date: 02/01/2019
ms.author: aahi
ms.openlocfilehash: e7f5af42d0bb0079746cc9e64b621adfebd565d1
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "74072665"
---
# <a name="bing-entity-search-api-endpoint"></a>Bing Varlık Arama API'si uç noktası


Bing Varlık Arama API'si bir sorguya göre Web 'den varlıkları döndüren bir uç nokta vardır. Bu arama sonuçları JSON içinde döndürülür.

## <a name="get-entity-results-from-the-endpoint"></a>Uç noktadan varlık sonuçları al

**BING API**'yi kullanarak varlık sonuçları almak için `GET` aşağıdaki uç noktaya bir istek gönderin. Arama isteğinizi özelleştirmek için [üst bilgileri](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference#headers) ve [sorgu parametrelerini](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference#query-parameters) kullanın. Arama istekleri parametresi kullanılarak gönderilebilir `?q=` .

```cURL
 GET https://api.cognitive.microsoft.com/bing/v7.0/entities
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bing Varlık Arama API'si nedir?](overview.md)

## <a name="see-also"></a>Ayrıca bkz. 

Üstbilgiler, parametreler, Pazar kodları, yanıt nesneleri, hatalar ve daha fazlası hakkında daha fazla bilgi için [Bing varlık arama API'si v7](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference) Reference makalesine bakın.
