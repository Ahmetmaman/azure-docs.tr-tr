---
title: Language Understanding için önceden oluşturulmuş etki alanları
titleSuffix: Azure Cognitive Services
description: LUIS, bir dizi ortak, konuşma kullanıcı senaryoları hızlı bir şekilde eklemek için önceden oluşturulmuş etki alanları içerir.
services: cognitive-services
author: diberry
ms.custom: seodec18
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 07/29/2019
ms.author: diberry
ms.openlocfilehash: 7028c3bee549603dc29bfcdeb386bcabc52ff03c
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68637870"
---
# <a name="add-prebuilt-domains-for-common-usage-scenarios"></a>Genel kullanım senaryoları için önceden oluşturulmuş etki alanlarını ekleyin 

LUIS, bir dizi ortak hedefleri ve konuşma hızlı bir şekilde eklemek için önceden oluşturulmuş etki alanlarının önceden oluşturulmuş hedefleri içerir. Bu yetenekler modellerini tasarlamak zorunda kalmadan yeteneklerini damıtarak konuşma bağlamında kullanılabilen istemci uygulamanıza eklemek için hızlı ve kolay bir yolu budur. 

## <a name="add-a-prebuilt-domain"></a>Önceden oluşturulmuş bir alan ekleme

1. Üzerinde **uygulamalarım** sayfasında, uygulamanızı seçin. Uygulamanıza açılır **derleme** uygulama bölümü. 

1. Üzerinde **hedefleri** sayfasında **önceden oluşturulmuş etki alanları eklemek** alt, sol araç çubuğu. 

1. Seçin **Takvim** hedefi ardından select **etki alanı Ekle** düğmesi.

    ![Takvim önceden oluşturulmuş etki alanı ekleme](./media/luis-prebuilt-domains/add-prebuilt-domain.png)

1. Seçin **hedefleri** Takvim ıntents görüntülemek için sol gezinti bölmesinde. Bu etki alanındaki her amacı ön ekine sahip `Calendar.`. Konuşma yanı sıra, bu etki alanı için iki varlık uygulamaya eklenir: `Calendar.Location` ve `Calendar.Subject`. 

## <a name="train-and-publish"></a>Eğitme ve yayımlama

1. Seçerek uygulama etki alanı eklendikten sonra eğitim **eğitme** üst, sağ araç çubuğu. 

1. Üst araç çubuğunda, seçin **Yayımla**. Yayımlama **üretim**. 

1. Yeşil bir başarı bildirimi görüntülendiğinde seçin **uç noktalar listesine bakın** bağlantı uç noktaları görmek için.

1. Bir uç nokta seçin. Bu uç noktasına yeni bir tarayıcı sekmesi açılır. Tarayıcı sekmesini açık tutun ve devam **Test** bölümü.

## <a name="test"></a>Test etme

Test uç noktasında yeni hedefi için bir değer olarak eklenen **q** parametresi: `Schedule a meeting with John Smith in Seattle next week`.

LUIS doğru amacı ve toplantı konu döndürür:

```json
{
  "query": "Schedule a meeting with John Smith in Seattle next week",
  "topScoringIntent": {
    "intent": "Calendar.Add",
    "score": 0.824783146
  },
  "entities": [
    {
      "entity": "a meeting with john smith",
      "type": "Calendar.Subject",
      "startIndex": 9,
      "endIndex": 33,
      "score": 0.484055847
    }
  ]
}
```

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Önceden oluşturulmuş etki alanı başvurusu](./luis-reference-prebuilt-domains.md)
