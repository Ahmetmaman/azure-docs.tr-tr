---
title: Makine öğrenimi algoritması seçme
titleSuffix: Azure Machine Learning
description: Kümeleme, sınıflandırma veya regresyon denemeleri için denetimli ve denetlenen öğrenme için Azure Machine Learning algoritmaları seçme.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.custom: how-to
author: FrancescaLazzeri
ms.author: lazzeri
ms.reviewer: cgronlun
ms.date: 05/07/2020
ms.openlocfilehash: 5ad0d2c520d5347e36a9cead4ed6c5526d885ca4
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "87319533"
---
# <a name="how-to-select-algorithms-for-azure-machine-learning"></a>Azure Machine Learning için algoritmalar seçme

Ortak bir soru, "hangi makine öğrenimi algoritmasını kullanmalıyım?" dir. Seçtiğiniz algoritma, birincil olarak veri bilimi senaryolarınızın iki farklı yönlerine bağlıdır:

 - **Verilerinize ne yapmak istiyorsunuz?** Özellikle, geçmiş verilerinize öğrenerek yanıtlamak istediğiniz iş sorusu nedir?

 - **Veri bilimi senaryonuz için gereksinimler nelerdir?** Özellikle, doğruluk, eğitim süresi, doğrity, parametre sayısı ve çözümünüzün desteklediği özellik sayısı nedir?

 ![Algoritmaların seçilmesiyle ilgili konular: ne bilmeniz istiyorsunuz? Senaryo gereksinimleri nelerdir?](./media/how-to-select-algorithms/how-to-select-algorithms.png)

## <a name="business-scenarios-and-the-machine-learning-algorithm-cheat-sheet"></a>İş senaryoları ve Machine Learning algoritması sayfası

[Azure Machine Learning algoritması](https://docs.microsoft.com/azure/machine-learning/algorithm-cheat-sheet?WT.mc_id=docs-article-lazzeri) bir sorun, ilk dikkat etmenize yardımcı olur: **verilerinize ne yapmak**istiyorsunuz? Machine Learning algoritması oluşturma sayfasında, yapmak istediğiniz görevi bulun ve ardından tahmine dayalı analiz çözümü için bir [Azure Machine Learning tasarımcı](https://docs.microsoft.com/azure/machine-learning/concept-designer?WT.mc_id=docs-article-lazzeri) algoritması bulun. 

Machine Learning tasarımcı, çok [Lass karar ormanı](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/multiclass-decision-forest?WT.mc_id=docs-article-lazzeri), [öneri sistemleri](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/evaluate-recommender?WT.mc_id=docs-article-lazzeri), [sinir ağ gerileme](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/neural-network-regression?WT.mc_id=docs-article-lazzeri), çok [Lass sinir ağı](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/multiclass-neural-network?WT.mc_id=docs-article-lazzeri)ve [K-Ortalamalar kümesi](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/k-means-clustering?WT.mc_id=docs-article-lazzeri)gibi kapsamlı bir algoritma portföyü sağlar. Her algoritma, farklı bir makine öğrenimi sorun türünü ele almak için tasarlanmıştır. Her algoritmanın nasıl çalıştığı ve algoritmayı iyileştirmek için parametrelerin nasıl ayarlanacağı hakkındaki belgelerle birlikte, tüm bir liste için [Machine Learning tasarımcı algoritması ve modül başvurusu '](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/module-reference?WT.mc_id=docs-article-lazzeri) na bakın.

> [!NOTE]
> Machine Learning algoritması sayfasını indirmek için [Azure Machine Learning algoritması başvuru sayfasına](https://docs.microsoft.com/azure/machine-learning/algorithm-cheat-sheet?WT.mc_id=docs-article-lazzeri)gidin.
> 
> 

Azure Machine Learning algoritma oluşturma sayfasındaki kılavuzla birlikte, çözümünüz için bir makine öğrenimi algoritması seçerken diğer gereksinimleri göz önünde bulundurun. Doğruluk, eğitim süresi, doğrallığı, parametre sayısı ve özellik sayısı gibi göz önünde bulundurulması gereken ek etmenler aşağıda verilmiştir.

## <a name="comparison-of-machine-learning-algorithms"></a>Makine öğrenimi algoritmalarının karşılaştırması

Bazı öğrenme algoritmaları, verilerin yapısı veya istenen sonuçlar hakkında belirli varsayımlar yapar. İhtiyaçlarınıza uyan bir tane bulabiliyorsanız, size daha yararlı sonuçlar, daha kesin tahminler veya daha hızlı eğitim süreleri verebilir.

Aşağıdaki tabloda, sınıflandırma, gerileme ve kümeleme ailelerinden algoritmaların en önemli özelliklerinden bazıları özetlenmektedir:

| **Algoritma** | **Veritabanınızın** | **Eğitim süresi** | **Doğrity** | **Parametreler** | **Notlar** |
| --- |:---:|:---:|:---:|:---:| --- |
| **Sınıflandırma ailesi** | | | | | |
| [İki sınıf Lojistik gerileme](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/two-class-logistic-regression?WT.mc_id=docs-article-lazzeri) |İyi  |Hızlı |Evet |4 | |
| [İki sınıf karar ormanı](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/two-class-decision-forest?WT.mc_id=docs-article-lazzeri) |Mükemmel |Orta |Hayır |5 |Daha yavaş Puanlama sürelerini gösterir. Ağaç tahminlerinin biriktirme bölümünde Tread kilitlemesinin neden olduğu daha yavaş Puanlama süreleri nedeniyle Bire Karşı Hepsi Çoklu Sınıf ile çalışmamayı önerin |
| [İki sınıf önceden artırılmış karar ağacı](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/two-class-boosted-decision-tree?WT.mc_id=docs-article-lazzeri) |Mükemmel |Orta |Hayır |6 |Büyük bellek ayak izi |
| [İki sınıf sinir ağı](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/two-class-neural-network?WT.mc_id=docs-article-lazzeri) |İyi |Orta |Hayır |8 | |
| [İki sınıflı ortalama Perceptron](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/two-class-averaged-perceptron?WT.mc_id=docs-article-lazzeri) |İyi |Orta |Evet |4 | |
| [İki sınıflı destek vektör makinesi](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/two-class-support-vector-machine?WT.mc_id=docs-article-lazzeri) |İyi |Hızlı |Evet |5 |Büyük özellik kümeleri için iyi |
| [Birden çok Lass Lojistik gerileme](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/multiclass-logistic-regression?WT.mc_id=docs-article-lazzeri) |İyi |Hızlı |Evet |4 | |
| [Birden çok Lass karar ormanı](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/multiclass-decision-forest?WT.mc_id=docs-article-lazzeri) |Mükemmel |Orta |Hayır |5 |Daha yavaş Puanlama sürelerini gösterir |
| [Birden çok sınıf tarafından artırılmış karar ağacı](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/multiclass-boosted-decision-tree?WT.mc_id=docs-article-lazzeri) |Mükemmel |Orta |Hayır |6 | Daha az kapsama sahip küçük risklerle doğruluğu artırmaya eğilimlidir |
| [Birden çok Lass sinir ağı](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/multiclass-neural-network?WT.mc_id=docs-article-lazzeri) |İyi |Orta |Hayır |8 | |
| [Tek vs-hepsi birden çok sınıf](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/one-vs-all-multiclass?WT.mc_id=docs-article-lazzeri) | - | - | - | - |Seçilen iki sınıf yöntemin özelliklerine bakın |
| **Regresyon ailesi** | | | | | |
| [Doğrusal regresyon](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/linear-regression?WT.mc_id=docs-article-lazzeri) |İyi |Hızlı |Evet |4 | |
| [Karar ormanı gerileme](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/decision-forest-regression?WT.mc_id=docs-article-lazzeri)|Mükemmel |Orta |Hayır |5 | |
| [Artırılmış karar ağacı gerileme](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/boosted-decision-tree-regression?WT.mc_id=docs-article-lazzeri) |Mükemmel |Orta |Hayır |6 |Büyük bellek ayak izi |
| [Sinir ağ gerileme](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/neural-network-regression?WT.mc_id=docs-article-lazzeri) |İyi |Orta |Hayır |8 | |
| **Kümeleme ailesi** | | | | | |
| [K-kümelemenin anlamı](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/k-means-clustering?WT.mc_id=docs-article-lazzeri) |Mükemmel |Orta |Evet |8 |Kümeleme algoritması |

## <a name="requirements-for-a-data-science-scenario"></a>Bir veri bilimi senaryosu için gereksinimler

Verilerinize ne yapmak istediğinizi öğrendikten sonra çözümünüz için ek gereksinimler belirlemeniz gerekir. 

Aşağıdaki gereksinimler için seçimler ve büyük olasılıkla denge sağlayın:

- Veritabanınızın
- Eğitim süresi
- Doğrity
- Parametre sayısı
- Özellik sayısı

## <a name="accuracy"></a>Veritabanınızın

Makine öğreniminde doğruluk, toplam durum için gerçek sonuçların oranı olarak bir modelin verimliliğini ölçer. Machine Learning tasarımcısında, [model değerlendir modülü](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/evaluate-model?WT.mc_id=docs-article-lazzeri) bir sektör standardı değerlendirme ölçümleri kümesini hesaplar. Eğitilen bir modelin doğruluğunu ölçmek için bu modülü kullanabilirsiniz.

Mümkün olan en doğru yanıtı almak her zaman gerekli değildir. Bazen, için kullanmak istediğiniz seçeneğe bağlı olarak yaklaşık bir yaklaşık değer vardır. Bu durumda, daha yakın yöntemlerle işlem süresini önemli ölçüde kesmeniz mümkün olabilir. Yaklaşık Yöntemler, aşırı sığdırmayı önlemek için de doğal olarak eğilimlidir.

Modeli değerlendir modülünü kullanmanın üç yolu vardır:

- Modeli değerlendirmek için eğitim verilerinize ilişkin puanlar oluşturun
- Modelde puanlar oluşturun, ancak bu puanları ayrılmış bir test kümesinde puanlar ile karşılaştırın
- Aynı veri kümesini kullanarak iki farklı ancak ilgili modellerin puanlarını karşılaştırın

Makine öğrenimi modellerinin doğruluğunu değerlendirmek için kullanabileceğiniz ölçümlerin ve yaklaşımların tüm listesi için, bkz. [modeli değerlendir modülü](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/evaluate-model?WT.mc_id=docs-article-lazzeri).

## <a name="training-time"></a>Eğitim süresi

Denetimli öğreniminde eğitim, hataları en aza indiren bir makine öğrenimi modeli oluşturmak için geçmiş verileri kullanma anlamına gelir. Bir modeli eğitmek için gereken dakika veya saat sayısı, algoritmalar arasında harika bir işlem olduğunu gösterir. Eğitim süresi genellikle doğrulukla yakından ilişkilidir; genellikle diğerine eşlik eden. 

Ayrıca, bazı algoritmalar diğerlerine göre veri noktası sayısına daha duyarlıdır. Özellikle veri kümesi büyükse, zaman sınırlaması olduğu için belirli bir algoritma seçebilirsiniz.

Machine Learning tasarımcısında, makine öğrenimi modelinin oluşturulması ve kullanılması genellikle üç adımlı bir işlemdir:

1.  Belirli bir algoritma türü seçerek ve ardından parametrelerini veya hiper parametrelerini tanımlayarak bir modeli yapılandırın. 

2.  Etiketli ve algoritmayla uyumlu verilerin bulunduğu bir veri kümesi sağlayın. [Model modülünü eğitmek](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/train-model?WT.mc_id=docs-article-lazzeri)için hem verileri hem de modeli bağlayın.

3.  Eğitim tamamlandıktan sonra, yeni verilerde tahmine dayalı hale getirmek için bir [Puanlama modülleriyle](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/score-model?WT.mc_id=docs-article-lazzeri) eğitilen modeli kullanın.

## <a name="linearity"></a>Doğrity

İstatistikte ve makine öğreniminde doğrusun, veri kümenizdeki bir değişken ve bir sabit arasında doğrusal bir ilişki olduğu anlamına gelir. Örneğin, doğrusal sınıflandırma algoritmaları sınıfların düz bir çizgi (veya daha yüksek boyutlu bir analog) ile ayrılacaksa olduğunu varsayar.

Birçok makine öğrenimi algoritması, doğrlılığını kullanır. Azure Machine Learning tasarımcısında şunları içerir: 

- [Birden çok Lass Lojistik gerileme](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/multiclass-logistic-regression?WT.mc_id=docs-article-lazzeri)
- [İki sınıf Lojistik gerileme](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/two-class-logistic-regression?WT.mc_id=docs-article-lazzeri)
- [Vektör makinelerini destekleme](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/two-class-support-vector-machine?WT.mc_id=docs-article-lazzeri)  

Doğrusal regresyon algoritmaları, veri eğilimlerinin düz bir çizgi olduğunu varsayar. Bu varsayım bazı sorunlar için kötü değildir, ancak diğerleri için doğruluğu azaltır. Dezavantajlarına rağmen doğrusal algoritmalar ilk strateji olarak popüler. Algorithmically basit ve hızlı bir şekilde eğitme eğilimindedir.

![Doğrusal olmayan sınıf sınırı](./media/how-to-select-algorithms/nonlinear-class-boundary.png)

Doğrusal olmayan ***sınıf sınırı***: *bir doğrusal sınıflandırma algoritmasına güvenmek, düşük doğrulukla sonuçlanır.*

![Doğrusal olmayan bir eğilim içeren veriler](./media/how-to-select-algorithms/nonlinear-trend.png)

Doğrusal ***olmayan bir eğilim Içeren veriler***: *Doğrusal regresyon yöntemi kullanmak gerekenden çok daha büyük hatalar oluşturur.*

## <a name="number-of-parameters"></a>Parametre sayısı

Parametreler, bir algoritma ayarlarken bir veri bilimconu açmak için aldığı KBS 'lardır. Bunlar hata toleransı veya yineleme sayısı gibi algoritmaların davranışını etkileyen sayılardır veya algoritmanın nasıl davrandığı çeşitleri arasındaki seçeneklere sahiptir. Algoritmanın eğitim süresi ve doğruluğu, bazı durumlarda yalnızca doğru ayarları almak açısından hassas olabilir. Genellikle, çok sayıda parametreye sahip algoritmalar iyi bir bileşim bulmak için en fazla deneme ve hata gerektirir.

Alternatif olarak, Machine Learning tasarımcısında [ayarlama modeli hiper parametreleri modülü](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/tune-model-hyperparameters?WT.mc_id=docs-article-lazzeri) vardır: Bu modülün hedefi, bir makine öğrenimi modeli için en uygun hiper parametreleri belirlemektir. Modül farklı ayarların birleşimlerini kullanarak birden çok modeli oluşturur ve sınar. Ayarların birleşimlerini almak için ölçümleri tüm modeller üzerinde karşılaştırır. 

Parametre alanının yayıldığından emin olmanın harika bir yolu olsa da, bir modeli eğitmek için gereken süre parametre sayısıyla üstel olarak artar. Çok sayıda parametreye sahip olan baş, genellikle algoritmanın daha fazla esneklik olduğunu gösterir. Genellikle parametre ayarlarının doğru birleşimini bulabileceğiniz için çok iyi bir doğruluk elde edebilir.

## <a name="number-of-features"></a>Özellik sayısı

Machine Learning 'de, bir özellik çözümlemek istediğiniz olgudur tarafından ölçülebilir bir değişkendir. Belirli veri türleri için, özellik sayısı veri noktalarının sayısıyla karşılaştırıldığında çok büyük olabilir. Bu durum genellikle, genelikler veya metin verileri ile yapılır. 

Çok sayıda özellik bazı öğrenme algoritmalarından oluşabilir ve eğitim süresi unfeasibly uzun sürebilir. [Destek vektör makineleri](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/two-class-support-vector-machine?WT.mc_id=docs-article-lazzeri) özellikle çok sayıda özelliği olan senaryolar için uygundur. Bu nedenle, çok sayıda uygulamada metin ve resim sınıflandırmasına bilgi alımı kullanılmaktadır. Destek vektör makineleri hem sınıflandırma hem de gerileme görevleri için kullanılabilir.

Özellik seçimi, girdilere, belirtilen bir çıktı verildiğinde istatistiksel testlerin uygulanması işlemini ifade eder. Amaç, çıktının hangi sütunlarda daha tahmin olduğunu belirlemektir. Machine Learning tasarımcısında [filtre tabanlı özellik seçimi modülü](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/filter-based-feature-selection?WT.mc_id=docs-article-lazzeri) arasından seçim yapmak için birden çok özellik seçimi algoritması sağlar. Modül, Pearson bağıntı ve Chi-kare değerleri gibi bağıntı yöntemleri içerir.

Veri kümenizin Özellik önem puanlarını bir kümesini hesaplamak için [permütasyon özelliği önem derecesini](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/permutation-feature-importance?WT.mc_id=docs-article-lazzeri) de kullanabilirsiniz. Daha sonra, bir modelde kullanılacak en iyi özellikleri belirlemenize yardımcı olması için bu puanlara yararlanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

 - [Azure Machine Learning Tasarımcısı hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/machine-learning/concept-designer?WT.mc_id=docs-article-lazzeri)
 - Azure Machine Learning tasarımcısında bulunan tüm makine öğrenimi algoritmalarının açıklamaları için, bkz. [Machine Learning tasarımcı algoritması ve modül başvurusu](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/module-reference?WT.mc_id=docs-article-lazzeri)
 - Derin öğrenme, makine öğrenimi ve AI arasındaki ilişkiyi araştırmak için bkz [. derin öğrenme vs. Machine Learning](https://docs.microsoft.com/azure/machine-learning/concept-deep-learning-vs-machine-learning?WT.mc_id=docs-article-lazzeri)
