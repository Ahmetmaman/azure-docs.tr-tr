---
title: Azure Application Gateway nedir?
description: Uygulamanıza web trafiğini yönetmek için bir Azure uygulama ağ geçidini nasıl kullanabileceğinizi öğrenin.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: overview
ms.custom: mvc
ms.date: 08/26/2020
ms.author: victorh
ms.openlocfilehash: 4344cd38d9a58eec27c6202e81b8ef678a510681
ms.sourcegitcommit: 24a12d4692c4a4c97f6e31a5fbda971695c4cd68
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2021
ms.locfileid: "102176017"
---
# <a name="what-is-azure-application-gateway"></a>Azure Application Gateway nedir?

Azure Application Gateway, web uygulamalarınıza trafiği yönetmenizi sağlayan bir web trafiği yük dengeleyicisidir. Geleneksel yük dengeleyiciler aktarım katmanında (OSI katman 4 - TCP ve UDP) çalışır ve trafiği kaynak IP adresi ve bağlantı noktasına göre hedef bir IP adresi ve bağlantı noktasına yönlendirir.

Application Gateway, bir HTTP isteğinin ek özniteliklerine (örneğin, URI yolu veya ana bilgisayar üstbilgileri) göre yönlendirme kararları verebilir. Örneğin, gelen URL’yi temel alarak trafiği yönlendirebilirsiniz. Yani `/images` gelen URL’deyse, trafiği görüntüler için yapılandırılmış belirli bir sunucu kümesine (havuz olarak da bilinir) yönlendirebilirsiniz. `/video`URL 'de ise, bu trafik videolar için iyileştirilmiş başka bir havuza yönlendirilir.

![imageURLroute](./media/application-gateway-url-route-overview/figure1-720.png)

Bu yönlendirme türü, uygulama katmanı (OSI katman 7) yük dengelemesi olarak bilinir. Azure Application Gateway, URL tabanlı yönlendirmeyi ve daha fazlasını yapabilir.

>[!NOTE]
> Azure, senaryolarınız için tam olarak yönetilen yük dengeleme çözümleri sunar. 
> * DNS tabanlı küresel yönlendirme yapmak istiyorsanız ve Aktarım Katmanı Güvenliği (TLS) protokolü sonlandırma ("SSL yük boşaltma"), HTTP/HTTPS isteği veya uygulama katmanı işleme gereksinimlerine sahip **değilseniz** [Traffic Manager](../traffic-manager/traffic-manager-overview.md)gözden geçirin. 
> * Web trafiğinizi küresel yönlendirmeyi iyileştirmeniz ve hızlı genel yük devretme ile en üst katman Son Kullanıcı performansını ve güvenilirliğini iyileştirmek için bkz. [ön kapı](../frontdoor/front-door-overview.md).
> * Ağ katmanı yük dengelemesi yapmak için [Load Balancer](../load-balancer/load-balancer-overview.md)gözden geçirin. 
> 
> Uçtan uca senaryolarınız, bu çözümlerin gerektiğinde birleştirilmesinin avantajlarından yararlanabilir.
> Azure yük dengeleme seçenekleri karşılaştırması için bkz. [Azure 'da Yük Dengeleme seçeneklerine genel bakış](/azure/architecture/guide/technology-choices/load-balancing-overview).


## <a name="features"></a>Özellikler

Application Gateway özellikleri hakkında bilgi edinmek için bkz. [Azure Application Gateway özellikleri](features.md).

## <a name="pricing-and-sla"></a>Fiyatlandırma ve SLA

Application Gateway fiyatlandırma bilgileri için bkz. [Application Gateway fiyatlandırması](https://azure.microsoft.com/pricing/details/application-gateway/).

Application Gateway SLA bilgileri için bkz. [APPLICATION Gateway SLA](https://azure.microsoft.com/support/legal/sla/application-gateway/v1_2/).

## <a name="whats-new"></a>Yenilikler

Azure Application Gateway yenilikleri hakkında bilgi edinmek için bkz. [Azure Updates](https://azure.microsoft.com/updates/?category=networking&query=Application%20Gateway).

## <a name="next-steps"></a>Sonraki adımlar

Gereksinimlerinize ve ortamınıza bağlı olarak, Azure portal, Azure PowerShell ya da Azure CLı kullanarak bir test Application Gateway oluşturabilirsiniz.

- [Hızlı başlangıç: Azure Application Gateway ile doğrudan web trafiği-Azure portal](quick-create-portal.md)
- [Hızlı Başlangıç: Azure Application Gateway ile web trafiğini yönlendirme - Azure PowerShell](quick-create-powershell.md)
- [Hızlı Başlangıç: Azure Application Gateway ile web trafiğini yönlendirme - Azure CLI](quick-create-cli.md)