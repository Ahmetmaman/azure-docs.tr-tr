---
title: Azure ExpressRoute ile ilgili SSS | Microsoft Docs
description: ExpressRoute SSS, desteklenen Azure hizmetlerini, maliyet, verileri ve bağlantıları, SLA'sı, sağlayıcıları ve konumları, bant genişliği ve ek teknik ayrıntılar hakkında bilgi içerir.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: cherylmc
ms.openlocfilehash: 3e2648bc019612835baa172210984d8fc1c6d801
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2018
ms.locfileid: "44304105"
---
# <a name="expressroute-faq"></a>ExpressRoute SSS

## <a name="what-is-expressroute"></a>ExpressRoute nedir?

ExpressRoute, Microsoft veri merkezleri ve şirket içindeki veya ortak yerleşim tesisinizden altyapınız arasında özel bağlantılar oluşturmanızı sağlayan bir Azure hizmetidir. ExpressRoute bağlantıları değil genel Internet üzerinden gidin ve daha yüksek güvenlik, güvenilirlik ve hız tipik daha düşük gecikme süreleriyle Internet üzerinden sunar.

### <a name="what-are-the-benefits-of-using-expressroute-and-private-network-connections"></a>ExpressRoute ve özel ağ bağlantılarıyla kullanmanın avantajları nelerdir?

ExpressRoute bağlantıları ortak İnternet üzerinden geçmemektedir. Bunlar, daha yüksek güvenlik, güvenilirlik ve hız, düşük ve tutarlı gecikme süresi Internet üzerinden genel bağlantılara sağlar. Bazı durumlarda arasında veri aktarmak için ExpressRoute bağlantıları kullanarak şirket içi cihazlar ile Azure önemli maliyet avantajları sağlayabilir.

### <a name="where-is-the-service-available"></a>Hizmet nerede kullanılabilir?

Hizmet konumu ve kullanılabilirlik için şu sayfaya bakın: [ExpressRoute iş ortakları ve konumları](expressroute-locations.md).

### <a name="how-can-i-use-expressroute-to-connect-to-microsoft-if-i-dont-have-partnerships-with-one-of-the-expressroute-carrier-partners"></a>Ben iş ortaklıkları taşıyıcı ExpressRoute iş ortakları ile yoksa, Microsoft'a bağlanmak için ExpressRoute nasıl kullanabilirim?

Bölgesel bir operatör seçin ve Ethernet bağlantı sağlayıcı konumları desteklenen exchange birine gelirsiniz. Sağlayıcı konumu Microsoft'ta ile eşleyebilirsiniz. Son Kısım denetleyin [ExpressRoute iş ortakları ve konumları](expressroute-locations.md) hizmet sağlayıcınıza exchange konumlardan herhangi birinde mevcut olup olmadığını görmek için. Ardından, Azure'a bağlanmak için hizmet sağlayıcısı aracılığıyla bir ExpressRoute bağlantı hattı sipariş edebilirsiniz.

### <a name="how-much-does-expressroute-cost"></a>ExpressRoute maliyeti ne kadar?

Denetleme [fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/expressroute/) fiyatlandırma bilgileri için.

### <a name="if-i-pay-for-an-expressroute-circuit-of-a-given-bandwidth-does-the-vpn-connection-i-purchase-from-my-network-service-provider-have-to-be-the-same-speed"></a>Belirli bir bant bir ExpressRoute bağlantı hattı için ödeme yapmam, VPN bağlantısını benim ağ hizmeti sağlayıcısı'ndan satın alırım aynı hızda olması gerekiyor mu?

Hayır. Bir VPN bağlantısı herhangi bir hızına hizmet sağlayıcınızdan satın alabilirsiniz. Ancak, bir Azure bağlantısı satın aldığınız ExpressRoute bağlantı hattı bant sınırlıdır.

### <a name="if-i-pay-for-an-expressroute-circuit-of-a-given-bandwidth-do-i-have-the-ability-to-burst-up-to-higher-speeds-if-necessary"></a>Belirli bir bant bir ExpressRoute bağlantı hattı için ödeme yapmam, yüksek hız kadar gerekirse geçmenize olanak zorundayım?

Evet. ExpressRoute bağlantı hatları, hiçbir ek ücret ödenmeden iki kereye kadar bant genişliği sınırını veri bloğu için izin verecek şekilde yapılandırılır. Bu özellik destekleyip desteklemediğini görmek için hizmet sağlayıcınıza başvurun.

### <a name="can-i-use-the-same-private-network-connection-with-virtual-network-and-other-azure-services-simultaneously"></a>Aynı özel ağ bağlantısı ile sanal ağ ve diğer Azure hizmetleriyle aynı anda kullanabilir miyim?

Evet. Kez ayarlamak, bir ExpressRoute bağlantı hattı, bir sanal ağ içindeki Hizmetler ve diğer Azure hizmetleriyle aynı anda erişmenize olanak sağlar. Özel eşleme yolu üzerinden sanal ağları ve Microsoft eşleme yolu üzerinden diğer hizmetler için bağlantı.

### <a name="does-expressroute-offer-a-service-level-agreement-sla"></a>ExpressRoute hizmet düzeyi sözleşmesi (SLA) sunduğu?

Bilgi için [ExpressRoute SLA'sı](https://azure.microsoft.com/support/legal/sla/) sayfası.

## <a name="supported-services"></a>Desteklenen hizmetler

ExpressRoute destekler [üç yönlendirme etki alanı](expressroute-circuit-peerings.md) çeşitli hizmetler için.

### <a name="private-peering"></a>Özel eşleme

* Tüm sanal makineler ve bulut hizmetleri de dahil olmak üzere, sanal ağlar

### <a name="public-peering"></a>Ortak eşleme

>[!NOTE]
>Ortak eşleme ExpressRoute devreleri üzerinde devre dışı bırakıldı. Azure Hizmetleri, Microsoft eşlemesi üzerinde kullanılabilir.
>

* Power BI
* Dynamics 365 Finans ve operasyon (eski adıyla Dynamics AX Online bilinir) için
* Azure hizmetlerinin çoğu desteklenir. Lütfen doğrudan destek doğrulamak için kullanmak istediğiniz hizmeti ile denetleyin.<br>
  Aşağıdaki hizmetler desteklenmez:
    * CDN
    * Azure DevOps Hizmetleri yük testi
    * Multi-factor Authentication
    * Traffic Manager

### <a name="microsoft-peering"></a>Microsoft eşlemesi

* [Office 365](http://aka.ms/ExpressRouteOffice365)
* Dynamics 365 
* Power BI
* Azure Active Directory
* Azure DevOps Hizmetleri yük testi
* Azure hizmetlerinin çoğu desteklenir. Lütfen doğrudan destek doğrulamak için kullanmak istediğiniz hizmeti ile denetleyin.<br>Aşağıdaki hizmetler **desteklenmiyor**:
    * CDN
    * Azure DevOps Hizmetleri yük testi
    * Multi-factor Authentication
    * Traffic Manager

## <a name="data-and-connections"></a>Veri ve bağlantıları

### <a name="are-there-limits-on-the-amount-of-data-that-i-can-transfer-using-expressroute"></a>ExpressRoute kullanarak aktarabilirsiniz veri miktarına bir sınır var mıdır?

Biz bir sınır veri aktarımı miktarı ayarlamayın. Başvurmak [fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/expressroute/) bant genişliği ücretleri hakkında bilgi için.

### <a name="what-connection-speeds-are-supported-by-expressroute"></a>Hangi bağlantı hızları ExpressRoute tarafından destekleniyor mu?

Bant genişliği teklifler desteklenir:

50 Mb/sn, 100 Mb/sn, 200 Mb/sn, 500 Mb/sn, 1 Gb/sn, 2 Gb/sn, 5 Gb/sn, 10 Gb/sn

### <a name="which-service-providers-are-available"></a>Hangi hizmet sağlayıcıları kullanılabilir mi?

Bkz: [ExpressRoute iş ortakları ve konumları](expressroute-locations.md) hizmet sağlayıcıları ve konumları listesi.

## <a name="technical-details"></a>Teknik Ayrıntılar

### <a name="what-are-the-technical-requirements-for-connecting-my-on-premises-location-to-azure"></a>Şirket içi konumunuzu Azure'a bağlamak için teknik gereksinimleri nelerdir?

Bkz: [ExpressRoute ön koşullar sayfasında](expressroute-prerequisites.md) gereksinimleri.

### <a name="are-connections-to-expressroute-redundant"></a>Bağlantılar için ExpressRoute yedekli misiniz?

Evet. Her ExpressRoute bağlantı hattı çapraz bağlantıları yüksek kullanılabilirlik sağlamak için yapılandırılmış bir yedek çifti var.

### <a name="will-i-lose-connectivity-if-one-of-my-expressroute-links-fail"></a>Bağlantı, ExpressRoute Bağlantılarım biri başarısız olursa kaybedersiniz?

Çapraz bağlantılarından biri başarısız olursa bağlantı kaybetmez. Yedekli bağlantı, Ağ Yükü desteklemek ve ExpressRoute devreniz yüksek kullanılabilirliğini sağlamak kullanılabilir. Ayrıca, bağlantı hattı düzeyinde esnekliği elde etmek için farklı bir eşleme konumda bir bağlantı hattı oluşturabilirsiniz.

### <a name="how-do-i-ensure-high-availability-on-a-virtual-network-connected-to-expressroute"></a>Expressroute'a bağlanan bir sanal ağ üzerinde yüksek kullanılabilirlik nasıl emin olabilirim?

Sanal ağınıza farklı konumlarda eşleme (örneğin, Singapur, singapur2) ExpressRoute devreleri bağlanarak yüksek kullanılabilirlik elde edebilirsiniz. Bir ExpressRoute bağlantı hattı kalırsa, bağlantı üzerinden başka bir ExpressRoute bağlantı hattına başarısız olur. Varsayılan olarak, sanal ağınızı trafiğe eşit maliyet çoklu yol yönlendirmesi (ECMP üzerinde) göre yönlendirilir. Bağlantı ağırlığına tek bir devreniz diğerine tercih etmek için kullanabilirsiniz. Daha fazla bilgi için [ExpressRoute yönlendirmeyi en iyi duruma getirme](expressroute-optimize-routing.md).

### <a name="onep2plink"></a>Bulut değişiminde ortak konumlu değilim ve benim hizmet sağlayıcısı, noktadan noktaya bağlantısı sunar, my şirket içi ağınız ile Microsoft arasında iki fiziksel bağlantıları sıralamak ihtiyacım var?

Hizmet sağlayıcınıza fiziksel bağlantı üzerinden iki Ethernet sanal bağlantı hatları oluşturabilir, yalnızca tek bir fiziksel bağlantı gerekir. Fiziksel bağlantı (örneğin, bir fiber optik) üzerinde bir katman 1 (L1) cihaz sonlandırılır (resme bakın). İki Ethernet sanal bağlantı hatları, birincil bağlantı hattı için bir tane ve bir ikincil için farklı VLAN kimliği ile etiketlenir. Bu VLAN kimlikleri dış 802.1Q Ethernet üstbilgisi ' dir. İç 802.1Q Ethernet üstbilgi (gösterilmemiştir) belirli bir eşlenmiş [ExpressRoute Yönlendirme etki alanı](expressroute-circuit-peerings.md).

![](./media/expressroute-faqs/expressroute-p2p-ref-arch.png)

### <a name="can-i-extend-one-of-my-vlans-to-azure-using-expressroute"></a>Ben my VLAN'ları birini için Azure ExpressRoute kullanarak uzatabilir miyim?

Hayır. Katman 2 bağlantısı uzantıları Azure'a desteklemiyoruz.

### <a name="can-i-have-more-than-one-expressroute-circuit-in-my-subscription"></a>Aboneliğimde birden fazla ExpressRoute bağlantı hattı olabilir mi?

Evet. Aboneliğinizde birden fazla ExpressRoute bağlantı hattı olabilir. Varsayılan sınır 10 olarak ayarlanır. Gerekirse sınırı artırmak için Microsoft Support başvurabilirsiniz.

### <a name="can-i-have-expressroute-circuits-from-different-service-providers"></a>ExpressRoute devreleri farklı hizmet sağlayıcılarından sahip olabilir miyim?

Evet. ExpressRoute bağlantı hatları birçok hizmet sağlayıcının sahip olabilir. Her ExpressRoute bağlantı hattı yalnızca bir hizmet sağlayıcısı ile ilişkilidir. 

### <a name="i-see-two-expressroute-peering-locations-in-the-same-metro-for-example-singapore-and-singapore2-which-peering-location-should-i-choose-to-create-my-expressroute-circuit"></a>İki ExpressRoute eşleme konumlarına aynı metro, örneğin, Singapur ve singapur2 görüyorum. ExpressRoute bağlantı hattımı oluşturmak eşleme konumu seçmeliyim?
Hizmet sağlayıcınız ExpressRoute iki sitelerdeki sunuyorsa, sağlayıcınızla birlikte çalışmanız ve ExpressRoute ' ayarlamak için her iki site seçin. 

### <a name="can-i-have-multiple-expressroute-circuits-in-the-same-metro-can-i-link-them-to-the-same-virtual-network"></a>Birden çok ExpressRoute bağlantı hatları aynı metro olabilir mi? Ben bunları aynı sanal ağa bağlayabilir miyim?

Evet. Aynı veya farklı hizmet sağlayıcıları ile birden çok ExpressRoute bağlantı hattına sahip olabilir. Birden fazla ExpressRoute eşleme konumlarına metro varsa ve bağlantı hatlarının eşleme farklı konumlarda oluşturulur, aynı sanal ağa bağlayabilirsiniz. Bağlantı hatlarının aynı eşleme konumunda oluşturduysanız, aynı sanal ağa bağlayamazsınız. Azure portalında veya PowerShell/CLI API'sindeki her konum adı bir eşleme konumundan temsil eder. Örneğin, "Singapur" ve "Singapur2" eşleme konumları seçin ve her aynı sanal ağa bağlantı hatları bağlanın. 

### <a name="how-do-i-connect-my-virtual-networks-to-an-expressroute-circuit"></a>My sanal ağları ExpressRoute devresine nasıl bağlanabilirim

Temel adımlar şunlardır:

* Bir ExpressRoute bağlantı hattı kurmak ve etkinleştirmediğiniz servis sağlayıcınız yoksa.
* BGP eşleme (s), veya sağlayıcı yapılandırmanız gerekir.
* Sanal ağı ExpressRoute devresine bağlama.

Daha fazla bilgi için [bağlantı hattı sağlama ve devre durumları için ExpressRoute iş akışları](expressroute-workflows.md).

### <a name="are-there-connectivity-boundaries-for-my-expressroute-circuit"></a>ExpressRoute bağlantı hattımı bağlantı sınırları vardır?

Evet. [ExpressRoute iş ortakları ve konumları](expressroute-locations.md) makale, bir ExpressRoute bağlantı hattı için bağlantı sınırları genel bir bakış sağlar. Bağlantı bir ExpressRoute bağlantı hattı için tek bir coğrafi bölgede sınırlıdır. Bağlantı, ExpressRoute premium özelliği etkinleştirerek coğrafi bölgeler arası için genişletilebilir.

### <a name="can-i-link-to-more-than-one-virtual-network-to-an-expressroute-circuit"></a>Bir ExpressRoute bağlantı hattı için birden fazla sanal ağa bağlayabilir miyim?

Evet. Standart bir ExpressRoute bağlantı hattı ve 100 adede kadar en fazla 10 sanal ağlara bağlantılar sahip bir [premium ExpressRoute bağlantı hattı](#expressroute-premium). 

### <a name="i-have-multiple-azure-subscriptions-that-contain-virtual-networks-can-i-connect-virtual-networks-that-are-in-separate-subscriptions-to-a-single-expressroute-circuit"></a>Sanal ağlar içeren birden çok Azure aboneliğiniz var. Tek bir ExpressRoute bağlantı hattı için farklı Aboneliklerde bulunan sanal ağlara bağlanabilir?

Evet. Tek bir ExpressRoute bağlantı hattı kullanmak için 10 adede kadar diğer Azure aboneliklerine yetki verebilir. Bu sınır, ExpressRoute premium özelliği etkinleştirerek artırılabilir.

Daha fazla bilgi için [bir ExpressRoute bağlantı hattı arasında birden çok abonelik paylaşımı](expressroute-howto-linkvnet-arm.md).

### <a name="i-have-multiple-azure-subscriptions-associated-to-different-azure-active-directory-tenants-or-enterprise-agreement-enrollments-can-i-connect-virtual-networks-that-are-in-separate-tenants-and-enrollments-to-a-single-expressroute-circuit-not-in-the-same-tenant-or-enrollment"></a>Farklı Azure Active Directory kiracıları veya Kurumsal Anlaşma kayıtlar için ilişkili birden çok Azure aboneliğiniz var. Ayrı kiracılar ve tek bir ExpressRoute bağlantı hattına aynı Kiracı veya kayıt kayıtları sanal ağlara bağlanabilir?

Evet. ExpressRoute yetkilendirmeleri abonelik, Kiracı ve kayıt sınırları ek yapılandırma gerektirmeden yayılabilir. 

Daha fazla bilgi için [bir ExpressRoute bağlantı hattı arasında birden çok abonelik paylaşımı](expressroute-howto-linkvnet-arm.md).

### <a name="are-virtual-networks-connected-to-the-same-circuit-isolated-from-each-other"></a>Sanal ağlar birbirlerinden aynı bağlantı hattına bağlı?

Hayır. Bir yönlendirme açısından aynı ExpressRoute bağlantı hattına bağlı tüm sanal ağları, aynı yönlendirme etki alanının parçası olan ve birbirinden yalıtılmış değildir. Rota yalıtım gerekiyorsa, ayrı bir ExpressRoute bağlantı hattı oluşturma gerekir.

### <a name="can-i-have-one-virtual-network-connected-to-more-than-one-expressroute-circuit"></a>Birden fazla ExpressRoute bağlantı hattına bağlı bir sanal ağa sahip olabilir miyim?

Evet. Tek bir sanal ağ ile en fazla dört ExpressRoute bağlantı hattına bağlayabilirsiniz. Bunlar dört farklı sıralanmalıdır [ExpressRoute konumları](expressroute-locations.md).

### <a name="can-i-access-the-internet-from-my-virtual-networks-connected-to-expressroute-circuits"></a>ExpressRoute bağlantı hatlarına bağlı my sanal ağlardan İnternet'e erişebilir miyim?

Evet. Varsayılan yol (0.0.0.0/0) ya da Internet rotası önekleri BGP oturumu üzerinden tanıtılan değil, bir ExpressRoute bağlantı hattına bağlı sanal ağdan Internet'e bağlanabilir.

### <a name="can-i-block-internet-connectivity-to-virtual-networks-connected-to-expressroute-circuits"></a>Ben, ExpressRoute bağlantı hatlarına bağlı sanal ağlar için Internet bağlantısı engelleyebilir miyim?

Evet. Dağıtılan bir sanal ağ içindeki sanal makinelerin tüm Internet bağlantısı engellemek için varsayılan yol (0.0.0.0/0) duyurmak ve tüm trafik ExpressRoute bağlantı hattı üzerinden çıkış yol.

Varsayılan yolları tanıtma, biz trafik (Azure depolama ve SQL DB gibi) eşlemesi Microsoft üzerinden sunulan hizmetler için şirket içinde geri zorlar. Microsoft eşleme yolu veya Internet üzerinden Azure'a trafiği döndürülecek yönlendiricilerinizi yapılandırma gerekir. Hizmeti için hizmet uç noktası etkinleştirdiyseniz, hizmet trafiği şirket içinde zorunlu değildir. Trafiğin Azure omurga ağında kalır. Hizmet uç noktaları hakkında daha fazla bilgi için bkz: [sanal ağ hizmet uç noktaları](../virtual-network/virtual-network-service-endpoints-overview.md?toc=%2fazure%2fexpressroute%2ftoc.json)

### <a name="can-virtual-networks-linked-to-the-same-expressroute-circuit-talk-to-each-other"></a>Aynı ExpressRoute bağlantı hattına bağlı sanal ağlar birbiriyle iletişim kurabilir?

Evet. Aynı ExpressRoute bağlantı hattına bağlı sanal ağ içinde dağıtılan sanal makinelerin birbiriyle iletişim kurabilir.

### <a name="can-i-use-site-to-site-connectivity-for-virtual-networks-in-conjunction-with-expressroute"></a>ExpressRoute ile birlikte sanal ağlar için siteden siteye bağlantı kullanabilir miyim?

Evet. ExpressRoute ile siteden siteye VPN'ler bulunabilir.

### <a name="can-i-move-a-virtual-network-from-site-to-site--point-to-site-configuration-to-use-expressroute"></a>Bir sanal ağı ExpressRoute kullanmak için siteden siteye / noktadan siteye yapılandırma taşıyabilir miyim?

Evet. Sanal ağınızdaki bir ExpressRoute ağ geçidi oluşturmanız gerekir. İşlemle ilişkili küçük bir kapalı kalma süresi yoktur.

### <a name="why-is-there-a-public-ip-address-associated-with-the-expressroute-gateway-on-a-virtual-network"></a>Neden bir sanal ağ ExpressRoute ağ geçidi ile ilişkili bir genel IP adresi var mı?

Genel IP adresi, yalnızca iç yönetimi için kullanılır ve sanal ağınızda güvenlik riskini oluşturmadığına.

### <a name="are-there-limits-on-the-number-of-routes-i-can-advertise"></a>Tanıtabilir miyim yolların sayısına yönelik sınırlar var mıdır?

Evet. En fazla 4000 rota önekleri özel eşleme ve Microsoft eşlemesi için 200 kabul. Bu, ExpressRoute premium özelliğini etkinleştirirseniz, özel eşdüzey hizmet sağlama için 10.000 yollar artırabilir.

### <a name="are-there-restrictions-on-ip-ranges-i-can-advertise-over-the-bgp-session"></a>BGP oturumunda tanıtmayı IP aralıkları kısıtlamalar var mı?

Size özel önekleri (RFC1918) için Microsoft eşleme BGP oturumu kabul etmeyin.

### <a name="what-happens-if-i-exceed-the-bgp-limits"></a>Sınırlar BGP aşarsam ne olur?

BGP oturumu düşürülür. Bunlar sınırın altına ön ek sayısı ölçeklendirilinceye sonra sıfırlanır.

### <a name="what-is-the-expressroute-bgp-hold-time-can-it-be-adjusted"></a>ExpressRoute BGP Durma süresini nedir? Ayarlanabilir mi?

Durma süresini 180'dir. 60 saniyede gönderilen etkin tutma iletileri. Bu ayarlar değiştirilemez Microsoft tarafında sabittir. Farklı zamanlayıcılar yapılandırmak için mümkündür ve BGP oturumu parametreleri uygun şekilde gerçekleştirilir.

### <a name="can-i-change-the-bandwidth-of-an-expressroute-circuit"></a>ExpressRoute bağlantı hattının bant genişliğini değiştirebilirim?

Evet, Azure portalında veya PowerShell kullanarak ExpressRoute bağlantı hattı bant artırmak deneyebilirsiniz. Değişikliğiniz varsa kapasite kullanılabilir bağlantı hattınızın oluşturulduğu fiziksel bağlantı noktası üzerinde başarılı olur. 

Değişikliğinizi başarısız, ya da geçerli bağlantı noktası üzerinde sol yeterli kapasite yoktur anlamına gelir ve daha yüksek bant genişliği ile yeni bir ExpressRoute bağlantı hattı oluşturmak gereken veya o konumda hiç ek kapasite olan ise bu durumda, artırmak mümkün olmayacaktır bant genişliği. 

Bant genişliğini artırmanız desteklemek için kendi ağları içinde kısıtlamalar güncelleştirdiğinizden emin olmak için bağlantı sağlayıcınız ile takip gerekecektir. Ancak, ExpressRoute bağlantı hattı bant indiremezsiniz. Düşük bant genişliği ile yeni bir ExpressRoute bağlantı hattı oluşturma ve eski bağlantı hattını Sil gerekir.

### <a name="how-do-i-change-the-bandwidth-of-an-expressroute-circuit"></a>ExpressRoute bağlantı hattının bant genişliğini nasıl değiştirebilirim?

REST API'si veya PowerShell cmdlet'ini kullanarak ExpressRoute bağlantı hattı bant güncelleştirebilirsiniz.

## <a name="expressroute-premium"></a>ExpressRoute premium

### <a name="what-is-expressroute-premium"></a>ExpressRoute premium nedir?

ExpressRoute premium, aşağıdaki özellikler koleksiyonudur:

* Yönlendirme tablosu sınırından 4000 yolların özel eşdüzey hizmet sağlama için 10.000 yollar artırdık.
* Artırılmış ExpressRoute işlem hattına bağlı sanal ağlar sayısı (varsayılan: 10). Daha fazla bilgi için [ExpressRoute sınırları](#limits) tablo.
* Office 365 ve Dynamics 365 bağlantısı.
* Microsoft Çekirdek ağı üzerinden genel bağlantı. Artık jeopolitik bir bölgedeki bir sanal ağ ile ExpressRoute bağlantı hattına başka bir bölgede de bağlayabilirsiniz.<br>
    **Örnekler:**

    *  Silikon Vadisi'nde oluşturulan bir ExpressRoute devresi için Batı Avrupa'da oluşturulan bir sanal ağa bağlayabilirsiniz. 
    *  İçin örneğin, SQL Azure Batı Avrupa, Silikon vadisi, bağlantı hattı bağlantı kurabilirsiniz olacak şekilde Microsoft eşlemesi, diğer coğrafi bölgelerdeki ön eklerin tanıtılıp.


### <a name="limits"></a>Ben ExpressRoute premium etkinleştirilirse kaç sanal ağlar miyim ExpressRoute devresine bağlayabilir miyim?

Aşağıdaki tablolarda, ExpressRoute sınırları ve ExpressRoute bağlantı hattı başına Vnet sayısı gösterilmektedir:

[!INCLUDE [ExpressRoute limits](../../includes/expressroute-limits.md)]

### <a name="how-do-i-enable-expressroute-premium"></a>ExpressRoute premium nasıl etkinleştirebilirim?

ExpressRoute premium özellikleri özelliği etkinleştirilmişse etkinleştirilebilir ve bağlantı hattı durumu güncelleştirerek kapatılabilir. ExpressRoute premium devresi oluşturma zamanında etkinleştirebilir veya REST API çağrısı / PowerShell cmdlet'i.

### <a name="how-do-i-disable-expressroute-premium"></a>ExpressRoute premium nasıl devre dışı bırakabilirim?

ExpressRoute premium, REST API veya PowerShell cmdlet'i çağırarak devre dışı bırakabilirsiniz. ExpressRoute premium devre dışı bırakmadan önce varsayılan limitleri karşılamak için bağlantı gereksinimlerinizi ölçeği emin olmanız gerekir. Varsayılan sınırları aşacak kullanımınızı ölçeklendirir ExpressRoute premium devre dışı bırakma isteği başarısız olur.

### <a name="can-i-pick-and-choose-the-features-i-want-from-the-premium-feature-set"></a>Çekme ve özellikleri zengin özellik kümesinden seçebilir miyim?

Hayır. Özellikleri seçemezsiniz. ExpressRoute premium üzerinde etkinleştirdiğinizde tüm özelliklerini etkinleştiririz.

### <a name="how-much-does-expressroute-premium-cost"></a>ExpressRoute premium maliyeti ne kadar?

Başvurmak [fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/expressroute/) maliyeti.

### <a name="do-i-pay-for-expressroute-premium-in-addition-to-standard-expressroute-charges"></a>ExpressRoute Premium standart ExpressRoute ücretlerine ek olarak ödeme yapabilirim?

Evet. ExpressRoute bağlantı hattı bağlantı sağlayıcı tarafından gerekli süreliğine ve üstünde ExpressRoute premium ücretleri uygulanır.

## <a name="expressroute-for-office-365"></a>Office 365 için ExpressRoute

[!INCLUDE [expressroute-office365-include](../../includes/expressroute-office365-include.md)]

### <a name="how-do-i-create-an-expressroute-circuit-to-connect-to-office-365-services"></a>Office 365 hizmetlerine bağlanan bir ExpressRoute bağlantı hattı nasıl oluşturulur?

1. Gözden geçirme [ExpressRoute ön koşullar sayfasında](expressroute-prerequisites.md) gereksinimleri karşıladığından emin olmak için.
2. Bağlantı gereksinimlerinizi karşılandığından emin olmak için hizmet sağlayıcıları ve konumları olarak listesini gözden geçirin. [ExpressRoute iş ortakları ve konumları](expressroute-locations.md) makalesi.
3. Gözden geçirerek kapasite gereksinimlerinizi planlayın [ağ planlama ve Office 365 için performans ayarlama](http://aka.ms/tune/).
4. Bağlantı kurmak için iş akışlarında listelenen adımları takip [bağlantı hattı sağlama ve devre durumları için ExpressRoute iş akışları](expressroute-workflows.md).

> [!IMPORTANT]
> Office 365 hizmetlerine bağlantıyı yapılandırırken ExpressRoute premium eklentisi etkinleştirdiğinizden emin olun.
> 
> 

### <a name="can-my-existing-expressroute-circuits-support-connectivity-to-office-365-services-and-dynamics-365"></a>Uygulamam var olan ExpressRoute bağlantı hatları, Office 365 Hizmetleri ve Dynamics 365 bağlantısı destekleyebilir mi?

Evet. Office 365 hizmetlerine bağlantıyı desteklemek için mevcut bir ExpressRoute bağlantı hattınızı yapılandırılabilir. Premium eklenti etkin ve Office 365 hizmetlerine bağlanmak için yeterli kapasiteleri olduğundan emin olun. [Ağ planlama ve Office 365 için performans ayarlama](http://aka.ms/tune/) bağlantınızı planladığınız yardımcı gerekir. Ayrıca bkz [oluşturun ve bir ExpressRoute bağlantı hattını değiştirme](expressroute-howto-circuit-classic.md).

### <a name="what-office-365-services-can-be-accessed-over-an-expressroute-connection"></a>Hangi Office 365 Hizmetleri, bir ExpressRoute bağlantısı üzerinden erişilebilir?

Başvurmak [Office 365 URL'leri ve IP adresi aralıkları](http://aka.ms/o365endpoints) sayfa ExpressRoute üzerinde desteklenen hizmetlerin güncel bir listesi.

### <a name="how-much-does-expressroute-for-office-365-services-cost"></a>Ne kadar ExpressRoute için Office 365 Hizmetleri maliyeti?

Office 365 Hizmetleri premium eklenti, etkin olmasını gerektirir. Bkz: [fiyatlandırma ayrıntıları sayfasına](https://azure.microsoft.com/pricing/details/expressroute/) maliyetleri için.

### <a name="what-regions-is-expressroute-for-office-365-supported-in"></a>Office 365 için ExpressRoute hangi bölgeler desteklenir?

Bkz: [ExpressRoute iş ortakları ve konumları](expressroute-locations.md) bilgi.

### <a name="can-i-access-office-365-over-the-internet-even-if-expressroute-was-configured-for-my-organization"></a>ExpressRoute Kuruluşum için yapılandırılmış olsa bile Internet üzerinden Office 365 erişebilirim?

Evet. ExpressRoute ağınız için yapılandırılmış olsa bile, office 365 hizmet uç noktaları Internet erişilebilir. Konumunuz ağ, ExpressRoute aracılığıyla Office 365 hizmetlerine bağlanmak için yapılandırılmışsa, kuruluşunuzun ağ ekibinizle birlikte denetleyin.

### <a name="how-can-i-plan-for-high-availability-for-office-365-network-traffic-on-azure-expressroute"></a>Nasıl Office 365 ağ trafiği için yüksek kullanılabilirlik için Azure ExpressRoute planlıyorum?
Öneri için bkz. [yüksek kullanılabilirlik ve Azure ExpressRoute ile yük devretme](https://aka.ms/erhighavailability)

### <a name="can-i-access-office-365-us-government-community-gcc-services-over-an-azure-us-government-expressroute-circuit"></a>Office 365 US Government Community (GCC) Hizmetleri Azure ABD kamu ExpressRoute devresi erişebilir miyim?

Evet. Office 365 GCC hizmet uç noktaları, Azure ABD kamu ExpressRoute aracılığıyla erişilebilir. Ancak, ilk Microsoft'a bildirmek için istediğinize önekleri sağlamak için Azure portalında bir destek bileti açmanız gerekir. Destek bileti çözümlendikten sonra Office 365 GCC hizmetlerine bağlantı kurulur. 

## <a name="route-filters-for-microsoft-peering"></a>Microsoft eşlemesi için rota filtreleri

### <a name="i-am-turning-on-microsoft-peering-for-the-first-time-what-routes-will-i-see"></a>I 'M kapatma ilk kez, Microsoft eşlemesi üzerinde hangi rotalar görüyorum?

Tüm rotalar görmezsiniz. Devreniz önek tanıtımları başlatmak için bir rota filtresinde eklemek zorunda. Yönergeler için [Microsoft eşlemesi için rota filtreleri yapılandırma](how-to-routefilter-powershell.md).

### <a name="i-turned-on-microsoft-peering-and-now-i-am-trying-to-select-exchange-online-but-it-is-giving-me-an-error-that-i-am-not-authorized-to-do-it"></a>Microsoft eşlemesi üzerinde açtım ve Exchange Online'ı seçmek artık çalışıyorum, ancak bana miyim yapmak için yetkilendirilmemiş hata veriyor.

Rota filtrelerini kullanırken, tüm müşteriler, Microsoft eşlemesi üzerinde etkinleştirebilirsiniz. Ancak, Office 365 hizmetlerini kullanma için Office 365 tarafından yetkili yine.

### <a name="do-i-need-to-get-authorization-for-turning-on-dynamics-365-over-microsoft-peering"></a>Alma üzerinde Dynamics 365, Microsoft eşlemesi üzerinden açma için yetkilendirme gerekiyor mu?

Hayır, yetkilendirme için Dynamics 365 gerekmez. Bir kural oluşturmak ve Dynamics 365 topluluğu yetkilendirme olmadan seçin.

### <a name="i-enabled-microsoft-peering-prior-to-august-1-2017-how-can-i-take-advantage-of-route-filters"></a>Ben Microsoft nasıl rota filtreleri,'ndan yararlanabilir miyim 1 Ağustos 2017'den önce eşleme etkin mi?

Mevcut bağlantı hattınız için Office 365 ve Dynamics 365 öneklerinin reklam devam eder. Aynı Microsoft eşlemesi üzerinden Azure genel öneklerinin reklam eklemek istiyorsanız, bir rota filtresinde oluşturabilir, (ihtiyacınız Office 365 Hizmetleri ve Dynamics 365 gibi) tanıtılan gereksinim duyduğunuz hizmetleri seçin ve Microsoft filtre ekleme eşleme. Yönergeler için [Microsoft eşlemesi için rota filtreleri yapılandırma](how-to-routefilter-powershell.md).

### <a name="i-have-microsoft-peering-at-one-location-now-i-am-trying-to-enable-it-at-another-location-and-i-am-not-seeing-any-prefixes"></a>Microsoft eşlemesi bir konumda sahibim, artık başka bir konumda etkinleştirmek getirmeye çalışıyorum ve tüm ön ekleri görüyorum değil.

* 1 Ağustos 2017'den önce yapılandırılmış olan ExpressRoute devrelerinin Microsoft eşdüzey hizmet sağlama, tüm hizmet ön eklerin rota filtreleri tanımlanmamış olsa bile, Microsoft eşlemesi tanıtılan sahip olur.

* 1 Ağustos 2017 veya sonrasında yapılandırılmış ExpressRoute devrelerinin Microsoft eşlemesi tüm ön ekleri olmaz bağlantı hattına bir rota filtresinde bağlanana kadar tanıtılan. Varsayılan olarak, hiçbir ön ekleri görürsünüz.
