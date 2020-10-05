---
title: 'Hızlı başlangıç: Kaynak Yöneticisi şablonu kullanarak sanal ağ oluşturma'
titleSuffix: Azure Virtual Network
description: Azure sanal ağı oluşturmak için Kaynak Yöneticisi şablonu kullanmayı öğrenin.
services: virtual-network
author: KumudD
ms.service: virtual-network
ms.topic: quickstart
ms.date: 06/23/2020
ms.author: kumud
ms.custom: ''
ms.openlocfilehash: c9e9ea2e59537a1c0c8e372a766fba3aa9a1b9a0
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2020
ms.locfileid: "88122189"
---
# <a name="quickstart-create-a-virtual-network---resource-manager-template"></a>Hızlı başlangıç: sanal ağ Kaynak Yöneticisi şablonu oluşturma

Bu hızlı başlangıçta, Azure Resource Manager şablonunu kullanarak iki alt ağa sahip bir sanal ağ oluşturmayı öğreneceksiniz. Bir sanal ağ, Azure 'daki özel ağınız için temel yapı taşdır. VM 'Ler gibi Azure kaynaklarının birbirleriyle ve internet ile güvenli bir şekilde iletişim kurmasına olanak sağlar.


[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Bu hızlı başlangıcı [Azure Portal](quick-create-portal.md), [Azure POWERSHELL](quick-create-powershell.md)veya [Azure CLI](quick-create-cli.md)kullanarak da tamamlayabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="review-the-template"></a>Şablonu gözden geçirme

Bu hızlı başlangıçta kullanılan şablon [Azure hızlı başlangıç şablonlarından](https://github.com/Azure/azure-quickstart-templates/blob/master/101-vnet-two-subnets/azuredeploy.json)

:::code language="json" source="~/quickstart-templates/101-vnet-two-subnets/azuredeploy.json" range="001-96" highlight="56-92":::

Şablonda aşağıdaki Azure kaynakları tanımlandı:
- [**Microsoft. Network/virtualNetworks**](https://docs.microsoft.com/azure/templates/microsoft.network/virtualnetworks): bir Azure sanal ağı oluşturun.
-  [**Microsoft. Network/virtualNetworks/alt ağları**](https://docs.microsoft.com/azure/templates/microsoft.network/virtualnetworks/subnets) -bir alt ağ oluşturun.

## <a name="deploy-the-template"></a>Şablonu dağıtma

Kaynak Yöneticisi şablonu Azure 'a dağıtma:

1. Azure 'da oturum açmak için **Azure 'A dağıt** ' ı seçin ve şablonu açın. Şablon iki alt ağa sahip bir sanal ağ oluşturur.

   [![Azure’a dağıtın](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-virtual-network-2vms-create%2Fazuredeploy.json)

2. Portalda, **Iki alt ağa sahip bir sanal ağ oluşturun** sayfasında, aşağıdaki değerleri yazın veya seçin:
   - **Kaynak grubu**: **Yeni oluştur**' u seçin, kaynak grubu için bir ad yazın ve **Tamam**' ı seçin.
   - **Sanal ağ adı**: yeni sanal ağ için bir ad yazın.
3. **Gözden Geçir + oluştur**’u, sonra da **Oluştur**’u seçin.

## <a name="review-deployed-resources"></a>Dağıtılan kaynakları gözden geçirme

Sanal ağla oluşturulan kaynakları keşfedebilir.

Bir şablondaki bir sanal ağın JSON sözdizimi ve özellikleri hakkında bilgi edinmek için bkz. [Microsoft. Network/virtualNetworks](https://docs.microsoft.com/azure/templates/microsoft.network/virtualnetworks).

## <a name="clean-up-resources"></a>Kaynakları temizleme

Sanal Ağla oluşturduğunuz kaynaklara artık ihtiyacınız kalmadığında, kaynak grubunu silin. Bu, sanal ağı ve tüm ilgili kaynakları kaldırır.

Kaynak grubunu silmek için `Remove-AzResourceGroup` cmdlet 'ini çağırın:

```azurepowershell-interactive
Remove-AzResourceGroup -Name <your resource group name>
```

## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçta, iki alt ağa sahip bir Azure sanal ağı dağıttınız. Azure sanal ağları hakkında daha fazla bilgi edinmek için sanal ağlar için öğreticiye geçin.

> [!div class="nextstepaction"]
> [Ağ trafiğini filtreleme](tutorial-filter-network-traffic.md)
