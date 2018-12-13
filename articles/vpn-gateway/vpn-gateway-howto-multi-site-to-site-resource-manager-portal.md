---
title: 'Birden çok VPN gateway siteden siteye bağlantıları bir sanal ağa ekleyin: Azure Portal: Resource Manager | Microsoft Docs'
description: Var olan bir bağlantısı olan bir VPN ağ geçidi için çok siteli S2S bağlantısı ekleme
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: jpconnock
editor: ''
tags: azure-resource-manager
ms.assetid: f3e8b165-f20a-42ab-afbb-bf60974bb4b1
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/14/2018
ms.author: cherylmc
ms.openlocfilehash: a814834be3225764c3b6f237bd515ca087f975a7
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52873130"
---
# <a name="add-a-site-to-site-connection-to-a-vnet-with-an-existing-vpn-gateway-connection"></a>Bir sanal ağa bir VPN ağ geçidi bağlantısı var olan bir siteden siteye bağlantı ekleme

> [!div class="op_single_selector"]
> * [Azure portal](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md)
> * [PowerShell (klasik)](vpn-gateway-multi-site.md)
>
> 

Bu makalede Azure portalını kullanarak mevcut bir bağlantısı olan bir VPN ağ geçidine siteden siteye (S2S) bağlantı eklemenize yardımcı olur. Bu bağlantı türü genellikle "çok siteli" yapılandırmayı olarak adlandırılır. Bir S2S bağlantısı, noktadan siteye bağlantısı veya VNet-VNet bağlantısı zaten bir sanal ağa bir S2S bağlantısı ekleyebilirsiniz. Bağlantılar eklerken bazı sınırlamalar vardır. Denetleme [başlamadan önce](#before) yapılandırmanıza başlamadan önce doğrulamak için bu makaledeki bir bölüm. 

Bu makalede, Resource Manager RouteBased VPN ağ geçidi olan sanal ağlar için geçerlidir. Bu adımlar, ExpressRoute/için siteden siteye bağlantı yapılandırmaları uygulanmaz. Bkz: [ExpressRoute/S2S bir arada var olabilen bağlantılar](../expressroute/expressroute-howto-coexist-resource-manager.md) arada var olabilen bağlantılar hakkında bilgi için.

### <a name="deployment-models-and-methods"></a>Dağıtım modelleri ve yöntemleri
[!INCLUDE [vpn-gateway-classic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

Bu tabloda yeni makaleler güncelleştiriyoruz ve bu yapılandırma için ek araçlar kullanılabilir. Bir makale kullanılabilir olduğunda, doğrudan bu tablodan bağlantısı verilir.

[!INCLUDE [vpn-gateway-table-multi-site](../../includes/vpn-gateway-table-multisite-include.md)]

## <a name="before"></a>Başlamadan önce
Aşağıdakileri doğrulayın:

* Bir arada var olabilen ExpressRoute/S2S bağlantısı oluşturma değil.
* Var olan bir bağlantı ile Resource Manager dağıtım modeli kullanılarak oluşturulmuş bir sanal ağ var.
* RouteBased ağınız için sanal ağ geçididir. PolicyBased VPN ağ geçidi varsa, sanal ağ geçidini silme ve RouteBased olarak yeni bir VPN ağ geçidi oluşturmanız gerekir.
* Bu sanal ağa bağlandığını Vnet'ler hiçbiri için adres aralıklarını hiçbiri çakışıyor.
* Uyumlu VPN cihazı ve bunu yapıp birisi var. Bkz. [VPN Cihazları Hakkında](vpn-gateway-about-vpn-devices.md). VPN cihazınızı yapılandırma konusuyla veya şirket içi ağ yapılandırmanızdaki IP adresi aralıklarıyla ilgili fazla bilginiz yoksa size bu ayrıntıları sağlayabilecek biriyle çalışmanız gerekir.
* VPN cihazınız için dışarıya yönelik genel bir IP adresi var. Bu IP adresi bir NAT’nin arkasında olamaz.

## <a name="part1"></a>1. Bölüm - bağlantı yapılandırma
1. Tarayıcıdan [Azure portalına](http://portal.azure.com) gidin ve gerekiyorsa Azure hesabınızda oturum açın.
2. Tıklayın **tüm kaynakları** bulun, **sanal ağ geçidi** kaynakları listesinden bulup tıklayın.
3. Üzerinde **sanal ağ geçidi** sayfasında **bağlantıları**.
   
    ![Bağlantılar sayfası](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/connectionsblade.png "Connections page")<br>
4. Üzerinde **bağlantıları** sayfasında **+ Ekle**.
   
    ![Ekle bağlantısını düğmesini](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/addbutton.png "Ekle bağlantı düğmesi")<br>
5. Üzerinde **Bağlantı Ekle** sayfasında, aşağıdaki alanları doldurun:
   
   * **Ad:** siteye vermek istediğiniz adı bağlantı oluşturuyorsunuz.
   * **Bağlantı türü:** seçin **siteden siteye (IPSec)**.
     
     ![Ekle bağlantısını sayfasını](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/addconnectionblade.png "Ekle bağlantı sayfası")<br>

## <a name="part2"></a>Bölüm 2 - bir yerel ağ geçidi ekleme
1. Tıklayın **yerel ağ geçidi** ***bir yerel ağ geçidi seçin***. Bu açılır **yerel ağ geçidi Seç** sayfası.
   
    ![Yerel ağ geçidi Seç](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/chooselng.png "yerel ağ geçidi Seç")<br>
2. Tıklayın **Yeni Oluştur** açmak için **yerel ağ geçidi Oluştur** sayfası.
   
    ![Oluşturma yerel ağ geçidi sayfasının](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/createlngblade.png "yerel ağ geçidi oluştur")<br>
3. Üzerinde **yerel ağ geçidi Oluştur** sayfasında, aşağıdaki alanları doldurun:
   
   * **Ad:** yerel ağ geçidi kaynağı için vermek istediğiniz adı.
   * **IP adresi:** sitesinde bağlanmak istediğiniz VPN cihazının genel IP adresi.
   * **Adres alanı:** yeni bir yerel ağ alanına yönlendirilmesini istediğiniz adres alanı.
4. Tıklayın **Tamam** üzerinde **yerel ağ geçidi Oluştur** değişiklikleri kaydetmek için sayfa.

## <a name="part3"></a>3. bölüm - paylaşılan bir anahtar ekleyin ve bağlantıyı oluşturun
1. Üzerinde **Bağlantı Ekle** sayfasında, bağlantınızı oluşturmak için kullanmak istediğiniz paylaşılan anahtarı ekleyin. VPN cihazınızın paylaşılan anahtarı alma veya bir burada olun ve sonra VPN cihazının aynı paylaşılan anahtarı kullanacak şekilde yapılandırın. Önemli şey, anahtarlarının tam olarak aynı olduğundan emin olabilir.
   
    ![Paylaşılan anahtar](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/sharedkey.png "Shared key")<br>
2. Sayfanın en altında tıklayın **Tamam** bağlantı oluşturmak için.

## <a name="part4"></a>4. Kısım - VPN bağlantısını doğrulama


[!INCLUDE [vpn-gateway-verify-connection-ps-rm](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bağlantınız tamamlandıktan sonra sanal ağlarınıza sanal makineler ekleyebilirsiniz. Bkz: [sanal makineler öğrenme yolu](/learn/paths/deploy-a-website-with-azure-virtual-machines/) daha fazla bilgi için.
