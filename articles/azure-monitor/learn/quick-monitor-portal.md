---
title: ASP.NET Web Uygulamanızı Azure Application Insights ile izleme | Microsoft Docs
description: Application Insights ile izlemek üzere bir ASP.NET Web uygulamasını hızlıca ayarlamaya yönelik yönergeler sağlar
ms.subservice: application-insights
ms.topic: quickstart
author: mrbullwinkle
ms.author: mbullwin
ms.date: 06/26/2019
ms.custom: mvc
ms.openlocfilehash: 074010a2f3b1f4f4a58b3c4727bf4eed28402e0a
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82142627"
---
# <a name="start-monitoring-your-aspnet-web-application"></a>ASP.NET Web Uygulamanızı izlemeye başlama

Azure Application Insights ile web uygulamanızı kullanılabilirlik, performans ve kullanım bakımından kolayca izleyebilirsiniz.  Ayrıca, bir kullanıcının bildirmesini beklemeden uygulamanızdaki hataları hızlıca tanımlayıp tespit edebilirsiniz.  Application Insights’tan uygulamanızın verimi ve performansı hakkında topladığınız bilgileri kullanarak, uygulamanızı korumak ve geliştirmek için bilinçli seçimler yapabilirsiniz.

Bu hızlı başlangıç, var olan bir ASP.NET web uygulamasına Application Insights ekleme ve uygulamanızı çözümlemek için kullanabileceğiniz çeşitli yöntemlerden yalnızca biri olan canlı istatistikleri çözümlemeye başlama işlemini gösterir. Bir ASP.NET Web uygulamanız yoksa, [ASP.NET Web uygulaması oluşturma hızlı](../../app-service/app-service-web-get-started-dotnet-framework.md)başlangıcı ' nı izleyerek bir tane oluşturabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar
Bu hızlı başlangıcı tamamlamak için:

- Aşağıdaki iş yükleriyle [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) ' i yükledikten sonra:
    - ASP.NET ve web geliştirme
    - Azure geliştirme


Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="enable-application-insights"></a>Application Insights'ı etkinleştirme

1. Projenizi Visual Studio 2019 ' de açın.
2. Proje menüsünden **Application Insights’ı Yapılandır**’ı seçin. Visual Studio, uygulamanıza Application Insights SDK'sını ekler.

    > [!IMPORTANT]
    > Application Insights ekleme işlemi, ASP.NET şablon türüne göre değişiklik gösterir. **Boş** bir şablon veya **Azure Mobil Uygulaması** şablonu kullanıyorsanız **Proje** > **Application Insights Telemetri Ekleme** seçeneğini belirleyin. Diğer tüm ASP.NET şablonları için yukarıdaki adımda yer alan yönergelere başvurun. 

3. **Kullanmaya Başlayın**’a tıklayın (Visual Studio'nun önceki sürümlerinde bunun yerine **Ücretsiz Başlayın** düğmesi vardır).

    ![Visual Studio’ya Application Insights ekleme](./media/quick-monitor-portal/add-application-insights-b.png)

4. Aboneliğinizi seçin ve **Kaydet**’e tıklayın.

5. **Proje** > **NuGet paketleri** > **paket kaynağını seçin: NuGet.org** > Application Insights SDK paketlerini en son kararlı sürüme**güncelleştirin** .

6. **Hata Ayıkla** menüsünden **Hata Ayıklamayı Başlat**’ı seçerek veya F5 tuşuna basarak uygulamanızı çalıştırın.

## <a name="confirm-app-configuration"></a>Uygulama yapılandırmasını onaylama

Application Insights, uygulamanızın nerede çalıştığına bakmaksızın telemetri verilerini toplar. Bu verileri görüntülemeyi başlatmak için aşağıdaki adımları kullanın.

1. **View** -> **Diğer**Windows -> **Application Insights aramasını**görüntüle ' ye tıklayarak Application Insights açın.  Geçerli oturumunuzdaki telemetriye bakın.<BR><br>![Visual Studio'da telemetri](./media/quick-monitor-portal/telemetry-in-vs.png)

2. İstek ayrıntılarını görmek için listedeki ilk isteğe tıklayın (Bu örnekte, GET Home/Index). Durum kodu ve yanıt süresinin her ikisinin de istekle ilgili diğer değerli bilgilerle birlikte eklendiğine dikkat edin.<br><br>![Visual Studio'da yanıt ayrıntıları](media/quick-monitor-portal/request-details.png)

## <a name="start-monitoring-in-the-azure-portal"></a>Azure portalında izlemeyi başlatın

Artık Application Insights’ı Azure portalında açarak çalışan uygulamanıza ilişkin çeşitli ayrıntıları görüntüleyebilirsiniz.

1. Çözüm Gezgini **bağlı hizmetler** klasörünü (bulut ve tak simgesi) genişletin, **Application Insights** klasörüne sağ tıklayın ve **Application Insights Portal 'ı aç**' a tıklayın.  Uygulamanıza ilişkin bazı bilgiler ve çeşitli seçenekler görürsünüz.

    ![Uygulama Eşlemesi](media/quick-monitor-portal/04-overview.png)

2. Uygulama bileşenleriniz arasındaki bağımlılık ilişkilerinin görsel düzenini almak için **Uygulama haritası**’na tıklayın.  Her bileşen yük, performans, hatalar ve uyarılar gibi KPI'leri gösterir.

    ![Uygulama Eşlemesi](media/quick-monitor-portal/05-appmap.png)

3. Uygulama bileşenlerinden birinde bulunan](media/quick-monitor-portal/app-viewinlogs-icon.png) **günlüklerde (Analiz)** uygulama **Analizi** simgesine ![tıklayın. Bu, Application Insights tarafından toplanan tüm verileri analiz etmek için zengin bir sorgu dili sağlayan **Günlükler (Analiz)** açar. Bu örnekte, istek sayısını grafik olarak işleyen bir sorgu oluşturulur. Diğer verileri çözümlemek için kendi sorgularınızı yazabilirsiniz.

    ![Analiz](media/quick-monitor-portal/6viewanalytics.png)

4. Araştır altında sol tarafta **canlı ölçüm akışı** ' a tıklayın. Burada uygulamanızın çalışması sırasında canlı istatistikler gösterilir. Buna gelen istek sayısı, bu isteklerin süresi ve oluşan her türlü hata gibi bilgiler dahildir. Ayrıca, işlemci ve bellek gibi önemli performans ölçümlerini inceleyebilirsiniz.

    ![Canlı Akış](media/quick-monitor-portal/7livemetrics.png)

    Uygulamanızı Azure'da barındırmaya hazır olduğunuzda, artık yayımlayabilirsiniz. [ASP.NET Web Uygulaması Oluşturma Hızlı Başlangıcı](../../app-service/app-service-web-get-started-dotnet.md#update-the-app-and-redeploy) içinde açıklanan adımları izleyin.

5. Application Insights izleme özelliği eklemek için Visual Studio'yu kullanıyorsanız istemci tarafı izleme özelliklerini otomatik olarak ekleyebilirsiniz. İstemci tarafı izleme özelliklerini bir uygulamaya el ile eklemek için aşağıdaki JavaScript kodunu uygulamanıza eklemeniz gerekir:

```html
<!-- 
To collect user behavior analytics about your application, 
insert the following script into each page you want to track.
Place this code immediately before the closing </head> tag,
and before any other scripts. Your first data will appear 
automatically in just a few seconds.
-->
<script type="text/javascript">
var appInsights=window.appInsights||function(a){
  function b(a){c[a]=function(){var b=arguments;c.queue.push(function(){c[a].apply(c,b)})}}var c={config:a},d=document,e=window;setTimeout(function(){var b=d.createElement("script");b.src=a.url||"https://az416426.vo.msecnd.net/scripts/a/ai.0.js",d.getElementsByTagName("script")[0].parentNode.appendChild(b)});try{c.cookie=d.cookie}catch(a){}c.queue=[];for(var f=["Event","Exception","Metric","PageView","Trace","Dependency"];f.length;)b("track"+f.pop());if(b("setAuthenticatedUserContext"),b("clearAuthenticatedUserContext"),b("startTrackEvent"),b("stopTrackEvent"),b("startTrackPage"),b("stopTrackPage"),b("flush"),!a.disableExceptionTracking){f="onerror",b("_"+f);var g=e[f];e[f]=function(a,b,d,e,h){var i=g&&g(a,b,d,e,h);return!0!==i&&c["_"+f](a,b,d,e,h),i}}return c
  }({
      instrumentationKey:"<your instrumentation key>"
  });

window.appInsights=appInsights,appInsights.queue&&0===appInsights.queue.length&&appInsights.trackPageView();
</script>
```

Daha fazla bilgi edinmek için [açık kaynak JavaScript SDK'sı](https://github.com/Microsoft/ApplicationInsights-JS) GitHub deposunu ziyaret edin.

## <a name="clean-up-resources"></a>Kaynakları temizleme
Testi tamamladığınızda, kaynak grubunu ve tüm ilgili kaynakları silebilirsiniz. Bunu yapmak için aşağıdaki adımları izleyin.
1. Azure portal sol taraftaki menüden **kaynak grupları** ' na ve ardından **myresourcegroup**' a tıklayın.
2. Kaynak grubu sayfanızda, **Sil**’e tıklayın, metin kutusuna **myResourceGroup** yazın ve ardından **Sil**’e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçta, uygulamanızı Azure Application Insights tarafından izlemeye etkinleştirdiniz.  İstatistikleri izlemek ve uygulamanızdaki sorunları tespit etmek üzere nasıl kullanacağınızı öğrenmek için öğreticilere ilerleyin.

> [!div class="nextstepaction"]
> [Azure Application Insights öğreticileri](tutorial-runtime-exceptions.md)
