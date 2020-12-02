---
title: Oto Rest ile Azure dijital TWINS için özel SDK 'lar oluşturma
titleSuffix: Azure Digital Twins
description: Azure dijital TWINS 'i C# dışındaki dillerde kullanmak için bkz. özel SDK 'lar oluşturma.
author: baanders
ms.author: baanders
ms.date: 4/24/2020
ms.topic: how-to
ms.service: digital-twins
ms.custom: devx-track-js
ms.openlocfilehash: c1dbdc4761c107a8e5028a43ead9710d45526016
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2020
ms.locfileid: "96461179"
---
# <a name="create-custom-sdks-for-azure-digital-twins-using-autorest"></a>Oto Rest kullanarak Azure dijital TWINS için özel SDK 'lar oluşturma

Şu anda, Azure Digital TWINS API 'Leri ile etkileşime yönelik yalnızca yayımlanan veri düzlemi SDK 'Ları .NET (C#), JavaScript ve Java içindir. Bu SDK 'Lar hakkında genel bilgi edinmek için bkz. [*nasıl yapılır: Azure dijital TWINS API 'leri ve SDK 'Larını kullanma*](how-to-use-apis-sdks.md). Başka bir dilde çalışıyorsanız, bu makalede, oto Rest kullanarak kendi veri düzlemi SDK 'sını tercih ettiğiniz dilde nasıl oluşturabileceğiniz gösterilecektir.

>[!NOTE]
> Ayrıca, dilerseniz bir denetim düzlemi SDK 'Sı oluşturmak için, oto Rest 'i de kullanabilirsiniz. Bunu yapmak için, veri düzlemi yerine [Denetim düzlemi Swagger klasöründen](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/digitaltwins/resource-manager/Microsoft.DigitalTwins/) en son **Denetim düzlemi Swagger** (openapı) dosyasını kullanarak bu makaledeki adımları izleyin.

## <a name="set-up-your-machine"></a>Makinenizi kurma

Bir SDK oluşturmak için şunlar gerekir:
* [Oto Rest](https://github.com/Azure/autorest), sürüm 2.0.4413 (sürüm 3 Şu anda desteklenmiyor)
* Otomatik Rest için önkoşul olarak [Node.js](https://nodejs.org)
* [Veri düzlemi Swagger klasöründeki](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/digitaltwins/data-plane/Microsoft.DigitalTwins)en son Azure dijital TWINS **veri düzlemi Swagger** (openapı) dosyası ve buna eşlik eden örnek klasörü.  Swagger dosyasını ve örnekleri klasöründeki *digitaltwins.js* yerel makinenize indirin.

Makinenizde yukarıdaki listeden her şey varsa, SDK 'Yı oluşturmak için oto Rest 'i kullanmaya hazırsınız demektir.

## <a name="create-the-sdk-with-autorest"></a>Oto Rest ile SDK oluşturma 

Node.js yüklüyse, doğru oto Rest sürümünün yüklü olduğundan emin olmak için bu komutu çalıştırabilirsiniz:
```cmd/sh
npm install -g autorest@2.0.4413
```

Azure dijital TWINS Swagger dosyasında oto Rest 'i çalıştırmak için aşağıdaki adımları izleyin:
1. Azure dijital TWINS Swagger dosyasını ve buna eşlik eden örnek klasörünü çalışma dizinine kopyalayın.
2. Bu çalışma dizinine geçiş yapmak için bir komut istemi penceresi kullanın.
3. Aşağıdaki komutla, oto REST komutunu çalıştırın. `<language>`Yer tutucusunu tercih ettiğiniz dille değiştirin: `python` , `java` ,, vb `go` . ( [Oto Rest Benioku](https://github.com/Azure/autorest)dosyasında seçeneklerin tam listesini bulabilirsiniz.)

```cmd/sh
autorest --input-file=digitaltwins.json --<language> --output-folder=ADTApi --add-credentials --azure-arm --namespace=ADTApi
```

Sonuç olarak, çalışma dizininizde *Adtapı* adlı yeni bir klasör görürsünüz. Oluşturulan SDK dosyaları *Adtapı* ad alanına sahip olacaktır. Bu makaledeki kullanım örneklerinin geri kalanı aracılığıyla bu ad alanını kullanmaya devam edersiniz.

Oto Rest, çok çeşitli dil kodu oluşturucuları destekler.

## <a name="add-the-sdk-to-a-visual-studio-project"></a>SDK 'Yı bir Visual Studio projesine ekleme

Doğrudan bir .NET çözümüne, oto Rest tarafından oluşturulan dosyaları dahil edebilirsiniz. Ancak, Azure dijital TWINS SDK 'sını çeşitli ayrı projelere (istemci uygulamalarınız, Azure Işlevleri uygulamalarınız vb.) eklemek isteyeceksiniz. Bu nedenle, oluşturulan dosyalardan ayrı bir proje (.NET sınıf kitaplığı) oluşturmak yararlı olabilir. Daha sonra, bu sınıf kitaplığı projesini bir proje başvurusu olarak çeşitli çözümlere dahil edebilirsiniz.

Bu bölüm, SDK 'nın kendi projesi olan ve diğer projelere dahil olabilen bir sınıf kitaplığı olarak nasıl oluşturulacağı hakkında yönergeler sağlar. Bu adımlar **Visual Studio 'yu** kullanır (en son sürümü [buradan](https://visualstudio.microsoft.com/downloads/)yükleyebilirsiniz).

Adımlar şunlardır:

1. Sınıf kitaplığı için yeni bir Visual Studio çözümü oluşturma
2. Proje adı olarak *Adtapi* kullanın
3. Çözüm Gezgini ' nde, oluşturulan çözümün *Adtapi* projesini sağ seçin ve *> var olan öğe Ekle ' yi seçin...*
4. SDK 'Yı oluşturduğunuz klasörü bulun ve kök düzeyindeki dosyaları seçin
5. "Tamam" düğmesine basın
6. Projeye bir klasör ekleyin (Çözüm Gezgini içinde projeyi sağ seçin ve *> yeni klasör ekle*' yi seçin.
7. Klasör *modellerini* adlandırın
8. Çözüm Gezgini ' nde *modeller* klasörünü sağ seçin ve *> var olan öğeyi Ekle ' yi seçin...*
9. Oluşturulan SDK 'nın *modeller* klasöründeki dosyaları seçin ve "Tamam" düğmesine basın

SDK 'Yı başarılı bir şekilde derlemek için, projenizin bu başvurulara ihtiyacı olacak:
* `Microsoft.Rest.ClientRuntime`
* `Microsoft.Rest.ClientRuntime.Azure`

Bunları eklemek için *NuGet paket yöneticisi > açık araçlar > çözüm Için NuGet Paketlerini Yönet...*.

1. Panelde, *tarayıcı* sekmesinin seçili olduğundan emin olun
2. *Microsoft. Rest* 'i arayın
3. `ClientRuntime`Ve paketlerini seçip `ClientRuntime.Azure` çözümünüze ekleyin

Artık projeyi oluşturabilir ve yazdığınız herhangi bir Azure dijital TWINS uygulamasına bir proje başvurusu olarak ekleyebilirsiniz.

## <a name="general-guidelines-for-generated-sdks"></a>Oluşturulan SDK 'lar için genel yönergeler

Bu bölüm, oluşturulan SDK 'Yı kullanmaya yönelik genel bilgileri ve yönergeleri içerir.

### <a name="synchronous-and-asynchronous-calls"></a>Zaman uyumlu ve zaman uyumsuz çağrılar

Tüm SDK işlevleri, zaman uyumlu ve zaman uyumsuz sürümlerde gelir.

### <a name="typed-and-untyped-data"></a>Türü belirtilmiş ve türsüz veriler

REST API çağrılar genellikle kesin türü belirtilmiş nesneleri döndürür. Ancak Azure dijital TWINS, kullanıcıların TWINS için kendi özel türlerini tanımlamasına izin vermediğinden, birçok Azure dijital TWINS çağrısı için statik dönüş verilerini önceden tanımlamanız mümkün değildir. Bunun yerine, API 'Ler uygun olduğunda türü kesin belirlenmiş sarmalayıcı türleri döndürür ve özel olarak yazılmış ikizi verileri Json.NET nesnelerinde bulunur (API imzalarında "Object" veri türünün göründüğü her yerde kullanılır). Bu nesneleri kodunuzda uygun şekilde çevirebilirsiniz.

### <a name="error-handling"></a>Hata işleme

SDK 'da bir hata oluştuğunda (404 gibi HTTP hataları dahil), SDK bir özel durum oluşturur. Bu nedenle, try/catch blokları içindeki tüm API çağrılarını kapsüllemek önemlidir.

İşte bir ikizi eklemeye ve bu işlemdeki hataları yakamaya çalışan bir kod parçacığı:

```csharp
try
{
    await client.CreateOrReplaceDigitalTwinAsync<BasicDigitalTwin>(id, initData);
    Console.WriteLine($"Created a twin successfully: {id}");
}
catch (ErrorResponseException e)
{
    Console.WriteLine($"*** Error creating twin {id}: {e.Response.StatusCode}"); 
}
```

### <a name="paging"></a>Sayfalama

Oto Rest, SDK için iki tür sayfalama deseni üretir:
* Sorgu API 'SI hariç tüm API 'Ler için bir tane
* Bir sorgu API 'SI için

Sorgu olmayan sayfalama düzeninde, Azure dijital TWINS 'ten gelen giden ilişkilerin disk belleğine alınmış bir listesini almayı gösteren bir kod parçacığı aşağıda verilmiştir:

```csharp
 try 
 {
     // List the relationships.
    AsyncPageable<BasicRelationship> results = client.GetRelationshipsAsync<BasicRelationship>(srcId);
    Console.WriteLine($"Twin {srcId} is connected to:");
    // Iterate through the relationships found.
    int numberOfRelationships = 0;
    await foreach (string rel in results)
    {
         ++numberOfRelationships;
         // Do something with each relationship found
         Console.WriteLine($"Found relationship-{rel.Name}->{rel.TargetId}");
    }
    Console.WriteLine($"Found {numberOfRelationships} relationships on {srcId}");
} catch (RequestFailedException rex) {
    Console.WriteLine($"Relationship retrieval error: {rex.Status}:{rex.Message}");   
}
```

İkinci model yalnızca sorgu API 'SI için oluşturulur. Açıkça bir kullanır `continuationToken` .

Bu düzene sahip bir örnek aşağıda verilmiştir:

```csharp
string query = "SELECT * FROM digitaltwins";
string conToken = null; // continuation token from the query
int page = 0;
try
{
    // Repeat the query while there are pages
    do
    {
        QuerySpecification spec = new QuerySpecification(query, conToken);
        QueryResult qr = await client.Query.QueryTwinsAsync(spec);
        page++;
        Console.WriteLine($"== Query results page {page}:");
        if (qr.Items != null)
        {
            // Query returns are JObjects
            foreach(JObject o in qr.Items)
            {
                string twinId = o.Value<string>("$dtId");
                Console.WriteLine($"  Found {twinId}");
            }
        }
        Console.WriteLine($"== End query results page {page}");
        conToken = qr.ContinuationToken;
    } while (conToken != null);
} catch (ErrorResponseException e)
{
    Console.WriteLine($"*** Error in twin query: ${e.Response.StatusCode}");
}
```

## <a name="next-steps"></a>Sonraki adımlar

SDK 'nizi kullanabileceğiniz bir istemci uygulaması oluşturma adımlarını gözden geçir:
* [*Öğretici: istemci uygulamasını kodlayın*](tutorial-code.md)
