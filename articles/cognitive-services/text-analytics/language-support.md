---
title: Dil desteği-Metin Analizi API'si
titleSuffix: Azure Cognitive Services
description: Metin Analizi API'si tarafından desteklenen doğal dillerin bir listesi. Bu makalede, her bir işlem için hangi dillerin desteklendiği açıklanmaktadır.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 10/07/2020
ms.author: aahi
ms.openlocfilehash: ed2a5b4688965f790567018bc11051b77c494e7a
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/13/2020
ms.locfileid: "91977740"
---
# <a name="text-analytics-api-v3-language-support"></a>Metin Analizi API'si v3 dil desteği 

> [!IMPORTANT]
> Metin Analizi API'si sürüm 3. x şu bölgelerde kullanılamıyor: Orta Hindistan, BAE Kuzey, Çin Kuzey 2, Çin Doğu.


#### <a name="sentiment-analysis"></a>[Yaklaşım Analizi](#tab/sentiment-analysis)

| Dil              | Dil kodu | v2 desteği | v3 desteği | V3 model sürümü başlatılıyor: |              Notlar |
|:----------------------|:-------------:|:----------:|:----------:|:--------------------------:|-------------------:|
| Basitleştirilmiş Çince    |   `zh-hans`   |     ✓      |     ✓      |         2019-10-01         | `zh` Ayrıca kabul edildi |
| Geleneksel Çince   |   `zh-hant`   |            |     ✓      |         2019-10-01         |                    |
| Danca               |     `da`      |     ✓      |            |                            |                    |
| Felemenkçe                 |     `nl`      |     ✓      |            |                            |                    |
| İngilizce               |     `en`      |     ✓      |     ✓      |         2019-10-01         |                    |
| Fince               |     `fi`      |     ✓      |            |                            |                    |
| Fransızca                |     `fr`      |     ✓      |     ✓      |         2019-10-01         |                    |
| Almanca                |     `de`      |     ✓      |     ✓      |         2019-10-01         |                    |
| Yunanca                 |     `el`      |     ✓      |            |                            |                    |
| Hintçe                 |     `hi`      |           |      ✓      |          2020-04-01                  |                    |
| İtalyanca               |     `it`      |     ✓      |     ✓      |         2019-10-01         |                    |
| Japonca              |     `ja`      |     ✓      |     ✓      |         2019-10-01         |                    |
| Korece                |     `ko`      |            |     ✓      |         2019-10-01         |                    |
| Norveççe (Bokmål)   |     `no`      |     ✓      |     ✓       |        2020-07-01         |                    |
| Lehçe                |     `pl`      |     ✓      |            |                            |                    |
| Portekizce (Portekiz) |    `pt-PT`    |     ✓      |     ✓      |         2019-10-01         | `pt` Ayrıca kabul edildi |
| Rusça               |     `ru`      |     ✓      |            |                            |                    |
| İspanyolca               |     `es`      |     ✓      |     ✓      |         2019-10-01         |                    |
| İsveççe               |     `sv`      |     ✓      |            |                            |                    |
| Türkçe               |     `tr`      |     ✓      |     ✓       |         2020-07-01        |                    |

### <a name="opinion-mining-v31-preview-only"></a>Görüşme madenciliği (v 3.1-yalnızca Önizleme)

| Dil              | Dil kodu | V3 modeli sürümünden itibaren: |              Notlar |
|:----------------------|:-------------:|:------------------------------------:|-------------------:|
| İngilizce               |     `en`      |              2020-04-01              |                    |


#### <a name="named-entity-recognition-ner"></a>[Adlandırılmış varlık tanıma (NER)](#tab/named-entity-recognition)

> [!NOTE]
> * NER v3 Şu anda yalnızca Ingilizce ve Ispanyolca dilleri desteklemektedir. NER v3 'i farklı bir dille çağırırsanız, dil sürüm 2,1 ' de desteklendiğinden, API v 2.1 sonuçları döndürür.
> * v 2.1 Yalnızca Ingilizce, Çince-Basitleştirilmiş, Fransızca, Almanca ve Ispanyolca dilleri için kullanılabilir varlıkların tam kümesini döndürür.  Desteklenen diğer diller için "kişi", "konum" ve "kuruluş" varlıkları döndürülür.

| Dil               | Dil kodu | v 2.1 desteği | v3 desteği | V3 modeli sürümünden itibaren: |       Notlar        |
|:-----------------------|:-------------:|:----------:|:----------:|:-------------------------------:|:------------------:|
| Arapça                |     `ar`      |     ✓      |            |                                 |                    |
| Çekçe                 |     `cs`      |     ✓      |            |                                 |                    |
| Basitleştirilmiş Çince     |   `zh-hans`   |     ✓      |            |                                 | `zh` Ayrıca kabul edildi |
| Geleneksel Çince   |   `zh-hant`   |     ✓      |            |                                 |                    |
| Danca                |     `da`      |     ✓      |            |                                 |                    |
| Felemenkçe                 |     `nl`      |     ✓      |            |                                 |                    |
| İngilizce                |     `en`      |     ✓      |     ✓      |           2019-10-01            |                    |
| Fince               |     `fi`      |     ✓      |            |                                 |                    |
| Fransızca                 |     `fr`      |     ✓      |            |                                 |                    |
| Almanca                 |     `de`      |     ✓      |            |                                 |                    |
| İbranice                |     `he`      |     ✓      |            |                                 |                    |
| Macarca             |     `hu`      |     ✓      |            |                                 |                    |
| İtalyanca               |     `it`      |     ✓      |            |                                 |                    |
| Japonca              |     `ja`      |     ✓      |            |                                 |                    |
| Korece                |     `ko`      |     ✓      |            |                                 |                    |
| Norveççe (Bokmål)   |     `no`      |     ✓      |            |                                 | `nb` Ayrıca kabul edildi |
| Lehçe                |     `pl`      |     ✓      |            |                                 |                    |
| Portekizce (Portekiz) |    `pt-PT`    |     ✓      |            |                                 | `pt` Ayrıca kabul edildi |
| Portekizce (Brezilya)   |    `pt-BR`    |     ✓      |            |                                 |                    |
| Rusça              |     `ru`      |     ✓      |            |                                 |                    |
| İspanyolca               |     `es`      |     ✓      |     ✓       |              2020-04-01                   |                    |
| İsveççe               |     `sv`      |     ✓      |            |                                 |                    |
| Türkçe               |     `tr`      |     ✓      |            |                                 |                    |

#### <a name="key-phrase-extraction"></a>[Anahtar ifade ayıklama](#tab/key-phrase-extraction)

> [!NOTE]
> 2020-07-01 ' den önceki Anahtar İfade Ayıklama model sürümlerinin 64 karakter sınırı vardır. Bu sınır sonraki model sürümlerinde yok.

| Dil              | Dil kodu | v2 desteği | v3 desteği | V3 model sürümü ile başlayarak kullanılabilir: |       Notlar        |
|:----------------------|:-------------:|:----------:|:----------:|:-----------------------------------------:|:------------------:|
| Felemenkçe                 |     `nl`      |     ✓      |     ✓      |                2019-10-01                 |                    |
| İngilizce               |     `en`      |     ✓      |     ✓      |                2019-10-01                 |                    |
| Fince               |     `fi`      |     ✓      |     ✓      |                2019-10-01                 |                    |
| Fransızca                |     `fr`      |     ✓      |     ✓      |                2019-10-01                 |                    |
| Almanca                |     `de`      |     ✓      |     ✓      |                2019-10-01                 |                    |
| İtalyanca               |     `it`      |     ✓      |     ✓      |                2019-10-01                 |                    |
| Japonca              |     `ja`      |     ✓      |     ✓      |                2019-10-01                 |                    |
| Korece                |     `ko`      |     ✓      |     ✓      |                2019-10-01                 |                    |
| Norveççe (Bokmål)   |     `no`      |     ✓      |     ✓      |                2019-10-01                 | `nb` Ayrıca kabul edildi |
| Lehçe                |     `pl`      |     ✓      |     ✓      |                2019-10-01                 |                    |
| Portekizce (Portekiz) |    `pt-PT`    |     ✓      |     ✓      |                2019-10-01                 | `pt` Ayrıca kabul edildi |
| Portekizce (Brezilya)   |    `pt-BR`    |     ✓      |     ✓      |                2019-10-01                 |                    |
| Rusça               |     `ru`      |     ✓      |     ✓      |                2019-10-01                 |                    |
| İspanyolca               |     `es`      |     ✓      |     ✓      |                2019-10-01                 |                    |
| İsveççe               |     `sv`      |     ✓      |     ✓      |                2019-10-01                 |                    |

#### <a name="entity-linking"></a>[Varlık bağlama](#tab/entity-linking)

| Dil | Dil kodu | v2 desteği | v3 desteği | V3 model sürümü ile başlayarak kullanılabilir: | Notlar |
|:---------|:-------------:|:----------:|:----------:|:-----------------------------------------:|:-----:|
| İngilizce  |     `en`      |     ✓      |     ✓      |                2019-10-01                 |       |
| İspanyolca  |     `es`      |     ✓      |     ✓      |                2019-10-01                 |       |

#### <a name="language-detection"></a>[Dil Algılama](#tab/language-detection)

Metin Analizi API'si, çok çeşitli diller, çeşitler, diapacts ve bazı bölgesel/kültürel dillerini algılayabilir.  Dil Algılama, bir dilin "betiğini" döndürür. Örneğin, "bir köpek var" ifadesi için  `en` yerine döndürülür  `en-US` . Tek özel durum, dil algılama yeteneğinin döndürdüğü `zh_CHS` veya bir `zh_CHT` komut dosyasını verilen metin olarak belirleyebileceği yalnızca Çince 'dir. Belirli bir betiğin bir Çince belge için belirlenemediği durumlarda, yalnızca döndürülür `zh` .

Bu özellik için dillerin tam listesini yayımlamadık, ancak çeşitli diller, çeşitler, diapacts ve bazı bölgesel/kültürel dillerini algılayabilir. 

Daha az sıklıkta kullanılan bir dilde ifade ettiğiniz bir içeriğiniz varsa, bir kodu döndürüp döndürdüğünü görmek için Dil Algılama deneyebilirsiniz. Tespit edilemez dillerin yanıtı `unknown` .

---

## <a name="see-also"></a>Ayrıca bkz.

* [Metin Analizi API'si nedir?](overview.md)   
