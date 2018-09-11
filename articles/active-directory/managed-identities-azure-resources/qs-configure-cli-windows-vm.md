---
title: Sistem ve kullanıcı tarafından atanan yönetilen kimlikleri, Azure CLI kullanarak bir Azure sanal makinesinde yapılandırma
description: Sistem ve kullanıcı tarafından atanan yönetilen kimlikleri, Azure CLI kullanarak bir Azure sanal makinesinde yapılandırma yönergeleri adım adım.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/14/2017
ms.author: daveba
ms.openlocfilehash: 094ee070a3eae9d102bd137e1a0ed7299b2b45a3
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2018
ms.locfileid: "44345905"
---
# <a name="configure-managed-identities-for-azure-resources-on-an-azure-vm-using-azure-cli"></a>Azure CLI kullanarak bir Azure sanal makinesinde Azure kaynakları için yönetilen kimlik Yapılandır

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Azure kaynakları için yönetilen kimlikleri Azure Active Directory'de otomatik olarak yönetilen bir kimlikle Azure hizmetleri sağlar. Bu kimlik, Azure AD kimlik doğrulaması, kimlik bilgilerini kodunuzda zorunda kalmadan destekleyen herhangi bir hizmeti kimlik doğrulaması için kullanabilirsiniz. 

Bu makalede, Azure CLI kullanarak, Azure sanal makinesinde Azure kaynaklarını işlemleri için yönetilen şu kimlikler gerçekleştirmeyi öğreneceksiniz:

- Enable ve disable Azure VM'de sistem tarafından atanan yönetilen kimlik
- Bir kullanıcı tarafından atanan yönetilen kimlik Azure VM'de ekleyip

## <a name="prerequisites"></a>Önkoşullar

- Azure kaynakları için yönetilen kimliklerle bilmiyorsanız kullanıma [genel bakış bölümünde](overview.md). **Gözden geçirmeyi unutmayın [sistem tarafından atanan ve kullanıcı tarafından atanan bir yönetilen kimlik arasındaki farkı](overview.md#how-does-it-work)**.
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).
- Bu makalede yönetim işlemlerini gerçekleştirmek için aşağıdaki Azure rol tabanlı erişim denetimi atamalarını hesabınızın gerekir:
    > [!NOTE]
    > Hiçbir ek Azure AD dizini rol atamaları gerekli.
    - [Sanal makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) bir VM oluşturun ve etkinleştirin ve sistem ve/veya yönetilen kimlik kullanıcı tarafından atanan bir Azure VM'den kaldırın.
    - [Yönetilen kimlik Katılımcısı](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rolüne bir kullanıcı tarafından atanan oluşturmak için yönetilen kimliği.
    - [Yönetilen kimlik işleci](/azure/role-based-access-control/built-in-roles#managed-identity-operator) gelen ve sanal makine rolü atamak ve bir kullanıcı tarafından atanan kaldırmak için yönetilen kimliği.
- CLI betiği örnekleri çalıştırmak için üç seçeneğiniz vardır:
    - Kullanım [Azure Cloud Shell](../../cloud-shell/overview.md) Azure portalından (sonraki bölüme bakın).
    - Katıştırılmış Azure Cloud Shell aracılığıyla her kod bloğunun sağ üst köşesinde bulunan "Try It" düğmesini kullanın.
    - [Azure CLI'ın en son sürümü yükleyin](https://docs.microsoft.com/cli/azure/install-azure-cli) yerel CLI konsol kullanmak istiyorsanız.
      
      > [!NOTE]
      > Komutlar, en son sürümünü yansıtacak şekilde güncelleştirildi [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli).     
        

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="system-assigned-managed-identity"></a>Sistem tarafından atanan yönetilen kimlik

Bu bölümde, Azure CLI kullanarak bir Azure sanal makinesinde sistem tarafından atanan yönetilen kimlik devre öğrenin.

### <a name="enable-system-assigned-managed-identity-during-creation-of-an-azure-vm"></a>Bir Azure VM oluşturma sırasında sistem tarafından atanan yönetilen kimlik etkinleştir

Etkin sistem tarafından atanan yönetilen kimlik ile bir Azure VM oluşturmak için:

1. Azure CLI'yi yerel bir konsolda kullanıyorsanız, önce [az login](/cli/azure/reference-index#az-login) kullanarak Azure'da oturum açın. VM'yi dağıtırken kullanmak istediğiniz Azure aboneliğiyle ilişkilendirilmiş bir hesap kullanın:

   ```azurecli-interactive
   az login
   ```

2. VM'nizin ve onunla ilgili kaynakların kapsaması ve dağıtımı için, [az group create](/cli/azure/group/#az-group-create) komutunu kullanarak bir [kaynak grubu](../../azure-resource-manager/resource-group-overview.md#terminology) oluşturun. Bunun yerine kullanmak istediğiniz bir kaynak grubunuz varsa, bu adımı atlayabilirsiniz:

   ```azurecli-interactive 
   az group create --name myResourceGroup --location westus
   ```

3. [az vm create](/cli/azure/vm/#az-vm-create) kullanarak VM oluşturun. Aşağıdaki örnekte adlı bir VM oluşturur *myVM* bir sistem tarafından atanan ile kimliği tarafından istendiği yönetilen `--assign-identity` parametresi. `--admin-username` ve `--admin-password` parametreleri, sanal makinede oturum açmak için yönetici hesabının kullanıcı adı ve parolasını belirtir. Bu değerleri ortamınıza uyacak şekilde güncelleştirin: 

   ```azurecli-interactive 
   az vm create --resource-group myResourceGroup --name myVM --image win2016datacenter --generate-ssh-keys --assign-identity --admin-username azureuser --admin-password myPassword12
   ```

### <a name="enable-system-assigned-managed-identity-on-an-existing-azure-vm"></a>Yönetilen kimlik sistemi atanmış mevcut bir Azure sanal makinesinde etkinleştirin

Sistem tarafından atanan yönetilen mevcut VM'yi hizmetinizle ihtiyacınız varsa:

1. Azure CLI'yi yerel bir konsolda kullanıyorsanız, önce [az login](/cli/azure/reference-index#az-login) kullanarak Azure'da oturum açın. VM içeren Azure aboneliği ile ilişkili olan bir hesap kullanın.

   ```azurecli-interactive
   az login
   ```

2. Kullanım [az vm kimliği atamak](/cli/azure/vm/identity/#az-vm-identity-assign) ile `identity assign` komutu etkinleştirmek var olan bir sanal makineye sistem tarafından atanan kimliği:

   ```azurecli-interactive
   az vm identity assign -g myResourceGroup -n myVm
   ```

### <a name="disable-system-assigned-identity-from-an-azure-vm"></a>Bir Azure VM'den sistem tarafından atanan kimliği devre dışı

Artık sistem tarafından atanan kimliği gerekiyor, ancak yine de kullanıcı tarafından atanan kimlikleri gereken bir sanal makine varsa, aşağıdaki komutu kullanın:

```azurecli-interactive
az vm update -n myVM -g myResourceGroup --set identity.type='UserAssigned' 
```

Artık sistem tarafından atanan kimlik gereken bir sanal makineye sahip ve kullanıcı tarafından atanan kimlik varsa, aşağıdaki komutu kullanın:

> [!NOTE]
> Değer `none` büyük/küçük harfe duyarlıdır. Küçük harf olması gerekir. 

```azurecli-interactive
az vm update -n myVM -g myResourceGroup --set identity.type="none"
```

Yönetilen kimlik (Ocak 2019'da kullanımdan kaldırma planlanan), Azure kaynaklarını VM uzantısı için kaldırmak için kullanıcı `-n ManagedIdentityExtensionForWindows` veya `-n ManagedIdentityExtensionForLinux` anahtarı (sanal makine türünü bağlı olarak) [az vm uzantısı silme](https://docs.microsoft.com/cli/azure/vm/#assign-identity):

```azurecli-interactive
az vm identity --resource-group myResourceGroup --vm-name myVm -n ManagedIdentityExtensionForWindows
```

## <a name="user-assigned-managed-identity"></a>Kullanıcı tarafından atanan yönetilen kimlik

Bu bölümde, bir kullanıcı tarafından atanan bir yönetilen kimlik Azure CLI kullanarak bir Azure VM'den ekleyip öğreneceksiniz.

### <a name="assign-a-user-assigned-managed-identity-during-the-creation-of-an-azure-vm"></a>Bir Azure VM oluşturma sırasında kullanıcı tarafından atanan bir yönetilen kimlik atama

Bu bölümde kullanıcı tarafından atanan bir yönetilen kimlik atama ile bir VM oluşturulmasına size kılavuzluk eder. Kullanmak istediğiniz bir VM'niz varsa, bu bölümü atlayın ve sonraki devam edin.

1. Kullanmak istediğiniz bir kaynak grubu zaten varsa bu adımı atlayabilirsiniz. Oluşturma bir [kaynak grubu](~/articles/azure-resource-manager/resource-group-overview.md#terminology) kapsama ve kullanıcı tarafından atanan yönetilen kimliğinizi dağıtımını kullanarak [az grubu oluşturma](/cli/azure/group/#az-group-create). `<RESOURCE GROUP>` ve `<LOCATION>` parametre değerlerini kendi değerlerinizle değiştirmeyi unutmayın. :

   ```azurecli-interactive 
   az group create --name <RESOURCE GROUP> --location <LOCATION>
   ```

2. Bir kullanıcı tarafından atanan yönetilen kimlik kullanarak oluşturduğunuz [az kimliği oluşturma](/cli/azure/identity#az-identity-create).  `-g` Parametresi, kullanıcı tarafından atanan bir yönetilen kimlik oluşturulduğu, kaynak grubunu belirtir ve `-n` parametre adını belirtir.    
    
   [!INCLUDE [ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

   ```azurecli-interactive
   az identity create -g myResourceGroup -n myUserAssignedIdentity
   ```
   Yanıt, aşağıdakine benzer şekilde oluşturulmuş kullanıcı tarafından atanan yönetilen kimlik ayrıntılarını içerir. Kullanıcı tarafından atanan yönetilen kimliğe atanır kaynağı kimliği değeri aşağıdaki adımda kullanılır.

   ```json
   {
       "clientId": "73444643-8088-4d70-9532-c3a0fdc190fz",
       "clientSecretUrl": "https://control-westcentralus.identity.azure.net/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<myUserAssignedIdentity>/credentials?tid=5678&oid=9012&aid=73444643-8088-4d70-9532-c3a0fdc190fz",
       "id": "/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>",
       "location": "westcentralus",
       "name": "<USER ASSIGNED IDENTITY NAME>",
       "principalId": "e5fdfdc1-ed84-4d48-8551-fe9fb9dedfll",
       "resourceGroup": "<RESOURCE GROUP>",
       "tags": {},
       "tenantId": "733a8f0e-ec41-4e69-8ad8-971fc4b533bl",
       "type": "Microsoft.ManagedIdentity/userAssignedIdentities"    
   }
   ```

3. [az vm create](/cli/azure/vm/#az-vm-create) kullanarak VM oluşturun. Aşağıdaki örnek, yeni kullanıcı tarafından atanan kimliği tarafından belirtilen ile ilişkili bir VM oluşturur `--assign-identity` parametresi. `<RESOURCE GROUP>`, `<VM NAME>`, `<USER NAME>`, `<PASSWORD>` ve `<USER ASSIGNED IDENTITY NAME>` parametre değerlerini kendi değerlerinizle değiştirmeyi unutmayın. 

   ```azurecli-interactive 
   az vm create --resource-group <RESOURCE GROUP> --name <VM NAME> --image UbuntuLTS --admin-username <USER NAME> --admin-password <PASSWORD> --assign-identity <USER ASSIGNED IDENTITY NAME>
   ```

### <a name="assign-a-user-assigned-managed-identity-to-an-existing-azure-vm"></a>Kullanıcı tarafından atanan bir yönetilen kimlik mevcut bir Azure VM'ye atayın

1. Kullanıcı tarafından atanan kimliği oluşturmak için [az identity create](/cli/azure/identity#az-identity-create) kullanın.  `-g` Parametresi, kullanıcı tarafından atanan kimlik oluşturulduğu, kaynak grubunu belirtir ve `-n` parametre adını belirtir. `<RESOURCE GROUP>` ve `<USER ASSIGNED IDENTITY NAME>` parametre değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

    > [!IMPORTANT]
    > Adında özel karakterler (örneğin, alt çizgi) ile kullanıcı tarafından atanan yönetilen kimlikleri oluşturma şu anda desteklenmiyor. Lütfen alfasayısal karakterler kullanın. Güncelleştirmeler için sonra yeniden denetleyin.  Daha fazla bilgi için [SSS ve bilinen sorunlar](known-issues.md)

    ```azurecli-interactive
    az identity create -g <RESOURCE GROUP> -n <USER ASSIGNED IDENTITY NAME>
    ```
   Yanıt, aşağıdakine benzer şekilde oluşturulmuş kullanıcı tarafından atanan yönetilen kimlik ayrıntılarını içerir. 

   ```json
   {
        "clientId": "73444643-8088-4d70-9532-c3a0fdc190fz",
        "clientSecretUrl": "https://control-westcentralus.identity.azure.net/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>/credentials?tid=5678&oid=9012&aid=73444643-8088-4d70-9532-c3a0fdc190fz",
        "id": "/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>",
        "location": "westcentralus",
        "name": "<USER ASSIGNED IDENTITY NAME>",
        "principalId": "e5fdfdc1-ed84-4d48-8551-fe9fb9dedfll",
        "resourceGroup": "<RESOURCE GROUP>",
        "tags": {},
        "tenantId": "733a8f0e-ec41-4e69-8ad8-971fc4b533bl",
        "type": "Microsoft.ManagedIdentity/userAssignedIdentities"    
   }
   ```

2. Kullanıcı tarafından atanan kimlik kullanarak VM atama [az vm kimliği atamak](/cli/azure/vm#az-vm-identity-assign). `<RESOURCE GROUP>` ve `<VM NAME>` parametre değerlerini kendi değerlerinizle değiştirmeyi unutmayın. `<USER ASSIGNED IDENTITY NAME>` Kullanıcı tarafından atanan yönetilen kimliğin kaynağı `name` önceki adımda oluşturulan özelliği:

    ```azurecli-interactive
    az vm identity assign -g <RESOURCE GROUP> -n <VM NAME> --identities <USER ASSIGNED IDENTITY>
    ```

### <a name="remove-a-user-assigned-managed-identity-from-an-azure-vm"></a>Kullanıcı tarafından atanan bir yönetilen kimlik bir Azure VM'den kaldırın.

Kullanıcı tarafından atanan bir yönetilen kimlik VM kullanımdan kaldırılacağı [az vm kimliğini kaldırma](/cli/azure/vm#az-vm-identity-remove). Bu tek ise kullanıcı tarafından atanan sanal makineye atanan kimlik yönetilen `UserAssigned` kimlik türü değerinden kaldırılacak.  `<RESOURCE GROUP>` ve `<VM NAME>` parametre değerlerini kendi değerlerinizle değiştirmeyi unutmayın. `<USER ASSIGNED IDENTITY>` Kullanıcı tarafından atanan kimlik olacaktır `name` özelliğini kullanarak sanal makine kimlik bölümünde bulunabilir `az vm identity show`:

```azurecli-interactive
az vm identity remove -g <RESOURCE GROUP> -n <VM NAME> --identities <USER ASSIGNED IDENTITY>
```

Sanal makinenizin sistem tarafından atanan bir yönetilen kimlik yok ve tüm kullanıcı tarafından atanan kimlikleri kaldırmak istiyorsanız aşağıdaki komutu kullanın:

> [!NOTE]
> Değer `none` büyük/küçük harfe duyarlıdır. Küçük harf olması gerekir.

```azurecli-interactive
az vm update -n myVM -g myResourceGroup --set identity.type="none" identity.userAssignedIdentities=null
```

Makineniz varsa her ikisi de sistem tarafından atanan ve kullanıcı tarafından atanan kimlikleri yalnızca sistem tarafından atanan kullanılacak geçerek tüm kullanıcı tarafından atanan kimlikleri kaldırabilirsiniz. Aşağıdaki komutu kullanın:

```azurecli-interactive
az vm update -n myVM -g myResourceGroup --set identity.type='SystemAssigned' identity.userAssignedIdentities=null 
```

## <a name="next-steps"></a>Sonraki adımlar
- [Azure kaynaklarına genel bakış için yönetilen kimlik](overview.md)
- Tam Azure VM oluşturmak için hızlı başlangıç kılavuzları, bkz: 
  - [CLI ile bir Windows sanal makinesi oluşturma](../../virtual-machines/windows/quick-create-cli.md)  
  - [CLI ile Linux sanal makinesi oluşturma](../../virtual-machines/linux/quick-create-cli.md) 

















