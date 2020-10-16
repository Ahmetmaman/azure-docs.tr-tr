---
title: Sorgu istatistikleri koleksiyonunu iyileştirme-PostgreSQL için Azure veritabanı-tek sunucu
description: Bu makalede, bir PostgreSQL için Azure veritabanı 'nda sorgu istatistikleri toplamayı en iyi hale getirebileceğinizi açıklanmaktadır-tek sunucu
author: dianaputnam
ms.author: dianas
ms.service: postgresql
ms.topic: how-to
ms.date: 5/6/2019
ms.openlocfilehash: bc731f6f6a5a60bce0851bf8fe5874f7149f3899
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "90901458"
---
# <a name="optimize-query-statistics-collection-on-an-azure-database-for-postgresql---single-server"></a>PostgreSQL için Azure veritabanı 'nda sorgu istatistikleri toplamayı iyileştirme-tek sunucu
Bu makalede, bir PostgreSQL için Azure veritabanı sunucusu üzerinde sorgu istatistikleri koleksiyonunun nasıl iyileştirileceği açıklanır.

## <a name="use-pg_stats_statements"></a>Pg_stats_statements kullan
**Pg_stat_statements** , PostgreSQL Için Azure veritabanı 'nda varsayılan olarak etkinleştirilen bir PostgreSQL uzantısıdır. Uzantı, bir sunucu tarafından yürütülen tüm SQL deyimlerinin yürütme istatistiklerini izlemek için bir yol sağlar. Bu modül her sorgu yürütmeye takar ve önemsiz olmayan bir performans maliyetiyle birlikte gelir. **Pg_stat_statements** etkinleştirme, disk üzerindeki dosyalara sorgu metin yazmaları zorlar.

Uzun sorgu metnine sahip benzersiz sorgulara sahipseniz veya **pg_stat_statements**etkin bir şekilde izleyemezseniz, en iyi performans için **pg_stat_statements** devre dışı bırakın. Bunu yapmak için ayarı olarak değiştirin `pg_stat_statements.track = NONE` .

Bazı müşteri iş yükleri **pg_stat_statements** devre dışı bırakıldığında yüzde 50 oranında bir performans geliştirmesini gördük. Pg_stat_statements devre dışı bıraktığınızda yaptığınız zorunluluğunu getirir, performans sorunlarını giderememe sorunlardır.

Ayarlamak için `pg_stat_statements.track = NONE` :

- Azure portal, [PostgreSQL kaynak yönetimi sayfasına gidin ve sunucu parametreleri dikey penceresini seçin](howto-configure-server-parameters-using-portal.md).

  :::image type="content" source="./media/howto-optimize-query-stats-collection/pg_stats_statements_portal.png" alt-text="PostgreSQL sunucu parametresi dikey penceresi":::

- [Azure CLI](howto-configure-server-parameters-using-cli.md) az Postgres Server yapılandırma kümesini olarak kullanın `--name pg_stat_statements.track --resource-group myresourcegroup --server mydemoserver --value NONE` .

## <a name="use-the-query-store"></a>Sorgu deposunu kullanma 
PostgreSQL için Azure veritabanı 'ndaki [sorgu depolama](concepts-query-store.md) özelliği sorgu istatistiklerini izlemek için daha etkili bir yöntem sağlar. Bu özelliği *pg_stats_statements*kullanımına alternatif olarak öneririz. 

## <a name="next-steps"></a>Sonraki adımlar
`pg_stat_statements.track = NONE` [Azure Portal](howto-configure-server-parameters-using-portal.md) veya [Azure CLI](howto-configure-server-parameters-using-cli.md)kullanarak ayarlamayı göz önünde bulundurun.

Daha fazla bilgi için bkz. 
- [Sorgu deposu kullanım senaryoları](concepts-query-store-scenarios.md) 
- [Sorgu deposu en iyi yöntemleri](concepts-query-store-best-practices.md) 
