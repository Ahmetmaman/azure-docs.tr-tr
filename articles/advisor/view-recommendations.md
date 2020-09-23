---
title: Size bağımsız olarak Azure Advisor önerilerini görüntüleyin
description: Paraziti azaltmak için Azure Advisor önerilerini görüntüleyin ve filtreleyin.
ms.topic: article
ms.date: 04/03/2019
ms.openlocfilehash: 43bdaba7d774bba8857a6eb3ef296d7ab8e1c264
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90986865"
---
# <a name="view-azure-advisor-recommendations-that-matter-to-you"></a>Size bağımsız olarak Azure Advisor önerilerini görüntüleyin

Azure Danışmanı, Azure dağıtımlarınızı iyileştirmenize yardımcı olacak öneriler sağlar. Danışman içinde, önerilerinizi yalnızca sizin için daraltmanıza yardımcı olan birkaç özelliğe erişebilirsiniz.

## <a name="configure-subscriptions-and-resource-groups"></a>Abonelikleri ve kaynak gruplarını yapılandırma

Danışman size ve kuruluşunuza yönelik abonelikler ve kaynak grupları seçme olanağı sağlar. Yalnızca seçtiğiniz abonelikler ve kaynak grupları için öneriler görürsünüz. Varsayılan olarak, tümü seçilidir. Yapılandırma ayarları abonelik veya kaynak grubu için geçerlidir; bu nedenle, bu aboneliğe veya kaynak grubuna erişimi olan herkese aynı ayarlar uygulanır. Yapılandırma ayarları Azure portal veya program aracılığıyla değiştirilebilir.

Azure portal değişiklik yapmak için:

1. Azure portal [Azure Advisor](https://aka.ms/azureadvisordashboard) 'ı açın.

1. Menüden **yapılandırma** ' yı seçin.

   ![Danışman yapılandırma menüsü](./media/view-recommendations/configuration.png)

1. Danışman önerilerini almak için herhangi bir aboneliğin veya kaynak grubunun **dahil etme** sütunundaki kutuyu işaretleyin. Kutu devre dışıysa, bu abonelikte veya kaynak grubunda bir yapılandırma değişikliği yapma izniniz olmayabilir. [Azure Advisor izinleri](permissions.md)hakkında daha fazla bilgi edinin.

1. Değişiklik yaptıktan sonra alt kısımdaki **Uygula** ' ya tıklayın.

## <a name="filtering-your-view-in-the-azure-portal"></a>Azure portal görünümlerinizin filtrelenmesi

Yapılandırma ayarları değiştirilene kadar etkin kalır. Tek bir görüntüleme için önerilerin görünümünü sınırlandırmak istiyorsanız, danışman panelinin en üstünde yer alarak sunulan açılan listeleri kullanabilirsiniz. Genel bakış, yüksek kullanılabilirlik, güvenlik, performans, maliyet ve tüm öneri panellerinden, görmek istediğiniz abonelikleri, kaynak türlerini ve öneri durumunu seçebilirsiniz.

   :::image type="content" source="./media/view-recommendations/filtering.png" alt-text="Filtre seçeneklerini gösteren Azure Danışmanı ekran görüntüsü.":::

## <a name="dismissing-and-postponing-recommendations"></a>Kayıp ve öneriler erteleme

Azure Danışmanı, tek bir kaynaktaki önerileri kapatmanıza veya erteetmenize olanak tanır. Bir öneriyi kapatırsanız, el ile etkinleştirmediğiniz takdirde onu bir daha görmezsiniz. Ancak, bir öneriyi erteleme, öneriyi otomatik olarak yeniden etkinleştiren süreyi belirtmenize olanak tanır. Erteleme Azure portal veya program aracılığıyla yapılabilir.

### <a name="postpone-a-single-recommendation-in-the-azure-portal"></a>Azure portal tek bir öneriyi erteleyin 

1. Azure portal [Azure Advisor](https://aka.ms/azureadvisordashboard) 'ı açın.
1. Önerilerinizi görüntülemek için bir öneri kategorisi seçin
1. Öneriler listesinden bir öneri seçin
1. Ertelemek veya kapatmak istediğiniz öneri için ertele veya Kapat ' ı seçin

     :::image type="content" source="./media/view-recommendations/postpone-dismiss.png" alt-text="Seçim sütununu gösteren ve tek bir öneri vurgulanmış olarak eylemleri ertele ve Kapat için yönetilen diskleri kullan penceresinin ekran görüntüsü.":::

### <a name="postpone-or-dismiss-a-multiple-recommendations-in-the-azure-portal"></a>Azure portal birden çok öneriyi erteleyin veya kapatın

1. Azure portal [Azure Advisor](https://aka.ms/azureadvisordashboard) 'ı açın.
1. Önerilerinizi görüntülemek için bir öneri kategorisi seçin.
1. Öneriler listesinden bir öneri seçin.
1. Öneriyi ertelemek veya kapatmak istediğiniz tüm kaynaklar için satırın solundaki onay kutusunu seçin.
1. Tablonun sol üst kısmında **ertele** veya **Kapat** ' ı seçin.

     :::image type="content" source="./media/view-recommendations/postpone-dismiss-multiple.png" alt-text="Seçim sütununu gösteren ve tablonun sol üst kısmındaki eylemleri ertele ve Kapat eylemlerini gösteren, yönetilen diskleri kullan penceresinin ekran görüntüsü.":::

> [!NOTE]
> Bir öneriyi kapatmak veya ertelemek için katkıda bulunan veya sahip izninizin olması gerekir. Azure Advisor izinleri hakkında daha fazla bilgi edinin.

> [!NOTE]
> Seçim kutuları devre dışıysa, öneriler yüklenmeye devam edebilir. Lütfen ertelemeyi veya kapatmak denemeden önce tüm önerilerin yüklenmesini bekleyin.

### <a name="reactivate-a-postponed-or-dismissed-recommendation"></a>Ertelenen veya kapatılan öneriyi yeniden etkinleştirin

Ertelenen veya kapatılan bir öneriyi etkinleştirebilirsiniz. Bu eylem Azure portal veya program aracılığıyla yapılabilir. Azure portalında:

1. Azure portal [Azure Advisor](https://aka.ms/azureadvisordashboard) 'ı açın.

1. Genel Bakış panelinde filtreyi **erteleyecek**şekilde değiştirin. Advisor daha sonra ertelenen veya kapatılan önerileri görüntüler.

    :::image type="content" source="./media/view-recommendations/activate-postponed.png" alt-text="Seçili ertelenen açılan menüyü gösteren Azure Advisor penceresinin ekran görüntüsü.":::

1. **Ertelenmiş** ve **Kapatılan** önerileri görmek için bir kategori seçin.

1. Öneriler listesinden bir öneri seçin. Bu, bu öneriye ilişkin Ertelenen veya kapatılan kaynakları göstermek için zaten **ertelenen & kapatıldı** sekmesi seçili olan önerileri açar.

1. Satırın sonundaki **Etkinleştir** ' e tıklayın. Tıklandığında öneri bu kaynak için etkin olur ve bu nedenle bu tablodan kaldırılır. Öneri artık **etkin** sekmesinde görünür.
 
     :::image type="content" source="./media/view-recommendations/activate-postponed-2.png" alt-text="Etkinleştir eylemi vurgulanmış şekilde ertelenen & kapatıldı sekmesini gösteren geçici silme penceresinin ekran görüntüsü.":::

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure Advisor 'da size yönelik önerileri nasıl görüntüleyebileceğiniz açıklanmaktadır. Advisor hakkında daha fazla bilgi edinmek için bkz.: 

- [Azure Danışmanı nedir?](advisor-overview.md)
- [Advisor 'ı kullanmaya başlama](advisor-get-started.md)
- [Azure Danışmanı izinleri](permissions.md)



