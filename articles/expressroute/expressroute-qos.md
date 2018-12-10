---
title: 'QoS gereksinimleri - ExpressRoute: Azure | Microsoft Docs'
description: Bu sayfa, yapılandırma ve hizmet kalitesi yönetmeye yönelik ayrıntılı gereksinimler sağlanmıştır. Skype kurumsal iş/ses Hizmetleri ele alınmıştır.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: cherylmc
ms.custom: seodec18
ms.openlocfilehash: cb9acb5434fad6cd7898be14404c5ed4125ab96a
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2018
ms.locfileid: "53141178"
---
# <a name="expressroute-qos-requirements"></a>ExpressRoute QoS gereksinimleri
Skype Kurumsal’da farklı QoS davranışları gerektiren çeşitli iş yükleri vardır. ExpressRoute aracılığıyla ses hizmetleri kullanmayı planlıyorsanız, aşağıda açıklanan gereksinimlere uymanız gerekir.

![](./media/expressroute-qos/expressroute-qos.png)

> [!NOTE]
> QoS gereksinimleri yalnızca Microsoft eşlemeleri için geçerlidir. Azure ortak eşleme ve Azure özel eşlemesinde alınan ağ trafiğinizdeki DSCP değerleri 0’a ayarlanacaktır. 
> 
> 

Aşağıdaki tabloda Skype Kurumsal tarafından kullanılan DSCP işaretlerinin bir listesi verilmiştir. Daha fazla bilgi için [Skype Kurumsal için QoS’i yönetme](https://technet.microsoft.com/library/gg405409.aspx) bölümüne başvurun.

| **Trafik Sınıfı** | **İşleme (DSCP İşaretleme)** | **Skype Kurumsal İş Yükleri** |
| --- | --- | --- |
| **Ses** |EF (46) |Skype / Lync ses |
| **Etkileşimli** |AF41 (34) |Video, VBSS |
| AF21 (18) |Uygulama paylaşımı | |
| **Varsayılan** |AF11 (10) |Dosya aktarımı |
| CS0 (0) |Diğer | |

* İş yükleri sınıflandırmanız ve doğru DSCP değerlerini işaretlemeniz gerekir. Ağınızda DSCP işaretlerini ayarlamak için [burada](https://technet.microsoft.com/library/gg405409.aspx) sağlanan yönergeleri izleyin.
* Ağınızda çoklu QoS kuyruklarını yapılandırmanız ve desteklemeniz gerekir. Ses, tek başına bir sınıf olmalı ve ses için RFC 3246 içinde belirtilen EF işlemi uygulanmalıdır. 
* Kuyruğa alma mekanizması, sıkışma algılama ilkesi ve trafik sınıfı başına bant genişliği ayırmayı siz belirleyebilirsiniz. Ancak, Skype Kurumsal iş yükleri için DSCP işaretlemesi korunmalıdır. AF31 (26) gibi yukarıda listelenmeyen DSCP işaretlerini kullanıyorsanız, paketi Microsoft'a göndermeden önce bu DSCP değerinin 0 olarak yeniden yazılması gerekir. Microsoft yalnızca yukarıdaki tabloda gösterilen DSCP değeriyle işaretlenen paketleri gönderir. 

## <a name="next-steps"></a>Sonraki adımlar
* [Yönlendirme](expressroute-routing.md) ve [NAT](expressroute-nat.md) için gereksinimlere bakın.
* ExpressRoute bağlantınızı yapılandırmak için aşağıdaki bağlantılara bakın.
  
  * [ExpressRoute bağlantı hattı oluşturma](expressroute-howto-circuit-classic.md)
  * [Yönlendirmeyi yapılandırma](expressroute-howto-routing-classic.md)
  * [ExpressRoute bağlantı hattına bir Sanal Ağ bağlama](expressroute-howto-linkvnet-classic.md)

