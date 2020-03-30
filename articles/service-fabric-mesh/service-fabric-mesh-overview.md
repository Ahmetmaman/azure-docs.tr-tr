---
title: Azure Hizmet Kumaş Örgü'ye Genel Bakış
description: Azure Service Fabric Mesh hakkında bilgi edinin. Service Fabric Mesh ile uygulamanızın altyapı gereksinimleri konusunda endişelenmeden uygulamanızı dağıtabilir ve ölçeklendirebilirsiniz.
author: dkkapur
ms.author: dekapur
ms.date: 10/1/2018
ms.topic: overview
ms.openlocfilehash: d6522d417556104a1ece703c725f3fbeab49d683
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "75458978"
---
# <a name="what-is-service-fabric-mesh"></a>Service Fabric Mesh nedir?

Bu video Service Fabric Mesh için bir genel bakış sağlamaktadır.
> [!VIDEO https://www.youtube.com/embed/7qWeVGzAid0]

Azure Service Fabric Mesh, geliştiricilerin sanal makineleri, depolama alanını veya ağ bileşenlerini yönetmeden mikro hizmet uygulamaları dağıtmasını sağlayan tam olarak yönetilen bir hizmettir. Service Fabric Mesh üzerinde barındırılan uygulamalar, altyapı konusunda endişelenmenize gerek kalmadan çalışır ve ölçeklendirilir.  Service Fabric Mesh, binlerce makineden oluşan kümelere sahiptir.  Tüm küme işlemleri geliştiriciden gizli olarak gerçekleştirilir. Kodunuzu yükleyin ve ihtiyacınız olan kaynakları, kullanılabilirlik gereksinimlerini ve kaynak sınırlarını belirtin.  Service Fabric Mesh, altyapıyı otomatik olarak dağıtır ve altyapı hatalarını da yöneterek uygulamalarınızın yüksek oranda kullanılabilir durumda olmasını sağlar. Altyapı konusunda değil yalnızca uygulamanızın durumuna ve yanıt süresine dikkat etmeniz gerekir.  

[!INCLUDE [preview note](./includes/include-preview-note.md)]

Bu makale, Service Fabric Mesh'in önemli avantajlarına genel bakış niteliğindedir.

## <a name="great-developer-experience"></a>Üstün geliştirici deneyimi

Service Fabric Mesh, kapsayıcı içinde çalıştırılabilen tüm programlama dillerini ve çerçeveleri destekler. Visual Studio 2019 ve Visual Studio Code araç desteği ,NET ve .NET Core uygulamaları için güçlü bir düzenleme ve hata ayıklama deneyimi sağlar. 

Service Fabric Mesh ile şunları yapabilirsiniz:

- Var olan uygulamaları kapsayıcılara taşıyarak uygun ölçekte modernleştirebilir ve çalıştırabilirsiniz.
- Azure'da yeni mikro hizmet uygulamalarını uygun ölçekte derleyebilir ve dağıtabilirsiniz.  Diğer Azure hizmetleri veya kapsayıcılarda çalışan mevcut uygulamalarla tümleştirebilirsiniz. Her microservice güvenli, ağ yalıtılmış uygulamanın bir parçasıdır. Microservice CPU çekirdekleri, bellek, disk alanı ve daha fazlası için tanımlanmış kaynak yönetim ilkeleri vardır.
- Var olan uygulamalarda değişiklik yapmadan tümleştirme ve uzatma işlemleri gerçekleştirebilirsiniz. Var olan uygulamayı yeni uygulamaya bağlamak için kendi sanal ağınızı kullanabilirsiniz.  
- Service Fabric Mesh'e geçiş yaparak var olan Cloud Services uygulamalarınızı modernleştirebilirsiniz.  

## <a name="simple-operational-lifecycle"></a>Basit işlemsel yaşam döngüsü

Çalışan uygulamaları, izleme uygulamalarını ve üretim ortamlarında hata ayıklamayı kolayca yönetin. Bu yönetim, uygulama yükseltmeleri ve sürüm içerir. Bu uygulamalar tek bir mikro hizmetten veya kendi ağında bulunan birden fazla mikro hizmetten oluşabilir. Uygulamalar hızlı dağıtım, yerleştirme ve yük devretme süreleri sayesinde verimli bir şekilde çalışır.

Service Fabric Mesh ile şunları yapabilirsiniz:

- Altyapı sağlamak veya yönetmek zorunda kalmadan uygulamaları dağıtma ve yönetme.  Service Fabric Mesh altyapıyı sizin için sağlar, yükseltir, düzeltme eki uygular ve bakımını yapar.
- Uygulamaları kolayca paketlemek ve dağıtmak için tümleşik araçları kullanarak sürekli tümleştirmeye geçiş yapabilirsiniz.
- Azure Kaynak Yöneticisi kaynaklarının tüm özelliklerinden yararlanın. Bu özelliklere örnek olarak denetim izi ve [rol tabanlı erişim denetimi](/azure/role-based-access-control/overview)verilebilir). Azure'daki Hizmet Kumaş Kafesi hizmetine dağıttığınız tüm kaynaklar Azure Kaynak Yöneticisi kaynaklarıdır. Bu kaynaklar uygulamaları, hizmetleri, sırları ve benzeri içerir.
- Kaynakları [Azure portal](https://portal.azure.com), Resource Manager şablonları veya Azure CLI/PowerShell kitaplıklarını kullanarak dağıtabilir ve yönetebilirsiniz.
- [Application Insights](/azure/application-insights/)'ı (veya istediğiniz bir aracı) kullanarak işlem izleme ve uyarı ayarlarını yapabilir, platformdan işlem ve tanılama izlemelerini alabilirsiniz.
- [Application Insights](/azure/application-insights/)'ı veya istediğiniz bir aracı kullanarak uygulama modelinden gelen uygulama tanılama bilgilerine erişebilirsiniz.
- Uygulama tanımında hizmetler için otomatik ölçek kuralları belirterek kaynak kullanımını en iyi duruma getirin.

## <a name="mission-critical-platform-capabilities"></a>Görev açısından kritik platform özellikleri

Service Fabric Mesh, [Azure Kullanılabilirlik Alanları](/azure/availability-zones/az-overview)'nı ve/veya coğrafi bölge sınırlarını kullanan bir küme koleksiyonu oluşturur. Service Fabric Mesh, ölçek, donanım gereksinimleri, dayanıklılık gereksinimleri ve güvenlik ilkeleri gibi bir dizi amacı olan uygulamaları açıklar.  Uygulama dağıtıldığında Service Fabric Mesh çalıştırmak için en uygun yeri belirler.

Service Fabric Mesh ile şunları yapabilirsiniz:

- Yüksek kullanılabilirlik, ölçek büyütme/küçültme, bulunabilirlik, yönetim, mesaj yönlendirme, güvenilir mesajlaşma, kesintisiz yükseltme, güvenlik/gizli dizi yönetimi, olağanüstü durum kurtarma, durum yönetimi, yapılandırma yönetimi ve dağıtılmış işlem özelliklerinden faydalanabilirsiniz.
- Uygulama oluştururken çoklu uygulama modelleri arasından seçim yapabilirsiniz.
- Swagger tarafından oluşturulan dile özgü bağlamaları kullanarak REST uç noktaları ile sunulan platform özelliklerini kullanabilirsiniz.
- Coğrafi güvenilirlik için uygulamaları birden fazla [Kullanılabilirlik Alanında](/azure/availability-zones/az-overview) ve bölgede dağıtabilirsiniz.
- Azure tarafından sağlanan tüm güvenlik ve uyumluluk özelliklerinden faydalanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Yalnızca birkaç adımda Visual Studio ile örnek bir proje dağıtabilirsiniz. Daha fazla bilgi için bkz. [ASP.NET Core web sitesi oluşturma](service-fabric-mesh-quickstart-dotnet-core.md). 

[Sık sorulan sorulara](service-fabric-mesh-faq.md) yanıtlar bulun.


<!-- Links -->

[service-fabric-overview]: ../service-fabric/service-fabric-overview.md
