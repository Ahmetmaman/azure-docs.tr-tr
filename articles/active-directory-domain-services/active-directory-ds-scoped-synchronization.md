---
title: 'Azure Active Directory Domain Services: eşitleme kapsamı | Microsoft Docs'
description: Yönetilen etki alanlarınızı Azure AD'den kapsamlı eşitlemeyi yapılandırma
services: active-directory-ds
documentationcenter: ''
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: 9389cf0f-0036-4b17-95da-80838edd2225
ms.service: active-directory
ms.component: domains
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2018
ms.author: maheshu
ms.openlocfilehash: 1cbc434a91b7fb549fe465fe7e06b7bc5200a22d
ms.sourcegitcommit: 5b8d9dc7c50a26d8f085a10c7281683ea2da9c10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2018
ms.locfileid: "47181711"
---
# <a name="configure-scoped-synchronization-from-azure-ad-to-your-managed-domain"></a>Yönetilen etki alanınızı Azure AD'den kapsamlı eşitlemeyi yapılandırma
Bu makalede, yalnızca belirli kullanıcı hesaplarını Azure AD dizininizi Azure AD Domain Services yönetilen Etki Alanınızla eşitlenmek üzere yapılandırma işlemini göstermektedir.


## <a name="group-based-scoped-synchronization"></a>Grup tabanlı kapsamlı eşitleme
Varsayılan olarak, yönetilen Etki Alanınızla tüm kullanıcıları ve grupları Azure AD dizininizdeki eşitlenir. Yalnızca bazı kullanıcılar yönetilen etki alanını kullan, yalnızca bu kullanıcı hesaplarını eşitlemek. Grup tabanlı kapsamlı eşitleme bunu yapmanızı sağlar. Yapılandırıldığında, yalnızca kullanıcı hesapları, belirttiğiniz bir gruba ait olan yönetilen etki alanına eşitlenir.

Aşağıdaki tabloda nasıl kapsamlı eşitleme kullanılacağını belirlemenize yardımcı olur:

| **Geçerli durumu** | **İstenen durum** | **Gerekli yapılandırma** |
| --- | --- | --- |
| Mevcut yönetilen etki alanınıza, tüm kullanıcı hesapları ve grupları eşitlemek için yapılandırılır. | Yönetilen etki alanınıza belirli gruplara ait kullanıcı hesaplarını eşitlemek istediğiniz. | [Mevcut yönetilen etki alanını Sil](active-directory-ds-disable-aadds.md). Ardından, yeniden yapılandırılmış kapsamlı eşitleme ile oluşturmak için bu makaledeki yönergeleri izleyin. |
| Mevcut bir yönetilen etki alanına sahip değilsiniz. | Yeni bir yönetilen etki alanı oluşturmak ve yalnızca belirli gruplara ait kullanıcı hesaplarını eşitlemek istediğiniz. | Kapsamlı eşitleme ile yapılandırılmış yeni bir yönetilen etki alanı oluşturmak için bu makaledeki yönergeleri izleyin. |
| Mevcut yönetilen etki alanınıza yalnızca belirli gruplara ait hesaplarını eşitlemek üzere yapılandırılır. | Kullanıcıları Yönet etki eşitlenmesi gerektiğini grupları listesini değiştirmek istediğiniz. | Kapsamlı eşitleme değiştirmek için bu makaledeki yönergeleri izleyin. |

> [!WARNING]
> **Eşitleme kapsamı değiştirmek, yeniden eşitleme gitmek yönetilen etki alanınızı neden olur.**
>
 * Yönetilen bir etki alanı için eşitleme kapsamı değiştirdiğinizde, bir tam eşitleme gerçekleşir.
 * Yönetilen etki alanında artık gerekli olan nesneler silinir. Yeni nesneler yönetilen etki alanında oluşturulur.
 * Yeniden eşitleme, yönetilen etki alanınızı ve Azure AD dizininizi nesne (kullanıcılar, gruplar ve grup üyelikleri) sayısına bağlı olarak tamamlanması uzun sürebilir. Yüz binlerce nesne içeren büyük dizinler için yeniden eşitleme birkaç gün sürebilir.
>
>


## <a name="create-a-new-managed-domain-and-enable-group-based-scoped-synchronization"></a>Yeni bir yönetilen etki alanı ve grup tabanlı kapsamlı eşitlemeyi etkinleştirme
Bu adım kümesini tamamlamak için PowerShell kullanın. Yönergelere bakın [Azure Active Directory etki alanı PowerShell kullanarak Services'i etkinleştirme](active-directory-ds-enable-using-powershell.md). Bu makaledeki adımlarda birkaç değiştirildiğinde biraz daha kapsamlı eşitleme yapılandırılamadı.

Grup tabanlı kapsamlı eşitleme yönetilen etki alanınıza yapılandırmak için aşağıdaki adımları tamamlayın:

1. Aşağıdaki görevleri tamamlayın:
  * [1. Görev: gerekli PowerShell modüllerini yükleyin](active-directory-ds-enable-using-powershell.md#task-1-install-the-required-powershell-modules).
  * [2. Görev: Azure AD dizininizde gerekli hizmet sorumlusu oluşturma](active-directory-ds-enable-using-powershell.md#task-2-create-the-required-service-principal-in-your-azure-ad-directory).
  * [3. Görev: Oluşturma ve yapılandırma 'AAD DC Administrators' grubunun](active-directory-ds-enable-using-powershell.md#task-3-create-and-configure-the-aad-dc-administrators-group).
  * [4. Görev: Azure AD Domain Services kaynak sağlayıcısını kaydedin](active-directory-ds-enable-using-powershell.md#task-4-register-the-azure-ad-domain-services-resource-provider).
  * [5. Görev: bir kaynak grubu oluşturma](active-directory-ds-enable-using-powershell.md#task-5-create-a-resource-group).
  * [6. Görev: Oluşturma ve sanal ağ yapılandırma](active-directory-ds-enable-using-powershell.md#task-6-create-and-configure-the-virtual-network).

2. Yönetilen etki alanınızla eşitlenmesini istediğiniz grupların görünen adını belirtin ve istediğiniz grupları seçin.

3. Kaydet [betik aşağıdaki bölümdeki](active-directory-ds-scoped-synchronization.md#script-to-select-groups-to-synchronize-to-the-managed-domain-select-groupstosyncps1) adlı bir dosyaya ```Select-GroupsToSync.ps1```. Betik yürütme aşağıdaki gibi:

  ```powershell
  .\Select-GroupsToSync.ps1 -groupsToAdd @("AAD DC Administrators", “GroupName1”, “GroupName2”)
  ```

  > [!WARNING]
  > **'AAD DC Administrators' grubuna eklemeyi unutmayın.**
  >
  > Kapsamlı eşitleme için yapılandırılan Grup listesinde 'AAD DC Administrators' grubuna eklemeniz gerekir. Bu grup eklemezseniz, yönetilen etki alanında kullanılamaz.
  >

4. Şimdi, yönetilen etki alanı oluşturun ve grup tabanlı kapsamlı eşitleme yönetilen etki alanı için etkinleştirin. Özelliği ```"filteredSync" = "Enabled"``` içinde ```Properties``` parametresi. Kopyalama kaynağı aşağıdaki kod parçası, örneği için bkz: [görev 7: Azure AD Domain Services yönetilen etki alanı sağlama](active-directory-ds-enable-using-powershell.md#task-7-provision-the-azure-ad-domain-services-managed-domain).

  ```powershell
  $AzureSubscriptionId = "YOUR_AZURE_SUBSCRIPTION_ID"
  $ManagedDomainName = "contoso100.com"
  $ResourceGroupName = "ContosoAaddsRg"
  $VnetName = "DomainServicesVNet_WUS"
  $AzureLocation = "westus"

  # Enable Azure AD Domain Services for the directory.
  New-AzureRmResource -ResourceId "/subscriptions/$AzureSubscriptionId/resourceGroups/$ResourceGroupName/providers/Microsoft.AAD/DomainServices/$ManagedDomainName" `
  -Location $AzureLocation `
  -Properties @{"DomainName"=$ManagedDomainName; "filteredSync" = "Enabled"; `
    "SubnetId"="/subscriptions/$AzureSubscriptionId/resourceGroups/$ResourceGroupName/providers/Microsoft.Network/virtualNetworks/$VnetName/subnets/DomainServices"} `
  -ApiVersion 2017-06-01 -Force -Verbose
  ```

  > [!TIP]
  > Eklenecek unutmadığınızdan ```"filteredSync" = "Enabled"``` içinde ```-Properties``` parametresi, yönetilen etki alanı için kapsamlı eşitleme etkin.


## <a name="script-to-select-groups-to-synchronize-to-the-managed-domain-select-groupstosyncps1"></a>Komut (Select-GroupsToSync.ps1) yönetilen etki alanı Eşitleme gruplarını seçmek için
Aşağıdaki betiği bir dosyaya kaydet (```Select-GroupsToSync.ps1```). Bu betik, Azure AD Domain Services yönetilen etki alanında seçilen grupları eşitlemek için yapılandırır. Belirtilen gruba ait olan tüm kullanıcı hesapları için yönetilen etki alanı eşitlenecektir.

```powershell
param (
    [Parameter(Position = 0)]
    [String[]]$groupsToAdd
)

Connect-AzureAD
$sp = Get-AzureADServicePrincipal -Filter "AppId eq '2565bd9d-da50-47d4-8b85-4c97f669dc36'"
$role = $sp.AppRoles | where-object -FilterScript {$_.DisplayName -eq "User"}

Write-Output "`n****************************************************************************"

Write-Output "Total group-assignments need to be added: $($groupsToAdd.Count)"
$newGroupIds = New-Object 'System.Collections.Generic.HashSet[string]'
foreach ($groupName in $groupsToAdd)
{
    try
    {
        $group = Get-AzureADGroup -Filter "DisplayName eq '$groupName'"
        $newGroupIds.Add($group.ObjectId)

        Write-Output "Group-Name: $groupName, Id: $($group.ObjectId)"
    }
    catch
    {
        Write-Error "Failed to find group: $groupName. Exception: $($_.Exception)."
    }
}

Write-Output "****************************************************************************`n"
Write-Output "`n****************************************************************************"

$currentAssignments = Get-AzureADServiceAppRoleAssignment -ObjectId $sp.ObjectId
Write-Output "Total current group-assignments: $($currentAssignments.Count), SP-ObjectId: $($sp.ObjectId)"

$currAssignedObjectIds = New-Object 'System.Collections.Generic.HashSet[string]'
foreach ($assignment in $currentAssignments)
{
    Write-Output "Assignment-ObjectId: $($assignment.PrincipalId)"

    if ($newGroupIds.Contains($assignment.PrincipalId) -eq $false)
    {
        Write-Output "This assignment is not needed anymore. Removing it! Assignment-ObjectId: $($assignment.PrincipalId)"
        Remove-AzureADServiceAppRoleAssignment -ObjectId $sp.ObjectId -AppRoleAssignmentId $assignment.ObjectId
    }
    else
    {
        $currAssignedObjectIds.Add($assignment.PrincipalId)
    }
}

Write-Output "****************************************************************************`n"
Write-Output "`n****************************************************************************"

foreach ($id in $newGroupIds)
{
    try
    {
        if ($currAssignedObjectIds.Contains($id) -eq $false)
        {
            Write-Output "Adding new group-assignment. Role-Id: $($role.Id), Group-Object-Id: $id, ResourceId: $($sp.ObjectId)"
            New-AzureADGroupAppRoleAssignment -Id $role.Id -ObjectId $id -PrincipalId $id -ResourceId $sp.ObjectId
        }
        else
        {
            Write-Output "Group-ObjectId: $id is already assigned."
        }
    }
    catch
    {
        Write-Error "Exception occured assigning Object-ID: $id. Exception: $($_.Exception)."
    }
}

Write-Output "****************************************************************************`n"
```


## <a name="modify-group-based-scoped-synchronization"></a>Grup tabanlı kapsamlı eşitleme değiştirme
Olan kullanıcılar yönetilen Etki Alanınızla eşitlenmesi gerektiğini grupları listesini değiştirmek için yeniden çalıştırma [PowerShell Betiği](active-directory-ds-scoped-synchronization.md#script-to-select-groups-to-synchronize-to-the-managed-domain-select-groupstosyncps1) ve yeni grup listesini belirtin. 'AAD DC Administrators' grubunun her zaman bu listede belirtmeyi unutmayın.

> [!WARNING]
> **'AAD DC Administrators' grubuna eklemeyi unutmayın.**
>
> Kapsamlı eşitleme için yapılandırılan Grup listesinde 'AAD DC Administrators' grubuna eklemeniz gerekir. Bu grup eklemezseniz, yönetilen etki alanında kullanılamaz.
>


## <a name="disable-group-based-scoped-synchronization"></a>Grup tabanlı kapsamlı eşitleme devre dışı bırak
Grup tabanlı devre dışı bırakmak için aşağıdaki PowerShell betiğini kullanın, yönetilen etki alanınız için eşitleme kapsamı:

```powershell
// Login to your Azure AD tenant
Login-AzureRmAccount

// Retrieve the Azure AD Domain Services resource.
$DomainServicesResource = Get-AzureRmResource -ResourceType "Microsoft.AAD/DomainServices"

// Disable group-based scoped synchronization.
$disableScopedSync = @{"filteredSync" = "Disabled"}

Set-AzureRmResource -Id $DomainServicesResource.ResourceId -Properties $disableScopedSync
```

## <a name="next-steps"></a>Sonraki adımlar
* [Azure AD Etki Alanı Hizmetleri'nde eşitleme anlama](active-directory-ds-synchronization.md)
* [Azure Active Directory etki alanı PowerShell kullanarak Services'i etkinleştirme](active-directory-ds-enable-using-powershell.md)
