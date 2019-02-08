---
title: "Hızlı Başlangıç: Bing görsel arama için SDK'sı kullanarak görüntü Öngörüler elde edinC#"
titleSuffix: Azure Cognitive Services
description: Bing görsel arama SDK'sını kullanarak bir görüntüyü karşıya yüklemeyi öğrenin ve ilgili Öngörüler elde edin.
services: cognitive-services
author: mikedodaro
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: quickstart
ms.date: 05/16/2018
ms.author: v-gedod
ms.openlocfilehash: 3c6a103d3d4c1890a58b833d29c88eb040ac9810
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55860703"
---
# <a name="quickstart-get-image-insights-using-the-bing-visual-search-sdk-for-c"></a>Hızlı Başlangıç: Bing görsel arama için SDK'sı kullanarak görüntü Öngörüler elde edinC#

Bing görsel arama hizmetinden görüntü öngörü alma başlamak için bu hızlı başlangıçta kullanmak kullanarak C# SDK. Bing görsel arama çoğu programlama dilleri ile uyumlu bir REST API olsa da SDK hizmeti uygulamalarınızla tümleştirmek için kolay bir yol sağlar. Bu örnek için kaynak kodu bulunabilir [GitHub](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7/BingVisualSearch).

## <a name="prerequisites"></a>Önkoşullar

* [Visual Studio 2017](https://www.visualstudio.com/downloads/)’nin herhangi bir sürümü.
* Linux/MacOS kullanıyorsanız bu uygulama, [Mono](http://www.mono-project.com/) kullanılarak çalıştırılabilir.
* Görsel arama NuGet paketi. 
    - Visual Studio'daki Çözüm Gezgini'nde projenize sağ tıklayıp menüden `Manage NuGet Packages` öğesini seçin. `Microsoft.Azure.CognitiveServices.Search.VisualSearch` paketini yükleyin. NuGet paketlerini yükleme Ayrıca aşağıdakileri yükler:
        - Microsoft.Rest.ClientRuntime
        - Microsoft.Rest.ClientRuntime.Azure
        - Newtonsoft.Json


[!INCLUDE [cognitive-services-bing-image-search-signup-requirements](../../../includes/cognitive-services-bing-image-search-signup-requirements.md)]

<a name="client"></a>

## <a name="create-and-initialize-the-application"></a>Uygulamayı oluşturma ve başlatma

1. Visual Studio'da yeni bir proje oluşturun. Ardından aşağıdaki yönergelerini ekleyin.
    
    ```csharp
    using Microsoft.Azure.CognitiveServices.Search.VisualSearch;
    using Microsoft.Azure.CognitiveServices.Search.VisualSearch.Models;
    ```

2. Abonelik anahtarınızla istemci örneği.
    
    ```csharp
    var client = new VisualSearchClient(new ApiKeyServiceClientCredentials("YOUR-ACCESS-KEY"));
    ```
    
## <a name="send-a-search-request"></a>Arama isteği gönder 

1. Oluşturma bir `FileStream` görüntüleriniz için (Bu durumda `TestImages/image.jpg`). Ardından kullanarak bir arama isteği göndermek için istemci kullanmak `client.Images.VisualSearchMethodAsync()`. 
    
    ```csharp
     System.IO.FileStream stream = new FileStream(Path.Combine("TestImages", "image.jpg"), FileMode.Open);
     // The knowledgeRequest parameter is not required if an image binary is passed in the request body
     var visualSearchResults = client.Images.VisualSearchMethodAsync(image: stream, knowledgeRequest: (string)null).Result;
    ```
    
2. Önceki sorgunun sonuçlarını ayrıştırın:

    ```csharp
    // Visual Search results
    if (visualSearchResults.Image?.ImageInsightsToken != null)
    {
        Console.WriteLine($"Uploaded image insights token: {visualSearchResults.Image.ImageInsightsToken}");
    }
    else
    {
        Console.WriteLine("Couldn't find image insights token!");
    }
    
    // List of tags
    if (visualSearchResults.Tags.Count > 0)
    {
        var firstTagResult = visualSearchResults.Tags[0];
        Console.WriteLine($"Visual search tag count: {visualSearchResults.Tags.Count}");
    
        // List of actions in first tag
        if (firstTagResult.Actions.Count > 0)
        {
            var firstActionResult = firstTagResult.Actions[0];
            Console.WriteLine($"First tag action count: {firstTagResult.Actions.Count}");
            Console.WriteLine($"First tag action type: {firstActionResult.ActionType}");
        }
        else
        {
            Console.WriteLine("Couldn't find tag actions!");
        }
    }
    ```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Tek sayfa uygulaması oluşturma](tutorial-bing-visual-search-single-page-app.md)
