---
title: Visual Studio 'da iş diyagramını kullanarak Azure Stream Analytics sorguları yerel olarak ayıklayın
description: Bu makalede, Visual Studio için Azure Stream Analytics Araçları ' nda iş diyagramı kullanarak sorguların yerel olarak nasıl hata ayıklaması yapılacağı açıklanır.
author: su-jie
ms.author: sujie
ms.service: stream-analytics
ms.topic: how-to
ms.date: 01/23/2020
ms.openlocfilehash: d0e94fda1fb21be1a01516f4cecf657426ae867e
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "98019457"
---
# <a name="debug-azure-stream-analytics-queries-locally-using-job-diagram-in-visual-studio"></a>Visual Studio 'da iş diyagramını kullanarak Azure Stream Analytics sorguları yerel olarak ayıklayın

Çıkış olmayan ya da beklenmeyen sonuçlara neden olan işler, akış sorguları için ortak sorun giderme senaryolarından oluşur. Sorgunuzu Visual Studio 'da yerel olarak test ederken iş diyagramını kullanarak her adımın ara sonuç kümesini ve ölçümlerini inceleyebilirsiniz. İş diyagramları sorunları giderirken bir sorunun kaynağını hızlı bir şekilde ayırmanıza yardımcı olabilir.

## <a name="debug-a-query-using-job-diagram"></a>İş diyagramı kullanarak bir sorgu hatalarını ayıklama

Giriş verilerini çıktı verilerine dönüştürmek için bir Azure Stream Analytics betiği kullanılır. İş diyagramı, verilerin giriş kaynaklarından (Olay Hub 'ı, IoT Hub, vb.) birden çok sorgu adımı ve son olarak çıkış havuzları aracılığıyla nasıl akacağını gösterir. Her sorgu adımı, bir ifade kullanılarak betikte tanımlanan geçici bir sonuç kümesiyle eşlenir `WITH` . Bir sorunun kaynağını bulmak için her bir ara sonuç kümesindeki her bir sorgu adımındaki ölçümleri ve verileri görüntüleyebilirsiniz.

> [!NOTE]
> Bu iş diyagramı yalnızca tek bir düğümdeki yerel test için verileri ve ölçümleri gösterir. Performans ayarlama ve sorun giderme için kullanılmamalıdır.

### <a name="start-local-testing"></a>Yerel Testi Başlat

Visual Studio kullanarak Stream Analytics işi oluşturmayı veya [var olan bir işi yerel bir projeye aktarmayı](stream-analytics-vs-tools.md#export-jobs-to-a-project)öğrenmek Için bu [hızlı](stream-analytics-quick-create-vs.md) başlangıcı kullanın. Sorguyu yerel giriş verileriyle test etmek istiyorsanız, bu [yönergeleri](stream-analytics-live-data-local-testing.md)izleyin. Canlı giriş ile test etmek istiyorsanız bir sonraki adıma geçin.

> [!NOTE]
> Bir işi yerel projeye dışa aktarıp canlı bir giriş akışına karşı test etmek istiyorsanız, tüm girişlerin kimlik bilgilerini tekrar belirtmeniz gerekir.  

Betik düzenleyicisinden giriş ve çıkış kaynağını seçin ve **yerel olarak çalıştır**' ı seçin. İş Diyagramı sağ tarafta görüntülenir.

### <a name="view-the-intermediate-result-set"></a>Ara sonuç kümesini görüntüleme  

1. Betiğe gitmek için sorgu adımını seçin. Sol taraftaki düzenleyicide ilgili betiğe otomatik olarak yönlendirilirsiniz.

   ![İş diyagramı git betiği](./media/debug-locally-using-job-diagram/navigate-script.png)

2. Sorgu adımı ' nı seçin ve açılan iletişim kutusunda **Önizleme** ' yi seçin. Sonuç kümesi, alt Sonuç penceresindeki bir sekmede gösterilir.

   ![İş diyagramı önizleme sonucu](./media/debug-locally-using-job-diagram/preview-result.png)

### <a name="view-step-metrics"></a>Adım ölçümlerini görüntüle

Bu bölümde, diyagramın her bir bölümü için kullanılabilen ölçümleri keşfedebilirsiniz.

#### <a name="input-sources-live-stream"></a>Giriş kaynakları (canlı akış)

![İş diyagramı canlı giriş kaynakları](./media/debug-locally-using-job-diagram/live-input.png)

|Metric|Açıklama|
|-|-|
|**Taxırıde**| Girişin adı.|
|**Olay Hub’ı** | Giriş kaynağı türü.|
|**Ekinlikler**|Okunan olay sayısı.|
|**Biriktirme listesindeki olay kaynakları**|Event Hubs ve IoT Hub girişleri için kaç tane daha fazla ileti okunması gerekiyor.|
|**Bayt cinsinden olaylar**|Okunan bayt sayısı.|
| **Düşürülmüş olaylar**|Seri durumdan çıkarma dışında bir sorunu olan olay sayısı.|
|**Erken olaylar**| Yüksek filigrandan önce uygulama zaman damgasına sahip olay sayısı.|
|**Geç olaylar**| Yüksek filigrandan sonra uygulama zaman damgasına sahip olay sayısı.|
|**Olay Kaynakları**| Okunan veri birimi sayısı. Örneğin, Blobların sayısı.|

#### <a name="input-sources-local-input"></a>Giriş kaynakları (yerel giriş)

![İş diyagramı yerel giriş kaynakları](./media/debug-locally-using-job-diagram/local-input.png)

|Metric|Açıklama|
|-|-|
|**Taxırıde**| Girişin adı.|
|**Satır Sayısı**| Adımdan oluşturulan satır sayısı.|
|**Veri Boyutu**| Bu adımdan oluşturulan verilerin boyutu.|
|**Yerel giriş**| Yerel verileri giriş olarak kullanın.|

#### <a name="query-steps"></a>Sorgu adımları

![İş diyagramı sorgu adımı](./media/debug-locally-using-job-diagram/query-step.png)

|Metric|Açıklama|
|-|-|
|**TripData**|Geçici sonuç kümesinin adı.|
|**Satır Sayısı**| Adımdan oluşturulan satır sayısı.|
|**Veri Boyutu**| Bu adımdan oluşturulan verilerin boyutu.|
  
#### <a name="output-sinks-live-output"></a>Çıkış havuzları (canlı çıkış)

![Yerel çıkış havuzlarını gösteren iş diyagramı.](./media/debug-locally-using-job-diagram/live-output.png)

|Metric|Açıklama|
|-|-|
|**regionaggEH**|Çıkışın adı.|
|**Ekinlikler**|Havuza çıkış yapılacak olay sayısı.|

#### <a name="output-sinks-local-output"></a>Çıkış havuzları (yerel çıkış)

![İş diyagramı yerel çıkış havuzları](./media/debug-locally-using-job-diagram/local-output.png)

|Metric|Açıklama|
|-|-|
|**regionaggEH**|Çıkışın adı.|
|**Yerel çıkış**| Yerel bir dosyaya giden sonuç çıktısı.|
|**Satır Sayısı**| Yerel dosyaya giden satır sayısı.|
|**Veri Boyutu**| Yerel dosyanın veri çıktısının boyutu.|

### <a name="close-job-diagram"></a>İş diyagramını kapat

Artık iş diyagramına ihtiyacınız yoksa sağ üst köşedeki **Kapat** ' ı seçin. Diyagram penceresini kapattıktan sonra, görüntülemek için yerel testi yeniden başlatmanız gerekir.

### <a name="view-job-level-metrics-and-stop-running"></a>İş düzeyi ölçümlerini görüntüle ve çalışmayı durdur

Diğer iş düzeyi ölçümleri, açılan konsolda görüntülenir. İşi durdurmak istiyorsanız konsolda **CTRL + C** tuşlarına basın.

![İş diyagramı işi durdur](./media/debug-locally-using-job-diagram/stop-job.png)

## <a name="limitations"></a>Sınırlamalar

* Power BI ve Azure Data Lake Storage 1. çıkış havuzları, kimlik doğrulama modeli sınırlamaları nedeniyle desteklenmez.

* Yalnızca bulut girişi seçeneklerinde [zaman ilkeleri](./stream-analytics-time-handling.md) desteklenir, ancak yerel giriş seçenekleri değildir.

## <a name="next-steps"></a>Sonraki adımlar

* [Hızlı başlangıç: Visual Studio 'Yu kullanarak Stream Analytics işi oluşturma](stream-analytics-quick-create-vs.md)
* [Azure Stream Analytics işleri görüntülemek için Visual Studio 'Yu kullanma](stream-analytics-vs-tools.md)
* [Visual Studio için Azure Stream Analytics araçları 'nı kullanarak canlı verileri yerel olarak test etme (Önizleme)](stream-analytics-live-data-local-testing.md)