---
title: Application Insights Profiler ' ı kullanarak bir Azure sanal makinesinde çalışan web uygulamalarının profilini | Microsoft Docs
description: Application Insights Profiler ' ı kullanarak bir Azure VM üzerinde Web apps profil.
services: application-insights
documentationcenter: ''
author: cweining
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.reviewer: mbullwin
ms.date: 08/06/2018
ms.author: cweining
ms.openlocfilehash: 01e5ffc7cf00f7d0e0e97f521b6b6de2abd56567
ms.sourcegitcommit: 3f4ffc7477cff56a078c9640043836768f212a06
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2019
ms.locfileid: "57307692"
---
# <a name="profile-web-apps-running-on-an-azure-virtual-machine-or-a-virtual-machine-scale-set-by-using-application-insights-profiler"></a>Application Insights Profiler ' ı kullanarak bir Azure sanal makine veya sanal makine ölçek çalışan profili web uygulamalarını ayarlama

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Ayrıca, bu hizmetleri Azure Application Insights Profiler dağıtabilirsiniz:
* [Azure App Service](../../azure-monitor/app/profiler.md?toc=/azure/azure-monitor/toc.json)
* [Azure Cloud Services](profiler-cloudservice.md?toc=/azure/azure-monitor/toc.json)
* [Azure Service Fabric](profiler-vm.md?toc=/azure/azure-monitor/toc.json)

## <a name="deploy-profiler-on-a-virtual-machine-or-a-virtual-machine-scale-set"></a>Bir sanal makine veya sanal makine ölçek kümesi üzerinde Profiler'ı dağıtma
Bu makalede, Azure sanal makinesi (VM) veya Azure sanal makine ölçek kümesi üzerinde çalışan Application Insights Profiler ' ı almak nasıl gösterir. Profiler VM'ler için Azure tanılama uzantısı ile yüklenir. Profiler'ı çalıştırmak için uzantısını yapılandırın ve uygulamanıza Application Insights SDK'sını derleme.

1. Application Insights SDK'sini ekleyin, [ASP.NET uygulaması](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net) veya normal [.NET uygulaması](windows-services.md?toc=/azure/azure-monitor/toc.json).  
  İsteklerinizi profillerini görüntülemek için Application Insights istek telemetrisi göndermeniz gerekir.

1. Azure tanılama uzantısı, sanal Makinenize yükleyin. Tam Resource Manager şablonu örnekleri için bkz:  
    * [Sanal makine](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachine.json)
    * [Sanal makine ölçek kümesi](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachineScaleSet.json)
    
    ApplicationInsightsProfilerSink WadCfg içinde önemli parçasıdır. Azure tanılama için iKey veri göndermek Profiler'ı etkinleştirmek için başka bir havuz bu bölümüne ekleyin.
    
    ```json
      "SinksConfig": {
        "Sink": [
          {
            "name": "ApplicationInsightsSink",
            "ApplicationInsights": "85f73556-b1ba-46de-9534-606e08c6120f"
          },
          {
            "name": "MyApplicationInsightsProfilerSink",
            "ApplicationInsightsProfiler": "85f73556-b1ba-46de-9534-606e08c6120f"
          }
        ]
      },
    ```

1. Değiştirilmiş ortam dağıtım tanımı dağıtın.  

   Tam şablon dağıtımı değişiklikler genellikle uygulama içerir veya PowerShell cmdlet'lerini veya Visual Studio bir bulut hizmeti tabanlı yayımlama.  

   Aşağıdaki PowerShell komutlarını yalnızca Azure tanılama uzantısını üzerinde alternatif bir yaklaşım mevcut sanal makineler var. Daha önce bahsedilen ProfilerSink Get-AzVMDiagnosticsExtension komutu tarafından döndürülen yapılandırmasına ekleyin ve ardından güncelleştirilmiş config Set-AzVMDiagnosticsExtension Command'e.

    ```powershell
    $ConfigFilePath = [IO.Path]::GetTempFileName()
    # After you export the currently deployed Diagnostics config to a file, edit it to include the ApplicationInsightsProfiler sink.
    (Get-AzVMDiagnosticsExtension -ResourceGroupName "MyRG" -VMName "MyVM").PublicSettings | Out-File -Verbose $ConfigFilePath
    # Set-AzVMDiagnosticsExtension might require the -StorageAccountName argument
    # If your original diagnostics configuration had the storageAccountName property in the protectedSettings section (which is not downloadable), be sure to pass the same original value you had in this cmdlet call.
    Set-AzVMDiagnosticsExtension -ResourceGroupName "MyRG" -VMName "MyVM" -DiagnosticsConfigurationPath $ConfigFilePath
    ```

1. Hedeflenen uygulama çalışıyorsa [IIS](https://www.microsoft.com/web/downloads/platform.aspx), etkinleştirme `IIS Http Tracing` Windows özelliği.

   a. Ortam için uzaktan erişim'kurmak ve sonra [Ekle Windows özellikleri]( https://docs.microsoft.com/iis/configuration/system.webserver/tracing/) penceresi. Veya, (yönetici olarak) PowerShell'de aşağıdaki komutu çalıştırın:  

    ```powershell
    Enable-WindowsOptionalFeature -FeatureName IIS-HttpTracing -Online -All
    ```  
   b. Uzaktan erişim, bir sorun olduğunu oluşturma, kullanabileceğiniz [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) şu komutu çalıştırın:  

    ```powershell
    az vm run-command invoke -g MyResourceGroupName -n MyVirtualMachineName --command-id RunPowerShellScript --scripts "Enable-WindowsOptionalFeature -FeatureName IIS-HttpTracing -Online -All"
    ```

1. Uygulamanızı dağıtın.

## <a name="can-profiler-run-on-on-premises-servers"></a>Profiler, şirket içi sunucularda çalıştırabilir miyim?
Şirket içi sunucular için Application Insights Profiler ' ı desteklemek için hiçbir plan sahibiz.

## <a name="next-steps"></a>Sonraki adımlar

- Uygulamanız için trafiği oluşturur (örneğin, başlatma bir [kullanılabilirlik testi](monitor-web-app-availability.md)). Ardından, izlemeleri Application Insights örneğine gönderilmek üzere başlatmak 10-15 dakika bekleyin.
- Bkz: [Profiler izlemeleri](profiler-overview.md?toc=/azure/azure-monitor/toc.json) Azure portalında.
- Profiler sorunlarını giderme konusunda yardım için bkz: [sorun giderme Profiler](profiler-troubleshooting.md?toc=/azure/azure-monitor/toc.json).
