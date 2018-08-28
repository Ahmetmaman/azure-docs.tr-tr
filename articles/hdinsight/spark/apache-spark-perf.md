---
title: Performans - Azure HDInsight için Spark işlerini en iyi duruma getirme
description: Spark kümeleri, en iyi performans için ortak stratejiler gösterilmektedir.
services: hdinsight
ms.service: hdinsight
author: maxluk
ms.author: maxluk
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/11/2018
ms.openlocfilehash: 4a7777be01cc15ed5cc4c9c091230afe1ddfa897
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "43047451"
---
# <a name="optimize-spark-jobs"></a>Spark işlerini en iyi duruma getirme

Spark küme yapılandırması, belirli iş yükü için en iyi duruma getirmeyi öğrenin.  En sık karşılaşılan hatalı yapılandırmalar (özellikle yanlış boyutlu yürütücüler), uzun süre çalışan işlemleri ve Kartezyen işlemlerinde neden görevler nedeniyle bellek baskısı zorluktur. İşleri uygun önbelleğe alma ve verme için hızlandırabilirsiniz [veri dengesizliği](#optimize-joins-and-shuffles). En iyi performans için izleme ve uzun süre çalışan ve kaynak tüketen Spark iş yürütmeleri gözden geçirin.

Aşağıdaki bölümlerde, yaygın bir Spark işi iyileştirmeler ve önerileri açıklanmaktadır.

## <a name="choose-the-data-abstraction"></a>Veri Özet seçin

Soyut veri ve Spark 1.x kullandığı Rdd spark 2.x sunulan veri çerçevelerini ve veri kümeleri. Şu göreli değeri göz önünde bulundurun:

* **Veri çerçevelerini**
    * Çoğu durumda en iyi seçimi
    * Sorgu iyileştirme Catalyst ile sağlar
    * Tüm aşama kod oluşturma
    * Doğrudan bellek erişimi
    * Düşük atık toplama (GC) kullanımı
    * Değil olarak Geliştirici dostu veri kümeleri, herhangi bir derleme zamanı denetimleri veya etki alanı nesnesi programlama olduğundan
* **Veri kümeleri**
    * Performans etkisi kabul edilebilir olduğu karmaşık ETL işlem hatları hazır
    * Burada, performans etkisini önemli ölçüde olabilir toplamada iyi değil
    * Sorgu iyileştirme Catalyst ile sağlar
    * Etki alanı nesnesi programlama ve derleme zamanı denetimleri sağlayarak Geliştirici dostu
    * Serileştirme/seri durumundan çıkarma yükü ekler
    * Yüksek GC ek yükü
    * Tüm aşama kod oluşturma keser
* **Rdd**
    * Spark 2.x gerektirmeyen Rdd, kullanılacak yeni bir özel RDD oluşturmaya gerekmedikçe
    * Hiçbir sorgu iyileştirme Catalyst ile
    * Tüm aşama kod üretme
    * Yüksek GC ek yükü
    * Spark 1.x eski kullanmalısınız API'leri

## <a name="use-optimal-data-format"></a>En iyi veri biçimini kullanın

Spark, csv, json, xml, parquet, orc ve avro gibi birçok biçimi destekler. Spark uzatabilirsiniz dış veri kaynaklarıyla - pek çok daha fazla biçimde desteklemek daha fazla bilgi için bkz: [Spark paketleri](https://spark-packages.org).

En iyi performans ile parquet biçimi *snappy sıkıştırma*, Spark varsayılan değer olan 2.x. Parquet, verileri sütunlu biçiminde depolar ve Spark, yüksek oranda iyileştirilmiştir.

## <a name="select-default-storage"></a>Varsayılan depolama alanı seçin

Yeni bir Spark kümesi oluşturduğunuzda, kümenin varsayılan depolama alanı olarak Azure Blob Depolama veya Azure Data Lake Store tercih yapma seçeneğine sahip olursunuz. Kümenizi sildiğinizde, verilerinizi otomatik olarak silinmez için iki seçenek de uzun vadeli depolama avantajı geçici kümeler için size. Geçici bir küme oluşturun ve verilerinize erişmeye devam edebilirsiniz.

| Store türü | Dosya Sistemi | Hız | Geçici | Kullanım örnekleri |
| --- | --- | --- | --- | --- |
| Azure Blob Depolama | **wasb:**//url/ | **Standart** | Evet | Geçici küme |
| Azure Data Lake Store | **Adl:**//url/ | **Daha hızlı** | Evet | Geçici küme |
| Yerel HDFS | **hdfs:**//url/ | **Hızlı** | Hayır | Etkileşimli 7/24 küme |

## <a name="use-the-cache"></a>Önbellek kullanma

Spark gibi farklı yöntemler üzerinden kullanılabilecek kendi yerel önbelleğe alma mekanizması sağlar `.persist()`, `.cache()`, ve `CACHE TABLE`. Bu yerel önbelleğe alma de küçük veri kümeleriyle önbellek Ara sonuçlar için gerek duyduğunuz ETL işlem hatları olduğu gibi etkilidir. Önbelleğe alınan bir tablo bölümleme verileri saklamasa beri ancak Spark yerel önbelleğe alma şu anda iyi bölümleme ile çalışmaz. Daha fazla geneldir ve güvenilir bir önbelleğe alma teknik *depolama katmanı önbelleğe alma*.

* Yerel Spark (önerilmez) önbelleğe alma
    * Küçük veri kümeleri için iyidir.
    * Hangi Spark yayınlarda gelecekte değişebilir bölümleme ile değil çalışır.

* (Önerilen) depolama düzeyi önbelleğe alma
    * Kullanılarak uygulanan [Alluxio](http://www.alluxio.org/).
    * Belleğe ve SSD önbelleği kullanır.

* Yerel HDFS (önerilir)
    * `hdfs://mycluster` yolu.
    * SSD önbelleği kullanır.
    * Önbellek yeniden oluşturma gerektiren küme sildiğinizde önbelleğe alınan veriler kaybolacak.

## <a name="use-memory-efficiently"></a>Belleği verimli şekilde kullanma

Spark, bellek kaynakları yönetme Spark işlerinin yürütülmesi en iyi duruma getirme, temel bir yönü, bu nedenle, bellekte veri yerleştirerek çalışır.  Kümenizin bellek kullanabildiğinden uygulayabileceğiniz çeşitli teknikler vardır.

* Daha küçük veri bölümlerine tercih eder ve veri boyutu, türleri ve bölümleme stratejisinde dağıtımlarında hesap.
* Yeni, daha verimli göz önünde bulundurun [Kryo veri seri hale getirme](https://github.com/EsotericSoftware/kryo), varsayılan Java serileştirme yerine.
* YARN, kullanarak kendisini ayıran gibi tercih ettiğiniz `spark-submit` batch tarafından işlenecek.
* İzleyin ve Spark yapılandırma ayarlarını ayarlayın.

Referans olması açısından Spark bellek yapısı ve bazı önemli Yürütücü bellek parametreler sonraki görüntüde gösterilmektedir.

### <a name="spark-memory-considerations"></a>Spark bellek konuları

YARN kullanıyorsanız, YARN her Spark düğümdeki tüm kapsayıcıları tarafından kullanılan bellek en yüksek toplamını denetler.  Aşağıdaki diyagramda, anahtar nesneleri ve aralarındaki ilişkiler gösterilmektedir.

![YARN Spark bellek yönetimi](./media/apache-spark-perf/yarn-spark-memory.png)

'Yetersiz bellek' iletileri çözmek için aşağıdakileri deneyin:

* DAG yönetim seçeneği gözden geçirin. Eşleme tarafında reducting tarafından azaltmak, önceden bölüm (veya demetlemek) veri kaynağı, tek seçeneği en üst düzeye çıkarmak ve gönderilen veri miktarını azaltın.
* Tercih ettiğiniz `ReduceByKey` sabit bellek sınırına ile `GroupByKey`, toplamalar, Pencereleme ve diğer işlevleri sağlar ancak ann sınırsız bellek sınırı vardır.
* Tercih ettiğiniz `TreeReduce`, hangi çalışır daha fazla yürütücüler veya bölüm için `Reduce`, sürücüsündeki tüm iş yapar.
* Alt düzey RDD nesneler yerine veri çerçevelerini yararlanın.
* "İlk N", çeşitli toplamalar veya Pencereleme işlemleri gibi eylemleri kapsayan ComplexTypes oluşturun.

## <a name="optimize-data-serialization"></a>Veri seri hale getirme en iyi duruma getirme

Spark işlerinde dağıtılır, uygun veri seri hale getirme en iyi performansı için önemlidir.  Spark için iki seri hale getirme seçeneği vardır:

* Java serileştirme varsayılandır.
* Kryo serileştirme daha yeni bir biçimidir ve daha hızlı neden olabilir ve daha Java serileştirme sıkıştırın.  Programınızda sınıflarını Kaydet ve tüm Serializable türler henüz desteklemiyor Kryo gerektirir.

## <a name="use-bucketing"></a>Benzeyebilir kullanın

Benzeyebilir veri bölümleme için benzer, ancak her bir demete sütun değerleri kümesi yerine tek basılı tutabilirsiniz. Ürün tanımlayıcıları gibi değerlere (milyon veya daha fazla), çok sayıda üzerinde bölümleme için benzeyebilir çalışır. Bir demet satırın demetine anahtarı karma işlevi tarafından belirlenir. Bunlar hakkında nasıl bunlar kümelenmiş sıralanır ve meta verileri depolamak için kümelenmiş tablolar benzersiz iyileştirmeler sağlar.

Bazı gelişmiş benzeyebilir özellikleri şunlardır:

* En iyi duruma getirme benzeyebilir meta-bilgilere göre sorgulama
* En iyi duruma getirilmiş toplama
* En iyi duruma getirilmiş birleştirmeler

Bölümlendirme ve aynı anda benzeyebilir kullanabilirsiniz.

## <a name="optimize-joins-and-shuffles"></a>Birleşimler ve seçeneği en iyi duruma getirme

Bir birleşim veya karışık yavaş işleri varsa, büyük olasılıkla nedeni *veri dengesizliği*, işi veri asimetri olduğu. Örneğin, bir harita işi 20 saniye sürebilir, ancak burada veri alanına karışık veya bir iş çalıştırmak saat sürer.   Veri dengesizliği düzeltmek için tüm anahtarı salt veya kullanmalısınız bir *yalıtılmış salt* yalnızca bazı alt anahtarlar için.  Yalıtılmış bir salt kullanıyorsanız, salted anahtarları eşlemi birleştirmelerdeki kümesini yalıtmak için daha fazla filtrelemek. Başka bir seçenek, bir demet sütun ekleyin ve demet ilk önceden toplama oluşturmaktır.

Birleştirme türü başka bir faktör yavaş birleştirmeler neden olabilir. Varsayılan olarak, Spark kullanan `SortMerge` birleştirme türü. Bu tür birleşim, büyük veri kümeleri için idealdir, ancak bunu sol ve sağ tarafında veri birleştirmeden önce sıralamanız gerekir çünkü aksi hesaplama açısından pahalıdır.

A `Broadcast` birleşim en uygun daha küçük veri kümeleri için veya bir birleşim tarafı diğer taraftan çok daha küçük olduğu. Bu birleşim türü için tüm yürütücüler bir tarafı yayınlar ve böylece daha fazla bellek yayınlar için genel gerektirir.

Ayarlayarak yapılandırmanızda birleşim türünü değiştirebilirsiniz `spark.sql.autoBroadcastJoinThreshold`, veya DataFrame API'lerini kullanarak bir birleşme ipucu ayarlayabilirsiniz (`dataframe.join(broadcast(df2))`).

```scala
// Option 1
spark.conf.set("spark.sql.autoBroadcastJoinThreshold", 1*1024*1024*1024)

// Option 2
val df1 = spark.table("FactTableA")
val df2 = spark.table("dimMP")
df1.join(broadcast(df2), Seq("PK")).
    createOrReplaceTempView("V_JOIN")
sql("SELECT col1, col2 FROM V_JOIN")
```

Kümelenmiş tabloları kullandığınız sonra bir birleştirme türü, üçüncü sahip `Merge` birleştirme. Doğru önceden bölümlenmiş ve önceden sıralanmış bir veri kümesi pahalı sıralama aşama atlanacak bir `SortMerge` birleştirme.

Özellikle daha karmaşık sorgularda birleştirme sırası önemlidir. En seçmeli birleştirmelerle başlatın. Ayrıca, mümkün olduğunda toplamalardan sonra satır sayısını artırmak birleştirmeler taşıyın.

Kartezyen birleşimler, özellikle olması durumunda, paralellik yönetmek için iç içe geçmiş yapılar ekleyebilirsiniz Pencereleme ve belki de Spark işinizdeki bir veya daha fazla adımları atlayın.

## <a name="customize-cluster-configuration"></a>Küme yapılandırması özelleştirme

Spark kümesi yükünüze bağlı olarak, varabilir iyileştirilmiş Spark iş yürütme varsayılan olmayan Spark yapılandırması daha fazla neden olur.  Tüm varsayılan olmayan küme yapılandırmalarını doğrulamak için örnek iş yüklerini test Kıyaslama gerçekleştirin.

Ayarlayabileceğiniz bazı ortak Parametreler şunlardır:

* `--num-executors` Yürütücü uygun sayısını ayarlar.
* `--executor-cores` Çekirdek sayısı için her bir yürütücü ayarlar. Genellikle bazı kullanılabilir bellek diğer işlemleri kullanma gibi middle-sized yürütücüler olması gerekir.
* `--executor-memory` yığın boyutu YARN üzerinde denetimleri her Yürütücü için bellek boyutunu ayarlar. Yürütme yükü için bazı bellek bırakmanız gerekir.

### <a name="select-the-correct-executor-size"></a>Doğru Yürütücü boyutunu seçin

Yürütücü yapılandırmanızı karar verirken, Java atık toplama (GC) yükü göz önünde bulundurun.

* Yürütücü boyutunu azaltmak için faktörleri:
    1. GC ek yükü < %10 tutmak için 32 GB aşağıda yığın boyutunu küçültün.
    2. GC ek yükü < %10 tutmak için çekirdek sayısını azaltın.

* Yürütücü boyutunu artırmak için faktörleri:
    1. Yürütücü ek yükü arasındaki iletişimi azaltın.
    2. Yürütücüler (N2) arasında açık bağlantıları üzerinde daha büyük kümeleri azaltmak (> 100 yürütücüler).
    3. Bellek kullanımı yoğun görevleri için uyum sağlamak için yığın boyutunu artırın.
    4. İsteğe bağlı: Yürütücü başına bellek yükünü azaltın.
    5. İsteğe bağlı: kullanımı ve eşzamanlılık CPU oversubscribing tarafından artırın.

Bir genel kural Yürütücü boyutu seçerken karşısında:
    
1. Yürütücü 30 GB ile başlayın ve kullanılabilir makine çekirdeklerinin dağıtın.
2. Daha büyük kümeleri (> 100 yürütücüler) için Yürütücü çekirdek sayısını artırın.
3. Artırma veya azaltma deneme çalıştırmaları ve GC yükü gibi önceki faktörlere göre boyutları.

Eş zamanlı sorgu çalıştırırken aşağıdakileri dikkate alın:

1. Yürütücü ve tüm makine çekirdek 30 GB ile başlayın.
2. CPU oversubscribing tarafından birden çok paralel Spark uygulamaları oluşturma (yaklaşık % 30 gecikme geliştirme).
3. Sorguları paralel uygulamalar arasında dağıtın.
4. Artırma veya azaltma deneme çalıştırmaları ve GC yükü gibi önceki faktörlere göre boyutları.

Zaman Çizelgesi Görünümü, SQL grafiği, iş istatistiklerini ve benzeri bakarak sorgu performansınızı aykırı değerleri veya diğer performans sorunlarını izleyin. Bir veya birkaç yürütücüler bazen diğerlerine göre daha yavaş ve görevleri yürütmek için daha uzun sürer. Bu sık (> 30 düğümleri) daha büyük kümeleri üzerinde gerçekleşir. Bu durumda, Zamanlayıcı için yavaş görevleri dengeleyebilirsiniz. böylece iş görevleri daha büyük bir sayıya böler. Örneğin, en az iki katı daha fazla görevleri Yürütücü çekirdek olarak vardır. Kurgusal yürütmeyi görevler ile de etkinleştirebilirsiniz `conf: spark.speculation = true`.

## <a name="optimize-job-execution"></a>İş yürütme en iyi duruma getirme

* Örneğin, verileri iki kez kullanmak ve ardından bunları önbelleğe gerekirse, önbelleğe alın.
* Tüm yürütücüler yayın değişkenleri. Değişkenleri yalnızca bir kez daha hızlı arama sonucu, serileştirilir.
* İş parçacığı havuzu birçok görev için daha hızlı işlem ile sonuçlanır sürücüsünü kullanın.

Performans sorunları için düzenli olarak, çalışan işleri izleyin. Bazı sorunları daha fazla içgörüye ihtiyacınız varsa, profil oluşturma araçları aşağıdaki performans birini göz önünde bulundurun:

* [Intel PAL aracı](https://github.com/intel-hadoop/PAT) CPU, depolama ve ağ bant genişliği kullanımını izler.
* [Oracle Java 8 görev denetimi](http://www.oracle.com/technetwork/java/javaseproducts/mission-control/java-mission-control-1998576.html) Spark hem de Yürütücü kodu profiller.

Spark 2.x sorgu performansı için anahtar üzerinde tam aşamalı kod oluşturma bağlıdır Tungsten altyapısıdır. Bazı durumlarda, tam aşamalı kod oluşturmayı devre dışı bırakılabilir. Örneğin değişemeyen bir türü kullanıyorsanız (`string`) toplama ifadesindeki `SortAggregate` yerine görünür `HashAggregate`. Örneğin, daha iyi performans için aşağıdakileri deneyin ve kod oluşturma'ı yeniden etkinleştirin:

```sql
MAX(AMOUNT) -> MAX(cast(AMOUNT as DOUBLE))
```

## <a name="next-steps"></a>Sonraki adımlar

* [Azure HDInsight üzerinde çalışan Spark işlerinde hata ayıklama](apache-spark-job-debugging.md)
* [HDInsight üzerinde Spark kümesi kaynaklarını yönetme](apache-spark-resource-manager.md)
* [Uzak bir Spark kümesine göndermek için Spark REST API kullanma](apache-spark-livy-rest-interface.md)
* [Spark'ı ayarlama](https://spark.apache.org/docs/latest/tuning.html)
* [Aslında, Spark'ı ayarlamak için bunu işleri nasıl çalışır](https://www.slideshare.net/ilganeli/how-to-actually-tune-your-spark-jobs-so-they-work)
* [Kryo seri hale getirme](https://github.com/EsotericSoftware/kryo)
