---
title: Azure Maliyet Yönetimi’ne Genel Bakış | Microsoft Docs
description: Azure Maliyet Yönetimi, Azure harcamalarınızı izleyip kontrol altına almanıza ve kaynak kullanımını iyileştirmenize yardımcı olan bir maliyet yönetimi çözümüdür.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 12/05/2018
ms.topic: overview
ms.service: cost-management
manager: benshy
ms.custom: ''
ms.openlocfilehash: a90ef531cedb5e4c32a8f0af8b6cca86a93fb39a
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2018
ms.locfileid: "52997117"
---
# <a name="what-is-azure-cost-management"></a>Azure Maliyet Yönetimi nedir?

Maliyet yönetimi, işlerinizle ilişkili maliyetleri etkili bir şekilde planladığınız ve denetlediğiniz bir işlemdir. Maliyet yönetimi görevleri normalde finans, yönetim ve uygulama takımları tarafından gerçekleştirilir. Azure Maliyet Yönetimi, Microsoft Kurumsal Anlaşmaları (EA) olan kuruluşların planlama yaparken maliyeti dikkate almasına yardımcı olur. Ayrıca, maliyetleri etkili bir şekilde analiz etmeye ve bulut harcamalarını iyileştirmek için gerçekleştirilecek eylemlere de katkı sağlar. Kuruluş olarak maliyet yönetimine nasıl yaklaşmak gerektiği hakkında daha fazla bilgi edinmek için, [Azure Maliyet Yönetimi en iyi yöntemleri](cost-mgt-best-practices.md) makalesini gözden geçirin.

Faturalandırma, maliyet yönetimiyle ilgili olsa da aynı şey değildir. Faturalandırma, müşterilere mal veya hizmetler karşılığı fatura hazırlama ve ticari ilişkileri yönetme işlemidir.  Faturalandırma görevleri genellikle tedarik ve finans takımları tarafından yürütülür.

Maliyet yönetimi, gelişmiş analizle kurumsal maliyet ve kullanım düzenlerini gösterir. Maliyet Yönetimi'ndeki raporlar Azure maliyetini, kullanımını, ayrılmış örneğini ve Azure Hibrit Avantajı kullanımını gösterir. Raporlar hep birlikte, kullanımla ilişkili iç ve dış maliyetlerinizi ve Azure Market ücretlerinizi ortaya koyar. Rezervasyon satın almaları, destek ve vergiler gibi diğer ücretler henüz raporlarda gösterilmemektedir. Raporlar, harcamalarınızı ve kaynak kullanımınızı anlamanıza, ayrıca harcama anomalilerini bulmanıza yardımcı olur. Tahmine dayalı analiz de sağlanır. Maliyet Yönetimi harcamalarınızın nasıl düzenlendiğini ve maliyetleri nasıl düşürebileceğinizi net bir şekilde göstermek için Azure yönetim gruplarını, bütçelerini ve önerilerini kullanır.

Azure portalı veya çeşitli API'leri kullanarak dışarı aktarma otomasyonu yapabilir ve bu yolla maliyet verilerini dış sistemler ve süreçlerle tümleştirebilirsiniz. Otomatik faturalandırma verilerini dışarı aktarma özelliği ve zamanlanmış raporlar da sağlanır.

## <a name="plan-and-control-expenses"></a>Giderleri planlama ve denetleme

Maliyet Yönetimi maliyetlerinizi planlamanıza ve denetlemenize şu yollardan yardımcı olur: Maliyet analizi, bütçeler, öneriler ve maliyet yönetimi verilerini dışarı aktarma.

Kurumsal maliyetlerinizi incelemek ve analiz etmek için maliyet analizini kullanırsınız. Maliyetlerin nerede tahakkuk ettiğini anlamak ve harcama eğilimlerini belirlemek için kuruluşa göre toplu maliyetleri görüntüleyebilirsiniz. Bütçeye göre aylık, üç aylık, hatta yıllık maliyet eğilimlerini tahmin etmek için, zaman içinde tahakkuk eden maliyetleri de görebilirsiniz.

Bütçeler kuruluşunuzda finansal sorumluluğu planlamanıza ve buna uymanıza yardımcı olur. Maliyet eşiklerinin veya sınırlarının aşılmasını önlemeye yardım ederler. Bütçeler, maliyetleri proaktif olarak yönetmek için diğer kişileri harcamalarıyla ilgili bilgilendirmenize de katkıda bulunabilir. Bunlardan yararlanarak, zaman içinde harcamaların nasıl bir ilerleme gösterdiğini görebilirsiniz.

Öneriler, boşta olan ve az kullanılan kaynakları belirleyerek verimliliği nasıl iyileştirebileceğinizi ve geliştirebileceğinizi gösterir. Öte yandan daha ucuz kaynak seçeneklerini de gösterebilirler. Önerilere uyarak önlem aldığınızda, kaynakları kullanım yönteminizi değiştirerek tasarruf edebilirsiniz. Önlem almak için, önce maliyet iyileştirme önerilerini görüntüleyip olası kullanım verimsizliklerini görürsünüz. Ardından, öneriye göre harekete geçip Azure kaynak kullanımınızı daha uygun maliyetli bir seçeneği kullanacak şekilde değiştirirsiniz. Sonra da yaptığınız değişikliğin başarısından emin olmak için işleminizi doğrularsınız.

Maliyet yönetimi verilerine erişmek veya bu verileri incelemek için dış sistemleri kullanıyorsanız, verileri Azure'dan kolayca dışarı aktarabilirsiniz. Ayrıca CSV biçiminde günlük zamanlanmış dışarı aktarmalar ayarlayabilir ve veri dosyalarını Azure depolama alanında depolayabilirsiniz. Sonra bu verileri dış sisteminizden erişebilirsiniz.

## <a name="consider-cloudyn"></a>Cloudyn'i göz önünde bulundurun

[Cloudyn](overview.md), Maliyet Yönetimi ile ilgili bir Azure hizmetidir. Cloudyn ile, Azure kaynaklarınız için bulut kullanımını ve harcamalarını izleyebilirsiniz. Ayrıca AWS ve Google gibi diğer bulut sağlayıcılarını da destekler. Anlaşılması kolay pano raporları, maliyet ayırma ve ücret hesaplama/yansıtma konusunda yardımcı olur. Şu anda, Maliyet Yönetimi'nin geri gösterme/geri ödeme ve diğer bulut hizmeti sağlayıcıları için desteği yoktur. Öte yandan, Cloudyn bunları _destekleyen_ bir seçenektir. Şu anda Maliyet Yönetimi yalnızca Azure EA hesaplarını destekler. Bireysel veya Kullandıkça Öde hesaplarını ya da Microsoft Bulut Hizmeti Sağlayıcısı hesaplarını desteklemez. Oysa Cloudyn bunları destekler. Söz konusu hesaplardan birini kullanıyorsanız, maliyetlerinizin yönetimine yardımcı olması için Cloudyn'den yararlanabilirsiniz.

## <a name="additional-azure-tools"></a>Ek Azure araçları

Azure'un, Azure Maliyet Yönetimi özellik kümesi kapsamında yer almayan başka araçları da vardır. Bununla birlikte, bu araçlar maliyet yönetimi işleminde önemli bir rol oynar. Söz konusu araçlar hakkında daha fazla bilgi edinmek için aşağıdaki bağlantılara bakın.

- [Azure Fiyatlandırma Hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/) - Önceden bulut maliyetlerinizi tahmin etmek için bu aracı kullanın.
- [Azure Geçişi](../migrate/migrate-overview.md) - Azure yedek çözümünden neler gerektiği hakkında içgörüler elde etmek için geçerli veri merkezi yükünüzü değerlendirin.
- [Azure Danışmanı](../advisor/advisor-overview.md) - Kullanılmayan sanal makineleri belirleyin ve Azure ayrılmış örnek satın almalarıyla ilgili öneriler alın.
- [Azure Hibrit Avantajı](https://azure.microsoft.com/pricing/hybrid-benefit/) - Tasarruf etmek için geçerli şirket içi Windows Server veya SQL Server lisanslarınızı Azure'da sanal makineler için kullanın.


## <a name="next-steps"></a>Sonraki adımlar

Artık Maliyet Yönetimi ile tanıştığınıza göre, bir sonraki adımda hizmeti kullanmaya başlayacaksınız.

- Azure Maliyet Yönetimi'ni kullanarak [maliyetleri analiz etmeye](quick-acm-cost-analysis.md) başlayın.
- Ayrıca, [Azure Maliyet Yönetimi en iyi yöntemlerini](cost-mgt-best-practices.md) de okuyabilirsiniz.
