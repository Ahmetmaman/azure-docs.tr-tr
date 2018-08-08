---
title: MapReduce kullanma ve Curl ile HDInsight - Azure Hadoop
description: Uzaktan Hadoop ile MapReduce işleri Curl kullanarak HDInsight üzerinde çalıştırmayı öğrenin.
services: hdinsight
author: jasonwhowell
editor: jasonwhowell
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/27/2018
ms.author: jasonh
ms.openlocfilehash: f6b72a464bfd5eee2bedc52fd9f7163f2c970c13
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39590726"
---
# <a name="run-mapreduce-jobs-with-hadoop-on-hdinsight-using-rest"></a>REST kullanarak HDInsight üzerinde Hadoop MapReduce işlerle çalışma

HDInsight kümesi üzerinde bir Hadoop MapReduce işlerini çalıştırmak için WebHCat REST API'sini kullanmayı öğrenin. Curl MapReduce işlerini çalıştırmak için ham HTTP isteklerini kullanarak HDInsight ile nasıl etkileşim kurabileceğine göstermek için kullanılır.

> [!NOTE]
> Zaten Linux tabanlı Hadoop sunucularını kullanma ile ilgili bilgi sahibi olduğunuz, ancak yeni HDInsight için, bkz [Linux tabanlı HDInsight üzerinde Hadoop hakkında bilmeniz gerekenler](../hdinsight-hadoop-linux-information.md) belge.


## <a id="prereq"></a>Önkoşullar

* HDInsight kümesi üzerinde bir Hadoop
* Windows PowerShell veya [Curl](http://curl.haxx.se/) ve [jq](http://stedolan.github.io/jq/)

## <a id="curl"></a>Bir MapReduce işi çalıştırın

> [!NOTE]
> WebHCat ile Curl veya başka bir REST iletişimini kullanırken HDInsight küme yöneticisinin kullanıcı adı ve parolasını sağlayarak isteklerin kimliğini doğrulaması gerekir. Sunucuya istek göndermek için kullanılan URI'ın bir parçası olarak küme adını kullanmanız gerekir.
>
> REST API kullanılarak korunmaktadır [temel erişimi kimlik doğrulaması](http://en.wikipedia.org/wiki/Basic_access_authentication). Ayrıca, kimlik bilgilerinizin sunucuya güvenli bir şekilde gönderildiğinden emin olmak için HTTPS kullanarak istekleri her zaman yapmanız gerekir.

1. Bu belgedeki betikler tarafından kullanılan Küme oturum açma ayarlamak için followig komutlardan birini kullanın:

    ```bash
    read -p "Enter your cluster login account name: " LOGIN
    ```

    ```powershell
    $creds = Get-Credential -UserName admin -Message "Enter the cluster login name and password"
    ```

2. Küme adı ayarlamak için aşağıdaki komutlardan birini kullanın:

    ```bash
    read -p "Enter the HDInsight cluster name: " CLUSTERNAME
    ```

    ```powershell
    $clusterName = Read-Host -Prompt "Enter the HDInsight cluster name"
    ```

3. HDInsight kümenize bağlanabildiğinizi doğrulamak için bir komut satırında aşağıdaki komutu kullanın:

    ```bash
    curl -u $LOGIN -G https://$CLUSTERNAME.azurehdinsight.net/templeton/v1/status
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clustername.azurehdinsight.net/templeton/v1/status" `
        -Credential $creds `
        -UseBasicParsing
    $resp.Content
    ```

    Aşağıdaki JSON ile benzer bir yanıt alırsınız:

        {"status":"ok","version":"v1"}

    Bu komutta kullanılan parametreler aşağıdaki gibidir:

   * **-u**: İstek kimliğini doğrulamak için kullanılan parola ve kullanıcı adını belirtir.
   * **-G**: Bu işlem bir GET isteği olduğunu belirtir

   URI başlangıcını **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, tüm istekler için aynıdır.

4. Bir MapReduce işi göndermek için aşağıdaki komutu kullanın:

    ```bash
    JOBID=`curl -u $LOGIN -d user.name=$LOGIN -d jar=/example/jars/hadoop-mapreduce-examples.jar -d class=wordcount -d arg=/example/data/gutenberg/davinci.txt -d arg=/example/data/output https://$CLUSTERNAME.azurehdinsight.net/templeton/v1/mapreduce/jar | jq .id`
    echo $JOBID
    ```

    ```powershell
    $reqParams = @{}
    $reqParams."user.name" = "admin"
    $reqParams.jar = "/example/jars/hadoop-mapreduce-examples.jar"
    $reqParams.class = "wordcount"
    $reqParams.arg = @()
    $reqParams.arg += "/example/data/gutenberg/davinci.txt"
    $reqparams.arg += "/example/data/output"
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/templeton/v1/mapreduce/jar" `
       -Credential $creds `
       -Body $reqParams `
       -Method POST `
       -UseBasicParsing
    $jobID = (ConvertFrom-Json $resp.Content).id
    $jobID
    ```

    (/ Mapreduce/jar) URI'nin sonuna, bu isteği bir jar dosyası içinde bir sınıftaki bir MapReduce işi başlatır WebHCat söyler. Bu komutta kullanılan parametreler aşağıdaki gibidir:

   * **-d**: `-G` POST yöntemine istek varsayılan olarak bu nedenle, kullanılmaz. `-d` istekle beraber gönderilen veri değerleri belirtir.
    * **User.Name**: komutu çalıştıran kullanıcının
    * **jar**: olmasını sınıfı içeren jar dosyasını konumunu çalıştı
    * **sınıf**: MapReduce mantığı içeren sınıfı
    * **arg**: MapReduce işi için geçirilecek bağımsız değişkenleri. Bu durumda, giriş metin dosyasına ve çıkış için kullanılan dizini

   Bu komut, iş durumunu denetlemek için kullanılan bir iş kimliği döndürülmesi gerekir:

       job_1415651640909_0026

5. İşin durumunu denetlemek için aşağıdaki komutu kullanın:

    ```bash
    curl -G -u $LOGIN -d user.name=$LOGIN https://$CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/$JOBID | jq .status.state
    ```

    ```powershell
    $reqParams=@{"user.name"="admin"}
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/templeton/v1/jobs/$jobID" `
       -Credential $creds `
       -Body $reqParams `
       -UseBasicParsing
    # ConvertFrom-JSON can't handle duplicate names with different case
    # So change one to prevent the error
    $fixDup=$resp.Content.Replace("jobID","job_ID")
    (ConvertFrom-Json $fixDup).status.state
    ```

    İş tamamlandığında, durum döndürülür `SUCCEEDED`.

   > [!NOTE]
   > Bu Curl isteği bir JSON belgesi işle ilgili bilgileri döndürür. Jq, yalnızca durum değeri almak için kullanılır.

6. İş durumunu değiştiği için `SUCCEEDED`, Azure Blob depolama alanından iş sonuçlarını alabilirsiniz. `statusdir` Sorguyla geçirilen parametre içeren çıkış dosyasının konumu. Bu örnekte, konumdur `/example/curl`. Bu adres kümeleri varsayılan depolama alanı tanımlı işlemin çıktısını depolar `/example/curl`.

Liste ve kullanarak bu dosyaları indirmek [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli). Azure clı'dan bloblarla çalışma hakkında daha fazla bilgi için bkz. [Azure depolama ile Azure CLI 2.0 kullanarak](../../storage/common/storage-azure-cli.md#create-and-manage-blobs) belge.

## <a id="nextsteps"></a>Sonraki adımlar

HDInsight MapReduce işleri hakkında genel bilgi için:

* [HDInsight üzerinde Hadoop ile MapReduce kullanma](hdinsight-use-mapreduce.md)

Diğer yollar hakkında daha fazla bilgi için HDInsight üzerinde Hadoop ile çalışabilirsiniz:

* [HDInsight üzerinde Hadoop ile Hive kullanma](hdinsight-use-hive.md)
* [HDInsight üzerinde Hadoop ile Pig kullanma](hdinsight-use-pig.md)

Bu makalede kullanılan REST arabirimi hakkında daha fazla bilgi için bkz. [WebHCat başvuru](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference).
