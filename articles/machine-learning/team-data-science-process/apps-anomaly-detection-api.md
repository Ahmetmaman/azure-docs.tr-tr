---
title: Azure Machine Learning Anomali algılama API'si - Team Data Science Process'i
description: Anomali algılama API'si, zaman serisi verilerinde zaman eşit aralıklı sayısal değerlerle anomalileri algılar Microsoft Azure Machine Learning ile oluşturulmuş bir örnek verilmiştir.
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 06/05/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=alokkirpal, previous-ms.author=alok
ms.openlocfilehash: 16f13cd4ad580ea2f163fe87b5924c1462890972
ms.sourcegitcommit: 0f54f1b067f588d50f787fbfac50854a3a64fff7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/12/2019
ms.locfileid: "64926177"
---
# <a name="machine-learning-anomaly-detection-api"></a>Machine Learning Anomali algılama API'si

> [!NOTE]
> Bu öğe bakım aşamasındadır. İş, operasyonel ve IoT ölçümlerinden gelen anormallikleri algılamak için Azure bilişsel Hizmetler kapsamındaki Machine Learning algoritmalarının bir galerisiyle desteklenen [anomali ALGıLAYıCı API hizmetini](https://azure.microsoft.com/services/cognitive-services/anomaly-detector/) kullanmanızı öneririz.

## <a name="overview"></a>Genel Bakış
[Anomali algılama API'sini](https://gallery.cortanaintelligence.com/MachineLearningAPI/Anomaly-Detection-2) zaman eşit aralıklı sayısal değerler ile zaman serisi verilerinde anormallikleri algılar, Azure Machine Learning ile oluşturulmuş bir örnek verilmiştir.

Bu API zaman serisi verilerinde görülen anormal bir düzen aşağıdaki türlerini algılayabilir:

* **Pozitif ve olumsuz eğilimler**: Örneğin, bellek kullanımını izlerken, Bellek sızıntısını önemli bir şekilde ele alırken, bu durum
* **Dinamik değer aralığındaki değişiklikler**: Örneğin, bir bulut hizmeti tarafından oluşturulan özel durumları izlerken, dinamik değer aralığındaki tüm değişiklikler hizmetin sistem durumunda dengesizmi olduğunu belirtebilir ve
* **Ani artışlar ve DIB 'ler**: Örneğin, bir hizmette oturum açma hatalarının sayısını veya bir e-ticaret sitesinde kullanıma alma sayısını izlerken, ani veya DIB 'ler anormal davranışları gösterebilir.

Bu makine öğrenimi algılayıcıları değerlerindeki bu tür değişiklikler anomali puanları değerlerine zaman ve rapor süregiden değişiklikler üzerinden izleyin. Bunlar geçici eşiği ayarlama gerektirmez ve puanlarını hatalı pozitif sonuç oranı denetlemek için kullanılabilir. Anomali algılama API'si, zaman içinde KPI'ları izleyerek hizmet izleme gibi çeşitli senaryolarda yararlı aramaları sayısı, sayı tıklama, dosya okuma gibi bellek, CPU sayaçları aracılığıyla performans izleme, vb. gibi ölçümleri aracılığıyla kullanımını izleme. zaman içinde.

Anomali algılama teklifi, başlamanıza yardımcı olmak için kullanışlı Araçlar ile birlikte gelir.

* [Web uygulaması](https://anomalydetection-aml.azurewebsites.net/) değerlendirmek ve verileriniz üzerinde anomali algılama API'leri sonuçlarını görselleştirmenize yardımcı olur.

> [!NOTE]
> Deneyin **BT Anomali öngörüleri çözüm** tarafından desteklenen [bu API](https://gallery.cortanaintelligence.com/MachineLearningAPI/Anomaly-Detection-2)
>
<!-- This Solution is no longer available
> To get this end to end solution deployed to your Azure subscription <a href="https://gallery.cortanaintelligence.com/Solution/Anomaly-Detection-Pre-Configured-Solution-1" target="_blank">**Start here >**</a>
-->

## <a name="api-deployment"></a>API dağıtımı
API'yi kullanmak için Azure aboneliğinizde bir Azure Machine Learning web hizmeti olarak nerede barındırılacağını dağıtmanız gerekir.  Bu işlemi yapabileceğiniz [Azure AI Gallery](https://gallery.cortanaintelligence.com/MachineLearningAPI/Anomaly-Detection-2).  Bu işlem, Azure aboneliğinize iki Azure Machine Learning Studio Web hizmeti (ve bunların ilgili kaynaklarını) dağıtır. Bu, mevsimsellik algılama ile anomali algılama ve bir mevsimlik algılama olmadan bir tane.  Dağıtım tamamlandıktan sonra API 'lerinizi [Azure Machine Learning Studio Web Hizmetleri](https://services.azureml.net/webservices/) sayfasından yönetebileceksiniz.  Bu sayfada, API'yi çağırmak için uç noktalarına, API anahtarları, hem de örnek kodu bulmak mümkün olacaktır.  Daha ayrıntılı yönergeleri [burada](https://docs.microsoft.com/azure/machine-learning/machine-learning-manage-new-webservice).

## <a name="scaling-the-api"></a>API ölçeklendirme
Varsayılan olarak, dağıtımınızı 1.000 işlem/ay ve işlem 2 saat/ay içeren ücretsiz bir fatura geliştirme ve Test planı gerekir.  Gereksinimlerinize göre başka bir plana yükseltebilirsiniz.  Farklı planı fiyatlandırması hakkında ayrıntılı bilgi kullanılabilir [burada](https://azure.microsoft.com/pricing/details/machine-learning/) "Üretim Web API'si fiyatlandırma" altında.

## <a name="managing-aml-plans"></a>AML yönetme planları
Faturalandırma planınıza yönetebileceğiniz [burada](https://services.azureml.net/plans/).  Plan adı API dağıtırken seçtiğiniz kaynak grubu adı yanı sıra, aboneliğiniz için benzersiz bir dize hesaplanır.  Planınızı yükseltmek yönergeler kullanılabilir [burada](https://docs.microsoft.com/azure/machine-learning/machine-learning-manage-new-webservice) "faturalandırma planları yönetme" bölümünde.

## <a name="api-definition"></a>API Tanımı
Web hizmeti REST tabanlı bir API sağlayan bir web veya mobil uygulama, R, Python, Excel gibi farklı yollarla tüketilebilir HTTPS üzerinden vb.  Bu hizmet bir REST API çağrısı ile zaman serisi verilerinizi göndermek ve aşağıda açıklanan üç anomali türlerini birlikte çalışır.

## <a name="calling-the-api"></a>API çağırma
API'yi çağırmak için API anahtarı ve uç noktası konumunu bilmeniz gerekir.  Bunların her ikisi de API 'yi çağırmak için örnek kod ile birlikte [Azure Machine Learning Studio Web Hizmetleri](https://services.azureml.net/webservices/) sayfasından kullanılabilir.  İstenen API için gidin ve ardından bunları bulmak için "Tüket" sekmesini tıklatın.  API Swagger bir API olarak çağırabilirsiniz unutmayın (yani URL parametresi ile `format=swagger`) veya olarak bir olmayan - Swagger API'si (yani olmadan `format` URL parametresi).  Örnek kod, Swagger biçimini kullanır.  Aşağıda bir örnek istek ve yanıt, Swagger olmayan biçime sahip.  Bu örnekler, mevsimsellik uç noktasına yönelik olması.  Mevsimsellik olmayan uç noktayı benzerdir.

### <a name="sample-request-body"></a>Örnek istek gövdesi
İstek, iki nesne içeriyor: `Inputs` ve `GlobalParameters`.  Diğerleri oluşturulmazken aşağıdaki örnek istekte bazı parametreler açıkça gönderilen (aşağı kaydırın parametreleri her uç nokta için tam bir listesi için).  Açıkça istekte gönderilmez parametreleri, aşağıda verilen varsayılan değerleri kullanır.

    {
                "Inputs": {
                        "input1": {
                                "ColumnNames": ["Time", "Data"],
                                "Values": [
                                        ["5/30/2010 18:07:00", "1"],
                                        ["5/30/2010 18:08:00", "1.4"],
                                        ["5/30/2010 18:09:00", "1.1"]
                                ]
                        }
                },
        "GlobalParameters": {
            "tspikedetector.sensitivity": "3",
            "zspikedetector.sensitivity": "3",
            "bileveldetector.sensitivity": "3.25",
            "detectors.spikesdips": "Both"
        }
    }

### <a name="sample-response"></a>Örnek Yanıtı
Görmek için unutmayın `ColumnNames` alanı içermelidir `details=true` isteğiniz bir URL parametresi olarak.  Anlamı arkasında bu alanların her biri için aşağıdaki tabloya bakın.

    {
        "Results": {
            "output1": {
                "type": "table",
                "value": {
                    "Values": [
                        ["5/30/2010 6:07:00 PM", "1", "1", "0", "0", "-0.687952590518378", "0", "-0.687952590518378", "0", "-0.687952590518378", "0"],
                        ["5/30/2010 6:08:00 PM", "1.4", "1.4", "0", "0", "-1.07030497733224", "0", "-0.884548154298423", "0", "-1.07030497733224", "0"],
                        ["5/30/2010 6:09:00 PM", "1.1", "1.1", "0", "0", "-1.30229513613974", "0", "-1.173800281031", "0", "-1.30229513613974", "0"]
                    ],
                    "ColumnNames": ["Time", "OriginalData", "ProcessedData", "TSpike", "ZSpike", "BiLevelChangeScore", "BiLevelChangeAlert", "PosTrendScore", "PosTrendAlert", "NegTrendScore", "NegTrendAlert"],
                    "ColumnTypes": ["DateTime", "Double", "Double", "Double", "Double", "Double", "Int32", "Double", "Int32", "Double", "Int32"]
                }
            }
        }
    }


## <a name="score-api"></a>Puan API
Puan API, anomali algılama mevsimsel olmayan zaman serisi verileri üzerinde çalıştırmak için kullanılır. API, anomali algılayıcıları birtakım veriler üzerinde çalışır ve anomali puanlarını döndürür.
Aşağıdaki şekilde puanı API algılayabilir anomalileri örneği gösterilmektedir. Bu zaman serisi 2 farklı düzey değişiklikleri ve 3 ani sahiptir. Kırmızı nokta siyah noktalar algılanan ani değişiklikleri gösterirken, düzeyi değiştirme, algılanan zamanı gösterir.
![Puan API][1]

### <a name="detectors"></a>Algılayıcılar
Anomali algılama API'si algılayıcıları 3 kategoriden destekler. Özel giriş parametreleri ve çıkışları her algılayıcısı ayrıntıları aşağıdaki tabloda bulunabilir.

| Algılayıcı kategorisi | Algılayıcısı | Açıklama | Giriş parametreleri | Çıkışlar |
| --- | --- | --- | --- | --- |
| Depo algılayıcıları |TSpike algılayıcısı |Ani artışlar ve düşüşler kadar değerlerine göre ilk ve üçüncü Dörtte birlikler olan algılama |*tspikealgılayıcı. duyarlılık:* 1-10 aralığında tamsayı değeri alır, varsayılan: 03 Daha yüksek değerler daha fazla bilgi yakalar ve bu sayede daha az hassas hale gelir |TSpike: ikili değerleri – '1' bir depo/DIP algılanırsa, '0' Aksi |
| Depo algılayıcıları | ZSpike algılayıcısı |Ani artışlar ve düşüşler ne kadar datapoints'ler ortalamasını olmasına göre algılayın |*zspikealgılayıcı. duyarlılık:* 1-10 aralığında tamsayı değeri Al, varsayılan: 03 Daha yüksek değerler, daha az hassas hale getirmek için daha fazla bilgi yakalar |ZSpike: ikili değerleri – '1' bir depo/DIP algılanırsa, '0' Aksi |
| Yavaş eğilim algılayıcısı |Yavaş eğilim algılayıcısı |Yavaş pozitif eğilim kümesi duyarlılığına göre algılayın |*trendalgılayıcısı. duyarlılık:* algılayıcı puanı eşiği (varsayılan: 3,25, 3,25 – 5, bunu seçmek için makul bir aralıktır; Daha az hassas) |tscore: eğilim üzerindeki anomali puanı temsil eden kayan sayısı |
| Düzeyi değiştirme algılayıcıları | Çift yönlü düzeyi değişiklik algılayıcısı |Yukarı ve aşağı düzeyi değişiklik kümesi duyarlılığına göre algılayın |*bileşik velalgılayıcısı. duyarlılık:* algılayıcı puanı eşiği (varsayılan: 3,25, 3,25 – 5, bunu seçmek için makul bir aralıktır; Daha az hassas) |rpscore: anomali puanı yukarı ve aşağı düzeyi değişiklik gösteren kayan sayı |

### <a name="parameters"></a>Parametreler
Aşağıdaki tabloda daha ayrıntılı bilgiler bu giriş parametreleri listelenir:

| Giriş parametreleri | Açıklama | Varsayılan ayar | Tür | Geçerli aralık | Önerilen aralık |
| --- | --- | --- | --- | --- | --- |
| detectors.historywindow |Anomali puanı hesaplama için kullanılan geçmişinde (veri noktalarının sayısı) |500 |integer |10-2000 |Zaman serisi bağımlı |
| detectors.spikesdips | Olup olmadığının algılanması gerektiğini yalnızca ani, yalnızca Düşüşler veya her ikisi |Her ikisi de |Numaralandırılan |Her ikisi de ani artışları, Dıp'lerin |Her ikisi de |
| bileveldetector.sensitivity |Duyarlılık düzeyinde çift yönlü algılayıcısı değiştirin. |3.25 |double |None |3,25 5 (daha düşük değerler daha hassas ortalama) |
| trenddetector.sensitivity |Pozitif eğilim algılayıcısı duyarlılık. |3.25 |double |None |3,25 5 (daha düşük değerler daha hassas ortalama) |
| tspikedetector.sensitivity |Duyarlılık TSpike algılayıcısı |3 |integer |1-10 |3-5 (daha düşük değerler daha hassas ortalama) |
| zspikedetector.sensitivity |Duyarlılık ZSpike algılayıcısı |3 |integer |1-10 |3-5 (daha düşük değerler daha hassas ortalama) |
| postprocess.tailRows |Çıktı sonuçları tutulacak en son veri noktası sayısı |0 |integer |0 (tutun, tüm veri noktaları), ya da sonuçları tutmak noktalarının sayısını belirtin |Yok |

### <a name="output"></a>Çıktı
API, tüm algılayıcılar zaman serisi verilerinizle çalışır ve anomali puanlarını ve her nokta için ikili depo göstergeleri sürede döndürür. Aşağıdaki tabloda API çıkışları listeler.

| Çıkışlar | Açıklama |
| --- | --- |
| Zaman |Ham verileri veya toplanan (ve/veya) imputed verilerden zaman damgaları, toplama (ve/veya) eksik veri imputation uygulanır |
| Veriler |Değerleri ham verileri veya toplanan (ve/veya) imputed veri, toplama (ve/veya) eksik veri imputation uygulanır |
| TSpike |Bir depo TSpike algılayıcı tarafından algılanan olup olmadığını belirtmek için ikili göstergesi |
| ZSpike |Bir depo ZSpike algılayıcı tarafından algılanan olup olmadığını belirtmek için ikili göstergesi |
| rpscore |Kayan sayı temsil eden anomali puan çift yönlü düzeyi değişiklik |
| rpalert |bir çift yönlü düzeyi var. gösteren 1/0 değeri giriş hassasiyete göre anomali değiştirme |
| tscore |Kayan sayı temsil eden anomali pozitif eğilim üzerindeki Puanlama |
| talert |Giriş hassasiyete göre bir pozitif eğilim anomali var. gösteren 1/0 değeri olan |

## <a name="scorewithseasonality-api"></a>ScoreWithSeasonality API
ScoreWithSeasonality API, anomali algılama dönemsel desenleri olan zaman serisi çalıştırmak için kullanılır. Bu API, sapmalar dönemsel desenleri algılamak kullanışlıdır.
Aşağıdaki şekilde bir mevsimsel zaman serisinde algılanan anomalileri örneği gösterilmektedir. Zaman serisi, bir depo (1 siyah nokta), (2 siyah nokta ve biri sonunda) iki dıps ve tek düzey değişikliği (kırmızı nokta) sahiptir. DIP zaman serisi ve düzey değişikliği ortasında yalnızca ölçek dönemsel bileşenleri serisinden kaldırıldıktan sonra dikkat edin.
![Mevsimsellik API][2]

### <a name="detectors"></a>Algılayıcılar
Mevsimsellik uç noktasını algılayıcılar olanlara mevsimsellik olmayan uç noktasını, ancak biraz farklı parametre adları (aşağıda listelenmiştir) ile benzerdir.

### <a name="parameters"></a>Parametreler

Aşağıdaki tabloda daha ayrıntılı bilgiler bu giriş parametreleri listelenir:

| Giriş parametreleri | Açıklama | Varsayılan ayar | Tür | Geçerli aralık | Önerilen aralık |
| --- | --- | --- | --- | --- | --- |
| preprocess.aggregationInterval |Zaman serisi toplama aralığı toplamak için saniye cinsinden girin |0 (hiçbir toplama gerçekleştirilir) |integer |0: toplama, 0 > Aksi halde atlayın |5 dakika ile 1 gün, zaman serisi bağımlı |
| preprocess.aggregationFunc |İçinde belirtilen AggregationInterval verileri toplamak için kullanılan işlevi |Ortalama |Numaralandırılan |Ortalama, toplam uzunluğu |Yok |
| preprocess.replaceMissing |Eksik veri impute için değerler |lkv (bilinen son değer) |Numaralandırılan |sıfır, lkv ortalaması |Yok |
| detectors.historywindow |Anomali puanı hesaplama için kullanılan geçmişinde (veri noktalarının sayısı) |500 |integer |10-2000 |Zaman serisi bağımlı |
| detectors.spikesdips | Olup olmadığının algılanması gerektiğini yalnızca ani, yalnızca Düşüşler veya her ikisi |Her ikisi de |Numaralandırılan |Her ikisi de ani artışları, Dıp'lerin |Her ikisi de |
| bileveldetector.sensitivity |Duyarlılık düzeyinde çift yönlü algılayıcısı değiştirin. |3.25 |double |None |3,25 5 (daha düşük değerler daha hassas ortalama) |
| postrenddetector.sensitivity |Pozitif eğilim algılayıcısı duyarlılık. |3.25 |double |None |3,25 5 (daha düşük değerler daha hassas ortalama) |
| negtrenddetector.sensitivity |Negatif eğilim algılayıcısı duyarlılık. |3.25 |double |None |3,25 5 (daha düşük değerler daha hassas ortalama) |
| tspikedetector.sensitivity |Duyarlılık TSpike algılayıcısı |3 |integer |1-10 |3-5 (daha düşük değerler daha hassas ortalama) |
| zspikedetector.sensitivity |Duyarlılık ZSpike algılayıcısı |3 |integer |1-10 |3-5 (daha düşük değerler daha hassas ortalama) |
| seasonality.Enable |Mevsimsellik analiz gerçekleştirilmesi olup |true |boole |TRUE, false |Zaman serisi bağımlı |
| seasonality.numSeasonality |Algılanacak Periyodik döngülerini sayısı |1 |integer |1, 2 |1-2 |
| seasonality.Transform |Mevsimsel olmadığını (ve) eğilim bileşenleri anomali algılama uygulamadan önce kaldırılması |deseason |Numaralandırılan |None, deseason, deseasontrend |Yok |
| postprocess.tailRows |Çıktı sonuçları tutulacak en son veri noktası sayısı |0 |integer |0 (tutun, tüm veri noktaları), ya da sonuçları tutmak noktalarının sayısını belirtin |Yok |

### <a name="output"></a>Çıktı
API, tüm algılayıcılar zaman serisi verilerinizle çalışır ve anomali puanlarını ve her nokta için ikili depo göstergeleri sürede döndürür. Aşağıdaki tabloda API çıkışları listeler.

| Çıkışlar | Açıklama |
| --- | --- |
| Zaman |Ham verileri veya toplanan (ve/veya) imputed verilerden zaman damgaları, toplama (ve/veya) eksik veri imputation uygulanır |
| OriginalData |Değerleri ham verileri veya toplanan (ve/veya) imputed veri, toplama (ve/veya) eksik veri imputation uygulanır |
| ProcessedData |Şunlardan biri: <ul><li>Önemli mevsimsellik algılandı ve bu seçeneğe deseason zaman serisi sezona göre ayarlanır;</li><li>sezona göre ayarlanır ve önemli mevsimsellik algılanırsa zaman serileri ve deseasontrend seçeneği belirlenmiş eğilimden arındırılmış</li><li>Aksi takdirde, bu OriginalData aynıdır</li> |
| TSpike |Bir depo TSpike algılayıcı tarafından algılanan olup olmadığını belirtmek için ikili göstergesi |
| ZSpike |Bir depo ZSpike algılayıcı tarafından algılanan olup olmadığını belirtmek için ikili göstergesi |
| BiLevelChangeScore |Kayan sayı temsil eden anomali puan düzeyi değişiklik |
| BiLevelChangeAlert |Giriş hassasiyete göre bir düzey değişikliği anomali olduğunu gösteren 1/0 değeri var. |
| PosTrendScore |Kayan sayı temsil eden anomali pozitif eğilim üzerindeki Puanlama |
| PosTrendAlert |Giriş hassasiyete göre bir pozitif eğilim anomali var. gösteren 1/0 değeri olan |
| NegTrendScore |Kayan sayı temsil eden anomali puana negatif eğilim üzerindeki |
| NegTrendAlert |Giriş hassasiyete göre bir negatif eğilim anomali var. gösteren 1/0 değeri olan |

[1]: ./media/apps-anomaly-detection-api/anomaly-detection-score.png
[2]: ./media/apps-anomaly-detection-api/anomaly-detection-seasonal.png

