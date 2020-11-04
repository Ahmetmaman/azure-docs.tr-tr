---
title: Azure Cosmos DB diziler ve nesnelerle çalışma
description: Azure Cosmos DB diziler ve nesneler oluşturmak için SQL sözdizimini öğrenin. Bu makalede, dizi nesnelerinde işlemler gerçekleştirmek için bazı örnekler de sağlanmaktadır
author: timsander1
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 12/02/2019
ms.author: tisande
ms.openlocfilehash: 80d158e209943d65b9934d5425ccce7d69422bc4
ms.sourcegitcommit: fa90cd55e341c8201e3789df4cd8bd6fe7c809a3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2020
ms.locfileid: "93338375"
---
# <a name="working-with-arrays-and-objects-in-azure-cosmos-db"></a>Azure Cosmos DB diziler ve nesnelerle çalışma
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

Azure Cosmos DB SQL API 'sinin temel bir özelliği dizi ve nesne oluşturma 'dır.

## <a name="arrays"></a>Diziler

Aşağıdaki örnekte gösterildiği gibi diziler oluşturabilirsiniz:

```sql
SELECT [f.address.city, f.address.state] AS CityState
FROM Families f
```

Sonuçlar:

```json
[
  {
    "CityState": [
      "Seattle",
      "WA"
    ]
  },
  {
    "CityState": [
      "NY", 
      "NY"
    ]
  }
]
```

[Alt sorgu](sql-query-subquery.md) sonuçlarından bir dizi oluşturmak için [dizi ifadesini](sql-query-subquery.md#array-expression) de kullanabilirsiniz. Bu sorgu, bir dizideki tüm alt öğelerin verilen benzersiz adlarını alır.

```sql
SELECT f.id, ARRAY(SELECT DISTINCT VALUE c.givenName FROM c IN f.children) as ChildNames
FROM f
```

## <a name="iteration"></a><a id="Iteration"></a>Mesinde

SQL API 'si, FROM kaynağında [ın anahtar sözcüğüyle](sql-query-keywords.md#in) eklenen yeni bir yapı ile JSON dizileri üzerinde yineleme desteği sağlar. Aşağıdaki örnekte:

```sql
SELECT *
FROM Families.children
```

Sonuçlar:

```json
[
  [
    {
      "firstName": "Henriette Thaulow",
      "gender": "female",
      "grade": 5,
      "pets": [{ "givenName": "Fluffy"}]
    }
  ], 
  [
    {
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female",
        "grade": 1
    }, 
    {
        "familyName": "Miller",
        "givenName": "Lisa",
        "gender": "female",
        "grade": 8
    }
  ]
]
```

Sonraki sorgu, kapsayıcıda yineleme gerçekleştirir `children` `Families` . Çıkış dizisi, önceki sorgudan farklı. Bu örnek `children` , sonuçları böler ve sonuçları tek bir diziye düzleştirir:  

```sql
SELECT *
FROM c IN Families.children
```

Sonuçlar:

```json
[
  {
      "firstName": "Henriette Thaulow",
      "gender": "female",
      "grade": 5,
      "pets": [{ "givenName": "Fluffy" }]
  },
  {
      "familyName": "Merriam",
      "givenName": "Jesse",
      "gender": "female",
      "grade": 1
  },
  {
      "familyName": "Miller",
      "givenName": "Lisa",
      "gender": "female",
      "grade": 8
  }
]
```

Aşağıdaki örnekte gösterildiği gibi, her bir dizi girişi üzerinde daha fazla filtre uygulayabilirsiniz:

```sql
SELECT c.givenName
FROM c IN Families.children
WHERE c.grade = 8
```

Sonuçlar:

```json
[{
  "givenName": "Lisa"
}]
```

Ayrıca, bir dizi yinelemesinin sonucunu toplayabilirsiniz. Örneğin, aşağıdaki sorgu tüm aileler arasındaki alt öğe sayısını sayar:

```sql
SELECT COUNT(child)
FROM child IN Families.children
```

Sonuçlar:

```json
[
  {
    "$1": 3
  }
]
```

## <a name="next-steps"></a>Sonraki adımlar

- [Başlarken](sql-query-getting-started.md)
- [Azure Cosmos DB .NET örnekleri](https://github.com/Azure/azure-cosmos-dotnet-v3)
- [Birleştirme](sql-query-join.md)
