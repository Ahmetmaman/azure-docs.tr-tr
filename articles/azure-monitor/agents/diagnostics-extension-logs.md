---
title: Azure Izleyici 'de IIS ve tablo depolaması için blob depolamayı kullanma | Microsoft Docs
description: Azure Izleyici, tablo depolama veya blob depolamaya yazılan IIS günlüklerine tanılama yazan Azure hizmetleri için günlükleri okuyabilir.
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 02/14/2020
ms.openlocfilehash: deb6b5f3718c1a7c84e3591bf9abcceb72b785da
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102054975"
---
# <a name="collect-data-from-azure-diagnostics-extension-to-azure-monitor-logs"></a>Azure tanılama uzantılarından Azure Izleyici günlüklerine veri toplama
Azure tanılama uzantısı, Azure [izleyici 'de](../agents/agents-overview.md) sanal makineler dahil olmak üzere Azure işlem kaynaklarının Konuk işletim sisteminden izleme verilerini toplayan bir aracıdır. Bu makalede, Azure depolama 'dan Azure Izleyici günlüklerine tanılama uzantısı tarafından toplanan verilerin nasıl toplanacağı açıklanır.

> [!NOTE]
> Azure Izleyici 'deki Log Analytics Aracısı genellikle Konuk işletim sisteminden Azure Izleyici günlüklerine veri toplamak için tercih edilen yöntemdir. Aracıların ayrıntılı bir karşılaştırması için bkz. [Azure izleyici aracılarına genel bakış](../agents/agents-overview.md) .

## <a name="supported-data-types"></a>Desteklenen veri türleri
Azure tanılama uzantısı, verileri bir Azure depolama hesabında depolar. Bu verileri toplamak için Azure Izleyici günlüklerinin aşağıdaki konumlarda olması gerekir:

| Günlük Türü | Kaynak Türü | Konum |
| --- | --- | --- |
| IIS günlükleri |Sanal Makineler <br> Web rolleri <br> Çalışan rolleri |WAD-IIS-LogFiles (BLOB depolama) |
| Syslog |Sanal Makineler |LinuxsyslogVer2v0 (tablo depolama) |
| Işlem olaylarını Service Fabric |Service Fabric düğümleri |WADServiceFabricSystemEventTable |
| Güvenilir aktör olaylarını Service Fabric |Service Fabric düğümleri |WADServiceFabricReliableActorEventTable |
| Güvenilir hizmet olaylarını Service Fabric |Service Fabric düğümleri |WADServiceFabricReliableServiceEventTable |
| Windows olay günlükleri |Service Fabric düğümleri <br> Sanal Makineler <br> Web rolleri <br> Çalışan rolleri |WADWindowsEventLogsTable (tablo depolama) |
| Windows ETW günlükleri |Service Fabric düğümleri <br> Sanal Makineler <br> Web rolleri <br> Çalışan rolleri |Wadelenebilir Venttable (tablo depolama) |

## <a name="data-types-not-supported"></a>Veri türleri desteklenmiyor

- Konuk işletim sistemindeki performans verileri
- Azure Web sitelerinden IIS günlükleri


## <a name="enable-azure-diagnostics-extension"></a>Azure tanılama uzantısını etkinleştirme
Tanılama uzantısını yükleme ve yapılandırma hakkında ayrıntılı bilgi için bkz. [Windows Azure tanılama uzantısı 'nı (WAD) yükleme ve yapılandırma](../agents/diagnostics-extension-windows-install.md) veya [Linux Tanılama uzantısı 'nı kullanma](../../virtual-machines/extensions/diagnostics-linux.md) . Bu, depolama hesabını belirtmenize ve Azure Izleyici günlüklerine iletmek istediğiniz verilerin toplanmasını yapılandırmanıza olanak tanır.


## <a name="collect-logs-from-azure-storage"></a>Azure depolama 'dan günlükleri topla
Azure depolama hesabından tanılama uzantısı verileri toplamayı etkinleştirmek için aşağıdaki yordamı kullanın:

1. Azure portal, **Log Analytics çalışma alanları** ' na gidin ve çalışma alanınızı seçin.
1. Menüdeki **çalışma alanı veri kaynakları** bölümünde **depolama hesapları günlükleri** ' ne tıklayın.
2. **Ekle**' ye tıklayın.
3. Toplanacak verileri içeren **Depolama hesabını** seçin.
4. Toplamak istediğiniz **veri türünü** seçin.
5. Kaynak değeri veri türüne göre otomatik olarak doldurulur.
6. Yapılandırmayı kaydetmek için **Tamam** ' ı tıklatın.
7. Ek veri türleri için tekrarlayın.

Yaklaşık 30 dakika içinde, Log Analytics çalışma alanındaki depolama hesabından verileri görebilirsiniz. Yalnızca yapılandırma uygulandıktan sonra depolamaya yazılan veriler görüntülenir. Çalışma alanı, depolama hesabından önceden var olan verileri okuyamıyor.

> [!NOTE]
> Portal, kaynağın depolama hesabında mevcut olduğunu veya yeni verilerin yazılmakta olduğunu doğrulamaz.



## <a name="next-steps"></a>Sonraki adımlar

* Desteklenen Azure hizmetleri için [Azure hizmetleri için günlükleri ve ölçümleri toplayın](../essentials/resource-logs.md#send-to-log-analytics-workspace) .
* Verilerle ilgili Öngörüler sağlamak için [çözümleri etkinleştirin](../insights/solutions.md) .
* Verileri çözümlemek için [arama sorguları](../logs/log-query-overview.md) ' nı kullanın.