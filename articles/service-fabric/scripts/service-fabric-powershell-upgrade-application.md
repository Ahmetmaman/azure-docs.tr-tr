---
title: PowerShell 'de bir Service Fabric uygulamasını yükseltme
description: Azure PowerShell betik örneği-PowerShell kullanarak bir Azure Service Fabric uygulamasını yükseltme ve izleme.
services: service-fabric
documentationcenter: ''
author: athinanthny
manager: chackdan
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: service-fabric
ms.workload: multiple
ms.topic: sample
ms.date: 01/18/2018
ms.author: atsenthi
ms.custom: mvc
ms.openlocfilehash: 1b04a0c4d2865cf72d9fbccb51f0f083872ea799
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "87037891"
---
# <a name="upgrade-a-service-fabric-application"></a>Service Fabric uygulamasını yükseltme

Bu örnek betik, çalışan bir Service Fabric uygulama örneğini 1.3.0 sürümüne yükseltir. Betik yeni uygulama paketini küme görüntü deposuna kopyalar, uygulama türünü kaydeder ve gereksiz uygulama paketini kaldırır.  Betik izlenen bir yükseltme başlatır ve yükseltme tamamlanana veya geri alınana kadar yükseltme durumunu sürekli olarak denetler. Parametreleri gereken şekilde özelleştirin. 

Gerekirse, [Service Fabric SDK’sı](../service-fabric-get-started.md) ile Service Fabric PowerShell modülünü yükleyin. 

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/service-fabric/upgrade-application/upgrade-application.ps1 "Upgrade an application")]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [Get-ServiceFabricApplication](/powershell/module/servicefabric/get-servicefabricapplication?view=azureservicefabricps) | Service Fabric kümesindeki tüm uygulamaları veya belirli bir uygulamayı alır.  |
| [Get-ServiceFabricApplicationUpgrade](/powershell/module/servicefabric/get-servicefabricapplicationupgrade?view=azureservicefabricps) | Service Fabric uygulama yükseltmesinin durumunu alır. |
| [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) | Service Fabric kümesinde kayıtlı Service Fabric uygulama türlerini alır. |
| [Unregister-ServiceFabricApplicationType](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) | Service Fabric uygulama türünün kaydını siler.  |
| [Copy-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) | Bir Service Fabric uygulama paketini görüntü deposuna kopyalar.  |
| [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) | Service Fabric uygulama türünü kaydeder. |
| [Start-ServiceFabricApplicationUpgrade](/powershell/module/servicefabric/start-servicefabricapplicationupgrade?view=azureservicefabricps) | Service Fabric bir uygulamayı belirtilen uygulama türü sürümüne yükseltir. |
| [Remove-ServiceFabricApplicationPackage](/powershell/module/servicefabric/remove-servicefabricapplicationpackage?view=azureservicefabricps) | Service Fabric uygulama paketini görüntü deposundan kaldırır.|


## <a name="next-steps"></a>Sonraki adımlar

Service Fabric PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/service-fabric/overview?view=azureservicefabricps).

Azure Service Fabric için ek Powershell örneklerine [Azure PowerShell örnekleri](../service-fabric-powershell-samples.md) içinden erişilebilir.
