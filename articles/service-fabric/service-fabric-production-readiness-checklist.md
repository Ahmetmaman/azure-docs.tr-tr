---
title: Azure Service Fabric üretim hazırlık denetim listesi | Microsoft Docs
description: Aşağıdaki en iyi yöntemleri izleyerek Service Fabric uygulama ve küme üretim hazırlanın.
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 7/10/2018
ms.author: subramar
ms.openlocfilehash: 7557e2b993a5059df8aea63c7394539acc28c110
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2018
ms.locfileid: "49403533"
---
# <a name="production-readiness-checklist"></a>Üretim hazırlığı denetim listesi

Uygulama ve küme üretim trafiği almaya hazır mı? Çalıştırma ve test, uygulama ve kümeniz mutlaka üretime geçmeye hazırdır anlamına gelmez. Uygulama ve küme aşağıdaki denetim listesini giderek sorunsuz çalışmasını tutun. Bu tüm öğeler kullanıma kesinlikle öneririz. Kuşkusuz, belirli satır öğesi (örneğin, kendi tanılama çerçeveleri) için alternatif çözümler kullanmayı seçebilirsiniz.


## <a name="pre-requisites-for-production"></a>Üretim için ön koşullar

1. 20'den fazla çekirdek veya 10 düğümü olan kümeler için sistem hizmetleri için bir adanmış birincil düğüm türü oluşturun. Ekleme [yerleştirme kısıtlamaları](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md) sistem hizmetleri için birincil düğüm türü ayırmak için. 
2. Birincil düğüm türü için bir D2v2 ya da daha yüksek bir SKU kullanın. En az 50 GB sabit disk kapasitesi ile bir SKU seçmek için önerilir.
2. Üretim kümeleri olmalıdır [güvenli](service-fabric-cluster-security.md). Güvenli bir küme ayarlama örneği için bkz. Bu [küme şablonu](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/7-VM-Windows-3-NodeTypes-Secure-NSG). Sertifikası ortak adlarının kullanın ve kendinden imzalı sertifikaları kullanmaktan kaçının.
4. Ekleme [kapsayıcıları ve hizmetleri üzerinde kaynak kısıtlamaları](service-fabric-resource-governance.md), böylece birden fazla düğüm kaynakların %75 kullandıkları yok. 
5. Anlama ve ayarlama [dayanıklılık düzeyi](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster). Gümüş veya daha yüksek bir dayanıklılık düzeyi, durum bilgisi olan iş yükleri çalıştıran düğümü türleri için önerilir. Birincil düğüm türü, Gümüş veya daha yüksek bir dayanıklılık düzeyine sahip olmalıdır.
6. Anlama ve çekme [güvenilirlik düzeyi](service-fabric-cluster-capacity.md#the-reliability-characteristics-of-the-cluster) düğümü türü. Gümüş veya daha yüksek güvenilirlik önerilir.
7. Yük ve ölçeklendirme testi tanımlamak için iş yüklerinizi [kapasite gereksinimlerini](service-fabric-cluster-capacity.md) kümenizin. 
8. Hizmet ve uygulamalarınızı izlenir ve uygulama günlüklerini güncellenmekte oluşturulan ve depolanan, uyarı ile. Örneğin, [Service Fabric uygulamanızı günlük ekleme](service-fabric-how-to-diagnostics-log.md) ve [Log Analytics ile kapsayıcıları izlemek](service-fabric-diagnostics-oms-containers.md).
9. Küme, uyarı ile izlenir (örneğin, [Log Analytics](service-fabric-diagnostics-event-analysis-oms.md)). 
10. Sanal makine ölçek kümesi altyapının uyarı ile izlenir (örneğin, [Log Analytics](service-fabric-diagnostics-oms-agent.md).
11. Küme sahip [birincil ve ikincil sertifikaları](service-fabric-cluster-security-update-certs-azure.md) her zaman (, kilitli dolayısıyla).
12. Geliştirme, hazırlama ve üretim için ayrı küme korur. 
13. [Uygulama yükseltmeleri](service-fabric-application-upgrade.md) ve [Küme yükseltme](service-fabric-tutorial-upgrade-cluster.md) geliştirme test ve ilk kümeleri hazırlama. 
14. Üretim kümelerinde otomatik güncelleştirmeleri devre dışı bırakın ve geliştirme ve kümeler (gerektiği gibi Geri Al) hazırlama için açın. 
15. Hizmetiniz bir kurtarma noktası hedefi (RPO) oluşturmak ve ayarlanmış bir [olağanüstü durum kurtarma işlemi](service-fabric-disaster-recovery.md) ve test etmek.
16. Planlama [ölçeklendirme](service-fabric-cluster-scaling.md) kümenizi el ile veya programlama yoluyla.
17. Planlama [düzeltme eki uygulama](service-fabric-patch-orchestration-application.md) Küme düğümlerinizi. 
18. Böylece son değişikliklerinizi sürekli olarak test edilen bir CI/CD işlem hattı oluşturun. Örneğin, kullanarak [Azure DevOps](service-fabric-tutorial-deploy-app-with-cicd-vsts.md) veya [Jenkins](service-fabric-cicd-your-linux-applications-with-jenkins.md)
19. Test, geliştirme ve kümeleri ile yük altında hazırlama [hata analizi hizmeti](service-fabric-testability-overview.md) ve denetlenen anlamına [chaos](service-fabric-controlled-chaos.md). 
20. Planlama [ölçeklendirme](service-fabric-concepts-scalability.md) uygulamalarınızı. 


Service Fabric güvenilir hizmetler veya Reliable Actors programlama modellerini kullanıyorsanız, iade aşağıdaki öğeler gerekir:
21. Uygulama hizmeti kodunuza iptal belirtecini uygularken olduğunu denetlemek için yerel geliştirme sırasında yükseltme `RunAsync` yöntemi ve özel iletişim dinleyicileri kapatma.
22. Önlemek [planlarken düşebileceğiniz yaygın tuzaklardan](service-fabric-work-with-reliable-collections.md) güvenilir koleksiyonlar kullanarak.
23. Çalıştırırken sayaçları yük testleri ve çöp toplama veya kaçan yığın büyüme yüksek oranlarda denetleyin .NET CLR bellek performansı izleyin.
24. Çevrimdışı yedeğini tutmak [Reliable Services ve Reliable Actors](service-fabric-reliable-services-backup-restore.md) ve geri yükleme işlemini test edin. 


## <a name="optional-best-practices"></a>İsteğe bağlı en iyi uygulamalar

Yukarıdaki listelerin üretime geçmeye ön koşullar olsa da, aşağıdaki öğeler de bulundurulmalıdır:
25. Takın [Service Fabric sistem durumu modeli](service-fabric-health-introduction.md) genişletme yerleşik sistem durumu değerlendirme ve raporlama için.
26. Uygulamanızı ve raporları izleme özel bir bekçi dağıtma [yük](service-fabric-cluster-resource-manager-metrics.md) için [kaynak Dengeleme](service-fabric-cluster-resource-manager-balancing.md). 


## <a name="next-steps"></a>Sonraki adımlar
* [Bir Service Fabric Windows kümesi dağıtma](service-fabric-tutorial-create-vnet-and-windows-cluster.md)
* [Bir Service Fabric Linux kümesi dağıtma](service-fabric-tutorial-create-vnet-and-linux-cluster.md)
* Service Fabric [uygulama yaşam döngüsü](service-fabric-application-lifecycle.md) hakkında bilgi edinin.
