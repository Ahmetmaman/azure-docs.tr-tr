---
title: Azure işlevleri için Zamanlayıcı tetikleyicisi
description: Azure işlevleri'nde Zamanlayıcı Tetikleyicileri kullanma hakkında bilgi edinin.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
keywords: Azure işlevleri, İşlevler, olay işleme dinamik işlem, sunucusuz mimari
ms.assetid: d2f013d1-f458-42ae-baf8-1810138118ac
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.date: 08/08/2018
ms.author: glenga
ms.custom: ''
ms.openlocfilehash: e6a3df79bf0786b536dc4c454d19beea2730125a
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44093133"
---
# <a name="timer-trigger-for-azure-functions"></a>Azure işlevleri için Zamanlayıcı tetikleyicisi 

Bu makalede, Azure işlevleri'nde Zamanlayıcı tetikleyici ile nasıl çalışılacağı açıklanmaktadır. Bir işlev bir zamanlamaya göre çalışan bir zamanlayıcı tetikleyicisi sağlar. 

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-1x"></a>Paketler - 1.x işlevleri

Zamanlayıcı tetikleyicisi sağlanan [Microsoft.Azure.WebJobs.Extensions](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions) NuGet paketi sürüm 2.x. Paket için kaynak kodu konusu [azure webjobs sdk uzantıları](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/v2.x/src/WebJobs.Extensions/Extensions/Timers/) GitHub deposu.

[!INCLUDE [functions-package-auto](../../includes/functions-package-auto.md)]

## <a name="packages---functions-2x"></a>Paketler - 2.x işlevleri

Zamanlayıcı tetikleyicisi sağlanan [Microsoft.Azure.WebJobs.Extensions](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions) NuGet paketi sürüm 3.x. Paket için kaynak kodu konusu [azure webjobs sdk uzantıları](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions/Extensions/Timers/) GitHub deposu.

[!INCLUDE [functions-package-auto](../../includes/functions-package-auto.md)]

## <a name="example"></a>Örnek

Dile özgü örneğe bakın:

* [C#](#trigger---c-example)
* [C# betiği (.csx)](#trigger---c-script-example)
* [F#](#trigger---f-example)
* [JavaScript](#trigger---javascript-example)
* [Java](#trigger---java-example)

### <a name="c-example"></a>C# örneği

Aşağıdaki örnekte gösterildiği bir [C# işlevi](functions-dotnet-class-library.md) , beş dakikada bir çalışır:

```cs
[FunctionName("TimerTriggerCSharp")]
public static void Run([TimerTrigger("0 */5 * * * *")]TimerInfo myTimer, TraceWriter log)
{
    if(myTimer.IsPastDue)
    {
        log.Info("Timer is running late!");
    }
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
}
```

### <a name="c-script-example"></a>C# betiği örneği

Aşağıdaki örnek, bir zamanlayıcı tetikleyicisi bağlama gösterir. bir *function.json* dosyası ve bir [C# betik işlevi](functions-reference-csharp.md) bağlama kullanan. İşlev, bu işlev çağrısını bir eksik zamanlama yinelenme nedeniyle olup olmadığını belirten bir günlüğe yazar.

Veri bağlama işte *function.json* dosyası:

```json
{
    "schedule": "0 */5 * * * *",
    "name": "myTimer",
    "type": "timerTrigger",
    "direction": "in"
}
```

C# betik kodunu şu şekildedir:

```csharp
public static void Run(TimerInfo myTimer, TraceWriter log)
{
    if(myTimer.IsPastDue)
    {
        log.Info("Timer is running late!");
    }
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}" );  
}
```

### <a name="f-example"></a>F # örneği

Aşağıdaki örnek, bir zamanlayıcı tetikleyicisi bağlama gösterir. bir *function.json* dosyası ve bir [F # betik işlevi](functions-reference-fsharp.md) bağlama kullanan. İşlev, bu işlev çağrısını bir eksik zamanlama yinelenme nedeniyle olup olmadığını belirten bir günlüğe yazar.

Veri bağlama işte *function.json* dosyası:

```json
{
    "schedule": "0 */5 * * * *",
    "name": "myTimer",
    "type": "timerTrigger",
    "direction": "in"
}
```

F # betik kodunu şu şekildedir:

```fsharp
let Run(myTimer: TimerInfo, log: TraceWriter ) =
    if (myTimer.IsPastDue) then
        log.Info("F# function is running late.")
    let now = DateTime.Now.ToLongTimeString()
    log.Info(sprintf "F# function executed at %s!" now)
```

### <a name="javascript-example"></a>JavaScript örneği

Aşağıdaki örnek, bir zamanlayıcı tetikleyicisi bağlama gösterir. bir *function.json* dosyası ve bir [JavaScript işlevi](functions-reference-node.md) bağlama kullanan. İşlev, bu işlev çağrısını bir eksik zamanlama yinelenme nedeniyle olup olmadığını belirten bir günlüğe yazar.

Veri bağlama işte *function.json* dosyası:

```json
{
    "schedule": "0 */5 * * * *",
    "name": "myTimer",
    "type": "timerTrigger",
    "direction": "in"
}
```

JavaScript kodu şu şekildedir:

```JavaScript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();

    if(myTimer.isPastDue)
    {
        context.log('Node.js is running late!');
    }
    context.log('Node.js timer trigger function ran!', timeStamp);   

    context.done();
};
```

### <a name="java-example"></a>Java örnek

Aşağıdaki örnek işlevi tetikler ve beş dakikada çalıştırılır. `@TimerTrigger` İşlev üzerindeki ek açıklama tanımlar aynı dize biçimi kullanarak zamanlama [sıralanmış iş ifadeleri](http://en.wikipedia.org/wiki/Cron#CRON_expression).

```java
@FunctionName("keepAlive")
public void keepAlive(
  @TimerTrigger(name = "keepAliveTrigger", schedule = "0 *&#47;5 * * * *") String timerInfo,
      ExecutionContext context
 ) {
     // timeInfo is a JSON string, you can deserialize it to an object using your favorite JSON library
     context.getLogger().info("Timer is triggered: " + timerInfo);
}
```

## <a name="attributes"></a>Öznitelikler

İçinde [C# sınıfı kitaplıklar](functions-dotnet-class-library.md), kullanın [TimerTriggerAttribute](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions/Extensions/Timers/TimerTriggerAttribute.cs).

Özniteliğin Oluşturucusu bir CRON ifadesi alır veya `TimeSpan`. Kullanabileceğiniz `TimeSpan` yalnızca işlev uygulamasını bir App Service planı üzerinde çalışıyorsa. Aşağıdaki örnek, bir CRON ifadesi gösterir:

```csharp
[FunctionName("TimerTriggerCSharp")]
public static void Run([TimerTrigger("0 */5 * * * *")]TimerInfo myTimer, TraceWriter log)
{
    if (myTimer.IsPastDue)
    {
        log.Info("Timer is running late!");
    }
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
}
 ```

## <a name="configuration"></a>Yapılandırma

Aşağıdaki tabloda ayarladığınız bağlama yapılandırma özelliklerini açıklayan *function.json* dosya ve `TimerTrigger` özniteliği.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**type** | yok | "TimerTrigger için" olarak ayarlanmalıdır. Bu özellik, Azure portalında tetikleyicisi oluşturduğunuzda otomatik olarak ayarlanır.|
|**direction** | yok | "İçin" ayarlanmalıdır. Bu özellik, Azure portalında tetikleyicisi oluşturduğunuzda otomatik olarak ayarlanır. |
|**Adı** | yok | İşlev kodunu Zamanlayıcı nesneyi temsil eden değişken adı. | 
|**schedule**|**ScheduleExpression**|A [CRON ifadesi](#cron-expressions) veya [TimeSpan](#timespan) değeri. A `TimeSpan` bir App Service planı üzerinde çalıştırılan bir işlev uygulaması için kullanılabilir. Bir uygulama ayarında zamanlama ifadeyi ve ayar adı içinde sarmalanmış bir uygulama için bu özelliği ayarlayın **%** Bu örnekte olduğu gibi işaretlere: "% ScheduleAppSetting %". |
|**runOnStartup**|**runOnStartup**|Varsa `true`, çalışma zamanı başladığında işlevi çağrılır. Örneğin, işlev uygulaması uyanır eylemsizlik nedeniyle boşta filtrelemesinden geçtikten sonra çalışma zamanı başlatır. ne zaman işlev uygulaması, işlev değişiklikleri nedeniyle ve işlev uygulamasını kullanıma ölçeklendirildiğinde yeniden başlatır. Bu nedenle **runOnStartup** nadiren şimdiye kadar ayarlanması gerekir `true`yüksek oranda beklenmeyen zamanlarda execute kodunu hale getirecek şekilde.|
|**useMonitor**|**useMonitor**|Kümesine `true` veya `false` zamanlama izlenmesi gereken olup olmadığını belirtmek için. İzleme zamanlaması bile işlevi uygulama örneklerini yeniden başlattığınızda zamanlama doğru yönetilmesini sağlamak yardımcı olmak için zamanlama örnekleri'ni kalıcıdır. Açıkça ayarlanmazsa varsayılan olup olmadığını `true` bir yinelenme aralığı 1 dakikadan daha uzun olan zamanlamalar. Dakika başına birden çok kez tetikleyen zamanlamalar için varsayılandır `false`.

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="usage"></a>Kullanım

Bir zamanlayıcı tetikleyicisi işlevi çağrıldığında [Zamanlayıcı nesne](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions/Extensions/Timers/TimerInfo.cs) işleve geçirilir. Aşağıdaki JSON bir zamanlayıcı nesne örneği gösterimidir. 

```json
{
    "Schedule":{
    },
    "ScheduleStatus": {
        "Last":"2016-10-04T10:15:00+00:00",
        "LastUpdated":"2016-10-04T10:16:00+00:00",
        "Next":"2016-10-04T10:20:00+00:00"
    },
    "IsPastDue":false
}
```

`IsPastDue` Özelliği `true` geçerli işlev çağrısını olduğunda zamanlanmış daha. Örneğin, bir işlevi uygulamanın yeniden eksik için çağrıya neden olabilir.

## <a name="cron-expressions"></a>Sıralanmış iş ifadeleri 

Azure işlevleri kullanan [NCronTab](https://github.com/atifaziz/NCrontab) sıralanmış iş ifadeleri olarak yorumlamak için kitaplığı. Bir CRON ifadesi altı alanları içerir:

`{second} {minute} {hour} {day} {month} {day-of-week}`

Her bir alan, şu tür değerlerden biri olabilir:

|Tür  |Örnek  |Tetiklendiğinde  |
|---------|---------|---------|
|Belirli bir değer |<nobr>"0 5 * * * *"</nobr>|hh:05:00 hh olduğu her saat (saatte bir)|
|Tüm değerleri (`*`)|<nobr>"0 * 5 * * *"</nobr>|5:mm adresindeki: her gün 00 mm (60 günde kez) saat, dakika başı olduğu|
|Aralık (`-` işleci)|<nobr>"5-7 * * * * *"</nobr>|hh:mm:05, hh:mm:06 ve hh:mm:07 ss: dd olduğu her saat (3 kez dakika), dakika başı|  
|Bir değerler kümesi (`,` işleci)|<nobr>"5,8,10 * * * * *"</nobr>|hh:mm:05, hh:mm:08 ve hh:mm:10 ss: dd olduğu her saat (3 kez dakika), dakika başı|
|Aralık değeri (`/` işleci)|<nobr>"0 */5 * * * *"</nobr>|hh:05:00, hh:10:00, da hh:15:00, aracılığıyla ve benzeri hh:55:00 hh olduğu her saat (12 kat bir saat)|

Aylık veya gün belirtmek için sayısal değerler, adları veya adlarının kısaltmalarını kullanabilirsiniz:

* Gün boyunca sayısal 6 0 0 ile Pazar başladığı değerlerdir.
* İngilizce dilinde adlarıdır. Örneğin: `Monday`, `January`.
* Adları büyük/küçük harfe duyarsızdır.
* Adları kısaltılmış. Üç harf olduğundan önerilen kısaltması uzunluğu.  Örneğin: `Mon`, `Jan`. 

### <a name="cron-examples"></a>CRON örnekleri

Azure işlevleri'nde Zamanlayıcı tetikleyicisi için kullanabileceğiniz sıralanmış iş ifadeleri bazı örnekleri aşağıda verilmiştir.

|Örnek|Tetiklendiğinde  |
|---------|---------|
|`"0 */5 * * * *"`|beş dakikada bir kez|
|`"0 0 * * * *"`|sonra her saat başında|
|`"0 0 */2 * * *"`|Her iki saatte bir kez|
|`"0 0 9-17 * * *"`|saatte bir kez gelen 09: 00-17|
|`"0 30 9 * * *"`|her gün 09:30:00|
|`"0 30 9 * * 1-5"`|Haftanın her günü 09:30:00|
|`"0 30 9 * Jan Mon"`|9: 30'da Ocak Ayındaki her Pazartesi|
>[!NOTE]   
>CRON ifadesi çevrimiçi örnekleri bulabilirsiniz, ancak çoğu atlamak `{second}` alan. Eksik bir tanesi kopyalarsanız, ekleme `{second}` alan. Genellikle, bu alanda bir yıldız sıfır istersiniz.

### <a name="cron-time-zones"></a>CRON saat dilimleri

Bir saat ve tarih, zaman aralığı için bir CRON ifadesi sayıları bakın. Örneğin, 5'te `hour` alan 5: 00'da, her 5 saat ifade eder.

Sıralanmış iş ifadeleri ile kullanılan varsayılan saat dilimini Eşgüdümlü Evrensel Saat (UTC) ' dir. Başka bir saat dilimi temel alınarak, CRON ifadesi için adlı işlev uygulamanız için bir uygulama ayarı oluşturmak `WEBSITE_TIME_ZONE`. Gösterildiği gibi değeri istenen saat dilimi adını ayarlayın [Microsoft saat dilimi dizin](https://technet.microsoft.com/library/cc749073). 

Örneğin, *Doğu Standart Saati* UTC-05:00. Ateş 10 11.00 EST her gün tetikleyin, UTC saat dilimi için hesapları aşağıdaki CRON ifadesi kullanmak, Zamanlayıcı için:

```json
"schedule": "0 0 15 * * *"
``` 

Veya adlı işlev uygulamanız için bir uygulama ayarı oluşturmak `WEBSITE_TIME_ZONE` ve değerine **Doğu Standart Saati**.  Ardından aşağıdaki CRON ifadesi kullanır: 

```json
"schedule": "0 0 10 * * *"
``` 

Kullanırken `WEBSITE_TIME_ZONE`, zaman, gün ışığından yararlanma saatine gibi belirli saat dilimi zaman değişiklikler için ayarlanır. 

## <a name="timespan"></a>Zaman aralığı

 A `TimeSpan` bir App Service planı üzerinde çalıştırılan bir işlev uygulaması için kullanılabilir.

Bir CRON ifadesi aksine bir `TimeSpan` değeri her işlev çağrısını arasındaki zaman aralığını belirtir. Bir işlev belirtilen aralığından daha uzun çalıştırdıktan sonra tamamlandığında, Zamanlayıcıyı hemen işlevi yeniden çağırır.

Bir dize olarak ifade edilen `TimeSpan` biçimi `hh:mm:ss` olduğunda `hh` 24 saatten az olduğu. İlk iki basamak 24 veya daha büyük olduğunda biçimdir `dd:hh:mm`. İşte bazı örnekler:

|Örnek |Tetiklendiğinde  |
|---------|---------|
|"01:00:00" | her saat        |
|"00:01:00"|her dakika         |
|"24:00:00" | her 24 gün        |

## <a name="scale-out"></a>Ölçeklendirme

Zamanlayıcı ile tetiklenen bir işlev yalnızca tek bir örneği, birden fazla örneğe ölçeklendirir çıkış bir işlev uygulaması, tüm örneklerinde çalıştırılır.

## <a name="function-apps-sharing-storage"></a>Depolama paylaşımı işlev uygulamaları

Birden fazla işlev uygulaması arasında bir depolama hesabını paylaşmak, her işlev uygulaması farklı olduğundan emin olun `id` içinde *host.json*. Atlayabilirsiniz `id` özelliği veya her işlevi uygulamanın el ile ayarlamanız `id` için farklı bir değer. Zamanlayıcı tetikleyicisi depolama kilidi olacağına dair yalnızca bir zamanlayıcı örneği birden çok örneği için bir işlev uygulaması kullanıma ölçeklendirildiğinde emin olmak için kullanır. İki işlev uygulamaları aynı paylaşıyorsa `id` ve her bir zamanlayıcı tetikleyicisi kullanan, tek bir zamanlayıcı çalışır.

## <a name="retry-behavior"></a>Yeniden deneme davranışı

Bir işlev başarısız olduktan sonra kuyruk tetikleyicisi, Zamanlayıcı tetikleyicisi yeniden deneme değil. Bir işlevi başarısız olduğunda, zamanlamaya göre yeniden açana kadar çağrılır değil.

## <a name="troubleshooting"></a>Sorun giderme

Zamanlayıcı tetikleyicisi beklendiği gibi çalışmıyor ne yapılacağını hakkında daha fazla bilgi için bkz. [Investigating ve sorunları raporlama Zamanlayıcı ile tetiklenen değil tetikleme işlevleri](https://github.com/Azure/azure-functions-host/wiki/Investigating-and-reporting-issues-with-timer-triggered-functions-not-firing).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bir zamanlayıcı tetikleyicisi kullanan bir hızlı başlangıca gidin](functions-create-scheduled-function.md)

> [!div class="nextstepaction"]
> [Azure işlevleri Tetikleyicileri ve bağlamaları hakkında daha fazla bilgi edinin](functions-triggers-bindings.md)
