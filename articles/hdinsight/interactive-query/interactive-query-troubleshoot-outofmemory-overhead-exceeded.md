---
title: Apache Hive birleştirmelerde, OutOfMemory hatası-Azure HDInsight
description: OutOfMemory hatalarıyla ilgilenme "GC ek limiti aşıldı hatası"
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 07/30/2019
ms.openlocfilehash: 0c396cde38d8cba8e1f3eaf8527429647868a0c8
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "98935962"
---
# <a name="scenario-joins-in-apache-hive-leads-to-an-outofmemory-error-in-azure-hdinsight"></a>Senaryo: Apache Hive birleşimler Azure HDInsight 'ta bir OutOfMemory hatasına yol açar

Bu makalede, Azure HDInsight kümelerinde etkileşimli sorgu bileşenlerini kullanırken sorunlar için sorun giderme adımları ve olası çözümleri açıklanmaktadır.

## <a name="issue"></a>Sorun

Apache Hive birleştirmeleri için varsayılan davranış, bir tablonun tüm içeriğini belleğe yüklemek ve böylece bir harita/azaltma adımını gerçekleştirmeye gerek kalmadan bir birleştirmenin gerçekleştirilmesini sağlar. Hive tablosu belleğe sığmayacak kadar büyükse sorgu başarısız olabilir.

## <a name="cause"></a>Nedeni

Birleşimler yeterli büyüklükte Hive üzerinde çalışırken aşağıdaki hatayla karşılaşıldı:

```
Caused by: java.lang.OutOfMemoryError: GC overhead limit exceeded error.
```

## <a name="resolution"></a>Çözüm

Hive 'in, aşağıdaki Hive yapılandırma değerini ayarlayarak, birleştirmelerde tabloları (bir harita/azaltma adımını gerçekleştirmek yerine) belleğe yüklemesini önleyin:

```
hive.auto.convert.join=false
```

## <a name="next-steps"></a>Sonraki adımlar

Bu değeri ayarlamak sorununuzu gidermezse, aşağıdakilerden birini ziyaret edin...

* Azure [topluluk desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıt alın.

* [@AzureSupport](https://twitter.com/azuresupport)Azure Community 'yi doğru kaynaklara bağlayarak müşteri deneyimini iyileştirmeye yönelik resmi Microsoft Azure hesabı ile bağlanın: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **destek** ' i seçin veya **Yardım + Destek** hub 'ını açın. Daha ayrıntılı bilgi için lütfen [Azure destek isteği oluşturma](../../azure-portal/supportability/how-to-create-azure-support-request.md)konusunu inceleyin. Abonelik yönetimi ve faturalandırma desteği 'ne erişim Microsoft Azure aboneliğinize dahildir ve [Azure destek planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla teknik destek sağlanır.