---
title: 2. x ve üzeri Işlevler için giriş bağlamasını Azure Cosmos DB
description: Azure Işlevlerinde Azure Cosmos DB giriş bağlamayı kullanmayı öğrenin.
author: craigshoemaker
ms.topic: reference
ms.date: 02/24/2020
ms.author: cshoe
ms.custom: devx-track-csharp, devx-track-python
ms.openlocfilehash: 21ca30b24c4824a2d303d02f3df712328885e199
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102435214"
---
# <a name="azure-cosmos-db-input-binding-for-azure-functions-2x-and-higher"></a>Azure Işlevleri 2. x ve üzeri için giriş bağlamasını Azure Cosmos DB

Azure Cosmos DB giriş bağlaması SQL API'sini kullanarak bir veya daha fazla Azure Cosmos DB belgesini getirir ve işlevin giriş parametresine iletir. Belge kimliği veya sorgu parametreleri, işlevi çağıran tetikleyiciye göre belirlenebilir.

Kurulum ve yapılandırma ayrıntıları hakkında bilgi için bkz. [genel bakış](./functions-bindings-cosmosdb-v2.md).

> [!NOTE]
> Koleksiyon [bölümlenmiş](../cosmos-db/partitioning-overview.md#logical-partitions)ise, arama işlemlerinin bölüm anahtarı değerini de belirtmesi gerekir.
>

<a id="example" name="example"></a>

# <a name="c"></a>[C#](#tab/csharp)

Bu bölüm aşağıdaki örnekleri içerir:

* [Kuyruk tetikleyicisi, JSON 'dan KIMLIK arama](#queue-trigger-look-up-id-from-json-c)
* [HTTP tetikleyicisi, sorgu dizesinden KIMLIĞI ara](#http-trigger-look-up-id-from-query-string-c)
* [HTTP tetikleyicisi, rota verilerinden KIMLIK arama](#http-trigger-look-up-id-from-route-data-c)
* [HTTP tetikleyicisi, SqlQuery kullanarak rota verilerinden KIMLIK arama](#http-trigger-look-up-id-from-route-data-using-sqlquery-c)
* [HTTP tetikleyicisi, SqlQuery kullanarak birden çok belge al](#http-trigger-get-multiple-docs-using-sqlquery-c)
* [HTTP tetikleyicisi, DocumentClient kullanarak birden çok belge edinme](#http-trigger-get-multiple-docs-using-documentclient-c)

Örnekler basit bir `ToDoItem` türe başvurur:

```cs
namespace CosmosDBSamplesV2
{
    public class ToDoItem
    {
        [JsonProperty("id")]
        public string Id { get; set; }
        
        [JsonProperty("partitionKey")]
        public string PartitionKey { get; set; }
        
        public string Description { get; set; }
    }
}
```

<a id="queue-trigger-look-up-id-from-json-c"></a>

### <a name="queue-trigger-look-up-id-from-json"></a>Kuyruk tetikleyicisi, JSON 'dan KIMLIK arama 

Aşağıdaki örnekte, tek bir belgeyi alan bir [C# işlevi](functions-dotnet-class-library.md) gösterilmektedir. İşlev, JSON nesnesi içeren bir kuyruk iletisi tarafından tetiklenir. Sıra tetikleyicisi, JSON `ToDoItemLookup` öğesini, aranacak kimliği ve bölüm anahtarı değerini içeren türünde bir nesne olarak ayrıştırır. Bu KIMLIK ve bölüm anahtarı değeri, `ToDoItem` belirtilen veritabanından ve koleksiyondan bir belge almak için kullanılır.

```cs
namespace CosmosDBSamplesV2
{
    public class ToDoItemLookup
    {
        public string ToDoItemId { get; set; }

        public string ToDoItemPartitionKeyValue { get; set; }
    }
}
```

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.Extensions.Logging;

namespace CosmosDBSamplesV2
{
    public static class DocByIdFromJSON
    {
        [FunctionName("DocByIdFromJSON")]
        public static void Run(
            [QueueTrigger("todoqueueforlookup")] ToDoItemLookup toDoItemLookup,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection",
                Id = "{ToDoItemId}",
                PartitionKey = "{ToDoItemPartitionKeyValue}")]ToDoItem toDoItem,
            ILogger log)
        {
            log.LogInformation($"C# Queue trigger function processed Id={toDoItemLookup?.ToDoItemId} Key={toDoItemLookup?.ToDoItemPartitionKeyValue}");

            if (toDoItem == null)
            {
                log.LogInformation($"ToDo item not found");
            }
            else
            {
                log.LogInformation($"Found ToDo item, Description={toDoItem.Description}");
            }
        }
    }
}
```

<a id="http-trigger-look-up-id-from-query-string-c"></a>

### <a name="http-trigger-look-up-id-from-query-string"></a>HTTP tetikleyicisi, sorgu dizesinden KIMLIĞI ara

Aşağıdaki örnekte, tek bir belgeyi alan bir [C# işlevi](functions-dotnet-class-library.md) gösterilmektedir. İşlev, aranacak KIMLIĞI ve bölüm anahtarı değerini belirtmek için bir sorgu dizesi kullanan bir HTTP isteği tarafından tetiklenir. Bu KIMLIK ve bölüm anahtarı değeri, `ToDoItem` belirtilen veritabanından ve koleksiyondan bir belge almak için kullanılır.

>[!NOTE]
>HTTP sorgu dizesi parametresi, büyük/küçük harfe duyarlıdır.
>

```cs
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.Extensions.Logging;

namespace CosmosDBSamplesV2
{
    public static class DocByIdFromQueryString
    {
        [FunctionName("DocByIdFromQueryString")]
        public static IActionResult Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)]
                HttpRequest req,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection",
                Id = "{Query.id}",
                PartitionKey = "{Query.partitionKey}")] ToDoItem toDoItem,
            ILogger log)
        {
            log.LogInformation("C# HTTP trigger function processed a request.");

            if (toDoItem == null)
            {
                log.LogInformation($"ToDo item not found");
            }
            else
            {
                log.LogInformation($"Found ToDo item, Description={toDoItem.Description}");
            }
            return new OkResult();
        }
    }
}
```

<a id="http-trigger-look-up-id-from-route-data-c"></a>

### <a name="http-trigger-look-up-id-from-route-data"></a>HTTP tetikleyicisi, rota verilerinden KIMLIK arama

Aşağıdaki örnekte, tek bir belgeyi alan bir [C# işlevi](functions-dotnet-class-library.md) gösterilmektedir. İşlev, aranacak KIMLIĞI ve bölüm anahtarı değerini belirtmek için rota verilerini kullanan bir HTTP isteği tarafından tetiklenir. Bu KIMLIK ve bölüm anahtarı değeri, `ToDoItem` belirtilen veritabanından ve koleksiyondan bir belge almak için kullanılır.

```cs
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.Extensions.Logging;

namespace CosmosDBSamplesV2
{
    public static class DocByIdFromRouteData
    {
        [FunctionName("DocByIdFromRouteData")]
        public static IActionResult Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post",
                Route = "todoitems/{partitionKey}/{id}")]HttpRequest req,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection",
                Id = "{id}",
                PartitionKey = "{partitionKey}")] ToDoItem toDoItem,
            ILogger log)
        {
            log.LogInformation("C# HTTP trigger function processed a request.");

            if (toDoItem == null)
            {
                log.LogInformation($"ToDo item not found");
            }
            else
            {
                log.LogInformation($"Found ToDo item, Description={toDoItem.Description}");
            }
            return new OkResult();
        }
    }
}
```

<a id="http-trigger-look-up-id-from-route-data-using-sqlquery-c"></a>

### <a name="http-trigger-look-up-id-from-route-data-using-sqlquery"></a>HTTP tetikleyicisi, SqlQuery kullanarak rota verilerinden KIMLIK arama

Aşağıdaki örnekte, tek bir belgeyi alan bir [C# işlevi](functions-dotnet-class-library.md) gösterilmektedir. İşlev, aranacak KIMLIĞI belirtmek için rota verilerini kullanan bir HTTP isteği tarafından tetiklenir. Bu KIMLIK, `ToDoItem` belirtilen veritabanından ve koleksiyondan bir belgeyi almak için kullanılır.

Örnek, bir bağlama ifadesinin parametresini nasıl kullanacağınızı gösterir `SqlQuery` . Veri yolu `SqlQuery` parametresini gösterildiği gibi parametreye geçirebilirsiniz, ancak şu anda [sorgu dizesi değerlerini geçiremezsiniz](https://github.com/Azure/azure-functions-host/issues/2554#issuecomment-392084583).

> [!NOTE]
> Yalnızca KIMLIĞE göre sorgulama yapmanız gerekiyorsa, [Önceki örneklerde](#http-trigger-look-up-id-from-query-string-c)olduğu gibi, daha az [istek birimi](../cosmos-db/request-units.md)tükettiği için bir arama kullanmanız önerilir. Nokta okuma işlemleri (GET), KIMLIĞE göre sorgulardan [daha etkilidir](../cosmos-db/optimize-cost-reads-writes.md) .
>

```cs
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using System.Collections.Generic;
using Microsoft.Extensions.Logging;

namespace CosmosDBSamplesV2
{
    public static class DocByIdFromRouteDataUsingSqlQuery
    {
        [FunctionName("DocByIdFromRouteDataUsingSqlQuery")]
        public static IActionResult Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post",
                Route = "todoitems2/{id}")]HttpRequest req,
            [CosmosDB("ToDoItems", "Items",
                ConnectionStringSetting = "CosmosDBConnection",
                SqlQuery = "select * from ToDoItems r where r.id = {id}")]
                IEnumerable<ToDoItem> toDoItems,
            ILogger log)
        {
            log.LogInformation("C# HTTP trigger function processed a request.");

            foreach (ToDoItem toDoItem in toDoItems)
            {
                log.LogInformation(toDoItem.Description);
            }
            return new OkResult();
        }
    }
}
```

<a id="http-trigger-get-multiple-docs-using-sqlquery-c"></a>

### <a name="http-trigger-get-multiple-docs-using-sqlquery"></a>HTTP tetikleyicisi, SqlQuery kullanarak birden çok belge al

Aşağıdaki örnek, belge listesini alan bir [C# işlevini](functions-dotnet-class-library.md) gösterir. İşlev bir HTTP isteği tarafından tetiklenir. Sorgu `SqlQuery` öznitelik özelliğinde belirtilir.

```cs
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using System.Collections.Generic;
using Microsoft.Extensions.Logging;

namespace CosmosDBSamplesV2
{
    public static class DocsBySqlQuery
    {
        [FunctionName("DocsBySqlQuery")]
        public static IActionResult Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)]
                HttpRequest req,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection",
                SqlQuery = "SELECT top 2 * FROM c order by c._ts desc")]
                IEnumerable<ToDoItem> toDoItems,
            ILogger log)
        {
            log.LogInformation("C# HTTP trigger function processed a request.");
            foreach (ToDoItem toDoItem in toDoItems)
            {
                log.LogInformation(toDoItem.Description);
            }
            return new OkResult();
        }
    }
}

```

<a id="http-trigger-get-multiple-docs-using-documentclient-c"></a>

### <a name="http-trigger-get-multiple-docs-using-documentclient"></a>HTTP tetikleyicisi, DocumentClient kullanarak birden çok belge edinme

Aşağıdaki örnek, belge listesini alan bir [C# işlevini](functions-dotnet-class-library.md) gösterir. İşlev bir HTTP isteği tarafından tetiklenir. Kod, `DocumentClient` bir belge listesini okumak için Azure Cosmos DB bağlama tarafından sunulan bir örneği kullanır. `DocumentClient`Örnek, yazma işlemleri için de kullanılabilir.

> [!NOTE]
> Testi kolaylaştırmak için [ıdocumentclient](/dotnet/api/microsoft.azure.documents.idocumentclient) arabirimini de kullanabilirsiniz.

```cs
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.Documents.Client;
using Microsoft.Azure.Documents.Linq;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.Extensions.Logging;
using System;
using System.Linq;
using System.Threading.Tasks;

namespace CosmosDBSamplesV2
{
    public static class DocsByUsingDocumentClient
    {
        [FunctionName("DocsByUsingDocumentClient")]
        public static async Task<IActionResult> Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post",
                Route = null)]HttpRequest req,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection")] DocumentClient client,
            ILogger log)
        {
            log.LogInformation("C# HTTP trigger function processed a request.");

            var searchterm = req.Query["searchterm"];
            if (string.IsNullOrWhiteSpace(searchterm))
            {
                return (ActionResult)new NotFoundResult();
            }

            Uri collectionUri = UriFactory.CreateDocumentCollectionUri("ToDoItems", "Items");

            log.LogInformation($"Searching for: {searchterm}");

            IDocumentQuery<ToDoItem> query = client.CreateDocumentQuery<ToDoItem>(collectionUri)
                .Where(p => p.Description.Contains(searchterm))
                .AsDocumentQuery();

            while (query.HasMoreResults)
            {
                foreach (ToDoItem result in await query.ExecuteNextAsync())
                {
                    log.LogInformation(result.Description);
                }
            }
            return new OkResult();
        }
    }
}
```

# <a name="c-script"></a>[C# betiği](#tab/csharp-script)

Bu bölüm aşağıdaki örnekleri içerir:

* [Kuyruk tetikleyicisi, dizeden KIMLIK arama](#queue-trigger-look-up-id-from-string-c-script)
* [Kuyruk tetikleyicisi, SqlQuery kullanarak birden çok belge alma](#queue-trigger-get-multiple-docs-using-sqlquery-c-script)
* [HTTP tetikleyicisi, sorgu dizesinden KIMLIĞI ara](#http-trigger-look-up-id-from-query-string-c-script)
* [HTTP tetikleyicisi, rota verilerinden KIMLIK arama](#http-trigger-look-up-id-from-route-data-c-script)
* [HTTP tetikleyicisi, SqlQuery kullanarak birden çok belge al](#http-trigger-get-multiple-docs-using-sqlquery-c-script)
* [HTTP tetikleyicisi, DocumentClient kullanarak birden çok belge edinme](#http-trigger-get-multiple-docs-using-documentclient-c-script)

HTTP tetikleyicisi örnekleri basit bir `ToDoItem` türe başvurur:

```cs
namespace CosmosDBSamplesV2
{
    public class ToDoItem
    {
        public string Id { get; set; }
        public string Description { get; set; }
    }
}
```

<a id="queue-trigger-look-up-id-from-string-c-script"></a>

### <a name="queue-trigger-look-up-id-from-string"></a>Kuyruk tetikleyicisi, dizeden KIMLIK arama

Aşağıdaki örnek, bir *function.js* dosyasında bir Cosmos DB girişi bağlamasını ve bağlamayı kullanan bir [C# betik işlevini](functions-reference-csharp.md) gösterir. İşlevi tek bir belgeyi okur ve belgenin metin değerini güncelleştirir.

Dosyadaki *function.js* bağlama verileri aşağıda verilmiştir:

```json
{
    "name": "inputDocument",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "id" : "{queueTrigger}",
    "partitionKey": "{partition key value}",
    "connectionStringSetting": "MyAccount_COSMOSDB",
    "direction": "in"
}
```
[Yapılandırma](#configuration) bölümünde bu özellikler açıklanmaktadır.

C# betik kodu aşağıda verilmiştir:

```cs
    using System;

    // Change input document contents using Azure Cosmos DB input binding
    public static void Run(string myQueueItem, dynamic inputDocument)
    {
      inputDocument.text = "This has changed.";
    }
```

<a id="queue-trigger-get-multiple-docs-using-sqlquery-c-script"></a>

### <a name="queue-trigger-get-multiple-docs-using-sqlquery"></a>Kuyruk tetikleyicisi, SqlQuery kullanarak birden çok belge alma

Aşağıdaki örnek, bir *function.js* dosyasında bir Azure Cosmos DB girişi bağlamasını ve bağlamayı kullanan bir [C# betik işlevini](functions-reference-csharp.md) gösterir. İşlevi, sorgu parametrelerini özelleştirmek için bir kuyruk tetikleyicisi kullanarak bir SQL sorgusu tarafından belirtilen birden çok belgeyi alır.

Sıra tetikleyicisi bir parametre sağlar `departmentId` . Bir kuyruk iletisi `{ "departmentId" : "Finance" }` , finans departmanı için tüm kayıtları döndürür.

Dosyadaki *function.js* bağlama verileri aşağıda verilmiştir:

```json
{
    "name": "documents",
    "type": "cosmosDB",
    "direction": "in",
    "databaseName": "MyDb",
    "collectionName": "MyCollection",
    "sqlQuery": "SELECT * from c where c.departmentId = {departmentId}",
    "connectionStringSetting": "CosmosDBConnection"
}
```

[Yapılandırma](#configuration) bölümünde bu özellikler açıklanmaktadır.

C# betik kodu aşağıda verilmiştir:

```csharp
    public static void Run(QueuePayload myQueueItem, IEnumerable<dynamic> documents)
    {
        foreach (var doc in documents)
        {
            // operate on each document
        }
    }

    public class QueuePayload
    {
        public string departmentId { get; set; }
    }
```

<a id="http-trigger-look-up-id-from-query-string-c-script"></a>

### <a name="http-trigger-look-up-id-from-query-string"></a>HTTP tetikleyicisi, sorgu dizesinden KIMLIĞI ara

Aşağıdaki örnekte, tek bir belgeyi alan bir [C# betik işlevi](functions-reference-csharp.md) gösterilmektedir. İşlev, aranacak KIMLIĞI ve bölüm anahtarı değerini belirtmek için bir sorgu dizesi kullanan bir HTTP isteği tarafından tetiklenir. Bu KIMLIK ve bölüm anahtarı değeri, `ToDoItem` belirtilen veritabanından ve koleksiyondan bir belge almak için kullanılır.

Dosyada *function.js* :

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    },
    {
      "type": "cosmosDB",
      "name": "toDoItem",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connectionStringSetting": "CosmosDBConnection",
      "direction": "in",
      "Id": "{Query.id}",
      "PartitionKey" : "{Query.partitionKeyValue}"
    }
  ],
  "disabled": false
}
```

C# betik kodu aşağıda verilmiştir:

```cs
using System.Net;
using Microsoft.Extensions.Logging;

public static HttpResponseMessage Run(HttpRequestMessage req, ToDoItem toDoItem, ILogger log)
{
    log.LogInformation("C# HTTP trigger function processed a request.");

    if (toDoItem == null)
    {
         log.LogInformation($"ToDo item not found");
    }
    else
    {
        log.LogInformation($"Found ToDo item, Description={toDoItem.Description}");
    }
    return req.CreateResponse(HttpStatusCode.OK);
}
```

<a id="http-trigger-look-up-id-from-route-data-c-script"></a>

### <a name="http-trigger-look-up-id-from-route-data"></a>HTTP tetikleyicisi, rota verilerinden KIMLIK arama

Aşağıdaki örnekte, tek bir belgeyi alan bir [C# betik işlevi](functions-reference-csharp.md) gösterilmektedir. İşlev, aranacak KIMLIĞI ve bölüm anahtarı değerini belirtmek için rota verilerini kullanan bir HTTP isteği tarafından tetiklenir. Bu KIMLIK ve bölüm anahtarı değeri, `ToDoItem` belirtilen veritabanından ve koleksiyondan bir belge almak için kullanılır.

Dosyada *function.js* :

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "methods": [
        "get",
        "post"
      ],
      "route":"todoitems/{partitionKeyValue}/{id}"
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    },
    {
      "type": "cosmosDB",
      "name": "toDoItem",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connectionStringSetting": "CosmosDBConnection",
      "direction": "in",
      "Id": "{id}",
      "PartitionKey": "{partitionKeyValue}"
    }
  ],
  "disabled": false
}
```

C# betik kodu aşağıda verilmiştir:

```cs
using System.Net;
using Microsoft.Extensions.Logging;

public static HttpResponseMessage Run(HttpRequestMessage req, ToDoItem toDoItem, ILogger log)
{
    log.LogInformation("C# HTTP trigger function processed a request.");

    if (toDoItem == null)
    {
         log.LogInformation($"ToDo item not found");
    }
    else
    {
        log.LogInformation($"Found ToDo item, Description={toDoItem.Description}");
    }
    return req.CreateResponse(HttpStatusCode.OK);
}
```

<a id="http-trigger-get-multiple-docs-using-sqlquery-c-script"></a>

### <a name="http-trigger-get-multiple-docs-using-sqlquery"></a>HTTP tetikleyicisi, SqlQuery kullanarak birden çok belge al

Aşağıdaki örnekte, belge listesini alan bir [C# betik işlevi](functions-reference-csharp.md) gösterilmektedir. İşlev bir HTTP isteği tarafından tetiklenir. Sorgu `SqlQuery` öznitelik özelliğinde belirtilir.

Dosyada *function.js* :

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    },
    {
      "type": "cosmosDB",
      "name": "toDoItems",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connectionStringSetting": "CosmosDBConnection",
      "direction": "in",
      "sqlQuery": "SELECT top 2 * FROM c order by c._ts desc"
    }
  ],
  "disabled": false
}
```

C# betik kodu aşağıda verilmiştir:

```cs
using System.Net;
using Microsoft.Extensions.Logging;

public static HttpResponseMessage Run(HttpRequestMessage req, IEnumerable<ToDoItem> toDoItems, ILogger log)
{
    log.LogInformation("C# HTTP trigger function processed a request.");

    foreach (ToDoItem toDoItem in toDoItems)
    {
        log.LogInformation(toDoItem.Description);
    }
    return req.CreateResponse(HttpStatusCode.OK);
}
```

<a id="http-trigger-get-multiple-docs-using-documentclient-c-script"></a>

### <a name="http-trigger-get-multiple-docs-using-documentclient"></a>HTTP tetikleyicisi, DocumentClient kullanarak birden çok belge edinme

Aşağıdaki örnekte, belge listesini alan bir [C# betik işlevi](functions-reference-csharp.md) gösterilmektedir. İşlev bir HTTP isteği tarafından tetiklenir. Kod, `DocumentClient` bir belge listesini okumak için Azure Cosmos DB bağlama tarafından sunulan bir örneği kullanır. `DocumentClient`Örnek, yazma işlemleri için de kullanılabilir.

Dosyada *function.js* :

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    },
    {
      "type": "cosmosDB",
      "name": "client",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connectionStringSetting": "CosmosDBConnection",
      "direction": "inout"
    }
  ],
  "disabled": false
}
```

C# betik kodu aşağıda verilmiştir:

```cs
#r "Microsoft.Azure.Documents.Client"

using System.Net;
using Microsoft.Azure.Documents.Client;
using Microsoft.Azure.Documents.Linq;
using Microsoft.Extensions.Logging;

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, DocumentClient client, ILogger log)
{
    log.LogInformation("C# HTTP trigger function processed a request.");

    Uri collectionUri = UriFactory.CreateDocumentCollectionUri("ToDoItems", "Items");
    string searchterm = req.GetQueryNameValuePairs()
        .FirstOrDefault(q => string.Compare(q.Key, "searchterm", true) == 0)
        .Value;

    if (searchterm == null)
    {
        return req.CreateResponse(HttpStatusCode.NotFound);
    }

    log.LogInformation($"Searching for word: {searchterm} using Uri: {collectionUri.ToString()}");
    IDocumentQuery<ToDoItem> query = client.CreateDocumentQuery<ToDoItem>(collectionUri)
        .Where(p => p.Description.Contains(searchterm))
        .AsDocumentQuery();

    while (query.HasMoreResults)
    {
        foreach (ToDoItem result in await query.ExecuteNextAsync())
        {
            log.LogInformation(result.Description);
        }
    }
    return req.CreateResponse(HttpStatusCode.OK);
}
```

# <a name="java"></a>[Java](#tab/java)

Bu bölüm aşağıdaki örnekleri içerir:

* [HTTP tetikleyicisi, sorgu dizesinden KIMLIĞI ara dize parametresi](#http-trigger-look-up-id-from-query-string---string-parameter-java)
* [HTTP tetikleyicisi, sorgu dizesinden KIMLIĞI ara-POJO parametresi](#http-trigger-look-up-id-from-query-string---pojo-parameter-java)
* [HTTP tetikleyicisi, rota verilerinden KIMLIK arama](#http-trigger-look-up-id-from-route-data-java)
* [HTTP tetikleyicisi, SqlQuery kullanarak rota verilerinden KIMLIK arama](#http-trigger-look-up-id-from-route-data-using-sqlquery-java)
* [HTTP tetikleyicisi, SqlQuery kullanarak rota verilerinden birden çok belge alın](#http-trigger-get-multiple-docs-from-route-data-using-sqlquery-java)

Örnekler basit bir `ToDoItem` türe başvurur:

```java
public class ToDoItem {

  private String id;
  private String description;

  public String getId() {
    return id;
  }

  public String getDescription() {
    return description;
  }

  @Override
  public String toString() {
    return "ToDoItem={id=" + id + ",description=" + description + "}";
  }
}
```

<a id="http-trigger-look-up-id-from-query-string---string-parameter-java"></a>

### <a name="http-trigger-look-up-id-from-query-string---string-parameter"></a>HTTP tetikleyicisi, sorgu dizesinden KIMLIĞI ara dize parametresi

Aşağıdaki örnekte, tek bir belgeyi alan bir Java işlevi gösterilmektedir. İşlev, aranacak KIMLIĞI ve bölüm anahtarı değerini belirtmek için bir sorgu dizesi kullanan bir HTTP isteği tarafından tetiklenir. Bu KIMLIK ve bölüm anahtarı değeri, belirtilen veritabanından ve koleksiyondan bir belgeyi dize biçiminde almak için kullanılır.

```java
public class DocByIdFromQueryString {

    @FunctionName("DocByIdFromQueryString")
    public HttpResponseMessage run(
            @HttpTrigger(name = "req",
              methods = {HttpMethod.GET, HttpMethod.POST},
              authLevel = AuthorizationLevel.ANONYMOUS)
            HttpRequestMessage<Optional<String>> request,
            @CosmosDBInput(name = "database",
              databaseName = "ToDoList",
              collectionName = "Items",
              id = "{Query.id}",
              partitionKey = "{Query.partitionKeyValue}",
              connectionStringSetting = "Cosmos_DB_Connection_String")
            Optional<String> item,
            final ExecutionContext context) {

        // Item list
        context.getLogger().info("Parameters are: " + request.getQueryParameters());
        context.getLogger().info("String from the database is " + (item.isPresent() ? item.get() : null));

        // Convert and display
        if (!item.isPresent()) {
            return request.createResponseBuilder(HttpStatus.BAD_REQUEST)
                          .body("Document not found.")
                          .build();
        }
        else {
            // return JSON from Cosmos. Alternatively, we can parse the JSON string
            // and return an enriched JSON object.
            return request.createResponseBuilder(HttpStatus.OK)
                          .header("Content-Type", "application/json")
                          .body(item.get())
                          .build();
        }
    }
}
 ```

[Java işlevleri çalışma zamanı kitaplığı](/java/api/overview/azure/functions/runtime)'nda, `@CosmosDBInput` değeri Cosmos DB gelen işlev parametrelerinde ek açıklamayı kullanın.  Bu ek açıklama, kullanılarak yerel Java türleri, POJOs veya null atanabilir değerlerle kullanılabilir `Optional<T>` .

<a id="http-trigger-look-up-id-from-query-string---pojo-parameter-java"></a>

### <a name="http-trigger-look-up-id-from-query-string---pojo-parameter"></a>HTTP tetikleyicisi, sorgu dizesinden KIMLIĞI ara-POJO parametresi

Aşağıdaki örnekte, tek bir belgeyi alan bir Java işlevi gösterilmektedir. İşlev, aranacak KIMLIĞI ve bölüm anahtarı değerini belirtmek için bir sorgu dizesi kullanan bir HTTP isteği tarafından tetiklenir. Bu KIMLIK ve bölüm anahtarı değeri, belirtilen veritabanından ve koleksiyondan bir belgeyi almak için kullanılır. Daha sonra belge daha ```ToDoItem``` önce oluşturulan Pojo örneğine dönüştürülür ve işleve bağımsız değişken olarak geçirilir.

```java
public class DocByIdFromQueryStringPojo {

    @FunctionName("DocByIdFromQueryStringPojo")
    public HttpResponseMessage run(
            @HttpTrigger(name = "req",
              methods = {HttpMethod.GET, HttpMethod.POST},
              authLevel = AuthorizationLevel.ANONYMOUS)
            HttpRequestMessage<Optional<String>> request,
            @CosmosDBInput(name = "database",
              databaseName = "ToDoList",
              collectionName = "Items",
              id = "{Query.id}",
              partitionKey = "{Query.partitionKeyValue}",
              connectionStringSetting = "Cosmos_DB_Connection_String")
            ToDoItem item,
            final ExecutionContext context) {

        // Item list
        context.getLogger().info("Parameters are: " + request.getQueryParameters());
        context.getLogger().info("Item from the database is " + item);

        // Convert and display
        if (item == null) {
            return request.createResponseBuilder(HttpStatus.BAD_REQUEST)
                          .body("Document not found.")
                          .build();
        }
        else {
            return request.createResponseBuilder(HttpStatus.OK)
                          .header("Content-Type", "application/json")
                          .body(item)
                          .build();
        }
    }
}
 ```

<a id="http-trigger-look-up-id-from-route-data-java"></a>

### <a name="http-trigger-look-up-id-from-route-data"></a>HTTP tetikleyicisi, rota verilerinden KIMLIK arama

Aşağıdaki örnekte, tek bir belgeyi alan bir Java işlevi gösterilmektedir. İşlev, aranacak KIMLIĞI ve bölüm anahtarı değerini belirtmek için yol parametresi kullanan bir HTTP isteği tarafından tetiklenir. Bu KIMLIK ve bölüm anahtarı değeri, belirtilen veritabanından ve koleksiyondan bir belgeyi almak için kullanılır ```Optional<String>``` .

```java
public class DocByIdFromRoute {

    @FunctionName("DocByIdFromRoute")
    public HttpResponseMessage run(
            @HttpTrigger(name = "req",
              methods = {HttpMethod.GET, HttpMethod.POST},
              authLevel = AuthorizationLevel.ANONYMOUS,
              route = "todoitems/{partitionKeyValue}/{id}")
            HttpRequestMessage<Optional<String>> request,
            @CosmosDBInput(name = "database",
              databaseName = "ToDoList",
              collectionName = "Items",
              id = "{id}",
              partitionKey = "{partitionKeyValue}",
              connectionStringSetting = "Cosmos_DB_Connection_String")
            Optional<String> item,
            final ExecutionContext context) {

        // Item list
        context.getLogger().info("Parameters are: " + request.getQueryParameters());
        context.getLogger().info("String from the database is " + (item.isPresent() ? item.get() : null));

        // Convert and display
        if (!item.isPresent()) {
            return request.createResponseBuilder(HttpStatus.BAD_REQUEST)
                          .body("Document not found.")
                          .build();
        }
        else {
            // return JSON from Cosmos. Alternatively, we can parse the JSON string
            // and return an enriched JSON object.
            return request.createResponseBuilder(HttpStatus.OK)
                          .header("Content-Type", "application/json")
                          .body(item.get())
                          .build();
        }
    }
}
 ```

 <a id="http-trigger-look-up-id-from-route-data-using-sqlquery-java"></a>

### <a name="http-trigger-look-up-id-from-route-data-using-sqlquery"></a>HTTP tetikleyicisi, SqlQuery kullanarak rota verilerinden KIMLIK arama

Aşağıdaki örnekte, tek bir belgeyi alan bir Java işlevi gösterilmektedir. İşlev, aranacak KIMLIĞI belirtmek için yol parametresi kullanan bir HTTP isteği tarafından tetiklenir. Bu KIMLIK, ```ToDoItem[]``` sorgu ölçütlerine bağlı olarak çok sayıda belge döndürüldüğünden, belirtilen veritabanından ve koleksiyondan bir belgeyi almak için kullanılır.

> [!NOTE]
> Yalnızca KIMLIĞE göre sorgulama yapmanız gerekiyorsa, [Önceki örneklerde](#http-trigger-look-up-id-from-query-string---pojo-parameter-java)olduğu gibi, daha az [istek birimi](../cosmos-db/request-units.md)tükettiği için bir arama kullanmanız önerilir. Nokta okuma işlemleri (GET), KIMLIĞE göre sorgulardan [daha etkilidir](../cosmos-db/optimize-cost-reads-writes.md) .
>

```java
public class DocByIdFromRouteSqlQuery {

    @FunctionName("DocByIdFromRouteSqlQuery")
    public HttpResponseMessage run(
            @HttpTrigger(name = "req",
              methods = {HttpMethod.GET, HttpMethod.POST},
              authLevel = AuthorizationLevel.ANONYMOUS,
              route = "todoitems2/{id}")
            HttpRequestMessage<Optional<String>> request,
            @CosmosDBInput(name = "database",
              databaseName = "ToDoList",
              collectionName = "Items",
              sqlQuery = "select * from Items r where r.id = {id}",
              connectionStringSetting = "Cosmos_DB_Connection_String")
            ToDoItem[] item,
            final ExecutionContext context) {

        // Item list
        context.getLogger().info("Parameters are: " + request.getQueryParameters());
        context.getLogger().info("Items from the database are " + item);

        // Convert and display
        if (item == null) {
            return request.createResponseBuilder(HttpStatus.BAD_REQUEST)
                          .body("Document not found.")
                          .build();
        }
        else {
            return request.createResponseBuilder(HttpStatus.OK)
                          .header("Content-Type", "application/json")
                          .body(item)
                          .build();
        }
    }
}
 ```

 <a id="http-trigger-get-multiple-docs-from-route-data-using-sqlquery-java"></a>

### <a name="http-trigger-get-multiple-docs-from-route-data-using-sqlquery"></a>HTTP tetikleyicisi, SqlQuery kullanarak rota verilerinden birden çok belge alın

Aşağıdaki örnekte, birden çok belgeyi alan bir Java işlevi gösterilmektedir. İşlevi, ```desc``` alanında aranacak dizeyi belirtmek için bir Route parametresi kullanan BIR http isteği tarafından tetiklenir ```description``` . Arama terimi, belirtilen veritabanından ve koleksiyondan bir belge koleksiyonunu almak, sonuç kümesini öğesine dönüştürmek ```ToDoItem[]``` ve işleve bağımsız değişken olarak geçirmek için kullanılır.

```java
public class DocsFromRouteSqlQuery {

    @FunctionName("DocsFromRouteSqlQuery")
    public HttpResponseMessage run(
            @HttpTrigger(name = "req",
              methods = {HttpMethod.GET},
              authLevel = AuthorizationLevel.ANONYMOUS,
              route = "todoitems3/{desc}")
            HttpRequestMessage<Optional<String>> request,
            @CosmosDBInput(name = "database",
              databaseName = "ToDoList",
              collectionName = "Items",
              sqlQuery = "select * from Items r where contains(r.description, {desc})",
              connectionStringSetting = "Cosmos_DB_Connection_String")
            ToDoItem[] items,
            final ExecutionContext context) {

        // Item list
        context.getLogger().info("Parameters are: " + request.getQueryParameters());
        context.getLogger().info("Number of items from the database is " + (items == null ? 0 : items.length));

        // Convert and display
        if (items == null) {
            return request.createResponseBuilder(HttpStatus.BAD_REQUEST)
                          .body("No documents found.")
                          .build();
        }
        else {
            return request.createResponseBuilder(HttpStatus.OK)
                          .header("Content-Type", "application/json")
                          .body(items)
                          .build();
        }
    }
}
 ```

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Bu bölüm, çeşitli kaynaklardan bir KIMLIK değeri belirterek tek bir belgeyi okuyan aşağıdaki örnekleri içerir:

* [Kuyruk tetikleyicisi, JSON 'dan KIMLIK arama](#queue-trigger-look-up-id-from-json-javascript)
* [HTTP tetikleyicisi, sorgu dizesinden KIMLIĞI ara](#http-trigger-look-up-id-from-query-string-javascript)
* [HTTP tetikleyicisi, rota verilerinden KIMLIK arama](#http-trigger-look-up-id-from-route-data-javascript)
* [Kuyruk tetikleyicisi, SqlQuery kullanarak birden çok belge alma](#queue-trigger-get-multiple-docs-using-sqlquery-javascript)

<a id="queue-trigger-look-up-id-from-json-javascript"></a>

### <a name="queue-trigger-look-up-id-from-json"></a>Kuyruk tetikleyicisi, JSON 'dan KIMLIK arama

Aşağıdaki örnek, bir *function.js* dosyasında bir Cosmos DB girişi bağlamasını ve bağlamayı kullanan bir [JavaScript işlevini](functions-reference-node.md) gösterir. İşlevi tek bir belgeyi okur ve belgenin metin değerini güncelleştirir.

Dosyadaki *function.js* bağlama verileri aşağıda verilmiştir:

```json
{
    "name": "inputDocumentIn",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "id" : "{queueTrigger_payload_property}",
    "partitionKey": "{queueTrigger_payload_property}",
    "connectionStringSetting": "MyAccount_COSMOSDB",
    "direction": "in"
},
{
    "name": "inputDocumentOut",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "createIfNotExists": false,
    "partitionKey": "{queueTrigger_payload_property}",
    "connectionStringSetting": "MyAccount_COSMOSDB",
    "direction": "out"
}
```

[Yapılandırma](#configuration) bölümünde bu özellikler açıklanmaktadır.

JavaScript kodu aşağıda verilmiştir:

```javascript
    // Change input document contents using Azure Cosmos DB input binding, using context.bindings.inputDocumentOut
    module.exports = function (context) {
    context.bindings.inputDocumentOut = context.bindings.inputDocumentIn;
    context.bindings.inputDocumentOut.text = "This was updated!";
    context.done();
    };
```

<a id="http-trigger-look-up-id-from-query-string-javascript"></a>

### <a name="http-trigger-look-up-id-from-query-string"></a>HTTP tetikleyicisi, sorgu dizesinden KIMLIĞI ara

Aşağıdaki örnekte, tek bir belgeyi alan bir [JavaScript işlevi](functions-reference-node.md) gösterilmektedir. İşlev, aranacak KIMLIĞI ve bölüm anahtarı değerini belirtmek için bir sorgu dizesi kullanan bir HTTP isteği tarafından tetiklenir. Bu KIMLIK ve bölüm anahtarı değeri, `ToDoItem` belirtilen veritabanından ve koleksiyondan bir belge almak için kullanılır.

Dosyada *function.js* :

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    },
    {
      "type": "cosmosDB",
      "name": "toDoItem",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connectionStringSetting": "CosmosDBConnection",
      "direction": "in",
      "Id": "{Query.id}",
      "PartitionKey": "{Query.partitionKeyValue}"
    }
  ],
  "disabled": false
}
```

JavaScript kodu aşağıda verilmiştir:

```javascript
module.exports = function (context, req, toDoItem) {
    context.log('JavaScript queue trigger function processed work item');
    if (!toDoItem)
    {
        context.log("ToDo item not found");
    }
    else
    {
        context.log("Found ToDo item, Description=" + toDoItem.Description);
    }

    context.done();
};
```

<a id="http-trigger-look-up-id-from-route-data-javascript"></a>

### <a name="http-trigger-look-up-id-from-route-data"></a>HTTP tetikleyicisi, rota verilerinden KIMLIK arama

Aşağıdaki örnekte, tek bir belgeyi alan bir [JavaScript işlevi](functions-reference-node.md) gösterilmektedir. İşlev, aranacak KIMLIĞI ve bölüm anahtarı değerini belirtmek için rota verilerini kullanan bir HTTP isteği tarafından tetiklenir. Bu KIMLIK ve bölüm anahtarı değeri, `ToDoItem` belirtilen veritabanından ve koleksiyondan bir belge almak için kullanılır.

Dosyada *function.js* :

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "methods": [
        "get",
        "post"
      ],
      "route":"todoitems/{partitionKeyValue}/{id}"
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    },
    {
      "type": "cosmosDB",
      "name": "toDoItem",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connectionStringSetting": "CosmosDBConnection",
      "direction": "in",
      "Id": "{id}",
      "PartitionKey": "{partitionKeyValue}"
    }
  ],
  "disabled": false
}
```

JavaScript kodu aşağıda verilmiştir:

```javascript
module.exports = function (context, req, toDoItem) {
    context.log('JavaScript queue trigger function processed work item');
    if (!toDoItem)
    {
        context.log("ToDo item not found");
    }
    else
    {
        context.log("Found ToDo item, Description=" + toDoItem.Description);
    }

    context.done();
};
```

<a id="queue-trigger-get-multiple-docs-using-sqlquery-javascript"></a>

### <a name="queue-trigger-get-multiple-docs-using-sqlquery"></a>Kuyruk tetikleyicisi, SqlQuery kullanarak birden çok belge alma

Aşağıdaki örnek, bir *function.js* dosyasında bir Azure Cosmos DB girişi bağlamasını ve bağlamayı kullanan bir [JavaScript işlevini](functions-reference-node.md) gösterir. İşlevi, sorgu parametrelerini özelleştirmek için bir kuyruk tetikleyicisi kullanarak bir SQL sorgusu tarafından belirtilen birden çok belgeyi alır.

Sıra tetikleyicisi bir parametre sağlar `departmentId` . Bir kuyruk iletisi `{ "departmentId" : "Finance" }` , finans departmanı için tüm kayıtları döndürür.

Dosyadaki *function.js* bağlama verileri aşağıda verilmiştir:

```json
{
    "name": "documents",
    "type": "cosmosDB",
    "direction": "in",
    "databaseName": "MyDb",
    "collectionName": "MyCollection",
    "sqlQuery": "SELECT * from c where c.departmentId = {departmentId}",
    "connectionStringSetting": "CosmosDBConnection"
}
```

[Yapılandırma](#configuration) bölümünde bu özellikler açıklanmaktadır.

JavaScript kodu aşağıda verilmiştir:

```javascript
module.exports = function (context, input) {
  var documents = context.bindings.documents;
  for (var i = 0; i < documents.length; i++) {
    var document = documents[i];
    // operate on each document
  }
  context.done();
};
```

# <a name="powershell"></a>[PowerShell](#tab/powershell)

* [Kuyruk tetikleyicisi, JSON 'dan KIMLIK arama](#queue-trigger-look-up-id-from-json-ps)
* [HTTP tetikleyicisi, sorgu dizesinden KIMLIĞI ara](#http-trigger-id-query-string-ps)
* [HTTP tetikleyicisi, rota verilerinden KIMLIK arama](#http-trigger-id-route-data-ps)
* [Kuyruk tetikleyicisi, SqlQuery kullanarak birden çok belge alma](#queue-trigger-multiple-docs-sqlquery-ps)

### <a name="queue-trigger-look-up-id-from-json"></a>Kuyruk tetikleyicisi, JSON 'dan KIMLIK arama

Aşağıdaki örnek, tek bir Cosmos DB belgesinin nasıl okunacağını ve güncelleşdiğini gösterir. Belgenin benzersiz tanımlayıcısı, bir kuyruk iletisindeki JSON değeri aracılığıyla sağlanır.

Cosmos DB girişi bağlama öncelikle işlevin yapılandırma dosyasında bulunan bağlamalar listesinde (_function.jsüzerinde_) listelenir.

<a name="queue-trigger-look-up-id-from-json-ps"></a>

```json
{
  "name": "InputDocumentIn",
  "type": "cosmosDB",
  "databaseName": "MyDatabase",
  "collectionName": "MyCollection",
  "id": "{queueTrigger_payload_property}",
  "partitionKey": "{queueTrigger_payload_property}",
  "connectionStringSetting": "CosmosDBConnection",
  "direction": "in"
},
{
  "name": "InputDocumentOut",
  "type": "cosmosDB",
  "databaseName": "MyDatabase",
  "collectionName": "MyCollection",
  "createIfNotExists": false,
  "partitionKey": "{queueTrigger_payload_property}",
  "connectionStringSetting": "CosmosDBConnection",
  "direction": "out"
}
```

_run.ps1_ dosyası, gelen belgeyi okuyan ve değişiklikleri çıktılar olan PowerShell koduna sahiptir.

```powershell
param($QueueItem, $InputDocumentIn, $TriggerMetadata) 

$Document = $InputDocumentIn 
$Document.text = 'This was updated!' 

Push-OutputBinding -Name InputDocumentOut -Value $Document  
```

<a name="http-trigger-id-query-string-ps"></a>

### <a name="http-trigger-look-up-id-from-query-string"></a>HTTP tetikleyicisi, sorgu dizesinden KIMLIĞI ara

Aşağıdaki örnek, bir Web API 'sinden tek bir Cosmos DB belgesinin nasıl okunacağını ve güncelleşdiğini gösterir. Belgenin benzersiz tanımlayıcısı, bağlamanın özelliğinde tanımlandığı şekilde HTTP isteğinden bir QueryString parametresi aracılığıyla sağlanır `"Id": "{Query.Id}"` .

Cosmos DB girişi bağlama öncelikle işlevin yapılandırma dosyasında bulunan bağlamalar listesinde (_function.jsüzerinde_) listelenir.

```json
{ 
  "bindings": [ 
    { 
      "type": "cosmosDB", 
      "name": "ToDoItem", 
      "databaseName": "ToDoItems", 
      "collectionName": "Items", 
      "connectionStringSetting": "CosmosDBConnection", 
      "direction": "in", 
      "Id": "{Query.id}", 
      "PartitionKey": "{Query.partitionKeyValue}" 
    },
    { 
      "authLevel": "anonymous", 
      "name": "Request", 
      "type": "httpTrigger", 
      "direction": "in", 
      "methods": [ 
        "get", 
        "post" 
      ] 
    }, 
    { 
      "name": "Response", 
      "type": "http", 
      "direction": "out" 
    },
  ], 
  "disabled": false 
} 
```
  
_run.ps1_ dosyası, gelen belgeyi okuyan ve değişiklikleri çıktılar olan PowerShell koduna sahiptir.

```powershell
using namespace System.Net 

param($Request, $ToDoItem, $TriggerMetadata) 

Write-Host 'PowerShell HTTP trigger function processed a request' 

if (-not $ToDoItem) { 
    Write-Host 'ToDo item not found' 

    Push-OutputBinding -Name Response -Value ([HttpResponseContext]@{ 
        StatusCode = [HttpStatusCode]::NotFound 
        Body = $ToDoItem.Description 
    }) 

} else { 

    Write-Host "Found ToDo item, Description=$($ToDoItem.Description)" 
 
    Push-OutputBinding -Name Response -Value ([HttpResponseContext]@{ 
        StatusCode = [HttpStatusCode]::OK 
        Body = $ToDoItem.Description 
    }) 
}
```

<a name="http-trigger-id-route-data-ps"></a>

### <a name="http-trigger-look-up-id-from-route-data"></a>HTTP tetikleyicisi, rota verilerinden KIMLIK arama

Aşağıdaki örnek, bir Web API 'sinden tek bir Cosmos DB belgesinin nasıl okunacağını ve güncelleşdiğini gösterir. Belgenin benzersiz tanımlayıcısı bir rota parametresi aracılığıyla sağlanır. Route parametresi HTTP istek bağlamanın `route` özelliğinde tanımlanır ve Cosmos DB `"Id": "{Id}"` Binding özelliğinde başvurulur.

Cosmos DB girişi bağlama öncelikle işlevin yapılandırma dosyasında bulunan bağlamalar listesinde (_function.jsüzerinde_) listelenir.

```json
{ 
  "bindings": [ 
    { 
      "type": "cosmosDB", 
      "name": "ToDoItem", 
      "databaseName": "ToDoItems", 
      "collectionName": "Items", 
      "connectionStringSetting": "CosmosDBConnection", 
      "direction": "in", 
      "Id": "{id}", 
      "PartitionKey": "{partitionKeyValue}" 
    },
    { 
      "authLevel": "anonymous", 
      "name": "Request", 
      "type": "httpTrigger", 
      "direction": "in", 
      "methods": [ 
        "get", 
        "post" 
      ], 
      "route": "todoitems/{partitionKeyValue}/{id}" 
    }, 
    { 
      "name": "Response", 
      "type": "http", 
      "direction": "out" 
    }
  ], 
  "disabled": false 
} 
```

_run.ps1_ dosyası, gelen belgeyi okuyan ve değişiklikleri çıktılar olan PowerShell koduna sahiptir.

```powershell
using namespace System.Net 

param($Request, $ToDoItem, $TriggerMetadata) 

Write-Host 'PowerShell HTTP trigger function processed a request' 

if (-not $ToDoItem) { 
    Write-Host 'ToDo item not found' 

    Push-OutputBinding -Name Response -Value ([HttpResponseContext]@{ 
        StatusCode = [HttpStatusCode]::NotFound 
        Body = $ToDoItem.Description 
    }) 

} else { 
    Write-Host "Found ToDo item, Description=$($ToDoItem.Description)" 
  
    Push-OutputBinding -Name Response -Value ([HttpResponseContext]@{ 
        StatusCode = [HttpStatusCode]::OK 
        Body = $ToDoItem.Description 
    }) 
} 
```

<a name="queue-trigger-multiple-docs-sqlquery-ps"></a>

### <a name="queue-trigger-get-multiple-docs-using-sqlquery"></a>Kuyruk tetikleyicisi, SqlQuery kullanarak birden çok belge alma

Aşağıdaki örnek, birden çok Cosmos DB belgenin nasıl okunacağını gösterir. İşlevin yapılandırma dosyası (_function.js_), öğesini içeren bağlama özelliklerini tanımlar `sqlQuery` . Özelliği için belirtilen SQL deyimleri, `sqlQuery` işlevine sunulan belge kümesini seçer.

```json
{ 
  "name": "Documents", 
  "type": "cosmosDB", 
  "direction": "in", 
  "databaseName": "MyDb", 
  "collectionName": "MyCollection", 
  "sqlQuery": "SELECT * from c where c.departmentId = {departmentId}", 
  "connectionStringSetting": "CosmosDBConnection" 
} 
```

_Run1.PS_ dosyası, gelen belgeleri okuyan PowerShell koduna sahiptir.

```powershell
param($QueueItem, $Documents, $TriggerMetadata) 

foreach ($Document in $Documents) { 
    # operate on each document 
} 
```

# <a name="python"></a>[Python](#tab/python)

Bu bölüm, çeşitli kaynaklardan bir KIMLIK değeri belirterek tek bir belgeyi okuyan aşağıdaki örnekleri içerir:

* [Kuyruk tetikleyicisi, JSON 'dan KIMLIK arama](#queue-trigger-look-up-id-from-json-python)
* [HTTP tetikleyicisi, sorgu dizesinden KIMLIĞI ara](#http-trigger-look-up-id-from-query-string-python)
* [HTTP tetikleyicisi, rota verilerinden KIMLIK arama](#http-trigger-look-up-id-from-route-data-python)
* [Kuyruk tetikleyicisi, SqlQuery kullanarak birden çok belge alma](#queue-trigger-get-multiple-docs-using-sqlquery-python)

<a id="queue-trigger-look-up-id-from-json-python"></a>

### <a name="queue-trigger-look-up-id-from-json"></a>Kuyruk tetikleyicisi, JSON 'dan KIMLIK arama

Aşağıdaki örnek, bir *function.js* dosyasında bir Cosmos DB girişi bağlamasını ve bağlamayı kullanan bir [Python işlevini](functions-reference-python.md) gösterir. İşlevi tek bir belgeyi okur ve belgenin metin değerini güncelleştirir.

Dosyadaki *function.js* bağlama verileri aşağıda verilmiştir:

```json
{
    "name": "documents",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "id" : "{queueTrigger_payload_property}",
    "partitionKey": "{queueTrigger_payload_property}",
    "connectionStringSetting": "MyAccount_COSMOSDB",
    "direction": "in"
},
{
    "name": "$return",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "createIfNotExists": false,
    "partitionKey": "{queueTrigger_payload_property}",
    "connectionStringSetting": "MyAccount_COSMOSDB",
    "direction": "out"
}
```

[Yapılandırma](#configuration) bölümünde bu özellikler açıklanmaktadır.

Python kodu aşağıda verilmiştir:

```python
import azure.functions as func


def main(queuemsg: func.QueueMessage, documents: func.DocumentList) -> func.Document:
    if documents:
        document = documents[0]
        document['text'] = 'This was updated!'
        return document
```

<a id="http-trigger-look-up-id-from-query-string-python"></a>

### <a name="http-trigger-look-up-id-from-query-string"></a>HTTP tetikleyicisi, sorgu dizesinden KIMLIĞI ara

Aşağıdaki örnekte, tek bir belgeyi alan bir [Python işlevi](functions-reference-python.md) gösterilmektedir. İşlev, aranacak KIMLIĞI ve bölüm anahtarı değerini belirtmek için bir sorgu dizesi kullanan bir HTTP isteği tarafından tetiklenir. Bu KIMLIK ve bölüm anahtarı değeri, `ToDoItem` belirtilen veritabanından ve koleksiyondan bir belge almak için kullanılır.

Dosyada *function.js* :

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    },
    {
      "type": "cosmosDB",
      "name": "todoitems",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connectionStringSetting": "CosmosDBConnection",
      "direction": "in",
      "Id": "{Query.id}",
      "PartitionKey": "{Query.partitionKeyValue}"
    }
  ],
  "scriptFile": "__init__.py"
}
```

Python kodu aşağıda verilmiştir:

```python
import logging
import azure.functions as func


def main(req: func.HttpRequest, todoitems: func.DocumentList) -> str:
    if not todoitems:
        logging.warning("ToDo item not found")
    else:
        logging.info("Found ToDo item, Description=%s",
                     todoitems[0]['description'])

    return 'OK'
```

<a id="http-trigger-look-up-id-from-route-data-python"></a>

### <a name="http-trigger-look-up-id-from-route-data"></a>HTTP tetikleyicisi, rota verilerinden KIMLIK arama

Aşağıdaki örnekte, tek bir belgeyi alan bir [Python işlevi](functions-reference-python.md) gösterilmektedir. İşlev, aranacak KIMLIĞI ve bölüm anahtarı değerini belirtmek için rota verilerini kullanan bir HTTP isteği tarafından tetiklenir. Bu KIMLIK ve bölüm anahtarı değeri, `ToDoItem` belirtilen veritabanından ve koleksiyondan bir belge almak için kullanılır.

Dosyada *function.js* :

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "methods": [
        "get",
        "post"
      ],
      "route":"todoitems/{partitionKeyValue}/{id}"
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    },
    {
      "type": "cosmosDB",
      "name": "todoitems",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connection": "CosmosDBConnection",
      "direction": "in",
      "Id": "{id}",
      "PartitionKey": "{partitionKeyValue}"
    }
  ],
  "disabled": false,
  "scriptFile": "__init__.py"
}
```

Python kodu aşağıda verilmiştir:

```python
import logging
import azure.functions as func


def main(req: func.HttpRequest, todoitems: func.DocumentList) -> str:
    if not todoitems:
        logging.warning("ToDo item not found")
    else:
        logging.info("Found ToDo item, Description=%s",
                     todoitems[0]['description'])
    return 'OK'
```

<a id="queue-trigger-get-multiple-docs-using-sqlquery-python"></a>

### <a name="queue-trigger-get-multiple-docs-using-sqlquery"></a>Kuyruk tetikleyicisi, SqlQuery kullanarak birden çok belge alma

Aşağıdaki örnek, bir *function.js* dosyasında bir Azure Cosmos DB girişi bağlamasını ve bağlamayı kullanan bir [Python işlevini](functions-reference-python.md) gösterir. İşlevi, sorgu parametrelerini özelleştirmek için bir kuyruk tetikleyicisi kullanarak bir SQL sorgusu tarafından belirtilen birden çok belgeyi alır.

Sıra tetikleyicisi bir parametre sağlar `departmentId` . Bir kuyruk iletisi `{ "departmentId" : "Finance" }` , finans departmanı için tüm kayıtları döndürür.

Dosyadaki *function.js* bağlama verileri aşağıda verilmiştir:

```json
{
    "name": "documents",
    "type": "cosmosDB",
    "direction": "in",
    "databaseName": "MyDb",
    "collectionName": "MyCollection",
    "sqlQuery": "SELECT * from c where c.departmentId = {departmentId}",
    "connectionStringSetting": "CosmosDBConnection"
}
```

[Yapılandırma](#configuration) bölümünde bu özellikler açıklanmaktadır.

Python kodu aşağıda verilmiştir:

```python
import azure.functions as func

def main(queuemsg: func.QueueMessage, documents: func.DocumentList):
    for document in documents:
        # operate on each document
```

 ---

## <a name="attributes-and-annotations"></a>Öznitelikler ve ek açıklamalar

# <a name="c"></a>[C#](#tab/csharp)

[C# sınıf kitaplıklarında](functions-dotnet-class-library.md) [cosmosdb](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.CosmosDB/CosmosDBAttribute.cs) özniteliğini kullanın.

Özniteliğin Oluşturucusu, veritabanı adını ve koleksiyon adını alır. Yapılandırabileceğiniz bu ayarlar ve diğer özellikler hakkında daha fazla bilgi için [aşağıdaki yapılandırma bölümüne](#configuration)bakın.

# <a name="c-script"></a>[C# betiği](#tab/csharp-script)

Öznitelikler C# betiği tarafından desteklenmez.

# <a name="java"></a>[Java](#tab/java)

[Java işlevleri çalışma zamanı kitaplığından](/java/api/overview/azure/functions/runtime) `@CosmosDBOutput` Cosmos DB yazılan parametrelerde ek açıklamayı kullanın. Ek açıklama parametre türü olmalıdır `OutputBinding<T>` ; burada `T` yerel bir Java türü ya da Pojo vardır.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Öznitelikler JavaScript tarafından desteklenmez.

# <a name="powershell"></a>[PowerShell](#tab/powershell)

Öznitelikler PowerShell tarafından desteklenmez.

# <a name="python"></a>[Python](#tab/python)

Öznitelikler Python tarafından desteklenmez.

---

## <a name="configuration"></a>Yapılandırma

Aşağıdaki tabloda, dosyasında ve özniteliğinde *function.js* ayarladığınız bağlama yapılandırma özellikleri açıklanmaktadır `CosmosDB` .

|function.jsözelliği | Öznitelik özelliği |Description|
|---------|---------|----------------------|
|**türüyle**     | yok | Olarak ayarlanmalıdır `cosmosDB` .        |
|**Görünüm**     | yok | Olarak ayarlanmalıdır `in` .         |
|**ada**     | yok | İşlevdeki belgeyi temsil eden bağlama parametresinin adı.  |
|**Dosyasında** |**Dosyasında** |Belgeyi içeren veritabanı.        |
|**Ma** |**CollectionName** | Belgeyi içeren koleksiyonun adı. |
|**id**    | **Numarasını** | Alınacak belgenin KIMLIĞI. Bu özellik [bağlama ifadelerin](./functions-bindings-expressions-patterns.md)kullanılmasını destekler. Hem hem de `id` **SQLQuery** özelliklerini ayarlama. Bunlardan birini ayarlamazsanız, tüm koleksiyon alınır. |
|**sqlQuery**  |**SqlQuery**  | Birden çok belge almak için kullanılan bir SQL sorgusu Azure Cosmos DB. Özelliği, şu örnekte olduğu gibi çalışma zamanı bağlamalarını destekler: `SELECT * FROM c where c.departmentId = {departmentId}` . Hem hem de `id` özelliklerini ayarlama `sqlQuery` . Bunlardan birini ayarlamazsanız, tüm koleksiyon alınır.|
|**connectionStringSetting**     |**ConnectionStringSetting**|Azure Cosmos DB Bağlantı dizenizi içeren uygulama ayarının adı. |
|**partitionKey**|**PartitionKey**|Arama için bölüm anahtarı değerini belirtir. Bağlama parametreleri içerebilir. [Bölümlenmiş](../cosmos-db/partitioning-overview.md#logical-partitions) koleksiyonlardaki aramalar için gereklidir.|
|**preferredLocations**| **PreferredLocations**| Seçim Azure Cosmos DB hizmetindeki coğrafi olarak çoğaltılan veritabanı hesapları için tercih edilen konumları (bölgeleri) tanımlar. Değerler virgülle ayrılmalıdır. Örneğin, "Doğu ABD, Orta Güney ABD, Kuzey Avrupa". |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="usage"></a>Kullanım

# <a name="c"></a>[C#](#tab/csharp)

İşlev başarıyla çıktığında, giriş belgesinde adlandırılmış giriş parametreleri aracılığıyla yapılan tüm değişiklikler otomatik olarak kalıcı yapılır.

# <a name="c-script"></a>[C# betiği](#tab/csharp-script)

İşlev başarıyla çıktığında, giriş belgesinde adlandırılmış giriş parametreleri aracılığıyla yapılan tüm değişiklikler otomatik olarak kalıcı yapılır.

# <a name="java"></a>[Java](#tab/java)

[Java işlevleri çalışma zamanı kitaplığından](/java/api/overview/azure/functions/runtime), [@CosmosDBInput](/java/api/com.microsoft.azure.functions.annotation.cosmosdbinput) ek açıklama Cosmos DB verileri işleve gösterir. Bu ek açıklama, kullanılarak yerel Java türleri, POJOs veya null atanabilir değerlerle kullanılabilir `Optional<T>` .

# <a name="javascript"></a>[JavaScript](#tab/javascript)

İşlev çıkışı sırasında güncelleştirmeler otomatik olarak yapılmaz. Bunun yerine, `context.bindings.<documentName>In` `context.bindings.<documentName>Out` güncelleştirmeleri yapmak için ve kullanın. Daha fazla ayrıntı için bkz. [JavaScript örneği](#example) .

# <a name="powershell"></a>[PowerShell](#tab/powershell)

İşlev çıkışından sonra belge güncelleştirmeleri otomatik olarak yapılmaz. Bir işlevdeki belgeleri güncelleştirmek için [Çıkış bağlaması](./functions-bindings-cosmosdb-v2-input.md)kullanın. Daha fazla ayrıntı için bkz. [PowerShell örneği](#example) .

# <a name="python"></a>[Python](#tab/python)

Veriler bir parametre aracılığıyla işlev için kullanılabilir hale getirilir `DocumentList` . Belgede yapılan değişiklikler otomatik olarak kalıcı olmaz.

---

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Cosmos DB bir belge oluşturulduğunda veya değiştirildiğinde (tetikleyici) bir işlev çalıştırma](./functions-bindings-cosmosdb-v2-trigger.md)
- [Azure Cosmos DB belgedeki değişiklikleri kaydetme (çıktı bağlama)](./functions-bindings-cosmosdb-v2-output.md)