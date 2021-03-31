---
title: Dayanıklı İşlevler-Azure için tekton
description: Azure Işlevleri için Dayanıklı İşlevler uzantısında tekton kullanımı.
author: cgillum
ms.topic: conceptual
ms.date: 09/10/2020
ms.author: azfuncdf
ms.openlocfilehash: 50ed473d61dff19f41f77a79513c0ddab521e56f
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "91325751"
---
# <a name="singleton-orchestrators-in-durable-functions-azure-functions"></a>Dayanıklı İşlevler içindeki tekil düzenleyiciler (Azure Işlevleri)

Arka plan işleri için, genellikle belirli bir Orchestrator örneğinin aynı anda çalıştığından emin olmanız gerekir. Bu tür bir tek [dayanıklı işlevler](durable-functions-overview.md) davranışın, bir Orchestrator 'a belirli BIR örnek kimliği atayarak bir Orchestrator 'a atanmasını sağlayabilirsiniz.

## <a name="singleton-example"></a>Tek örnek

Aşağıdaki örnek, tek bir arka plan işi düzenlemesi oluşturan bir HTTP tetikleyici işlevini gösterir. Kod, belirtilen örnek KIMLIĞI için yalnızca bir örneğin mevcut olmasını sağlar.

# <a name="c"></a>[C#](#tab/csharp)

```cs
[FunctionName("HttpStartSingle")]
public static async Task<HttpResponseMessage> RunSingle(
    [HttpTrigger(AuthorizationLevel.Function, methods: "post", Route = "orchestrators/{functionName}/{instanceId}")] HttpRequestMessage req,
    [DurableClient] IDurableOrchestrationClient starter,
    string functionName,
    string instanceId,
    ILogger log)
{
    // Check if an instance with the specified ID already exists or an existing one stopped running(completed/failed/terminated).
    var existingInstance = await starter.GetStatusAsync(instanceId);
    if (existingInstance == null 
    || existingInstance.RuntimeStatus == OrchestrationRuntimeStatus.Completed 
    || existingInstance.RuntimeStatus == OrchestrationRuntimeStatus.Failed 
    || existingInstance.RuntimeStatus == OrchestrationRuntimeStatus.Terminated)
    {
        // An instance with the specified ID doesn't exist or an existing one stopped running, create one.
        dynamic eventData = await req.Content.ReadAsAsync<object>();
        await starter.StartNewAsync(functionName, instanceId, eventData);
        log.LogInformation($"Started orchestration with ID = '{instanceId}'.");
        return starter.CreateCheckStatusResponse(req, instanceId);
    }
    else
    {
        // An instance with the specified ID exists or an existing one still running, don't create one.
        return new HttpResponseMessage(HttpStatusCode.Conflict)
        {
            Content = new StringContent($"An instance with ID '{instanceId}' already exists."),
        };
    }
}
```

> [!NOTE]
> Önceki C# kodu Dayanıklı İşlevler 2. x içindir. Dayanıklı İşlevler 1. x için `OrchestrationClient` özniteliği yerine özniteliği kullanmanız gerekir `DurableClient` ve `DurableOrchestrationClient` yerine parametre türünü kullanmanız gerekir `IDurableOrchestrationClient` . Sürümler arasındaki farklılıklar hakkında daha fazla bilgi için [dayanıklı işlevler sürümler](durable-functions-versions.md) makalesine bakın.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

**function.json**

```json
{
  "bindings": [
    {
      "authLevel": "function",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "route": "orchestrators/{functionName}/{instanceId}",
      "methods": ["post"]
    },
    {
      "name": "starter",
      "type": "orchestrationClient",
      "direction": "in"
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    }
  ]
}
```

**index.js**

```javascript
const df = require("durable-functions");

module.exports = async function(context, req) {
    const client = df.getClient(context);

    const instanceId = req.params.instanceId;
    const functionName = req.params.functionName;

    // Check if an instance with the specified ID already exists or an existing one stopped running(completed/failed/terminated).
    const existingInstance = await client.getStatus(instanceId);
    if (!existingInstance 
        || existingInstance.runtimeStatus == "Completed" 
        || existingInstance.runtimeStatus == "Failed" 
        || existingInstance.runtimeStatus == "Terminated") {
        // An instance with the specified ID doesn't exist or an existing one stopped running, create one.
        const eventData = req.body;
        await client.startNew(functionName, instanceId, eventData);
        context.log(`Started orchestration with ID = '${instanceId}'.`);
        return client.createCheckStatusResponse(req, instanceId);
    } else {
        // An instance with the specified ID exists or an existing one still running, don't create one.
        return {
            status: 409,
            body: `An instance with ID '${instanceId}' already exists.`,
        };
    }
};
```

# <a name="python"></a>[Python](#tab/python)

**function.json**

```json
{
  "bindings": [
    {
      "authLevel": "function",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "route": "orchestrators/{functionName}/{instanceId}",
      "methods": ["post"]
    },
    {
      "name": "starter",
      "type": "orchestrationClient",
      "direction": "in"
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    }
  ]
}
```

**__init__. Kopyala**

```python
import logging
import azure.functions as func
import azure.durable_functions as df

async def main(req: func.HttpRequest, starter: str) -> func.HttpResponse:
    client = df.DurableOrchestrationClient(starter)
    instance_id = req.route_params['instanceId']
    function_name = req.route_params['functionName']

    existing_instance = await client.get_status(instance_id)

    if existing_instance != None or existing_instance.runtime_status in ["Completed", "Failed", "Terminated"]:
        event_data = req.get_body()
        instance_id = await client.start_new(function_name, instance_id, event_data)
        logging.info(f"Started orchestration with ID = '{instance_id}'.")
        return client.create_check_status_response(req, instance_id)
    else:
        return {
            'status': 409,
            'body': f"An instance with ID '${instance_id}' already exists"
        }

```

---

Varsayılan olarak, örnek kimlikleri rastgele oluşturulan GUID 'lerdir. Bununla birlikte, önceki örnekte, örnek KIMLIĞI URL 'den rota verilerinde geçirilir. `GetStatusAsync` `getStatus` `get_status` Belirtilen kimliğe sahip bir örneğin zaten çalışır durumda olup olmadığını denetlemek için kod çağırır (C#), (JavaScript) veya (Python). Böyle bir örnek çalışmıyorsa, bu KIMLIKLE yeni bir örnek oluşturulur.

> [!NOTE]
> Bu örnekte olası bir yarış durumu var. İki tane **Httpstartone** örneği aynı anda yürütülecektir, her iki işlev çağrısı de başarıyı bildirir ancak yalnızca bir düzenleme örneği başlatılır. Gereksinimlerinize bağlı olarak, bu istenmeyen yan etkilere sahip olabilir. Bu nedenle, bu tetikleyici işlevini aynı anda yürütebilmesi için iki isteğin olmadığından emin olmanız önemlidir.

Orchestrator işlevinin uygulama ayrıntıları gerçek anlamda değildir. Bu, başlayan ve tamamlanmış normal bir Orchestrator işlevi olabilir ya da sonsuza kadar (yani, bir [Eternal düzenleme](durable-functions-eternal-orchestrations.md)) çalışan bir düzenleyici olabilir. Önemli nokta, tek seferde çalışan yalnızca bir örnek vardır.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Düzenleyicmelerin yerel HTTP özellikleri hakkında bilgi edinin](durable-functions-http-features.md)
