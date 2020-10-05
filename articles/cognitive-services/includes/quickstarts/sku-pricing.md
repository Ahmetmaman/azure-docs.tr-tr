---
title: Bilişsel hizmet SKU 'Ları ve fiyatlandırma
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 09/01/2020
ms.author: pafarley
ms.openlocfilehash: cb066ecc1dc11985c0ccb0fa687a15d6b038b9fa
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2020
ms.locfileid: "89321609"
---
Aşağıdaki SKU 'ların ve fiyatlandırma bilgilerinin listesine bakın. 

#### <a name="multi-service"></a>Çoklu hizmet

| Hizmet                    | Tür                      |
|----------------------------|---------------------------|
| Birden çok hizmet. Daha fazla bilgi için [fiyatlandırma](https://azure.microsoft.com/pricing/details/cognitive-services/) sayfasına bakın.            | `CognitiveServices`     |


#### <a name="vision"></a>Görsel

| Hizmet                    | Tür                      |
|----------------------------|---------------------------|
| Görüntü İşleme            | `ComputerVision`          |
| Özel Görüntü İşleme tahmin | `CustomVision.Prediction` |
| Özel Görüntü İşleme-eğitim   | `CustomVision.Training`   |
| Yüz                       | `Face`                    |
| Form Tanıma            | `FormRecognizer`          |
| Mürekkep Tanıma             | `InkRecognizer`           |

#### <a name="search"></a>Arama

| Hizmet            | Tür                  |
|--------------------|-----------------------|
| Bing Otomatik Öneri   | `Bing.Autosuggest.v7` |
| Bing Özel Arama | `Bing.CustomSearch`   |
| Bing Varlık Arama | `Bing.EntitySearch`   |
| Bing Arama        | `Bing.Search.v7`      |
| Bing Yazım Denetimi   | `Bing.SpellCheck.v7`  |

#### <a name="speech"></a>Konuşma

| Hizmet            | Tür                 |
|--------------------|----------------------|
| Konuşma Hizmetleri    | `SpeechServices`     |
| Konuşma Tanıma | `SpeakerRecognition` |

#### <a name="language"></a>Dil

| Hizmet            | Tür                |
|--------------------|---------------------|
| Form anlama | `FormUnderstanding` |
| LUIS               | `LUIS`              |
| Soru-Cevap Oluşturucu          | `QnAMaker`          |
| Metin Analizi     | `TextAnalytics`     |
| Metin Çevirisi   | `TextTranslation`   |

#### <a name="decision"></a>Karar

| Hizmet           | Tür               |
|-------------------|--------------------|
| Anomali Algılayıcısı  | `AnomalyDetector`  |
| Content Moderator | `ContentModerator` |
| Kişiselleştirme      | `Personalizer`     |


#### <a name="pricing-tiers-and-billing"></a>Fiyatlandırma katmanları ve faturalandırma

Fiyatlandırma katmanları (ve faturalandırılan miktar), kimlik doğrulama bilgilerinizi kullanarak göndereceğiniz işlem sayısına bağlıdır. Her fiyatlandırma katmanı şunları belirtir:
* saniye başına izin verilen en fazla işlem sayısı (TPS).
* fiyatlandırma katmanında etkinleştirilen hizmet özellikleri.
* önceden tanımlı işlem sayısı için maliyet. Bu sayının üzerine gitmek, hizmetinize ilişkin [fiyatlandırma ayrıntılarında](https://azure.microsoft.com/pricing/details/cognitive-services/custom-vision-service/) belirtilen ek ücret oluşmasına neden olur.

> [!NOTE]
> Bilişsel hizmetlerin birçoğu hizmeti denemek için kullanabileceğiniz ücretsiz bir katmana sahiptir. Ücretsiz katmanı kullanmak için, `F0` kaynağınız IÇIN SKU olarak kullanın.