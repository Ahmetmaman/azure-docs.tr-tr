---
title: Metin denetimi API'si - Content Moderator'ı kullanarak metin
titlesuffix: Azure Cognitive Services
description: Metin denetimi, çevrimiçi konsolda metin denetimi API'si kullanarak test edin.
services: cognitive-services
author: sanjeev3
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 08/05/2017
ms.author: sajagtap
ms.openlocfilehash: 794638496931f72a12fcb3bd8819b04c7e2e7c97
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55877057"
---
# <a name="moderate-text-from-the-api-console"></a>API Konsolu Orta metni

Kullanım [metin denetimi API'si](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66f) metin içeriğinizi taramak için Azure Content Moderator içinde. İşlem, içeriğinizi küfür tarar ve kara özel ve paylaşılan içerikte karşılaştırır.


## <a name="get-your-api-key"></a>API anahtarınızı alın
Çevrimiçi konsolunda API'yi test sürüşü önce abonelik anahtarınızı gerekir. Bu dosya çubuğunda bulunur **ayarları** sekmesinde **Ocp-Apim-Subscription-Key** kutusu. Daha fazla bilgi için bkz. [Genel Bakış](overview.md).

## <a name="navigate-to-the-api-reference"></a>API başvuruya Git
Git [metin denetimi API'si başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66f). 

  **Metin - ekran** sayfası açılır.

## <a name="open-the-api-console"></a>API Konsolu
İçin **açık API sınama Konsolu**, en yakın konumunuzu açıklayan bölgeyi seçin. 

  ![Metin - ekran sayfası kayıt seçimi](images/test-drive-region.png)

  **Metin - ekran** API konsolu açılır.

## <a name="select-the-inputs"></a>Girişleri seçin

### <a name="parameters"></a>Parametreler
Metin ekranınızın kullanmak istediğiniz sorgu parametreleri seçin. Bu örnekte, varsayılan değeri kullanın **dil**. İşlem yürütme işleminin bir parçası otomatik olarak olasılığı dil algılar olduğundan aynı zamanda boş bırakılabilir.

> [!NOTE]
> İçin **dil** parametresi, Ata `eng` veya makine destekli görmek için boş bırakın **sınıflandırma** yanıt (Önizleme özelliği). **Bu özellik yalnızca İngilizce destekler**.
>
> İçin **küfür koşulları** algılama, kullanımı [ISO 639-3 kodu](http://www-01.sil.org/iso639-3/codes.asp) bu konuda listelenen desteklenen dillerin makale veya boş bırakın.

İçin **düzeltme**, **PII**, ve **(Önizleme) sınıflandırmak**seçin **true**. Bırakın **ListId** boş.

  ![Metin - ekran konsol sorgu parametreleri](images/text-api-console-inputs.PNG)

### <a name="content-type"></a>İçerik türü
İçin **Content-Type**, ekran istediğiniz içerik türü seçin. Bu örnekte, varsayılan kullanmak **metin/düz** içerik türü. İçinde **Ocp-Apim-Subscription-Key** kutusuna, abonelik anahtarınızı girin.

### <a name="sample-text-to-scan"></a>Örnek metin taramak için
İçinde **istek gövdesi** kutusunda, metin girin. Aşağıdaki örnek, kasıtlı bir yazım yanlışı metni gösterir.

> [!NOTE]
> Aşağıdaki örnek metni geçersiz sosyal güvenlik numarası kasıtlıdır. Amacı, iletmek örnek giriş ve çıkış biçimi sağlamaktır.

```
    Is this a grabage or crap email abcdef@abcd.com, phone: 6657789887, IP: 255.255.255.255, 1 Microsoft Way, Redmond, WA 98052.
    These are all UK phone numbers, the last two being Microsoft UK support numbers: +44 870 608 4000 or 0344 800 2400 or 0800 820 3300.
    Also, 999-99-9999 looks like a social security number (SSN).
```

### <a name="text-classification-feature"></a>Metin sınıflandırma özelliği

Aşağıdaki örnekte, Content Moderator'ın makine destekli metin sınıflandırma yanıt görürsünüz. Potansiyel olarak istenmeyen içeriği belirlemenize yardımcı olur. Bayrak eklenen içeriği bağlama bağlı olarak uygun olarak sayılabilecek. Her kategori olasılığını yaygınlaşmıştır ek olarak, bu içeriğin bir insan tarafından İnceleme önerebilir. Özelliği, olası uygunsuz, negatif veya discriminatory dil tanımlamak için eğitilen bir modeli kullanır. Bu argo kullanımlar, kısaltılmış sözcükler, gözden geçirme için rahatsız edici ve bilerek yanlış yazılan sözcükleri içerir. 

#### <a name="explanation"></a>Açıklama

- `Category1` cinsel açık veya bazı durumlarda yetişkinlere yönelik olarak kabul edilebilir dil olası varlığını temsil eder.
- `Category2` cinsel müstehcen veya bazı durumlarda olgun düşünülebilecek dil olası varlığını temsil eder.
- `Category3` Belirli durumlarda rahatsız edici olduğu düşünülebilecek dil olası varlığını temsil eder.
- `Score` 0 ile 1 arasındadır. Yüksek puanı, kategori uygun olabilir yüksek modeli tahmin etmektir. Bu önizleme, el ile kodlanmış sonuçları yerine istatistiksel bir model kullanır. Her kategori için gereksinimlerinizi nasıl hizalandığını belirlemek için kendi içeriğe sahip test etmenizi öneririz.
- `ReviewRecommended` true veya false iç puanına göre eşikleri bağlı değil. Müşteriler, bu değeri kullanın veya kendi içerik ilkelere dayalı özel eşikler karar değerlendirmelisiniz.

### <a name="analyze-the-response"></a>Yanıt analiz edin
Şu yanıtı çeşitli içgörüler API'den gösterir. Bu, olası küfürleri, PII, sınıflandırma (Önizleme) ve otomatik olarak düzeltti sürümünü içerir.

> [!NOTE]
> Makine destekli 'Sınıflandırma' özellik Önizleme aşamasındadır ve yalnızca İngilizce dilini desteklemektedir.

```
{
    "OriginalText": "Is this a grabage or crap email abcdef@abcd.com, phone: 6657789887, IP: 255.255.255.255, 1 Microsoft Way, Redmond, WA 98052.\r\nThese are all UK phone numbers, the last two being Microsoft UK support numbers: +44 870 608 4000 or 0344 800 2400 or 0800 820 3300.\r\nAlso, 544-56-7788 looks like a social security number (SSN).",
    "NormalizedText": "Is this a grabage or crap email abcdef@ abcd. com, phone: 6657789887, IP: 255. 255. 255. 255, 1 Microsoft Way, Redmond, WA 98052. \r\nThese are all UK phone numbers, the last two being Microsoft UK support numbers: +44 870 608 4000 or 0344 800 2400 or 0800 820 3300. \r\nAlso, 544- 56- 7788 looks like a social security number ( SSN) .",
"Misrepresentation": null,
    "PII": {
            "Email": [{
                "Detected": "abcdef@abcd.com",
                "SubType": "Regular",
                "Text": "abcdef@abcd.com",
                "Index": 32
                }],
            "IPA": [{
                "SubType": "IPV4",
                "Text": "255.255.255.255",
                "Index": 72
                }],
            "Phone": [{
                "CountryCode": "US",
                "Text": "6657789887",
                "Index": 56
                }, {
                "CountryCode": "US",
                "Text": "870 608 4000",
                "Index": 211
                }, {
                "CountryCode": "UK",
                "Text": "+44 870 608 4000",
                "Index": 207
                }, {
                "CountryCode": "UK",
                "Text": "0344 800 2400",
                "Index": 227
                }, {
                "CountryCode": "UK",
                "Text": "0800 820 3300",
                "Index": 244
            }],
         "Address": [{
                 "Text": "1 Microsoft Way, Redmond, WA 98052",
                "Index": 89
            }],
            "SSN": [{
                "Text": "999999999",
                "Index": 56
            }, {
                "Text": "999-99-9999",
                "Index": 266
            }]
        },
    "Classification": {
        "ReviewRecommended": true,
        "Category1": {
            "Score": 1.5113095059859916E-06
        },
        "Category2": {
            "Score": 0.12747249007225037
        },
        "Category3": {
            "Score": 0.98799997568130493
        }
        },
    "Language": "eng",
    "Terms": [{
            "Index": 21,
            "OriginalIndex": 21,
            "ListId": 0,
         "Term": "crap"
        }],
    "Status": {
            "Code": 3000,
            "Description": "OK",
            "Exception": null
        },
     "TrackingId": "2eaa012f-1604-4e36-a8d7-cc34b14ebcb4"
}
```

JSON yanıtı tüm bölümlerinde ayrıntılı bir açıklaması için başvurmak [metin denetimi API'si genel bakış](text-moderation-api.md).

## <a name="next-steps"></a>Sonraki adımlar

Kodunuzda REST API kullanma veya ile başlayan [metin denetimi .NET hızlı](text-moderation-quickstart-dotnet.md) uygulamanızla tümleştirmek için.
