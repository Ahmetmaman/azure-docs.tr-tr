---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 12/05/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 555a8e3e92dc1d12cb7c6d6e06d2511f15a2c862
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2018
ms.locfileid: "52973128"
---
|**SKU**   | **S2S/VNet-VNet<br>Tünelleri** | **P2S<br> SSTP Bağlantıları** | **P2S<br> IKEv2 Bağlantıları** | **Toplam<br>Aktarım Hızı Kıyaslaması** | **BGP** |
|---       | ---        | ---       | ---            | ---       | --- |
|**Temel** | En çok, 10    | En çok, 128  | Desteklenmiyor  | 100 Mbps  | Desteklenmiyor|
|**VpnGw1**| En çok, 30*   | En çok, 128  | En çok, 250       | 650 Mbps  | Desteklenen |
|**VpnGw2**| En çok, 30*   | En çok, 128  | En çok, 500       | 1 Gbps    | Desteklenen |
|**VpnGw3**| En çok, 30*   | En çok, 128  | En çok, 1000      | 1,25 Gb/sn | Desteklenen |


(*) 30'dan fazla S2S VPN tüneline ihtiyacınız varsa [Sanal WAN](../articles/virtual-wan/virtual-wan-about.md) kullanın.

* Toplam Aktarım Hızı Kıyaslaması, tek bir ağ geçidi üzerinde yer alan birden fazla tünelin ölçümlerine bağlıdır. Bir VPN Gateway’e yönelik Toplam Aktarım Hızı Karşılaştırması S2S ve P2S’nin birleşimidir. **Çok sayıda P2S bağlantısına sahipseniz S2S bağlantınız aktarım hızı sınırlamalarından dolayı olumsuz etkilenebilir.** Toplam aktarım hızı kıyaslaması Internet trafiğinin koşulları ve uygulamanızın davranışı nedeniyle garantili bir verimlilik değildir.

* Bu bağlantı sınırları ayrıdır. Örneğin, bir VpnGw1 SKU’sunda 128 SSTP bağlantısına ek olarak 250 IKEv2 bağlantısına sahip olabilirsiniz.

* Fiyatlandırma bilgileri [Fiyatlandırma](https://azure.microsoft.com/pricing/details/vpn-gateway) sayfasında bulunabilir.

* SLA (Hizmet Düzeyi Sözleşmesi) bilgilerine [SLA](https://azure.microsoft.com/support/legal/sla/vpn-gateway/) sayfasından ulaşılabilir.

* VPN ağ geçitleri için VpnGw1, VpnGw2 ve VpnGw3 yalnızca Resource Manager dağıtım modeli kullanıldığında desteklenir.

* Temel SKU eski SKU olarak kabul edilir. Temel SKU, belirli özellik sınırlamaları vardır. Yeni ağ geçidi SKU'ları birine bir temel SKU kullanan bir ağ geçidini yeniden boyutlandırma, bunun yerine VPN ağ geçidiniz geçemezsiniz içeren yeni bir SKU'ya için değiştirmeniz gerekir. Temel SKU kullanmadan önce gereksinim duyduğunuz özellik desteklendiğinden emin olun.
