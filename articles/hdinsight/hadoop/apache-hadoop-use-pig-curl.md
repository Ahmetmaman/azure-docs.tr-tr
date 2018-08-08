---
title: HDInsight - Azure geri KALANI ile Hadoop Pig kullanma
description: Azure HDInsight Hadoop kümesinde Pig Latin işlerini çalıştırmak için REST kullanma konusunda bilgi edinin.
services: hdinsight
author: jasonwhowell
editor: jasonwhowell
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 04/10/2018
ms.author: jasonh
ms.openlocfilehash: 6804e4661948c444db0ec3ecc241abc8ba6e00eb
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39599114"
---
# <a name="run-pig-jobs-with-hadoop-on-hdinsight-by-using-rest"></a>Pig işleri, REST kullanarak HDInsight üzerinde Hadoop ile çalıştırın

[!INCLUDE [pig-selector](../../../includes/hdinsight-selector-use-pig.md)]

Bir Azure HDInsight kümesi için REST istekleri yaparak pig Latin işleri çalıştırmayı öğrenin. Curl WebHCat REST API'yi kullanarak HDInsight ile nasıl etkileşim kurabileceğine göstermek için kullanılır.

> [!NOTE]
> Zaten Linux tabanlı Hadoop sunucularını kullanma ile ilgili bilgi sahibi olduğunuz, ancak yeni HDInsight için, bkz. [Linux tabanlı HDInsight ipuçları](../hdinsight-hadoop-linux-information.md).

## <a id="prereq"></a>Önkoşullar

* Bir Azure HDInsight (Hadoop HDInsight üzerinde) kümesi (Linux tabanlı veya Windows tabanlı)

  > [!IMPORTANT]
  > Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

* [Curl](http://curl.haxx.se/)

* [jq](http://stedolan.github.io/jq/)

## <a id="curl"></a>Pig işleri Curl kullanarak çalıştırma

> [!NOTE]
> REST API aracılığıyla güvenli [temel erişimi kimlik doğrulaması](http://en.wikipedia.org/wiki/Basic_access_authentication). Her zaman güvenli HTTP (HTTPS) kullanarak kimlik bilgilerinizin sunucuya güvenli bir şekilde gönderildiğinden emin olmak için istekleri olun.
>
> Bu bölümdeki komutları kullanılırken, değiştirin `USERNAME` kümesinin kimliğini doğrulama ve değiştirme için kullanıcıyla `PASSWORD` kullanıcı hesabının parolası ile. `CLUSTERNAME` değerini kümenizin adıyla değiştirin.
>


1. HDInsight kümenize bağlanabildiğinizi doğrulamak için bir komut satırında aşağıdaki komutu kullanın:

    ```bash
    curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status
    ```

    Aşağıdaki JSON yanıtı almanız gerekir:

        {"status":"ok","version":"v1"}

    Bu komutta kullanılan parametreler aşağıdaki gibidir:

    * **-u**: kullanıcı adı ve istek kimliğini doğrulamak için kullanılan parola
    * **-G**: Bu isteği bir GET isteği olduğunu belirtir

     URL'nin başına **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, tüm istekler için aynıdır. Yol **/status**, sunucu için istek WebHCat (Ayrıca templeton olarak da bilinir) durumunu döndürmek üzere olduğunu gösterir.

2. Bir küme için Pig Latin işi göndermek için aşağıdaki kodu kullanın:

    ```bash
    curl -u USERNAME:PASSWORD -d user.name=USERNAME -d execute="LOGS=LOAD+'/example/data/sample.log';LEVELS=foreach+LOGS+generate+REGEX_EXTRACT($0,'(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)',1)+as+LOGLEVEL;FILTEREDLEVELS=FILTER+LEVELS+by+LOGLEVEL+is+not+null;GROUPEDLEVELS=GROUP+FILTEREDLEVELS+by+LOGLEVEL;FREQUENCIES=foreach+GROUPEDLEVELS+generate+group+as+LOGLEVEL,COUNT(FILTEREDLEVELS.LOGLEVEL)+as+count;RESULT=order+FREQUENCIES+by+COUNT+desc;DUMP+RESULT;" -d statusdir="/example/pigcurl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/pig
    ```

    Bu komutta kullanılan parametreler aşağıdaki gibidir:

    * **-d**: çünkü `-G` kullanılmıyorsa, varsayılan POST yöntemine istek. `-d` istekle beraber gönderilen veri değerleri belirtir.

    * **User.Name**: komutu çalıştıran kullanıcının
    * **yürütme**: yürütmek için Pig Latin açıklamaları
    * **statusdir**: Bu görev için durum yazılan dizini

    > [!NOTE]
    > Pig Latin açıklamaları boşluk tarafından değiştirilen uyarı `+` Curl ile kullanılan karakter.

    Bu komut, örneğin, iş durumunu denetlemek için kullanılan bir iş kimliği döndürülmesi gerekir:

        {"id":"job_1415651640909_0026"}

3. İşin durumunu denetlemek için aşağıdaki komutu kullanın.

     ```bash
    curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state
    ```

     Değiştirin `JOBID` ile önceki adımda döndürülen değer. Örneğin, dönüş değeri `{"id":"job_1415651640909_0026"}`, ardından `JOBID` olduğu `job_1415651640909_0026`.

    İş bitip bitmediğini durumudur **başarılı**.

    > [!NOTE]
    > Bu Curl isteği bir JavaScript nesne gösterimi (JSON) belge ile iş hakkındaki bilgileri döndürür ve jq yalnızca durum değeri almak için kullanılır.

## <a id="results"></a>Sonuçları Görüntüle

İş durumunu değiştiği için **başarılı**, iş sonuçları alabilir. `statusdir` Sorguyla geçirilen parametre içerir; bu durumda, çıkış dosyasının konumu `/example/pigcurl`.

HDInsight, Azure depolama veya Azure Data Lake Store varsayılan veri deposu olarak kullanabilirsiniz. Hangisinin bağlı olarak, kullandığınız veri almanın çeşitli yolları vardır. Daha fazla bilgi için bkz. depolama bölümünü [Linux tabanlı HDInsight bilgi](../hdinsight-hadoop-linux-information.md#hdfs-azure-storage-and-data-lake-store) belge.

## <a id="summary"></a>Özet

Bu belgede gösterildiği gibi ham bir HTTP isteği çalıştırma, izleme ve HDInsight kümenizi Pig işleri sonuçlarını görüntülemek için kullanabilirsiniz.

Bu makalede kullanılan REST arabirimi hakkında daha fazla bilgi için bkz. [WebHCat başvuru](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference).

## <a id="nextsteps"></a>Sonraki adımlar

HDInsight Pig hakkında genel bilgi için:

* [HDInsight üzerinde Hadoop ile Pig kullanma](hdinsight-use-pig.md)

Diğer yollar hakkında daha fazla bilgi için HDInsight üzerinde Hadoop ile çalışabilirsiniz:

* [HDInsight üzerinde Hadoop ile Hive kullanma](hdinsight-use-hive.md)
* [HDInsight üzerinde Hadoop ile MapReduce kullanma](hdinsight-use-mapreduce.md)
