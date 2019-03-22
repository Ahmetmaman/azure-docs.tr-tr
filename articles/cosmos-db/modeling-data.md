---
title: Bir NoSQL veritabanı, belge verilerini modelleme
titleSuffix: Azure Cosmos DB
description: NoSQL veritabanlarında veri modelleme, ilişkisel bir veritabanı ve belge veritabanını veri modelleme arasındaki farklar hakkında bilgi edinin.
author: aliuy
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: andrl
ms.custom: seodec18
ms.openlocfilehash: f122d60a4f4df011a0adbe7806e70ae173222641
ms.sourcegitcommit: ab6fa92977255c5ecbe8a53cac61c2cd2a11601f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58295105"
---
# <a name="modeling-document-data-for-nosql-databases"></a>NoSQL veritabanları için belge verilerini modelleme

Gibi Azure Cosmos DB, şemasız veritabanları yaparken Süper kolay veri modelinizi değişiklikler benimsemek yine de bazı zaman düşünmeye verilerinizle ilgili harcadığınız.

Nasıl veri depolanacak geçiyor? Uygulamanızı nasıl almak ve veri sorgulamak için geçiyor? Uygulama koyu okuma veya yazma ağır mi?

Bu makaleyi okuduktan sonra aşağıdaki soruları yanıtlamak mümkün olacaktır:

* Nasıl bir belge veritabanında bir belge dikkat etmeniz?
* Veri modelleme nedir ve neden miyim dikkat etmelisiniz?
* Bir belge veritabanında modelleme verileri ilişkisel bir veritabanı için farklı mı?
* İlişkisel olmayan bir veritabanında veri ilişkileri nasıl express?
* Ben veri siteme ne zaman ve ne zaman veri bağlamayın?

## <a name="embedding-data"></a>Veri ekleme

Azure Cosmos DB gibi bir belge deposu, veri modelleme başlattığınızda varlıklarınızı değerlendirilecek deneyin **müstakil belgeleri** JSON'da temsil edilir.

Biz kolları sıvayın önce çok daha çok, bize birkaç adımda geri alın ve nasıl biz bize çoğunu zaten alışkın olduğu bir konu ilişkisel bir veritabanındaki bir şey model göz gerekir. Aşağıdaki örnek, bir kişinin ilişkisel bir veritabanında depolanabilir nasıl gösterir.

![İlişkisel veritabanı modeli](./media/sql-api-modeling-data/relational-data-model.png)

İlişkisel veritabanları ile çalışırken, biz normalleştirmek için Normalleştir, Normalleştir yıllardır verilen.

Verilerinizi genellikle normalleştirme bir kişi gibi bir varlık alma ve verileri ayrı parçaya bozucu içerir. Yukarıdaki örnekte, bir kişi, birden çok adresi kayıtları yanı sıra birden çok kişi ayrıntı kaydı olabilir. Biz bile bir adım ötesine gidin ve kişi ayrıntılarını ortak başka ayıklayarak kesme alanlarını ister bir tür. Aynı adresi için burada her kaydı gibi bir türe sahip *giriş* veya *iş*.

Kılavuzluk yapar verileri normalleştirmek için olduğunda **yedekli veri depolanmasını önlemek** her kaydı ve bunun yerine veriye başvurmaktadır. Bu örnekte, tüm kişi ayrıntıları ve adresleri olan bir kişi okunacak BİRLEŞTİRMELER etkili bir şekilde çalışma zamanında verilerinizi toplamak için kullanmanız gerekir.

    SELECT p.FirstName, p.LastName, a.City, cd.Detail
    FROM Person p
    JOIN ContactDetail cd ON cd.PersonId = p.Id
    JOIN ContactDetailType on cdt ON cdt.Id = cd.TypeId
    JOIN Address a ON a.PersonId = p.Id

Yazma işlemleri, tek bir kişi, iletişim ayrıntılarını ve adresleri ile güncelleştirilmesi arasında bireysel birçok tabloları gerektirir.

Şimdi biz bir belge veritabanında kendi içinde bir varlık olarak aynı verileri modelleyecek nasıl göz atalım.

    {
        "id": "1",
        "firstName": "Thomas",
        "lastName": "Andersen",
        "addresses": [
            {
                "line1": "100 Some Street",
                "line2": "Unit 1",
                "city": "Seattle",
                "state": "WA",
                "zip": 98012
            }
        ],
        "contactDetails": [
            {"email": "thomas@andersen.com"},
            {"phone": "+1 555 555-5555", "extension": 5555}
        ]
    }

Artık sahibiz yukarıdaki yaklaşımı kullanarak **normalleştirilmişlikten çıkarılmış** kişi nereye kaydetmek size **katıştırılmış** gibi kendi kişi ayrıntıları ve adresler, bu kişinin tek bir JSON ile ilgili tüm bilgileri Belge.
Ayrıca, biz için sabit bir şema sınırlı değil çünkü biz farklı şekiller kişi ayrıntılarını tamamen sahip gibi şeyleri esnekliğine sahipsiniz.

Veritabanından bir tam kişi kaydı alma tek bir okuma işlemi tek bir koleksiyon karşı ve tek bir belgenin sunulmuştur. Bir kişi kaydı, iletişim ayrıntılarını ve adresleri ile güncelleştirilmesi, bir tek bir yazma işlemi tek bir belge olan.

Veri normal durumdan çıkarmayı, daha az sorgular ve bunun yaygın işlemlerin tamamlanması güncelleştirmeleri uygulamanız gerekebilir.

### <a name="when-to-embed"></a>Ekleme zamanı

Genel olarak, katıştırılmış veri kullanın ne zaman modelleri:

* Vardır **bulunan** varlıklar arasında ilişkiler.
* Vardır **bir birkaç** varlıklar arasında ilişkiler.
* Katıştırılmış veri, **seyrek değişen**.
* Orada katıştırılmış veri olmaz büyütün **bağlı olmadan**.
* Katıştırılmış veriler var. **integral** bir belgedeki verileri.

> [!NOTE]
> Genellikle, veri modelleri sağlamak daha iyi çıkarılmışsa **okuma** performans.

### <a name="when-not-to-embed"></a>Ne zaman değil ekleme

Bir belge veritabanında kural karşısında her şeyi normalleştirilmişlikten çıkarmak ve tüm veriler tek bir belgeye olsa da bu kaçınılmalıdır bazı durumlar için açabilir.

Bu JSON parçacığı yararlanın.

    {
        "id": "1",
        "name": "What's new in the coolest Cloud",
        "summary": "A blog post by someone real famous",
        "comments": [
            {"id": 1, "author": "anon", "comment": "something useful, I'm sure"},
            {"id": 2, "author": "bob", "comment": "wisdom from the interwebs"},
            …
            {"id": 100001, "author": "jane", "comment": "and on we go ..."},
            …
            {"id": 1000000001, "author": "angry", "comment": "blah angry blah angry"},
            …
            {"id": ∞ + 1, "author": "bored", "comment": "oh man, will this ever end?"},
        ]
    }

Bu, biz tipik bir blog veya CMS, sistem modelleme, gibi ne yorumlar embedded post varlıkla görünür olabilir. Bu örnekte sorun açıklamaları dizisi olmasıdır **sınırsız**, herhangi tek bir posta olabilir açıklama sayısını (pratik) sınır olmadığını anlamına gelir. Belge boyutunu önemli ölçüde büyüyebilir gibi bu bir sorun olacaktır.

Belge boyutunu kablo yanı sıra okuma ve uygun ölçekte, belge güncelleştirme üzerinden veri iletmek için özelliği büyüdükçe etkilenir.

Bu durumda, aşağıdaki modeli dikkate alınması gereken daha iyi olur.

    Post document:
    {
        "id": "1",
        "name": "What's new in the coolest Cloud",
        "summary": "A blog post by someone real famous",
        "recentComments": [
            {"id": 1, "author": "anon", "comment": "something useful, I'm sure"},
            {"id": 2, "author": "bob", "comment": "wisdom from the interwebs"},
            {"id": 3, "author": "jane", "comment": "....."}
        ]
    }

    Comment documents:
    {
        "postId": "1"
        "comments": [
            {"id": 4, "author": "anon", "comment": "more goodness"},
            {"id": 5, "author": "bob", "comment": "tails from the field"},
            ...
            {"id": 99, "author": "angry", "comment": "blah angry blah angry"}
        ]
    },
    {
        "postId": "1"
        "comments": [
            {"id": 100, "author": "anon", "comment": "yet more"},
            ...
            {"id": 199, "author": "bored", "comment": "will this ever end?"}
        ]
    }

Bu model, en son üç sahip açıklamaları dizisi bir sabit ile posta kendisi, katıştırılmış bağlı bu süre. Diğer açıklamaları 100 yorumların toplu gruplandırılan ve ayrı belgelerinde depolanan. Kurgusal uygulamamız bir kerede 100 açıklamaları yükleyen kullanıcı izin verdiğinden, toplu iş boyutu 100 seçildi.  

Katıştırılmış veri belgeler arasında genellikle kullanıldığında ve sık sık değişir katıştırma verileri yararlı olduğu başka bir durumdur.

Bu JSON parçacığı yararlanın.

    {
        "id": "1",
        "firstName": "Thomas",
        "lastName": "Andersen",
        "holdings": [
            {
                "numberHeld": 100,
                "stock": { "symbol": "zaza", "open": 1, "high": 2, "low": 0.5 }
            },
            {
                "numberHeld": 50,
                "stock": { "symbol": "xcxc", "open": 89, "high": 93.24, "low": 88.87 }
            }
        ]
    }

Bu, bir kişinin stok Portföy temsil eder. Her Portföy belgesine hisse senedi bilgi eklemek seçtik. İlgili veriler sıklıkla değişiyorsa olduğu bir ortamda uygulama alım-satım, sık sık değişen veriler Katıştırılıyor stok gibi bir hisse senedi satılan her zaman her Portföy belge sürekli olarak güncelleştirdiğiniz anlamına zordur.

Hisse senedi *zaza* yüzlerce kez tek bir satılan gün ve binlerce kullanıcıya sahip olabilir *zaza* Portföyüne üzerinde. Yukarıdaki birçok kez yüzlerce Portföy belgeleri güncelleştirmek için gerekir gibi bir veri modeli ile bir sisteme önde gelen her gün, iyi ölçeklendirme olmaz.

## <a name="referencing-data"></a>Başvuru verileri

Bu nedenle, veriler Katıştırılıyor güzelce için çoğu durumda çalışır ancak verilerinizi normal durumdan çıkarmayı değer olandan daha fazla sorunlara neden, senaryoları olduğunu işaretlenmemiştir. Bu nedenle biz şimdi ne yapacaksınız?

İlişkisel veritabanları, varlıklar arasındaki ilişkilerin oluşturabileceğiniz tek yer değildir. Bir belge veritabanında gerçekten başka belgelerde verilerle ilgili bir belgedeki bilgiler olabilir. Artık, ben bir dakika bile size Azure Cosmos DB ilişkisel bir veritabanında veya başka bir belge veritabanında daha uygun sistemler oluşturabilir, ancak basit ilişkiler bir sakınca yoktur ve yararlı olabilir sağduyulu değil.

Daha önce bir stok Portföyünden örneği kullanmak için seçtik aşağıdaki JSON ancak bu kez katıştırmak yerine Portföy stok öğede diyoruz. Stok öğesi gün içinde sık güncelleştirilmesi gereken yalnızca belge değiştiğinde bu şekilde, tek bir hisse senedi belge olur.

    Person document:
    {
        "id": "1",
        "firstName": "Thomas",
        "lastName": "Andersen",
        "holdings": [
            { "numberHeld":  100, "stockId": 1},
            { "numberHeld":  50, "stockId": 2}
        ]
    }

    Stock documents:
    {
        "id": "1",
        "symbol": "zaza",
        "open": 1,
        "high": 2,
        "low": 0.5,
        "vol": 11970000,
        "mkt-cap": 42000000,
        "pe": 5.89
    },
    {
        "id": "2",
        "symbol": "xcxc",
        "open": 89,
        "high": 93.24,
        "low": 88.87,
        "vol": 2970200,
        "mkt-cap": 1005000,
        "pe": 75.82
    }

Bu yaklaşımın hemen bir dezavantajı, ancak uygulamanızı bir kişinin Portföy görüntülenirken tutulan her hisse senedi hakkındaki bilgileri gösterecek şekilde gerekli olup olmadığını olan; Bu durumda veritabanına stok her belge için bilgileri yüklemek için birden fazla dönüş yapmak gerekir. Burada, gün içinde sık gerçekleşir, ancak büyük olasılıkla bu belirli bir sistemi performansı üzerinde daha az etkisi olan okuma işlemleri üzerindeki sırayla tehlikeye yazma işlemlerinin verimliliğini artırmak için bir karar yaptık.

> [!NOTE]
> Normalleştirilmiş veri modelleri **daha fazla gidiş dönüş gerektirebilir** sunucu.

### <a name="what-about-foreign-keys"></a>Yabancı anahtarlar hakkında neler diyeceksiniz?

Şu anda bir kısıtlama kavramı olduğundan, yabancı anahtar veya aksi takdirde belgelerde sahip belge arası ilişkileri etkili bir şekilde "zayıf bağlantılar" ve veritabanı tarafından doğrulanmaz. Bir belge gerçekten var başvuran veri emin olmak, uygulamanızdaki veya sunucu tarafı Tetikleyicileri veya saklı yordamları Azure Cosmos DB üzerinde kullanarak bunu gerekir.

### <a name="when-to-reference"></a>Zaman başvurmak için

Genel olarak, normalleştirilmiş verileri kullanma ne zaman modelleri:

* Temsil eden **-çok** ilişkileri.
* Temsil eden **çoktan çoğa** ilişkileri.
* İlgili verileri **sık sık değişen**.
* Başvurulan veri olabilir **sınırsız**.

> [!NOTE]
> Genellikle normalleştirme sağlayan daha iyi **yazma** performans.

### <a name="where-do-i-put-the-relationship"></a>İlişki nereye yerleştirmeniz gerekir?

İlişkinin büyüme, başvuruyu depolanacağı hangi belge belirlemenize yardımcı olur.

Biz, Yayımcılar ve kitapları modeller JSON yanıtına aşağıdaki bakarsanız.

    Publisher document:
    {
        "id": "mspress",
        "name": "Microsoft Press",
        "books": [ 1, 2, 3, ..., 100, ..., 1000]
    }

    Book documents:
    {"id": "1", "name": "Azure Cosmos DB 101" }
    {"id": "2", "name": "Azure Cosmos DB for RDBMS Users" }
    {"id": "3", "name": "Taking over the world one JSON doc at a time" }
    ...
    {"id": "100", "name": "Learn about Azure Cosmos DB" }
    ...
    {"id": "1000", "name": "Deep Dive into Azure Cosmos DB" }

Yayımcı başına books sayısı ile sınırlı büyüme küçükse, ardından yayımcı belge içinde kitap başvuru depolamak yararlı olabilir. Yayımcı başına books sayısını sınırsız olarak ise, ancak daha sonra bu veri modeli Yukarıdaki örnek yayımcı belgeyi olduğu gibi değişebilir, büyüyen dizilerine sunulmasını sağlar.

Bir bit etrafında şeyler geçiş hala aynı verileri temsil eder, ancak artık bu büyük değişmez koleksiyonlara kaçınır modeli neden olur.

    Publisher document:
    {
        "id": "mspress",
        "name": "Microsoft Press"
    }

    Book documents:
    {"id": "1","name": "Azure Cosmos DB 101", "pub-id": "mspress"}
    {"id": "2","name": "Azure Cosmos DB for RDBMS Users", "pub-id": "mspress"}
    {"id": "3","name": "Taking over the world one JSON doc at a time"}
    ...
    {"id": "100","name": "Learn about Azure Cosmos DB", "pub-id": "mspress"}
    ...
    {"id": "1000","name": "Deep Dive into Azure Cosmos DB", "pub-id": "mspress"}

Yukarıdaki örnekte, biz yayımcı belge üzerinde sınırsız koleksiyon bırakılan. Bunun yerine yalnızca yayımcı başvuru her kitap belgeyi sahibiz.

### <a name="how-do-i-model-manymany-relationships"></a>Çok: çok ilişkileri nasıl model?

İlişkisel bir veritabanındaki *çok: çok* ilişkiler genellikle yalnızca diğer tablodaki kayıtların birlikte katılın birleştirme tablolarla modellenir.

![Tabloları birleştirme](./media/sql-api-modeling-data/join-table.png)

Belgeleri kullanarak aynı şeyi çoğaltmak, aşağıdakine benzer bir veri modeli oluşturmak için fikri size cazip olabilir.

    Author documents:
    {"id": "a1", "name": "Thomas Andersen" }
    {"id": "a2", "name": "William Wakefield" }

    Book documents:
    {"id": "b1", "name": "Azure Cosmos DB 101" }
    {"id": "b2", "name": "Azure Cosmos DB for RDBMS Users" }
    {"id": "b3", "name": "Taking over the world one JSON doc at a time" }
    {"id": "b4", "name": "Learn about Azure Cosmos DB" }
    {"id": "b5", "name": "Deep Dive into Azure Cosmos DB" }

    Joining documents:
    {"authorId": "a1", "bookId": "b1" }
    {"authorId": "a2", "bookId": "b1" }
    {"authorId": "a1", "bookId": "b2" }
    {"authorId": "a1", "bookId": "b3" }

Bu işe yarar. Ancak, kendi kitaplarıyla yazar ya da yükleme veya yazarı ile bir kitap yüklerken her zaman veritabanında en az iki ek sorgular gerekir. Katılma belge ve birleştirilen gerçek belge getirmek için başka bir sorgu için bir sorgu.

Tüm bu birleşim tablosundan yapıyor yapıştırma, birlikte iki veri parçasını ardından neden tamamen kaldırın?
Aşağıdakileri göz önünde bulundurun.

    Author documents:
    {"id": "a1", "name": "Thomas Andersen", "books": ["b1, "b2", "b3"]}
    {"id": "a2", "name": "William Wakefield", "books": ["b1", "b4"]}

    Book documents:
    {"id": "b1", "name": "Azure Cosmos DB 101", "authors": ["a1", "a2"]}
    {"id": "b2", "name": "Azure Cosmos DB for RDBMS Users", "authors": ["a1"]}
    {"id": "b3", "name": "Learn about Azure Cosmos DB", "authors": ["a1"]}
    {"id": "b4", "name": "Deep Dive into Azure Cosmos DB", "authors": ["a2"]}

Artık, ı yazar varsa hemen yazılmış hangi kitapları biliyorum ve ben yüklenen bir kitap belge varsa buna karşılık yazarları kimliklerini biliyorum. Bu, sunucu sayısını gidiş dönüşleri uygulamanızın yapması gereken tablo birleştirme azaltma karşı Ara sorgu kaydeder.

## <a name="hybrid-data-models"></a>Karma veri modelleri

Biz artık inceledik katıştırma (veya normal durumdan çıkarmayı) ve veri başvuru (veya normalleştirilmesi), her sahip kullanıcıların upsides ve her ödün anlatıldığı gibi.

Her zaman ya da olması gerekmez ya da biraz karışık şeyler Korkmuş çekinmeyin.

Uygulamanızın belirli kullanım desenleri ve iş yüklerini burada karıştırma katıştırılmış durumlar olabilir göre başvurulan veri anlamlı ve iyi bir performans düzeyi faydalanırken, sağlama daha az sunucu ile basit uygulama mantığı gelişlerin yuvarlak.

Aşağıdaki JSON göz önünde bulundurun.

    Author documents:
    {
        "id": "a1",
        "firstName": "Thomas",
        "lastName": "Andersen",
        "countOfBooks": 3,
        "books": ["b1", "b2", "b3"],
        "images": [
            {"thumbnail": "https://....png"}
            {"profile": "https://....png"}
            {"large": "https://....png"}
        ]
    },
    {
        "id": "a2",
        "firstName": "William",
        "lastName": "Wakefield",
        "countOfBooks": 1,
        "books": ["b1"],
        "images": [
            {"thumbnail": "https://....png"}
        ]
    }

    Book documents:
    {
        "id": "b1",
        "name": "Azure Cosmos DB 101",
        "authors": [
            {"id": "a1", "name": "Thomas Andersen", "thumbnailUrl": "https://....png"},
            {"id": "a2", "name": "William Wakefield", "thumbnailUrl": "https://....png"}
        ]
    },
    {
        "id": "b2",
        "name": "Azure Cosmos DB for RDBMS Users",
        "authors": [
            {"id": "a1", "name": "Thomas Andersen", "thumbnailUrl": "https://....png"},
        ]
    }

Burada (çoğunlukla) burada diğer varlıkların verilerden en üst düzey belgeye gömülü, ancak diğer veri başvurulan ekli model izlenen.

Kitap belge bakarsanız, birkaç görebiliriz yazarlar dizisi baktığımızda, alanlar ilginç. Var olan bir `id` alan alanın geri standart uygulama normalleştirilmiş bir modelde bir yazar belgesi başvurmak için kullanıyoruz ancak biz de sahip `name` ve `thumbnailUrl`. Biz ile takılmış `id` ve ilgili Yazar belge "bağlantı" kullanılarak gerekli herhangi bir ek bilgi almak için uygulama ekledi ancak uygulamamız yazarın adı ve her bir kitap ile küçük bir resim görüntüler. Biz kaydedebilir gidiş dönüş kitap bir listedeki her sunucuya normal durumdan çıkarmayı tarafından görüntülenen **bazı** veri yazar.

Emin olun, yazarın adı değiştirilmiş ya da fotoğrafını güncelleştirme istedikleri Git ve sürekli yayınlanmış ancak uygulamamız için yazarlar adları genellikle değişmez varsayımına dayanarak her kitabın güncelleştirmek gerekir, bu kabul edilebilir bir tasarım kararıdır.  

Örnekte, vardır **toplamalar'önceden hesaplanan** okuma işlemi pahalı işleme kaydetmek için değerler. Örnekte, yazar belgeye gömülü verilerin bazıları, çalışma zamanında hesaplanır verilerdir. Yeni bir kitap her yayımlandığında bir kitap belge oluşturulur **ve** countOfBooks alanı belirli bir yazar için mevcut kitap belgelerin göre hesaplanan bir değere ayarlanır. Bu iyileştirme biz okuma iyileştirmek için yazma işlemleri üzerinde hesaplamalar yapmak burada gücünüze okuma yoğun sistemleri yararlı olabilir.

Azure Cosmos DB desteklediğinden önceden hesaplanan alanlar modeliyle yeteneğini mümkün hale getirilir **çok belgeli işlemler**. Birçok NoSQL deposu, belgeler arasında işlemleri yapmak ve tasarım kararları, "her zaman her şeyi, bu sınırlama nedeniyle ekleme" gibi bu nedenle Danışmanı. Azure Cosmos DB ile sunucu tarafı Tetikleyiciler veya books ekleyebileceğiniz ve yazarlar bir ACID işlemi içinde tüm güncelleştirme saklı yordamları kullanabilirsiniz. Sizin artık **sahip** her şeyi yalnızca verilerinizi tutarlı kalmasından emin olmak için bir belgeye gömülecek.

## <a name="distinguishing-between-different-document-types"></a>Farklı belge türleri arasında ayrım

Bazı senaryolarda, aynı koleksiyonda farklı belge türlerinin karışımı isteyebilirsiniz; birden çok, istediğiniz zaman bu karşılaşılır ilgili belgeler için de aynı oturmak [bölüm](partitioning-overview.md). Örneğin, hem books put kitap aynı koleksiyonda incelemeler ve tarafından bölümleme `bookId`. Böyle bir durumda, genellikle belgelerinizi bunları ayırt etmek için kendi türünü tanımlayan bir alan eklemek istersiniz.

    Book documents:
    {
        "id": "b1",
        "name": "Azure Cosmos DB 101",
        "bookId": "b1",
        "type": "book"
    }

    Review documents:
    {
        "id": "r1",
        "content": "This book is awesome",
        "bookId": "b1",
        "type": "review"
    },
    {
        "id": "r2",
        "content": "Best book ever!",
        "bookId": "b1",
        "type": "review"
    }

## <a name="next-steps"></a>Sonraki adımlar

Şemadan bağımsız bir dünyada modelleme verileri olarak şimdiye kadar önemli olduğunu anlamak için bu makaledeki büyük paketler var.

Yalnızca olmadığından bir ekranda veri parçasını temsil etmek için tek bir yolu, verilerinizi modellemek için tek bir yolu yoktur. Uygulamanızı ve nasıl oluşturacak, anlamak için kullanma ve verileri işlemek. Ardından, burada sunulan yönergeler uygulanarak, uygulamanızın anında ihtiyaçlarını gideren bir model oluşturma hakkında ayarlayabilirsiniz. Uygulamalarınızı değiştirmeniz gerektiğinde değiştirebilir ve veri modelinizin bir kolayca gelişmek benimsemek için bir şemasız veritabanı esnekliğinden yararlanabilir.

Azure Cosmos DB hakkında daha fazla bilgi için hizmetin başvuran [belgeleri](https://azure.microsoft.com/documentation/services/cosmos-db/) sayfası.

Bir parçaya verilerinizin birden çok bölüm nasıl bakın anlamak için [Azure Cosmos DB'de bölümleme veri](sql-api-partition-data.md).
