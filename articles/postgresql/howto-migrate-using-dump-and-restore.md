---
title: Dökümünü al ve geri yükle-PostgreSQL için Azure veritabanı-tek sunucu
description: Bir PostgreSQL veritabanının bir döküm dosyasına nasıl ayıklanacağını ve PostgreSQL için Azure veritabanı 'nda pg_dump tarafından oluşturulan bir dosyadan geri yükleme işlemini açıklar-tek sunucu.
author: sr-msft
ms.author: srranga
ms.service: postgresql
ms.topic: how-to
ms.date: 09/22/2020
ms.openlocfilehash: 4fe15d1bd23f36b7289c54bedf575ae4760600e0
ms.sourcegitcommit: 19dce034650c654b656f44aab44de0c7a8bd7efe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2020
ms.locfileid: "91710813"
---
# <a name="migrate-your-postgresql-database-using-dump-and-restore"></a>PostgreSQL veritabanınızı döküm ve geri yükleme kullanarak geçirme
[!INCLUDE[applies-to-postgres-single-flexible-server](includes/applies-to-postgres-single-flexible-server.md)]

Bir PostgreSQL veritabanını bir döküm dosyasına ayıklamak ve pg_dump tarafından oluşturulan bir arşiv dosyasından PostgreSQL veritabanını geri yüklemek için [pg_restore](https://www.postgresql.org/docs/current/static/app-pgrestore.html) [pg_dump](https://www.postgresql.org/docs/current/static/app-pgdump.html) kullanabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar
Bu nasıl yapılır kılavuzunda ilerlemek için şunlar gerekir:
- Üzerinde erişime ve veritabanına izin vermek için güvenlik duvarı kuralları içeren bir [PostgreSQL sunucusu Için Azure veritabanı](quickstart-create-server-database-portal.md) .
- [pg_dump](https://www.postgresql.org/docs/current/static/app-pgdump.html) ve [pg_restore](https://www.postgresql.org/docs/current/static/app-pgrestore.html) komut satırı yardımcı programları yüklendi

PostgreSQL veritabanınızı dökümünü almak ve geri yüklemek için şu adımları izleyin:

## <a name="create-a-dump-file-using-pg_dump-that-contains-the-data-to-be-loaded"></a>Yüklenecek verileri içeren pg_dump kullanarak döküm dosyası oluşturma
Mevcut bir PostgreSQL veritabanını şirket içinde veya bir VM 'de yedeklemek için şu komutu çalıştırın:
```bash
pg_dump -Fc -v --host=<host> --username=<name> --dbname=<database name> -f <database>.dump
```
Örneğin, yerel bir sunucunuz ve içinde **TestDB** adlı bir veritabanınız varsa
```bash
pg_dump -Fc -v --host=localhost --username=masterlogin --dbname=testdb -f testdb.dump
```


## <a name="restore-the-data-into-the-target-azure-database-for-postgresql-using-pg_restore"></a>Pg_restore kullanarak verileri PostgreSQL için Azure veritabanı 'na geri yükleme
Hedef veritabanını oluşturduktan sonra, verileri döküm dosyasından hedef veritabanına geri yüklemek için pg_restore komutunu ve-d,--dbname parametresini kullanabilirsiniz.
```bash
pg_restore -v --no-owner --host=<server name> --port=<port> --username=<user-name> --dbname=<target database name> <database>.dump
```

--No-Owner parametresini eklemek, geri yükleme sırasında oluşturulan tüm nesnelerin,--username ile belirtilen kullanıcıya ait olmasını sağlar. Daha fazla bilgi için bkz. [pg_restore](https://www.postgresql.org/docs/9.6/static/app-pgrestore.html)resmi PostgreSQL belgeleri.

> [!NOTE]
> PostgreSQL sunucunuz TLS/SSL bağlantıları gerektiriyorsa (PostgreSQL için Azure veritabanı sunucuları için varsayılan olarak açık), `PGSSLMODE=require` pg_restore ARACıNıN TLS ile bağlanacağı bir ortam değişkeni ayarlayın. TLS olmadan hata okunmayabilir  `FATAL:  SSL connection is required. Please specify SSL options and retry.`
>
> Windows komut satırında, `SET PGSSLMODE=require` pg_restore komutunu çalıştırmadan önce komutunu çalıştırın. Linux veya bash içinde `export PGSSLMODE=require` pg_restore komutunu çalıştırmadan önce komutunu çalıştırın.
>

Bu örnekte, **TestDB. dump** döküm dosyasındaki verileri, **mydemoserver.Postgres.Database.Azure.com**hedef sunucusundaki **mypgsqldb** veritabanına geri yükleyin.

Bu **Pg_restore** **tek sunucu**için nasıl kullanılacağına ilişkin bir örnek aşağıda verilmiştir:

```bash
pg_restore -v --no-owner --host=mydemoserver.postgres.database.azure.com --port=5432 --username=mylogin@mydemoserver --dbname=mypgsqldb testdb.dump
```
**Esnek sunucu**için bu **pg_restore** kullanmanın bir örneği aşağıda verilmiştir:

```bash
pg_restore -v --no-owner --host=mydemoserver.postgres.database.azure.com --port=5432 --username=mylogin --dbname=mypgsqldb testdb.dump
```
---

## <a name="optimizing-the-migration-process"></a>Geçiş işlemini iyileştirme

Mevcut PostgreSQL veritabanınızı PostgreSQL için Azure veritabanı 'na geçirmenin bir yolu, kaynağı kaynak üzerinde yedeklemeli ve Azure 'da geri yüklemektir. Geçişi gerçekleştirmek için gereken süreyi en aza indirmek için yedekleme ve geri yükleme komutlarıyla aşağıdaki parametreleri kullanmayı göz önünde bulundurun.

> [!NOTE]
> Ayrıntılı sözdizimi bilgileri için [pg_dump](https://www.postgresql.org/docs/current/static/app-pgdump.html) ve [pg_restore](https://www.postgresql.org/docs/current/static/app-pgrestore.html)makalelerine bakın.
>

### <a name="for-the-backup"></a>Yedekleme için
- Geri yüklemeyi hızlandırmak üzere paralel olarak gerçekleştirebilmek için-FC anahtarıyla yedeklemeyi gerçekleştirin. Örnek:

    ```bash
    pg_dump -h my-source-server-name -U source-server-username -Fc -d source-databasename -f Z:\Data\Backups\my-database-backup.dump
    ```

### <a name="for-the-restore"></a>Geri yükleme için
- Yedekleme dosyasını, geçiş yaptığınız PostgreSQL için Azure veritabanı sunucusu ile aynı bölgedeki bir Azure VM 'ye taşımanızı ve ağ gecikmesini azaltmak için bu sanal makineden pg_restore almanızı öneririz. Ayrıca, sanal makinenin [hızlandırılmış ağ](../virtual-network/create-vm-accelerated-networking-powershell.md) etkinleştirilmiş olarak oluşturulmasını öneririz.

- Varsayılan olarak zaten yapılmalıdır, ancak create INDEX deyimlerinin veri ekleme deyimlerinden sonra olduğunu doğrulamak için döküm dosyasını açın. Böyle değilse, veri eklendikten sonra CREATE INDEX deyimlerini taşıyın.

- Restore ile paralel hale getirmek için-FC ve-j anahtarlarıyla geri yükleyin *#* . *#* , hedef sunucudaki çekirdekler sayısıdır. *#* Etkiyi görmek için hedef sunucunun çekirdek sayısının iki katı olarak ayarlamayı da deneyebilirsiniz. Örnek:

Bu **Pg_restore** **tek sunucu**için nasıl kullanılacağına ilişkin bir örnek aşağıda verilmiştir:
```bash
 pg_restore -h my-target-server.postgres.database.azure.com -U azure-postgres-username@my-target-server -Fc -j 4 -d my-target-databasename Z:\Data\Backups\my-database-backup.dump
```
**Esnek sunucu**için bu **pg_restore** kullanmanın bir örneği aşağıda verilmiştir:
```bash
 pg_restore -h my-target-server.postgres.database.azure.com -U azure-postgres-username@my-target-server -Fc -j 4 -d my-target-databasename Z:\Data\Backups\my-database-backup.dump
 ```

- Ayrıca, sonda ve komut *kümesi synchronous_commit = on '* a *synchronous_commit = off;* komut kümesini ekleyerek döküm dosyasını düzenleyebilirsiniz. Son olarak, uygulamalar verileri değiştirmeden önce verilerin daha sonra kaybedilmesine neden olabilir.

- PostgreSQL için Azure veritabanı sunucusunda, geri yüklemeden önce aşağıdakileri yapmayı deneyin:
    - Geçiş sırasında bu istatistiklere gerek duyulmadığından, sorgu performansını izlemeyi devre dışı bırakın. Bunu, pg_stat_statements. Track, pg_qs. query_capture_mode ve pgms_wait_sampling. query_capture_mode öğesini NONE olarak ayarlayarak yapabilirsiniz.

    - Geçişi hızlandırmak için 32 sanal çekirdek bellek için Iyileştirilmiş gibi yüksek bir işlem ve yüksek bellek SKU 'su kullanın. Geri yükleme tamamlandıktan sonra tercih ettiğiniz SKU 'nuzu kolayca azaltabilirsiniz. SKU 'nun daha yüksek olması, pg_restore komutunda karşılık gelen parametreyi artırarak elde edebilirsiniz `-j` .

    - Hedef sunucuda daha fazla ıOPS, geri yükleme performansını iyileştirebilir. Sunucunun depolama boyutunu artırarak daha fazla ıOPS sağlayabilirsiniz. Bu ayar geri alınamaz, ancak daha yüksek bir ıOPS 'nin gelecekte gerçek iş yükünüzün avantajına sahip olup olmadığını düşünün.

Bu komutları üretimde kullanmadan önce test ortamında sınamayı ve doğrulamayı unutmayın.

## <a name="next-steps"></a>Sonraki adımlar
- Dışarı aktarma ve içeri aktarma kullanarak bir PostgreSQL veritabanını geçirmek için, bkz. [dışarı aktarma ve içeri aktarma kullanarak PostgreSQL veritabanınızı geçirme](howto-migrate-using-export-and-import.md).
- Veritabanını PostgreSQL için Azure veritabanı 'na geçirme hakkında daha fazla bilgi için bkz. [veritabanı geçiş kılavuzu](https://aka.ms/datamigration).
