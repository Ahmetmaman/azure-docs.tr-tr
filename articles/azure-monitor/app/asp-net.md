---
title: Azure Application Insights ile ASP.NET Web uygulaması analizi ayarlama | Microsoft Docs
description: Performans, kullanılabilirlik ve kullanıcı davranış analizi araçları, ASP.NET Web sitesi için yapılandırma şirket içinde barındırılan veya Azure'da.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: d0eee3c0-b328-448f-8123-f478052751db
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 01/19/2018
ms.author: mbullwin
ms.openlocfilehash: f41d54f97861a4df7d50cb3b4f0f99970de354cc
ms.sourcegitcommit: 818d3e89821d101406c3fe68e0e6efa8907072e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2019
ms.locfileid: "54121507"
---
# <a name="set-up-application-insights-for-your-aspnet-website"></a>ASP.NET web siteniz için Application Insights'ı ayarlama

Bu yordam ASP.NET web uygulamanızı [Azure Application Insights](../../azure-monitor/app/app-insights-overview.md) hizmetine telemetri gönderecek şekilde yapılandırır. Kendi IIS sunucunuzda şirket içi olarak veya Bulut’ta barındırılan ASP.NET uygulamaları için çalışır. Uygulamanızın performansını ve nasıl kullanıldığını anlamanıza yardımcı olan grafikler ve güçlü bir sorgu dilinin yanı sıra hata ya da performans sorunları hakkında otomatik uyarılar alırsınız. Çoğu geliştirici, özellikleri bu haliyle mükemmel bulsa da, gerekirse telemetriyi genişletip özelleştirebilirsiniz.

Visual Studio'da kurulum yalnızca birkaç tıklama ile yapılır. Telemetri hacmini sınırlayarak ücret doğmamasını sağlayabilirsiniz. Bunun yapılması, çok fazla kullanıcı olmadan bir siteyi deneyip hatalarını ayıklamanıza veya izlemenize olanak tanır. Devam edip üretim merkezinizi izlemeye karar verdiğinizde, sınırı daha sonra kolayca artırabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar
Application Insights’ı ASP.NET web sitenize eklemek için şunu yapmanız gerekir:

- [Windows için Visual Studio 2017](https://www.visualstudio.com/downloads/)’yi aşağıdaki iş yükleri ile yükleyin:
    - ASP.NET ve web geliştirme
    - Azure geliştirme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="ide"></a> 1. adım: Application Insights SDK ekleme

> [!IMPORTANT]
> Application Insights ekleme işlemi, ASP.NET şablon türüne göre değişiklik gösterir. **Boş** bir şablon veya **Azure Mobil Uygulaması** şablonu kullanıyorsanız **Proje** > **Application Insights Telemetri Ekleme** seçeneğini belirleyin. Diğer tüm ASP.NET şablonları için aşağıdaki yönergelere başvurun. 

Çözüm Gezgini’nde web uygulamanızın adına sağ tıklayın ve **Application Insights’ı Yapılandır** seçeneğini belirleyin

![Application Insights’ı Yapılandır seçeneğinin vurgulandığı Çözüm Gezgini’nin ekran görüntüsü](./media/asp-net/0001-configure-application-insights.png)

(Application Insights SDK sürümünüze bağlı olarak, son SDK sürümüne güncelleştirme yapmanız istenebilir. İstenirse **SDK’yı güncelleştir**’i seçin.)

![Ekran: Microsoft Application Insights SDK'ın yeni bir sürümü kullanılabilir. SDK’yı güncelleştir seçeneğinin vurgulandığı görüntü](./media/asp-net/0002-update-sdk.png)

Application Insights Yapılandırması ekranı:

**Ücretsiz Olarak Başla**’yı seçin.

![Uygulamanızı Application Insights'a kaydedin sayfasının ekran görüntüsü](./media/asp-net/0004-start-free.png)

Verilerin depolandığı kaynak grubunu veya konumu ayarlamak isterseniz **Ayarları yapılandır**'a tıklayın. Kaynak grupları, verilere erişimi denetlemek için kullanılır. Örneğin aynı sistemin parçalarını oluşturan birden uygulamanız varsa bunların Application Insights verilerini aynı kaynak grubuna ekleyebilirsiniz.

 **Kaydol**’u seçin. 

![Uygulamanızı Application Insights'a kaydedin sayfasının ekran görüntüsü](./media/asp-net/0005-register-ed.png)

 Telemetri hem hata ayıklama sırasında hem de uygulamanızı yayımladıktan sonra [Azure portalına](https://portal.azure.com) gönderilir.
> [!NOTE]
> Hata ayıklama sırasında portala telemetri göndermek istemiyorsanız, uygulamanıza Application Insights SDK’sını ekleyin, ancak portalda bir kaynak yapılandırmayın. Hata ayıklama sırasında telemetri verilerini Visual Studio'da görebilirsiniz. Daha sonra bu yapılandırma sayfasına dönebilir veya uygulamanızı dağıtana kadar bekleyip [telemetriyi çalışma zamanında açabilirsiniz](../../azure-monitor/app/monitor-performance-live-website-now.md).

## <a name="run"></a> 2. adım: Uygulamanızı çalıştırma
F5 tuşuna basarak uygulamanızı çalıştırın. Farklı sayfalar açarak telemetri verileri oluşturun.

Visual Studio'da günlüğe kaydedilmiş etkinliklerin sayısını görürsünüz.

![Visual Studio’nun ekran görüntüsü. Hata ayıklama sırasında Application Insights düğmesi görünür.](./media/asp-net/0006-Events.png)

## <a name="step-3-see-your-telemetry"></a>3. Adım: Telemetrinize bakma
Visual Studio’da veya Application Insights web portalında telemetrinizi görebilirsiniz. Uygulamanızın hatalarını ayıklamanıza yardımcı olması için Visual Studio'da telemetri arayın. Sisteminiz canlıyken web portalında performans ve kullanımı izleyin. 

### <a name="see-your-telemetry-in-visual-studio"></a>Visual Studio'da telemetrinize bakma

Visual Studio’da Application Insights verilerini görüntülemek için şunları yapın.  **Çözüm Gezgini** > **Bağlı Hizmetler**’i seçin > **Application Insights**’a sağ tıklayın ve ardından **Canlı Telemetri Ara**’ya tıklayın.

Visual Studio Application Insights Arama penceresinde, uygulamanızın sunucu tarafında oluşturulan telemetri için uygulamanızdan alınan veriler görünümü açılır. Filtrelerle denemeler yapın ve daha fazla ayrıntı için herhangi bir etkinliğe tıklayın.

![Application Insights penceresindeki Hata ayıklama oturumundan alınan veriler görünümünün ekran görüntüsü.](./media/asp-net/55.png)

> [!Tip]
> Herhangi bir veri gösterilmiyorsa zaman aralığının doğru olduğundan emin olup Ara simgesine tıklayın.

[Visual Studio’daki Application Insights araçları hakkında daha fazla bilgi edinin](../../azure-monitor/app/visual-studio.md).

<a name="monitor"></a>
### <a name="see-telemetry-in-web-portal"></a>Web portalında telemetriye bakma

Yalnızca SDK’yı yüklemeyi seçmediyseniz telemetriyi Application Insights web portalında da görüntüleyebilirsiniz. Portalda, Visual Studio’ya kıyasla daha çok grafik, analiz aracı ve bileşenler arası görünüm bulunur. Portal ayrıca uyarılar sağlar.

Application Insights kaynağınızı açın. [Azure portalında](https://portal.azure.com/) oturum açarak aradığınız öğeyi orada bulabilir veya **Çözüm Gezgini** > **Bağlı Hizmetler**’i seçip > **Application Insights** > **Application Insights Portal’ı aç**’a sağ tıklayarak sayfaya yönlendirilebilirsiniz.

Portal, uygulamanızdan alınan telemetri görünümünde açılır.

![Application Insights’a genel bakış sayfasının ekran görüntüsü](./media/asp-net/66.png)

Daha fazla ayrıntı görmek için portalda istediğiniz kutucuğa veya grafiğe tıklayın.

[Azure portalında Application Insights kullanma hakkında daha fazla bilgi edinin](../../azure-monitor/app/app-insights-dashboards.md).

## <a name="step-4-publish-your-app"></a>4. Adım: Uygulamanızı yayımlama
Uygulamanızı IIS sunucunuza veya Azure’a yayımlayın. Her şeyin sorunsuz çalıştığından emin olmak için [Canlı Ölçümler Akışı](../../azure-monitor/app/metrics-explorer.md#live-metrics-stream)’nı izleyin.

Telemetriniz Application Insights portalında biriktirilir ve burada ölçümlerinizi izleyebilir, telemetrinizde arama yapabilir ve [panolar](../../azure-monitor/app/app-insights-dashboards.md) ayarlayabilirsiniz. Ayrıca, güçlü [Log Analytics sorgu dili](https://aka.ms/LogAnalyticsLanguage)’ni kullanarak kullanımı ve performansı analiz edebilir ya da belirli olayları bulabilirsiniz.

Telemetrinizi tanılama araması ve [eğilimler](../../azure-monitor/app/visual-studio-trends.md) gibi araçlarla [Visual Studio](../../azure-monitor/app/visual-studio.md)’da analiz etmeye de devam edebilirsiniz.

> [!NOTE]
> Uygulamanız [azaltma sınırlarına](../../azure-monitor/app/pricing.md#limits-summary) yaklaşmak için yeterli telemetri gönderiyorsa, otomatik [örnekleme](../../azure-monitor/app/sampling.md) etkinleştirilir. Örnekleme, tanılama amaçlı bağlantı verilerini korurken uygulamanızdan gönderilen telemetri miktarını azaltır.
>
>

## <a name="land"></a> İşiniz tamamlandı

Tebrikler! Application Insights paketini uygulamanıza yüklediniz ve Azure üzerinde Application Insights hizmetine telemetri gönderecek şekilde yapılandırdınız.

![Telemetri hareketlerinin diyagramı](./media/asp-net/01-scheme.png)

Uygulamanızın telemetrisini alan Azure kaynağı bir *izleme anahtarı* ile tanımlanır. Bu anahtarı ApplicationInsights.config dosyasında bulabilirsiniz.


## <a name="upgrade-to-future-sdk-versions"></a>Gelecek SDK sürümlerine yükseltme
[SDK’nın yeni bir sürümüne](https://github.com/Microsoft/ApplicationInsights-dotnet-server/releases) yükseltme yapmak için, **NuGet paket yöneticisini** açıp yüklü paketleri filtreleyin. **Microsoft.ApplicationInsights.Web**’i seçip **Yükselt**’i seçin.

ApplicationInsights.config’de herhangi bir özelleştirme gerçekleştirdiyseniz yükseltmeden önce dosyanın bir kopyasını kaydedin. Daha sonra, yaptığınız değişiklikleri yeni sürümle birleştirin.

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a>Sonraki adımlar

İlginizi çekiyorsa inceleyebileceğiniz alternatif konu başlıkları da mevcuttur:

* [Çalışma zamanında bir web uygulamasını izleme](../../azure-monitor/app/monitor-performance-live-website-now.md)
* [Azure Cloud Services](../../azure-monitor/app/cloudservices.md)

### <a name="more-telemetry"></a>Daha fazla telemetri

* **[Tarayıcı ve sayfa yükleme verileri](../../azure-monitor/app/javascript.md)** - Web sayfalarınıza bir kod parçacığı ekleyin.
* **[Daha ayrıntılı bağımlılık ve özel durum izlemesi alın](../../azure-monitor/app/monitor-performance-live-website-now.md)** - Sunucunuza Durum İzleyicisi yükleyin.
* Kullanıcı eylemlerini saymak, zamanlamak veya ölçmek için **[özel olaylar kodlayın](../../azure-monitor/app/api-custom-events-metrics.md)**.
* **[Günlük verilerini alma](../../azure-monitor/app/asp-net-trace-logs.md)** - Günlük verilerini telemetrinizle ilişkilendirin.

### <a name="analysis"></a>Analiz

* **[Visual Studio’da Application Insights ile çalışma](../../azure-monitor/app/visual-studio.md)**<br/>Telemetri, tanılama araması ve kodun detayına gitme ile hata ayıklama hakkında bilgi içerir.
* **[Application Insights portalıyla çalışma](../../azure-monitor/app/app-insights-dashboards.md)**<br/> Panolar, güçlü tanılama ve analiz araçları, uyarılar, uygulamanızın canlı bağımlılık haritası ve telemetriyi dışarı aktarma hakkında bilgi içerir.
* **[Analytics](../../azure-monitor/log-query/get-started-portal.md)** - Güçlü sorgu dili.

### <a name="alerts"></a>Uyarılar

* [Kullanılabilirlik testleri](../../azure-monitor/app/monitor-web-app-availability.md): Sitenizin Web'de görünür olduğundan emin olmak için testler oluşturun.
* [Akıllı tanılama](../../azure-monitor/app/proactive-diagnostics.md): Bunları ayarlamak için herhangi bir şey yapmak zorunda kalmamak için bu testler otomatik olarak çalıştırın. Uygulamanızda olağan dışı oranda başarısız istek olup olmadığını bildirirler.
* [Ölçüm uyarıları](../../azure-monitor/app/alerts.md): Bu ölçüm bir eşiği aştığında sizi uyaracak ayarlayın. Bunları, uygulamanıza kodladığınız özel ölçümlerde ayarlayabilirsiniz.

### <a name="automation"></a>Otomasyon

* [Application Insights kaynağı oluşturmayı otomatikleştirme](../../azure-monitor/app/powershell.md)
