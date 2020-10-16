---
title: Hızlandırılmış veritabanı kurtarma
titleSuffix: Azure SQL
description: Hızlandırılmış veritabanı kurtarma, Azure SQL portföyündeki veritabanları için hızlı ve tutarlı veritabanı kurtarma, anlık işlem geri alma ve agresif günlük kesme sağlar.
ms.service: sql-database
ms.subservice: high-availability
ms.custom: sqldbrb=4
ms.devlang: ''
ms.topic: conceptual
author: mashamsft
ms.author: mathoma
ms.reviewer: sstein
ms.date: 05/19/2020
ms.openlocfilehash: 4c679b6bb0f5645ea7a972be03ba3621b824a501
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91327638"
---
# <a name="accelerated-database-recovery-in-azure-sql"></a>Azure SQL 'de hızlandırılmış veritabanı kurtarma 
[!INCLUDE[appliesto-sqldb-sqlmi](includes/appliesto-sqldb-sqlmi.md)]

**Hızlandırılmış veritabanı kurtarma (ADR)** , özellikle uzun süre çalışan işlemler söz konusu olduğunda veritabanı kullanılabilirliğini, SQL Server veritabanı altyapısı kurtarma işlemini yeniden tasarlayarak büyük ölçüde artıran bir SQL Server veritabanı altyapısı özelliğidir. 

ADR Şu anda Azure SQL veritabanı, Azure SQL yönetilen örneği, Azure SYNAPSE Analytics 'teki veritabanları (Şu anda önizleme aşamasındadır) ve SQL Server 2019 ' den başlayarak Azure VM 'lerinde SQL Server kullanılabilir. 

> [!NOTE] 
> ADR, Azure SQL veritabanı ve Azure SQL yönetilen örneği 'nde varsayılan olarak etkindir ve her iki ürün için ADR 'nin devre dışı bırakılması desteklenmez. 

## <a name="overview"></a>Genel Bakış

ADR 'nin başlıca avantajları şunlardır:

- **Hızlı ve tutarlı veritabanı kurtarma**

  ADR ile, uzun süre çalışan işlemler genel kurtarma süresini etkilemez, sistemdeki etkin işlem sayısı veya boyutları ne olursa olsun hızlı ve tutarlı veritabanı kurtarmayı etkinleştirir.

- **Anlık işlem geri alma**

  ADR ile işlem geri alma işlemi, işlemin etkin olduğu zamandan veya gerçekleştirilen güncelleştirmelerin sayısına bakılmaksızın anında gerçekleşir.

- **Agresif günlük kesilmesi**

  ADR ile, işlem günlüğü, etkin uzun süreli işlemlerin varlığına bile, denetimin dışına büyümesini önleyen bir şekilde kesilir.

## <a name="standard-database-recovery-process"></a>Standart veritabanı kurtarma işlemi

Veritabanı [Kurtarma, bu kurtarma modelini](https://people.eecs.berkeley.edu/~brewer/cs262/Aries.pdf) izler ve aşağıdaki diyagramda gösterilen ve diyagramda daha ayrıntılı olarak açıklanacak üç aşamadan oluşur.

![geçerli kurtarma işlemi](./media/accelerated-database-recovery/current-recovery-process.png)

- **Analiz aşaması**

  Son başarılı denetim noktasının başından (veya en eski kirli sayfa LSN), veritabanının durdurulduğu sırada her bir işlemin durumunu tespit etmek için işlem günlüğünü ileri doğru taratın.

- **Yeniden yineleme aşaması**

  Tüm kaydedilmiş işlemleri yeniden gerçekleştirerek veritabanını kilitlenme süresi sırasında duruma getirmek için, işlem günlüğü sonuna kadar en eski işlenmemiş işlemden ileri tarama.

- **Geri alma aşaması**

  Kilitlenme süresi itibariyle etkin olan her işlem için, bu işlemin gerçekleştirdiği işlemleri geri alarak günlüğü geriye doğru geçer.

Bu tasarıma bağlı olarak, SQL Server veritabanı altyapısının beklenmeyen bir yeniden başlatmadan kurtarmak için geçen süre (kabaca), kilitlenme sırasında sistemdeki en uzun etkin işlemin boyutuyla orantılıdır. Kurtarma işlemi tamamlanmamış tüm işlemlerin geri alınmasını gerektirir. Gerekli sürenin uzunluğu, işlemin gerçekleştirildiği iş ile orantılıdır ve etkin olduğu zaman. Bu nedenle, kurtarma işlemi uzun süre çalışan işlemler (büyük bir tabloya karşı büyük toplu ekleme işlemleri veya dizin derleme işlemleri gibi) bulunması uzun sürebilir.

Ayrıca, bu tasarıma göre büyük bir işlemin iptal edilmesi/geri alınması, yukarıda açıklandığı gibi aynı geri alma kurtarma aşamasını kullandığından uzun zaman alabilir.

Ayrıca, SQL Server veritabanı altyapısı uzun süre çalışan işlemler olduğunda işlem günlüğünü kesemez çünkü kurtarma ve geri alma işlemleri için karşılık gelen günlük kayıtları gereklidir. SQL Server veritabanı altyapısının bu tasarımının bir sonucu olarak, bazı müşteriler, işlem günlüğü boyutunun çok büyük bir şekilde büyüdüğü ve büyük miktarlarda sürücü alanı tükettiği sorunu ortaya almak için kullanılır.

## <a name="the-accelerated-database-recovery-process"></a>Hızlandırılmış veritabanı kurtarma işlemi

ADR, SQL Server veritabanı altyapısı kurtarma sürecini tamamen yeniden tasarlayarak Yukarıdaki sorunları giderir:

- Günlüğü en eski etkin işlemin başlangıcına kadar taramak zorunda kalmadan, bunu sabit bir zaman/anında yapın. ADR ile, işlem günlüğü yalnızca son başarılı denetim noktasından (veya en eski kirli sayfa günlüğü sıra numarası (LSN)) işlenir. Sonuç olarak, kurtarma süresi uzun süren işlemlerden etkilenmez.
- Tüm işlem için günlüğü işlemek zorunda olmadığından, gerekli işlem günlüğü alanını en aza indirin. Sonuç olarak, işlem günlüğü, denetim noktaları ve yedeklemeler gerçekleşilerek kesilebilir.

ADR yüksek düzeyde, tüm fiziksel veritabanı değişikliklerinin sürümü çalıştırılarak hızlı veritabanı kurtarmaya erişir ve yalnızca sınırlı olan ve neredeyse anında geri alınabilecek mantıksal işlemleri geri alabilir. Kilitlenme süresi itibariyle etkin olan herhangi bir işlem iptal edildi olarak işaretlenir ve bu nedenle, bu işlemler tarafından oluşturulan tüm sürümler eşzamanlı kullanıcı sorguları tarafından yoksayılabilir.

ADR kurtarma işlemi, geçerli kurtarma işlemiyle aynı üç aşamadan oluşur. Bu aşamaların ADR ile nasıl çalıştığı, aşağıdaki diyagramda gösterilmektedir ve diyagram sonrasında daha ayrıntılı olarak açıklanmıştır.

![ADR kurtarma işlemi](./media/accelerated-database-recovery/adr-recovery-process.png)

- **Analiz aşaması**

  İşlem, sLog dosyası yeniden oluşturma ve sürüm bilgisi olmayan işlemler için günlük kayıtlarının kopyalanması eklenmesiyle aynı kalır.
  
- Yeniden **yineleme** aşaması

  İki aşamaya kesildi (P)
  - 1. aşama

      SLog öğesinden Yinele (en eski, son denetim noktasına kadar). Yalnızca sLog ' dan birkaç kaydı işlemesi gerektiğinden, yineleme hızlı bir işlemdir.

  - 2. aşama

     Işlem günlüğünden yineleme, son denetim noktasından başlar (en eski işlenmemiş işlem yerine)

- **Geri alma aşaması**

   ADR ile geri alma aşaması, sürüm temelli olmayan işlemleri ve kalıcı sürüm deposunu (PVS), satır düzeyinde sürüm tabanlı geri alma işlemini gerçekleştirmek üzere mantıksal geri alma ile geri almak için sLog kullanarak neredeyse anında tamamlar.

## <a name="adr-recovery-components"></a>ADR kurtarma bileşenleri

ADR 'nin dört temel bileşeni şunlardır:

- **Kalıcı sürüm deposu (PVS)**

  Kalıcı sürüm deposu, geleneksel sürüm deposu yerine veritabanında oluşturulan satır sürümlerinin kalıcı hale getirilmesine yönelik yeni bir SQL Server veritabanı altyapısı mekanizmasıdır `tempdb` . PVS, kaynak yalıtımına izin verir ve okunabilir ikinciller kullanılabilirliğini geliştirir.

- **Mantıksal olarak döndürülüyor**

  Mantıksal geri döndürme, satır düzeyinde sürüm tabanlı geri alma gerçekleştirmekten sorumlu zaman uyumsuz bir işlemdir ve tüm sürümlenmiş işlemler için anlık işlem geri alma ve geri alma sağlar. Mantıksal döndürmeyi şu şekilde gerçekleştirilir:

  - Tüm durdurulan işlemleri izleyin ve bunları diğer işlemlere görünmez olarak işaretleyin. 
  - İşlem günlüğünü fiziksel olarak taramak ve tek seferde değişiklikleri geri almak yerine tüm kullanıcı işlemleri için PVS 'yi kullanarak geri alma işlemi gerçekleştiriliyor.
  - İşlem iptalinden hemen sonra tüm kilitler serbest bırakılıyor. Abort yalnızca bellekteki değişiklikleri işaretlemeyi içerdiğinden, işlem çok verimlidir ve bu nedenle kilitlerin uzun bir süre tutulması gerekmez.

- **sLog**

  sLog, sürüm dışı işlemlere ait günlük kayıtlarını depolayan, ikincil bir bellek içi günlük akışıdır (meta veri önbelleği eksik doğrulaması, kilit alma işlemleri vb.). SLog:

  - Düşük birim ve bellek içi
  - Denetim noktası işlemi sırasında serileştirilerek diskte kalıcı hale getirildi
  - İşlemler işleme olarak düzenli aralıklarla kesildi
  - Yalnızca sürümlenmemiş işlemleri işleyerek Yinele ve geri al işlemini hızlandırır  
  - Yalnızca gerekli günlük kayıtlarını koruyarak agresif işlem günlüğü kesilmesine izin vermez

- **Leyicisi**

  Temizleyici, düzenli aralıklarla uyandığı ve gerekli olmayan sayfa sürümlerini temizlediğinde zaman uyumsuz işlemdir.

## <a name="accelerated-database-recovery-patterns"></a>Hızlandırılmış veritabanı kurtarma desenleri

ADR 'den en çok kullanılan iş yükü türleri şunlardır:

- Uzun süre çalışan işlemlere sahip iş yükleri.
- Etkin işlemlerin işlem günlüğünün önemli ölçüde büyümesine neden olduğu görülmüş olan iş yükleri.  
- Uzun süre çalışan çok sayıda kurtarma (beklenmeyen hizmet yeniden başlatma veya el ile işlem geri alma gibi) nedeniyle fazla kullanım süresi dolan iş yükleri.
