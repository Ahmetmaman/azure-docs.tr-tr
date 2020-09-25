---
title: Veri bilimi görevlerini yürütme-Team Data Science Işlemi
description: Veri bilimccisi, bir veri bilimi projesini, denetimli, sürümlü ve işbirliğine dayalı bir şekilde nasıl yürütebilirler.
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 5fe11b20ff8e96bc4b0d997c49201aafdd2e2cac
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91320532"
---
# <a name="execute-data-science-tasks-exploration-modeling-and-deployment"></a>Veri bilimi görevlerini yürütme: araştırma, modelleme ve dağıtım

Veri araştırması, modelleme ve dağıtım tipik bir veri bilimi görevi içerir. Bu makalede etkileşimli veri araştırma, **analiz ve raporlama (ıDEAR)** ve **Otomatik modelleme ve raporlama (Amar)** yardımcı programlarının nasıl kullanılacağı, etkileşimli veri araştırması, veri analizi, raporlama ve model oluşturma gibi çeşitli yaygın veri bilimi görevlerinin nasıl tamamlanacağı gösterilmektedir. Bir üretim ortamına model dağıtmaya yönelik seçenekler şunlar olabilir:

- [Azure Machine Learning](../index.yml)
- [ML Hizmetleri ile SQL-Server](https://docs.microsoft.com/sql/advanced-analytics/r/r-services)
- [Microsoft Machine Learning Sunucusu](https://docs.microsoft.com/machine-learning-server/what-is-machine-learning-server)


## <a name="1--exploration"></a>1. <a name='DataQualityReportUtility-1'></a> araştırma 

Bir veri bilimcesi, araştırma ve raporlamayı çeşitli yollarla gerçekleştirebilir: Python için kullanılabilen kitaplıkları ve paketleri (örneğin, Matplotlib) veya R (ggçiz veya latku gibi) kullanarak. Veri bilimcileri, söz konusu kodu belirli senaryolar için veri keşfi ihtiyaçlarına uyacak şekilde özelleştirebilir. Yapılandırılmış verilerle ilgilenme ihtiyacı, metin veya görüntüler gibi yapılandırılmamış veriler için farklıdır. 

Azure Machine Learning gibi ürünler Ayrıca, özellik oluşturma da dahil olmak üzere veri denetimi ve araştırmasına yönelik [Gelişmiş veri hazırlığı](../how-to-create-register-datasets.md) sağlar. Kullanıcı, ihtiyaçlarını en iyi şekilde sunan araçlara, kitaplıklara ve paketlere karar vermelidir. 

Bu aşamanın sonundaki teslim edilebilir bir veri araştırma rapordur. Rapor, modelleme için kullanılacak verilerin ayrıntılı bir görünümünü ve verilerin modelleme adımına devam etmek için uygun olup olmadığına ilişkin bir değerlendirme sağlamalıdır. Yarı otomatik araştırma, modelleme ve raporlama için aşağıdaki bölümlerde ele alınan Team Data Science süreci (TDSP) yardımcı programları, standartlaştırılmış veri keşif ve modelleme raporları da sağlar. 

### <a name="interactive-data-exploration-analysis-and-reporting-using-the-idear-utility"></a>IDEAR yardımcı programını kullanarak etkileşimli veri araştırması, analiz ve raporlama

Bu R markaşağı tabanlı veya Python Not defteri tabanlı yardımcı program, veri kümelerini değerlendirmek ve araştırmak için esnek ve etkileşimli bir araç sağlar. Kullanıcılar veri kümesinden en az kodlama ile rapor oluşturabilir. Kullanıcılar etkileşimli araçtaki araştırma sonuçlarını, istemcilere teslim edilebilir veya sonraki modelleme adımında hangi değişkenlerin dahil edileceği hakkında kararlar almak için kullanılan bir son rapora dışa aktarmak için düğmelere tıklabilirler.

Bu durumda araç yalnızca bellekteki veri çerçeveleri üzerinde çalışmaktadır. Araştırmak üzere veri kümesinin parametrelerini belirtmek için bir YAML dosyası gereklidir. Daha fazla bilgi için bkz. [TDSP Data Science yardımcı programlarında ıdear](https://github.com/Azure/Azure-TDSP-Utilities/tree/master/DataScienceUtilities/DataReport-Utils).


## <a name="2--modeling"></a>2. <a name='ModelingUtility-2'></a> modelleme

Çeşitli dillerde eğitim modelleri için çok sayıda araç seti ve paket vardır. Veri bilimcileri, uygun iş kullanım örnekleri ve üretim senaryoları için doğruluk ve gecikme süresi ile ilgili performans konuları karşılanmadığı sürece, ne kadar rahat olduğunu bilmekten çekinmeyin.

Sonraki bölümde, yarı otomatik modelleme için R tabanlı TDSP yardımcı programının nasıl kullanılacağı gösterilmektedir. Bu AMAR yardımcı programı, daha iyi bir performans sağlayan model sağlamak üzere ayarlanmış olması gereken parametreleri ve temel çizgi modellerini hızlı bir şekilde oluşturmak için kullanılabilir.
Aşağıdaki model Yönetimi bölümünde birden çok modeli kaydetmek ve yönetmek için bir sistemin nasıl yapılacağı gösterilmektedir.


### <a name="model-training-modeling-and-reporting-using-the-amar-utility"></a>Model eğitimi: AMAR yardımcı programını kullanarak modelleme ve raporlama

[Otomatik modelleme ve raporlama (AMAR) yardımcı programı](https://github.com/Azure/Azure-TDSP-Utilities/tree/master/DataScienceUtilities/Modeling) , model oluşturma işlemini hiper parametre ile gerçekleştirmek ve bu modellerin doğruluğunu karşılaştırmak için özelleştirilebilir, yarı otomatik bir araç sağlar. 

Model oluşturma yardımcı programı, farklı bölümlerinde kolay gezinmek için içindekiler tablosu ile kendi kendine dahil edilen HTML çıktısı oluşturmak üzere çalıştırılabilen bir R Markdown dosyasıdır. Markaşağı dosyası çalıştırıldığında üç algoritma yürütülür (KNIT): glmnet paketini kullanarak regularized gerileme, Rasgeleforest paketini kullanan rastgele orman ve xgboost paketini kullanarak ağaçları artırma. Bu algoritmaların her biri eğitilen bir model üretir. Daha sonra bu modellerin doğruluğu karşılaştırılır ve göreli Özellik önem çizimleri raporlanır. Şu anda iki yardımcı program vardır: biri ikili sınıflandırma görevi için, diğeri ise regresyon görevi içindir. Bunlarla ilgili birincil farklılıklar, bu öğrenme görevleri için denetim parametrelerinin ve doğruluk ölçümlerinin belirtilme yöntemidir. 

Şunları belirtmek için bir YAML dosyası kullanılır:

- veri girişi (bir SQL kaynağı veya bir R-veri dosyası) 
- Eğitim için verilerin bölümü ve test için ne bölüm kullanılır
- hangi algoritmaların çalıştırılacağı 
- Model iyileştirmesi için denetim parametreleri seçimi:
    - çapraz doğrulama 
    - önyükleme
    - çapraz doğrulamanın katları
- Her algoritma için Hyper-parametre kümeleri. 

Algoritmaların sayısı, iyileştirme için katların sayısı, Hyper-parametreleri ve tarama yapılacak Hyper-parametre kümelerinin sayısı, modelleri hızlı bir şekilde çalıştırmak için YAML dosyasında da değiştirilebilir. Örneğin, daha az sayıda CV katları, daha az sayıda parametre kümesi ile çalıştırılabilirler. Garanti verilmişse, daha yüksek sayıda CV kattı veya daha fazla sayıda parametre kümesi ile daha ayrıntılı bir şekilde çalıştırılabilir.

Daha fazla bilgi için bkz. [TDSP veri bilimi yardımcı programlarında otomatik modelleme ve raporlama yardımcı programı](https://github.com/Azure/Azure-TDSP-Utilities/tree/master/DataScienceUtilities/Modeling).

### <a name="model-management"></a>Model yönetimi
Birden çok Model derlendikten sonra, modelleri kaydetmek ve yönetmek için genellikle bir sisteme sahip olmanız gerekir. Genellikle betiklerin veya API 'lerin ve arka uç veritabanının veya sürüm oluşturma sisteminin bir birleşimine ihtiyacınız vardır. Bu yönetim görevleri için göz önünde bulundurmanız gereken birkaç seçenek şunlardır:

1. [Azure Machine Learning-model yönetimi hizmeti](../index.yml)
2. [MıT 'ten ModelDB](http://modeldb.csail.mit.edu:3000/projects) 
3. [Model yönetimi sistemi olarak SQL-Server](https://blogs.technet.microsoft.com/dataplatforminsider/2016/10/17/sql-server-as-a-machine-learning-model-management-system/)
4. [Microsoft Machine Learning Sunucusu](https://docs.microsoft.com/sql/advanced-analytics/r/r-server-standalone)

## <a name="3--deployment"></a>3. <a name='Deployment-3'></a> dağıtım

Üretim dağıtımı, bir modelin bir işletmede etkin bir rol oynamasını sağlar. Dağıtılan bir modelden tahminler, iş kararları vermek için kullanılabilir.

### <a name="production-platforms"></a>Üretim platformları
Modelleri üretime koymak için çeşitli yaklaşımlar ve platformlar vardır. Birkaç seçenek aşağıda verilmiştir:


- [Azure Machine Learning 'de model dağıtımı](../how-to-deploy-and-where.md)
- [SQL Server 'da bir modelin dağıtılması](https://docs.microsoft.com/sql/advanced-analytics/tutorials/sqldev-py6-operationalize-the-model)
- [Microsoft Machine Learning Sunucusu](https://docs.microsoft.com/sql/advanced-analytics/r/r-server-standalone)

> [!NOTE]
> Dağıtımdan önce, model Puanlama gecikmesinin süresinin üretilmesi, üretimde kullanılmak üzere yeterince düşüktür.
>
>

**Belirli senaryolar**için işlemdeki tüm adımları gösteren izlenecek yollarda daha fazla örnek mevcuttur. Bunlar, [örnek yönergeler](walkthroughs.md) makalesinde listelenmiş ve küçük resim açıklamalarıyla bağlantılandırılır. Bulut, şirket içi araçları ve hizmetleri akıllı bir uygulama oluşturmak için bir iş akışı veya işlem hattı halinde nasıl birleştirebileceğinizi gösterir.

> [!NOTE]
> Azure Machine Learning Studio kullanarak dağıtım için bkz. [Azure Machine Learning Web hizmeti dağıtma](../classic/deploy-a-machine-learning-web-service.md).
>
>

### <a name="ab-testing"></a>A/B testi
Üretimde birden çok model olduğunda, modellerin performansını karşılaştırmak için [bir/B testi](https://en.wikipedia.org/wiki/A/B_testing) gerçekleştirmek yararlı olabilir. 

 
## <a name="next-steps"></a>Sonraki adımlar

Veri bilimi [projelerinin Ilerlemesini izlemek](track-progress.md) , bir veri bilimcinin bir veri bilimi projesinin ilerlemesini nasıl izleyebilme şeklini gösterir.

[Model işlemi ve CI/CD](ci-cd-flask.md) , geliştirilmiş modeller ile CI/CD 'nin nasıl gerçekleştirileceğini gösterir.
