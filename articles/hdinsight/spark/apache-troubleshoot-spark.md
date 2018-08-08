---
title: Azure HDInsight Spark sorunlarını giderme
description: Apache Spark ve Azure HDInsight ile çalışma hakkında sık sorulan soruların yanıtlarını alın.
services: hdinsight
ms.service: hdinsight
author: jasonwhowell
ms.author: jasonh
ms.topic: conceptual
ms.date: 11/2/2017
ms.openlocfilehash: 7c7f89864d9394ff4527f9a0354b9276f7c01c49
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39591746"
---
# <a name="troubleshoot-spark-by-using-azure-hdinsight"></a>Azure HDInsight'ı kullanarak Spark sorunlarını giderme

Apache Ambari, Apache Spark yükü ile çalışırken sık karşılaşılan sorunlar ve çözümleri hakkında bilgi edinin.

## <a name="how-do-i-configure-a-spark-application-by-using-ambari-on-clusters"></a>Spark uygulaması kümelerinde Ambari kullanarak nasıl yapılandırabilirim?

### <a name="resolution-steps"></a>Çözüm adımları

Bu yordamı için yapılandırma değerlerini daha önce HDInsight ayarlandı. Hangi Spark belirlemek için hangi değerlere ayarlanması ve yapılandırmaları gerekir [ne bir Spark uygulaması OutofMemoryError özel durumu neden](#what-causes-a-spark-application-outofmemoryerror-exception). 

1. Kümeleri listesinde seçin **Spark2**.

    ![Küme listeden seçin.](./media/apache-troubleshoot-spark/update-config-1.png)

2. Seçin **yapılandırmaları** sekmesi.

    ![Yapılandırmaları sekmesini seçin](./media/apache-troubleshoot-spark/update-config-2.png)

3. Listeden yapılandırmaları seçin **özel spark2 varsayılanları**.

    ![Özel spark Varsayılanları seçin](./media/apache-troubleshoot-spark/update-config-3.png)

4. Gibi ayarlamak için gereken değer ayarı Ara **spark.executor.memory**. Bu durumda, değerini **4608m** çok yüksek.

    ![Spark.executor.memory alanı seçin](./media/apache-troubleshoot-spark/update-config-4.png)

5. Önerilen ayar için değer ayarlayın. Değer **2048m** için bu ayar önerilir.

    ![Değeri değiştirmek için 2048 m](./media/apache-troubleshoot-spark/update-config-5.png)

6. Değer kaydedin ve ardından yapılandırmayı kaydedin. Araç çubuğunda **Kaydet**.

    ![Ayar ve yapılandırmayı kaydedin](./media/apache-troubleshoot-spark/update-config-6a.png)

    Tüm yapılandırmaları dikkat etmeniz gerekiyorsa size bildirilir. Öğeleri not edin ve ardından **yine de devam**. 

    ![Select yine de devam](./media/apache-troubleshoot-spark/update-config-6b.png)

    Yapılandırma değişiklikleri hakkında bir not yazın ve ardından **Kaydet**.

    ![Yaptığınız değişiklikleri hakkında bir not girin](./media/apache-troubleshoot-spark/update-config-6c.png)

7. Bir yapılandırma kaydedildiği zaman hizmeti yeniden başlatmanız istenir. Seçin **yeniden**.

    ![Yeniden başlatma seçin](./media/apache-troubleshoot-spark/update-config-7a.png)

    Yeniden başlatma işlemini onaylayın.

    ![Tüm yeniden Onayla seçeneğini belirleyin](./media/apache-troubleshoot-spark/update-config-7b.png)

    Çalışan işlemler gözden geçirebilirsiniz.

    ![Çalışan işlemleri gözden geçirin](./media/apache-troubleshoot-spark/update-config-7c.png)

8. Yapılandırmaları ekleyebilirsiniz. Listeden yapılandırmaları seçin **özel spark2 varsayılanları**ve ardından **Özellik Ekle**.

    ![Özellik Ekle'yi seçin](./media/apache-troubleshoot-spark/update-config-8.png)

9. Yeni bir özellik tanımlayın. Veri türü gibi belirli ayarlar için bir iletişim kutusunu kullanarak, tek bir özellik tanımlayabilirsiniz. Veya, her satırda bir tanım'ı kullanarak birden çok özellik tanımlayabilirsiniz. 

    Bu örnekte, **spark.driver.memory** özellik değeriyle tanımlanan **4g**.

    ![Yeni bir özellik tanımlayın](./media/apache-troubleshoot-spark/update-config-9.png)

10. Yapılandırmayı kaydetmek ve 6 ve 7. adımda açıklandığı hizmeti yeniden başlatın.

Bu değişiklikler, küme çapında ancak Spark işi gönderdiğinizde geçersiz kılınabilir.

### <a name="additional-reading"></a>Ek okuma

[HDInsight kümelerinde Spark iş gönderme](https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/)


## <a name="how-do-i-configure-a-spark-application-by-using-a-jupyter-notebook-on-clusters"></a>Kümeleri Jupyter Not Defteri kullanarak Spark uygulaması nasıl yapılandırabilirim?

### <a name="resolution-steps"></a>Çözüm adımları

1. Hangi Spark belirlemek için hangi değerlere ayarlanması ve yapılandırmaları gerekir [ne bir Spark uygulaması OutofMemoryError özel durumu neden](#what-causes-a-spark-application-outofmemoryerror-exception).

2. Jupyter Not Defteri, ilk hücrenin sonra **%% yapılandırma** yönergesi, Spark yapılandırmaları geçerli JSON biçiminde belirtin. Gerçek değerleri gerektiği gibi değiştirin:

    ![Yapılandırma Ekle](./media/apache-troubleshoot-spark/add-configuration-cell.png)

### <a name="additional-reading"></a>Ek okuma

[HDInsight kümelerinde Spark iş gönderme](https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/)


## <a name="how-do-i-configure-a-spark-application-by-using-livy-on-clusters"></a>Kümelerinde Livy kullanarak Spark uygulaması nasıl yapılandırabilirim?

### <a name="resolution-steps"></a>Çözüm adımları

1. Hangi Spark belirlemek için hangi değerlere ayarlanması ve yapılandırmaları gerekir [ne bir Spark uygulaması OutofMemoryError özel durumu neden](#what-causes-a-spark-application-outofmemoryerror-exception). 

2. Spark uygulaması Livy için cURL gibi bir REST istemcisi kullanarak gönderin. Aşağıdakine benzer bir komut kullanın. Gerçek değerleri gerektiği gibi değiştirin:

    ```apache
    curl -k --user 'username:password' -v -H 'Content-Type: application/json' -X POST -d '{ "file":"wasb://container@storageaccountname.blob.core.windows.net/example/jars/sparkapplication.jar", "className":"com.microsoft.spark.application", "numExecutors":4, "executorMemory":"4g", "executorCores":2, "driverMemory":"8g", "driverCores":4}'  
    ```

### <a name="additional-reading"></a>Ek okuma

[HDInsight kümelerinde Spark iş gönderme](https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/)


## <a name="how-do-i-configure-a-spark-application-by-using-spark-submit-on-clusters"></a>Kullanarak uygulama spark-submit Spark kümelerinde nasıl yapılandırabilirim?

### <a name="resolution-steps"></a>Çözüm adımları

1. Hangi Spark belirlemek için hangi değerlere ayarlanması ve yapılandırmaları gerekir [ne bir Spark uygulaması OutofMemoryError özel durumu neden](#what-causes-a-spark-application-outofmemoryerror-exception).

2. Spark-shell, aşağıdakine benzer bir komut kullanarak başlatın. Gerçek değer yapılandırmalarının gerektiği gibi değiştirin: 

    ```apache
    spark-submit --master yarn-cluster --class com.microsoft.spark.application --num-executors 4 --executor-memory 4g --executor-cores 2 --driver-memory 8g --driver-cores 4 /home/user/spark/sparkapplication.jar
    ```

### <a name="additional-reading"></a>Ek okuma

[HDInsight kümelerinde Spark iş gönderme](https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/)


## <a name="what-causes-a-spark-application-outofmemoryerror-exception"></a>Spark uygulaması OutofMemoryError özel durumu nedeni nedir?

### <a name="detailed-description"></a>Ayrıntılı bir açıklaması

Spark uygulaması, aşağıdaki türde Yakalanmayan Özel durum ile başarısız olur:

```apache
ERROR Executor: Exception in task 7.0 in stage 6.0 (TID 439) 

java.lang.OutOfMemoryError 
    at java.io.ByteArrayOutputStream.hugeCapacity(Unknown Source) 
    at java.io.ByteArrayOutputStream.grow(Unknown Source) 
    at java.io.ByteArrayOutputStream.ensureCapacity(Unknown Source) 
    at java.io.ByteArrayOutputStream.write(Unknown Source) 
    at java.io.ObjectOutputStream$BlockDataOutputStream.drain(Unknown Source) 
    at java.io.ObjectOutputStream$BlockDataOutputStream.setBlockDataMode(Unknown Source) 
    at java.io.ObjectOutputStream.writeObject0(Unknown Source) 
    at java.io.ObjectOutputStream.writeObject(Unknown Source) 
    at org.apache.spark.serializer.JavaSerializationStream.writeObject(JavaSerializer.scala:44) 
    at org.apache.spark.serializer.JavaSerializerInstance.serialize(JavaSerializer.scala:101) 
    at org.apache.spark.executor.Executor$TaskRunner.run(Executor.scala:239) 
    at java.util.concurrent.ThreadPoolExecutor.runWorker(Unknown Source) 
    at java.util.concurrent.ThreadPoolExecutor$Worker.run(Unknown Source) 
    at java.lang.Thread.run(Unknown Source) 
```

```apache
ERROR SparkUncaughtExceptionHandler: Uncaught exception in thread Thread[Executor task launch worker-0,5,main] 

java.lang.OutOfMemoryError 
    at java.io.ByteArrayOutputStream.hugeCapacity(Unknown Source) 
    at java.io.ByteArrayOutputStream.grow(Unknown Source) 
    at java.io.ByteArrayOutputStream.ensureCapacity(Unknown Source) 
    at java.io.ByteArrayOutputStream.write(Unknown Source) 
    at java.io.ObjectOutputStream$BlockDataOutputStream.drain(Unknown Source) 
    at java.io.ObjectOutputStream$BlockDataOutputStream.setBlockDataMode(Unknown Source) 
    at java.io.ObjectOutputStream.writeObject0(Unknown Source) 
    at java.io.ObjectOutputStream.writeObject(Unknown Source) 
    at org.apache.spark.serializer.JavaSerializationStream.writeObject(JavaSerializer.scala:44) 
    at org.apache.spark.serializer.JavaSerializerInstance.serialize(JavaSerializer.scala:101) 
    at org.apache.spark.executor.Executor$TaskRunner.run(Executor.scala:239) 
    at java.util.concurrent.ThreadPoolExecutor.runWorker(Unknown Source) 
    at java.util.concurrent.ThreadPoolExecutor$Worker.run(Unknown Source) 
    at java.lang.Thread.run(Unknown Source) 
```

### <a name="probable-cause"></a>Olası neden

Bu özel durumun en olası nedeni, Java sanal makineleri (JVMs) yeterli yığın bellek tahsis edildiği ' dir. Bu JVMs Spark uygulaması kapsamında Yürütücü veya sürücü başlatılır. 

### <a name="resolution-steps"></a>Çözüm adımları

1. Spark veri en büyük boyutunu belirlemek uygulama işler. En fazla girdi verilerini, giriş verilerinin dönüştürülmesiyle üretilen Ara veriler ve Ara veriler uygulamanın daha fazla dönüştürürken üretilen çıktı verilerinin boyutuna bağlı olarak bir tahmin yapabilirsiniz. Bu işlem, bir ilk biçimsel tahmin kuramıyorsa bir yinelemeli olabilir. 

2. Kullanılacak gideceğinizi HDInsight küme bellek ve çekirdek Spark uygulamasını barındırmak için yeterli kaynağa sahip olduğundan emin olun. Bu küme ölçümleri bölümünü değerlerini YARN kullanıcı Arabiriminde görüntüleyerek belirlemek **kullanılan bellek** vs. **Bellek toplam**, ve **sanal çekirdekler kullanılan** vs. **Sanal çekirdekler toplam**.

3. Aşağıdaki Spark yapılandırmaları kullanılabilir bellek ve çekirdeklerin % 90'ı aşmamalıdır uygun değerlere ayarlayın. Değerler de Spark uygulamasının bellek gereksinimlerini olmalıdır: 

    ```apache
    spark.executor.instances (Example: 8 for 8 executor count) 
    spark.executor.memory (Example: 4g for 4 GB) 
    spark.yarn.executor.memoryOverhead (Example: 384m for 384 MB) 
    spark.executor.cores (Example: 2 for 2 cores per executor) 
    spark.driver.memory (Example: 8g for 8GB) 
    spark.driver.cores (Example: 4 for 4 cores)   
    spark.yarn.driver.memoryOverhead (Example: 384m for 384MB) 
    ```

    Hesaplama için tüm yürütücüleri tarafından kullanılan toplam bellek: 
    
    ```apache
    spark.executor.instances * (spark.executor.memory + spark.yarn.executor.memoryOverhead) 
    ```
   Hesaplama için sürücü tarafından kullanılan toplam bellek:
    
    ```apache
    spark.driver.memory + spark.yarn.driver.memoryOverhead
    ```

### <a name="additional-reading"></a>Ek okuma

- [Spark bellek yönetimine genel bakış](http://spark.apache.org/docs/latest/tuning.html#memory-management-overview)
- [Bir HDInsight kümesi üzerinde bir Spark uygulaması hata ayıklama](https://blogs.msdn.microsoft.com/azuredatalake/2016/12/19/spark-debugging-101/)


### <a name="see-also"></a>Ayrıca Bkz.
[Azure HDInsight'ı kullanarak sorun giderme](../../hdinsight/hdinsight-troubleshoot-guide.md)

