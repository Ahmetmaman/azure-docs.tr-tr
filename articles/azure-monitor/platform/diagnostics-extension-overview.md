---
title: Azure Tanılama uzantısına genel bakış
description: Azure tanılama 'yı kullanarak hata ayıklama, performans, izleme, bulut hizmetlerinde trafik analizi, sanal makineler ve Service Fabric
ms.subservice: diagnostic-extension
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 02/14/2020
ms.openlocfilehash: 4d424a22a26119dcb3ef6a0e5c4f3dc0c13b1aa4
ms.sourcegitcommit: 2f9f306fa5224595fa5f8ec6af498a0df4de08a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2021
ms.locfileid: "98927588"
---
# <a name="azure-diagnostics-extension-overview"></a>Azure Tanılama uzantısına genel bakış
Azure Tanılama uzantısı, Azure [izleyici 'de](agents-overview.md) sanal makineler dahil olmak üzere Azure işlem kaynaklarının Konuk işletim sisteminden izleme verilerini toplayan bir aracıdır. Bu makalede, desteklediği belirli işlevler ve yükleme ve yapılandırma seçenekleri de dahil olmak üzere Azure Tanılama uzantısına genel bakış sunulmaktadır. 

> [!NOTE]
> Azure Tanılama uzantısı, işlem kaynaklarının Konuk işletim sisteminden izleme verilerini toplamak için kullanılabilen aracılardan biridir. Gereksinimleriniz için uygun aracıları seçme konusunda farklı aracıların ve yönergelerin açıklaması için bkz. [Azure izleyici aracılarına genel bakış ](agents-overview.md) .

## <a name="primary-scenarios"></a>Birincil senaryolar
Tanılama uzantısı tarafından giderilen birincil senaryolar şunlardır:

- Azure Izleyici ölçümlerine Konuk ölçümleri toplayın.
- Arşivleme için Azure depolama 'ya Konuk günlükleri ve ölçümleri gönderin.
- Azure 'un dışından gönderilmek üzere Azure Olay Hub 'larına Konuk günlükleri ve ölçümleri gönderin.


## <a name="comparison-to-log-analytics-agent"></a>Log Analytics aracısına karşılaştırma
Azure Izleyici 'deki Log Analytics Aracısı, sanal makinelerin Konuk işletim sisteminden izleme verileri toplamak için de kullanılabilir. Gereksinimlerinize bağlı olarak ya da her ikisini de kullanmayı seçebilirsiniz. Azure izleyici aracılarının ayrıntılı bir karşılaştırması için bkz. [Azure izleyici aracılarına genel bakış](agents-overview.md) . 

Dikkate alınması gereken önemli farklılıklar şunlardır:

- Azure Tanılama uzantısı yalnızca Azure sanal makineler ile kullanılabilir. Log Analytics Aracısı, Azure 'da, diğer bulutlarda ve Şirket içindeki sanal makinelerle birlikte kullanılabilir.
- Azure Tanılama uzantısı, verileri Azure depolama 'ya, [Azure Izleme ölçümlerine](data-platform-metrics.md) (yalnızca Windows) ve Event Hubs gönderir. Log Analytics Aracısı verileri [Azure Izleyici günlüklerine](data-platform-logs.md)toplar.
- Log Analytics Aracısı, [Azure Güvenlik Merkezi](../../security-center/index.yml)gibi [çözümler](../monitor-reference.md#insights-and-core-solutions), [VM'ler için Azure izleyici](../insights/vminsights-overview.md)ve diğer hizmetler için gereklidir.

## <a name="costs"></a>Maliyetler
Azure tanılama uzantısı için bir maliyet yoktur, ancak alınan veriler için ücret ödemeniz gerekebilir. Veri topladığınız hedef için [Azure izleyici fiyatlandırmasını](https://azure.microsoft.com/pricing/details/monitor/) denetleyin.

## <a name="data-collected"></a>Toplanan veriler
Aşağıdaki tablolarda Windows ve Linux Tanılama uzantısı tarafından toplanabilecek veriler listelenmektedir.

### <a name="windows-diagnostics-extension-wad"></a>Windows Tanılama uzantısı (WAD)

| Veri Kaynağı | Description |
| --- | --- |
| Windows olay günlükleri   | Windows olay günlüğü 'nden olaylar. |
| Performans sayaçları | Sayısal değerler, işletim sistemi ve iş yüklerinin farklı yönlerinin performansını ölçerek. |
| IIS Günlükleri             | Konuk işletim sisteminde çalışan IIS Web siteleri için kullanım bilgileri. |
| Uygulama günlükleri     | Uygulamanız tarafından yazılan izleme iletileri. |
| .NET EventSource günlükleri |.NET [EventSource](/dotnet/api/system.diagnostics.tracing.eventsource) sınıfını kullanarak kod yazma olayları |
| [Bildirim tabanlı ETW günlükleri](/windows/desktop/etw/about-event-tracing) |Herhangi bir işlem tarafından oluşturulan Windows olayları için olay Izleme. |
| Kilitlenme dökümü (Günlükler)   | Uygulamanın kilitlenirse işlemin durumu hakkında bilgiler. |
| Dosya tabanlı Günlükler    | Uygulamanız veya hizmetiniz tarafından oluşturulan Günlükler. |
| Aracı tanılama günlükleri | Azure Tanılama hakkında bilgi. |


### <a name="linux-diagnostics-extension-lad"></a>Linux Tanılama uzantısı (LAD)

| Veri Kaynağı | Description |
| --- | --- |
| Syslog | Linux olay günlüğü sistemine gönderilen olaylar.   |
| Performans sayaçları  | Sayısal değerler, işletim sistemi ve iş yüklerinin farklı yönlerinin performansını ölçerek. |
| Günlük dosyaları | Dosya tabanlı günlüğe gönderilen girişler.  |

## <a name="data-destinations"></a>Veri hedefleri
Hem Windows hem de Linux için Azure tanılama uzantısı her zaman bir Azure depolama hesabına veri toplar. Bkz. [Windows Azure tanılama uzantısı 'nı (WAD) yükleyip yapılandırma](diagnostics-extension-windows-install.md) ve bu verilerin toplandığı belirli tablo ve Blobların bir listesi için [ölçümleri ve günlükleri Izlemek üzere Linux Tanılama uzantısı 'nı kullanın](../../virtual-machines/extensions/diagnostics-linux.md) .

Diğer ek hedeflere veri göndermek için bir veya daha fazla *veri havuzları* yapılandırın. Aşağıdaki bölümlerde, Windows ve Linux Tanılama uzantısı için mevcut olan havuzlar listelenmektedir.

### <a name="windows-diagnostics-extension-wad"></a>Windows Tanılama uzantısı (WAD)

| Hedef | Description |
|:---|:---|
| Azure İzleyici Ölçümleri | Performans verilerini Azure Izleyici ölçümlerine toplayın. Bkz. [Azure izleyici ölçüm veritabanına Konuk işletim sistemi ölçümleri gönderme](collect-custom-metrics-guestos-resource-manager-vm.md).  |
| Event Hubs | Azure 'un dışında veri göndermek için Azure Event Hubs kullanın. Bkz. [akış Azure Tanılama verileri Event Hubs](diagnostics-extension-stream-event-hubs.md) |
| Azure depolama Blobları | Tablolara ek olarak Azure Storage 'daki bloblara veri yazma. |
| Application Insights | Diğer uygulama izlemeyle tümleştirilecek Application Insights için sanal makinenizde çalışan uygulamalardan veri toplayın. Bkz. [Application Insights Tanılama verileri gönderme](diagnostics-extension-to-application-insights.md). |

Ayrıca, Log Analytics Aracısı genellikle bu işlev için kullanılsa da, Azure Izleyici günlükleri ile analiz yapmak için depolama alanından WAD verileri toplayabilirsiniz Log Analytics bir çalışma alanına toplayabilirsiniz. Doğrudan bir Log Analytics çalışma alanına veri gönderebilir ve ek işlevsellik sağlayan çözümleri ve öngörüleri destekler.  Bkz. [Azure depolama 'Dan Azure tanılama günlüklerini toplama](diagnostics-extension-logs.md). 


### <a name="linux-diagnostics-extension-lad"></a>Linux Tanılama uzantısı (LAD)
LAD, verileri Azure depolama 'daki tablolara yazar. Aşağıdaki tabloda yer aldığı havuzları destekler.

| Hedef | Description |
|:---|:---|
| Event Hubs | Azure 'un dışında veri göndermek için Azure Event Hubs kullanın. |
| Azure depolama Blobları | Tablolara ek olarak Azure Storage 'daki bloblara veri yazma. |
| Azure İzleyici Ölçümleri | Telegraf aracısını, LAD 'ya ek olarak yükleme. Bkz. [etkileyen bir LINUX VM için özel ölçümleri toplama telegraf Aracısı](collect-custom-metrics-linux-telegraf.md).


## <a name="installation-and-configuration"></a>Yükleme ve yapılandırma
Tanılama uzantısı, Azure 'da bir [sanal makine uzantısı](../../virtual-machines/extensions/overview.md) olarak uygulanır, bu nedenle Kaynak Yöneticisi şablonları, POWERSHELL ve CLI kullanarak aynı yükleme seçeneklerini destekler. Sanal makine uzantılarını yükleme ve sürdürme hakkında genel Ayrıntılar için bkz. [Windows](../../virtual-machines/extensions/features-windows.md) ve sanal makine uzantıları ve [Linux için](../../virtual-machines/extensions/features-linux.md) sanal makine uzantıları ve özellikleri.

Ayrıca, sanal makine menüsünün **izleme** bölümündeki **tanılama ayarları** altında Azure Portal hem Windows hem de Linux Tanılama uzantısını yükleyip yapılandırabilirsiniz.

Windows ve Linux için tanılama uzantısını yükleme ve yapılandırma hakkında ayrıntılı bilgi için aşağıdaki makalelere bakın.

- [Windows Azure tanılama uzantısı 'nı (WAD) yükleyip yapılandırma](diagnostics-extension-windows-install.md)
- [Ölçümleri ve günlükleri izlemek için Linux Tanılama Uzantısı’nı kullanma](../../virtual-machines/extensions/diagnostics-linux.md)

## <a name="other-documentation"></a>Diğer belgeler

###  <a name="azure-cloud-service-classic-web-and-worker-roles"></a>Azure bulut hizmeti (klasik) Web ve çalışan rolleri
- [Bulut hizmeti Izlemeye giriş](../../cloud-services/cloud-services-how-to-monitor.md)
- [Azure Cloud Services Azure Tanılama etkinleştirme](../../cloud-services/cloud-services-dotnet-diagnostics.md)
- [Azure Cloud Services için Application Insights](../app/cloudservices.md)<br>[Azure Tanılama bir Cloud Services uygulamasının akışını izleme](../../cloud-services/cloud-services-dotnet-diagnostics-trace-flow.md) 

### <a name="azure-service-fabric"></a>Azure Service Fabric
- [Yerel makine geliştirme kurulumundaki hizmetleri izleme ve tanılama](../../service-fabric/service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

## <a name="next-steps"></a>Sonraki adımlar


* [Azure tanılama 'de performans sayaçlarını kullanmayı](../../cloud-services/diagnostics-performance-counters.md)öğrenin.
* Azure depolama tablolarında verilerinizi başlatma veya bulma ile ilgili sorun yaşıyorsanız, bkz. [sorun giderme Azure tanılama](diagnostics-extension-troubleshooting.md)
