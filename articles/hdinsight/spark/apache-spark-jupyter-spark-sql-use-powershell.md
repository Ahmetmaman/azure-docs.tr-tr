---
title: 'Hızlı Başlangıç: Azure PowerShell kullanarak HDInsight’ta Spark kümesi oluşturma'
description: Bu hızlı başlangıçta, Azure PowerShell kullanılarak nasıl Azure HDInsight’ta Apache Spark kümesi oluşturulacağı ve basit bir Spark SQL sorgusu çalıştırılacağı gösterilmektedir.
services: azure-hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: quickstart
ms.date: 05/07/2018
ms.author: hrasheed
ms.custom: mvc
ms.openlocfilehash: fcc25449d4e65dc36be0eb2f13cb35bf23ae6f00
ms.sourcegitcommit: 345b96d564256bcd3115910e93220c4e4cf827b3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2018
ms.locfileid: "52498051"
---
# <a name="quickstart-create-an-apache-spark-cluster-in-hdinsight-using-powershell"></a>Hızlı Başlangıç: PowerShell kullanarak HDInsight Apache Spark kümesi oluşturma
Oluşturmayı [Apache Spark](https://spark.apache.org/) karşı Spark SQL sorgularının nasıl çalıştırılacağını ve Azure HDInsight kümesinde [Apache Hive](https://hive.apache.org/) tablolar. Apache Spark, bellek içi işleme kullanarak hızlı veri analizi ve küme hesaplama sağlar. HDInsight’ta Spark hakkında daha fazla bilgi için bkz. [Genel Bakış: Azure HDInsight’ta Apache Spark](apache-spark-overview.md).

Bu hızlı başlangıçta, HDInsight Spark kümesi oluşturmak için Azure PowerShell kullanırsınız. Küme, küme depolama alanı olarak Azure Depolama Bloblarını kullanır. Data Lake Storage Gen2'yi kullanma hakkında daha fazla bilgi için bkz. [Hızlı başlangıç: HDInsight'ta kümeleri ayarlama](../../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md).

> [!IMPORTANT]
> İster kullanın, ister kullanmayın, HDInsight kümeleri faturalaması dakika başına eşit olarak dağıtılmıştır. Kullanmayı bitirdikten sonra kümenizi sildiğinizden emin olun. Daha fazla bilgi için bu makalenin [Kaynakları temizleme](#clean-up-resources) bölümüne bakın.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="create-an-hdinsight-spark-cluster"></a>HDInsight Spark kümesi oluşturma

HDInsight kümesi oluşturma işlemi, aşağıdaki Azure nesnelerinin ve kaynaklarının oluşturulmasını kapsar:

- Bir Azure kaynak grubu. Azure kaynak grubu, Azure kaynakları için bir kapsayıcıdır. 
- Bir Azure depolama hesabı veya Azure Data Lake Store.  Her HDInsight kümesi için bağımlı bir veri depolama alanı gerekir. Bu hızlı başlangıçta bir depolama hesabı oluşturursunuz.
- Farklı küme türlerinde bir HDInsight kümesi.  Bu hızlı başlangıçta bir Spark 2.3 kümesi oluşturursunuz.

Kaynakları oluşturmak için bir PowerShell betiği kullanırsınız.  Betiği çalıştırdığınızda aşağıdaki değerleri girmeniz istenir:

|Parametre|Değer|
|------|------|
|Azure kaynak grubu adı | Kaynak grubu için benzersiz bir ad girin.|
|Konum| Azure bölgesini belirtin (örneğin, 'Orta ABD'). |
|Varsayılan depolama hesabı adı | Depolama hesabına benzersiz bir ad verin. |
|Küme adı | HDInsight Spark kümesine benzersiz bir ad verin.|
|Küme oturum açma kimlik bilgileri | Hızlı başlangıcın ilerleyen kısmında küme panosuna bağlanmak için bu hesabı kullanırsınız.|
|SSH kullanıcı kimlik bilgileri | HDInsight kümeleriyle uzak bir komut satırı oturumu oluşturmak için SSH istemcileri kullanılabilir.|



1. Aşağıdaki kod bloğu için sağ üst köşedeki **Deneyin** seçeneğine tıklayarak [Azure Cloud Shell](../../cloud-shell/overview.md)’i açın ve Azure’a bağlanmak için yönergeleri izleyin.
2. Aşağıdaki PowerShell betiğini kopyalayıp cloud shell’e yapıştırın. 

    ```azurepowershell-interactive
    ### Create a Spark 2.3 cluster in Azure HDInsight
        
    # Create the resource group
    $resourceGroupName = Read-Host -Prompt "Enter the resource group name"
    $location = Read-Host -Prompt "Enter the Azure region to create resources in, such as 'Central US'"
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
    
    $defaultStorageAccountName = Read-Host -Prompt "Enter the default storage account name"
    
    # Create an Azure storae account and container
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $defaultStorageAccountName `
        -Type Standard_LRS `
        -Location $location
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                    -ResourceGroupName $resourceGroupName `
                                    -Name $defaultStorageAccountName)[0].Value
    $defaultStorageContext = New-AzureStorageContext `
                                    -StorageAccountName $defaultStorageAccountName `
                                    -StorageAccountKey $defaultStorageAccountKey
    
    # Create a Spark 2.3 cluster
    $clusterName = Read-Host -Prompt "Enter the name of the HDInsight cluster"
    # Cluster login is used to secure HTTPS services hosted on the cluster
    $httpCredential = Get-Credential -Message "Enter Cluster login credentials" -UserName "admin"
    # SSH user is used to remotely connect to the cluster using SSH clients
    $sshCredentials = Get-Credential -Message "Enter SSH user credentials"
    
    # Default cluster size (# of worker nodes), version, type, and OS
    $clusterSizeInNodes = "1"
    $clusterVersion = "3.6"
    $clusterType = "Spark"
    $clusterOS = "Linux"
    
    # Set the storage container name to the cluster name
    $defaultBlobContainerName = $clusterName
    
    # Create a blob container. This holds the default data store for the cluster.
    New-AzureStorageContainer `
        -Name $clusterName -Context $defaultStorageContext 
    
    $sparkConfig = New-Object "System.Collections.Generic.Dictionary``2[System.String,System.String]"
    $sparkConfig.Add("spark", "2.3")
    
    # Create the HDInsight cluster
    New-AzureRmHDInsightCluster `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $clusterName `
        -Location $location `
        -ClusterSizeInNodes $clusterSizeInNodes `
        -ClusterType $clusterType `
        -OSType $clusterOS `
        -Version $clusterVersion `
        -ComponentVersion $sparkConfig `
        -HttpCredential $httpCredential `
        -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultStorageContainer $clusterName `
        -SshCredential $sshCredentials 
    
    Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName
    ```
Kümenin oluşturulması yaklaşık 20 dakika sürer. Sonraki oturumuna devam etmeden önce küme oluşturulması gerekir.

HDInsight kümelerini oluştururken sorunlarla karşılaşırsanız, bunu yapmak için doğru izinlere sahip olmayabilirsiniz. Daha fazla bilgi için bkz. [Erişim denetimi gereksinimleri](../hdinsight-administer-use-portal-linux.md#create-clusters).

## <a name="create-a-jupyter-notebook"></a>Jupyter not defteri oluşturma

[Jupyter not defteri](https://jupyter.org/) çeşitli destekleyen etkileşimli bir not defteri ortam programlama dilleri. Not defteri, verilerle etkileşim kurmanıza, kodu markdown metniyle birleştirmenize ve basit görselleştirmeler gerçekleştirmenize olanak sağlar. 

1. [Azure portalı](https://portal.azure.com) açın.
2. **HDInsight kümeleri**’ni ve sonra oluşturduğunuz kümeyi seçin.

    ![Azure portalında HDInsight kümesini açma](./media/apache-spark-jupyter-spark-sql/azure-portal-open-hdinsight-cluster.png)

3. Portaldan **Küme panoları**’nı ve sonra **Jupyter Notebook**’u seçin. İstendiğinde, küme için küme oturum açma kimlik bilgilerini girin.

   ![Jupyter not defterini açarak etkileşimli Spark SQL sorgusu çalıştırma](./media/apache-spark-jupyter-spark-sql/hdinsight-spark-open-jupyter-interactive-spark-sql-query.png "Jupyter not defterini açarak etkileşimli Spark SQL sorgusu çalıştırma")

4. **Yeni** > **PySpark** seçeneklerini belirleyerek bir not defteri oluşturun. 

   ![Jupyter Not Defteri’ni oluşturarak etkileşimli Spark SQL sorgusu çalıştırma](./media/apache-spark-jupyter-spark-sql/hdinsight-spark-create-jupyter-interactive-spark-sql-query.png "Jupyter Not Defteri’ni oluşturarak etkileşimli Spark SQL sorgusu çalıştırma")

   Untitled(Untitled.pynb) adıyla yeni bir not defteri oluşturulur ve açılır.


## <a name="run-spark-sql-statements"></a>Spark SQL deyimleri çalıştırma

SQL (Yapılandırılmış Sorgu Dili), veri sorgulama ve tanımlama için en çok kullanılan dildir. Bilinen SQL söz dizimini kullanan Spark SQL, yapısal verileri işleyen bir Apache Spark uzantısı olarak çalışır.

1. Çekirdeğin hazır olduğunu doğrulayın. Not defterinde çekirdek adının yanında boş bir daire görmeniz, çekirdeğin hazır olduğu anlamına gelir. Dolu daire, çekirdeğin meşgul olduğunu belirtir.

    ![HDInsight Spark'ta Hive sorgusu](./media/apache-spark-jupyter-spark-sql/jupyter-spark-kernel-status.png "HDInsight Spark'ta Hive sorgusu")

    Not defterini ilk kez başlattığınızda, çekirdek arka planda birkaç görev gerçekleştirir. Çekirdeğin hazır olmasını bekleyin. 
2. Aşağıdaki kodu boş bir hücreye yapıştırın ve kodu çalıştırmak için **SHIFT + ENTER** tuşlarına basın. Komut, kümedeki Hive tablolarını listeler:

    ```PySpark
    %%sql
    SHOW TABLES
    ```
    HDInsight Spark kümeniz için yapılandırılmış bir Jupyter not defteri kullanırken, Spark SQL ile Hive sorguları çalıştırmak için kullanabileceğiniz önceden ayarlanmış bir `sqlContext` alırsınız. `%%sql`, Hive sorgusunu çalıştırmak için Jupyter Not Defteri’ne `sqlContext` ön ayarını kullanmasını söyler. Sorgu, varsayılan olarak tüm HDInsight kümelerinde sağlanan Hive tablosundaki (**hivesampletable**) ilk 10 satırı getirir. Sonuçları almak 30 saniye kadar sürer. Çıkış aşağıdakine benzer olacaktır: 

    ![HDInsight Spark'ta Hive sorgusu](./media/apache-spark-jupyter-spark-sql/hdinsight-spark-get-started-hive-query.png "HDInsight Spark'ta Hive sorgusu")

    Jupyter’de bir sorguyu her çalıştırdığınızda web tarayıcınızın pencere başlığında not defteri başlığı ile birlikte **(Meşgul)** durumu gösterilir. Ayrıca sağ üst köşedeki **PySpark** metninin yanında içi dolu bir daire görürsünüz.
    
2. `hivesampletable` komutundaki verileri görmek için başka bir sorgu çalıştırın.

    ```PySpark
    %%sql
    SELECT * FROM hivesampletable LIMIT 10
    ```
    
    Sorgu çıkışının görüntülenmesi için ekranın yenilenmesi gerekir.

    ![HDInsight Spark'ta Hive sorgusu çıkışı](./media/apache-spark-jupyter-spark-sql/hdinsight-spark-get-started-hive-query-output.png "HDInsight Spark'ta Hive sorgusu çıkışı")

2. Not defterindeki **Dosya** menüsünden **Kapat ve Durdur**’u seçin. Not defterini kapatmak, küme kaynaklarını serbest bırakır.

## <a name="clean-up-resources"></a>Kaynakları temizleme
HDInsight, verilerinizi Azure Depolama’da veya Azure Data Lake Store’da depolar, böylece kullanılmadığında bir kümeyi güvenle silebilirsiniz. Ayrıca, kullanılmıyorken dahi HDInsight kümesi için sizden ücret kesilir. Küme ücretleri depolama ücretlerinin birkaç katı olduğundan, kullanılmadığında kümelerin silinmesi mantıklı olandır. [Sonraki adımlar](#next-steps) içinde listelenen öğretici üzerinde hemen çalışmayı planlıyorsanız, kümeyi tutmak isteyebilirsiniz.

Azure portalına geri dönüp **Sil**’i seçin.

![HDInsight kümesini silme](./media/apache-spark-jupyter-spark-sql/hdinsight-azure-portal-delete-cluster.png "HDInsight kümesini silme")

Kaynak grubu adını seçerek de kaynak grubu sayfasını açabilir ve sonra **Kaynak grubunu sil**’i seçebilirsiniz. Kaynak grubunu silerek hem HDInsight Spark kümesini hem de varsayılan depolama hesabını silersiniz.

## <a name="next-steps"></a>Sonraki adımlar 

Bu hızlı başlangıçta HDInsight Spark kümesi oluşturmayı ve temel Spark SQL sorgusunu çalıştırmayı öğrendiniz. HDInsight Spark kümesini kullanarak örnek veriler üzerinde etkileşimli sorgular çalıştırma hakkında daha fazla bilgi edinmek için sonraki makaleye ilerleyin.

> [!div class="nextstepaction"]
>[Apache Spark üzerinde etkileşimli sorguları çalıştırma](./apache-spark-load-data-run-query.md)
