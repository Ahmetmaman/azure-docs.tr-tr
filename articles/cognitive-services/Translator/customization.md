---
title: Çeviri özelleştirme - Translator metin çevirisi API'si
titlesuffix: Azure Cognitive Services
description: Microsoft Translator hub'ı tercih edilen terimleri ve stil kullanarak kendi makine çevirisi sistem oluşturmak için kullanın.
services: cognitive-services
author: Jann-Skotdal
manager: cgronlun
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: article
ms.date: 05/10/2018
ms.author: v-jansko
ms.openlocfilehash: e4e512a69fc783e6c4878298d848a9dccf8768c3
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55226936"
---
# <a name="customize-your-text-translations"></a>Metin çevirilerinizi özelleştirin

Microsoft özel Translator Translator Text API (yalnızca sürüm 3) kullanarak metin çevirme, Microsoft Translator'ın gelişmiş sinirsel makine çevirisi özelleştirme olanağı tanıyan Microsoft Translator hizmeti bir özelliğidir.

Bu özellik ile kullanıldığında konuşma çevirisi özelleştirmek için de kullanılabilir [Bilişsel hizmetler konuşma](https://docs.microsoft.com/azure/cognitive-services/speech-service/).

## <a name="custom-translator"></a>Özel Çevirmen

Özel Translator ile kendi iş ve sektör kullanılan terminolojiyi anladığınızdan sinirsel çeviri sistemleri oluşturabilirsiniz. Özelleştirilmiş çeviri sistemi ardından mevcut uygulamalarınızın, iş akışları ve Web siteleri ile tümleştirin.

### <a name="how-does-it-work"></a>Nasıl çalışır?

Önceden çevrilmiş belgelerinizi (leaflets, Web sayfaları, belgeler vb.) etki alanına özgü terminoloji ve stili daha iyi bir genel çeviri sistemi yansıtan bir çeviri sistem oluşturmak için kullanın. Kullanıcılar TMX, XLIFF, TXT, DOCX ve XLSX belgeler karşıya yükleyebilirsiniz.  

Sistem, ayrıca, belge düzeyinde paralel ancak henüz cümle düzeyinde hizalanmamış verileri kabul eder. Kullanıcılar, birden çok dilde ancak ayrı bir belge sürümleri aynı içeriğe erişimi varsa, özel Translator cümleler belgeler arasında otomatik olarak eşleşecek şekilde mümkün olacaktır.  Sistem tek dilli Kinsoku'ya veri veya her iki dilde çevirileri arttırmak için paralel eğitim verilerini tamamlamak için de kullanabilirsiniz.

Özelleştirilmiş bir sistem, ardından normal kategori parametresi kullanarak Microsoft Translator metin API'si çağrısı aracılığıyla kullanılabilir.

Eğitim veri miktarı ve uygun türe, kazanç 5 ve 10 arasında beklenecek yaygın görülen bir durumdur veya özel Translator kullanarak daha da fazla BLEU çeviri kalitesini işaret eder.

Mevcut verileri temel alan özelleştirme çeşitli düzeyleri hakkında daha fazla ayrıntı bulunabilir [özel Translator Kullanıcı Kılavuzu](https://aka.ms/CustomTranslatorDocs).


## <a name="microsoft-translator-hub"></a>Microsoft Translator Hub

Eski Microsoft Translator hub'ı, istatistiksel makine çevirisi çevirmek için kullanılabilir. [Daha fazla bilgi](https://www.microsoft.com/en-us/translator/hub.aspx)

## <a name="custom-translator-versus-hub"></a>Özel Translator Hub karşılaştırması

|   | **Hub** | **Özel Translator**|
|:-----|:----:|:----:|
|Özelleştirme özelliği durumu   | Genel Erişilebilirlik  | Genel Erişilebilirlik |
| Metin çevirisi API'si sürümü  | Yalnızca v2   | Yalnızca v3 |
| SMT özelleştirme | Evet   | Hayır |
| NMT özelleştirme | Hayır    | Evet |
| Yeni birleşik konuşma Hizmetleri özelleştirme | Hayır    | Evet |
| [İzleme yok](https://www.aka.ms/notrace) | Evet  | Evet |

## <a name="collaborative-translations-framework"></a>İşbirliğine dayalı çevirileri Framework

> [!NOTE]
> 1 Şubat 2018'den itibaren AddTranslation() ve AddTranslationArray() artık Translator Text API V2.0 ile kullanılabilir. Bu yöntemleri başarısız olur ve hiçbir şey yazılır. Translator metin API'si V3.0 bu yöntemleri desteklemez.

>Translator Hub API'SİNDE benzer işlevselliği kullanılabilir. Bkz: [ https://hub.microsofttranslator.com/swagger ](https://hub.microsofttranslator.com/swagger).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Özel Translator'ı kullanarak bir özelleştirilmiş dil sistemi](https://aka.ms/CustomTranslatorDocs)
