---
title: Yerleşik veri ayıklama, doğal dil işleme - Azure Search görüntü
description: Veri ayıklama, doğal dil, görüntü işleme bilişsel beceriler Ekle semantiği ve yapısı ham içeriği bir Azure Search işlem hattı.
manager: pablocas
author: luiscabrer
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: bc1353ffb4514622ce0ef6e5c3ced76adc7f999f
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53314797"
---
# <a name="predefined-skills-for-content-enrichment-azure-search"></a>İçerik zenginleştirme (Azure Search) için önceden tanımlanmış beceriler

Bu makalede, Azure Search ile sağlanan bilişsel beceriler hakkında bilgi edinin. A *bilişsel beceri* şekilde içeriği dönüştürür bir işlemdir. Genellikle, bu, verileri ayıklayan veya yapısı algılar ve bu nedenle ilişkin giriş verileri çoğaltan bir bileşendir. Neredeyse her zaman, çıktı metin tabanlıdır. A *beceri kümesi* zenginleştirme işlem hattı tanımlayın becerileri koleksiyonudur. 

> [!NOTE]
> 21 aralık 2018 tarihinden itibaren Bilişsel hizmetler kaynağı bir Azure Search beceri kümesi ile ilişkilendirmek mümkün olmayacak. Bu beceri yürütmesi için ücretlendirme başlatmak için bize izin verir. Bu tarihte, biz de belge çözme aşamasının bir parçası olarak görüntü ayıklama için başlayacağız. Belgelerden metin ayıklama işlemi ek masraf olmadan sağlanmaya devam edecektir.
>
> Var olan konumunda yerleşik yetenek yürütülmesini ücretlendirilir [Bilişsel hizmetler ödeme-olarak-, Git fiyat](https://azure.microsoft.com/pricing/details/cognitive-services/) . Görüntü ayıklama fiyatlandırma Önizleme fiyatıyla ücretlendirilirsiniz ve üzerinde açıklanmıştır [Azure fiyatlandırma sayfasını arama](https://go.microsoft.com/fwlink/?linkid=2042400). Bilgi [daha fazla](cognitive-search-attach-cognitive-services.md).

## <a name="predefined-skills"></a>Önceden tanımlanmış beceriler

Birkaç becerileri ne bunlar kullanma veya üretmek esnektir. Genel olarak, çoğu becerilerinizi kendi eğitim verilerini kullanarak modeli eğitme olamaz yani önceden eğitilmiş modeller üzerinde temel alır. Özel bir yetenek oluşturma hakkında yönergeler için bkz. [özel arabirim tanımlama](cognitive-search-custom-skill-interface.md) ve [örnek: özel bir yetenek oluşturma](cognitive-search-create-custom-skill-example.md). Aşağıdaki tabloda, sıralar ve Microsoft tarafından sağlanan yetenekleri açıklanır. 

| Nitelik | Açıklama |
|-------|-------------|
| [Microsoft.Skills.Text.KeyPhraseSkill](cognitive-search-skill-keyphrases.md) | Bu yetenek, terim yerleştirme, dilsel kurallar, diğer koşulları yakınlık ve nasıl olağan dışı kaynak verileri terimdir göre önemli tümcecikleri algılamak için pretrained modeli kullanır. |
| [Microsoft.Skills.Text.LanguageDetectionSkill](cognitive-search-skill-language-detection.md)  | Hangi dili algılayın pretrained modelidir bu beceri (belge başına bir dil kimliği) kullanır. Birden çok dil aynı metin Segmentte kullanıldığında, genellikle kullanılan dilin LCID çıkış alınır.|
| [Microsoft.Skills.Text.MergerSkill](cognitive-search-skill-textmerger.md) | Alanlar koleksiyonu tek bir alana metinden birleştirir.  |
| [Microsoft.Skills.Text.NamedEntityRecognitionSkill](cognitive-search-skill-named-entity-recognition.md) | Bu yetenek, varlıklar için sabit bir kategoriler kümesi oluşturmak için bir pretrained modeli kullanır: kişiler, konum, kuruluş. |
| [Microsoft.Skills.Text.SentimentSkill](cognitive-search-skill-sentiment.md)  | Bu yetenek, bir kayıt kayıt temelinde pozitif veya negatif yaklaşım puanını için pretrained modeli kullanır. Puan, 0 ile 1 arasında ' dir. Nötr puanları için null durumu yaklaşım algılandı ve metin, dilden bağımsız olarak kabul edilir ortaya çıkar.  |
| [Microsoft.Skills.Text.SplitSkill](cognitive-search-skill-textsplit.md) | Böylece zenginleştirin veya içeriği aşamalı olarak artırmak sayfalarına metin böler. |
| [Microsoft.Skills.Vision.ImageAnalysisSkill](cognitive-search-skill-image-analysis.md) | Bu yetenek, görüntü içeriğini tanımlamak ve bir metin açıklama oluşturmak için bir görüntü algılama algoritması kullanır. |
| [Microsoft.Skills.Vision.OcrSkill](cognitive-search-skill-ocr.md) | Optik karakter tanıma. |
| [Microsoft.Skills.Util.ShaperSkill](cognitive-search-skill-shaper.md) | Haritalar çıkışı bir karmaşık türü (tam ad, çok satırlı adresi ya da son adı ve bir kişisel kimlik için kullanılabilecek bir çok parçalı veri türü.) |

## <a name="see-also"></a>Ayrıca bkz.

+ [Bir beceri kümesi tanımlama](cognitive-search-defining-skillset.md)
+ [Özel becerileri arabirim tanımı](cognitive-search-custom-skill-interface.md)
+ [Öğretici: Zenginleştirilmiş bilişsel arama ile dizinleme](cognitive-search-tutorial-blob.md)