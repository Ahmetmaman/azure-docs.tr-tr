---
title: Apache Hadoop & güvenli aktarım depolama-Azure HDInsight
description: Güvenli aktarım özellikli Azure depolama hesapları ile HDInsight kümeleri oluşturma hakkında bilgi edinin.
ms.service: hdinsight
ms.topic: how-to
ms.custom: hdinsightactive
ms.date: 02/18/2020
ms.openlocfilehash: a02da7237252811d89e2c19a29f49f0bf9bb3804
ms.sourcegitcommit: 2f9f306fa5224595fa5f8ec6af498a0df4de08a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2021
ms.locfileid: "98945729"
---
# <a name="apache-hadoop-clusters-with-secure-transfer-storage-accounts-in-azure-hdinsight"></a>Azure HDInsight 'ta güvenli aktarım depolama hesapları olan kümeler Apache Hadoop

[Güvenli aktarım gereklidir](../storage/common/storage-require-secure-transfer.md) özelliği, güvenli bir bağlantı üzerinden tüm istekleri hesabınıza uygulayarak Azure Depolama hesabınızın güvenliğini artırır. Bu özellik ve wasbs şeması yalnızca HDInsight kümesi 3.6 veya sonraki sürümlerde desteklenir.

> [!IMPORTANT]
> Bir küme oluşturduktan sonra güvenli depolama aktarımını etkinleştirmek, depolama hesabınızı kullanırken hatalara neden olabilir ve önerilmez. Güvenli aktarım özelliği zaten etkin olan bir depolama hesabı kullanılarak yeni bir küme oluşturulması daha iyidir.

## <a name="storage-accounts"></a>Depolama hesapları

### <a name="azure-portal"></a>Azure portalı

Varsayılan olarak, Azure portal ' de bir depolama hesabı oluşturduğunuzda güvenli aktarım gerekli özelliği etkinleştirilir.

Mevcut bir depolama hesabını Azure portal güncelleştirmek için bkz. [Azure Portal ile güvenli aktarım gerektir](../storage/common/storage-require-secure-transfer.md#require-secure-transfer-for-an-existing-storage-account).

### <a name="powershell"></a>PowerShell

[New-AzStorageAccount](/powershell/module/az.storage/new-azstorageaccount)PowerShell cmdlet 'i için parametrenin `-EnableHttpsTrafficOnly` olarak ayarlandığından emin olun `1` .

Var olan bir depolama hesabını PowerShell ile güncelleştirmek için bkz. [PowerShell ile güvenli aktarım gerektir](../storage/common/storage-require-secure-transfer.md#require-secure-transfer-with-powershell).

### <a name="azure-cli"></a>Azure CLI’si

Azure CLı komutu [az Storage Account Create](/cli/azure/storage/account#az-storage-account-create)parametresi için parametresinin `--https-only` olarak ayarlandığından emin olun `true` .

Azure CLı ile mevcut bir depolama hesabını güncelleştirmek için bkz. [Azure CLI ile güvenli aktarım gerektir](../storage/common/storage-require-secure-transfer.md#require-secure-transfer-with-azure-cli).

## <a name="add-additional-storage-accounts"></a>Başka depolama hesapları ekleme

Güvenli aktarım özellikli başka depolama hesapları eklemek için birkaç seçenek vardır:

* Son bölümdeki Azure Resource Manager şablonunu değiştirin.
* [Azure portalını](https://portal.azure.com) kullanarak bir küme oluşturun ve bağlı depolama hesabını belirtin.
* Var olan bir HDInsight kümesine güvenli aktarım özellikli başka depolama hesapları eklemek için betik eylemini kullanın. Daha fazla bilgi için bkz. [HDInsight’a başka depolama hesapları ekleme](hdinsight-hadoop-add-storage.md).

## <a name="next-steps"></a>Sonraki adımlar

* Varsayılan veri deposu olarak [Apache Hadoop](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsUserGuide.html) m yerine Azure Storage (te) kullanımı
* HDInsight’ın Azure Depolama’yı nasıl kullandığı hakkında daha fazla bilgi için bkz. [HDInsight ile Azure Depolama kullanma](hdinsight-hadoop-use-blob-storage.md).
* HDInsight’a veril yükleme hakkında daha fazla bilgi için bkz. [Verileri HDInsight’a yükleme](hdinsight-upload-data.md).