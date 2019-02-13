---
title: 'Öğretici: RBAC ve PowerShell kullanarak bir kullanıcıya erişim izni verme | Microsoft Docs'
description: Bir kullanıcıya bir abonelik içindeki her şeyi görüntüleme izni vermek için rol tabanlı erişim denetimini (RBAC), bir kaynak grubundaki her şeyi yönetme izni vermek için de Azure PowerShell'i kullanın.
services: active-directory
documentationCenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: role-based-access-control
ms.devlang: ''
ms.topic: tutorial
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 02/02/2019
ms.author: rolyon
ms.openlocfilehash: 7f5e67edd2a5300ae1898e2620ee8ace80ac6d92
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2019
ms.locfileid: "56109036"
---
# <a name="tutorial-grant-access-for-a-user-using-rbac-and-azure-powershell"></a>Öğretici: RBAC ve Azure PowerShell kullanarak bir kullanıcı için erişim izni ver

[Rol tabanlı erişim denetimi (RBAC)](overview.md), Azure'daki kaynaklara erişimi yönetmek için kullanılan sistemdir. Bu öğreticide bir kullanıcıya bir abonelik içindeki her şeyi görüntüleme ve bir kaynak grubundaki her şeyi yönetme izni vermek için Azure PowerShell'i kullanacaksınız.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir kullanıcıya farklı kapsamlarda erişim izni verme
> * Erişimi listeleme
> * Erişimi kaldırma

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [az-powershell-update](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

- Azure Active Directory'de kullanıcı oluşturma izni (veya mevcut bir kullanıcı)
- [Azure Cloud Shell](/azure/cloud-shell/quickstart-powershell)

## <a name="role-assignments"></a>Rol atamaları

RBAC'de erişim vermek için bir rol ataması oluşturmanız gerekir. Rol ataması üç öğeden oluşur: güvenlik sorumlusu, rol tanımı ve kapsam. Bu öğreticide gerçekleştireceğiniz iki rol ataması aşağıda verilmiştir:

| Güvenlik sorumlusu | Rol tanımı | Kapsam |
| --- | --- | --- |
| Kullanıcı<br>(RBAC Tutorial User) | [Okuyucu](built-in-roles.md#reader) | Abonelik |
| Kullanıcı<br>(RBAC Tutorial User)| [Katılımcı](built-in-roles.md#contributor) | Kaynak grubu<br>(rbac-tutorial-resource-group) |

   ![Bir kullanıcıya rol atama](./media/tutorial-role-assignments-user-powershell/rbac-role-assignments-user.png)

## <a name="create-a-user"></a>Kullanıcı oluşturma

Rol atamak için kullanıcı, grup veya hizmet sorumlu gerekir. Kullanıcınız yoksa bir tane oluşturabilirsiniz.

1. Azure Cloud Shell'de parola karmaşıklık gereksinimlerinize uygun bir parola oluşturun.

    ```azurepowershell
    $PasswordProfile = New-Object -TypeName Microsoft.Open.AzureAD.Model.PasswordProfile
    $PasswordProfile.Password = "Password"
    ```

1. [New-AzureADUser](/powershell/module/azuread/new-azureaduser) komutunu kullanarak etki alanınızda yeni bir kullanıcı oluşturun.

    ```azurepowershell
    New-AzureADUser -DisplayName "RBAC Tutorial User" -PasswordProfile $PasswordProfile `
      -UserPrincipalName "rbacuser@example.com" -AccountEnabled $true -MailNickName "rbacuser"
    ```
    
    ```Example
    ObjectId                             DisplayName        UserPrincipalName    UserType
    --------                             -----------        -----------------    --------
    11111111-1111-1111-1111-111111111111 RBAC Tutorial User rbacuser@example.com Member
    ```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Kaynak grubu kapsamında rol atamasını göstermek için bir kaynak grubu kullanmanız gerekir.

1. Kullanarak bölge konumların bir listesini alın [Get-AzLocation](/powershell/module/az.resources/get-azlocation) komutu.

   ```azurepowershell
   Get-AzLocation | select Location
   ```

1. Size yakın bir konum seçip bir değişkene atayın.

   ```azurepowershell
   $location = "westus"
   ```

1. Kullanarak yeni bir kaynak grubu oluşturma [yeni AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) komutu.

   ```azurepowershell
   New-AzResourceGroup -Name "rbac-tutorial-resource-group" -Location $location
   ```

   ```Example
   ResourceGroupName : rbac-tutorial-resource-group
   Location          : westus
   ProvisioningState : Succeeded
   Tags              :
   ResourceId        : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/rbac-tutorial-resource-group
   ```

## <a name="grant-access"></a>Erişim verme

Kullanıcı erişimi vermek için kullandığınız [yeni AzRoleAssignment](/powershell/module/az.resources/new-azroleassignment) rol atamak için komutu. Güvenlik sorumlusunu, rol tanımını ve kapsamı belirtmeniz gerekir.

1. Kullanarak Kimliğini alın [Get-AzSubscription](/powershell/module/Az.Accounts/Get-AzSubscription) komutu.

    ```azurepowershell
    Get-AzSubscription
    ```

    ```Example
    Name     : Pay-As-You-Go
    Id       : 00000000-0000-0000-0000-000000000000
    TenantId : 22222222-2222-2222-2222-222222222222
    State    : Enabled
    ```

1. Abonelik kapsamını bir değişkene kaydedin.

    ```azurepowershell
    $subScope = "/subscriptions/00000000-0000-0000-0000-000000000000"
    ```

1. [Okuyucu](built-in-roles.md#reader) rolünü abonelik kapsamında kullanıcıya atayın.

    ```azurepowershell
    New-AzRoleAssignment -SignInName rbacuser@example.com `
      -RoleDefinitionName "Reader" `
      -Scope $subScope
    ```

    ```Example
    RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Authorization/roleAssignments/44444444-4444-4444-4444-444444444444
    Scope              : /subscriptions/00000000-0000-0000-0000-000000000000
    DisplayName        : RBAC Tutorial User
    SignInName         : rbacuser@example.com
    RoleDefinitionName : Reader
    RoleDefinitionId   : acdd72a7-3385-48ef-bd42-f606fba81ae7
    ObjectId           : 11111111-1111-1111-1111-111111111111
    ObjectType         : User
    CanDelegate        : False
    ```

1. [Katılımcı](built-in-roles.md#contributor) rolünü kaynak grubu kapsamında kullanıcıya atayın.

    ```azurepowershell
    New-AzRoleAssignment -SignInName rbacuser@example.com `
      -RoleDefinitionName "Contributor" `
      -ResourceGroupName "rbac-tutorial-resource-group"
    ```

    ```Example
    RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/rbac-tutorial-resource-group/providers/Microsoft.Authorization/roleAssignments/33333333-3333-3333-3333-333333333333
    Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/rbac-tutorial-resource-group
    DisplayName        : RBAC Tutorial User
    SignInName         : rbacuser@example.com
    RoleDefinitionName : Contributor
    RoleDefinitionId   : b24988ac-6180-42a0-ab88-20f7382dd24c
    ObjectId           : 11111111-1111-1111-1111-111111111111
    ObjectType         : User
    CanDelegate        : False
    ```

## <a name="list-access"></a>Erişimi listeleme

1. Abonelik için erişimi doğrulamak için [Get-AzRoleAssignment](/powershell/module/az.resources/get-azroleassignment) rol atamalarını listelemek için komutu.

    ```azurepowershell
    Get-AzRoleAssignment -SignInName rbacuser@example.com -Scope $subScope
    ```

    ```Example
    RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Authorization/roleAssignments/22222222-2222-2222-2222-222222222222
    Scope              : /subscriptions/00000000-0000-0000-0000-000000000000
    DisplayName        : RBAC Tutorial User
    SignInName         : rbacuser@example.com
    RoleDefinitionName : Reader
    RoleDefinitionId   : acdd72a7-3385-48ef-bd42-f606fba81ae7
    ObjectId           : 11111111-1111-1111-1111-111111111111
    ObjectType         : User
    CanDelegate        : False
    ```

    Çıktıda Okuyucu rolünün abonelik kapsamında RBAC Tutorial User kullanıcısına atanmış olduğunu görebilirsiniz.

1. Kaynak grubu için erişimi doğrulamak için [Get-AzRoleAssignment](/powershell/module/az.resources/get-azroleassignment) rol atamalarını listelemek için komutu.

    ```azurepowershell
    Get-AzRoleAssignment -SignInName rbacuser@example.com -ResourceGroupName "rbac-tutorial-resource-group"
    ```

    ```Example
    RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/rbac-tutorial-resource-group/providers/Microsoft.Authorization/roleAssignments/33333333-3333-3333-3333-333333333333
    Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/rbac-tutorial-resource-group
    DisplayName        : RBAC Tutorial User
    SignInName         : rbacuser@example.com
    RoleDefinitionName : Contributor
    RoleDefinitionId   : b24988ac-6180-42a0-ab88-20f7382dd24c
    ObjectId           : 11111111-1111-1111-1111-111111111111
    ObjectType         : User
    CanDelegate        : False
    
    RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Authorization/roleAssignments/22222222-2222-2222-2222-222222222222
    Scope              : /subscriptions/00000000-0000-0000-0000-000000000000
    DisplayName        : RBAC Tutorial User
    SignInName         : rbacuser@example.com
    RoleDefinitionName : Reader
    RoleDefinitionId   : acdd72a7-3385-48ef-bd42-f606fba81ae7
    ObjectId           : 11111111-1111-1111-1111-111111111111
    ObjectType         : User
    CanDelegate        : False
    ```

    Çıktıda hem Katılımcı hem de Okuyucu rollerinin RBAC Tutorial User kullanıcısına atanmış olduğunu görebilirsiniz. Katılımcı rolü rbac-tutorial-resource-group kapsamındadır, Okuyucu rolü ise abonelik kapsamından devralınır.

## <a name="optional-list-access-using-the-azure-portal"></a>(İsteğe bağlı) Azure Portal'ı kullanarak erişimi listeleme

1. Rol atamalarının Azure portalında nasıl göründüğünü görmek için aboneliğin **Erişim denetimi (IAM)** dikey penceresini görüntüleyin.

    ![Bir kullanıcının abonelik kapsamındaki rol atamaları](./media/tutorial-role-assignments-user-powershell/role-assignments-subscription-user.png)

1. Kaynak grubunun **Erişim denetimi (IAM)** dikey penceresini görüntüleyin.

    ![Bir kullanıcının kaynak grubu kapsamındaki rol atamaları](./media/tutorial-role-assignments-user-powershell/role-assignments-resource-group-user.png)

## <a name="remove-access"></a>Erişimi kaldırma

Kullanıcılara, gruplara ve uygulamalara erişimi kaldırmak için [Remove-AzRoleAssignment](/powershell/module/az.resources/remove-azroleassignment) bir rol ataması kaldırılamadı.

1. Kullanıcının kaynak grubu kapsamındaki Katılımcı rol atamasını kaldırmak için aşağıdaki komutu kullanın.

    ```azurepowershell
    Remove-AzRoleAssignment -SignInName rbacuser@example.com `
      -RoleDefinitionName "Contributor" `
      -ResourceGroupName "rbac-tutorial-resource-group"
    ```

1. Kullanıcının abonelik kapsamındaki Okuyucu rol atamasını kaldırmak için aşağıdaki komutu kullanın.

    ```azurepowershell
    Remove-AzRoleAssignment -SignInName rbacuser@example.com `
      -RoleDefinitionName "Reader" `
      -Scope $subScope
    ```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğretici ile oluşturulan kaynakları temizlemek için kaynak grubunu ve kullanıcıyı silebilirsiniz.

1. Kaynak grubu silme [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) komutu.

    ```azurepowershell
    Remove-AzResourceGroup -Name "rbac-tutorial-resource-group"
    ```

    ```Example
    Confirm
    Are you sure you want to remove resource group 'rbac-tutorial-resource-group'
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):
    ```
    
1. Onaylamanız istendiğinde **Y** yazın. Silme işlemi birkaç saniye sürer.

1. Kullanıcıyı silmek için [Remove-AzureADUser](/powershell/module/azuread/remove-azureaduser) komutunu kullanın.

    ```azurepowershell
    Remove-AzureADUser -ObjectId "rbacuser@example.com"
    ```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [RBAC ve PowerShell kullanarak erişimi yönetme](role-assignments-powershell.md)
