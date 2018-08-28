---
title: HDInsight - Azure, Hadoop ile MapReduce ve SSH bağlantısı
description: HDInsight üzerinde Hadoop kullanarak MapReduce işlerini çalıştırmak için SSH'ı kullanmayı öğrenin.
services: hdinsight
author: jasonwhowell
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 04/10/2018
ms.author: jasonh
ms.openlocfilehash: 71472edf74fba433f24b83362b2880575b73ce85
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "43047332"
---
# <a name="use-mapreduce-with-hadoop-on-hdinsight-with-ssh"></a>HDInsight ile SSH üzerinde Hadoop ile MapReduce kullanma

[!INCLUDE [mapreduce-selector](../../../includes/hdinsight-selector-use-mapreduce.md)]

Güvenli Kabuk (SSH) bağlantısı HDInsight MapReduce işlerini gönderme hakkında bilgi edinin.

> [!NOTE]
> Zaten Linux tabanlı Hadoop sunucularını kullanma ile ilgili bilgi sahibi olduğunuz, ancak yeni HDInsight için, bkz [Linux tabanlı HDInsight ipuçları](../hdinsight-hadoop-linux-information.md).

## <a id="prereq"></a>Önkoşullar

* Linux tabanlı HDInsight (Hadoop HDInsight üzerinde) kümesi

  > [!IMPORTANT]
  > Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

* Bir SSH istemcisi. Daha fazla bilgi için [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md)

## <a id="ssh"></a>SSH ile bağlanma

SSH kullanarak kümeye bağlanın. Örneğin, aşağıdaki komut adlı bir kümeye bağlanır **myhdinsight** olarak **sshuser** hesabı:

```bash
ssh sshuser@myhdinsight-ssh.azurehdinsight.net
```

**SSH kimlik doğrulaması için bir sertifika anahtarı kullanırsanız**, istemci sisteminizde özel anahtar konumunu belirtin, örneğin gerekebilir:

```bash
ssh -i ~/mykey.key sshuser@myhdinsight-ssh.azurehdinsight.net
```

**SSH kimlik doğrulaması için parola kullanıyorsanız**, istendiğinde parolayı sağlamanız gerekir.

HDInsight ile SSH kullanma hakkında daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a id="hadoop"></a>Hadoop komutlarını kullanma

1. HDInsight kümesine bağlandıktan sonra bir MapReduce işi başlatmak için aşağıdaki komutu kullanın:

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/WordCountOutput
    ```

    Bu komut başlatır `wordcount` bulunan sınıfı `hadoop-mapreduce-examples.jar` dosya. Kullandığı `/example/data/gutenberg/davinci.txt` en depolanan girdi ve çıktı olarak belge `/example/data/WordCountOutput`.

    > [!NOTE]
    > Bu bir MapReduce işi ve örnek veriler hakkında daha fazla bilgi için bkz. [, HDInsight üzerinde Hadoop MapReduce kullanma](hdinsight-use-mapreduce.md).

2. İş, işler ve iş tamamlandığında bilgi aşağıdaki metne benzer döndürür gibi ayrıntıları gösterir:

        File Input Format Counters
        Bytes Read=1395666
        File Output Format Counters
        Bytes Written=337623

3. İş tamamlandığında, Çıkış dosyalarını listelemek için aşağıdaki komutu kullanın:

    ```bash
    hdfs dfs -ls /example/data/WordCountOutput
    ```

    Bu komut iki dosya görüntüleme `_SUCCESS` ve `part-r-00000`. `part-r-00000` Bu iş için çıktı dosyası içerir.

    > [!NOTE]
    > Bazı MapReduce işleri sonuçları arasında birden fazla bölme **bölümü r ###** dosyaları. Bu durumda, kullanın ### dosyaların sırasını göstermek için soneki.

4. Çıkışı görüntülemek için aşağıdaki komutu kullanın:

    ```bash
    hdfs dfs -cat /example/data/WordCountOutput/part-r-00000
    ```

    Bu komut bulunan bir kelimelerin listesini görüntüler **wasb://example/data/gutenberg/davinci.txt** dosya ve her sözcüğün oluştu sayısı. Dosyada bulunan verilerin bir örnek aşağıda gösterilmiştir:

        wreathed        3
        wreathing       1
        wreaths         1
        wrecked         3
        wrenching       1
        wretched        6
        wriggling       1

## <a id="summary"></a>Özet

Gördüğünüz gibi Hadoop komutlarını bir HDInsight kümesinde MapReduce işlerini çalıştırın ve ardından işi çıktısını görüntülemek için kolay bir yol sağlar.

## <a id="nextsteps"></a>Sonraki adımlar

HDInsight MapReduce işleri hakkında genel bilgi için:

* [HDInsight üzerinde Hadoop MapReduce kullanma](hdinsight-use-mapreduce.md)

Diğer yollar hakkında daha fazla bilgi için HDInsight üzerinde Hadoop ile çalışabilirsiniz:

* [HDInsight üzerinde Hadoop ile Hive kullanma](hdinsight-use-hive.md)
* [HDInsight üzerinde Hadoop ile Pig kullanma](hdinsight-use-pig.md)
