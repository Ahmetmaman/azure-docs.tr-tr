---
title: Azure Application Insights Telemetri veri modeli | Microsoft Docs
description: Application Insights veri modeline genel bakış
services: application-insights
documentationcenter: .net
manager: carmonm
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 10/14/2019
ms.reviewer: sergkanz
ms.openlocfilehash: fd5848eb44ecd32612943662b5a4fd0a5091cd9d
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "91461300"
---
# <a name="application-insights-telemetry-data-model"></a>Application Insights telemetri veri modeli

[Azure Application Insights](./app-insights-overview.md) , uygulamanızın performansını ve kullanımını çözümleyebilmeniz için web uygulamanızdan Azure Portal telemetri gönderir. Telemetri modeli, platform ve dilden bağımsız izleme oluşturmak mümkün olacak şekilde standartlaştırılmış hale gelir. 

Application Insights tarafından toplanan veriler bu tipik uygulama yürütme modeli:

![Application Insights uygulama modeli](./media/data-model/application-insights-data-model.png)

Aşağıdaki telemetri türleri uygulamanızın yürütülmesini izlemek için kullanılır. Aşağıdaki üç tür, genellikle Web uygulaması çerçevesindeki Application Insights SDK tarafından otomatik olarak toplanır:

* Uygulamanız tarafından alınan bir isteği günlüğe kaydetmek için [**istek**](data-model-request-telemetry.md) oluşturuldu. Örneğin, Application Insights Web SDK, Web uygulamanızın aldığı her HTTP isteği için otomatik olarak bir Istek telemetrisi öğesi oluşturur. 

    **İşlem** , bir isteği işleyen yürütmenin iş parçacıklarından oluşur. Ayrıca, verileri düzenli aralıklarla işleyen bir Web işinde "uyandırma" gibi diğer işlem türlerini izlemek için de [kod yazabilirsiniz](./api-custom-events-metrics.md#trackrequest) .  Her işlemin bir KIMLIĞI vardır. Bu KIMLIK, uygulamanız isteği işlerken oluşturulan tüm Telemetriyi [gruplandırmak](./correlation.md) için kullanılabilir. Her işlem başarılı olur ya da başarısız olur ve bir süre süresi vardır.
* [**Özel durum**](data-model-exception-telemetry.md) -genellikle bir işlemin başarısız olmasına neden olan bir özel durumu temsil eder.
* [**Bağımlılık**](data-model-dependency-telemetry.md) -uygulamanızdan bir REST API veya SQL gibi bir dış hizmete veya depoya olan çağrıyı temsil eder. ASP.NET ' de, SQL 'e bağımlılık çağrıları tarafından tanımlanır `System.Data` . HTTP uç noktalarına yapılan çağrılar tarafından tanımlanır `System.Net` . 

Application Insights özel telemetri için üç ek veri türü sağlar:

* [Trace](data-model-trace-telemetry.md) -size tanıdık bir izleme çerçevesi kullanarak tanılama günlüğü uygulamak için doğrudan veya bir bağdaştırıcı aracılığıyla kullanılır `Log4Net` `System.Diagnostics` .
* [Olay](data-model-event-telemetry.md) -genellikle kullanım düzenlerini çözümlemek için hizmetinize yönelik kullanıcı etkileşimini yakalamak için kullanılır.
* [Ölçüm](data-model-metric-telemetry.md) -düzenli skaler ölçümleri raporlamak için kullanılır.

Her telemetri öğesi, uygulama sürümü veya Kullanıcı oturum kimliği gibi [bağlam bilgilerini](data-model-context.md) tanımlayabilir. Bağlam, belirli senaryoları engelleyen bir türü kesin belirlenmiş alanlar kümesidir. Uygulama sürümü düzgün şekilde başlatıldığında Application Insights, yeniden dağıtım ile bağıntılı uygulama davranışında yeni desenleri algılayabilir. Oturum kimliği, kullanıcılar üzerinde kesinti veya bir sorun etkisi hesaplamak için kullanılabilir. Bazı başarısız bağımlılık için farklı oturum kimliği sayısını hesaplama, hata izleme veya kritik özel durum, bir etkiyi daha iyi anlamak için sağlar.

Application Insights telemetri modeli, telemetrinin bir parçası olan işlem ile [ilişkilendirilmesi](./correlation.md) için bir yol tanımlar. Örneğin, bir istek bir SQL veritabanı çağrısı ve kayıtlı tanılama bilgileri yapabilir. İstek telemetrisine geri bağlayan telemetri öğeleri için bağıntı bağlamını ayarlayabilirsiniz.

## <a name="schema-improvements"></a>Şema geliştirmeleri

Application Insights veri modeli, uygulama telemetrinizi modeletmenin basit ve temel, güçlü bir yoludur. Temel senaryoları desteklemek ve Gelişmiş kullanım için şemayı genişletmeye izin vermek için modeli basit ve ince tutmak istiyoruz.

[Veri modeli veya şema sorunlarını ve önerilerini raporlamak Için GitHub deponuzu kullanın](https://github.com/microsoft/ApplicationInsights-dotnet/issues/new/choose).

## <a name="next-steps"></a>Sonraki adımlar

- [Özel telemetri yazma](./api-custom-events-metrics.md)
- [Telemetrinin nasıl genişletileceğini ve filtreleneceğini](./api-filtering-sampling.md)öğrenin.
- Veri modeli tabanlı telemetri miktarını en aza indirmek için [örnekleme](./sampling.md) kullanın.
- Application Insights tarafından desteklenen [platformları](./platforms.md) inceleyin.

