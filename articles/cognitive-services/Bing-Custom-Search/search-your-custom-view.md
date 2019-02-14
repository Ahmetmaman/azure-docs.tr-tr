---
title: Özel Görünüm - Bing özel arama arama
titlesuffix: Azure Cognitive Services
description: Özel bir web görünümü arama açıklar.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: conceptual
ms.date: 02/12/2019
ms.author: maheshb
ms.openlocfilehash: e0c1ec55a7f21ad223629d6d478cbd97c7974abb
ms.sourcegitcommit: de81b3fe220562a25c1aa74ff3aa9bdc214ddd65
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2019
ms.locfileid: "56237986"
---
# <a name="call-your-bing-custom-search-instance-from-the-portal"></a>Bing özel arama örneğinizin portalından çağırın

Özel arama deneyiminizi yapılandırdıktan sonra ondan içinde Bing özel arama sınayabilirsiniz [portalı](https://customsearch.ai). 

![bing özel arama Portalı'nın bir ekran görüntüsü](media/portal-search-screen.png)
## <a name="create-a-search-query"></a>Bir arama sorgusu oluşturun 

Bing özel arama açtıktan sonra [portalı](https://customsearch.ai), arama örneğinizi seçin ve tıklayın **üretim** sekmesi. Altında **uç noktaları**, bir API uç noktası (örneğin, Web API'si) seçin. Aboneliğiniz, hangi uç noktaları gösterileceğini belirler.

Bir arama sorgusu oluşturmak için uç noktanız için parametre değerlerini girin. Seçtiğiniz uç nokta bağlı olarak gösterilmesi parametreleri değişebileceğini unutmayın. Bkz:[özel arama API'si başvurusu](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#query-parameters) daha fazla bilgi için. 

Önemli bazı parametreler aşağıda verilmiştir:


|Parametre  |Açıklama  |
|---------|---------|
|Sorgu     | Aranacak arama terimi. Yalnızca Web, resim, Video ve otomatik öneri uç noktaları için kullanılabilir |
|Özel yapılandırma kimliği | Seçili özel arama örneği yapılandırma kimliği. Bu alan salt okunur. |
|Market     | Sonuçları Pazar kaynaklanan. Yalnızca, Web, resim, Video ve UI barındırılan uç noktalar için de kullanılabilir.        |
|Abonelik Anahtarı | Test etmek için abonelik anahtarı. Açılır listeden bir anahtar seçin veya el ile girin.          |

Tıklayarak **ek parametreler** aşağıdaki parametreleri gösterir:  

|Parametre  |Açıklama  |
|---------|---------|
|Güvenli arama     | Web sayfaları için yetişkinlere yönelik içeriğe filtrelemek için kullanılan bir filtre. Yalnızca, Web, resim, Video ve UI barındırılan uç noktalar için de kullanılabilir.        |
|Kullanıcı arabirimi dili    | Kullanıcı arabirimi dizeleri için kullanılan dil. Örneğin, görüntü ve video kullanıcı arabiriminde, barındırılan etkinleştirirseniz **görüntü** ve **Video** belirtilen dil sekmelerini kullanın.        |
|Sayı     | Yanıtta döndürülecek arama sonuçlarının sayısı. Yalnızca Web, görüntü ve Video uç noktaları için kullanılabilir.         |
|Uzaklık    | Sonuç döndürülmeden önce atlamak için arama sonuçları sayısı. Yalnızca Web, görüntü ve Video uç noktaları için kullanılabilir.        |
    
Gerekli tüm seçenekler belirttikten sonra tıklayın **çağrı** JSON yanıtı sağ bölmede görüntülemek için. UI barındırılan uç noktayı seçin, arama deneyimini alt bölmede test edebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Çağrı C# ile özel görünümü](./call-endpoint-csharp.md)
- [Java ile özel görünümünüzü çağırın](./call-endpoint-java.md)
- [Çağrı, özel bir görünümü ile NodeJs](./call-endpoint-nodejs.md)
- [Python ile özel görünümünüzü çağırın](./call-endpoint-python.md)

- [C# SDK'sı ile özel görünümünüzü çağırın](./sdk-csharp-quick-start.md)
