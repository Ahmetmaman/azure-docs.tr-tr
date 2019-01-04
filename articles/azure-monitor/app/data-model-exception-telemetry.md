---
title: Azure Application Insights Telemetri veri modeli - özel durum Telemetrisi | Microsoft Docs
description: Özel durum telemetrisi için Application Insights veri modeli
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 04/25/2017
ms.reviewer: sergkanz
ms.author: mbullwin
ms.openlocfilehash: 5800bbc71158da42bc0baf21a4219be3ce54b22b
ms.sourcegitcommit: da69285e86d23c471838b5242d4bdca512e73853
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2019
ms.locfileid: "54000538"
---
# <a name="exception-telemetry-application-insights-data-model"></a>Özel durum telemetrisi: Application Insights veri modeli

İçinde [Application Insights](../../application-insights/app-insights-overview.md), izlenen uygulamanın yürütülmesi sırasında oluşan bir işlenen veya işlenmeyen özel durum bir özel durumun örneğini temsil eder.

## <a name="problem-id"></a>Sorun kimliği

Burada kodda özel durum oluştu, tanımlayıcısı'ı tıklatın. Özel durumları gruplandırmak için kullanılır. Genellikle bir özel durum türü ve birleşim çağrı yığınındaki bir işleve.

En fazla uzunluk: 1024 karakter

## <a name="severity-level"></a>Önem derecesi

İzleme önem düzeyi. Değeri olabilir `Verbose`, `Information`, `Warning`, `Error`, `Critical`.

## <a name="exception-details"></a>Özel durum ayrıntıları

(Genişletilmesi için)

## <a name="custom-properties"></a>Özel Özellikler

[!INCLUDE [application-insights-data-model-properties](../../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a>Özel ölçümler

[!INCLUDE [application-insights-data-model-measurements](../../../includes/application-insights-data-model-measurements.md)]

## <a name="next-steps"></a>Sonraki adımlar

- Bkz: [veri modeli](data-model.md) için Application Insights türleri ve veri modeli.
- Bilgi edinmek için nasıl [Application Insights ile web uygulamalarınızda özel durumları tanılama](../../azure-monitor/app/asp-net-exceptions.md).
- Kullanıma [platformları](../../azure-monitor/app/platforms.md) Application Insights tarafından desteklenir.
