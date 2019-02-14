---
title: Azure Stream Analytics (Önizleme) anomali algılama
description: Bu makalede, Azure Stream Analytics ve Azure Machine Learning birlikte anomalileri algılamak için nasıl kullanılacağını açıklar.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 02/13/2019
ms.custom: seodec18
ms.openlocfilehash: bdd512972f1a684a3b76ae0323bbadd87bf0d659
ms.sourcegitcommit: de81b3fe220562a25c1aa74ff3aa9bdc214ddd65
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2019
ms.locfileid: "56238326"
---
# <a name="anomaly-detection-in-azure-stream-analytics"></a>Azure Stream analytics'te anomali algılama

Azure Stream Analytics, yerleşik makine öğrenimi tabanlı anomali algılama iki en yaygın olarak karşılaşılan anomalileri izlemek için kullanılan bir özellikleri sunar: geçici ve kalıcı. İle **AnomalyDetection_SpikeAndDip** ve **AnomalyDetection_ChangePoint** İşlevler, doğrudan Stream Analytics işinizi anomali algılama gerçekleştirebilirsiniz.

Makine öğrenimi modelleri birörnek örneklenen zaman serisi varsayılır. Zaman serisi Tekdüzen değilse, anomali algılama çağırmadan önce bir atlayan pencere ile bir toplama adım ekleyebilirsiniz.

Makine öğrenimi işlemleri mevsimsellik eğilimleri veya çok değişkenli bağıntılar desteklemez.

## <a name="model-accuracy-and-performance"></a>Model doğruluk ve performans

Genellikle, daha fazla veri kayan pencere modelin doğruluğunu artırır. Belirtilen kayan pencere verileri, o zaman çerçevesi için değerler normal kendi aralığının bir parçası olarak kabul edilir. Model yalnızca geçerli olay anormal olup olmadığını denetlemek için kayan pencere üzerinde olay geçmişi dikkate alır. Kayan pencere taşınırken, eski değerleri modelin eğitimleri çıkarılan.

İşlevler, ne oldukları kadar gördünüz üzerinde dayalı belirli bir normal oluşturarak çalışır. Aykırı değerleri güven düzeyi içinde kurulu normal karşı karşılaştırma tarafından tanımlanır. Pencere boyutu bir anomali ortaya çıktığında, bunu tanımasına olacaktır, böylece normal davranış modeli eğitmek için gereken en düşük olayları bağlı olmalıdır.

Geçmiş olaylar daha yüksek bir sayı karşı Karşılaştırılacak gerektiğinden, modelin yanıt süresi geçmiş boyutla artar aklınızda bulundurun. Olayları daha iyi performans için gereken sayıda yalnızca dahil etmek için önerilir.

Zaman serisi boşlukları belirli noktalarda olaylar sürede almıyor modelinin bir sonucu olabilir. Bu durum imputation kullanarak Stream Analytics tarafından işlenir. Aynı kayan pencere için bir süre yanı sıra, geçmiş boyutu, olayları gelmesi beklenen ortalaması hesaplamak için kullanılır.

## <a name="spike-and-dip"></a>Depo ve DIP

Zaman serisi olay akışı geçici anomalileri ani artışlar ve düşüşler olarak bilinir. Ani artışlar ve düşüşler makine öğrenimi tabanlı işlecini kullanarak izlenebilir **AnomalyDetection_SpikeAndDip**.

![Depo ve DIP anomali örneği](./media/stream-analytics-machine-learning-anomaly-detection/anomaly-detection-spike-dip.png)

İkinci bir depo ilki küçükse, aynı kayan pencere daha küçük depo için hesaplanan puanı yeterli güven düzeyi içinde ilk depo için puana göre belirlenen önemli değildir büyük olasılıkla. Modelin güvenilirlik düzeyi ayarı bu tür anormal durumları yakalamak için azaltmayı deneyebilirsiniz. Ancak, çok sayıda uyarı almak başlatırsanız, daha yüksek bir olasılık aralığı kullanabilirsiniz.

Aşağıdaki örnek sorgu 1 olay bir 2 dakikalık kayan pencere, 120 olayların geçmişini ile saniyede Tekdüzen giriş fiyatı varsayar. Son SELECT deyimi ayıklar ve puan ve anomali çıkaran durumu % 95 güven düzeyine sahip.

```SQL
WITH AnomalyDetectionStep AS
(
    SELECT
        EVENTENQUEUEDUTCTIME AS time,
        CAST(temperature AS float) AS temp,
        AnomalyDetection_SpikeAndDip(CAST(temperature AS float), 95, 120, 'spikesanddips')
            OVER(LIMIT DURATION(second, 120)) AS SpikeAndDipScores
    FROM input
)
SELECT
    time,
    temp,
    CAST(GetRecordPropertyValue(SpikeAndDipScores, 'Score') AS float) AS
    SpikeAndDipScore,
    CAST(GetRecordPropertyValue(SpikeAndDipScores, 'IsAnomaly') AS bigint) AS
    IsSpikeAndDipAnomaly
INTO output
FROM AnomalyDetectionStep
```

## <a name="change-point"></a>Noktasını Değiştir

Zaman serisi olay akışı kalıcı anomalileri değerlerinin dağıtımındaki değişiklikler olay, düzey değişiklikleri ve eğilimleri gibi akışında ' dir. Stream Analytics'te dayalı Machine Learning'i kullanma gibi anomalileri algılanan **AnomalyDetection_ChangePoint** işleci.

Kalıcı değişiklikler çok süreden daha uzun ani artışlar ve düşüşler en son ve yıkıcı olaylar olduğunu gösteriyor olabilir. Kalıcı değişiklikler genellikle çıplak gözle kolayca görünür değildir, ancak ile algılanabilir **AnomalyDetection_ChangePoint** işleci.

Aşağıdaki resimde, bir düzey değişikliği örneğidir:

![Düzeyi değiştirme anomali örneği](./media/stream-analytics-machine-learning-anomaly-detection/anomaly-detection-level-change.png)

Aşağıdaki resimde eğilim değişiklik örneğidir:

![Eğilim değişiklik anomali örneği](./media/stream-analytics-machine-learning-anomaly-detection/anomaly-detection-trend-change.png)

Aşağıdaki örnek sorgu 1 olay bir 20 dakikalık kayan pencere 1200 olayların bir geçmiş boyutu ile saniye başına Tekdüzen giriş fiyatı varsayar. Son SELECT deyimi ayıklar ve puan ve anomali çıkaran durumu % 80'e güven düzeyine sahip.

```SQL
WITH AnomalyDetectionStep AS
(
    SELECT
        EVENTENQUEUEDUTCTIME AS time,
        CAST(temperature AS float) AS temp,
        AnomalyDetection_ChangePoint(CAST(temperature AS float), 80, 1200) 
        OVER(LIMIT DURATION(minute, 20)) AS ChangePointScores
    FROM input
)
SELECT
    time,
    temp,
    CAST(GetRecordPropertyValue(ChangePointScores, 'Score') AS float) AS
    ChangePointScore,
    CAST(GetRecordPropertyValue(ChangePointScores, 'IsAnomaly') AS bigint) AS
    IsChangePointAnomaly
INTO output
FROM AnomalyDetectionStep

```

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Stream analytics'e giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://msdn.microsoft.com/library/azure/dn834998.ASpx)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)

