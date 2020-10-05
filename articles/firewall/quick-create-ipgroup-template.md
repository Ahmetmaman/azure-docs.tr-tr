---
title: 'Hızlı başlangıç: Azure Güvenlik Duvarı ve IP grupları oluşturma-Kaynak Yöneticisi şablonu'
description: Bu hızlı başlangıçta, bir Azure Güvenlik Duvarı ve IP grupları oluşturmak için bir Azure Resource Manager şablonu (ARM şablonu) kullanmayı öğreneceksiniz.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: quickstart
ms.custom: subject-armqs
ms.date: 08/28/2020
ms.author: victorh
ms.openlocfilehash: 7966cbe44fc8984cf3c3d9e2d08a11bf599ff158
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2020
ms.locfileid: "89079141"
---
# <a name="quickstart-create-an-azure-firewall-and-ip-groups---arm-template"></a>Hızlı başlangıç: Azure Güvenlik Duvarı ve IP grupları oluşturma-ARM şablonu

Bu hızlı başlangıçta, bir ağ kuralında ve uygulama kuralında kullanılan örnek IP gruplarıyla bir Azure Güvenlik Duvarı dağıtmak için bir Azure Resource Manager şablonu (ARM şablonu) kullanırsınız. Bir IP grubu, IP adreslerini, aralıklarını ve alt ağları tek bir nesne içinde tanımlamanızı ve gruplandıreklemenizi sağlayan üst düzey bir kaynaktır. Bu, Azure Güvenlik duvarı kurallarında IP adreslerini yönetmek için yararlıdır. IP adreslerini el ile girebilir veya bir dosyadan içeri aktarabilirsiniz.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Ortamınız önkoşulları karşılıyorsa ve ARM şablonlarını kullanma hakkında bilginiz varsa, **Azure’a dağıtma** düğmesini seçin. Şablon Azure portalda açılır.

[![Azure’a dağıtma](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-azurefirewall-create-with-ipgroups-and-linux-jumpbox%2Fazuredeploy.json)

## <a name="prerequisites"></a>Ön koşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="review-the-template"></a>Şablonu gözden geçirme

Bu şablon, Azure Güvenlik duvarını desteklemek için gerekli kaynaklarla birlikte bir Azure Güvenlik Duvarı ve IP grupları oluşturur.

Bu hızlı başlangıçta kullanılan şablon [Azure Hızlı Başlangıç Şablonlarından](https://azure.microsoft.com/resources/templates/101-azurefirewall-create-with-ipgroups-and-linux-jumpbox) alınmıştır.

:::code language="json" source="~/quickstart-templates/101-azurefirewall-create-with-ipgroups-and-linux-jumpbox/azuredeploy.json":::

Şablonda birden çok Azure kaynağı tanımlanmış:

- [**Microsoft. Network/ıpgroups**](/azure/templates/microsoft.network/ipGroups)
- [**Microsoft. Storage/storageAccounts**](/azure/templates/microsoft.storage/storageAccounts)
- [**Microsoft. Network/routeTables**](/azure/templates/microsoft.network/routeTables)
- [**Microsoft. Network/networkSecurityGroups**](/azure/templates/microsoft.network/networksecuritygroups)
- [**Microsoft. Network/virtualNetworks**](/azure/templates/microsoft.network/virtualnetworks)
- [**Microsoft. Network/Publicıpaddresses**](/azure/templates/microsoft.network/publicipaddresses)
- [**Microsoft. Network/NetworkInterfaces**](/azure/templates/microsoft.network/networkinterfaces)
- [**Microsoft.Compute/virtualMachines**](/azure/templates/microsoft.compute/virtualmachines)
- [**Microsoft. Network/azureFirewalls**](/azure/templates/microsoft.network/azureFirewalls)

## <a name="deploy-the-template"></a>Şablonu dağıtma

ARM şablonunu Azure 'a dağıtma:

1. Azure 'da oturum açmak için **Azure 'A dağıt** ' ı seçin ve şablonu açın. Şablon bir Azure Güvenlik Duvarı, ağ altyapısı ve iki sanal makine oluşturur.

   [![Azure’a dağıtın](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-azurefirewall-create-with-ipgroups-and-linux-jumpbox%2Fazuredeploy.json)

2. Portalda, **ıpgroups Ile Azure Güvenlik Duvarı oluşturma** sayfasında, aşağıdaki değerleri yazın veya seçin:
   - Abonelik: mevcut abonelikler arasından seçim yapın 
   - Kaynak grubu: mevcut kaynak gruplarından seçim yapın veya **Yeni oluştur**' u seçin ve **Tamam**' ı seçin.
   - Konum: bir konum seçin
   - Sanal ağ adı: yeni sanal ağ (VNet) için bir ad yazın 
   - IP grubu adı 1: IP grubu 1 için tür adı 
   - IP grubu adı 2: IP grubu için tür adı 2 
   - Yönetici Kullanıcı adı: yönetici kullanıcı hesabı için Kullanıcı adı yazın 
   - Kimlik doğrulaması: sshPublicKey veya Password seçin 
   - Yönetici parolası: yönetici parolasını veya anahtarını yazın

3. **Yukarıda belirtilen hüküm ve koşulları kabul ediyorum** ' u seçin ve ardından **satın al**' ı seçin. Dağıtımın tamamlanması 10 dakika veya daha uzun sürebilir.

## <a name="review-deployed-resources"></a>Dağıtılan kaynakları gözden geçirme

Azure portal, dağıtılmış kaynakları, özellikle de IP grupları kullanan güvenlik duvarı kurallarını gözden geçirin.

:::image type="content" source="media/quick-create-ipgroup-template/ipgroups.png" alt-text="IP grupları.":::

:::image type="content" source="media/quick-create-ipgroup-template/network-rule.png" alt-text="IP grupları.":::

Bir şablondaki güvenlik duvarının JSON sözdizimi ve özellikleri hakkında bilgi edinmek için bkz. [Microsoft. Network azureFirewalls Template Reference](/azure/templates/Microsoft.Network/2019-11-01/azureFirewalls).

## <a name="clean-up-resources"></a>Kaynakları temizleme

Güvenlik duvarıyla oluşturduğunuz kaynaklara artık ihtiyacınız kalmadığında, kaynak grubunu silin. Bu, güvenlik duvarını ve tüm ilgili kaynakları kaldırır.

Kaynak grubunu silmek için `Remove-AzResourceGroup` cmdlet 'ini çağırın:

```azurepowershell-interactive
Remove-AzResourceGroup -Name "<your resource group name>"
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: Azure portal kullanarak Azure Güvenlik duvarını karma ağda dağıtma ve yapılandırma](tutorial-hybrid-portal.md)