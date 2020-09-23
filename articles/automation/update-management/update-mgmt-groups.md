---
title: Azure Otomasyonu ile dinamik grupları kullanın Güncelleştirme Yönetimi
description: Bu makalede, Azure Otomasyonu Güncelleştirme Yönetimi ile dinamik grupların nasıl kullanılacağı açıklanır.
services: automation
ms.subservice: update-management
ms.date: 07/28/2020
ms.topic: conceptual
ms.openlocfilehash: 61339a22fc3823325ff5356112c2df8a800010a0
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90985204"
---
# <a name="use-dynamic-groups-with-update-management"></a>Güncelleştirme Yönetimi ile dinamik grupları kullanma

Güncelleştirme Yönetimi, güncelleştirme dağıtımları için Azure veya Azure olmayan VM 'Lerin dinamik bir grubunu hedefetmenize olanak tanır. Dinamik bir grup kullanmak, makineleri güncelleştirmek için dağıtımınızı düzenleme zorunluluğunu ortadan önler.

> [!NOTE]
> Dinamik Gruplar klasik VM 'Ler ile çalışmaz.

Azure veya Azure dışı makineler için dinamik grupları, Azure portal **güncelleştirme yönetiminden** belirleyebilirsiniz. Bkz. [VM güncelleştirmelerini yönetme](update-mgmt-manage-updates-for-vm.md).

Dinamik bir grup, Azure Otomasyonu 'nun dağıtım zamanında değerlendirdiği bir sorgu tarafından tanımlanır. Dinamik grup sorgusu çok sayıda makine alsa bile Azure Otomasyonu aynı anda yalnızca en fazla 1000 makineyi işleyebilir. Bkz. [Azure aboneliği ve hizmet sınırları, kotalar ve kısıtlamalar](../../azure-resource-manager/management/azure-subscription-service-limits.md#update-management).

> [!NOTE]
> 1000 'den fazla makinenin güncelleştirilmesini bekleseniz, güncelleştirmeleri birden çok güncelleştirme zamanlaması arasında bölmeniz önerilir. 

## <a name="define-dynamic-groups-for-azure-machines"></a>Azure makineleri için dinamik grupları tanımlama

Azure makineleri için dinamik bir grup sorgusu tanımlarken, dinamik grubu doldurmak için aşağıdaki öğeleri kullanabilirsiniz:

* Abonelik
* Kaynak grupları
* Konumlar
* Etiketler

![Grupları seçin](./media/update-mgmt-groups/select-groups.png)

Dinamik grup sorgunuzun sonuçlarını önizlemek için **Önizleme**' ye tıklayın. Önizleme, geçerli zamanda grup üyeliğini gösterir. Örnekte, `Role` **BackEndServer**grubu için etiketi olan makineler aranıyor. Bu etiket daha fazla makineye eklendiyse, bu Etiketler bu gruba karşı gelecekteki tüm dağıtımlara eklenir.

![Önizleme grupları](./media/update-mgmt-groups/preview-groups.png)

## <a name="define-dynamic-groups-for-non-azure-machines"></a>Azure dışı makineler için dinamik grupları tanımlama

Azure dışı makineler için dinamik bir grup, bilgisayar grupları olarak da adlandırılan kaydedilmiş aramaları kullanır. Kayıtlı bir aramanın nasıl oluşturulduğunu öğrenmek için bkz. [bilgisayar grubu oluşturma](../../azure-monitor/platform/computer-groups.md#creating-a-computer-group). Kayıtlı aramanız oluşturulduktan sonra, Azure portal **güncelleştirme yönetiminde** kayıtlı aramalar listesinden seçebilirsiniz. Kayıtlı aramada bilgisayarları önizlemek için **Önizleme** ' ye tıklayın.

![Ekran görüntüsü, Azure olmayan (Önizleme) ve sağ taraftaki Önizleme bölmesi için Grup Seç sayfasını gösterir.](./media/update-mgmt-groups/select-groups-2.png)

## <a name="next-steps"></a>Sonraki adımlar

Güncelleştirme değerlendirmelerini, dağıtımları ve diğer ilgili yönetim görevlerini çözümlemek için [Azure izleyici günlüklerini sorgulayabilirsiniz](update-mgmt-query-logs.md) . Başlamanıza yardımcı olmak için önceden tanımlanmış sorgular içerir.
