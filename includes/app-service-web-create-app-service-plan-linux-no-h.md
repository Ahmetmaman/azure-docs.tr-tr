---
title: include dosyası
description: include dosyası
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 10/24/2018
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 1eb35ec5b26c86e227152f14d4d91c0a2f087082
ms.sourcegitcommit: f6050791e910c22bd3c749c6d0f09b1ba8fccf0c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50035505"
---
Cloud Shell’de, kaynak grubunda [`az appservice plan create`](/cli/azure/appservice/plan?view=azure-cli-latest#az-appservice-plan-create) komutuyla bir App Service planı oluşturun.

<!-- [!INCLUDE [app-service-plan](app-service-plan-linux.md)] -->

Aşağıdaki örnek, **Temel** fiyatlandırma katmanı (`--sku B1`) ve bir Linux kapsayıcısı (`--is-linux`) içinde `myAppServicePlan` adlı bir App Service planı oluşturur.

```azurecli-interactive
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku B1 --is-linux
```

App Service planı oluşturulduğunda Azure CLI, aşağıdaki örneğe benzer bilgiler gösterir:

```json
{ 
  "adminSiteName": null,
  "appServicePlanName": "myAppServicePlan",
  "geoRegion": "West Europe",
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/0000-0000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myAppServicePlan",
  "kind": "linux",
  "location": "West Europe",
  "maximumNumberOfWorkers": 1,
  "name": "myAppServicePlan",
  < JSON data removed for brevity. >
  "targetWorkerSizeId": 0,
  "type": "Microsoft.Web/serverfarms",
  "workerTierName": null
} 
```
