---
title: 'Hızlı başlangıç: Ruby ve Bing Haber Arama REST API bir haber araması gerçekleştirin'
titleSuffix: Azure Cognitive Services
description: Bing Haber Arama REST API Ruby kullanarak bir istek göndermek ve bir JSON yanıtı almak için bu hızlı başlangıcı kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-news-search
ms.topic: quickstart
ms.date: 05/22/2020
ms.author: aahi
ms.custom: seodec2018
ms.openlocfilehash: bd6ba70499a672cc036d7ed249baaefae67776db
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93095484"
---
# <a name="quickstart-perform-a-news-search-using-ruby-and-the-bing-news-search-rest-api"></a>Hızlı başlangıç: Ruby ve Bing Haber Arama REST API kullanarak bir haber arama gerçekleştirme

> [!WARNING]
> Bing Arama API'leri bilişsel hizmetlerden Bing Arama hizmetlere taşınıyor. **30 ekim 2020 ' den** itibaren, [burada](https://aka.ms/cogsvcs/bingmove)belgelenen işlem sonrasında Bing arama yeni örneklerin sağlanması gerekir.
> Bilişsel hizmetler kullanılarak sağlanan Bing Arama API'leri, sonraki üç yıl boyunca veya Kurumsal Anlaşma sonuna kadar, hangisi önce gerçekleşene kadar desteklenecektir.
> Geçiş yönergeleri için bkz. [Bing arama Services](https://aka.ms/cogsvcs/bingmigration).

Bing Haber Arama API'si ilk çağrısını yapmak için bu hızlı başlangıcı kullanın. Bu basit Ruby uygulaması, API 'ye bir arama sorgusu gönderir ve JSON yanıtını işler.

Bu uygulama Ruby dilinde yazılmış olsa da, API çoğu programlama dili ile uyumlu olan bir yenilenmiş Web hizmetidir. 

Bu örneğe ilişkin kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/ruby/Search/BingNewsSearchv7.rb)' da kullanılabilir.

## <a name="prerequisites"></a>Önkoşullar

* Ruby [2,4 veya üzeri](https://www.ruby-lang.org/en/downloads/)

[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](../../../includes/cognitive-services-bing-news-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Uygulamayı oluşturma ve başlatma

1. Aşağıdaki paketleri kod dosyanıza aktarın:

    ```ruby
    require 'net/https'
    require 'uri'
    require 'json'
    ```

2. API uç noktası, haber arama URL 'SI, abonelik anahtarınız ve arama terimiyle ilgili değişkenler oluşturun. Aşağıdaki kodda genel uç noktasını kullanabilir veya kaynağınız için Azure portal görüntülenmiş [özel alt etki alanı](../../cognitive-services/cognitive-services-custom-subdomains.md) uç noktasını kullanabilirsiniz.

    ```ruby
    accessKey = "enter key here"
    uri  = "https://api.cognitive.microsoft.com"
    path = "/bing/v7.0/news/search"
    term = "Microsoft"
    ```

## <a name="format-and-make-an-api-request"></a>API isteğini biçimlendirme ve API isteğinde bulunma

API isteği için bir arama URL 'SI biçimlendirmek üzere önceki adımdaki değişkenleri kullanın. Sonra, isteği gönderin.

```ruby
uri = URI(uri + path + "?q=" + URI.escape(term))
request = Net::HTTP::Get.new(uri)
request['Ocp-Apim-Subscription-Key'] = accessKey
response = Net::HTTP.start(uri.host, uri.port, :use_ssl => uri.scheme == 'https') do |http|
   http.request(request)
end
```

## <a name="process-and-print-the-json-response"></a>JSON yanıtını işleme ve yazdırma

Yanıt alındıktan sonra JSON 'ı ayrıştırır ve sonra hem yanıt gövdesini hem de üst bilgilerini yazdırın.

```ruby
puts "\nRelevant Headers:\n\n"
response.each_header do |key, value|
   # header names are coerced to lowercase
   if key.start_with?("bingapis-") or key.start_with?("x-msedge-") then
      puts key + ": " + value
   end
end
puts "\nJSON Response:\n\n"
puts JSON::pretty_generate(JSON(response.body))
```

## <a name="example-json-response"></a>Örnek JSON yanıtı

Başarılı yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür:

```json
{
   "_type": "News",
   "readLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/news\/search?q=Microsoft",
   "totalEstimatedMatches": 36,
   "sort": [
      {
         "name": "Best match",
         "id": "relevance",
         "isSelected": true,
         "url": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/news\/search?q=Microsoft"
      },
      {
         "name": "Most recent",
         "id": "date",
         "isSelected": false,
         "url": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/news\/search?q=Microsoft&sortby=date"
      }
   ],
   "value": [
      {
         "name": "Microsoft to open flagship London brick-and-mortar retail store",
         "url": "http:\/\/www.contoso.com\/article\/microsoft-to-open-flagshi...",
         "image": {
            "thumbnail": {
               "contentUrl": "https:\/\/www.bing.com\/th?id=ON.F9E4A49EC010417...",
               "width": 220,
               "height": 146
            }
         },
         "description": "After years of rumors about Microsoft opening a brick-and-mortar...", 
         "about": [
           {
             "readLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/entiti...", 
             "name": "Microsoft"
           }, 
           {
             "readLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/entit...", 
             "name": "London"
           }
         ], 
         "provider": [
           {
             "_type": "Organization", 
             "name": "Contoso"
           }
         ], 
          "datePublished": "2017-09-21T21:16:00.0000000Z", 
          "category": "ScienceAndTechnology"
      }, 

      . . .
      
      {
         "name": "Microsoft adds Availability Zones to its Azure cloud platform",
         "url": "https:\/\/contoso.com\/2017\/09\/21\/microsoft-adds-availability...",
         "image": {
            "thumbnail": {
               "contentUrl": "https:\/\/www.bing.com\/th?id=ON.0AE7595B9720...",
               "width": 700,
               "height": 466
            }
         },
         "description": "Microsoft has begun adding Availability Zones to its...",
         "about": [
            {
               "readLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/entities\/a093e9b...",
               "name": "Microsoft"
            },
            {
               "readLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/entities\/cf3abf7d-e379-...",
               "name": "Windows Azure"
            },
            {
               "readLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/entities\/9cdd061c-1fae-d0...",
               "name": "Cloud"
            }
         ],
         "provider": [
            {
               "_type": "Organization",
               "name": "Contoso"
            }
         ],
         "datePublished": "2017-09-21T09:01:00.0000000Z",
         "category": "ScienceAndTechnology"
      }
   ]
}
```

 
## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Tek sayfalı web uygulaması oluşturma](tutorial-bing-news-search-single-page-app.md)
