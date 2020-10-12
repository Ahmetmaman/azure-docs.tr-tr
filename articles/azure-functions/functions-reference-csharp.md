---
title: Azure Işlevleri C# betik geliştirici başvurusu
description: C# betiği kullanarak Azure Işlevleri geliştirmeyi anlayın.
author: craigshoemaker
ms.topic: conceptual
ms.custom: devx-track-csharp
ms.date: 12/12/2017
ms.author: cshoe
ms.openlocfilehash: 4d5388f850f47323f6ad79f9f91e617e506546bf
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "88205440"
---
# <a name="azure-functions-c-script-csx-developer-reference"></a>Azure Işlevleri C# betiği (. CSX) geliştirici başvurusu

<!-- When updating this article, make corresponding changes to any duplicate content in functions-dotnet-class-library.md -->

Bu makale, C# betiği (*. CSX*) kullanarak Azure işlevleri geliştirmeye giriş niteliğindedir.

Azure Işlevleri C# ve C# betiği programlama dillerini destekler. [Visual Studio sınıf kitaplığı projesinde c# kullanma](functions-develop-vs.md)hakkında rehberlik arıyorsanız bkz. [c# geliştirici başvurusu](functions-dotnet-class-library.md).

Bu makalede, [Azure işlevleri geliştirici kılavuzunu](functions-reference.md)zaten okuduğunuzu varsaymış olursunuz.

## <a name="how-csx-works"></a>. CSX nasıl kullanılır?

Azure Işlevleri için C# betik deneyimi, [Azure Web İşleri SDK 'sını](https://github.com/Azure/azure-webjobs-sdk/wiki/Introduction)temel alır. Veriler, yöntem bağımsız değişkenleri aracılığıyla C# işlevinizde akar. Bağımsız değişken adları bir dosyada belirtilir `function.json` ve işlev günlükçüsü ve iptal belirteçleri gibi öğelere erişim için önceden tanımlanmış adlar vardır.

*. CSX* biçimi, daha az "demirbaş" yazmanızı ve yalnızca bir C# işlevi yazmaya odaklanabilmenizi sağlar. Bir ad alanı ve sınıf içindeki her şeyi sarmalama yerine yalnızca bir `Run` yöntem tanımlayın. Tüm derleme başvurularını ve ad alanlarını dosyanın başına her zamanki gibi ekleyin.

Bir örnek başlatıldığında bir işlev uygulamasının *. CSX* dosyaları derlenir. Bu derleme adımı, C# sınıf kitaplıklarıyla karşılaştırılan C# betik işlevleri için soğuk başlangıç gibi şeyler daha uzun sürebilir. Bu derleme adımı ayrıca C# betik işlevlerinin Azure portal düzenlenebilir olmasının yanı sıra C# sınıf kitaplıkları da değildir.

## <a name="folder-structure"></a>Klasör yapısı

C# betik projesi için klasör yapısı aşağıdaki gibi görünür:

```
FunctionsProject
 | - MyFirstFunction
 | | - run.csx
 | | - function.json
 | | - function.proj
 | - MySecondFunction
 | | - run.csx
 | | - function.json
 | | - function.proj
 | - host.json
 | - extensions.csproj
 | - bin
```

İşlev uygulamasını yapılandırmak için kullanılabilecek bir paylaşılan [host.js](functions-host-json.md) dosyası vardır. Her işlevin kendi kod dosyası (. CSX) ve bağlama yapılandırma dosyası (function.js) vardır.

Çalışma zamanının [sürüm 2. x ve sonraki sürümlerinde](functions-versions.md) gerekli olan bağlama uzantıları, `extensions.csproj` dosyasında, klasördeki gerçek kitaplık dosyalarıyla birlikte tanımlanmıştır `bin` . Yerel olarak geliştirme yaparken, [bağlama uzantılarını kaydetmeniz](./functions-bindings-register.md#extension-bundles)gerekir. Azure portal işlevler geliştirirken, bu kayıt sizin için yapılır.

## <a name="binding-to-arguments"></a>Bağımsız değişkenlere bağlama

Giriş veya çıkış verileri, `name` yapılandırma dosyasındaki *function.js* özelliği aracılığıyla bir C# betik işlevi parametresine bağlıdır. Aşağıdaki örnek, bir *function.js* dosya ve Queue-tetiklenen bir işlev için *. CSX dosyası çalıştırır* . Kuyruk iletisinden veri alan parametre, `myQueueItem` özelliğin değeri olduğundan adlandırılır `name` .

```json
{
    "disabled": false,
    "bindings": [
        {
            "type": "queueTrigger",
            "direction": "in",
            "name": "myQueueItem",
            "queueName": "myqueue-items",
            "connection":"MyStorageConnectionAppSetting"
        }
    ]
}
```

```csharp
#r "Microsoft.WindowsAzure.Storage"

using Microsoft.Extensions.Logging;
using Microsoft.WindowsAzure.Storage.Queue;
using System;

public static void Run(CloudQueueMessage myQueueItem, ILogger log)
{
    log.LogInformation($"C# Queue trigger function processed: {myQueueItem.AsString}");
}
```

`#r` [Bu makalede daha sonra](#referencing-external-assemblies)açıklanan ifade açıklanmaktadır.

## <a name="supported-types-for-bindings"></a>Bağlamalar için desteklenen türler

Her bağlamanın kendi desteklenen türleri vardır; Örneğin, bir blob tetikleyicisi bir dize parametresi, POCO parametresi, bir `CloudBlockBlob` parametre veya desteklenen diğer birkaç türden herhangi biri ile kullanılabilir. [BLOB bağlamaları için bağlama başvuru makalesi](functions-bindings-storage-blob-trigger.md#usage) , blob Tetikleyicileri için desteklenen tüm parametre türlerini listeler. Daha fazla bilgi için, bkz. [Tetikleyiciler ve bağlamalar](functions-triggers-bindings.md) ve [her bağlama türü için bağlama başvurusu belgeleri](functions-triggers-bindings.md#next-steps).

[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

## <a name="referencing-custom-classes"></a>Özel sınıflara başvurma

Özel bir düz eski CLR nesnesi (POCO) sınıfı kullanmanız gerekiyorsa, sınıf tanımını aynı dosyaya dahil edebilir veya ayrı bir dosyaya yerleştirebilirsiniz.

Aşağıdaki örnek, POCO sınıfı tanımını içeren *Run. CSX* örneğini gösterir.

```csharp
public static void Run(string myBlob, out MyClass myQueueItem)
{
    log.Verbose($"C# Blob trigger function processed: {myBlob}");
    myQueueItem = new MyClass() { Id = "myid" };
}

public class MyClass
{
    public string Id { get; set; }
}
```

POCO sınıfında her özellik için tanımlanmış bir alıcı ve ayarlayıcı olmalıdır.

## <a name="reusing-csx-code"></a>. CSX kodunu yeniden kullanma

*Run. CSX* dosyanızdaki diğer *. CSX* dosyalarında tanımlanmış sınıfları ve yöntemleri kullanabilirsiniz. Bunu yapmak için `#load` *Run. CSX* dosyanızdaki yönergeleri kullanın. Aşağıdaki örnekte, adlı bir günlüğe kaydetme yordamı `MyLogger` *mygünlükçü. CSX* içinde paylaşılır ve yönergesi kullanılarak *. CSX çalıştırılmak* üzere yüklendi `#load` :

Örnek *Run. CSX*:

```csharp
#load "mylogger.csx"

using Microsoft.Extensions.Logging;

public static void Run(TimerInfo myTimer, ILogger log)
{
    log.LogInformation($"Log by run.csx: {DateTime.Now}");
    MyLogger(log, $"Log by MyLogger: {DateTime.Now}");
}
```

Örnek *mygünlükçü. CSX*:

```csharp
public static void MyLogger(ILogger log, string logtext)
{
    log.LogInformation(logtext);
}
```

Bir POCO nesnesi kullanarak işlevler arasında geçirilen verileri kesin bir şekilde yazmak istediğinizde, paylaşılan bir *. CSX* dosyası kullanmak yaygın bir modeldir. Aşağıdaki basitleştirilmiş örnekte, bir HTTP tetikleyicisi ve kuyruk tetikleyicisi, `Order` sıralama verilerini kesin olarak yazmak için adlı BIR POCO nesnesini paylaşır:

HTTP tetikleyicisi için *Run. CSX* örneği:

```cs
#load "..\shared\order.csx"

using System.Net;
using Microsoft.Extensions.Logging;

public static async Task<HttpResponseMessage> Run(Order req, IAsyncCollector<Order> outputQueueItem, ILogger log)
{
    log.LogInformation("C# HTTP trigger function received an order.");
    log.LogInformation(req.ToString());
    log.LogInformation("Submitting to processing queue.");

    if (req.orderId == null)
    {
        return new HttpResponseMessage(HttpStatusCode.BadRequest);
    }
    else
    {
        await outputQueueItem.AddAsync(req);
        return new HttpResponseMessage(HttpStatusCode.OK);
    }
}
```

Queue tetikleyicisi için *Run. CSX* örnek:

```cs
#load "..\shared\order.csx"

using System;
using Microsoft.Extensions.Logging;

public static void Run(Order myQueueItem, out Order outputQueueItem, ILogger log)
{
    log.LogInformation($"C# Queue trigger function processed order...");
    log.LogInformation(myQueueItem.ToString());

    outputQueueItem = myQueueItem;
}
```

Örnek *Order. CSX*:

```cs
public class Order
{
    public string orderId {get; set; }
    public string custName {get; set;}
    public string custAddress {get; set;}
    public string custEmail {get; set;}
    public string cartId {get; set; }

    public override String ToString()
    {
        return "\n{\n\torderId : " + orderId +
                  "\n\tcustName : " + custName +
                  "\n\tcustAddress : " + custAddress +
                  "\n\tcustEmail : " + custEmail +
                  "\n\tcartId : " + cartId + "\n}";
    }
}
```

Yönergeyle göreli bir yol kullanabilirsiniz `#load` :

* `#load "mylogger.csx"` işlev klasöründe bulunan bir dosyayı yükler.
* `#load "loadedfiles\mylogger.csx"` işlev klasöründeki bir klasörde bulunan bir dosyayı yükler.
* `#load "..\shared\mylogger.csx"` bir klasörde bulunan bir dosyayı işlev klasörüyle aynı düzeyde, yani doğrudan *Wwwroot*altında yükler.

`#load`Yönergesi *. cs* dosyalarıyla değil yalnızca *. CSX* dosyaları ile birlikte kullanılabilir.

## <a name="binding-to-method-return-value"></a>Metot dönüş değerine bağlama

function.jsüzerindeki adı kullanarak bir çıkış bağlaması için bir yöntem dönüş değeri kullanabilirsiniz `$return` . *function.json* Örnekler için bkz. [Tetikleyiciler ve bağlamalar](./functions-bindings-return-value.md).

Dönüş değerini yalnızca başarılı bir işlev yürütmesi her zaman çıkış bağlamasına geçirilecek bir dönüş değeri ile sonuçlanırsa kullanın. Aksi takdirde, `ICollector` `IAsyncCollector` aşağıdaki bölümde gösterildiği gibi, veya kullanın.

## <a name="writing-multiple-output-values"></a>Birden çok çıkış değeri yazma

Bir çıkış bağlamasına birden çok değer yazmak için veya başarılı bir işlev çağrılması çıkış bağlamasına geçirilecek herhangi bir şeye neden olmazsa, [`ICollector`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/ICollector.cs) veya [`IAsyncCollector`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IAsyncCollector.cs) türlerini kullanın. Bu türler, yöntem tamamlandığında çıkış bağlamaya yazılan salt yazılır koleksiyonlardır.

Bu örnek, kullanarak birden çok kuyruk iletisini aynı kuyruğa yazar `ICollector` :

```csharp
public static void Run(ICollector<string> myQueue, ILogger log)
{
    myQueue.Add("Hello");
    myQueue.Add("World!");
}
```

## <a name="logging"></a>Günlüğe Kaydetme

Çıktıyı C# ' deki akış günlüklerinizi günlüğe kaydetmek için, [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger)türünde bir bağımsız değişken ekleyin. Bunu sizin adınızla yapmanızı öneririz `log` . `Console.Write`Azure işlevleri 'nde kullanmaktan kaçının.

```csharp
public static void Run(string myBlob, ILogger log)
{
    log.LogInformation($"C# Blob trigger function processed: {myBlob}");
}
```

> [!NOTE]
> Yerine kullanabileceğiniz daha yeni bir günlük çerçevesi hakkında daha fazla bilgi için `TraceWriter` bkz. **Azure işlevlerini Izleme** makalesindeki [C# işlevlerinde yazma günlükleri](functions-monitoring.md#write-logs-in-c-functions) .

## <a name="async"></a>Zaman Uyumsuz

Bir işlevi [zaman uyumsuz](/dotnet/csharp/programming-guide/concepts/async/)yapmak için `async` anahtar sözcüğünü kullanın ve bir `Task` nesne döndürün.

```csharp
public async static Task ProcessQueueMessageAsync(
        string blobName,
        Stream blobInput,
        Stream blobOutput)
{
    await blobInput.CopyToAsync(blobOutput, 4096);
}
```

`out`Zaman uyumsuz işlevlerde parametreleri kullanamazsınız. Çıktı bağlamaları için, bunun yerine [işlev dönüş değeri](#binding-to-method-return-value) veya [toplayıcı nesnesi](#writing-multiple-output-values) kullanın.

## <a name="cancellation-tokens"></a>İptal belirteçleri

Bir işlev bir [CancellationToken](/dotnet/api/system.threading.cancellationtoken) parametresini kabul edebilir ve bu, işlev sonlandırıldıktan sonra işletim sisteminin kodunuza bildirmesini sağlar. Bu bildirimi, işlevin verileri tutarsız bir durumda bırakmak için beklenmedik bir şekilde sonlandığından emin olmak için kullanabilirsiniz.

Aşağıdaki örnek, yaklaşan işlev sonlandırmasının nasıl kontrol alınacağını gösterir.

```csharp
using System;
using System.IO;
using System.Threading;

public static void Run(
    string inputText,
    TextWriter logger,
    CancellationToken token)
{
    for (int i = 0; i < 100; i++)
    {
        if (token.IsCancellationRequested)
        {
            logger.WriteLine("Function was cancelled at iteration {0}", i);
            break;
        }
        Thread.Sleep(5000);
        logger.WriteLine("Normal processing for queue message={0}", inputText);
    }
}
```

## <a name="importing-namespaces"></a>Ad alanlarını içeri aktarma

Ad alanlarını içeri aktarmanız gerekiyorsa, yan tümcesiyle birlikte her zamanki gibi yapabilirsiniz `using` .

```csharp
using System.Net;
using System.Threading.Tasks;
using Microsoft.Extensions.Logging;

public static Task<HttpResponseMessage> Run(HttpRequestMessage req, ILogger log)
```

Aşağıdaki ad alanları otomatik olarak içeri aktarılır ve bu nedenle isteğe bağlıdır:

* `System`
* `System.Collections.Generic`
* `System.IO`
* `System.Linq`
* `System.Net.Http`
* `System.Threading.Tasks`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`

## <a name="referencing-external-assemblies"></a>Dış derlemelere başvurma

Çerçeve derlemeleri için yönergesini kullanarak başvuruları ekleyin `#r "AssemblyName"` .

```csharp
#r "System.Web.Http"

using System.Net;
using System.Net.Http;
using System.Threading.Tasks;
using Microsoft.Extensions.Logging;

public static Task<HttpResponseMessage> Run(HttpRequestMessage req, ILogger log)
```

Aşağıdaki derlemeler Azure Işlevleri barındırma ortamı tarafından otomatik olarak eklenir:

* `mscorlib`
* `System`
* `System.Core`
* `System.Xml`
* `System.Net.Http`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`
* `Microsoft.Azure.WebJobs.Extensions`
* `System.Web.Http`
* `System.Net.Http.Formatting`

Aşağıdaki derlemelere basit ad (örneğin,) tarafından başvurulabilir `#r "AssemblyName"` :

* `Newtonsoft.Json`
* `Microsoft.WindowsAzure.Storage`
* `Microsoft.ServiceBus`
* `Microsoft.AspNet.WebHooks.Receivers`
* `Microsoft.AspNet.WebHooks.Common`
* `Microsoft.Azure.NotificationHubs`

## <a name="referencing-custom-assemblies"></a>Özel derlemelere başvurma

Özel bir derlemeye başvurmak için, *paylaşılan* bir derlemeyi ya da *özel* bir derlemeyi kullanabilirsiniz:

* Paylaşılan derlemeler bir işlev uygulaması içindeki tüm işlevler arasında paylaşılır. Özel bir derlemeye başvurmak için, derlemeyi `bin` [işlev uygulaması kök klasörünüzde](functions-reference.md#folder-structure) (Wwwroot) adlı bir klasöre yükleyin.

* Özel derlemeler belirli bir işlevin bağlamının parçasıdır ve farklı sürümlerin dışarıdan yüklenmesini destekler. Özel derlemelerin `bin` işlev dizinindeki bir klasöre yüklenmesi gerekir. Dosya adını kullanarak derlemelere başvuru yapın, örneğin `#r "MyAssembly.dll"` .

İşlev klasörünüze dosya yükleme hakkında daha fazla bilgi için bkz. [Paket Yönetimi](#using-nuget-packages)hakkındaki bölüm.

### <a name="watched-directories"></a>İzlenen dizinler

İşlev betiği dosyasını içeren dizin, derlemelerdeki değişiklikler için otomatik olarak izlenebilir. Diğer dizinlerdeki derleme değişikliklerini izlemek için, bunları `watchDirectories` [host.js](functions-host-json.md)' de listeye ekleyin.

## <a name="using-nuget-packages"></a>NuGet paketlerini kullanma
NuGet paketlerini 2. x ve sonraki bir C# işlevinde kullanmak için işlev uygulamasının dosya sistemindeki işlevin klasörüne bir *function. proj* dosyası yükleyin. Aşağıda, *Microsoft. ProjectOxford. Face* sürüm *1.1.0*'e bir başvuru ekleyen bir *function. proj* dosyası verilmiştir:

```xml
<Project Sdk="Microsoft.NET.Sdk">
    <PropertyGroup>
        <TargetFramework>netstandard2.0</TargetFramework>
    </PropertyGroup>

    <ItemGroup>
        <PackageReference Include="Microsoft.ProjectOxford.Face" Version="1.1.0" />
    </ItemGroup>
</Project>
```

Özel bir NuGet akışı kullanmak için, akışı İşlev Uygulaması kökündeki bir *Nuget.Config* dosyasında belirtin. Daha fazla bilgi için bkz. [NuGet davranışını yapılandırma](/nuget/consume-packages/configuring-nuget-behavior).

> [!NOTE]
> 1. x C# işlevlerinde, NuGet paketlerine bir *function. proj* dosyası yerine bir *project.js* dosyası ile başvurulur.

1. x işlevleri için bunun yerine *project.js* bir dosya kullanın. Dosyadaki bir örnek *project.js* aşağıda verilmiştir:

```json
{
  "frameworks": {
    "net46":{
      "dependencies": {
        "Microsoft.ProjectOxford.Face": "1.1.0"
      }
    }
   }
}
```

### <a name="using-a-functionproj-file"></a>Function. proj dosyası kullanma

1. Azure portal işlevi açın. Günlükler sekmesi paket yükleme çıkışını görüntüler.
2. Bir *function. proj* dosyasını karşıya yüklemek Için Azure işlevleri geliştirici başvurusu konusundaki [nasıl güncelleştirme işlev uygulama dosyalarında](functions-reference.md#fileupdate) açıklanan yöntemlerden birini kullanın.
3. *Function. proj* dosyası karşıya yüklendikten sonra, işlevinizin akış günlüğünde aşağıdaki örneğe benzer bir çıktı görürsünüz:

```
2018-12-14T22:00:48.658 [Information] Restoring packages.
2018-12-14T22:00:48.681 [Information] Starting packages restore
2018-12-14T22:00:57.064 [Information] Restoring packages for D:\local\Temp\9e814101-fe35-42aa-ada5-f8435253eb83\function.proj...
2016-04-04T19:02:50.511 Restoring packages for D:\home\site\wwwroot\HttpTriggerCSharp1\function.proj...
2018-12-14T22:01:00.844 [Information] Installing Newtonsoft.Json 10.0.2.
2018-12-14T22:01:01.041 [Information] Installing Microsoft.ProjectOxford.Common.DotNetStandard 1.0.0.
2018-12-14T22:01:01.140 [Information] Installing Microsoft.ProjectOxford.Face.DotNetStandard 1.0.0.
2018-12-14T22:01:09.799 [Information] Restore completed in 5.79 sec for D:\local\Temp\9e814101-fe35-42aa-ada5-f8435253eb83\function.proj.
2018-12-14T22:01:10.905 [Information] Packages restored.
```

## <a name="environment-variables"></a>Ortam değişkenleri

Bir ortam değişkeni veya bir uygulama ayarı değeri almak için, `System.Environment.GetEnvironmentVariable` Aşağıdaki kod örneğinde gösterildiği gibi kullanın:

```csharp
public static void Run(TimerInfo myTimer, ILogger log)
{
    log.LogInformation($"C# Timer trigger function executed at: {DateTime.Now}");
    log.LogInformation(GetEnvironmentVariable("AzureWebJobsStorage"));
    log.LogInformation(GetEnvironmentVariable("WEBSITE_SITE_NAME"));
}

public static string GetEnvironmentVariable(string name)
{
    return name + ": " +
        System.Environment.GetEnvironmentVariable(name, EnvironmentVariableTarget.Process);
}
```

<a name="imperative-bindings"></a>

## <a name="binding-at-runtime"></a>Çalışma zamanında bağlama

C# ve diğer .NET dillerinde, *function.jsüzerinde* [*bildirim temelli*](https://en.wikipedia.org/wiki/Declarative_programming) bağlamaların aksine, [kesinlik temelli](https://en.wikipedia.org/wiki/Imperative_programming) bağlama modelini kullanabilirsiniz. Bağlama parametrelerinin tasarım zamanı yerine çalışma zamanında hesaplanması gerektiğinde, kesinlik temelli bağlama kullanışlıdır. Bu düzende, işlev kodunuzda, desteklenen giriş ve çıkış bağlamalarına bağlanabilirsiniz.

Zorunlu bir bağlamayı aşağıdaki gibi tanımlayın:

- İstediğiniz kesinlik bağlamalarınız için *function.js* bir **giriş eklemeyin.**
- Bir giriş parametresi veya olarak [`Binder binder`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Host/Bindings/Runtime/Binder.cs) geçirin [`IBinder binder`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IBinder.cs) .
- Veri bağlamayı gerçekleştirmek için aşağıdaki C# stilini kullanın.

```cs
using (var output = await binder.BindAsync<T>(new BindingTypeAttribute(...)))
{
    ...
}
```

`BindingTypeAttribute` , bağlamalarınızı tanımlayan ve `T` Bu bağlama türü tarafından desteklenen bir giriş veya çıkış türü olan .net özniteliğidir. `T` bir `out` parametre türü (gibi `out JObject` ) olamaz. Örneğin, Mobile Apps tablo çıkış bağlaması [altı çıkış türünü](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs#L17-L22)destekler, ancak yalnızca [ \<T> ICollector](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/ICollector.cs) veya [`IAsyncCollector<T>`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IAsyncCollector.cs) for kullanabilirsiniz `T` .

### <a name="single-attribute-example"></a>Tek öznitelik örneği

Aşağıdaki örnek kod, çalışma zamanında tanımlanmış blob yolu ile bir [Depolama Blobu çıkış bağlaması](functions-bindings-storage-blob-output.md) oluşturur ve sonra blob 'a bir dize yazar.

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host.Bindings.Runtime;

public static async Task Run(string input, Binder binder)
{
    using (var writer = await binder.BindAsync<TextWriter>(new BlobAttribute("samples-output/path")))
    {
        writer.Write("Hello World!!");
    }
}
```

[Blobattribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Extensions.Storage/Blobs/BlobAttribute.cs) , [Depolama Blobu](functions-bindings-storage-blob.md) girişini veya çıkış bağlamasını tanımlar ve [TextWriter](/dotnet/api/system.io.textwriter) desteklenen bir çıkış bağlama türüdür.

### <a name="multiple-attribute-example"></a>Birden çok öznitelik örneği

Yukarıdaki örnek, işlev uygulamasının ana depolama hesabı bağlantı dizesi (yani) için uygulama ayarını alır `AzureWebJobsStorage` . [Storageaccountattribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs) ' i ekleyip öznitelik dizisini Içine geçirerek depolama hesabı için kullanılacak özel bir uygulama ayarı belirtebilirsiniz `BindAsync<T>()` . `Binder`Değil parametresini kullanın `IBinder` .  Örneğin:

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host.Bindings.Runtime;

public static async Task Run(string input, Binder binder)
{
    var attributes = new Attribute[]
    {
        new BlobAttribute("samples-output/path"),
        new StorageAccountAttribute("MyStorageAccount")
    };

    using (var writer = await binder.BindAsync<TextWriter>(attributes))
    {
        writer.Write("Hello World!");
    }
}
```

Aşağıdaki tabloda, her bir bağlama türü ve tanımlandıkları paketler için .NET öznitelikleri listelenmektedir.

> [!div class="mx-codeBreakAll"]
> | Bağlama | Öznitelik | Başvuru ekleme |
> |------|------|------|
> | Cosmos DB | [`Microsoft.Azure.WebJobs.DocumentDBAttribute`](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.CosmosDB/CosmosDBAttribute.cs) | `#r "Microsoft.Azure.WebJobs.Extensions.CosmosDB"` |
> | Event Hubs | [`Microsoft.Azure.WebJobs.ServiceBus.EventHubAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/v2.x/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubAttribute.cs), [`Microsoft.Azure.WebJobs.ServiceBusAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/b798412ad74ba97cf2d85487ae8479f277bdd85c/test/Microsoft.Azure.WebJobs.ServiceBus.UnitTests/ServiceBusAccountTests.cs) | `#r "Microsoft.Azure.Jobs.ServiceBus"` |
> | Mobile Apps | [`Microsoft.Azure.WebJobs.MobileTableAttribute`](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs) | `#r "Microsoft.Azure.WebJobs.Extensions.MobileApps"` |
> | Notification Hubs | [`Microsoft.Azure.WebJobs.NotificationHubAttribute`](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/v2.x/src/WebJobs.Extensions.NotificationHubs/NotificationHubAttribute.cs) | `#r "Microsoft.Azure.WebJobs.Extensions.NotificationHubs"` |
> | Service Bus | [`Microsoft.Azure.WebJobs.ServiceBusAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/b798412ad74ba97cf2d85487ae8479f277bdd85c/test/Microsoft.Azure.WebJobs.ServiceBus.UnitTests/ServiceBusAttributeTests.cs), [`Microsoft.Azure.WebJobs.ServiceBusAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/b798412ad74ba97cf2d85487ae8479f277bdd85c/test/Microsoft.Azure.WebJobs.ServiceBus.UnitTests/ServiceBusAccountTests.cs) | `#r "Microsoft.Azure.WebJobs.ServiceBus"` |
> | Depolama kuyruğu | [`Microsoft.Azure.WebJobs.QueueAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/), [`Microsoft.Azure.WebJobs.StorageAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs) | |
> | Depolama blobu | [`Microsoft.Azure.WebJobs.BlobAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Extensions.Storage/Blobs/BlobAttribute.cs), [`Microsoft.Azure.WebJobs.StorageAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs) | |
> | Depolama tablosu | [`Microsoft.Azure.WebJobs.TableAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs), [`Microsoft.Azure.WebJobs.StorageAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs) | |
> | Twilio | [`Microsoft.Azure.WebJobs.TwilioSmsAttribute`](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.Twilio/TwilioSMSAttribute.cs) | `#r "Microsoft.Azure.WebJobs.Extensions.Twilio"` |

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Tetikleyiciler ve bağlamalar hakkında daha fazla bilgi](functions-triggers-bindings.md)

> [!div class="nextstepaction"]
> [Azure Işlevleri için en iyi uygulamalar hakkında daha fazla bilgi edinin](functions-best-practices.md)
