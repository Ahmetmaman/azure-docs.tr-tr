---
title: Olağanüstü durum kurtarma ve geçiş için Azure Site Recovery ile Azure ExpressRoute kullanarak hakkında | Microsoft Docs
description: Olağanüstü durum kurtarma ve geçiş için Azure Site Recovery hizmeti ile Azure ExpressRoute kullanmayı açıklar.
services: site-recovery
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 12/27/2018
ms.author: mayg
ms.openlocfilehash: 1fabbe3a9a486abc862bfb6c2671c60d11d8e8c7
ms.sourcegitcommit: 9f87a992c77bf8e3927486f8d7d1ca46aa13e849
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/28/2018
ms.locfileid: "53809939"
---
# <a name="azure-expressroute-with-azure-site-recovery"></a>Azure Site Recovery ile Azure ExpressRoute

Microsoft Azure ExpressRoute, bağlantı sağlayıcı tarafından kolaylaştırılan özel bağlantı üzerinden şirket içi ağlarınızı Microsoft bulutuna genişletmenizi sağlar. ExpressRoute'u kullanarak Microsoft Azure, Office 365 ve Dynamics 365 gibi Microsoft bulut hizmetleriyle bağlantı kurabilirsiniz.

Bu makalede, nasıl Azure ExpressRoute Azure Site Recovery ile olağanüstü durum kurtarma ve geçiş için kullanabileceğiniz açıklanır.

## <a name="expressroute-circuits"></a>ExpressRoute bağlantı hatları

Bir ExpressRoute bağlantı hattı, şirket içi altyapınızı ve bağlantı sağlayıcı üzerinden Microsoft bulut hizmetleri arasında mantıksal bağlantıyı temsil eder. Birden çok ExpressRoute bağlantı hattına sipariş edebilirsiniz. Her bağlantı hattı aynı veya farklı bölgelerde olabilir ve farklı bağlantı sağlayıcıları aracılığıyla şirket içinde bağlanabilir. ExpressRoute bağlantı hatları hakkında daha fazla bilgi [burada](../expressroute/expressroute-circuit-peerings.md).

## <a name="expressroute-routing-domains"></a>ExpressRoute yönlendirme etki alanları

Bir ExpressRoute bağlantı hattı kendisiyle ilişkilendirilmiş birden fazla Yönlendirme etki alanları şunlardır:
-   [Azure özel eşdüzey hizmet sağlama](../expressroute/expressroute-circuit-peerings.md#privatepeering) - Azure işlem Hizmetleri, yani sanal makineler (Iaas) ve bir sanal ağda dağıtılan bulut hizmetlerini (PaaS) üzerinden özel eşleme etki alanına bağlanabilir. Özel Eşleme etki Microsoft Azure'a çekirdek ağınızı güvenilir bir uzantısı olarak kabul edilir.
-   [Azure ortak eşleme](../expressroute/expressroute-circuit-peerings.md#publicpeering) -Azure depolama, SQL veritabanları ve Web siteleri gibi hizmetleri genel IP adreslerinde sunulur. Özel VIP'ler genel eşleme Yönlendirme etki alanı aracılığıyla, bulut hizmetleri de dahil olmak üzere genel IP adreslerinde barındırılan hizmetler bağlanabilirsiniz. Genel eşdüzey hizmet sağlama için yeni oluşturma kullanım dışıdır ve Microsoft Peering Azure PaaS Hizmetleri için bunun yerine kullanılmalıdır.
-   [Microsoft eşlemesi](../expressroute/expressroute-circuit-peerings.md#microsoftpeering) -Microsoft çevrimiçi hizmetlerine (Office 365, Dynamics 365 ve Azure PaaS Hizmetleri) olduğundan Microsoft eşlemesi aracılığıyla. Microsoft eşlemesi, Azure PaaS hizmetlerine bağlanmak için önerilen Yönlendirme etki alanı adıdır.

Daha fazla bilgi edinin ve ExpressRoute Yönlendirme etki alanları karşılaştırın [burada](../expressroute/expressroute-circuit-peerings.md#peeringcompare).

## <a name="on-premises-to-azure-replication-with-expressroute"></a>Şirket içi ExpressRoute ile Azure'a çoğaltma

Azure Site kurtarma sağlayan olağanüstü durum kurtarma ve azure'a geçiş için şirket içi [Hyper-V sanal makinelerini](hyper-v-azure-architecture.md), [VMware sanal makinelerini](vmware-azure-architecture.md), ve [fiziksel sunucuları](physical-azure-architecture.md). Tüm şirket içi Azure senaryoları için çoğaltma verileri gönderilen ve bir Azure depolama hesabında depolanır. Çoğaltma sırasında herhangi bir sanal makine ücreti ödeme yapmayın. Azure'a yük devretme çalıştırdığınızda, Site Recovery, Azure Iaas sanal makineleri otomatik olarak oluşturur.

Site Recovery, genel bir uç nokta bir Azure depolama hesabına veri çoğaltır. Site Recovery çoğaltması için ExpressRoute kullanmak için kullanabileceği [genel eşdüzey hizmet sağlama](../expressroute/expressroute-circuit-peerings.md#publicpeering) veya [Microsoft eşlemesi](../expressroute/expressroute-circuit-peerings.md#microsoftpeering). Microsoft eşlemesi, çoğaltma için önerilen Yönlendirme etki alanıdır. Emin [ağ gereksinimleri](vmware-azure-configuration-server-requirements.md#network-requirements) çoğaltma için de karşılandığından. Sanal makine ya da sunucuları için bir Azure sanal ağı yük devretme sonra erişebilirsiniz kullanarak [özel eşdüzey hizmet sağlama](../expressroute/expressroute-circuit-peerings.md#privatepeering). Çoğaltma özel eşdüzey hizmet sağlama üzerinden desteklenmiyor.

Birleşik senaryo, aşağıdaki diyagramda gösterilmiştir: ![Şirket içi-Azure'a ExpressRoute ile](./media/concepts-expressroute-with-site-recovery/site-recovery-with-expressroute.png)

## <a name="azure-to-azure-replication-with-expressroute"></a>ExpressRoute ile azure'dan Azure'a çoğaltma

Azure Site kurtarma sağlayan olağanüstü durum kurtarma [Azure sanal makineleri](azure-to-azure-architecture.md). İster kullanın, Azure sanal makineleri bağlı olarak [Azure yönetilen diskler](../virtual-machines/windows/managed-disks-overview.md), çoğaltma verileri bir Azure depolama hesabı veya çoğaltma yönetilen Disk üzerinde ' % s'hedef Azure bölgeniz için gönderilir. Çoğaltma uç noktaları ortak olsa da, varsayılan olarak, Azure VM çoğaltma için çoğaltma trafiğini Internet bağımsız olarak Azure bölgesi kaynak sanal ağ içinde bulunduğu erişmez. Azure'nın varsayılan sistem yolu 0.0.0.0/0 adres ön eki ile geçersiz kılabilirsiniz bir [özel rota](../virtual-network/virtual-networks-udr-overview.md#custom-routes) ve bir şirket içi ağ sanal Gereci (NVA) VM trafiğine yöneltmektir, ancak bu yapılandırma, Site Recovery için önerilmez Çoğaltma. Özel yollar kullanıyorsanız yapmanız gerekenler [bir sanal ağ hizmet uç noktası oluşturma](azure-to-azure-about-networking.md#create-network-service-endpoint-for-storage) , sanal ağ için "depolama" böylece çoğaltma trafiğini Azure sınır bırakmaz.

Azure VM'LERİNDE olağanüstü durum kurtarma için varsayılan olarak, ExpressRoute çoğaltma için gerekli değildir. Sanal makineler üzerinde hedef Azure bölgesi başarısız olduktan sonra bunları erişebilirsiniz kullanarak [özel eşdüzey hizmet sağlama](../expressroute/expressroute-circuit-peerings.md#privatepeering).

Kaynak bölgesi Azure Vm'lerinde, şirket içi veri merkezinizden bağlanmak için zaten ExpressRoute kullanıyorsanız, yük devretme hedef bölgede ExpressRoute bağlantınızın yeniden kurmak için planlayabilirsiniz. Hedef bölgede yeni bir sanal ağ bağlantısı üzerinden bağlanmak veya ayrı bir ExpressRoute bağlantı hattı ve olağanüstü durum kurtarma için bağlantı kullanmak için aynı ExpressRoute bağlantı hattı kullanabilirsiniz. Farklı olası senaryolar açıklanmıştır [burada](azure-vm-disaster-recovery-with-expressroute.md#fail-over-azure-vms-when-using-expressroute).

Herhangi bir Azure bölgesine ayrıntılı olarak aynı coğrafi kümedeki Azure sanal makinelerini çoğaltma [burada](../site-recovery/azure-to-azure-support-matrix.md#region-support). Seçtiğiniz hedef Azure bölgesi aynı jeopolitik bölgede kaynağı olarak içinde değilse, ExpressRoute Premium etkinleştirmeniz gerekebilir. Daha fazla ayrıntı için denetleyin [ExpressRoute konumları](../expressroute/expressroute-locations.md#azure-regions-to-expressroute-locations-within-a-geopolitical-region) ve [ExpressRoute fiyatlandırması](https://azure.microsoft.com/pricing/details/expressroute/).

## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinin [ExpressRoute devreleri](../expressroute/expressroute-circuit-peerings.md).
- Daha fazla bilgi edinin [ExpressRoute Yönlendirme etki alanları](../expressroute/expressroute-circuit-peerings.md#peeringcompare).
- Daha fazla bilgi edinin [ExpressRoute konumları](../expressroute/expressroute-locations.md).
- Olağanüstü durum kurtarma hakkında daha fazla bilgi [ExpressRoute ile Azure sanal makineleri ](azure-vm-disaster-recovery-with-expressroute.md).
