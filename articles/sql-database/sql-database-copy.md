---
title: Bir Azure SQL veritabanını kopyalama | Microsoft Docs
description: Aynı sunucuda veya farklı bir sunucu üzerinde mevcut bir Azure SQL veritabanını işlemsel olarak tutarlı bir kopyasını oluşturun.
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 09/14/2018
ms.openlocfilehash: 2ce86bee96f22b4079afee3eacbeee7ea15a6ffa
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2018
ms.locfileid: "47226016"
---
# <a name="copy-an-azure-sql-database"></a>Bir Azure SQL veritabanını kopyalama

Azure SQL veritabanı, aynı sunucuda veya farklı bir sunucu üzerinde mevcut bir Azure SQL veritabanını işlemsel olarak tutarlı bir kopyasını oluşturmak için çeşitli yöntemler sunar. Azure portalı, PowerShell veya T-SQL kullanarak SQL veritabanına kopyalayabilirsiniz. 

## <a name="overview"></a>Genel Bakış

Veritabanı kopyasını kopya isteğini tarihindeki kaynak veritabanı anlık görüntüsüdür. Aynı sunucuda veya farklı bir sunucu, hizmet katmanını ve işlem boyutu veya bir başka işlem boyutu aynı hizmet katmanında (sürüm) seçebilirsiniz. Kopyalama tamamlandıktan sonra tam olarak işlevsel, bağımsız bir veritabanı haline gelir. Bu noktada, yükseltebilir veya tüm sürüm'e düşürebilir. Oturumlar, kullanıcılar ve izinler bağımsız olarak yönetilebilir.  

## <a name="logins-in-the-database-copy"></a>Veritabanı kopyasını oturum açma

Aynı mantıksal sunucu için veritabanı kopyalama, aynı giriş bilgilerini hem veritabanlarında kullanılabilir. Güvenlik sorumlusu veritabanını kopyalamak için kullandığınız yeni bir veritabanı üzerinde veritabanı sahibi olur. Tüm veritabanı kullanıcıları ve izinlerini kendi güvenlik tanımlayıcılarını (SID'ler) veritabanı kopyasını kopyalanır.  

Bir veritabanı için farklı bir mantıksal sunucu kopyaladığınızda, güvenlik sorumlusu yeni sunucuda yeni bir veritabanı üzerinde veritabanı sahibi olur. Kullanırsanız [kapsanan veritabanı kullanıcıları](sql-database-manage-logins.md) birincil ve ikincil veritabanları her zaman aynı kullanıcı kimlik bilgilerine sahip veri erişimi için kopyalama tamamlandıktan sonra hemen ile aynı kimlik bilgilerini erişebildiğinizden emin olun . 

Kullanırsanız [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md), kopyalama kimlik bilgilerini yönetme gereksinimini tamamen ortadan kaldırabilir. Veritabanını yeni bir sunucuya kopyalama, oturum açma bilgileri yeni sunucuda mevcut değil çünkü ancak oturum açma tabanlı erişim, çalışmayabilir. Farklı bir mantıksal sunucu için veritabanı kopyalama sırasında oturumları yönetme hakkında bilgi edinmek için [olağanüstü durum kurtarma işleminden sonra Azure SQL veritabanı güvenliği yönetmek nasıl](sql-database-geo-replication-security-config.md). 

Kopyalama başarılı olduktan sonra ve diğer kullanıcıların eşleştirilir önce veritabanı sahibi, kopyalama, başlatılan oturum açma için yeni veritabanı oturum açabilir. Kopyalama işlemi tamamlandıktan sonra oturum açma bilgileri çözümlemek için bkz: [çözmek oturumları](#resolve-logins).

## <a name="copy-a-database-by-using-the-azure-portal"></a>Bir veritabanını Azure portalını kullanarak kopyalama

Azure portalını kullanarak bir veritabanını kopyalamak için veritabanınızın sayfasını açın ve ardından **kopyalama**. 

   ![Veritabanı kopyalama](./media/sql-database-copy/database-copy.png)

## <a name="copy-a-database-by-using-powershell"></a>Bir veritabanını PowerShell kullanarak kopyalama

PowerShell kullanarak bir veritabanını kopyalamak için kullanmak [New-AzureRmSqlDatabaseCopy](/powershell/module/azurerm.sql/new-azurermsqldatabasecopy) cmdlet'i. 

```PowerShell
New-AzureRmSqlDatabaseCopy -ResourceGroupName "myResourceGroup" `
    -ServerName $sourceserver `
    -DatabaseName "MySampleDatabase" `
    -CopyResourceGroupName "myResourceGroup" `
    -CopyServerName $targetserver `
    -CopyDatabaseName "CopyOfMySampleDatabase"
```

Tam örnek betik için bkz: [bir veritabanını yeni bir sunucuya kopyalama](scripts/sql-database-copy-database-to-new-server-powershell.md).

## <a name="copy-a-database-by-using-transact-sql"></a>Transact-SQL kullanarak bir veritabanını kopyalama

Ana veritabanı sunucu düzeyi asıl oturum açma veya kopyalamak istediğiniz veritabanı oluşturulan oturum açma için oturum açın. Başarılı olması için veritabanı kopyalama için sunucu düzeyinde sorumlu olmayan oturum açma bilgileri. dbmanager rolünün üyeleri olmalıdır. Oturum açma bilgileri ve sunucuya bağlanma hakkında daha fazla bilgi için bkz. [oturum açma bilgilerini yönetme](sql-database-manage-logins.md).

Kaynak veritabanıyla kopyalamaya Başla [CREATE DATABASE](https://msdn.microsoft.com/library/ms176061.aspx) deyimi. Bu deyimi yürütüp, veritabanı kopyalama işlemi başlatır. Veritabanı kopyalama zaman uyumsuz bir işlem olduğundan, veritabanı kopyalama tamamlanmadan önce CREATE DATABASE deyimi döndürür.

### <a name="copy-a-sql-database-to-the-same-server"></a>Bir SQL veritabanını aynı sunucuya kopyalama
Ana veritabanı sunucu düzeyi asıl oturum açma veya kopyalamak istediğiniz veritabanı oluşturulan oturum açma için oturum açın. Başarılı olması için veritabanı kopyalama için sunucu düzeyinde sorumlu olmayan oturum açma bilgileri. dbmanager rolünün üyeleri olmalıdır.

Bu komut, Veritabanı1 Veritabanı2 aynı sunucu üzerinde adlı yeni bir veritabanına kopyalar. Veritabanınızın boyutuna bağlı olarak, kopyalama işleminin tamamlanması biraz zaman alabilir.

    -- Execute on the master database.
    -- Start copying.
    CREATE DATABASE Database2 AS COPY OF Database1;

### <a name="copy-a-sql-database-to-a-different-server"></a>Bir SQL veritabanını farklı bir sunucuya kopyalama

Oluşturulacak yeni veritabanının bulunduğu SQL veritabanı sunucusu hedef sunucunun ana veritabanına oturum açın. Kaynak veritabanı kaynak SQL veritabanı sunucu üzerindeki veritabanı sahibi olarak aynı adı ve parolası olan bir oturum açma bilgilerini kullanacak. Hedef sunucuda oturum açma ayrıca dbmanager rolünün bir üyesi olmanız veya sunucu düzeyinde asıl oturum açma olması gerekir.

Bu komut, Veritabanı1 Veritabanı2 Sunucu2'adlı yeni bir veritabanı Sunucu1 kopyalar. Veritabanınızın boyutuna bağlı olarak, kopyalama işleminin tamamlanması biraz zaman alabilir.

    -- Execute on the master database of the target server (server2)
    -- Start copying from Server1 to Server2
    CREATE DATABASE Database2 AS COPY OF server1.Database1;


### <a name="monitor-the-progress-of-the-copying-operation"></a>Kopyalama işlemi ilerlemesini izleme

Kopyalama işlemi sys.databases ve sys.dm_database_copies görünümleri sorgulayarak izleyin. Kopyalama devam ederken **state_desc** sys.databases görünümünün yeni veritabanı için sütunu ayarlayın **kopyalama**.

* Kopyalama başarısız olursa **state_desc** sys.databases görünümünün yeni veritabanı için sütunu ayarlayın **ŞÜPHELENİYORSANIZ**. Yeni veritabanı üzerinde bırakma deyimi yürütün ve daha sonra tekrar deneyin.
* Kopyalama başarılı olursa **state_desc** sys.databases görünümünün yeni veritabanı için sütunu ayarlayın **çevrimiçi**. Kopyalama tamamlandıktan ve yeni veritabanı kaynak veritabanının bağımsız olarak değiştirilebilir normal bir veritabanıdır.

> [!NOTE]
> İptal işlemi devam ederken kopyalama karar verirseniz, yürütme [DROP DATABASE](https://msdn.microsoft.com/library/ms178613.aspx) yeni veritabanı deyimi. Alternatif olarak, DROP DATABASE deyimi kaynak veritabanı üzerinde çalıştırma da kopyalama işlemi iptal eder.
> 

## <a name="resolve-logins"></a>Oturum açma bilgileri çözün

Yeni veritabanı hedef sunucuda çevrimiçi olduktan sonra kullanmak [ALTER USER](https://msdn.microsoft.com/library/ms176060.aspx) kullanıcıların yeni veritabanı oturum açma bilgileri hedef sunucuda yeniden eşlemek için deyimi. Yalnız bırakılmış kullanıcılar çözmek için bkz [yalnız bırakılmış kullanıcılar sorun giderme](https://msdn.microsoft.com/library/ms175475.aspx). Ayrıca bkz: [olağanüstü durum kurtarma işleminden sonra Azure SQL veritabanı güvenliği yönetmek nasıl](sql-database-geo-replication-security-config.md).

Yeni veritabanı içindeki tüm kullanıcılar sahip oldukları kaynak veritabanında izinlerini korur. Veritabanı kopyasını başlatan kullanıcı yeni veritabanının veritabanı sahibi olur ve yeni bir güvenlik tanımlayıcısı (SID) atanır. Kopyalama başarılı olduktan sonra ve diğer kullanıcıların eşleştirilir önce veritabanı sahibi, kopyalama, başlatılan oturum açma için yeni veritabanı oturum açabilir.

Farklı bir mantıksal sunucu için veritabanı kopyalama sırasında kullanıcılar ve oturum açma bilgilerini yönetme hakkında bilgi edinmek için [olağanüstü durum kurtarma işleminden sonra Azure SQL veritabanı güvenliği yönetmek nasıl](sql-database-geo-replication-security-config.md).

## <a name="next-steps"></a>Sonraki adımlar

* Oturum açma bilgileri hakkında daha fazla bilgi için bkz. [oturum açma bilgilerini yönetme](sql-database-manage-logins.md) ve [olağanüstü durum kurtarma işleminden sonra Azure SQL veritabanı güvenliği yönetmek nasıl](sql-database-geo-replication-security-config.md).
* Veritabanını dışarı aktarma için bkz: [veritabanını dışarı aktarma için bir BACPAC](sql-database-export.md).
