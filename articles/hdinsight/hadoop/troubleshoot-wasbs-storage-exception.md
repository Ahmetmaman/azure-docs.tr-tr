---
title: Erişilmekte olan hesap Azure HDInsight 'ta http hatasını desteklemiyor
description: Bu makalede, Azure HDInsight kümeleriyle etkileşim kurarken sorun giderme adımları ve olası çözümleri açıklanmaktadır.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 02/06/2020
ms.openlocfilehash: b7f3a3b76169b99389fe8222177ddcb713c27713
ms.sourcegitcommit: d767156543e16e816fc8a0c3777f033d649ffd3c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2020
ms.locfileid: "92546594"
---
# <a name="the-account-being-accessed-does-not-support-http-error-in-azure-hdinsight"></a>Erişilmekte olan hesap Azure HDInsight 'ta http hatasını desteklemiyor

Bu makalede, Azure HDInsight kümeleriyle etkileşim kurarken sorun giderme adımları ve olası çözümleri açıklanmaktadır.

## <a name="issue"></a>Sorun

Aşağıdaki hata iletisi alındı:

```
com.microsoft.azure.storage.StorageException: The account being accessed does not support http.
```

## <a name="cause"></a>Nedeni

Hata iletisinin alınmasının birden çok nedeni vardır:

* Depolama hesabının [güvenli aktarımı](../../storage/common/storage-require-secure-transfer.md) etkin ve yanlış [URI şeması](../hdinsight-hadoop-linux-information.md#URI-and-scheme) kullanılıyor.

* Güvenli aktarım *devre dışı* olan bir depolama hesabıyla oluşturulmuş bir küme oluşturuldu. Bundan sonra, depolama hesabında güvenli aktarım etkinleştirildi.

## <a name="resolution"></a>Çözüm

Azure depolama veya Data Lake Storage 2. için güvenli aktarım etkinse, URI, `wasbs://` sırasıyla veya olur `abfss://` .  Ayrıca bkz. [Güvenli aktarım](../../storage/common/storage-require-secure-transfer.md).

Yeni kümeler için, istenen güvenli aktarım ayarına zaten sahip olan bir depolama hesabı kullanın. Var olan bir küme tarafından kullanılan bir depolama hesabı için güvenli aktarım ayarını değiştirmeyin.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmüyorsanız veya sorununuzu çözemediyseniz, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* Azure [topluluk desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıt alın.

* [@AzureSupport](https://twitter.com/azuresupport)Müşteri deneyimini iyileştirmek için resmi Microsoft Azure hesabına bağlanın. Azure Community 'yi doğru kaynaklara bağlama: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **destek** ' i seçin veya **Yardım + Destek** hub 'ını açın. Daha ayrıntılı bilgi için [Azure destek isteği oluşturma](../../azure-portal/supportability/how-to-create-azure-support-request.md)konusunu inceleyin. Abonelik yönetimi ve faturalandırma desteği 'ne erişim Microsoft Azure aboneliğinize dahildir ve [Azure destek planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla teknik destek sağlanır.