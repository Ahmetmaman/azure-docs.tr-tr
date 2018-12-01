---
title: Azure Application ınsights HockeyApp verileri araştırma | Microsoft Docs
description: Kullanım ve Application Insights ile Azure uygulama performansını analiz edin.
services: application-insights
documentationcenter: windows
author: mrbullwinkle
manager: carmonm
ms.assetid: 97783cc6-67d6-465f-9926-cb9821f4176e
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 03/30/2017
ms.author: mbullwin
ms.openlocfilehash: 2e2343c958141032d91c9ea9ee22ed052ff43d60
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2018
ms.locfileid: "52728013"
---
# <a name="exploring-hockeyapp-data-in-application-insights"></a>Application ınsights'ta HockeyApp verileri araştırma

> [!NOTE]
> HockeyApp, artık yeni uygulamalar için kullanılabilir. HockeyApp var olan dağıtımlar çalışmaya devam eder. Visual Studio App Center artık önerilen Microsoft gelen yeni mobil uygulamaları izlemek için bir hizmettir. [App Center ve Application Insights ile uygulamalarınızı ayarlama hakkında bilgi edinin](app-insights-mobile-center-quickstart.md).

[HockeyApp](https://azure.microsoft.com/services/hockeyapp/) Canlı Masaüstü ve mobil uygulamaları izlemeye yönelik bir hizmettir. HockeyApp od özel gönderebilir ve kullanımını izlemek ve (kilitlenme verilerini alma) ek tanılama konusunda yardımcı olmak için telemetri izleme. Bu akış telemetri güçlü kullanarak sorgulanabilir [Analytics](app-insights-analytics.md) özelliği [Azure Application Insights](app-insights-overview.md). Ayrıca, aşağıdakileri yapabilirsiniz [özel dışarı aktarma ve izleme telemetri](app-insights-export-telemetry.md). Bu özellikleri etkinleştirmek için Application Insights için HockeyApp özel veri aktaran bir köprü ayarlayın.

## <a name="the-hockeyapp-bridge-app"></a>HockeyApp Köprüsü uygulaması
HockeyApp Köprüsü uygulaması özel HockeyApp ve Application ınsights izleme telemetrisi analiz ve sürekli dışarı aktarma özellikleri erişim sağlayan bir çekirdek özelliğidir. HockeyApp Köprüsü uygulaması oluşturulduktan sonra HockeyApp tarafından toplanan özel ve izleme olaylarını bu özelliklerden erişilebilir. Şimdi bu köprüsü uygulamalarından birini ayarlamayı öğrenin.

Hesap ayarları, HockeyApp içinde Aç [API belirteçleri](https://rink.hockeyapp.net/manage/auth_tokens). Yeni bir belirteç oluşturun veya mevcut bir yeniden. En düşük hakları, "salt okunur" gerekir. Bir kopyasını API belirteci alır.

![HockeyApp API belirteci alma](./media/app-insights-hockeyapp-bridge-app/01.png)

Microsoft Azure portal'ı açın ve [Application Insights kaynağı oluşturma](app-insights-create-new-resource.md). Uygulama türü "İçin HockeyApp Köprüsü uygulaması" olarak ayarlayın:

![Yeni Application Insights kaynağı](./media/app-insights-hockeyapp-bridge-app/02.png)

Bir ad ayarlayın gerekmez; bu HockeyApp adından otomatik olarak ayarlanır.

HockeyApp köprüsü alanlar görüntülenir. 

![Köprü alanlar girin](./media/app-insights-hockeyapp-bridge-app/03.png)

Daha önce not ettiğiniz HockeyApp belirteci girin. Bu eylem "HockeyApp uygulaması" açılan menüsünün HockeyApp tüm uygulamalarınız ile doldurur. Kullanmak istediğiniz birini seçin ve kalan alanları doldurun. 

Yeni kaynak açın. 

Veri akışını başlatmak için bir süre sürer.

![Application Insights kaynağı verileri bekleniyor](./media/app-insights-hockeyapp-bridge-app/04.png)

İşte bu kadar! HockeyApp belgelenmiş uygulamanızda bu noktadan itibaren toplanan özel ve izleme verileri artık Ayrıca, Application Insights analiz ve sürekli dışarı aktarma özellikleri kullanılabilir.

Kısa bir süre için kullanıma sunuldu bu özelliklerin her biri gözden geçirelim.

## <a name="analytics"></a>Analiz
Analytics, geçici tanılayın ve nedenlerini ve desenleri daha hızlı keşfedeceksiniz telemetrinizi analiz etmenize imkan sağlar, verileri sorgulamak için güçlü bir araçtır.

![Analiz](./media/app-insights-hockeyapp-bridge-app/05.png)

* [Analytics hakkında daha fazla bilgi edinin](../log-analytics/query-language/get-started-analytics-portal.md)

## <a name="continuous-export"></a>Sürekli dışarı aktarma
Sürekli dışarı aktarma, bir Azure Blob Depolama kapsayıcısına verilerinizi vermenize olanak sağlar. Verileriniz şu anda Application Insights tarafından sunulan saklama süresinden daha uzun süre tutmanız gerekiyorsa bu çok kullanışlıdır. Blob storage'da verileri tutmak, SQL veritabanı veya depolama çözümü, tercih edilen veri işleyin.

[Sürekli dışarı aktarma hakkında daha fazla bilgi edinin](app-insights-export-telemetry.md)

## <a name="next-steps"></a>Sonraki adımlar
* [Verilerinizi analiz uygulama](../log-analytics/query-language/get-started-analytics-portal.md)

