---
title: 'PowerShell: elastik havuzu etkin Coğrafi çoğaltmayı yapılandırma'
description: Azure SQL veritabanı 'nda havuza alınmış bir veritabanı için etkin Coğrafi çoğaltmayı ayarlamak ve yük devretmek için örnek betik Azure PowerShell.
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: sqldbrb=1
ms.devlang: PowerShell
ms.topic: sample
author: mashamsft
ms.author: mathoma
ms.reviewer: sstein
ms.date: 03/12/2019
ms.openlocfilehash: e23ae783bee48af0d0f1cf8bcaea1f8f1e307c54
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91325309"
---
# <a name="use-powershell-to-configure-active-geo-replication-for-a-pooled-database-in-azure-sql-database"></a>Azure SQL veritabanı 'nda havuza alınmış bir veritabanı için etkin Coğrafi çoğaltmayı yapılandırmak için PowerShell 'i kullanma
[!INCLUDE[appliesto-sqldb](../../includes/appliesto-sqldb.md)]

Bu Azure PowerShell betiği örneği, Azure SQL veritabanı 'nda havuza alınmış bir veritabanı için etkin Coğrafi çoğaltmayı yapılandırır ve veritabanının ikincil çoğaltmasına yük devreder.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]
[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

PowerShell 'i yerel olarak yükleyip kullanmayı tercih ederseniz bu öğretici az PowerShell 1.4.0 veya üstünü gerektirir. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzAccount` komutunu da çalıştırmanız gerekir.

## <a name="sample-scripts"></a>Örnek betikler

[!code-powershell-interactive[main](../../../../powershell_scripts/sql-database/setup-geodr-and-failover/setup-geodr-and-failover-elastic-pool.ps1?highlight=17-20 "Set up active geo-replication for elastic pool")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Kaynak grubunu ve onunla ilişkili tüm kaynakları kaldırmak için aşağıdaki komutu kullanın.

```powershell
Remove-AzResourceGroup -ResourceGroupName $primaryresourcegroupname
Remove-AzResourceGroup -ResourceGroupName $secondaryresourcegroupname
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [New-AzSqlServer](/powershell/module/az.sql/new-azsqlserver) | Veritabanları ve elastik havuzlar barındıran bir sunucu oluşturur. |
| [New-Azsqtalaçıkartma havuzu](/powershell/module/az.sql/new-azsqlelasticpool) | Elastik havuz oluşturur. |
| [New-AzSqlDatabase](/powershell/module/az.sql/new-azsqldatabase) | Sunucuda bir veritabanı oluşturur. |
| [Set-AzSqlDatabase](/powershell/module/az.sql/set-azsqldatabase) | Veritabanı özelliklerini güncelleştirir veya bir veritabanını elastik havuzun içine veya dışına ya da elastik havuzlar arasında taşır. |
| [New-AzSqlDatabaseSecondary](/powershell/module/az.sql/new-azsqldatabasesecondary)| Mevcut bir veritabanı için ikincil bir veritabanı oluşturur ve veri çoğaltmaya başlar. |
| [Get-AzSqlDatabase](/powershell/module/az.sql/get-azsqldatabase)| Bir veya daha fazla veritabanını alır. |
| [Set-AzSqlDatabaseSecondary](/powershell/module/az.sql/set-azsqldatabasesecondary)| Yük devretmeyi başlatmak için ikincil bir veritabanını birincil veritabanına geçirir.|
| [Get-AzSqlDatabaseReplicationLink](/powershell/module/az.sql/get-azsqldatabasereplicationlink) | Bir Azure SQL veritabanı ile kaynak grubu veya mantıksal SQL Server arasındaki coğrafi çoğaltma bağlantılarını alır. |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |
|||

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/).

Ek SQL Veritabanı PowerShell betiği örnekleri [Azure SQL Veritabanı PowerShell betikleri](../powershell-script-content-guide.md) içinde bulunabilir.
