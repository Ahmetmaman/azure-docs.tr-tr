---
title: Azure DevTest Labs maliyeti ve sahipliği yönetme
description: Bu makalede, ortamınızdaki maliyeti iyileştirmenize ve sahipliğini ortamınıza göre hizalamanıza yardımcı olacak bilgiler sağlanmaktadır.
ms.topic: article
ms.date: 06/26/2020
ms.reviewer: christianreddington,anthdela,juselph
ms.openlocfilehash: dfac69a055c9b0c75032622caf7fb8502fad3406
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "92328275"
---
# <a name="governance-of-azure-devtest-labs-infrastructure---manage-cost-and-ownership"></a>Azure DevTest Labs altyapı yönetimi-maliyeti ve sahipliği yönetme
Geliştirme ve test ortamlarınızı oluşturmayı düşünmenizde, maliyet ve sahiplik birincil kaygılardır. Bu bölümde, ücretlerinizi iyileştirmek ve ortamınızda sahipliği hizalamak için size yardımcı olacak bilgiler bulabilirsiniz.

## <a name="optimize-for-cost"></a>Maliyet için iyileştirin

### <a name="question"></a>Soru
DevTest Labs ortamım dahilinde maliyeti nasıl iyileştirebilirim?

### <a name="answer"></a>Yanıt
Maliyet için iyileştirmenize yardımcı olan DevTest Labs 'in birçok yerleşik özelliği vardır. Kullanıcılarınızın etkinliklerini sınırlamak için [maliyet yönetimi, eşikler](devtest-lab-configure-cost-management.md) [ve ilkeler](devtest-lab-set-lab-policy.md) makalelerine bakın. 

Geliştirme ve test iş yükleri için DevTest Labs 'i kullanmaya çalıştığınız için, Kurumsal Anlaşma bir parçası olarak [Kurumsal Geliştirme ve test abonelik avantajını](https://azure.microsoft.com/offers/ms-azr-0148p/)kullanmayı düşünebilirsiniz. Alternatif olarak, Kullandıkça Öde müşterisiyseniz kullandıkça [öde DevTest teklifini](https://azure.microsoft.com/offers/ms-azr-0023p/)göz önünde bulundurmanız gerekebilir.

Bu yaklaşım size birçok avantaj sağlar:

- Windows sanal makineler, bulut Hizmetleri, HDInsight, App Service ve Logic Apps özel geliştirme ve test ücretleri
- Diğer Azure hizmetlerinde harika Kurumsal Anlaşma (EA) ücretleri
- Windows 8.1 ve Windows 10 dahil olmak üzere galerideki özel geliştirme ve test görüntülerine erişim
 
Yalnızca etkin Visual Studio aboneleri (Standart abonelikler, yıllık bulut abonelikleri ve aylık bulut abonelikleri), kurumsal bir geliştirme/test aboneliği içinde çalışan Azure kaynaklarını kullanabilir. Ancak, son kullanıcılar geri bildirim sağlamak veya onay testleri gerçekleştirmek amacıyla uygulamaya erişebilir. Bu aboneliğin içindeki kaynakların kullanımı, uygulama geliştirme ve test etme ile kısıtlıdır ve hiçbir çalışma süresi garantisi sunulmaz.

DevTest teklifini kullanmaya karar verirseniz, bu avantajın yalnızca geliştirme ve uygulamalarınızın test edilmesine yönelik olduğunu unutmayın. Abonelik içindeki kullanım, Azure DevOps ve HockeyApp kullanımı dışında mali olarak desteklenen bir SLA 'yı taşımaz.

## <a name="define-role-based-access-across-your-organization"></a>Kuruluşunuz genelinde rol tabanlı erişim tanımlayın
### <a name="question"></a>Soru
Geliştirici/test işlerini yapabilirken yönetebilmemesini sağlamak için DevTest Labs Ortamlarım için Azure rol tabanlı erişim denetimi Nasıl yaparım? tanımlayın? 

### <a name="answer"></a>Yanıt
Geniş bir düzen mevcuttur, ancak ayrıntı kuruluşunuza bağlıdır.

Merkezi olarak yalnızca gerekli olan nedir ve proje ve uygulama ekiplerinin gerekli denetim düzeyine sahip olmasını sağlar. Genellikle, bu, aboneliğin sahibi olduğu ve ağ yapılandırması gibi çekirdek BT işlevlerini işleyeceği anlamına gelir. Bir abonelik için **sahip** kümesi küçük olmalıdır. Bu sahipler, bir gereksinim olduğunda ek sahipleri aday yapabilir veya "genel IP yok" gibi abonelik düzeyinde ilkeler uygulayabilir.

Katman1 veya katman 2 desteği gibi bir abonelik genelinde erişim gerektiren kullanıcıların bir alt kümesi olabilir. Bu durumda, bu kullanıcılara, kaynakları yönetebilmeleri, ancak kullanıcı erişimi sağlamamasını veya ilke ayarlamanıza olanak tanımak için **katkıda** bulunan erişimini sağlamanızı öneririz.

DevTest Labs kaynağı, proje/uygulama takımına yakın olan sahiplere ait olmalıdır. Bunun nedeni, makineler ve gerekli yazılımlar açısından gereksinimlerini anladıkları için. Çoğu kuruluşta, bu DevTest Labs kaynağının sahibi genellikle proje/geliştirme lideridir. Bu sahip, Laboratuvar ortamındaki kullanıcıları ve ilkeleri yönetebilir ve DevTest Labs ortamındaki tüm VM 'Leri yönetebilir.

Proje/uygulama ekibi üyeleri DevTest Labs kullanıcıları rolüne eklenmelidir. Bu kullanıcılar sanal makineler oluşturabilir (Laboratuvar ve abonelik düzeyindeki ilkelerle birlikte). Bunlar, kendi sanal makinelerini de yönetebilir. Diğer kullanıcılara ait sanal makineleri yönetemez.

Daha fazla bilgi için bkz. [Azure Kurumsal yapı iskelesi – seçkin abonelik idare](/azure/architecture/cloud-adoption/appendix/azure-scaffold) belgeleri.


## <a name="next-steps"></a>Sonraki adımlar
Bkz. [Şirket ilkesi ve uyumluluğu](devtest-lab-guidance-governance-policy-compliance.md).
