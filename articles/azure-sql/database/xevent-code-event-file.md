---
title: XEvent olay dosyası kodu
description: Azure SQL veritabanı 'nda genişletilmiş bir olayda olay dosyası hedefini gösteren iki aşamalı kod örneği için PowerShell ve Transact-SQL sağlar. Azure depolama, bu senaryonun gerekli bir parçasıdır.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: sqldbrb=1
ms.devlang: PowerShell
ms.topic: sample
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.reviewer: sstein
ms.date: 06/06/2020
ms.openlocfilehash: d7a57f98551cf91ed87858caba0907471bcf6b12
ms.sourcegitcommit: d60976768dec91724d94430fb6fc9498fdc1db37
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/02/2020
ms.locfileid: "96501235"
---
# <a name="event-file-target-code-for-extended-events-in-azure-sql-database"></a>Azure SQL veritabanı 'nda genişletilmiş olaylar için olay dosyası hedef kodu
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

[!INCLUDE [sql-database-xevents-selectors-1-include](../../../includes/sql-database-xevents-selectors-1-include.md)]

Genişletilmiş bir olay için bilgileri yakalamak ve raporlamak için sağlam bir yol için bir kod örneği istersiniz.

Microsoft SQL Server, olay [dosyası hedefi](/previous-versions/sql/sql-server-2016/ff878115(v=sql.130)) , olay çıktılarını yerel bir sabit sürücü dosyasına depolamak için kullanılır. Ancak bu tür dosyalar Azure SQL veritabanı için kullanılamaz. Bunun yerine, olay dosyası hedefini desteklemek için Azure Storage hizmetini kullanırız.

Bu konuda, iki aşamalı bir kod örneği sunulmaktadır:

- PowerShell, bulutta bir Azure depolama kapsayıcısı oluşturmak için.
- Transact-SQL:
  - Azure depolama kapsayıcısını bir olay dosyası hedefine atamak için.
  - Olay oturumu oluşturmak ve başlatmak için ve bu şekilde devam edin.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

> [!IMPORTANT]
> PowerShell Azure Resource Manager modülü Azure SQL veritabanı tarafından hala desteklenmektedir, ancak gelecekteki tüm geliştirmeler az. SQL modülüne yöneliktir. Bu cmdlet 'ler için bkz. [Azurerd. SQL](/powershell/module/AzureRM.Sql/). Az Module ve Azurerd modüllerinde komutların bağımsız değişkenleri önemli ölçüde aynıdır.

- Bir Azure hesabı ve aboneliği [Ücretsiz deneme sürümü](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.
- İçinde tablo oluşturabileceğiniz herhangi bir veritabanı.
  
  - İsteğe bağlı olarak, dakikalar içinde [bir **AdventureWorksLT** demo veritabanı oluşturabilirsiniz](single-database-create-quickstart.md) .

- SQL Server Management Studio (ssms.exe), ideal olarak en son aylık güncelleştirme sürümü.
  En son ssms.exe şuradan indirebilirsiniz:
  
  - [SQL Server Management Studio indirme](/sql/ssms/download-sql-server-management-studio-ssms)başlıklı konu.
  - [İndirmenin doğrudan bağlantısı.](https://go.microsoft.com/fwlink/?linkid=616025)

- [Azure PowerShell modüllerinin](https://go.microsoft.com/?linkid=9811175) yüklü olması gerekir.

  - Modüller- **New-AzStorageAccount** gibi komutlar sağlar.

## <a name="phase-1-powershell-code-for-azure-storage-container"></a>1. Aşama: Azure depolama kapsayıcısı için PowerShell kodu

Bu PowerShell, iki aşamalı kod örneğinin Aşama 1 ' dir.

Betik, bir önceki çalıştırıldıktan sonra temizlik komutları ile başlar ve yeniden başlatılamaz.

1. PowerShell betiğini Notepad.exe gibi bir basit metin düzenleyicisine yapıştırın ve betiği **. ps1** uzantısına sahip bir dosya olarak kaydedin.
2. PowerShell ıSE 'yi yönetici olarak başlatın.
3. İsteminde şunu yazın:<br/>`Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser`<br/>ve ardından ENTER tuşuna basın.
4. PowerShell ıSE 'de **. ps1** dosyanızı açın. Betiği çalıştırın.
5. Betik ilk olarak Azure 'da oturum açarken yeni bir pencere başlatır.

   - Oturumunuzu kesintiye uğratmadan betiği yeniden çalıştırırsanız, **Add-AzureAccount** komutunu açıklama eklemek uygun bir seçeneğe sahip olursunuz.

![Azure modülü yüklü olan PowerShell ıSE, betiği çalıştırmaya hazırlanıyor.](./media/xevent-code-event-file/event-file-powershell-ise-b30.png)

### <a name="powershell-code"></a>PowerShell kodu

Bu PowerShell betiği az Module 'ü zaten yüklediğinizi varsayar. Bilgi için bkz. [Azure PowerShell modülünü Install](/powershell/azure/install-Az-ps).

```powershell
## TODO: Before running, find all 'TODO' and make each edit!!

cls;

#--------------- 1 -----------------------

'Script assumes you have already logged your PowerShell session into Azure.
But if not, run  Connect-AzAccount (or  Connect-AzAccount), just one time.';
#Connect-AzAccount;   # Same as  Connect-AzAccount.

#-------------- 2 ------------------------

'
TODO: Edit the values assigned to these variables, especially the first few!
';

# Ensure the current date is between
# the Expiry and Start time values that you edit here.

$subscriptionName    = 'YOUR_SUBSCRIPTION_NAME';
$resourceGroupName   = 'YOUR_RESOURCE-GROUP-NAME';

$policySasExpiryTime = '2018-08-28T23:44:56Z';
$policySasStartTime  = '2017-10-01';

$storageAccountLocation = 'YOUR_STORAGE_ACCOUNT_LOCATION';
$storageAccountName     = 'YOUR_STORAGE_ACCOUNT_NAME';
$contextName            = 'YOUR_CONTEXT_NAME';
$containerName          = 'YOUR_CONTAINER_NAME';
$policySasToken         = ' ? ';

$policySasPermission = 'rwl';  # Leave this value alone, as 'rwl'.

#--------------- 3 -----------------------

# The ending display lists your Azure subscriptions.
# One should match the $subscriptionName value you assigned
#   earlier in this PowerShell script.

'Choose an existing subscription for the current PowerShell environment.';

Select-AzSubscription -Subscription $subscriptionName;

#-------------- 4 ------------------------

'
Clean up the old Azure Storage Account after any previous run,
before continuing this new run.';

if ($storageAccountName) {
    Remove-AzStorageAccount `
        -Name              $storageAccountName `
        -ResourceGroupName $resourceGroupName;
}

#--------------- 5 -----------------------

[System.DateTime]::Now.ToString();

'
Create a storage account.
This might take several minutes, will beep when ready.
  ...PLEASE WAIT...';

New-AzStorageAccount `
    -Name              $storageAccountName `
    -Location          $storageAccountLocation `
    -ResourceGroupName $resourceGroupName `
    -SkuName           'Standard_LRS';

[System.DateTime]::Now.ToString();
[System.Media.SystemSounds]::Beep.Play();

'
Get the access key for your storage account.
';

$accessKey_ForStorageAccount = `
    (Get-AzStorageAccountKey `
        -Name              $storageAccountName `
        -ResourceGroupName $resourceGroupName
        ).Value[0];

"`$accessKey_ForStorageAccount = $accessKey_ForStorageAccount";

'Azure Storage Account cmdlet completed.
Remainder of PowerShell .ps1 script continues.
';

#--------------- 6 -----------------------

# The context will be needed to create a container within the storage account.

'Create a context object from the storage account and its primary access key.
';

$context = New-AzStorageContext `
    -StorageAccountName $storageAccountName `
    -StorageAccountKey  $accessKey_ForStorageAccount;

'Create a container within the storage account.
';

$containerObjectInStorageAccount = New-AzStorageContainer `
    -Name    $containerName `
    -Context $context;

'Create a security policy to be applied to the SAS token.
';

New-AzStorageContainerStoredAccessPolicy `
    -Container  $containerName `
    -Context    $context `
    -Policy     $policySasToken `
    -Permission $policySasPermission `
    -ExpiryTime $policySasExpiryTime `
    -StartTime  $policySasStartTime;

'
Generate a SAS token for the container.
';
try {
    $sasTokenWithPolicy = New-AzStorageContainerSASToken `
        -Name    $containerName `
        -Context $context `
        -Policy  $policySasToken;
}
catch {
    $Error[0].Exception.ToString();
}

#-------------- 7 ------------------------

'Display the values that YOU must edit into the Transact-SQL script next!:
';

"storageAccountName: $storageAccountName";
"containerName:      $containerName";
"sasTokenWithPolicy: $sasTokenWithPolicy";

'
REMINDER: sasTokenWithPolicy here might start with "?" character, which you must exclude from Transact-SQL.
';

'
(Later, return here to delete your Azure Storage account. See the preceding  Remove-AzStorageAccount -Name $storageAccountName)';

'
Now shift to the Transact-SQL portion of the two-part code sample!';

# EOFile
```

PowerShell betiğinin bittiği sırada yazdırdığı birkaç adlandırılmış değeri unutmayın. Bu değerleri, Aşama 2 olarak takip eden Transact-SQL betiğine düzenlemeniz gerekir.

<!--
TODO:   Consider whether the preceding PowerShell code example deserves to be updated to the latest package (AzureRM.SQL?).
2020/June/06   Adding the !NOTE below about "ADLS Gen2 storage accounts".
Related to   https://github.com/MicrosoftDocs/azure-docs/issues/56520
-->

> [!NOTE]
> Önceki PowerShell kod örneğinde, SQL genişletilmiş olayları ADLS 2. depolama hesaplarıyla uyumlu değildir.

## <a name="phase-2-transact-sql-code-that-uses-azure-storage-container"></a>2. Aşama: Azure Storage kapsayıcısını kullanan Transact-SQL kodu

- Bu kod örneğinin 1. aşamasında, bir Azure depolama kapsayıcısı oluşturmak için bir PowerShell betiği çalıştırdınız.
- Sonraki aşama 2 ' de, aşağıdaki Transact-SQL betiği kapsayıcıyı kullanmalıdır.

Betik, bir önceki çalıştırıldıktan sonra temizlik komutları ile başlar ve yeniden başlatılamaz.

PowerShell betiği, sona erdikten sonra birkaç adlandırılmış değer yazdırılmıştır. Bu değerleri kullanmak için Transact-SQL betiğini düzenlemeniz gerekir. Düzenleme noktalarını bulmak için Transact-SQL komut dosyasında **Todo** bulun.

1. SQL Server Management Studio açın (ssms.exe).
2. Azure SQL veritabanı 'nda veritabanınıza bağlanın.
3. Yeni bir sorgu bölmesi açmak için tıklayın.
4. Aşağıdaki Transact-SQL betiğini sorgu bölmesine yapıştırın.
5. Betikteki her **Todo** bulun ve uygun düzenlemeleri yapın.
6. Betiği kaydedin ve çalıştırın.

> [!WARNING]
> Önceki PowerShell betiği tarafından oluşturulan SAS anahtarı değeri '? ' ile başlayabilir (soru işareti). SAS anahtarını aşağıdaki T-SQL komut dosyasında kullandığınızda, *önde gelen '? ' öğesini kaldırmanız* gerekir. Aksi takdirde, çalışmalarınız güvenlik tarafından engelleniyor olabilir.

### <a name="transact-sql-code"></a>Transact-SQL kodu

```sql
---- TODO: First, run the earlier PowerShell portion of this two-part code sample.
---- TODO: Second, find every 'TODO' in this Transact-SQL file, and edit each.

---- Transact-SQL code for Event File target on Azure SQL Database.

SET NOCOUNT ON;
GO

----  Step 1.  Establish one little table, and  ---------
----  insert one row of data.

IF EXISTS
    (SELECT * FROM sys.objects
        WHERE type = 'U' and name = 'gmTabEmployee')
BEGIN
    DROP TABLE gmTabEmployee;
END
GO

CREATE TABLE gmTabEmployee
(
    EmployeeGuid         uniqueIdentifier   not null  default newid()  primary key,
    EmployeeId           int                not null  identity(1,1),
    EmployeeKudosCount   int                not null  default 0,
    EmployeeDescr        nvarchar(256)          null
);
GO

INSERT INTO gmTabEmployee ( EmployeeDescr )
    VALUES ( 'Jane Doe' );
GO

------  Step 2.  Create key, and  ------------
------  Create credential (your Azure Storage container must already exist).

IF NOT EXISTS
    (SELECT * FROM sys.symmetric_keys
        WHERE symmetric_key_id = 101)
BEGIN
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = '0C34C960-6621-4682-A123-C7EA08E3FC46' -- Or any newid().
END
GO

IF EXISTS
    (SELECT * FROM sys.database_scoped_credentials
        -- TODO: Assign AzureStorageAccount name, and the associated Container name.
        WHERE name = 'https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent')
BEGIN
    DROP DATABASE SCOPED CREDENTIAL
        -- TODO: Assign AzureStorageAccount name, and the associated Container name.
        [https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent] ;
END
GO

CREATE
    DATABASE SCOPED
    CREDENTIAL
        -- use '.blob.',   and not '.queue.' or '.table.' etc.
        -- TODO: Assign AzureStorageAccount name, and the associated Container name.
        [https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent]
    WITH
        IDENTITY = 'SHARED ACCESS SIGNATURE',  -- "SAS" token.
        -- TODO: Paste in the long SasToken string here for Secret, but exclude any leading '?'.
        SECRET = 'sv=2014-02-14&sr=c&si=gmpolicysastoken&sig=EjAqjo6Nu5xMLEZEkMkLbeF7TD9v1J8DNB2t8gOKTts%3D'
    ;
GO

------  Step 3.  Create (define) an event session.  --------
------  The event session has an event with an action,
------  and a has a target.

IF EXISTS
    (SELECT * from sys.database_event_sessions
        WHERE name = 'gmeventsessionname240b')
BEGIN
    DROP
        EVENT SESSION
            gmeventsessionname240b
        ON DATABASE;
END
GO

CREATE
    EVENT SESSION
        gmeventsessionname240b
    ON DATABASE

    ADD EVENT
        sqlserver.sql_statement_starting
            (
            ACTION (sqlserver.sql_text)
            WHERE statement LIKE 'UPDATE gmTabEmployee%'
            )
    ADD TARGET
        package0.event_file
            (
            -- TODO: Assign AzureStorageAccount name, and the associated Container name.
            -- Also, tweak the .xel file name at end, if you like.
            SET filename =
                'https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent/anyfilenamexel242b.xel'
            )
    WITH
        (MAX_MEMORY = 10 MB,
        MAX_DISPATCH_LATENCY = 3 SECONDS)
    ;
GO

------  Step 4.  Start the event session.  ----------------
------  Issue the SQL Update statements that will be traced.
------  Then stop the session.

------  Note: If the target fails to attach,
------  the session must be stopped and restarted.

ALTER
    EVENT SESSION
        gmeventsessionname240b
    ON DATABASE
    STATE = START;
GO

SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM gmTabEmployee;

UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 2
    WHERE EmployeeDescr = 'Jane Doe';

UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 13
    WHERE EmployeeDescr = 'Jane Doe';

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM gmTabEmployee;
GO

ALTER
    EVENT SESSION
        gmeventsessionname240b
    ON DATABASE
    STATE = STOP;
GO

-------------- Step 5.  Select the results. ----------

SELECT
        *, 'CLICK_NEXT_CELL_TO_BROWSE_ITS_RESULTS!' as [CLICK_NEXT_CELL_TO_BROWSE_ITS_RESULTS],
        CAST(event_data AS XML) AS [event_data_XML]  -- TODO: In ssms.exe results grid, double-click this cell!
    FROM
        sys.fn_xe_file_target_read_file
            (
                -- TODO: Fill in Storage Account name, and the associated Container name.
                -- TODO: The name of the .xel file needs to be an exact match to the files in the storage account Container (You can use Storage Account explorer from the portal to find out the exact file names or you can retrieve the name using the following DMV-query: select target_data from sys.dm_xe_database_session_targets. The 3rd xml-node, "File name", contains the name of the file currently written to.)
                'https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent/anyfilenamexel242b',
                null, null, null
            );
GO

-------------- Step 6.  Clean up. ----------

DROP
    EVENT SESSION
        gmeventsessionname240b
    ON DATABASE;
GO

DROP DATABASE SCOPED CREDENTIAL
    -- TODO: Assign AzureStorageAccount name, and the associated Container name.
    [https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent]
    ;
GO

DROP TABLE gmTabEmployee;
GO

PRINT 'Use PowerShell Remove-AzStorageAccount to delete your Azure Storage account!';
GO
```

Çalıştırdığınızda hedef iliştirilemezse olay oturumunu durdurmanız ve yeniden başlatmanız gerekir:

```sql
ALTER EVENT SESSION ... STATE = STOP;
GO
ALTER EVENT SESSION ... STATE = START;
GO
```

## <a name="output"></a>Çıktı

Transact-SQL betiği tamamlandığında, **event_data_XML** sütun üst bilgisinin altındaki bir hücreye tıklayın. Tek bir **\<event>** Update ifadesini gösteren bir öğe görüntülenir.

**\<event>** Test sırasında oluşturulan bir öğe aşağıda verilmiştir:

```xml
<event name="sql_statement_starting" package="sqlserver" timestamp="2015-09-22T19:18:45.420Z">
  <data name="state">
    <value>0</value>
    <text>Normal</text>
  </data>
  <data name="line_number">
    <value>5</value>
  </data>
  <data name="offset">
    <value>148</value>
  </data>
  <data name="offset_end">
    <value>368</value>
  </data>
  <data name="statement">
    <value>UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 2
    WHERE EmployeeDescr = 'Jane Doe'</value>
  </data>
  <action name="sql_text" package="sqlserver">
    <value>

SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM gmTabEmployee;

UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 2
    WHERE EmployeeDescr = 'Jane Doe';

UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 13
    WHERE EmployeeDescr = 'Jane Doe';

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM gmTabEmployee;
</value>
  </action>
</event>
```

Yukarıdaki Transact-SQL betiği, event_file okumak için aşağıdaki sistem işlevini kullandı:

- [sys.fn_xe_file_target_read_file (Transact-SQL)](/sql/relational-databases/system-functions/sys-fn-xe-file-target-read-file-transact-sql)

Genişletilmiş olaylardan verilerin görüntülenmesine yönelik gelişmiş seçeneklerin açıklaması şurada bulunabilir:

- [Genişletilmiş olaylardaki hedef verilerin gelişmiş görüntüleme](/sql/relational-databases/extended-events/advanced-viewing-of-target-data-from-extended-events-in-sql-server)

## <a name="converting-the-code-sample-to-run-on-sql-server"></a>Kod örneğini SQL Server üzerinde çalışacak şekilde dönüştürme

Yukarıdaki Transact-SQL örneğini Microsoft SQL Server çalıştırmak istediğinizi varsayalım.

- Kolaylık olması için, Azure depolama kapsayıcısının kullanımını *C:\myeventdata.XEL* gibi basit bir dosyayla tamamen değiştirmek isteyebilirsiniz. Dosya, SQL Server barındıran bilgisayarın yerel sabit sürücüsüne yazılır.
- **Ana anahtar oluştur** ve **kimlik bilgisi oluşturma** için HERHANGI bir Transact-SQL deyimlerine ihtiyacınız yoktur.
- **Olay oturumu oluştur** Ifadesinde, **add target** yan tümcesinde, **filename =** için atanan http değerini, *C:\MyFile.XEL* gibi bir tam yol dizesiyle değiştirirsiniz.
  
  - Azure depolama hesabı gerekmez.

## <a name="more-information"></a>Daha fazla bilgi

Azure depolama hizmetindeki hesaplar ve kapsayıcılar hakkında daha fazla bilgi için bkz.:

- [.NET 'ten blob depolamayı kullanma](../../storage/blobs/storage-quickstart-blobs-dotnet.md)
- [Kapsayıcıları, Blobları ve meta verileri adlandırma ve başvuru](/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata)
- [Kök Kapsayıcınle çalışma](/rest/api/storageservices/Working-with-the-Root-Container)
- [1. ders: bir Azure kapsayıcısında depolanan erişim ilkesi ve paylaşılan erişim imzası oluşturma](/sql/relational-databases/tutorial-use-azure-blob-storage-service-with-sql-server-2016#1---create-stored-access-policy-and-shared-access-storage)
  - [2. ders: paylaşılan erişim imzası kullanarak SQL Server kimlik bilgileri oluşturma](/sql/relational-databases/tutorial-use-azure-blob-storage-service-with-sql-server-2016#2---create-a-sql-server-credential-using-a-shared-access-signature)
- [Microsoft SQL Server için genişletilmiş olaylar](/sql/relational-databases/extended-events/extended-events)