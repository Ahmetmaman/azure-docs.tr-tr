---
title: ExpressRoute genel ulaşma hakkında | Microsoft Docs
description: Bu makale, ExpressRoute Global erişim ne olduğunu açıklar.
documentationcenter: na
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 10/09/2018
ms.author: cherylmc
ms.openlocfilehash: c96841274f73be766b0157f036746599cc836dd3
ms.sourcegitcommit: 7b0778a1488e8fd70ee57e55bde783a69521c912
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "49068052"
---
# <a name="expressroute-global-reach-preview"></a>ExpressRoute küresel erişim (Önizleme)
ExpressRoute, Microsoft Cloud, şirket içi ağlara bağlanmak için özel ve esnek bir yoludur. Özel veri Merkezinize veya şirket ağınızda Azure, Office 365 ve Dynamics 365 gibi pek çok Microsoft bulut hizmetlerine erişebilir. Örneğin, bir ExpressRoute bağlantı hattı Silikon ve ile ExpressRoute bağlantı hattına aynı şehirdeki başka bir şube ofisindeki, Londra, San Francisco bir şube ofisi olabilir. Her iki şube ofisleri, ABD Batı ve UK Güney Azure kaynaklarını yüksek hızlı bağlantıyı olabilir. Ancak, şube ofislerinde doğrudan birbirleri ile veri değişimi olamaz. Diğer bir deyişle, 10.0.1.0/24 10.0.3.0/24 ve 10.0.4.0/24, ancak 10.0.2.0/24 veriler gönderebilir.

![Olmadan][1]

İle **ExpressRoute Global erişim**, birlikte özel bir ağ, şirket içi ağlar arasında yapmak için ExpressRoute bağlantı hattına bağlayabilirsiniz. Yukarıdaki örnekte, ek olarak, ExpressRoute Global erişim, İstanbul ofis (10.0.1.0/24) verilerle İstanbul ofis (10.0.2.0/24) var olan ExpressRoute bağlantı hatları aracılığıyla ve global Microsoft ağı üzerinden doğrudan değiştirebilir. 

![ile][2]

## <a name="use-case"></a>Kullanım Örneği
ExpressRoute Global erişim hizmet sağlayıcınızın WAN uygulama tamamlar ve şube ofislerinizde dünya genelindeki bağlanmak için tasarlanmıştır. Örneğin, hizmet sağlayıcınıza öncelikli olarak Amerika Birleşik Devletleri'nde çalışır ve tüm Dallarınızın ABD'deki bağladığından, ancak hizmet sağlayıcısı, Japonya ve Hong Kong çalışmaz, ExpressRoute Global erişim ile bir yerel hizmet sağlayıcısı ile çalışabilir ve Microsoft, ExpressRoute ile global ağımız ABD dışındaki Dallarınızı var. bağlanır.

![Kullanım örneği][3]

## <a name="availability"></a>Kullanılabilirlik 
ExpressRoute Global erişim şu anda aşağıdaki konumlarda desteklenir.

* Amerika Birleşik Devletleri
* Birleşik Krallık 
* İrlanda
* Hollanda
* Avustralya
* Hong Kong 
* Japonya

Konumunda, ExpressRoute devreleri oluşturulması [ExpressRoute eşleme konumlarına](expressroute-locations.md) yukarıdaki ülke veya bölge. ExpressRoute Global erişim arasında etkinleştirmek için [farklı coğrafi bölgeler](expressroute-locations.md), Premium SKU, devreler olması gerekir.

## <a name="next-steps"></a>Sonraki Adımlar
1. [ExpressRoute Global erişim hakkında daha fazla bilgi edinin](expressroute-faqs.md)
2. [ExpressRoute Global erişim olanağı tanıma](expressroute-howto-set-global-reach.md)
3. [ExpressRoute bağlantı hattı için Azure sanal ağı bağlama](expressroute-howto-linkvnet-arm.md)


<!--Image References-->
[1]: ./media/expressroute-global-reach/1.png "Küresel erişim olmadan diyagramı"
[2]: ./media/expressroute-global-reach/2.png "Küresel erişime sahip diyagramı"
[3]: ./media/expressroute-global-reach/3.png "Küresel erişim durumunun kullanın"
