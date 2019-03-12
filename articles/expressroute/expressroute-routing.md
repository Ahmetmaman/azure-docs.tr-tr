---
title: 'Gereksinimleri - ExpressRoute yönlendirme: Azure | Microsoft Docs'
description: Bu sayfada, ExpressRoute devreleri için yönlendirmeyi yapılandırma ve yönetmeye yönelik ayrıntılı gereksinimler verilmektedir.
services: expressroute
author: ganesr
ms.service: expressroute
ms.topic: conceptual
ms.date: 01/11/2019
ms.author: ganesr
ms.custom: seodec18
ms.openlocfilehash: d3d4bbb0b9007ef61c96f980b9f3c3ee19dd11b5
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2019
ms.locfileid: "57539131"
---
# <a name="expressroute-routing-requirements"></a>ExpressRoute yönlendirme gereksinimleri
Microsoft bulut hizmetlerine ExpressRoute kullanarak bağlanmak için yönlendirmeyi ayarlamanız ve yönetmeniz gerekir. Bazı bağlantı sağlayıcıları yönlendirme ayarlama ve yönetimini yönetilen bir hizmet olarak sunar. Bu hizmetin sunulup sunulmadığını öğrenmek için bağlantı sağlayıcınıza başvurun. Bu hizmet sağlanmıyorsa aşağıdaki gereksinimlere uymalısınız:

Bağlantıyı kolaylaştırmak üzere ayarlanması gereken yönlendirme oturumlarının bir açıklaması için [Devreler ve yönlendirme etki alanları](expressroute-circuit-peerings.md) makalesine bakın.

> [!NOTE]
> Microsoft, yüksek kullanılabilirlik yapılandırmaları için yönlendirici artıklık protokollerini (örn. HSRP, VRRP) desteklemez. Yüksek kullanılabilirlik için eşlik başına yedek bir BGP oturumları çifti kullanılır.
> 
> 

## <a name="ip-addresses-used-for-peerings"></a>Eşlemeler için kullanılan IP adresleri
Ağınız ile Microsoft'un Enterprise edge (MSEE) yönlendiricileri arasındaki yönlendirmeyi yapılandırmak için birkaç IP adresi bloğu ayırmanız gerekir. Bu bölümde gereksinimlerin bir listesi verilmekte ve bu IP adreslerinin nasıl elde edilip kullanılacağı hakkında kurallar açıklanmaktadır.

### <a name="ip-addresses-used-for-azure-private-peering"></a>Azure özel eşleme için kullanılan IP adresleri
Eşlikleri yapılandırmak için özel IP adresleri veya ortak IP adresleri kullanabilirsiniz. Yolları yapılandırmak için kullanılan adres aralığı, Azure’da sanal ağlar oluşturmak için kullanılan adres aralıkları ile üst üste gelmemelidir. 

* Arabirimleri yönlendirmek için bir /29 alt ağı veya iki /30 alt ağı ayırmanız gerekir.
* Yönlendirme için kullanılan alt ağlar özel IP adresleri veya ortak IP adresleri olabilir.
* Alt ağlar Microsoft bulutunda kullanılmak üzere müşteri tarafından ayrılan aralıkla çakışmamalıdır.
* Bir /29 alt ağı kullanıldığında iki /30 alt ağına bölünür. 
  * Birinci /30 alt ağı birincil bağlantı ve ikinci /30 alt ağı ikincil bağlantı için kullanılır.
  * /30 alt ağın her biri için yönlendiriciniz üzerindeki /30 al ağının birinci IP adresini kullanmanız gerekir. Microsoft bir BGP oturumu ayarlamak için /30 alt ağının ikinci IP adresini kullanır.
  * [Kullanılabilirlik SLA](https://azure.microsoft.com/support/legal/sla/)’sının geçerli olması için her iki BGP oturumunu da ayarlamanız gerekir.  

#### <a name="example-for-private-peering"></a>Özel eşleme örneği
Eşlik oluşturmak için a.b.c.d/29 kullanmayı seçerseniz iki /30 alt ağına bölünür. Aşağıdaki örnekte, a.b.c.d/29 alt ağının nasıl kullanıldığına dikkat edin:

* a.b.c.d/29; a.b.c.d/30 ve a.b.c.d+4/30 olarak ayrılır ve sağlama API'leri yoluyla Microsoft'a geçirilir.
  * a.b.c.d+1'i Birincil PE'nin VRF IP'si olarak kullanırsınız ve Microsoft birincil MSEE'nin VRF IP'si olarak a.b.c.d+2 kullanır.
  * a.b.c.d+5'i ikincil PE'nin VRF IP'si olarak kullanırsınız ve Microsoft ikincil MSEE'nin VRF IP'si olarak a.b.c.d+6 kullanır.

Özel eşleme oluşturmak için 192.168.100.128/29’u seçtiğiniz bir durum düşünün. 192.168.100.128/29 adresi 192.168.100.128 ile 192.168.100.135 arasındaki adresleri içerir, bunlar arasında:

* 192.168.100.128/30 adresi bağlantı 1’e atanır, sağlayıcı 192.168.100.129 ve Microsoft 192.168.100.130 kullanır.
* 192.168.100.132/30 adresi bağlantı 2’ye atanır, sağlayıcı 192.168.100.133 ve Microsoft 192.168.100.134 kullanır.

### <a name="ip-addresses-used-for-microsoft-peering"></a>Microsoft eşlemesi için kullanılan IP adresleri
BGP oturumlarını ayarlamak için sahip olduğunuz ortak IP adreslerini kullanmanız gerekir. Microsoft, IP adreslerinin sahipliğini Routing Internet Registries ve Internet Routing Registries ile doğrulayabilmelidir.

* Portalda Microsoft Eşlemesi için Tanıtılan Genel Önekler altında listelenen IP adresleri, bu IP'lerden gelen trafiğe izin vermek için Microsoft çekirdek yönlendiricilerde gerekli ACL'leri oluşturacaktır. 
* Bir ExpressRoute devresindeki her eşleme (birden fazla varsa) için BGP eşliği oluşturmak üzere benzersiz bir /29 (IPv4) veya /125 (IPv6) alt ağı veya iki /30 (IPv4) veya /126 (IPv6) alt ağı kullanmanız gerekir.
* Bir /29 alt ağı kullanıldığında iki /30 alt ağına bölünür.
* Birinci /30 alt ağı birincil bağlantı ve ikinci /30 alt ağı ikincil bağlantı için kullanılır.
* /30 alt ağın her biri için yönlendiriciniz üzerindeki /30 al ağının birinci IP adresini kullanmanız gerekir. Microsoft bir BGP oturumu ayarlamak için /30 alt ağının ikinci IP adresini kullanır.
* Bir /125 alt ağı kullanıldığında iki /126 alt ağına bölünür.
* Birinci /126 alt ağı birincil bağlantı ve ikinci /126 alt ağı ikincil bağlantı için kullanılır.
* /126 alt ağın her biri için yönlendiriciniz üzerindeki /126 al ağının birinci IP adresini kullanmanız gerekir. Microsoft bir BGP oturumu ayarlamak için /126 alt ağının ikinci IP adresini kullanır.
* [Kullanılabilirlik SLA](https://azure.microsoft.com/support/legal/sla/)’sının geçerli olması için her iki BGP oturumunu da ayarlamanız gerekir.

### <a name="ip-addresses-used-for-azure-public-peering"></a>Azure genel eşlemesi için kullanılan IP adresleri

> [!NOTE]
> Azure genel eşdüzey hizmet sağlama, yeni bağlantı hatları için avialable değil.
> 

BGP oturumlarını ayarlamak için sahip olduğunuz ortak IP adreslerini kullanmanız gerekir. Microsoft, IP adreslerinin sahipliğini Routing Internet Registries ve Internet Routing Registries ile doğrulayabilmelidir. 

* Bir ExpressRoute devresindeki her eşleme (birden fazla varsa) için BGP eşliği oluşturmak üzere benzersiz bir /29 alt ağı veya iki /30 alt ağı kullanmanız gerekir. 
* Bir /29 alt ağı kullanıldığında iki /30 alt ağına bölünür. 
  * Birinci /30 alt ağı birincil bağlantı ve ikinci /30 alt ağı ikincil bağlantı için kullanılır.
  * /30 alt ağın her biri için yönlendiriciniz üzerindeki /30 al ağının birinci IP adresini kullanmanız gerekir. Microsoft bir BGP oturumu ayarlamak için /30 alt ağının ikinci IP adresini kullanır.
  * [Kullanılabilirlik SLA](https://azure.microsoft.com/support/legal/sla/)’sının geçerli olması için her iki BGP oturumunu da ayarlamanız gerekir.

## <a name="public-ip-address-requirement"></a>Genel IP adresi gereksinimi

### <a name="private-peering"></a>Özel eşleme
Özel eşleme için genel veya özel IPv4 adresleri kullanmayı tercih edebilirsiniz. Özel eşleme sırasında adreslerin diğer müşterilerle çakışmasını önlemek için trafiğinizin uçtan uca yalıtılmasını sağlarız. Bu adresler İnternet’e tanıtılmaz. 

### <a name="microsoft-peering"></a>Microsoft eşlemesi
Microsoft eşleme yolu, Microsoft bulut hizmetlerine bağlanmanızı sağlar. Bunlara Exchange Online, SharePoint Online, Skype Kurumsal ve Dynamics 365 gibi Office 365 hizmetleri dahildir. Microsoft, Microsoft eşlemesi üzerinde çift yönlü bağlantıyı destekler. Microsoft bulut hizmetlerini hedefleyen trafik, Microsoft ağına girmeden önce geçerli genel IPv4 adresleri kullanmalıdır.

IP adresi ve AS numarasının aşağıdaki kayıt defterlerinden birinde size kayıtlı olduğundan emin olun:

* [ARIN](https://www.arin.net/)
* [APNIC](https://www.apnic.net/)
* [AFRINIC](https://www.afrinic.net/)
* [LACNIC](https://www.lacnic.net/)
* [RIPENCC](https://www.ripe.net/)
* [RADB](https://www.radb.net/)
* [ALTDB](https://altdb.net/)

Ön ekleriniz ve AS numaranız yukarıdaki kayıtlarla atanmamışsa ön eklerinizin ve ASN değerinizin el ile doğrulanması amacıyla bir destek olayı oluşturmanız gerekir. Destek ekibi kaynakları kullanma izniniz olduğunu gösteren Yetki Yazısı gibi bir belge talep edecektir.

Özel AS Numarası, Microsoft Eşlemesi ile birlikte kullanılabilir ancak bunun için de el ile doğrulama yapılması gerekecektir. Ayrıca, alınan ön ekler için AS YOLU’ndaki özel AS numaralarını kaldırırız. Bunun sonucu olarak, [Microsoft Eşlemesi için yönlendirmeyi etkilemek](expressroute-optimize-routing.md) amacıyla AS YOLU’nda özel AS numaraları ekleyemezsiniz. 

> [!IMPORTANT]
> Aynı genel IP yolu genel İnternet'e ve ExpressRoute üzerinden tanıtmıyoruz. Asimetrik yönlendirmeye yol açar yanlış yapılandırma riski azaltmak için kesinlikle öneririz [NAT IP adresleri](expressroute-nat.md) Microsoft'a tanıtılan üzerinden ExpressRoute internet'e hiç bildirilmez bir aralıktan olabilir. Bunu elde etmek mümkün değilse, bir Internet bağlantısı kullanan ExpressRoute üzerinden daha belirli bir aralık tanıtma emin olmak için gereklidir. NAT için genel yönlendirme yanı sıra ayrıca genel IP ExpressRoute üzerinden tanıtabilirsiniz içinden, Microsoft Office 365 uç noktaları ile iletişim kuran sunucular şirket içi ağınızda tarafından kullanılan adresleri. 
> 
> 

### <a name="public-peering-deprecated---not-available-for-new-circuits"></a>Ortak eşleme (kullanım dışı - yeni bağlantı hatları için kullanılamaz)
Azure ortak eşleme yolu, Azure’da barındırılan tüm hizmetlere ortak IP adresleri üzerinden bağlanmanıza olanak sağlar. Bunlar [ExpessRoute hakkında SSS](expressroute-faqs.md)’de listelenen tüm hizmetleri ve ISV’ler tarafından Microsoft Azure üzerinde barındırılan hizmetleri içerir. Ortak eşleme üzerinden Microsoft Azure hizmetlerine bağlama, her zaman sizin ağınızdan Microsoft ağına doğru başlatılır. Microsoft ağını hedefleyen trafik için Genel IP adreslerini kullanmanız gerekir.

> [!IMPORTANT]
> Tüm Azure PaaS hizmetlerine Microsoft eşlemesi üzerinden erişilebilir.
>   

Özel AS numarası, genel eşleme ile izin verilir.

## <a name="dynamic-route-exchange"></a>Dinamik yönlendirme değişimi
Yönlendirme değişimi bir eBGP protokolü üzerinden olacaktır. EBGP oturumları MSEE’ler ile yönlendiricileriniz arasında oluşturulur. BGP oturumlarının kimlik doğrulaması zorunlu değildir. Gerekirse bir MD5 karması yapılandırılabilir. BGP oturumlarını yapılandırma hakkında bilgi için [Yönlendirmeyi yapılandırma](how-to-routefilter-portal.md) ve [Devre sağlama iş akışları ve devre durumları](expressroute-workflows.md) bölümlerine bakın.

## <a name="autonomous-system-numbers"></a>Otonom Sistem numaraları
Microsoft Azure genel, Azure özel ve Microsoft eşlemesi için AS 12076 kullanır. 65515 ile 65520 arasındaki ASN’ler şirket içi kullanım için ayrılmıştır. Hem 16 hem de 32 bit AS numaraları desteklenir.

Veri aktarımı simetrisi etrafında bir gereksinim yoktur. İleri ve geri dönüş yolları farklı yönlendirici çiftlerinden geçiş yapabilir. Size ait birden fazla devre çiftinin arasında ya da taraflarından aynı yollar tanıtılmalıdır. Yol ölçümlerinin aynı olması gerekmez.

## <a name="route-aggregation-and-prefix-limits"></a>Yol toplama ve ön ek sınırları
Azure özel eşleme aracılığıyla bize tanıtılan 4000’e kadar ön eki destekliyoruz. ExpressRoute premium eklentisi etkinse bu sayı en fazla 10.000 ön eke kadar artırılabilir. Azure genel ve Microsoft eşlemesi için BGP oturumu başına en fazla 200 ön ek kabul edilmektedir. 

Ön ek sayısı bu sınırı aşarsa BGP oturumu düşürülür. Yalnızca özel eşleme bağlantısında varsayılan yollar kabul edilir. Sağlayıcının Azure genel ve Microsoft eşlemesi yollarından varsayılan yolu ve özel IP adreslerini (RFC 1918) filtrelemesi gerekir. 

## <a name="transit-routing-and-cross-region-routing"></a>Geçiş yönlendirme ve çapraz bölge yönlendirme
ExpressRoute geçiş yönlendirici olarak yapılandırılamaz. Geçiş yönlendirme hizmetleri için bağlantı sağlayıcınızı kullanmanız gerekir.

## <a name="advertising-default-routes"></a>Varsayılan yolları tanıtma
Varsayılan yollar yalnızca Azure özel eşleme oturumlarında kullanılabilir. Böyle bir durumda, ilişkili sanal ağlardaki tüm trafik ağınıza yönlendirilecektir. Varsayılan yolların özel eşlemede tanıtılması Azure’daki Internet yolunun engellenmesiyle sonuçlanır. Azure içinde barındırılan hizmetler için internetten gelen ve giden trafiği yönlendirmek üzere kurumsal edge kullanmanız gerekir. 

 Diğer Azure hizmetleri ve altyapı hizmetleri ile bağlantıyı etkinleştirmek üzere aşağıdaki öğelerden birinin yerinde olduğundan emin olmanız gerekir:

* Trafiği ortak Uç noktalara yönlendirmek için Azure ortak eşleme etkindir.
* İnternet bağlantısı gerektiren her alt ağ için İnternet bağlantısına izin vermek üzere kullanıcı tanımlı yönlendirmeyi kullanırsınız.

> [!NOTE]
> Varsayılan yolların tanıtılması, Windows ve diğer VM lisans etkinleştirmelerini bozar. Bu sorunu çözmek için [buradaki](https://blogs.msdn.com/b/mast/archive/2015/05/20/use-azure-custom-routes-to-enable-kms-activation-with-forced-tunneling.aspx) yönergeleri izleyin.
> 
> 

## <a name="bgp"></a>BGP toplulukları desteği
Bu bölüm BGP toplulukların ExpressRoute ile nasıl kullanıldığına genel bir bakış sağlar. Microsoft genel ve Microsoft eşleme yollarındaki rotaları uygun topluluk değerleriyle etiketleyerek tanıtır. Bunu yapmanın gerekçesi ve topluluk değerlerine ilişkin ayrıntılar aşağıda açıklanmıştır. Ancak, Microsoft kendisine tanıtılan rotalara etiketlenmiş hiçbir topluluk değerini kabul etmez.

Microsoft’a ExpressRoute aracılığıyla jeopolitik bir bölgedeki herhangi bir eşleme konumundan bağlanıyorsanız jeopolitik sınır dahilindeki tüm bölgelerde bütün Microsoft bulut hizmetlerine erişiminiz olacaktır. 

Örneğin, Microsoft’a Amsterdam’da ExpressRoute aracılığıyla bağlandıysanız Kuzey Avrupa ve Batı Avrupa’da barındırılan tüm Microsoft bulut hizmetlerine erişiminiz olur. 

Jeopolitik bölgeler, ilişkili Azure bölgeleri ve karşılık gelen ExpressRoute eşleme konumlarını içeren ayrıntılı liste için [ExpressRoute iş ortakları ve eşleme konumları](expressroute-locations.md) sayfasına bakın.

Bir jeopolitik bölge için birden fazla ExpressRoute devresi satın alabilirsiniz. Birden fazla bağlantıya sahip olmanız coğrafi artıklık nedeniyle yüksek kullanılabilirliğe ilişkin önemli avantajlar sunar. Birden fazla ExpressRoute devrenizin olduğu durumlarda, Microsoft eşlemesi ve genel eşleme yollarındaki Microsoft'tan tanıtılan ön ekleri aynı kümesini alır. Bu durum ağınız ile Microsoft arasında birden fazla yol olacağı anlamına gelir. Bu durum ağınızın içinde en iyi olmayan yönlendirme kararlarına neden olabilir. Sonuç olarak, farklı hizmetlerde en iyi düzeyin altında bağlantı deneyimleri yaşayabilirsiniz. [Kullanıcılar için en iyi yönlendirmeyi](expressroute-optimize-routing.md) sunmak üzere uygun yönlendirme kararlarını almak için topluluk değerlerini kullanabilirsiniz.

| **Microsoft Azure bölgesi** | **BGP topluluk değeri** |
| --- | --- |
| **Kuzey Amerika** | |
| Doğu ABD | 12076:51004 |
| Doğu ABD 2 | 12076:51005 |
| Batı ABD | 12076:51006 |
| Batı ABD 2 | 12076:51026 |
| Batı Orta ABD | 12076:51027 |
| Orta Kuzey ABD | 12076:51007 |
| Orta Güney ABD | 12076:51008 |
| Orta ABD | 12076:51009 |
| Orta Kanada | 12076:51020 |
| Doğu Kanada | 12076:51021 |
| **Güney Amerika** | |
| Güney Brezilya | 12076:51014 |
| **Avrupa** | |
| Kuzey Avrupa | 12076:51003 |
| Batı Avrupa | 12076:51002 |
| Birleşik Krallık Güney | 12076:51024 |
| Birleşik Krallık Batı | 12076:51025 |
| Fransa Orta | 12076:51030 |
| Fransa Güney | 12076:51031 |
| **Asya Pasifik** | |
| Doğu Asya | 12076:51010 |
| Güneydoğu Asya | 12076:51011 |
| **Japonya** | |
| Japonya Doğu | 12076:51012 |
| Japonya Batı | 12076:51013 |
| **Avustralya** | |
| Avustralya Doğu | 12076:51015 |
| Avustralya Güneydoğu | 12076:51016 |
| **Australia Government** | |
| Avustralya Orta | 12076:51032 |
| Avustralya Orta 2 | 12076:51033 |
| **Hindistan** | |
| Hindistan Güney | 12076:51019 |
| Hindistan Batı | 12076:51018 |
| Hindistan Orta | 12076:51017 |
| **Güney Kore** | |
| Kore Güney | 12076:51028 |
| Kore Orta | 12076:51029 |


Microsoft tarafından tanıtılan tüm yollar uygun topluluk değeriyle etiketlenecektir. 

> [!IMPORTANT]
> Genel ön ekler, uygun bir topluluk değeri ile etiketlenir.
> 
> 

Yukarıdakilerin yanı sıra Microsoft, ön ekleri ait oldukları hizmet göre etiketleyecektir. Bu durum yalnızca Microsoft eşlemesi için geçerlidir. Aşağıdaki tabloda hizmetin BGP topluluk değeri ile eşleşmesi gösterilmektedir.

| **Hizmet** | **BGP topluluk değeri** |
| --- | --- |
| Exchange Online | 12076:5010 |
| SharePoint Online | 12076:5020 |
| Skype Kurumsal Çevrimiçi Sürüm | 12076:5030 |
| Dynamics 365 | 12076:5040 |
| Azure genel Hizmetleri * | 12076:5050 |
| Diğer Office 365 Çevrimiçi hizmetleri | 12076:5100 |

* Azure küresel hizmetler şu anda yalnızca Azure DevOps içerir.


> [!NOTE]
> Microsoft, Microsoft'a tanıtılan yollar üzerinde ayarladığınız hiçbir BGP topluluk değerini dikkate almaz.
> 
> 

### <a name="bgp-community-support-in-national-clouds"></a>Ulusal Bulutlarda BGP Topluluk desteği

| **Ulusal Bulutlar Azure Bölgesi**| **BGP topluluk değeri** |
| --- | --- |
| **ABD Devleti** |  |
| ABD Devleti Arizona | 12076:51106 |
| US Gov Iowa | 12076:51109 |
| ABD Devleti Virginia | 12076:51105 |
| ABD Devleti Texas | 12076:51108 |
| US DoD Orta | 12076:51209 |
| US DoD Doğu | 12076:51205 |


| **Ulusal Bulutlardaki Hizmet** | **BGP topluluk değeri** |
| --- | --- |
| **ABD Devleti** |  |
| Exchange Online |12076:5110 |
| SharePoint Online |12076:5120 |
| Skype Kurumsal Çevrimiçi Sürüm |12076:5130 |
| Dynamics 365 |12076:5140 |
| Diğer Office 365 Çevrimiçi hizmetleri |12076:5200 |

## <a name="next-steps"></a>Sonraki adımlar
* ExpressRoute bağlantınızı yapılandırın.
  
  * [Bağlantı hattı oluşturma ve değiştirme](expressroute-howto-circuit-arm.md)
  * [Eşleme yapılandırması oluşturma ve değiştirme](expressroute-howto-routing-arm.md)
  * [ExpressRoute bağlantı hattına bir Sanal Ağ bağlama](expressroute-howto-linkvnet-arm.md)
