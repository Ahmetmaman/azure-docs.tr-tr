---
title: dosya dahil etme
description: dosya dahil etme
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 10/19/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 9c51ce726545d1c64d69c86c36fc69ea43c3b882
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "76279413"
---
[Application Insights ölçümleri hesaplanırken](../articles/azure-functions/functions-monitoring.md#configure-the-aggregator)kaç tane işlev etkinleştirmeleri toplanmış olduğunu belirtir. 

```json
{
    "aggregator": {
        "batchSize": 1000,
        "flushTimeout": "00:00:30"
    }
}
```

|Özellik |Varsayılan  | Açıklama |
|---------|---------|---------| 
|batchSize|1000|Toplanacak en fazla istek sayısı.| 
|flushTimeout|00:00:30|Toplanacak en uzun süre.| 

İşlev etkinleştirmeleri, ilk iki sınıra ulaşıldığında toplanır.
