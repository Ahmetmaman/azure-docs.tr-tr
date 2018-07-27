---
title: PowerShell - Azure HDInsight'ı kullanarak Hadoop kümeleri oluşturma | Microsoft Docs
description: HDInsight için Linux üzerinde Azure PowerShell kullanarak Hadoop, HBase, Storm veya Spark kümeleri oluşturmayı öğrenin.
services: hdinsight
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 4208deca-d64a-45e1-8948-2673d5d7678c
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 02/21/2018
ms.author: nitinme
ms.openlocfilehash: 57ae7c3cab6c0084bb43585f65d79ae6cc265356
ms.sourcegitcommit: a5eb246d79a462519775a9705ebf562f0444e4ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2018
ms.locfileid: "39263735"
---
# <a name="create-linux-based-clusters-in-hdinsight-using-azure-powershell"></a>Linux tabanlı kümeler Azure PowerShell kullanarak HDInsight oluşturma

[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

Azure PowerShell, denetlemek ve dağıtım ve Microsoft azure'da iş yüklerinizi yönetimini otomatikleştirmek için kullanabileceğiniz güçlü bir komut dosyası ortamıdır. Bu belge, Azure PowerShell kullanarak bir Linux tabanlı HDInsight kümesi oluşturma hakkında bilgi sağlar. Ayrıca, bir örnek betiği içerir.

> [!NOTE]
> Azure PowerShell, Windows istemcilerinde yalnızca kullanılabilir. Linux, Unix ya da Mac OS X istemci kullanıyorsanız bkz [Azure CLI kullanarak Linux tabanlı HDInsight kümesi oluşturmayı](hdinsight-hadoop-create-linux-clusters-azure-cli.md) bir küme oluşturmak için Azure CLI kullanma hakkında bilgi.

## <a name="prerequisites"></a>Önkoşullar
Bu yordamı başlatmadan önce aşağıdakilerin olması gerekir:

* Azure aboneliği. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* [Azure PowerShell](/powershell/azure/install-azurerm-ps)

    > [!IMPORTANT]
    > Azure Service Manager kullanılarak HDInsight kaynaklarının yönetilmesi için Azure PowerShell desteği **kullanım dışı bırakılmış** ve 1 Ocak 2017 tarihinde kaldırılmıştır. Bu belgede yer alan adımlar, Azure Resource Manager ile çalışan yeni HDInsight cmdlet'lerini kullanır.
    >
    > Lütfen adımları [Azure PowerShell yükleme](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) Azure PowerShell'in en son sürümünü yüklemek için. Azure Resource Manager’la çalışan yeni cmdlet’lerle kullanmak için değiştirilmesi gereken komut dosyalarınız varsa, daha fazla bilgi için bkz. [HDInsight kümeleri için Azure Resource Manager tabanlı geliştirme araçlarına geçme](hdinsight-hadoop-development-using-azure-resource-manager.md).

## <a name="create-cluster"></a>Küme oluşturma

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Azure PowerShell kullanarak bir HDInsight kümesi oluşturmak için aşağıdaki yordamları tamamlamanız gerekir:

* Bir Azure kaynak grubu oluşturun
* Azure Depolama hesabı oluşturma
* Bir Azure Blob kapsayıcısı oluşturma
* HDInsight kümesi oluşturma

Aşağıdaki komut dosyasını yeni bir küme oluşturma işlemini göstermektedir:

[!code-powershell[main](../../powershell_scripts/hdinsight/create-cluster/create-cluster.ps1?range=5-71)]

Küme oturum açma için belirlediğiniz değerleri kümesi için Hadoop kullanıcı hesabı oluşturmak için kullanılır. Web kullanıcı arabirimleri veya REST API'leri gibi kümesi üzerinde barındırılan hizmetlere bağlanmak için bu hesabı kullanın.

SSH kullanıcısı için belirlediğiniz değerler, küme için SSH kullanıcı oluşturmak için kullanılır. Küme üzerinde uzak bir SSH oturumu başlatın ve işleri çalıştırmak için bu hesabı kullanın. Daha fazla bilgi için [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md) belgesine bakın.

> [!IMPORTANT]
> 32'den fazla çalışan düğümleri (veya küme oluşturma sırasında oluşturulduktan sonra küme ölçeklendirme) kullanmayı planlıyorsanız, bir baş düğüm boyutu en az 8 çekirdek ve 14 GB RAM ile de belirtmeniz gerekir.
>
> Düğüm boyutları ve ilişkili maliyetler hakkında daha fazla bilgi için bkz. [HDInsight fiyatlandırması](https://azure.microsoft.com/pricing/details/hdinsight/).

Uygulamanın bir küme oluşturmak için en fazla 20 dakika sürebilir.

## <a name="create-cluster-configuration-object"></a>Küme oluşturma: Yapılandırma nesnesi

Bir HDInsight yapılandırma nesnesi kullanarak da oluşturabilirsiniz `New-AzureRmHDInsightClusterConfig` cmdlet'i. Kümenizin ek yapılandırma seçenekleri etkinleştirmek için bu yapılandırma nesnesi daha sonra değiştirebilirsiniz. Son olarak, `-Config` parametresinin `New-AzureRmHDInsightCluster` yapılandırmasını kullanmak için cmdlet.

Aşağıdaki betiği HDInsight küme türü üzerinde bir R Server'ı yapılandırmak için bir yapılandırma nesnesi oluşturur. Kenar düğümünde RStudio ve ek bir depolama hesabı yapılandırmasını sağlar.

[!code-powershell[main](../../powershell_scripts/hdinsight/create-cluster/create-cluster-with-config.ps1?range=59-98)]

> [!WARNING]
> HDInsight kümesinden farklı bir konumda bir depolama hesabının kullanılması desteklenmez. Bu örnek kullanırken, sunucu ile aynı konumda ek depolama hesabı oluşturun.

## <a name="customize-clusters"></a>Kümeleri özelleştirme

* Bkz: [özelleştirme HDInsight kümeleri Bootstrap ile](hdinsight-hadoop-customize-cluster-bootstrap.md#use-azure-powershell).
* Bkz: [özelleştirme HDInsight kümelerini betik eylemi kullanarak](hdinsight-hadoop-customize-cluster-linux.md).

## <a name="delete-the-cluster"></a>Küme silme

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="troubleshoot"></a>Sorun giderme

HDInsight kümeleri oluştururken sorun yaşarsanız bkz. [erişim denetimi gereksinimleri](hdinsight-administer-use-portal-linux.md#create-clusters).

## <a name="next-steps"></a>Sonraki adımlar

Bir HDInsight kümesi başarıyla oluşturdunuz, bir küme ile çalışmanıza öğrenmek için aşağıdaki kaynakları kullanın.

### <a name="hadoop-clusters"></a>Hadoop kümeleri

* [HDInsight ile Hive kullanma](hadoop/hdinsight-use-hive.md)
* [HDInsight ile Pig kullanma](hadoop/hdinsight-use-pig.md)
* [HDInsight ile MapReduce kullanma](hadoop/hdinsight-use-mapreduce.md)

### <a name="hbase-clusters"></a>HBase kümeleri

* [HDInsight üzerinde HBase kullanmaya başlama](hbase/apache-hbase-tutorial-get-started-linux.md)
* [HDInsight üzerinde HBase için Java uygulamaları geliştirin](hbase/apache-hbase-build-java-maven-linux.md)

### <a name="storm-clusters"></a>Storm kümeleri

* [HDInsight üzerinde Storm için Java topolojileri geliştirme](storm/apache-storm-develop-java-topology.md)
* [HDInsight üzerinde Storm Python bileşenlerini kullanın](storm/apache-storm-develop-python-topology.md)
* [HDInsight üzerinde Storm topolojileri dağıtma ve izleme](storm/apache-storm-deploy-monitor-topology-linux.md)

### <a name="spark-clusters"></a>Spark kümeleri

* [Scala kullanarak tek başına uygulama oluşturma](spark/apache-spark-create-standalone-application.md)
* [Livy kullanarak Spark kümesinde işleri uzaktan çalıştırma](spark/apache-spark-livy-rest-interface.md)
* [BI ile Spark: BI araçlarıyla HDInsight’ta Spark kullanarak etkileşimli veri çözümlemesi gerçekleştirme](spark/apache-spark-use-bi-tools.md)
* [Machine Learning ile Spark: Yemek inceleme sonuçlarını tahmin etmek için HDInsight’ta Spark kullanma](spark/apache-spark-machine-learning-mllib-ipython.md)

