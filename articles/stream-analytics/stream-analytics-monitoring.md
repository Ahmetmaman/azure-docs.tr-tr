---
title: Azure Stream Analytics işi izlemeyi anlama
description: Bu makalede, Azure portalında Azure Stream Analytics işlerini izleme açıklar.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 12/07/2018
ms.custom: seodec18
ms.openlocfilehash: 656f749fd2a930c51bfd7d1a99642fae87694846
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53096636"
---
# <a name="understand-stream-analytics-job-monitoring-and-how-to-monitor-queries"></a>Stream Analytics işi izleme ve sorguları izleme anlama

## <a name="introduction-the-monitor-page"></a>Giriş: İzleme sayfası
Azure portal her ikisi de izleme ve sorgu ve iş performansınızı sorun giderme için kullanılan temel performans ölçümlerini yüzey. Bu ölçümler görmek için Gözat görmeniz için ölçümleri ilgilendiğiniz ve görüntülemek Stream Analytics işi **izleme** bölümüne genel bakış sayfasında.  

![Stream Analytics işini bağlantı izleme](./media/stream-analytics-monitoring/02-stream-analytics-monitoring-block.png)

Penceresinde gösterildiği gibi görünür:

![Stream Analytics işi izleme Panosu](./media/stream-analytics-monitoring/01-stream-analytics-monitoring.png)  

## <a name="metrics-available-for-stream-analytics"></a>Stream Analytics için mevcut olan ölçümler
| Ölçüm                 | Tanım                               |
| ---------------------- | ---------------------------------------- |
| Biriktirme Listesindeki Giriş Olayları       | Biriktirme listesindeki giriş olayların sayısı. |
| Veri Dönüştürme Hataları | Beklenen çıktıyı şemaya dönüştürülemiyor çıkış olaylarının sayısı. |
| Erken Giriş Olayları       | Erken alınan olay sayısı. |
| Başarısız İşlev İstekleri | Başarısız (varsa) Azure Machine Learning işlevi çağrı sayısı. |
| İşlev Olayları        | (Varsa) Azure Machine Learning işleve gönderilen olay sayısı. |
| İşlev İstekleri      | Azure Machine Learning işlevi (varsa) yapılan çağrı sayısı. |
| Giriş Serileştirme Kaldırma Hataları       | Seri durumdan çıkarılamadı olayların sayısı.  |
| Giriş Olayı Bayt Sayısı      | Stream Analytics işinde bayt tarafından alınan veri miktarı. Bu olaylar için giriş kaynağı gönderildiğini doğrulamak için kullanılabilir. |
| Giriş Olayları           | Stream Analytics işinde olay sayısı tarafından alınan veri miktarı. Bu olaylar için giriş kaynağı gönderildiğini doğrulamak için kullanılabilir. |
| Alınan Giriş Kaynakları       | Bir giriş kaynağından gelen olay sayısı. |
| Geç Giriş Olayları      | Geç varış toleransı penceresi ayarı olay sıralama İlkesi yapılandırmasına bağlı olarak, ya da bırakılmış olan kaynak ya da zaman geç gelen olay sayısı ayarlandı. |
| Sıra dışı olayları    | Bırakılan veya olay sıralama ilkesine göre ayarlanmış bir zaman damgası, verilen sıranın dışında alınan olay sayısı. Out of toleransı penceresi ayarının yapılandırmasını tarafından etkilenmiş. |
| Çıkış Olayları          | Çıkış hedefte olayların sayısı Stream Analytics işi tarafından gönderilen veri miktarı. |
| Çalışma Zamanı Hataları         | Sorgu işleme (başlayan kümeniz, olaylar veya sonuçlar outputing sırasında bulunan hataları hariç) ilişkili hatalarının toplam sayısı |
| SU Kullanım Yüzdesi       | Akış birimi kullanımına iş ölçek sekmesinden bir işe atanmış. Bu gösterge % 80 düzeyine ulaşması gerektiğini, yukarıdaki var veya olay işlenmesi gecikebilir veya ilerleme durduruldu yüksek olasılık. |
| Eşik Gecikmesi       | İşin tüm çıkışları tüm bölümleri arasında en yüksek eşik gecikmesi. |

## <a name="customizing-monitoring-in-the-azure-portal"></a>Azure portalında izleme özelleştirme
Grafikte gösterilen ölçümleri türünü ayarlamak ve zaman aralığı grafiği Düzenle ayarlarında. Ayrıntılar için bkz [özelleştirme izleme nasıl](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).

  ![Stream Analytics, izleme zaman grafiği sorgulama](./media/stream-analytics-monitoring/08-stream-analytics-monitoring.png)  


## <a name="latest-output"></a>Son çıkış
İşi izlemek için başka bir ilgi çekici veri noktası genel bakış sayfasında gösterilen son çıkış zamanı gelmiştir.
Bu uygulamanın süresi (Olay verileri itibaren zaman damgası kullanarak zaman)'dır işinin en son çıktı.

## <a name="get-help"></a>Yardım alın
Daha fazla yardım için [Azure Stream Analytics forumumuzu](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics) deneyin.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Stream analytics'e giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)

