---
title: Sanal merkez yönlendirmesi hakkında
titleSuffix: Azure Virtual WAN
description: Bu makalede sanal hub yönlendirmeyi açıklanmaktadır
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 02/17/2021
ms.author: cherylmc
ms.custom: fasttrack-edit
ms.openlocfilehash: c1a2a54bf2d4c5de3e6cfca66256f60592fc1f3e
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101737520"
---
# <a name="about-virtual-hub-routing"></a>Sanal merkez yönlendirmesi hakkında

Bir sanal hub 'daki yönlendirme özellikleri, Sınır Ağ Geçidi Protokolü (BGP) kullanarak ağ geçitleri arasındaki tüm yönlendirmeyi yöneten bir yönlendirici tarafından sağlanır. Bir sanal hub, siteden siteye VPN ağ geçidi, ExpressRoute ağ geçidi, Noktadan siteye ağ geçidi, Azure Güvenlik Duvarı gibi birden çok ağ geçidi içerebilir. Bu yönlendirici Ayrıca, bir sanal hub 'a bağlanan sanal ağlar arasında geçiş bağlantısı sağlar ve 50 Gbps 'in toplam verimini destekleyebilir. Bu yönlendirme özellikleri standart sanal WAN müşterileri için geçerlidir.

Yönlendirmeyi yapılandırmak için bkz. [sanal hub yönlendirmeyi yapılandırma](how-to-virtual-hub-routing.md).

## <a name="routing-concepts"></a><a name="concepts"></a>Yönlendirme kavramları

Aşağıdaki bölümlerde, sanal hub yönlendirmesinde temel kavramlar açıklanır.

### <a name="hub-route-table"></a><a name="hub-route"></a>Merkez yol tablosu

Bir sanal hub yol tablosu, bir veya daha fazla yol içerebilir. Rota, adını, etiketini, hedef türünü, hedef önekleri listesini ve bir paketin yönlendirileceği sonraki atlama bilgilerini içerir. Bir **bağlantı** genellikle bir rota tablosuna ilişkili veya yayan bir yönlendirme yapılandırmasına sahip olur.

### <a name="connections"></a><a name="connection"></a>Bağlantılar

Bağlantılar, yönlendirme yapılandırmasına sahip Kaynak Yöneticisi kaynaklardır. Dört bağlantı türü şunlardır:

* **VPN bağlantısı**: VPN sitesini bir sanal hub VPN Gateway 'e bağlar.
* **ExpressRoute bağlantısı**: bir ExpressRoute devresini bir sanal hub ExpressRoute ağ geçidine bağlar.
* **P2S yapılandırma bağlantısı**: BIR kullanıcı VPN (Noktadan siteye) yapılandırmasını bir sanal hub Kullanıcı VPN (Noktadan siteye) ağ geçidine bağlar.
* **Hub sanal ağ bağlantısı**: sanal ağları bir sanal hub 'a bağlar.

Kurulum sırasında bir sanal ağ bağlantısı için yönlendirme yapılandırmasını ayarlayabilirsiniz. Varsayılan olarak, tüm bağlantılar varsayılan yol tablosuna ilişkilendirir ve yayılır.

### <a name="association"></a><a name="association"></a>Kaldırma

Her bağlantı, bir yol tablosuyla ilişkilendirilir. Bir bağlantıyı bir yol tablosuyla ilişkilendirmek, trafiğin yol tablosundaki yollar olarak belirtilen hedefe gönderilmesine izin verir. Bağlantının Yönlendirme yapılandırması ilişkili yol tablosunu gösterir.  Aynı yol tablosuyla birden çok bağlantı ilişkilendirilebilir. Tüm VPN, ExpressRoute ve kullanıcı VPN bağlantıları aynı (varsayılan) yol tablosuyla ilişkilendirilir.

Varsayılan olarak, tüm bağlantılar bir sanal hub 'daki **varsayılan yol tablosuyla** ilişkilendirilir. Her sanal hub 'ın, statik yollar eklemek için düzenlenebilen kendi varsayılan yol tablosu vardır. Yollar, aynı öneklere göre dinamik olarak öğrenilen yolların önüne statik olarak eklendi.

:::image type="content" source="./media/about-virtual-hub-routing/concepts-association.png" alt-text="Kaldırma":::

### <a name="propagation"></a><a name="propagation"></a>Yayma

Bağlantılar yolları bir yol tablosuna dinamik olarak yayar. VPN bağlantısı, ExpressRoute bağlantısı veya P2S yapılandırma bağlantısıyla, rotalar sanal hub 'dan BGP kullanılarak şirket içi yönlendiriciye dağıtılır. Yollar bir veya birden çok yol tablosuna yayılamaz.

Her sanal hub için bir **yol tablosu** da kullanılabilir. None yol tablosuna yayılması, bağlantıdan yayılmaması için hiçbir yolun gerekli olmadığını gösterir. VPN, ExpressRoute ve kullanıcı VPN bağlantıları, rotaları aynı yol tabloları kümesine yayar.

:::image type="content" source="./media/about-virtual-hub-routing/concepts-propagation.png" alt-text="Yayma":::

### <a name="labels"></a><a name="labels"></a>Etiketler

Etiketler, yönlendirme tablolarını mantıksal olarak gruplamak için bir mekanizma sağlar. Bu, özellikle birden çok yol tablosuna yapılan bağlantılardan yolların yayılması sırasında yararlı olur. Örneğin, **varsayılan yol tablosunun** ' default ' adlı yerleşik bir etiketi vardır. Kullanıcılar bağlantı yollarını ' default ' etiketine yaydığı zaman, otomatik olarak sanal WAN 'daki her hub 'daki tüm varsayılan rota tablolarına uygulanır.

### <a name="configuring-static-routes-in-a-virtual-network-connection"></a><a name="static"></a>Bir sanal ağ bağlantısında statik yolları yapılandırma

Statik yolları yapılandırmak, bir sonraki atlama IP 'si aracılığıyla trafiği bir sanal hub 'a bağlı olan bir bağlı ağ VNet 'te sağlanan bir ağ sanal gereci (NVA) olabilecek bir mekanizma sağlar. Statik yol, bir yol adından, hedef ön eklerin listesinden ve bir sonraki atlama IP 'si ile oluşur.

## <a name="route-tables-for-pre-existing-routes"></a><a name="route"></a>Önceden var olan yollar için rota tabloları

Yol tablolarında ilişkilendirme ve yayma özellikleri artık vardır. Önceden var olan bir yol tablosu, bu özelliklere sahip olmayan bir yol tablosudur. Hub yönlendirmesinde önceden varolan yollar varsa ve yeni özellikleri kullanmak istiyorsanız, aşağıdakileri göz önünde bulundurun:

* **Sanal hub 'da önceden var olan yollara sahip standart sanal WAN müşterileri**:

   Azure portal Hub için yönlendirme bölümünde önceden var olan yollara sahipseniz, önce bunları silmeniz ve ardından yeni rota tabloları oluşturmaya (Azure portal Hub için yönlendirme tabloları bölümünde kullanılabilir) denemeniz gerekir.

* **Sanal hub 'da önceden var olan yollara sahip temel sanal WAN müşterileri**:

   Azure portal Hub için yönlendirme bölümünde önceden mevcut olan yollar varsa öncelikle bunları silmeniz ve ardından temel sanal WAN 'ınızı standart sanal WAN 'a **yükseltmeniz** gerekir. Bkz. [bir sanal WAN 'ı temel 'Ten standart sürümüne yükseltme](upgrade-virtual-wan.md).

## <a name="hub-reset"></a><a name="reset"></a>Hub sıfırlaması

Sanal hub **sıfırlaması** yalnızca Azure Portal kullanılabilir. Sıfırlama işlemi, yol tabloları, hub yönlendiricisi veya sanal hub kaynağının kendisi gibi başarısız kaynakları doğru maliyetli sağlama durumuna getirmek için bir yol sağlar. Destek için Microsoft 'a başvurmadan önce hub 'ı sıfırlamayı düşünün. Bu işlem, bir sanal hub 'daki ağ geçitlerini sıfırlamaz.

## <a name="additional-considerations"></a><a name="considerations"></a>Diğer konular

Sanal WAN yönlendirmeyi yapılandırırken aşağıdakileri göz önünde bulundurun:

* Tüm dal bağlantılarının (Noktadan siteye, siteden siteye ve ExpressRoute) varsayılan yol tablosuyla ilişkilendirilmesi gerekir. Bu şekilde, tüm dallar aynı ön ekleri öğrenmeyecektir.
* Tüm dal bağlantıları, yollarını aynı rota tabloları kümesine yaymalı. Örneğin, dalların varsayılan yol tablosuna yayılması gerektiğine karar verirseniz, bu yapılandırma tüm dallar arasında tutarlı olmalıdır. Sonuç olarak, varsayılan yol tablosuyla ilişkili tüm bağlantılar dalların tamamına erişebilecek.
* Azure Güvenlik Duvarı aracılığıyla dal dalı Şu anda desteklenmiyor.
* Azure Güvenlik Duvarı 'Nı birden çok bölgede kullanırken, tüm bağlı olan sanal ağların aynı yol tablosuyla ilişkilendirilmesi gerekir. Örneğin, Azure güvenlik duvarından geçiş yaparken sanal ağların bir alt kümesine sahip olma, Azure Güvenlik duvarını aynı sanal hub 'da atlayarak başka sanal ağlarda atlama mümkün değildir.
* Her VNet bağlantısı için tek bir sonraki atlama IP 'si yapılandırılabilir.

## <a name="next-steps"></a>Sonraki adımlar

* Yönlendirmeyi yapılandırmak için bkz. [sanal hub yönlendirmeyi yapılandırma](how-to-virtual-hub-routing.md).
* Sanal WAN hakkında daha fazla bilgi için bkz. [SSS](virtual-wan-faq.md).
