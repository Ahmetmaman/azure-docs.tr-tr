---
title: Çok sayıda uygulamaya bir Azure key vault - Azure anahtar kasası erişim izni verme | Microsoft Docs
description: Çok sayıda uygulamaya bir anahtar kasasına erişim izni vermeyi öğreneceksiniz
services: key-vault
documentationcenter: ''
author: amitbapat
manager: barbkess
tags: azure-resource-manager
ms.assetid: 785d4e40-fb7b-485a-8cbc-d9c8c87708e6
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/07/2019
ms.author: ambapat
ms.openlocfilehash: 187d455003cf8b1c9402e24755c5f15b703cd9ad
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2019
ms.locfileid: "56114408"
---
# <a name="grant-several-applications-access-to-a-key-vault"></a>Bir anahtar kasasına çeşitli uygulamaları erişimi verme

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Erişim denetimi İlkesi, bir anahtar kasası çeşitli uygulamalar erişim vermek için kullanılabilir. Bir erişim denetimi ilkesi en fazla 1024 uygulamaları destekler ve aşağıdaki gibi yapılandırılır:

1. Bir Azure Active Directory güvenlik grubu oluşturun. 
2. Hizmet sorumlularını güvenlik grubunun ilişkili tüm uygulamalar ekleyin.
3. Güvenlik grubu, anahtar Kasası'na erişim.

## <a name="prerequisites"></a>Önkoşullar

Önkoşullar şunlardır:
* [Azure PowerShell'i yükleme](/powershell/azure/overview).
* [Azure Active Directory V2 PowerShell modülünü yükleme](https://www.powershellgallery.com/packages/AzureAD).
* Azure Active Directory kiracısı grupları oluştur/Düzenle için izinler. İzinleriniz yoksa, Azure Active Directory yöneticinize başvurmanız gerekebilir. Bkz: [Azure Key Vault hakkında anahtarlara, parolalara ve sertifikalara](about-keys-secrets-and-certificates.md) Key Vault hakkında ayrıntılı bilgi için erişim ilkesi izinleri.

## <a name="granting-key-vault-access-to-applications"></a>Uygulamalar için Key Vault erişim izni verme

PowerShell'de aşağıdaki komutları çalıştırın:

```powershell
# Connect to Azure AD 
Connect-AzureAD 
 
# Create Azure Active Directory Security Group 
$aadGroup = New-AzureADGroup -Description "Contoso App Group" -DisplayName "ContosoAppGroup" -MailEnabled 0 -MailNickName none -SecurityEnabled 1 
 
# Find and add your applications (ServicePrincipal ObjectID) as members to this group 
$spn = Get-AzureADServicePrincipal –SearchString "ContosoApp1" 
Add-AzureADGroupMember –ObjectId $aadGroup.ObjectId -RefObjectId $spn.ObjectId 
 
# You can add several members to this group, in this fashion. 
 
# Set the Key Vault ACLs 
Set-AzKeyVaultAccessPolicy –VaultName ContosoVault –ObjectId $aadGroup.ObjectId `
-PermissionsToKeys decrypt,encrypt,unwrapKey,wrapKey,verify,sign,get,list,update,create,import,delete,backup,restore,recover,purge `
–PermissionsToSecrets get,list,set,delete,backup,restore,recover,purge `
–PermissionsToCertificates get,list,delete,create,import,update,managecontacts,getissuers,listissuers,setissuers,deleteissuers,manageissuers,recover,purge,backup,restore `
-PermissionsToStorage get,list,delete,set,update,regeneratekey,getsas,listsas,deletesas,setsas,recover,backup,restore,purge 
 
# Of course you can adjust the permissions as required 
```

Farklı bir uygulama grubu için izin kümesi vermek gerekirse, bu tür uygulamalar için ayrı bir Azure Active Directory güvenlik grubu oluşturun.

## <a name="next-steps"></a>Sonraki adımlar

Kullanma hakkında daha fazla bilgi edinin [anahtar kasanızın güvenliğini sağlama](key-vault-secure-your-key-vault.md).
