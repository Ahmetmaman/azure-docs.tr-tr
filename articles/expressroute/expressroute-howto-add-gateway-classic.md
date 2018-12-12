---
title: 'İçin ExpressRoute - Klasik sanal ağ geçidi yapılandırma: Azure PowerShell | Microsoft Docs'
description: Klasik dağıtım için bir VNet ağ geçidi bir ExpressRoute yapılandırma için PowerShell kullanarak VNet model.
services: expressroute
author: charwen
ms.service: expressroute
ms.topic: article
ms.date: 11/05/2018
ms.author: charwen
ms.custom: seodec18
ms.openlocfilehash: 101e03d07a15e9058ef236a575251b052017db32
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53083296"
---
# <a name="configure-a-virtual-network-gateway-for-expressroute-using-powershell-classic"></a>PowerShell (Klasik) kullanarak ExpressRoute için sanal ağ geçidi yapılandırma
> [!div class="op_single_selector"]
> * [Resource Manager - PowerShell](expressroute-howto-add-gateway-resource-manager.md)
> * [Klasik - PowerShell](expressroute-howto-add-gateway-classic.md)
> * [Video - Azure portalı](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-vpn-gateway-for-your-virtual-network)
> 
> 

Bu makalede, ekleme, yeniden boyutlandırma ve önceden var olan bir VNet için bir sanal ağ (VNet) ağ geçidini kaldırmak için adımlarında size yol gösterir. Bu yapılandırmanın adımları kullanılarak oluşturulan özel sanal ağlar için olan **Klasik dağıtım modeli** ve ExpressRoute yapılandırması kullanılır. 

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]

**Azure dağıtım modelleri hakkında**

[!INCLUDE [vpn-gateway-classic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="before-beginning"></a>Başlamadan önce
Bu yapılandırma için gerekli Azure PowerShell cmdlet'lerinin yüklü olduğunu doğrulayın (1.0.2 veya üzeri). Cmdlet'ler yüklemediyseniz yapılandırma adımlarına başlamadan önce yapmanız gerekir. Azure PowerShell'i yükleme hakkında daha fazla bilgi için bkz. [Azure PowerShell'i yükleme ve yapılandırma işlemini](/powershell/azure/overview).

[!INCLUDE [expressroute-gateway-classic-ps](../../includes/expressroute-gateway-classic-ps-include.md)]

## <a name="next-steps"></a>Sonraki adımlar
VNet ağ geçidinin oluşturduktan sonra ağınız bir ExpressRoute bağlantı hattına bağlayabilirsiniz. Bkz: [bir sanal ağı ExpressRoute devresine bağlama](expressroute-howto-linkvnet-classic.md).

