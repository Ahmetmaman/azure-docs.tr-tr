---
title: Önceden oluşturulmuş varlık sayısı-LUSıS
titleSuffix: Azure Cognitive Services
description: Bu makale numarası önceden oluşturulmuş varlık bilgilerini Language Understanding (LUIS) içerir.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 05/07/2019
ms.author: diberry
ms.openlocfilehash: 88f36fb6d73e2ec88940e7eb53d982824e194074
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68560194"
---
# <a name="number-prebuilt-entity-for-a-luis-app"></a>Bir LUSıS uygulaması için önceden oluşturulmuş varlık sayısı
Hangi sayısal değerleri ölçme, express ve bilgi parçalarını tanımlamak için kullanılan birçok yolu vardır. Bu makalede yalnızca bazı olası örnekler yer almaktadır. LUIS, kullanıcı konuşma farklılığı yorumlar ve tutarlı bir sayısal değerleri döndürür. Bu varlık zaten eğitildi çünkü uygulama hedefleri için numarası içeren örnek Konuşma ekleme gerekmez. 

## <a name="types-of-number"></a>Sayı türleri
Numara [Tanıyıcılar-metin](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-Numbers.yaml) GitHub deposundan yönetiliyor

## <a name="examples-of-number-resolution"></a>Sayı çözümleme örnekleri

| İfade        | Varlık   | Çözüm |
| ------------- |:----------------:| --------------:|
| ```one thousand times```  | ```"one thousand"``` |   ```"1000"```      | 
| ```1,000 people```        | ```"1,000"```    |   ```"1000"```      |
| ```1/2 cup```         | ```"1 / 2"```    |    ```"0.5"```      |
|  ```one half the amount```     | ```"one half"```     |    ```"0.5"```      |
| ```one hundred fifty orders``` | ```"one hundred fifty"``` | ```"150"``` |
| ```one hundred and fifty books``` | ```"one hundred and fifty"``` | ```"150"```|
| ```a grade of one point five```| ```"one point five"``` |  ```"1.5"``` |
| ```buy two dozen eggs```    | ```"two dozen"``` | ```"24"``` |


LUIS, tanınan bir değer içeren bir **`builtin.number`** varlıkta `resolution` alanını JSON yanıtı döndürür.

## <a name="resolution-for-prebuilt-number"></a>Önceden oluşturulmuş numaralı çözümleme


### <a name="api-version-2x"></a>API sürüm 2. x

Aşağıdaki örnek, çözüm için "iki düzine" utterance 24, değeri içeren bir JSON yanıtı, luıs'den gösterir.

```json
{
  "query": "order two dozen eggs",
  "topScoringIntent": {
    "intent": "OrderFood",
    "score": 0.105443209
  },
  "intents": [
    {
      "intent": "None",
      "score": 0.105443209
    },
    {
      "intent": "OrderFood",
      "score": 0.9468431361
    },
    {
      "intent": "Help",
      "score": 0.000399122015
    },
  ],
  "entities": [
    {
      "entity": "two dozen",
      "type": "builtin.number",
      "startIndex": 6,
      "endIndex": 14,
      "resolution": {
        "subtype": "integer",
        "value": "24"
      }
    }
  ]
}
```

### <a name="preview-api-version-3x"></a>Preview API sürüm 3. x

Aşağıdaki JSON `verbose` parametresi olarak `false`ayarlanmıştır:

```json
{
    "query": "order two dozen eggs",
    "prediction": {
        "normalizedQuery": "order two dozen eggs",
        "topIntent": "None",
        "intents": {
            "None": {
                "score": 0.7124502
            }
        },
        "entities": {
            "number": [
                24
            ]
        }
    }
}
```

Aşağıdaki JSON `verbose` parametresi olarak `true`ayarlanmıştır:

```json
{
    "query": "order two dozen eggs",
    "prediction": {
        "normalizedQuery": "order two dozen eggs",
        "topIntent": "None",
        "intents": {
            "None": {
                "score": 0.7124502
            }
        },
        "entities": {
            "number": [
                24
            ],
            "$instance": {
                "number": [
                    {
                        "type": "builtin.number",
                        "text": "two dozen",
                        "startIndex": 6,
                        "length": 9,
                        "modelTypeId": 2,
                        "modelType": "Prebuilt Entity Extractor"
                    }
                ]
            }
        }
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

Hakkında bilgi edinin [para birimi](luis-reference-prebuilt-currency.md), [sıralı](luis-reference-prebuilt-ordinal.md), ve [yüzdesi](luis-reference-prebuilt-percentage.md). 
