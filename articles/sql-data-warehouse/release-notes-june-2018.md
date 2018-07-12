---
title: Haziran 2018 tarihinden itibaren Azure SQL veri ambarı sürüm notları | Microsoft Docs
description: Azure SQL veri ambarı için sürüm notları.
services: sql-data-warehouse
author: twounder
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 06/25/2018
ms.author: twounder
ms.reviewer: twounder
ms.openlocfilehash: 4b097568abdad5e84605856d0246b0407d881758
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38723976"
---
# <a name="whats-new-in-azure-sql-data-warehouse-june-2018"></a>Azure SQL veri ambarı'nda yenilikler nelerdir? Haziran 2018
Azure SQL veri ambarı, sürekli olarak iyileştirmeler alır. Bu makalede, Haziran 2018'de sunulan değişiklikler ve yeni özellikleri açıklar. 

## <a name="user-defined-restore-points"></a>Kullanıcı tanımlı geri yükleme noktaları
SQL veri ambarı, veri ambarınızın anlık görüntüleri otomatik olarak her 8 saatte bir sekiz saatlik kurtarma noktası hedefi (RPO) güvence altına almak alır. Bu anlık görüntüler bir kolayca veri Ambarınızı çalışan yönetim yükünü otomatik karşın, iş gereksinimlerinize göre kritik zamanlarda anlık gerek yoktur. Örneğin, önemli veri yüklemesi ya da dağıtımı yeni komut dosyaları hemen önce bir anlık görüntü işlemi hemen önce geri yükleme noktası etkinleştirmek için veri ambarı'na yönlendiriliyorsunuz. 

SQL veri ambarı destekler [kullanıcı tanımlı bir geri yükleme noktaları](https://azure.microsoft.com/blog/quick-recovery-time-with-sql-data-warehouse-using-user-defined-restore-points/) aracılığıyla [yeni AzureRmSqlDatabaseRestorePoint](https://docs.microsoft.com/powershell/module/azurerm.sql/new-azurermsqldatabaserestorepoin) cmdlet'i.

```PowerShell
New-AzureRmSqlDatabaseRestorePoint
    -ResourceGroupName $ResourceGroupName
    -ServerName $ServerName
    -DatabaseName $DatabaseName
    -RestorePointLabel $RestorePointName
```

## <a name="column-level-security"></a>Sütun düzeyi güvenlik
Veri erişimi ve güvenlik, veri ambarı'nda yönetme, müşteriler ve iş ortakları ile güven oluşturmak için önemlidir. SQL veri ambarı [sütun düzeyi güvenlik (CLS) artık destekliyor](https://azure.microsoft.com/blog/column-level-security-is-now-supported-in-azure-sql-data-warehouse/) veri Ambarınızı tasarlamanız gerekmeden tablolarınızı belirli sütunlardaki kullanıcı erişimini kısıtlayarak hassas verileri görüntülemeye yönelik izinleri ayarlamanızı sağlar.

CLS standardını kullanarak kullanıcının yürütme bağlamı veya grup üyeliklerini temel tablo sütunlarına erişimini denetlemesine olanak tanır [GRANT](https://docs.microsoft.com/azure/sql-data-warehouse/column-level-security) T-SQL deyimi. Erişimi kısıtlama mantıksal bulunduğu veritabanı katmanındaki yerine başka bir uygulamadaki verileri uzağa genel güvenlik uygulaması basitleştirir.


```sql
CREATE USER Nurse WITHOUT LOGIN;   
GRANT SELECT ON Membership(MemberID, FirstName, LastName, Phone, Email) TO Nurse;   
EXECUTE AS USER = 'Nurse';
SELECT * FROM Membership;

Msg 230, Level 14, State 1, Line 12 
The SELECT permission was denied on the column 'SSN' of the object 'Membership', database 'CLS_TestDW', schema 'dbo'.
```

## <a name="objectschemaname"></a>OBJECT_SCHEMA_NAME
[OBJECT_SCHEMA_NAME()]() işlevi şema kapsamlı nesneler için veritabanı şema adını döndürür. Bu işlev ETL araçlarındaki yaygın haline gelmiştir zaman şema doğrulama nesnesi. 

```sql
SELECT
    OBJECT_SCHEMA_NAME([object_id]) [table_schema]
    , [name]                        [table_name]
FROM
    [sys].[tables];
```

**Örnek sonuçları**
```
table_schema    | table_name
-----------------------------
dbo               customer
dbo               lineitem
dbo               nation
dbo               orders
```

## <a name="support-for-the-systimezoneinfo-view"></a>Sys.time_zone_info görünümü desteği
[Sys.time_zone_info](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-time-zone-info-transact-sql) görünümü, Azure SQL Data warehouse'da desteklenen saat dilimleri hakkında bilgi döndürür.

```sql
SELECT * FROM [sys].[time_zone_info];
```

**Örnek sonuçları**
```
name                            | current_utc_offset | is_currently_dst
-------------------------------------------------------------------------
Pacific Standard Time (Mexico)    -07:00               1
US Mountain Standard Time         -07:00               0
Mountain Standard Time (Mexico)   -06:00               1
Central Standard Time             -05:00               1
```

## <a name="auto-stats-operations-appear-in-sysdmpdwexecrequests-behavior-change"></a>Otomatik İstatistikler operations sys.dm_pdw_exec_requests (davranış değişikliği) görünür.

Sunulmasıyla birlikte [otomatik Create STATISTICS](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-statistics#automatic-creation-of-statistics), Azure SQL veri ambarı sorgu yürütme en iyi duruma getirme istatistikleri üretir. İstatistikleri otomatik olarak oluşturulan bir kayıtta ekleyerek, izleme olanağı Haziran 2018'den sürüm ekler [sys.dm_pdw_exec_requests](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql) herhangi her görüntülemek [CREATE STATISTICS](https://docs.microsoft.com/sql/t-sql/statements/create-statistics-transact-sql) işlemi yürütülür.

```sql
SELECT
    [start_time]
    , [end_time]
    , [command]
FROM
    [sys].[dm_pdw_exec_requests]
WHERE
    [command] LIKE 'CREATE STATISTICS _WA_Sys%';
```
**Örnek sonuçları**
```
start_time                | end_time                | command
------------------------------------------------------------------------------------------------------------------------------
2018-06-06 19:06:26.173    2018-06-06 19:06:26.173    CREATE STATISTICS _WA_Sys_00000001_63D998CC ON dbo.LineItem(l_orderkey);
```