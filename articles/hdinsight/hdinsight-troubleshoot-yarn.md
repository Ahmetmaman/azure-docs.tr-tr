---
title: Azure HDInsight, YARN sorunlarını giderme
description: Azure HDInsight ve Apache Hadoop YARN ile çalışma hakkında sık sorulan soruların yanıtlarını alın.
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.topic: conceptual
ms.date: 12/06/2018
ms.openlocfilehash: d5582038c35ba3b599be89b7b7939e644d55ea78
ms.sourcegitcommit: c37122644eab1cc739d735077cf971edb6d428fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2018
ms.locfileid: "53408839"
---
# <a name="troubleshoot-apache-hadoop-yarn-by-using-azure-hdinsight"></a>Azure HDInsight'ı kullanarak Apache Hadoop YARN sorunlarını giderme

Apache Ambari, Apache Hadoop YARN yükü ile çalışırken sık karşılaşılan sorunlar ve çözümleri hakkında bilgi edinin.

## <a name="how-do-i-create-a-new-yarn-queue-on-a-cluster"></a>Bir kümede yeni YARN kuyruk nasıl oluşturabilirim?

### <a name="resolution-steps"></a>Çözüm adımları 

Yeni YARN kuyruk oluşturmak için Ambari aşağıdaki adımları kullanın ve ardından kapasite ayırma tüm kuyrukları arasında dengeleme. 

Bu örnekte, iki mevcut kuyrukları (**varsayılan** ve **thriftsvr**) hem de % 50 kapasiteden yeni kuyruğu (spark) % 50 kapasitesini sunan % 25 kapasiteye değiştirilir.
| Kuyruk | Kapasite | Maksimum kapasite |
| --- | --- | --- | --- |
| default | %25 | 50% |
| thrftsvr | %25 | 50% |
| Spark | 50% | 50% |

1. Seçin **Ambari görünümleri** simgesi ve Kılavuz düzeni seçin. Ardından, **YARN Kuyruk yöneticisi**.

    ![Ambari görünümlerini simgesini seçin](media/hdinsight-troubleshoot-yarn/create-queue-1.png)
2. Seçin **varsayılan** kuyruk.

    ![Varsayılan kuyruk seçin](media/hdinsight-troubleshoot-yarn/create-queue-2.png)
3. İçin **varsayılan** kuyruk, değiştirme **kapasite** 50 %'için % 25. İçin **thriftsvr** kuyruk, değiştirme **kapasite** % 25.

    ![Varsayılan ve thriftsvr kuyruklar için % 25'ine kapasitesini değiştirme](media/hdinsight-troubleshoot-yarn/create-queue-3.png)
4. Yeni bir kuyruk oluşturmak için Seç **kuyruk Ekle**.

    ![Kuyruk Ekle'yi seçin](media/hdinsight-troubleshoot-yarn/create-queue-4.png)

5. Yeni kuyruk adı.

    ![Spark kuyruk adı](media/hdinsight-troubleshoot-yarn/create-queue-5.png)  

6. Bırakın **kapasite** % 50 tıklayın ve ardından değerlerinde **eylemleri** düğmesi.

    ![Eylemler düğmeyi seçin](media/hdinsight-troubleshoot-yarn/create-queue-6.png)  
7. Seçin **kaydedin ve yenileyin kuyrukları**.

    ![Kaydet'i seçin ve kuyruklarını Yenile](media/hdinsight-troubleshoot-yarn/create-queue-7.png)  

Bu değişiklikler hemen YARN Zamanlayıcı UI görülebilir.

### <a name="additional-reading"></a>Ek okuma

- [Apache Hadoop YARN CapacityScheduler](https://hadoop.apache.org/docs/r2.7.2/hadoop-yarn/hadoop-yarn-site/CapacityScheduler.html)


## <a name="how-do-i-download-yarn-logs-from-a-cluster"></a>Bir kümeden YARN günlüklerini nasıl indiririm?


### <a name="resolution-steps"></a>Çözüm adımları 

1. Güvenli Kabuk (SSH) istemcisi kullanarak HDInsight kümesine bağlanın. Daha fazla bilgi için [ek okuma](#additional-reading-2).

2. Şu anda çalışan YARN uygulamaları tüm uygulama kimliklerini listelemek için aşağıdaki komutu çalıştırın:

    ```apache
    yarn top
    ```
    Kimlikleri listelenen **APPLİCATİONID** sütun. Günlükleri indirebilirsiniz **APPLİCATİONID** sütun.

    ```apache
    YARN top - 18:00:07, up 19d, 0:14, 0 active users, queue(s): root
    NodeManager(s): 4 total, 4 active, 0 unhealthy, 0 decommissioned, 0 lost, 0 rebooted
    Queue(s) Applications: 2 running, 10 submitted, 0 pending, 8 completed, 0 killed, 0 failed
    Queue(s) Mem(GB): 97 available, 3 allocated, 0 pending, 0 reserved
    Queue(s) VCores: 58 available, 2 allocated, 0 pending, 0 reserved
    Queue(s) Containers: 2 allocated, 0 pending, 0 reserved

                      APPLICATIONID USER             TYPE      QUEUE   #CONT  #RCONT  VCORES RVCORES     MEM    RMEM  VCORESECS    MEMSECS %PROGR       TIME NAME
     application_1490377567345_0007 hive            spark  thriftsvr       1       0       1       0      1G      0G    1628407    2442611  10.00   18:20:20 Thrift JDBC/ODBC Server
     application_1490377567345_0006 hive            spark  thriftsvr       1       0       1       0      1G      0G    1628430    2442645  10.00   18:20:20 Thrift JDBC/ODBC Server
    ```

3. YARN kapsayıcı günlükleri indirmek için tüm uygulama yöneticileri için aşağıdaki komutu kullanın:
   
    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> -am ALL > amlogs.txt
    ```

    Bu komut amlogs.txt adlı bir günlük dosyası oluşturur. 

4. YARN kapsayıcı günlükleri yalnızca en son uygulama şablonu indirmek için aşağıdaki komutu kullanın:

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> -am -1 > latestamlogs.txt
    ```

    Bu komut latestamlogs.txt adlı bir günlük dosyası oluşturur. 

4. İlk iki uygulama yöneticileri için YARN kapsayıcı günlükleri indirmek için aşağıdaki komutu kullanın:

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> -am 1,2 > first2amlogs.txt 
    ```

    Bu komut first2amlogs.txt adlı bir günlük dosyası oluşturur. 

5. Tüm YARN kapsayıcı günlükleri indirmek için aşağıdaki komutu kullanın:

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> > logs.txt
    ```

    Bu komut logs.txt adlı bir günlük dosyası oluşturur. 

6. YARN kapsayıcı günlüğü için belirli bir kapsayıcıya yüklemek için aşağıdaki komutu kullanın:

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> -containerId <container_id> > containerlogs.txt 
    ```

    Bu komut containerlogs.txt adlı bir günlük dosyası oluşturur.

### <a name="additional-reading-2"></a>Ek okuma

- [SSH kullanarak HDInsight için (Apache Hadoop) bağlanma](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix)
- [Apache Hadoop YARN kavramları ve uygulamaları](https://hortonworks.com/blog/apache-hadoop-yarn-concepts-and-applications/)


### <a name="see-also"></a>Ayrıca Bkz.
[Azure HDInsight'ı kullanarak sorun giderme](hdinsight-troubleshoot-guide.md)
