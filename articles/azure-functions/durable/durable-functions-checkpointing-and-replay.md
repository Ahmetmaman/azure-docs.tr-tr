---
title: Kontrol noktalarına ve dayanıklı işlevler - Azure içinde yeniden yürütme
description: Denetim noktası oluşturma ve yanıt işleyişi dayanıklı işlevler uzantısını Azure işlevleri için öğrenin.
services: functions
author: kashimiz
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 10/23/2018
ms.author: azfuncdf
ms.openlocfilehash: ce930adc4cb2c635b54b3d41ea4a3ac272541698
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2018
ms.locfileid: "52643169"
---
# <a name="checkpoints-and-replay-in-durable-functions-azure-functions"></a>Kontrol noktalarına ve yeniden yürütme içinde dayanıklı işlevler (Azure işlevleri)

Dayanıklı İşlevler, en önemli özelliklerinden biridir **güvenilir yürütme**. Orchestrator işlevlerini ve etkinlik işlevlerini bir veri merkezi içinde farklı vm'lerinde çalışıyor olabilir ve bu sanal makineler veya ağ altyapının % 100 güvenilir değil.

Bu rağmen güvenilir bir biçimde yürütülmesini düzenlemeleri dayanıklı işlevler sağlar. Bunu sürücü işlev çağrısını depolama kuyruklarını kullanarak ve düzenli olarak denetim noktası oluşturma yürütme geçmişini depolama tablolarına yapar (olarak bilinen bir bulut tasarım modeli kullanarak [olay kaynağını belirleme](https://docs.microsoft.com/azure/architecture/patterns/event-sourcing)). Bu geçmiş, ardından bir düzenleyici işlevi bellek içi durumunu otomatik olarak yeniden çalınabilir.

## <a name="orchestration-history"></a>Orchestration geçmişi

Aşağıdaki orchestrator işlevi olduğunu varsayalım:

#### <a name="c"></a>C#

```csharp
[FunctionName("E1_HelloSequence")]
public static async Task<List<string>> Run(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    var outputs = new List<string>();

    outputs.Add(await context.CallActivityAsync<string>("E1_SayHello", "Tokyo"));
    outputs.Add(await context.CallActivityAsync<string>("E1_SayHello", "Seattle"));
    outputs.Add(await context.CallActivityAsync<string>("E1_SayHello", "London"));

    // returns ["Hello Tokyo!", "Hello Seattle!", "Hello London!"]
    return outputs;
}
```

#### <a name="javascript-functions-v2-only"></a>JavaScript (yalnızca işlevler v2)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const output = [];
    output.push(yield context.df.callActivity("E1_SayHello", "Tokyo"));
    output.push(yield context.df.callActivity("E1_SayHello", "Seattle"));
    output.push(yield context.df.callActivity("E1_SayHello", "London"));

    return output;
});
```

Her `await` (C#) veya `yield` deyimi (JavaScript), dayanıklı görev Framework kontrol noktaları tablo depolama işleve yürütme durumu. Bu durumu ne olarak adlandırılır olan *düzenleme geçmişi*.

## <a name="history-table"></a>Geçmiş tablosu

Genel olarak bakıldığında, dayanıklı görev Framework her noktasında şunları yapar:

1. Yürütme geçmişi, Azure depolama tablolara kaydeder.
2. İşlevler için Kaybolmamasının iletileri çağırmak orchestrator istiyor.
3. Orchestrator için Kaybolmamasının iletileri &mdash; gibi dayanıklı bir zamanlayıcı iletileri.

Denetim noktası işlemi tamamlandıktan sonra orchestrator işlevi oluncaya kadar yapmak için daha fazla iş bellekten kaldırılmak ücretsizdir.

> [!NOTE]
> Azure depolama, tablo depolama ve kuyruk halinde veri kaydetme arasında işlem konusunda garanti sağlamaz. Dayanıklı işlevler depolama sağlayıcısını hatalarını işlemek için kullandığı *nihai tutarlılık* desenleri. Bu desenleri bir kilitlenme veya bir denetim noktası ortasında bağlantı kaybı ise hiçbir veri kaybı olduğundan emin olun.

Tamamlandığında, daha önce gösterilen işlev geçmişini aşağıdaki Azure tablo depolama (gösterim amacıyla kısaltılır) şuna benzer:

| PartitionKey (InstanceId)                     | EventType             | Zaman damgası               | Girdi | Ad             | Sonuç                                                    | Durum | 
|----------------------------------|-----------------------|----------|--------------------------|-------|------------------|-----------------------------------------------------------|---------------------| 
| eaee885b | OrchestratorStarted   | 2017-05-05T18:45:32.362Z |       |                  |                                                           |                     | 
| eaee885b | ExecutionStarted      | 2017-05-05T18:45:28.852Z | Null  | E1_HelloSequence |                                                           |                     | 
| eaee885b | TaskScheduled         | 2017-05-05T18:45:32.670Z |       | E1_SayHello      |                                                           |                     | 
| eaee885b | OrchestratorCompleted | 2017-05-05T18:45:32.670Z |       |                  |                                                           |                     | 
| eaee885b | OrchestratorStarted   | 2017-05-05T18:45:34.232Z |       |                  |                                                           |                     | 
| eaee885b | TaskCompleted         | 2017-05-05T18:45:34.201Z |       |                  | "" "Merhaba Tokyo!" ""                                        |                     | 
| eaee885b | TaskScheduled         | 2017-05-05T18:45:34.435Z |       | E1_SayHello      |                                                           |                     | 
| eaee885b | OrchestratorCompleted | 2017-05-05T18:45:34.435Z |       |                  |                                                           |                     | 
| eaee885b | OrchestratorStarted   | 2017-05-05T18:45:34.857Z |       |                  |                                                           |                     | 
| eaee885b | TaskCompleted         | 2017-05-05T18:45:34.763Z |       |                  | "" "Seattle Merhaba!" ""                                      |                     | 
| eaee885b | TaskScheduled         | 2017-05-05T18:45:34.857Z |       | E1_SayHello      |                                                           |                     | 
| eaee885b | OrchestratorCompleted | 2017-05-05T18:45:34.857Z |       |                  |                                                           |                     | 
| eaee885b | OrchestratorStarted   | 2017-05-05T18:45:35.032Z |       |                  |                                                           |                     | 
| eaee885b | TaskCompleted         | 2017-05-05T18:45:34.919Z |       |                  | "" "Merhaba Londra!" ""                                       |                     | 
| eaee885b | ExecutionCompleted    | 2017-05-05T18:45:35.044Z |       |                  | "[""Tokyo Merhaba!" ",""Merhaba Seattle!" ",""Merhaba Londra!" "]" | Tamamlandı           | 
| eaee885b | OrchestratorCompleted | 2017-05-05T18:45:35.044Z |       |                  |                                                           |                     | 

Sütun değerleri birkaç Notlar:
* **PartitionKey**: düzenleme örneği Kimliğini içerir.
* **EventType**: olay türünü temsil eder. Şu türlerden biri olabilir:
    * **OrchestrationStarted**: orchestrator işlevi ilk kez çalıştırma veya bir await sürdürülemez. `Timestamp` Sütunu belirleyici değerini doldurmak için kullanılır [CurrentUtcDateTime](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CurrentUtcDateTime) API.
    * **ExecutionStarted**: orchestrator işlevi ilk kez çalıştırma başlatıldı. Bu olay işlevi girişinde de içeren `Input` sütun.
    * **TaskScheduled**: etkinlik işlevi zamanlandı. Etkinlik işlevin adını yakalanan `Name` sütun.
    * **TaskCompleted**: bir etkinlik işlev tamamlandı. İşlevin sonucu bulunduğu `Result` sütun.
    * **TimerCreated**: kalıcı bir zamanlayıcı oluşturuldu. `FireAt` Sütun Zamanlayıcı süresinin dolma zamanlanmış UTC saati içerir.
    * **TimerFired**: kalıcı bir zamanlayıcı tetiklendi.
    * **EventRaised**: dış bir olaya düzenleme örneğine gönderildi. `Name` Sütunu olayın adını yakalar ve `Input` sütunu olayın yükünü yakalar.
    * **OrchestratorCompleted**: bekleniyor Düzenleyici işlevi.
    * **ContinueAsNew**: orchestrator işlevi tamamlandı ve kendisini yeni durumuyla yeniden başlatıldı. `Result` Sütunu yeniden örneğinde girdi olarak kullanılan değeri içerir.
    * **ExecutionCompleted**: orchestrator işlev tamamlanana kadar çalıştırıldı (veya başarısız). İşlev veya hata ayrıntılarını çıkışlarına depolanan `Result` sütun.
* **Zaman damgası**: geçmiş olayın UTC zaman damgası.
* **Ad**: çağrıldı işlevin adı.
* **Giriş**: işlevin giriş JSON ile biçimlendirilmiş.
* **Sonuç**: işlev çıkışı; diğer bir deyişle, dönüş değeri.

> [!WARNING]
> Bir hata ayıklama aracı kullanışlı olsa da, bu tabloyu temel bağımlılığın almaz. Dayanıklı işlevler uzantısını geliştikçe değişebilir.

Gelen işlevin sürdürür her zaman bir `await`, dayanıklı görev Framework orchestrator işlevi sıfırdan yeniden çalıştırır. Geçerli zaman uyumsuz işlem alınması olup olmadığını belirlemek için yürütme geçmişini danışır her yeniden çalıştırmada yerleştirin.  Framework işlemi gerçekleşen, bu işlemin çıktısı hemen yeniden yürütür ve diğerine geçer `await`. Bu işlem, tüm geçmişi, bu noktada orchestrator işlevindeki tüm yerel değişkenlerin önceki değerlere geri yüklenir durumdayken kadar devam eder.

## <a name="orchestrator-code-constraints"></a>Orchestrator kod kısıtlamaları

Yeniden yürütme davranışını bir düzenleyici işlevi içinde yazılan kod türü kısıtlamalar oluşturur:

* Orchestrator kod olmalıdır **belirleyici**. Bu, birden çok kez yeniden yürütülmesi gereken ve her zaman aynı sonucu gerekir. Örneğin, geçerli tarih/saat almak, rastgele sayılar alın, rastgele bir GUID oluşturun veya uzak uç noktalarına çağrı için hiçbir doğrudan çağırır.

  Orchestrator kodunun geçerli tarih/saat almak gerekiyorsa, bunu kullanmalısınız [CurrentUtcDateTime](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CurrentUtcDateTime) API yeniden yürütme için güvenlidir.

  Orchestrator kod, rastgele bir GUID oluşturun yapması gerekiyorsa, bunu kullanmalısınız [NewGuid](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_NewGuid) API yeniden yürütme için güvenlidir.

  Etkinlik işlevlerde belirleyici olmayan işlemleri yapılması gerekir. Bu, diğer giriş veya çıkış bağlamaları herhangi bir etkileşim içerir. Bu, belirleyici olmayan değerleri üzerinde ilk yürütme kez oluşturulur ve yürütme geçmişine kaydedilir, sağlar. Sonraki yürütmeleri sonra otomatik olarak kaydedilen değer kullanır.

* Orchestrator kod olmalıdır **engelleyici olmayan**. Örneğin, yani hiçbir g/ç ve çağrı `Thread.Sleep` veya eşdeğer API'ler.

  Bir orchestrator gecikmesi gerekiyorsa kullanabilirsiniz [CreateTimer](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CreateTimer_) API.

* Orchestrator kod gerekir **hiçbir zaman herhangi bir zaman uyumsuz işlemi başlatmak** dışındaki kullanarak [DurableOrchestrationContext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html) API. Örneğin, Hayır `Task.Run`, `Task.Delay` veya `HttpClient.SendAsync`. Dayanıklı görev Framework tek bir iş parçacığı üzerinde orchestrator kodu yürütür ve diğer zaman uyumsuz API'leri tarafından zamanlanabilir diğer tüm iş parçacıkları ile etkileşime giremezler.

* **Sonsuz döngüler kaçınılmalıdır** orchestrator kod. Dayanıklı görev Framework yürütme geçmişi orchestration işlevi ilerledikçe kaydettiğinden, sonsuz bir döngüye belleğin tükenmek üzere orchestrator örneği neden olabilir. Sonsuz döngü senaryoları için API'leri gibi kullanan [ContinueAsNew](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_ContinueAsNew_) işlevi yürütme yeniden başlatın ve önceki yürütme geçmişini atmak için.

Bu kısıtlamaları, ilk olarak, bunlar izlemek sabit olmayan uygulamada göz korkutucu görünse olsa da. Dayanıklı görev Framework yukarıdaki kuralları ihlalleri saptamaya çalışır ve oluşturur bir `NonDeterministicOrchestrationException`. Ancak, bu algılama en yüksek çaba, davranıştır ve ona bağlı olmaması gerekir.

> [!NOTE]
> Tüm bu kurallar tarafından tetiklenen işlevler uygulamak `orchestrationTrigger` bağlama. Etkinlik işlevleri tarafından tetiklenen `activityTrigger` bağlama ve kullanan işlevler `orchestrationClient` bağlama, bu bir sınırlamalara sahip.

## <a name="durable-tasks"></a>Kalıcı görevleri

> [!NOTE]
> Bu bölümde, dayanıklı görev çerçevesi iç uygulama ayrıntıları açıklanmaktadır. Bu bilgiler bilmeden dayanıklı işlevler kullanabilirsiniz. Yalnızca yeniden yürütme davranışını anlamanıza yardımcı olmak için tasarlanmıştır.

Orchestrator işlevlerde güvenli bir şekilde beklenmesini görevler zaman zaman denir *kalıcı görevleri*. Oluşturulan ve dayanıklı görev Framework tarafından yönetilen bir görevler şunlardır. Örnekler tarafından döndürülen görevler `CallActivityAsync`, `WaitForExternalEvent`, ve `CreateTimer`.

Bunlar *kalıcı görevleri* dahili olarak bir listesini kullanarak yönetilen `TaskCompletionSource` nesneleri. Yeniden, yürütme sırasında bu görevleri orchestrator kod yürütme işleminin bir parçası olarak oluşturulan ve ilgili geçmiş olaylar dağıtıcı numaralandırır olarak tamamlanır. Bu tüm geçmişi yeniden kadar eş zamanlı olarak tek bir iş parçacığı kullanarak gerçekleştirilir. Geçmişi yeniden yürütme sonunda tamamlanmamış kalıcı görevleri uygun eylemlerin gerçekleştirilmesini sahiptir. Örneğin, bir ileti etkinliği bir işlevi çağırmak için sıraya alınan olabilir.

Burada açıklanan yürütme davranışını neden orchestrator işlev kodu asla gerekir anlamanıza yardımcı olması `await` dayanıklı olmayan görev: dağıtıcı iş parçacığı tamamlanmasını bekleyin ve bu görev tarafından herhangi bir geri çağırma olasılığı izleme bozuk olabilir orchestrator işlevi durumu. Bazı çalışma zamanı denetimleri Bunu önlemek deneyin içindir.

Dayanıklı görev Framework orchestrator işlevleri nasıl çalışır hakkında daha fazla bilgi isterseniz, başvurmanız yapılacak en iyi şey olan [dayanıklı görev kaynak kodu github'da](https://github.com/Azure/durabletask). Özellikle, bkz: [TaskOrchestrationExecutor.cs](https://github.com/Azure/durabletask/blob/master/src/DurableTask.Core/TaskOrchestrationExecutor.cs) ve [TaskOrchestrationContext.cs](https://github.com/Azure/durabletask/blob/master/src/DurableTask.Core/TaskOrchestrationContext.cs)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Örnek yönetimi hakkında bilgi edinin](durable-functions-instance-management.md)
