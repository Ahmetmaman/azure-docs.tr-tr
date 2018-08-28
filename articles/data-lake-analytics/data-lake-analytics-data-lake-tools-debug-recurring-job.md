---
title: Azure Data Lake Analytics yinelenen işlerinde hata ayıklama
description: Olağan dışı bir yinelenen iş hata ayıklamak için Visual Studio için Azure Data Lake Araçları'nı kullanmayı öğrenin.
services: data-lake-analytics
author: yanancai
ms.author: yanacai
ms.reviewer: jasonwhowell
ms.assetid: dc9b21d8-c5f4-4f77-bcbc-eff458f48de2
ms.service: data-lake-analytics
ms.topic: conceptual
ms.date: 05/20/2018
ms.openlocfilehash: 33c3b91e7bf9fa64e3ba3f98a9396045753d0c2a
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "43045703"
---
# <a name="troubleshoot-an-abnormal-recurring-job"></a>Olağan dışı bir yinelenen iş sorunlarını giderme

Bu makalede nasıl kullanılacağını gösterir [Visual Studio için Azure Data Lake Araçları](http://aka.ms/adltoolsvs) yinelenen işleri ile ilgili sorunları gidermek için. Ardışık düzenin ve gelen yinelenen işleri hakkında daha fazla bilgi [Azure Data Lake ve Azure HDInsight blogu](https://blogs.msdn.microsoft.com/azuredatalake/2017/09/19/managing-pipeline-recurring-jobs-in-azure-data-lake-analytics-made-easy/).

Yinelenen işleri genellikle aynı sorgu mantığının ve benzer bir giriş veri paylaşın. Örneğin, her Pazartesi sabahı 8'de çalışan yinelenen bir iş olduğunu düşünün. Geçen haftaki haftalık etkin kullanıcı sayısı için. Bu işlerin betikleri sorgu mantığı içeren bir betik şablon paylaşın. Bu işlerin girişleri geçen hafta için kullanım verilerdir. Genellikle aynı sorgu mantığı ve benzer bir giriş paylaşımı performans bu işlerin benzer ve kararlı olduğunu gösterir. Yinelenen işlerinizi birine aniden olağan dışı, başarısız, çok yavaşlatır gerçekleştirirse, isteyebilirsiniz:

- Önceki çalıştırmaları ne olduğunu görmek için yinelenen işin istatistikleri raporların bakın.
- Karşılaştırma olağan dışı iş ne yaptığını anlamak için normal bir tane ile değiştirildi.

**İş görünümü ilgili** yardımcı Visual Studio için Azure Data Lake araçları içinde her iki durumda ile sorun giderme ilerleme hızlandırın.

## <a name="step-1-find-recurring-jobs-and-open-related-job-view"></a>1. adım: yinelenen işleri bulmak ve ilgili iş görünümünde Aç

Yinelenen bir iş sorununu gidermek için ilgili iş görünümü kullanmak için önce yinelenen iş Visual Studio'da bulun ve ardından ilgili iş görünümünü açmak gerekir.

### <a name="case-1-you-have-the-url-for-the-recurring-job"></a>1. durum: Yinelenen iş URL'si sahip

Aracılığıyla **Araçları** > **Data Lake** > **iş görünümü**, Visual Studio'da iş görünümünü açmak için iş URL'yi yapıştırabilirsiniz. Seçin **ilgili işleri görüntüle** ilgili iş görünümünü açmak için.

![Data Lake Analytics araçları, görünüm ilgili işleri bağlantısı](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/view-related-job.png)
 
### <a name="case-2-you-have-the-pipeline-for-the-recurring-job-but-not-the-url"></a>Durum 2: Yinelenen iş, ancak URL için işlem hattı sahip

Visual Studio'da Sunucu Gezgini aracılığıyla ardışık düzenli tarayıcı açın > Azure Data Lake Analytics hesabınızı > **işlem hatları**. (Sunucu Gezgini'nde, bu düğüm bulamazsanız [son eklentinizi indirin](http://aka.ms/adltoolsvs).) 

![İşlem hatları düğümü seçildiğinde](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/pipeline-browser.png)

İşlem hattı tarayıcıda Data Lake Analytics hesabı için tüm işlem hatları sol altında listelenir. Tüm yinelenen işleri bulmak için işlem hatlarını genişletin ve ardından sorun var olanı seçin. İlgili iş görünümü sağ tarafta açılır.

![Bir işlem hattı seçerek ve ilişkili iş görünümünü açma](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/recurring-job-view.png)

## <a name="step-2-analyze-a-statistics-report"></a>2. adım: bir istatistik raporunu analiz etme

İlgili iş görünümü üst kısmında bir özeti ve istatistikleri raporu gösterilir. Burada, olası sorunun kök nedenini bulabilirsiniz. 

1.  Raporda x ekseni iş gönderme zamanı gösterir. Olağan dışı iş bulmak için kullanın.
2.  İşlem Aşağıdaki diyagramda istatistikleri kontrol edin ve sorun ve olası çözümleri hakkında Öngörüler elde için kullanın.

![İstatistiklerine için işlem diyagramı](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/recurring-job-metrics-debugging-flow.png)

## <a name="step-3-compare-the-abnormal-job-to-a-normal-job"></a>3. adım: normal bir iş olağan dışı iş karşılaştırma

Tüm ilgili iş görünümü altındaki proje listesi üzerinden yinelenen işleri gönderilen bulabilirsiniz. Daha fazla Öngörüler ve olası çözümler bulmak için olağan dışı iş sağ tıklayın. Olağan dışı iş normal bir öncekine ile karşılaştırmak için iş farkı görünümünü kullanın.

![İşleri karşılaştırma için kısayol menüsü](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/compare-job.png)

Bu iki işleri arasında büyük farklar dikkat edin. Bu farklar, muhtemelen performans sorunlarına neden oluyor. Daha fazla denetlemek için aşağıdaki diyagramda adımları kullanın:

![İşleri arasındaki farklar denetleme işlemi diyagramı](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/recurring-job-diff-debugging-flow.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Veri dengesizliği sorunları çözün](data-lake-analytics-data-lake-tools-data-skew-solutions.md)
* [Başarısız olan U-SQL işleri için kullanıcı tanımlı C# kodu hatalarını ayıklama](data-lake-analytics-debug-u-sql-jobs.md)
