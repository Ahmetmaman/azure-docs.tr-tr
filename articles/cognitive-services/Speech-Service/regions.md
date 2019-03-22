---
title: Bölge - konuşma Hizmetleri
titlesuffix: Azure Cognitive Services
description: Konuşma hizmeti bölgeleri için başvuru.
services: cognitive-services
author: mahilleb-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/12/2019
ms.author: panosper
ms.custom: seodec18
ms.openlocfilehash: 1be51c4f5aede58a975dc9441b3bec7a83f7becd
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/18/2019
ms.locfileid: "57900922"
---
# <a name="speech-service-supported-regions"></a>Konuşma hizmeti desteklenen bölgeler

Konuşma hizmeti, uygulamanızın sesi metne dönüştürün, gizli metin okuma ve konuşma çevirisi gerçekleştirin sağlar. Hizmet, REST API'leri ve Speech SDK'sı için benzersiz uç noktaları ile birden fazla bölgede kullanılabilir.

Aboneliğiniz için bölge eşleşen uç nokta kullandığınızdan emin olun.

## <a name="speech-sdk"></a>Konuşma SDK'sı

İçinde [Speech SDK'sı](speech-sdk.md), bölgelerin bir dize olarak belirtilen (örneğin, bir parametre olarak `SpeechConfig.FromSubscription` konuşma SDK for C#).

### <a name="speech-recognition-and-translation"></a>Konuşma tanıma ve çeviri

Bu bölgeler için Speech SDK'sı kullanılabilir **konuşma tanıma** ve **çeviri**:

  Bölge | Konuşma SDK parametresi | Konuşma özelleştirme portalı
 ------|-------|--------
 Batı ABD | `westus` | https://westus.cris.ai
 Batı ABD 2 | `westus2` | https://westus2.cris.ai
 Doğu ABD | `eastus` | https://eastus.cris.ai
 Doğu ABD 2 | `eastus2` | https://eastus2.cris.ai
 Orta ABD | 'centralus' | https://centralus.cris.ai
 Orta Kuzey ABD | 'northcentralus' | https://northcentralus.cris.ai
 Orta Güney ABD | 'southcentralus' | https://southcentralus.cris.ai
 Doğu Asya | `eastasia` | https://eastasia.cris.ai
 Güneydoğu Asya | `southeastasia` | https://southeastasia.cris.ai
 Japonya Doğu | `japaneast` | https://japaneast.cris.ai
 Kore Orta | `koreacentral` | https://koreacentral.cris.ai
 Avustralya Doğu | `australiaeast` | https://australiaeast.cris.ai
 Orta Kanada | `canadacentral` | https://canadacentral.cris.ai
 Kuzey Avrupa | `northeurope` | https://northeurope.cris.ai
 Batı Avrupa | `westeurope` | https://westeurope.cris.ai
 Birleşik Krallık Güney | `uksouth` | https://uksouth.cris.ai
 Fransa Orta | `francecentral` | https://francecentral.cris.ai

### <a name="intent-recognition"></a>Amaç tanıma

Kullanılabilir bölgeler için **niyeti tanıma** Speech SDK'sı aşağıda verilmiştir:

 Genel bölge | Bölge | Konuşma SDK parametresi
 ------|-------|--------
 Asya | Doğu Asya | `eastasia`
 Asya | Güneydoğu Asya | `southeastasia`
 Avustralya | Avustralya Doğu | `australiaeast`
 Avrupa | Kuzey Avrupa | `northeurope`
 Avrupa | Batı Avrupa | `westeurope`
 Kuzey Amerika | Doğu ABD | `eastus`
 Kuzey Amerika | Doğu ABD 2 | `eastus2`
 Kuzey Amerika | Orta Güney ABD | `southcentralus`
 Kuzey Amerika | Batı Orta ABD | `westcentralus`
 Kuzey Amerika | Batı ABD | `westus`
 Kuzey Amerika | Batı ABD 2 | `westus2`
 Güney Amerika | Güney Brezilya | `brazilsouth`

Bu, bir alt kümesi tarafından desteklenen yayımlama bölgeler [Language Understanding hizmeti (LUIS)](/azure/cognitive-services/luis/luis-reference-regions).

## <a name="rest-apis"></a>REST API'leri

Konuşma hizmeti de konuşma metin ve metin okuma istekleri için REST uç noktalarını kullanıma sunar.

### <a name="speech-to-text"></a>Konuşmayı Metne Dönüştürme

Konuşmayı metne referans belgeleri için bkz. [REST API'leri](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis).

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-speech-to-text.md)]

### <a name="text-to-speech"></a>Metin okuma

Metin okuma referans belgeleri için bkz. [REST API'leri](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis).

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-text-to-speech.md)]
