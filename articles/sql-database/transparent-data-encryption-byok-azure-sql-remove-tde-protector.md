---
title: PowerShell - TDE koruyucusu Remove - Azure SQL veritabanı | Microsoft Docs
description: Bir Azure SQL veritabanı veya TDE Getir bilgisayarınızı kendi anahtarını (BYOK) destekli kullanarak veri ambarı için bir riskli olabilecek TDE koruyucusuna yanıt için nasıl yapılır Kılavuzu.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: aliceku
ms.author: aliceku
ms.reviewer: vanto
manager: craigg
ms.date: 10/12/2018
ms.openlocfilehash: 4a3677dc5402948fc0105190d1891d709291d0f7
ms.sourcegitcommit: 3f4ffc7477cff56a078c9640043836768f212a06
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2019
ms.locfileid: "57317739"
---
# <a name="remove-a-transparent-data-encryption-tde-protector-using-powershell"></a>PowerShell kullanarak bir saydam veri şifrelemesi (TDE) koruyucusu Kaldır

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

- Bir Azure aboneliğiniz varsa ve bu abonelik bir yönetici olması gerekir
- Azure PowerShell yüklenmiş ve çalışıyor olması gerekir. 
- Bu nasıl yapılır kılavuzunda, zaten Azure Key vault'tan bir anahtar TDE koruyucusu olarak bir Azure SQL veritabanı veya veri ambarı için kullandığınızı varsayar. Bkz: [BYOK destekli saydam veri şifrelemesi](transparent-data-encryption-byok-azure-sql.md) daha fazla bilgi için.

## <a name="overview"></a>Genel Bakış

Bu nasıl yapılır kılavuzunda, bir Azure SQL veritabanı veya müşteri tarafından yönetilen anahtarları Azure Key vault'taki - Getir bilgisayarınızı kendi anahtarını (BYOK) desteği ile TDE kullanarak veri ambarı için bir riskli olabilecek TDE koruyucusuna yanıt açıklar. TDE için BYOK destekli hakkında daha fazla bilgi için bkz: [genel bakış sayfasında](transparent-data-encryption-byok-azure-sql.md). 

Aşağıdaki yordamlar, yalnızca olağanüstü durumlarda veya test ortamlarında yapılmalıdır. Nasıl yapılır kılavuzunda dikkatle gözden geçirin, etkin olarak kullanılan TDE silme olarak Azure anahtar Kasası'ndaki koruyucuları sonuçlanabilir **veri kaybı**. 

Bir hizmet veya kullanıcı anahtarına erişimi yetkisiz gibi bir anahtar tehlikeye için hiç olmadığı kadar kuşkulanılıyor, anahtarı silmek idealdir.

Unutmayın, bir kez TDE koruyucusu, anahtar Kasası'nda silinir **sunucunun altındaki şifreli veritabanlarına yönelik tüm bağlantılar engellenir ve bu veritabanları çevrimdışı ve 24 saat içinde bırakılır**. Güvenliği aşılmış bir anahtarla şifrelenmiş eski yedeklere artık erişilemez.

Bu nasıl yapılır kılavuzunda sonra olay yanıtı istenen sonuca bağlı olarak iki yaklaşım üzerinden geçer:

- Azure SQL veritabanlarını korumak için / Data Warehouses **erişilebilir**
- Azure SQL veritabanı olmak için / Data Warehouses **erişilemez**

## <a name="to-keep-the-encrypted-resources-accessible"></a>Şifrelenmiş kaynakları erişilebilir tutmak için

1. Oluşturma bir [anahtar Kasası'nda yeni anahtar](https://docs.microsoft.com/powershell/module/azurerm.keyvault/add-azurekeyvaultkey?view=azurermps-4.1.0). Erişim denetimi bir kasa düzeyinde sağlandığı bu yeni anahtar riskli olabilecek TDE koruyucusuna'ndan ayrı bir anahtar kasasındaki oluşturulduğundan emin olun. 
2. Yeni anahtarı kullanarak sunucuya ekleme [Ekle AzSqlServerKeyVaultKey](/powershell/module/az.sql/add-azsqlserverkeyvaultkey) ve [kümesi AzSqlServerTransparentDataEncryptionProtector](/powershell/module/az.sql/set-azsqlservertransparentdataencryptionprotector) cmdlet'leri ve sunucunun yeni TDE koruyucusu güncelleştirin.

   ```powershell
   # Add the key from Key Vault to the server  
   Add-AzSqlServerKeyVaultKey `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> `
   -KeyId <KeyVaultKeyId>

   # Set the key as the TDE protector for all resources under the server
   Set-AzSqlServerTransparentDataEncryptionProtector `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> `
   -Type AzureKeyVault -KeyId <KeyVaultKeyId> 
   ```

3. Sunucu emin olun ve tüm çoğaltmaları kullanarak yeni TDE koruyucusuna güncelleştirilmiş [Get-AzSqlServerTransparentDataEncryptionProtector](/powershell/module/az.sql/get-azsqlservertransparentdataencryptionprotector) cmdlet'i. 

   >[!NOTE]
   > Bu, tüm veritabanlarına ve ikincil veritabanları sunucusu altında yaymak yeni TDE koruyucusuna birkaç dakika sürebilir.

   ```powershell
   Get-AzSqlServerTransparentDataEncryptionProtector `
   -ServerName <LogicalServerName> `
   -ResourceGroupName <SQLDatabaseResourceGroupName>
   ```

4. Ele bir [yeni anahtar yedekleme](/powershell/module/az.keyvault/backup-azurekeyvaultkey) anahtar Kasası'nda.

   ```powershell
   <# -OutputFile parameter is optional; 
   if removed, a file name is automatically generated. #>
   Backup-AzureKeyVaultKey `
   -VaultName <KeyVaultName> `
   -Name <KeyVaultKeyName> `
   -OutputFile <DesiredBackupFilePath>
   ```
 
5. Key Vault kullanarak güvenliği aşılan anahtar Sil [Remove-AzKeyVaultKey](/powershell/module/az.keyvault/remove-azurekeyvaultkey) cmdlet'i. 

   ```powershell
   Remove-AzKeyVaultKey `
   -VaultName <KeyVaultName> `
   -Name <KeyVaultKeyName>
   ```
 
6. Gelecekte kullanarak Key Vault için bir anahtarı geri [geri yükleme-AzKeyVaultKey](/powershell/module/az.keyvault/restore-azurekeyvaultkey) cmdlet:
   ```powershell
   Restore-AzKeyVaultKey `
   -VaultName <KeyVaultName> `
   -InputFile <BackupFilePath>
   ```

## <a name="to-make-the-encrypted-resources-inaccessible"></a>Şifrelenmiş kaynakları erişilemez hale getirmek için

1. Riskli olabilecek anahtarla şifrelenmiş veritabanları bırakın.

   Herhangi bir noktada veritabanının bir zaman içinde nokta geri yükleme (anahtarı sağlayın sürece) yapılabilir, böylece veritabanı ve günlük dosyaları, otomatik olarak desteklenir. En son işlemlerin en fazla 10 dakika olası veri kaybını önlemek için etkin bir TDE koruyucusuna silme işleminden önce veritabanlarının bırakılmalıdır. 
2. Key vault'taki TDE koruyucusu, anahtar malzemesi yedekleyin.
3. Key Vault'tan riskli olabilecek anahtarı Kaldır

## <a name="next-steps"></a>Sonraki adımlar

- Güvenlik gereksinimlerine uymak için bir sunucunun TDE koruyucusuna döndürme hakkında bilgi edinin: [Saydam veri şifrelemesi koruyucu PowerShell kullanarak Döndür](transparent-data-encryption-byok-azure-sql-key-rotation.md)
- Destek TDE için kendi anahtarını Getir ile kullanmaya başlayın: [PowerShell kullanarak Key vault'tan kendi anahtarınızı kullanarak TDE Aç](transparent-data-encryption-byok-azure-sql-configure.md)
