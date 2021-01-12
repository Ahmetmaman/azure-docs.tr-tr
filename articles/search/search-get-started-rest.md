---
title: "Hızlı başlangıç: REST API 'Leri kullanarak arama dizini oluşturma"
titleSuffix: Azure Cognitive Search
description: Bu REST API hızlı başlangıçta, Postman veya Visual Studio Code kullanarak Azure Bilişsel Arama REST API 'Lerinin nasıl çağrılacağını öğrenin.
zone_pivot_groups: URL-test-interface-rest-apis
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: quickstart
ms.devlang: rest-api
ms.date: 11/17/2020
ms.openlocfilehash: b701a80c3c3b7e5a558d875aca2fb34aea89edff
ms.sourcegitcommit: aacbf77e4e40266e497b6073679642d97d110cda
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2021
ms.locfileid: "98119637"
---
# <a name="quickstart-create-an-azure-cognitive-search-index-using-rest-apis"></a>Hızlı başlangıç: REST API 'Leri kullanarak Azure Bilişsel Arama dizini oluşturma

Bu makalede, istekleri göndermek ve almak için [Azure BILIŞSEL arama REST API 'lerini](/rest/api/searchservice) ve API istemcisini kullanarak REST API isteklerini etkileşimli olarak nasıl ifade edeceğinizi açıklanmaktadır. Bir API istemcisiyle bu yönergelerle, herhangi bir kod yazmadan önce istek gönderebilir ve yanıtları görüntüleyebilirsiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

Makale Postman masaüstü uygulamasını kullanır. Önceden tanımlanmış istekleri kullanmayı tercih ediyorsanız [bir Postman koleksiyonunu indirebilir ve içeri aktarabilirsiniz](https://github.com/Azure-Samples/azure-search-postman-samples/tree/master/Quickstart) .

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıç için aşağıdaki hizmetler ve araçlar gereklidir. 

+ [Postman masaüstü uygulaması](https://www.getpostman.com/) , Azure bilişsel arama istek göndermek için kullanılır.

+ Geçerli aboneliğinizde [bir Azure bilişsel arama hizmeti oluşturun](search-create-service-portal.md) veya [var olan bir hizmeti bulun](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) . Bu hızlı başlangıç için ücretsiz bir hizmet kullanabilirsiniz. 

## <a name="copy-a-key-and-url"></a>Anahtar ve URL 'YI kopyalama

REST çağrıları için her istekte hizmet URL'sinin ve bir erişim anahtarının iletilmesi gerekir. Her ikisiyle de bir arama hizmeti oluşturulur. bu nedenle, aboneliğinize Azure Bilişsel Arama eklediyseniz, gerekli bilgileri almak için aşağıdaki adımları izleyin:

1. [Azure Portal oturum açın](https://portal.azure.com/)ve arama hizmetine **genel bakış** sayfasında URL 'yi alın. Örnek uç nokta `https://mydemo.search.windows.net` şeklinde görünebilir.

1. **Ayarlar**  >  **anahtarlar**' da, hizmette tam haklar için bir yönetici anahtarı alın. Üzerinde bir tane almanız gereken iş sürekliliği için iki adet değiştirilebilir yönetici anahtarı vardır. Nesneleri eklemek, değiştirmek ve silmek için isteklerde birincil veya ikincil anahtarı kullanabilirsiniz.

![HTTP uç noktası ve erişim anahtarı al](media/search-get-started-rest/get-url-key.png "HTTP uç noktası ve erişim anahtarı al")

Tüm istekler hizmetinize gönderilen her istekte bir API anahtarı gerektirir. İstek başına geçerli bir anahtara sahip olmak, isteği gönderen uygulama ve bunu işleyen hizmet arasında güven oluşturur.

## <a name="connect-to-azure-cognitive-search"></a>Azure Bilişsel Arama bağlanma

Bu bölümde, Azure Bilişsel Arama bağlantıları kurmak için tercih ettiğiniz Web aracını kullanın. Her araç oturum için istek üst bilgisi bilgilerini sürdürür, bu da yalnızca API anahtarı ve Içerik türü bir kez girmeniz gereken anlamına gelir.

Her iki araç için de bir komut seçmeniz gerekir (GET, POST, PUT, vb.), bir URL uç noktası sağlamanız ve bazı görevler için isteğin gövdesinde JSON sağlamalısınız. Arama hizmeti adı 'nı (-SEARCH-SERVICE-NAME) geçerli bir değerle değiştirin. `$select=name`Yalnızca her bir dizinin adını döndürecek şekilde ekleyin. 

> `https://<YOUR-SEARCH-SERVICE-NAME>.search.windows.net/indexes?api-version=2020-06-30&$select=name`

HTTPS ön ekine, hizmetin adına, bir nesnenin adına (Bu durumda, dizinler koleksiyonuna) ve [api sürümüne](search-api-versions.md)dikkat edin. Api sürümü, geçerli sürüm için olarak belirtilen, gerekli küçük harfli bir dizedir `?api-version=2020-06-30` . API sürümleri düzenli olarak güncelleştirilir. api-version parametresini her isteğe dahil etmeniz hangisinin kullanıldığıyla ilgili tam denetim sahibi olmanızı sağlar.  

İstek üst bilgisi bileşimi iki öğe içerir: `Content-Type` ve `api-key` Azure bilişsel arama kimlik doğrulaması için kullanılır. Yönetici API anahtarını (-AZURE-SEARCH-ADMIN-API-KEY) geçerli bir değerle değiştirin. 

```http
api-key: <YOUR-AZURE-SEARCH-ADMIN-API-KEY>
Content-Type: application/json
```

Postman 'da, aşağıdaki ekran görüntüsü gibi görünen bir istek girin. Komut olarak **Al** ' ı SEÇIN, URL 'yi sağlayın ve **Gönder**' e tıklayın. Bu komut Azure Bilişsel Arama bağlanır, dizinler koleksiyonunu okur ve başarılı bir bağlantıda HTTP durum kodu 200 döndürür. Hizmetiniz zaten dizinler içeriyorsa, yanıt Dizin tanımlarını da içerir.

![Postman istek URL 'SI ve üstbilgisi](media/search-get-started-rest/postman-url.png "Postman istek URL 'SI ve üstbilgisi")

## <a name="1---create-an-index"></a>1 - Dizin oluşturma

Azure Bilişsel Arama, genellikle dizini verilerle yüklemeden önce oluşturursunuz. [Create ındex REST API](/rest/api/searchservice/create-index) , bu görev için kullanılır. 

URL, dizin adını içerecek şekilde genişletilir `hotels` .

Bunu Postman 'da yapmak için:

1. Komutunu **koymak** için değiştirin.

2. Bu URL 'ye kopyalayın `https://<YOUR-SEARCH-SERVICE-NAME>.search.windows.net/indexes/hotels-quickstart?api-version=2020-06-30` .

3. İsteğin gövdesinde Dizin tanımını (aşağıda, kopyalama öncesi kod verilmiştir) sağlayın.

4. **Gönder**’e tıklayın.

![İstek gövdesinde JSON belgesi dizini oluştur](media/search-get-started-rest/postman-request.png "İstek gövdesinde JSON belgesi dizini oluştur")

### <a name="index-definition"></a>Dizin tanımı

Alanlar koleksiyonu belge yapısını tanımlar. Her belge bu alanlara sahip olmalıdır ve her bir alan bir veri türüne sahip olmalıdır. Dize alanları tam metin aramasında kullanılır. Aranabilir olması için sayısal verilerin olması gerekiyorsa, sayısal verileri dizeler olarak atamalısınız.

Alan öznitelikleri izin verilen eylemi belirler. REST API'leri varsayılan olarak birçok eyleme izin verir. Örneğin tüm dizelerde arama, getirme, filtreleme ve modelleme özellikleri varsayılan olarak etkindir. Genellikle, öznitelikleri yalnızca bir davranışı kapatmanız gerektiğinde ayarlamanız gerekir.

```json
{
    "name": "hotels-quickstart",  
    "fields": [
        {"name": "HotelId", "type": "Edm.String", "key": true, "filterable": true},
        {"name": "HotelName", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": true, "facetable": false},
        {"name": "Description", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": false, "facetable": false, "analyzer": "en.lucene"},
        {"name": "Category", "type": "Edm.String", "searchable": true, "filterable": true, "sortable": true, "facetable": true},
        {"name": "Tags", "type": "Collection(Edm.String)", "searchable": true, "filterable": true, "sortable": false, "facetable": true},
        {"name": "ParkingIncluded", "type": "Edm.Boolean", "filterable": true, "sortable": true, "facetable": true},
        {"name": "LastRenovationDate", "type": "Edm.DateTimeOffset", "filterable": true, "sortable": true, "facetable": true},
        {"name": "Rating", "type": "Edm.Double", "filterable": true, "sortable": true, "facetable": true},
        {"name": "Address", "type": "Edm.ComplexType", 
        "fields": [
        {"name": "StreetAddress", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "searchable": true},
        {"name": "City", "type": "Edm.String", "searchable": true, "filterable": true, "sortable": true, "facetable": true},
        {"name": "StateProvince", "type": "Edm.String", "searchable": true, "filterable": true, "sortable": true, "facetable": true},
        {"name": "PostalCode", "type": "Edm.String", "searchable": true, "filterable": true, "sortable": true, "facetable": true},
        {"name": "Country", "type": "Edm.String", "searchable": true, "filterable": true, "sortable": true, "facetable": true}
        ]
     }
  ]
}
```

Bu isteği gönderdiğinizde dizinin başarıyla oluşturulduğunu belirten HTTP 201 yanıtı almanız gerekir. Bu eylemi portaldan doğrulayabilirsiniz ancak portal sayfasındaki yenileme aralıkları nedeniyle bilgilerin güncellenmesi bir-iki dakika sürebilir.

> [!TIP]
> HTTP 504 yanıtı alırsanız HTTPS'yi belirten URL'yi doğrulayın. HTTP 400 veya 404 yanıtı görürseniz kopyala-yapıştır hatası olmadığını doğrulamak için istek gövdesini kontrol edin. HTTP 403 genelde api anahtarı ile ilgili bir sorunu gösterir (geçersiz anahtar veya api anahtarının nasıl belirtildiğine ilişkin söz dizimi sorunu).

## <a name="2---load-documents"></a>2-belge yükleme

Dizini oluşturma ve dizini doldurma ayrı adımlardır. Azure Bilişsel Arama 'de Dizin, tüm aranabilir verileri içerir. Bu senaryoda, veriler JSON belgeleri olarak sağlanır. Bu görev için [REST API belge ekleme, güncelleştirme veya silme](/rest/api/searchservice/addupdate-or-delete-documents) işlemi kullanılıyor. 

URL, `docs` koleksiyonları ve işlemi içerecek şekilde genişletilir `index` .

Bunu Postman 'da yapmak için:

1. Komutu **Post** olarak değiştirin.

2. Bu URL 'ye kopyalayın `https://<YOUR-SEARCH-SERVICE-NAME>.search.windows.net/indexes/hotels-quickstart/docs/index?api-version=2020-06-30` .

3. İsteğin gövdesinde JSON belgelerini (kopya-Ready kodu aşağıda bulunur) sağlayın.

4. **Gönder**’e tıklayın.

![İstek gövdesinde JSON belgeleri](media/search-get-started-rest/postman-docs.png "İstek gövdesinde JSON belgeleri")

### <a name="json-documents-to-load-into-the-index"></a>Dizine yüklenecek JSON belgeleri

İstek Gövdesi, oteller dizinine eklenecek dört belge içerir.

```json
{
    "value": [
    {
    "@search.action": "upload",
    "HotelId": "1",
    "HotelName": "Secret Point Motel",
    "Description": "The hotel is ideally located on the main commercial artery of the city in the heart of New York. A few minutes away is Time's Square and the historic centre of the city, as well as other places of interest that make New York one of America's most attractive and cosmopolitan cities.",
    "Category": "Boutique",
    "Tags": [ "pool", "air conditioning", "concierge" ],
    "ParkingIncluded": false,
    "LastRenovationDate": "1970-01-18T00:00:00Z",
    "Rating": 3.60,
    "Address": 
        {
        "StreetAddress": "677 5th Ave",
        "City": "New York",
        "StateProvince": "NY",
        "PostalCode": "10022",
        "Country": "USA"
        } 
    },
    {
    "@search.action": "upload",
    "HotelId": "2",
    "HotelName": "Twin Dome Motel",
    "Description": "The hotel is situated in a  nineteenth century plaza, which has been expanded and renovated to the highest architectural standards to create a modern, functional and first-class hotel in which art and unique historical elements coexist with the most modern comforts.",
    "Category": "Boutique",
    "Tags": [ "pool", "free wifi", "concierge" ],
    "ParkingIncluded": false,
    "LastRenovationDate": "1979-02-18T00:00:00Z",
    "Rating": 3.60,
    "Address": 
        {
        "StreetAddress": "140 University Town Center Dr",
        "City": "Sarasota",
        "StateProvince": "FL",
        "PostalCode": "34243",
        "Country": "USA"
        } 
    },
    {
    "@search.action": "upload",
    "HotelId": "3",
    "HotelName": "Triple Landscape Hotel",
    "Description": "The Hotel stands out for its gastronomic excellence under the management of William Dough, who advises on and oversees all of the Hotel’s restaurant services.",
    "Category": "Resort and Spa",
    "Tags": [ "air conditioning", "bar", "continental breakfast" ],
    "ParkingIncluded": true,
    "LastRenovationDate": "2015-09-20T00:00:00Z",
    "Rating": 4.80,
    "Address": 
        {
        "StreetAddress": "3393 Peachtree Rd",
        "City": "Atlanta",
        "StateProvince": "GA",
        "PostalCode": "30326",
        "Country": "USA"
        } 
    },
    {
    "@search.action": "upload",
    "HotelId": "4",
    "HotelName": "Sublime Cliff Hotel",
    "Description": "Sublime Cliff Hotel is located in the heart of the historic center of Sublime in an extremely vibrant and lively area within short walking distance to the sites and landmarks of the city and is surrounded by the extraordinary beauty of churches, buildings, shops and monuments. Sublime Cliff is part of a lovingly restored 1800 palace.",
    "Category": "Boutique",
    "Tags": [ "concierge", "view", "24-hour front desk service" ],
    "ParkingIncluded": true,
    "LastRenovationDate": "1960-02-06T00:00:00Z",
    "Rating": 4.60,
    "Address": 
        {
        "StreetAddress": "7400 San Pedro Ave",
        "City": "San Antonio",
        "StateProvince": "TX",
        "PostalCode": "78216",
        "Country": "USA"
        }
    }
  ]
}
```

Birkaç saniye içinde oturum listesinde bir HTTP 201 yanıtı görmeniz gerekir. Bu, belgelerin başarıyla oluşturulduğunu belirtir. 

207 yanıtı alırsanız en az bir belge karşıya yüklenemedi. 404 yanıtı alırsanız üst bilgi veya istek gövdesinde söz dizimi hatanız vardır. Uç noktayı `/docs/index` içerecek şekilde değiştirdiğinizden emin olun.

> [!Tip]
> Seçili veri kaynaklarında dizin oluşturma için gerekli kodları sadeleştiren ve miktarını azaltan alternatif *dizin oluşturucu* yaklaşımını kullanabilirsiniz. Daha fazla bilgi için bkz. [Dizin oluşturucu işlemleri](/rest/api/searchservice/indexer-operations).


## <a name="3---search-an-index"></a>3 - Dizin arama

Artık bir dizin ve belge kümesi yüklendikten sonra, [arama belgelerini](/rest/api/searchservice/search-documents)kullanarak bunlara yönelik sorgular verebilirsiniz REST API.

URL, arama işleci kullanılarak belirtilen bir sorgu ifadesi içerecek şekilde genişletilir.

Bunu Postman 'da yapmak için:

1. Komutunu **almak** için değiştirin.

2. Bu URL 'ye kopyalayın `https://<YOUR-SEARCH-SERVICE-NAME>.search.windows.net/indexes/hotels-quickstart/docs?search=*&$count=true&api-version=2020-06-30` .

3. **Gönder**’e tıklayın.

Bu sorgu boş ve arama sonuçlarındaki belgelerin sayısını döndürüyor. **Gönder**' e tıkladıktan sonra istek ve yanıt, Postman için aşağıdaki ekran görüntüsüne benzer görünmelidir. Durum kodu 200 olmalıdır.

 ![URL 'de arama dizesiyle al](media/search-get-started-rest/postman-query.png "URL 'de arama dizesiyle al")

Söz dizimi için bir fikir almak üzere birkaç başka sorgu örneği deneyin. Dize araması yapabilir, tam $filter sorgular yapabilir, sonuç kümesini sınırlayabilir, aramanın belirli alanlarla kapsamını ve daha fazlasını yapabilirsiniz.

Geçerli URL 'yi aşağıdaki gibi değiştirin ve sonuçları görüntülemek için her seferinde **Gönder** ' e tıklayın.

```
# Query example 1 - Search on restaurant and wifi
# Return only the HotelName, Description, and Tags fields
https://<YOUR-SEARCH-SERVICE>.search.windows.net/indexes/hotels-quickstart/docs?search=restaurant wifi&$count=true&$select=HotelName,Description,Tags&api-version=2020-06-30

# Query example 2 - Apply a filter to the index to find hotels rated 4 or highter
# Returns the HotelName and Rating. Two documents match
https://<YOUR-SEARCH-SERVICE>.search.windows.net/indexes/hotels-quickstart/docs?search=*&$filter=Rating gt 4&$select=HotelName,Rating&api-version=2020-06-30

# Query example 3 - Take the top two results, and show only HotelName and Category in the results
https://<YOUR-SEARCH-SERVICE>.search.windows.net/indexes/hotels-quickstart/docs?search=boutique&$top=2&$select=HotelName,Category&api-version=2020-06-30

# Query example 4 - Sort by a specific field (Address/City) in ascending order
https://<YOUR-SEARCH-SERVICE>.search.windows.net/indexes/hotels-quickstart/docs?search=pool&$orderby=Address/City asc&$select=HotelName, Address/City, Tags, Rating&api-version=2020-06-30
```

## <a name="get-index-properties"></a>Dizin özelliklerini al

Belge sayılarını ve dizin boyutunu sorgulamak için de [Istatistikleri al](/rest/api/searchservice/get-index-statistics) ' da kullanabilirsiniz: 

```http
https://<YOUR-SEARCH-SERVICE-NAME>.search.windows.net/indexes/hotels-quickstart/stats?api-version=2020-06-30
```

`/stats`URL 'nize ekleme, dizin bilgilerini döndürür. Postman uygulamasında isteğinizin aşağıdakine benzer olması ve yanıtta belge sayısı ile kullanılan alanın bayt cinsinden değerinin belirtilmesi gerekir.

 ![Dizin bilgilerini al](media/search-get-started-rest/postman-system-query.png "Dizin bilgilerini al")

api-version söz diziminin farklı olduğuna dikkat edin. Bu istek için api-version parametresine `?` ekleyin. `?`URL yolunu sorgu dizesinden ayırır, & sorgu dizesindeki her bir ' name = value ' çiftini ayırır. Bu sorgu için api-version, sorgu dizesindeki ilk ve tek öğedir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kendi aboneliğinizde çalışırken, projenin sonunda oluşturduğunuz kaynaklara hala ihtiyacınız olup olmadığını belirlemek iyi bir fikirdir. Çalışır durumda bırakılan kaynaklar maliyetlerin artmasına neden olabilir. Kaynakları teker teker silebilir veya tüm kaynak grubunu silerek kaynak kümesinin tamamını kaldırabilirsiniz.

Sol gezinti bölmesindeki **tüm kaynaklar** veya **kaynak grupları** bağlantısını kullanarak portalda kaynakları bulabilir ve yönetebilirsiniz.

Ücretsiz bir hizmet kullanıyorsanız, üç Dizin, Dizin Oluşturucu ve veri kaynağı ile sınırlı olduğunu unutmayın. Sınırın altında kalmak için portalda ayrı ayrı öğeleri silebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

Temel görevlerin nasıl gerçekleştirileceğini öğrenmiş olduğunuza göre, Dizin oluşturucular veya dizin oluşturma için içerik dönüştürmeleri ekleyen [bir zenginleştirme işlem hattı ayarlama](cognitive-search-tutorial-blob.md) gibi daha gelişmiş özellikler için daha ileri bir REST API çağrısı yapabilirsiniz. Sonraki adımınız için aşağıdaki bağlantıyı öneririz:

> [!div class="nextstepaction"]
> [Öğretici: Azure Bloblarından aranabilir içerik oluşturmak için REST ve AI kullanma](cognitive-search-tutorial-blob.md)