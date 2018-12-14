---
title: Dayanıklı işlevler - Azure için alt düzenlemeleri
description: Dayanıklı işlevler uzantısında düzenlemeleri Azure işlevleri için düzenlemeleri çağırmanıza yapma.
services: functions
author: kashimiz
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: azfuncdf
ms.openlocfilehash: df4cfd8cdf720dd085c3f14ad518c557f270ffa4
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2018
ms.locfileid: "53340870"
---
# <a name="sub-orchestrations-in-durable-functions-azure-functions"></a>Dayanıklı işlevler (Azure işlevleri) içinde alt düzenlemeleri

Etkinlik işlevlerini çağırma ek olarak orchestrator işlevleri diğer orchestrator işlevleri çağırabilir. Örneğin, bir kitaplık dışında daha büyük bir düzenleme orchestrator işlevlerin oluşturabilirsiniz. Ya da bir düzenleyici işlevi birden çok örneği paralel olarak çalıştırılabilir.

Bir düzenleyici işlevi çağrılarak başka bir düzenleyici işlevi çağırabilir [CallSubOrchestratorAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CallSubOrchestratorAsync_) veya [CallSubOrchestratorWithRetryAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CallSubOrchestratorWithRetryAsync_) .NET, yöntemler veya `callSubOrchestrator` veya `callSubOrchestratorWithRetry` JavaScript yöntemleri. [Hata işleme & maaş](durable-functions-error-handling.md#automatic-retry-on-failure) makalede otomatik yeniden deneme hakkında daha fazla bilgi bulunur.

Alt orchestrator İşlevler, arayanın açısından etkinlik işlevleri gibi davranır. Bir değer döndürürler bir özel durum ve üst Düzenleyici işlevi tarafından bekleniyor olabilir.

## <a name="example"></a>Örnek

Aşağıdaki örnekte, bir IOT ("nesnelerin interneti") senaryo gösterilmektedir sağlanması gereken birden çok cihaz olduğu. Aşağıdaki gibi görünebilir CİHAZDAN her biri için yapılması gereken belirli bir düzenleme vardır:

### <a name="c"></a>C#

```csharp
public static async Task DeviceProvisioningOrchestration(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    string deviceId = context.GetInput<string>();

    // Step 1: Create an installation package in blob storage and return a SAS URL.
    Uri sasUrl = await context.CallActivityAsync<Uri>("CreateInstallationPackage", deviceId);

    // Step 2: Notify the device that the installation package is ready.
    await context.CallActivityAsync("SendPackageUrlToDevice", Tuple.Create(deviceId, sasUrl));

    // Step 3: Wait for the device to acknowledge that it has downloaded the new package.
    await context.WaitForExternalEvent<bool>("DownloadCompletedAck");

    // Step 4: ...
}
```

### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2.x işlevleri)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const deviceId = context.df.getInput();

    // Step 1: Create an installation package in blob storage and return a SAS URL.
    const sasUrl = yield context.df.callActivity("CreateInstallationPackage", deviceId);

    // Step 2: Notify the device that the installation package is ready.
    yield context.df.callActivity("SendPackageUrlToDevice", { id: deviceId, url: sasUrl });

    // Step 3: Wait for the device to acknowledge that it has downloaded the new package.
    yield context.df.waitForExternalEvent("DownloadCompletedAck");

    // Step 4: ...
});
```

Bu orchestrator işlevi olarak kullanılabilir-olan tek seferlik cihaz sağlama veya daha büyük bir düzenleme parçası olabilir. İkinci durumda, üst orchestrator işlevi örneklerini zamanlayabilirsiniz `DeviceProvisioningOrchestration` kullanarak `CallSubOrchestratorAsync` (C#) veya `callSubOrchestrator` (JavaScript) API.

Paralel olarak birden çok orchestrator işlevleri çalıştırma gösteren bir örnek aşağıda verilmiştir.

### <a name="c"></a>C#

```csharp
[FunctionName("ProvisionNewDevices")]
public static async Task ProvisionNewDevices(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    string[] deviceIds = await context.CallActivityAsync<string[]>("GetNewDeviceIds");

    // Run multiple device provisioning flows in parallel
    var provisioningTasks = new List<Task>();
    foreach (string deviceId in deviceIds)
    {
        Task provisionTask = context.CallSubOrchestratorAsync("DeviceProvisioningOrchestration", deviceId);
        provisioningTasks.Add(provisionTask);
    }

    await Task.WhenAll(provisioningTasks);

    // ...
}
```

### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2.x işlevleri)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const deviceIds = yield context.df.callActivity("GetNewDeviceIds");

    // Run multiple device provisioning flows in parallel
    const provisioningTasks = [];
    for (const deviceId of deviceIds) {
        const provisionTask = context.df.callSubOrchestrator("DeviceProvisioningOrchestration", deviceId);
        provisioningTasks.push(provisionTask);
    }

    yield context.df.Task.all(provisioningTasks);

    // ...
});
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Görev merkezleri nedir ve nasıl yapılandırılacakları öğrenin](durable-functions-task-hubs.md)
