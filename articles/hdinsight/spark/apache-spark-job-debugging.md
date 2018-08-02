---
title: Azure HDInsight üzerinde çalışan Apache Spark işlerinde hata ayıklama | Microsoft Docs
description: Spark geçmiş sunucusu YARN UI ve Spark UI izlemek ve Azure HDInsight Spark kümesinde çalışan işleri hata ayıklamak için kullanın
services: hdinsight
documentationcenter: ''
author: mumian
manager: cgronlun
editor: cgronlun
tags: azure-portal
ms.assetid: 59af05a7-2bd9-44b0-b55f-2438d294198b
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 12/20/2017
ms.author: jgao
ms.openlocfilehash: 2593a9e782298bbd6e40bde611a430844febbce3
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2018
ms.locfileid: "39398497"
---
# <a name="debug-apache-spark-jobs-running-on-azure-hdinsight"></a>Azure HDInsight üzerinde çalışan Apache Spark işlerinde hata ayıklama

Bu makalede, izlemek ve Spark geçmiş sunucusu YARN UI ve Spark UI kullanarak HDInsight kümelerinde çalışan Spark işlerinde hata ayıklama hakkında bilgi edinin. Mevcut bir Not Defteri kullanarak Spark kümesi ile bir Spark işi başlatmak **Machine learning: Gıda denetimi verileri MLLib kullanarak Tahmine dayalı analiz**. Örneğin, gönderilen tüm diğer yaklaşımı de kullanarak bir uygulama izlemek için aşağıdaki adımları kullanabilirsiniz **spark-submit**.

## <a name="prerequisites"></a>Önkoşullar
Aşağıdakilere sahip olmanız gerekir:

* Azure aboneliği. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* HDInsight üzerinde bir Apache Spark kümesi. Yönergeler için bkz. [Azure HDInsight'ta Apache Spark kümeleri oluşturma](apache-spark-jupyter-spark-sql.md).
* Not Defteri çalıştırma başlaması gereken  **[Machine learning: Gıda denetimi verileri MLLib kullanarak Tahmine dayalı analiz](apache-spark-machine-learning-mllib-ipython.md)**. Bu not defteri çalıştırmak yönergeler için bağlantıyı izleyin.  

## <a name="track-an-application-in-the-yarn-ui"></a>YARN kullanıcı arabiriminde bir uygulama izleme
1. YARN UI başlatın. Tıklayın **küme Panosu**ve ardından **YARN**.
   
    ![YARN kullanıcı arabirimini Başlat](./media/apache-spark-job-debugging/launch-yarn-ui.png)
   
   > [!TIP]
   > Alternatif olarak, Ambari UI YARN kullanıcı Arabiriminden da başlatabilirsiniz. Ambari UI başlatmak için tıklayın **küme Panosu**ve ardından **HDInsight küme Panosu**. Ambari Arabiriminden tıklayın **YARN**, tıklayın **hızlı bağlantılar**etkin Kaynak Yöneticisi'ni tıklatın ve ardından **kaynak yöneticisi kullanıcı Arabirimi**.    
   > 
   > 
2. Jupyter not defterlerini kullanarak Spark işi başlatıldığından, uygulama adına sahip **remotesparkmagics** (Not defterlerinden başlatılan tüm uygulamalara ilişkin ad budur). İşle ilgili daha fazla bilgi almak için uygulama kimliği uygulama adı karşı tıklayın. Bu, uygulama görünümü çalıştırır.
   
    ![Spark uygulaması kimliği bulunamıyor](./media/apache-spark-job-debugging/find-application-id.png)
   
    Jupyter Not defterlerinden başlatılan, böyle uygulamalar için her zaman durumudur **çalıştıran** not defterini çıkana kadar.
3. Uygulama görünümünden uygulama ve günlükleri (stdout/stderr) ile ilişkili kapsayıcılar bulma daha fazla sınırlandıramazsınız detaya gidebilirsiniz. Bağlama karşılık gelen'ı tıklatarak da Spark kullanıcı arabirimini başlatabilirsiniz **izleme URL**, aşağıda gösterildiği gibi. 
   
    ![Kapsayıcı günlüklerini indir](./media/apache-spark-job-debugging/download-container-logs.png)

## <a name="track-an-application-in-the-spark-ui"></a>Uygulamanın kullanıcı arabiriminde Spark izleme
Spark kullanıcı Arabiriminde daha önce başlatıldı uygulama tarafından üretilen Spark işlere detaya gidebilirsiniz.

1. Spark Arabiriminden uygulama görünümü başlatmak için karşı bağlantısı **izleme URL**yukarıdaki ekran görüntüsünde gösterildiği gibi. Jupyter not defteri çalışan uygulama tarafından başlatılan tüm Spark işleri görebilirsiniz.
   
    ![Spark işleri görüntüle](./media/apache-spark-job-debugging/view-spark-jobs.png)
2. Tıklayın **yürütücüler** her Yürütücü işleme ve Depolama bilgileri görmek için sekmesinde. Tıklayarak çağrı yığınını alabilirsiniz **iş parçacığı dökümü** bağlantı.
   
    ![Spark Yürütücü görüntüleyin](./media/apache-spark-job-debugging/view-spark-executors.png)
3. Tıklayın **aşamaları** uygulamayla ilişkili aşamaları görmek için sekmesinde.
   
    ![Spark aşamaları görünümünü](./media/apache-spark-job-debugging/view-spark-stages.png)
   
    Her aşama için görüntüleyebilirsiniz yürütme istatistikleri gibi birden çok görevler çalıştırılabileceğini aşağıda gösterilmiştir.
   
    ![Spark aşamaları görünümünü](./media/apache-spark-job-debugging/view-spark-stages-details.png) 
4. Aşama Ayrıntılar sayfasından DAG görselleştirme başlatabilirsiniz. Genişletin **DAG görselleştirme** aşağıda gösterildiği gibi sayfanın en üstündeki bağlayın.
   
    ![Spark aşamaları DAG görselleştirmeyi görüntüleyin](./media/apache-spark-job-debugging/view-spark-stages-dag-visualization.png)
   
    DAG ya da doğrudan Aclyic grafik uygulama farklı aşamaları temsil eder. Graftaki her mavi kutu uygulamadan çağrılan bir Spark işlem temsil eder.
5. Aşama Ayrıntılar sayfasından uygulama zaman çizelgesi görünümü da başlatabilirsiniz. Genişletin **olay zaman çizelgesi** aşağıda gösterildiği gibi sayfanın en üstündeki bağlayın.
   
    ![Spark aşamaları olay zaman çizelgesini görüntüleyin](./media/apache-spark-job-debugging/view-spark-stages-event-timeline.png)
   
    Bu, bir zaman çizelgesi biçiminde Spark olayları görüntüler. Zaman Çizelgesi Görünümü üç düzeyde işindeki ve aşama içinde işleri arasında kullanılabilir. Yukarıdaki resimde, belirli bir aşama için zaman çizelgesi görünümü yakalar.
   
   > [!TIP]
   > Seçerseniz **etkinleştirmek** onay kutusu kaydırma sola ve sağa arasında zaman çizelgesi görünümü.
   > 
   > 
6. Spark Arabirimindeki diğer sekmelere de Spark örneğinde hakkında yararlı bilgiler sağlar.
   
   * Uygulamanızı bir Rdd oluşturursa, depolama sekmesi - olanlar depolama sekmesi hakkında bilgi bulabilirsiniz.
   * Ortam sekmesi - Bu sekme, bir Spark örneği hakkında yararlı bilgiler çok gibi sağlar 
     * Scala sürümü
     * Kümeyle ilişkili olay günlüğü dizini
     * Uygulama için Yürütücü çekirdek sayısı
     * Etc.

## <a name="find-information-about-completed-jobs-using-the-spark-history-server"></a>Spark geçmiş sunucusu kullanarak tamamlanan işler hakkında bilgi
Bir iş tamamlandığında, işle ilgili bilgiler Spark geçmiş sunucusu kalıcı hale getirilir.

1. Spark geçmiş sunucusu, küme dikey penceresinden başlatmak için tıklayın **küme Panosu**ve ardından **Spark geçmiş sunucusu**.
   
    ![Spark geçmiş sunucusu başlatma](./media/apache-spark-job-debugging/launch-spark-history-server.png)
   
   > [!TIP]
   > Alternatif olarak, Ambari UI Spark geçmiş sunucusu Arabiriminden da başlatabilirsiniz. Ambari UI, küme dikey penceresinden başlatmak için tıklayın **küme Panosu**ve ardından **HDInsight küme Panosu**. Ambari Arabiriminden tıklayın **Spark**, tıklayın **hızlı bağlantılar**ve ardından **Spark geçmiş sunucusu kullanıcı Arabiriminin**.
   > 
   > 
2. Listelenen tüm tamamlanan uygulamalar görürsünüz. Bir uygulama kimliği, bir uygulamaya daha fazla bilgi için detaya gitmek için tıklayın.
   
    ![Spark geçmiş sunucusu başlatma](./media/apache-spark-job-debugging/view-completed-applications.png)

## <a name="see-also"></a>Ayrıca bkz.
*  [Azure HDInsight’ta Apache Spark kümesi kaynaklarını yönetme](apache-spark-resource-manager.md)
*  [Genişletilmiş Spark geçmiş sunucusu kullanarak Spark işlerinde hata ayıklama](apache-azure-spark-history-server.md)

### <a name="for-data-analysts"></a>Veri analistleri için

* [Machine Learning ile Spark: HVAC verilerini kullanarak bina sıcaklığını çözümlemek için HDInsight’ta Spark kullanma](apache-spark-ipython-notebook-machine-learning.md)
* [Machine Learning ile Spark: Yemek inceleme sonuçlarını tahmin etmek için HDInsight’ta Spark kullanma](apache-spark-machine-learning-mllib-ipython.md)
* [HDInsight’ta Spark kullanarak Web sitesi günlüğü çözümlemesi](apache-spark-custom-library-website-log-analysis.md)
* [HDInsight’ta Spark kullanarak Application Insight telemetri verilerinin analizi](apache-spark-analyze-application-insight-logs.md)
* [Azure HDInsight Spark üzerinde dağıtılmış derin öğrenme için Caffe kullanma](apache-spark-deep-learning-caffe.md)

### <a name="for-spark-developers"></a>Spark geliştiricileri için

* [Scala kullanarak tek başına uygulama oluşturma](apache-spark-create-standalone-application.md)
* [Livy kullanarak Spark kümesinde işleri uzaktan çalıştırma](apache-spark-livy-rest-interface.md)
* [Spark Scala uygulamaları oluşturmak ve göndermek amacıyla IntelliJ IDEA için HDInsight Araçları Eklentisini kullanma](apache-spark-intellij-tool-plugin.md)
* [Spark uygulamalarında uzaktan hata ayıklamak amacıyla IntelliJ IDEA için HDInsight Araçları Eklentisi kullanma](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [HDInsight’ta Spark kümesi ile Zeppelin not defterlerini kullanma](apache-spark-zeppelin-notebook.md)
* [HDInsight için Spark kümesinde Jupyter not defteri için kullanılabilir çekirdekler](apache-spark-jupyter-notebook-kernels.md)
* [Jupyter not defterleri ile dış paketleri kullanma](apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter’i bilgisayarınıza yükleme ve bir HDInsight Spark kümesine bağlanma](apache-spark-jupyter-notebook-install-locally.md)


