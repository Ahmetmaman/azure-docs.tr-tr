---
title: Bing Insights örnekleri-Bing Görsel Arama
titleSuffix: Azure Cognitive Services
description: Bu makalede, Bing Görsel Arama nasıl kullanabileceği ve Bing.com üzerinde görüntü öngörülerini görüntüleyebilen örnekler yer almaktadır.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: conceptual
ms.date: 04/03/2019
ms.author: scottwhi
ms.openlocfilehash: e0a3bdc7e68301c067b7e1ca80a3e708522e0df1
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2021
ms.locfileid: "96493364"
---
# <a name="examples-of-bing-insights-usage"></a>Bing Öngörüler kullanım örnekleri

> [!WARNING]
> Bing Arama API'leri bilişsel hizmetlerden Bing Arama hizmetlere taşınıyor. **30 ekim 2020 ' den** itibaren, [burada](/bing/search-apis/bing-web-search/create-bing-search-service-resource)belgelenen işlem sonrasında Bing arama yeni örneklerin sağlanması gerekir.
> Bilişsel hizmetler kullanılarak sağlanan Bing Arama API'leri, sonraki üç yıl boyunca veya Kurumsal Anlaşma sonuna kadar, hangisi önce gerçekleşene kadar desteklenecektir.
> Geçiş yönergeleri için bkz. [Bing arama Services](/bing/search-apis/bing-web-search/create-bing-search-service-resource).

Bu makalede, Bing 'in Bing.com üzerinde görüntü öngörülerini nasıl kullanabileceği ve görüntüleyeceği hakkında örnekler yer almaktadır.

## <a name="pagesincluding-insight-example"></a>Öngörüler Içeren Pagesexample

Aşağıda ilk Web sayfasının bağlantısı görüntülenmektedir ve kullanıcının görüntüyü içeren diğer Web sayfalarının listesini genişletmesine ve daraltmaya izin verir:

![Genişletilmiş sayfalar dahil](./media/pages-including.PNG)

## <a name="shoppingsources-insight-example"></a>ShoppingSources Insight örneği

Aşağıda, Bing 'in görüntüde görülen ürünlere ait alışveriş kaynaklarını nasıl görüntüleyeceği gösterilmektedir:

![Alışveriş kaynakları](./media/shopping-sources.PNG)

## <a name="visualsearch-insight-example"></a>VisualSearch Insight örneği

Aşağıda, Bing 'in görsel açıdan benzer görüntüleri nasıl görüntüleyeceği gösterilmektedir (örnekteki **ilgili görüntüler** bölümüne bakın):

![Görsel olarak benzer görüntüler](./media/similar-images.PNG)

## <a name="recipes-insight-example"></a>Tarifler öngörü örneği

Aşağıda, Bing 'in görüntüde gösterilen yiyecek için yemek tariflerini nasıl görüntüleyeceği gösterilmektedir. Örnek, kullanıcının kullanılabilir Tarifler olduğunu bilmesini sağlar:

![Dahil olan yemek tarifleri ve sayfalar](./media/recipes-pages-including.PNG)

 Ve Kullanıcı listeyi genişlediğinde yapılan Tarifler için bağlantı sağlar:

![Genişletilmiş tarif sayfaları dahil](./media/expanded-recipes-pages-including.PNG)

## <a name="relatedsearches-insight-example"></a>Relatedaramaları öngörü örneği

Aşağıda, Bing 'in başkaları tarafından yapılan görüntülerle ilgili aramaları nasıl görüntüleyeceği gösterilmektedir. Kullanıcı görüntüye tıklarsa, Kullanıcı ilgili sorgu için Bing.com/images arama sonuçları sayfasına alınır.

![Görüntüler için ilgili aramalar](./media/bordered-related-searches.PNG)

## <a name="entity-insight-example"></a>Entity Insight örneği

Aşağıda, Bing 'in görüntüde gösterilen varlık (kişi, yer veya şey) hakkındaki bilgileri nasıl görüntüleyeceği gösterilmektedir. Kullanıcı varlık bağlantısına tıkladığında, Kullanıcı varlık için Bing.com arama sonuçları sayfasına alınır:

![Görüntüde gösterilen varlık](./media/entity.PNG)

## <a name="displaying-other-insights-that-the-user-might-explore"></a>Kullanıcının keşfedebilecek diğer öngörüleri görüntüleme

Aşağıda, Bing 'in kullanıcının keşfedebilecek görüntüyle ilgili diğer bilgileri nasıl görüntüleyeceği gösterilmektedir.

![Görüntüyle ilgili diğer öngörüleri keşfet](./media/apple-pie-more-tags.PNG)

## <a name="bounding-boxes-and-hot-spots"></a>Sınırlayıcı kutular ve etkin noktalar

Varsayılan olmayan Etiketler, resmin geçerli olduğu görüntüde ilgilendiğiniz alanı tanımlayan sınırlayıcı kutuyu içerir. Sınırlama kutusu görüntünün tamamını tanımlamaz görüntüde etkin bir nokta oluşturmak için sınırlayıcı kutuyu kullanın. Kullanıcı, etkin nokta (veya dikdörtgen) altında bulunan içerikle ilgili bilgi almak için etkin noktaya tıklayabilir. Örneğin, görüntü yüksek moda sahip bir görüntü ise sonuçlar, görüntüde gösterilen aksesuar (ve sınırlayıcı kutular) içerebilir; Örneğin, bir Purse, jekaynak, scarfs vb. Aşağıdaki örnek görüntüde gösterilen güneş için bir etkin nokta dikdörtgeni göstermektedir:

![Sınırlayıcı kutu ve etkin nokta](./media/click-to-search.PNG)

## <a name="next-steps"></a>Sonraki adımlar

İlk isteğinizi kullanmaya başlamak için hızlı başlangıçlara bakın:

* [C#](quickstarts/csharp.md)

* [Java](quickstarts/java.md)

* [node.js](quickstarts/nodejs.md)

* [Python](quickstarts/python.md)