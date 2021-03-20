---
title: 'Performansı ayarlama: Hive, HDInsight & Azure Data Lake Storage 2. | Microsoft Docs'
description: Hive, HDInsight ve Azure Data Lake Storage 2. kullanarak g/ç yoğunluklu sorgular için ayarlama kılavuzunu anlayın.
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: how-to
ms.date: 11/18/2019
ms.author: normesta
ms.reviewer: stewu
ms.openlocfilehash: 4b1e5dd3c72122ade2fd4d4092bb18a7acf215f5
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "95912952"
---
# <a name="tune-performance-hive-hdinsight--azure-data-lake-storage-gen2"></a>Performansı ayarlama: Hive, HDInsight & Azure Data Lake Storage 2.

Varsayılan ayarlar, birçok farklı kullanım durumunda iyi bir performans sağlamak üzere ayarlanmıştır.  G/ç yoğun sorgularda, Hive Azure Data Lake Storage 2. daha iyi performans sağlamak için ayarlanabilir.  

## <a name="prerequisites"></a>Önkoşullar

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).
* **Data Lake Storage 2. hesabı**. Bir oluşturma hakkında yönergeler için bkz [. hızlı başlangıç: Azure Data Lake Storage 2. depolama hesabı oluşturma](../common/storage-account-create.md)
* Data Lake Storage 2. hesabına erişimi olan **Azure HDInsight kümesi** . Bkz. [Azure HDInsight kümeleri ile Azure Data Lake Storage 2. kullanma](../../hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2.md)
* **HDInsight üzerinde Hive çalıştırma**.  HDInsight üzerinde Hive işleri çalıştırma hakkında bilgi edinmek için bkz. [HDInsight 'Ta Hive kullanma](../../hdinsight/hadoop/hdinsight-use-hive.md)
* **Data Lake Storage 2. performans ayarlama yönergeleri**.  Genel performans kavramları için bkz. [Data Lake Storage 2. performans ayarlama Kılavuzu](data-lake-storage-performance-tuning-guidance.md)

## <a name="parameters"></a>Parametreler

Gelişmiş Data Lake Storage 2. performansını ayarlamaya yönelik en önemli ayarlar şunlardır:

* **Hive. tez. Container. size** : her görev tarafından kullanılan bellek miktarı

* **tez. Grouping. min-size** – her bir eşleştiricisindeki minimum boyut

* **tez. Grouping. Max-size** – her bir Eşleştiricideki en büyük boyut

* **hive.exec. Reducer. bytes. per. Reducer** – her Reducer boyutu

**Hive. tez. Container. size** -kapsayıcı boyutu, her görev için kullanılabilir bellek miktarını belirler.  Bu, Hive içindeki eşzamanlılık denetimi için ana giriştir.  

**tez. Grouping. min-size** – Bu parametre, her Eşleyici için en küçük boyutu ayarlamanıza olanak sağlar.  Tez 'nin seçtiği mapbir sayı bu parametrenin değerinden küçükse tez burada ayarlanan değeri kullanır.

**tez. Grouping. Max-size** – parametresi, her bir Eşleştiricideki en büyük boyutu ayarlamanıza olanak sağlar.  Tez 'nin seçtiği mapbir sayı bu parametrenin değerinden büyükse tez burada ayarlanan değeri kullanır.

**hive.exec. Reducer. bytes. per. Reducer** – Bu parametre her bir Reducer boyutunu ayarlar.  Varsayılan olarak, her Reducer 256 MB 'dir.  

## <a name="guidance"></a>Rehber

**hive.exec. Reducer. bytes. per. Reducer** – varsayılan değer sıkıştırılmamış olduğunda iyi sonuç verir.  Sıkıştırılmış veriler için Reducer boyutunu azaltmanız gerekir.  

**Hive. tez. Container. size ayarlayın** – her düğümde, bellek yarn. NodeManager. Resource. Memory-MB tarafından belirtilir ve varsayılan olarak HDI kümesi üzerinde doğru şekilde ayarlanmalıdır.  YARN 'de uygun belleği ayarlama hakkında daha fazla bilgi için bu [gönderisini](../../hdinsight/hdinsight-hadoop-hive-out-of-memory-error-oom.md)inceleyin.

G/ç yoğunluklu iş yükleri, tez kapsayıcı boyutunu azaltarak daha paralellik avantajdan faydalanabilir. Bu, kullanıcıya eşzamanlılık artıran daha fazla kapsayıcı sağlar.  Ancak, bazı Hive sorguları önemli miktarda bellek (ör. Mapjoın) gerektirir.  Görev yeterli belleğe sahip değilse, çalışma zamanı sırasında yetersiz bellek özel durumu alacaksınız.  Bellek dışında özel durumlar alıyorsanız belleği artırmanız gerekir.   

Ya da paralellik çalıştıran, eşzamanlı görev sayısı toplam YARN bellekle sınırlı olacaktır.  YARN kapsayıcılarının sayısı, kaç tane eşzamanlı görevin çalıştırılacağını dikte eder.  Düğüm başına YARN belleği bulmak için, ambarı 'na gidebilirsiniz.  YARN 'ye gidin ve configs sekmesini görüntüleyin.  YARN belleği Bu pencerede görüntülenir.  

- Toplam YARN bellek = düğüm * düğüm başına YARN bellek
- \# YARN kapsayıcıları = toplam YARN bellek/tez kapsayıcı boyutu

Data Lake Storage 2. kullanarak performansı iyileştirmeye yönelik anahtar, olabildiğince fazla eşzamanlılık artışı sağlar.  Tez, oluşturulması gereken görev sayısını otomatik olarak hesaplar, bu nedenle ayarlamanız gerekmez.   

## <a name="example-calculation"></a>Örnek hesaplama

8 düğümlü bir D14 kümeniz olduğunu varsayalım.  

- Toplam YARN bellek = düğüm * düğüm başına YARN bellek
- Toplam YARN bellek = 8 düğüm * 96GB = 768GB
- \# YARN kapsayıcıları = 768/3072MB = 256

## <a name="further-information-on-hive-tuning"></a>Hive ayarlama hakkında daha fazla bilgi

Hive sorgularınızı ayarlamaya yardımcı olacak birkaç blog aşağıda verilmiştir:
* [HDInsight 'ta Hadoop için Hive sorgularını iyileştirme](../../hdinsight/hdinsight-hadoop-optimize-hive-query.md)
* [Azure HDInsight’ta Apache Hive sorgularını iyileştirme](../../hdinsight/hdinsight-hadoop-optimize-hive-query.md)
* [HDInsight 'ta en iyileştirme Hive ile Ignite konuşur](https://channel9.msdn.com/events/Machine-Learning-and-Data-Sciences-Conference/Data-Science-Summit-2016/MSDSS25)