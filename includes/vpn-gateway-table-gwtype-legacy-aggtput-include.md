---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 9734859c0bf22201c146e5d8a220f3146f6051c4
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "67188261"
---
Aşağıdaki tabloda ağ geçidi türleri ve ağ geçidi SKU’suna göre tahmini toplam verimlilik gösterilmiştir. Bu tablo Kaynak Yöneticisi ve klasik dağıtım modellerine yöneliktir. 

Ağ geçidi SKU'ları arasında fiyatlandırma farklılık gösterir. Daha fazla bilgi için bkz. [VPN Gateway Fiyatlandırması](https://azure.microsoft.com/pricing/details/vpn-gateway).

UltraPerformance ağ geçidi SKU’sunun bu tabloda temsil edilmediğini unutmayın. UltraPerformance SKU’su hakkında bilgi edinmek için [ExpressRoute](../articles/expressroute/expressroute-about-virtual-network-gateways.md) belgelerine bakın.

|  | **VPN Gateway performansı (1)** | **VPN Gateway maks. IPsec tüneli (2)** | **ExpressRoute Gateway performansı** | **VPN Gateway ve ExpressRoute bir arada** |
| --- | --- | --- | --- | --- |
| **Temel SKU (3)(5)(6)** |100 Mbps |10 |500 Mbps (6) |Hayır |
| **Standart SKU (4)(5)** |100 Mbps |10 |1000 Mb/sn |Yes |
| **Yüksek Performanslı SKU (4)** |200 Mb/sn |30 |2000 Mb/sn |Yes |


(1) VPN işlemesi aynı Azure bölgesinde yer alan Vnet'ler arasındaki ölçümleri temel alan kaba bir tahmindir. Bu seçenek, İnternet üzerinden kurulan şirket içi ve şirket dışı karışık bağlantılar için garantili bir verimlilik değildir. Mümkün olan en yüksek verimlilik ölçümüdür.

(2) RouteBased VPN’lere başvuran tünel sayısı. PolicyBased VPN yalnızca tek bir Siteden Siteye VPN tünelini destekleyebilir.

(3) BGP, Temel SKU için desteklenmez.

(4) PolicyBased VPN'ler bu SKU için desteklenmez. Bunlar yalnızca Temel SKU için desteklenir.

(5) Etkin-etkin S2S VPN Gateway bağlantıları bu SKU için desteklenmiyor. Etkin-etkin, yalnızca Yüksek Performanslı SKU üzerinde desteklenir.

(6) temel SKU 'SU ExpressRoute ile kullanım için kullanım dışıdır.
