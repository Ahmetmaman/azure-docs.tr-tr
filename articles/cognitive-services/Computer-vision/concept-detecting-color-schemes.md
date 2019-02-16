---
title: Renk düzenleri - görüntü işleme algılama
titleSuffix: Azure Cognitive Services
description: Görüntü işleme API'sini kullanarak resimlerdeki renk şeması algılama için ilgili kavramları.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 02/08/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 6b25da9b2569b0185d41684c45a22a3eb3377511
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/15/2019
ms.locfileid: "56313084"
---
# <a name="detect-color-schemes-in-images"></a>Görüntüleri renk düzenleri algılayın

Görüntü işleme, üç farklı öznitelikler sağlamak için bir görüntü renkleri Çözümler: görüntüyü bir bütün olarak baskın renkler kümesi baskın ön plan rengini ve baskın arka plan rengi. Renkler kümeye ait döndürdü: siyah, mavi, brown, gri, yeşil, orange, pembe, mor, red, Deniz Mavisi, teknik ve sarı. 

Görüntü işleme, ayrıca bir bileşimine baskın renk doygunluğu ve dayalı görüntüde, en canlı rengi temsil eder bir Vurgu rengi ayıklar. Vurgu rengi, onaltılık bir HTML renk kodu döndürülür. 

Görüntü işleme, ayrıca bir resmin siyah beyaz olmadığını gösteren bir Boole değeri döndürür.

## <a name="color-scheme-detection-examples"></a>Renk şeması algılama örnekleri

Aşağıdaki örnek, örnek görüntüde renk düzenini tespit edilirken, görüntü işleme tarafından döndürülen JSON yanıtı gösterir. Bu durumda, örnek görüntüde siyah beyaz bir resim değil ancak baskın ön plan ve arka plan rengini siyah olur ve baskın bir bütün olarak resmin siyah beyaz renklerdir.

![Dış Mekanda Dağ](./Images/mountain_vista.png)

```json
{
    "color": {
        "dominantColorForeground": "Black",
        "dominantColorBackground": "Black",
        "dominantColors": ["Black", "White"],
        "accentColor": "BB6D10",
        "isBwImg": false
    },
    "requestId": "0dc394bf-db50-4871-bdcc-13707d9405ea",
    "metadata": {
        "height": 202,
        "width": 300,
        "format": "Jpeg"
    }
}
```

### <a name="dominant-color-examples"></a>Baskın renk örnekleri

Aşağıdaki tablo döndürülen ön plan, arka plan ve her bir örnek görüntü için görüntü renkleri gösterir.

| Görüntü | Baskın renkler |
|-------|-----------------|
|![Yeşil bir arka plan beyaz çiçek](./Images/flower.png)| Ön plan: Siyah<br/>Arka planı: Beyaz<br/>Renkler: Siyah, beyaz-yeşil|
![İstasyonu çalışan bir eğitimi](./Images/train_station.png) | Ön plan: Siyah<br/>Arka planı: Siyah<br/>Renkler: Siyah |

### <a name="accent-color-examples"></a>Vurgu rengi örnekleri

 Aşağıdaki tabloda, her bir örnek görüntü için onaltılık bir HTML renk değeri olarak döndürülen Vurgu rengi gösterilmektedir.

| Görüntü | Vurgu rengi |
|-------|--------------|
|![Üzerinde bir Sıradağlar rock gün batımı duran bir kişi](./Images/mountain_vista.png) | #BB6D10 |
|![Yeşil bir arka plan beyaz çiçek](./Images/flower.png) | #C6A205 |
|![İstasyonu çalışan bir eğitimi](./Images/train_station.png) | #474A84 |

### <a name="black--white-detection-examples"></a>Siyah & Beyaz algılama örnekleri

Aşağıdaki tablo, görüntü işleme'nın siyah beyaz değerlendirme örnek görüntüleri gösterir.

| Görüntü | Siyah & Beyaz? |
|-------|----------------|
|![Siyah beyaz Manhattan stockholm'deki resmi](./Images/bw_buildings.png) | true |
|![Mavi bir ev ve ön yard](./Images/house_yard.png) | false |

## <a name="next-steps"></a>Sonraki adımlar

Kavramları hakkında bilgi edinin [resim türleri algılama](concept-detecting-image-types.md).
