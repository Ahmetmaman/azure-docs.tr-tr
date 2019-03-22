---
title: Azure SQL veritabanı özellik karşılaştırması | Microsoft Docs
description: Bu makalede, Azure SQL veritabanı farklı türdeki kullanılabilir olan özelliklerin SQL Server karşılaştırılmaktadır.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: bonova, carlrab
manager: craigg
ms.date: 02/08/2019
ms.openlocfilehash: 82ed8783444a414d4706f97c8c9b6ea18734561e
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58311756"
---
# <a name="feature-comparison-azure-sql-database-versus-sql-server"></a>Özellik karşılaştırması: Azure SQL veritabanı SQL Server ile karşılaştırması

Azure SQL veritabanı ile SQL Server ortak bir kod temeli paylaşır. Azure SQL veritabanı tarafından desteklenen bir SQL Server özelliklerinin oluşturduğunuz Azure SQL veritabanı türüne bağlıdır. Azure SQL veritabanı ile bir parçası olarak bir veritabanı oluşturabilirsiniz bir [yönetilen örnek](sql-database-managed-instance.md), tek bir veritabanı veya elastik bir havuzun parçası olarak.

Microsoft Azure SQL veritabanı'na özellik eklemeye devam eder. Hizmet güncelleştirmeleri Web sayfası için Azure bu filtreleri kullanarak en son güncelleştirmeler için ziyaret edin:

- [SQL Veritabanı hizmeti](https://azure.microsoft.com/updates/?service=sql-database) için filtrelenmiş.
- SQL Veritabanı özellikleri için Genel Kullanılabilirlik [(GA) duyuruları](https://azure.microsoft.com/updates/?service=sql-database&update-type=general-availability).

## <a name="sql-server-feature-support-in-azure-sql-database"></a>Azure SQL veritabanı'nda SQL Server özellik desteği

Aşağıdaki tabloda, SQL Server'ın temel özelliklerinin listeler ve bağlantı özelliği hakkında daha fazla bilgi için bu özellik kısmen veya tamamen desteklenir ve hakkında bilgi sağlar.

| **SQL özellik** | **Tek veritabanları ve elastik havuzlar tarafından desteklenmiyor** | **Yönetilen örnekleri tarafından desteklenir** |
| --- | --- | --- |
| [Etkin coğrafi çoğaltma](sql-database-active-geo-replication.md) | Evet - tüm hiper ölçekli dışında katmanları hizmet | Hayır, bkz: [otomatik yük devretme grupları](sql-database-auto-failover-group.md) |
| [Otomatik yük devretme grupları](sql-database-auto-failover-group.md) | Evet - tüm hiper ölçekli dışında katmanları hizmet | Evet, içinde [genel önizlemeye sunuldu](sql-database-auto-failover-group.md)|
| [Her zaman şifreli](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine) | Evet - bkz [sertifika deposu](sql-database-always-encrypted.md) ve [anahtar kasası](sql-database-always-encrypted-azure-key-vault.md) | Evet - bkz [sertifika deposu](sql-database-always-encrypted.md) ve [anahtar kasası](sql-database-always-encrypted-azure-key-vault.md) |
| [Always On kullanılabilirlik grupları](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server) | [Yüksek kullanılabilirlik](sql-database-high-availability.md) ile her veritabanı bulunur. Olağanüstü durum kurtarma ele alınmıştır [Azure SQL veritabanı ile iş sürekliliğine genel bakış](sql-database-business-continuity.md) | [Yüksek kullanılabilirlik](sql-database-high-availability.md) ile her veritabanı bulunur. Olağanüstü durum kurtarma ele alınmıştır [Azure SQL veritabanı ile iş sürekliliğine genel bakış](sql-database-business-continuity.md) |
| [Veritabanı ekleme](https://docs.microsoft.com/sql/relational-databases/databases/attach-a-database) | Hayır | Hayır |
| [Uygulama rolleri](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/application-roles) | Evet | Evet |
|[Denetim](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine) | [Evet](sql-database-auditing.md)| [Evet](sql-database-managed-instance-auditing.md) |
| [Otomatik yedekleme](sql-database-automated-backups.md) | Evet | Evet |
| [Otomatik ayarlama (plan zorlama)](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning)| [Evet](sql-database-automatic-tuning.md)| [Evet](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning) |
| [Otomatik ayarlama (dizin)](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning)| [Evet](sql-database-automatic-tuning.md)| Hayır |
| [Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/what-is) | Evet | Evet |
| [BACPAC dosyası (dışarı aktarma)](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/export-a-data-tier-application) | Evet - bkz [SQL veritabanını dışarı aktarma](sql-database-export.md) | Evet |
| [BACPAC dosyası (içeri aktarma)](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/import-a-bacpac-file-to-create-a-new-user-database) | Evet - bkz [SQL veritabanı içeri aktarma](sql-database-import.md) | Evet |
| [Yedekleme komutu](https://docs.microsoft.com/sql/t-sql/statements/backup-transact-sql) | Hayır, yalnızca sistem tarafından başlatılan otomatik yedeklemeler - bkz [otomatik yedeklemeler](sql-database-automated-backups.md) | Sistem tarafından başlatılan otomatik yedeklemeler ve kullanıcı tarafından başlatılan yalnızca kopya yedekleri - bkz [fark yedekleme](sql-database-managed-instance-transact-sql-information.md#backup) |
| [Yerleşik işlevler](https://docs.microsoft.com/sql/t-sql/functions/functions) | Çoğu - bkz ayrı İşlevler | Evet - bkz [saklı yordamlar, İşlevler, Tetikleyiciler farkları](sql-database-managed-instance-transact-sql-information.md#stored-procedures-functions-triggers) |
| [Değişiklik verilerini yakalama](https://docs.microsoft.com/sql/relational-databases/track-changes/about-change-data-capture-sql-server) | Hayır | Evet |
| [Değişiklik izleme](https://docs.microsoft.com/sql/relational-databases/track-changes/about-change-tracking-sql-server) | Evet |Evet |
| [Harmanlama - veritabanı](https://docs.microsoft.com/sql/relational-databases/collations/set-or-change-the-database-collation) | Evet | Evet |
| [Harmanlama - server/örneği](https://docs.microsoft.com/sql/relational-databases/collations/set-or-change-the-server-collation) | Hayır | Evet, içinde [genel önizlemeye sunuldu](scripts/sql-managed-instance-create-powershell-azure-resource-manager-template.md)|
| [Columnstore dizinleri](https://docs.microsoft.com/sql/relational-databases/indexes/columnstore-indexes-overview) | Evet - [Premium katman, standart katman - S3 ve üstü, genel amaçlı katmanı ve iş açısından kritik katmanları](https://docs.microsoft.com/sql/relational-databases/indexes/columnstore-indexes-overview) |Evet |
| [Ortak dil çalışma zamanı (CLR)](https://docs.microsoft.com/sql/relational-databases/clr-integration/common-language-runtime-clr-integration-programming-concepts) | Hayır | Evet - bkz [CLR farkları](sql-database-managed-instance-transact-sql-information.md#clr) |
| [Kapsanan veritabanları](https://docs.microsoft.com/sql/relational-databases/databases/contained-databases) | Evet | Hayır [geri yükleme-belirli bir noktaya geri yükleme gibi hata nedeniyle](sql-database-managed-instance-transact-sql-information.md#cannot-restore-contained-database) |
| [Bağımsız kullanıcılar](https://docs.microsoft.com/sql/relational-databases/security/contained-database-users-making-your-database-portable) | Evet | Evet |
| [Denetim akışı dil anahtar sözcükleri](https://docs.microsoft.com/sql/t-sql/language-elements/control-of-flow) | Evet | Evet |
| [Veritabanları arası sorgular](https://docs.microsoft.com/sql/relational-databases/linked-servers/linked-servers-database-engine) | Hayır - bkz [esnek sorgular](sql-database-elastic-query-overview.md) | Evet, artı [esnek sorgular](sql-database-elastic-query-overview.md) |
| [Veritabanları arası işlemler](https://docs.microsoft.com/sql/relational-databases/linked-servers/linked-servers-database-engine) | Hayır | Evet, örneği içinde. Bkz: [bağlı sunucu farklar](sql-database-managed-instance-transact-sql-information.md#linked-servers) arası örnek sorgular için. |
| [İmleçler](https://docs.microsoft.com/sql/t-sql/language-elements/cursors-transact-sql) | Evet |Evet |
| [Veri sıkıştırma](https://docs.microsoft.com/sql/relational-databases/data-compression/data-compression) | Evet |Evet |
| [Veritabanı posta](https://docs.microsoft.com/sql/relational-databases/database-mail/database-mail) | Hayır | Evet |
| [Veri geçiş hizmeti (DMS)](https://docs.microsoft.com/sql/dma/dma-overview) | Evet | Evet |
| [Veritabanı yansıtma](https://docs.microsoft.com/sql/database-engine/database-mirroring/database-mirroring-sql-server) | Hayır | Hayır |
| [Veritabanı yapılandırma ayarları](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) | Evet | Evet |
| [Veri Kalitesi Hizmetleri (DQS)](https://docs.microsoft.com/sql/data-quality-services/data-quality-services) | Hayır | Hayır |
| [Veritabanı anlık görüntüleri](https://docs.microsoft.com/sql/relational-databases/databases/database-snapshots-sql-server) | Hayır | Hayır |
| [Veri türleri](https://docs.microsoft.com/sql/t-sql/data-types/data-types-transact-sql) | Evet |Evet |
| [DBCC deyimleri](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-transact-sql) | Çoğu - bkz ayrı deyimler | Evet - bkz [DBCC farkları](sql-database-managed-instance-transact-sql-information.md#dbcc) |
| [DDL deyimleri](https://docs.microsoft.com/sql/t-sql/statements/statements) | Çoğu - bkz ayrı deyimler | Evet - bkz [T-SQL farklılıkları](sql-database-managed-instance-transact-sql-information.md) |
| [DDL Tetikleyicileri](https://docs.microsoft.com/sql/relational-databases/triggers/ddl-triggers) | Yalnızca veritabanı |  Evet |
| [Bölüm dağıtılmış görünümleri](https://docs.microsoft.com/sql/t-sql/statements/create-view-transact-sql#partitioned-views) | Hayır | Evet |
| [Dağıtılmış işlemler - MS DTC](https://docs.microsoft.com/sql/relational-databases/native-client-ole-db-transactions/supporting-distributed-transactions) | Hayır - bkz [elastik işlemler](sql-database-elastic-transactions-overview.md) |  Hayır - bkz [bağlı sunucu farkları](sql-database-managed-instance-transact-sql-information.md#linked-servers) |
| [DML deyimleri](https://docs.microsoft.com/sql/t-sql/queries/queries) | Evet | Evet |
| [DML Tetikleyicileri](https://docs.microsoft.com/sql/relational-databases/triggers/create-dml-triggers) | Çoğu - bkz ayrı deyimler |  Evet |
| [DMV’ler](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/system-dynamic-management-views) | Çoğu - bkz ayrı Dmv'ler |  Evet - bkz [T-SQL farklılıkları](sql-database-managed-instance-transact-sql-information.md) |
|[Dinamik veri maskeleme](https://docs.microsoft.com/sql/relational-databases/security/dynamic-data-masking)|[Evet](sql-database-dynamic-data-masking-get-started.md)| [Evet](sql-database-dynamic-data-masking-get-started.md) |
| [Elastik havuzlar](sql-database-elastic-pool.md) | Evet | Yerleşik-tek bir yönetilen örnek aynı kaynak havuzu paylaşan birden çok veritabanına sahip olabilir |
| [Olay bildirimleri](https://docs.microsoft.com/sql/relational-databases/service-broker/event-notifications) | Hayır - bkz [uyarıları](sql-database-insights-alerts-portal.md) | Hayır |
| [İfadeler](https://docs.microsoft.com/sql/t-sql/language-elements/expressions-transact-sql) |Evet | Evet |
| [Genişletilmiş olaylar](https://docs.microsoft.com/sql/relational-databases/extended-events/extended-events) | Bazıları - bkz [SQL veritabanı'nda olaylar genişletilmiş](sql-database-xevent-db-diff-from-svr.md) | Evet - bkz [genişletilmiş olaylar farkları](sql-database-managed-instance-transact-sql-information.md#extended-events) |
| [Genişletilmiş saklı yordamlar](https://docs.microsoft.com/sql/relational-databases/extended-stored-procedures-programming/creating-extended-stored-procedures) | Hayır | Hayır |
[Dosyalar ve dosya grupları](https://docs.microsoft.com/sql/relational-databases/databases/database-files-and-filegroups) | Yalnızca birincil dosya grubu | Evet |
| [FILESTREAM](https://docs.microsoft.com/sql/relational-databases/blob/filestream-sql-server) | Hayır | Hayır |
| [Tam metin araması](https://docs.microsoft.com/sql/relational-databases/search/full-text-search) |  Üçüncü taraf sözcük ayırıcılar desteklenmez |Üçüncü taraf sözcük ayırıcılar desteklenmez |
| [İşlevler](https://docs.microsoft.com/sql/t-sql/functions/functions) | Çoğu - bkz ayrı İşlevler | Evet - bkz [saklı yordamlar, İşlevler, Tetikleyiciler farkları](sql-database-managed-instance-transact-sql-information.md#stored-procedures-functions-triggers) |
| [Coğrafi geri yükleme](sql-database-recovery-using-backups.md#geo-restore) | Evet - tüm hiper ölçekli dışında katmanları hizmet | Hayır, COPY_ONLY geri yükleyebilirsiniz düzenli aralıklarla - aldığınız tam yedeklemeler bakın [fark yedekleme](sql-database-managed-instance-transact-sql-information.md#backup) ve [geri farklar](sql-database-managed-instance-transact-sql-information.md#restore-statement). |
| [Grafik işleme](https://docs.microsoft.com/sql/relational-databases/graphs/sql-graph-overview) | Evet | Evet |
| [Bellek içi iyileştirme](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization) | Evet - [yalnızca Premium ve iş açısından kritik katmanları](sql-database-in-memory.md) | Evet - [iş yalnızca kritik katmanı](sql-database-managed-instance.md) |
| [JSON veri desteği](https://docs.microsoft.com/sql/relational-databases/json/json-data-sql-server) | [Evet](sql-database-json-features.md) | [Evet](sql-database-json-features.md) |
| [Dil öğeleri](https://docs.microsoft.com/sql/t-sql/language-elements/language-elements-transact-sql) | Çoğu - bkz. ayrı ayrı öğeler |  Evet - bkz [T-SQL farklılıkları](sql-database-managed-instance-transact-sql-information.md) |
| [Bağlı sunucular](https://docs.microsoft.com/sql/relational-databases/linked-servers/linked-servers-database-engine) | Hayır - bkz [esnek sorgu](sql-database-elastic-query-horizontal-partitioning.md) | Yalnızca SQL Server ve SQL veritabanı |
| [Günlük aktarma](https://docs.microsoft.com/sql/database-engine/log-shipping/about-log-shipping-sql-server) | [Yüksek kullanılabilirlik](sql-database-high-availability.md) ile her veritabanı bulunur. Olağanüstü durum kurtarma ele alınmıştır [Azure SQL veritabanı ile iş sürekliliğine genel bakış](sql-database-business-continuity.md) |[Yüksek kullanılabilirlik](sql-database-high-availability.md) ile her veritabanı bulunur. Olağanüstü durum kurtarma ele alınmıştır [Azure SQL veritabanı ile iş sürekliliğine genel bakış](sql-database-business-continuity.md) |
| [Ana Veri Hizmetleri (AVH)](https://docs.microsoft.com/sql/master-data-services/master-data-services-overview-mds) | Hayır | Hayır |
| [En az günlük toplu içeri aktarma](https://docs.microsoft.com/sql/relational-databases/import-export/prerequisites-for-minimal-logging-in-bulk-import) | Hayır | Hayır |
| [Sistem verilerini değiştirme](https://docs.microsoft.com/sql/relational-databases/databases/system-databases) | Hayır | Evet |
| [OLE Otomasyonu nesnesi etkin](https://docs.microsoft.com/sql/database-engine/configure-windows/ole-automation-procedures-server-configuration-option) | Hayır | Hayır |
| [Çevrimiçi dizin işlemleri](https://docs.microsoft.com/sql/relational-databases/indexes/perform-index-operations-online) | Evet | Evet |
| [OPENDATASOURCE](https://docs.microsoft.com/sql/t-sql/functions/opendatasource-transact-sql)|Hayır|Evet - bkz [T-SQL farklılıkları](sql-database-managed-instance-transact-sql-information.md)|
| [OPENJSON](https://docs.microsoft.com/sql/t-sql/functions/openjson-transact-sql)|Evet|Evet|
| [OPENQUERY](https://docs.microsoft.com/sql/t-sql/functions/openquery-transact-sql)|Hayır|Evet - bkz [T-SQL farklılıkları](sql-database-managed-instance-transact-sql-information.md)|
| [OPENROWSET](https://docs.microsoft.com/sql/t-sql/functions/openrowset-transact-sql)|Hayır|Evet - bkz [T-SQL farklılıkları](sql-database-managed-instance-transact-sql-information.md)|
| [OPENXML](https://docs.microsoft.com/sql/t-sql/functions/openxml-transact-sql)|Evet|Evet|
| [İşleçler](https://docs.microsoft.com/sql/t-sql/language-elements/operators-transact-sql) | Çoğu - bkz. tek tek işleçler |Evet - bkz [T-SQL farklılıkları](sql-database-managed-instance-transact-sql-information.md) |
| [Bölümleme](https://docs.microsoft.com/sql/relational-databases/partitions/partitioned-tables-and-indexes) | Evet | Evet |
| [Zaman veritabanını geri yükleme noktası](https://docs.microsoft.com/sql/relational-databases/backup-restore/restore-a-sql-server-database-to-a-point-in-time-full-recovery-model) | Evet - bkz. hiper ölçekli - dışındaki tüm hizmet katmanları [SQL veritabanını kurtarma](sql-database-recovery-using-backups.md#point-in-time-restore) | Evet - bkz [SQL veritabanını kurtarma](sql-database-recovery-using-backups.md#point-in-time-restore) |
| [Polybase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide) | Hayır | Hayır |
| [İlke tabanlı yönetim](https://docs.microsoft.com/sql/relational-databases/policy-based-management/administer-servers-by-using-policy-based-management) | Hayır | Hayır |
| [Doğrulamaları](https://docs.microsoft.com/sql/t-sql/queries/predicates) | Evet | Evet |
| [Sorgu bildirimleri](https://docs.microsoft.com/sql/relational-databases/native-client/features/working-with-query-notifications) | Hayır | Evet |
| [Sorgu Performansı İçgörüleri](sql-database-query-performance.md) | Evet | Hayır |
| [R Hizmetleri](https://docs.microsoft.com/sql/advanced-analytics/r-services/sql-server-r-services) | Evet, içinde [genel önizlemeye sunuldu](https://docs.microsoft.com/sql/advanced-analytics/what-s-new-in-sql-server-machine-learning-services)  | Hayır |
| [Kaynak İdarecisi](https://docs.microsoft.com/sql/relational-databases/resource-governor/resource-governor) | Hayır | Evet |
| [RESTORE deyimleri](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-for-restoring-recovering-and-managing-backups-transact-sql) | Hayır | Evet - bkz [farklar geri yükleme](sql-database-managed-instance-transact-sql-information.md#restore-statement) |
| [Veritabanını yedekten geri yükleyin](https://docs.microsoft.com/sql/relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases#restore-data-backups) | Yalnızca - otomatik yedeklerden bkz [SQL veritabanını kurtarma](sql-database-recovery-using-backups.md) | Otomatik yedeklerden - bkz [SQL veritabanı kurtarma](sql-database-recovery-using-backups.md) ve tam yedeklerden - [fark yedekleme](sql-database-managed-instance-transact-sql-information.md#backup) |
| [Satır düzeyi güvenlik](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) | Evet | Evet |
| [Anlamsal arama](https://docs.microsoft.com/sql/relational-databases/search/semantic-search-sql-server) | Hayır | Hayır |
| [Sıra numaraları](https://docs.microsoft.com/sql/relational-databases/sequence-numbers/sequence-numbers) | Evet | Evet |
| [Hizmet Aracısı](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-service-broker) | Hayır | Evet - bkz [hizmet Aracısı farkları](sql-database-managed-instance-transact-sql-information.md#service-broker) |
| [Sunucu Yapılandırma ayarları](https://docs.microsoft.com/sql/database-engine/configure-windows/server-configuration-options-sql-server) | Hayır | Evet - bkz [T-SQL farklılıkları](sql-database-managed-instance-transact-sql-information.md) |
| [Küme deyimleri](https://docs.microsoft.com/sql/t-sql/statements/set-statements-transact-sql) | Çoğu - bkz ayrı deyimler | Evet - bkz [T-SQL farklılıkları](sql-database-managed-instance-transact-sql-information.md)|
| [SMO](https://docs.microsoft.com/sql/relational-databases/server-management-objects-smo/sql-server-management-objects-smo-programming-guide) | Evet | Evet |
| [Uzamsal](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server) | Evet | Evet |
| [SQL analizi](https://docs.microsoft.com/azure/azure-monitor/insights/azure-sql) | Evet | Evet |
| [SQL Data Sync](sql-database-get-started-sql-data-sync.md) | Evet | Hayır |
| [SQL Server Agent](https://docs.microsoft.com/sql/ssms/agent/sql-server-agent) | Hayır - bkz [esnek işler](sql-database-elastic-jobs-getting-started.md) | Evet - bkz [SQL Server Agent farkları](sql-database-managed-instance-transact-sql-information.md#sql-server-agent) |
| [SQL Server Analysis Services (SSAS)](https://docs.microsoft.com/sql/analysis-services/analysis-services) | Hayır - bkz [Azure Analysis Services](https://azure.microsoft.com/services/analysis-services/) | Hayır - bkz [Azure Analysis Services](https://azure.microsoft.com/services/analysis-services/) |
| [SQL Server denetimi](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine) | Hayır - bkz [SQL veritabanı denetimi](sql-database-auditing.md) | Evet - bkz [farklar denetleme](sql-database-managed-instance-transact-sql-information.md#auditing) |
| [SQL Server Veri Araçları (SSDT)](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt) | Evet | Evet |
| [SQL Server Integration Services (SSIS)](https://docs.microsoft.com/sql/integration-services/sql-server-integration-services) | Evet, bir yönetilen SSIS paketleri depolandığı Azure SQL veritabanı tarafından barındırılan ve Azure SSIS tümleştirme çalışma zamanı (IR) yürütülen SSISDB içinde Azure Data Factory (ADF) ortamında görmek [ADF içinde Azure-SSIS IR oluşturma](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime). <br/><br/>SQL veritabanı sunucusu ve yönetilen örneği SSIS özellikleri karşılaştırmak için bkz [karşılaştırma Azure SQL veritabanı tek veritabanlarını elastik havuzları ve yönetilen örneği](../data-factory/create-azure-ssis-integration-runtime.md#compare-sql-database-single-databaseelastic-pool-and-sql-database-managed-instance). | Evet, bir yönetilen SSIS paketleri depolandığı yönetilen örneği tarafından barındırılan ve Azure SSIS tümleştirme çalışma zamanı (IR) yürütülen SSISDB içinde Azure Data Factory (ADF) ortamında görmek [ADF içinde Azure-SSIS IR oluşturma](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime). <br/><br/>SQL veritabanı yönetilen örneği, SSIS özellikleri karşılaştırmak için bkz [karşılaştırma Azure SQL veritabanı tek veritabanlarını elastik havuzları ve yönetilen örneği](../data-factory/create-azure-ssis-integration-runtime.md#compare-sql-database-single-databaseelastic-pool-and-sql-database-managed-instance). |
| [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) | Evet | Evet |
| [SQL Server PowerShell](https://docs.microsoft.com/sql/relational-databases/scripting/sql-server-powershell) | Evet | Evet |
| [SQL Server Profiler](https://docs.microsoft.com/sql/tools/sql-server-profiler/sql-server-profiler) | Hayır - bkz [genişletilmiş olaylar](sql-database-xevent-db-diff-from-svr.md) | Evet |
| [SQL Server çoğaltma](https://docs.microsoft.com/sql/relational-databases/replication/sql-server-replication) | [Yalnızca işlem ve anlık görüntü çoğaltma abonesi](sql-database-single-database-migrate.md) | Evet, içinde [genel önizlemeye sunuldu](https://docs.microsoft.com/sql/relational-databases/replication/replication-with-sql-database-managed-instance) |
| [SQL Server Raporlama Hizmetleri (SSRS)](https://docs.microsoft.com/sql/reporting-services/create-deploy-and-manage-mobile-and-paginated-reports) | Hayır - [Power BI bakın](https://docs.microsoft.com/power-bi/) | Hayır - [Power BI bakın](https://docs.microsoft.com/power-bi/) |
| [Saklı yordamlar](https://docs.microsoft.com/sql/relational-databases/stored-procedures/stored-procedures-database-engine) | Evet | Evet |
| [Sistem saklı işlevleri](https://docs.microsoft.com/sql/relational-databases/system-functions/system-functions-for-transact-sql) | Çoğu - bkz ayrı İşlevler | Evet - bkz [saklı yordamlar, İşlevler, Tetikleyiciler farkları](sql-database-managed-instance-transact-sql-information.md#stored-procedures-functions-triggers) |
| [Sistem saklı yordamları](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/system-stored-procedures-transact-sql) | Bazıları - bkz bireysel saklı yordamlar | Evet - bkz [saklı yordamlar, İşlevler, Tetikleyiciler farkları](sql-database-managed-instance-transact-sql-information.md#stored-procedures-functions-triggers) |
| [Sistem tabloları](https://docs.microsoft.com/sql/relational-databases/system-tables/system-tables-transact-sql) | Bazıları - bkz tek tek tablolar | Evet - bkz [T-SQL farklılıkları](sql-database-managed-instance-transact-sql-information.md) |
| [Sistem kataloğu görünümleri](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/catalog-views-transact-sql) | Bazıları - bkz. tek bir görünüm | Evet - bkz [T-SQL farklılıkları](sql-database-managed-instance-transact-sql-information.md) |
| [Geçici tablolar](https://docs.microsoft.com/sql/t-sql/statements/create-table-transact-sql#database-scoped-global-temporary-tables-azure-sql-database) | Yerel ve veritabanı kapsamlı genel geçici tablolar | Yerel ve örnek kapsamlı genel geçici tablolar |
| [Zamana bağlı tablolar](https://docs.microsoft.com/sql/relational-databases/tables/temporal-tables) | [Evet](sql-database-temporal-tables.md) | [Evet](sql-database-temporal-tables.md) |
|Tehdit algılama|  [Evet](sql-database-threat-detection.md)|[Evet](sql-database-managed-instance-threat-detection.md)|
| [İzleme Bayrakları](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql) | Hayır | Hayır |
| [Değişkenler](https://docs.microsoft.com/sql/t-sql/language-elements/variables-transact-sql) | Evet | Evet |
| [Saydam veri şifrelemesi (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-tde) | Evet - yalnızca katmanları genel amaçlı ve iş açısından kritik hizmet| [Evet](transparent-data-encryption-azure-sql.md) |
[VNet](../virtual-network/virtual-networks-overview.md) | Kısmi - bkz [VNet uç noktaları](sql-database-vnet-service-endpoint-rule-overview.md) | Evet, yalnızca Resource Manager modeli |
| [Windows Server Yük Devretme Kümelemesi](https://docs.microsoft.com/sql/sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server) | [Yüksek kullanılabilirlik](sql-database-high-availability.md) ile her veritabanı bulunur. Olağanüstü durum kurtarma ele alınmıştır [Azure SQL veritabanı ile iş sürekliliğine genel bakış](sql-database-business-continuity.md) | [Yüksek kullanılabilirlik](sql-database-high-availability.md) ile her veritabanı bulunur. Olağanüstü durum kurtarma ele alınmıştır [Azure SQL veritabanı ile iş sürekliliğine genel bakış](sql-database-business-continuity.md) |
| [XML dizinleri](https://docs.microsoft.com/sql/t-sql/statements/create-xml-index-transact-sql) | Evet | Evet |

## <a name="next-steps"></a>Sonraki adımlar

- Azure SQL Veritabanı hizmeti hakkında bilgi edinmek için bkz. [SQL Veritabanı nedir?](sql-database-technical-overview.md)
- Bir yönetilen örneği hakkında daha fazla bilgi için bkz: [yönetilen örnek nedir?](sql-database-managed-instance.md).
