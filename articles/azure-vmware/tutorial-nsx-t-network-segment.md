---
title: Öğretici-Azure VMware çözümünde NSX-T ağ kesimi oluşturma
description: VCenter 'da VM 'Ler için kullanılan NSX-T ağ kesimlerini oluşturmayı öğrenin
ms.topic: tutorial
ms.date: 09/21/2020
ms.openlocfilehash: 780cac15efc043b9ae44b77af1234adca3fec5a1
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91254542"
---
# <a name="tutorial-create-an-nsx-t-network-segment-in-azure-vmware-solution"></a>Öğretici: Azure VMware çözümünde NSX-T ağ kesimi oluşturma

NSX-T Manager 'da oluşturulan ağ kesimleri, vCenter 'daki sanal makineler (VM) için ağlar olarak kullanılır. VCenter 'da oluşturulan VM 'Ler NSX-T içinde oluşturulan ağ kesimlerine yerleştirilir ve vCenter 'da görünür.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Ağ kesimleri eklemek için NSX-T Yöneticisi 'nde gezin
> * Yeni bir ağ kesimi Ekle
> * VCenter 'daki yeni ağ kesimini gözlemleyin

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlayabilmeniz için vCenter ve NSX-T Manager yönetim arabirimlerine erişimi olan bir Azure VMware çözümü özel bulutu gereklidir. Daha fazla bilgi için bkz. [Azure 'Da VMware özel bulutunuz için ağı yapılandırma](tutorial-configure-networking.md) öğreticisi.

## <a name="provision-a-network-segment-in-nsx-t"></a>NSX-T içinde bir ağ kesimi sağlama

1. Özel bulutunuz için vCenter 'da, **SDDC-Datacenter > ağları** ' nı seçin ve henüz bir ağ olmadığını unutmayın.

   :::image type="content" source="media/nsxt/vcenter-without-ls01.png" alt-text="Özel bulutunuz için vCenter 'da, SDDC-Datacenter > ağları ' nı seçin ve henüz bir ağ olmadığını unutmayın.":::

1. Özel bulutunuz için NSX-T Yöneticisi ' nde **ağ**' ı seçin.

   :::image type="content" source="media/nsxt/nsxt-network-overview.png" alt-text="Özel bulutunuz için NSX-T Yöneticisi ' nde ağ ' ı seçin.":::

1. **Segmentleri**seçin.

   :::image type="content" source="media/nsxt/nsxt-select-segments.png" alt-text="Ağa Genel Bakış sayfasında segmentler ' i seçin.":::

1. NSX-T segmentlerine Genel Bakış sayfasında, **segment Ekle**' yi seçin. Özel bulut sağlama kapsamında üç segment oluşturulur ve VM 'Ler için kullanılamaz.  Bu amaçla yeni bir ağ kesimi eklemeniz gerekir.

   :::image type="content" source="media/nsxt/nsxt-segments-overview.png" alt-text="NSX-T segmentlerine Genel Bakış sayfasında, SEGMENT Ekle ' yi seçin.":::

1. Segmenti adlandırın, **bağlı ağ geçidi**olarak önceden yapılandırılmış Katman1 ağ geçidini (tntxx-T1) seçin, **türü** esnek olarak bırakın, önceden yapılandırılmış olan kaplama **taşıma bölgesini** (TNTXX-kaplaması-TZ) seçin ve ardından alt ağları ayarla ' yı seçin. Bu bölümdeki diğer tüm ayarlar ve **bağlantı noktaları** ve **kesim profilleri** varsayılan olarak yapılandırma olarak kalır.

   :::image type="content" source="media/nsxt/nsxt-create-segment-specs.png" alt-text="Segment adı, bağlı ağ geçidi ve tür ve taşıma bölgesini ayarlayın, sonra alt ağ ayarla ' yı seçin.":::

1. Yeni segment için ağ geçidinin IP adresini ayarlayın ve ardından **Ekle**' yi seçin. Kullandığınız IP adresinin çakışmayan bir RFC1918 adres bloğunda olması gerekir. Bu, yeni kesimdeki VM 'lere bağlanabilmenizi sağlar.

   :::image type="content" source="media/nsxt/nsxt-create-segment-gateway.png" alt-text="Yeni segment için ağ geçidinin IP adresini ayarlayın ve ardından Ekle ' yi seçin.":::

1. **Uygula** ' yı seçerek yeni ağ kesimini uygulayın ve ardından **Kaydet**' i seçerek yapılandırmayı kaydedin.

   :::image type="content" source="media/nsxt/nsxt-create-segment-apply.png" alt-text="Yeni ağ kesimini, Uygula ile NSX-T yapılandırmasına uygulayın.":::

   :::image type="content" source="media/nsxt/nsxt-create-segment-save.png" alt-text="Yeni ağ kesimini Kaydet ile NSX-T yapılandırmasına kaydedin.":::

1. Yeni ağ kesimi şimdi oluşturulmuştur ve **Hayır**seçeneğini belirleyerek segmenti yapılandırmaya devam etme seçeneğini reddedebilirsiniz.

   :::image type="content" source="media/nsxt/nsxt-create-segment-continue-no.png" alt-text="Yeni oluşturulan ağ segmentini, Hayır seçeneğini belirleyerek daha fazla yapılandırmaya reddedin.":::

1. **Ağ > segmentleri** seçerek ve yeni segmentin listede olduğunu (Bu durumda "LS01") seçerek NSX-T ' de yeni ağ kesiminin mevcut olduğunu onaylayın.

   :::image type="content" source="media/nsxt/nsxt-new-segment-overview-2.png" alt-text="Yeni ağ kesiminin NSX-T ' de mevcut olduğunu doğrulayın.":::

1. **Ağ > SDDC-Datacenter** ' ı seçerek ve yeni segmentin listede olduğunu (Bu durumda "LS01") seçerek yeni ağ kesiminin vCenter 'da mevcut olduğunu doğrulayın.

   :::image type="content" source="media/nsxt/vcenter-with-ls01-2.png" alt-text="Yeni ağ segmentinin vCenter 'da bulunduğunu onaylayın.":::

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, vCenter 'daki VM 'Ler için kullanılan NSX-T ağ kesimlerini oluşturdunuz. Artık [Azure VMware çözümünde VM 'leri dağıtmak için bir Içerik kitaplığı oluşturabilir](deploy-vm-content-library.md) ve bu öğreticide oluşturduğunuz ağda bir VM sağlayabilirsiniz.

Aksi takdirde, Azure VMware çözümünde özel bir buluta nasıl ExpressRoute Global Reach eşleme oluşturulacağını öğrenmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Şirket içi ortamlar için özel bir buluta](tutorial-expressroute-global-reach-private-cloud.md)

<!-- LINKS - external-->

<!-- LINKS - internal -->
