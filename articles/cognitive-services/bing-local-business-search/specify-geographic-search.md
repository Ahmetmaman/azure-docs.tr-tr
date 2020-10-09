---
title: Bing yerel Iş Arama API 'sindeki sonuçları filtrelemek için coğrafi sınırları kullanın
titleSuffix: Azure Cognitive Services
description: Arama sonuçlarının Bing yerel Iş Arama API 'sinden nasıl filtreleneceğini öğrenmek için bu makaleyi kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-local-business
ms.topic: conceptual
ms.date: 11/01/2018
ms.author: rosh
ms.openlocfilehash: 213457bc583494bbe039269b96b25990f7d0a961
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "69906240"
---
# <a name="use-geographic-boundaries-to-filter-results-from-the-bing-local-business-search-api"></a>Bing yerel Iş Arama API 'sindeki sonuçları filtrelemek için coğrafi sınırları kullanın

Bing yerel Iş Arama API 'SI, `localCircularView` veya sorgu parametrelerini kullanarak aramak istediğiniz belirli coğrafi alanda sınır ayarlamanıza olanak sağlar `localMapView` . Sorgularda yalnızca bir parametre kullandığınızdan emin olun. 

Bir arama terimi açık bir coğrafi konum içeriyorsa, Bing yerel Iş API 'SI, arama sonuçlarının sınırlarını ayarlamak için otomatik olarak bunu kullanacaktır. Örneğin, arama terimi ise, `sailing in San Diego` `San Diego` konum olarak kullanılır ve sorgu parametrelerinde veya Kullanıcı üst bilgilerinde belirtilen diğer konumlar göz ardı edilir. 

Arama Teriminde bir coğrafi konum algılanmazsa ve sorgu parametreleri kullanılarak hiçbir coğrafi konum belirtilmemişse, Bing yerel Iş Arama API 'SI, isteğin `X-Search-ClientIP` veya üst bilgilerden konum belirlemeyi dener `X-Search-Location` . Hiçbir üst bilgi belirtilmemişse, API isteğin istemci IP 'sinden veya mobil cihazlar için GPS koordinatlarından konumu tespit eder.

## <a name="localcircularview"></a>Localmılarview

`localCircularView`Parametresi, bir RADIUS tarafından tanımlanan bir dizi Enlem/Boylam koordinatı etrafında dairesel bir coğrafi alan oluşturur. Bu parametre kullanılırken, Bing yerel Iş Arama API 'sindeki yanıtlar yalnızca bu dairenin içindeki konumları içerir ve bu da, `localMapView` arama alanının dışında kalan konumları içerebilen parametrenin aksine.

Dairesel bir coğrafi arama alanı belirtmek için, dairenin ortası olarak kullanılacak bir enlem ve boylam, ölçü cinsinden bir yarıçap seçin. Bu parametre daha sonra bir sorgu dizesine eklenebilir, örneğin: `q=Restaurants&localCircularView=47.6421,-122.13715,5000` .

Sorgu Tamam:

```
https://api.cognitive.microsoft.com/bing/v7.0/localbusinesses/search?q=restaurant&localCircularView=47.6421,-122.13715,5000&appid=0123456789ABCDEF&mkt=en-us&form=monitr
```

## <a name="localmapview"></a>localMapView

`localMapView`Parametresi, Güneydoğu ve Kuzeybatı köşelerini belirtmek için iki koordinat kümesini kullanarak arama yapılacak dikdörtgen bir coğrafi alanı belirtir. Bu parametre kullanılırken, Bing yerel Iş Arama API 'sindeki yanıtlar, `localCircularView` yalnızca arama alanındaki konumları içeren parametresinin aksine, belirtilen alanın içindeki ve yalnızca dışındaki konumları içerebilir.

Dikdörtgen bir arama alanı belirtmek için, sınırın Güneydoğu ve Kuzeybatı köşeleri olarak kullanılacak iki Enlem/Boylam koordinatı kümesi seçin. Aşağıdaki örnekte olduğu gibi, ilk olarak Güneydoğu koordinatları tanımlamanız gerekir: `localMapView=47.619987,-122.181671,47.6421,-122.13715` .

Sorgu Tamam:

```
https://api.cognitive.microsoft.com/bing/v7.0/localbusinesses/search?q=restaurant&localMapView=47.619987,-122.181671,47.6421,-122.13715&appid=0123456789ABCDEF&mkt=en-us&form=monitr
```

## <a name="next-steps"></a>Sonraki adımlar
- [Yerel Iş arama Java hızlı başlangıç](quickstarts/local-search-java-quickstart.md)
- [Yerel Iş arama C# hızlı başlangıç](quickstarts/local-quickstart.md)
- [Yerel Iş arama düğümü hızlı başlangıç](quickstarts/local-search-node-quickstart.md)
- [Yerel Iş arama Python hızlı başlangıç](quickstarts/local-search-python-quickstart.md)
