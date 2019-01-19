---
title: Yaklaşım bilişsel arama beceri - Azure Search
description: Bir Azure Search zenginleştirme ardışık metinden pozitif ve negatif yaklaşım puanını ayıklayın.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.date: 01/17/2019
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: 0661fb3e839f62a854ccace7477da4c7bf1a4c4c
ms.sourcegitcommit: 82cdc26615829df3c57ee230d99eecfa1c4ba459
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2019
ms.locfileid: "54410943"
---
#   <a name="sentiment-cognitive-skill"></a>Yaklaşım bilişsel beceri

**Yaklaşım** beceri yapılandırılmamış metinleri pozitif ve negatif sürekliliği ve her kayıt için değerlendirir, 0 ile 1 arasında bir sayısal puan döndürür. Puanın 1’e yakın olması yaklaşımın olumlu olduğunu, 0’a yakın olması ise olumsuz olduğunu gösterir. Bu yetenek, makine öğrenimi modellerini tarafından sağlanan kullanan [metin analizi](https://docs.microsoft.com/azure/cognitive-services/text-analytics/overview) Bilişsel Hizmetler'e gösterdiğiniz.

> [!NOTE]
> Yapabilecekleriniz 21 aralık 2018 tarihinden itibaren [Bilişsel hizmetler kaynağı ekleme](cognitive-search-attach-cognitive-services.md) ile bir Azure Search beceri kümesi. Bu beceri yürütmesi için ücretlendirmeye başlarız olanak tanır. Bu tarihte, biz de belge çözme aşamasının bir parçası olarak görüntü ayıklama için ücretlendirme başladı. Metin ayıklama belgelerden hiçbir ek ücret ödemeden sunulmaya devam eder.
>
> [Bilişsel yerleşik yetenek](cognitive-search-predefined-skills.md) yürütme ücretlendirilir [Bilişsel hizmetler ödeme-olarak-, Git fiyat](https://azure.microsoft.com/pricing/details/cognitive-services), adresindeki görevi doğrudan gerçekleştirilen gibi aynı oranı. Görüntü ayıklama şu anda Önizleme fiyatlandırması sunulan bir Azure Search ücrettir. Ayrıntılar için bkz [Azure fiyatlandırma sayfasını arama](https://go.microsoft.com/fwlink/?linkid=2042400) veya [nasıl](search-sku-tier.md#how-billing-works).

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.SentimentSkill

## <a name="data-limits"></a>Veri sınırları
Bir kaydın en büyük boyutu tarafından ölçülen 5000 karakter arasında olmalıdır `String.Length`. Kullanım, yaklaşım Çözümleyicisi göndermeden önce verileri bölün gerekiyorsa [metin bölme beceri](cognitive-search-skill-textsplit.md).


## <a name="skill-parameters"></a>Yetenek parametreleri

Parametreler büyük/küçük harfe duyarlıdır.

| Parametre Adı |                      |
|----------------|----------------------|
| defaultLanguageCode | (isteğe bağlı) Dil açıkça belirtmeyin belgelerine uygulamak için dil kodu. <br/> Bkz: [desteklenen dillerin tam listesini](../cognitive-services/text-analytics/text-analytics-supported-languages.md) |

## <a name="skill-inputs"></a>Beceri girişleri 

| Adı girin | Açıklama |
|--------------------|-------------|
| metin | Analiz edilecek metin.|
| languageCode  |  (İsteğe bağlı) Kayıt dili belirten bir dize. Bu parametre belirtilmezse, varsayılan değer "en" dir. <br/>Bkz: [desteklenen dillerin tam listesini](../cognitive-services/text-analytics/text-analytics-supported-languages.md).|

## <a name="skill-outputs"></a>Beceri çıkışları

| Çıkış adı | Açıklama |
|--------------------|-------------|
| puan | 0 ve 1 arasında bir değer çözümlenmiş metin duyarlılığını temsil eder. Olumsuz 0 değerine sahip, nötr yaklaşım 0,5 yakın olması ve pozitif yaklaşımı 1 değerine sahip.|


##  <a name="sample-definition"></a>Örnek tanımı

```json
{
    "@odata.type": "#Microsoft.Skills.Text.SentimentSkill",
    "inputs": [
        {
            "name": "text",
            "source": "/document/content"
        },
        {
            "name": "languageCode",
            "source": "/document/languagecode"
        }
    ],
    "outputs": [
        {
            "name": "score",
            "targetName": "mySentiment"
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
                "text": "I had a terrible time at the hotel. The staff was rude and the food was awful.",
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
                "score": 0.01
            }
        }
    ]
}
```

## <a name="notes"></a>Notlar
Boş ise, yaklaşım puanını kayıtları için döndürülmez.

## <a name="error-cases"></a>Hata durumları
Bir dil desteklenmiyorsa, bir hata oluşturulur ve hiçbir yaklaşım puanı döndürülür.

## <a name="see-also"></a>Ayrıca bkz.

+ [Önceden tanımlanmış beceriler](cognitive-search-predefined-skills.md)
+ [Bir beceri kümesi tanımlama](cognitive-search-defining-skillset.md)