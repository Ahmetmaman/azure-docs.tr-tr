---
title: Veri dönüştürme-LUSıS
titleSuffix: Azure Cognitive Services
description: Language Understanding (LUSıS) ' de tahmine göre nasıl değiştirilebileceğinizi öğrenin
services: cognitive-services
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 07/29/2019
ms.openlocfilehash: 42a9caff0433808734ee853cbad90a2088bf4e1e
ms.sourcegitcommit: 10d00006fec1f4b69289ce18fdd0452c3458eca5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2020
ms.locfileid: "95019254"
---
# <a name="convert-data-format-of-utterances"></a>Söyleyenlerdeki veri biçimlerini Dönüştür
LUO, tahmine göre bir kullanıcı için aşağıdaki dönüştürmeleri sağlar "

* Bilişsel [Hizmetler konuşma](../Speech-Service/overview.md) hizmetini kullanarak konuşmayı metne dönüştürme.

## <a name="speech-to-text"></a>Konuşmayı metne dönüştürme

Konuşmadan metne, LUSıS ile tümleştirme olarak sunulmaktadır.

### <a name="intent-conversion-concepts"></a>Amaç dönüştürme kavramları
Luo 'da konuşmayı metne dönüştürme, bir uç noktaya konuşmalar göndermenizi ve bir LUSıS tahmini yanıtı almanızı sağlar. İşlem, LUSıS ile [konuşma](/azure/cognitive-services/Speech) hizmetinin bir Tümleştirmesidir. Bir [öğreticide](../speech-service/how-to-recognize-intents-from-speech-csharp.md)konuşma amacı hakkında daha fazla bilgi edinin.

### <a name="key-requirements"></a>Önemli gereksinimler
Bu tümleştirme için bir **Bing Konuşma API'si** anahtarı oluşturmanız gerekmez. Azure portal oluşturulan **Language Understanding** anahtarı bu tümleştirme için geçerlidir. Ludo başlangıç anahtarını kullanmayın.

### <a name="pricing-tier"></a>Fiyatlandırma Katmanı
Bu tümleştirme, her zamanki Language Understanding fiyatlandırma katmanlarından farklı bir [fiyatlandırma](luis-limits.md#key-limits) modeli kullanır.

### <a name="quota-usage"></a>Kota kullanımı
Bilgi için bkz. [anahtar sınırları](luis-limits.md#key-limits) .

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Verileri ayıklama](luis-concept-data-extraction.md)