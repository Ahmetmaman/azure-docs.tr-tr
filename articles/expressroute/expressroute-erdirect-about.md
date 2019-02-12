---
title: ExpressRoute doğrudan - Azure hakkında | Microsoft Docs
description: Bu sayfa ExpressRoute doğrudan (Önizleme) genel bir bakış sağlar.
services: expressroute
author: mialdrid
ms.service: expressroute
ms.topic: conceptual
ms.date: 02/11/2019
ms.author: jaredro
ms.custom: seodec18
ms.openlocfilehash: 3124e98dd035080c2989849232b978c5d4a563ea
ms.sourcegitcommit: 39397603c8534d3d0623ae4efbeca153df8ed791
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2019
ms.locfileid: "56100746"
---
# <a name="about-expressroute-direct-preview"></a>ExpressRoute hakkında doğrudan (Önizleme)

ExpressRoute doğrudan, Microsoft'un küresel ağı dünya genelindeki stratejik dağıtılmış eşleme konumlarda doğrudan bağlanma özelliği sağlar. ExpressRoute doğrudan uygun ölçekte etkin/etkin bağlantı destekleyen çift 100 Gbps bağlantı sağlar.

ExpressRoute doğrudan sağlayan önemli özellikler dahil ancak bunlarla sınırlı değildir:

* Depolama ve Cosmos DB gibi hizmetler için Büyük Veri Alımı özelliği
* Fiziksel yalıtım gibi özel ve ayrı bağlantı gerektirir ve düzenlenen sektörler için: Banka, devlet ve perakende
* Bağlantı hattı dağıtımının iş birimine dayalı detaylı denetimi

> [!IMPORTANT]
> ExpressRoute doğrudan şu anda Önizleme aşamasındadır.
>
> Bu genel önizleme bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılmamalıdır. Belirli özellikler desteklenmiyor olabilir, kısıtlı yeteneklere sahip olabilir veya tüm Azure konumlarında mevcut olmayabilir. Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="enroll-in-the-preview"></a>Önizlemeye kaydolma

ExpressRoute doğrudan kullanmadan önce ilk önizleme aboneliğinizde kaydolmanız gerekir. Kaydetmek için bir e-posta Gönder <ExpressRouteDirect@microsoft.com> abonelik Kimliğinizi, aşağıdaki ayrıntılar dahil olmak üzere:

* Senaryo ile gerçekleştirmek için aradığınız **ExpressRoute doğrudan**
* Konum bkz - [iş ortakları ve eşleme konumları](expressroute-locations-providers.md) tüm konumlara tam listesi için
* Uygulama zaman çizelgesi
* Diğer sorular

## <a name="expressroute-using-a-service-provider-and-expressroute-direct"></a>ExpressRoute hizmet sağlayıcısı ve ExpressRoute doğrudan kullanma

| **ExpressRoute kullanarak bir hizmet sağlayıcısı** | **ExpressRoute doğrudan** | 
| --- | --- |
| Hızlı ekleme ve mevcut altyapısıyla bağlantısını etkinleştirmek için hizmet sağlayıcıları kullanır. | 100 GB/sn altyapı ve tam yönetim tüm katmanların gerektirir
| Sağlayıcı Ethernet ve MPLS gibi yüzlerce ile tümleşir | Düzenlenen sektör ve büyük veri alımı için doğrudan/adanmış kapasite |
| 50 MB/sn devreler SKU'lardan 10 GB/sn | Müşteri, aşağıdaki bağlantı hattı SKU'ları bir birleşimini seçebilirsiniz: 5 GB/sn, 10 GB/sn, 40 GB/sn, 100 GB/sn için 200 GB/sn'lik toplam sınırlı-
| Tek bir kiracı için en iyi duruma getirilmiş | Tek kiracılı/bulut hizmeti sağlayıcıları için en iyi duruma getirilmiş / birden çok iş birimleri

## <a name="expressroute-direct-circuits"></a>Doğrudan ExpressRoute bağlantı hatları

Microsoft Azure ExpressRoute, bağlantı sağlayıcı tarafından kolaylaştırılan özel bağlantı üzerinden şirket içi ağlarınızı Microsoft bulutuna genişletmenizi sağlar. ExpressRoute'u kullanarak Microsoft Azure, Office 365 ve Dynamics 365 gibi Microsoft bulut hizmetleriyle bağlantı kurabilirsiniz.  

Her eşleme konumunda, Microsoft'un genel ağ erişimi olan ve varsayılan olarak herhangi bir bölgede coğrafi bölge erişebilir ve tüm küresel bölgeye sahip premium devresi erişebilir.  

Çoğu senaryoda işlevselliğini çalışması için bir ExpressRoute hizmet sağlayıcısı'nı kullanan bağlantı hatlarına eşdeğerdir. Daha fazla ayrıntı düzeyi ve ExpressRoute doğrudan kullanarak sunulan yeni özellikleri desteklemek için doğrudan ExpressRoute bağlantı hatları üzerinde mevcut bazı temel özellikleri vardır.

## <a name="circuit-skus"></a>Bağlantı hattı SKU'ları

ExpressRoute doğrudan Azure depolama ve diğer büyük veri hizmetlerle büyük veri alma senaryolarını destekler. ExpressRoute bağlantı hattına ExpressRoute doğrudan üzerinde şimdi de destek **40 GB/sn** ve **100 GB/sn** bağlantı hattı SKU'ları. Fiziksel bağlantı noktası çiftleri **100 GB/sn** yalnızca ve herhangi bir birleşimini 200 GB/sn için en fazla 5 GB/sn, 10 GB/sn, 40 GB/sn, 100 GB/sn - bant genişlikleri birden çok sanal devresiyle olabilir. 

## <a name="vlan-tagging"></a>VLAN etiketleme

ExpressRoute doğrudan QinQ hem Dot1Q VLAN etiketleme destekler.

* **QinQ VLAN etiketleme** üzerinde yalıtılmış Yönlendirme etki alanları için sağlayan bir ExpressRoute bağlantı hattı temelinde. Azure, dinamik olarak bağlantı hattı oluşturma sırasında bir S etiketi ayırır ve değiştirilemez. Her eşleme (özel ve Microsoft) devreye benzersiz bir C-Tag VLAN yararlanacaktır. C-Tag devreler ExpressRoute doğrudan bağlantı noktalarında arasında benzersiz olması gerekli değildir.

* **Dot1Q VLAN etiketleme** tek bir VLAN etiketli için sağlayan bir ExpressRoute doğrudan bağlantı noktası çifti temelinde. Tüm devreler ve ExpressRoute doğrudan bağlantı noktası çifti üzerinde eşlemeler arasında bir eşleme kullanılan C-Tag benzersiz olması gerekir.

## <a name="workflow"></a>İş akışı

[ ![İş akışı](./media/expressroute-erdirect-about/workflow1.png)](./media/expressroute-erdirect-about/workflow1.png#lightbox)

## <a name="sla"></a>SLA

ExpressRoute doğrudan aynı kurumsal sınıf SLA ile küresel Microsoft ağına aktif/aktif yedekli bağlantılar sağlar. ExpressRoute altyapı gereksizdir ve küresel Microsoft ağına bağlantısı yedekli ve çeşitli ve uygun şekilde ölçeklenen müşteri gereksinimlerine sahip. Önizleme süresince olacaktır SLA ve yalnızca üretim dışı iş yükleri için kabul edilmesi.

## <a name="next-steps"></a>Sonraki adımlar

[ExpressRoute doğrudan yapılandırın](expressroute-howto-erdirect.md)
