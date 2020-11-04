---
title: Machine Learning Studio (klasik)-Azure için ne yapabilirim?
description: Machine Learning Studio (klasik), kullanıma yönelik kullanım algoritmalarından ve modüllerden hızlı bir şekilde model oluşturmaya yönelik bir sürükle ve bırak aracıdır.
services: machine-learning
author: likebupt
ms.author: keli19
ms.custom: seodec18
ms.assetid: e65c8fe1-7991-4a2a-86ef-fd80a7a06269
ms.service: machine-learning
ms.subservice: studio
ms.topic: overview
ms.date: 08/19/2020
ms.openlocfilehash: 1aadf1fe59c5300a4f16ea96b1e1d7a5fbcbdd6d
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2020
ms.locfileid: "93325731"
---
# <a name="what-can-i-do-with-machine-learning-studio-classic"></a>Machine Learning Studio (klasik) ile ne yapabilirim?

**Uygulama hedefi:** ![ Evet ](../../../includes/media/aml-applies-to-skus/yes.png) Machine Learning Studio (klasik) ![ ](../../../includes/media/aml-applies-to-skus/no.png)[Azure Machine Learning](../overview-what-is-machine-learning-studio.md#ml-studio-classic-vs-azure-machine-learning-studio) yok  

[!INCLUDE [Designer notice](../../../includes/designer-notice.md)]

Machine Learning Studio (klasik), makine öğrenimi modellerini derlemek, test etmek ve dağıtmak için kullanabileceğiniz bir sürükle ve bırak aracıdır. Studio (klasik) modelleri Web Hizmetleri olarak yayımlar, bu da Excel gibi özel uygulamalar veya BI araçları tarafından kolayca tüketilebilir.


## <a name="studio-classic--interactive-workspace"></a>Studio (klasik) etkileşimli çalışma alanı

Tahmine dayalı bir analiz modeli geliştirmek için genellikle bir veya daha fazla kaynaktaki verileri kullanır, çeşitli veri işleme ve istatistiksel işlevler aracılığıyla bu verileri dönüştürür ve analiz eder ve bir sonuç kümesi oluşturur. Bir modelin bu şekilde geliştirilmesi yinelemeli bir işlemdir. Çeşitli işlevleri ve bunların parametrelerini değiştirirken, eğitilmiş ve verimli bir model elde ettiğinizi düşüneceğiniz ana kadar sonuçlarınız yakınsanır.

Machine Learning Studio (klasik), tahmine dayalı bir analiz modelini kolayca oluşturmak, test etmek ve yinelemek için size etkileşimli ve görsel bir çalışma alanı sağlar. Machine Learning Studio (klasik) ' de çalıştırdığınız bir _*_deneme_*_ oluşturmak için, * *_veri kümeleri_* _ ve analiz _*_modüllerini_*_ etkileşimli bir tuvale sürükleyip bırakabilirsiniz. Model tasarımınızı yinelemek için, denemeyi düzenleyin, isterseniz bir kopyasını kaydedin ve yeniden çalıştırın. Hazırsanız, _*_eğitim denemenizi_*_ bir tahmine _*_dayalı_*_ denemeye dönüştürebilir ve sonra modelinize başkaları tarafından erişilebilmesi için bunu bir _*_Web hizmeti_*_ olarak yayımlayabilirsiniz.

Programlama gerekmez, tahmine dayalı analiz modelinizi oluşturmak için veri kümelerini ve modülleri görsel olarak bağlayın.

![Machine Learning Studio (klasik) diyagram: denemeleri oluşturma, çok sayıda kaynak için veri okuma, puanlanmış veriler yazma, yazma modelleri.](./media/what-is-ml-studio/azure-ml-studio-diagram.jpg)

## <a name="download-the-ml-studio-classic-overview-diagram"></a>ML Studio (klasik) genel bakış diyagramını indirin
_ *Microsoft ml Studio (klasik) özelliklerine genel bakış* * diyagramını indirin ve Machine Learning Studio (klasik) özelliklerine ilişkin üst düzey bir görünüm alın. Diyagramı yakınınızda tutmak için tabloid boyutunda (11 x 17 inç) yazdırabilirsiniz.

**Diyagramı buradan indirin: [Microsoft Machine Learning Studio (klasik) özelliklerine genel bakış](https://download.microsoft.com/download/C/4/6/C4606116-522F-428A-BE04-B6D3213E9E52/ml_studio_overview_v1.1.pdf)** 
 ![ Microsoft Machine Learning Studio (klasik) özelliklerine genel bakış](./media/what-is-ml-studio/ml_studio_overview_v1.1.png)


## <a name="components-of-a-studio-classic--experiment"></a>Studio (klasik) deneme bileşenleri
Bir deneme, tahmine dayalı bir modeli oluşturmak için birlikte bağladığınız analitik modüllere veri sağlayan veri kümelerinden oluşur. Geçerli bir deneme özellikle şu özelliklere sahiptir:

* Denemenin en az bir veri kümesi ve bir modülü vardır
* Veri kümeleri yalnızca modüllere bağlanabilir
* Modüller veri kümelerine veya diğer modüllere bağlanabilir
* Modüllerin tüm giriş bağlantı noktalarının veri akışına bir tür bağlantısı olması gerekir
* Her bir modül için gereken tüm parametreler ayarlanmalıdır

Bir denemeyi sıfırdan oluşturabilir veya var olan bir örnek denemeyi şablon olarak kullanabilirsiniz. Daha fazla bilgi için bkz. [Örnek denemeleri kopyalayarak yeni makine öğrenimi denemeleri oluşturma](sample-experiments.md).

Deneme oluşturma örneği için bkz. [Machine Learning Studio deneme oluşturma (klasik)](create-experiment.md).

Tahmine dayalı analiz çözümü oluşturmaya yönelik daha kapsamlı bir anlatım için bkz. [Machine Learning Studio (klasik) ile tahmine dayalı bir çözüm geliştirme](tutorial-part1-credit-risk.md).

### <a name="datasets"></a>Veri kümeleri
Veri kümesi, modelleme sürecinde kullanılabilmesi için Machine Learning Studio (klasik) karşıya yüklenmiş olan bir veri kümesidir. Deneme için Machine Learning Studio (klasik) bir dizi örnek veri kümesi bulunur ve ihtiyacınız olduğunda daha fazla veri kümesi yükleyebilirsiniz. Dahil olan veri kümelerine aşağıda birkaç örnek verilmiştir:

* **Çeşitli otomobiller için MPG verileri** - Otomobiller için silindir, beygir gücü, vb. sayısına göre tanımlanan galon başına mil (MPG) değerleri.
* **Meme kanseri verileri** - Meme kanseri tanılama verileri.
* **Orman yangını verileri** - Kuzey doğu Portekiz'de orman yangını boyutları.

Bir deneme oluştururken, tuvalin solunda kullanılabilen veri kümeleri listesinden seçim yapabilirsiniz.

Machine Learning Studio (klasik) eklenen örnek veri kümelerinin bir listesi için bkz. [Machine Learning Studio (klasik) örnek veri kümelerini kullanma](use-sample-datasets.md).

### <a name="modules"></a>Modül
Bir modül, verilerinizde gerçekleştirebileceğiniz bir algoritmadır. Machine Learning Studio (klasik), veri giriş işlevlerinden eğitim, Puanlama ve doğrulama işlemlerine kadar birçok modül içerir. Dahil olan modüllere aşağıda birkaç örnek verilmiştir:

* [ARFF'ye Dönüştürme][convert-to-arff] - Seri hale getirilmiş .NET veri kümesini Öznitelik-İlişki Dosyası Biçimi'ne (ARFF) dönüştürür.
* [Basit İstatistikleri Hesaplama][elementary-statistics] - Ortalama, standart sapma vb. basit istatistikleri hesaplar.
* [Doğrusal Regresyon][linear-regression] - Çevrimiçi bir gradyan düşüşü tabanlı doğrusal regresyon modeli oluşturur.
* [Model Puanlama][score-model] - Eğitilmiş bir sınıflandırma veya regresyon modelini puanlar.

Bir deneme oluştururken, tuvalin solunda bulunan modüllerin listesinden seçim yapabilirsiniz.

Bir modül, modülün iç algoritmalarını yapılandırmak için kullanabileceğiniz parametreler kümesine sahip olabilir. Tuvalde bir modül seçtiğinizde, modülün parametreleri tuvalin sağındaki **Özellikler** bölmesinde görüntülenir  Modelinizi ayarlamak için, bu bölmedeki parametreleri değiştirebilirsiniz.

Kullanılabilir makine öğrenimi algoritmalarının büyük kitaplığındaki bazı yardım için bkz. [Microsoft Machine Learning Studio için algoritma seçme (klasik)](../how-to-select-algorithms.md).

## <a name="deploying-a-predictive-analytics-web-service"></a>Tahmine dayalı analiz web hizmetini dağıtma
Tahmine dayalı analiz modeliniz hazırlandıktan sonra, bunu bir Web hizmeti olarak Machine Learning Studio (klasik) ile dağıtabilirsiniz. Bu işlemle ilgili daha fazla bilgi için bkz. [Azure Machine Learning Web hizmeti dağıtma](deploy-a-machine-learning-web-service.md).

## <a name="next-steps"></a>Sonraki adımlar
Tahmine dayalı analiz ve makine öğrenimine ilişkin temel bilgileri, [adım adım hızlı başlangıç](create-experiment.md) ve [örnek oluşturma](sample-experiments.md)aracılığıyla öğrenebilirsiniz.

<!-- Module References -->
[convert-to-arff]: /azure/machine-learning/studio-module-reference/convert-to-arff
[elementary-statistics]: /azure/machine-learning/studio-module-reference/compute-elementary-statistics
[linear-regression]: /azure/machine-learning/studio-module-reference/linear-regression
[score-model]: /azure/machine-learning/studio-module-reference/score-model