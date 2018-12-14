---
title: Azure SQL veritabanı yönetilen örneği için SQL Server örneği geçirme | Microsoft Docs
description: Azure SQL veritabanı yönetilen örneği için bir SQL Server örneği geçirmeyi öğrenin.
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: bonova
ms.author: bonova
ms.reviewer: carlrab
manager: craigg
ms.date: 10/15/2018
ms.openlocfilehash: 3808511e588ba4284dee16cf7ca88bfd5a382c3a
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2018
ms.locfileid: "53337487"
---
# <a name="sql-server-instance-migration-to-azure-sql-database-managed-instance"></a>Azure SQL veritabanı yönetilen örneği SQL Server örneği geçirme

Bu makalede, bir SQL Server 2005 veya üzeri sürümü örneğine geçirmek için yöntemler hakkında bilgi [Azure SQL veritabanı yönetilen örneği](sql-database-managed-instance.md).

Yüksek düzeyde, veritabanı geçiş işlemi aşağıdaki gibi görünür:

![Geçiş işlemi](./media/sql-database-managed-instance-migration/migration-process.png)

- [Yönetilen örnek uyumluluğunu değerlendirmek](#assess-managed-instance-compatibility)
- [Uygulama bağlantı seçeneğini belirleme](sql-database-managed-instance-connect-app.md)
- [En iyi şekilde boyutlandırılmış yönetilen örneğine dağıtma](#deploy-to-an-optimally-sized-managed-instance)
- [Geçiş yöntemi seçin ve geçirme](#select-migration-method-and-migrate)
- [Uygulama izleme](#monitor-applications)

> [!NOTE]
> Tek bir veritabanı tek veritabanı veya elastik havuzun geçirmek için bkz [bir SQL Server veritabanını Azure SQL veritabanı'na geçirme](sql-database-cloud-migrate.md).

## <a name="assess-managed-instance-compatibility"></a>Yönetilen örnek uyumluluğunu değerlendirmek

İlk olarak, yönetilen örneği, uygulamanızın veritabanı gereksinimleriyle uyumlu olup olmadığını belirler. Yönetilen örnek kolay lift and shift ile geçiş için çoğu şirket içi SQL Server kullanan mevcut uygulamaları ya da sanal makineler sağlamak için tasarlanmıştır. Ancak, bazen özellikleri gerektirebilir veya henüz desteklenmeyen bazı özellikler ve geçici bir çözüm uygulama maliyeti çok yüksek.

Kullanım [Data Migration Yardımcısı (DMA)](https://docs.microsoft.com/sql/dma/dma-overview) olası algılamak için veritabanı işlevselliğini etkileyen Azure SQL veritabanı uyumluluk sorunları. DMA yönetilen örneğe geçiş hedef olarak henüz desteklemiyor, ancak değerlendirme Azure SQL veritabanınızda çalıştırın ve dikkatli bir şekilde bildirilen özellik eşliği ve uyumluluk sorunlarına karşı ürün belgelerinin listesini incelemek için önerilir. Bkz: [Azure SQL veritabanı özellikleri](sql-database-features.md) denetlemek için bazı bildirilen engelleyici sorunlar yönetilen örneğinde engelleyicilerin ortadan çoğu engelleme sorunları için bir Azure SQL veritabanına geçiş engelleme kaldırıldı, yönetilen ile vardır Örneği. Örnek veritabanları arası sorgular gibi özellikler, aynı örneği, diğer SQL kaynakları, CLR, genel geçici tablolar, bağlantılı sunucuya içinde veritabanları arası işlemler için örnek düzeyi görünümleri, hizmet aracısı ve benzeri yönetilen örnekleri'nde kullanılabilir.

Varsa bazı bildirilen Azure SQL veritabanı yönetilen örneği'nde kaldırılmaz engelleme sorunları, alternatif bir seçenek gibi düşünün gerekebilir [azure'daki sanal makinelerde SQL Server](https://azure.microsoft.com/services/virtual-machines/sql-server/). İşte bazı örnekler:

- İşletim sistemi veya dosya sistemi, yükleme üçüncü taraf veya özel aracıları aynı sanal makinede SQL Server örneği için doğrudan erişim gerekiyorsa.
- Yine, FILESTREAM gibi desteklenmeyen özelliklerle ilgili katı bağımlılığı varsa / FileTable, PolyBase ve çapraz örnek işlemleri.
- Kesinlikle, belirli bir SQL Server sürümünde kalmak ihtiyacınız varsa (2012 örneği için).
- İşlem gereksinimlerinizi çok daha düşük olduğunda bu yönetilen örnek genel önizlemede sunar (bir sanal çekirdek, örneği için) ve veritabanı birleştirme kabul edilebilir bir seçenek değil.

## <a name="deploy-to-an-optimally-sized-managed-instance"></a>En iyi şekilde boyutlandırılmış yönetilen örneğine dağıtma

Yönetilen örnek buluta taşımak için planlama şirket içi iş yükleri için uygun hale getirilir. Tanıttığı bir [yeni satın alma modeli](sql-database-service-tiers-vcore.md) seçerken doğru düzeyde kaynakları iş yükleriniz için büyük esneklik sağlar. Şirket içi dünyasında, fiziksel çekirdek ve g/ç bant genişliği'ni kullanarak bu iş yükleri boyutlandırma için büyük olasılıkla alışkın olduğunuz. Yeni satın alma modeli yönetilen örneği için temel sanal çekirdek ya da "Şununla," ek depolama alanı ve kullanılabilir GÇ ayrı olarak temel alır. VCore modeli bir basittir ve kullandığınız bulut işlem gereksinimlerinizi anlamak için şirket içi bugün. Bu yeni modeli, hedef ortamınızda bulut için doğru boyutu sağlar.

İşlem seçebilirsiniz ve depolama kaynakları dağıtım süresi ve daha sonra Tanıtımı kullanarak uygulamanızı için kapalı kalma süresi olmadan değiştirin [Azure portalında](sql-database-scale-resources.md):

![Yönetilen örnek boyutlandırma](./media/sql-database-managed-instance-migration/managed-instance-sizing.png)

Sanal ağ altyapısı ve yönetilen örneğe nasıl oluşturulacağını öğrenmek için bkz: [bir yönetilen örnek oluşturma](sql-database-managed-instance-get-started.md).

> [!IMPORTANT]
> Hedef sanal ağ ve alt ağ her zaman içinde belge tutmak önemlidir [yönetilen örnek sanal ağ gereksinimleri](sql-database-managed-instance-connectivity-architecture.md#network-requirements). Hiçbir uyumsuzluk yeni kopyalarını oluşturmak veya önceden oluşturulmuş bu kullanarak engelleyebilirsiniz. Daha fazla bilgi edinin [yeni oluşturma](sql-database-managed-instance-create-vnet-subnet.md) ve [mevcut yapılandırma](sql-database-managed-instance-configure-vnet-subnet.md) ağlar.

## <a name="select-migration-method-and-migrate"></a>Geçiş yöntemi seçin ve geçirme

Yönetilen örnek hedefleri kullanıcı senaryoları yığın veritabanı geçişi, şirket içi veya Iaas veritabanı uygulamaları gerekir. Lift- and -shift arka uç, düzenli olarak örnek düzeyi kullanın ve / veya platformlar arası işlevleri uygulamaların gerektiğinde bunlar en iyi seçimdir. Senaryonuz buysa, karşılık gelen bir Azure ortamında, uygulamalarının mimarisini yeniden tasarlamak zorunda kalmadan tüm örneğine taşıyabilirsiniz.

SQL örnekleri taşımak için dikkatli bir şekilde planlamanız gerekir:

- (Çalışan aynı örneğinin üzerinde birlikte bulunan olanlar) olması gereken tüm veritabanlarının geçiş
- Oturum açma bilgileri, kimlik bilgileri, SQL Aracısı işleri ve işleçler ve sunucu düzeyi Tetikleyiciler dahil olmak üzere, uygulamanızın bağımlı örnek düzeyi nesneleri geçirme.

Yönetilen örnek, yerleşik olarak gibi platforma normal DBA etkinliklerin bazıları temsilci olanak sağlayan tam olarak yönetilen bir hizmettir. Bu nedenle, bazı örnek düzeyi verileri, düzenli yedeklemeler veya Always On yapılandırması için bakım işleri gibi olarak geçirilmesi gerekmez [yüksek kullanılabilirlik](sql-database-high-availability.md) yerleşiktir.

Yönetilen örnek (şu anda bunlar yalnızca desteklenen geçiş yöntemleridir) aşağıdaki veritabanı geçiş seçeneklerini destekler:

- Azure veritabanı geçiş hizmeti - sıfıra yakın kapalı kalma süresiyle geçiş
- Yerel `RESTORE DATABASE FROM URL` - yerel SQL Server yedeklemeleri kullanır ve bazı kapalı kalma süresi gerektirir.

### <a name="azure-database-migration-service"></a>Azure Veritabanı Geçiş Hizmeti

[Azure veritabanı geçiş hizmeti (DMS)](../dms/dms-overview.md) birden çok veritabanı kaynağını sorunsuz geçiş için en düşük kapalı kalma süresi ile Azure Data platformlarına sağlamak için tasarlanmış, tam olarak yönetilen bir hizmettir. Bu hizmeti mevcut üçüncü taraf ve SQL Server veritabanlarını Azure'a taşımak için gereken görevleri kolaylaştırır. Dağıtım seçenekleri genel Önizleme sırasında bir Azure sanal Makineler'de Azure SQL veritabanı yönetilen örneği ve SQL Server içerir. DMS önerilen geçiş, kurumsal iş yükleri için yöntemidir.

Şirket içi SQL Server üzerinde SQL Server Integration Services (SSIS) kullandığınız DMS SSIS paketlerini depolar geçirme SSIS kataloğunu (SSISDB) henüz desteklemiyor, ancak Azure-SSIS Integration Runtime (IR) Azure Data Factory (ADF) sağlayabilirsiniz, olur Azure SQL veritabanı'nda yeni SSISDB oluşturma/yönetilen örnek ve ardından ona paketlerinizi yeniden, bkz: [ADF içinde Azure-SSIS IR oluşturma](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime).

DMS için bu senaryo ve yapılandırma adımları hakkında daha fazla bilgi edinmek için [şirket içi veritabanı DMS kullanarak yönetilen örneği'ne geçiş](../dms/tutorial-sql-server-to-managed-instance.md).  

### <a name="native-restore-from-url"></a>URL yerel YEDEKTEN geri yükleyin

SQL Server şirket içinden alınan yerel yedeklemeler (.bak dosyaları) geri yükleme veya [sanal makinelerde SQL Server](https://azure.microsoft.com/services/virtual-machines/sql-server/)üzerinden [Azure depolama](https://azure.microsoft.com/services/storage/), bir SQL DB yönetilen örneği önemli özellikleri, hızlı ve kolay bir şekilde çevrimdışı etkinleştirir, geçiş veritabanı.

Aşağıdaki diyagram, işlemin üst düzey bir genel bakış sağlar:

![geçiş akışı](./media/sql-database-managed-instance-migration/migration-flow.png)

Aşağıdaki tabloda, kaynak SQL Server sürümüne bağlı olarak kullanabileceğiniz yöntemleri çalıştırdığınız hakkında daha fazla bilgi sağlar:

|Adım|SQL altyapısı ve sürüm|Yedekleme / geri yükleme yöntemi|
|---|---|---|
|Azure Storage'a yedekleme yerleştirin|Önceki SQL 2012 SP1 CU2|.Bak dosyası doğrudan Azure depolamaya yükleme|
||2012 SP1 CU2 - 2016|Kullanım dışı doğrudan Yedekleme kullanılarak [WITH CREDENTIAL](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-transact-sql) söz dizimi|
||2016 ve üzeri|Doğrudan Yedekleme kullanılarak [ile SAS kimlik bilgisi](https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-backup-to-url)|
|Azure depolama biriminden yönetilen örneğine geri yükleyin.|[Geri yükleme kaynak URL ile SAS kimlik bilgisi](sql-database-managed-instance-get-started-restore.md)|

> [!IMPORTANT]
> - Tarafından korunan bir veritabanını geçirirken [saydam veri şifrelemesi](transparent-data-encryption-azure-sql.md) Azure SQL veritabanı yönetilen yerel bir geri yükleme seçeneğini kullanarak örneği için karşılık gelen sertifika şirket içi veya SQL Server Iaas geçirilmesi gerekiyor Veritabanı geri yükleme işleminden önce. Ayrıntılı adımlar için bkz. [yönetilen örneğe geçirme TDE cert](sql-database-managed-instance-migrate-tde-certificate.md)
> - Sistem veritabanlarının geri yükleme desteklenmiyor. Örnek düzeyi nesneler (ana veya msdb veritabanlarında depolanan) geçirmek için bunları komut dosyası ve hedef örneğinde T-SQL betiklerini çalıştırma öneririz.

Bir SAS kimlik bilgisi kullanarak yönetilen örneği için veritabanı yedeklemesini geri yükleme işlemini gösteren bir hızlı başlangıç için bkz: [yedekten bir yönetilen örneğe geri](sql-database-managed-instance-get-started-restore.md).

> [!VIDEO https://www.youtube.com/embed/RxWYojo_Y3Q]

## <a name="monitor-applications"></a>Uygulamaları izleme

Uygulama davranışını ve geçişten sonra performansı izleyin. Yönetilen örneği'nde bazı değişiklikler yalnızca bir kez etkin [veritabanı uyumluluk düzeyi değiştirildi](https://docs.microsoft.com/sql/relational-databases/databases/view-or-change-the-compatibility-level-of-a-database). Veritabanını Azure SQL veritabanına geçirme özgün uyumluluk düzeyi, çoğu durumda tutar. Bir kullanıcı veritabanı uyumluluk düzeyini 100 veya daha yüksek geçişten önce, aynı geçişten sonra kalır. Bir kullanıcı veritabanı uyumluluk düzeyi 90 yükseltilen veritabanında, geçiş işleminden önce olduysa uyumluluk düzeyi en düşük desteklenen uyumluluk düzeyinde yönetilen örneği olan 100'e ayarlanır. Sistem veritabanlarının uyumluluk düzeyi, 140 değeri.

Geçiş risklerini azaltmak için yalnızca performansı izleme sonra veritabanı uyumluluk düzeyini değiştirin. Kullanım Query Store açıklandığı gibi iş yükü performansı önce ve sonra veritabanı uyumluluk düzeyi değiştirme hakkında bilgi almak için en uygun aracı olarak [tutmak performans kararlılık yükseltme sırasında SQL Server sürüme](https://docs.microsoft.com/sql/relational-databases/performance/query-store-usage-scenarios#CEUpgrade).

Tam olarak yönetilen bir platformda olduktan sonra otomatik olarak SQL veritabanı hizmetinin bir parçası olarak sağlanan avantajlar yararlanın. Örneği için yedeklemeler yönetilen örneği'nde oluşturmanız gerekmez - hizmet yedekleme sizin için otomatik olarak gerçekleştirir. Artık zamanlama, alma ve yedekleri yönetme hakkında endişe gerekir. Yönetilen örnek kullanarak bu elde tutma dönemi içinde zaman içinde herhangi bir noktasına geri yükleme olanağı sağlayan [işaret zaman Kurtarma (PITR) içinde](sql-database-recovery-using-backups.md#point-in-time-restore). Ayrıca, yüksek oranda kullanılabilir ayarlama endişelenmeniz gerekmez [yüksek kullanılabilirlik](sql-database-high-availability.md) yerleşiktir.

Güvenliği güçlendirmek için kullanılabilir olan özelliklerin bazılarını kullanarak göz önünde bulundurun:

- Azure Active Directory kimlik doğrulaması ve veritabanı düzeyinde
- Kullanım [Gelişmiş güvenlik özellikleri](sql-database-security-overview.md) gibi [denetim](sql-database-managed-instance-auditing.md), [tehdit algılama](sql-advanced-threat-protection.md), [satır düzeyi güvenlik](https://docs.microsoft.com/sql/relational-databases/security/row-level-security), ve [dinamik Veri maskeleme](https://docs.microsoft.com/sql/relational-databases/security/dynamic-data-masking) ) örneğinizin güvenliğini sağlamak için.

## <a name="next-steps"></a>Sonraki adımlar

- Yönetilen örnekleri hakkında daha fazla bilgi için bkz: [yönetilen örnek nedir?](sql-database-managed-instance.md).
- Bir yedekten içeren bir öğretici için bkz. [bir yönetilen örnek oluşturma](sql-database-managed-instance-get-started.md).
- DMS kullanarak öğretici gösteren geçiş için bkz: [şirket içi veritabanı DMS kullanarak yönetilen örneği'ne geçiş](../dms/tutorial-sql-server-to-managed-instance.md).  
