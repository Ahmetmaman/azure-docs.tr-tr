---
title: Quickstart - C# ile API'ye sorgu gönderme - Bing Yerel İş Arama
titleSuffix: Azure Cognitive Services
description: Azure Bilişsel Hizmeti olan Bing Yerel İş Arama API'sine istek göndermeye başlamak için bu hızlı başlangıcı kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-local-business
ms.topic: quickstart
ms.date: 11/29/2019
ms.author: aahi
ms.openlocfilehash: 2265471001896652a4ce35dbf8bd84aca50000fb
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74665690"
---
# <a name="quickstart-send-a-query-to-the-bing-local-business-search-api-in-c"></a>Hızlı başlatma: C'deki Bing Yerel İşletme Arama API'sine sorgu gönderme #

Azure Bilişsel Hizmeti olan Bing Yerel İş Arama API'sine istek göndermeye başlamak için bu hızlı başlangıcı kullanın. Bu basit uygulama C# olarak yazılmış olsa da, API, HTTP isteklerini yapma ve JSON'u ayrıştırma yeteneğine sahip herhangi bir programlama diliyle uyumlu bir RESTful Web hizmetidir.

Bu örnek uygulama, arama sorgusu `hotel in Bellevue`için API'den yerel yanıt verilerini alır.

## <a name="prerequisites"></a>Ön koşullar

* [Visual Studio 2019](https://www.visualstudio.com/downloads/)herhangi bir baskı .
* Linux/MacOS kullanıyorsanız bu uygulama, [Mono](https://www.mono-project.com/) kullanılarak çalıştırılabilir.

Bing Arama API'lerine sahip bir [Bilişsel Hizmetler API hesabınız](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) olması gerekir. [Ücretsiz deneme](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) bu hızlı başlangıç için yeterlidir.  Ayrıca bakınız [Bilişsel Hizmetler Fiyatlandırma - Bing Arama API.](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/)

## <a name="create-the-request"></a>İstek oluşturma 

Aşağıdaki kod, `WebRequest`erişim anahtarı üstbilgisini ayarlar ve "Bellevue'deki restoran" için bir sorgu dizesi ekler.  Ardından isteği gönderir ve yanıtı JSON metnini içeren bir dizeye atar.

```csharp
    // Replace the accessKey string value with your valid access key.
    const string accessKey = "enter key here";

    const string uriBase = "https://api.cognitive.microsoft.com/bing/v7.0/localbusinesses/search";   

    const string searchTerm = "restaurant in Bellevue";
    // Construct the URI of the search request
    var uriQuery = uriBase + "?q=" + Uri.EscapeDataString(searchQuery) + mkt=en-us;

    // Run the Web request and get response.
    WebRequest request = HttpWebRequest.Create(uriQuery);
    request.Headers["Ocp-Apim-Subscription-Key"] = accessKey; 

    HttpWebResponse response = (HttpWebResponse)request.GetResponseAsync().Result;
    string json = new StreamReader(response.GetResponseStream()).ReadToEnd();
```

## <a name="run-the-complete-application"></a>Uygulamanın tamamını çalıştırın

Bing Yerel İşletme Arama API'si, Bing arama motorunun yerelleştirilmiş arama sonuçlarını döndürür.
1. Visual Studio'da (Community Edition uygundur) yeni bir Konsol çözümü oluşturun.
2. Program.cs dosyasını aşağıda sağlanan kod ile değiştirin.
3. AccessKey değerini aboneliğiniz için geçerli bir erişim anahtarıyla değiştirin.
4. Programı çalıştırın.

```csharp
using System;
using System.Collections.Generic;
using System.Text;
using System.Net;
using System.IO;

namespace localSearch
{
    class Program
    {
        // **********************************************
        // *** Update or verify the following values. ***
        // **********************************************

        // Replace the accessKey string value with your valid access key.
        const string accessKey = "enter key here";

        const string uriBase = "https://api.cognitive.microsoft.com/bing/v7.0/localbusinesses/search";   

        const string searchTerm = "restaurant in Bellevue";

        // Returns search results including relevant headers
        struct SearchResult
        {
            public String jsonResult;
            public Dictionary<String, String> relevantHeaders;
        }

        static void Main()
        {
            Console.OutputEncoding = System.Text.Encoding.UTF8;
            Console.WriteLine("Searching locally for: " + searchTerm);

            SearchResult result = BingLocalSearch(searchTerm, accessKey);

            Console.WriteLine("\nRelevant HTTP Headers:\n");
            foreach (var header in result.relevantHeaders)
                Console.WriteLine(header.Key + ": " + header.Value);

            Console.WriteLine("\nJSON Response:\n");
            Console.WriteLine(JsonPrettyPrint(result.jsonResult));

            Console.Write("\nPress Enter to exit ");
            Console.ReadLine();
        }

        /// <summary>
        /// Performs a Bing Local business search and return the results as a SearchResult.
        /// </summary>
        static SearchResult BingLocalSearch(string searchQuery, string key)
        {
            // Construct the URI of the search request
            var uriQuery = uriBase + "?q=" + Uri.EscapeDataString(searchQuery) + "&mkt=en-us";

            // Perform the Web request and get the response
            WebRequest request = HttpWebRequest.Create(uriQuery);
            request.Headers["Ocp-Apim-Subscription-Key"] = key; 

            HttpWebResponse response = (HttpWebResponse)request.GetResponseAsync().Result;
            string json = new StreamReader(response.GetResponseStream()).ReadToEnd();

            // Create result object for return
            var searchResult = new SearchResult();
            searchResult.jsonResult = json;
            searchResult.relevantHeaders = new Dictionary<String, String>();

            // Extract Bing HTTP headers
            foreach (String header in response.Headers)
            {
                if (header.StartsWith("BingAPIs-") || header.StartsWith("X-MSEdge-"))
                    searchResult.relevantHeaders[header] = response.Headers[header];
            }

            return searchResult;
        }

        /// <summary>
        /// Formats the given JSON string by adding line breaks and indents.
        /// </summary>
        /// <param name="json">The raw JSON string to format.</param>
        /// <returns>The formatted JSON string.</returns>
        static string JsonPrettyPrint(string json)
        {
            if (string.IsNullOrEmpty(json))
                return string.Empty;

            json = json.Replace(Environment.NewLine, "").Replace("\t", "");

            StringBuilder sb = new StringBuilder();
            bool quote = false;
            bool ignore = false;
            int offset = 0;
            int indentLength = 3;

            foreach (char ch in json)
            {
                switch (ch)
                {
                    case '"':
                        if (!ignore) quote = !quote;
                        break;
                    case '\'':
                        if (quote) ignore = !ignore;
                        break;
                }

                if (quote)
                    sb.Append(ch);
                else
                {
                    switch (ch)
                    {
                        case '{':
                        case '[':
                            sb.Append(ch);
                            sb.Append(Environment.NewLine);
                            sb.Append(new string(' ', ++offset * indentLength));
                            break;
                        case '}':
                        case ']':
                            sb.Append(Environment.NewLine);
                            sb.Append(new string(' ', --offset * indentLength));
                            sb.Append(ch);
                            break;
                        case ',':
                            sb.Append(ch);
                            sb.Append(Environment.NewLine);
                            sb.Append(new string(' ', offset * indentLength));
                            break;
                        case ':':
                            sb.Append(ch);
                            sb.Append(' ');
                            break;
                        default:
                            if (ch != ' ') sb.Append(ch);
                            break;
                    }
                }
            }

            return sb.ToString().Trim();
        }
    }
}

```

## <a name="next-steps"></a>Sonraki adımlar
- [Yerel İş Arama Java quickstart](local-search-java-quickstart.md)
- [Yerel İş Arama Düğümü hızlı başlat](local-search-node-quickstart.md)
- [Yerel İş Arama Python quickstart](local-search-python-quickstart.md)
