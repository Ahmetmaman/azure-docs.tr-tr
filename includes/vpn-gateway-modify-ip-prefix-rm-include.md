---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 02/14/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 76a602ae722bd975e634631819ebc703e8896c98
ms.sourcegitcommit: c95e2d89a5a3cf5e2983ffcc206f056a7992df7d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/24/2020
ms.locfileid: "95554683"
---
### <a name="to-modify-local-network-gateway-ip-address-prefixes---no-gateway-connection"></a><a name="noconnection"></a>Yerel ağ geçidinin IP adresi ön eklerini değiştirmek için - ağ geçidi bağlantısı yok

Başka adres ön ekleri eklemek için:

1. LocalNetworkGateway için değişkeni ayarlayın.

   ```azurepowershell-interactive
   $local = Get-AzLocalNetworkGateway -Name Site1 -ResourceGroupName TestRG1
   ```
2. Ön ekleri değiştirin.

   ```azurepowershell-interactive
   Set-AzLocalNetworkGateway -LocalNetworkGateway $local `
   -AddressPrefix @('10.101.0.0/24','10.101.1.0/24','10.101.2.0/24')
   ```

Adres ön eklerini kaldırmak için:

  Artık gereği olmayan önekleri bırakın. Bu örnekte, artık 10.101.2.0/24 ön ekine gerek kalmaz (önceki örnekte), bu önek hariç olmak üzere yerel ağ geçidini güncelleştiririz.

1. LocalNetworkGateway için değişkeni ayarlayın.

   ```azurepowershell-interactive
   $local = Get-AzLocalNetworkGateway -Name Site1 -ResourceGroupName TestRG1
   ```
2. Ağ geçidini güncelleştirilmiş öneklerle ayarlayın.

   ```azurepowershell-interactive
   Set-AzLocalNetworkGateway -LocalNetworkGateway $local `
   -AddressPrefix @('10.101.0.0/24','10.101.1.0/24')
   ```

### <a name="to-modify-local-network-gateway-ip-address-prefixes---existing-gateway-connection"></a><a name="withconnection"></a>Yerel ağ geçidinin IP adresi ön eklerini değiştirmek için - ağ geçidi bağlantısı var

Ağ geçidi bağlantınız varsa ve yerel ağ geçidinizde bulunan IP adresi ön eklerini eklemek veya kaldırmak istiyorsanız aşağıdaki adımları sırasıyla uygulamanız gerekir. Bunun sonucunda, VPN bağlantınızda kesinti oluşur. IP adresi öneklerini değiştirirken, VPN ağ geçidini silmeniz gerekmez. Yalnızca bağlantıyı kaldırmanız gerekir.

1. Bağlantıyı kaldırın.

   ```azurepowershell-interactive
   Remove-AzVirtualNetworkGatewayConnection -Name VNet1toSite1 -ResourceGroupName TestRG1
   ```
2. Yerel ağ geçidini değiştirilen adres önekleriyle ayarlayın.
   
   LocalNetworkGateway için değişkeni ayarlayın.

   ```azurepowershell-interactive
   $local = Get-AzLocalNetworkGateway -Name Site1 -ResourceGroupName TestRG1
   ```
   
   Ön ekleri değiştirin.
   
   ```azurepowershell-interactive
   Set-AzLocalNetworkGateway -LocalNetworkGateway $local `
   -AddressPrefix @('10.101.0.0/24','10.101.1.0/24')
   ```
3. Bağlantıyı oluşturun. Bu örnekte bir IPsec bağlantı türü yapılandırıyoruz. Bağlantınızı yeniden oluşturduktan sonra, yapılandırmanız için belirtilen bağlantı türünü kullanın. Ek bağlantı türleri için [PowerShell cmdlet](/powershell/module/Azurerm.Network/New-AzureRmVirtualNetworkGatewayConnection) sayfasına bakın.
   
   VirtualNetworkGateway için değişkeni ayarlayın.

   ```azurepowershell-interactive
   $gateway1 = Get-AzVirtualNetworkGateway -Name VNet1GW  -ResourceGroupName TestRG1
   ```
   
   Bağlantıyı oluşturun. Bu örnek 2. adımda ayarladığınız $local değişkenini kullanır.

   ```azurepowershell-interactive
   New-AzVirtualNetworkGatewayConnection -Name VNet1toSite1 `
   -ResourceGroupName TestRG1 -Location 'East US' `
   -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
   -ConnectionType IPsec `
   -RoutingWeight 10 -SharedKey 'abc123'
   ```