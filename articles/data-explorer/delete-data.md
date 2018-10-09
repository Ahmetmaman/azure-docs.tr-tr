---
title: Azure veri Gezgini'nde verileri silme
description: Bu makalede Azure temizleme dahil olmak üzere verileri araştırmak, toplu silme senaryolar ve bağlı bekletme siler.
author: orspod
ms.author: v-orspod
ms.reviewer: mblythe
ms.service: data-explorer
services: data-explorer
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 07ee05128333df963c2d8ff3dd3f6442a3843d07
ms.sourcegitcommit: 0bb8db9fe3369ee90f4a5973a69c26bff43eae00
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2018
ms.locfileid: "48866899"
---
# <a name="delete-data-from-azure-data-explorer"></a>Azure veri Gezgini'nde verileri silme

Azure Veri Gezgini, bu makalede ele birkaç toplu silme yaklaşımları destekler. Hızlı Okuma için iyileştirilmiş olduğundan bunu gerçek zamanlı olarak başına kayıt silme işlemi desteklemiyor.

* Bir veya daha fazla tablo artık gerekli değilse, tablo bırakma kullanarak bunları silin veya tablolar komutunu bırakılamıyor.

    ```Kusto
    .drop table <TableName>

    .drop tables (<TableName1>, <TableName2>,...)
    ```

* Eski veriler artık gerekli değilse, saklama dönemi veritabanı veya tablo düzeyinde değiştirerek silin.

    Bir veritabanı ya da 90 gün boyunca ayarlanır tablo göz önünde bulundurun. İş değiştiğinde, bunu şimdi yalnızca 60 günün verilerini gereklidir. Bu durumda, eski verileri aşağıdaki yollardan birini silin.

    ```Kusto
    .alter-merge database <DatabaseName> policy retention softdelete = 60d

    .alter-merge table <TableName> policy retention softdelete = 60d
    ```

    Daha fazla bilgi için [Bekletme İlkesi](https://docs.microsoft.com/azure/kusto/concepts/retentionpolicy).

Veri silme sorunları ile ilgili yardıma ihtiyacınız varsa, Lütfen bir destek isteği açın [Azure portalında](https://portal.azure.com).
