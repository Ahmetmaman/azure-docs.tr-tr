---
title: Metni Böl bilişsel arama beceri (Azure Search) | Microsoft Docs
description: Metin öbekleri veya metin tabanlı bir Azure Search zenginleştirme ardışık düzeninde bir uzunluk sayfaları Kes.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: 583d2ac5a8ac4c236612cdfe78595da1812c56fa
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/17/2018
ms.locfileid: "45730775"
---
#   <a name="text-split-cognitive-skill"></a>Bilişsel beceri metni Böl

**Metin bölme** beceri metin metin öbeklere böler. Metin cümleler veya sayfaları belirli bir süre sonu isteyip istemediğinizi belirtebilirsiniz. Bu yetenek, diğer aşağı yönde becerileri uzunluğu gereksinimleri en büyük metin varsa özellikle yararlıdır. 

> [!NOTE]
> Bilişsel Arama, genel önizleme aşamasındadır. Görüntü ayıklama ve normalleştirme ve beceri yürütmesi şu anda ücretsiz sunulmaktadır. Daha sonraki bir zamanda, bu özelliklerin fiyatlandırması duyurulacaktır. 

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.SplitSkill 

## <a name="skill-parameters"></a>Yetenek parametreleri

Parametreler büyük/küçük harfe duyarlıdır.

| Parametre adı     | Açıklama |
|--------------------|-------------|
| textSplitMode      | "Sayfalar" veya "cümleler" | 
| maximumPageLength | TextSplitMode "sayfalarına" olarak ayarlanırsa bu ölçülen en fazla sayfa uzunluğu başvuruyor `String.Length`. En düşük değer 100'dür. | 
| defaultLanguageCode   | (isteğe bağlı) Aşağıdaki dil kodları: `da, de, en, es, fi, fr, it, ko, pt`. İngilizce (TR) varsayılandır. Dikkate alınması gereken bazı noktalar:<ul><li>Bir languagecode countrycode biçimi geçirirseniz, yalnızca biçim languagecode bölümü kullanılır.</li><li>Dilin önceki listede değilse, bölünmüş beceri karakter sınırlarında metni keser.</li><li>Dil kodu sağlayarak, yarıya Çince, Japonca ve Korece gibi dilleri boşluk olmayan bir sözcük kesme önlemek açısından kullanışlıdır.</li></ul>  |


## <a name="skill-inputs"></a>Beceri girişleri

| Parametre adı       | Açıklama      |
|----------------------|------------------|
| metin  | Dizeye Bölünecek metin. |
| languageCode  | (İsteğe bağlı) Belge için dil kodu.  |

## <a name="skill-outputs"></a>Beceri çıkışları 

| Parametre adı     | Açıklama |
|--------------------|-------------|
| textItems | Ayıklanan alt dizeler dizisi. |


##  <a name="sample-definition"></a>Örnek tanımı

```json
{
    "@odata.type": "#Microsoft.Skills.Text.SplitSkill",
    "textSplitMode" : "pages", 
    "maximumPageLength": 1000,
    "defaultLanguageCode": "en",
    "inputs": [
        {
            "name": "text",
            "source": "/document/content"
        },
        {
            "name": "languageCode",
            "source": "/document/language"
        }
    ],
    "outputs": [
        {
            "name": "textItems",
            "targetName": "mypages"
        }
    ]
}
```

##  <a name="sample-input"></a>Örnek Giriş

```json
{
    "values": [
        {
            "recordId": "1",
            "data": {
                "text": "This is a the loan application for Joe Romero, he is a Microsoft employee who was born in Chile and then moved to Australia…",
                "languageCode": "en"
            }
        },
        {
            "recordId": "2",
            "data": {
                "text": "This is the second document, which will be broken into several pages...",
                "languageCode": "en"
            }
        }
    ]
}
```

##  <a name="sample-output"></a>Örnek çıktı

```json
{
    "values": [
        {
            "recordId": "1",
            "data": {
                "textItems": [
                    "This is the loan…",
                    "On the second page we…"
                ]
            }
        },
        {
            "recordId": "2",
            "data": {
                "textItems": [
                    "This is the second document...",
                    "On the second page of the second doc…"
                ]
            }
        }
    ]
}
```

## <a name="error-cases"></a>Hata durumları
Bir dil desteklenmiyorsa, bir uyarı oluşturulduğu ve metin karakter sınırlarında bölünür.

## <a name="see-also"></a>Ayrıca bkz.

+ [Önceden tanımlanmış beceriler](cognitive-search-predefined-skills.md)
+ [Bir beceri kümesi tanımlama](cognitive-search-defining-skillset.md)
