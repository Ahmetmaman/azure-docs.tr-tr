---
title: Azure Active Directory rol atamak için bir grup oluşturun | Microsoft Docs
description: Azure AD 'de rol atanabilir bir grup oluşturmayı öğrenin. Azure portal, PowerShell veya Graph API Azure rollerini yönetin.
services: active-directory
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 07/27/2020
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3f14960f98a4257b0c8f920915327661cd8b867c
ms.sourcegitcommit: ce8eecb3e966c08ae368fafb69eaeb00e76da57e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/21/2020
ms.locfileid: "92310400"
---
# <a name="create-a-role-assignable-group-in-azure-active-directory"></a>Azure Active Directory rol atanabilir Grup oluşturma

Yalnızca ' ıttobletorole ' özelliği true olarak ayarlanmış bir gruba bir rol atayabilir ya da Azure AD portalında Azure **ad rolleri** ile oluşturulmuş bir gruba bir rol atanabilir. Bu Grup özniteliği, grubun Azure Active Directory (Azure AD) bir role atanabileceği bir grup oluşturur. Bu makalede, bu özel Grup türünün nasıl oluşturulacağı açıklanır. **Note:** Iastifbletorole özelliği true olarak ayarlanan bir grup dinamik üyelik türünde olamaz. Daha fazla bilgi için bkz. [Azure AD rol atamalarını yönetmek için Grup kullanma](./roles-groups-concept.md).

## <a name="using-azure-ad-admin-center"></a>Azure AD Yönetim Merkezi 'ni kullanma

1. Azure AD kuruluşunda ayrıcalıklı rol yöneticisi veya genel yönetici izinleriyle [Azure AD Yönetim merkezinde](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) oturum açın.
1. **Gruplar**  >  **tüm gruplar**  >  **Yeni Grup**' u seçin.

    [![Azure Active Directory açın ve yeni bir grup oluşturun.](media/roles-groups-create-eligible/new-group.png "Azure Active Directory açın ve yeni bir grup oluşturun.")](media/roles-groups-create-eligible/new-group.png#<lightbox>)

1. **Yeni Grup** sekmesinde grup türü, ad ve açıklama belirtin.
1. **Azure AD rollerini açmak gruba atanabilir**. Bu anahtar yalnızca ayrıcalıklı rol yöneticileri ve genel yöneticiler tarafından görülebilir çünkü bunlar, anahtarı ayarlayabilen yalnızca iki roldür.

    [![Yeni grubu rol ataması için uygun hale getirin](media/roles-groups-create-eligible/eligible-switch.png "Yeni grubu rol ataması için uygun hale getirin")](media/roles-groups-create-eligible/eligible-switch.png#<lightbox>)

1. Grubun üyelerini ve sahiplerini seçin. Ayrıca gruba rol atama seçeneğiniz de vardır, ancak burada bir rol atanması gerekmez.

    [![Rol atanabilir gruba üye ekleyin ve rolleri atayın.](media/roles-groups-create-eligible/specify-members.png "Rol atanabilir gruba üye ekleyin ve rolleri atayın.")](media/roles-groups-create-eligible/specify-members.png#<lightbox>)

1. Üyeler ve sahipler belirtilmişse **Oluştur**' u seçin.

    [![Oluştur düğmesi sayfanın en altında bulunur.](media/roles-groups-create-eligible/create-button.png "Oluştur düğmesi sayfanın en altında bulunur.")](media/roles-groups-create-eligible/create-button.png#<lightbox>)

Grup, kendisine atamış olabileceğiniz rollerle oluşturulur.

## <a name="using-powershell"></a>PowerShell'i kullanma

### <a name="install-the-azure-ad-preview-module"></a>Azure AD önizleme modülünü yükler

```powershell
install-module azureadpreview
import-module azureadpreview
```

Modülün kullanıma hazırsa emin olmak için aşağıdaki komutu verin:

```powershell
get-module azureadpreview
```

### <a name="create-a-group-that-can-be-assigned-to-role"></a>Role atanabilecek bir grup oluşturun

```powershell
$group = New-AzureADMSGroup -DisplayName "Contoso_Helpdesk_Administrators" -Description "This group is assigned to Helpdesk Administrator built-in role in Azure AD." -MailEnabled $true -SecurityEnabled $true -MailNickName "contosohelpdeskadministrators" -IsAssignableToRole $true
```

Bu grup türü için `isPublic` her zaman false olur ve `isSecurityEnabled` her zaman true olur.

### <a name="copy-one-groups-users-and-service-principals-into-a-role-assignable-group"></a>Bir grubun kullanıcılarını ve hizmet sorumlularını rol atanabilir bir gruba kopyalama

```powershell
#Basic set up
install-module azureadpreview
import-module azureadpreview
get-module azureadpreview

#Connect to Azure AD. Sign in as Privileged Role Administrator or Global Administrator. Only these two roles can create a role-assignable group.
Connect-AzureAD

#Input variabled: Existing group
$idOfExistingGroup = "14044411-d170-4cb0-99db-263ca3740a0c"

#Input variables: New role-assignable group
$groupName = "Contoso_Bellevue_Admins"
$groupDescription = "This group is assigned to Helpdesk Administrator built-in role in Azure AD."
$mailNickname = "contosobellevueadmins"

#Create new security group which is a role assignable group. For creating a Microsoft 365 group, set GroupTypes="Unified" and MailEnabled=$true
$roleAssignablegroup = New-AzureADMSGroup -DisplayName $groupName -Description $groupDescription -MailEnabled $false -MailNickname $mailNickname -SecurityEnabled $true -IsAssignableToRole $true

#Get details of existing group
$existingGroup = Get-AzureADMSGroup -Id $idOfExistingGroup
$membersOfExistingGroup = Get-AzureADGroupMember -ObjectId $existingGroup.Id

#Copy users and service principals from existing group to new group
foreach($member in $membersOfExistingGroup){
if($member.ObjectType -eq 'User' -or $member.ObjectType -eq 'ServicePrincipal'){
Add-AzureADGroupMember -ObjectId $roleAssignablegroup.Id -RefObjectId $member.ObjectId
}
}
```

## <a name="using-microsoft-graph-api"></a>Microsoft Graph API 'sini kullanma

### <a name="create-a-role-assignable-group-in-azure-ad"></a>Azure AD 'de rol atanabilir Grup oluşturma

```powershell
POST https://graph.microsoft.com/beta/groups
{
"description": "This group is assigned to Helpdesk Administrator built-in role of Azure AD.",
"displayName": "Contoso_Helpdesk_Administrators",
"groupTypes": [
"Unified"
],
"mailEnabled": true,
"securityEnabled": true
"mailNickname": "contosohelpdeskadministrators",
"isAssignableToRole": true,
}
```

Bu grup türü için `isPublic` her zaman false olur ve `isSecurityEnabled` her zaman true olur.

## <a name="next-steps"></a>Sonraki adımlar

- [Bulut grubuna rol atama](roles-groups-assign-role.md)
- [Rol atamalarını yönetmek için bulut gruplarını kullanma](roles-groups-concept.md)
- [Bulut gruplarına atanan rollerle ilgili sorunları giderme](roles-groups-faq-troubleshooting.md)