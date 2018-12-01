---
title: Azure Application ınsights'ta Docker uygulamalarını izleme | Microsoft Docs
description: Docker performans sayaçları, olayları ve özel durumlar üzerinde Application Insights, kapsayıcılı uygulamaları alınan telemetri birlikte görüntülenebilir.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 27a3083d-d67f-4a07-8f3c-4edb65a0a685
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 11/20/2018
ms.author: mbullwin
ms.openlocfilehash: 91ba5024dc87ac7707d5ba1d2cdc94a7bfeb02b5
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2018
ms.locfileid: "52727469"
---
# <a name="monitor-docker-applications-in-application-insights"></a>Application ınsights'ta Docker uygulamalarını izleme

Yaşam döngüsü olayları ve performans sayaçları gelen [Docker](https://www.docker.com/) kapsayıcıları Application Insights grafiğinin. Yükleme [Application Insights](https://hub.docker.com/r/microsoft/applicationinsights/) görüntü bir kapsayıcıda, konak ve diğer görüntüleri yanı sıra, ana bilgisayar için performans sayaçlarını görüntüler.

Docker ile uygulamalarınızı basit kapsayıcıların tüm bağımlılıklarla birlikte eksiksiz olarak dağıtın. Bunlar bir Docker altyapısını çalıştıran herhangi bir ana makinede çalıştırın.

Çalıştırdığınızda [Application Insights görüntü](https://hub.docker.com/r/microsoft/applicationinsights/) Docker konak üzerinde bu avantajlar alın:

* Yaşam döngüsü telemetri çalıştıran tüm kapsayıcıları hakkında konakta - başlatmak, durdurmak ve benzeri.
* Tüm kapsayıcılar için performans sayaçları. CPU, bellek, ağ kullanımı ve daha fazlası.
* Varsa, [Java için Application Insights SDK'sı yüklü](app-insights-java-live.md) kapsayıcılarda çalıştırılan uygulamalar, bu uygulamaların tüm telemetri kapsayıcı ve ana makine tanımlayan ek özelliklere sahip. Örneğin, birden fazla ana çalışan bir uygulamanın bir örneği varsa, uygulama telemetrinizi ana bilgisayarı tarafından kolayca filtre uygulayabilirsiniz.

> [!NOTE]
> Bu çözümü kullanım dışıdır. Kapsayıcı izleme geçerli yaptığımız yatırımlardan hakkında daha fazla bilgi edinmek için kullanıma almasını öneririz [kapsayıcılar için Azure İzleyici](https://docs.microsoft.com/azure/azure-monitor/insights/container-insights-overview).

## <a name="set-up-your-application-insights-resource"></a>Kendi Application Insights kaynağını ayarlama

1. Oturum [Microsoft Azure Portal'da](https://azure.com) ; uygulamanız için Application Insights kaynağını açın veya [yeni bir tane oluşturun](app-insights-create-new-resource.md). 
   
    *Hangi kaynak kullanmalıyım?* Konağınız üzerinde çalışan uygulamalar başkası tarafından geliştirilen sonra yapmanız [yeni bir Application Insights kaynağı oluşturun](app-insights-create-new-resource.md). Burada görüntüleyebilir ve telemetriyi analiz budur. (Uygulama türü için ' genel' seçin.)
   
    Ancak uygulamalardan geliştiriciyseniz sonra umuyoruz [Application Insights SDK'sı eklenen](app-insights-java-live.md) bunların her biri için. Tüm gerçekten bileşenleri tek bir iş uygulamasının iseler, tüm bir kaynağa telemetri gönderecek şekilde yapılandırmanız ve Docker yaşam döngüsü ve performans verilerini görüntülemek için aynı kaynak kullanacaksınız. 
   
    Uygulamaların çoğu geliştirilen, ancak bunların telemetri görüntülemek için ayrı kaynaklar kullanıyorsanız, üçüncü bir senaryodur. Bu durumda, Docker verileri için ayrı bir kaynak oluşturmak için büyük olasılıkla de istersiniz.

2. Tıklayın **Essentials** açılır ve izleme anahtarını kopyalayın. Bu SDK, telemetri gönderileceği bildirmek için kullanın.

Bu tarayıcı penceresini elinizde bulundurun telemetrinizi kısa süre içinde görünmesini için geri dönen.

## <a name="run-the-application-insights-monitor-on-your-host"></a>Konağınız üzerinde Application Insights izlemeyi Çalıştır

Telemetri görüntülemek için bir yere kendinizi, toplar ve bunları gönderir kapsayıcılı uygulamayı ayarlayabilirsiniz.

1. Docker ana bilgisayarına bağlayın.
2. Bu komutta, izleme anahtarını düzenleyin ve ardından çalıştırın:
   
   ```
   
   docker run -v /var/run/docker.sock:/docker.sock -d microsoft/applicationinsights ikey=000000-1111-2222-3333-444444444
   ```

Docker ana bilgisayar başına yalnızca bir Application Insights görüntü gereklidir. Ardından, uygulamanızın birden çok Docker ana bilgisayarda dağıtılmışsa, her konakta komutu yineleyin.

## <a name="update-your-app"></a>Uygulamanızı güncelleştirin
Uygulamanız ile işaretlenmiş ise [Java için Application Insights SDK'sı](app-insights-java-get-started.md), altında aşağıdaki satırı, projenizdeki Applicationınsights.XML dosyasının içine ekleyin `<TelemetryInitializers>` öğesi:

```xml

    <Add type="com.microsoft.applicationinsights.extensibility.initializer.docker.DockerContextInitializer"/> 
```

Bu, kapsayıcı ve konak kimliği gibi Docker bilgileri uygulamanızdan gönderilen her telemetri öğesine ekler.

## <a name="view-your-telemetry"></a>Telemetrinizi görüntüleme
Azure portalında Application Insights kaynağınıza dönün.

Docker kutucuğa tıklayın.

Özellikle, Docker altyapısı üzerinde çalışan diğer kapsayıcılar varsa, kısa süre içinde Docker uygulamadan gelen verileri görürsünüz.

### <a name="docker-container-events"></a>Docker kapsayıcı olayları
![Örnek](./media/app-insights-docker/13.png)

Tek tek olayları araştırmak için tıklayın [arama](app-insights-diagnostic-search.md). Arama ve istediğiniz olayları bulmak için filtre uygulayın. Daha fazla ayrıntı almak için herhangi bir etkinliğe tıklayın.

### <a name="exceptions-by-container-name"></a>Özel durumların kapsayıcı adı
![Örnek](./media/app-insights-docker/14.png)

### <a name="docker-context-added-to-app-telemetry"></a>Docker bağlamı uygulama telemetri eklendi
AI SDK'sı ile izleme eklenmiş uygulama gönderilen istek telemetrisi ile Docker bağlam bilgilerini zenginleştirilmiş.

## <a name="q--a"></a>Soru-Cevap
*Application Insights Docker elde edilemiyor bana hangi mülklere?*

* Performans sayaçları kapsayıcı ve görüntü ayrıntılı dökümü.
* Kapsayıcı ve uygulama verilerini tek bir Panoda tümleştirin.
* [Telemetriyi dışarı aktarma](app-insights-export-telemetry.md) daha detaylı analiz bir veritabanı, Power BI veya diğer Pano.

*Uygulamadan telemetri nasıl alabilirim?*

* Uygulama içinde Application Insights SDK'sını yükleyin. Bilgi edinmek için nasıl: [Java web uygulamalarını](app-insights-java-get-started.md), [Windows web uygulamaları](app-insights-asp-net.md).

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a>Sonraki adımlar

* [Java için Application Insights](app-insights-java-get-started.md)
* [Node.js için Application Insights](app-insights-nodejs.md)
* [ASP.NET için Application Insights](app-insights-asp-net.md)
