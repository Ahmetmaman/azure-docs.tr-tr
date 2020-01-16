---
title: Azure'da Cloudyn’e genel bakış | Microsoft Docs
description: Cloudyn, Azure ve diğer bulut kaynaklarını kullanmanıza yardımcı olan çoklu bulut maliyet yönetimi çözümüdür.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 05/14/2019
ms.topic: overview
ms.service: cost-management-billing
manager: benshy
ms.custom: seodec18
ms.openlocfilehash: d4a31518dcd6fb5722fc19617b153bafb8a20f68
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2020
ms.locfileid: "75987659"
---
# <a name="what-is-the-cloudyn-service"></a>Cloudyn hizmeti nedir?

Microsoft’un bir bağlı şirketi olan Cloudyn, Azure kaynaklarınızın yanı sıra AWS ve Google dahil diğer bulut sağlayıcıları için bulut kullanımınızı ve harcamalarınızı izlemenize imkan tanır. Anlaşılması kolay pano raporları, maliyet ayırma ve ücret hesaplama/yansıtma konusunda yardımcı olur. Cloudyn, yönetip ayarlamak isteyeceğiniz az kullanılan kaynakları belirleyerek bulut harcamalarınızı en iyi duruma getirmenize yardımcı olur.

Tanıtım videosunu izlemek için bkz. [Azure Cloudyn’e Giriş](https://azure.microsoft.com/resources/videos/azure-cost-management-overview-and-demo).

Azure Maliyet Yönetimi, Cloudyn'e benzer işlevler sunar. Azure Maliyet Yönetimi, yerel Azure maliyet yönetimi çözümüdür. Maliyet analizi yapmanıza, bütçe oluşturup yönetmenize, verileri dışarı aktarmanıza ve tasarruf önerilerini gözden geçirip gerekli eylemleri gerçekleştirmenize yardımcı olur. Daha fazla bilgi için bkz. [Azure Maliyet Yönetimi](../cost-management-billing-overview.md).

Azure maliyet yönetimi [ve Cloudyn videosunu](https://www.youtube.com/watch?v=PmwFWwSluh8) , iş gereksinimlerinize göre Azure Cost Management veya Cloudyn ' i kullanmanız gereken önerileri görmek için izleyin.

>[!VIDEO https://www.youtube.com/embed/PmwFWwSluh8]

## <a name="cloudyn-features-moving-to-azure-cost-management"></a>Cloudyn özellikleri Azure maliyet yönetimine taşınıyor

Microsoft, Cloudyn 'i almış ve kendi maliyet yönetimi özelliklerini Cloudyn portalından yerel olarak Azure 'a geçirmektedir. Yeni özellikleri kullanmak için Azure portal oturum açın ve Azure hizmetleri listesinde [maliyet yönetimi ve faturalandırma](https://ms.portal.azure.com/#blade/Microsoft_Azure_CostManagement/Menu/overview) ' a gidin. Cloudyn ile karşılaştırıldığında, yerel deneyim yaklaşık sekiz saat boyunca gelişmiş performans ve daha düşük veri gecikme süresi sağlar.

Azure maliyet yönetimi için Kurumsal Anlaşma, Kullandıkça öde ve MSDN teklif kategorilerinin temel özellik geçişi tamamlanmıştır. CSP abonelikleri, Azure maliyet yönetimi 'ne geçiş sürecinde.

Henüz geçirilmeyen bir teklif kategorili varsa Cloudyn portalını kullanmaya devam etmelisiniz. Diğer herkes Azure maliyet yönetimi 'ni kullanabilir.

| Microsoft Azure teklifleri ve özellikleri | Önerilen maliyet yönetimi hizmeti |
| --- | --- |
| Azure Kurumsal Anlaşma | [Azure Maliyet Yönetimi](https://ms.portal.azure.com/#blade/Microsoft_Azure_CostManagement/Menu/overview) |
| Azure Web Direct (PAYG/MSDN) | [Azure Maliyet Yönetimi](https://ms.portal.azure.com/#blade/Microsoft_Azure_CostManagement/Menu/overview) |
| Azure Devlet Kurumları | [Azure Maliyet Yönetimi](https://ms.portal.azure.com/#blade/Microsoft_Azure_CostManagement/Menu/overview) |
| Azure CSP | [Cloudyn](https://azure.cloudyn.com) |
| AWS için platformlar arası maliyet analizi desteği (önizlemede) | [Azure Maliyet Yönetimi](https://ms.portal.azure.com/#blade/Microsoft_Azure_CostManagement/Menu/overview) |
| AWS önerileri | [Cloudyn](https://azure.cloudyn.com) |

Aşağıdaki özelliklerden bazıları Cloudyn 'de mevcuttur, ancak bunların hepsi Azure maliyet yönetimi 'nde kullanıma sunulmuştur.

- API'ler
- Azure işlem önerileri
- Azure ayırma önerileri
- Bütçeler
- Maliyet analizi
- Azure depolama hesabına veri aktarma
- Düşük gecikme süresi
- Power BI içerik paketi ve bağlayıcı
- Kaynak etiketi desteği

## <a name="monitor-usage-and-spending"></a>Kullanımı ve harcamayı izleyin

Kuruluşlar zaman içinde kullandığı kaynaklar için ödeme yaptığından bulut altyapıları için kullanımınızı ve harcamanızı izlemeniz kritik öneme sahiptir. Kullanım sözleşme eşiklerini aşarsa kısa bir sürede beklenmedik fazla kullanım ücretleriyle karşılaşılabilir. Birkaç önemli etken, geçici izleme yapmayı zorlaştırır. İlk olarak, ortalama kullanıma göre maliyet tahmini yapılırken belirli bir fatura dönemi boyunca tüketiminizin tutarlı kalacağı varsayılır. İkinci olarak, maliyetler bütçenize yaklaştığında veya bütçenizi aştığında harcamanızı proaktif olarak ayarlayabilmeniz için bildirim almanız önemlidir. Ayrıca, bulut hizmeti sağlayıcıları eşikle karşılaştırmalı maliyet tahmini veya dönemler arası karşılaştırma raporları sağlamıyor olabilir.

Raporlar, harcamayı izleyerek bulut kullanımını, maliyetleri ve eğilimleri analiz etmenize ve izlemenize yardımcı olur. Zaman İçinde raporlarını kullanarak normal eğilimlerden farklı olan anormal durumları algılayabilirsiniz. İyileştirme raporlarında bulut dağıtımınızdaki verimsizlikler görülebilir. Ayrıca, maliyet analizi raporlarında verimsizlikler olduğunu fark edebilirsiniz.

## <a name="manage-costs"></a>Maliyetleri yönetme

Zaman içinde kullanımı ve maliyetleri analiz ederek eğilimleri belirlemeniz durumunda geçmiş veriler maliyetleri yönetmenize yardımcı olabilir. Daha sonra bu eğilimler gelecekte yapılacak harcamaların tahmin edilmesinde kullanılabilir. Cloudyn, kullanışlı tahmini maliyet raporları da içerir.

Maliyet ayrımı, etiketleme ilkenize göre maliyetlerinizi analiz ederek maliyetleri yönetir. Maliyet ayırmayı daha ayrıntılı hale getirmek için özel hesaplarınızda, kaynaklarınızda ve varlıklarınızda etiketleri kullanabilirsiniz. Kategori Yöneticisi, etiketlerinizi düzenleyerek ek yönetim sağlanmasına yardımcı olur. Ayrıca, tüketim davranışlarını etkilemek veya kiracı müşterilere ücret uygulamak amacıyla ücret hesaplama/yansıtma için maliyet ayırmayı kullanarak kaynak kullanımını ve bununla ilişkili maliyetleri gösterirsiniz.

Erişim denetimi, kullanıcıların ve takımların yalnızca gereksinim duydukları maliyet yönetimi verilerine erişebilmesini sağlayarak maliyetlerin yönetilmesine yardımcı olur. Erişim atamak için varlık yapısı, kullanıcı yönetimi ve alıcı listeleri ile zamanlanmış raporları kullanabilirsiniz.

Uyarılar, olağan dışı veya fazla harcama gerçekleştiğinde size bildirimde bulunarak maliyetlerin yönetilmesine yardımcı olur. Ayrıca, uyarılar anormal harcama durumları ve fazla harcama riskleri konusunda başka ilgili kişilere de otomatik olarak bildirimde bulunabilir. Bütçe ve maliyet eşikleri konusunda uyarıları destekleyen çeşitli raporlar vardır. Bununla birlikte, CSP iş ortağı hesapları veya abonelikleri için şu an uyarılar desteklenmemektedir.

## <a name="improve-efficiency"></a>Verimliliği artırın

Cloudyn ile en uygun VM kullanımını belirleyebilir ve boş VM’leri tespit edebilir ya da boş VM’leri ve bağlı olmayan diskleri kaldırabilirsiniz. Boyutlandırma Optimizasyonu ve Verimsizlik raporlarındaki bilgileri kullanarak VM’lerin boyutunu düşürme veya boş VM’leri kaldırma planı oluşturabilirsiniz. Bununla birlikte, şu an CSP iş ortağı hesapları ve aboneliklerinde iyileştirme raporları desteklenmemektedir.

AWS Ayrılmış Örnekleri sağladıysanız satın alma önerilerini görüntüleme, kullanılmayan ayırmaları değiştirme ve sağlama planlama imkanı sunan Optimizasyon raporlarıyla ayrılmış örnek kullanımınızı geliştirebilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar

Artık Cloudyn ile tanıştığınıza göre bir sonraki adım bulut ortamınızı kaydetmek ve verilerinizi keşfetmeye başlamaktır.

- [Tek bir Azure aboneliğini kaydetme](quick-register-azure-sub.md)
