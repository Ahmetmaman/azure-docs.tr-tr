---
title: Zaman içinde bir noktaya geri yükleme (ıNR)
titleSuffix: Azure SQL Managed Instance
description: Azure SQL yönetilen örneğindeki bir veritabanını önceki bir zaman noktasına geri yükleyin.
services: sql-database
ms.service: sql-managed-instance
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: how-to
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: sstein, mathoma
ms.date: 08/25/2019
ms.openlocfilehash: 9b4d0fadf157ce1eef6821ccbc32f5725aea611f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91616525"
---
# <a name="restore-a-database-in-azure-sql-managed-instance-to-a-previous-point-in-time"></a>Azure SQL yönetilen örneğindeki bir veritabanını önceki bir zaman noktasına geri yükleme
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

Bir veritabanını geçmişteki bir zamanda başka bir veritabanının kopyası olarak oluşturmak için belirli bir noktaya geri yükleme (ıNR) kullanın. Bu makalede, Azure SQL yönetilen örneği 'nde bir veritabanının bir noktadan noktaya geri yüklemesi nasıl yapılacağı açıklanır.

Zaman içinde geri yükleme, hatalardan kaynaklanan olaylar, yanlış yüklenmiş veriler veya önemli verilerin silinmesi gibi kurtarma senaryolarında yararlıdır. Bunu yalnızca test veya denetim için de kullanabilirsiniz. Yedekleme dosyaları, veritabanı ayarlarınıza bağlı olarak 7-35 gün boyunca tutulur.

Bir zaman noktaya geri yükleme bir veritabanını geri yükleyebilir:

- Mevcut bir veritabanından.
- Silinen bir veritabanından.
- aynı SQL yönetilen örneğine veya başka bir SQL yönetilen örneğine. 

## <a name="limitations"></a>Sınırlamalar

SQL yönetilen örneği için bir noktadan noktaya geri yükleme aşağıdaki sınırlamalara sahiptir:

- SQL yönetilen örneği 'nin bir örneğinden diğerine geri yüklerken, her iki örnek de aynı abonelikte ve bölgede olmalıdır. Bölgeler arası ve çapraz abonelik geri yüklemesi şu anda desteklenmiyor.
- Tüm SQL yönetilen örneğinin zaman içinde geri yüklenmesi mümkün değildir. Bu makalede yalnızca olası özellikler açıklanmaktadır: SQL yönetilen örneği üzerinde barındırılan bir veritabanının zaman içindeki nokta geri yüklemesi.

> [!WARNING]
> SQL yönetilen örneğinizin depolama boyutunu unutmayın. Geri yüklenecek verilerin boyutuna bağlı olarak, örnek depolamanın dışında kalabilirsiniz. Geri yüklenen veriler için yeterli alan yoksa farklı bir yaklaşım kullanın.

Aşağıdaki tabloda, SQL yönetilen örneği için zaman içinde geri yükleme senaryoları gösterilmektedir:

|           |Mevcut VERITABANıNı SQL yönetilen örneği 'nin aynı örneğine geri yükleme| Var olan VERITABANıNı başka bir SQL yönetilen örneğine geri yükleme|Bırakılan DB 'yi aynı SQL yönetilen örneğine geri yükleme|Bırakılan VERITABANıNı başka bir SQL yönetilen örneğine geri yükleme|
|:----------|:----------|:----------|:----------|:----------|
|**Azure portalındaki**| Evet|Hayır |Evet|Hayır|
|**Azure CLI**|Evet |Evet |Hayır|Hayır|
|**PowerShell**| Evet|Evet |Evet|Evet|

## <a name="restore-an-existing-database"></a>Var olan bir veritabanını geri yükleme

Azure portal, PowerShell veya Azure CLı kullanarak mevcut bir veritabanını aynı SQL yönetilen örneğine geri yükleyin. Bir veritabanını başka bir SQL yönetilen örneğine geri yüklemek için, hedef SQL yönetilen örneği ve kaynak grubunun özelliklerini belirleyebilmeniz için PowerShell veya Azure CLı kullanın. Bu parametreleri belirtmezseniz, veritabanı varsayılan olarak mevcut SQL yönetilen örneğine geri yüklenir. Azure portal Şu anda başka bir SQL yönetilen örneğine geri yüklemeyi desteklemiyor.

# <a name="portal"></a>[Portal](#tab/azure-portal)

1. [Azure portalında](https://portal.azure.com) oturum açın. 
2. SQL yönetilen örneğinize gidin ve geri yüklemek istediğiniz veritabanını seçin.
3. Veritabanı sayfasında **geri yükle** ' yi seçin:

    ![Azure portal kullanarak bir veritabanını geri yükleme](./media/point-in-time-restore/restore-database-to-mi.png)

4. **Geri yükle** sayfasında, veritabanını geri yüklemek istediğiniz tarih ve saat için noktayı seçin.
5. Veritabanınızı geri yüklemek için **Onayla** ' yı seçin. Bu eylem, yeni bir veritabanı oluşturan ve belirli bir zaman noktasındaki özgün veritabanındaki verilerle dolduran geri yükleme işlemini başlatır. Kurtarma işlemi hakkında daha fazla bilgi için bkz. [Kurtarma süresi](../database/recovery-using-backups.md#recovery-time).

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Zaten yüklü Azure PowerShell yoksa, bkz. [Azure PowerShell modülünü yükleme](https://docs.microsoft.com/powershell/azure/install-az-ps).

Veritabanını PowerShell kullanarak geri yüklemek için aşağıdaki komutta parametre değerlerini belirtin. Ardından şu komutu çalıştırın:

```powershell-interactive
$subscriptionId = "<Subscription ID>"
$resourceGroupName = "<Resource group name>"
$managedInstanceName = "<SQL Managed Instance name>"
$databaseName = "<Source-database>"
$pointInTime = "2018-06-27T08:51:39.3882806Z"
$targetDatabase = "<Name of new database to be created>"

Get-AzSubscription -SubscriptionId $subscriptionId
Select-AzSubscription -SubscriptionId $subscriptionId

Restore-AzSqlInstanceDatabase -FromPointInTimeBackup `
                              -ResourceGroupName $resourceGroupName `
                              -InstanceName $managedInstanceName `
                              -Name $databaseName `
                              -PointInTime $pointInTime `
                              -TargetInstanceDatabaseName $targetDatabase `
```

Veritabanını başka bir SQL yönetilen örneğine geri yüklemek için, hedef kaynak grubu ve hedef SQL yönetilen örneği adlarını da belirtin:  

```powershell-interactive
$targetResourceGroupName = "<Resource group of target SQL Managed Instance>"
$targetInstanceName = "<Target SQL Managed Instance name>"

Restore-AzSqlInstanceDatabase -FromPointInTimeBackup `
                              -ResourceGroupName $resourceGroupName `
                              -InstanceName $managedInstanceName `
                              -Name $databaseName `
                              -PointInTime $pointInTime `
                              -TargetInstanceDatabaseName $targetDatabase `
                              -TargetResourceGroupName $targetResourceGroupName `
                              -TargetInstanceName $targetInstanceName 
```

Ayrıntılar için bkz. [restore-Azsqlınstancedatabase](https://docs.microsoft.com/powershell/module/az.sql/restore-azsqlinstancedatabase).

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Azure CLı yüklü değilse, bkz. [Azure CLI 'Yı yükleme](/cli/azure/install-azure-cli?view=azure-cli-latest).

Azure CLı kullanarak veritabanını geri yüklemek için aşağıdaki komutta parametre değerlerini belirtin. Ardından şu komutu çalıştırın:

```azurecli-interactive
az sql midb restore -g mygroupname --mi myinstancename |
-n mymanageddbname --dest-name targetmidbname --time "2018-05-20T05:34:22"
```

Veritabanını başka bir SQL yönetilen örneğine geri yüklemek için, hedef kaynak grubu ve SQL yönetilen örneği adlarını da belirtin:  

```azurecli-interactive
az sql midb restore -g mygroupname --mi myinstancename -n mymanageddbname |
       --dest-name targetmidbname --time "2018-05-20T05:34:22" |
       --dest-resource-group mytargetinstancegroupname |
       --dest-mi mytargetinstancename
```

Kullanılabilir parametrelerin ayrıntılı bir açıklaması için bkz. [SQL yönetilen örneği 'nde bir veritabanını geri yüklemek Için CLI belgeleri](https://docs.microsoft.com/cli/azure/sql/midb?view=azure-cli-latest#az-sql-midb-restore).

---

## <a name="restore-a-deleted-database"></a>Silinen veritabanını geri yükleme

Silinen bir veritabanının geri yüklenmesi, PowerShell veya Azure portal kullanılarak yapılabilir. Silinen bir veritabanını aynı örneğe geri yüklemek için Azure portal ya da PowerShell kullanın. Silinen bir veritabanını başka bir örneğe geri yüklemek için PowerShell 'i kullanın. 

### <a name="portal"></a>Portal 


Azure portal kullanarak yönetilen bir veritabanını kurtarmak için SQL yönetilen örnek genel bakış sayfasını açın ve **silinen veritabanları**' nı seçin. Geri yüklemek istediğiniz silinmiş bir veritabanını seçin ve yedeklemeden geri yüklenen verilerle oluşturulacak yeni veritabanının adını yazın.

  ![Silinen Azure SQL örneği veritabanını geri yükleme ekran görüntüsü](./media/point-in-time-restore/restore-deleted-sql-managed-instance-annotated.png)

.. /.. /SQL-Database

### <a name="powershell"></a>PowerShell

Bir veritabanını aynı örneğe geri yüklemek için, parametre değerlerini güncelleştirin ve ardından aşağıdaki PowerShell komutunu çalıştırın: 

```powershell-interactive
$subscriptionId = "<Subscription ID>"
Get-AzSubscription -SubscriptionId $subscriptionId
Select-AzSubscription -SubscriptionId $subscriptionId

$resourceGroupName = "<Resource group name>"
$managedInstanceName = "<SQL Managed Instance name>"
$deletedDatabaseName = "<Source database name>"
$targetDatabaseName = "<target database name>"

$deletedDatabase = Get-AzSqlDeletedInstanceDatabaseBackup -ResourceGroupName $resourceGroupName `
-InstanceName $managedInstanceName -DatabaseName $deletedDatabaseName

Restore-AzSqlinstanceDatabase -Name $deletedDatabase.Name `
   -InstanceName $deletedDatabase.ManagedInstanceName `
   -ResourceGroupName $deletedDatabase.ResourceGroupName `
   -DeletionDate $deletedDatabase.DeletionDate `
   -PointInTime UTCDateTime `
   -TargetInstanceDatabaseName $targetDatabaseName
```

Veritabanını başka bir SQL yönetilen örneğine geri yüklemek için, hedef kaynak grubu ve hedef SQL yönetilen örneği adlarını da belirtin:

```powershell-interactive
$targetResourceGroupName = "<Resource group of target SQL Managed Instance>"
$targetInstanceName = "<Target SQL Managed Instance name>"

Restore-AzSqlinstanceDatabase -Name $deletedDatabase.Name `
   -InstanceName $deletedDatabase.ManagedInstanceName `
   -ResourceGroupName $deletedDatabase.ResourceGroupName `
   -DeletionDate $deletedDatabase.DeletionDate `
   -PointInTime UTCDateTime `
   -TargetInstanceDatabaseName $targetDatabaseName `
   -TargetResourceGroupName $targetResourceGroupName `
   -TargetInstanceName $targetInstanceName 
```

## <a name="overwrite-an-existing-database"></a>Var olan veritabanının üzerine yaz

Var olan bir veritabanının üzerine yazmak için şunları yapmanız gerekir:

1. Üzerine yazmak istediğiniz var olan veritabanını bırakın.
2. Zaman içinde geri yüklenen veritabanını, bıraktığınız veritabanının adıyla yeniden adlandırın.

### <a name="drop-the-original-database"></a>Özgün veritabanını bırak

Azure portal, PowerShell veya Azure CLı kullanarak veritabanını bırakabilirsiniz.

Ayrıca, SQL yönetilen örneğine doğrudan bağlanarak SQL Server Management Studio (SSMS) ve ardından aşağıdaki Transact-SQL (T-SQL) komutunu çalıştırarak da veritabanını bırakabilirsiniz:

```sql
DROP DATABASE WorldWideImporters;
```

SQL yönetilen örneğindeki veritabanınıza bağlanmak için aşağıdaki yöntemlerden birini kullanın:

- [Bir Azure sanal makinesi aracılığıyla SSMS/Azure Data Studio](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-configure-vm)
- [Noktadan siteye](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-configure-p2s)
- [Genel uç nokta](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-public-endpoint-configure)

# <a name="portal"></a>[Portal](#tab/azure-portal)

Azure portal, SQL yönetilen örneğinden veritabanını seçin ve **Sil**' i seçin.

   ![Azure portal kullanarak bir veritabanını silme](./media/point-in-time-restore/delete-database-from-mi.png)

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

SQL yönetilen örneğinden var olan bir veritabanını bırakmak için aşağıdaki PowerShell komutunu kullanın:

```powershell
$resourceGroupName = "<Resource group name>"
$managedInstanceName = "<SQL Managed Instance name>"
$databaseName = "<Source database>"

Remove-AzSqlInstanceDatabase -Name $databaseName -InstanceName $managedInstanceName -ResourceGroupName $resourceGroupName
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Var olan bir veritabanını bir SQL yönetilen örneğinden bırakmak için aşağıdaki Azure CLı komutunu kullanın:

```azurecli-interactive
az sql midb delete -g mygroupname --mi myinstancename -n mymanageddbname
```

---

### <a name="alter-the-new-database-name-to-match-the-original-database-name"></a>Yeni veritabanı adını özgün veritabanı adıyla eşleşecek şekilde değiştirin

Doğrudan SQL yönetilen örneğine bağlanın ve SQL Server Management Studio başlatın. Ardından, aşağıdaki Transact-SQL (T-SQL) sorgusunu çalıştırın. Sorgu, geri yüklenen veritabanının adını üzerine yazmak istediğiniz bırakılmış veritabanının adını değiştirecek.

```sql
ALTER DATABASE WorldWideImportersPITR MODIFY NAME = WorldWideImporters;
```

SQL yönetilen örneği 'nde veritabanınıza bağlanmak için aşağıdaki yöntemlerden birini kullanın:

- [Azure sanal makinesi](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-configure-vm)
- [Noktadan siteye](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-configure-p2s)
- [Genel uç nokta](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-public-endpoint-configure)

## <a name="next-steps"></a>Sonraki adımlar

[Otomatikleştirilmiş yedeklemeler](../database/automated-backups-overview.md)hakkında bilgi edinin.
