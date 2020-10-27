---
title: Azure HDInsight 'ta YARN sorunlarını giderme
description: Apache Hadoop YARN ve Azure HDInsight ile çalışma hakkında sık sorulan soruların yanıtlarını alın.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 08/15/2019
ms.openlocfilehash: 84224172dbfd63fee51b3a7b80f5990b04e5e228
ms.sourcegitcommit: d767156543e16e816fc8a0c3777f033d649ffd3c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2020
ms.locfileid: "92535034"
---
# <a name="troubleshoot-apache-hadoop-yarn-by-using-azure-hdinsight"></a>Azure HDInsight kullanarak Apache Hadoop YARN sorunlarını giderme

Apache ambarı 'nda Apache Hadoop YARN yükleriyle çalışırken en üstteki sorunlar ve çözümlerinin çözümleri hakkında bilgi edinin.

## <a name="how-do-i-create-a-new-yarn-queue-on-a-cluster"></a>Kümede yeni bir YARN kuyruğu oluşturmak Nasıl yaparım? mı?

### <a name="resolution-steps"></a>Çözüm adımları

Yeni bir YARN kuyruğu oluşturmak için aşağıdaki adımları kullanın ve ardından Tüm kuyruklar arasında kapasite ayırmayı dengeleyin.

Bu örnekte, iki mevcut kuyruk ( **varsayılan** ve **thriftsvr** ) her ikisi de %50 kapasitesinden %25 kapasiteye (Spark) %50 kapasiteye sahip olacak şekilde değiştirilmiştir.

| Kuyruk | Kapasite | Maksimum kapasite |
| --- | --- | --- |
| default | 25% | 50% |
| thrftsvr | 25% | 50% |
| spark | 50% | 50% |

1. **Ambarı görünümleri** simgesini seçin ve ardından Kılavuz stilini seçin. Sonra, **Yarn kuyruğu Yöneticisi** ' ni seçin.

    ![Apache ambarı panosu YARN Kuyruk Yöneticisi](media/hdinsight-troubleshoot-yarn/apache-yarn-create-queue-1.png)
2. **Varsayılan** kuyruğu seçin.

    ![Apache ambarı YARN varsayılan kuyruğu seçin](media/hdinsight-troubleshoot-yarn/apache-yarn-create-queue-2.png)
3. **Varsayılan** sıra için **kapasiteyi** %50 ' dan %25 ' e değiştirin. **Thriftsvr** kuyruğu için **kapasiteyi** %25 olarak değiştirin.

    ![Varsayılan ve thriftsvr kuyrukları için kapasiteyi %25 olarak değiştirin](media/hdinsight-troubleshoot-yarn/apache-yarn-create-queue-3.png)
4. Yeni bir kuyruk oluşturmak için **kuyruk Ekle** ' yi seçin.

    ![Apache ambarı YARN Pano ekleme kuyruğu](media/hdinsight-troubleshoot-yarn/apache-yarn-create-queue-4.png)

5. Yeni kuyruğu adlandırın.

    ![Apache ambarı YARN Pano adı kuyruğu](media/hdinsight-troubleshoot-yarn/apache-yarn-create-queue-5.png)  

6. **Kapasite** değerlerini %50 ' de bırakın ve sonra **Eylemler** düğmesini seçin.

    ![Apache ambarı YARN seçim eylemi](media/hdinsight-troubleshoot-yarn/apache-yarn-create-queue-6.png)  
7. **Kuyrukları Kaydet ve Yenile '** yi seçin.

    ![Kuyrukları Kaydet ve Yenile ' yi seçin](media/hdinsight-troubleshoot-yarn/apache-yarn-create-queue-7.png)  

Bu değişiklikler, YARN Zamanlayıcı kullanıcı arabiriminde hemen görünür.

### <a name="additional-reading"></a>Ek okuma

- [Apache Hadoop YARN CapacityScheduler](https://hadoop.apache.org/docs/r2.7.2/hadoop-yarn/hadoop-yarn-site/CapacityScheduler.html)

## <a name="how-do-i-download-yarn-logs-from-a-cluster"></a>Nasıl yaparım? YARN günlüklerini bir kümeden indir mi?

### <a name="resolution-steps"></a>Çözüm adımları

1. Bir Secure Shell (SSH) istemcisi kullanarak HDInsight kümesine bağlanın. Daha fazla bilgi için bkz. [ek okuma](#additional-reading-2).

1. Şu anda çalışmakta olan YARN uygulamalarının tüm uygulama kimliklerini listelemek için aşağıdaki komutu çalıştırın:

    ```apache
    yarn top
    ```

    Kimlikler, **ApplicationId** sütununda listelenir. Günlükleri **ApplicationId** sütunundan indirebilirsiniz.

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

1. Tüm uygulama yöneticileri için YARN kapsayıcı günlüklerini indirmek için aşağıdaki komutu kullanın:

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> -am ALL > amlogs.txt
    ```

    Bu komut amlogs.txt adlı bir günlük dosyası oluşturur.

1. YARN kapsayıcı günlüklerini yalnızca en son uygulama ana kopyası için indirmek üzere aşağıdaki komutu kullanın:

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> -am -1 > latestamlogs.txt
    ```

    Bu komut latestamlogs.txt adlı bir günlük dosyası oluşturur.

1. İlk iki uygulama ana için YARN kapsayıcı günlüklerini indirmek için aşağıdaki komutu kullanın:

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> -am 1,2 > first2amlogs.txt
    ```

    Bu komut first2amlogs.txt adlı bir günlük dosyası oluşturur.

1. Tüm YARN kapsayıcı günlüklerini indirmek için aşağıdaki komutu kullanın:

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> > logs.txt
    ```

    Bu komut logs.txt adlı bir günlük dosyası oluşturur.

1. Belirli bir kapsayıcı için YARN kapsayıcı günlüğünü indirmek için aşağıdaki komutu kullanın:

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> -containerId <container_id> > containerlogs.txt
    ```

    Bu komut containerlogs.txt adlı bir günlük dosyası oluşturur.

### <a name="additional-reading"></a><a name="additional-reading-2"></a>Ek okuma

- [SSH kullanarak HDInsight 'a (Apache Hadoop) bağlanma](./hdinsight-hadoop-linux-use-ssh-unix.md)
- [Apache Hadoop YARN kavramları ve uygulamaları](https://hadoop.apache.org/docs/r2.7.4/hadoop-yarn/hadoop-yarn-site/WritingYarnApplications.html#Concepts_and_Flow)

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmüyorsanız veya sorununuzu çözemediyseniz, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

- Azure [topluluk desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıt alın.

- [@AzureSupport](https://twitter.com/azuresupport)Müşteri deneyimini iyileştirmek için resmi Microsoft Azure hesabına bağlanın. Azure Community 'yi doğru kaynaklara bağlama: yanıtlar, destek ve uzmanlar.

- Daha fazla yardıma ihtiyacınız varsa [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **destek** ' i seçin veya **Yardım + Destek** hub 'ını açın. Daha ayrıntılı bilgi için [Azure destek isteği oluşturma](../azure-portal/supportability/how-to-create-azure-support-request.md)konusunu inceleyin. Abonelik yönetimi ve faturalandırma desteği 'ne erişim Microsoft Azure aboneliğinize dahildir ve [Azure destek planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla teknik destek sağlanır.