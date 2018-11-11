---
title: Hadoop korumalı alanı - öykünücüsü - Azure HDInsight'ı kullanarak bilgi edinin
description: 'Hadoop ekosistemi kullanma hakkında öğrenmeye başlayın için bir Azure sanal makinesinde Hortonworks Hadoop korumalı alan ayarlayabilirsiniz. '
keywords: hadoop öykünücüsü, hadoop korumalı alanı
ms.reviewer: jasonh
services: hdinsight
author: hrasheed-msft
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 12/11/2017
ms.author: hrasheed
ms.openlocfilehash: add1833671b995e747af6a447c65114d1add76e9
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51013114"
---
# <a name="get-started-with-a-hadoop-sandbox-an-emulator-on-a-virtual-machine"></a>Hadoop korumalı alanı, bir sanal makinede bir öykünücü ile çalışmaya başlama

Hadoop ekosistemi hakkında bilgi edinmek için bir sanal makinede Hortonworks Hadoop korumalı alanı yüklemeyi öğrenin. Korumalı alan, Hadoop, Hadoop dağıtılmış dosya sistemi (HDFS) ve iş gönderme hakkında bilgi edinmek için bir yerel geliştirme ortamı sağlar. Hadoop ile ilgili bilgi sahibi olduktan sonra bir HDInsight kümesi oluşturarak Azure üzerinde Hadoop kullanmaya başlayabilirsiniz. Nasıl başlayacağınızı hakkında daha fazla bilgi için bkz. [HDInsight üzerinde Hadoop kullanmaya başlama](apache-hadoop-linux-tutorial-get-started.md).

## <a name="prerequisites"></a>Önkoşullar
* [Oracle VirtualBox](https://www.virtualbox.org/). Buradan indirip [burada](https://www.virtualbox.org/wiki/Downloads).



## <a name="download-and-install-the-virtual-machine"></a>Sanal makineye yükleyip
1. Gözat [Hortonworks indirir](http://hortonworks.com/downloads/#sandbox).

2. Tıklayın **İNDİRMEK için VIRTUALBOX** en son Hortonworks korumalı alanı bir VM'de indirilemedi. Hortonworks ile yükleme başlamadan önce kaydetmeniz istenir. Ağ hızınıza bağlı olarak indirmek için bir ila iki saat sürer.
   
    ![Bağlantı resmi için Hortonworks korumalı alanı VirtualBox için indirin](./media/apache-hadoop-emulator-get-started/download-sandbox.png)
3. Aynı web sayfasından tıklayın **Virtual Box üzerinde içeri aktarma** sanal makine için yükleme yönergelerini içeren bir PDF indirmek için bağlantı.

Eski bir HDP sürüm korumalı alan indirmek için arşiv genişletin:

![Hortonworks korumalı alanı arşiv](./media/apache-hadoop-emulator-get-started/hortonworks-sandbox-archive.png)


## <a name="start-the-virtual-machine"></a>Sanal makineyi Başlat

1. Oracle VM VirtualBox açın.
2. Gelen **dosya** menüsünde, tıklayın **alma Gereci**ve ardından Hortonworks korumalı alanı görüntüsünü belirtin.
1. Hortonworks korumalı alanı seçip **Başlat**, ardından **Normal başlangıç**. Sanal makinenin önyükleme işlemi tamamlandıktan sonra oturum açma yönergeleri görüntüler.
   
    ![Normal Başlat](./media/apache-hadoop-emulator-get-started/normal-start.png)
2. Bir web tarayıcısı açın ve görüntülenen URL'ye gidin (genellikle http://127.0.0.1:8888).

## <a name="set-sandbox-passwords"></a>Korumalı alan parola ayarlama

1. Gelen **başlama** select Hortonworks Sandbox sayfası adımında **görünüm Gelişmiş Seçenekler**. Bilgiler, SSH kullanarak korumalı alan oturum açmak için bu sayfada kullanın. Sağlanan adını ve parolayı kullanın.
   
   > [!NOTE]
   > Bir SSH istemcisi yüklü değilse, sanal makine tarafından verilen web tabanlı SSH kullanabilirsiniz **http://localhost:4200/**.
   > 
   
    SSH kullanarak bağlandığınız ilk kez kök hesabın parolasını değiştirmeniz istenir. SSH kullanarak oturum açışınızda kullanacağınız yeni bir parola girin.

2. Oturum açtıktan sonra aşağıdaki komutu girin:
   
        ambari-admin-password-reset
   
    İstendiğinde, Ambari yönetici hesabı için bir parola sağlayın. Ambari Web kullanıcı arabirimini eriştiğinde kullanılır.

## <a name="use-hive-commands"></a>Hive komutlarını kullanma

1. Korumalı bir SSH bağlantısından, Hive kabuğunu başlatmak için aşağıdaki komutu kullanın:
   
        hive
2. Kabuk başlatıldıktan sonra korumalı alan ile sağlanan tabloları görüntülemek için aşağıdakileri kullanın:
   
        show tables;
3. 10 satırlardan almak için aşağıdakini kullanın `sample_07` tablosu:
   
        select * from sample_07 limit 10;

## <a name="next-steps"></a>Sonraki adımlar
* [Hortonworks korumalı alanı ile Visual Studio kullanmayı öğrenin](../hdinsight-hadoop-emulator-visual-studio.md)
* [Hortonworks korumalı alanı işin öğrenme](http://hortonworks.com/hadoop-tutorial/learning-the-ropes-of-the-hortonworks-sandbox/)
* [Hadoop Öğreticisi - HDP ile çalışmaya başlama](http://hortonworks.com/hadoop-tutorial/hello-world-an-introduction-to-hadoop-hcatalog-hive-and-pig/)

