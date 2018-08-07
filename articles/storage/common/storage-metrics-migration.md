---
title: Azure depolama ölçümleri geçiş | Microsoft Docs
description: Azure İzleyici tarafından yönetilen yeni ölçümler için eski ölçümleri geçirmeyi öğrenin.
services: storage
author: fhryo-msft
ms.service: storage
ms.topic: article
ms.date: 03/30/2018
ms.author: fryu
ms.component: common
ms.openlocfilehash: fc11e29b03df617c4b5bb6f4fbb43cd478001d42
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2018
ms.locfileid: "39521430"
---
# <a name="azure-storage-metrics-migration"></a>Azure depolama ölçümleri geçişi

Azure İzleyici deneyimini birleştirme stratejisini ile hizalanmış, Azure depolama ölçümlerini Azure İzleyici platforma tümleştirilir. Gelecekte, eski ölçüm hizmeti, Azure ilkesine göre erken bir bildirim ile sona erer. Eski depolama ölçümleri güveniyorsanız, ölçüm bilgilerinizi korumak için hizmet bitiş tarihinden önce geçirmek gerekir.

Bu makalede, eski ölçümleri yeni ölçümlere geçme işlemini göstermektedir.

## <a name="understand-old-metrics-that-are-managed-by-azure-storage"></a>Azure Depolama tarafından yönetilen eski ölçüleri anlama

Azure depolama eski ölçüm değerleri toplar ve toplar ve bunları aynı depolama hesabındaki tablolar $Metric depolar. İzleme bir hesap için Azure portalını kullanabilirsiniz. Azure depolama SDK'ları, şemaya dayalı $Metric tablolardan verileri okumak için de kullanabilirsiniz. Daha fazla bilgi için [depolama analizi](./storage-analytics.md).

Eski ölçüm yalnızca Azure Blob Depolama kapasite ölçümleri sağlar. Eski ölçüm, Blob Depolama, tablo depolama, Azure dosyaları ve kuyruk depolama üzerinde işlem ölçümlerini sağlar. 

Eski ölçüm düz bir şemada tasarlanmıştır. Ölçüm tetikleme trafik düzenlerini olmadığında tasarım ölçüm sıfır değerini verir. Örneğin, **ServerTimeoutError** değeri ayarı $Metric tablolardaki 0 olduğunda bile sunucu zaman aşımı hatalarını Canlı trafiği bir depolama hesabına almadığınız.

## <a name="understand-new-metrics-managed-by-azure-monitor"></a>Azure İzleyici tarafından yönetilen yeni ölçümler anlama

Yeni depolama ölçümlerine için Azure depolama, ölçüm verilerini Azure İzleyici arka ucuna yayar. Azure İzleyici, veri portalından da dahil olmak üzere birleştirilmiş bir izleme deneyimi sağlar veri alımı yanı sıra. Daha fazla ayrıntı için bu başvurabilirsiniz [makale](../../monitoring-and-diagnostics/monitoring-overview-metrics.md).

Blob, tablo, dosya, kuyruk ve premium depolama kapasite ölçümleri ve işlem ölçümlerini yeni ölçümler sağlar.

Çok boyutlu Azure İzleyicisi'nin sağladığı özelliklerden biridir. Azure depolama, yeni ölçüm şema tanımlama tasarım devralır. İçin desteklenen boyutlar ölçümlere ilişkin ayrıntılı bilgi bulabilirsiniz [Azure İzleyici'de Azure depolama ölçümleri](./storage-metrics-in-azure-monitor.md). Birden çok boyut tasarımı, her iki bant alımı ve kapasite ölçümleri engeller maliyet verimliliği sağlar. Sonuç olarak, ilgili ölçümleri trafiğiniz tetiklediğini değil, ilgili ölçüm verileri üretilmez. Trafiğiniz sunucu zaman aşımı hatalarını tetiklediğini değil, ölçüm değeri sorguladığınızda, Azure İzleyici herhangi bir veri döndürmüyor **işlemleri** boyut ile **ResponseType** eşittir **ServerTimeoutError**.

## <a name="metrics-mapping-between-old-metrics-and-new-metrics"></a>Eski ölçüm ve yeni ölçümler arasında eşleme ölçümleri

Ölçüm verilerini programlı bir şekilde okuyun, programlarınızda yeni ölçüm şeması kullanmaları gerekir. Değişiklikleri daha iyi anlamak için aşağıdaki tabloda listelenen eşleme başvurabilirsiniz:

**Kapasite ölçümleri**

| Eski ölçüm | Yeni ölçüm |
| ------------------- | ----------------- |
| **Kapasite**            | **BlobCapacity** boyutuyla **BlobType** eşit **BlockBlob** veya **PageBlob** |
| **ObjectCount**        | **BLOB sayısı** boyutuyla **BlobType** eşit **BlockBlob** veya **PageBlob** |
| **ContainerCount**      | **ContainerCount** |

Aşağıdaki ölçümler eski ölçümleri desteklemeyen yeni teklifler şunlardır:
* **TableCapacity**
* **TableCount**
* **TableEntityCount**
* **QueueCapacity**
* **QueueCount**
* **QueueMessageCount**
* **FileCapacity**
* **FileCount**
* **FileShareCount**
* **UsedCapacity**

**İşlem ölçümleri**

| Eski ölçüm | Yeni ölçüm |
| ------------------- | ----------------- |
| **AnonymousAuthorizationError** | Boyut ile işlemleri **ResponseType** eşit **AuthorizationError** |
| **AnonymousClientOtherError** | Boyut ile işlemleri **ResponseType** eşit **ClientOtherError** |
| **AnonymousClientTimeoutError** | Boyut ile işlemleri **ResponseType** eşit **ClientTimeoutError** |
| **AnonymousNetworkError** | Boyut ile işlemleri **ResponseType** eşit **NetworkError** |
| **AnonymousServerOtherError** | Boyut ile işlemleri **ResponseType** eşit **ServerOtherError** |
| **AnonymousServerTimeoutError** | Boyut ile işlemleri **ResponseType** eşit **ServerTimeoutError** |
| **AnonymousSuccess** | Boyut ile işlemleri **ResponseType** eşit **başarılı** |
| **AnonymousThrottlingError** | Boyut ile işlemleri **ResponseType** eşit **ClientThrottlingError** veya **ServerBusyError** |
| **AuthorizationError** | Boyut ile işlemleri **ResponseType** eşit **AuthorizationError** |
| **Kullanılabilirlik** | **Kullanılabilirlik** |
| **AverageE2ELatency** | **Başarı E2e** |
| **AverageServerLatency** | **SuccessServerLatency** |
| **ClientOtherError** | Boyut ile işlemleri **ResponseType** eşit **ClientOtherError** |
| **ClientTimeoutError** | Boyut ile işlemleri **ResponseType** eşit **ClientTimeoutError** |
| **NetworkError** | Boyut ile işlemleri **ResponseType** eşit **NetworkError** |
| **PercentAuthorizationError** | Boyut ile işlemleri **ResponseType** eşit **AuthorizationError** |
| **PercentClientOtherError** | Boyut ile işlemleri **ResponseType** eşit **ClientOtherError** |
| **Percentnetworkerror'da** | Boyut ile işlemleri **ResponseType** eşit **NetworkError** |
| **PercentServerOtherError** | Boyut ile işlemleri **ResponseType** eşit **ServerOtherError** |
| **PercentSuccess** | Boyut ile işlemleri **ResponseType** eşit **başarılı** |
| **Percentthrottlingerror'da** | Boyut ile işlemleri **ResponseType** eşit **ClientThrottlingError** veya **ServerBusyError** |
| **Percenttimeouterror'da** | Boyut ile işlemleri **ResponseType** eşit **ServerTimeoutError** veya **ResponseType** eşit **ClientTimeoutError** |
| **SASAuthorizationError** | Boyut ile işlemleri **ResponseType** eşit **AuthorizationError** |
| **SASClientOtherError** | Boyut ile işlemleri **ResponseType** eşit **ClientOtherError** |
| **SASClientTimeoutError** | Boyut ile işlemleri **ResponseType** eşit **ClientTimeoutError** |
| **SASNetworkError** | Boyut ile işlemleri **ResponseType** eşit **NetworkError** |
| **SASServerOtherError** | Boyut ile işlemleri **ResponseType** eşit **ServerOtherError** |
| **SASServerTimeoutError** | Boyut ile işlemleri **ResponseType** eşit **ServerTimeoutError** |
| **SASSuccess** | Boyut ile işlemleri **ResponseType** eşit **başarılı** |
| **SASThrottlingError** | Boyut ile işlemleri **ResponseType** eşit **ClientThrottlingError** veya **ServerBusyError** |
| **ServerOtherError** | Boyut ile işlemleri **ResponseType** eşit **ServerOtherError** |
| **ServerTimeoutError** | Boyut ile işlemleri **ResponseType** eşit **ServerTimeoutError** |
| **Başarılı** | Boyut ile işlemleri **ResponseType** eşit **başarılı** |
| **ThrottlingError** | **İşlem** boyutuyla **ResponseType** eşit **ClientThrottlingError** veya **ServerBusyError**|
| **TotalBillableRequests** | **İşlemler** |
| **TotalEgress** | **Çıkış** |
| **Totalıngress** | **Giriş** |
| **TotalRequests** | **İşlemler** |

## <a name="faq"></a>SSS

### <a name="how-should-i-migrate-existing-alert-rules"></a>Mevcut uyarı kuralları nasıl geçirmeniz gerekir?

Eski depolama ölçümlere göre Klasik uyarı kuralları oluşturursanız, yeni uyarı kuralları yeni ölçüm şemaya dayalı oluşturmanız gerekir.

### <a name="is-new-metric-data-stored-in-the-same-storage-account-by-default"></a>Yeni ölçüm verileri varsayılan olarak aynı depolama hesabında depolanır?

Hayır. Ölçüm verileri bir depolama hesabına arşivleme için kullanın [Azure İzleyici tanılama ayarı API](https://docs.microsoft.com/rest/api/monitor/diagnosticsettings/createorupdate).

## <a name="next-steps"></a>Sonraki adımlar

* [Azure İzleyici](../../monitoring-and-diagnostics/monitoring-overview.md)
* [Azure İzleyici'de depolama ölçümleri](./storage-metrics-in-azure-monitor.md)
