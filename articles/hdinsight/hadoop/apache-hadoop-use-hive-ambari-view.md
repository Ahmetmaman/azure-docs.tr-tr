---
title: Hive (Apache Hadoop) - Azure HDInsight üzerinde çalışmak için Apache Ambari görünümlerini kullanma
description: Hive sorguları göndermek için web tarayıcınızdan Hive görünümü kullanmayı öğrenin. Hive görünümü Ambari Web kullanıcı Arabirimi ile Linux tabanlı HDInsight kümenizi sağlanan bir parçasıdır.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/16/2018
ms.author: hrasheed
ms.openlocfilehash: cb68e93553be66d0d0be0edf61e491217bfe4d48
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2019
ms.locfileid: "58091316"
---
# <a name="use-apache-ambari-hive-view-with-apache-hadoop-in-hdinsight"></a>HDInsight, Apache Hadoop ile Apache Ambari Hive görünümünü kullanırsınız.

[!INCLUDE [hive-selector](../../../includes/hdinsight-selector-use-hive.md)]

Apache Ambari Hive görünümünü kullanarak Hive sorguları çalıştırmayı öğrenin. Hive görünümü yazmak için en iyi duruma getirmek ve web tarayıcınızdan Hive sorguları çalıştırmak sağlar.

## <a name="prerequisites"></a>Önkoşullar

* Bir Linux tabanlı Apache Hadoop üzerine HDInsight kümesi sürüm 3.4.

  > [!IMPORTANT]  
  > Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

* Bir web tarayıcısı

## <a name="run-a-hive-query"></a>Hive sorgusu çalıştırma

1. [Azure portalı](https://portal.azure.com) açın.

2. HDInsight kümenizi seçin ve ardından **Ambari görünümleri** gelen **hızlı bağlantılar** bölümü.

    ![Portalın Hızlı Bağlantılar bölümüne](./media/apache-hadoop-use-hive-ambari-view/quicklinks.png)

    Kimlik doğrulaması için istendiğinde, küme oturum açma bilgilerini kullanacak (varsayılan `admin`) hesap adı ve kümeyi oluştururken belirttiğiniz parola.

3. Görünümler listesinden seçin __Hive görünümü__.

    ![Seçilen Hive görünümü](./media/apache-hadoop-use-hive-ambari-view/select-hive-view.png)

    Yığın Görünümü sayfasında, aşağıdaki görüntüye benzer:

    ![Hive görünümü sorgu çalışma sayfasının görüntüsü](./media/apache-hadoop-use-hive-ambari-view/ambari-hive-view.png)

4. Gelen __sorgu__ sekmesinde, aşağıdaki HiveQL ifadelerini çalışma sayfasına yapıştırın:

    ```hiveql
    DROP TABLE log4jLogs;
    CREATE EXTERNAL TABLE log4jLogs(
        t1 string,
        t2 string,
        t3 string,
        t4 string,
        t5 string,
        t6 string,
        t7 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
    STORED AS TEXTFILE LOCATION '/example/data/';
    SELECT t4 AS loglevel, COUNT(*) AS count FROM log4jLogs 
        WHERE t4 = '[ERROR]' 
        GROUP BY t4;
    ```

    Bu deyimler, aşağıdaki eylemleri gerçekleştirin:

   * `DROP TABLE`: Tablo zaten mevcut durumda tablo ve veri dosyalarını siler.

   * `CREATE EXTERNAL TABLE`: Yeni bir "dış" Tablo kovanında oluşturur.
     Dış tablolar yalnızca tablo tanımı kovanında depolayın. Verileri özgün konumunda bırakılır.

   * `ROW FORMAT`: Verilerin nasıl biçimlendirildiğini gösterir. Bu durumda, her günlük alanlar boşlukla ayrılır.

   * `STORED AS TEXTFILE LOCATION`: Verilerin depolandığı ve metin olarak depolandığını gösterir.

   * `SELECT`: Burada ' % s'değeri [Hata] sütunu t4 içeren tüm satırların sayımını seçer.

     > [!IMPORTANT]  
     > Bırakın __veritabanı__ seçimde __varsayılan__. Bu belgedeki örneklerde, HDInsight ile dahil varsayılan veritabanı kullanın.

5. Sorgu başlatmak için **yürütme** düğmesinin altında çalışma sayfası. Düğme turuncu olur ve metin değişikliklerini **Durdur**.

6. Sorgu tamamlandıktan sonra **sonuçları** sekmesinde işlemin sonuçları görüntülenir. Sorgu sonucu aşağıda gösterilmiştir:

        loglevel       count
        [ERROR]        3

    Kullanabileceğiniz **günlükleri** iş oluşturulan günlük bilgilerini görüntülemek için sekmesinde.

   > [!TIP]  
   > İndirme veya sonuçlarını kaydetmek **sonuçlarını kaydetmek** üst kısımdaki açılan iletişim kutusunda sol üst **sorgu işleminin sonuçları** bölümü.

### <a name="visual-explain"></a>Visual açıklayın

Bir sorgu planı görselleştirmesini görüntülemek için seçin **Visual açıklayan** sekmesi altında çalışma sayfası.

**Visual açıklayan** sorgu görünümü karmaşık sorgular akışını anlamak yararlı olabilir. Bu görünüm bir metinsel eşdeğerini kullanarak görebilirsiniz **açıklama** sorgu Düzenleyicisi'nde düğmesi.

### <a name="tez-ui"></a>Tez UI

Tez kullanıcı Arabirimi için sorguyu görüntülemek için seçin **Tez** sekmesi altında çalışma sayfası.

> [!IMPORTANT]  
> Tez tüm sorguları çözümlemek için kullanılmaz. Çok sayıda sorgu Tez kullanmadan çözebilirsiniz. 

Tez sorgusunu çözümlemek için kullanıldıysa, yönlendirilmiş Çevrimsiz graf (DAG) görüntülenir. Geçmişte çalıştırma sorgular için DAG görüntülemek istiyorsanız veya Tez işlemde hata ayıklamak isterseniz kullanın [Tez görünümü](../hdinsight-debug-ambari-tez-view.md) yerine.

## <a name="view-job-history"></a>İş geçmişini görüntüleme

__İşleri__ sekmesi, Hive sorguları geçmişini görüntüler.

![İş geçmişini görüntüsü](./media/apache-hadoop-use-hive-ambari-view/job-history.png)

## <a name="database-tables"></a>Veritabanı tabloları

Kullanabileceğiniz __tabloları__ Hive veritabanı tabloları ile çalışmak için sekmesinde.

![Tabloları sekmesinin resmi](./media/apache-hadoop-use-hive-ambari-view/tables.png)

## <a name="saved-queries"></a>Kaydedilmiş sorgular

Gelen **sorgu** sekmesinde sorguları isteğe bağlı olarak kaydedebilir. Bir sorgu kaydettikten sonra ondan kullanabilirsiniz __kaydedilmiş sorgular__ sekmesi.

![Kaydedilmiş Sorgular sekmesinin resmi](./media/apache-hadoop-use-hive-ambari-view/saved-queries.png)

> [!TIP]  
> Kaydedilmiş Sorgular varsayılan küme depolama alanında depolanır. Kaydedilmiş Sorgular yolunda bulabilirsiniz `/user/<username>/hive/scripts`. Bunlar düz metin depolanır `.hql` dosyaları.
>
> Kümeyi silmek, ancak depolama tutmak gibi bir yardımcı program kullanabilirsiniz [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/) veya Data Lake Depolama Gezgini (gelen [Azure portalı](https://portal.azure.com)) sorguları alınamıyor.

## <a name="user-defined-functions"></a>Kullanıcı tanımlı işlevler

Kullanıcı tanımlı işlevler (UDF) Hive genişletebilirsiniz. Bir UDF HiveQL işlevselliği veya kolayca modellenmiş değil mantığı uygulamak için kullanın.

Bildirme ve UDF'ler kümesini kullanarak kaydetme **UDF** Hive görünümü üst kısmındaki sekme. Bu UDF'ler kullanılabilir **sorgu Düzenleyicisi**.

![UDF sekmesinin resmi](./media/apache-hadoop-use-hive-ambari-view/user-defined-functions.png)

Hive görünümü için bir UDF ekledikten sonra bir **Ekle UDF'ler** düğmesinin alt kısmında **sorgu Düzenleyicisi**. Bu girişi seçerek Hive Görünümü'nde tanımlanan UDF'ler açılan listesini görüntüler. Bir UDF seçerek sorgunuza UDF etkinleştirmek için HiveQL ifadelerini ekler.

Örneğin, aşağıdaki özelliklere sahip bir UDF tanımladıysanız:

* Kaynak adı: myudfs

* Resource path: /myudfs.jar

* UDF adı: myawesomeudf

* UDF sınıf adı: com.myudfs.Awesome

Kullanarak **Ekle UDF'ler** düğmesi adlı bir giriş görüntüler **myudfs**, bu kaynak için tanımlanan her UDF için başka bir açılan liste. Bu durumda **myawesomeudf**. Bu girişi seçerek aşağıdaki sorgu başına ekler:

```hiveql
add jar /myudfs.jar;
create temporary function myawesomeudf as 'com.myudfs.Awesome';
```

Bu gibi durumlarda, UDF ardından sorgunuza kullanabilirsiniz. Örneğin, `SELECT myawesomeudf(name) FROM people;`.

HDInsight üzerindeki Hive'a UDF'ler kullanarak daha fazla bilgi için aşağıdaki makalelere bakın:

* [Apache Hive ve HDInsight, Apache Pig ile Python kullanma](python-udf-hdinsight.md)
* [HDInsight için özel bir Apache Hive UDF ekleme](https://blogs.msdn.com/b/bigdatasupport/archive/2014/01/14/how-to-add-custom-hive-udfs-to-hdinsight.aspx)

## <a name="hive-settings"></a>Hive ayarları

Yürütme altyapısı (varsayılan) Tez kovana MapReduce değiştirme gibi çeşitli Hive ayarları değiştirebilirsiniz.

## <a id="nextsteps"></a>Sonraki adımlar

HDInsight üzerindeki Hive'a hakkında genel bilgi için:

* [HDInsight üzerinde Apache Hadoop ile Apache Hive'ı kullanma](hdinsight-use-hive.md)

Diğer yollar hakkında daha fazla bilgi için HDInsight üzerinde Hadoop ile çalışabilirsiniz:

* [HDInsight üzerinde Apache Hadoop ile Apache Pig kullanma](hdinsight-use-pig.md)
* [HDInsight üzerinde Apache Hadoop ile MapReduce kullanma](hdinsight-use-mapreduce.md)
