---
title: Azure SYNAPSE Analytics 'te SYNAPSE SQL havuzu geliştirme kaynakları
description: Azure SYNAPSE Analytics için geliştirme kavramları, tasarım kararları, öneriler ve kodlama teknikleri.
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
ms.date: 08/29/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.openlocfilehash: 95f712f196c37650b52220c9e34f6cb6b50bff23
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "89460618"
---
# <a name="design-decisions-and-coding-techniques-for-a-synapse-sql-pool-in-azure-synapse-analytics"></a>Azure SYNAPSE Analytics 'te SYNAPSE SQL havuzu için tasarım kararları ve kodlama teknikleri 
 Bu makalede, Azure SYNAPSE 'deki bir SQL havuzu için önemli tasarım kararlarını, önerileri ve kodlama tekniklerini daha iyi anlamanıza yardımcı olacak ek kaynaklar bulacaksınız.

## <a name="key-design-decisions"></a>Anahtar tasarım kararları
Aşağıdaki makalelerde, Azure SYNAPSE 'de SQL havuzu özelliğini kullanarak dağıtılmış bir veri ambarı geliştirmeye yönelik kavramlar ve tasarım kararları vurgulanacak:

* [bağlantının](../sql/connect-overview.md)
* [zamanlı](resource-classes-for-workload-management.md)
* [hareket](sql-data-warehouse-develop-transactions.md)
* [Kullanıcı tanımlı şemalar](sql-data-warehouse-develop-user-defined-schemas.md)
* [Tablo dağıtımı](sql-data-warehouse-tables-distribute.md)
* [tablo dizinleri](sql-data-warehouse-tables-index.md)
* [Tablo bölümleri](sql-data-warehouse-tables-partition.md)
* [CTAS](sql-data-warehouse-develop-ctas.md)
* [girecek](sql-data-warehouse-tables-statistics.md)

## <a name="development-recommendations-and-coding-techniques"></a>Geliştirme önerileri ve kodlama teknikleri
Aşağıdaki makalelerde, bir SQL havuzu geliştirmeye yönelik belirli kodlama teknikleri, ipuçları ve öneriler verilmektedir:

* [saklı yordamlar](sql-data-warehouse-develop-stored-procedures.md)
* [etikete](sql-data-warehouse-develop-label.md)
* [Görünümler](performance-tuning-materialized-views.md)
* [geçici tablolar](sql-data-warehouse-tables-temporary.md)
* [dinamik SQL](sql-data-warehouse-develop-dynamic-sql.md)
* [döngüye](sql-data-warehouse-develop-loops.md)
* [seçeneklere göre Gruplandır](sql-data-warehouse-develop-group-by-options.md)
* [değişken ataması](sql-data-warehouse-develop-variable-assignment.md)

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla başvuru bilgisi için bkz. [T-SQL deyimleri](sql-data-warehouse-reference-tsql-statements.md).
