---
title: Azure Batch havuz silme başlangıç olayı | Microsoft Docs
description: Başvuru için Batch havuz silme başlangıç olayı.
services: batch
author: laurenhughes
manager: jeconnoc
ms.assetid: ''
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 04/20/2017
ms.author: lahugh
ms.openlocfilehash: 2352971af3844b56f93c16ebaf6cb23bd5fd8a5a
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55474301"
---
# <a name="pool-delete-start-event"></a>Havuz silme başlangıç olayı

 Havuz silme işlemi başlatıldığında bu olay yayılır. Havuz silme zaman uyumsuz bir olay olduğundan, silme işlemi tamamlandıktan sonra derleyicisindeki bir havuz silme tamamlama olayı bekleyebilirsiniz.

 Aşağıdaki örnek, bir havuz silme başlangıç olayı gövdesi gösterir.

```
{
    "id": "myPool1"
}
```

|Öğe|Type|Notlar|
|-------------|----------|-----------|
|id|String|Havuz kimliği.|