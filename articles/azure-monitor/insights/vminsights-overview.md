---
title: Azure İzleyici VM'ler (Önizleme) nedir? | Microsoft Docs
description: VM'ler için Azure İzleyici bir sistem durumunu ve uygulama bileşenleri ve diğer kaynaklarla ilgili bağımlılıkları otomatik olarak keşfetme yanı sıra Azure VM'nin işletim sistemi izleme performansını birleştirir ve iletişimi eşleyen bir Azure İzleyici özelliğidir. Bunlar arasında. Bu makalede, genel bir bakış sağlar.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: ''
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/13/2019
ms.author: magoedte
ms.openlocfilehash: f7a0300619d82f760c0e307601efbd3987eb6067
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/18/2019
ms.locfileid: "58004595"
---
# <a name="what-is-azure-monitor-for-vms-preview"></a>Azure İzleyici VM'ler (Önizleme) nedir?

Azure sanal makinelerinizi (VM) sanal makineler için Azure İzleyici izler ve uygun ölçekte sanal makine ölçek kümeleri. Windows ve Linux Vm'leri sistem durumu ve performansı analiz eder ve kendi işlemlerini ve diğer kaynakları ve işlemleri dış bağımlılıkları izler. 

VM'ler için Azure İzleyici, bir çözüm olarak, başka bir bulut sağlayıcısında veya şirket içi VM'ler için performans ve uygulama bağımlılıklarını izlemek için destek içerir. Üç anahtar özellik kapsamlı Öngörüler sunun:

* **Windows ve Linux çalıştıran Azure VM'lerin mantıksal bileşenler**: Önceden yapılandırılmış bir sistem durumu ölçütlerine göre ölçülür ve değerlendirilen koşul karşılandığında, sizi uyarır.  

* **Popüler önceden tanımlı performans grafiklerini**: Konuk VM işletim sisteminden temel performans ölçümlerini görüntüleyin.

* **Bağımlılık Haritası**: Çeşitli kaynak gruplarında ve Aboneliklerde VM'den birbirine bağlı bileşenlerle görüntüler.  

Özellikler, üç Perspektifler düzenlenmiştir:

* Durum
* Performans
* Eşleme

>[!NOTE]
>Şu anda, sistem durumu özelliği yalnızca Azure sanal makineler ve sanal makine ölçek kümeleri sunulur. Performans ve harita özelliklerini hem Azure Vm'leri hem de barındırılan sanal makineleri ortamınızda veya diğer bulut sağlayıcısı destekler.

Azure İzleyici günlüklerine ile tümleştirme, güçlü toplama ve filtreleme sunar ve zaman içinde veri eğilimlerini çözümleyebilirsiniz. Kapsamlı iş yükü izleme gibi Azure İzleyici ya da tek başına hizmet eşlemesi ile elde edilemeyecek.  

Bu veriler tek bir sanal makinede sanal makineden doğrudan görüntüleyebileceğiniz veya sanal makinelerinizin toplu bir görünümünü sunmak için Azure İzleyicisi'ni kullanabilirsiniz. Bu görünüm, her özelliğin perspektif dayanır:

* **Sistem durumu**: Vm'leri bir kaynak grubuna ilgilidir.
* **Harita** ve **performans**: VM'ler, belirli bir Log Analytics çalışma alanına rapor için yapılandırılır.

![Azure portalında sanal makine ınsights perspektifi](./media/vminsights-overview/vminsights-azmon-directvm-01.png)

VM'ler için Azure İzleyici, tahmin edilebilir performans ve kullanılabilirliği önemli uygulamalara teslim edebilirsiniz. Bu kritik işletim sistemi olayları, performans sorunlarını ve ağ sorunları tanımlar. VM'ler için Azure İzleyici ayrıca, bir sorun için diğer bağımlılıkları ilişkili olup olmadığını anlamanıza yardımcı olabilir.  

## <a name="data-usage"></a>Veri kullanımı 

VM'ler için Azure İzleyici'yi dağıttığınızda, sanal makineleriniz tarafından toplanan verileri alınır ve Azure İzleyici'de depolanan. Sistem durumu ölçütlerini ölçümleri Azure İzleyici'de bir zaman serisi veritabanına depolanır, toplanan performans ve bağımlılık verileri Log Analytics çalışma alanında depolanır. Yayımlanan fiyatlandırmaya göre [Azure fiyatlandırma sayfasını İzleyici](https://azure.microsoft.com/pricing/details/monitor/), VM'ler için Azure İzleyici için faturalandırılır:

* Alınan ve depolanan veriler.
* Zaman serisi, izlenen sistem durumu ölçütlerini ölçüm sayısı.
* Oluşturulan uyarı kuralları.
* Gönderilen bildirimleri. 

Performans sayaçlarının dize uzunluğu günlük boyutunu değişir ve mantıksal diskleri ve sanal Makineye ayrılan ağ bağdaştırıcısı sayısını artırabilirsiniz. Zaten bir çalışma alanı varsa ve bu sayaçları toplamak, yinelenen herhangi bir ücret uygulanır. Hizmet eşlemesi zaten kullanıyorsanız, gördüğünüz tek değişiklik, Azure İzleyici gönderilen ek bağlantı verilerdir.

## <a name="next-steps"></a>Sonraki adımlar
Sanal makinelerinizi izlemenize yardımcı yöntemler ve gereksinimleri hakkında bilgilere [VM'ler için Azure İzleyici'ı Dağıtma](vminsights-onboard.md).
