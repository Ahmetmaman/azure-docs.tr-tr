---
title: include dosyası
description: include dosyası
services: functions
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/27/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 1b553cbd720fcb76899844712ce5053af46f7ccb
ms.sourcegitcommit: f31bfb398430ed7d66a85c7ca1f1cc9943656678
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/28/2018
ms.locfileid: "47452964"
---
## <a name="deploy-the-function-app-project-to-azure"></a>İşlev uygulaması projesini Azure'a dağıtma

İşlev uygulaması Azure'da oluşturulduktan sonra proje kodunuzu Azure'a dağıtmak için [`func azure functionapp publish`](../articles/azure-functions/functions-run-local.md#project-file-deployment) komutunu kullanabilirsiniz.

```bash
func azure functionapp publish <FunctionAppName>
```

Kolay okunması için kırpılmış olan aşağıdaki çıktıya benzer bir şey görürsünüz.

```output
Getting site publishing info...

...

Preparing archive...
Uploading content...
Upload completed successfully.
Deployment completed successfully.
Syncing triggers...
```

Artık işlevlerinizi Azure'da test edebilirsiniz.