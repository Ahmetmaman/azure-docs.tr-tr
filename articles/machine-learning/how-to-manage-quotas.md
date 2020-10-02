---
title: Kaynakları & kotaları yönetme
titleSuffix: Azure Machine Learning
description: Azure Machine Learning kaynaklar üzerinde kotalar ve daha fazla kota isteme hakkında bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.reviewer: jmartens
author: nishankgu
ms.author: nigup
ms.date: 09/30/2020
ms.topic: conceptual
ms.custom: troubleshooting,contperfq4
ms.openlocfilehash: 4e5634633aacb56e7c7744b3afdb3aec0fe2b70f
ms.sourcegitcommit: d479ad7ae4b6c2c416049cb0e0221ce15470acf6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/01/2020
ms.locfileid: "91629599"
---
# <a name="manage--increase-quotas-for-resources-with-azure-machine-learning"></a>Azure Machine Learning sahip kaynaklara yönelik kotaları yönetme & artırma


Bu makalede, [Azure Machine Learning](overview-what-is-azure-ml.md) aboneliğiniz ve yönetebileceğiniz kotalar için Azure kaynakları üzerinde önceden yapılandırılmış sınırlar hakkında bilgi edineceksiniz. Bu sınırlar, sahtekarlık nedeniyle yapılan bütçeyi önlemek ve Azure kapasite kısıtlamalarını kabul etmek için yerinde konur. 

Diğer Azure hizmetlerinde olduğu gibi, Azure Machine Learning ilişkili belirli kaynaklarda sınırlamalar vardır. Bu sınırlar, model eğitimi veya çıkarım/Puanlama için kullanılan gerçek temeldeki işlem üzerinde sınırlara kadar [çalışma alanı](concept-workspace.md) sayısı üst sınırından aralığıdır. 

Azure Machine Learning kaynaklarınızı üretim iş yükleri için tasarlayıp ölçeklendirerek, bu sınırları göz önünde bulundurun. Örneğin, kümeniz hedef düğüm sayısına ulaşmazsa, aboneliğiniz için bir Azure Machine Learning Işlem çekirdeği sınırına ulaşmış olabilirsiniz. Sınırı veya kotayı varsayılan sınırın üzerinde yükseltmek istiyorsanız, ücretsiz bir çevrimiçi müşteri destek isteği açın. Limitler, Azure kapasite kısıtlamaları nedeniyle aşağıdaki tablolarda gösterilen en büyük sınır değerinin üzerinde yer alınamıyor. En fazla limit sütunu yoksa, kaynak ayarlanabilir sınırlara sahip olmaz.


Kotaları yönetme ile birlikte, [Azure Machine Learning için maliyetleri nasıl planlayacağınızı &](concept-plan-manage-cost.md)de öğrenebilirsiniz.

## <a name="special-considerations"></a>Özel Konular

+ Kota, kapasite garantisi değil, kredi limiti olur. Büyük ölçekli kapasite gereksinimleriniz varsa Azure desteği 'ne başvurun. Ayrıca, [kotalarınızı artırabilirsiniz](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-quota-errors).

+ Kotayı Azure Machine Learning dahil olmak üzere aboneliklerinizde bulunan tüm hizmetler arasında paylaşılır. Tek özel durum, çekirdek işlem kotasından ayrı bir kota içeren Azure Machine Learning işlem amaçlıdır. Kapasite ihtiyaçlarınızı değerlendirirken tüm hizmetler genelinde kota kullanımını hesapladığınızdan emin olun.

+ Varsayılan sınırlar, ücretsiz deneme, Kullandıkça öde ve dv2, F, G gibi bir VM Serisi gibi teklif kategorisi türüne göre farklılık gösterir.

## <a name="default-resource-quotas"></a>Varsayılan kaynak kotaları

Azure aboneliğinizdeki çeşitli kaynak türlerine göre kota sınırlarının bir dökümü aşağıda verilmiştir.

> [!IMPORTANT]
> Sınırlar değişikliğe tabidir. En son, her zaman Azure için hizmet düzeyi kota [belgesinde](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits/) bulunabilir.

### <a name="virtual-machines"></a>Sanal makineler
Her Azure aboneliği için, hizmetinizdeki veya tek başına sanal makine sayısında bir sınır vardır. Sanal makine çekirdekleri, her ikisi de ayrı olarak zorlanan bölgesel toplam sınıra ve her ikisi de her ikisi de uygulanan boyut serisi (dv2, F, vb.) sınırına sahiptir. Örneğin, ABD Doğu toplam VM çekirdek limiti 30, A serisi çekirdek limiti 30 ve D serisi çekirdek limiti 30 olan bir abonelik düşünün. Bu aboneliğin 30 adet A1 sanal makinesi veya 30 adet D1 sanal makinesi ya da ikisinin toplamda 30 çekirdeği geçmeyecek bir birleşimini (örneğin, 10 adet A1 sanal makinesi ve 20 adet D1 sanal makinesi) dağıtmasına izin verilir.

[!INCLUDE [azure-subscription-limits-azure-resource-manager](../../includes/azure-subscription-limits-azure-resource-manager.md)]

### <a name="azure-machine-learning-compute"></a>Azure Machine Learning İşlemi
[Azure Machine Learning işlem](concept-compute-target.md#azure-machine-learning-compute-managed)için, hem çekirdek sayısı hem de bir abonelikte bölge başına izin verilen benzersiz işlem kaynakları sayısı için varsayılan bir kota sınırı vardır. Bu kota, yukarıdaki VM çekirdeği kotasından ayrıdır ve AmlCompute, kaynakları barındırılan bir modelde, kaynak dağıtan bir yönetilen hizmet olduğundan, çekirdek sınırları iki kaynak türü arasında paylaşılmaz.

Kullanılabilir kaynaklar:
+ Bölge başına adanmış çekirdekler, EA ve CSP teklif türleri için daha yüksek varsayılan değerlere sahip abonelik teklif türüne bağlı olarak varsayılan 24-300 sınırına sahiptir.  Abonelik başına adanmış çekirdek sayısı artırılabilir ve her bir VM ailesi için farklı olabilir. NCv2, NCv3 veya ND serisi gibi bazı özelleştirilmiş sanal makine aileleri, varsayılan sıfır çekirdekilerle başlar. Artış seçeneklerini tartışmak üzere bir kota isteği oluşturarak Azure desteği 'ne başvurun.

+ Bölge başına düşük öncelikli çekirdekler, EA ve CSP teklif türleri için daha yüksek varsayılan değerlere sahip abonelik teklifi türüne bağlı olarak varsayılan 100-3000 sınırına sahiptir. Abonelik başına düşük öncelikli çekirdek sayısı artırılabilir ve VM aileleri arasında tek bir değer olabilir. Artış seçeneklerini tartışmak için Azure desteğine başvurun.

+ Bölge başına kümelerin varsayılan 200 limiti vardır. Bunlar, bir eğitim kümesi ve bir işlem örneği (Kota amaçları için tek düğümlü bir küme olarak kabul edilir) arasında paylaşılır. Bu sınırın ötesinde bir artış istemek istiyorsanız Azure desteği 'ne başvurun.

+ Dönüştürüldükten sonra aşılmayacak başka kesin sınırlar vardır.

| **Kaynak** | **Üst sınır** |
| --- | --- |
| Kaynak grubu başına en fazla çalışma alanı | 800 |
| Tek bir Azure Machine Learning Işlem (AmlCompute) kaynağındaki en fazla düğüm | 100 düğümleri |
| Düğüm başına maksimum GPU MPı işlemi | 1-4 |
| Düğüm başına maksimum GPU çalışanı | 1-4 |
| En yüksek iş ömrü | 21 gün<sup>1</sup> |
| Düşük öncelikli bir düğümdeki maksimum iş ömrü | 7 gün<sup>2</sup> |
| Düğüm başına en fazla parametre sunucusu | 1 |

<sup>1</sup> en fazla yaşam süresi, bir çalıştırmanın başlayacağı ve bittiği zaman için geçen süreyi ifade eder. Tamamlanan çalıştırmalar süresiz olarak devam ederse; Maksimum yaşam süresi içinde tamamlanmayan çalışma verilerine erişilemiyor.
<sup>2</sup> düşük öncelikli bir düğümdeki işler bir kapasite kısıtlaması olduğunda bu şekilde geçersiz hale gelebilir. İşinize işaret eden bir işaret uygulamanızı uygulamanızı öneririz.

### <a name="azure-machine-learning-pipelines"></a>Azure Machine Learning işlem hatları
[Azure Machine Learning Işlem hatları](concept-ml-pipelines.md)için, bir işlem hattındaki adım sayısı ve bir abonelikte bölge başına yayınlanan işlem hattı sayısı için bir kota sınırı vardır.
- İşlem hattında izin verilen en fazla adım sayısı 30.000 ' dir
- Her ay abonelik başına yayınlanan işlem hattı tarafından tetiklenen en fazla zamanlama tabanlı çalıştırma ve BLOB çeklerinin toplamı 100.000 ' dir.

### <a name="container-instances"></a>Kapsayıcı örnekleri

Ayrıca, belirli bir süre içinde (kapsamdaki saatlik) veya tüm aboneliğiniz üzerinde dönebileceğinizi kapsayıcı örnekleri sayısında bir sınır vardır.
Sınırlar için bkz. [Container Instances sınırları](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#container-instances-limits).

### <a name="storage"></a>Depolama
Belirli bir abonelikte her bölge için depolama hesabı sayısı sınırı vardır. Varsayılan sınır 250 ' dir ve hem standart hem de Premium Depolama hesaplarını içerir. Belirli bir bölgede 250 'den fazla depolama hesabına ihtiyacınız varsa, [Azure desteği](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest/)aracılığıyla bir istek yapın. Azure depolama ekibi, iş kasalarınızı inceleyerek, belirli bir bölge için 250 depolama hesabını onaylayabilir.


## <a name="workspace-level-quota"></a>Çalışma alanı düzeyi kotası

Çeşitli [çalışma alanları](concept-workspace.md)arasında Azure Machine Learning işlem hedefi (Amlcompute) için kaynak ayırmaları daha iyi yönetmek üzere, abonelik düzeyi KOTALARıNı (VM ailesi) dağıtmanıza ve çalışma alanı düzeyinde yapılandırmanıza olanak tanıyan bir özellik sunuyoruz. Varsayılan davranış, tüm çalışma alanlarının herhangi bir VM ailesi için abonelik düzeyi kotasıyla aynı kotanın aynısına sahip olması olabilir. Ancak, çalışma alanlarının sayısı arttıkça ve değişen öncelikteki iş yükleri aynı kaynakları paylaşmaya başladıkça, kullanıcılar kapasiteyi daha iyi paylaşmak ve kaynak çekişme sorunlarından kaçınmak için bir yol ister. Azure Machine Learning, kullanıcıların her çalışma alanında belirli bir VM ailesi için maksimum kota ayarlaması sunarak yönetilen işlem teklifiyle birlikte bir çözüm sağlar. Bu, kapasitenizi çalışma alanları arasında dağıtmaya benzer ve kullanıcılar, en yüksek kullanım için de fazla ayırmayı tercih edebilir. 

Çalışma alanı düzeyinde kotalar ayarlamak için aboneliğinizdeki herhangi bir çalışma alanına gidin ve sol bölmedeki **kullanımlar + kotalar** seçeneğine tıklayın. Daha sonra kotaları **Yapılandır** sekmesini seçerek kotaları görüntüleyin, HERHANGI bir VM ailesini genişletin ve bu VM ailesi altında listelenen herhangi bir çalışma alanında bir kota sınırı ayarlayın. Negatif bir değer veya abonelik düzeyi kotasından daha yüksek bir değer ayarlayamamanız gerektiğini unutmayın. Ayrıca, gözlemleyeceksiniz, varsayılan olarak tüm çalışma alanları, ayrılan kotanın tam kullanımına izin vermek için tüm abonelik kotasının tamamına atanır.

[![Azure Machine Learning çalışma alanı düzeyi kotası](./media/how-to-manage-quotas/azure-machine-learning-workspace-quota.png)](./media/how-to-manage-quotas/azure-machine-learning-workspace-quota.png)


> [!NOTE]
> Çalışma alanı düzeyinde kota ayarlamak için abonelik düzeyi izinlerine sahip olmanız gerekir. Bu, tek bir çalışma alanı sahibinin kotalarını düzenleyememesi veya artırmaması ve başka bir çalışma alanı için ayrılan kaynakları encroaching başlatması için zorlanır. Bu nedenle, abonelik Yöneticisi bu kotaları ayırmak ve çalışma alanları genelinde dağıtmak için idealdir.



## <a name="view-your-usage-and-quotas"></a>Kullanımınızı ve kotaları görüntüleme

Aboneliğinizde Azure Machine Learning Işlem kotası diğer Azure kaynakları kotasından ayrı olarak yönetilir. Bu kotayı görüntülemek için Machine Learning Hizmetleri detayına göz duyarsınız.  

1. Sol bölmede **Machine Learning hizmet** ' i seçin ve gösterilen listeden herhangi bir çalışma alanı seçin.

2. Sonraki dikey pencerede, **destek + sorun giderme bölümü** altında, geçerli kota limitlerinizi ve kullanımınızı görüntülemek için **kullanım + kotalar** ' i seçin.

3. Kota sınırlarını görüntülemek için bir abonelik seçin. İlgilendiğiniz bölgeye filtre getirmeyi unutmayın.

4. Artık bir abonelik düzeyi görünümü ve çalışma alanı düzeyi görünümü arasında geçiş yapabilirsiniz:
    + **Abonelik görünümü:** Bu, çekirdek kota kullanımınızı VM ailesi ile görüntülemenizi, çalışma alanına göre genişletmeyi ve gerçek küme adlarına göre genişletmeyi görmenizi sağlar. Bu görünüm, belirli bir VM ailesi için temel kullanımların ayrıntılarını hızla almak ve bu çalışma alanlarının her biri için temel alınan kümeler tarafından daha fazla bilgi almak için idealdir. Bu görünümdeki genel kural (kullanım/kota) olduğundan, kullanım, en güncel ölçekli çekirdek sayısı ve kota, kaynağın ölçeklendirebileceği mantıksal maksimum çekirdek sayısıdır. Her **çalışma alanı**için kota, belırlı bir VM ailesi için ölçeklenebileceğinizi en fazla çekirdek sayısını gösteren çalışma alanı düzeyi kotası (yukarıda açıklandığı gibi) olacaktır. Benzer şekilde, kota **, kümenin max_nodes** özelliği tarafından tanımlanan en fazla düğüm sayısına göre ölçeklenebilen çekirdeklerdir.
    
    + **Çalışma alanı görünümü:** Bu, çalışma alanına göre çekirdek kotasının kullanımını görüntülemenize, VM ailesine göre genişleterek ve gerçek küme adlarına göre daha fazla genişleetmenize olanak tanır. Bu görünüm, belirli bir çalışma alanının ana kullanımının ayrıntılarını hızla almak ve bu ailelerin her biri için temel alınan kümeler tarafından daha fazla bilgi almak için idealdir.

Sanal makineler, depolama, ağ gibi çeşitli Azure kaynakları için kotayı görüntülemek Azure portal kolaydır.

1. Sol bölmede, **tüm hizmetler** ' i seçin ve genel kategorisi altında **abonelikler** ' i seçin.

2. Abonelikler listesinden, kotasını Aradığınız aboneliği seçin.

3. Geçerli kota limitlerinizi ve kullanımınızı görüntülemek için **kullanım + kotalar** ' ı seçin. Sağlayıcıyı ve konumları seçmek için filtreleri kullanın. 

## <a name="request-quota-increases"></a>Kota artışı isteme

Sınırı veya kotayı varsayılan sınırın üzerinde yükseltmek istiyorsanız, ücretsiz [bir çevrimiçi müşteri destek isteği açın](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest/) .

Sınırlar, tablolarda gösterilen en büyük sınır değerinin üzerinde olamaz. Maksimum sınır yoksa, kaynak ayarlanabilir sınırlara sahip olmaz. [Kotayı nasıl artıracağınız hakkında adım adım yönergeler bölümüne bakın](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-quota-errors).

Kota artışı istediğinizde, kotayı artırmak için istediğiniz hizmeti seçmeniz gerekir, bu, Machine Learning hizmet kotası, kapsayıcı örnekleri veya depolama kotası gibi hizmetler olabilir. Azure Machine Learning Işlem için ek olarak, yukarıdaki adımları izleyerek kotayı görüntülerken kota **iste** düğmesine tıklayabilirsiniz.

> [!NOTE]
> [Ücretsiz deneme abonelikleri](https://azure.microsoft.com/offers/ms-azr-0044p) sınır veya kota artışına uygun değildir. [Ücretsiz bir deneme aboneliğiniz](https://azure.microsoft.com/offers/ms-azr-0044p)varsa, bir [Kullandıkça Öde](https://azure.microsoft.com/offers/ms-azr-0003p/) aboneliğine yükseltebilirsiniz. Daha fazla bilgi için bkz. [Azure Ücretsiz deneme sürümünü Kullandıkça Öde](../billing/billing-upgrade-azure-subscription.md) ve  [ücretsiz deneme aboneliği hakkında SSS](https://azure.microsoft.com/free/free-account-faq).

## <a name="private-endpoint-and-private-dns-quota-increases"></a>Özel uç nokta ve özel DNS kotası artar

Bir abonelikte oluşturulabilecek özel uç noktalar ve özel DNS bölgeleri sayısında sınırlamalar vardır. Azure Machine Learning (müşteri) aboneliğinizde kaynak oluşturduğunda, Microsoft 'a ait abonelikte kaynak oluşturan bazı senaryolar vardır. Aşağıdaki senaryolarda, Microsoft 'a ait abonelikte bir kota indirimi istemeniz gerekebilir:

* __Müşteri tarafından yönetilen bir anahtarla özel bağlantı etkin çalışma alanı (CMK)__
* __Sanal ağınızın arkasındaki çalışma alanı için Azure Container Registry__
* __Çalışma alanınıza özel bir bağlantı etkin Azure Kubernetes hizmet kümesi iliştirme__.

Bu senaryolar için bir kesinti istemek üzere aşağıdaki adımları kullanın:

1. [Azure destek Isteği oluşturun](/azure/azure-portal/supportability/how-to-create-azure-support-request#create-a-support-request) ve __temel bilgiler__ bölümünde aşağıdaki seçenekleri belirleyin:

    | Alan | Seçim |
    | ----- | ----- |
    | Sorun türü | Teknik |
    | Hizmet | Hizmetlerim. Açılan listede __Machine Learning__ ' yi seçin. |
    | Sorun türü | Çalışma alanı kurulumu, SDK ve CLı |
    | Sorun alt türü | Çalışma alanını sağlama veya yönetme sorunu |

2. __Ayrıntılar__ bölümünde, kullanmak istediğiniz Azure bölgesini ve kullanmayı planladığınız senaryoyu sağlamak için __Açıklama__ alanını kullanın. Birden çok abonelik için kota artışı istemeniz gerekiyorsa, bu alandaki abonelik kimliklerini de listeleyin.

3. İsteği oluşturmak için __Oluştur__ ' a kullanın.

## <a name="next-steps"></a>Sonraki adımlar

+ [Azure Machine Learning için maliyetleri planlayın & yönetin](concept-plan-manage-cost.md)
