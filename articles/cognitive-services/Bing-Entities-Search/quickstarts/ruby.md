---
title: 'Hızlı başlangıç: Ruby-Bing Varlık Arama kullanarak REST API bir arama isteği gönderme'
titleSuffix: Azure Cognitive Services
description: Bing Varlık Arama REST API Ruby kullanarak bir istek göndermek ve bir JSON yanıtı almak için bu hızlı başlangıcı kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-entity-search
ms.topic: quickstart
ms.date: 05/08/2020
ms.author: aahi
ms.openlocfilehash: 20eec55a7a875ff124737069d699e76674b27c3b
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93084655"
---
# <a name="quickstart-send-a-search-request-to-the-bing-entity-search-rest-api-using-ruby"></a>Hızlı başlangıç: Ruby kullanarak Bing Varlık Arama REST API arama isteği gönderme

> [!WARNING]
> Bing Arama API'leri bilişsel hizmetlerden Bing Arama hizmetlere taşınıyor. **30 ekim 2020 ' den** itibaren, [burada](https://aka.ms/cogsvcs/bingmove)belgelenen işlem sonrasında Bing arama yeni örneklerin sağlanması gerekir.
> Bilişsel hizmetler kullanılarak sağlanan Bing Arama API'leri, sonraki üç yıl boyunca veya Kurumsal Anlaşma sonuna kadar, hangisi önce gerçekleşene kadar desteklenecektir.
> Geçiş yönergeleri için bkz. [Bing arama Services](https://aka.ms/cogsvcs/bingmigration).

Bing Varlık Arama API'si ilk çağrısını yapmak ve JSON yanıtını görüntülemek için bu hızlı başlangıcı kullanın. Bu basit Ruby uygulaması, API 'ye bir haber arama sorgusu gönderir ve yanıtı görüntüler. Bu uygulamanın kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/ruby/Search/BingEntitySearchv7.rb)' da kullanılabilir.

Bu uygulama Ruby dilinde yazılmış olsa da, API çoğu programlama dili ile uyumlu olan bir yenilenmiş Web hizmetidir.

## <a name="prerequisites"></a>Önkoşullar

* [Ruby 2,4](https://www.ruby-lang.org/en/downloads/) veya üzeri.

[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](../../../../includes/cognitive-services-bing-entity-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Uygulamayı oluşturma ve başlatma

1. En sevdiğiniz IDE veya kod düzenleyicide, bir haber Ruby dosyası oluşturun ve aşağıdaki paketleri içeri aktarın:

    ```ruby
    require 'net/https'
    require 'cgi'
    require 'json'
    ```

2. API uç noktanız, haber arama URL 'niz, abonelik anahtarınız ve arama sorgunuz için değişkenler oluşturun. Aşağıdaki kodda genel uç noktasını kullanabilir veya kaynağınız için Azure portal görüntülenmiş [özel alt etki alanı](../../../cognitive-services/cognitive-services-custom-subdomains.md) uç noktasını kullanabilirsiniz.
    
    ```ruby
    host = 'https://api.cognitive.microsoft.com'
    path = '/bing/v7.0/entities'
    
    mkt = 'en-US'
    query = 'italian restaurants near me'
    ```

## <a name="format-and-make-an-api-request"></a>API isteğini biçimlendirme ve API isteğinde bulunma

1. Market değişkeninizi parametresine ekleyerek isteğiniz için parametreler dizesini oluşturun `?mkt=` . Sorgunuzu kodlayın ve `&q=` parametreye ekleyin. API konağını, yolunu ve isteğinizi ve parametreleri bir URI nesnesi olarak atayın.

    ```ruby
    params = '?mkt=' + mkt + '&q=' + CGI.escape(query)
    uri = URI (host + path + params)
    ```

2. İsteği oluşturmak için son adımdaki değişkenleri kullanın. Abonelik anahtarınızı `Ocp-Apim-Subscription-Key` üstbilgiye ekleyin.

    ```ruby
    request = Net::HTTP::Get.new(uri)
    request['Ocp-Apim-Subscription-Key'] = subscriptionKey
    ```

3. İsteği gönderin ve yanıtı yazdırın.

    ```ruby
    response = Net::HTTP.start(uri.host, uri.port, :use_ssl => uri.scheme == 'https') do |http|
        http.request (request)
    end

    puts JSON::pretty_generate (JSON (response.body))
    ```

## <a name="example-json-response"></a>Örnek JSON yanıtı

Başarılı yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür: 

```json
{
  "_type": "SearchResponse",
  "queryContext": {
    "originalQuery": "italian restaurant near me",
    "askUserForLocation": true
  },
  "places": {
    "value": [
      {
        "_type": "LocalBusiness",
        "webSearchUrl": "https://www.bing.com/search?q=sinful+bakery&filters=local...",
        "name": "Liberty's Delightful Sinful Bakery & Cafe",
        "url": "https://www.contoso.com/",
        "entityPresentationInfo": {
          "entityScenario": "ListItem",
          "entityTypeHints": [
            "Place",
            "LocalBusiness"
          ]
        },
        "address": {
          "addressLocality": "Seattle",
          "addressRegion": "WA",
          "postalCode": "98112",
          "addressCountry": "US",
          "neighborhood": "Madison Park"
        },
        "telephone": "(800) 555-1212"
      },

      . . .
      {
        "_type": "Restaurant",
        "webSearchUrl": "https://www.bing.com/search?q=Pickles+and+Preserves...",
        "name": "Munson's Pickles and Preserves Farm",
        "url": "https://www.princi.com/",
        "entityPresentationInfo": {
          "entityScenario": "ListItem",
          "entityTypeHints": [
            "Place",
            "LocalBusiness",
            "Restaurant"
          ]
        },
        "address": {
          "addressLocality": "Seattle",
          "addressRegion": "WA",
          "postalCode": "98101",
          "addressCountry": "US",
          "neighborhood": "Capitol Hill"
        },
        "telephone": "(800) 555-1212"
      },
      
      . . .
    ]
  }
}
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Tek sayfalı web uygulaması oluşturma](../tutorial-bing-entities-search-single-page-app.md)

* [Bing Varlık Arama API'si nedir?](../search-the-web.md)
* [Bing varlık arama API'si Başvurusu](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference).
