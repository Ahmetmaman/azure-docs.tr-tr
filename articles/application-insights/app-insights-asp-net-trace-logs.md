---
title: .NET izleme günlüklerini Application ınsights'ı keşfedin
description: İzleme, NLog veya Log4Net ile oluşturulan günlüklerinde arama yapma.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 0c2a084f-6e71-467b-a6aa-4ab222f17153
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 05/03/2017
ms.author: mbullwin
ms.openlocfilehash: b1cd2e8d7649de48f34efb0c7d839e17906a29bf
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39044994"
---
# <a name="explore-net-trace-logs-in-application-insights"></a>.NET izleme günlüklerini Application ınsights'ı keşfedin
NLog, log4Net veya System.Diagnostics.Trace ASP.NET uygulamanızı Tanılama izleme için kullanırsanız, gönderilen günlüklerinizi olabilir [Azure Application Insights][start], burada, arama ve keşfedebilirsiniz bunları. Günlüklerinizi, böylece her kullanıcı isteği hizmet ile ilişkilendirilmiş izlemeleri tanımlamak ve bunları diğer olayları ve özel durum raporları ilişkilendirmek uygulamanızdan gelen telemetri ile birleştirilir.

> [!NOTE]
> Günlük yakalama modülü gerekiyor mu? 3. taraf günlükçüler için kullanışlı bir bağdaştırıcı olduğundan, ancak NLog kullanmıyorsanız, log4Net veya System.Diagnostics.Trace göz önünde bulundurun yalnızca çağırma [Application Insights TrackTrace()](app-insights-api-custom-events-metrics.md#tracktrace) doğrudan.
>
>

## <a name="install-logging-on-your-app"></a>Uygulamanıza günlük yükle
Seçilen günlük Çerçevenizi projenize yükleyin. Bu app.config veya web.config bir girişe neden.

System.Diagnostics.Trace kullanıyorsanız, web.config dosyasına bir giriş eklemek gerekir:

```XML

    <configuration>
     <system.diagnostics>
       <trace autoflush="false" indentsize="4">
         <listeners>
           <add name="myListener"
             type="System.Diagnostics.TextWriterTraceListener"
             initializeData="TextWriterOutput.log" />
           <remove name="Default" />
         </listeners>
       </trace>
     </system.diagnostics>
   </configuration>
```
## <a name="configure-application-insights-to-collect-logs"></a>Application Insights'ı günlükleri toplamak için yapılandırma
**[Projenize Application Insights ekleme](app-insights-asp-net.md)**  bunu, henüz yapmadıysanız. Günlük toplayıcıyı dahil etmek için bir seçenek göreceksiniz.

Veya **Application ınsights'ı Yapılandır** Çözüm Gezgini'nde projenize sağ tıklayarak. Seçeneğini **izleme koleksiyonunu yapılandırma**.

*Application Insights menüsü ya da günlük Toplayıcı seçeneği yok?* Deneyin [sorun giderme](#troubleshooting).

## <a name="manual-installation"></a>El ile yükleme
Proje türünüzü (örneğin bir Windows Masaüstü Proje) Application Insights yükleyicisi tarafından desteklenmiyor, bu yöntemi kullanın.

1. Log4Net veya NLog kullanmayı planlıyorsanız, projenize yükleyin.
2. Çözüm Gezgini'nde projenize sağ tıklayıp seçin **NuGet paketlerini Yönet**.
3. "Application Insights" araması yapın
4. Uygun paket - aşağıdakilerden birini seçin:

   * Microsoft.ApplicationInsights.TraceListener (System.Diagnostics.Trace çağrıları yakalamak için)
   * Microsoft.ApplicationInsights.EventSourceListener (EventSource olaylarını yakalamak için)
   * Microsoft.ApplicationInsights.EtwCollector (ETW olaylarını yakalamak için)
   * Microsoft.ApplicationInsights.NLogTarget
   * Microsoft.ApplicationInsights.Log4NetAppender

NuGet paketi, gerekli bütünleştirilmiş kodları yükler ve web.config veya app.config de değiştirir.

## <a name="insert-diagnostic-log-calls"></a>Tanılama Günlüğü çağrıları Ekle
System.Diagnostics.Trace kullanıyorsanız, tipik bir çağrı olabilir:

    System.Diagnostics.Trace.TraceWarning("Slow response - database01");

Log4net veya NLog isterseniz:

    logger.Warn("Slow response - database01");

## <a name="using-eventsource-events"></a>EventSource olaylarını kullanma
Yapılandırabileceğiniz [System.Diagnostics.Tracing.EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) izlemeleri olarak Application ınsights'a gönderilmek üzere olayları. İlk olarak, yükleme `Microsoft.ApplicationInsights.EventSourceListener` NuGet paketi. Ardından Düzenle `TelemetryModules` bölümünü [Applicationınsights.config](app-insights-configuration-with-applicationinsights-config.md) dosya.

```xml
    <Add Type="Microsoft.ApplicationInsights.EventSourceListener.EventSourceTelemetryModule, Microsoft.ApplicationInsights.EventSourceListener">
      <Sources>
        <Add Name="MyCompany" Level="Verbose" />
      </Sources>
    </Add>
```

Her kaynak için aşağıdaki parametreleri ayarlayabilirsiniz:
 * `Name` Toplanacak EventSource adını belirtir.
 * `Level` Toplanacak günlüğe kaydetme düzeyini belirtir. Herhangi birini `Critical`, `Error`, `Informational`, `LogAlways`, `Verbose`, `Warning`.
 * `Keywords` (İsteğe bağlı) kullanmak için anahtar sözcükler birleşimleri tamsayı değerini belirtir.

## <a name="using-diagnosticsource-events"></a>DiagnosticSource olaylarını kullanma
Yapılandırabileceğiniz [System.Diagnostics.DiagnosticSource](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md) izlemeleri olarak Application ınsights'a gönderilmek üzere olayları. İlk olarak, yükleme [ `Microsoft.ApplicationInsights.DiagnosticSourceListener` ](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DiagnosticSourceListener) NuGet paketi. Ardından Düzenle `TelemetryModules` bölümünü [Applicationınsights.config](app-insights-configuration-with-applicationinsights-config.md) dosya.

```xml
    <Add Type="Microsoft.ApplicationInsights.DiagnosticSourceListener.DiagnosticSourceTelemetryModule, Microsoft.ApplicationInsights.DiagnosticSourceListener">
      <Sources>
        <Add Name="MyDiagnosticSourceName" />
      </Sources>
    </Add>
```

İzlemek istediğiniz her DiagnosticSource için sahip bir girdi Ekle `Name` özniteliği, DiagnosticSource adına ayarlayın.

## <a name="using-etw-events"></a>ETW olaylarını kullanma
ETW olayları olarak izlemeleri Application ınsights'a gönderilmek üzere yapılandırabilirsiniz. İlk olarak, yükleme `Microsoft.ApplicationInsights.EtwCollector` NuGet paketi. Ardından Düzenle `TelemetryModules` bölümünü [Applicationınsights.config](app-insights-configuration-with-applicationinsights-config.md) dosya.

> [!NOTE] 
> SDK'sı barındırma işlemi, "Performans günlük kullanıcılar" veya yöneticileri üyesi olan bir kimlik altında çalışıyorsa ETW olayları sadece toplanabilir.

```xml
    <Add Type="Microsoft.ApplicationInsights.EtwCollector.EtwCollectorTelemetryModule, Microsoft.ApplicationInsights.EtwCollector">
      <Sources>
        <Add ProviderName="MyCompanyEventSourceName" Level="Verbose" />
      </Sources>
    </Add>
```

Her kaynak için aşağıdaki parametreleri ayarlayabilirsiniz:
 * `ProviderName` Toplanacak ETW sağlayıcısı adıdır.
 * `ProviderGuid` toplamak için ETW sağlayıcısı GUID belirtir yerine kullanılabilir `ProviderName`.
 * `Level` Toplanacak günlüğe kaydetme düzeyini ayarlar. Herhangi birini `Critical`, `Error`, `Informational`, `LogAlways`, `Verbose`, `Warning`.
 * `Keywords` (İsteğe bağlı) kullanmak için anahtar sözcüğü birleşimleri tamsayı değerini ayarlar.

## <a name="using-the-trace-api-directly"></a>Kullanarak doğrudan API izlemesi
Application Insights izleme API'sini doğrudan çağırabilir. Günlüğe kaydetme bağdaştırıcıları bu API'yi kullanın.

Örneğin:

    var telemetry = new Microsoft.ApplicationInsights.TelemetryClient();
    telemetry.TrackTrace("Slow response - database01");

TrackTrace bir avantajı, iletide görece uzun veri koyabilirsiniz ' dir. Örneğin, gönderme verisi kodlayın.

Ayrıca, önem derecesi mesajınızı ekleyebilirsiniz. Ve diğer telemetriyi gibi filtre veya arama izlemeler farklı kümeleri için yardımcı olmak için kullanabileceğiniz özellik değerlerini ekleyebilirsiniz. Örneğin:

    var telemetry = new Microsoft.ApplicationInsights.TelemetryClient();
    telemetry.TrackTrace("Slow database response",
                   SeverityLevel.Warning,
                   new Dictionary<string,string> { {"database", db.ID} });

Bu, etkinleştirmek [arama][diagnostic], ilgili belirli bir veritabanına belirli bir önem derecesi düzeyinin tüm iletileri bir kolayca filtreleyin.

## <a name="explore-your-logs"></a>Günlüklerinizi keşfedin
Uygulamanızı ya da hata ayıklama modunda çalıştırmak veya canlı dağıtın.

Uygulamanızın genel bakış dikey penceresinde [Application Insights portalında][portal], seçin [arama][diagnostic].

![Uygulama anlayışları'nda arama klasörlerini seçin.](./media/app-insights-asp-net-trace-logs/020-diagnostic-search.png)

![Arama](./media/app-insights-asp-net-trace-logs/10-diagnostics.png)

Örneğin yapabilecekleriniz:

* Günlük izlemelerini ya da belirli özelliklere sahip öğeleri Filtrele
* Belirli bir öğeyi ayrıntılı olarak inceleyin.
* Aynı kullanıcı istekle ilgili diğer telemetriyi Bul (diğer bir deyişle, aynı Operationıd ile)
* Bu sayfa yapılandırmasını sık kullanılan olarak Kaydet

> [!NOTE]
> **Örnekleme.** Uygulamanız çok miktarda veri gönderiyorsa ve ASP.NET sürüm 2.0.0-beta3 veya daha sonraki uygulamalar için Application Insights SDK kullanıyorsanız, uyarlamalı örnekleme özelliği telemetrenizin yalnızca yüzdesini çalıştırır ve gönderir. [Örnekleme hakkında daha fazla bilgi edinin.](app-insights-sampling.md)
>
>

## <a name="next-steps"></a>Sonraki adımlar
[Hataları ve ASP.NET özel durumları tanılama][exceptions]

[Arama hakkında daha fazla bilgi][diagnostic].

## <a name="troubleshooting"></a>Sorun giderme
### <a name="how-do-i-do-this-for-java"></a>Nasıl Java için yapmam gerekir?
Kullanım [Java günlük bağdaştırıcıları](app-insights-java-trace-logs.md).

### <a name="theres-no-application-insights-option-on-the-project-context-menu"></a>Proje bağlam menüsünde Application Insights seçeneği yoktur
* Application Insights araçları bu geliştirme makinenizde yüklü denetleyin. Visual Studio menü Araçlar, uzantılar ve güncelleştirmeler menüsünde Application Insights araçları için bakın. Yüklü sekmede listelenmiyorsa, çevrimiçi sekmesini açın ve yükleyin.
* Bu türde bir proje Application Insights araçları tarafından desteklenmiyor olabilir. Kullanım [el ile yükleme](#manual-installation).

### <a name="no-log-adapter-option-in-the-configuration-tool"></a>Yapılandırma Aracı günlük bağdaştırıcısı seçeneği
* Günlüğe kaydetme çerçevesi yüklemeniz gerekir.
* System.Diagnostics.Trace kullanıyorsanız emin [bunların içinde yapılandırılan `web.config` ](https://msdn.microsoft.com/library/system.diagnostics.eventlogtracelistener.aspx).
* Application Insights'ın en son sürümünü destekler? Visual Studio'da **Araçları** menüsünde seçin **Uzantılar ve güncelleştirmeler**açın **güncelleştirmeleri** sekmesi. Geliştirici analiz araçları ise var. güncelleştirmek için tıklayın.

### <a name="emptykey"></a>"İzleme anahtarını boş bırakılamaz" hatasını alıyorum
Application Insights'ı yüklemeden günlük bağdaştırıcısı Nuget paketi yüklü gibi görünüyor.

Çözüm Gezgini'nde sağ `ApplicationInsights.config` ve **güncelleştirme Application Insights**. Azure'da oturum açmanız davet bir iletişim kutusu alırsınız ve bir Application Insights kaynağı oluşturun veya mevcut bir yeniden kullanın. Bunu çözer.

### <a name="i-can-see-traces-in-diagnostic-search-but-not-the-other-events"></a>Tanılama araması, ancak diğer olayları değil izlemelerinde görebiliyorum.
Bazen tüm olayları ve istek ardışık düzenine ulaşması biraz sürebilir.

### <a name="limits"></a>Ne kadar veri tutulur?
Çeşitli faktörler, korunan veri miktarı etkiler. Bkz: [sınırları](app-insights-api-custom-events-metrics.md#limits) daha fazla bilgi için müşteri olay ölçümleri sayfasının bölümünde. 

### <a name="im-not-seeing-some-of-the-log-entries-that-i-expect"></a>Bazı beklediğim günlük girişlerini görüyorum değil
Uygulamanız çok miktarda veri gönderiyorsa ve ASP.NET sürüm 2.0.0-beta3 veya daha sonraki uygulamalar için Application Insights SDK kullanıyorsanız, uyarlamalı örnekleme özelliği telemetrenizin yalnızca yüzdesini çalıştırır ve gönderir. [Örnekleme hakkında daha fazla bilgi edinin.](app-insights-sampling.md)

## <a name="add"></a>Sonraki adımlar
* [Kullanılabilirlik ve yanıt hızını testleri ayarlama][availability]
* [Sorun giderme][qna]

<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[exceptions]: app-insights-asp-net-exceptions.md
[portal]: https://portal.azure.com/
[qna]: app-insights-troubleshoot-faq.md
[start]: app-insights-overview.md
