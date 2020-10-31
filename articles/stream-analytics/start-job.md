---
title: Azure Stream Analytics işi başlatma
description: Bu makalede Azure portal, PowerShell ve Visual Studio 'dan bir Stream Analytics işinin nasıl başlatılacağı açıklanmaktadır.
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: how-to
ms.date: 04/03/2019
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: f89b23d0c20aafeeb07a744545208f6056efa2c9
ms.sourcegitcommit: 857859267e0820d0c555f5438dc415fc861d9a6b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93130825"
---
# <a name="how-to-start-an-azure-stream-analytics-job"></a>Azure Stream Analytics işi başlatma

Azure portal, Visual Studio ve PowerShell 'i kullanarak Azure Stream Analytics işinizi başlatabilirsiniz. Bir iş başlattığınızda, işin çıkış oluşturmaya başlaması için bir zaman seçersiniz. Azure portal, Visual Studio ve PowerShell 'in her birinin başlangıç saatini ayarlamak için farklı yöntemleri vardır. Bu yöntemler aşağıda açıklanmıştır.

## <a name="start-options"></a>Başlatma seçenekleri
Bir işi başlatmak için aşağıdaki üç seçenek mevcuttur. Aşağıda bahsedilen tüm saatlerin, [zaman damgasında](/stream-analytics-query/timestamp-by-azure-stream-analytics)belirtilen tüm zamanların olduğunu unutmayın. ZAMAN DAMGASı belirtilmemişse, varış süresi kullanılacaktır.
* **Şimdi** : çıkış olayı akışının başlangıç noktasını, iş başladığında olduğu gibi yapar. Zamana bağlı bir operatör kullanılıyorsa (örneğin, zaman penceresi, GECIKME veya JOIN), Azure Stream Analytics giriş kaynağındaki verilere otomatik olarak geri bakar. Örneğin, bir işi "Şimdi" başlatırsanız ve sorgunuz 5 dakikalık bir pencere penceresi kullanıyorsa, Azure Stream Analytics girişte 5 dakikadan önce verileri arar.
İlk olası çıkış olayında, geçerli zamandan eşit veya ondan büyük bir zaman damgası olabilir ve ASA, çıkışa mantıksal olarak katkıda bulunan tüm giriş olaylarının hesaba katılmış olmasını garanti eder. Örneğin, kısmi pencereli toplamalar oluşturulmaz. Her zaman tamamen toplanan değer.

* **Özel** : çıktının başlangıç noktasını seçebilirsiniz. Aynı şekilde, **isteğe bağlı** bir operatör kullanılırsa Azure Stream Analytics, bu andan önce gelen verileri otomatik olarak okuyacaktır. 

* **Son durdurulduğunda** . Bu seçenek, iş daha önce başlatıldığında kullanılabilir ancak el ile veya başarısız olarak durdurulur. Bu seçeneği seçerken Azure Stream Analytics, bir veri kaybolmayacak şekilde işi yeniden başlatmak için son çıkış süresini kullanır. Önceki seçeneklere benzer şekilde, Azure Stream Analytics zamana bağlı bir operatör kullanılıyorsa, bu zamandan önce verileri otomatik olarak okur. Birkaç giriş bölümünün saati farklı olabileceğinden, tüm bölümlerin en erken durma saati kullanılır, bu nedenle çıktıda bazı yinelemeler görülebilir. Tam bir kez işleme hakkında daha fazla bilgi için, [olay teslim garantisi](/stream-analytics-query/event-delivery-guarantees-azure-stream-analytics)sayfasında kullanılabilir.


## <a name="azure-portal"></a>Azure portal

Azure portal işinize gidin ve genel bakış sayfasında **Başlat** ' ı seçin. Bir **iş çıkışı başlangıç zamanı** seçin ve ardından **Başlat** ' ı seçin.

**İş çıkışı başlangıç zamanı** seçeneklerinden birini belirleyin. Seçenekler *artık* *son durdurulduğunda* , *özel* ve ve iş daha önce çalıştırıldıysa. Bu seçenekler hakkında daha fazla bilgi için yukarıya bakın.

## <a name="visual-studio"></a>Visual Studio

İş görünümünde, işi başlatmak için yeşil ok düğmesini seçin. **Iş çıkışı başlangıç modunu** ayarlayın ve **Başlat** ' ı seçin. İş durumu **çalışmaya** çalışacak şekilde değişir.

**Iş çıkışı başlangıç modu** için üç seçenek vardır: *jobstarttime* , *Customtime* ve *lastoutputeventtime* . Bu özellik yoksa, varsayılan olarak *Jobstarttime* olur. Bu seçenekler hakkında daha fazla bilgi için yukarıya bakın.


## <a name="powershell"></a>PowerShell

PowerShell kullanarak işinizi başlatmak için aşağıdaki cmdlet 'i kullanın:

```powershell
Start-AzStreamAnalyticsJob `
  -ResourceGroupName $resourceGroup `
  -Name $jobName `
  -OutputStartMode 'JobStartTime'
```

**Outputstartmode** için üç seçenek vardır: *jobstarttime* , *Customtime* ve *lastoutputeventtime* . Bu özellik yoksa, varsayılan olarak *Jobstarttime* olur. Bu seçenekler hakkında daha fazla bilgi için yukarıya bakın.

Cmdlet hakkında daha fazla bilgi için `Start-AzStreamAnalyitcsJob` [Start-AzStreamAnalyticsJob başvurusunu](/powershell/module/az.streamanalytics/start-azstreamanalyticsjob)görüntüleyin.

## <a name="next-steps"></a>Sonraki adımlar

* [Hızlı başlangıç: Azure portalını kullanarak Stream Analytics işi oluşturma](stream-analytics-quick-create-portal.md)
* [Hızlı başlangıç: Azure PowerShell kullanarak Stream Analytics işi oluşturma](stream-analytics-quick-create-powershell.md)
* [Hızlı başlangıç: Visual Studio için Azure Stream Analytics araçlarını kullanarak bir Stream Analytics işi oluşturma](stream-analytics-quick-create-vs.md)