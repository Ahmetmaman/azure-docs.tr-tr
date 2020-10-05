---
title: Bing Haber Arama C# istemci kitaplığı hızlı başlangıç
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 03/12/2020
ms.author: aahi
ms.openlocfilehash: fbc7d7b35b46e96e9cd007a3794a8ccc561f91e4
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2020
ms.locfileid: "84878912"
---
C# için Bing Haber Arama istemci kitaplığıyla haberleri aramaya başlamak için bu hızlı başlangıcı kullanın. Bing Haber Arama, çoğu programlama dili ile uyumlu bir REST API sahip olsa da, istemci kitaplığı, hizmeti uygulamalarınızla tümleştirmenin kolay bir yolunu sağlar. Bu örneğe ilişkin kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7/BingNewsSearch)' da bulunabilir.

## <a name="prerequisites"></a>Önkoşullar

* Herhangi bir [Visual Studio 2017 veya üzeri](https://www.visualstudio.com/downloads/)sürümü.
* NuGet paketi olarak kullanılabilen [Json.NET](https://www.newtonsoft.com/json) çerçevesi.
* Linux/MacOS kullanıyorsanız bu uygulama, [Mono](https://www.mono-project.com/) kullanılarak çalıştırılabilir.

* [BING haber arama SDK NuGet paketi](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.NewsSearch/2.0.0). Bu paketin yüklenmesi aşağıdakileri de yapar:
    * Microsoft.Rest.ClientRuntime
    * Microsoft.Rest.ClientRuntime.Azure
    * Newtonsoft.Json

Bing Haber Arama istemci kitaplığını kullanarak bir konsol uygulaması ayarlamak için, `Manage NuGet Packages` Visual Studio 'daki Çözüm Gezgini seçeneğine gidin.  `Microsoft.Azure.CognitiveServices.Search.NewsSearch` paketini ekleyin.

[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](~/includes/cognitive-services-bing-news-search-signup-requirements.md)]

## <a name="create-and-initialize-a-project"></a>Proje oluşturma ve başlatma

1. Visual Studio 'da yeni bir C# konsol çözümü oluşturun. Ardından aşağıdakini ana kod dosyasına ekleyin.
    
    ```csharp
    using System;
    using System.Linq;
    using Microsoft.Azure.CognitiveServices.Search.NewsSearch;
    ```

2. Bir arama terimi olan API anahtarınız için bir değişken oluşturun ve ardından haber arama istemcisinin örneğini oluşturun.

    ```csharp
    var key = "YOUR-ACCESS-KEY";
    var searchTerm = "Quantum Computing";
    var client = new NewsSearchClient(new ApiKeyServiceClientCredentials(key));
    ```

## <a name="send-a-request-and-parse-the-result"></a>İstek gönderme ve sonucu ayrıştırma

1. Bing Haber Arama hizmetine bir arama isteği göndermek için istemcisini kullanın:
    ```csharp
    var newsResults = client.News.SearchAsync(query: searchTerm, market: "en-us", count: 10).Result;
    ```

2. Herhangi bir sonuç döndürülürse, bunları ayrıştırın:

    ```csharp
    if (newsResults.Value.Count > 0)
    {
        var firstNewsResult = newsResults.Value[0];
    
        Console.WriteLine($"TotalEstimatedMatches value: {newsResults.TotalEstimatedMatches}");
        Console.WriteLine($"News result count: {newsResults.Value.Count}");
        Console.WriteLine($"First news name: {firstNewsResult.Name}");
        Console.WriteLine($"First news url: {firstNewsResult.Url}");
        Console.WriteLine($"First news description: {firstNewsResult.Description}");
        Console.WriteLine($"First news published time: {firstNewsResult.DatePublished}");
        Console.WriteLine($"First news provider: {firstNewsResult.Provider[0].Name}");
    }
    
    else
    {
        Console.WriteLine("Couldn't find news results!");
    }
    Console.WriteLine("Enter any key to exit...");
    Console.ReadKey();
    ```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Tek sayfalı web uygulaması oluşturma](../../tutorial-bing-news-search-single-page-app.md)
