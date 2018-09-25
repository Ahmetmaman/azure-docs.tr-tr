---
title: Azure Klasik CLI - Azure HDInsight'ı kullanarak Hadoop kümelerini yönetme
description: Azure HDInsight Hadoop kümelerini yönetmek için Azure Klasik CLI'yı kullanmayı öğrenin.
services: hdinsight
ms.reviewer: jasonh
author: jasonwhowell
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 05/14/2018
ms.author: jasonh
ms.openlocfilehash: 2586b9219eb145b2033fe2d8fc64b8ae72f34eda
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46958297"
---
# <a name="manage-hadoop-clusters-in-hdinsight-using-the-azure-classic-cli"></a>Klasik Azure CLI kullanarak bir HDInsight Hadoop kümelerini yönetme
[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Nasıl kullanacağınızı öğrenin [Klasik Azure CLI'yı](../cli-install-nodejs.md) Azure HDInsight Hadoop kümelerini yönetmek için. Klasik CLI, Node.js içinde uygulanmıştır. Windows, Mac ve Linux da dahil olmak üzere, Node.js'yi destekleyen herhangi bir platformda kullanılabilir.

[!INCLUDE [classic-cli-warning](../../includes/requires-classic-cli.md)]

## <a name="prerequisites"></a>Önkoşullar
Bu makaleye başlamadan önce aşağıdakilere sahip olmanız ve aşağıdaki işlemleri yapmış olmanız gerekir:

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Azure Klasik CLI** -bkz [yüklemek ve Azure Klasik CLI yapılandırma](../cli-install-nodejs.md) yükleme ve yapılandırma bilgileri için.
* **Azure'a bağlanma**, aşağıdaki komutu kullanarak:

    ```cli
    azure login
    ```
  
    Bir iş veya Okul hesabını kullanarak kimlik doğrulaması ile ilgili daha fazla bilgi için bkz: [Klasik Azure clı'dan Azure aboneliğine Bağlan](/cli/azure/authenticate-azure-cli).
* Şu komutu kullanarak **Azure Resource Manager moduna geçin**:
  
    ```cli
    azure config mode arm
    ```

Yardım almak için kullanın **-h** geçin.  Örneğin:

```cli
azure hdinsight cluster create -h
```

## <a name="create-clusters-with-the-cli"></a>CLI ile küme oluşturma
Bkz: [Klasik Azure CLI kullanarak HDInsight kümeleri oluşturma](hdinsight-hadoop-create-linux-clusters-azure-cli.md).

## <a name="list-and-show-cluster-details"></a>Küme ayrıntıları Listele ve Göster
Liste ve küme ayrıntıları göstermek için aşağıdaki komutları kullanın:

```cli
azure hdinsight cluster list
azure hdinsight cluster show <Cluster Name>
```

![Komut satırı görünümünü küme listesi][image-cli-clusterlisting]

## <a name="delete-clusters"></a>Kümeleri Sil
Bir kümeyi silmek için aşağıdaki komutu kullanın:

```cli
azure hdinsight cluster delete <Cluster Name>
```

Kümeyi içeren kaynak grubunu silerek bir küme de silebilirsiniz. Lütfen unutmayın, bu varsayılan depolama hesabı dahil olmak üzere grubundaki tüm kaynakları siler.

```cli
azure group delete <Resource Group Name>
```

## <a name="scale-clusters"></a>Kümeleri ölçeklendirme
Hadoop kümenizin boyutunu değiştirmek için:

```cli
azure hdinsight cluster resize [options] <clusterName> <Target Instance Count>
```


## <a name="enabledisable-http-access-for-a-cluster"></a>Bir küme için HTTP erişimini etkinleştir/devre dışı bırak

```cli
azure hdinsight cluster enable-http-access [options] <Cluster Name> <userName> <password>
azure hdinsight cluster disable-http-access [options] <Cluster Name>
```

## <a name="enabledisable-rdp-access-for-a-cluster"></a>Bir kümenin RDP erişimini etkinleştir/devre dışı bırak

```cli
azure hdinsight cluster enable-rdp-access [options] <Cluster Name> <rdpUserName> <rdpPassword> <rdpExpiryDate>
azure hdinsight cluster disable-rdp-access [options] <Cluster Name>
```

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, farklı HDInsight küme yönetim görevlerinin nasıl gerçekleştirileceğini öğrendiniz. Daha fazla bilgi için aşağıdaki makalelere bakın:

* [HDInsight Azure portalını kullanarak yönetme][hdinsight-admin-portal]
* [HDInsight, Azure PowerShell kullanarak yönetme][hdinsight-admin-powershell]
* [Azure HDInsight'ı Kullanmaya Başlama][hdinsight-get-started]
* [Klasik Azure CLI kullanma][azure-command-line-tools]

[azure-command-line-tools]: ../cli-install-nodejs.md
[azure-create-storageaccount]:../storage/common/storage-create-storage-account.md
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/


[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-get-started]:hadoop/apache-hadoop-linux-tutorial-get-started.md

[image-cli-account-download-import]: ./media/hdinsight-administer-use-command-line/HDI.CLIAccountDownloadImport.png
[image-cli-clustercreation]: ./media/hdinsight-administer-use-command-line/HDI.CLIClusterCreation.png
[image-cli-clustercreation-config]: ./media/hdinsight-administer-use-command-line/HDI.CLIClusterCreationConfig.png
[image-cli-clusterlisting]: ./media/hdinsight-administer-use-command-line/command-line-list-of-clusters.png "Kümeleri Listele ve Göster"
