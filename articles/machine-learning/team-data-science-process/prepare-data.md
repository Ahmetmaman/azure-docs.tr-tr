---
title: Temiz ve Azure Machine Learning - Team Data Science Process için verileri hazırlama
description: Önceden işleme ve machine learning için etkili bir şekilde kullanılmasını hazırlamak için verileri temizledik.
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.component: team-data-science-process
ms.topic: article
ms.date: 11/09/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: a926edc3409290a0e8cd89fd909427833f9e1427
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2018
ms.locfileid: "53134378"
---
# <a name="tasks-to-prepare-data-for-enhanced-machine-learning"></a>Gelişmiş machine learning için verileri hazırlama görevleri
Ön işleme ve verileri temizleme veri kümesi machine learning için etkili bir şekilde kullanılabilmesi için önce genellikle gerçekleştirilmesi gereken önemli görevlerdir. Ham veriler genellikle gürültülü ve güvenilmeyen ve değerleri eksik olabilir. Modelleme için bu verileri kullanarak, yanıltıcı sonuçlara neden olabilir. Bu görevler Team Data Science işlem (TDSP) bir parçasıdır ve genellikle ilk İnceleme bulmak ve gerekli ön işleme planlamak için kullanılan bir veri kümesinin izleyin. Özetlenen adımları daha ayrıntılı TDSP işlemi hakkında yönergeler için bkz: [Team Data Science Process](overview.md).

Ön işleme ve temizleme görevlerini, veri araştırma görevi gibi çeşitli ortamlar, örneğin SQL veya Hive veya Azure Machine Learning Studio ve çeşitli araçları ve dilleri, R veya Python verilerinizin depolandığı bağlı olarak, gibi gerçekleştirilebilme ve nasıl biçimlendirilir. TDSP doğası gereği yinelemeli olduğundan, bu görevleri iş akışı işleminin çeşitli adımları yere alabilir.

Bu makalede, çeşitli veri işleme kavramları ve önce veya sonra veri Azure Machine Learning içine alma üstlendiği görevleri tanıtır.

Veri keşfi ve Azure Machine Learning studio içinde yapılan ön işleme örneği için bkz: [Azure Machine Learning Studio'da verilerin önceden işlenmesi](https://azure.microsoft.com/documentation/videos/preprocessing-data-in-azure-ml-studio/) video.

## <a name="why-pre-process-and-clean-data"></a>Neden önceden işleme ve veri temizleme?
Gerçek veriler çeşitli kaynaklardan veri toplandıktan ve işlemleri ve sürdürmenin veya bozuk veri kümesinin kaliteden ödün içerebilir. Ortaya çıkan tipik veri kalite sorunları şunlardır:

* **Tamamlanmamış**: öznitelikler veya eksik değerler içeren veri eksik.
* **Gürültülü**: veri hatalı kayıt veya aykırı değerler içeriyor.
* **Tutarsız**: veri çakışan kayıtları veya tutarsızlıklar içeriyor.

Kalite verileri, kalite Tahmine dayalı modelleri için bir önkoşuldur. "Çöp içinde çöp" kaçının, veri kalitesini artırmak ve performans bu nedenle modellemek için veri sorunları erkenden saptayın ve karşılık gelen veri işleme ve temizleme adımları karar vermek için bir veri sistem durumu ekran yürütmek için zorunludur.

## <a name="what-are-some-typical-data-health-screens-that-are-employed"></a>Çalışan bazı tipik veri sistem durumu ekranlar nelerdir?
Biz, veri genel kalitesini denetleyerek denetleyebilirsiniz:

* Sayısını **kayıtları**.
* Sayısını **öznitelikleri** (veya **özellikleri**).
* Öznitelik **veri türleri** (nominal, sıralı veya sürekli).
* Sayısını **eksik değerleri**.
* **Doğru biçimlendirilmiş XML** veri.
  * TSV veya CSV veri olması durumunda sütun ayırıcıları ve satır ayıraçlarını sütunları ve satırları her zaman doğru ayrı kontrol edin.
  * HTML veya XML biçiminde veriler ise verileri kendi ilgili standartlarına göre biçimlendirilmemiş olup olmadığını denetleyin.
  * Ayrıştırma de bu kadar yapılandırılmış bilgiler yarı yapılandırılmış veya yapılandırılmamış verileri ayıklamak için gerekli olabilir.
* **Tutarsız veri kayıtlarının**. Onay değerleri aralığı izin verilir. Örneğin Öğrenci GPA, belirlenen aralıkta GPA olup olmadığını denetleyin veri içeriyorsa, 0 söyleyin ~ 4.

Veri ile ilgili sorunlar bulduğunuzda **işleme adımları** genellikle temizleme eksik değerleri, veri normalleştirme ayrılma içerir. gerekli, kaldırma ve/veya değiştirmek için metin işleme katıştırılmış veri etkileyebilecek karakterler hizalama, karma veri ortak alanları ve diğer türleri.

**Azure Machine Learning kullanan iyi biçimlendirilmiş bir tablo veri**.  Verileri tablo biçiminde ise, veri ön işleme, doğrudan Azure Machine Learning Machine Learning Studio'da ile gerçekleştirilebilir.  Veri tablosal biçimde, XML'de olduğu say değilse ayrıştırma verileri tablo biçimine dönüştürmek için gerekli olabilir.  

## <a name="what-are-some-of-the-major-tasks-in-data-pre-processing"></a>Veri ön işleme önemli görevleri bazıları nelerdir?
* **Veri temizleme**: doldurun veya eksik değerleri algılayacak ve gürültülü veri ve aykırı değerleri.
* **Veri dönüştürme**: boyutları ve gürültüsünü azaltmak için veri Normalleştir.
* **Veri azaltma**: örnek verileri kayıtları ya da daha kolay veri işleme için öznitelikler.
* **Veri ayrılma**: Dönüştür sürekli özniteliklerin kullanım kolaylığı için kategorik öznitelikleri belirli machine learning yöntemleriyle.
* **Metin Temizleme**: Örneğin, bir sekmeyle ayrılmış veri dosyası katıştırılmış sekmelerde katıştırılmış için kayıtları, vb. kesilebilir yeni satırlar veri hizalanmama neden olabilecek katıştırılmış karakterleri kaldırın.

Aşağıdaki bölümlerde bu veri işleme adımlardan bazıları ayrıntılı olarak açıklanmaktadır.

## <a name="how-to-deal-with-missing-values"></a>Eksik değerleri ile nasıl?
Eksik değerleri ile dağıtmak için öncelikle sorunu daha iyi işleyebilmek için eksik değerleri nedeni tanımlamak idealdir. Tipik eksik değer işleme yöntemler şunlardır:

* **Silme**: eksik değerleri ile kayıt kaldırma
* **Kukla değiştirme**: eksik değerleri bir kukla değer ile değiştirin: Örneğin, *bilinmeyen* kategorik veya 0 için sayısal değerler için.
* **Değiştirme anlamına**: eksik veri sayısal ise, eksik değerlerin ortalaması ile değiştirin.
* **Sık kullanılan değiştirme**: eksik veri kategorik ise, eksik değerleri en sık rastlanan öğeyle değiştirin.
* **Regresyon değiştirme**: eksik değerleri gerileyen değerlerle değiştirmek için bir regresyon yöntemi kullanın.  

## <a name="how-to-normalize-data"></a>Veri'leri normalleştirmek nasıl?
Veri normalleştirme sayısal değerleri belirtilen bir aralıkta yeniden ölçeklendirir. Popüler veri normalleştirme yöntemler şunlardır:

* **Min-Maks normalleştirme**: doğrusal olarak bir aralıkta verileri dönüştürme, 0 ve 1 en küçük değer burada ölçeği 0 ve en büyük değeri 1 arasında varsayalım.
* **Z-puanı normalleştirme**: ortalama ve standart sapma göre verileri ölçeklendirmenize: veri ortalaması arasındaki farkı tarafından standart sapma bölün.
* **Ondalık ölçeklendirme**: öznitelik değeri ondalık taşıyarak veri ölçeklendirin.  

## <a name="how-to-discretize-data"></a>Veri ayırmak nasıl?
Veri nominal öznitelikleri veya aralıklarla sürekli değer dönüştürerek ayrılmış. Bu bazı yollar şunlardır:

* **Eşit genişlik gruplama**: bir özniteliğin tüm olası değerler aralığı aynı boyutta N gruplara ayırın ve depo sayı ile denk gelen bir depo değerler atayın.
* **Eşit yükseklik gruplama**: N gruplara, her örneği aynı sayısını içeren bir özniteliğin tüm olası değerler aralığı ayırın ve ardından bin sayı ile denk gelen bir depo değerler atayın.  

## <a name="how-to-reduce-data"></a>Verileri azaltmak nasıl?
Daha kolay veri işleme için veri boyutunu düşürmek için çeşitli yöntemler vardır. Veri boyutu ve etki alanına bağlı olarak, aşağıdaki yöntemleri uygulanabilir:

* **Kayıt örnekleme**: veri kayıtları örnek ve yalnızca temsili bir alt kümesi verileri seçin.
* **Öznitelik örnekleme**: yalnızca bir öznitelik alt kümesinden en önemli verileri seçin.  
* **Toplama**: veri gruplara ayırın ve her grup için sayıları depolayın. Örneğin, günlük gelir rakamları son 20 yılda bir restoran zinciri verilerin boyutunu azaltmak için aylık gelir toplanabilir.  

## <a name="how-to-clean-text-data"></a>Metin verilerini temizlemek nasıl?
**Tablosal veri metin alanlarında** sütunları hizalama ve/veya kaydı sınırlarını etkileyen karakter içerebilir. Örneğin, bir sekmeyle ayrılmış dosya neden olan sütun hizalanmama sekmeleri katıştırılmış ve katıştırılmış yeni satır karakterleri kayıt satırları bölün. Metin yazma/okuma sırasında hatalı bir metin kodlama işleme için bilgi kaybı, giriş okunamaz karakter, örneğin, null değerlere ve de etkiler metni ayrıştırma yanlışlıkla yol açar. Dikkatli ayrıştırma ve düzenleme metin alanları için uygun hizalama ve/veya metin yapılandırılmamış veya yarı yapılandırılmış verilerden ayıklama yapılandırılmış verilere temizlemek için gerekli olabilir.

**Veri keşfi** verileri erken bir görünüm sunar. Bu adım sırasında birkaç veri sorun yazdığı ortaya çıkarıldı olabilir ve karşılık gelen yöntemlere bu sorunları gidermeye yönelik uygulanabilir.  Sorunun kaynağı nedir ve nasıl sorun sunulmuş gibi sorular sormak önemlidir. Bu ayrıca bunları çözmek için yapılması gereken veri işleme adımları karar vermenize yardımcı olur. Bir veri türetilen amaçlayan ınsights türü, veri işleme çaba önceliğini belirlemek için de kullanılabilir.

## <a name="references"></a>Başvurular
> *Veri madenciliği: Kavramları ve teknikleri*, üçüncü baskı, Morgan Kaufmann Jiawei Han Micheline Kamber ve Jian Pei 2011
> 
> 

