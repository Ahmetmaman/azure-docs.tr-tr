---
title: SQL veri ambarı'nda gereç sorgular için etiketler kullanarak | Microsoft Docs
description: Çözümleri geliştirmek için Azure SQL veri ambarı'nda gereç sorgular için etiketleri kullanma hakkında ipuçları.
services: sql-data-warehouse
author: ronortloff
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: implement
ms.date: 04/17/2018
ms.author: rortloff
ms.reviewer: igorstan
ms.openlocfilehash: 5c60a7dc44471599834c4b1225b397c8e6eabbd5
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55456468"
---
# <a name="using-labels-to-instrument-queries-in-azure-sql-data-warehouse"></a>Azure SQL veri ambarı'nda gereç sorgular için etiketleri kullanma
Çözümleri geliştirmek için Azure SQL veri ambarı'nda gereç sorgular için etiketleri kullanma hakkında ipuçları.


## <a name="what-are-labels"></a>Etiketleri nelerdir?
SQL veri ambarı, sorgu etiketleri adlı bir kavram destekler. Herhangi bir derinliğe geçmeden önce bir örneğe göz atalım:

```sql
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query Label')
;
```

Son satırı sorgu dizesine 'Sorgu etiketi' etiketleri. Etiket Dmv'ler sorgu-mümkün olduğundan bu etiket, özellikle yararlı olur. Etiketler için sorgulama sorguları bulmak ve çalıştırmak için bir ELT ilerlemeyi tanımlamaya yardımcı olmaya yönelik bir mekanizma sağlar.

İyi bir adlandırma kuralı gerçekten yardımcı olur. Örneğin, etiket proje ile başlayarak, yordam, DEYİM veya YORUM sorgu kaynak denetimindeki tüm kod arasında benzersiz olarak tanımlanabilmesi için yardımcı olur.

Aşağıdaki sorgu, etiketle aramak için bir dinamik yönetim görünümünü kullanır.

```sql
SELECT  *
FROM    sys.dm_pdw_exec_requests r
WHERE   r.[label] = 'My Query Label'
;
```

> [!NOTE]
> Sorgulanırken köşeli parantezler veya word etiket etrafına çift tırnak işareti koymak için gereklidir. Etiket, ayrılmış bir sözcük ve değil sınırlı olduğunda bir hataya neden olur. 
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla geliştirme ipuçları için bkz: [geliştirmeye genel bakış](sql-data-warehouse-overview-develop.md).


