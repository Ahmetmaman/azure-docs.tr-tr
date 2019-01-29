---
title: Proje URL'si Önizleme uç noktası
titlesuffix: Azure Cognitive Services
description: URL önizlemesi uç nokta özeti.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: url-preview
ms.topic: reference
ms.date: 03/29/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: 8ed28ef356a4e884fca61b950c59506762fb8bdc
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2019
ms.locfileid: "55099671"
---
# <a name="project-url-preview-endpoint"></a>Proje URL'si Önizleme uç noktası

Bir uç nokta URL'si Önizleme API içerir.

## <a name="endpoint"></a>Uç Nokta
URL önizlemesi almak için aşağıdaki uç noktaya bir istek gönderin. Diğer belirtimleri için başlık ve URL parametrelerini kullanın.

AL:
```
https://api.labs.cognitive.microsoft.com/urlpreview/v7.0/search?q=https://swiftkey.com

```

### <a name="query-parameters"></a>Sorgu parametreleri
|Name|Value|Type|Gereklidir|  
|----------|-----------|----------|--------------|  
|q|URL önizlemesi|Dize |Evet|
|safeSearch|Geçersiz yetişkinlere yönelik içeriği veya korsan içeriği, 400 ' hata koduyla engellendi ve *isFamilyFriendly* bayrağı alınmadı. <p>Yetişkinlere yönelik içeriği için yasal, aşağıda davranıştır. Durum kodu 200 döndürür ve *isFamilyFriendly* bayrağı false olarak ayarlanır.<ul><li>safeSearch strict =: Başlık, açıklama, URL ve görüntü döndürülmez.</li><li>safeSearch Orta; = Başlık, URL ve açıklama ancak açıklayıcı görüntü alın.</li><li>safeSearch; = Tüm yanıt nesneleri/öğeleri – başlık, URL, açıklama ve resim alın.</li></ul> |Dize|Gerekli değildir. </br> Varsayılan olarak safeSearch için strict =.| 

## <a name="response-object"></a>Yanıt nesnesi

Aşağıdaki örnekte gösterildiği gibi yanıt HTTP üst bilgileri ve öznitelikleri ile Web sayfası nesnesi içeriyor: `name`, `url`, `description`, `isFamilyFriendly`, ve `primaryImageOfPage`.

```
BingAPIs-TraceId: 15AFE52A97AA422F960433A94803F6CE
BingAPIs-SessionId: 40587764F42142D3A8BA99F66B2B3BB6
X-MSEdge-ClientID: 0389E3EDED106B5E1424E82FEC436A56
BingAPIs-Market: en-US
X-MSEdge-Ref: Ref A: 15AFE52A97AA422F960433A94803F6CE Ref B: PAOEDGE0418 Ref C: 2018-03-30T16:36:27Z
{
  "_type": "WebPage",
  "name": "SwiftKey - Smart prediction technology for easier mobile ...",
  "url": "https://swiftkey.com/",
   "description": "Discover the best Android and iPhone and iPad apps for faster, easier typing with emoji, colorful themes and more - download SwiftKey Keyboard free today.",
  "isFamilyFriendly": true,
  "primaryImageOfPage": {
    "contentUrl": "https://swiftkey.com/images/og/default.jpg"
  }
}

```

## <a name="next-steps"></a>Sonraki adımlar
- [C# hızlı başlangıcı](csharp.md)
- [Java hızlı başlangıcı](java-quickstart.md)
- [JavaScript hızlı başlangıcı](javascript.md)
- [Node hızlı başlangıcı](node-quickstart.md)
- [Python hızlı başlangıcı](python-quickstart.md)
