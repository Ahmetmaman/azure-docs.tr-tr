---
title: Bir Azure SQL veri ambarını geri yükleme | Microsoft Docs
description: Bir Azure SQL veri ambarı geri yüklemek için rehberlik etme.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 08/29/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: cf96e153c07211fe176839e713935adcf6b9dbe1
ms.sourcegitcommit: 1fb353cfca800e741678b200f23af6f31bd03e87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2018
ms.locfileid: "43306971"
---
# <a name="restoring-azure-sql-data-warehouse"></a>Azure SQL veri ambarı geri yükleme 
Bu makalede şunları öğreneceksiniz:

- Geri yükleme noktası oluştur
- Bir otomatik geri yükleme noktası veya kullanıcı tanımlı bir geri yükleme noktasından geri yükleme
- Silinen bir veritabanını geri yükleme
- Coğrafi bir yedekten geri yükleyin
- Kullanıcı tanımlı bir geri yükleme noktasından veri Ambarınızı kopyasını oluşturma

## <a name="before-you-begin"></a>Başlamadan önce
**DTU kapasitenizi doğrulayın.** Her SQL veri ambarı varsayılan DTU kotası olan bir SQL server tarafından (örn. myserver.database.windows.net) barındırılır.  SQL veri ambarı geri yüklemeden önce olduğunu doğrulayın. SQL server'ınızı geri yüklenen veritabanı için yeterli kalan DTU kotası vardır. Gerekli DTU'yu hesaplama veya daha fazla DTU istemek için öğrenmek için bkz: [DTU kota değişiklik isteği][Request a DTU quota change].

# <a name="restore-through-powershell"></a>PowerShell aracılığıyla geri yükleme

## <a name="install-powershell"></a>PowerShell yükleme
SQL veri ambarı ile Azure PowerShell'i kullanmak için Azure PowerShell 1.0 veya üzeri bir sürümü yüklemeniz gerekir.  Çalıştırarak sürümünüzü kontrol edebilirsiniz **Get-Module - ListAvailable-Name AzureRM**.  En son sürümü yüklenebilir [Microsoft Web Platformu yükleyicisi][Microsoft Web Platform Installer].  En son sürümü yükleme hakkında daha fazla bilgi için bkz. [Azure PowerShell'i yükleme ve yapılandırma][How to install and configure Azure PowerShell].

## <a name="restore-an-active-or-paused-database"></a>Etkin ya da duraklatılmış bir veritabanını geri yükleme
Geri yükleme noktası kullanılmakta olan bir veritabanını geri yüklemek için [Restore-AzureRmSqlDatabase] [ Restore-AzureRmSqlDatabase] PowerShell cmdlet'i.

1. Windows PowerShell'i açın.

2. Azure hesabınıza bağlanın ve hesabınızla ilişkili tüm abonelikleri listeleyin.

3. Geri yüklenecek veritabanını içeren aboneliği seçin.

4. Veritabanı için geri yükleme noktalarını listeleyin.

5. RestorePointCreationDate kullanarak istenen bir geri yükleme noktası seçin.

   > [!NOTE]
   > Geri yüklerken farklı ServiceObjectiveName (DWU) veya farklı bir bölgede bulunan farklı bir sunucu belirtebilirsiniz.

6. Veritabanını geri yüklemek için istenen geri yükleme noktası.

7. Geri yüklenen veritabanının çevrimiçi olduğunu doğrulayın.

```Powershell

$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.windows.net
$DatabaseName="<YourDatabaseName>"
$NewDatabaseName="<YourDatabaseName>"

Connect-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

# List the last 10 database restore points
((Get-AzureRMSqlDatabaseRestorePoints -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName ($DatabaseName)).RestorePointCreationDate)[-10 .. -1]

# Or list all restore points
Get-AzureRmSqlDatabaseRestorePoints -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Get the specific database to restore
$Database = Get-AzureRmSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Pick desired restore point using RestorePointCreationDate
$PointInTime="<RestorePointCreationDate>"  

# Restore database from a restore point
$RestoredDatabase = Restore-AzureRmSqlDatabase –FromPointInTimeBackup –PointInTime $PointInTime -ResourceGroupName $Database.ResourceGroupName -ServerName $Database.ServerName -TargetDatabaseName $NewDatabaseName –ResourceId $Database.ResourceID

# Verify the status of restored database
$RestoredDatabase.status

```

> [!NOTE]
> Geri yükleme tamamlandıktan sonra takip ederek, kurtarılan veritabanı yapılandırabilirsiniz [kurtarma işleminden sonra veritabanını yapılandırma][Configure your database after recovery].
>

## <a name="copy-your-data-warehouse-with-user-defined-restore-points"></a>Kullanıcı tanımlı bir geri yükleme noktaları sayesinde veri Ambarınızı kopyalayın
Kullanıcı tanımlı bir geri yükleme noktası kullanılmakta olan bir veritabanını geri yüklemek için [Restore-AzureRmSqlDatabase] [ Restore-AzureRmSqlDatabase] PowerShell cmdlet'i.

1. Windows PowerShell'i açın.
2. Azure hesabınıza bağlanın ve hesabınızla ilişkili tüm abonelikleri listeleyin.
3. Geri yüklenecek veritabanını içeren aboneliği seçin.
4. Veritabanınızın yakındaki bir kopyası için bir geri yükleme noktası oluştur
5. Veritabanınızı, geçici bir adla yeniden adlandırın.
5. En son geri yükleme noktası tarafından belirtilen RestorePointLabel alın.
6. Veritabanını geri yüklemeyi başlatmak için kaynak kimliğini alın
6. Veritabanını geri yüklemek için istenen geri yükleme noktası.
7. Geri yüklenen veritabanının çevrimiçi olduğunu doğrulayın.

```Powershell

$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.windows.net
$DatabaseName="<YourDatabaseName>"
$TempDatabaseName = "<YourTemporaryDatabaseName>"
$Label = "<YourRestorePointLabel"

Connect-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

# Create a restore point of the original database
New-AzureRmSqlDatabaseRestorePoint -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName -RestorePointLabel $Label

# Rename the database to a temporary name
Set-AzureRmSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName -NewName $TempDatabaseName

# Get the most recent restore point with the specified label
$LabelledRestorePoint = Get-AzureRmSqlDatabaseRestorePoints -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $TempDatabaseName | where {$_.RestorePointLabel -eq $Label} | sort {$_.RestorePointCreationDate} | select -Last 1

# Get the resource id of the database
$ResourceId = (Get-AzureRmSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $TempDatabaseName).ResourceId

# Restore the database to its original name from the labelled restore point from the temporary database
$RestoredDatabase = Restore-AzureRmSqlDatabase -FromPointInTimeBackup -ResourceGroupName $ResourceGroupName -ServerName $ServerName -ResourceId $ResourceId -PointInTime $LabelledRestorePoint.RestorePointCreationDate -TargetDatabaseName $DatabaseName

# Verify the status of restored database
$RestoredDatabase.status

# The original temporary database can be deleted at this point

```

## <a name="restore-a-deleted-database"></a>Silinen veritabanını geri yükleme
Silinen bir veritabanını geri yüklemek için kullanın [Restore-AzureRmSqlDatabase] [ Restore-AzureRmSqlDatabase] cmdlet'i.

1. Windows PowerShell'i açın.
2. Azure hesabınıza bağlanın ve hesabınızla ilişkili tüm abonelikleri listeleyin.
3. Silinen veritabanını geri içeren aboneliği seçin.
4. Belirli silinen veritabanını alır.
5. Silinen veritabanını geri yükleyin.
6. Geri yüklenen veritabanının çevrimiçi olduğunu doğrulayın.

```Powershell
$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.windows.net
$DatabaseName="<YourDatabaseName>"
$NewDatabaseName="<YourDatabaseName>"

Connect-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

# Get the deleted database to restore
$DeletedDatabase = Get-AzureRmSqlDeletedDatabaseBackup -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Restore deleted database
$RestoredDatabase = Restore-AzureRmSqlDatabase –FromDeletedDatabaseBackup –DeletionDate $DeletedDatabase.DeletionDate -ResourceGroupName $DeletedDatabase.ResourceGroupName -ServerName $DeletedDatabase.ServerName -TargetDatabaseName $NewDatabaseName –ResourceId $DeletedDatabase.ResourceID

# Verify the status of restored database
$RestoredDatabase.status
```

> [!NOTE]
> Geri yükleme tamamlandıktan sonra takip ederek, kurtarılan veritabanı yapılandırabilirsiniz [kurtarma işleminden sonra veritabanını yapılandırma][Configure your database after recovery].
>

## <a name="restore-from-an-azure-geographical-region"></a>Azure bir coğrafi bölgesinden geri yükleme
Bir veritabanını kurtarmak için kullanmak [Restore-AzureRmSqlDatabase] [ Restore-AzureRmSqlDatabase] cmdlet'i.

> [!NOTE]
> Gen2'ye coğrafi geri yükleme bir gerçekleştirebileceğiniz! Bunu yapmak için bir Gen2 ServiceObjectiveName belirtin (örneğin DW1000**c**) isteğe bağlı bir parametre olarak.
>

1. Windows PowerShell'i açın.
2. Azure hesabınıza bağlanın ve hesabınızla ilişkili tüm abonelikleri listeleyin.
3. Geri yüklenecek veritabanını içeren aboneliği seçin.
4. Kurtarmak istediğiniz veritabanını alır.
5. Veritabanı için kurtarma isteği oluşturun.
6. Coğrafi geri veritabanının durumunu doğrulayın.

```Powershell
Connect-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName "<Subscription_name>"

# Get the database you want to recover
$GeoBackup = Get-AzureRmSqlDatabaseGeoBackup -ResourceGroupName "<YourResourceGroupName>" -ServerName "<YourServerName>" -DatabaseName "<YourDatabaseName>"

# Recover database
$GeoRestoredDatabase = Restore-AzureRmSqlDatabase –FromGeoBackup -ResourceGroupName "<YourResourceGroupName>" -ServerName "<YourTargetServer>" -TargetDatabaseName "<NewDatabaseName>" –ResourceId $GeoBackup.ResourceID -ServiceObjectiveName "<YourTargetServiceLevel>"

# Verify that the geo-restored database is online
$GeoRestoredDatabase.status
```

> [!NOTE]
> Geri yükleme tamamlandıktan sonra veritabanını yapılandırmak için bkz: [kurtarma işleminden sonra veritabanını yapılandırma][Configure your database after recovery].
>

Kaynak veritabanı TDE etkinse kurtarılmış veritabanını TDE etkin olacaktır.

# <a name="restore-through-the-azure-portal"></a>Azure Portalı aracılığıyla geri yükleme

## <a name="create-a-user-defined-restore-point"></a>Bir kullanıcı tanımlı bir geri yükleme noktası oluştur
1. [Azure portalında][Azure portal] oturum açın.

2. İçin bir geri yükleme noktası oluşturmak istediğiniz SQL veri ambarı gidin.

3. Genel Bakış dikey pencerenin en üstünde seçin **+ yeni geri yükleme noktası**.

    ![Yeni Geri Yükleme Noktası](./media/sql-data-warehouse-restore-database-portal/creating_restore_point_0.png)
    
4. Geri yükleme noktası için bir ad belirtin.

    ![Geri yükleme noktası adı](./media/sql-data-warehouse-restore-database-portal/creating_restore_point_1.png)

## <a name="restore-an-active-or-paused-database"></a>Etkin ya da duraklatılmış bir veritabanını geri yükleme
1. [Azure portalında][Azure portal] oturum açın.
2. Öğesinden geri yüklemek istediğiniz SQL veri ambarı gidin.
3. Genel Bakış dikey pencerenin en üstünde seçin **geri**.

    ![ Geri Yüklemeye Genel Bakış](./media/sql-data-warehouse-restore-database-portal/restoring_0.png)
    
4. Şunlardan birini seçin **otomatik geri yükleme noktaları** veya **kullanıcı tanımlı bir geri yükleme noktaları**.

    ![Otomatik Geri Yükleme Noktaları](./media/sql-data-warehouse-restore-database-portal/restoring_1.png)
    
5. Kullanıcı tanımlı noktaları, geri yüklemek için **geri yükleme noktası seçin** veya **yeni bir kullanıcı tanımlı bir geri yükleme noktası oluşturma**.

    ![Kullanıcı tanımlı bir geri yükleme noktaları](./media/sql-data-warehouse-restore-database-portal/restoring_2_udrp.png)

## <a name="restore-a-deleted-database"></a>Silinen veritabanını geri yükleme
1. [Azure portalında][Azure portal] oturum açın.
2. SQL server, silinen veritabanını barındırılan gidin.
3. Silinen veritabanları simgesine içindekiler tablosundan seçin.

    ![Silinen veritabanları](./media/sql-data-warehouse-restore-database-portal/restoring_deleted_0.png)
    
4. Geri yüklemek istiyorsanız silinen veritabanını seçin.

    ![Silinen veritabanlarını seçin](./media/sql-data-warehouse-restore-database-portal/restoring_deleted_1.png)
    
5. Yeni bir veritabanı adı belirtin.

    ![Veritabanı adı belirtin](./media/sql-data-warehouse-restore-database-portal/restoring_deleted_2.png)

<!--Image references-->

<!--Article references-->
[Azure SQL Database business continuity overview]: ../sql-database/sql-database-business-continuity.md
[Request a DTU quota change]: ./sql-data-warehouse-get-started-create-support-ticket.md
[Configure your database after recovery]: ../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery
[How to install and configure Azure PowerShell]: /powershell/azureps-cmdlets-docs
[Overview]: ./sql-data-warehouse-restore-database-overview.md
[Portal]: ./sql-data-warehouse-restore-database-portal.md
[PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[REST]: ./sql-data-warehouse-restore-database-rest-api.md
[Configure your database after recovery]: ../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery

<!--MSDN references-->
[Restore-AzureRmSqlDatabase]: https://docs.microsoft.com/powershell/module/azurerm.sql/restore-azurermsqldatabase

<!--Other Web references-->
[Azure Portal]: https://portal.azure.com/
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
