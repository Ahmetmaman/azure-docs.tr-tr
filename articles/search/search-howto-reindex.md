---
title: Azure Search dizini yeniden oluşturmanız veya aranabilir içeriği - Azure Search Yenile
description: Yeni öğeler eklemek, var olan öğeleri veya belgeleri güncelleştirin veya tam yeniden derleme veya kısmi artımlı bir Azure Search dizini yenilemek için dizin geçersiz belgelerde silme.
services: search
author: HeidiSteen
manager: cgronlun
ms.service: search
ms.topic: conceptual
ms.date: 02/13/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 907ab5cd3272a3d3f64dcfd7c9628a609f4db2f4
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/16/2019
ms.locfileid: "56327655"
---
# <a name="how-to-rebuild-an-azure-search-index"></a>Nasıl bir Azure Search dizini yeniden oluşturma

Bu makalede, Azure Search dizini altında yeniden oluşturulması gereken durumlar ve devam eden bir sorgu isteği yeniden etkisini Azaltıcı öneriler yeniden açıklanmaktadır.

A *yeniden* bırakılıyor ve alan tabanlı tüm ters dizinleri dahil olmak üzere bir dizin ile ilişkili fiziksel veri yapılarını yeniden ifade eder. Azure Search'te, bırakma ve her bir alanı yeniden oluşturun. Bir dizini yeniden oluşturma için tüm alan depolama silinmesi gerekir, yeniden bir mevcut ya da düzeltilmiş dizin şemasını temel alan ve sonra dizini atıldığında veya dış kaynaklardan alınıp verilerle yeniden. Dizinleri yeniden oluştur geliştirme sırasında yaygın bir uygulamadır, ancak karmaşık türleri ekleme veya öneri araçları için alanlar ekleme gibi yapısal değişiklikler, uyum sağlamak için bir üretim düzeyinde dizini yeniden oluşturmanız gerekebilir.

Dizin çevrimdışına yeniden kullanılmasının *veri yenileme* arka plan görevi olarak çalışır. Eklemek, kaldırmak ve sorguları genellikle tamamlanması uzun zaman rağmen belgeleri sorgu iş yükleri için en az kesinti değiştirin. Dizin içeriği güncelleştirme hakkında daha fazla bilgi için bkz: [ekleme, güncelleştirme veya silme belgeleri](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents).

## <a name="rebuild-conditions"></a>Koşullar yeniden oluşturun

| Koşul | Açıklama |
|-----------|-------------|
| Bir alan tanımı değiştirme | Bir alan adı, veri türü veya özel düzeltilmesi [dizin öznitelikleridir](https://docs.microsoft.com/rest/api/searchservice/create-index) (aranabilir, filtrelenebilir, sıralanabilir, modellenebilir) tam yeniden derleme gerektirir. |
| Bir alana bir çözümleyici ekleme | [Çözümleyiciler](search-analyzers.md) dizin içinde tanımlanır ve ardından alanlara atanmış. Herhangi bir zamanda bir dizine yeni Çözümleyicisi ekleyebilirsiniz, ancak yalnızca *atama* alanı oluşturulduğunda bir çözümleyici. Bunun için her ikisi de true **Çözümleyicisi** ve **indexAnalyzer** özellikleri. **SearchAnalyzer** özelliği bir özel durum. |
| Güncelleştirmeye veya silmeye Çözümleyicisi yapısı | Silemez veya tüm dizini yeniden sürece varolan analiz bileşenleri (Çözümleyicisi, tokenizer, belirteç filtre veya char filtre) değiştirin. |
| Bir alan için bir öneri aracı ekleme | Bir alan zaten var ve eklemek istiyorsanız bir [öneri Araçları](index-add-suggesters.md) oluşturmak, dizini yeniden oluşturmanız gerekir. |
| Bir alan silme | Bir alanın tüm izlemeleri fiziksel olarak kaldırın için dizini yeniden oluşturmanız gerekir. Hemen bir yeniden derleme pratik olmadığı durumlarda, "silindi" alanına erişimi devre dışı bırakma, uygulama kodu değiştirebilirsiniz. Fiziksel alan tanımı ve içeriği dizine alan atlar bir şemayı kullanarak bir sonraki yeniden kadar söz konusu kalır. |
| Katmanları arasında geçiş | Daha fazla kapasiteye ihtiyaç duyuyorsanız, yerinde yükseltme yoktur. Yeni Kapasite noktada yeni bir hizmet oluşturuldu ve sıfırdan yeni hizmette dizinler oluşturulmalıdır. |

Mevcut fiziksel yapıları etkilemeden diğer herhangi bir değişiklik yapılabilir. Özellikle, aşağıdaki değişiklikleri yapın *değil* bir dizini yeniden derleme gerektirir:

+ Yeni alan ekleme
+ Ayarlama **alınabilir** varolan bir alan özniteliği
+ Ayarlanmış bir **searchAnalyzer** varolan bir alanı
+ Yeni bir çözümleyici yapısı içinde bir dizin ekleyin
+ Eklemek, güncelleştirmek ya da Puanlama profilleri Sil
+ Ekleme, güncelleştirme veya silme CORS ayarları
+ Ekleme, güncelleştirme veya synonymMaps Sil

Yeni bir alan eklediğinizde, var olan dizini oluşturulan belgeler yeni alan için bir null değer verilir. Gelecekteki veri yenileme, dış kaynak veri değerleri, Azure Search tarafından eklenen null değerlere değiştirin. Dizin içeriği güncelleştirme hakkında daha fazla bilgi için bkz: [ekleme, güncelleştirme veya silme belgeleri](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents).

## <a name="partial-or-incremental-indexing"></a>Kısmi veya artımlı dizin oluşturma

Azure arama'yı kullanarak bir alan başına temelinde dizin silme veya belirli alanları yeniden seçerek denetleyemezsiniz. Benzer şekilde, için yerleşik bir mekanizma bulunmamaktadır [ölçütlere göre belgelerini](https://stackoverflow.com/questions/40539019/azure-search-what-is-the-best-way-to-update-a-batch-of-documents). Ölçüt temelli dizin oluşturma için sahip olduğunuz herhangi bir gereksinim özel kod aracılığıyla karşılanması gerekir.

Ne kolayca yapabilirsiniz, ancak olan *Yenile belgeleri* bir dizinde. Arama çözümlerinin çoğu, dış kaynak veriler geçicidir ve kaynak verileri ve arama dizin eşitlemesi yaygın bir uygulamadır. Kod içinde çağrı [ekleme, güncelleştirme veya silme belgeleri](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents) işlemi veya [.NET eşdeğeri](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.indexesoperationsextensions.createorupdate?view=azure-dotnet) dizin içeriği güncelleştirmek veya yeni bir alan için değerler eklemek için.

## <a name="partial-indexing-with-indexers"></a>Dizin oluşturucular ile kısmi

[Dizin oluşturucular](search-indexer-overview.md) veri yenileme görevini basitleştirir. Bir dizin oluşturucu yalnızca bir tablo veya görünüm dış veri kaynağında dizin oluşturabilirsiniz. Birden çok tablo dizin için en kolay yaklaşım tablolar ve projeleri birleştiren bir görünüm dizine eklemek istediğiniz sütun oluşturmaktır. 

Dış veri kaynaklarında gezinen dizin oluşturucuları kullanarak, kaynak verilerde bir "yüksek su işareti" sütununu kontrol edin. Varsa, yalnızca yeni veya değiştirilen içerik içeren satırları seçerek artımlı bir değişiklik algılama için kullanabilirsiniz. İçin [Azure Blob Depolama](search-howto-indexing-azure-blob-storage.md#incremental-indexing-and-deletion-detection), `lastModified` alanı kullanılır. Üzerinde [Azure tablo depolama](search-howto-indexing-azure-tables.md#incremental-indexing-and-deletion-detection), `timestamp` aynı amaca hizmet eder. Benzer şekilde, her ikisi de [Azure SQL veritabanı dizin oluşturucu](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md#capture-new-changed-and-deleted-rows) ve [Azure Cosmos DB dizinleyici](search-howto-index-cosmosdb.md#indexing-changed-documents) satır güncelleştirmeleri bayrak eklemek için alanlar içerir. 

Dizin oluşturucular hakkında daha fazla bilgi için bkz: [Dizin oluşturucuya genel bakış](search-indexer-overview.md) ve [sıfırlama dizin oluşturucu REST API](https://docs.microsoft.com/rest/api/searchservice/reset-indexer).

## <a name="how-to-rebuild-an-index"></a>Nasıl bir dizini yeniden oluşturma

Dizin şemaları flux bir durumda olduğunda sık, tam planında etkin geliştirme sırasında yeniden oluşturur. Zaten üretimde uygulamalar için sorgu kapalı kalma süresini önlemek için mevcut bir dizine yan yana çalışan yeni bir dizin oluşturmanızı öneririz.

Katı bir SLA'sı gereksinimleri varsa, özellikle geliştirme ile bu iş için yeni bir hizmet sağlama ve bir üretim dizinden tam yalıtım gerçekleşen dizin oluşturmayı düşünebilirsiniz. Ayrı bir hizmet kaynak Çekişme olasılığını ortadan kendi donanımda çalışır. Geliştirme tamamlandığında, dizin ve yeni uç nokta için sorguları yeniden yönlendirme yeni bir dizin yerine, ya da bırakabilir veya özgün Azure Search hizmetinizde düzeltilmiş bir dizin yayımlamak için tamamlanan kodu çalıştırırsınız. Şu anda başka bir hizmete bir kullanıma hazır dizini taşımak için bir mekanizma yoktur.

Dizin güncelleştirmeleri hizmet düzeyinde okuma / yazma izinleri gereklidir. Programlı olarak çağırabilirsiniz [güncelleştirme dizin REST API](https://docs.microsoft.com/rest/api/searchservice/update-index) veya tam yeniden derleme için .NET API'leri. İstek aynıdır [dizin REST API oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-index), ancak farklı bir bağlama.

1. Dizin adı yeniden kullanıyorsanız [mevcut dizini bırakın](https://docs.microsoft.com/rest/api/searchservice/delete-index). Bu dizinin hedefleyen tüm sorguları hemen bırakılır. Bir dizinin silinmesi alanlar koleksiyonu ve diğer yapıları için fiziksel depolama alanı yok etme alınamaz. Bıraktığınız önce bir dizini silme etkileri hakkında açık olduğundan emin olun. 

2. Dizin şeması ile değiştirilmiş ya da değiştirilmiş alan tanımları sağlayın. Şema gereksinimleri bölümünde belgelenmiştir [Create Index](https://docs.microsoft.com/rest/api/searchservice/create-index).

3. Sağlayan bir [yönetici anahtarını](https://docs.microsoft.com/azure/search/search-security-api-keys) istekte.

4. Gönderme bir [dizin güncelleştirme](https://docs.microsoft.com/rest/api/searchservice/update-index) fiziksel ifadesi, Azure Search dizini yeniden oluşturmak için komutu. İstek gövdesi dizin şemasını içerir, aynı zamanda profilleri, Çözümleyicileri, öneri araçları ve CORS seçenekleri puanlamasında oluşturur.

5. [Belgeler dizinde yük](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents) bir dış kaynaktan. Bu API, bir var olan ve değiştirilmemiş bir dizin şemasını güncelleştirilmiş belgeleri ile yeniliyorsanız de kullanabilirsiniz.

Dizin oluşturma, fiziksel depolama dizin şeması içindeki her alan için aranabilir her alan için oluşturulan bir ters dizini ile ayrılır. Aranabilir olmayan alanlar filtreleri veya ifadelerinde kullanılabilir, ancak değil sahip ters dizinleri ve değil tam metin veya benzer olan aranabilir. Bir dizinin yeniden oluşturulması bu ters dizinleri silindiğinde ve yeniden sağladığınız dizin şemasını temel alan.

Dizin yüklediğinizde, her bir alanın ters dizin tüm kimlikleri karşılık gelen Belge Haritası ile her belgenin benzersiz, parçalanmış sözcükleri doldurulur. Örneğin, Oteller veri kümesi dizin oluşturulurken bir şehir alan için oluşturulan ters dizin koşulları Seattle, Portland ve benzeri içerebilir. Seattle veya Portland Şehir alanına içeren belgeleri terimi yanı sıra listede, belge kimliği gerekir. Tüm [ekleme, güncelleştirme veya silme](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents) işlemi, hüküm ve belge kimliği listesi güncelleştirilir uygun şekilde.

## <a name="view-updates"></a>Güncelleştirmeleri görüntüle

İlk belgenin yüklendikten hemen sonra bir dizini sorgulama başlayabilirsiniz. Bir belgenin kimliği biliyorsanız [arama belge REST API](https://docs.microsoft.com/rest/api/searchservice/lookup-document) belirli belgeyi döndürür. Daha geniş test etmek için dizin tam yüklü olmadığı kadar bekleyin ve ardından görmeyi beklediğiniz bağlam doğrulamak için sorguları kullanın.

## <a name="see-also"></a>Ayrıca bkz.

+ [Dizin Oluşturucu’ya genel bakış](search-indexer-overview.md)
+ [Büyük veri kümelerinde uygun ölçekte dizini](search-howto-large-index.md)
+ [Portalda dizin oluşturma](search-import-data-portal.md)
+ [Azure SQL veritabanı dizin oluşturucu](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
+ [Azure Cosmos DB dizinleyici](search-howto-index-cosmosdb.md)
+ [Azure Blob Depolama dizin oluşturucu](search-howto-indexing-azure-blob-storage.md)
+ [Azure Tablo Depolama dizin oluşturucu](search-howto-indexing-azure-tables.md)
+ [Azure Search'te güvenlik](search-security-overview.md)