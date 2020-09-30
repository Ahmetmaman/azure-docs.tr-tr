---
title: Azure Bilişsel Arama yenilikleri
description: Azure Bilişsel Arama Azure Search hizmet yeniden adlandırma özelliği de dahil olmak üzere yeni ve geliştirilmiş özelliklerin duyuruları.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: overview
ms.date: 09/22/2020
ms.custom: references_regions
ms.openlocfilehash: 7714ec29b3cbe17c7700b48111ea2b455aa18b7e
ms.sourcegitcommit: f5580dd1d1799de15646e195f0120b9f9255617b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/29/2020
ms.locfileid: "91532236"
---
# <a name="whats-new-in-azure-cognitive-search"></a>Azure Bilişsel Arama yenilikleri

Hizmette nelerin yeni olduğunu öğrenin. Hizmette güncel kalmasını sağlamak için bu sayfaya yer işareti ekleyin.

## <a name="september-2020"></a>Eylül 2020

Azure Active Directory ' de bir arama hizmeti için kimlik oluşturun ve ardından Azure veri kaynaklarına yönelik salt okuma izinleri vermek için RBAC izinlerini kullanın. İsteğe bağlı olarak, IP kuralları bir seçenek değilse, [Güvenilen hizmet özel durum](search-indexer-howto-access-trusted-service-exception.md) özelliğini seçin.


|Özellik&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  | Kategori | Açıklama | Kullanılabilirlik  |
|------------------------------|----------|-------------|---------------|
| [Yönetilen hizmet kimliği](search-howto-managed-identities-data-sources.md) | Dizin oluşturucular, güvenlik | Azure Active Directory ' de bir arama hizmeti için kimlik oluşturun ve Azure veri kaynaklarına erişim sağlamak için RBAC izinlerini kullanın. Bu yaklaşım bağlantı dizesinde kimlik bilgileri gereksinimini ortadan kaldırır. <br><br>Yönetilen hizmet kimliğini kullanmanın ek bir yolu, IP kurallarının bir seçenek olmaması durumunda [güvenilir bir hizmet özel durumdur](search-indexer-howto-access-trusted-service-exception.md) . | Genel olarak kullanılabilir. Portalı kullanırken bu özelliğe erişin veya API-Version = 2020-06-30 ile [veri kaynağı (REST) oluşturun](https://docs.microsoft.com/rest/api/searchservice/create-data-source) . |
| [Özel bağlantı kullanan giden istekler](search-indexer-howto-access-private.md) | Dizin oluşturucular, güvenlik | Dizin oluşturucularının Azure özel bağlantısı tarafından güvenliği sağlanmış Azure kaynaklarına erişirken kullanabileceği bir paylaşılan özel bağlantı kaynağı oluşturun. Dizin Oluşturucu bağlantılarını güvenli hale getirmek için kullanabileceğiniz tüm yollar hakkında daha fazla bilgi için bkz. [Azure ağ güvenliği özelliklerini kullanarak Dizin Oluşturucu kaynakları güvenli hale getirme](search-indexer-securing-resources.md). | Genel olarak kullanılabilir. Portal veya [paylaşılan özel bağlantı kaynağını](https://docs.microsoft.com/rest/api/searchmanagement/sharedprivatelinkresources) api-Version = 2020-08-01 ile kullanırken bu özelliğe erişin. |
| [Yönetim REST API (2020-08-01)](https://docs.microsoft.com/rest/api/searchmanagement/management-api-versions) | REST | Yeni kararlı REST API, paylaşılan özel bağlantı kaynakları oluşturmak için destek ekler. | Genel olarak kullanılabilir. |
| [Yönetim REST API (2020-08-01-Önizleme)](https://docs.microsoft.com/rest/api/searchmanagement/management-api-versions) | REST | Azure Işlevleri için paylaşılan özel bağlantı kaynağı ve MySQL için Azure SQL veritabanları ekler. | Genel Önizleme. |
| [Yönetim .NET SDK 4,0](https://docs.microsoft.com/dotnet/api/overview/azure/search/management) | .NET SDK | 2020-08-01 sürümünü hedefleyen Yönetim SDK 'Sı için Azure SDK güncelleştirmesi REST API. | Genel olarak kullanılabilir. |

## <a name="august-2020"></a>Ağustos 2020

|Özellik&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  | Kategori | Açıklama | Kullanılabilirlik  |
|---------|------------------|-------------|---------------|
| [Çift şifreleme](search-security-overview.md#encryption) | Güvenlik | Yeni Arama hizmetlerinde müşteri tarafından yönetilen anahtar (CMK) şifrelemesini yapılandırarak depolama katmanında çift şifrelemeyi etkinleştirin. Yeni bir hizmet oluşturun,, [müşteri tarafından yönetilen anahtarları](search-security-manage-encryption-keys.md) dizinlere veya eş anlamlı Maps 'a yapılandırın ve uygulayın ve bu içerik üzerinde çift şifrelemeden yararlanın. | 1 Ağustos 2020 ' den sonra oluşturulan tüm Arama hizmetlerinde, bu bölgelerde genel kullanıma sunulmuştur: Batı ABD 2, Doğu ABD, Orta Güney ABD, US Gov Virginia, US Gov Arizona. Hizmeti oluşturmak için Portal, yönetim REST API 'Leri veya SDK 'Ları kullanın. |

## <a name="july-2020"></a>Temmuz 2020

|Özellik&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  | Kategori | Açıklama | Kullanılabilirlik  |
|---------|------------------|-------------|---------------|
| [Azure.Search.Documstalar istemci kitaplığı](/dotnet/api/overview/azure/search.documents-readme) | .NET için Azure SDK | Azure SDK ekibi tarafından yayınlanan ve diğer .NET istemci kitaplıkları ile tutarlılık için tasarlanan .NET istemci kitaplığı. <br/><br/>Sürüm 11, arama REST API-Version = 2020-06-30 ' ı hedefler, ancak henüz bilgi deposu, Jeo-uzamsal türler veya [FieldBuilder](/dotnet/api/microsoft.azure.search.fieldbuilder)'ı desteklemez. <br/><br/>Daha fazla bilgi için bkz.  [hızlı başlangıç: Dizin oluşturma](search-get-started-dotnet.md) ve [Azure.Search.Documtları yükseltme (v11)](search-dotnet-sdk-migration-version-11.md). | Genel olarak kullanılabilir. </br> [Azure.Search.Documstaları paketini](https://www.nuget.org/packages/Azure.Search.Documents/) NuGet 'ten yüklersiniz. |
| [azure.search.documstalar istemci kitaplığı](/python/api/overview/azure/search-documents-readme)  | Python için Azure SDK| Azure SDK ekibi tarafından yayınlanan Python istemci kitaplığı, diğer Python istemci kitaplıkları ile tutarlılık için tasarlanmıştır. <br/><br/>Sürüm 11 ' de arama REST API 'si-sürüm = 2020-06-30 hedefdedir. | Genel olarak kullanılabilir. </br> Pypı 'den [Azure-Search-Documents paketini](https://pypi.org/project/azure-search-documents/) yükler. |
| [@azure/search-documents istemci kitaplığı](/javascript/api/overview/azure/search-documents-readme)  | JavaScript için Azure SDK | Azure SDK ekibi tarafından yayınlanan JavaScript istemci kitaplığı, diğer JavaScript istemci kitaplıkları ile tutarlılık için tasarlanmıştır. <br/><br/>Sürüm 11 ' de arama REST API 'si-sürüm = 2020-06-30 hedefdedir. | Genel olarak kullanılabilir. </br> [ @azure/search-documents Paketi](https://www.npmjs.com/package/@azure/search-documents) NPM 'den yükleyeceksiniz. |

## <a name="june-2020"></a>Haziran 2020

|Özellik&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  | Kategori | Açıklama | Kullanılabilirlik  |
|---------|------------------|-------------|---------------|
[**Bilgi deposu**](knowledge-store-concept-intro.md) | Yapay zeka zenginleştirme | Bir AI zenginleştirme dizin oluşturucunun çıkışı, Azure depolama 'da içeriği diğer uygulamalarda ve işlemlerde kullanılmak üzere depolar. | Genel olarak kullanılabilir. </br> [Arama REST API 2020-06-30](/rest/api/searchservice/) veya üzerini ya da portalını kullanın. |
| [**REST API ara 2020-06-30**](/rest/api/searchservice/) | REST | REST API 'lerin yeni kararlı bir sürümü. Bilgi deposuna ek olarak, bu sürüm arama ilgisi ve Puanlama geliştirmeleri içerir. | Genel olarak kullanılabilir. |
| [**Okapi BM25 ilgi algoritması**](https://en.wikipedia.org/wiki/Okapi_BM25) | Sorgu | Yeni ilgi derecelendirme algoritması, 15 Temmuz 'dan sonra oluşturulan tüm yeni arama hizmetleri için otomatik olarak kullanılır. Daha önce oluşturulan hizmetler için `similarity` Dizin alanları üzerinde özelliğini ayarlayarak kabul edebilirsiniz. | Genel olarak kullanılabilir. </br> [Arama REST API 2020-06-30](/rest/api/searchservice/) veya üstünü ya da REST API 2019-05-06 kullanın. |
| **executionEnvironment** | Güvenlik (Dizin oluşturucular) | `private`Özel bir uç nokta üzerinden dış veri kaynaklarına yönelik tüm bağlantıları zorlamak için bu Dizin Oluşturucu yapılandırma özelliğini olarak ayarlayın. Yalnızca Azure özel bağlantı özelliğinden yararlanan Arama Hizmetleri için geçerlidir. | Genel olarak kullanılabilir. </br> Bu genel yapılandırma parametresini ayarlamak için [arama REST API 2020-06-30](/rest/api/searchservice/) ' i kullanın. |

## <a name="may-2020-microsoft-build"></a>Mayıs 2020 (Microsoft derleme)

|Özellik&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  | Kategori | Açıklama | Kullanılabilirlik  |
|---------|------------------|-------------|---------------|
| [**Hata ayıklama oturumları**](cognitive-search-debug-session.md) | Yapay zeka zenginleştirme | Hata ayıklama oturumları, mevcut bir beceri sorunları araştırmak ve çözmek için portal tabanlı bir arabirim sağlar. Hata ayıklama oturumunda oluşturulan düzeltmeler üretim becerileri kaydedilebilir. [Bu öğreticiyi](cognitive-search-tutorial-debug-sessions.md)kullanmaya başlayın. | Genel Önizleme, portalda. |
| [**Bağlantılı güvenlik duvarı desteği için IP kuralları**](service-configure-firewall.md) | Güvenlik | Arama hizmeti uç noktasına erişimi belirli IP adresleriyle sınırlayın. | Genel olarak kullanılabilir. </br> [Yönetim REST API 2020-03-13](/rest/api/searchmanagement/) veya üstünü ya da portalını kullanın. |
| [**Özel arama uç noktası için Azure özel bağlantısı**](service-create-private-endpoint.md) | Güvenlik| Yalnızca istemci uygulamaları ve aynı sanal ağdaki diğer Azure hizmetleri tarafından erişilebilen bir özel bağlantı kaynağı olarak çalıştırarak, genel İnternet 'ten bir arama hizmetini koruyun. | Genel olarak kullanılabilir. </br> [Yönetim REST API 2020-03-13](/rest/api/searchmanagement/) veya üstünü ya da portalını kullanın. |
| [**sistem tarafından yönetilen kimlik (Önizleme)**](search-howto-managed-identities-data-sources.md) | Güvenlik (Dizin oluşturucular) | Dizin oluşturma için desteklenen Azure veri kaynağıyla bağlantıları kurmak üzere Azure Active Directory ile bir arama hizmetini Güvenilen hizmet olarak kaydettirin. Azure SQL veritabanı, Azure Cosmos DB ve Azure depolama gibi Azure veri kaynaklarından içerik alan [Dizin oluşturucular](search-indexer-overview.md) için geçerlidir. | Genel Önizleme. </br> Arama hizmetini kaydetmek için portalı kullanın. |
| [**SessionID sorgu parametresi**](index-similarity-and-scoring.md), [scoringStatistics = Global parametre](index-similarity-and-scoring.md#scoring-statistics) | Sorgu (ilgi) | Arama puanlarını hesaplama için bir oturum oluşturmak üzere bir sorguya SessionID ekleyerek, daha tutarlı arama puanı hesaplamaları için scoringStatistics = Global ile tüm parçalardan puanları toplayın. | Genel olarak kullanılabilir. </br> [Arama REST API 2020-06-30](/rest/api/searchservice/) veya üstünü ya da REST API 2019-05-06 kullanın. |
| [**featuresMode (Önizleme)**](index-similarity-and-scoring.md#featuresMode-param) | Sorgu | Daha fazla ayrıntı göstermek için bu sorgu parametresini ekleyin: alan başına benzerlik puanı, alan dönemi sıklığı başına ve eşleşen benzersiz belirteçlerin başına alan sayısı. Bu veri noktalarını özel Puanlama algoritmalarında kullanabilirsiniz. Bu özelliği gösteren bir örnek için bkz. [ilgiyi aramak için makine öğrenimi ekleme (LearnToRank)](https://github.com/Azure-Samples/search-ranking-tutorial). | Genel Önizleme. </br> [Arama REST API 2020-06-30-önizleme](/rest/api/searchservice/index-preview) veya REST API 2019-05-06-Preview kullanın. |

## <a name="march-2020"></a>Mart 2020

|Özellik&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  | Kategori | Açıklama | Kullanılabilirlik  |
|---------|------------------|-------------|---------------|
| [**Yerel blob geçici silme (Önizleme)**](search-howto-index-changed-deleted-blobs.md) | Dizin Oluşturucular | Azure Bilişsel Arama 'deki bir Azure Blob depolama Dizin Oluşturucu, geçici olarak silinen bir durumdaki Blobları tanır ve dizin oluşturma sırasında karşılık gelen arama belgesini kaldırır. | Genel Önizleme. </br> Yerel "geçici silme" özelliği etkin olan bir Azure blob veri kaynağında Dizin Oluşturucu Çalıştır [REST API 2020-06-30-Preview](/rest/api/searchservice/index-preview) ve REST API 2019-05-06-Preview ' i kullanın. |
| [**Yönetim REST API (2020-03-13)**](/rest/api/searchmanagement/management-api-versions) | REST | Bir arama hizmeti oluşturmak ve yönetmek için yeni kararlı REST API. IP güvenlik duvarı ve özel bağlantı desteği ekler | Genel olarak kullanılabilir. |

## <a name="february-2020"></a>Şubat 2020

|Özellik&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  | Kategori | Açıklama | Kullanılabilirlik  |
|---------|------------------|-------------|---------------|
| [**PII algılama (Önizleme)**](cognitive-search-skill-pii-detection.md) | Yapay zeka zenginleştirme | Dizin oluşturma sırasında bir giriş metninin kişisel bilgilerini çıkaran ve bu metinden çeşitli yollarla maske sağlayan yeni bilişsel bir yetenek. | Genel Önizleme. </br> Portalı veya [arama REST API 2020-06-30-önizleme](/rest/api/searchservice/index-preview) veya REST API 2019-05-06-Preview ' i kullanın. |
| [**Özel varlık arama (Önizleme)**](cognitive-search-skill-custom-entity-lookup.md )| Yapay zeka zenginleştirme | Özel, Kullanıcı tanımlı bir sözcük ve tümcecik listesinden metin sağlayan yeni bir bilişsel beceri. Bu listeyi kullanarak tüm belgeleri eşleşen varlıklarla Etiketler. Bu beceri, benzer ancak tam olmayan eşleşmeleri bulmak için uygulanabilecek belirsiz eşleştirmeyi de destekler. | Genel Önizleme. </br> Portalı veya [arama REST API 2020-06-30-önizleme](/rest/api/searchservice/index-preview) veya REST API 2019-05-06-Preview ' i kullanın. |

## <a name="january-2020"></a>Ocak 2020

|Özellik&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  | Kategori | Açıklama | Kullanılabilirlik  |
|---------|------------------|-------------|---------------|
| [**Müşteri tarafından yönetilen şifreleme anahtarları**](search-security-manage-encryption-keys.md) |Güvenlik | Platformun yerleşik şifrelemeye ek olarak ek bir şifreleme katmanı ekler. Oluşturduğunuz ve yönettiğiniz bir şifreleme anahtarını kullanarak, yük bir arama hizmetine ulaşmadan önce dizin içeriğini ve eş anlamlı haritaları şifreleyebilirsiniz. | Genel olarak kullanılabilir. </br> Arama REST API 2019-05-06 veya üstünü kullanın. Yönetilen kod için, özellik Önizleme dışında olsa bile, doğru paket yine de [.NET SDK sürüm 8,0-Önizleme](search-dotnet-sdk-migration-version-9.md) ' dir. |
| [**Bağlantılı güvenlik duvarı desteği için IP kuralları (Önizleme)**](service-configure-firewall.md) | Güvenlik | Arama hizmeti uç noktasına erişimi belirli IP adresleriyle sınırlayın. Preview API 'sinin [CreateOrUpdate API](/rest/api/searchmanagement/2019-10-01-preview/createorupdate-service)'de yeni **ıprule** ve **networkruleset** özellikleri vardır. Bu önizleme özelliği seçili bölgelerde kullanılabilir. |  Api-Version = 2019-10 -01-Preview kullanarak genel önizleme.  |
| [**Özel arama uç noktası için Azure özel bağlantısı (Önizleme)**](service-create-private-endpoint.md) | Güvenlik| Yalnızca istemci uygulamaları ve aynı sanal ağdaki diğer Azure hizmetleri tarafından erişilebilen bir özel bağlantı kaynağı olarak çalıştırarak, genel İnternet 'ten bir arama hizmetini koruyun. | Api-Version = 2019-10 -01-Preview kullanarak genel önizleme.  |

## <a name="features-in-2019"></a>2019 'deki Özellikler

### <a name="december-2019"></a>Aralık 2019

+ [Demo uygulaması oluşturma (Önizleme)](search-create-app-portal.md) , portalda bir dizine erişim (salt okuma) ile INDIRILEBILIR bir HTML dosyası üreten yeni bir sihirbazdır. Dosya, arama hizmetinizde bir dizine bağlı olan, işlemsel bir "localhost" stili Web uygulaması işleyen katıştırılmış komut dosyası ile birlikte gelir. Sayfalar sihirbazda yapılandırılabilir ve bir arama çubuğu, sonuç alanı, kenar çubuğu gezintisi ve typeahead sorgu desteği bulunabilir. İş akışını veya görünümü genişletmek veya özelleştirmek için HTML 'yi çevrimdışı değiştirebilirsiniz. Tanıtım uygulaması, genellikle üretim senaryolarında gerekli olan güvenlik ve barındırma katmanlarını içerecek şekilde kolayca genişletilmez. Tam istemci uygulaması için kısa bir kesme yerine doğrulama ve test aracı olarak düşünmeniz gerekir.

+ [Güvenli bağlantılar için özel bir uç nokta oluşturma (Önizleme)](service-create-private-endpoint.md) arama hizmetinize güvenli bağlantılar için özel bir bağlantı ayarlamayı açıklar. Bu önizleme özelliği, istek üzerine kullanılabilir ve çözümün bir parçası olarak [Azure özel bağlantısı](../private-link/private-link-overview.md) ve [Azure sanal ağı](../virtual-network/virtual-networks-overview.md) kullanır.

### <a name="november-2019---ignite-conference"></a>Kasım 2019-Menite Konferansı

+ [Artımlı zenginleştirme (Önizleme)](cognitive-search-incremental-indexing-conceptual.md) , bir zenginleştirme ardışık düzenine önbelleğe alma ve statefullstate ekler. böylece, zaten işlenmiş olan içeriği kaybetmeden belirli adımlarla veya aşamalarda çalışabilirsiniz. Daha önce, bir zenginleştirme ardışık düzeninde yapılan herhangi bir değişiklik tam yeniden oluşturma gerektirdi. Artımlı çözümlemenin, özellikle de görüntü analizinden oluşan çıkış, korunur.

<!-- 
+ Custom Entity Lookup is a cognitive skill used during indexing that allows you to provide a list of custom entities (such as part numbers, diseases, or names of locations you care about) that should be found within the text. It supports fuzzy matching, case-insensitive matching, and entity synonyms. -->

+ [Belge ayıklama (Önizleme)](cognitive-search-skill-document-extraction.md) , dizin oluşturma sırasında kullanılan bilişsel bir beceriye sahiptir ve bir dosyanın içeriğini bir beceri içinden ayıklamanızı sağlar. Daha önce belge çözme yalnızca beceri yürütmeden önce oluşmuştur. Bu beceriye ek olarak, bu işlemi beceri yürütme içinde de gerçekleştirebilirsiniz.

+ [Metin çevirisi](cognitive-search-skill-text-translation.md) , dizinleme sırasında metni değerlendiren ve her kayıt için belirtilen hedef dile çevrilen metni döndüren bilişsel bir yetenküldür.

+ [Power BI şablonlar](https://github.com/Azure-Samples/cognitive-search-templates/blob/master/README.md) , görselleştirmelerinizi ve Power BI masaüstündeki bir bilgi deposunda zenginleştirme içeriği analizini başlatabilir. Bu şablon, [veri Içeri aktarma Sihirbazı](knowledge-store-create-portal.md)aracılığıyla oluşturulan Azure Tablo projeksiyonlarını için tasarlanmıştır.

+ [Azure Data Lake Storage 2. (Önizleme)](search-howto-index-azure-data-lake-storage.md), [Cosmos DB GREMLIN API (önizleme)](search-howto-index-cosmosdb.md)ve [Cosmos DB Cassandra API (Önizleme)](search-howto-index-cosmosdb.md) Dizin oluşturucularda artık desteklenmektedir. [Bu formu](https://aka.ms/azure-cognitive-search/indexer-preview)kullanarak kaydolabilirsiniz. Önizleme programına kabul edildikten sonra bir onay e-postası alacaksınız.

### <a name="july-2019"></a>Temmuz 2019

+ [Azure Kamu Bulutu](../azure-government/compare-azure-government-global-azure.md#azure-cognitive-search)'nda genel olarak kullanılabilir.

<a name="new-service-name"></a>

## <a name="new-service-name"></a>Yeni hizmet adı

Azure Search, çekirdek işlemlerde bilişsel yeteneklerin ve AI işlemenin genişletilmiş (henüz isteğe bağlı) kullanımını yansıtmak için artık **Azure bilişsel arama** olarak yeniden adlandırıldı. API sürümleri, NuGet paketleri, ad alanları ve uç noktalar değiştirilmez. Yeni ve mevcut arama çözümleri, hizmet adı değişikliğinden etkilenmez.

## <a name="service-updates"></a>Hizmet güncelleştirmeleri

Azure Bilişsel Arama için [hizmet güncelleştirme duyuruları](https://azure.microsoft.com/updates/?product=search&status=all) Azure Web sitesinde bulunabilir.