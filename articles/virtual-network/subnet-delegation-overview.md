---
title: Azure sanal ağında alt ağ temsili nedir?
description: Azure sanal ağ 'da alt ağ temsili hakkında bilgi edinin
services: virtual-network
documentationcenter: na
author: KumudD
manager: mtillman
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/20/2019
ms.author: kumud
ms.openlocfilehash: 1fbb683754aed5b2a2e6e9c022713b7e87ad9ba9
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91329216"
---
# <a name="what-is-subnet-delegation"></a>Alt ağ temsili nedir?

Alt ağ temsili, seçtiğiniz bir Azure PaaS hizmeti için sanal ağınıza eklenmesi gereken belirli bir alt ağı belirlemenizi sağlar. Alt ağ temsili, Azure hizmetlerinin sanal ağlarına tümleştirilmesini yönetme konusunda müşteriye tam denetim sağlar.

Bir Azure hizmetine bir alt ağ temsilcisinden, bu hizmetin bu alt ağ için bazı temel ağ yapılandırma kuralları kurmasını sağlar ve bu da Azure hizmetinin örneklerini kararlı bir şekilde çalıştırabilmesini sağlar. Sonuç olarak, Azure hizmeti, şöyle bir dağıtım öncesi veya sonrası koşulları oluşturabilir:
- hizmeti paylaşılan ve ayrılmış bir alt ağda dağıtın.
- hizmetin düzgün çalışması için gerekli olan bir dizi ağ hedefi Ilkesi dağıtımına ekleyin.

##  <a name="advantages-of-subnet-delegation"></a>Alt ağ temsilcisinin avantajları

Belirli hizmetlere bir alt ağ temsilciliği vermek aşağıdaki avantajları sağlar:

- bir veya daha fazla Azure hizmeti için alt ağ atamanıza ve alt ağdaki örnekleri gereksinimlere göre yönetmeye yardımcı olur. Örneğin, sanal ağ sahibi, kaynakları ve erişimi daha iyi yönetmek için yetkilendirilmiş bir alt ağ için aşağıdakileri tanımlayabilir:
    - ağ güvenlik grupları ile ağ filtreleme trafiği ilkeleri.
    - Kullanıcı tanımlı yollarla ilkeleri yönlendirme.
    - hizmet uç noktası yapılandırmalarına sahip hizmetler tümleştirmesi.
- , ağ hedefi Ilkeleri biçiminde dağıtımların ön koşullarını tanımlayarak hizmetlerin, sanal ağla daha iyi tümleşmesine yardımcı olur. Bu, eklenen hizmetin çalışmasını etkileyebilecek eylemlerin PUT sırasında engellenmesini sağlar.


## <a name="who-can-delegate"></a>Kimler temsil edebilir?
Alt ağ temsili, sanal ağ sahiplerinin belirli bir Azure hizmeti için alt ağlardan birini belirlemek üzere gerçekleştirmesi gereken bir uygulamadır. İçindeki Azure hizmeti, müşteri iş yüklerinin tüketimi için örnekleri bu alt ağa dağıtır.

## <a name="impact-of-subnet-delegation-on-your-subnet"></a>Alt ağınızdaki alt ağ temsilcisinin etkisi
Her bir Azure hizmeti kendi dağıtım modelini tanımlar ve bu özellikler, aşağıdaki gibi, temsilci olarak atanmış bir alt ağda ne yaptığını veya desteklemedikleri özellikleri tanımlayabilir:
- aynı alt ağda bulunan diğer Azure hizmetleri veya VM/sanal makine ölçek kümesi ile paylaşılan alt ağ veya yalnızca bu hizmetin örneklerine sahip ayrılmış bir alt ağı destekler.
- , temsilci alt ağıyla NSG ilişkilendirmesini destekler.
- , yetkilendirilmiş alt ağ ile ilişkili NSG 'yi destekler, başka bir alt ağ ile de ilişkilendirilebilir.
- temsilci alt ağıyla yol tablosu ilişkilendirmesini sağlar.
- temsil edilen alt ağla ilişkili yol tablosunun başka bir alt ağla ilişkilendirilmesine izin verir.
- temsilci alt ağdaki en az IP adresi sayısını belirler.
- temsil edilen alt ağdaki IP adresi alanının özel IP adres alanından (10.0.0.0/8, 192.168.0.0/16, 172.16.0.0/12) olduğunu belirler.
- özel DNS yapılandırmasının bir Azure DNS girişi olduğunu belirler.
- alt ağ veya sanal ağın silinebilmesi için önce temsilciyi kaldırma gerekir.

Eklenen hizmetler, aşağıdaki gibi kendi ilkelerini de ekleyebilir:
- **Güvenlik ilkeleri**: belirli bir hizmetin çalışması için gereken güvenlik kuralları koleksiyonu.
- **Yol ilkeleri**: belirli bir hizmetin çalışması için gereken yolların toplanması.

## <a name="what-subnet-delegation-does-not-do"></a>Alt ağ temsili ne yapmaz

Yetkilendirilmiş bir alt ağa eklenen Azure Hizmetleri, temsilci olmayan alt ağlar için kullanılabilen temel özellikler kümesine hala sahiptir, örneğin:
-  Azure Hizmetleri, örnekleri müşteri alt ağlarına ekleyebilir, ancak mevcut iş yüklerini etkilemez.
-  Bu hizmetlerin uygulandığı ilkeler veya rotalar esnektir ve müşteri tarafından geçersiz kılınabilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Bir alt ağ için temsilci seçme](manage-subnet-delegation.md)
