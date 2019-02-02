---
title: "Hızlı Başlangıç: Bing Video arama için SDK'sı kullanarak video arayınC#"
titleSuffix: Azure Cognitive Services
description: Bing Video arama için SDK'sı kullanarak video arama istekleri göndermek için bu hızlı başlangıçta kullanmak C#.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.subservice: bing-video-search
ms.topic: quickstart
ms.date: 01/31/2019
ms.author: rosh
ms.openlocfilehash: 820c6a57a4a8266b68c5a245c2c2cf316aa071a9
ms.sourcegitcommit: ba035bfe9fab85dd1e6134a98af1ad7cf6891033
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2019
ms.locfileid: "55569544"
---
# <a name="quickstart-perform-a-video-search-with-the-bing-video-search-sdk-for-c"></a>Hızlı Başlangıç: Bing Video arama için SDK ile video aramasıC#

Bing Video arama için SDK'sı ile haberler için aramaya başlamak için bu hızlı başlangıçta kullanmak C#. Bing Video arama çoğu programlama dilleri ile uyumlu bir REST API olsa da SDK hizmeti uygulamalarınızla tümleştirmek için kolay bir yol sağlar. Bu örnek için kaynak kodu bulunabilir [GitHub](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7/BingVideoSearch). Bu, daha fazla ek açıklamalar ve özellikleri içerir.

## <a name="prerequisites"></a>Önkoşullar

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)’nin herhangi bir sürümü.
* Kullanılabilir Json.NET framework [bir NuGet paketi olarak](https://www.nuget.org/packages/Newtonsoft.Json/).

Bing Video arama SDK'sını projenize eklemek için Gözat `Manage NuGet Packages` Visual Studio'daki Çözüm Gezgini'nden seçeneği.  `Microsoft.Azure.CognitiveServices.Search.VideoSearch` paketini ekleyin.

Yükleme [[videoyu Search SDK'sı NuGet paketi]](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.VideoSearch/1.2.0) de aşağıdaki bağımlılıkları yükler:

* Microsoft.Rest.ClientRuntime
* Microsoft.Rest.ClientRuntime.Azure
* Newtonsoft.Json

[!INCLUDE [cognitive-services-bing-video-search-signup-requirements](../../../../includes/cognitive-services-bing-video-search-signup-requirements.md)]


## <a name="create-and-initialize-a-project"></a>Proje oluşturma ve başlatma

1. Yeni bir C# çözümünü Visual Studio'da konsol. Ardından ana kod dosyasına aşağıdakileri ekleyin.

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using Microsoft.Azure.CognitiveServices.Search.VideoSearch;
    using Microsoft.Azure.CognitiveServices.Search.VideoSearch.Models;
    ```

2. İstemci yeni bir örneğini `ApiKeyServiceClientCredentials` nesne abonelik anahtarınız ve yapılandırıcının çağrılmasıyla.

    ```csharp
    var client = new VideoSearchAPI(new ApiKeyServiceClientCredentials("YOUR-ACCESS-KEY"));
    ```

## <a name="send-a-search-request-and-process-the-results"></a>Arama isteği göndermek ve sonuçları işlemek

1. İstemci, bir arama isteği göndermek üzere kullanın. "SwiftKey" için arama sorgusu kullanın.

    ```csharp
    var videoResults = client.Videos.SearchAsync(query: "SwiftKey").Result;
    ```

2. Hiçbir sonuç döndürülmedi, birinci ile alma `videoResults.Value[0]`. Ardından, videonun kimliği, başlık ve url yazdırın.

    ```csharp
    if (videoResults.Value.Count > 0)
    {
        var firstVideoResult = videoResults.Value[0];

        Console.WriteLine($"\r\nVideo result count: {videoResults.Value.Count}");
        Console.WriteLine($"First video id: {firstVideoResult.VideoId}");
        Console.WriteLine($"First video name: {firstVideoResult.Name}");
        Console.WriteLine($"First video url: {firstVideoResult.ContentUrl}");
    }
    else
    {
        Console.WriteLine("Couldn't find video results!");
    }
    ```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Tek sayfalı bir web uygulaması oluşturma](../tutorial-bing-video-search-single-page-app.md)

## <a name="see-also"></a>Ayrıca bkz. 

* [Bing Video arama API'si nedir?](../overview.md)
* [Bilişsel Hizmetler .NET SDK örnekleri](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7)
