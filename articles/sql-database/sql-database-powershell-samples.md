---
title: SQL Veritabanı için Azure PowerShell betik örnekleri | Microsoft Docs
description: Azure SQL Veritabanı sunucuları, elastik havuzlar, veritabanları ve güvenlik duvarları oluşturmanıza ve yönetmenize yardımcı olacak Azure PowerShell betik örnekleri.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: PowerShell
ms.topic: sample
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 03/12/2019
ms.openlocfilehash: 3a22caa39dad3ce296c43015f324270b7dc55ae8
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2019
ms.locfileid: "57862038"
---
# <a name="azure-powershell-samples-for-azure-sql-database"></a>Azure SQL Veritabanı için Azure PowerShell örnekleri

Azure SQL veritabanı, veritabanı, örnekleri ve Azure PowerShell kullanarak havuzları yapılandırmanızı sağlar.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz Bu öğretici AZ PowerShell 1.4.0 gerektirir veya üzeri. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzAccount` komutunu da çalıştırmanız gerekir.

## <a name="single-database-and-elastic-pools"></a>Tek veritabanı ve elastik havuzlar

Aşağıdaki tablo, Azure SQL Veritabanı’na yönelik örnek Azure PowerShell betiklerinin bağlantılarını içerir.

| |  |
|---|---|
|**Tek veritabanları ve elastik havuzlar oluşturma ve yapılandırma**||
| [Tek veritabanı oluşturma ve bir veritabanı sunucusu güvenlik duvarı kuralı yapılandırma](scripts/sql-database-create-and-configure-database-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bu PowerShell betiği, tek bir Azure SQL veritabanı oluşturur ve sunucu düzeyinde güvenlik duvarı kuralı yapılandırır. |
| [Elastik havuzlar oluşturma ve havuza alınmış veritabanlarını taşıma](scripts/sql-database-move-database-between-pools-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bu PowerShell betiği, Azure SQL Veritabanı elastik havuzları oluşturur, havuza alınmış veritabanlarını taşır ve işlem boyutlarını değiştirir.|
|**Coğrafi çoğaltmayı ve yük devretmeyi yapılandırma**||
| [Etkin coğrafi çoğaltmayı kullanarak tek bir veritabanını yapılandırma ve tek bir veritabanının yükünü devretme](scripts/sql-database-setup-geodr-and-failover-database-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Bu PowerShell betiği, tek bir Azure SQL veritabanı için etkin coğrafi çoğaltmayı yapılandırır ve ikincil bir çoğaltmaya yük devreder. |
| [Etkin coğrafi çoğaltmayı kullanarak havuza alınmış veritabanını yapılandırma ve havuza alınmış veritabanının yükünü devretme](scripts/sql-database-setup-geodr-and-failover-pool-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Bu PowerShell betiği, SQL elastik havuzundaki bir Azure SQL veritabanı için etkin coğrafi çoğaltmayı yapılandırır ve ikincil bir çoğaltmaya yük devreder. |
| [Tek bir veritabanı için bir yük devretme grubunu yapılandırma ve yük devretme grubunun yükünü devretme](scripts/sql-database-setup-geodr-failover-database-failover-group-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bu PowerShell betiği, Azure SQL Veritabanı sunucu örneği için bir yük devretme grubu yapılandırır, yük devretme grubuna bir veritabanı ekler ve ikincil sunucuya yük devreder |
|**Tek bir veritabanını ve elastik havuzu ölçekleme**||
| [Tek bir veritabanını ölçekleme](scripts/sql-database-monitor-and-scale-database-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bu PowerShell betiği, Azure SQL veritabanının performans ölçümlerini izler, veritabanını daha yüksek bir işlem boyutuna ölçekler ve performans ölçümlerinden birinde bir uyarı kuralı oluşturur. |
| [Elastik havuzu ölçekleme](scripts/sql-database-monitor-and-scale-pool-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bu PowerShell betiği, Azure SQL Veritabanı elastik havuzu performans ölçümlerini izler, veritabanını daha yüksek bir işlem boyutuna ölçekler ve performans ölçümlerinden birinde bir uyarı kuralı oluşturur.  |
| **Denetim ve tehdit algılama** |
| [Denetim ve tehdit algılamayı yapılandırma](scripts/sql-database-auditing-and-threat-detection-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Bu PowerShell betiği, Azure SQL veritabanı için denetim ve tehdit algılama ilkelerini yapılandırır. |
| **Veritabanını geri yükleme, kopyalama ve içeri aktarma**||
| [Veritabanını geri yükleme](scripts/sql-database-restore-database-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Bu PowerShell betiği, coğrafi olarak yedekli bir yedeklemeden Azure SQL veritabanını geri yükler ve silinmiş bir Azure SQL veritabanını en son yedeklemeye geri yükler. |
| [Bir veritabanını yeni bir sunucuya kopyalama](scripts/sql-database-copy-database-to-new-server-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Bu PowerShell betiği, yeni Azure SQL sunucusunda mevcut bir Azure SQL veritabanının kopyasını oluşturur. |
| [Bacpac dosyasından veritabanını içeri aktarma](scripts/sql-database-import-from-bacpac-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Bu PowerShell betiği bir veritabanını bacpac dosyasından Azure SQL sunucusuna içeri aktarır. |
| **Veritabanları arasında verileri eşitleme**||
| [SQL veritabanları arasında verileri eşitleme](scripts/sql-database-sync-data-between-sql-databases.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bu PowerShell betiği, Veri Eşitleme’yi birden çok Azure SQL veritabanı arasında eşitlenecek şekilde yapılandırır. |
| [SQL Veritabanı ile şirket içi SQL Server arasında verileri eşitleme](scripts/sql-database-sync-data-between-azure-onprem.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bu PowerShell betiği, bir Azure SQL veritabanı ile SQL Server şirket içi veritabanı arasında eşitleme yapmak için Veri Eşitleme’yi yapılandırır. |
| [SQL Data Sync için eşitleme şemasını güncelleştirme](scripts/sql-database-sync-update-schema.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bu PowerShell betiği, Veri Eşitleme için eşitleme şemasından öğeler ekler veya öğeleri kaldırır. |
|||

Daha fazla bilgi edinin [tek veritabanı Azure PowerShell API'si](sql-database-single-databases-manage.md#powershell-manage-sql-database-servers-and-single-databases).

## <a name="managed-instance"></a>Yönetilen Örnek

Aşağıdaki tabloda, Azure SQL veritabanı - yönetilen örnek için örnek Azure PowerShell betiklerinin bağlantılarını içerir.

| |  |
|---|---|
|**Oluşturma ve yönetilen örnekleri yapılandırma**||
| [Yönetilen Örnek oluşturma ve yönetme](https://blogs.msdn.microsoft.com/sqlserverstorageengine/20../../quick-start-script-create-azure-sql-managed-instance-using-powershell/) | Bu PowerShell betiği, Azure PowerShell kullanarak Yönetilen Örnek oluşturmayı ve yönetmeyi göstermektedir |
| [Oluşturma ve Azure Resource Manager şablonu kullanarak bir yönetilen örnek yönetme](scripts/sql-managed-instance-create-powershell-azure-resource-manager-template.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bu PowerShell Betiği oluşturma ve yönetme Azure PowerShell ve Azure Resource Manager şablonu kullanarak bir yönetilen örnek gösterilmektedir.|
| **Saydam veri şifrelemesi (TDE) yapılandırma**||
| [Saydam veri şifrelemesi kullanarak kendi anahtarınızı Azure anahtar Kasası'ndaki bir yönetilen örneğinde yönetme](scripts/transparent-data-encryption-byok-sql-managed-instance-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Bu PowerShell Betiği kullanarak Azure SQL yönetilen Azure Key vault'tan bir anahtar örneği, kendi anahtarını Getir senaryosunda saydam veri şifrelemesi (TDE) yapılandırır|
|||

Daha fazla bilgi edinin [yönetilen örnek Azure PowerShell API'si](sql-database-managed-instance-create-manage.md#powershell-create-and-manage-managed-instances).
