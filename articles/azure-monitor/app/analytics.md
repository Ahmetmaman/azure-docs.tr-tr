---
title: Analytics - Azure Application ınsights sorgu aracı ve güçlü arama | Microsoft Docs
description: "Analytics, Application Insights'ın güçlü tanılama arama aracında genel bakış. "
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 0a2f6011-5bcf-47b7-8450-40f284274b24
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 02/08/2018
ms.author: mbullwin
ms.openlocfilehash: c41444f94e4685d246de225500c8a5beefc74944
ms.sourcegitcommit: 3ab534773c4decd755c1e433b89a15f7634e088a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/07/2019
ms.locfileid: "54065622"
---
# <a name="analytics-in-application-insights"></a>Application Insights analiz
Analytics, güçlü arama ve sorgulama aracı [Application Insights](../../application-insights/app-insights-overview.md). Analytics web aracı olduğundan kurulum gerekli değildir. Application Insights uygulamalarınızı biri için yapılandırmış sonra uygulamanızın Analytics açarak uygulamanızın verilerini çözümleyebilirsiniz [genel bakış dikey penceresinde](../../azure-monitor/app/app-insights-dashboards.md).

![Portal.Azure.com açın, Application Insights kaynağınızı açın ve analiz tıklayın.](./media/analytics/001.png)

Ayrıca [Analytics playground](https://go.microsoft.com/fwlink/?linkid=859557) birçok örnek veri ücretsiz tanıtım ortamıyla olduğu.
<br>
<br>
> [!VIDEO https://channel9.msdn.com/events/Connect/2016/123/player] 

## <a name="query-data-in-analytics"></a>Analytics'te sorgu verileri
Tipik bir sorgu bir dizi tarafından izlenen bir tablo adı ile başlayan *işleçleri* ayırarak `|`.
Örneğin, şimdi kaç isteklerini uygulamamız sırasında son 3 saat farklı ülkelerden alınan bulun:
```AIQL
requests
| where timestamp > ago(3h)
| summarize count() by client_CountryOrRegion
| render piechart
```

Biz tablo adıyla başlayamaz *istekleri* gerektiğinde yöneltilen öğeleri ekleyin.  Önce yalnızca son 3 saat kayıtları gözden geçirmek için zaman filtresi tanımlayın.
Biz sonra ülke başına kayıt sayısı (veri sütunu bulunduğunda *client_CountryOrRegion*). Son olarak, bir pasta grafiğinin sonuçları işleyin.
<br>

![Sorgu sonuçları](./media/analytics/030.png)

Dil, çok cazip özelliklere sahiptir:

* [Filtre](/azure/kusto/query/whereoperator) ham uygulama telemetrinizi özel özellikler ve ölçümler de dahil olmak üzere herhangi bir alana göre.
* [Birleştirme](/azure/kusto/query/joinoperator) birden çok tablolar - karşılıklı olarak ilişkilendirmek sayfa görüntülemeleri, bağımlılık çağrıları, özel durumlar ve günlük izlemelerini ister.
* Güçlü istatistiksel [toplamalar](/azure/kusto/query/summarizeoperator).
* Hemen ve güçlü görselleştirmeler.
* [REST API](https://dev.applicationinsights.io/) sorguları programlı olarak örneğin Powershell'den çalıştırmak için kullanabilirsiniz.

[Tam dil başvurusu](https://go.microsoft.com/fwlink/?linkid=856079) desteklenen her bir komutun ayrıntılı ve düzenli olarak güncelleştirmektedir.

## <a name="next-steps"></a>Sonraki adımlar
* Kullanmaya başlama [analiz portalı](https://go.microsoft.com/fwlink/?linkid=856587)
* Başlama [sorgu yazma](https://go.microsoft.com/fwlink/?linkid=856078)
* Gözden geçirme [SQL-kullanıcıların kural sayfası](https://aka.ms/sql-analytics) en yaygın deyimleri çevirileri için.
* Üzerinde test sürüşü Analytics bizim [playground](https://analytics.applicationinsights.io/demo) uygulamanızın verilerini Application Insights'a henüz değil gönderiliyorsa.
* İzleme [tanıtım videosunu](https://applicationanalytics-media.azureedge.net/home_page_video.mp4).
