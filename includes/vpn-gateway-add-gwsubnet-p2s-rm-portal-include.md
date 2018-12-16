---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 10/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 5642533fe1015e88c3b27e83139bfd26cb0b1a53
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/15/2018
ms.locfileid: "53444432"
---
1. [Portal](http://portal.azure.com)’da, sanal ağ geçidini oluşturmak istediğiniz Resource Manager sanal ağına gidin.
2. VNet’inizin sayfasının **Ayarlar** bölümünde, **Alt Ağlar** sayfasını genişletmek için **Alt Ağlar**'a tıklayın.
3. **Alt ağlar** sayfasında, **+Alt ağ**’a tıklayarak **Alt ağ ekle** sayfasını açın. 

   ![Ağ geçidi alt ağını ekleme](./media/vpn-gateway-add-gwsubnet-p2s-rm-portal-include/addgwsubnet.png "Ağ geçidi alt ağını ekleme")
4. Alt ağınız için **Ad** alanı otomatik olarak ‘GatewaySubnet’ değeriyle doldurulur. Alt ağın Azure tarafından ağ geçidi alt ağı olarak tanınması için bu değer gereklidir. Otomatik olarak doldurulmuş **Adres aralığı** değerlerini, yapılandırma gereksinimlerinize uyacak şekilde ayarlayın. Yol tablosu ya da hizmet uç noktaları yapılandırmayın.

   ![Alt ağı ekleme](./media/vpn-gateway-add-gwsubnet-p2s-rm-portal-include/p2sgwsub.png "Alt ağı ekleme")
5. Tıklayın **Tamam** alt ağ oluşturmak için sayfanın alt kısmındaki.
