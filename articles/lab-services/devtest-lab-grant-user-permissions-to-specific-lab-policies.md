---
title: Özel Laboratuvar ilkelerini kullanıcı izin verme | Microsoft Docs
description: DevTest Labs her kullanıcının ihtiyaçlarına göre özel Laboratuvar ilkelerini kullanıcı izinleri verme konusunda bilgi edinin
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: 5ca829f0-eb69-40a1-ae26-03a629db1d7e
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: spelluru
ms.openlocfilehash: 2b81c23b5cf9ea5d4bfc47d36ae251f762ffad11
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38539699"
---
# <a name="grant-user-permissions-to-specific-lab-policies"></a>Özel Laboratuvar ilkelerini kullanıcı izinleri verme
## <a name="overview"></a>Genel Bakış
Bu makalede belirli Laboratuvar ilkesini kullanıcılara izinler vermek için PowerShell kullanılması gösterilmektedir. Bu şekilde, izinleri her kullanıcının ihtiyaçlarına göre uygulanabilir. Örneğin, belirli bir kullanıcı VM ilke ayarları, ancak maliyet ilkeleri değiştirme olanağı vermek isteyebilirsiniz.

## <a name="policies-as-resources"></a>İlkeleri kaynakları olarak
Bölümünde açıklandığı gibi [Azure rol tabanlı erişim denetimi](../role-based-access-control/role-assignments-portal.md) makalede RBAC kaynakların Azure için ayrıntılı erişim yönetimi sağlar. RBAC kullanarak, DevOps, görevleri ayırabilir ve işlerini yapmak için ihtiyaçları olan kullanıcılara sadece erişim miktarını verin.

DevTest Labs'de RBAC eylem sağlayan bir kaynak türünü ilkedir **Microsoft.DevTestLab/labs/policySets/policies/**. Her bir laboratuvar İlkesi İlkesi kaynak türündeki bir kaynaktır ve RBAC rolü için kapsam olarak atanabilir.

Örneğin, kullanıcıların okuma/yazma izni vermek için **izin verilen VM boyutları** İlkesi ile çalışır, özel bir rol oluşturmanız **Microsoft.DevTestLab/labs/policySets/policies/*** Eylem ve ardından uygun kullanıcıları bu özel rolü kapsamında atayın **Microsoft.DevTestLab/labs/policySets/policies/AllowedVmSizesInLab**.

Özel RBAC rolleri hakkında daha fazla bilgi için bkz: [özel roller erişim denetimi](../role-based-access-control/custom-roles.md).

## <a name="creating-a-lab-custom-role-using-powershell"></a>PowerShell kullanarak Laboratuvar özel rol oluşturma
Başlamak için yükleme ve Azure PowerShell cmdlet'lerini yapılandırma açıklayacak şu makaleyi okuyun gerekecektir: [ https://azure.microsoft.com/blog/azps-1-0-pre ](https://azure.microsoft.com/blog/azps-1-0-pre).

Azure PowerShell cmdlet'leri ayarladıktan sonra aşağıdaki görevleri gerçekleştirebilirsiniz:

* Bir kaynak sağlayıcısı için tüm işlemler/eylemler listesi
* Belirli bir roldeki eylemler listesi:
* Özel rol oluşturma

Aşağıdaki PowerShell Betiği örnekleri bu görevlerin nasıl gerçekleştirileceği gösterilmektedir:

    ‘List all the operations/actions for a resource provider.
    Get-AzureRmProviderOperation -OperationSearchString "Microsoft.DevTestLab/*"

    ‘List actions in a particular role.
    (Get-AzureRmRoleDefinition "DevTest Labs User").Actions

    ‘Create custom role.
    $policyRoleDef = (Get-AzureRmRoleDefinition "DevTest Labs User")
    $policyRoleDef.Id = $null
    $policyRoleDef.Name = "Policy Contributor"
    $policyRoleDef.IsCustom = $true
    $policyRoleDef.AssignableScopes.Clear()
    $policyRoleDef.AssignableScopes.Add("/subscriptions/<SubscriptionID> ")
    $policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/policySets/policies/*")
    $policyRoleDef = (New-AzureRmRoleDefinition -Role $policyRoleDef)

## <a name="assigning-permissions-to-a-user-for-a-specific-policy-using-custom-roles"></a>Bir kullanıcıya özel rolleri kullanarak belirli bir ilke için izinler atama
Özel rollerinizi tanımladınız sonra bunları kullanıcılara atayabilirsiniz. Bir kullanıcıya özel bir rol atamak için önce edinmeniz gerekir **objectID** o bir kullanıcıyı temsil eden. Bunu yapmak için kullanın **Get-AzureRmADUser** cmdlet'i.

Aşağıdaki örnekte, **objectID** , *SomeUser* 05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3 kullanıcıdır.

    PS C:\>Get-AzureRmADUser -SearchString "SomeUser"

    DisplayName                    Type                           ObjectId
    -----------                    ----                           --------
    someuser@hotmail.com                                          05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3

Yapılandırmasını tamamladıktan **objectID** kullanıcı ve bir özel rol adı için kullanıcı bu rolü atayabilirsiniz **New-AzureRmRoleAssignment** cmdlet:

    PS C:\>New-AzureRmRoleAssignment -ObjectId 05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3 -RoleDefinitionName "Policy Contributor" -Scope /subscriptions/<SubscriptionID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.DevTestLab/labs/<LabName>/policySets/default/policies/AllowedVmSizesInLab

Önceki örnekte, **AllowedVmSizesInLab** İlkesi kullanılır. Herhangi birini kullanabilmeniz için ilkeler:

* MaxVmsAllowedPerUser
* MaxVmsAllowedPerLab
* AllowedVmSizesInLab
* LabVmsShutdown

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Sonraki adımlar
Bir kez dikkate alınması gereken sonraki adımlardan birkaçı şunlardır belirli Laboratuvar ilkeleri, kullanıcı izinlerini verdiyseniz:

* [Laboratuvara erişimin güvenliğini sağlama](devtest-lab-add-devtest-user.md)
* [Laboratuvar ilkeleri belirleme](devtest-lab-set-lab-policy.md)
* [Laboratuvar şablonu oluşturma](devtest-lab-create-template.md)
* [VM'leriniz için özel yapıtlar oluşturma](devtest-lab-artifact-author.md)
* [VM'yi bir laboratuvara ekleme](devtest-lab-add-vm.md)

