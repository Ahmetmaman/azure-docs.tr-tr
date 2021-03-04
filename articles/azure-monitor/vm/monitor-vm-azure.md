---
title: Azure Izleyici ile Azure sanal makinelerini izleme
description: Azure Izleyici kullanarak Azure 'daki sanal makinelerden izleme verilerinin nasıl toplanacağını ve analiz edileceğini açıklar.
ms.service: azure-monitor
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 05/05/2020
ms.openlocfilehash: 2c93471436030f9260f4fa0d95d656c27d382346
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102047052"
---
# <a name="monitoring-azure-virtual-machines-with-azure-monitor"></a>Azure Izleyici ile Azure sanal makinelerini izleme
Bu makalede, Azure Izleyici 'nin, Azure sanal makinelerindeki izleme verilerini toplamak ve analiz etmek için, sistem durumlarını korumak üzere nasıl kullanılacağı açıklanır. Sanal makineler, Azure Izleyici ile [diğer Azure kaynakları](../essentials/monitor-azure-resource.md)gibi kullanılabilirlik ve performans için izlenebilir, ancak Konuk işletim sistemini ve sistemi ve içinde çalışan iş yüklerini izlemeniz gerektiğinden diğer kaynaklardan benzersizdir. 

> [!NOTE]
> Bu makalede, Azure Izleyici 'de sanal makineleri izlemeye yönelik kavramlara ve seçeneklere yönelik kapsamlı bir genel bakış sunulmaktadır. Arka plandaki kavramlara odaklanmadan sanal makinelerinizi hızla izlemeye başlamak için bkz. [hızlı başlangıç: Azure izleyici Ile Azure sanal makinesini izleme](./quick-monitor-azure-vm.md).


## <a name="differences-from-other-azure-resources"></a>Diğer Azure kaynaklarından farklılıklar
Azure [izleyici Ile Azure kaynaklarını izlemek](../essentials/monitor-azure-resource.md) , Azure kaynakları tarafından oluşturulan izleme verilerini ve bu verileri çözümlemek ve uyarmak Için Azure izleyici 'nin özelliklerini nasıl kullanabileceğinizi açıklar. Azure sanal makinelerinizdeki aynı izleme verilerini aşağıdaki farklılıklarla toplayabilir ve üzerinde işlem yapabilirsiniz:

-  [Platform ölçümleri](../essentials/data-platform-metrics.md) , sanal makineler için yalnızca [sanal makine Konağı](#monitoring-data)için otomatik olarak toplanır. Konuk işletim sisteminden performans verilerini toplamak için bir aracıya ihtiyacınız vardır. 
- Sanal makineler, bir Azure kaynağı içinde gerçekleştirilen işlemlere Öngörüler sağlayan [kaynak günlükleri](../essentials/platform-logs-overview.md) oluşturmaz. Konuk işletim sisteminden günlük verilerini toplamak için bir aracı kullanırsınız.
- Depolama ve Olay Hub 'ları gibi diğer hedeflere platform ölçümleri göndermek için bir sanal makine için [Tanılama ayarları](../essentials/diagnostic-settings.md) oluşturabilirsiniz, ancak bu tanılama ayarlarını Azure Portal yapılandıramazsınız. 

## <a name="monitoring-data"></a>Verileri izleme
Azure 'daki sanal makineler, aşağıdaki diyagramda gösterildiği gibi [Günlükler](../logs/data-platform-logs.md) ve [ölçümler](../essentials/data-platform-metrics.md) oluşturur.

![Genel Bakış](media/monitor-vm-azure/logs-metrics.png)


### <a name="virtual-machine-host"></a>Sanal makine Konağı
Azure 'daki sanal makineler, [izleme verileri](../essentials/monitor-azure-resource.md#monitoring-data)bölümünde açıklandığı gibi, sanal makine konağı için aşağıdaki verileri diğer Azure kaynaklarıyla aynı şekilde oluşturur.

- [Platform ölçümleri](../essentials/data-platform-metrics.md) -düzenli aralıklarla otomatik olarak toplanan ve belirli bir zamanda kaynağın bazı yönlerini tanımlayan sayısal değerlerdir. Platform ölçümleri, sanal makine konağı için toplanır, ancak Konuk işletim sistemine yönelik ölçümleri toplamak için tanılama uzantısı gerekir.
- [Etkinlik günlüğü](../essentials/platform-logs-overview.md) -abonelik içindeki her bir Azure kaynağında, dışarıdaki (Yönetim düzlemi) gerçekleştirilen işlemlere ilişkin öngörüler sağlar. Bu, bir sanal makine için, başlatıldığı zaman ve herhangi bir yapılandırma değişikliği gibi bilgileri içerir.


### <a name="guest-operating-system"></a>Konuk işletim sistemi
Bir sanal makinenin Konuk işletim sisteminden veri toplamak için, her bir sanal makinede yerel olarak çalışan ve Azure Izleyici 'ye veri gönderen bir aracıya ihtiyacınız vardır. Azure Izleyici için, her biri farklı veri toplayan ve farklı konumlara veri yazan birden çok aracı mevcuttur. [Azure izleyici aracılarına genel bakış](../agents/agents-overview.md)konusunda farklı aracıların ayrıntılı bir karşılaştırmasını alın. 

- [Log Analytics aracı](../agents/agents-overview.md#log-analytics-agent) -Azure 'da, diğer bulut ortamlarında ve Şirket içindeki sanal makinelerde kullanılabilir. Azure Izleyici günlüklerine veri toplar. VM öngörülerini ve izleme çözümlerini destekler. Bu, System Center Operations Manager için kullanılan aracıdır.
- [Bağımlılık Aracısı](../agents/agents-overview.md#dependency-agent) -sanal makinede çalışan işlemlerle ilgili verileri ve bunların bağımlılıklarını toplar. , Verileri Azure 'a aktarmak ve VM öngörülerini, Hizmet Eşlemesi ve Wire Data 2.0 çözümlerini desteklemek için Log Analytics aracısına bağımlıdır.
- [Azure tanılama uzantısı](../agents/agents-overview.md#azure-diagnostics-extension) -yalnızca Azure izleyici sanal makineleri için kullanılabilir. Verileri birden çok konuma toplayabilir, ancak birincil olarak Windows sanal makineleri için Azure Izleyici ölçümlerine konuk performans verileri toplamak için kullanılır.
- [Telegraf Agent](../essentials/collect-custom-metrics-linux-telegraf.md) -Linux VM 'Lerinden Azure Izleyici ölçümlerine performans verileri toplayın.


## <a name="configuration-requirements"></a>Yapılandırma gereksinimleri
Azure Izleyici 'nin bir sanal makineyi izlemeye yönelik tüm özelliklerini etkinleştirmek için, sanal makine Konağı ve konuk işletim sisteminden izleme verilerini hem [Azure Izleyici ölçümleri](../logs/data-platform-logs.md) hem de [Azure izleyici günlüklerine](../logs/data-platform-logs.md)toplamanız gerekir. Aşağıdaki tabloda, bu koleksiyonun etkinleştirilmesi için gerçekleştirilmesi gereken yapılandırma listelenmektedir. Bu adımların tümünü, gereksinimlerinize bağlı olarak gerçekleştirmeyi tercih edebilirsiniz.

| Yapılandırma adımı | Tamamlanan eylemler | Etkinleştirilen özellikler |
|:---|:---|:---|
| Yapılandırma yok | -Ölçümlere toplanan konak platformu ölçümleri.<br>-Etkinlik günlüğü toplandı. | -Konak için ölçüm Gezgini.<br>-Konak için ölçüm uyarıları.<br>-Etkinlik günlüğü uyarıları. |
| [VM öngörülerini etkinleştir](#enable-vm-insights) | -Log Analytics Aracısı yüklendi.<br>-Bağımlılık Aracısı yüklendi.<br>-Günlüklere toplanan konuk performansı verileri.<br>-Günlüklere toplanan işlem ve bağımlılık ayrıntıları. | -Konuk performans verileri için performans grafikleri ve çalışma kitapları.<br>-Konuk performans verileri için sorguları günlüğe kaydedin.<br>-Konuk performans verileri için günlük uyarıları.<br>-Bağımlılık eşlemesi. |
| [Tanılama uzantısı ve telegraf Aracısı 'nı yükler](#enable-diagnostics-extension-and-telegraf-agent) | -Ölçümlere toplanan konuk performansı verileri. | -Konuk için ölçüm Gezgini.<br>-Konuk için ölçüm uyarıları.  |
| [Log Analytics çalışma alanını yapılandırma](#configure-log-analytics-workspace) | -Konuktaki toplanan olaylar. | -Konuk olayları için sorguları günlüğe kaydedin.<br>-Konuk olayları için günlük uyarıları. |
| [Sanal makine için tanılama ayarı oluştur](#collect-platform-metrics-and-activity-log) | -Günlüklere toplanan platform ölçümleri.<br>-Etkinlik günlüğü günlüklere toplandı. | -Konak ölçümleri için günlük sorguları.<br>-Konak ölçümleri için günlük uyarıları.<br>-Etkinlik günlüğü için sorgular günlüğe yazılır.

Bu yapılandırma adımlarının her biri aşağıdaki bölümlerde açıklanmıştır.

### <a name="enable-vm-insights"></a>VM öngörülerini etkinleştir
[VM öngörüleri](../vm/vminsights-overview.md) , Azure izleyici 'de sanal makineleri izlemeye yönelik birincil araç olan Azure izleyici 'de bir [anlayış](../monitor-reference.md) . Standart Azure Izleyici özellikleri üzerinde aşağıdaki ek değeri sağlar.

- Sanal makine konuk işletim sistemini ve iş yüklerini izlemeyi etkinleştirmek için Log Analytics aracısının ve bağımlılık aracısının basitleştirilmesi. 
- Sanal makinenin Konuk işletim sisteminden temel performans ölçümlerini incelemenize olanak tanıyan, önceden tanımlanmış popüler performans grafikleri ve çalışma kitapları.
- Her bir sanal makinede çalışan işlem ve diğer makineler ve dış kaynaklarla bağlantılı bileşenler görüntüleyen bağımlılık eşlemesi.

![VM öngörüleri performans görünümü](media/monitor-vm-azure/vminsights-01.png)

![VM Insights haritaları görünümü](media/monitor-vm-azure/vminsights-02.png)


Azure portal sanal makine menüsündeki **Öngörüler** seçeneğinden VM öngörülerini etkinleştirin. Ayrıntılar ve diğer yapılandırma yöntemleri için bkz. [VM öngörülerine genel bakış](vminsights-enable-overview.md) .

![VM öngörülerini etkinleştir](media/monitor-vm-azure/enable-vminsights.png)

### <a name="configure-log-analytics-workspace"></a>Log Analytics çalışma alanını yapılandırma
VM öngörüleri tarafından kullanılan Log Analytics Aracısı verileri bir [Log Analytics çalışma alanına](../logs/data-platform-logs.md)gönderir. Log Analytics çalışma alanını yapılandırarak ek performans verileri, olaylar ve diğer izleme verilerini aracıdan etkinleştirebilirsiniz. Çalışma alanına bağlanan herhangi bir aracı yapılandırmayı otomatik olarak indirecek ve tanımlanan verileri toplamaya başladıktan sonra yalnızca bir kez yapılandırılması gerekir. 

**Kullanmaya başlama** alanı **yapılandırması** ' nı seçerek, çalışma alanının yapılandırmasına doğrudan VM öngörülerine erişebilirsiniz. Menüsünü açmak için çalışma alanı adına tıklayın.

![Çalışma alanı yapılandırması](media/monitor-vm-azure/workspace-configuration.png)

Çalışma alanı menüsünden **Gelişmiş ayarlar** ' ı ve ardından veri kaynaklarını yapılandırmak için **veriler** ' i seçin. Windows aracıları için **Windows olay günlükleri** ' ni seçin ve *sistem* ve *uygulama* gibi ortak olay günlükleri ekleyin. Linux aracıları için **Syslog** ' ı seçin ve *Aralık* ve *Daemon* gibi ortak tesisler ekleyin. Kullanılabilir veri kaynaklarının bir listesi ve bunları yapılandırma hakkında ayrıntılı bilgi için bkz. [Azure izleyici 'de aracı veri kaynakları](../agents/agent-data-sources.md) . 

![Olayları yapılandırma](media/monitor-vm-azure/configure-events.png)


> [!NOTE]
> Performans sayaçlarını çalışma alanı yapılandırmasından toplanacak şekilde yapılandırabilirsiniz, ancak bu, VM öngörüleri tarafından toplanan performans sayaçlarıyla yedekli olabilir. VM öngörüleri, en yaygın sayaç kümesini dakikada bir kez bir sıklıkta toplar. Yalnızca VM öngörüleri tarafından henüz toplanmayan sayaçları toplamak istiyorsanız veya performans verilerini kullanarak mevcut sorgular varsa, yalnızca çalışma alanı tarafından toplanacak performans sayaçlarını yapılandırın.


### <a name="enable-diagnostics-extension-and-telegraf-agent"></a>Tanılama uzantısı ve telegraf Aracısı 'nı etkinleştir
VM öngörüleri, bir Log Analytics çalışma alanına veri gönderen Log Analytics aracısına dayalıdır. Bu, [günlük sorguları](../logs/log-query-overview.md), [günlük uyarıları](../alerts/alerts-log.md)ve [çalışma kitapları](../visualize/workbooks-overview.md)gibi Azure izleyici 'nin birden çok özelliğini destekler. [Tanılama uzantısı](../agents/diagnostics-extension-overview.md) , Windows sanal makinelerinin Konuk Işletim sisteminden Azure depolama 'ya performans verilerini toplar ve isteğe bağlı olarak [Azure izleyici ölçümlerine](../essentials/data-platform-metrics.md)performans verileri gönderir. Linux sanal makineleri için, [telegraf Aracısı](../essentials/collect-custom-metrics-linux-telegraf.md) Azure ölçümlerine veri göndermek için gereklidir.  Bu, Azure Izleyici 'nin [Ölçüm Gezgini](../essentials/metrics-getting-started.md) ve [ölçüm uyarıları](../alerts/alerts-metric.md)gibi diğer özelliklerine izin vermez. Tanılama uzantısını Azure Event Hubs kullanarak Azure Izleyici dışında olay ve performans verilerini gönderecek şekilde de yapılandırabilirsiniz.

Tek bir Windows sanal makinesi için tanılama uzantısı 'nı, VM menüsündeki **Tanılama ayarı** seçeneğinden Azure Portal. **Havuzlar** sekmesinde **Azure izleyicisini** etkinleştirme seçeneğini belirleyin. Uzantıyı bir şablondan veya komut satırından birden çok sanal makineye etkinleştirmek için bkz. [yükleme ve yapılandırma](../agents/diagnostics-extension-overview.md#installation-and-configuration). Log Analytics aracısının aksine, toplanacak veriler her bir sanal makinedeki uzantının yapılandırmasında tanımlanmıştır.

![Tanılama ayarı](media/monitor-vm-azure/diagnostic-setting.png)

Linux sanal makinelerinde telegraf aracılarını yapılandırma hakkında daha fazla bilgi için bkz. [telegraf for Install and configure](../essentials/collect-custom-metrics-linux-telegraf.md#install-and-configure-telegraf) . **Tanılama ayarı** menü seçeneği Linux için kullanılabilir, ancak yalnızca Azure Storage 'a veri göndermenize izin verir.

### <a name="collect-platform-metrics-and-activity-log"></a>Platform ölçümlerini ve etkinlik günlüğünü toplayın
Azure portal her bir sanal makine konağı için toplanan platform ölçümlerini ve etkinlik günlüğünü görüntüleyebilirsiniz. Bu verileri, sanal makine için toplanan diğer izleme verileriyle çözümlemek üzere VM Öngörüler ile aynı Log Analytics çalışma alanına toplayın. Bu koleksiyon bir [Tanılama ayarıyla](../essentials/diagnostic-settings.md)yapılandırıldı. [Abonelik için bir tanılama ayarıyla](../essentials/diagnostic-settings.md#create-in-azure-portal)etkinlik günlüğünü toplayın.

Sanal makine için bir tanılama ayarıyla platform ölçümleri toplayın. Diğer Azure kaynaklarından farklı olarak, Azure portal bir sanal makine için tanılama ayarı oluşturamazsınız, ancak [başka bir yöntem](../essentials/diagnostic-settings.md#create-using-powershell)kullanmanız gerekir. Aşağıdaki örneklerde, PowerShell ve CLı kullanarak bir sanal makine için ölçümlerin nasıl toplanacağı gösterilmektedir.

```powershell
Set-AzDiagnosticSetting -Name vm-diagnostics -ResourceId "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/my-resource-group/providers/Microsoft.Compute/virtualMachines/my-vm" -Enabled $true -MetricCategory AllMetrics -workspaceId "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/my-resource-group/providers/microsoft.operationalinsights/workspaces/my-workspace"
```

```azurecli
az monitor diagnostic-settings create \
--name VM-Diagnostics 
--resource /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/my-resource-group/providers/Microsoft.Compute/virtualMachines/my-vm \
--metrics '[{"category": "AllMetrics","enabled": true}]' \
--workspace /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/my-resource-group/providers/microsoft.operationalinsights/workspaces/my-workspace
```

## <a name="monitoring-in-the-azure-portal"></a>Azure portal izleme 
Bir sanal makine için izleme verileri koleksiyonunu yapılandırdıktan sonra, Azure portal erişmek için birden çok seçeneğiniz vardır:

- İzlenen tüm kaynaklardaki verilere erişmek için **Azure izleyici** menüsünü kullanın. 
- Ölçekteki sanal makine kümelerini izlemek için VM öngörülerini kullanın.
- Tek bir sanal makine için verileri, Azure portal menüsündeki menüden çözümleyin. Aşağıdaki tabloda, sanal makineler menüsünü izlemeye yönelik farklı seçenekler listelenmektedir.

![Azure portal izleme](media/monitor-vm-azure/monitor-menu.png)

| Menü seçeneği | Açıklama |
|:---|:---|
| Genel Bakış | Sanal makine konağı için [Platform ölçümlerini](../essentials/data-platform-metrics.md) görüntüler. [Ölçüm Gezgini](../essentials/metrics-getting-started.md)'nde bu verilerle çalışmak için bir grafiğe tıklayın. |
| Etkinlik günlüğü | Geçerli sanal makine için filtrelenmiş [etkinlik günlüğü](../essentials/activity-log.md#view-the-activity-log) girdileri. |
| Insights | Seçilen geçerli sanal makinenin haritası ile [VM öngörülerini](../vm/vminsights-overview.md) açar. |
| Uyarılar | Geçerli sanal makine için [uyarıları](../alerts/alerts-overview.md) görüntüler.  |
| Ölçümler | [Ölçüm Gezgini](../essentials/metrics-getting-started.md) ' ni geçerli sanal makineye ayarlı kapsam ile açın. |
| Tanılama ayarları | Geçerli sanal makine için [Tanılama uzantısını](../agents/diagnostics-extension-overview.md) etkinleştirin ve yapılandırın. |
| Danışman önerileri | [Azure Advisor](../../advisor/index.yml)'ın geçerli sanal makinesine yönelik öneriler. |
| Günlükler | [Kapsamın](../logs/scope.md) geçerli sanal makineye ayarlandığı [Log Analytics](../logs/log-analytics-overview.md) açın. |
| Bağlantı İzleyicisi | Geçerli sanal makine ile diğer sanal makineler arasındaki bağlantıları izlemek için [ağ Izleyicisi bağlantı izleyicisini](../../network-watcher/connection-monitor-overview.md) açın. |


## <a name="analyzing-metric-data"></a>Ölçüm verileri çözümleniyor
Sanal makine menüsünden **ölçümler** ' i açarak, Ölçüm Gezgini 'ni kullanarak sanal makineler için ölçümleri çözümleyebilirsiniz. Bu aracı kullanma hakkında ayrıntılı bilgi için bkz. [Azure Ölçüm Gezgini](../essentials/metrics-getting-started.md) kullanmaya başlama. 

Ölçümler için sanal makineler tarafından kullanılan üç ad alanı vardır:

| Ad Alanı | Açıklama | Gereksinim |
|:---|:---|:---|
| Sanal Makine Ana Bilgisayarı | Tüm Azure sanal makineleri için otomatik olarak toplanan konak ölçümleri. [Microsoft. COMPUTE/virtualMachines](../essentials/metrics-supported.md#microsoftcomputevirtualmachines)'teki ayrıntılı ölçüm listesi. | Yapılandırma gerekmeden otomatik olarak toplanır. |
| Konuk (klasik) | Sınırlı bir konuk işletim sistemi ve uygulama performansı verileri kümesi. Ölçüm uyarıları gibi diğer Azure Izleyici özellikleriyle Ölçüm Gezgini 'nde kullanılabilir.  | [Tanılama uzantısı](../agents/diagnostics-extension-overview.md) yüklendi. Veriler Azure depolama alanından okundu.  |
| Sanal makine konuğu | Konuk işletim sistemi ve uygulama performansı verileri, ölçümler kullanılarak tüm Azure Izleyici özellikleri için kullanılabilir. | Windows için [Tanılama uzantısı](../agents/diagnostics-extension-overview.md) , Azure izleyici havuzu etkin olarak yüklendi. Linux için [telegraf Aracısı yüklenir](../essentials/collect-custom-metrics-linux-telegraf.md). |

![Azure portal Ölçüm Gezgini](media/monitor-vm-azure/metrics.png)

## <a name="analyzing-log-data"></a>Günlük verileri çözümleniyor
Azure sanal makineleri, Azure Izleyici günlüklerine aşağıdaki verileri toplar. 

VM öngörüleri, *ınsightsmetriler* tablosuna yazılan önceden belirlenmiş bir dizi performans sayacı koleksiyonu sağlar. Bu, [kapsayıcı öngörüleri](../containers/container-insights-overview.md)tarafından kullanılan tablodır. 

| Veri kaynağı | Gereksinimler | Tablolar |
|:---|:---|:---|
| VM öngörüleri | Her bir sanal makinede etkinleştirin. | Insightsölçümlerini<br>VMBoundPort<br>VMComputer<br>VMConnection<br>VMProcess<br>Ayrıntılar için bkz. [VM öngörülerinin günlüklerini sorgulama](../vm/vminsights-log-search.md) . |
| Etkinlik günlüğü | Abonelik için tanılama ayarı. | AzureActivity |
| Konak ölçümleri | Sanal makine için tanılama ayarı. | AzureMetrics |
| Konuk işletim sisteminden veri kaynakları | Log Analytics aracısını etkinleştirin ve veri kaynaklarını yapılandırın. | Her veri kaynağı için belgelere bakın. |


> [!NOTE]
> Log Analytics Aracısı tarafından toplanan performans verileri, VM öngörüleri bunu *ınsightsölçümlerini* tablosu 'nda toplarken *performans tablosuna yazar* . Bu veriler aynıdır, ancak tablolar farklı bir yapıya sahiptir. *Perf* tabanlı mevcut sorgulara sahipseniz, *ınsightsölçümlerini* kullanmak için yeniden yazılması gerekir.


## <a name="alerts"></a>Uyarılar
Azure Izleyici 'deki [Uyarılar](../alerts/alerts-overview.md) , izleme verilerinizde önemli koşullar bulunduğunda ve potansiyel olarak mantıksal uygulama başlatma ya da bir Web kancası çağırma gibi bir eylem başladığınızda size önceden bildirimde bulunur. Uyarı kuralları, bir uyarının ne zaman oluşturulması gerektiğini belirlemek için kullanılan mantığı tanımlar. Azure Izleyici, uyarı kuralları tarafından kullanılan verileri toplar, ancak Azure aboneliğinizde uyarı koşullarını tanımlamak için kurallar oluşturmanız gerekir.

Aşağıdaki bölümlerde, her birini kullanmanız gereken uyarı kurallarının türleri ve önerileri açıklanır. Bu öneri, uyarı kuralı türünün işlevlerine ve maliyetine göre belirlenir. Uyarıların Ayrıntılar fiyatlandırması için bkz. [Azure izleyici fiyatlandırması](https://azure.microsoft.com/pricing/details/monitor/).


### <a name="activity-log-alert-rules"></a>Etkinlik günlüğü uyarı kuralları
Etkinlik günlüğünde belirli ölçütlerle eşleşen bir girdi oluşturulduğunda, [etkinlik günlüğü uyarı kuralları](../alerts/alerts-activity-log.md) ateşlenir. İhtiyaç duyduğunuz mantık etkinlik günlüğünde ise, ilk seçiminiz olması gereken bir maliyeti yoktur. 

Etkinlik günlüğü uyarıları için hedef kaynak belirli bir sanal makine, bir kaynak grubundaki tüm sanal makineler veya bir abonelikteki tüm sanal makineler olabilir.

Örneğin, sinyal adı için *sanal makineyi kapat* ' ı seçerek kritik bir sanal makine durdurulmuşsa bir uyarı oluşturun.

![Etkinlik günlüğü uyarısı](media/monitor-vm-azure/activity-log-alert.png)


### <a name="metric-alert-rules"></a>Ölçüm uyarı kuralları
Ölçüm değeri bir eşiği aştığında, [ölçüm uyarı kuralları](../alerts/alerts-metric.md) ateşlenir. Belirli bir eşik değeri tanımlayabilir veya Azure Izleyici 'nin geçmiş verilere göre bir eşiği dinamik olarak belirlemesine izin verebilirsiniz.  Ölçüm verileriyle mümkün olduğunca, daha az maliyetli ve günlük uyarısı kurallarından daha fazla yanıt verebileceğinden ölçüm uyarılarını kullanın. Ayrıca, ölçüm eşiğin altına düştüğünde kendilerini çözebilecekleri de durum bilgisi vardır.

Etkinlik günlüğü uyarıları için hedef kaynak belirli bir sanal makine veya bir kaynak grubundaki tüm sanal makineler olabilir.

Örneğin, bir sanal makinenin işlemcisi belirli bir değeri aştığında bir uyarı oluşturmak için, sinyal türü olarak *Yüzde CPU 'yu* kullanarak bir ölçüm uyarı kuralı oluşturun. Belirli bir eşik değeri ayarlayın veya Azure Izleyici 'nin dinamik bir eşik ayarı yapmasına izin verin. 

![Ölçüm uyarısı](media/monitor-vm-azure/metric-alert.png)

### <a name="log-alerts"></a>Günlük uyarıları
Zamanlanan günlük sorgusunun sonuçları belirli ölçütlerle eşleşiyorsa, [günlük uyarı kuralları](../alerts/alerts-log.md) ateşlenir. Günlük sorgusu uyarıları, uyarı kurallarının en pahalı ve en az yanıt verişlidir, ancak en çok farklı verilere erişebilir ve diğer uyarı kuralları tarafından gerçekleştirilemediği karmaşık mantık gerçekleştirebilir. 

Günlük sorgusunun hedef kaynağı bir Log Analytics çalışma alanıdır. Sorgudaki belirli bilgisayarlar için filtre uygulayın.

Örneğin, belirli bir kaynak grubundaki herhangi bir sanal makinenin çevrimdışı olup olmadığını kontrol eden bir uyarı oluşturmak için, son on dakikada bir sinyal kaçırıldığı her bilgisayar için bir kayıt döndüren aşağıdaki sorguyu kullanın. En az bir bilgisayar kaçırılmış bir sinyal içeriyorsa, 1 eşiğini kullanın.

```kusto
Heartbeat
| where TimeGenerated > ago(10m)
| where ResourceGroup == "my-resource-group"
| summarize max(TimeGenerated) by Computer
```

![Kaçırılan sinyal için günlük uyarısı](media/monitor-vm-azure/log-alert-01.png)

Abonelikteki herhangi bir Windows sanal makinesi üzerinde çok fazla sayıda başarısız oturum açma işlemi gerçekleştiyse uyarı oluşturmak için, Son saatteki her başarısız oturum açma olayı için bir kayıt döndüren aşağıdaki sorguyu kullanın. İzin verilen başarısız oturum açma sayısı için bir eşik kümesi kullanın. 

```kusto
Event
| where TimeGenerated > ago(1hr)
| where EventID == 4625
```

![Başarısız oturum açmalar için günlük uyarısı](media/monitor-vm-azure/log-alert-02.png)


## <a name="system-center-operations-manager"></a>System Center Operations Manager
System Center Operations Manager, sanal makinelerde iş yüklerinin ayrıntılı bir şekilde izlenmesini sağlar. İzleme platformlarının ve uygulama için farklı stratejilerin karşılaştırması için bkz. [bulut Izleme Kılavuzu](/azure/cloud-adoption-framework/manage/monitor/) .

Kullanmaya devam etmek istediğiniz mevcut bir Operations Manager ortamınız varsa, ek işlevsellik sağlamak için Azure Izleyici ile tümleştirilebilir. Azure Izleyici tarafından kullanılan Log Analytics Aracısı, her ikisine de veri gönderen izlenen sanal makineler olacak şekilde, Operations Manager için kullanılan bir aracısıdır. Yine de aracıyı VM Öngörüler 'e eklemeniz ve çalışma alanını yukarıda belirtilen ek verileri toplayacak şekilde yapılandırmanız gerekir, ancak sanal makineler, mevcut yönetim paketlerini değişiklik yapmadan bir Operations Manager ortamında çalıştırmaya devam edebilir.

Mevcut Operations Manager özelliklerini geliştiren Azure Izleyici özellikleri şunlardır:

- Günlük ve performans verilerinizi etkileşimli olarak analiz etmek için Log Analytics kullanın.
- Birden çok sanal makine genelinde uyarı koşullarını tanımlamak ve Operations Manager uyarıları kullanarak mümkün olmayan uzun süreli eğilimleri kullanmak için günlük uyarılarını kullanın.   

Mevcut Operations Manager Yönetimi grubunuzu Log Analytics çalışma alanınıza bağlama hakkında daha fazla bilgi için bkz. [Azure izleyici 'ye Operations Manager bağlama](../agents/om-agents.md) .


## <a name="next-steps"></a>Sonraki adımlar

* [Günlük sorgularını kullanarak Azure Izleyici günlüklerinde verileri çözümlemeyi öğrenin.](../logs/get-started-queries.md)
* [Azure Izleyici 'de ölçümleri ve günlükleri kullanarak uyarılar hakkında bilgi edinin.](../alerts/alerts-overview.md)