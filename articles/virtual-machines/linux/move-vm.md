---
title: Azure CLı kullanarak bir VM 'yi taşıma
description: Azure CLı kullanarak bir VM 'yi başka bir Azure aboneliğine veya kaynak grubuna taşıyın.
author: cynthn
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: how-to
ms.date: 09/12/2018
ms.author: cynthn
ms.openlocfilehash: db4c7e0126616e2d8bd120e7430c70b89c5cf36d
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "87291096"
---
# <a name="move-a-vm-to-another-subscription-or-resource-group"></a>Bir VM 'yi başka bir aboneliğe veya kaynak grubuna taşıma
Bu makalede, bir sanal makineyi (VM) kaynak grupları veya abonelikler arasında nasıl taşıyacağınız anlatılmaktadır. Bir VM 'yi abonelikler arasında taşımak, kişisel abonelikte bir VM oluşturduysanız ve şimdi bunu şirketinizin aboneliğine taşımak istiyorsanız yararlı olabilir.

> [!IMPORTANT]
>Yeni kaynak kimlikleri taşımanın bir parçası olarak oluşturulur. VM taşındıktan sonra, yeni kaynak kimliklerini kullanmak için araçlarınızı ve betikleri güncelleştirmeniz gerekecektir.
>


## <a name="use-the-azure-cli-to-move-a-vm"></a>Azure CLı kullanarak bir VM 'yi taşıma


Azure CLı kullanarak VM 'nizi taşıyabilmeniz için önce kaynak ve hedef aboneliklerin aynı kiracı içinde mevcut olduğundan emin olmanız gerekir. Her iki aboneliğin de aynı kiracı KIMLIĞINE sahip olup olmadığını denetlemek için [az Account Show](/cli/azure/account)kullanın.

```azurecli-interactive
az account show --subscription mySourceSubscription --query tenantId
az account show --subscription myDestinationSubscription --query tenantId
```
Kaynak ve hedef aboneliklerin kiracı kimlikleri aynı değilse, kaynakları yeni bir kiracıya taşımak için [desteğe](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) başvurmanız gerekir.

Bir VM 'yi başarılı bir şekilde taşımak için, VM 'yi ve tüm destekleyici kaynaklarını taşımanız gerekir. Bir kaynak grubundaki tüm kaynakları ve bunların kimliklerini listelemek için [az Resource List](/cli/azure/resource) komutunu kullanın. Kimlikleri daha sonra komutlara kopyalayabilir ve yapıştırabilmeniz için bu komutun çıkışını bir dosyaya yöneltmenize yardımcı olur.

```azurecli-interactive
az resource list --resource-group "mySourceResourceGroup" --query "[].{Id:id}" --output table
```

Bir VM 'yi ve kaynaklarını başka bir kaynak grubuna taşımak için [az Resource Move](/cli/azure/resource)kullanın. Aşağıdaki örnek, bir VM 'nin ve gereken en sık kullanılan kaynakların nasıl taşınacağını gösterir. **-IDS** parametresini kullanın ve taşıyacağınız kaynakların kimliği için virgülle ayrılmış bir liste (boşluk olmadan) geçirin.

```azurecli-interactive
vm=/subscriptions/mySourceSubscriptionID/resourceGroups/mySourceResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM
nic=/subscriptions/mySourceSubscriptionID/resourceGroups/mySourceResourceGroup/providers/Microsoft.Network/networkInterfaces/myNIC
nsg=/subscriptions/mySourceSubscriptionID/resourceGroups/mySourceResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNSG
pip=/subscriptions/mySourceSubscriptionID/resourceGroups/mySourceResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIPAddress
vnet=/subscriptions/mySourceSubscriptionID/resourceGroups/mySourceResourceGroup/providers/Microsoft.Network/virtualNetworks/myVNet
diag=/subscriptions/mySourceSubscriptionID/resourceGroups/mySourceResourceGroup/providers/Microsoft.Storage/storageAccounts/mydiagnosticstorageaccount
storage=/subscriptions/mySourceSubscriptionID/resourceGroups/mySourceResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccountname    

az resource move \
    --ids $vm,$nic,$nsg,$pip,$vnet,$storage,$diag \
    --destination-group "myDestinationResourceGroup"
```

VM 'yi ve kaynaklarını farklı bir aboneliğe taşımak istiyorsanız, hedef aboneliği belirtmek için **--Destination-SubscriptionID** parametresini ekleyin.

Belirtilen kaynakları taşımak istediğinizi onaylamanız istendiğinde, onaylamak için **Y** yazın.

[!INCLUDE [virtual-machines-common-move-vm](../../../includes/virtual-machines-common-move-vm.md)]

## <a name="next-steps"></a>Sonraki adımlar
Kaynak grupları ve abonelikler arasında birçok farklı kaynak türünü taşıyabilirsiniz. Daha fazla bilgi için bkz. [kaynakları yeni bir kaynak grubuna veya aboneliğe taşıma](../../azure-resource-manager/management/move-resource-group-and-subscription.md).    
