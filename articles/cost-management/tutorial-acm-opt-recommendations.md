---
title: Öğretici - iyileştirme önerilerden yararlanarak Azure maliyetlerini azaltma | Microsoft Docs
description: Bu öğreticide, iyileştirme önerileri hareket olduğunda Azure maliyetleri azaltmanıza yardımcı olur.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 02/05/2019
ms.topic: conceptual
ms.service: cost-management
manager: dougeby
ms.custom: seodec18
ms.openlocfilehash: 2255f14d026d406dc41802c321d0a673ee2f2f23
ms.sourcegitcommit: 415742227ba5c3b089f7909aa16e0d8d5418f7fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/06/2019
ms.locfileid: "55766869"
---
# <a name="tutorial-optimize-costs-from-recommendations"></a>Öğretici: Önerilerden maliyetlerini iyileştirme

Azure maliyet yönetimi, maliyet iyileştirme önerileri sağlamak için Azure Danışmanı ile çalışır. Azure Danışmanı en iyi duruma getirmek ve boşta ve az kullanılan kaynakları belirleyerek verimliliğini geliştirmenize yardımcı olur. Bu öğretici size burada isteyeceğiniz az kullanılan Azure kaynakları tanımlamak ve maliyetleri azaltmak için eyleme sonra bir örneği açıklanmaktadır.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Maliyet iyileştirme önerileri olası kullanım verimsizliklerini görüntülemek için görüntüleme
> * Bir öneri daha uygun maliyetli bir seçenek için bir sanal makine yeniden boyutlandırmak için eylem gerçekleştir
> * Sanal makine başarıyla boyutlandırılmış emin olmak için gerekeni doğrulayın

## <a name="prerequisites"></a>Önkoşullar
Öneriler de dahil olmak üzere, Azure hesap türleri için çeşitli kullanılabilir [Kurumsal Anlaşma (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/) müşteriler. Desteklenen bir hesap türleri için tam listesini görüntülemek için bkz: [anlamak maliyet Yönetimi verilerine](understand-cost-mgt-data.md). Maliyet verilerini görüntülemek için aşağıdaki kapsamlardan birine veya daha fazlasına en azından yazma erişiminiz olmalıdır.

- Abonelik
- Kaynak grubu

Etkinlik için en az 14 gün etkin sanal makinelere sahip olmalıdır.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma
[https://portal.azure.com](https://portal.azure.com/) adresinden Azure portalında oturum açın.

## <a name="view-cost-optimization-recommendations"></a>Maliyet iyileştirme önerileri görüntüleme

Azure portalında, hizmetler listesinde **Maliyet Yönetimi + Faturalama**’ya tıklayın. Listenin altında sonra **maliyet Yönetimi**seçin **Danışmanı önerilerini**. Advisor maliyet önerileri gösterilir.

![Azure portalında gösterilen maliyet Yönetimi Danışmanı önerileri](./media/tutorial-acm-opt-recommendations/advisor-recommendations.png)

Öneriler listesi, kullanım verimsizliklerini tanımlar veya ek paradan tasarruf etmenize yardımcı olabilecek satın alma önerileri gösterilir. Toplam **olası yıllık tasarrufları** kapatma ya da tüm öneri kurallara uygun olan Vm'leriniz serbest kaydedebileceğiniz toplam tutarı gösterir. Bunları kapatmak istemiyorsanız, bunları daha ucuz bir VM SKU için yeniden boyutlandırma düşünmelisiniz.

**Etkisi** kategori ile birlikte **olası yıllık tasarrufları**, mümkün olduğunca tasarruf etme olasılığı olan önerileri belirlemenize yardımcı olması için tasarlanmıştır. Yüksek etkili önerilerdir [satın alma, Kullandıkça Öde maliyetlerinden tasarruf sağlamak için sanal makine örnekleri ayrılmış](../advisor/advisor-cost-recommendations.md#buy-reserved-virtual-machine-instances-to-save-money-over-pay-as-you-go-costs) ve [İyileştir sanal makine harcama yeniden boyutlandırabilir veya az kullanılan örneklerinikapatılıyor](../advisor/advisor-cost-recommendations.md#optimize-virtual-machine-spend-by-resizing-or-shutting-down-underutilized-instances). Orta etki önerilerdir [beklemediğiniz sağlanan bir ExpressRoute bağlantı hatları ortadan kaldırarak Ücretlerle](../advisor/advisor-cost-recommendations.md#reduce-costs-by-eliminating-unprovisioned-expressroute-circuits) ve [silme veya boşta olan sanal ağ geçitlerini yeniden Ücretlerle](../advisor/advisor-cost-recommendations.md#reduce-costs-by-deleting-or-reconfiguring-idle-virtual-network-gateways).

## <a name="act-on-a-recommendation"></a>Bir öneriye hareket

Azure Danışmanı için 14 gün, sanal makine kullanımını izler ve ardından az kullanılan sanal makineleri tanımlar. Yedi MB, CPU kullanımı yüzde veya daha az beştir ve ağ kullanımı sanal makineleri olması veya kullanımı düşük sanal makineleri için dört daha az veya daha fazla gün olarak kabul edilir.

%5 veya daha az CPU kullanımı varsayılan ayardır, ancak ayarları ayarlayabilirsiniz. Ayarını ayarlama hakkında daha fazla bilgi için bkz. [ortalama CPU kullanımı kuralı veya az kullanılan sanal makine önerilerini yapılandırma](../advisor/advisor-get-started.md#configure-low-usage-vm-recommendation).

Bazı senaryolar tasarım gereği düşük kullanımı sonuçlanabilir olsa da, genellikle daha ucuz boyutları için sanal makinelerinizin boyutunu değiştirerek tasarruf sağlayabilirsiniz. Bir yeniden boyutlandırma eylemi seçerseniz tasarruf ettiğiniz gerçek miktarlar değişiklik gösterebilir. Bir sanal makine yeniden boyutlandırma, bir örneği atalım.

Öneriler listesinde tıklayın **doğru boyuta getirin veya kapatma potansiyelinden az kullanılmasına neden sanal makineler** öneri. Sanal makine adayları listesinde, yeniden boyutlandırma ve ardından sanal makineyi bir sanal makine seçin. Kullanım ölçümlerini doğrulayabilir sanal makinenin ayrıntıları gösterilir. **Olası yıllık tasarrufları** değerdir, kapatma veya VM kaldırırsanız tasarruf edebilirsiniz. Bir VM'yi yeniden boyutlandırmadan büyük olasılıkla, maliyet tasarrufu sağlayacak, ancak tam olası yıllık tasarruf miktarı kaydetmez.

![Öneri ayrıntılarını örneği](./media/tutorial-acm-opt-recommendations/recommendation-details.png)

VM ayrıntılarında uygun boyutlandırma aday olduğundan emin olmak için sanal makine kullanımını denetleyin.

![Geçmiş kullanımını gösteren örnek sanal makine ayrıntıları](./media/tutorial-acm-opt-recommendations/vm-details.png)

Geçerli sanal makinenin boyutu unutmayın. Sanal makine yeniden boyutlandırıldı olduğunu doğruladıktan sonra sanal makinelerin listesini görebilmesi için sanal makine ayrıntıları kapatın.

Kapatma veya yeniden boyutlandırmak için adayları listesinde seçin **sanal makine**.
![Sanal makine yeniden boyutlandırma seçeneği ile örnek önerisi](./media/tutorial-acm-opt-recommendations/resize-vm.png)

Ardından, kullanılabilir boyutlandırma seçeneklerin bir listesi ile sunulur. Senaryonuz için en iyi performans ve uygun maliyet sağlayacak bir tane seçin. Aşağıdaki örnekte, gelen seçenek seçilen yeniden boyutlandırır bir **DS14\_V2** için bir **DS13\_V2**. Öneri aşağıdaki $551.30/ay ya da $6,615.60/yıl kaydeder.

![Bir boyut seçebileceğiniz kullanılabilir VM boyutlarını örnek listesi](./media/tutorial-acm-opt-recommendations/choose-size.png)

Uygun bir boyut seçin sonra tıklayın **seçin** boyutlandırma eylemi başlatmak için.

Yeniden boyutlandırma, yeniden başlatmak için etkin olarak çalışan bir sanal makineyi gerektirir. Bir üretim ortamında sanal makine ise iş saat sonra yeniden boyutlandırma işlemi çalıştırmanızı öneririz. Yeniden başlatma zamanlama tarafından kısa bir süre içinde kullanılabilir olmaması nedeniyle kesintileri azaltabilir.

## <a name="verify-the-action"></a>Eylemi doğrula

VM yeniden boyutlandırma işlemi başarıyla tamamlandığında, Azure bir bildirim gösterilir.

![Sanal makine yeniden boyutlandırıldı başarılı bildirimi](./media/tutorial-acm-opt-recommendations/resized-notification.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Maliyet iyileştirme önerileri olası kullanım verimsizliklerini görüntülemek için görüntüleme
> * Bir öneri daha uygun maliyetli bir seçenek için bir sanal makine yeniden boyutlandırmak için eylem gerçekleştir
> * Sanal makine başarıyla boyutlandırılmış emin olmak için gerekeni doğrulayın

Maliyet yönetimi en iyi yöntemler makalesi okumadıysanız, maliyetleri yönetmenize yardımcı olmak için dikkate alınması gereken üst düzey yönergeler ve sağlar.

> [!div class="nextstepaction"]
> [Maliyet yönetimi en iyi uygulamalar](cost-mgt-best-practices.md)
