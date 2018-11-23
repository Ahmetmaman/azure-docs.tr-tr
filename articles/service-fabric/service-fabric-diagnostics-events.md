---
title: Azure Service Fabric olayları | Microsoft Docs
description: Azure Service Fabric kümenizi izlemenize yardımcı olması için kullanıma hazır sağlanan Service Fabric olayları hakkında bilgi edinin.
services: service-fabric
documentationcenter: .net
author: srrengar
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/21/2018
ms.author: srrengar
ms.openlocfilehash: 936a47593b9db6e4989c30b2df37dfd82c286c59
ms.sourcegitcommit: beb4fa5b36e1529408829603f3844e433bea46fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2018
ms.locfileid: "52290527"
---
# <a name="service-fabric-events"></a>Service Fabric olayları 

Service Fabric platform, küme içinde'olmuyor anahtar işletimsel etkinlikler için yapılandırılmış birkaç olayları yazar. Küme yükseltme bu aralıktan çoğaltma yerleştirme kararları için. Her olay, Service Fabric kümesinde aşağıdaki varlıkları birine eşlemeleri gösterir:
* Küme
* Uygulama
* Hizmet
* Bölüm
* Çoğaltma 
* Kapsayıcı

-Platform tarafından sunulan olayların tam listesi görmek için [Service Fabric listesi olayları](service-fabric-diagnostics-event-generation-operational.md).

Kümenizde olayları görmelisiniz senaryoları bazı örnekleri aşağıda verilmiştir. 
* Düğüm yaşam döngüsü olaylarını: düğümleri gündeme, aşağı git, Ölçek daraltma veya genişletme, yeniden başlatın ve etkin/devre dışı olarak bu olayları ne olduğunu gösteren sunulur ve varsa bir makine ile sorun veya var olan bir API olsa belirlemenize yardımcı bir düğümün durumunu değiştirmek için BT çağrılır.
* Küme Yükseltmesi: kümenizi yükseltilmiş olarak (SF sürümü veya yapılandırma değişikliği), yükseltmeyi tamamlamak başlatmak ve her biri, yükseltme etki alanları ile geri görürsünüz (veya geri alma). 
* Uygulama yükseltme: Küme yükseltme olduğu gibi olayları kapsamlı bir dizi aracılığıyla yükseltme yapar gibi bulunur. Bu olayların ne zaman yükseltme zamanlandı, geçerli yükseltme durumunu ve genel olaylar dizisini anlamak yararlı olabilir. Bu, ne yükseltmelerini başarıyla piyasaya sunuluyor veya bir geri alma olup tetiklendi görmek için geri arama kullanışlıdır.
* Uygulama/hizmet dağıtımı / silme: her uygulama, hizmet ve kapsayıcı, veya örneğin çoğaltmaları sayısının artırılması dışa ölçeklendirme oluşturulan veya silinen ve kullanışlı olan bir olay
* Bölüm taşır (yeniden): durum bilgisi olan bir bölümü yeniden yapılandırma (çoğaltma kümesi bir değişiklik) geçer her bir olay kaydedilir. Bu, ne sıklıkta bölüm çoğaltma kümenizi değiştirme veya yük devretmeyi anlama çalıştığınız ya da hangi düğümün zaman içinde herhangi bir noktada birincil çoğaltma çalıştığı izlemek yararlı olur.
* Kaos olayları: Service Fabric'in kullanırken [Chaos](service-fabric-controlled-chaos.md) hizmet, hizmet başlatılmış veya durdurulmuşsa her zaman veya sistemdeki bir hata ekler olayları görürsünüz.
* Sistem durumu olayları: Service Fabric her zaman bir uyarı veya hata sistem durumu raporu oluşturulur, bir varlığın bir Tamam sistem durumuna döner veya bir sistem durumu raporu süresi sistem durumu olaylarını kullanıma sunar. Bu olayları bir varlık için geçmiş sağlık istatistikleri izlemek çok yararlı olur. 

## <a name="how-to-access-events"></a>Olayları erişme

Service Fabric olayları erişilebilen birkaç farklı yolu vardır:
* Olayları ETW/Windows olay günlükleri gibi standart kanallar aracılığıyla günlüğe kaydedilir ve bu Log Analytics gibi destekleyen herhangi bir izleme aracı tarafından görselleştirilebilir. Varsayılan olarak, kümeler portalda oluşturulan tanılama açıkken varsa ve Azure tablo depolama birimine olayları gönderen Windows Azure tanılama aracısını, ancak yine de bu Log Analytics kaynağınızı tümleştirme gerekiyorsa. Yapılandırma hakkında daha fazla okuma [Azure tanılama aracısını](service-fabric-diagnostics-event-aggregation-wad.md) kümenizi daha fazla günlük veya performans sayaçlarını alacak şekilde Tanılama yapılandırmasını değiştirmek için ve [Log Analytics tümleştirmesi](service-fabric-diagnostics-event-analysis-oms.md)
* Doğrudan veya Service Fabric istemci kitaplığı aracılığıyla küme sorgu olanak tanıyan Eventstore'a hizmetin Rest API'ler. Bkz: [küme olayları için sorgu EventStore API'leri](service-fabric-diagnostics-eventstore-query.md).

## <a name="next-steps"></a>Sonraki adımlar
* Daha fazla bilgi - kümenizi izlemeye [platform ve küme izleme](service-fabric-diagnostics-event-generation-infra.md).
* Eventstore'a hizmeti hakkında-daha fazla bilgi [Eventstore'a hizmetine genel bakış](service-fabric-diagnostics-eventstore.md)
