---
title: Hadoop Hive ile HDInsight - Azure PowerShell kullanma
description: HDInsight üzerinde Hadoop Hive sorguları çalıştırmak için PowerShell kullanın.
services: hdinsight
author: jasonwhowell
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 04/23/2018
ms.author: jasonh
ms.openlocfilehash: 16caee1b04b8fb3ae2e83b8105b802e121092f60
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "43051785"
---
# <a name="run-hive-queries-using-powershell"></a>PowerShell kullanarak Hive sorguları çalıştırma
[!INCLUDE [hive-selector](../../../includes/hdinsight-selector-use-hive.md)]

Bu belgede, HDInsight kümesinde bir Hadoop Hive sorguları çalıştırmak için Azure kaynak grubu moduna Azure PowerShell kullanarak bir örnek sağlar.

> [!NOTE]
> Bu belgede ayrıntılı açıklamasını örneklerde kullanılan HiveQL ifadelerini ne sağlamaz. Bu örnekte kullanılan HiveQL hakkında daha fazla bilgi için bkz: [HDInsight üzerinde Hadoop ile Hive kullanma](hdinsight-use-hive.md).

## <a name="prerequisites"></a>Önkoşullar

* Linux tabanlı Hadoop HDInsight kümesi sürüm 3.4 üzerindeki.

  > [!IMPORTANT]
  > Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

* Azure PowerShell ile bir istemci.

[!INCLUDE [upgrade-powershell](../../../includes/hdinsight-use-latest-powershell.md)]

## <a name="run-a-hive-query"></a>Hive sorgusu çalıştırma

Azure PowerShell sağlar *cmdlet'leri* uzaktan üzerinde HDInsight Hive sorguları çalıştırmanıza izin verir. Dahili olarak, REST çağrıları için cmdlet'leri olun [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) HDInsight kümesi üzerinde.

Bir uzak HDInsight kümesinde Hive sorgularının çalıştırılması aşağıdaki cmdlet'ler kullanılır:

* `Connect-AzureRmAccount`: Azure PowerShell, Azure aboneliğiniz için kimlik doğrulaması yapar.
* `New-AzureRmHDInsightHiveJobDefinition`: Oluşturur bir *iş tanımı* belirtilen HiveQL ifadelerini kullanarak.
* `Start-AzureRmHDInsightJob`: İş tanımı için HDInsight gönderir ve bir iş başlatılır. A *iş* nesne döndürülür.
* `Wait-AzureRmHDInsightJob`: İş nesnesi, iş durumunu denetlemek için kullanır. Bekleme süresi aşılırsa veya iş tamamlanana kadar bekler.
* `Get-AzureRmHDInsightJobOutput`: Tanımlı işlemin çıktısını almak için kullanılır.
* `Invoke-AzureRmHDInsightHiveJob`: HiveQL ifadelerini çalıştırmak için kullanılır. Bu cmdlet blokları sorgu tamamlandıktan sonra sonuçları döndürür.
* `Use-AzureRmHDInsightCluster`: Ayarlar için kullanılacak geçerli küme `Invoke-AzureRmHDInsightHiveJob` komutu.

Aşağıdaki adımlarda, HDInsight kümenizin bir işi çalıştırmak için bu cmdlet'leri kullanma gösterilmektedir:

1. Bir Düzenleyicisi'ni kullanarak aşağıdaki kodun da Kaydet `hivejob.ps1`.

    [!code-powershell[main](../../../powershell_scripts/hdinsight/use-hive/use-hive.ps1?range=5-42)]

2. Yeni bir **Azure PowerShell** komut istemi. Dizinleri konumuna `hivejob.ps1` dosya ve betiği çalıştırmak için aşağıdaki komutu kullanın:

        .\hivejob.ps1

    Komut dosyası çalıştığında, küme için küme adına ve HTTPS/yönetici hesabı kimlik bilgilerini girmeniz istenir. Azure aboneliğinizde oturum açmak için ayrıca istenebilir.

3. İş tamamlandığında, bilgileri aşağıdaki metne benzer döndürür:

        Display the standard output...
        2012-02-03      18:35:34        SampleClass0    [ERROR] incorrect       id
        2012-02-03      18:55:54        SampleClass1    [ERROR] incorrect       id
        2012-02-03      19:25:27        SampleClass4    [ERROR] incorrect       id

4. Daha önce belirtildiği `Invoke-Hive` bir sorgu çalıştırmak ve yanıt için beklemek için kullanılabilir. Invoke-Hive nasıl çalıştığını görmek için aşağıdaki betiği kullanın:

    [!code-powershell[main](../../../powershell_scripts/hdinsight/use-hive/use-hive.ps1?range=50-71)]

    Çıkışı şu metin gibi görünür:

        2012-02-03    18:35:34    SampleClass0    [ERROR]    incorrect    id
        2012-02-03    18:55:54    SampleClass1    [ERROR]    incorrect    id
        2012-02-03    19:25:27    SampleClass4    [ERROR]    incorrect    id

   > [!NOTE]
   > Uzun HiveQL sorgular için Azure PowerShell kullanabilirsiniz **burada dizeler** cmdlet veya HiveQL komut dosyaları. Aşağıdaki kod parçacığını nasıl kullanılacağını gösterir `Invoke-Hive` cmdlet'ini HiveQL komut dosyasını çalıştırın. Wasb için HiveQL komut dosyası karşıya yüklenmelidir: / /.
   >
   > `Invoke-AzureRmHDInsightHiveJob -File "wasb://<ContainerName>@<StorageAccountName>/<Path>/query.hql"`
   >
   > Hakkında daha fazla bilgi için **burada dizeler**, bkz: <a href="http://technet.microsoft.com/library/ee692792.aspx" target="_blank">kullanarak Windows PowerShell burada-dizeleri</a>.

## <a name="troubleshooting"></a>Sorun giderme

İş tamamlandığında hiçbir bilgi döndürülürse, hata günlüklerini görüntüleyin. Bu işi hata bilgilerini görüntülemek için aşağıdaki sonuna ekleyin `hivejob.ps1` dosyasını kaydedin ve yeniden çalıştırın.

```powershell
# Print the output of the Hive job.
Get-AzureRmHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $job.JobId `
        -HttpCredential $creds `
        -DisplayOutputType StandardError
```

Bu cmdlet iş işleme sırasında yazılan STDERR bilgileri döndürür.

## <a name="summary"></a>Özet

Gördüğünüz gibi Azure PowerShell, bir HDInsight kümesinde Hive sorguları çalıştırma, iş durumunu izlemek ve çıktısını almak için kolay bir yol sağlar.

## <a name="next-steps"></a>Sonraki adımlar

HDInsight Hive hakkında genel bilgi için:

* [HDInsight üzerinde Hadoop ile Hive kullanma](hdinsight-use-hive.md)

Diğer yollar hakkında daha fazla bilgi için HDInsight üzerinde Hadoop ile çalışabilirsiniz:

* [HDInsight üzerinde Hadoop ile Pig kullanma](hdinsight-use-pig.md)
* [HDInsight üzerinde Hadoop ile MapReduce kullanma](hdinsight-use-mapreduce.md)
