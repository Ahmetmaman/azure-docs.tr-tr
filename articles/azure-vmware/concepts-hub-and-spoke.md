---
title: Kavram-bir hub ve bağlı bileşen mimarisinde bir Azure VMware çözüm dağıtımını tümleştirme
description: Azure 'da bir hub ve bağlı bileşen mimarisinde Azure VMware çözüm dağıtımını tümleştirme hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 10/26/2020
ms.openlocfilehash: 93c11ad9253fe78e1935da7b40e7251788f1f037
ms.sourcegitcommit: 4cb89d880be26a2a4531fedcc59317471fe729cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2020
ms.locfileid: "92674694"
---
# <a name="integrate-azure-vmware-solution-in-a-hub-and-spoke-architecture"></a>Azure VMware çözümünü bir hub ve bağlı bileşen mimarisinde tümleştirme

Bu makalede, Azure 'da var olan veya yeni bir [hub ve bağlı bileşen mimarisinde](/azure/architecture/reference-architectures/hybrid-networking/shared-services) bir Azure VMware çözüm dağıtımını tümleştirme önerileri sağlanır. 


Hub ve bağlı bileşen senaryosu, üzerinde iş yükleri içeren bir karma bulut ortamı olduğunu varsayar:

* IaaS veya PaaS hizmetlerini kullanarak yerel Azure
* Azure VMware Çözümü 
* Şirket içi sanal küre

## <a name="architecture"></a>Mimari

*Hub* , şirket Içi ve Azure VMware Çözüm özel bulutunuz için merkezi bir bağlantı noktası görevi gören bir Azure sanal ağı. Sanal *ağlar, platformlar* arası ağ iletişimini etkinleştirmek için hub ile ilişkilendirilen sanal ağlardır.

Şirket içi veri merkezi, Azure VMware çözümü özel bulutu ve hub arasındaki trafik Azure ExpressRoute bağlantıları üzerinden geçer. Bağlı bileşen sanal ağları genellikle IaaS tabanlı iş yükleri içerir, ancak sanal ağla doğrudan tümleştirme veya [Azure özel bağlantısı](../private-link/index.yml) etkinleştirilmiş diğer PaaS hizmetleri olan [App Service ortamı](../app-service/environment/intro.md)gibi PaaS hizmetlerine sahip olabilir.

>[!IMPORTANT]
>Sanal ağ başına dört ExpressRoute bağlantı hattı sınırını aşmadığı sürece Azure VMware çözümüne bağlanmak için mevcut bir ExpressRoute ağ geçidini kullanabilirsiniz.  Bununla birlikte, ExpressRoute aracılığıyla şirket içi Azure VMware çözümüne erişmek için ExpressRoute Global Reach sahip olmanız gerekir. Bu, ExpressRoute ağ geçidi, bağlı olan devreler arasında geçişli yönlendirme sağlamamalıdır.

Diyagramda, Azure 'da ExpressRoute Global Reach aracılığıyla şirket içi ve Azure VMware çözümüne bağlı bir hub ve bağlı bileşen dağıtımı örneği gösterilmektedir.

:::image type="content" source="./media/hub-spoke/azure-vmware-solution-hub-and-spoke-deployment.png" alt-text="Azure VMware Çözüm Merkezi ve bağlı bileşen tümleştirme dağıtımı" border="false" lightbox="./media/hub-spoke/azure-vmware-solution-hub-and-spoke-deployment.png":::

Mimaride aşağıdaki ana bileşenler bulunur:

-   **Şirket içi site:** Müşteri şirket içi veri merkezi, bir ExpressRoute bağlantısı üzerinden Azure 'a bağlandı.

-   **Azure VMware çözümü özel bulutu:** Her biri en fazla 16 düğüm içeren bir veya daha fazla vSphere kümesi tarafından oluşturulan Azure VMware Çözüm SDDC.

-   **ExpressRoute ağ geçidi:** Azure VMware çözümü özel bulutu, hub sanal ağı üzerindeki paylaşılan hizmetler ve bağlı olan sanal ağlarda çalışan iş yükleri arasındaki iletişimi mümkün bir şekilde sunar.

-   **ExpressRoute Global Reach:** Şirket içi ve Azure VMware çözümü özel bulutu arasında bağlantıyı mümkün bir şekilde sunar.


  > [!NOTE]
  > **S2S VPN konuları:** Azure VMware Çözüm üretim dağıtımları için, VMware HCX ağ gereksinimleri nedeniyle Azure S2S VPN desteklenmez. Ancak, bunu bir PoC dağıtımı için kullanabilirsiniz.


-   **Hub sanal ağı:** Şirket içi ağınıza ve Azure VMware çözümü özel buluta merkezi bağlantı noktası görevi görür.

-   **Bağlı bileşen sanal ağı**

    -   **IaaS bağlı bileşen:** IaaS bağlı bileşeni, VM kullanılabilirlik kümeleri ve sanal makine ölçek kümeleri dahil olmak üzere Azure IaaS tabanlı iş yüklerini ve ilgili ağ bileşenlerini barındırır.

    -   **PaaS bağlı bileşen:** PaaS bağlı, özel bir [uç nokta](../private-link/private-endpoint-overview.md) ve [özel bağlantı](../private-link/private-link-overview.md)sayesinde özel adresleme kullanarak Azure PaaS hizmetlerini barındırır.

-   **Azure Güvenlik Duvarı:** Bağlı bileşen ve Azure VMware çözümü arasındaki trafiği segmentlere ayırmak için merkezi bir parça olarak davranır.

-   **Application Gateway:** Azure IaaS/PaaS veya Azure VMware Çözüm sanal makineleri (VM 'Ler) üzerinde çalışan Web uygulamalarını gösterir ve korur. API Management gibi diğer hizmetlerle tümleştirilir.

## <a name="network-and-security-considerations"></a>Ağ ve güvenlik konuları

ExpressRoute bağlantıları, trafiği şirket içi, Azure VMware çözümü ve Azure ağ yapısı arasında akışa sağlar. Azure VMware çözümü, bu bağlantıyı uygulamak için [ExpressRoute Global Reach](../expressroute/expressroute-global-reach.md) kullanır.

ExpressRoute ağ geçidi, bağlı devreleri arasında geçişli yönlendirme sağlamadığından, şirket içi bağlantı, şirket içi vSphere ortamı ile Azure VMware çözümü arasında iletişim kurmak için ExpressRoute Global Reach de kullanmalıdır. 

* **Şirket içinden Azure VMware Çözüm trafiği akışı**

  :::image type="content" source="./media/hub-spoke/on-premises-azure-vmware-solution-traffic-flow.png" alt-text="Azure VMware Çözüm Merkezi ve bağlı bileşen tümleştirme dağıtımı" border="false" lightbox="./media/hub-spoke/on-premises-azure-vmware-solution-traffic-flow.png":::


* **Azure VMware çözümü ile hub VNET trafik akışı**

  :::image type="content" source="./media/hub-spoke/azure-vmware-solution-hub-vnet-traffic-flow.png" alt-text="Azure VMware Çözüm Merkezi ve bağlı bileşen tümleştirme dağıtımı" border="false" lightbox="./media/hub-spoke/azure-vmware-solution-hub-vnet-traffic-flow.png":::


Azure VMware Çözüm ağı ve bağlantı kavramları hakkında daha fazla ayrıntıyı [Azure VMware çözüm ürün belgelerinde](./concepts-networking.md)bulabilirsiniz.

### <a name="traffic-segmentation"></a>Trafik kesimlemesi

[Azure Güvenlik Duvarı](../firewall/index.yml) , hub sanal ağında dağıtılan merkez ve bağlı bileşen topolojisinin merkezi parçasıdır. Trafik kuralları oluşturmak ve farklı ışınsal ve Azure VMware Çözüm iş yükleri arasındaki iletişimi segmentlere ayırmak için Azure Güvenlik Duvarı veya başka bir Azure desteklenen ağ sanal gereci kullanın.

Trafiği Azure Güvenlik Duvarı 'na yönlendirmek için yol tabloları oluşturun.  Bağlı olan sanal ağlar için, Azure Güvenlik duvarının iç arabirimine varsayılan yolu ayarlayan bir yol oluşturun. Bu şekilde, sanal ağdaki bir iş yükünün Azure VMware çözüm adres alanına ulaşması gerektiğinde, güvenlik duvarı bunu değerlendirebilir ve buna izin vermek veya reddetmek için ilgili trafik kuralını uygulayabilir.  

:::image type="content" source="media/hub-spoke/create-route-table-to-direct-traffic.png" alt-text="Azure VMware Çözüm Merkezi ve bağlı bileşen tümleştirme dağıtımı" lightbox="media/hub-spoke/create-route-table-to-direct-traffic.png":::


> [!IMPORTANT]
> **Gatewaysubnet** ayarında 0.0.0.0/0 adres ön ekine sahip bir yol desteklenmez.

Karşılık gelen yol tablosundaki belirli ağların yollarını ayarlayın. Örneğin, Azure VMware Çözüm yönetimine ve iş yüklerinden gelen IP öneklerini, bağlı olan iş yüklerinden ve diğer yollardan ulaşmaya yönlendirir.

:::image type="content" source="media/hub-spoke/specify-gateway-subnet-for-route-table.png" alt-text="Azure VMware Çözüm Merkezi ve bağlı bileşen tümleştirme dağıtımı" lightbox="media/hub-spoke/specify-gateway-subnet-for-route-table.png":::

Daha ayrıntılı bir trafik ilkesi oluşturmak için, tekerlek ve hub içindeki ağ güvenlik grupları kullanılarak yapılan ikinci bir trafik düzeyi.

> [!NOTE]
> **Şirket Içinden Azure VMware çözümüne giden trafik:** VSphere tabanlı veya diğerleri olan şirket içi iş yükleri arasındaki trafik, Global Reach tarafından etkinleştirilir, ancak trafik hub 'daki Azure Güvenlik Duvarı 'na gitmez. Bu senaryoda, şirket içinde veya Azure VMware çözümünde trafik segmentleme mekanizmalarını uygulamanız gerekir.

### <a name="application-gateway"></a>Application Gateway

Azure Application Gateway v1 ve v2, Azure VMware Çözüm VM 'lerinde arka uç havuzu olarak çalışan Web Apps ile test edilmiştir. Application Gateway Şu anda Azure VMware Çözüm VM 'lerinde çalışan Web uygulamalarını internet 'e sunmak için desteklenen tek yöntemdir. Ayrıca, uygulamaları iç kullanıcılara güvenli bir şekilde kullanıma sunabilir.

Ayrıntılar ve gereksinimler için [Application Gateway](./protect-azure-vmware-solution-with-application-gateway.md) Azure VMware çözümüne özgü makaleyi gözden geçirin.

:::image type="content" source="media/hub-spoke/azure-vmware-solution-second-level-traffic-segmentation.png" alt-text="Azure VMware Çözüm Merkezi ve bağlı bileşen tümleştirme dağıtımı" border="false":::


### <a name="jump-box-and-azure-bastion"></a>Sıçrama kutusu ve Azure savunma

Azure VMware Çözüm ortamına, hub sanal ağı içindeki paylaşılan hizmet alt ağında dağıtılan bir Windows 10 veya Windows Server VM olan bir geçiş kutusuyla erişin.

>[!IMPORTANT]
>Azure 'da, Azure VMware çözümünün Internet 'e sunulmasını engellemek için bağlantı kutusu 'na bağlanmanız önerilir. Azure IaaS nesneleri olmadığından Azure VMware Çözüm VM 'lerine bağlanmak için Azure savunma 'yi kullanamazsınız.  

En iyi güvenlik uygulaması olarak, hub sanal ağı içinde [Microsoft Azure](../bastion/index.yml) savunma hizmeti dağıtın. Azure savunma, Azure 'da dağıtılan VM 'lere, bu kaynaklara genel IP adresleri sağlamaya gerek kalmadan sorunsuz RDP ve SSH erişimi sağlar. Azure savunma hizmetini sağladığınızda, seçili VM 'ye Azure portal erişebilirsiniz. Bağlantı kurulduktan sonra, geçiş kutusu masaüstünü gösteren yeni bir sekme açılır ve bu masaüstünden Azure VMware çözümüne özel bulut yönetim düzlemine erişebilirsiniz.

> [!IMPORTANT]
> Bağlantı kutusu VM 'sine genel bir IP adresi vermeyin veya 3389/TCP bağlantı noktasını genel İnternet 'e sunmaz. 


:::image type="content" source="media/hub-spoke/azure-bastion-hub-vnet.png" alt-text="Azure VMware Çözüm Merkezi ve bağlı bileşen tümleştirme dağıtımı" border="false":::


## <a name="azure-dns-resolution-considerations"></a>Azure DNS çözümleme konuları

Azure DNS çözümleme için kullanılabilecek iki seçenek vardır:

-   Hub üzerinde dağıtılan Azure Active Directory (Azure AD) etki alanı denetleyicilerini ( [kimlik hususları](#identity-considerations)bölümünde açıklanmıştır) ad sunucuları olarak kullanın.

-   Azure DNS bir özel bölge dağıtın ve yapılandırın.

En iyi yaklaşım, Azure VMware çözümü, şirket içi ve Azure için güvenilir ad çözümlemesi sağlamak üzere her ikisini de birleştirmek içindir.

Genel bir tasarım önerisi olarak, hub sanal ağında dağıtılan ve bağlı olan sanal ağlarda, DNS ayarlarında bu Azure DNS sunucularını kullanmak için yapılandırılmış olan en az iki Azure VM 'ye dağıtılan mevcut Azure DNS altyapısını (Bu örnekte Active Directory tümleşik DNS) kullanın.

Azure Özel DNS bölgesinin sanal ağa bağlandığı Azure Özel DNS kullanabilirsiniz.  DNS sunucuları, Azure Özel DNS altyapısını kullanarak DNS çalıştıran şirket içi veya Azure VMware çözümüne koşullu iletme ile karma çözümleyiciler olarak kullanılır. 

Azure DNS özel bölgeler için göz önünde bulundurmanız gereken bazı noktalar vardır:

* Bağlı olan sanal ağlarda dağıtılan VM 'Ler için DNS kayıtlarının yaşam döngüsünü otomatik olarak yönetmesi Azure DNS için otomatik kayıt etkinleştirilmelidir.
* Bir sanal ağın, oto kaydı etkin ile bağlantılandırılan en fazla özel DNS bölgesi sayısı yalnızca bir tane olabilir.
* Bir sanal ağın bağlı olduğu en fazla özel DNS bölgesi sayısı, tekrar kayıt özelliği olmadan 1000 ' dir.

Şirket içi ve Azure VMware Çözüm sunucuları, Azure Özel DNS bölgesi için Azure 'daki sanal makine çözümleyicilerine koşullu ileticilerle yapılandırılabilir.

## <a name="identity-considerations"></a>Kimlik konuları

Kimlik açısından en iyi yaklaşım, hub 'da en az bir AD etki alanı denetleyicisi dağıtmaktır. Bölge tarafından dağıtılan veya VM kullanılabilirlik kümesindeki iki paylaşılan hizmet alt ağını kullanın. Şirket içi AD etki alanınızı Azure 'a genişletmek için [Azure mimari merkezi](/azure/architecture/reference-architectures/identity/adds-extend-domain) bakın.

Ayrıca, vSphere ortamında kimlik ve DNS kaynağı olarak görev yapmak için Azure VMware Çözüm tarafında başka bir etki alanı denetleyicisi dağıtın.

Önerilen en iyi yöntem olarak, [ad etki alanını Azure Active Directory ile](/azure/architecture/reference-architectures/identity/azure-ad)tümleştirin.

<!-- LINKS - external -->
[Azure Architecture Center]: /azure/architecture/

[Hub & Spoke topology]: /azure/architecture/reference-architectures/hybrid-networking/hub-spoke

[Azure networking documentation]: ../networking/index.yml

<!-- LINKS - internal -->
