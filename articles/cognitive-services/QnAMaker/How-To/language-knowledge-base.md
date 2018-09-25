---
title: İngilizce olmayan Bilgi Bankası - soru-cevap Oluşturucu
titleSuffix: Azure Cognitive Services
description: Soru-cevap Oluşturucu, Bilgi Bankası içerikleri birçok dilde gösterilmesini destekler. Ancak her soru-cevap Oluşturucu hizmetini tek bir dil için ayrılmış olması. Belirli bir soru-cevap Oluşturucu hizmetini hedefleyen oluşturulan ilk Bilgi Bankası hizmet dili ayarlar.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: tulasim
ms.openlocfilehash: b983fb21e8e67a422b6757619d1d0dfe8b6b9dcc
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47033915"
---
# <a name="language-support-of-knowledge-base-content-for-qna-maker"></a>Bilgi Bankası içerikleri soru-cevap Oluşturucu için dil desteği
Soru-cevap Oluşturucu, Bilgi Bankası içerikleri birçok dilde gösterilmesini destekler. Ancak her soru-cevap Oluşturucu hizmetini tek bir dil için ayrılmış olması. Belirli bir soru-cevap Oluşturucu hizmetini hedefleyen oluşturulan ilk Bilgi Bankası hizmet dili ayarlar. Bkz: [burada](../Overview/languages-supported.md) desteklenen dillerin listesi.

Dil ayıklanırken veri kaynaklarının içeriği otomatik olarak kabul edilir. Bu hizmet yeni bir soru-cevap Oluşturucu hizmetini ve yeni Bilgi Bankası oluşturduktan sonra dil doğru şekilde ayarlandığını doğrulayabilirsiniz.

1. Gidin [Azure portalında](https://portal.azure.com/).

2. Seçin **kaynak grupları** ve soru-cevap Oluşturucu hizmeti, dağıtılmış ve seçin kaynak grubuna gidin **Azure Search** kaynak.

    ![Azure Search kaynağı seçin](../media/qnamaker-how-to-language-kb/select-azsearch.png)

3. Seçin **testkb** dizini. Bu Azure Search dizini her zaman oluşturulan ilk hesaptır ve bu hizmet, tüm bilgi bankalarından kaydedilmiş içeriğini içerir. 

    ![KB Test seçin](../media/qnamaker-how-to-language-kb/select-testkb.png)

4. Seçin **alanları** testkb ayrıntılarını gösteren bölüm.

    ![Alanları seçin](../media/qnamaker-how-to-language-kb/selectfields.png)

5. İçin kutuyu **Çözümleyicisi** dil ayrıntılarını görmek için.

    ![Çözümleyici seçin](../media/qnamaker-how-to-language-kb/select-analyzer.png)

6. Belirli bir dil Çözümleyicisi ayarlandığını bulmanız gerekir. Bu dil, Bilgi Bankası Oluşturma adımı sırasında otomatik olarak algılandı. Bu dil, kaynak oluşturulduktan sonra değiştirilemez.

    ![Seçili Çözümleyicisi](../media/qnamaker-how-to-language-kb/selected-analyzer.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Bot hizmeti ile bir soru-cevap Robotu oluşturun](../Tutorials/create-qna-bot.md)
