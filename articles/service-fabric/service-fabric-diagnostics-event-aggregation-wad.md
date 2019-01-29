---
title: Azure Service Fabric olay toplama ile Windows Azure tanılama | Microsoft Docs
description: Toplama ve izleme ve tanılama Azure Service Fabric kümelerinin WAD kullanarak olaylarını toplama hakkında bilgi edinin.
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
ms.date: 04/03/2018
ms.author: srrengar
ms.openlocfilehash: 89cd8e85c9902bb1caeedd80240811f59ebec409
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55187445"
---
# <a name="event-aggregation-and-collection-using-windows-azure-diagnostics"></a>Olay toplama ve Windows Azure Tanılama'yı kullanarak koleksiyon
> [!div class="op_single_selector"]
> * [Windows](service-fabric-diagnostics-event-aggregation-wad.md)
> * [Linux](service-fabric-diagnostics-event-aggregation-lad.md)
>
>

Bir Azure Service Fabric kümesi çalıştırırken, merkezi bir konumda tüm düğümlerden günlükleri toplamak için iyi bir fikirdir. Günlükleri sahip merkezi bir konumda, kümenizdeki sorunları veya uygulamalar ve hizmetler, kümede çalışan sorunları gidermek ve çözümlemenize yardımcı olur.

Karşıya yükleme ve günlükleri toplamak için bir yolu, günlükleri, Azure Depolama'ya yükler ve ayrıca Azure Application Insights veya olay hub'larına günlükleri gönderme seçeneği olan Windows Azure tanılama (WAD) uzantısı kullanmaktır. Olayları depolamadan okuyun ve bunları bir analiz platformu ürün gibi yerleştirmek için bir dış işlem kullanabilirsiniz [Log Analytics](../log-analytics/log-analytics-service-fabric.md) veya başka bir günlük ayrıştırma çözümü.

## <a name="prerequisites"></a>Önkoşullar
Bu makalede aşağıdaki araçları kullanılır:

* [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md)
* [Azure PowerShell](/powershell/azure/overview)
* [Azure Resource Manager şablonu](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="service-fabric-platform-events"></a>Service Fabric platform olaylarına
Service Fabric ayarlar, birkaç ile [kullanıma hazır günlük kanalları](service-fabric-diagnostics-event-generation-infra.md), aşağıdaki kanalları izleme göndermek için uzantı ve Tanılama verileri bir depolama tablosuna veya başka bir yerde ile önceden yapılandırılmış olan biri:
  * [İşletimsel olaylar](service-fabric-diagnostics-event-generation-operational.md): Service Fabric platform gerçekleştirir daha üst düzey işlem. Örnek uygulamaları ve Hizmetleri, düğüm durum değişikliklerini ve yükseltme bilgileri oluşturulmasını içerir. Bu olay izleme için Windows (ETW) günlükleri olarak gönderilir.
  * [Reliable Actors programlama modelini olayları](service-fabric-reliable-actors-diagnostics.md)
  * [Reliable Services programlama modeline olayları](service-fabric-reliable-services-diagnostics.md)

## <a name="deploy-the-diagnostics-extension-through-the-portal"></a>Tanılama uzantısı portal üzerinden dağıtın
Günlükleri toplama ilk adımı, Service Fabric kümesindeki sanal makine ölçek kümesi düğümleri tanılama uzantısını dağıtmaktır. Tanılama uzantısını her VM'de günlükleri toplar ve bunları belirttiğiniz depolama hesabına yükler. Aşağıdaki adımlar, Azure portalı ve Azure Resource Manager şablonları ile yeni ve var olan kümeleri için bunu nasıl özetlemektedir.

### <a name="deploy-the-diagnostics-extension-as-part-of-cluster-creation-through-azure-portal"></a>Azure portalı ile küme oluşturma bir parçası olarak tanılama uzantısını dağıtma
Kümenizi, küme yapılandırma adımında oluştururken, isteğe bağlı ayarları'nı genişletin ve tanılama ayarlandığından emin olun **üzerinde** (varsayılan ayar).

![Küme oluşturma için portaldaki Azure tanılama ayarları](media/service-fabric-diagnostics-event-aggregation-wad/azure-enable-diagnostics-new.png)

Şablonu indirebilir önerilir **Oluştur'u önce** son adımda. Ayrıntılar için başvurmak [bir Azure Resource Manager şablonu kullanarak bir Service Fabric kümesi ayarlayın](service-fabric-cluster-creation-via-arm.md). Verileri toplamak için (yukarıda listelenen) hangi kanalları üzerinde değişiklik yapmak için şablonu ihtiyacınız vardır.

![Küme şablonu](media/service-fabric-diagnostics-event-aggregation-wad/download-cluster-template.png)

Azure depolama, olayları toplayarak göre [Log Analytics'i ayarlama](service-fabric-diagnostics-oms-setup.md) Öngörüler edinin ve bunları Log Analytics portalında sorgulamak için

>[!NOTE]
>Şu anda filtreleyin veya tablolara gönderilen olayların temizleme işlemi yapamazsınız. Olayları tablodan kaldırmak için bir işlem uygulamayıp, tablonun büyümeye devam edecektir (varsayılan cap 50 GB'tır). Bu olan değiştirmek yönergeler [bu makalede daha aşağıda](service-fabric-diagnostics-event-aggregation-wad.md#update-storage-quota). Buna ek olarak, çalışan bir veri temizleme hizmeti örneği yok [izleme örnek](https://github.com/Azure-Samples/service-fabric-watchdog-service), ve bir 30 veya 90 günlük süre günlükleri depolamak için geçerli bir nedeniniz yoksa kendiniz için bir tane de yazma önerilir.

## <a name="deploy-the-diagnostics-extension-through-azure-resource-manager"></a>Tanılama uzantısını Azure Resource Manager aracılığıyla dağıtma

### <a name="create-a-cluster-with-the-diagnostics-extension"></a>Tanılama uzantısı ile küme oluşturma
Kaynak Yöneticisi'ni kullanarak bir küme oluşturmak için tam bir Resource Manager şablon için JSON tanılama yapılandırması eklemeniz gerekir. Resource Manager şablonu örneklerimizi parçası olarak eklenen tanılama yapılandırması ile örnek beş sanal makine küme kaynak yöneticisi şablonu sunuyoruz. Azure Örnekler Galerisi bu konumda görebilirsiniz: [Tanılama Kaynak Yöneticisi şablonu örnek ile beş düğümlü küme](https://azure.microsoft.com/resources/templates/service-fabric-secure-cluster-5-node-1-nodetype/).

Resource Manager şablonu tanılama ayarı görmek için azuredeploy.json dosyasını açın ve arama **IaaSDiagnostics**. Bu şablonu kullanarak bir küme oluşturmak için Seç **azure'a Dağıt** düğmesini önceki bağlantıda kullanılabilir.

Alternatif olarak, sizi Resource Manager örneği indirin, değişiklik yapmak ve kullanarak bir küme ile değiştirilmiş şablonu oluşturma `New-AzureRmResourceGroupDeployment` Azure PowerShell penceresinde komutu. Komutu içinde geçirdiğiniz parametreler için aşağıdaki kodu bakın. PowerShell kullanarak bir kaynak grubu dağıtma hakkında ayrıntılı bilgi için bkz [Azure Resource Manager şablonu ile bir kaynak grubu dağıtma](../azure-resource-manager/resource-group-template-deploy.md).

### <a name="add-the-diagnostics-extension-to-an-existing-cluster"></a>Tanılama uzantısını mevcut bir kümeye ekleme
Dağıtılan tanılama sahip olmayan var olan bir kümeniz varsa ekleyin veya küme şablonu güncelleştirebilir. Daha önce açıklandığı gibi portaldan şablonu indirin veya mevcut kümeyi oluşturmak için kullanılan Resource Manager şablonunu değiştirin. Aşağıdaki görevleri gerçekleştirerek template.json dosyasını değiştirin:

Yeni bir depolama kaynağı kaynaklar bölümüne ekleyerek şablonuna ekleyin.

```json
{
  "apiVersion": "2015-05-01-preview",
  "type": "Microsoft.Storage/storageAccounts",
  "name": "[parameters('applicationDiagnosticsStorageAccountName')]",
  "location": "[parameters('computeLocation')]",
  "sku": {
    "accountType": "[parameters('applicationDiagnosticsStorageAccountType')]"
  },
  "tags": {
    "resourceType": "Service Fabric",
    "clusterName": "[parameters('clusterName')]"
  }
},
```

 Ardından, Parametreler bölümü için depolama hesabı tanımları'sonra yalnızca arasında ekleme `supportLogStorageAccountName`. Yer tutucu metnini değiştirin *depolama hesabı adı buraya* istediğiniz depolama hesabı adı ile.

```json
    "applicationDiagnosticsStorageAccountType": {
      "type": "string",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS"
      ],
      "defaultValue": "Standard_LRS",
      "metadata": {
        "description": "Replication option for the application diagnostics storage account"
      }
    },
    "applicationDiagnosticsStorageAccountName": {
      "type": "string",
      "defaultValue": "**STORAGE ACCOUNT NAME GOES HERE**",
      "metadata": {
        "description": "Name for the storage account that contains application diagnostics data from the cluster"
      }
    },
```
Daha sonra güncelleştirme `VirtualMachineProfile` uzantıları dizi içinde aşağıdaki kodu ekleyerek template.json dosyasının. Başında veya burada eklenen bağlı olarak, son bir virgül ekleyin emin olun.

```json
{
    "name": "[concat(parameters('vmNodeType0Name'),'_Microsoft.Insights.VMDiagnosticsSettings')]",
    "properties": {
        "type": "IaaSDiagnostics",
        "autoUpgradeMinorVersion": true,
        "protectedSettings": {
        "storageAccountName": "[parameters('applicationDiagnosticsStorageAccountName')]",
        "storageAccountKey": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('applicationDiagnosticsStorageAccountName')),'2015-05-01-preview').key1]",
        "storageAccountEndPoint": "https://core.windows.net/"
        },
        "publisher": "Microsoft.Azure.Diagnostics",
        "settings": {
        "WadCfg": {
            "DiagnosticMonitorConfiguration": {
            "overallQuotaInMB": "50000",
            "EtwProviders": {
                "EtwEventSourceProviderConfiguration": [
                {
                    "provider": "Microsoft-ServiceFabric-Actors",
                    "scheduledTransferKeywordFilter": "1",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricReliableActorEventTable"
                    }
                },
                {
                    "provider": "Microsoft-ServiceFabric-Services",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricReliableServiceEventTable"
                    }
                }
                ],
                "EtwManifestProviderConfiguration": [
                {
                    "provider": "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8",
                    "scheduledTransferLogLevelFilter": "Information",
                    "scheduledTransferKeywordFilter": "4611686018427387904",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricSystemEventTable"
                    }
                }
                ]
            }
            }
        },
        "StorageAccount": "[parameters('applicationDiagnosticsStorageAccountName')]"
        },
        "typeHandlerVersion": "1.5"
    }
}
```

Açıklandığı template.json dosyasını değiştirdikten sonra Resource Manager şablonunu yeniden yayımlayın. Şablon dışarı aktarıldı, deploy.ps1 dosyasını çalıştırmak, şablonu yeniden yayımlar. Dağıtımından sonra emin **ProvisioningState** olduğu **başarılı**.

> [!TIP]
> Kapsayıcıları kümenize dağıtmak için kullanacaksanız, docker istatistiği ' için bu ekleyerek seçmek WAD etkinleştirmek, **WadCfg > DiagnosticMonitorConfiguration** bölümü.
>
>```json
>"DockerSources": {
>    "Stats": {
>        "enabled": true,
>        "sampleRate": "PT1M"
>    }
>},
>```

### <a name="update-storage-quota"></a>Depolama kotası güncelleştirme

Uzantısı tarafından doldurulan tabloları beri büyüdükçe kota isabet kadar kota boyutu düşürmeyi düşünün isteyebilirsiniz. Varsayılan değer 50 GB ve şablonda altında yapılandırılabilir `overallQuotaInMB` altında `DiagnosticMonitorConfiguration`

```json
"overallQuotaInMB": "50000",
```

## <a name="log-collection-configurations"></a>Günlük toplama yapılandırmaları
Diğer kanalları günlüklerinden de koleksiyonu için kullanılabilir olan, Azure'da çalışan kümeler için şablonda yapabileceğiniz yapılandırmaların çoğu bazıları aşağıda verilmiştir.

* İşlevsel kanal - tabanı: Varsayılan olarak, Service Fabric ve olaylar, yaklaşan bir düğüm, dağıtılmakta olan yeni bir uygulama veya yükseltme bir geri alma dahil olmak üzere, küme tarafından gerçekleştirilen üst düzey işlem etkin vb. Olayların bir listesi için başvurmak [işletimsel kanal olayları](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-generation-operational).
  
```json
      scheduledTransferKeywordFilter: "4611686018427387904"
  ```
* İşlevsel kanal - ayrıntılı: Bu sistem durumu raporlarının ve kararları yanı sıra her şeyi temel işlevsel kanal yük dengelemeyi içerir. Bu olaylar Sistem kullanarak sistem veya kodunuz tarafından oluşturulan veya Raporlama API'leri gibi yük [ReportPartitionHealth](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportpartitionhealth.aspx) veya [ReportLoad](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportload.aspx). Visual Studio Tanılama Olay Görüntüleyicisi'nde bu olayları görüntülemek için Ekle "Microsoft-ServiceFabric:4:0x4000000000000008" ETW sağlayıcıları listesi.

```json
      scheduledTransferKeywordFilter: "4611686018427387912"
  ```

* Veri ve ileti kanalı - oluşturun: Kritik günlükleri ve (şu anda yalnızca ReverseProxy) Mesajlaşma ve veri yolu, ayrıntılı işlevsel kanal günlüklerine ek olarak oluşturulan olayları. Bu, işlenen isteklerin yanı sıra, istek hataları ve diğer kritik sorunları Reverseproxy'ye işleme olaylardır. **Bu kapsamlı günlüğe kaydetme için Bizim önerimiz,**. Visual Studio Tanılama Olay Görüntüleyicisi'nde bu olayları görüntülemek için Ekle "Microsoft-ServiceFabric:4:0x4000000000000010" ETW sağlayıcıları listesi.

```json
      scheduledTransferKeywordFilter: "4611686018427387928"
  ```

* & Ayrıntılı olan Mesajlaşma kanalını - veri: Veri ve küme ve ayrıntılı işlevsel kanal Mesajlaşma kritik olmayan tüm günlükleri içeren ayrıntılı kanal. Ayrıntılı sorun giderme tüm ters proxy olayları için başvurmak [ters proxy tanılama Kılavuzu](service-fabric-reverse-proxy-diagnostics.md).  Visual Studio Tanılama Olay Görüntüleyicisi'nde bu olayları görüntülemek için Ekle "Microsoft-ServiceFabric:4:0x4000000000000020" ETW sağlayıcıları listesi.

```json
      scheduledTransferKeywordFilter: "4611686018427387944"
  ```

>[!NOTE]
>Bu kanal olayların çok yüksek hacimli sahipse, bu etkinleştirme olay toplama kanal sonuçları hızlıca oluşturulmasını izlemeleri çok ayrıntılı ve depolama kapasitesi kullanabilir. Yalnızca bu kesinlikle gerekli olduğunda etkinleştirin.


Etkinleştirmek için **temel işlevsel kanal** gürültü, en az miktarda ile kapsamlı günlüğe kaydetme için tavsiyemiz `EtwManifestProviderConfiguration` içinde `WadCfg` şablonunuzun aşağıdaki gibi görünür:

```json
  "WadCfg": {
        "DiagnosticMonitorConfiguration": {
          "overallQuotaInMB": "50000",
          "EtwProviders": {
            "EtwEventSourceProviderConfiguration": [
              {
                "provider": "Microsoft-ServiceFabric-Actors",
                "scheduledTransferKeywordFilter": "1",
                "scheduledTransferPeriod": "PT5M",
                "DefaultEvents": {
                  "eventDestination": "ServiceFabricReliableActorEventTable"
                }
              },
              {
                "provider": "Microsoft-ServiceFabric-Services",
                "scheduledTransferPeriod": "PT5M",
                "DefaultEvents": {
                  "eventDestination": "ServiceFabricReliableServiceEventTable"
                }
              }
            ],
            "EtwManifestProviderConfiguration": [
              {
                "provider": "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8",
                "scheduledTransferLogLevelFilter": "Information",
                "scheduledTransferKeywordFilter": "4611686018427387904",
                "scheduledTransferPeriod": "PT5M",
                "DefaultEvents": {
                  "eventDestination": "ServiceFabricSystemEventTable"
                }
              }
            ]
          }
        }
      },
```

## <a name="collect-from-new-eventsource-channels"></a>Yeni EventSource kanaldan Topla

Tanılama günlüklerini toplama, olduğunuz hakkında dağıtmak için gerçekleştirmenizi aynı yeni bir uygulamasını temsil eden yeni EventSource kanaldan güncelleştirmek için varolan bir tanılama kurulumu için daha önce açıklanan adımları küme.

Güncelleştirme `EtwEventSourceProviderConfiguration` yapılandırma uygulamadan önce yeni bir EventSource kanalları kullanarak güncelleştirmek için giriş eklemek için template.json dosyasını bölümünde `New-AzureRmResourceGroupDeployment` PowerShell komutu. Olay kaynağının adını, Visual Studio tarafından oluşturulan ServiceEventSource.cs dosyası kodunuzda bir parçası olarak tanımlanır.

Örneğin, olay kaynağınızı Eventsource My adlandırılmışsa Eventsource My olayların MyDestinationTableName adlı bir tabloya yerleştirmek için aşağıdaki kodu ekleyin.

```json
        {
            "provider": "My-Eventsource",
            "scheduledTransferPeriod": "PT5M",
            "DefaultEvents": {
            "eventDestination": "MyDestinationTableName"
            }
        }
```

Performans sayaçlarını veya olayları toplamak için sağlanan örnekleri'ni kullanarak Resource Manager şablonu değiştirme [bir Azure Resource Manager şablonu kullanarak izleme ve tanılama özellikli bir Windows sanal makine oluşturma](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Ardından, Resource Manager şablonunu yeniden yayımlayın.

## <a name="collect-performance-counters"></a>Performans sayaçlarını Topla

Kümenizden performans ölçümleri toplamak için "WadCfg > DiagnosticMonitorConfiguration" Resource Manager şablonunda, kümeniz için performans sayaçları ekleyin. Bkz: [WAD ile performans izleme](service-fabric-diagnostics-perf-wad.md) değiştirme adımları için `WadCfg` belirli performans sayaçları toplanamadı. Başvuru [Service Fabric performans sayaçları](service-fabric-diagnostics-event-generation-perf.md) , listesini performans sayaçları için toplama öneririz.
  
Aşağıdaki bölümde açıklanan şekilde bir Application Insights havuz kullanıyorsanız ve Bu ölçümler Application Insights'da gösterilmesi için istediğiniz, yukarıda gösterildiği gibi "havuzlarını" bölümünde havuz adını eklediğinizden emin olun. Bu ayrı ayrı yapılandırılır performans sayaçlarını Application Insights kaynağınızın otomatik olarak gönderir.


## <a name="send-logs-to-application-insights"></a>Günlükleri Application Insights'a gönderme

Application Insights (AI) izleme ve tanılama verisi gönderme WAD yapılandırmasının bir parçası yapılabilir. Olay analiz ve görselleştirme için yapay ZEKA kullanmaya karar verirseniz, okuma [bir yapay ZEKA havuzunu'kurmak nasıl](service-fabric-diagnostics-event-analysis-appinsights.md#add-the-application-insights-sink-to-the-resource-manager-template) , "WadCfg" bir parçası olarak.

## <a name="next-steps"></a>Sonraki adımlar

Azure tanılama doğru şekilde yapılandırdıktan sonra ETW ve EventSource günlükleri depolama tablolardaki verileri görebilirsiniz. Log Analytics, Kibana ya da Resource Manager şablonunda doğrudan yapılandırılmamış başka bir veri analizi ve görselleştirme platform kullanmayı tercih ederseniz, bu depolama tablolardaki verileri okumak için tercih ettiğiniz platforma ayarlayın emin olun. Bunu yapmak için Log Analytics görece basit ve açıklanan [olay ve günlük analizi](service-fabric-diagnostics-event-analysis-oms.md). Application Insights, bu bağlamda bir özel durum biraz, tanılama uzantı yapılandırmasını bir parçası olarak yapılandırılmış olduğundan, bu nedenle bakın [uygun makale](service-fabric-diagnostics-event-analysis-appinsights.md) AI kullanmayı seçerseniz.

>[!NOTE]
>Şu anda filtreleyin veya tabloya gönderilen olayların temizleme işlemi yapamazsınız. Olayları tablodan kaldırmak için bir işlem uygulamayıp, tablonun büyümeye devam edecektir. Şu anda çalışan bir veri temizleme hizmeti örneği yok [izleme örnek](https://github.com/Azure-Samples/service-fabric-watchdog-service), ve bir 30 veya 90 günlük süre günlükleri depolamak için geçerli bir nedeniniz yoksa kendiniz için bir tane de yazma önerilir.

* [Tanılama uzantısını kullanarak performans sayaçlarını veya günlük toplamayı öğrenin](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Olay analizi ve Application Insights ile Görselleştirme](service-fabric-diagnostics-event-analysis-appinsights.md)
* [Olay çözümleme ve görselleştirme Log Analytics ile](service-fabric-diagnostics-event-analysis-oms.md)
