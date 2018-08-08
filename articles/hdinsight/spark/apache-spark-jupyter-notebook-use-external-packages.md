---
title: Jupyter, Spark on Azure HDInsight ile özel Maven paketleri kullanma
description: Özel Maven paketlerini kullanmak için adım adım yönergeler HDInsight Spark kümeleri ile Jupyter not defterleri kullanılabilir yapılandırma.
services: hdinsight
author: jasonwhowell
editor: jasonwhowell
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/09/2018
ms.author: jasonh
ms.openlocfilehash: 51099f64546acc6f18269b2e7ec05106bb3baa2d
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2018
ms.locfileid: "39622040"
---
# <a name="use-external-packages-with-jupyter-notebooks-in-apache-spark-clusters-on-hdinsight"></a>HDInsight üzerinde Apache Spark kümeleri Jupyter not defterlerinde ile dış paketleri kullanma
> [!div class="op_single_selector"]
> * [Hücre Sihri kullanarak](apache-spark-jupyter-notebook-use-external-packages.md)
> * [Betik eylemi kullanarak](apache-spark-python-package-installation.md)
>
>

Harici, topluluk tarafından katkıda bulunulan kullanmak için HDInsight üzerinde Apache Spark kümesinde Jupyter not defteri yapılandırma konusunda bilgi **maven** kümede olmayan paketleri dahil kullanıma hazır. 

Arama yapabilirsiniz [Maven deposu](http://search.maven.org/) kullanılabilir paketler tam listesi için. Ayrıca, diğer kaynaklardan kullanılabilir paketler listesini alabilirsiniz. Örneğin, topluluk tarafından katkıda bulunulan paketlerin tam bir listesi kullanılabilir [Spark paketleri](http://spark-packages.org/).

Bu makalede, nasıl kullanılacağını öğreneceksiniz [spark csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) Jupyter not defteri ile paket.

## <a name="prerequisites"></a>Önkoşullar
Aşağıdakilere sahip olmanız gerekir:

* HDInsight üzerinde bir Apache Spark kümesi. Yönergeler için bkz. [Azure HDInsight'ta Apache Spark kümeleri oluşturma](apache-spark-jupyter-spark-sql.md).

## <a name="use-external-packages-with-jupyter-notebooks"></a>Jupyter not defterleri ile dış paketleri kullanma
1. [Azure Portal](https://portal.azure.com/)’daki başlangıç panosunda Spark kümenizin kutucuğuna tıklayın (başlangıç panosuna sabitlediyseniz). Ayrıca **Tüm** > **HDInsight Kümelerine Gözat** altından kümenize gidebilirsiniz.   

1. Spark kümesi dikey penceresinden **Hızlı Bağlantılar**’a ve sonra **Küme Panosu** dikey penceresinden **Jupyter Not Defteri**’ne tıklayın. İstenirse, küme için yönetici kimlik bilgilerini girin.

    > [!NOTE]
    > Aşağıdaki URL’yi tarayıcınızda açarak da Jupyter Notebook’a ulaşabilirsiniz. **CLUSTERNAME** değerini kümenizin adıyla değiştirin:
    > 
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
    > 

1. Yeni bir not defteri oluşturun. Tıklayın **yeni**ve ardından **Spark**.
   
    ![Yeni bir Jupyter not defteri oluşturma](./media/apache-spark-jupyter-notebook-use-external-packages/hdinsight-spark-create-notebook.png "Yeni bir Jupyter not defteri oluşturma")

1. Yeni bir not defteri oluşturulur ve Untitled.pynb adı ile açılır. Üstteki not defteri adına tıklayın ve kolay bir ad girin.
   
    ![Not defteri adını belirtme](./media/apache-spark-jupyter-notebook-use-external-packages/hdinsight-spark-name-notebook.png "Not defteri adını belirtme")

1. Kullanacağınız `%%configure` Sihirli bir dış paketini kullanmak üzere not defterini yapılandırmak için. Dış paketleri kullanma defterlerinde çağırmanızı emin `%%configure` ilk kodu hücreyi Sihirli. Bu, çekirdek oturumu başlamadan önce paketini kullanacak şekilde yapılandırıldığını sağlar.

    >[!IMPORTANT] 
    >Çekirdek ilk hücreye yapılandırmak, parantezi unutsanız bile kullanabilirsiniz `%%configure` ile `-f` parametresi, ancak oturumu yeniden başlatılacak ve tüm ilerleme kaybedilecek.

    | HDInsight sürümü | Komut |
    |-------------------|---------|
    |HDInsight 3.3 ve 3.4 HDInsight | `%%configure` <br>`{ "packages":["com.databricks:spark-csv_2.10:1.4.0"] }`|
    | HDInsight 3.5 ve HDInsight 3.6 | `%%configure`<br>`{ "conf": {"spark.jars.packages": "com.databricks:spark-csv_2.10:1.4.0" }}`|

1. Yukarıdaki kod parçacığında, Maven Central Repository dış paketi için maven koordinatları bekliyor. Bu kod parçacığı içinde `com.databricks:spark-csv_2.10:1.4.0` için maven koordinatı **spark csv** paket. Paket koordinatları oluşturmak nasıl aşağıda verilmiştir.
   
    a. Paket Maven depoda bulun. Bu öğretici için kullandığımız [spark csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar).
   
    b. Depodan değerlerini toplamak **GroupID**, **Artifactıd**, ve **sürüm**. Değerlerin Topladığınızdan kümenizi eşleştiğinden emin olun. Bu durumda, 2.10 Scala ve Spark 1.4.0 paket kullanıyoruz, ancak farklı sürümleri için uygun Scala veya Spark sürümü kümenizde seçmeniz gerekebilir. Kümenizde Scala sürümü çalıştırarak bulabilirsiniz `scala.util.Properties.versionString` Spark Jupyter çekirdek veya Spark Gönder. Kümenizde Spark sürümü çalıştırarak bulabilirsiniz `sc.version` Jupyter Not.
   
    ![Jupyter not defteri ile dış paketleri kullanma](./media/apache-spark-jupyter-notebook-use-external-packages/use-external-packages-with-jupyter.png "Jupyter not defteri ile dış paketleri kullanma")
   
    c. Virgül ile ayrılmış üç değerleri birleştirebilir (**:**).
   
        com.databricks:spark-csv_2.10:1.4.0

1. İle kod hücresini çalıştırmak `%%configure` Sihirli. Bu, sağladığınız paketini kullanmak için temel alınan Livy oturumu yapılandıracaksınız. Not defterini sonraki hücrelerde, aşağıda gösterildiği gibi paketin artık kullanabilirsiniz.
   
        val df = sqlContext.read.format("com.databricks.spark.csv").
        option("header", "true").
        option("inferSchema", "true").
        load("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

    HDInsight 3.6 için aşağıdaki kod parçacığı kullanmanız gerekir.

        val df = spark.read.format("com.databricks.spark.csv").
        option("header", "true").
        option("inferSchema", "true").
        load("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

1. Kod parçacıkları gibi ardından çalıştırın önceki adımda oluşturduğunuz veri verileri görüntülemek için aşağıda gösterilmiştir.
   
        df.show()
   
        df.select("Time").count()

## <a name="seealso"></a>Ayrıca bkz.
* [Genel Bakış: Azure HDInsight’ta Apache Spark](apache-spark-overview.md)

### <a name="scenarios"></a>Senaryolar
* [BI ile Spark: BI araçlarıyla HDInsight’ta Spark kullanarak etkileşimli veri çözümlemesi gerçekleştirme](apache-spark-use-bi-tools.md)
* [Machine Learning ile Spark: HVAC verilerini kullanarak bina sıcaklığını çözümlemek için HDInsight’ta Spark kullanma](apache-spark-ipython-notebook-machine-learning.md)
* [Machine Learning ile Spark: Yemek inceleme sonuçlarını tahmin etmek için HDInsight’ta Spark kullanma](apache-spark-machine-learning-mllib-ipython.md)
* [HDInsight’ta Spark kullanarak Web sitesi günlüğü çözümlemesi](apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Uygulamaları oluşturma ve çalıştırma
* [Scala kullanarak tek başına uygulama oluşturma](apache-spark-create-standalone-application.md)
* [Livy kullanarak Spark kümesinde işleri uzaktan çalıştırma](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Araçlar ve uzantılar

* [HDInsight Linux üzerinde Apache Spark kümeleri Jupyter not defterlerinde ile dış python paketlerini kullanma](apache-spark-python-package-installation.md)
* [Spark Scala uygulamaları oluşturmak ve göndermek amacıyla IntelliJ IDEA için HDInsight Araçları Eklentisini kullanma](apache-spark-intellij-tool-plugin.md)
* [Spark uygulamalarında uzaktan hata ayıklamak amacıyla IntelliJ IDEA için HDInsight Araçları Eklentisi kullanma](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [HDInsight’ta Spark kümesi ile Zeppelin not defterlerini kullanma](apache-spark-zeppelin-notebook.md)
* [HDInsight için Spark kümesinde Jupyter not defteri için kullanılabilir çekirdekler](apache-spark-jupyter-notebook-kernels.md)
* [Jupyter’i bilgisayarınıza yükleme ve bir HDInsight Spark kümesine bağlanma](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Kaynakları yönetme
* [Azure HDInsight’ta Apache Spark kümesi kaynaklarını yönetme](apache-spark-resource-manager.md)
* [HDInsight’ta bir Apache Spark kümesinde çalışan işleri izleme ve hata ayıklama](apache-spark-job-debugging.md)
