---
title: Çeviri özelleştirmesi-çevirmen
titleSuffix: Azure Cognitive Services
description: Tercih ettiğiniz terminolojiyi ve stili kullanarak kendi makine çevirisi sisteminizi oluşturmak için Microsoft Translator hub 'ını kullanın.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 05/26/2020
ms.author: swmachan
ms.openlocfilehash: 95cb4aa5827190abf125669f2423c808cf8c92a5
ms.sourcegitcommit: 22da82c32accf97a82919bf50b9901668dc55c97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/08/2020
ms.locfileid: "94368942"
---
# <a name="customize-your-text-translations"></a>Metin çevirilerinizi özelleştirin

Özel çevirici, çevirmen hizmetinin, kullanıcıların, çeviriciyi (yalnızca sürüm 3) kullanarak metin çevirirken Microsoft Translator 'ın gelişmiş sinir makine çevirisini özelleştirmesini sağlayan, Translator hizmetinin bir özelliğidir.

Özelliği, bilişsel [Hizmetler konuşmayla](../speech-service/index.yml)kullanıldığında konuşma çevirisini özelleştirmek için de kullanılabilir.

## <a name="custom-translator"></a>Özel Çevirmen

Özel çevirmenle, kendi iş ve sektöründe kullanılan terminolojiyi anlayan sinir çeviri sistemleri oluşturabilirsiniz. Özelleştirilmiş çeviri sistemi daha sonra mevcut uygulamalar, iş akışları ve Web siteleri ile tümleşir.

### <a name="how-does-it-work"></a>Nasıl çalışır?

Daha önce çevrilmiş belgelerinizi (sızıntı, Web sayfaları, belgeler vb.) kullanarak, bir standart çeviri sisteminden daha iyi bir şekilde, etki alanına özgü terminolojiyi ve stilinizi yansıtan bir çeviri sistemi oluşturun. Kullanıcılar TMX, XLıFF, TXT, DOCX ve XLSX belgelerini karşıya yükleyebilir.  

Sistem ayrıca belge düzeyinde paralel olan ancak tümce düzeyinde henüz hizalanmayan verileri de kabul eder. Kullanıcılar birden çok dilde aynı içeriğin sürümlerine erişebiliyorsa, ancak ayrı belgeler özel çeviricisinde, belgelerde otomatik olarak tümceler eşleştirebilir.  Sistem Ayrıca, çevirileri geliştirmek üzere paralel eğitim verilerini tamamlamak için her iki dilde de monolingual verileri kullanabilir.

Özelleştirilmiş sistem daha sonra Kategori parametresi kullanılarak bir çevirmene çağrı yoluyla kullanılabilir.

Uygun tür ve eğitim verisi miktarı verildiğinde, 5 ile 10 arasında kazanç beklenmez veya özel çevirici kullanarak çeviri kalitesine daha fazla BLEU noktası gelir.

Kullanılabilir verilere göre çeşitli özelleştirme düzeyleri hakkında daha fazla ayrıntı için [özel çevirmen kullanıcı kılavuzunda](./custom-translator/overview.md)bulabilirsiniz.


## <a name="microsoft-translator-hub"></a>Microsoft Translator hub 'ı

> [!NOTE]
> Eski Microsoft Translator hub 'ı 17 Mayıs 2019 tarihinde kullanımdan kaldırılacaktır. [Önemli geçiş bilgilerini ve tarihlerini görüntüleyin](https://www.microsoft.com/translator/business/hub/).  

## <a name="custom-translator-versus-hub"></a>Özel Translator ile hub karşılaştırması

| Özellik | Hub | Özel Çevirmen |
| ------- | :-: | :---------------: |
|Özelleştirme özelliği durumu    | Genel kullanılabilirlik    | Genel kullanılabilirlik |
| Metin API 'SI sürümü    | Yalnızca v2    | Yalnızca v3 |
| SMT özelleştirmesi    | Yes    | Hayır |
| NMT özelleştirmesi    | Hayır    | Yes |
| Yeni Birleşik konuşma Hizmetleri özelleştirmesi    | Hayır    | Yes |
| [Izleme yok](https://www.aka.ms/notrace) | Yes    | Yes |

## <a name="collaborative-translations-framework"></a>İşbirliğine dayalı Çeviriler çerçevesi

> [!NOTE]
> 1 Şubat 2018 itibariyle, AddTranslation () ve AddTranslationArray (), Translator v 2.0 ile birlikte kullanılamaz. Bu yöntemler başarısız olur ve hiçbir şey yazılmaz. Translator v 3.0 bu yöntemleri desteklemez.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Özelleştirilmiş bir dil sistemini özel çevirici kullanarak ayarlama](./custom-translator/overview.md)