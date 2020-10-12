---
title: dosya dahil etme
description: dosya dahil etme
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: b050d0cd5f6a21757685f9bc0991f8ce0a971e3c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "67188193"
---
VPN bağlantınız üzerinden bir sanal makineye bağlanmakta sorun yaşıyorsanız aşağıdaki kontrolleri yapın:

- VPN bağlantınızın başarılı olduğunu doğrulayın.
- VM’nin özel IP adresine bağlandığınızı doğrulayın.
- Bilgisayar adı yerine özel IP adresini kullanarak VM ile bağlantı kurabiliyorsanız, DNS’i düzgün yapılandırdığınızdan emin olun. VM’ler için ad çözümlemesinin nasıl çalıştığı hakkında daha fazla bilgi için bkz. [VM'ler için Ad Çözümlemesi](../articles/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).

Noktadan Siteye bağlantı üzerinden bağlandığınızda aşağıdaki ek denetimleri yapın:

- 'ipconfig' seçeneğini kullanarak bağlantıyı kurduğunuz bilgisayardaki Ethernet bağdaştırıcısına atanmış IPv4 adresini denetleyin. IP adresi bağlanacağınız sanal ağın adres aralığında veya VPNClientAddressPool adres aralığında ise, bu durum çakışan bir adres alanı olarak adlandırılır. Adres alanınız bu şekilde çakıştığında, ağ trafiği Azure’a ulaşmaz ve yerel ağda kalır.
- Sanal ağ için DNS sunucusu IP adresleri belirtildikten sonra VPN istemci yapılandırma paketinin oluşturulduğunu doğrulayın. DNS sunucusu IP adreslerini güncelleştirdiyseniz, yeni bir VPN istemci yapılandırma paketi oluşturup yükleyin.

Bir RDP bağlantısıyla ilgili sorun giderme hakkında daha fazla bilgi için bkz. [Bir VM ile Uzak Masaüstü bağlantılarında sorun giderme](../articles/virtual-machines/windows/troubleshoot-rdp-connection.md).