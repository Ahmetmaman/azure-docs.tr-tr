---
title: Azure Service Fabric - kümesi ile Azure İzleyici günlüklerine izleme | Microsoft Docs
description: Azure Service Fabric kümeleriniz izlemek için Azure İzleyici günlüklerine olayları analiz ve görselleştirme için ayarlama konusunda bilgi edinin.
services: service-fabric
documentationcenter: .net
author: srrengar
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/20/2019
ms.author: srrengar
ms.openlocfilehash: 33984b084023a3a2c31b6f6a0a7fc8a95c2d7689
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2019
ms.locfileid: "57242862"
---
# <a name="set-up-azure-monitor-logs-for-a-cluster"></a>Azure İzleyici günlüklerine kümesi için ayarlayın

Azure İzleyici günlüklerine küme düzeyi olayları izlemek için Bizim önerimiz olur. Azure Resource Manager, PowerShell veya Azure Market üzerinden Log Analytics çalışma alanı kurabilir. Güncelleştirilmiş bir Resource Manager şablonu gelecekte kullanım için dağıtımınızın korumak, Azure İzleyici günlüklerine ortamınızı ayarlamak üzere aynı şablonu kullanın. Tanılamayı ile dağıtılan bir küme zaten varsa, Market aracılığıyla dağıtımı çok kolaydır. Abonelik düzeyinde erişimi için dağıttığınız hesabı yoksa, PowerShell veya Resource Manager şablonu kullanarak dağıtın.

> [!NOTE]
> Kümenizi izlemek için Azure İzleyici günlüklerini ayarlamak için küme düzeyi veya platform düzeyi olayları görüntülemek için tanılamayı etkinleştirmeniz gerekir. Başvurmak [tanılama Windows kümelerinde kurma](service-fabric-diagnostics-event-aggregation-wad.md) ve [Linux kümelerinde tanılama kurma](service-fabric-diagnostics-event-aggregation-lad.md) daha fazla bilgi için

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="deploy-a-log-analytics-workspace-by-using-azure-marketplace"></a>Azure Marketi kullanarak Log Analytics çalışma alanı dağıtma

Bir küme dağıttıktan sonra bir Log Analytics çalışma alanı eklemek istiyorsanız, portalda Azure Market'e dön ve Ara **Service Fabric analizi**. Service Fabric'e özgü veriler içeren özel bir çözüm için Service Fabric dağıtımları budur. Bu işlem çözüm (öngörülerini görüntülemek için Pano) ve çalışma alanı (temel alınan küme veri toplama) oluşturur.

1. Seçin **yeni** sol gezinti menüsünde. 

2. Arama **Service Fabric analizi**. Görüntülenen kaynağı seçin.

3. **Oluştur**’u seçin.

    ![Service Fabric analizi pazarında](media/service-fabric-diagnostics-event-analysis-oms/service-fabric-analytics.png)

4. Service Fabric analizi oluşturma penceresinde **bir çalışma alanı seçin** için **OMS çalışma alanı** alan ve ardından **yeni bir çalışma alanı oluşturma**. Gerekli girişleri doldurun. Service Fabric kümesi ve çalışma alanı için abonelik aynı olduğunu burada tek gereksinim olmasıdır. Girdilerinizi doğrulandı, çalışma alanınızı dağıtma başlar. Dağıtım, yalnızca birkaç dakika sürer.

5. İşiniz bittiğinde seçin **Oluştur** Service Fabric analizi oluşturma penceresinin altındaki yeniden. Yeni çalışma alanı altında gösterildiğini doğrulayın **OMS çalışma alanı**. Bu eylem, oluşturduğunuz çalışma alanına çözüme ekler.

Windows kullanıyorsanız, Azure İzleyici günlüklerine küme olaylarınızı depolandığı depolama hesabına bağlanmak için aşağıdaki adımlarla devam edin. 

>[!NOTE]
>Bu deneyimini Linux kümeleri için henüz kullanılamıyor. 

### <a name="connect-the-log-analytics-workspace-to-your-cluster"></a>Log Analytics çalışma alanı kümenize bağlanın 

1. Çalışma alanı kümenizden gelen Tanılama verileri bağlanması gerekir. Service Fabric analizi çözümü oluşturduğunuz kaynak grubuna gidin. Seçin **ServiceFabric\<nameOfWorkspace\>**  ve kendi genel bakış sayfasına gidin. Burada, çözüm ayarları, çalışma alanı ayarları değiştirin ve Log Analytics çalışma alanına erişim.

2. Sol gezinti menüsünde altında **çalışma alanı veri kaynakları**seçin **depolama hesabı günlükleri**.

3. Üzerinde **depolama hesabı günlükleri** sayfasında **Ekle** kümenizin günlükleri çalışma alanına eklemek için üst.

4. Seçin **depolama hesabı** kümenizde oluşturulan uygun hesabı eklemek için. Varsayılan ad kullandıysanız, depolama hesabıdır **sfdg\<resourceGroupName\>**. Ayrıca bu, kümeniz için kullanılan değerini denetleyerek dağıtmak için kullanılan Azure Resource Manager şablonu ile doğrulayabilirsiniz **applicationDiagnosticsStorageAccountName**. Adı görünmez, aşağıya kaydırın ve **daha fazla Yükle**. Depolama hesabı adını seçin.

5. Veri türü belirtin. Ayarlayın **Service Fabric olayları**.

6. Kaynağı otomatik olarak ayarlandığından emin olun **WADServiceFabric\*EventTable**.

7. Seçin **Tamam** kümenizin günlükleri için çalışma alanınızı bağlamak için.

    ![Depolama hesabı günlükleri, Azure İzleyici günlüklerine Ekle](media/service-fabric-diagnostics-event-analysis-oms/add-storage-account.png)

Depolama hesabınızın parçası çalışma alanınızın veri kaynaklarında oturum gibi hesap artık gösterilir.

Service Fabric analizi çözümü, artık doğru kümenizin platform ve Uygulama günlüğünü tabloya bağlandığından bir Log Analytics çalışma alanında ekledik. Ek kaynaklar aynı şekilde çalışma alanına ekleyebilirsiniz.


## <a name="deploy-azure-monitor-logs-with-azure-resource-manager"></a>Azure İzleyici günlüklerine Azure Resource Manager ile dağıtma

Resource Manager şablonu kullanarak bir küme dağıttığınızda, şablonun yeni bir Log Analytics çalışma alanı oluşturur, Service Fabric çözümü çalışma alanına ekler ve uygun depolama tablolardan verileri okumak için yapılandırır.

Kullanabilir ve değiştirme [Bu örnek şablonu](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Windows-OMS-UnSecure) gereksinimlerinizi karşılayacak şekilde. Bu şablon şunları yapar

* 5 düğümlü Service Fabric kümesi oluşturur
* Bir Log Analytics çalışma alanı ve Service Fabric çözümü oluşturur
* Toplamak ve çalışma alanı için 2 örnek performans sayaçları göndermek için Log Analytics aracısını yapılandırır
* Service Fabric toplanacak WAD yapılandırır ve bunları Azure depolama tabloları gönderir (WADServiceFabric * EventTable)
* Bu tablodan olayları okumak için Log Analytics çalışma alanı yapılandırır


Kullanarak kümenize bir Resource Manager yükseltme şablonu dağıtabilirsiniz `New-AzureRmResourceGroupDeployment` AzureRM PowerShell modülü API. Bir örnek komut şöyle olabilir:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName "<resourceGroupName>" -TemplateFile "<templatefile>.json" 
``` 

Azure Resource Manager, bu komut bir güncelleştirme var olan bir kaynak olduğunu algılar. Yalnızca, mevcut dağıtım sürüş şablonu ve sağlanan yeni şablonu arasındaki değişiklikleri işler.

## <a name="deploy-azure-monitor-logs-with-azure-powershell"></a>Azure İzleyici günlüklerine Azure PowerShell ile dağıtma

Kullanarak log analytics kaynağınızı PowerShell aracılığıyla dağıtabilirsiniz `New-AzureRmOperationalInsightsWorkspace` komutu. Bu yöntemi kullanmak için yüklediğinizden emin olun [Azure PowerShell](https://docs.microsoft.com/powershell/azure/azurerm/install-azurerm-ps?view=azurermps-5.1.1). Yeni bir Log Analytics çalışma alanı oluşturma ve Service Fabric çözümü eklemek için bu betiği kullanın: 

```PowerShell

$SubID = "<subscription ID>"
$ResourceGroup = "<Resource group name>"
$Location = "<Resource group location>"
$WorkspaceName = "<Log Analytics workspace name>"
$solution = "ServiceFabric"

# Log in to Azure and access the correct subscription
Connect-AzureRmAccount
Select-AzureRmSubscription -SubscriptionId $SubID 

# Create the resource group if needed
try {
    Get-AzureRmResourceGroup -Name $ResourceGroup -ErrorAction Stop
} catch {
    New-AzureRmResourceGroup -Name $ResourceGroup -Location $Location
}

New-AzureRmOperationalInsightsWorkspace -Location $Location -Name $WorkspaceName -Sku Standard -ResourceGroupName $ResourceGroup
Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -IntelligencePackName $solution -Enabled $true

```

İşiniz bittiğinde, Azure İzleyici günlüklerine uygun bir depolama hesabına bağlanmak için önceki bölümdeki adımları izleyin.

Ayrıca, diğer çözümler ekleyin veya PowerShell kullanarak Log Analytics çalışma alanınıza diğer değişiklikleri yapın. Daha fazla bilgi için bkz. [Azure İzleyici'yi yönetmek, PowerShell kullanarak günlüklerini](../azure-monitor/platform/powershell-workspace-configuration.md).

## <a name="next-steps"></a>Sonraki adımlar
* [Log Analytics aracısını dağıtmayı](service-fabric-diagnostics-oms-agent.md) üzerine düğümlerinizi performans sayaçları toplamak ve docker istatistikleri ve kapsayıcılarınızı için günlükleri toplamak için
* Analytics'in [günlük arama ve sorgulama](../log-analytics/log-analytics-log-searches.md) özellikleri, Azure İzleyici günlüklerine bir parçası olarak sunulan
* [Azure İzleyici günlüklerine özel görünümlerini oluşturma için Görünüm Tasarımcısı'nı kullanın](../azure-monitor/platform/view-designer.md)
