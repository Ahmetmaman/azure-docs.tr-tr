---
title: Dayanıklı işlevler - Azure, hataları işleme
description: Dayanıklı işlevler uzantısını hataları işlemek için Azure işlevleri hakkında bilgi edinin.
services: functions
author: kashimiz
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: azfuncdf
ms.openlocfilehash: 7a55e28f34f36cd02b67e56c6262b9e1f06dde8f
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2018
ms.locfileid: "53338201"
---
# <a name="handling-errors-in-durable-functions-azure-functions"></a>Dayanıklı işlevler (Azure işlevleri) hataları işleme

Dayanıklı işlevi düzenlemeleri kodda uygulanır ve hata işleme özelliklerini programlama dilini kullanabilirsiniz. Bunu aklınızda gerçekten yok hata işleme ve maaş, düzenlemeleri ekleme yaparken öğrenmek için gereken tüm yeni kavramları. Ancak, bilmeniz gereken birkaç davranışları vardır.

## <a name="errors-in-activity-functions"></a>Etkinlik işlevlerini hataları

Bir etkinlik işlevinde oluşturulan herhangi bir özel durum orchestrator işleve sıraya ve olarak oluşturulan bir `FunctionFailedException`. Orchestrator işlevindeki gereksinimlerinize uygun hata işleme ve Dengeleme kodu yazabilirsiniz.

Örneğin, bir hesaptan para aktarımı aşağıdaki orchestrator işlevi göz önünde bulundurun:

### <a name="c"></a>C#

```csharp
#r "Microsoft.Azure.WebJobs.Extensions.DurableTask"

public static async Task Run(DurableOrchestrationContext context)
{
    var transferDetails = ctx.GetInput<TransferOperation>();

    await context.CallActivityAsync("DebitAccount",
        new
        {
            Account = transferDetails.SourceAccount,
            Amount = transferDetails.Amount
        });

    try
    {
        await context.CallActivityAsync("CreditAccount",
            new
            {
                Account = transferDetails.DestinationAccount,
                Amount = transferDetails.Amount
            });
    }
    catch (Exception)
    {
        // Refund the source account.
        // Another try/catch could be used here based on the needs of the application.
        await context.CallActivityAsync("CreditAccount",
            new
            {
                Account = transferDetails.SourceAccount,
                Amount = transferDetails.Amount
            });
    }
}
```

### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2.x işlevleri)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const transferDetails = context.df.getInput();

    yield context.df.callActivity("DebitAccount",
        {
            account = transferDetails.sourceAccount,
            amount = transferDetails.amount,
        }
    );

    try {
        yield context.df.callActivity("CreditAccount",
            {
                account = transferDetails.destinationAccount,
                amount = transferDetails.amount,
            }
        );
    }
    catch (error) {
        // Refund the source account.
        // Another try/catch could be used here based on the needs of the application.
        yield context.df.callActivity("CreditAccount",
            {
                account = transferDetails.sourceAccount,
                amount = transferDetails.amount,
            }
        );
    }
});
```

Çağrı **CreditAccount** işlevi hedef hesabı için başarısız olursa, orchestrator işlevi bunun için kaynak hesap fon alacak kaydetme dengeler.

## <a name="automatic-retry-on-failure"></a>Hata durumunda otomatik yeniden deneme

Etkinlik işlevlerini veya alt düzenleme işlevler çağırdığınızda, otomatik yeniden deneme ilkesi belirtebilirsiniz. Aşağıdaki örnek, en fazla üç kez bir işlevi çağırmak çalışır ve her yeniden deneme arasındaki 5 saniye bekler:

### <a name="c"></a>C#

```csharp
public static async Task Run(DurableOrchestrationContext context)
{
    var retryOptions = new RetryOptions(
        firstRetryInterval: TimeSpan.FromSeconds(5),
        maxNumberOfAttempts: 3);

    await ctx.CallActivityWithRetryAsync("FlakyFunction", retryOptions, null);

    // ...
}
```

### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2.x işlevleri)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const retryOptions = new df.RetryOptions(5000, 3);

    yield context.df.callActivityWithRetry("FlakyFunction", retryOptions);

    // ...
});
```

`CallActivityWithRetryAsync` (.NET) veya `callActivityWithRetry` (JavaScript) API götüren bir `RetryOptions` parametresi. Suborchestration çağrıları kullanarak `CallSubOrchestratorWithRetryAsync` (.NET) veya `callSubOrchestratorWithRetry` (JavaScript) API'si aynı bu yeniden deneme ilkelerini kullanabilirsiniz.

Otomatik yeniden deneme ilkesi özelleştirmek için birkaç seçenek vardır. Bu ülkelere şunlar dahildir:

* **En fazla deneme sayısı**: Yeniden deneme sayısı.
* **İlk yeniden deneme aralığı**: İlk yeniden deneme girişiminden önce beklenecek süre miktarı.
* **Geri alma katsayısı**: Geri alma sayısında artış oranını belirlemek için kullanılan katsayısı. Varsayılan olarak 1.
* **En fazla yeniden deneme aralığı**: Yeniden denemeler arasında beklenecek en uzun süreyi.
* **Yeniden deneme zaman aşımının**: Bunun yapılması harcayabileceğiniz en uzun süreyi yeniden dener. Süresiz olarak yeniden denemek için varsayılan davranıştır.
* **Tanıtıcı**: Kullanıcı tanımlı bir geri çağırma, işlev çağrısı denenen olup olmadığını belirleyen belirtilebilir.

## <a name="function-timeouts"></a>İşlevi zaman aşımları

Bir düzenleyici işlevi içinde bir işlev çağrısının tamamlanması çok uzun sürüyorsa bırakmasını isteyebilirsiniz. Bugün Bunu yapmak için uygun yolu oluşturmaktır bir [dayanıklı Zamanlayıcı](durable-functions-timers.md) kullanarak `context.CreateTimer` (.NET) veya `context.df.createTimer` (JavaScript) ile birlikte `Task.WhenAny` (.NET) veya `context.df.Task.any` (JavaScript) aşağıdaki örnekte olduğu gibi:

### <a name="c"></a>C#

```csharp
public static async Task<bool> Run(DurableOrchestrationContext context)
{
    TimeSpan timeout = TimeSpan.FromSeconds(30);
    DateTime deadline = context.CurrentUtcDateTime.Add(timeout);

    using (var cts = new CancellationTokenSource())
    {
        Task activityTask = context.CallActivityAsync("FlakyFunction");
        Task timeoutTask = context.CreateTimer(deadline, cts.Token);

        Task winner = await Task.WhenAny(activityTask, timeoutTask);
        if (winner == activityTask)
        {
            // success case
            cts.Cancel();
            return true;
        }
        else
        {
            // timeout case
            return false;
        }
    }
}
```

### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2.x işlevleri)

```javascript
const df = require("durable-functions");
const moment = require("moment");

module.exports = df.orchestrator(function*(context) {
    const deadline = moment.utc(context.df.currentUtcDateTime).add(30, "s");

    const activityTask = context.df.callActivity("FlakyFunction");
    const timeoutTask = context.df.createTimer(deadline.toDate());

    const winner = yield context.df.Task.any([activityTask, timeoutTask]);
    if (winner === activityTask) {
        // success case
        timeoutTask.cancel();
        return true;
    } else {
        // timeout case
        return false;
    }
});
```

> [!NOTE]
> Bu mekanizma Süren etkinlik işlevi yürütme sonlanmamasına. Bunun yerine, yalnızca sonucunu yoksay ve geçmek için orchestrator işlevi sağlar. Daha fazla bilgi için [zamanlayıcılar](durable-functions-timers.md#usage-for-timeout) belgeleri.

## <a name="unhandled-exceptions"></a>İşlenmeyen özel durumlar

Bir düzenleyici işlevi işlenmeyen bir özel durum ile başarısız olursa özel durumun ayrıntılarını günlüğe kaydedilir ve örneği ile tamamlanan bir `Failed` durumu.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Sorunları tanılamayı öğrenin](durable-functions-diagnostics.md)
