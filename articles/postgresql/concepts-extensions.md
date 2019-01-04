---
title: PostgreSQL uzantıları, PostgreSQL için Azure veritabanı'nda kullanın
description: PostgreSQL için Azure veritabanı'nda uzantılarını kullanarak veritabanınızı işlevselliğini genişletme özelliğini açıklar.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 11/12/2018
ms.openlocfilehash: d6d5a8500435a540f091a082e7dc0e0d6d455716
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/17/2018
ms.locfileid: "53540852"
---
# <a name="postgresql-extensions-in-azure-database-for-postgresql"></a>PostgreSQL için Azure veritabanı'nda PostgreSQL uzantıları
PostgreSQL Uzantıları'nı kullanarak veritabanını genişletmek olanağı sağlar. Birden çok ilişkili SQL nesneleri birlikte yüklenen ya da tek bir komutla veritabanından kaldırıldı tek bir pakette paketleme için uzantılar sağlar. Yerleşik özellikler gibi veritabanında yüklenen sonra uzantıları çalışabilir. PostgreSQL uzantıları hakkında daha fazla bilgi için bkz. [paketleme ilgili nesneleri uzantı](https://www.postgresql.org/docs/9.6/static/extend-extensions.html).

## <a name="how-to-use-postgresql-extensions"></a>PostgreSQL uzantıları kullanma
Kullanabilmeniz için önce veritabanınızı PostgreSQL uzantıları yüklenmelidir. Belirli bir uzantıyı yüklemek için çalıştırın [oluşturma UZANTISI](https://www.postgresql.org/docs/9.6/static/sql-createextension.html) psql aracından paketlenmiş nesneler veritabanınıza yüklemek için komutu.

PostgreSQL için Azure veritabanı, şu anda aşağıda listelenen anahtar uzantıları kümesini destekler. Uzantılar listelenenlerden ötesinde desteklenmiyor; kendi uzantınızı PostgreSQL hizmeti için Azure veritabanı ile oluşturulamıyor.

## <a name="extensions-supported-by-azure-database-for-postgresql"></a>PostgreSQL için Azure veritabanı tarafından desteklenen uzantılar
Aşağıdaki tablolar, şu anda PostgreSQL için Azure veritabanı tarafından desteklenen standart PostgreSQL uzantıları listeler. Bu bilgiler ayrıca çalıştırarak kullanılabilir `SELECT * FROM pg_available_extensions;`.

### <a name="data-types-extensions"></a>Veri türü uzantıları

> [!div class="mx-tableFixed"]
| **Uzantı** | **Açıklama** |
|---|---|
| [chkpass](https://www.postgresql.org/docs/9.6/static/chkpass.html) | Bir veri türü için otomatik olarak şifrelenmiş parolaları sağlar. |
| [citext](https://www.postgresql.org/docs/9.6/static/citext.html) | Büyük küçük harf duyarsız bir karakter dizesi türü sağlar. |
| [Küp](https://www.postgresql.org/docs/9.6/static/cube.html) | Bir veri türü için çok boyutlu küpler sağlar. |
| [hstore](https://www.postgresql.org/docs/9.6/static/hstore.html) | Anahtar/değer çiftleri kümesi depolamak için bir veri türü sağlar. |
| [yüklenmez](https://www.postgresql.org/docs/9.6/static/isn.html) | Veri türleri için Uluslararası Ürün standartları numaralandırma sağlar. |
| [ltree](https://www.postgresql.org/docs/9.6/static/ltree.html) | Bir veri türü, hiyerarşik ağaç benzeri yapıları için sağlar. |

### <a name="functions-extensions"></a>İşlevleri uzantıları

> [!div class="mx-tableFixed"]
| **Uzantı** | **Açıklama** |
|---|---|
| [earthdistance](https://www.postgresql.org/docs/9.6/static/earthdistance.html) | Dünya yüzeyinde Great daire uzaklıkları hesaplamak için bir yol sağlar. |
| [fuzzystrmatch](https://www.postgresql.org/docs/9.6/static/fuzzystrmatch.html) | Benzerlikleri ve dizeler arasındaki uzaklığı belirlemek için çeşitli işlevler sağlar. |
| [int](https://www.postgresql.org/docs/9.6/static/intarray.html) | Tamsayı dizileri null ücretsiz işlemek için İşlevler ve işleçler sağlar. |
| [pgcrypto](https://www.postgresql.org/docs/9.6/static/pgcrypto.html) | Şifreleme işlevlerini sağlar. |
| [PG\_partman](https://pgxn.org/dist/pg_partman/doc/pg_partman.html) | Bölümlenmiş tablolar zaman ya da kimliği tarafından yönetir. |
| [PG\_trgm](https://www.postgresql.org/docs/9.6/static/pgtrgm.html) | Alfasayısal metin trigram eşleşmesi temeline göre benzerlik belirlemek için İşlevler ve işleçler sağlar. |
| [tablefunc](https://www.postgresql.org/docs/9.6/static/tablefunc.html) | Çapraz dahil olmak üzere tüm tablolar işleme işlevleri sağlar. |
| [uuid ossp](https://www.postgresql.org/docs/9.6/static/uuid-ossp.html) | Evrensel benzersiz tanımlayıcı (UUID) oluşturur. |

### <a name="full-text-search-extensions"></a>Tam metin arama uzantıları

> [!div class="mx-tableFixed"]
| **Uzantı** | **Açıklama** |
|---|---|
| [dict\_int](https://www.postgresql.org/docs/9.6/static/dict-int.html) | Tamsayılar için metin arama sözlük şablonu sağlar. |
| [unaccent](https://www.postgresql.org/docs/9.6/static/unaccent.html) | Lexemes vurgular (aksan işaretleri) kaldıran metin arama sözlüğü. |

### <a name="index-types-extensions"></a>Dizin türleri uzantıları

> [!div class="mx-tableFixed"]
| **Uzantı** | **Açıklama** |
|---|---|
| [rowsetbulk\_GIN](https://www.postgresql.org/docs/9.6/static/btree-gin.html) | B-ağacı gibi belirli veri türleri için davranışı uygulayan örnek GIN işleci sınıflar sağlar. |
| [rowsetbulk\_gist](https://www.postgresql.org/docs/9.6/static/btree-gist.html) | B-Ağacı uygulayan GiST dizin işleci sınıflar sağlar. |

### <a name="language-extensions"></a>Dil uzantıları

> [!div class="mx-tableFixed"]
| **Uzantı** | **Açıklama** |
|---|---|
| [plpgsql](https://www.postgresql.org/docs/9.6/static/plpgsql.html) | PL/pgSQL yordam dili yüklenebilir. |
| [plv8](https://plv8.github.io/) | Bir Javascript dil uzantısı kullanılabilir saklı yordamlar, Tetikleyiciler, vb. için PostgreSQL için. |

### <a name="miscellaneous-extensions"></a>Diğer uzantılar

> [!div class="mx-tableFixed"]
| **Uzantı** | **Açıklama** |
|---|---|
| [PG\_buffercache](https://www.postgresql.org/docs/9.6/static/pgbuffercache.html) | Gerçek zamanlı olarak paylaşılan arabellek önbelleğinde neler olduğunu İnceleme için bir yol sağlar. |
| [PG\_prewarm](https://www.postgresql.org/docs/9.6/static/pgprewarm.html) | İlişki verileri arabellek önbelleğine yüklemek için bir yol sağlar. |
| [PG\_stat\_deyimleri](https://www.postgresql.org/docs/9.6/static/pgstatstatements.html) | Sunucu tarafından yürütülen tüm SQL deyimleri yürütme istatistikleri izlemek için bir yol sağlar. (Aşağıda bir not üzerinde bu uzantı için bakın). |
| [pgrowlocks](https://www.postgresql.org/docs/9.6/static/pgrowlocks.html) | Satır düzeyi kilitleme bilgilerini göstermek için bir yol sağlar. |
| [pgstattuple](https://www.postgresql.org/docs/9.6/static/pgstattuple.html) | Tanımlama grubu düzeyinde istatistikleri göstermek için bir yol sağlar. |
| [postgres\_fdw](https://www.postgresql.org/docs/9.6/static/postgres-fdw.html) | Dış PostgreSQL sunucuları depolanan verilere erişmek için kullanılan yabancı veri sarmalayıcıdır. |
| [hypopg](https://hypopg.readthedocs.io/en/latest/) | CPU veya disk maliyet yoktur kuramsal dizinler oluşturma bir yol sağlar. |
| [dblink](https://www.postgresql.org/docs/current/dblink.html) | Veritabanı oturum içindeki başka bir PostgreSQL veritabanına bağlantıları destekleyen bir modül. |


### <a name="postgis-extensions"></a>PostGIS uzantıları

> [!div class="mx-tableFixed"]
| **Uzantı** | **Açıklama** |
|---|---|
| [PostGIS](http://www.postgis.net/), postgis\_topoloji, postgis\_tiger\_geocoder, postgis\_sfcgal | PostgreSQL için uzamsal ve coğrafi nesneleri. |
| adresi\_standardizer, adresi\_standardizer\_veri\_ABD | Bir adresi şekli oluşturan öğeler ayrıştırmak için kullanılır. Coğrafi kodlama adresi normalleştirme adım desteklemek için kullanılır. |
| [pgrouting](https://pgrouting.org/) | PostGIS genişletir / PostgreSQL Jeo-uzamsal veritabanının Jeo-uzamsal sağlamak için yönlendirme işlevi. |


### <a name="using-pgstatstatements"></a>Pg_stat_statements kullanma
[Pg\_stat\_deyimleri uzantısı](https://www.postgresql.org/docs/9.6/static/pgstatstatements.html) her yürütme istatistikleri SQL deyimlerinin izleme bir yol sağlamak için Azure veritabanı üzerinde PostgreSQL sunucusu önceden yüklenmiş olduğu.
Ayar `pg_stat_statements.track`, denetleyen hangi deyimleri varsayılan uzantı tarafından sayılır `top`, doğrudan istemciler tarafından gönderilen tüm deyimleri izlenen anlamına gelir. İki diğer izleme düzeyleri `none` ve `all`. Bu ayar bir sunucu parametresi olarak yapılandırılabilir [Azure portalında](https://docs.microsoft.com/azure/postgresql/howto-configure-server-parameters-using-portal) veya [Azure CLI](https://docs.microsoft.com/azure/postgresql/howto-configure-server-parameters-using-cli).

Sorgu yürütme bilgilerini pg_stat_statements sağlar ve sunucu performansı üzerindeki etkisini arasında bir denge yok olarak her SQL deyimi kaydeder. Etkin bir şekilde pg_stat_statements uzantı kullanmıyorsanız, ayarlamanızı öneririz `pg_stat_statements.track` için `none`. Bazı üçüncü taraf hizmetleri izleme sorgu performansı öngörüleri sunmak için bu nedenle bu durum sizin olup olmadığını onaylayın pg_stat_statements üzerinde System.Environment.UserInteractive unutmayın.


## <a name="next-steps"></a>Sonraki adımlar
Kullanmak için bize bildirin istediğiniz uzantı görmüyorsanız. Var olan istekleri için oy verin veya yeni bir geri bildirim isteklerini oluşturup bizim [müşteri geri bildirim Forumu](https://feedback.azure.com/forums/597976-azure-database-for-postgresql).
