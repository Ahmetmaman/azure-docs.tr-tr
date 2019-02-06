---
title: IPSec tünelleri yeniden kurmak için bir Azure VPN Gateway'i sıfırlama | Microsoft Docs
description: Bu makalede, IPSec tünelleri yeniden kurmak için Azure VPN Gateway'i sıfırlama adımları anlatılmaktadır. Makale VPN ağ geçitleri hem Klasik hem de Resource Manager dağıtım modelleri için geçerlidir.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: 79d77cb8-d175-4273-93ac-712d7d45b1fe
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/24/2017
ms.author: cherylmc
ms.openlocfilehash: 8db17b92208bd956bd5f9b855249f03ecd5e2c59
ms.sourcegitcommit: 039263ff6271f318b471c4bf3dbc4b72659658ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/06/2019
ms.locfileid: "55756708"
---
# <a name="reset-a-vpn-gateway"></a>VPN Gateway sıfırlama

Bir veya daha fazla Siteden Siteye VPN tünelinde şirketler arası VPN bağlantısını kaybederseniz bir Azure VPN ağ geçidinin sıfırlanması yararlıdır. Bu durumda şirket içi VPN cihazlarınızın tümü düzgün çalışır, ancak Azure VPN ağ geçitleriyle IPsec tünelleri kuramaz. Bu makalede VPN gateway'inizi sıfırlama yardımcı olur.

### <a name="what-happens-during-a-reset"></a>Sıfırlama sırasında ne olacak?

Bir VPN ağ geçidi bir etkin bekleme yapılandırmasında çalışan iki VM örneğinden oluşur. Ağ geçidini sıfırlama, ağ geçidini yeniden başlatır ve ardından şirketler arası yapılandırmalar yeniden uygular. Ağ geçidi, sahip olduğu genel IP adresini tutar. Bu durum VPN yönlendirici yapılandırmasını Azure VPN ağ geçidi için yeni bir ortak IP adresi ile güncelleştirmenizin gerekli olmadığı anlamına gelir.

Ağ geçidini sıfırlama komutu verdiğinizde, Azure VPN ağ geçidinin geçerli etkin örneği hemen yeniden. Bekleme örneğine etkin örnekten (yeniden başlatılan), yük devretme sırasında kısa bir boşluk olacaktır. Boşluk bir dakikadan az olmalıdır.

İlk yeniden başlatma sonrasında bağlantı geri kazanılmazsa ikinci sanal makine örneğini (yeni etkin ağ geçidi) yeniden başlatmak için aynı komutu yeniden verin. Arka arkaya iki yeniden başlatma istenirse her iki sanal makine örneğinin (etkin ve bekleme) yeniden başlatılma süresi biraz daha uzun olur. Bu, sanal makinelerin yeniden başlatma işlemlerini tamamlaması için VPN bağlantısı üzerinde 2 ila 4 dakikaya varan daha uzun bir boşluğa neden olur.

Şirketler arası bağlantı sorunları yaşamaya devam ediyorsanız, iki yeniden başlatma sonrasında, lütfen Azure portalından bir destek isteği açın.

## <a name="before"></a>Başlamadan önce

Ağ geçidinizi sıfırlamadan önce her bir IPsec Siteden Siteye (S2S) VPN tüneli için aşağıda listelenen anahtar öğeleri doğrulayın. Öğelerdeki uyuşmazlık S2S VPN tünellerinin bağlantısının kesilmesiyle sonuçlanır. Doğrulama ve Azure VPN ağ geçitleri ve şirket içi yapılandırmalarını düzeltme üzerinde çalışan diğer bağlantılar ağ geçitleri için gereksiz yeniden başlatmalardan ve gelen kaydeder.

Ağ geçidinizi sıfırlamadan önce aşağıdakileri doğrulayın:

* Hem Azure VPN ağ geçidi hem de şirket içi VPN ağ geçidi için İnternet IP adresleri (VIP) hem Azure hem de şirket içi VPN ilkelerinde doğru şekilde yapılandırılmıştır.
* Önceden paylaşılan anahtar Azure ve şirket içi VPN ağ geçitlerinde aynı olmalıdır.
* Şifreleme, karma algoritmaları ve PFS (Kusursuz İletme Gizliliği) gibi belirli IPsec/IKE yapılandırmalarını uygularsanız hem Azure hem de şirket içi VPN ağ geçitlerinin aynı yapılandırmalara sahip olduğundan emin olun.

## <a name="portal"></a>Azure portal

Azure portalını kullanarak Resource Manager VPN ağ geçidi sıfırlayabilirsiniz. Klasik bir ağ geçidi sıfırlamak istiyorsanız, bkz. [PowerShell](#resetclassic) adımları.

### <a name="resource-manager-deployment-model"></a>Resource Manager dağıtım modeli

1. Açık [Azure portalında](https://portal.azure.com) ve sıfırlamak istediğiniz Resource Manager sanal ağ geçidine gidin.
2. Sanal ağ geçidi dikey penceresinde, 'Sıfırla' seçeneğine tıklayın.

  ![VPN ağ geçidi dikey Sıfırla](./media/vpn-gateway-howto-reset-gateway/reset-vpn-gateway-portal.png)
3. Sıfırlama dikey penceresinde tıklayın **sıfırlama** düğmesi.

## <a name="ps"></a>PowerShell

### <a name="resource-manager-deployment-model"></a>Resource Manager dağıtım modeli

Bir ağ geçidini sıfırlamayı cmdlet'i **sıfırlama-AzureRmVirtualNetworkGateway**. Sıfırlama gerçekleştirmeden önce en son sürümüne sahip olduğunuzdan emin olun [Resource Manager PowerShell cmdlet'lerinin](https://docs.microsoft.com/powershell/azure/azurerm/install-azurerm-ps?view=azurermps-4.0.0). Aşağıdaki örnek TestRG1 kaynak grubunda VNet1GW adlı bir sanal ağ geçidi sıfırlar:

```powershell
$gw = Get-AzureRmVirtualNetworkGateway -Name VNet1GW -ResourceGroupName TestRG1
Reset-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw
```

Sonuç:

Dönüş sonucu aldığınızda, ağ geçidi sıfırlama başarılı olduğunu varsayabilirsiniz. Ancak, hiçbir şey yoktur açıkça sıfırlama başarılı olduğunu gösteren dönüş sonucu. Ağ geçidi sıfırlama oluştuğu tam olarak görmek için geçmiş yakından görünmesini istiyorsanız, bu bilgiyi görüntüleyebilirsiniz [Azure portalında](https://portal.azure.com). Portalda gidin **'GatewayName' -> kaynak durumu**.

### <a name="resetclassic"></a>Klasik dağıtım modeli

Bir ağ geçidini sıfırlamayı cmdlet'i **Reset-AzureVNetGateway**. Sıfırlama gerçekleştirmeden önce en son sürümüne sahip olduğunuzdan emin olun [Hizmet Yönetimi (SM) PowerShell cmdlet'lerini](https://docs.microsoft.com/powershell/azure/servicemanagement/install-azure-ps?view=azuresmps-4.0.0#azure-service-management-cmdlets). Aşağıdaki örnekte "ContosoVNet" adlı bir sanal ağ için ağ geçidi sıfırlar:

```powershell
Reset-AzureVNetGateway –VnetName “ContosoVNet”
```

Sonuç:

```powershell
Error          :
HttpStatusCode : OK
Id             : f1600632-c819-4b2f-ac0e-f4126bec1ff8
Status         : Successful
RequestId      : 9ca273de2c4d01e986480ce1ffa4d6d9
StatusCode     : OK
```

## <a name="cli"></a>Azure CLI

Ağ geçidini sıfırlamaya kullanın [az ağ vnet-gateway reset](https://docs.microsoft.com/cli/azure/network/vnet-gateway) komutu. Aşağıdaki örnek VNet5GW TestRG5 kaynak grubunda adlı bir sanal ağ geçidi sıfırlar:

```azurecli
az network vnet-gateway reset -n VNet5GW -g TestRG5
```

Sonuç:

Dönüş sonucu aldığınızda, ağ geçidi sıfırlama başarılı olduğunu varsayabilirsiniz. Ancak, hiçbir şey yoktur açıkça sıfırlama başarılı olduğunu gösteren dönüş sonucu. Ağ geçidi sıfırlama oluştuğu tam olarak görmek için geçmiş yakından görünmesini istiyorsanız, bu bilgiyi görüntüleyebilirsiniz [Azure portalında](https://portal.azure.com). Portalda gidin **'GatewayName' -> kaynak durumu**.
