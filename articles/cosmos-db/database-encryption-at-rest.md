---
title: Azure Cosmos DB'de bekleme sırasında şifreleme
description: Azure Cosmos DB, bekleyen verilerin şifrelenmesi nasıl sağladığını ve nasıl uygulandığını öğrenin.
author: markjbrown
ms.author: sngun
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/23/2019
ms.custom: seodec18
ms.openlocfilehash: 366a8cc2d8b08c9508053eaeb8bf70622fd870cf
ms.sourcegitcommit: 87781a4207c25c4831421c7309c03fce5fb5793f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2020
ms.locfileid: "76548110"
---
# <a name="data-encryption-in-azure-cosmos-db"></a>Azure Cosmos DB veri şifreleme 

Bekleyen şifreleme gibi katı hal sürücülerine (SSD'ler), kalıcı depolama cihazlarında verilerin şifrelenmesi için yaygın olarak başvuran bir ifade ve sabit disk sürücülerinin (HDD'ler) ' dir. Cosmos DB, SSD'ler üzerinde birincil veritabanlarında depolar. Yedeklemeler ve medya ekler, HDD'ler ile genel olarak yedeklendiğinden Azure Blob storage'da depolanır. Cosmos DB için bekleyen şifrelemenin'ın yayınlanmasıyla birlikte, tüm veritabanları, medya ekler ve yedeklemeleri şifrelenir. Verileriniz artık (ağ üzerinden) aktarım sırasında şifrelenir ve rest (kalıcı depolama) uçtan uca şifreleme sağlar.

Cosmos DB bir PaaS hizmeti kullanımı çok kolay olduğu gibi. Cosmos DB'de depolanan tüm kullanıcı verileri, aktarım ve bekleme sırasında şifrelenir olduğundan, herhangi bir eylemde bulunmanız gerekmez. Bu başka bir yolu olan bekleyen şifreleme varsayılan olarak "açık" olduğu. Açma veya kapatma için denetim yoktur. Azure Cosmos DB, hesabın çalıştığı tüm bölgelerde AES-256 şifrelemesini kullanır. Karşılamaya devam ederken bu özelliği sunuyoruz. bizim [kullanılabilirliğini ve performansını SLA'lar](https://azure.microsoft.com/support/legal/sla/cosmos-db).

## <a name="implementation-of-encryption-at-rest-for-azure-cosmos-db"></a>Azure Cosmos DB için bekleyen şifrelemenin uygulaması

Bekleme sırasında şifreleme birkaç güvenli anahtar depolama sistemleri, şifreli ağları ve şifreleme API'leri dahil güvenlik teknolojileri kullanılarak uygulanır. Anahtarları Yönet sistemleriyle iletişim şifresini çözmek ve verileri işleyen sistemleri sahip. Şifrelenmiş verilerin depolanması ve anahtar yönetimini nasıl ayrılmış diyagramda gösterilmektedir. 

![Tasarım diyagramı](./media/database-encryption-at-rest/design-diagram.png)

Bir kullanıcı isteği temel akışı aşağıdaki gibidir:
- Kullanıcı veritabanı hesabı hazır hale getirdik ve yönetim Service kaynak sağlayıcısı için bir istek ile depolama anahtarlarını alınır.
- Cosmos DB bağlantı HTTPS ve güvenli aktarımı ile bir kullanıcı oluşturur. (SDK'ları ayrıntılarını soyut.)
- Kullanıcı daha önce oluşturulmuş bir güvenli bağlantı üzerinden depolanması için bir JSON belgesi gönderir.
- Kullanıcının dizin oluşturmayı devre dışı bırakılmış sürece JSON belgesini dizine alınır.
- JSON belge ve dizin verileri güvenli depolama yazılır.
- Düzenli olarak, veriler güvenli depolama alanından okuyun ve Azure şifreli Blob Store için yedeklenen.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="q-how-much-more-does-azure-storage-cost-if-storage-service-encryption-is-enabled"></a>S: ne kadar fazla depolama hizmeti şifrelemesi etkinleştirilirse Azure depolama maliyeti?
Y: hiçbir ek maliyet yoktur.

### <a name="q-who-manages-the-encryption-keys"></a>S: kimin şifreleme anahtarları yönetir?
C: anahtarlar, Microsoft tarafından yönetilir.

### <a name="q-how-often-are-encryption-keys-rotated"></a>S: ne sıklıkta şifreleme anahtarları döndürülür?
Y: Microsoft Cosmos DB izleyen şifreleme anahtar döndürme için iç yönergeleri kümesi vardır. Özel yönergeleri yayımlanmaz. Microsoft, yayımlama [Security Development Lifecycle (SDL)](https://www.microsoft.com/sdl/default.aspx), iç Kılavuzu kümesini görülür ve geliştiriciler için yararlı en iyi yöntemler vardır.

### <a name="q-can-i-use-my-own-encryption-keys"></a>Kendi şifreleme anahtarları kullanabilirim miyim?
Y: Evet, şimdi bu özellik yeni Cosmos hesapları için kullanılabilir ve hesap oluşturma sırasında gerçekleştirilmelidir. Daha fazla bilgi için lütfen [müşteri tarafından yönetilen anahtarlar](https://docs.microsoft.com/azure/cosmos-db/how-to-setup-cmk) belgesine gidin.

### <a name="q-what-regions-have-encryption-turned-on"></a>S: hangi bölgeleri açık şifreleme var mı?
Y: tüm Azure Cosmos DB bölgeler için tüm kullanıcı verilerini açık şifreleme vardır.

### <a name="q-does-encryption-affect-the-performance-latency-and-throughput-slas"></a>S: şifreleme performans, gecikme süresi ve aktarım hızı SLA'lar etkileyen mu?
Y: etkisi veya yoktur değişiklikleri SLA'ları tüm mevcut ve yeni hesaplar için bekleyen şifrelemenin etkin göre performans. Hakkında daha fazla edinebilirsiniz [Cosmos DB için SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db) son garantileri görmek için sayfayı.

### <a name="q-does-the-local-emulator-support-encryption-at-rest"></a>S: yerel öykünücü, bekleme sırasında şifreleme desteği mu?
Y: öykünücü, bir tek başına geliştirme/test aracıdır ve yönetilen Cosmos DB hizmetini kullanan anahtar yönetimi Hizmetleri'ni kullanmaz. Bizim önerimiz burada hassas öykünücü test verilerini depolama sürücülerinde BitLocker etkinleştirmektir. [Öykünücü destekler varsayılan veri dizinine değiştirme](local-emulator.md) iyi bilinen bir konum kullanarak yanı sıra.

## <a name="next-steps"></a>Sonraki adımlar

Cosmos DB güvenliğe ve en son geliştirmelere genel bakış için bkz. [Azure Cosmos veritabanı güvenliği](database-security.md).
Microsoft sertifikaları hakkında daha fazla bilgi için bkz. [Azure Güven Merkezi](https://azure.microsoft.com/support/trust-center/).
