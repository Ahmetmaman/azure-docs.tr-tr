---
title: Avere vFXT önkoşulları-Azure
description: Azure için avere vFXT 'de abonelikler, Kotalar ve depolama hizmeti uç noktaları gibi bir küme oluşturmadan önce gerçekleştirilecek görevler hakkında bilgi edinin.
author: ekpgh
ms.service: avere-vfxt
ms.topic: how-to
ms.date: 01/21/2020
ms.author: rohogue
ms.openlocfilehash: d87f57873a4bb84b20df3da3880017d9ef2484a5
ms.sourcegitcommit: f88074c00f13bcb52eaa5416c61adc1259826ce7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/21/2020
ms.locfileid: "92342119"
---
# <a name="prepare-to-create-the-avere-vfxt"></a>Avere vFXT oluşturmaya hazırlanma

Bu makalede bir avere vFXT kümesi oluşturmak için önkoşul görevleri açıklanmaktadır.

## <a name="create-a-new-subscription"></a>Yeni abonelik oluştur

Yeni bir Azure aboneliği oluşturarak başlayın. Her bir avere vFXT projesi için ayrı bir abonelik kullanarak gider izlemeyi ve temizlemeyi kolaylaştırın ve küme iş akışını sağlarken diğer projelerin kotalardan veya kaynak azaltmasından etkilenmemesini sağlayın.

Azure portal yeni bir Azure aboneliği oluşturmak için:

1. [Abonelikler dikey penceresine](https://ms.portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) gidin
1. Üstteki **+ Ekle** düğmesine tıklayın
1. İstenirse oturum aç
1. Bir teklif seçin ve yeni bir abonelik oluşturma adımlarında ilerleyin

## <a name="configure-subscription-owner-permissions"></a>Abonelik sahibi izinlerini yapılandırma

Abonelik için sahip izinlerine sahip bir kullanıcının vFXT kümesini oluşturması gerekir. Küme oluşturma, bir sahibin yazılım hizmet koşullarını kabul etmesini ve değişiklikleri ağ ve depolama kaynaklarına yetkilendirmesine gerek duyar.

Sahip olmayan bir Azure kümesi için avere vFXT oluşturma izni veren bazı geçici çözümler vardır. Bu senaryolar, kaynakların kısıtlanması ve oluşturana ek Azure rolleri atanması ile ilgilidir. Bu durumların tümünde, bir abonelik sahibi ayrıca [avere vFXT yazılım koşullarını](#accept-software-terms) zaman önüne kabul etmelidir.

| Senaryo | Kısıtlamalar | Avere vFXT kümesini oluşturmak için gereken erişim rolleri |
|----------|--------|-------|
| Kaynak Grubu Yöneticisi vFXT 'yi oluşturuyor | Sanal ağ, küme denetleyicisi ve küme düğümlerinin kaynak grubu içinde oluşturulması gerekir. | Hedef kaynak grubu kapsamındaki [Kullanıcı erişimi Yöneticisi](../role-based-access-control/built-in-roles.md#user-access-administrator) ve [katkıda bulunan](../role-based-access-control/built-in-roles.md#contributor) rolleri. |
| Mevcut, dış sanal ağı kullan | Küme denetleyicisi ve küme düğümleri vFXT 'nin kaynak grubu içinde oluşturulur, ancak farklı bir kaynak grubunda var olan bir sanal ağı kullanır. | (1) vFXT kaynak grubu kapsamındaki [Kullanıcı erişimi Yöneticisi](../role-based-access-control/built-in-roles.md#user-access-administrator) ve [katkıda bulunan](../role-based-access-control/built-in-roles.md#contributor) rolleri; ve (2) sanal [makine katılımcısı](../role-based-access-control/built-in-roles.md#virtual-machine-contributor), [Kullanıcı erişimi Yöneticisi](../role-based-access-control/built-in-roles.md#user-access-administrator)ve [avere katkıda](../role-based-access-control/built-in-roles.md#avere-contributor) bulunan rolleri sanal ağın kaynak grubu kapsamında. |
| Küme oluşturucuları için özel rol | Kaynak yerleştirme kısıtlaması yok. Bu yöntem, sahip olmayan önemli ayrıcalıklar sağlar. | Abonelik sahibi, [Bu makalede](avere-vfxt-non-owner.md)açıklandığı gibi bir Azure özel rolü oluşturur. |

## <a name="quota-for-the-vfxt-cluster"></a>VFXT kümesi için kota

Aşağıdaki Azure bileşenleri için yeterli kotayı sahip olup olmadığınızı denetleyin. Gerekirse, [bir kota artışı isteyin](../azure-portal/supportability/resource-manager-core-quotas-request.md).

> [!NOTE]
> Burada listelenen sanal makineler ve SSD bileşenleri, vFXT kümesinin kendisi içindir. İşlem grubunuz için kullanacağınız VM 'Ler ve SSD 'Ler için de kota gerekeceğini unutmayın.
>
> Kotayı, iş akışını çalıştırmayı planladığınız bölge için etkinleştirildiğinden emin olun.

|Azure bileşeni|Kota|
|----------|-----------|
|Sanal makineler|3 veya daha fazla E32s_v3 (küme düğümü başına bir tane) |
|Premium SSD depolama alanı|200 GB işletim sistemi alanına ek olarak düğüm başına 1 TB-4 TB önbellek alanı |
|Depolama hesabı (isteğe bağlı) |v2|
|Veri arka uç depolama alanı (isteğe bağlı) |Bir yeni LRS blob kapsayıcısı |
<!-- this table also appears in the overview - update it there if updating here -->

## <a name="accept-software-terms"></a>Yazılım koşullarını kabul et

> [!TIP]
> Bir abonelik sahibi avere vFXT kümesi oluşturacaktır bu adımı atlayın.

Küme oluşturma sırasında, avere vFXT yazılımının hizmet koşullarını kabul etmelisiniz. Abonelik sahibi değilseniz, abonelik sahibi bir süre önce kabul edin.

Bu adımın her abonelik için yalnızca bir kez yapılması gerekir.

Yazılım koşullarını önceden kabul etmek için:

1. Azure portal veya göz atarak bir bulut kabuğu açın <https://shell.azure.com> . Abonelik KIMLIĞINIZLE oturum açın.

   ```azurecli
    az login
    az account set --subscription <subscription ID>
   ```

1. Hizmet koşullarını kabul etmek ve Azure yazılım görüntüsü için avere vFXT için programlı erişimi etkinleştirmek üzere bu komutu verin:

   ```azurecli
   az vm image accept-terms --urn microsoft-avere:vfxt:avere-vfxt-controller:latest
   ```

## <a name="create-a-storage-service-endpoint-in-your-virtual-network-if-needed"></a>Sanal ağınızda (gerekirse) bir depolama hizmeti uç noktası oluşturun

[Hizmet uç noktası](../virtual-network/virtual-network-service-endpoints-overview.md) , Azure Blob trafiğini sanal ağ dışında yönlendirme yerine yerel olarak tutar. Arka uç veri depolama için Azure Blob kullanan Azure kümesi için avere vFXT 'nin kullanılması önerilir.

Kümeyi oluştururken yeni bir sanal ağ oluşturursanız, otomatik olarak bir uç nokta oluşturulur. Var olan bir sanal ağı sağlarsanız, küme oluşturma sırasında yeni bir BLOB depolama kapsayıcısı oluşturmak istiyorsanız Microsoft Storage hizmet uç noktası olması gerekir.<!-- if there is no endpoint in that situation, the cluster creation will fail -->

> [!TIP]
>
>* Küme oluşturmanın bir parçası olarak yeni bir sanal ağ oluşturuyorsanız bu adımı atlayın.
>* Küme oluşturma sırasında BLOB depolama alanı oluşturmadıysanız, bir uç nokta isteğe bağlıdır. Bu durumda, Azure Blob kullanmaya karar verirseniz hizmet uç noktasını daha sonra oluşturabilirsiniz.

Azure portal depolama hizmeti uç noktası oluşturun.

1. Portalda, sanal ağ listenizi açın.
1. Kümeniz için sanal ağı seçin.
1. Sol menüden **hizmet uç noktaları** ' na tıklayın.
1. Üstteki **Ekle** ' ye tıklayın.
1. Hizmeti seçin ``Microsoft.Storage`` .
1. Kümenin alt ağını seçin.
1. Alt kısımdaki **Ekle**' ye tıklayın.

   ![Hizmet uç noktası oluşturma adımları için ek açıklamaların bulunduğu ekran görüntüsünü Azure portal](media/avere-vfxt-service-endpoint.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu önkoşulları tamamladıktan sonra kümeyi oluşturabilirsiniz. Yönergeler için [vFXT kümesini dağıtma](avere-vfxt-deploy.md) makalesini okuyun.