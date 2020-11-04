---
title: Machine Learning sorumlu nedir (Önizleme)
titleSuffix: Azure Machine Learning
description: Machine Learning 'in ne olduğunu ve nasıl kullanılacağını öğrenin Azure Machine Learning
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: luquinta
author: luisquintanilla
ms.date: 08/05/2020
ms.openlocfilehash: f2ece76ef3594cadfa1fcda89259ee18eb1f9084
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2020
ms.locfileid: "93324857"
---
# <a name="what-is-responsible-machine-learning-preview"></a>Machine Learning sorumlu nedir? (önizleme)

Bu makalede, sorumlu makine öğrenimi (ML) ne olduğunu ve Azure Machine Learning ile uygulamaya nasıl koyabileceğinizi öğreneceksiniz.

## <a name="responsible-machine-learning-principles"></a>Sorumlu makine öğrenimi ilkeleri

AI sistemlerinin geliştirilmesi ve kullanımı boyunca güvenin çekirdek üzerinde olması gerekir. Platformda, süreçte ve modellerde güvenin. Microsoft 'ta, sorumlu makine öğrenimi aşağıdaki değerleri ve ilkeleri kapsar:

- Makine öğrenimi modellerini anlama
  - Model davranışını yorumlama ve açıklama
  - Model unfairness değerlendir ve azaltma
- Kişileri ve verilerini koruyun
  - Değişiklik gizliliğiyle veri pozlamasını önleyin
  - Homomorphic şifrelemesi kullanarak şifrelenmiş verilerle çalışma
- Uçtan uca makine öğrenimi işlemini denetleme
  - Machine Learning yaşam döngüsünü veri sayfaları ile belgeleme

:::image type="content" source="media/concept-responsible-ml/responsible-ml-pillars.png" alt-text="Sorumlu ML pillerini-yorumlilik, değişiklik gizliliği, homomorphic şifrelemesi, denetim izi-Azure Machine Learning":::

Yapay zeka ve özerk sistemler topluluğu dokusuna daha fazlasını tümleştirmekle, bu teknolojilerin istenmeden sonuçlý sonuçlarını tahmin etmek ve azaltmak için önceden önlem olarak bir çaba olması önemlidir.

## <a name="interpret-and-explain-model-behavior"></a>Model davranışını yorumlama ve açıklama

Sistem geliştiricileri, düzenleyiciler, kullanıcılar ve iş karar mekanizmalarının, sistemlerin neden belirli kararlar vermesini anlayabilmesi için zor olan sistemler, sistem geliştiricileri, düzenleyiciciler, kullanıcılar ve iş karar mekanizmaları gibi paydaşların sorun oluşturabileceğinden sorunlu olabilir. Bazı AI sistemleri diğerlerinden daha explainable ve bazı durumlarda, daha yüksek doğruluk ve daha fazla explainable olan bir sistem arasında bir zorunluluğunu getirir.

Intertable AI sistemleri oluşturmak için, Microsoft tarafından oluşturulan açık kaynaklı bir paket olan [ınterbir](https://github.com/interpretml/interpret)Package kullanın. Inter, [otomatik makine öğrenimi modelleri](how-to-machine-learning-interpretability-automl.md)de dahil olmak üzere [makine öğrenimi modellerinizi yorumlamak ve açıklamak](how-to-machine-learning-interpretability-aml.md)için, [Azure Machine Learning içinde yorumbir şekilde kullanılabilir](how-to-machine-learning-interpretability.md) .

## <a name="mitigate-fairness-in-machine-learning-models"></a>Makine öğrenimi modellerinde eşitliği 'i azaltma

AI sistemleri, topluluğu 'in günlük karar verme konusunda daha fazla sonuç aldığından, bu sistemlerin herkese açık sonuçlar sağlamak için iyi bir öneme sahip olma.

AI sistemlerindeki Unfairness, aşağıdaki istenmeyen sonuçlara neden olabilir:

- Vergi fırsatları, kaynaklar veya kişilerden gelen bilgiler.
- Sapmaları ve stereotipleri yeniden zorluyor.

Eşitliği 'in birçok yönü yakalanamaz veya ölçümler tarafından gösterilemez. AI sistemlerinin tasarımında ve geliştirilmesinde eşitliği iyileştirebilecek araçlar ve uygulamalar vardır.

AI sistemlerinde unfairness azaltmada iki temel adım değerlendirme ve risk azaltma. AI sistemlerinin olası unfairness 'i değerlendirebileceği ve azaltabilen açık kaynaklı bir paket olan [FairLearn](https://github.com/fairlearn/fairlearn)önerilir. Eşitliği ve FairLearn paketi hakkında daha fazla bilgi edinmek için bkz. [ml 'de eşitliği makalesi](./concept-fairness-ml.md).

## <a name="prevent-data-exposure-with-differential-privacy"></a>Değişiklik gizliliğiyle veri pozlamasını önleyin

Analiz için veriler kullanıldığında, verilerin kullanımı boyunca özel ve gizli olarak kalması önemlidir. Değişiklik gizliliği, kişilerin verilerinin güvende ve özel olmasını sağlamaya yardımcı olan bir sistem ve uygulamalar kümesidir.

Geleneksel senaryolarda, ham veriler dosya ve veritabanlarında depolanır. Kullanıcılar verileri çözümlerse genellikle ham verileri kullanır. Bu sorun, bir kişinin gizliliğine infringe olabileceğinden bir kaygıdır. Değişiklik gizliliği, kullanıcıların herhangi bir veri noktasını tanımlayamaması için verilere "gürültü" veya rasgelelik ekleyerek bu sorunla başa geçmeye çalışır.

Farklı şekilde özel sistemlerin uygulanması zordur. [Whitenoıse](https://github.com/opendifferentialprivacy/whitenoise-core) , küresel farklı özel sistemler oluşturmak için farklı bileşenler içeren açık kaynaklı bir projem. Değişiklik gizliliği ve Whitenoıse projesi hakkında daha fazla bilgi edinmek için, [değişiklik gizliliği ve çalışırken veri gizliliğini koruma](./concept-differential-privacy.md) makalesini inceleyin.

> [!NOTE]
> Araç takımını yeniden adlandırdığımızda, gelecek haftalarda yeni adı tanıtılacağını lütfen unutmayın. 

## <a name="work-on-encrypted-data-with-homomorphic-encryption"></a>Homomorphic şifrelemesi ile şifrelenmiş veriler üzerinde çalışma

Geleneksel bulut depolama ve hesaplama çözümlerinde, bulutun üzerinde işlem yapmak için müşteri verilerine şifrelenmemiş erişimi olması gerekir. Bu erişim, verileri bulut işleçlerine sunar. Veri gizliliği, bulut tarafından uygulanan ve müşteri tarafından güvenilen erişim denetimi ilkelerine dayanır.

Homomorphic şifrelemesi, bir gizli dizi (şifre çözme) anahtarına erişmek gerekmeden, şifrelenmiş verilerde hesaplamalar yapılmasına izin verir. Hesaplamaların sonuçları şifrelenir ve yalnızca gizli anahtar sahibi tarafından açığa çıkarlenebilir. Homomorphic şifrelemesi kullanarak, bulut işleçleri depolarken ve bilgi işlem yaptıkları verilere şifrelenmemiş olarak erişemez. Hesaplamalar doğrudan şifrelenmiş verilerde gerçekleştirilir. Veri gizliliği, son derece şifreleme durumuna dayanır ve veri sahibi tüm bilgi sürümlerini denetler. Microsoft 'ta homomorphic şifreleme hakkında daha fazla bilgi için bkz. [Microsoft Research](https://www.microsoft.com/research/project/homomorphic-encryption/).

Azure Machine Learning 'de homomorphic şifrelemeyi kullanmaya başlamak için, [Microsoft mühürlemek](https://github.com/microsoft/SEAL)üzere [şifrelenmiş çıkarım](https://pypi.org/project/encrypted-inference/) Python bağlamalarını kullanın. Microsoft mühürleme, şifreli tamsayılar veya gerçek sayılar üzerinde eklemeler ve çoğullar gerçekleştirilmesini sağlayan açık kaynaklı bir homomorphic şifreleme kitaplığıdır. Microsoft MÜHÜRLEMEK hakkında daha fazla bilgi için [Azure mimari merkezi](/azure/architecture/solution-ideas/articles/homomorphic-encryption-seal) veya [Microsoft Research projesi sayfasına](https://www.microsoft.com/research/project/microsoft-seal/)bakın.

[Azure Machine Learning ' de şifrelenmiş bir ınırm Web hizmeti dağıtmayı](how-to-homomorphic-encryption-seal.md)öğrenmek için aşağıdaki örneğe bakın.

## <a name="document-the-machine-learning-lifecycle-with-datasheets"></a>Machine Learning yaşam döngüsünü veri sayfaları ile belgeleme

Machine Learning işleminde doğru bilgilerin belgelenerek, her aşamada sorumlu kararlar alma anahtarıdır. Veri sayfaları, makine öğrenimi yaşam döngüsünün bir parçası olarak kullanılan ve oluşturulan makine öğrenimi varlıklarını belgelemek için bir yoldur.

Modeller "donuk kutular" olarak düşünüldük ve genellikle bunlar hakkında çok az bilgi vardır. Machine Learning sistemleri daha fazla bilgi edintiğinden ve karar vermek için kullanıldığından, veri sayfalarını kullanmak, daha sorumlu makine öğrenimi sistemleri geliştirmeye yönelik bir adımdır.

Bir veri sayfasının parçası olarak belgelemek isteyebileceğiniz bazı model bilgileri:

- Amaçlanan kullanım
- Model mimarisi
- Kullanılan eğitim verileri
- Kullanılan değerlendirme verileri
- Eğitim modeli performans ölçümleri
- Eşitliği bilgileri.

[Modeller için veri sayfalarını](https://github.com/microsoft/MLOps/blob/master/pytorch_with_datasheet/model_with_datasheet.ipynb)uygulamak üzere Azure Machine Learning SDK 'nın nasıl kullanılacağını öğrenmek için aşağıdaki örneğe bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- Daha fazla bilgi için, en iyi yöntemler hakkında bilgi edinmek için [sorumlu yenilik araç seti](/azure/architecture/guide/responsible-innovation/) bölümüne bakın.
- Machine Learning sistem belgeleri [hakkında](https://www.partnershiponai.org/about-ml/) daha fazla bilgi için bkz..