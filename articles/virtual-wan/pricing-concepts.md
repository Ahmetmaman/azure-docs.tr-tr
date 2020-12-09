---
title: Sanal WAN fiyatlandırması hakkında
titleSuffix: Azure Virtual WAN
description: Bu makalede ortak sanal WAN fiyatlandırma soruları açıklanmaktadır
services: virtual-wan
author: reyandap
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 12/08/2020
ms.author: reyandap
ms.custom: references_pricing
ms.openlocfilehash: 8ced9bb33be341d35904967092414676a6ffe3ac
ms.sourcegitcommit: 80c1056113a9d65b6db69c06ca79fa531b9e3a00
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/09/2020
ms.locfileid: "96905545"
---
# <a name="about-virtual-wan-pricing"></a>Sanal WAN fiyatlandırması hakkında

Azure sanal WAN, birden çok ağ ve güvenlik hizmetini birleştirilmiş bir çerçevede birlikte sunar. Hub 'lar Merkez içinde sunulan VPN, ExpressRoute, kullanıcı VPN (Noktadan siteye), güvenlik duvarı, yönlendirme vb. gibi çeşitli hizmetlerle birlikte Microsoft tarafından yönetilen, hub ve bağlı bileşen mimarisine dayalıdır.

Sanal WAN 'daki her hizmet fiyatlandırılır. Bu nedenle, tek bir fiyat önermesi sanal WAN için geçerli değildir. [Azure Fiyatlandırma Hesaplayıcı](https://azure.microsoft.com/pricing/calculator/) , sanal bir WAN 'da sağlanan hizmetleri temel alan maliyeti türetmeye yönelik bir mekanizma sağlar. Bu makalede, sanal WAN fiyatlandırması hakkında sık sorulan sorular ele alınmaktadır.

>[!NOTE]
>Geçerli fiyatlandırma bilgileri için bkz. [sanal WAN fiyatlandırması](https://azure.microsoft.com/pricing/details/virtual-wan/).
>

## <a name="common-pricing-questions"></a><a name="questions"></a>Genel fiyatlandırma soruları

### <a name="what-is-a-scale-unit"></a><a name="scale-unit"></a>Ölçek birimi nedir?

**Ölçek birimi** , bir sanal hub 'daki siteden sıteye (S2S), noktadan sıteye (P2S) ve EXPRESSROUTE (er) için toplam kapasiteye yönelik birimi sağlar. Örnek:

* **1 S2S VPN ölçek birimi** , bir sanal hub maliyetlendirme $0.361/saat içinde toplam 500 Mbps VPN Gateway (çift örnek esneklik için dağıtılır) kapasitesini belirtir.
* **1 er ölçek birimi** , sanal hub maliyetlendirme $0.42/HR içinde toplam 2 Gbps er ağ geçidi gerektirir.
* **5 er ölçek birimi** , bir sanal hub VNET içinde toplam 10 Gbps er Gateway, $0.42 * 5/hr ile ücretlendirilir. ER, 6. ve 10 ölçeğinde bir birim olan $0,25/HR değerini artırır.

### <a name="what-is-a-connection-unit"></a><a name="connection-unit"></a>Bağlantı birimi nedir?

Bir **bağlantı birimi** , Azure ağ geçitlerine bağlanan şirket Içi/Microsoft dışı uç nokta için geçerlidir. Siteden siteye VPN için bu değer dallar anlamına gelir. Kullanıcı VPN (Noktadan siteye) için bu değer uzak kullanıcıları belirtir. ExpressRoute için bu değer ExpressRoute bağlantı hattı bağlantıları anlamına gelir.<br>Örnek:

* Bir şube bağlantısı sanal hub maliyetlerinde Azure VPN 'ye bağlanıyor $0,05/hr. Bu nedenle 100 şube bağlantısı, Azure sanal hub 'a bağlanan şube bağlantıları maliyetlidir * 100/hr.

* Bir sanal hub 'a bağlanan iki ExpressRoute bağlantı hattı bağlantısı, $0,05 * 2/hr maliyetlidir.

* Azure sanal hub P2S Gateway 'e bağlanan üç uzak kullanıcı bağlantısı maliyeti $0.03 * 3/hr olacaktır.

### <a name="how-are-data-transfer-charges-calculated"></a><a name="data-transfer"></a>Veri aktarımı ücretleri nasıl hesaplanır?

* Azure 'a giren tüm trafik ücretlendirilmez. Azure 'dan gelen trafik (VPN, ExpressRoute veya Noktadan siteye kullanıcı VPN bağlantıları aracılığıyla), standart [Azure veri aktarımı ücretlerine](https://azure.microsoft.com/pricing/details/bandwidth/)tabidir.

* Bir sanal WAN hub 'ı ile uzak bir sanal WAN hub 'ı veya VNet arasında kaynak hub 'ından farklı bir bölgedeki veri aktarımı ücretleri için veri aktarımı ücretleri bir hub 'dan çıkmadan trafik için geçerlidir. Örnek: bir Doğu ABD hub 'ı terk eden trafik, bir Batı ABD hub 'ına giderek $0,02/GB üzerinden ücretlendirilir. Batı ABD hub 'ına giren trafik için ücret alınmaz. Hub 'a giden tüm trafik Inter-Region, [Azure veri aktarımı ücretlerine](https://azure.microsoft.com/pricing/details/bandwidth/)göre ücretlendirmelere tabidir. 

### <a name="what-is-the-difference-between-a-standard-hub-fee-and-a-standard-hub-processing-fee"></a><a name="fee"></a>Standart bir hub ücreti ve standart Merkez işleme ücreti arasındaki fark nedir?

Sanal WAN iki şekilde gelir:

* Kullanıcıların birden çok hub dağıtabilecek ve VPN siteden siteye bağlantısının tadını çıkartabileceğiniz **temel bir sanal WAN**. Temel bir sanal WAN, tam olarak sunulan hub 'lar, ExpressRoute bağlantısı, kullanıcı VPN/Noktadan siteye VPN bağlantısı, VNet-VNet geçişli bağlantı, VPN ve ExpressRoute aktarım bağlantısı veya Azure Güvenlik Duvarı gibi gelişmiş yeteneklere sahip değildir. Temel bir sanal WAN 'da hub 'lar için temel ücret veya veri işleme ücreti yoktur.

* **Standart sanal WAN** , tam olarak sunulan hub 'Lar, ExpressRoute bağlantısı, kullanıcı VPN/Noktadan siteye VPN bağlantısı, VNet-VNET geçişli bağlantı, VPN ve ExpressRoute aktarım bağlantısı, Azure Güvenlik duvarı vb. gibi gelişmiş özellikleri sağlar. Tüm sanal hub yönlendirmesi, sanal hub 'da birden çok hizmeti sağlayan bir yönlendirici tarafından sağlanır. Hub için, $0,25/HR ile fiyatlandırıldıkları temel bir ücret mevcuttur. VNet 'ten VNet 'e aktarım bağlantısı için sanal hub yönlendiricisinde veri işleme ücreti de vardır. Aşağıdaki şekle bakın.

 Aşağıdaki **örnekte** , VNET 1 ve VNET 2 olmak üzere iki sanal ağ vardır. VNET 1 ' deki bir VM 'den VNET 2 ' deki başka bir VM 'ye 1 GB/sn veri gönderildiğini varsayalım. Aşağıdaki ücretler geçerlidir:

* Sanal Merkez taban ücreti $0,25/SA

* 1 GB/sn için sanal merkez veri işleme ücreti $0,02/GB

**Örnek**

   :::image type="content" source="./media/pricing-concepts/figure-1.png" alt-text="Örnek":::

## <a name="next-steps"></a>Sonraki adımlar

* Sanal WAN hakkında daha fazla bilgi için bkz. [SSS](virtual-wan-faq.md).

* Geçerli fiyatlandırma için bkz. [sanal WAN fiyatlandırması](https://azure.microsoft.com/pricing/details/virtual-wan/).