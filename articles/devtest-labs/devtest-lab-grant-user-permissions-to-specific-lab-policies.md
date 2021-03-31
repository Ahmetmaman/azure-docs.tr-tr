---
title: Belirli laboratuvar ilkelerine Kullanıcı izinleri verme | Microsoft Docs
description: Her kullanıcının ihtiyaçlarına bağlı olarak DevTest Labs 'deki belirli laboratuvar ilkelerine Kullanıcı izinleri verme hakkında bilgi edinin
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: 976862476d25e4e9a4933d8a5319eec9d77ca39b
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "92328479"
---
# <a name="grant-user-permissions-to-specific-lab-policies"></a>Belirli laboratuvar ilkelerine Kullanıcı izinleri verme
## <a name="overview"></a>Genel Bakış
Bu makalede, PowerShell kullanarak kullanıcılara belirli bir laboratuvar ilkesi için izinler verme konusu gösterilmektedir. Bu şekilde, izinler her kullanıcının ihtiyaçlarına göre uygulanabilir. Örneğin, belirli bir kullanıcıya VM ilkesi ayarlarını değiştirme yeteneği vermek isteyebilirsiniz, ancak maliyet ilkelerini etkilemez.

## <a name="policies-as-resources"></a>Kaynaklar olarak ilkeler
Azure RBAC, [Azure rol tabanlı erişim denetimi (Azure RBAC)](../role-based-access-control/role-assignments-portal.md) makalesinde anlatıldığı gibi, Azure için kaynakların ayrıntılı erişim yönetimine izin verebilir. Azure RBAC kullanarak, DevOps ekibinizdeki görevleri ayırabilirsiniz ve yalnızca işlerini gerçekleştirmesi için gereken kullanıcılara erişim miktarını verebilirsiniz.

DevTest Labs 'de, ilke **Microsoft. DevTestLab/Labs/policySets/policies/** Azure RBAC eylemini sağlayan bir kaynak türüdür. Her laboratuvar ilkesi, Ilke kaynak türündeki bir kaynaktır ve bir Azure rolüne kapsam olarak atanabilir.

Örneğin, **Izin VERILEN VM boyutları** ilkesine kullanıcılara okuma/yazma izni vermek Için, **Microsoft. devtestlab/Labs/policysets/policies/** Action ile çalışan özel bir rol oluşturursunuz ve ardından **Microsoft. Devtestlab/Labs/Policysets/policies/AllowedVmSizesInLab** kapsamındaki bu özel role uygun kullanıcıları atamalısınız.

Azure RBAC içindeki özel roller hakkında daha fazla bilgi edinmek için bkz. [Azure özel rolleri](../role-based-access-control/custom-roles.md).

## <a name="creating-a-lab-custom-role-using-powershell"></a>PowerShell kullanarak laboratuvar özel rolü oluşturma
Başlamak için [Azure PowerShell yüklemeniz](/powershell/azure/install-az-ps)gerekir. 

Azure PowerShell cmdlet 'lerini ayarladıktan sonra, aşağıdaki görevleri gerçekleştirebilirsiniz:

* Bir kaynak sağlayıcısı için tüm işlemleri/eylemleri listeleme
* Belirli bir roldeki eylemleri listeleyin:
* Özel rol oluşturma

Aşağıdaki PowerShell betiği, bu görevlerin nasıl gerçekleştirileceğini gösteren örnekler gösterir:

```azurepowershell
# List all the operations/actions for a resource provider.
Get-AzProviderOperation -OperationSearchString "Microsoft.DevTestLab/*"

# List actions in a particular role.
(Get-AzRoleDefinition "DevTest Labs User").Actions

# Create custom role.
$policyRoleDef = (Get-AzRoleDefinition "DevTest Labs User")
$policyRoleDef.Id = $null
$policyRoleDef.Name = "Policy Contributor"
$policyRoleDef.IsCustom = $true
$policyRoleDef.AssignableScopes.Clear()
$policyRoleDef.AssignableScopes.Add("/subscriptions/<SubscriptionID> ")
$policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/policySets/policies/*")
$policyRoleDef = (New-AzRoleDefinition -Role $policyRoleDef)
```

## <a name="assigning-permissions-to-a-user-for-a-specific-policy-using-custom-roles"></a>Özel rolleri kullanarak belirli bir ilke için kullanıcıya izin atama
Özel rollerinizi tanımladıktan sonra kullanıcılara atayabilirsiniz. Bir kullanıcıya özel bir rol atamak için, önce bu kullanıcıyı temsil eden **ObjectID** 'yi edinmeniz gerekir. Bunu yapmak için **Get-AzADUser** cmdlet 'ini kullanın.

Aşağıdaki örnekte, *Someuser* kullanıcısının **ObjectID** 17DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3 ' dir.

```azurepowershell
PS C:\>Get-AzADUser -SearchString "SomeUser"

DisplayName                    Type                           ObjectId
-----------                    ----                           --------
someuser@hotmail.com                                          05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3
```

Kullanıcı için **ObjectID** ve özel bir rol adı olduktan sonra, bu rolü **New-azroleatama** cmdlet 'i ile kullanıcıya atayabilirsiniz:

```azurepowershell
PS C:\>New-AzRoleAssignment -ObjectId 05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3 -RoleDefinitionName "Policy Contributor" -Scope /subscriptions/<SubscriptionID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.DevTestLab/labs/<LabName>/policySets/default/policies/AllowedVmSizesInLab
```

Önceki örnekte, **AllowedVmSizesInLab** ilkesi kullanılır. Aşağıdaki ilkeleri kullanabilirsiniz:

* MaxVmsAllowedPerUser
* MaxVmsAllowedPerLab
* AllowedVmSizesInLab
* LabVmsShutdown

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Sonraki adımlar
Belirli laboratuvar ilkelerine Kullanıcı izinleri verdikten sonra, göz önünde bulundurmanız gereken birkaç adım aşağıda verilmiştir:

* [Laboratuvara erişimin güvenliğini sağlama](devtest-lab-add-devtest-user.md)
* [Laboratuvar ilkeleri belirleme](devtest-lab-set-lab-policy.md)
* [Laboratuvar şablonu oluşturma](devtest-lab-create-template.md)
* [VM'leriniz için özel yapıtlar oluşturma](devtest-lab-artifact-author.md)
* [VM'yi bir laboratuvara ekleme](devtest-lab-add-vm.md)
