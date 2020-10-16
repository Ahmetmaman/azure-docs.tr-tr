---
title: 'Application Insights: diller, platformlar ve tümleştirmeler | Microsoft Docs'
description: Application Insights için kullanılabilen diller, platformlar ve tümleştirmeler
ms.topic: conceptual
ms.date: 07/18/2019
ms.reviewer: olegan
ms.openlocfilehash: d3bb1473d94db04d1dedf2b09e19b83197cc6e0e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "88225113"
---
# <a name="supported-languages"></a>Desteklenen diller

* [C#|VB (.NET)](./asp-net.md)
* [Java](./java-get-started.md)
* [JavaScript](./javascript.md)
* [Node.JS](./nodejs.md)
* [Python](./opencensus-python.md)

## <a name="supported-platforms-and-frameworks"></a>Desteklenen platformlar ve çerçeveler

### <a name="instrumentation-for-already-deployed-applications-codeless-agent-based"></a>Zaten dağıtılan uygulamalar için izleme (codeless, aracı tabanlı)
* [Azure VM ve Azure sanal makine ölçek kümeleri](./azure-vm-vmss-apps.md)
* [Azure App Service](./azure-web-apps.md)
* [ASP.NET - zaten canlı olan uygulamalar için](./monitor-performance-live-website-now.md)
* Hem Web hem de çalışan rolleri dahil olmak üzere [Azure Cloud Services](./cloudservices.md)
* [Azure İşlevleri](../../azure-functions/functions-monitoring.md)
### <a name="instrumentation-through-code-sdks"></a>Kod üzerinden izleme (SDK)
* [ASP.NET](./asp-net.md)
* [ASP.NET Core](./asp-net-core.md)
* [Android](../learn/mobile-center-quickstart.md) (App Center)
* [iOS](../learn/mobile-center-quickstart.md) (App Center)
* [Java EE](./java-get-started.md)
* [Node.JS](https://www.npmjs.com/package/applicationinsights)
* [Python](./opencensus-python.md)
* [Evrensel Windows uygulaması](../learn/mobile-center-quickstart.md) (App Center)
* [Windows masaüstü uygulamaları, hizmetleri ve çalışan rolleri](./windows-desktop.md)
* [React](./javascript-react-plugin.md)
* [React Native](./javascript-react-native-plugin.md)

## <a name="logging-frameworks"></a>Günlük altyapıları
* [ILogger](./ilogger.md)
* [Log4Net, NLog veya System.Diagnostics.Trace](./asp-net-trace-logs.md)
* [Java, Log4J veya Logback](./java-trace-logs.md)
* [LogStash eklentisi](https://github.com/Azure/azure-diagnostics-tools/tree/master/Logstash/logstash-output-applicationinsights)
* [Azure İzleyici](/archive/blogs/msoms/application-insights-connector-in-oms)

## <a name="export-and-data-analysis"></a>Dışarı aktarma ve veri analizi
* [Power BI](https://powerbi.microsoft.com/blog/explore-your-application-insights-data-with-power-bi/)
* [Stream Analytics](./export-power-bi.md)

## <a name="unsupported-sdks"></a>Desteklenmeyen SDK’lar
Diğer topluluk tarafından desteklenen birkaç SDK 'nın mevcut olduğunu biliyoruz. Ancak, Azure Izleyici yalnızca bu sayfada listelenen desteklenen SDK 'Ları kullanırken destek sağlar. Diğer dillere yönelik desteğimizi genişletmeyi sürekli olarak değerlendirdik, bu nedenle en son SDK haberlerini almak için [GitHub duyuruları](https://github.com/microsoft/ApplicationInsights-Announcements/issues) sayfamızı izleyin. 

