---
title: Azure Cosmos DB sorgu dilinde matematik işlevleri
description: Bağımsız değişkenler olarak sunulan giriş değerlerine göre bir hesaplama gerçekleştirmek için Azure Cosmos DB matematik işlevleri hakkında bilgi edinin ve sayısal bir değer döndürün.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: bd53feb175c5be77f559a4d2e724a55e41df48eb
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "85562824"
---
# <a name="mathematical-functions-azure-cosmos-db"></a>Matematik işlevleri (Azure Cosmos DB)  

Matematik işlevleri, bağımsız değişken olarak sunulan giriş değerlerine göre bir hesaplama gerçekleştirir ve sayısal bir değer döndürür.

Aşağıdaki örnekte olduğu gibi sorgular çalıştırabilirsiniz:

```sql
    SELECT VALUE ABS(-4)
```

Sonuç:

```json
    [4]
```

## <a name="functions"></a>İşlevler

Aşağıdaki desteklenen yerleşik matematik işlevleri, genellikle giriş bağımsız değişkenlerine dayalı olarak bir hesaplama gerçekleştirir ve sayısal bir ifade döndürür:
 
* [ABS](sql-query-abs.md)
* [ACOS](sql-query-acos.md)
* [ASIN](sql-query-asin.md)
* [ATAN](sql-query-atan.md)
* [ATN2](sql-query-atn2.md)
* [CEILING](sql-query-ceiling.md)
* [COS](sql-query-cos.md)
* [COT](sql-query-cot.md)
* [DEGREES](sql-query-degrees.md)
* [EXP](sql-query-exp.md)
* [FLOOR](sql-query-floor.md)
* [LOG](sql-query-log.md)
* [LOG10](sql-query-log10.md)
* [PI](sql-query-pi.md)
* [POWER](sql-query-power.md)
* [RADIANS](sql-query-radians.md)
* [RAND](sql-query-rand.md)
* [ROUND](sql-query-round.md)
* [SIGN](sql-query-sign.md)
* [SIN](sql-query-sin.md)
* [SQRT](sql-query-sqrt.md)
* [SQUARE](sql-query-square.md)
* [TAN](sql-query-tan.md)
* [TRUNC](sql-query-trunc.md)

  
S_SAYI_ÜRET hariç tüm matematik işlevleri belirleyici işlevlerdir. Bu, belirli bir giriş değerleri kümesiyle her çağrıldıklarında aynı sonuçların döndürülmeyeceği anlamına gelir.

## <a name="next-steps"></a>Sonraki adımlar

- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB'ye giriş](introduction.md)
- [Kullanıcı tanımlı Işlevler](sql-query-udfs.md)
- [Toplamlar](sql-query-aggregates.md)
