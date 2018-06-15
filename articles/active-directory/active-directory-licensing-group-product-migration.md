---
title: Kullanıcılar Grup tabanlı Azure Active Directory'de lisans kullanarak ürün lisansları arasında güvenli bir şekilde geçirme | Microsoft Docs
description: Grup tabanlı Lisansı kullanılarak kullanıcılar farklı ürün lisansları (Office 365 Kurumsal E1 ve E3) arasında geçirmek için önerilen işlem açıklanır
services: active-directory
keywords: Azure AD lisanslama
documentationcenter: ''
author: piotrci
manager: mtillman
editor: ''
ms.assetid: ''
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/07/2018
ms.author: piotrci
ms.openlocfilehash: 068457044af7af7a55bdbcc4043da3028a68b2d0
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
ms.locfileid: "32140351"
---
# <a name="how-to-safely-migrate-users-between-product-licenses-by-using-group-based-licensing"></a>Kullanıcılar Grup tabanlı lisans kullanarak ürün lisansları arasında güvenli bir şekilde geçirme

Bu makalede, kullanıcıların Grup tabanlı lisans kullanırken ürün lisansları arasında taşımak için önerilen yöntem açıklanır. Geçiş sırasında hiçbir hizmet veya veri kaybı olduğundan emin olmak için bu yaklaşım belirtilir: kullanıcılar geçiş ürünleri arasında sorunsuz bir şekilde. Geçiş işlemi iki çeşidini ele alınmaktadır:

-   Office 365 Kurumsal E3 ve Office 365 Kurumsal E5 arasında geçiş yapma gibi çakışan hizmet planları içermeyen ürün lisansları arasında basit geçiş.

-   Office 365 Kurumsal E1 ve Office 365 Kurumsal E3 arasında geçiş yapma gibi bazı Çakışan hizmeti içeren ürünleri arasında daha karmaşık geçiş planlar. Çakışmaları hakkında daha fazla bilgi için bkz: [çakışan hizmet planları](https://docs.microsoft.com/azure/active-directory/active-directory-licensing-group-problem-resolution-azure-portal#conflicting-service-plans) ve [hizmet aynı anda atanamaz planları](https://docs.microsoft.com/azure/active-directory/active-directory-licensing-product-and-service-plan-reference#service-plans-that-cannot-be-assigned-at-the-same-time).

Bu makalede, geçirme ve doğrulama adımlarını gerçekleştirmek için kullanılan örnek PowerShell kodu içerir. Kodu nereye adımları el ile gerçekleştirmek için uygun olmadığı büyük ölçekli işlemler için özellikle yararlıdır.

## <a name="before-you-begin"></a>Başlamadan önce
Geçişe başlamadan önce bazı varsayımlarda tüm geçirilecek kullanıcıları için doğru doğrulamak önemlidir. Varsayımlar tüm kullanıcılar için doğru değilse geçiş için bazı başarısız olabilir. Sonuç olarak, bazı kullanıcıların Hizmetleri ya da veri erişimi kaybedebilirsiniz. Aşağıdaki varsayımlar doğrulanması gerekir:

-   Kullanıcınız *kaynak lisans* grup tabanlı Lisansı kullanılarak atanır. Merkezden taşımak ürün lisansına tek kaynak grubundan devralınır ve doğrudan atanmamış.

    >[!NOTE]
    >Lisansları de doğrudan atanırsa, uygulamayı engelleyebilirsiniz *hedef lisans*. Daha fazla bilgi edinmek [doğrudan ve Grup lisans atamasını](https://docs.microsoft.com/azure/active-directory/active-directory-licensing-group-advanced#direct-licenses-coexist-with-group-licenses). Kullanmak istediğiniz bir [PowerShell Betiği](https://docs.microsoft.com/azure/active-directory/active-directory-licensing-ps-examples#check-if-user-license-is-assigned-directly-or-inherited-from-a-group) kullanıcıların doğrudan lisansları olup olmadığını denetlemek için.

-   Hedef ürünü için kullanılabilir yeterli lisans var. Yeterince lisansa sahip değilseniz, bazı kullanıcılar değil alabilirsiniz *hedef lisans*. Yapabilecekleriniz [kullanılabilir lisans sayısını kontrol](https://portal.azure.com/#blade/Microsoft_AAD_IAM/LicensesMenuBlade/Products).

-   Kullanıcılar ile çakışabilir diğer atanan ürün lisansları sahip değilseniz *hedef lisans* veya kaldırılmasını önlemek *kaynak lisans*. Örneğin, çalışma alanına Analytics veya Project Online gibi bir eklenti ürünü lisanstan diğer ürünleri bir bağımlılığa sahip.

-   Grupları, ortamınızda nasıl yönetildiğini anlama. Örneğin, grupları şirket içi yönetiyorsanız ve bunları Azure Active Directory (Azure AD) ile Azure AD Connect içine eşitleme sonra Ekle/kullanıcılar, şirket içi sisteminizi kullanarak Kaldır. İşlem değişikliklerin Azure AD ile eşitleme ve grup tabanlı lisans toplanma zaman alır. Azure AD dinamik grup üyeliklerini kullanıyorsanız, ekleyin/kullanıcılar öznitelikleriyle değiştirerek Kaldır. Ancak, genel geçiş işlemi aynı kalır. Tek fark nasıl, ekleyin veya kullanıcılar grubunun üyeliği için Kaldır ' dir.

## <a name="migrate-users-between-products-that-dont-have-conflicting-service-plans"></a>Kullanıcılar çakışan hizmet planları yok ürünleri arasında geçirme
Geçiş kullanıcı lisanslarını değiştirmek için Grup tabanlı lisans kullanımını hedeftir bir *kaynak lisans* (Bu örnekte: Office 365 Kurumsal E3) için bir *hedef lisans* (Bu örnekte: Office 365 Kurumsal E5). Bir çakışma olmadan aynı anda tam olarak atanabilir bu iki ürün Bu senaryoda çakışan hizmet planları hiçbirini içeremez. Geçiş sırasında herhangi bir noktada kullanıcıları Hizmetleri ya da veri erişimi kaybedersiniz. Geçiş küçük "'toplu." gerçekleştirilir Her toplu işlemin sonucu doğrulayın ve işlemi sırasında oluşabilecek sorunları kapsamını en aza indirin. Genel olarak, işlem aşağıdaki gibidir:

1.  Kullanıcılar bir kaynak grubunun bir üyesidir ve bunlar devralır *kaynak lisans* o gruptan.

2.  Sahip bir hedef grup oluşturma *hedef lisans* ancak tüm üyeleri olmadan.

3.  Kullanıcıları toplu hedef grubuna ekleyin. Grup tabanlı lisans değişimi oluşturan seçer ve atar *hedef lisans*. İşlemi genişletilmiş bir toplu işlem ve diğer etkinlikleri Kiracı boyutuna bağlı olarak zaman miktarı alabilir.

4.  Grup tabanlı lisans kullanıcıları toplu tam olarak işlendiğini doğrulayın. Her kullanıcının olduğunu onaylayın *hedef lisans* atanmış. Kullanıcıların diğer ürünler veya yeterli lisans eksikliği çakışıyor gibi bir hata durumuna karşı karşıya kalırsınız kaydetmedi denetleyin. Hatalar hakkında daha fazla bilgi için bkz: [Active Directory Grup sorunu çözümleme lisans](https://docs.microsoft.com/azure/active-directory/active-directory-licensing-group-problem-resolution-azure-portal).

5.  Bu noktada, her ikisi de kullanıcınız *kaynak lisans* ve *hedef lisans* atanmış.

6.  Aynı kullanıcı toplu kaynak grubundan kaldırın. Grup tabanlı lisans değişiklik yanıt verir ve *kaynak lisansları* kullanıcılardan kaldırılır.

7.  Kullanıcılar sonraki toplu işlemi için bu işlemi yineleyin.

### <a name="migrate-a-single-user-by-using-the-azure-portal"></a>Tek bir kullanıcı Azure Portalı'nı kullanarak geçirme
Tek bir kullanıcı geçirmek nasıl basit bir anlatım budur.

**1. adım**: kullanıcının sahip olduğu bir *kaynak lisans* gruptan devralınan. Lisans için hiçbir doğrudan atamaları vardır:

![Bir kaynak lisans kullanıcıyla gruptan devralınan](media/active-directory-licensing-group-product-migration/UserWithSourceLicenseInherited.png)

**2. adım**: kullanıcı hedef grubuna eklenir ve grup tabanlı lisans değişikliği işler. Kullanıcının artık hem de sahip *kaynak lisans* ve *hedef lisans* gruplarından devralınan:

![Hem kaynak hem de hedef lisans kullanıcıyla gruplarından devralınan](media/active-directory-licensing-group-product-migration/UserWithBothSourceAndTargetLicense.png)

**3. adım**: Kullanıcı kaynak grubundan kaldırılır ve grup tabanlı lisans değişikliği işler. Kullanıcı artık yalnızca sahip *hedef lisans*:

![Hedef lisansa sahip kullanıcı gruptan devralınan](media/active-directory-licensing-group-product-migration/UserWithTargetLicenseAssigned.png)

### <a name="automate-migration-by-using-azure-powershell"></a>Azure PowerShell kullanarak geçiş otomatikleştirme
Aşağıdaki kod parçacığında, büyük ölçekli işlemi için geçiş işlemini otomatikleştirmek gösterilmiştir.

> [!NOTE]
> Örnek kod dahil edilen PowerShell işlevlerini kullanan [son bölümü](#powershell-automation-of-migration-and-verification-steps) bu makalenin.

```
# A batch of users that we want to migrate in this iteration.
# The batch can be specified as an array of User Principal Names (string) or ObjectIds (Guid).
# Note: The batch can be loaded from a text file that represents a larger batch of users that we want to migrate.
[string[]]$usersToMigrate = 'MigrationUser@tailspinonline.com','MigrationUser2@tailspinonline.com'

############### NON-CONFLICTING LICENSES SCENARIO ################

# The source group and source license to remove the user from.
$sourceGroupId = [Guid]'b82c04f0-ce30-4ff1-bac7-735d92d83036'
$sourceSkuId = 'TailspinOnline:ENTERPRISEPACK'      # <-- This is the Office 365 Enterprise E3 product.

# The target group and target license to assign to the user.
$targetGroupId = [Guid]'bcf279d1-5ad5-46a5-b469-4b8a552aa2fe'
$targetSkuId = 'TailspinOnline:ENTERPRISEPREMIUM'   # <-- This is the Office 365 Enterprise E5 product.

if(-Not (VerifyAssumptions $usersToMigrate $sourceGroupId $sourceSkuId $targetGroupId $targetSkuId))
{
    throw "Some users did not pass validation checks. See the output for details. Aborting migration process."
}

Write-Host "STEP 1: Adding users to the target group $targetGroupId. This will assign the target license $targetSkuId to all users"
AddUsersToGroup $usersToMigrate $targetGroupId

# Verify that the target license shows up in the conflict state for each user on the list.
# This step runs in a loop, forever, until all of the users are in the expected state.
# Group-based licensing (GBL) can take some time to reflect the changes on users.
# As a result, the loop should terminate after a period of time that's dependent on the size of the user collection.
# Note: If the loop hasn't terminated after a long period of time, stop the script.
#       Inspect the users that are reported as not yet in the expected state.
#       Verify that the users are not blocked for some other reason.
ExecuteVerificationLoop ${function:VerifySourceandTargetLicensePresent} 'STEP 2: Checking if all users still have the source license and now also have the target license from target group' $usersToMigrate $sourceGroupId $sourceSkuId $targetGroupId $targetSkuId

# Now it's safe to remove the users from the source group.
Write-Host "STEP 3: Removing users from the source group $sourceGroupId. This will remove the source license $sourceSkuId."
RemoveUsersFromGroup $usersToMigrate $sourceGroupId

# Verify that the target license is now active on each user and the source license is removed.
ExecuteVerificationLoop ${function:VerifySourceLicenseRemovedAndTargetLicenseAssignedFromGroup} 'STEP 4: Checking if all users have source license removed and target license assigned from target group' $usersToMigrate $sourceGroupId $sourceSkuId $targetGroupId $targetSkuId
```

**Örnek çıktı (geçişi iki kullanıcı için)**

```
Verifying initial assumptions:
Enough TailspinOnline:ENTERPRISEPREMIUM licenses available (13) for users: 2.
migrationuser@tailspinonline.com                OK
migrationuser2@tailspinonline.com               OK
Checks passed for all 2 users.

STEP 1: Adding users to the target group bcf279d1-5ad5-46a5-b469-4b8a552aa2fe. This will assign the target license TailspinOnline:ENTERPRISEPREMIUM to all users
Adding user MigrationUser@tailspinonline.com to group bcf279d1-5ad5-46a5-b469-4b8a552aa2fe
Adding user MigrationUser2@tailspinonline.com to group bcf279d1-5ad5-46a5-b469-4b8a552aa2fe

STEP 2: Checking if all users still have the source license and now also have the target license from target group. Check iteration: 1

MigrationUser@tailspinonline.com                Expected state: NO
MigrationUser2@tailspinonline.com               Expected state: NO

Total users checked: 2. In expected state: 0. Not yet: 2
Not all users are in expected state. Waiting 60 seconds to try again.

STEP 2: Checking if all users still have the source license and now also have the target license from target group. Check iteration: 2

MigrationUser@tailspinonline.com                Expected state: YES
MigrationUser2@tailspinonline.com               Expected state: YES

Total users checked: 2. In expected state: 2. Not yet: 0
Check passed for all users. Exiting check loop.

STEP 3: Removing users from the source group b82c04f0-ce30-4ff1-bac7-735d92d83036. This will remove the source license TailspinOnline:ENTERPRISEPACK.
Removing user MigrationUser@tailspinonline.com from group b82c04f0-ce30-4ff1-bac7-735d92d83036
Removing user MigrationUser2@tailspinonline.com from group b82c04f0-ce30-4ff1-bac7-735d92d83036

STEP 4: Checking if all users have source license removed and target license assigned from target group. Check iteration: 1

MigrationUser@tailspinonline.com                Expected state: NO
MigrationUser2@tailspinonline.com               Expected state: NO

Total users checked: 2. In expected state: 0. Not yet: 2
Not all users are in expected state. Waiting 60 seconds to try again.

STEP 4: Checking if all users have source license removed and target license assigned from target group. Check iteration: 2

MigrationUser@tailspinonline.com                Expected state: YES
MigrationUser2@tailspinonline.com               Expected state: YES

Total users checked: 2. In expected state: 2. Not yet: 0
Check passed for all users. Exiting check loop.
```

## <a name="migrate-users-between-products-that-have-conflicting-service-plans"></a>Kullanıcılar çakışan hizmet planları sahip ürünleri arasında geçirme
Geçiş kullanıcı lisanslarını değiştirmek için Grup tabanlı lisans kullanımını hedeftir bir *kaynak lisans* (Bu örnekte: Office 365 Kurumsal E1) için bir *hedef lisans* (Bu örnekte: Office 365 Kurumsal E3). Kullanıcıların sorunsuz bir şekilde geçirmek için çakışma olarak çözmek alacak şekilde çakışan hizmet planları, bu senaryoda iki ürün içerir. Bu çakışmaları hakkında daha fazla bilgi için bkz: [Active Directory Grup sorunu çözümleme Lisanslama: çakışan hizmet planları](https://docs.microsoft.com/azure/active-directory/active-directory-licensing-group-problem-resolution-azure-portal#conflicting-service-plans). Geçiş sırasında herhangi bir noktada kullanıcıları Hizmetleri ya da veri erişimi kaybedersiniz. Geçiş küçük "'toplu." gerçekleştirilir Her toplu işlemin sonucu doğrulayın ve işlemi sırasında oluşabilecek sorunları kapsamını en aza indirin. Genel olarak, işlem aşağıdaki gibidir:

1.  Kullanıcılar bir kaynak grubunun bir üyesidir ve bunlar devralır *kaynak lisans* o gruptan.

2.  Sahip bir hedef grup oluşturma *hedef lisans* ancak tüm üyeleri olmadan.

3.  Kullanıcıları toplu hedef grubuna ekleyin. Grup tabanlı lisans değişimi oluşturan seçer ve atamak çalışır *hedef lisans*. Bu iki ürün Hizmetleri'nde arasındaki çakışmaları nedeniyle ataması başarısız olur. Grup tabanlı lisans hatası her kullanıcı bir hata olarak kaydeder. İşlemi genişletilmiş bir toplu işlem ve diğer etkinlikleri Kiracı boyutuna bağlı olarak zaman miktarı alabilir.

4.  Grup tabanlı lisans kullanıcıları toplu tam olarak işlendiğini doğrulayın. Her kullanıcının kaydedilen çakışma hata olduğunu onaylayın. Bazı kullanıcılar bir beklenmeyen bir hata durumunda şunun kaydetmedi denetleyin. Hatalar hakkında daha fazla bilgi için bkz: [Active Directory Grup sorunu çözümleme lisans](https://docs.microsoft.com/azure/active-directory/active-directory-licensing-group-problem-resolution-azure-portal).

5.  Bu noktada, kullanıcıların çözümlenmedi *kaynak lisans* ve bir çakışma hata için *hedef lisans*. Kullanıcılara henüz yok *hedef lisans* atanmış.

6.  Aynı kullanıcı toplu kaynak grubundan kaldırın. Grup tabanlı lisans değişiklik yanıt verir ve *kaynak lisans* her kullanıcıdan kaldırılır. Çakışma hata (zaman hata katkı başka bir ürün lisans) aynı anda kaldırılır ve *hedef lisans* atanır. Bu işlem, geçiş sırasında hiçbir hizmet veya veri kaybı olmasını sağlar.

7.  Kullanıcılar sonraki toplu işlemi için bu işlemi yineleyin.

### <a name="migrate-a-single-user-by-using-the-azure-portal"></a>Tek bir kullanıcı Azure Portalı'nı kullanarak geçirme
Tek bir kullanıcı geçirmek nasıl basit bir anlatım budur.

**1. adım**: kullanıcının sahip olduğu bir *kaynak lisans* gruptan devralınan. Lisans için hiçbir doğrudan atamaları vardır:

![Bir kaynak lisans kullanıcıyla gruptan devralınan](media/active-directory-licensing-group-product-migration/UserWithSourceLicenseInheritedConflictScenario.png)

**2. adım**: kullanıcı hedef grubuna eklenir ve grup tabanlı lisans değişikliği işler. Kullanıcı hala olduğundan *kaynak lisans*, *hedef lisans* çakışma nedeniyle bir hata durumunda:

![Kullanıcı bir hata durumuna grubu ve hedef lisans devralınan kaynak lisansa sahip](media/active-directory-licensing-group-product-migration/UserWithSourceLicenseAndTargetLicenseInConflict.png)

**3. adım**: Kullanıcı kaynak grubundan kaldırılır ve grup tabanlı lisans değişikliği işler. *Hedef lisans* kullanıcıya uygulanan:

![Hedef lisansa sahip kullanıcı gruptan devralınan](media/active-directory-licensing-group-product-migration/UserWithTargetLicenseAssignedConflictScenario.png)


### <a name="automate-migration-by-using-azure-powershell"></a>Azure PowerShell kullanarak geçiş otomatikleştirme
Aşağıdaki kod parçacığında, büyük ölçekli işlemi için geçiş işlemini otomatikleştirmek gösterilmiştir.

> [!NOTE]
> Örnek kod dahil edilen PowerShell işlevlerini kullanan [son bölümü](#powershell-automation-of-migration-and-verification-steps) bu makalenin.

```
# A batch of users that we want to migrate in this iteration.
# The batch can be specified as an array of User Principal Names (string) or ObjectIds (Guid).
# Note: The batch can be loaded from a text file that represents a larger batch of users that we want to migrate.
[string[]]$usersToMigrate = 'MigrationUser@tailspinonline.com', 'MigrationUser2@tailspinonline.com'

############### CONFLICTING LICENSES SCENARIO ################

# The source group and source license to remove the user from.
$sourceGroupId = [Guid]'b82c04f0-ce30-4ff1-bac7-735d92d83036'
$sourceSkuId = 'TailspinOnline:STANDARDPACK'             # <-- This is the Office 365 Enterprise E1 product.

# The target group and target license to assign to the user.
$targetGroupId = [Guid]'bcf279d1-5ad5-46a5-b469-4b8a552aa2fe'
$targetSkuId = 'TailspinOnline:ENTERPRISEPACK'           # <-- This is the Office 365 Enterprise E3 product.

# Assumptions before migration:
# 1. Users are already in the source group and they have the source license assigned from that group.
# 2. Users don't have the same source license assigned from another group at the same time,
#    and they don't have the source license assigned directly.
#    IMPORTANT: If Assumption 2 isn't true, removing users from the source group in STEP 3
#               won't result in the target license being correctly applied.
# 3. There are enough available licenses to assign a target license to all of the users that are being migrated.
if(-Not (VerifyAssumptions $usersToMigrate $sourceGroupId $sourceSkuId $targetGroupId $targetSkuId))
{
    throw "Some users did not pass validation checks. See the output for details. Aborting migration process."
}

Write-Host "STEP 1: Adding users to the target group $targetGroupId. This will put target license $targetSkuId in conflict state with the source license $sourceSkuId"
AddUsersToGroup $usersToMigrate $targetGroupId

# Verify that the target license shows up in the conflict state for each user on the list.
# This step runs in a loop, forever, until all of the users are in the expected state.
# Group-based licensing (GBL) can take some time to reflect the changes on users.
# As a result, the loop should terminate after a period of time that's dependent on the size of the user collection.
# Note: If the loop hasn't terminated after a long period of time, stop the script.
#       Inspect the users that are reported as not yet in the expected state.
#       Verify that the users are not blocked for some other reason.
ExecuteVerificationLoop ${function:VerifySourceLicensePresentAndTargetLicenseInConflictState} 'STEP 2: Checking if all users still have the source license and are in conflict state for license from target group' $usersToMigrate $sourceGroupId $sourceSkuId $targetGroupId $targetSkuId

# Now it's safe to remove the users from the source group.
Write-Host "STEP 3: Removing users from the source group $sourceGroupId. This will remove the source license $sourceSkuId and remove the conflict on target license $targetSkuId which will become assigned."
RemoveUsersFromGroup $usersToMigrate $sourceGroupId

# Verify that the target license is now active on each user and the source license is removed.
ExecuteVerificationLoop ${function:VerifySourceLicenseRemovedAndTargetLicenseAssignedFromGroup} 'STEP 4: Checking if all users have source license removed and target license assigned from target group' $usersToMigrate $sourceGroupId $sourceSkuId $targetGroupId $targetSkuId
```

**Örnek çıktı (geçişi iki kullanıcı için)**

```
Verifying initial assumptions:
Enough TailspinOnline:ENTERPRISEPACK licenses available (61) for users: 2.
migrationuser@tailspinonline.com                OK
migrationuser2@tailspinonline.com               OK
Checks passed for all 2 users.
STEP 1: Adding users to the target group bcf279d1-5ad5-46a5-b469-4b8a552aa2fe. This will put target license TailspinOnline:ENTERPRISEPACK in conflict state with the source license TailspinOnline:STANDARDPACK
Adding user MigrationUser@tailspinonline.com to group bcf279d1-5ad5-46a5-b469-4b8a552aa2fe
Adding user MigrationUser2@tailspinonline.com to group bcf279d1-5ad5-46a5-b469-4b8a552aa2fe

STEP 2: Checking if all users still have the source license and are in conflict state for license from target group. Check iteration: 1

MigrationUser@tailspinonline.com                Expected state: NO
MigrationUser2@tailspinonline.com               Expected state: NO

Total users checked: 2. In expected state: 0. Not yet: 2
Not all users are in expected state. Waiting 60 seconds to try again.

STEP 2: Checking if all users still have the source license and are in conflict state for license from target group. Check iteration: 3

MigrationUser@tailspinonline.com                Expected state: YES
MigrationUser2@tailspinonline.com               Expected state: YES

Total users checked: 2. In expected state: 2. Not yet: 0
Check passed for all users. Exiting check loop.

STEP 3: Removing users from the source group b82c04f0-ce30-4ff1-bac7-735d92d83036. This will remove the source license TailspinOnline:STANDARDPACK and remove the conflict on target license TailspinOnline:ENTERPRISEPACK which will become assigned.
Removing user MigrationUser@tailspinonline.com from group b82c04f0-ce30-4ff1-bac7-735d92d83036
Removing user MigrationUser2@tailspinonline.com from group b82c04f0-ce30-4ff1-bac7-735d92d83036

STEP 4: Checking if all users have source license removed and target license assigned from target group. Check iteration: 1

MigrationUser@tailspinonline.com                Expected state: NO
MigrationUser2@tailspinonline.com               Expected state: NO

Total users checked: 2. In expected state: 0. Not yet: 2
Not all users are in expected state. Waiting 60 seconds to try again.

STEP 4: Checking if all users have source license removed and target license assigned from target group. Check iteration: 3

MigrationUser@tailspinonline.com                Expected state: YES
MigrationUser2@tailspinonline.com               Expected state: YES

Total users checked: 2. In expected state: 2. Not yet: 0
Check passed for all users. Exiting check loop.
```

<h2 id="powershell-automation-of-migration-and-verification-steps">Otomatikleştirmek ve geçişi doğrulama amacıyla PowerShell kodu yürütme</h2>

Bu bölümde, bu makalede açıklanan komut dosyalarını çalıştırmak için gerekli olan PowerShell kodunu içerir.

>[!WARNING]
>Bu kod örneği tanıtım amacıyla sağlanır. Ortamınızda kullanmak istiyorsanız, küçük bir ölçekte veya ayrı bir test Kiracı kodu ilk test göz önünde bulundurun. Ortamınıza özel ihtiyaçlarını karşılamak için kodu ayarlamanız gerekebilir.

Kod yürütmek için konusundaki yönergeleri kullanın [Azure AD PowerShell v1.0 kitaplıkları](https://docs.microsoft.com/powershell/azure/active-directory/install-msonlinev1?view=azureadps-1.0). Komut dosyasını çalıştırmadan önce çalıştırmak `connect-msolservice` cmdlet'ini Kiracı için oturum açın.

```
# BEGIN: Helper functions that are used in the scripts.

# GetUserObject function
# Retrieve a user object based on the ObjectId or UserPrincipalName.
function GetUserObject
{
    [OutputType([Microsoft.Online.Administration.User])]
    Param([object]$userId)

    $userIdType = $userId.GetType()

    if($userIdType -eq [Guid])
    {
        return Get-MsolUser -ObjectId $userId
    }
    elseif($userIdType -eq [string])
    {
        return Get-MsolUser -UserPrincipalName $userId
    }
    else
    {
        throw "User Id type must be a Guid or a string (UserPrincipalName). The user id type was: $($userIdType.Name)"
    }
}

# GetGuidUserId function
# Get a Guid objectId for a user, even when a UserPrincipal string is passed in.
# Guid ids are needed for group membership manipulation, where UPNs cannot be used.
function GetGuidUserId
{
    [OutputType([Guid])]
    Param([object]$userId)

    [Guid]$guidId = [Guid]::Empty
    if($userId.GetType() -eq [String])
    {
        $user = GetUserObject $userId
        return $user.ObjectId
    }
    elseif($userId.GetType() -eq [Guid])
    {
        return $userId
    }
    else
    {
        throw "UserId type must be a Guid or a string (UserPrincipalName). The user id type was: $($userId.GetType().Name)"
    }
}

# AddUsersToGroup function
# Add a collection of users to a group.
# Note: This function fails if a user is already a member of the specified group.
function AddUsersToGroup
{
    Param([object[]]$userIds, [Guid]$groupId)

    foreach($userId in $userIds)
    {
        $fetchId = GetGuidUserId $userId
        Write-Host "Adding user $userId to group $groupId"
        Add-MsolGroupMember -GroupObjectId $groupId -GroupMemberType User -GroupMemberObjectId $fetchId
    }
}

# RemoveUsersFromGroup function
# Remove a collection of users from a group.
# Note: This function fails if a user is not a member of the specified group.
function RemoveUsersFromGroup
{
    Param([object[]]$userIds, [Guid]$groupId)

    foreach($userId in $userIds)
    {
        $fetchId = GetGuidUserId $userId
        Write-Host "Removing user $userId from group $groupId"
        Remove-MsolGroupMember -GroupObjectId $groupId -GroupMemberType User -GroupMemberObjectId $fetchId
    }
}

# GetUserLicense function
# Return the license object that corresponds to the skuId.
# Return NULL if no object is found.
function GetUserLicense
{
    [OutputType([Microsoft.Online.Administration.UserLicense])]
    Param([Microsoft.Online.Administration.User]$user, [string]$skuId)

    # Look for the specific license SKU in all of the licenses that are assigned to the user.
    foreach($license in $user.Licenses)
    {
        if ($license.AccountSkuId -ieq $skuId)
        {
            return $license
        }
    }
    return $null
}

# IsLicenseAssignedToUser function
# Check if the specific SKU license is assigned to the user,
#    regardless of how the license is assigned (directly or via GBL).
function IsLicenseAssignedToUser
{
    [OutputType([bool])]
    Param([Microsoft.Online.Administration.User]$user,  [string]$skuId)

    $license = GetUserLicense $user $skuId

    return ($license -ne $null)
}

# GetObjectIdsAssigningLicense function
function GetObjectIdsAssigningLicense
{
    [OutputType([Guid[]])]
    Param([Microsoft.Online.Administration.User]$user, [string]$skuId)

    $license = GetUserLicense $user $skuId

    if ($license -ne $null)
    {
        return [Guid[]]$license.GroupsAssigningLicense
    }
    return [Array]::CreateInstance([Guid],0)
}

# UserHasLicenseAssignedFromThisGroup function
# Return TRUE if the user inherits the license from the specific group.
# Note: This function returns true only if the license is successfully assigned from the group.
#       If the license is in an error state, the function return false.
function UserHasLicenseAssignedFromThisGroup
{
    [OutputType([bool])]
    Param([Microsoft.Online.Administration.User]$user, [string]$skuId, [Guid]$groupId)

    [Guid[]]$objectsAssigningLicense = GetObjectIdsAssigningLicense $user $skuId

    # GroupsAssigningLicense contains a collection of object IDs for assigning the license.
    # This could be a group object or a user object (contrary to what the name suggests).
    foreach ($assignmentSource in $objectsAssigningLicense)
    {
        # If the collection contains at least one ID that doesn't match the user ID, the license is inherited from a group.
        # Note: The license might also be assigned directly, in addition to being inherited.
        if ($assignmentSource -ieq $groupId)
        {
            return $true
        }
    }
    return $false
}

# GetErrorsForLicense function
# Return error objects for a specific license.
function GetErrorsForLicense
{
    [OutputType([Microsoft.Online.Administration.IndirectLicenseError[]])]
    Param([Microsoft.Online.Administration.User]$user, [string]$skuId)

    [Microsoft.Online.Administration.IndirectLicenseError[]] $errorObjects = $user.IndirectLicenseErrors | Where {"$($_.AccountSku.AccountName):$($_.AccountSku.SkuPartNumber)" -ieq $skuId}

    if($errorObjects -eq $null)
    {
        #there are no errors at all
        return [Array]::CreateInstance([Microsoft.Online.Administration.IndirectLicenseError],0)
    }

    return $errorObjects
}

# GetErrorForLicenseFromGroup function
# Return an error label that's associated with a specific license that's inherited from a specific group.
# Return $null if there's no error.
function GetErrorForLicenseFromGroup
{
    [OutputType([string])]
    Param([Microsoft.Online.Administration.User]$user,  [Guid]$groupId, [string]$skuId)


    # There are some errors. Check if any of the errors are associated with the group.
    foreach($licenseError in GetErrorsForLicense $user $skuId)
    {
        if($licenseError.ReferencedObjectId -eq $groupId)
        {
            return $licenseError.Error
        }
    }
    return $null
}

# IsExpectedLicenseStateForGroup function
# Check if the license is in an expected state for a given group.
# If expectedError is set to a value, the function checks if the license is in the specific error state for the group.
# If expectedError is NULL, the function checks if the license is successfully assigned from the group.
function IsExpectedLicenseStateForGroup
{
    [OutputType([bool])]
    Param([Microsoft.Online.Administration.User]$user,  [Guid]$groupId, [string]$skuId, [string]$expectedError)

    # The license is expected to be fully assigned from the group and not in an error state.
    if([string]::IsNullOrEmpty($expectedError))
    {
        # Check if the assigned license is inherted from the expected group and without an error on it.
        return (UserHasLicenseAssignedFromThisGroup $user $skuId $groupId)
    }
    # The license is expected to be in the specific error state on the specific group.
    else
    {
        $error = GetErrorForLicenseFromGroup $user $groupId $skuId
        return ($error -ieq $expectedError)
    }
}

# VerifySourceLicensePresentAndTargetLicenseInConflictState function
# Detect if the licenses are in a specific state where the source license is assigned, but the target license is in a conflict state.
# Note: If the source license is gone, the function throws an exception to abort the script.
#       The conflict state can signify that something went wrong with the migration steps and the user lost access to services.
function VerifySourceLicensePresentAndTargetLicenseInConflictState
{
    [OutputType([bool])]
    Param([Microsoft.Online.Administration.User]$user,  [Guid]$sourceGroupId, [string]$sourceSkuId, [Guid]$targetGroupId, [string]$targetSkuId)

    # Check if the user still has the source license. If not, abort the script because something is seriously wrong.
    if(-Not (UserHasLicenseAssignedFromThisGroup $user $sourceSkuId $sourceGroupId))
    {
        throw "User $($user.UserPrincipalName) ($($user.ObjectId)) does not have the expected license $sourceSkuId from source group $sourceGroupId, which may result in loss of access and data. This is unexpected and should be investigated. Aborting execution."
    }
    # Check if the target license is in conflict, as expected.
    $conflictError = 'MutuallyExclusiveViolation'
    return (IsExpectedLicenseStateForGroup $user $targetGroupId $targetSkuId $conflictError)
}

# VerifySourceLicenseRemovedAndTargetLicenseAssignedFromGroup function
# Detect if the licenses are in a specific state where the source license isn't present,
#    but the target license is correctly assigned.
# Note: If the source license is gone and the target license isn't present,
#       the function throws an exception to abort the script.
#       Something went wrong and the user may have lost access to services.
function VerifySourceLicenseRemovedAndTargetLicenseAssignedFromGroup
{
    [OutputType([bool])]
    Param([Microsoft.Online.Administration.User]$user,  [Guid]$sourceGroupId, [string]$sourceSkuId, [Guid]$targetGroupId, [string]$targetSkuId)

    # Check if user has the source license completely removed, which is a prerequisite to the target license eventually kicking in.
    if(IsLicenseAssignedToUser $user $sourceSkuId)
    {
        return $false
    }

    # Check if the user has the target license at all. If not, abort the script because something is seriously wrong.
    if(-Not (IsLicenseAssignedToUser $user $targetSkuId))
    {
        throw "User $($user.UserPrincipalName) ($($user.ObjectId)) does not have the expected license $targetSkuId assigned, which may result in loss of access and data. This is unexpected and should be investigated. Aborting execution."
    }
    # Check if the target license is assigned from the expected target group, and no longer in an error state.
    return (IsExpectedLicenseStateForGroup $user $targetGroupId $targetSkuId $null)
}

# VerifySourceandTargetLicensePresent function
# Detect if the licenses are in the specific state where the source license is assigned and the target license is also assigned.
# Note: If the source license is gone, the function throws an exception to abort the script.
#       This state can signify that something went wrong with the migration steps and the user lost access to services.
function VerifySourceandTargetLicensePresent
{
    [OutputType([bool])]
    Param([Microsoft.Online.Administration.User]$user,  [Guid]$sourceGroupId, [string]$sourceSkuId, [Guid]$targetGroupId, [string]$targetSkuId)

    #check user still has source license - if not, abort because something is seriously wrong
    if(-Not (UserHasLicenseAssignedFromThisGroup $user $sourceSkuId $sourceGroupId))
    {
        throw "User $($user.UserPrincipalName) ($($user.ObjectId)) does not have the expected license $sourceSkuId from source group $sourceGroupId, which may result in loss of access and data. This is unexpected and should be investigated. Aborting execution."
    }
    #check if the target license is also assigned from the target group
    return (UserHasLicenseAssignedFromThisGroup $user $targetSkuId $targetGroupId)
}

# VerifyAssumptionsForUser function
# Verify basic assumptions that should be true for a user before we execute the migration process.
# The function prints details about the verification steps.
# Return TRUE if all of the assumptions are true.
function VerifyAssumptionsForUser
{
    [OutputType([bool])]
    Param([Microsoft.Online.Administration.User]$user,  [Guid]$sourceGroupId, [string]$sourceSkuId, [Guid]$targetGroupId, [string]$targetSkuId)

    $userName = $user.UserPrincipalName
    # 1. The user has the source license assigned from the source group.
    if(-Not (UserHasLicenseAssignedFromThisGroup $user $sourceSkuId $sourceGroupId))
    {
        Write-Host "$userName does not have source license $sourceSkuId assigned from source group $sourceGroupId."
        return $false
    }

    # 2. The user does't have the same source license assigned from another group at the same time,
    #    and the user doesn't have the source license assigned directly.
    [Guid[]]$otherObjectsAssigningLicense = GetObjectIdsAssigningLicense $user $sourceSkuId | Where {$_ -ne $sourceGroupId}
    foreach($otherObject in $otherObjectsAssigningLicense)
    {
        if($otherObject -eq $user.ObjectId)
        {
            Write-Host "$userName has source license assigned directly to the user."
        }
        else
        {
            Write-Host "$username has source license assigned from an additional unexpected group $otherObject."
        }
    }
    if($otherObjectsAssigningLicense -and $otherObjectsAssigningLicense.Count -gt 0)
    {
        return $false
    }

    # 3. The user doesn't have the target license assigned.
    if(IsLicenseAssignedToUser $user $targetSkuId)
    {
        Write-Host "$userName already has target license assigned."
        return $false
    }

    # 4. The user doesn't have the target license in an error state from some groups.
    [Microsoft.Online.Administration.IndirectLicenseError[]]$licenseErrors = GetErrorsForLicense $user $targetSkuId
    foreach($licenseError in $licenseErrors)
    {
        Write-Host "$userName has target license in error state $($licenseError.Error) from unexpected group $($licenseError.ReferencedObjectId)."
    }
    if($licenseErrors -and $licenseErrors.Count -gt 0)
    {
        return $false
    }

    Write-Host "$userName`t`tOK"
    return $true
}

# VerifyAssumptions function
# Check if all of the users to be migrated are in a correct state.
function VerifyAssumptions
{
    [OutputType([bool])]
    Param([object[]]$userIds, [Guid]$sourceGroupId, [string]$sourceSkuId, [Guid]$targetGroupId, [string]$targetSkuId)

    Write-Host "Verifying initial assumptions:"

    # Check if there are enough target licenses for all of the users.
    $skuState = Get-MsolAccountSku | Where {$_.AccountSkuId -ieq $targetSkuId}

    if($skuState -eq $null)
    {
        Write-Host "Target license SKU $targetSkuId does not exist in the tenant at all."
        return $false
    }

    $availableLicenses = $skuState.ActiveUnits - $skuState.ConsumedUnits

    if($userIds.Count -gt $availableLicenses)
    {
        Write-Host "Not enough licenses for all users. User count: $($userIds.Count), licenses available: $availableLicenses"
        return $false
    }
    else
    {
        Write-Host "Enough $targetSkuId licenses available ($availableLicenses) for users: $($userIds.Count)."
    }

    # Check if each user to be migrated is in an expected state.
    $usersOK = 0
    $usersNotOK = 0
    foreach($userId in $userIds)
    {
        $user = GetUserObject $userId
        if(VerifyAssumptionsForUser $user $sourceGroupId $sourceSkuId $targetGroupId $targetSkuId)
        {
            ++$usersOK
        }
        else
        {
            ++$usersNotOK
        }
    }
    if($usersNotOK -gt 0)
    {
        Write-Host "Checks passed for $usersOK users, but failed for $usersNotOK users."
        return $false
    }
    Write-Host "Checks passed for all $usersOK users."
    return $true
}

# ExecuteVerificationLoop function
# Execute a verification function (passed in as a delegate by using $checkFunction) for each user.
# The function tracks how many users passed/failed verification.
# The function repeats the verification loop until all of the users have passed the check.
#   The loop may never terminate if some users never reach the expected state.
#   If the loop doesn't terminate, you should investigate to determine the cause.
# Note: If the verification function fails with an exception,
#       such as the function detects an unexpected user state,
#       the loop terminates and investigation into the user state is needed.
function ExecuteVerificationLoop
{
    Param([System.Management.Automation.ScriptBlock]$checkFunction, [string]$consoleMessage, [object[]]$userIds,  [Guid]$sourceGroupId, [string]$sourceSkuId, [Guid]$targetGroupId, [string]$targetSkuId)

    # How long to wait until the loop is retried.
    $sleepIntervalSeconds = 60
    $retryIteration = 1

    While($true)
    {
        Write-Host "`n$consoleMessage. Check iteration: $retryIteration`n"

        $usersInExpectedState = 0
        $usersNotYet = 0

        foreach ($userId in $userIds)
        {
            $user = GetUserObject $userId
            if($checkFunction.InvokeReturnAsIs($user, $sourceGroupId, $sourceSkuId, $targetGroupId, $targetSkuId))
            {
                Write-Host "$userId`t`tExpected state: YES"
                ++$usersInExpectedState
            }
            else
            {
                Write-Host "$userId`t`tExpected state: NO"
                ++$usersNotYet
            }
        }

        Write-Host "`nTotal users checked: $($userIds.Count). In expected state: $usersInExpectedState. Not yet: $usersNotYet"

        if($usersNotYet -eq 0)
        {
            Write-Host "Check passed for all users. Exiting check loop."
            return
        }

        ++$retryIteration
        Write-Host "Not all users are in expected state. Waiting $sleepIntervalSeconds seconds to try again."
        Start-Sleep -Seconds $sleepIntervalSeconds
    }
}
# END: Helper functions that are used in the script.

# BEGIN: Execute the script.

# Enable strict execution mode.
Set-StrictMode -Version latest
# Stop the script when the first exception is thrown.
$ErrorActionPreference = "Stop"

# A batch of users that we want to migrate in this iteration.
# The batch can be specified as an array of User Principal Names (string) or ObjectIds (Guid).
# Note: The batch can be loaded from a text file that represents a larger batch of users that we want to migrate.
[string[]]$usersToMigrate = 'MigrationUser@tailspinonline.com', 'MigrationUser2@tailspinonline.com'

############### CONFLICTING LICENSES SCENARIO ################

# The source group and source license to remove the user from.
$sourceGroupId = [Guid]'b82c04f0-ce30-4ff1-bac7-735d92d83036'
$sourceSkuId = 'TailspinOnline:STANDARDPACK'             # <-- This is the Office 365 Enterprise E1 product.

# The target group and target license to assign to the user.
$targetGroupId = [Guid]'bcf279d1-5ad5-46a5-b469-4b8a552aa2fe'
$targetSkuId = 'TailspinOnline:ENTERPRISEPACK'           # <-- This is the Office 365 Enterprise E3 product.

# Assumptions before migration:
# 1. Users are already in the source group and they have the source license assigned from that group.
# 2. Users don't have the same source license assigned from another group at the same time,
#    and they don't have the source license assigned directly.
#    IMPORTANT: If Assumption 2 isn't true, removing users from the source group in STEP 3
#               won't result in the target license being correctly applied.
# 3. There are enough available licenses to assign a target license to all of the users that are being migrated.
if(-Not (VerifyAssumptions $usersToMigrate $sourceGroupId $sourceSkuId $targetGroupId $targetSkuId))
{
    throw "Some users did not pass validation checks. See the output for details. Aborting migration process."
}

Write-Host "STEP 1: Adding users to the target group $targetGroupId. This will put target license $targetSkuId in conflict state with the source license $sourceSkuId"
AddUsersToGroup $usersToMigrate $targetGroupId

# Verify that the target license shows up in the conflict state for each user on the list.
# This step runs in a loop, forever, until all of the users are in the expected state.
# Group-based licensing (GBL) can take some time to reflect the changes on users.
# As a result, the loop should terminate after a period of time that's dependent on the size of the user collection.
# Note: If the loop hasn't terminated after a long period of time, stop the script.
#       Inspect the users that are reported as not yet in the expected state.
#       Verify that the users are not blocked for some other reason.
ExecuteVerificationLoop ${function:VerifySourceLicensePresentAndTargetLicenseInConflictState} 'STEP 2: Checking if all users still have the source license and are in conflict state for license from target group' $usersToMigrate $sourceGroupId $sourceSkuId $targetGroupId $targetSkuId

# Now it's safe to remove the users from the source group.
Write-Host "STEP 3: Removing users from the source group $sourceGroupId. This will remove the source license $sourceSkuId and remove the conflict on target license $targetSkuId which will become assigned."
RemoveUsersFromGroup $usersToMigrate $sourceGroupId

# Verify that target license is now active on each user and the source license is removed.
ExecuteVerificationLoop ${function:VerifySourceLicenseRemovedAndTargetLicenseAssignedFromGroup} 'STEP 4: Checking if all users have source license removed and target license assigned from target group' $usersToMigrate $sourceGroupId $sourceSkuId $targetGroupId $targetSkuId

# END: Execute the script.
```

## <a name="next-steps"></a>Sonraki adımlar

Diğer senaryolar için aşağıdaki makalelerde grupları üzerinden lisans yönetimi hakkında bilgi edinin:

* [Tanımlamak ve Azure Active Directory'deki bir gruba lisans sorunlarını gidermek](active-directory-licensing-group-problem-resolution-azure-portal.md)
* [Azure Active Directory'de Grup tabanlı lisans için PowerShell örnekleri](active-directory-licensing-ps-examples.md)
* [Azure Active Directory grup tabanlı ilave senaryolar lisanslama](active-directory-licensing-group-advanced.md)
