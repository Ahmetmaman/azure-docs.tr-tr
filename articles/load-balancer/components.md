---
title: Azure Load Balancer bileşenleri
description: Azure Load Balancer bileşenlere genel bakış
services: load-balancer
documentationcenter: na
author: asudbring
ms.service: load-balancer
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/04/2020
ms.author: allensu
ms.openlocfilehash: 97b872c5fe0a155bb6e474f327f8d0c65e22b21f
ms.sourcegitcommit: ce8eecb3e966c08ae368fafb69eaeb00e76da57e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/21/2020
ms.locfileid: "92317442"
---
# <a name="azure-load-balancer-components"></a>Azure Load Balancer bileşenleri

Azure Load Balancer bazı önemli bileşenleri içerir. Bu bileşenler, aboneliğinizdeki aracılığıyla yapılandırılabilir:

* Azure portal
* Azure CLI’si
* Azure PowerShell
* Resource Manager Şablonları

## <a name="frontend-ip-configuration"></a>Ön uç IP yapılandırması <a name = "frontend-ip-configurations"></a>

Azure Load Balancer IP adresi. Bu, istemcilerle ilgili iletişim noktasıdır. Bu IP adresleri şunlardan biri olabilir:

- **Genel IP Adresi**
- **Özel IP adresi**

IP adresinin doğası, oluşturulan yük dengeleyicinin **türünü** belirler. Özel IP adresi seçimi bir iç yük dengeleyici oluşturur. Genel IP adresi seçimi bir genel yük dengeleyici oluşturur.

|  | Genel Load Balancer  | İç Yük Dengeleyici |
| ---------- | ---------- | ---------- |
| **Ön uç IP yapılandırması**| Genel IP adresi | Özel IP adresi|
| **Açıklama** | Ortak yük dengeleyici, gelen trafiğin genel IP ve bağlantı noktasını, sanal makinenin özel IP ve bağlantı noktasıyla eşleştirir. Yük dengeleyici trafiği VM 'den gelen yanıt trafiği için başka bir şekilde eşler. Yük Dengeleme kuralları uygulayarak, belirli trafik türlerini birden çok VM veya hizmet arasında dağıtabilirsiniz. Örneğin web isteği trafiğinin yükünü birden fazla web sunucusuna dağıtabilirsiniz.| İç yük dengeleyici, trafiği bir sanal ağ içindeki kaynaklara dağıtır. Azure, yük dengeli bir sanal ağın ön uç IP adreslerine erişimi kısıtlar. Ön uç IP adresleri ve sanal ağlar hiçbir şekilde doğrudan bir internet uç noktasına gösterilmez. İç iş kolu uygulamaları Azure'da çalışır ve Azure'dan veya şirket içi kaynaklardan erişim sağlanır. |
| **Desteklenen SKU 'Lar** | Temel, standart | Temel, standart |

![Katmanlı yük dengeleyici örneği](./media/load-balancer-overview/load-balancer.png)

Load Balancer birden çok ön uç IP 'si olabilir. [Birden çok ön uçlar](load-balancer-multivip-overview.md)hakkında daha fazla bilgi edinin.

## <a name="backend-pool"></a>Arka uç havuzu

Gelen isteğe hizmet veren bir sanal makine ölçek kümesindeki sanal makine veya örnek grubu. Yüksek hacimli gelen trafiği karşılamak üzere maliyeti etkili bir şekilde ölçeklendirmek için, bilgi işlem kılavuzu genellikle arka uç havuzuna daha fazla örnek eklenmesini önerir.

Yük dengeleyici, örnekleri yukarı veya aşağı ölçeklendirirseniz otomatik yeniden yapılandırma yoluyla kendisini anında yeniden yapılandırır. Arka uç havuzundan VM ekleme veya kaldırma işlemi ek işlem yapılmadan yük dengeleyiciyi yeniden yapılandırır. Arka uç havuzunun kapsamı, sanal ağdaki herhangi bir sanal makinedir.

Arka uç havuzunuzu nasıl tasarlayacağınızı düşünürken, yönetim işlemlerinin uzunluğunu iyileştirmek için en az sayıda ayrı arka uç havuzu kaynağı tasarlayın. Veri düzlemi performansı veya ölçeği üzerinde farklılık yoktur.

## <a name="health-probes"></a>Sistem durumu araştırmaları

Bir sistem durumu araştırması, arka uç havuzundaki örneklerin sistem durumunu tespit etmek için kullanılır. Yük dengeleyici oluşturma sırasında, yük dengeleyicinin kullanması için bir sistem durumu araştırması yapılandırın.  Bu sistem durumu araştırması, bir örneğin sağlıklı olup olmadığını ve trafik alıp alamayacağını tespit eder.

Sistem durumu araştırmalarının sağlıksız eşiğini tanımlayabilirsiniz. Bir araştırma yanıtlanamazsa Load Balancer sağlıksız örneklere yeni bağlantı göndermeyi durduruyor. Bir araştırma hatası varolan bağlantıları etkilemez. Bağlantı, uygulamaya kadar devam eder:

- Akışı sonlandırır
- Boşta kalma zaman aşımı oluştu
- VM kapanıyor

Load Balancer uç noktalar için farklı durum araştırma türleri sağlar: TCP, HTTP ve HTTPS. [Load Balancer sistem durumu araştırmaları hakkında daha fazla bilgi edinin](load-balancer-custom-probe-overview.md).

Temel Load Balancer HTTPS araştırmaları desteklemez. Temel Load Balancer tüm TCP bağlantılarını (kurulan bağlantılar dahil) kapatır.

## <a name="load-balancing-rules"></a>Yük Dengeleme kuralları

Bir Load Balancer kuralı, gelen trafiğin arka uç havuzundaki **Tüm** örneklere nasıl dağıtıldığını tanımlamak için kullanılır. Yük Dengeleme kuralı, belirli bir ön uç IP yapılandırmasını ve bağlantı noktasını birden çok arka uç IP adresine ve bağlantı noktasına eşler.

Örneğin, ön uç IP 'nizden gelen trafiği arka uç örneklerinizin 80 numaralı bağlantı noktasına yönlendirmek için 80 numaralı bağlantı noktası için bir yük dengeleme kuralı kullanın.

<p align="center">
  <img src="./media/load-balancer-components/lbrules.svg" alt= "Figure depicts how Azure Load Balancer directs frontend port 80 to three instances of backend port 80." width="512" title="Yük Dengeleme kuralları">
</p>

*Şekil: Yük Dengeleme kuralları*

## <a name="high-availability-ports"></a>Yüksek kullanılabilirlik bağlantı noktaları

**' Protocol-All ve port-0 '** ile yapılandırılmış bir yük dengeleyici kuralı. 

Bu kural, bir iç Standart Load Balancer tüm bağlantı noktalarına gelen tüm TCP ve UDP akışlarının yük dengelenmesi için tek bir kural sağlar. 

Yük Dengeleme kararı akış başına yapılır. Bu eylem aşağıdaki beş demet bağlantısına dayanır: 

1. 
    en yakın kullanılabilir uç noktayı arama
  
2. kaynak bağlantı noktası
3. hedef IP adresi
4. hedef bağlantı noktası
5. protokol

HA bağlantı noktaları Yük Dengeleme kuralları, sanal ağların içindeki ağ sanal gereçleri (NVA 'lar) için yüksek kullanılabilirlik ve ölçek gibi kritik senaryolarda size yardımcı olur. Bu özellik, çok sayıda bağlantı noktasının yük dengeli olması gerektiğinde yardımcı olabilir.

<p align="center">
  <img src="./media/load-balancer-components/harules.svg" alt="Figure depicts how Azure Load Balancer directs all frontend ports to three instances of all backend ports" width="512" title="HA bağlantı noktaları kuralları">
</p>

*Şekil: HA bağlantı noktası kuralları*

[Ha bağlantı noktaları](load-balancer-ha-ports-overview.md)hakkında daha fazla bilgi edinin.

## <a name="inbound-nat-rules"></a>Gelen NAT kuralları

Gelen NAT kuralı, ön uç IP adresine ve bağlantı noktası birleşimine gönderilen gelen trafiği iletir. Trafik, arka uç havuzundaki **belirli** bir sanal makineye veya örneğe gönderilir. Bağlantı noktası iletme, Yük Dengeleme ile aynı karma tabanlı dağıtım tarafından yapılır.

Örneğin, bir arka uç havuzundaki sanal makine örneklerinin Uzak Masaüstü Protokolü (RDP) veya Secure Shell (SSH) oturumlarını istiyorsanız. Aynı ön uç IP adresindeki bağlantı noktalarıyla birden çok iç uç nokta eşlenebilir. Ön uç IP adresleri, ek bir sıçrama kutusu olmadan sanal makinelerinizi uzaktan yönetmek için kullanılabilir.

<p align="center">
  <img src="./media/load-balancer-components/inboundnatrules.svg" alt="Figure depicts how Azure Load Balancer directs frontend ports 3389, 443, and 80 to backend ports with the same values on separate servers." width="512" title="Gelen NAT kuralları">
</p>

*Şekil: gelen NAT kuralları*

Sanal Makine Ölçek Kümeleri bağlamındaki gelen NAT kuralları gelen NAT havuzlarıdır. [Load Balancer bileşenleri ve sanal makine ölçek kümesi](../virtual-machine-scale-sets/virtual-machine-scale-sets-networking.md#azure-virtual-machine-scale-sets-with-azure-load-balancer)hakkında daha fazla bilgi edinin.

## <a name="outbound-rules"></a>Giden kuralları

Giden bir kural, arka uç havuzu tarafından tanımlanan tüm sanal makineler veya örnekler için giden ağ adresi çevirisi 'ni (NAT) yapılandırır. Bu kural, arka uçtaki örneklerin internet veya diğer uç noktalara iletişim kurmasını sağlar.

[Giden bağlantılar ve kurallar](load-balancer-outbound-connections.md)hakkında daha fazla bilgi edinin.

Temel yük dengeleyici giden kuralları desteklemez.

## <a name="limitations"></a>Sınırlamalar

- Load Balancer [sınırları](https://aka.ms/lblimits) hakkında bilgi edinin 
- Yük dengeleyici, belirli TCP veya UDP protokolleri için yük dengeleme ve bağlantı noktası iletme sağlar. Yük Dengeleme kuralları ve gelen NAT kuralları TCP ve UDP 'yi destekler, ancak ıCMP dahil diğer IP protokollerini desteklemez.
- Bir arka uç VM 'den bir iç Load Balancer ön uca giden akış başarısız olur.
- Yük dengeleyici kuralı iki sanal ağı yayılamaz.  Ön uçların ve arka uç örneklerinin aynı sanal ağda bulunması gerekir.  
- IP parçalarını iletme, Yük Dengeleme kurallarında desteklenmez. UDP ve TCP paketlerinin IP parçalanması, Yük Dengeleme kurallarında desteklenmez. HA bağlantı noktaları Yük Dengeleme kuralları, var olan IP parçalarını iletmek için kullanılabilir. Daha fazla bilgi için bkz. [yüksek kullanılabilirlik bağlantı noktalarına genel bakış](load-balancer-ha-ports-overview.md).

## <a name="next-steps"></a>Sonraki adımlar

- Load Balancer kullanmaya başlamak için bkz. [genel standart Load Balancer oluşturma](quickstart-load-balancer-standard-public-portal.md) .
- [Azure Load Balancer](load-balancer-overview.md)hakkında daha fazla bilgi edinin.
- [Genel IP adresi](https://docs.microsoft.com/azure/virtual-network/virtual-network-public-ip-address) hakkında bilgi edinin
- [Özel IP adresi](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm#private-ip-addresses) hakkında bilgi edinin
- [Standart Load Balancer ve kullanılabilirlik alanları](load-balancer-standard-availability-zones.md)kullanma hakkında bilgi edinin.
- [Standart Load Balancer tanılama](load-balancer-standard-diagnostics.md)hakkında bilgi edinin.
- [Boşta durumunda TCP sıfırlaması](load-balancer-tcp-reset.md)hakkında bilgi edinin.
- [Ha bağlantı noktaları Yük Dengeleme kurallarıyla standart Load Balancer](load-balancer-ha-ports-overview.md)hakkında bilgi edinin.
- [Ağ güvenlik grupları](../virtual-network/security-overview.md)hakkında daha fazla bilgi edinin.
- [Yük dengeleyici sınırları](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#load-balancer)hakkında daha fazla bilgi edinin.
- [Bağlantı noktası iletmeyi](https://docs.microsoft.com/azure/load-balancer/tutorial-load-balancer-port-forwarding-portal)kullanma hakkında bilgi edinin.
