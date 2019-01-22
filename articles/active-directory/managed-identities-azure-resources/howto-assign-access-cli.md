---
title: Azure CLI kullanarak bir Azure kaynağı için bir yönetilen kimlik erişim atama
description: Adım adım yönergeler bir kaynakta bir yönetilen kimlik atamak için Azure CLI kullanarak başka bir kaynağa erişin.
services: active-directory
documentationcenter: ''
author: daveba
manager: daveba
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/06/2017
ms.author: daveba
ms.openlocfilehash: ffed9e388b41b6c19ae8ed22b9c843f5ac8ceb3e
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2019
ms.locfileid: "54430424"
---
# <a name="assign-a-managed-identity-access-to-a-resource-using-azure-cli"></a>Azure CLI kullanarak bir kaynak için bir yönetilen kimlik erişim atama

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Yönetilen bir kimlik ile bir Azure kaynağı yapılandırdıktan sonra herhangi bir güvenlik sorumlusu gibi başka bir kaynak yönetilen kimlik erişim izni verebilirsiniz. Bu örnek Azure CLI kullanarak Azure depolama hesabınız için bir Azure sanal makine veya sanal makine ölçek kümesi'nin yönetilen kimlik erişim vermek nasıl gösterir.

## <a name="prerequisites"></a>Önkoşullar

- Azure kaynakları için yönetilen kimliklerle bilmiyorsanız kullanıma [genel bakış bölümünde](overview.md). **Gözden geçirmeyi unutmayın [sistem tarafından atanan ve kullanıcı tarafından atanan bir yönetilen kimlik arasındaki farkı](overview.md#how-does-it-work)**.
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).
- CLI betiği örnekleri çalıştırmak için üç seçeneğiniz vardır:
    - Kullanım [Azure Cloud Shell](../../cloud-shell/overview.md) Azure portalından (sonraki bölüme bakın).
    - Katıştırılmış Azure Cloud Shell aracılığıyla her kod bloğunun sağ üst köşesinde bulunan "Try It" düğmesini kullanın.
    - [Azure CLI'ın en son sürümü yükleyin](https://docs.microsoft.com/cli/azure/install-azure-cli) yerel CLI konsol kullanmak istiyorsanız. 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="use-rbac-to-assign-a-managed-identity-access-to-another-resource"></a>Başka bir kaynak için bir yönetilen kimlik erişimi atamak için RBAC kullanma

Etkinleştirdikten sonra bir Azure kaynak kimliği gibi yönetilen bir [Azure sanal makine](qs-configure-cli-windows-vm.md) veya [Azure sanal makine ölçek kümesi](qs-configure-cli-windows-vmss.md): 

1. Azure CLI'yi yerel bir konsolda kullanıyorsanız, önce [az login](/cli/azure/reference-index#az-login) kullanarak Azure'da oturum açın. Sanal makine veya sanal makine ölçek kümesini dağıtmak altında istediğiniz Azure aboneliği ile ilişkili olan bir hesabı kullanın:

   ```azurecli-interactive
   az login
   ```

2. Bu örnekte biz bir depolama hesabı için bir Azure sanal makine erişimini vermiş olursunuz. Önce kullandığımız [az kaynak listesi](/cli/azure/resource/#az-resource-list) myVM adlı sanal makine için hizmet sorumlusu almak için:

   ```azurecli-interactive
   spID=$(az resource list -n myVM --query [*].identity.principalId --out tsv)
   ```
   Bir Azure sanal makine ölçek kümesi için komutu burada dışında aynıdır, "DevTestVMSS" adlı sanal makine ölçek kümesi için hizmet sorumlusu Al:
   
   ```azurecli-interactive
   spID=$(az resource list -n DevTestVMSS --query [*].identity.principalId --out tsv)
   ```

3. Hizmet sorumlusu Kimliğini oluşturduktan sonra kullanma [az rol ataması oluşturma](/cli/azure/role/assignment#az-role-assignment-create) sanal makine veya sanal makine ölçek vermek için "Okuyucu" erişim "myStorageAcct" adlı bir depolama hesabı ayarlayın:

   ```azurecli-interactive
   az role assignment create --assignee $spID --role 'Reader' --scope /subscriptions/<mySubscriptionID>/resourceGroups/<myResourceGroup>/providers/Microsoft.Storage/storageAccounts/myStorageAcct
   ```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure kaynaklarına genel bakış için yönetilen kimlik](overview.md)
- Bir Azure sanal makinesinde yönetilen kimlik etkinleştirmek için bkz: [yapılandırma kimliklerini Azure CLI kullanarak bir Azure sanal makinesinde Azure kaynakları için yönetilen](qs-configure-cli-windows-vm.md).
- Bir Azure sanal makine ölçek kümesinde yönetilen kimlik etkinleştirmek için bkz: [yapılandırma yönetilen bir sanal makine ölçek kümesi Azure CLI kullanarak Azure kaynakları için kimlikleri](qs-configure-cli-windows-vmss.md).