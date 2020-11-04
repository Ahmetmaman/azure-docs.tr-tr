---
title: Azure Cosmos DB sorgu dilinde StringToNumber
description: Azure Cosmos DB SQL sistem işlevi StringToNumber 'ı hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: f2622958a2315458ccd01da4aea7c6b75941e755
ms.sourcegitcommit: fa90cd55e341c8201e3789df4cd8bd6fe7c809a3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2020
ms.locfileid: "93339572"
---
# <a name="stringtonumber-azure-cosmos-db"></a>StringToNumber (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

 Sayıya çevrilmiş bir ifade döndürür. İfade çevrilemez, tanımsız döndürür.  
  
## <a name="syntax"></a>Söz dizimi
  
```sql
StringToNumber(<str_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız değişkenler
  
*str_expr*  
   , JSON sayı ifadesi olarak ayrıştırılacak bir dize ifadesidir. JSON 'daki Sayılar tamsayı veya kayan nokta olmalıdır. JSON biçimi hakkında daha fazla bilgi için bkz. [JSON.org](https://json.org/)  
  
## <a name="return-types"></a>Dönüş türleri
  
  Bir sayı ifadesi veya tanımsız döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, `StringToNumber` farklı türlerde nasıl davrandığını gösterir. 

Boşluğa yalnızca sayıdan önce veya sonra izin verilir.

```sql
SELECT 
    StringToNumber("1.000000") AS num1, 
    StringToNumber("3.14") AS num2,
    StringToNumber("   60   ") AS num3, 
    StringToNumber("-1.79769e+308") AS num4
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
{{"num1": 1, "num2": 3.14, "num3": 60, "num4": -1.79769e+308}}
```  

JSON içinde geçerli bir sayı ya bir tamsayı ya da kayan nokta numarası olmalıdır.

```sql
SELECT   
    StringToNumber("0xF")
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
{{}}
```  

Geçirilen ifade sayı ifadesi olarak ayrıştırılacak; Bu girişler tür numarası olarak değerlendirilmez ve tanımsız döndürür. 

```sql
SELECT 
    StringToNumber("99     54"),   
    StringToNumber(undefined),
    StringToNumber("false"),
    StringToNumber(false),
    StringToNumber(" "),
    StringToNumber(NaN)
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
{{}}
```  

## <a name="remarks"></a>Açıklamalar

Bu sistem işlevi dizinden yararlanmayacak.

## <a name="next-steps"></a>Sonraki adımlar

- [Dize işlevleri Azure Cosmos DB](sql-query-string-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB'ye giriş](introduction.md)
