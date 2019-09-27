---
title: Dil desteği
titleSuffix: Azure Cognitive Services
description: Azure Bilişsel hizmetler görmek, duymak, ile konuşun ve kullanıcılarınızın anlamak uygulamalar oluşturmanıza olanak sağlar. Bu hizmetler arasında uygulamanızla doğal şekilde iletişim kurmasına izin vererek düzine üç'den fazla dil desteklenir.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.topic: conceptual
ms.date: 09/26/2019
ms.author: erhopf
ms.custom: seodec18
ms.openlocfilehash: d6519ad5a130eee25ab17135e26d7207047dcf7a
ms.sourcegitcommit: e9936171586b8d04b67457789ae7d530ec8deebe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71327270"
---
# <a name="natural-language-support-for-azure-cognitive-services"></a>Azure Bilişsel hizmetler için doğal dil desteği

Azure Bilişsel hizmetler görmek, duymak, ile konuşun ve kullanıcılarınızın anlamak uygulamalar oluşturmanıza olanak sağlar. Bu hizmetler arasında uygulamanızla doğal şekilde iletişim kurmasına izin vererek düzine üç'den fazla dil desteklenir.

Bu makalede, iki bölüme ayrılır. İlk Azure Bilişsel hizmetler yaygın olarak desteklenen çekirdek dilleri listeler. İkinci bölümde hizmetlerinin her biri için ek dil desteği ele alınmaktadır. Dil kullanılabilirlik, ülke/bölge ve Pazar göre değişiklik gösterebilir.

## <a name="core-languages"></a>Çekirdek dil

Bu çekirdek diller arasında Azure Bilişsel hizmetler desteklenir:

* Çince
* Türkçe
* Fransızca
* Almanca
* İtalyanca
* Japonca
* Korece ¹
* Portekizce
* İspanyolca

> [!NOTE]
> ¹ LUU, Video Indexer, Metin Analizi ve konuşmaya metin desteklenmez.

## <a name="additional-language-availability-by-service"></a>Ek dil hizmeti kullanılabilirliği

Bu tablolar dil kullanılabilirlik hizmet kategoriye göre Vurgula; hariç tutulan çekirdek diller. Ek dil desteği ve ülke/bölge ve Pazar kullanılabilirlik hakkında bilgi edinmek için aşağıdaki bağlantıları kullanın.

### <a name="vision"></a>Görsel

| | Arapça | Bulgarca | Katalanca | Hırvatça | Çekçe | Danca | Felemenkçe | Estonca | Fince | Yunanca | Hintçe | Macarca | İzlanda dili | Endonezya dili | Letonca | Litvanca | Malay dili | Norveççe | Lehçe | Rumence | Rusça | Sırpça | Slovakça | Slovence | İsveççe | Tamil dili | Tay Dili | Türkçe | Ukrayna dili | Vietnam dili |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| [Bilgisayar vizyonu: OCR @ NO__T-0 | :heavy_check_mark: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | : heavy_minus_sign: | :heavy_check_mark: | :heavy_check_mark: | : heavy_minus_sign: | :heavy_check_mark: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | : heavy_minus_sign: | :heavy_check_mark: | : heavy_minus_sign: | : heavy_minus_sign: | :heavy_check_mark: | : heavy_minus_sign: | : heavy_minus_sign: |
| [Video Indexer: Konuşmayı metne dönüştürme @ no__t-0 | :heavy_check_mark: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | :heavy_check_mark: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: |
| [Video Indexer: Konuşma çevirisi @ no__t-0 | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | : heavy_minus_sign: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |

### <a name="speech"></a>Konuşma

| | Arapça | Bulgarca | Katalanca | Hırvatça | Çekçe | Danca | Felemenkçe | Estonca | Fince | Yunanca | Hintçe | Macarca | İzlanda dili | Endonezya dili | Letonca | Litvanca | Malay dili | Norveççe | Lehçe | Rumence | Rusça | Sırpça | Slovakça | Slovence | İsveççe | Tamil dili | Tay Dili | Türkçe | Ukrayna dili | Vietnam dili |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| [Özel konuşma tanıma](https://docs.microsoft.com/azure/cognitive-services/custom-speech-service/customspeech-how-to-topics/cognitive-services-custom-speech-change-locale) | :heavy_check_mark: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | :heavy_check_mark: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | :heavy_check_mark: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | :heavy_check_mark: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | :heavy_check_mark: | : heavy_minus_sign: | : heavy_minus_sign: |
| [Speech hizmeti: Konuşmayı metne dönüştürme @ no__t-0 | :heavy_check_mark: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | :heavy_check_mark: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | :heavy_check_mark: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | :heavy_check_mark: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: |
| [Konuşma hizmeti: metin-için-konuşma tanıma](https://docs.microsoft.com/azure/cognitive-services/speech-service/supported-languages#text-to-speech) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | : heavy_minus_sign: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | : heavy_minus_sign: | :heavy_check_mark: | : heavy_minus_sign: | : heavy_minus_sign: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | : heavy_minus_sign: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | : heavy_minus_sign: | :heavy_check_mark: |
| [Speech hizmeti: Konuşma çevirisi @ no__t-0 | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | : heavy_minus_sign: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |

### <a name="language"></a>Dil

| | Arapça | Bulgarca | Katalanca | Hırvatça | Çekçe | Danca | Felemenkçe | Estonca | Fince | Yunanca | Hintçe | Macarca | İzlanda dili | Endonezya dili | Letonca | Litvanca | Malay dili | Norveççe | Lehçe | Rumence | Rusça | Sırpça | Slovakça | Slovence | İsveççe | Tamil dili | Tay Dili | Türkçe | Ukrayna dili | Vietnam dili |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| [Bing Yazım Denetimi](https://docs.microsoft.com/azure/cognitive-services/bing-spell-check/bing-spell-check-supported-languages) | :heavy_check_mark: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | :heavy_check_mark: | :heavy_check_mark: | : heavy_minus_sign: | :heavy_check_mark: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | :heavy_check_mark: | :heavy_check_mark: | : heavy_minus_sign: | :heavy_check_mark: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | :heavy_check_mark: | : heavy_minus_sign: | : heavy_minus_sign: | :heavy_check_mark: | : heavy_minus_sign: | : heavy_minus_sign: |
| [Language Understanding (LUIS)](https://docs.microsoft.com/azure/cognitive-services/luis/luis-supported-languages) | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | :heavy_check_mark: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | :heavy_check_mark: | : heavy_minus_sign: | : heavy_minus_sign: |
| [Soru-Cevap Oluşturma](https://docs.microsoft.com/azure/cognitive-services/qnamaker/overview/languages-supported) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Translator Metin Çevirisi](https://docs.microsoft.com/azure/cognitive-services/translator/languages) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Metin Analizi](https://docs.microsoft.com/azure/cognitive-services/text-analytics/text-analytics-supported-languages) | :heavy_check_mark: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | : heavy_minus_sign: | :heavy_check_mark: | :heavy_check_mark: | : heavy_minus_sign: | :heavy_check_mark: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | :heavy_check_mark: | :heavy_check_mark: | : heavy_minus_sign: | :heavy_check_mark: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | :heavy_check_mark: | : heavy_minus_sign: | : heavy_minus_sign: | :heavy_check_mark: | : heavy_minus_sign: | : heavy_minus_sign: |

### <a name="search"></a>Ara

| | Arapça | Bulgarca | Katalanca | Hırvatça | Çekçe | Danca | Felemenkçe | Estonca | Fince | Yunanca | Hintçe | Macarca | İzlanda dili | Endonezya dili | Letonca | Litvanca | Malay dili | Norveççe | Lehçe | Rumence | Rusça | Sırpça | Slovakça | Slovence | İsveççe | Tamil dili | Tay Dili | Türkçe | Ukrayna dili | Vietnam dili |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| [Bing Web Araması](https://docs.microsoft.com/azure/cognitive-services/bing-web-search/supported-countries-markets) | :heavy_check_mark: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | :heavy_check_mark: | :heavy_check_mark: | : heavy_minus_sign: | :heavy_check_mark: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | :heavy_check_mark: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | :heavy_check_mark: | :heavy_check_mark: | : heavy_minus_sign: | :heavy_check_mark: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | :heavy_check_mark: | : heavy_minus_sign: | : heavy_minus_sign: | :heavy_check_mark: | : heavy_minus_sign: | : heavy_minus_sign: |
| [Bing Resim Arama](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/supported-countries-markets) | :heavy_check_mark: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | :heavy_check_mark: | :heavy_check_mark: | : heavy_minus_sign: | :heavy_check_mark: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | :heavy_check_mark: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | :heavy_check_mark: | :heavy_check_mark: | : heavy_minus_sign: | :heavy_check_mark: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | :heavy_check_mark: | : heavy_minus_sign: | : heavy_minus_sign: | :heavy_check_mark: | : heavy_minus_sign: | : heavy_minus_sign: |
| [Bing Haber Arama](https://docs.microsoft.com/azure/cognitive-services/bing-news-search/supported-countries-markets) | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | :heavy_check_mark: | :heavy_check_mark: | : heavy_minus_sign: | :heavy_check_mark: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | :heavy_check_mark: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | :heavy_check_mark: | : heavy_minus_sign: | : heavy_minus_sign: | :heavy_check_mark: | : heavy_minus_sign: | : heavy_minus_sign: |
| [Bing Otomatik Öneri](https://docs.microsoft.com/azure/cognitive-services/Bing-Autosuggest/bing-autosuggest-supported-languages) | :heavy_check_mark: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | :heavy_check_mark: | :heavy_check_mark: | : heavy_minus_sign: | :heavy_check_mark: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | :heavy_check_mark: | :heavy_check_mark: | : heavy_minus_sign: | :heavy_check_mark: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | :heavy_check_mark: | : heavy_minus_sign: | : heavy_minus_sign: | :heavy_check_mark: | : heavy_minus_sign: | : heavy_minus_sign: |
| [Bing Görsel Arama](https://docs.microsoft.com/azure/cognitive-services/bing-visual-search/supported-countries-markets) | :heavy_check_mark: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | :heavy_check_mark: | :heavy_check_mark: | : heavy_minus_sign: | :heavy_check_mark: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | :heavy_check_mark: | : heavy_minus_sign: | :heavy_check_mark: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | :heavy_check_mark: | : heavy_minus_sign: | : heavy_minus_sign: | :heavy_check_mark: | : heavy_minus_sign: | : heavy_minus_sign: |
| [Bing Özel Arama](https://docs.microsoft.com/azure/cognitive-services/bing-custom-search/supported-countries-markets) | :heavy_check_mark: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | :heavy_check_mark: | :heavy_check_mark: | : heavy_minus_sign: | :heavy_check_mark: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | :heavy_check_mark: | :heavy_check_mark: | : heavy_minus_sign: | :heavy_check_mark: | : heavy_minus_sign: | : heavy_minus_sign: | : heavy_minus_sign: | :heavy_check_mark: | : heavy_minus_sign: | : heavy_minus_sign: | :heavy_check_mark: | : heavy_minus_sign: | : heavy_minus_sign: |

### <a name="decision"></a>Karar verme

| | Arapça | Bulgarca | Katalanca | Hırvatça | Çekçe | Danca | Felemenkçe | Estonca | Fince | Yunanca | Hintçe | Macarca | İzlanda dili | Endonezya dili | Letonca | Litvanca | Malay dili | Norveççe | Lehçe | Rumence | Rusça | Sırpça | Slovakça | Slovence | İsveççe | Tamil dili | Tay Dili | Türkçe | Ukrayna dili | Vietnam dili |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| [Içerik Yöneticisi: Metin filtreleme @ no__t-0 | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |

Aşağıdaki bilişsel hizmetler dilden bağımsız bir dildir ve dile dayalı sınırlamalara sahip değildir.

* [Kişiselleştirici (Önizleme)](https://docs.microsoft.com/azure/cognitive-services/personalizer/)
* [Anomali algılayıcısı (Önizleme)](https://docs.microsoft.com/azure/cognitive-services/anomaly-detector/overview)


## <a name="see-also"></a>Ayrıca bkz.

* [Bilişsel Hizmetler nedir?](welcome.md)
* [Hesap oluşturma](cognitive-services-apis-create-account.md)
