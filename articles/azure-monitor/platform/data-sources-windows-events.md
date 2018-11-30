---
title: Toplama ve Azure Log analytics'te Windows olay günlüklerini çözümleme | Microsoft Docs
description: Windows olay günlüklerini Log Analytics tarafından kullanılan en yaygın veri kaynaklarının biridir.  Bu makale koleksiyonu Windows olay günlükleri ve Log Analytics çalışma alanında oluşturdukları kayıtları ayrıntılarını nasıl yapılandırılacağını açıklar.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: ee52f564-995b-450f-a6ba-0d7b1dac3f32
ms.service: log-analytics
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/11/2017
ms.author: bwren
ms.component: ''
ms.openlocfilehash: 036378c5ed595ffbaeefad9407fba8519945cf5c
ms.sourcegitcommit: 922f7a8b75e9e15a17e904cc941bdfb0f32dc153
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/27/2018
ms.locfileid: "52337080"
---
# <a name="windows-event-log-data-sources-in-log-analytics"></a>Log analytics'te Windows olay günlüğü veri kaynakları
Windows olay günlükleri, en sık kullanılan bir [veri kaynakları](agent-data-sources.md) birçok uygulama Windows olay günlüğüne yazma beri Windows aracılarını kullanarak veri toplama için.  İzlemeniz gereken uygulamaları tarafından oluşturulan tüm özel günlükleri belirtmenin yanı sıra sistem ve uygulama gibi standart günlüklerinden olayları toplayabilir.

![Windows olayları](media/data-sources-windows-events/overview.png)     

## <a name="configuring-windows-event-logs"></a>Yapılandırma Windows olay günlükleri
Windows olay günlüklerini yapılandırma [günlük analizi ayarları veri menüde](agent-data-sources.md#configuring-data-sources).

Log Analytics'e yalnızca ayarlarında belirttiğiniz Windows olay günlükleri olayları toplar.  Bir olay günlüğü, günlük adını yazarak ve tıklayarak ekleyebilirsiniz **+**.  Her günlük için yalnızca seçilen önem dereceleri olayları toplanır.  Toplamak istediğiniz belirli bir günlük için önem derecelerini işaretleyin.  Olayları Filtrele için herhangi bir ek ölçüt sağlayamaz.

Bir olay günlüğü adı yazarken, Log Analytics ortak olay günlüğü adlarının önerileri sağlar. Eklemek istediğiniz günlük listede görünmüyorsa, günlüğün tam adını yazarak yine de ekleyebilirsiniz. Günlük tam adı, Olay Görüntüleyicisi'ni kullanarak bulabilirsiniz. Olay Görüntüleyicisi'nde açın *özellikleri* dizeden kopyalayın ve sayfa için günlüğe *tam adı* alan.

![Windows olayları Yapılandır](media/data-sources-windows-events/configure.png)

## <a name="data-collection"></a>Veri toplama
Log Analytics, olay oluşturulurken, seçilen bir önem derecesi izlenen bir olay günlüğünden eşleşen her olay toplar.  Aracı, onun yerine toplar, her olay günlüğüne kaydeder.  Aracıyı bir süre için çevrimdışı olursa, bu olayları aracının çevrimdışı durumdayken oluşturulmuş olsalar bile sonra Log Analytics olayları son devre dışı kaldığı toplar.  Bu olayları olay günlüğüne aracının çevrimdışı durumdayken üzerine yazılmasını uncollected olaylarla sarmalar, toplanmayan olasılığı yoktur.

>[!NOTE]
>Log Analytics, kaynak SQL Server tarafından oluşturulan denetim olaylarının toplamak değil *MSSQLSERVER* anahtar sözcükleri - içeren, olay kimliği 18453 *Klasik* veya *denetim başarı* ve anahtar sözcüğü *0xa0000000000000*.
>

## <a name="windows-event-records-properties"></a>Windows olay kayıtları özellikleri
Windows olay kayıtlarını bir türü sahip **olay** ve aşağıdaki tabloda gösterilen özelliklere sahiptir:

| Özellik | Açıklama |
|:--- |:--- |
| Bilgisayar |Olay toplandığı bilgisayarın adı. |
| EventCategory |Olayın kategorisi. |
| EventData |Tüm olay verileri ham biçiminde. |
| EventID |Olay sayısı. |
| eventLevel |Sayısal biçimde olayın önem derecesi. |
| EventLevelName |Metin biçiminde etkinliğin önem derecesi. |
| Olay günlüğü |Olay toplanan olay günlüğü adı. |
| ParameterXml |XML biçiminde olay parametre değerleri. |
| ManagementGroupName |System Center Operations Manager aracıları için yönetim grubunun adı.  Diğer aracılar için bu değer AOI -:<workspace ID> |
| RenderedDescription |Parametre değerleri ile olay açıklaması |
| Kaynak |Olayın kaynağı. |
| SourceSystem |Olay toplandığı aracı türü. <br> OpsManager – Windows Aracısı, doğrudan bağlanın veya Operations Manager yönetilen <br> Linux – tüm Linux aracıları  <br> AzureStorage – Azure tanılama |
| TimeGenerated |Tarih ve saat içinde Windows olay oluşturuldu. |
| UserName |Olayın günlüğe hesabın kullanıcı adı. |

## <a name="log-searches-with-windows-events"></a>Günlük aramaları Windows olayları
Aşağıdaki tabloda, Windows olay kayıtları almak günlük aramaları farklı örnekler sağlar.

| Sorgu | Açıklama |
|:---|:---|
| Olay |Tüm Windows olayları. |
| Olay &#124; burada EventLevelName "error" == |Tüm Windows olayları ile hata önem derecesi. |
| Olay &#124; kaynağına göre Count() işlevi özetleme |Kaynak olayların sayısı, Windows. |
| Olay &#124; burada EventLevelName "error" == &#124; kaynağına göre Count() işlevi özetleme |Kaynak tarafından sayısı, Windows hata olayları. |


## <a name="next-steps"></a>Sonraki adımlar
* Diğer toplamak için log Analytics'i yapılandırma [veri kaynakları](agent-data-sources.md) analiz.
* Hakkında bilgi edinin [günlük aramaları](../../log-analytics/log-analytics-queries.md) veri kaynakları ve çözümlerinden toplanan verileri analiz etmek için.  
* Kullanım [özel alanlar](../../log-analytics/log-analytics-custom-fields.md) olay kayıtları tek tek alanlarına ayrıştırılamıyor.
* Yapılandırma [performans sayaçlarını toplamayı](data-sources-performance-counters.md) Windows aracılarınızdan.
