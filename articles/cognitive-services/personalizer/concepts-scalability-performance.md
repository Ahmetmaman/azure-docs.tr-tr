---
title: Ölçeklenebilirlik ve performans-kişiselleştirici
titleSuffix: Azure Cognitive Services
description: 'Yüksek performanslı ve yüksek trafikli web siteleri ve uygulamalar, ölçeklenebilirlik ve performans için kişiselleştirmede göz önünde bulundurmanız gereken iki ana etkene sahiptir: gecikme süresi ve eğitim performansı.'
services: cognitive-services
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: conceptual
ms.date: 10/24/2019
ms.openlocfilehash: 52d22fce64a26267e73cb9a1df8614129bb96c3e
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "87132696"
---
# <a name="scalability-and-performance"></a>Ölçeklenebilirlik ve performans

Yüksek performanslı ve yüksek trafikli web siteleri ve uygulamalar, ölçeklenebilirlik ve performans için Kişiselleştiriciye göz önünde bulundurmanız gereken iki ana etkene sahiptir:

* Derecelendirme API 'SI çağrıları yaparken düşük gecikme süresi tutma
* Eğitim aktarım hızının olay girişi ile devam ettiğinizden emin olma

Kişiselleştirme, REST API aracılığıyla iletişime ayrılan çağrı süresinin büyük bir bölümünü hızla bir derece döndürebilir. Azure, isteklere hızla yanıt verebilme özelliğini otomatik olarak sağlar.

##  <a name="low-latency-scenarios"></a>Düşük gecikmeli senaryolar

Bazı uygulamalar bir derece döndürürken düşük gecikme süreleri gerektirir. Düşük gecikme süreleri gereklidir:

* Kullanıcının, derecelendirilen içeriği görüntülemeden önce fark edilebilir bir süre beklememasını sağlamak.
* Extreme trafiği yaşayan bir sunucuda, nadir işlem süresini ve ağ bağlantılarını kullanmaktan kaçının.


## <a name="scalability-and-training-throughput"></a>Ölçeklenebilirlik ve eğitim performansı

Kişiselleştirici, API 'Lerin derecelendirmeden ve geri alındıktan sonra zaman uyumsuz olarak gönderilen iletilere bağlı olarak yeniden paketlenmiş bir model güncelleştirerek işe yarar. Bu iletiler, uygulama için bir Azure EventHub kullanılarak gönderilir.

 Çoğu uygulama, kişiselleştirici için en fazla katılma ve eğitim aktarım hızına ulaşacaktır. Bu sınıra ulaşıldığında, uygulama yavaşlamadan, Olay Hub 'ı sıralarının, temizlenmeden daha hızlı doldurulmuş olduğunu fark eder.

## <a name="how-to-estimate-your-throughput-requirements"></a>Verimlilik gereksinimlerinizi tahmin etme

* Bağlam ve eylem JSON belgelerinin uzunluklarını ekleyerek sıralama olayı başına ortalama bayt sayısını tahmin edin.
* Bu tahmini ortalama bayt ile 20 MB/sn bölün.

Örneğin, ortalama yükünüzü 500 özelliği varsa ve her biri tahmini 20 karakter ise, her olay yaklaşık 10 KB olur. Bu tahminlerde 20.000.000/10.000 = 2.000 olay/sn, bu da 173.000.000 olaylar/gün ile ilgilidir. 

Bu sınırlara ulaşmanız durumunda, lütfen mimari önerisi için destek ekibimize başvurun.

## <a name="next-steps"></a>Sonraki adımlar

[Kişiselleştirici oluşturma ve yapılandırma](how-to-settings.md).