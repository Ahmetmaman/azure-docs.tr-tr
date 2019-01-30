---
title: Azure Service Fabric üretim hazırlık denetim listesi | Microsoft Docs
description: Aşağıdaki en iyi yöntemleri izleyerek Service Fabric uygulama ve küme üretim hazırlanın.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 7/10/2018
ms.author: aljo-microsoft
ms.openlocfilehash: 02ad880f3c4a4f5812b60887090c29a0a39f6742
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55206978"
---
# <a name="production-readiness-checklist"></a>Üretim hazırlığı denetim listesi

Uygulama ve küme üretim trafiği almaya hazır mı? Çalıştırma ve test, uygulama ve kümeniz mutlaka üretime geçmeye hazırdır anlamına gelmez. Uygulama ve küme aşağıdaki denetim listesini giderek sorunsuz çalışmasını tutun. Bu tüm öğeler kullanıma kesinlikle öneririz. Kuşkusuz, belirli satır öğesi (örneğin, kendi tanılama çerçeveleri) için alternatif çözümler kullanmayı seçebilirsiniz.


## <a name="pre-requisites-for-production"></a>Üretim için ön koşullar
1. [Azure Service Fabric güvenliği iyi](https://docs.microsoft.com/azure/security/azure-service-fabric-security-best-practices) şunlardır: 
* X.509 sertifikaları kullanma
* Güvenlik ilkelerini yapılandır
* Azure Service Fabric için SSL'yi yapılandırma
* Ağ yalıtımını ve güvenlik, Azure Service Fabric ile kullanma
* Güvenlik için Azure anahtar kasası ayarlama
* Microsoft.Network/loadBalancersAssign kullanıcıları rollere
* Reliable Actors güvenlik yapılandırması Actors programlama modelini kullanıyorsa uygulayın
2. 20'den fazla çekirdek veya 10 düğümü olan kümeler için sistem hizmetleri için bir adanmış birincil düğüm türü oluşturun. Ekleme [yerleştirme kısıtlamaları](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md) sistem hizmetleri için birincil düğüm türü ayırmak için.
3. Birincil düğüm türü için bir D2v2 ya da daha yüksek bir SKU kullanın. En az 50 GB sabit disk kapasitesi ile bir SKU seçmek için önerilir.
4. Üretim kümeleri olmalıdır [güvenli](service-fabric-cluster-security.md). Güvenli bir küme ayarlama örneği için bkz. Bu [küme şablonu](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/7-VM-Windows-3-NodeTypes-Secure-NSG). Sertifikası ortak adlarının kullanın ve kendinden imzalı sertifikaları kullanmaktan kaçının.
5. Ekleme [kapsayıcıları ve hizmetleri üzerinde kaynak kısıtlamaları](service-fabric-resource-governance.md), böylece birden fazla düğüm kaynakların %75 kullandıkları yok. 
6. Anlama ve ayarlama [dayanıklılık düzeyi](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster). Gümüş veya daha yüksek bir dayanıklılık düzeyi, durum bilgisi olan iş yükleri çalıştıran düğümü türleri için önerilir. Birincil düğüm türü, Gümüş veya daha yüksek bir dayanıklılık düzeyine sahip olmalıdır.
7. Anlama ve çekme [güvenilirlik düzeyi](service-fabric-cluster-capacity.md#the-reliability-characteristics-of-the-cluster) düğümü türü. Gümüş veya daha yüksek güvenilirlik önerilir.
8. Yük ve ölçeklendirme testi tanımlamak için iş yüklerinizi [kapasite gereksinimlerini](service-fabric-cluster-capacity.md) kümenizin. 
9. Hizmet ve uygulamalarınızı izlenir ve uygulama günlüklerini güncellenmekte oluşturulan ve depolanan, uyarı ile. Örneğin, [Service Fabric uygulamanızı günlük ekleme](service-fabric-how-to-diagnostics-log.md) ve [Log Analytics ile kapsayıcıları izlemek](service-fabric-diagnostics-oms-containers.md).
10. Küme, uyarı ile izlenir (örneğin, [Log Analytics](service-fabric-diagnostics-event-analysis-oms.md)). 
11. Sanal makine ölçek kümesi altyapının uyarı ile izlenir (örneğin, [Log Analytics](service-fabric-diagnostics-oms-agent.md).
12. Küme sahip [birincil ve ikincil sertifikaları](service-fabric-cluster-security-update-certs-azure.md) her zaman (, kilitli dolayısıyla).
13. Geliştirme, hazırlama ve üretim için ayrı küme korur. 
14. [Uygulama yükseltmeleri](service-fabric-application-upgrade.md) ve [Küme yükseltme](service-fabric-tutorial-upgrade-cluster.md) geliştirme test ve ilk kümeleri hazırlama. 
15. Üretim kümelerinde otomatik güncelleştirmeleri devre dışı bırakın ve geliştirme ve kümeler (gerektiği gibi Geri Al) hazırlama için açın. 
16. Hizmetiniz bir kurtarma noktası hedefi (RPO) oluşturmak ve ayarlanmış bir [olağanüstü durum kurtarma işlemi](service-fabric-disaster-recovery.md) ve test etmek.
17. Planlama [ölçeklendirme](service-fabric-cluster-scaling.md) kümenizi el ile veya programlama yoluyla.
18. Planlama [düzeltme eki uygulama](service-fabric-patch-orchestration-application.md) Küme düğümlerinizi. 
19. Böylece son değişikliklerinizi sürekli olarak test edilen bir CI/CD işlem hattı oluşturun. Örneğin, kullanarak [Azure DevOps](service-fabric-tutorial-deploy-app-with-cicd-vsts.md) veya [Jenkins](service-fabric-cicd-your-linux-applications-with-jenkins.md)
20. Test, geliştirme ve kümeleri ile yük altında hazırlama [hata analizi hizmeti](service-fabric-testability-overview.md) ve denetlenen anlamına [chaos](service-fabric-controlled-chaos.md). 
21. Planlama [ölçeklendirme](service-fabric-concepts-scalability.md) uygulamalarınızı. 


Service Fabric güvenilir hizmetler veya Reliable Actors programlama modellerini kullanıyorsanız, iade aşağıdaki öğeler gerekir:
22. Uygulama hizmeti kodunuza iptal belirtecini uygularken olduğunu denetlemek için yerel geliştirme sırasında yükseltme `RunAsync` yöntemi ve özel iletişim dinleyicileri kapatma.
23. Önlemek [planlarken düşebileceğiniz yaygın tuzaklardan](service-fabric-work-with-reliable-collections.md) güvenilir koleksiyonlar kullanarak.
24. Çalıştırırken sayaçları yük testleri ve çöp toplama veya kaçan yığın büyüme yüksek oranlarda denetleyin .NET CLR bellek performansı izleyin.
25. Çevrimdışı yedeğini tutmak [Reliable Services ve Reliable Actors](service-fabric-reliable-services-backup-restore.md) ve geri yükleme işlemini test edin.
26. Birincil NodeType sanal makine örnek sayınız, ideal olarak, kümeler güvenilirlik katmanı için en düşük eşit olmalıdır; uygun olduğunda en düşük katmanlı aşmayı koşullar bulunmaktadır: geçici olarak ne zaman dikey birincil NodeType sanal makine ölçek kümesi SKU'nuz ölçeklendirme.

## <a name="optional-best-practices"></a>İsteğe bağlı en iyi uygulamalar

Yukarıdaki listelerin üretime geçmeye ön koşullar olsa da, aşağıdaki öğeler de bulundurulmalıdır:
26. Takın [Service Fabric sistem durumu modeli](service-fabric-health-introduction.md) genişletme yerleşik sistem durumu değerlendirme ve raporlama için.
27. Uygulamanızı ve raporları izleme özel bir bekçi dağıtma [yük](service-fabric-cluster-resource-manager-metrics.md) için [kaynak Dengeleme](service-fabric-cluster-resource-manager-balancing.md). 


## <a name="next-steps"></a>Sonraki adımlar
* [Bir Service Fabric Windows kümesi dağıtma](service-fabric-tutorial-create-vnet-and-windows-cluster.md)
* [Bir Service Fabric Linux kümesi dağıtma](service-fabric-tutorial-create-vnet-and-linux-cluster.md)
* Service Fabric [uygulama yaşam döngüsü](service-fabric-application-lifecycle.md) hakkında bilgi edinin.
