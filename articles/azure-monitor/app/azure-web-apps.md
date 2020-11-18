---
title: Azure App Services performansını izleme | Microsoft Docs
description: Azure Uygulama Hizmetleri için uygulama performansı izleme. Grafik yükleme ve yanıt süresi, bağımlılık bilgileri ve performans üzerinde Uyarılar ayarlama.
ms.topic: conceptual
ms.date: 08/06/2020
ms.custom: devx-track-js, devx-track-dotnet
ms.openlocfilehash: f46d00f97dab18b0c7c1d4a5742a87308f814e9e
ms.sourcegitcommit: 0a9df8ec14ab332d939b49f7b72dea217c8b3e1e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/18/2020
ms.locfileid: "94832907"
---
# <a name="monitor-azure-app-service-performance"></a>Azure App Service performansını izleme

[Azure Uygulama hizmetlerinde](../../app-service/index.yml) çalışan ASP.NET ve ASP.NET Core tabanlı Web uygulamalarında izlemenin etkinleştirilmesi artık hiç olmadığı kadar kolay. Daha önce bir site uzantısını el ile yüklemek için, en son uzantı/aracı artık varsayılan olarak App Service görüntüsüne yerleşik olarak bulunur. Bu makale, Application Insights izlemenin nasıl etkinleştirilebileceğine ve büyük ölçekli dağıtımlar için işlemi otomatikleştirmek üzere ön kılavuz sağlamanıza yol gösterecektir.

> [!NOTE]
> **Geliştirme araçları** uzantıları aracılığıyla Application Insights bir site uzantısının el ile eklenmesi  >  **Extensions** kullanım dışıdır. Bu uzantı yükleme yöntemi, her yeni sürüm için el ile güncelleştirmelere bağımlıdır. Uzantının en son kararlı sürümü artık App Service görüntüsünün bir parçası olarak  [önceden yüklenmiştir](https://github.com/projectkudu/kudu/wiki/Azure-Site-Extensions) . Dosyalar içinde bulunur `d:\Program Files (x86)\SiteExtensions\ApplicationInsightsAgent` ve her kararlı sürümle otomatik olarak güncelleştirilir. Aşağıda izlemeyi etkinleştirmek için aracı tabanlı yönergeleri izlerseniz, devre dışı bırakılmış uzantıyı sizin için otomatik olarak kaldırır.

## <a name="enable-application-insights"></a>Application Insights'ı etkinleştirme

Azure App Services 'da barındırılan uygulamalar için uygulama izlemeyi etkinleştirmenin iki yolu vardır:

* **Aracı tabanlı uygulama izleme** (applicationınsiizsagent).  
    * Bu yöntem etkinleştirilmesi en kolayıdır ve gelişmiş yapılandırma gerekmez. Genellikle "çalışma zamanı" izleme olarak adlandırılır. Azure Uygulama Hizmetleri için, bu izleme düzeyini en az etkinleştirme ve ardından belirli senaryonuza bağlı olarak, el ile izleme yoluyla daha gelişmiş izleme gerekip gerekmediğini değerlendirebilirsiniz.

* Application Insights SDK 'Yı yükleyerek **uygulamayı kodla el ile işaretleme** .

    * Bu yaklaşım çok daha özelleştirilebilir, ancak [APPLICATION INSIGHTS SDK NuGet paketlerine bağımlılık eklemeyi](./asp-net.md)gerektirir. Bu yöntem, ayrıca paketlerin en son sürümüne yönelik güncelleştirmeleri yönetmeniz anlamına gelir.

    * Aracı tabanlı izleme ile varsayılan olarak yakalanmayan olayları/bağımlılıkları izlemek için özel API çağrıları yapmanız gerekiyorsa, bu yöntemi kullanmanız gerekir. Daha fazla bilgi edinmek için [özel olaylar ve ölçümler makalesine yönelik API](./api-custom-events-metrics.md) 'ye göz atın. Bu, şu anda Linux tabanlı iş yükleri için desteklenen tek seçenektir.

> [!NOTE]
> Hem aracı tabanlı izleme hem de el ile SDK tabanlı izleme algılanırsa, yalnızca el ile izleme ayarları kabul edilir. Bu, yinelenen verilerin gönderilmesini önlemektir. Bu konuda daha fazla bilgi edinmek için aşağıdaki [sorun giderme bölümüne](#troubleshooting) bakın.

## <a name="enable-agent-based-monitoring"></a>Aracı tabanlı izlemeyi etkinleştir

# <a name="aspnet"></a>[ASP.NET](#tab/net)

> [!NOTE]
> APPINSIGHTS_JAVASCRIPT_ENABLED ve urlCompression birleşimi desteklenmez. Daha fazla bilgi için bkz. [sorun giderme bölümündeki](#troubleshooting)açıklama.


1. App Service için Azure Denetim Masası 'nda Application Insights ' yi **seçin** .

    ![Ayarlar altında Application Insights ' ı seçin.](./media/azure-web-apps/settings-app-insights-01.png)

   * Bu uygulama için zaten bir Application Insights kaynağı ayarlamadığınız takdirde yeni bir kaynak oluşturmayı seçin. 

     > [!NOTE]
     > Yeni kaynağı oluşturmak için **Tamam** ' a tıkladığınızda, **izleme ayarlarını uygulamanız** istenir. **Devam** ' ı seçerseniz, yeni Application Insights kaynağınızı App Service 'e bağlayacaksınız, bunun yapılması da **App Service 'in yeniden başlatılmasını tetikler**. 

     ![Web uygulamanızı izleme](./media/azure-web-apps/create-resource-01.png)

2. Hangi kaynağı kullanacağınızı belirttikten sonra, Application Insights 'ın uygulamanız için her platform için veri toplamasını nasıl istediğinizi seçebilirsiniz. ASP.NET uygulama izleme iki farklı koleksiyon düzeyiyle varsayılan olarak yapılır.

    ![Ekran görüntüsü, yeni kaynak Oluştur seçiliyken Application Insights site uzantıları sayfasını gösterir.](./media/azure-web-apps/choose-options-new.png)
 
 Her yol için toplanan verilerin bir özeti aşağıda verilmiştir:
        
| Veriler | ASP.NET temel koleksiyonu | ASP.NET önerilen koleksiyon |
| --- | --- | --- |
| CPU, bellek ve G/Ç kullanım eğilimlerini ekler |Yes |Yes |
| Kullanım eğilimlerini toplar ve kullanılabilirlik sonuçlarıyla işlemler arasında bağıntı sağlar | Yes |Yes |
| Ana işlem tarafından işlenmeyen özel durumları toplar | Yes |Yes |
| Örnekleme kullanıldığında yük altındaki APM ölçümü doğruluğunu geliştirir | Yes |Yes |
| Mikro hizmetler ile istek/bağımlılık sınırları arasında bağıntı sağlar | Hayır (yalnızca tek örnekli APM özellikleri) |Yes |

3. Daha önce applicationinsights.config dosyası aracılığıyla denetleyebilmeniz gereken örnekleme gibi ayarları yapılandırmak için artık karşılık gelen bir ön ek ile uygulama ayarları aracılığıyla aynı ayarlarla etkileşime geçebilirsiniz. 

    * Örneğin, başlangıçtaki örnekleme yüzdesini değiştirmek için, bir uygulama ayarı oluşturabilirsiniz: `MicrosoftAppInsights_AdaptiveSamplingTelemetryProcessor_InitialSamplingPercentage` ve değeri `100` .

    * Desteklenen Uyarlamalı örnekleme telemetri işlemcisi ayarlarının listesi için [koda](https://github.com/microsoft/ApplicationInsights-dotnet/blob/master/BASE/Test/ServerTelemetryChannel.Test/TelemetryChannel.Tests/AdaptiveSamplingTelemetryProcessorTest.cs) ve [ilişkili belgelere](./sampling.md)başvurabilirsiniz.

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/netcore)

Aşağıdaki ASP.NET Core sürümleri desteklenir: ASP.NET Core 2,1, ASP.NET Core 2,2, ASP.NET Core 3,0, ASP.NET Core 3,1

ASP.NET Core, kendinden bağımsız dağıtım ve Linux tabanlı uygulamalardan tam Framework 'ü hedeflemek, aracı/uzantısı tabanlı izleme ile Şu anda **desteklenmemektedir** . (Kod aracılığıyla[el ile izleme](./asp-net-core.md) , önceki senaryolardan tümünde çalışır.)

1. App Service için Azure Denetim Masası 'nda Application Insights ' yi **seçin** .

    ![Ayarlar altında Application Insights ' ı seçin.](./media/azure-web-apps/settings-app-insights-01.png)

   * Bu uygulama için zaten bir Application Insights kaynağı ayarlamadığınız takdirde yeni bir kaynak oluşturmayı seçin. 

     > [!NOTE]
     > Yeni kaynağı oluşturmak için **Tamam** ' a tıkladığınızda, **izleme ayarlarını uygulamanız** istenir. **Devam** ' ı seçerseniz, yeni Application Insights kaynağınızı App Service 'e bağlayacaksınız, bunun yapılması da **App Service 'in yeniden başlatılmasını tetikler**. 

     ![Web uygulamanızı izleme](./media/azure-web-apps/create-resource-01.png)

2. Hangi kaynağı kullanacağınızı belirttikten sonra, uygulamanız için her platform için verileri nasıl toplayacağınızı Application Insights istediğinizi seçebilirsiniz. ASP.NET Core, ASP.NET Core 2,1, 2,2, 3,0 ve 3,1 için **Önerilen koleksiyon** veya **devre dışı** bırakma olanakları sunmaktadır.

    ![Platform başına seçenekleri belirleyin](./media/azure-web-apps/choose-options-new-net-core.png)

# <a name="nodejs"></a>[Node.js](#tab/nodejs)

App Service Web uygulamanızın içinden **Ayarlar** bölümünden  >  Application Insights etkinleştir '**i seçin**  >  **Enable**. Node.js aracı tabanlı izleme şu anda önizleme aşamasındadır.

# <a name="java"></a>[Java](#tab/java)

Kodunuzu değiştirmeden Java uygulamalarınız için otomatik araçları etkinleştirmek üzere [Application Insights java 3,0 aracısına](./java-in-process-agent.md) yönelik yönergeleri izleyin.
Otomatik tümleştirme henüz App Service için kullanılabilir değil.

# <a name="python"></a>[Python](#tab/python)

Python App Service tabanlı Web uygulamaları, şu anda otomatik aracı/uzantı tabanlı izlemeyi desteklemez. Python uygulamanız için izlemeyi etkinleştirmek üzere [uygulamanızı el ile el ile](./opencensus-python.md)yapmanız gerekir.

---

## <a name="enable-client-side-monitoring"></a>İstemci tarafı izlemeyi etkinleştir

# <a name="aspnet"></a>[ASP.NET](#tab/net)

İstemci tarafı izleme, ASP.NET için kabul edilir. İstemci tarafı izlemeyi etkinleştirmek için:

* **Ayarlar** **>** **Yapılandırma**
   * Uygulama ayarları ' nın altında **Yeni bir uygulama ayarı** oluşturun:

     Ad: `APPINSIGHTS_JAVASCRIPT_ENABLED`

     Değer: `true`

   * Ayarları **Kaydedin** ve uygulamanızı **Yeniden başlatın**.

İstemci tarafı izlemeyi devre dışı bırakmak için, uygulama ayarlarından ilişkili anahtar değer çiftini kaldırın ya da değeri false olarak ayarlayın.

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/netcore)

İstemci tarafı izleme, ' APPINSIGHTS_JAVASCRIPT_ENABLED ' uygulama ayarının mevcut olup olmamasına bakılmaksızın **Önerilen koleksiyona** sahip ASP.NET Core uygulamaları için **Varsayılan olarak etkindir** .

Bazı nedenlerle istemci tarafı izlemeyi devre dışı bırakmak istiyorsanız:

* **Ayarlar** **>** **Yapılandırma**
   * Uygulama ayarları ' nın altında **Yeni bir uygulama ayarı** oluşturun:

     ada `APPINSIGHTS_JAVASCRIPT_ENABLED`

     Değer: `false`

   * Ayarları **Kaydedin** ve uygulamanızı **Yeniden başlatın**.

# <a name="nodejs"></a>[Node.js](#tab/nodejs)

Node.js uygulamanız için istemci tarafı izlemeyi etkinleştirmek üzere, [istemci tarafı JavaScript SDK 'sını uygulamanıza el ile eklemeniz](./javascript.md)gerekir.

# <a name="java"></a>[Java](#tab/java)

Java uygulamanız için istemci tarafı izlemeyi etkinleştirmek üzere, [istemci tarafı JavaScript SDK 'sını uygulamanıza el ile eklemeniz](./javascript.md)gerekir.

# <a name="python"></a>[Python](#tab/python)

Python uygulamanız için istemci tarafı izlemeyi etkinleştirmek üzere, [istemci tarafı JavaScript SDK 'sını uygulamanıza el ile eklemeniz](./javascript.md)gerekir.

---

## <a name="automate-monitoring"></a>İzlemeyi otomatikleştirin

Application Insights ile telemetri toplamayı etkinleştirmek için, yalnızca uygulama ayarlarının ayarlanması gerekir:

   ![Kullanılabilir Application Insights ayarları ile uygulama ayarlarını App Service](./media/azure-web-apps/application-settings.png)

### <a name="application-settings-definitions"></a>Uygulama ayarları tanımları

|Uygulama ayarı adı |  Tanım | Değer |
|-----------------|:------------|-------------:|
|ApplicationInsightsAgent_EXTENSION_VERSION | Çalışma zamanı izlemeyi denetleyen ana uzantı. | `~2` |
|XDT_MicrosoftApplicationInsights_Mode |  Yalnızca varsayılan modda, en iyi performansı sağlamak için temel özellikler etkindir. | `default` veya `recommended`. |
|InstrumentationEngine_EXTENSION_VERSION | İkili yeniden yazma altyapısının `InstrumentationEngine` açılıp açılmeyeceğini denetler. Bu ayarın performans etkileri vardır ve soğuk başlatma/başlatma süresini etkiler. | `~1` |
|XDT_MicrosoftApplicationInsights_BaseExtensions | SQL & Azure Tablo metninin bağımlılık çağrılarında birlikte yakalanıp yakalanmayacağını denetler. Performans uyarısı: uygulamanın soğuk başlangıç saati etkilenecektir. Bu ayar için gerekir `InstrumentationEngine` . | `~1` |

### <a name="app-service-application-settings-with-azure-resource-manager"></a>Azure Resource Manager ile uygulama ayarlarını App Service

Uygulama Hizmetleri için uygulama ayarları, [Azure Resource Manager şablonlarıyla](../../azure-resource-manager/templates/template-syntax.md)yönetilebilir ve yapılandırılabilir. Bu yöntem, Azure Resource Manager otomasyonu ile yeni App Service kaynakları dağıtıldığında veya mevcut kaynakların ayarlarını değiştirirken kullanılabilir.

App Service için uygulama ayarları JSON ' un temel yapısı aşağıda verilmiştir:

```JSON
      "resources": [
        {
          "name": "appsettings",
          "type": "config",
          "apiVersion": "2015-08-01",
          "dependsOn": [
            "[resourceId('Microsoft.Web/sites', variables('webSiteName'))]"
          ],
          "tags": {
            "displayName": "Application Insights Settings"
          },
          "properties": {
            "key1": "value1",
            "key2": "value2"
          }
        }
      ]
```

Application Insights için yapılandırılmış uygulama ayarlarına sahip Azure Resource Manager bir şablon örneği için, bu [şablon](https://github.com/Andrew-MSFT/BasicImageGallery) faydalı olabilir, özellikle de [238. satırdan](https://github.com/Andrew-MSFT/BasicImageGallery/blob/c55ada54519e13ce2559823c16ca4f97ddc5c7a4/CoreImageGallery/Deploy/CoreImageGalleryARM/azuredeploy.json#L238)başlayan bir bölümdür.

### <a name="automate-the-creation-of-an-application-insights-resource-and-link-to-your-newly-created-app-service"></a>Bir Application Insights kaynağı oluşturmayı otomatikleştirin ve yeni oluşturduğunuz App Service bağlayın.

Yapılandırılmış tüm varsayılan Application Insights ayarlarla Azure Resource Manager şablonu oluşturmak için, Application Insights etkin yeni bir Web uygulaması oluşturacağım gibi işlemi başlatın.

**Otomasyon seçeneklerini** belirleyin

   ![Web uygulaması oluşturma menüsünü App Service](./media/azure-web-apps/create-web-app.png)

Bu seçenek, tüm gerekli ayarlarla yapılandırılmış en son Azure Resource Manager şablonunu oluşturur.

  ![App Service Web uygulaması şablonu](./media/azure-web-apps/arm-template.png)

Aşağıda bir örnek verilmiştir ve tüm örneklerini  `AppMonitoredSite` site adınızla değiştirin:

```json
{
    "resources": [
        {
            "name": "[parameters('name')]",
            "type": "Microsoft.Web/sites",
            "properties": {
                "siteConfig": {
                    "appSettings": [
                        {
                            "name": "APPINSIGHTS_INSTRUMENTATIONKEY",
                            "value": "[reference('microsoft.insights/components/AppMonitoredSite', '2015-05-01').InstrumentationKey]"
                        },
                        {
                            "name": "APPLICATIONINSIGHTS_CONNECTION_STRING",
                            "value": "[reference('microsoft.insights/components/AppMonitoredSite', '2015-05-01').ConnectionString]"
                        },
                        {
                            "name": "ApplicationInsightsAgent_EXTENSION_VERSION",
                            "value": "~2"
                        }
                    ]
                },
                "name": "[parameters('name')]",
                "serverFarmId": "[concat('/subscriptions/', parameters('subscriptionId'),'/resourcegroups/', parameters('serverFarmResourceGroup'), '/providers/Microsoft.Web/serverfarms/', parameters('hostingPlanName'))]",
                "hostingEnvironment": "[parameters('hostingEnvironment')]"
            },
            "dependsOn": [
                "[concat('Microsoft.Web/serverfarms/', parameters('hostingPlanName'))]",
                "microsoft.insights/components/AppMonitoredSite"
            ],
            "apiVersion": "2016-03-01",
            "location": "[parameters('location')]"
        },
        {
            "apiVersion": "2016-09-01",
            "name": "[parameters('hostingPlanName')]",
            "type": "Microsoft.Web/serverfarms",
            "location": "[parameters('location')]",
            "properties": {
                "name": "[parameters('hostingPlanName')]",
                "workerSizeId": "[parameters('workerSize')]",
                "numberOfWorkers": "1",
                "hostingEnvironment": "[parameters('hostingEnvironment')]"
            },
            "sku": {
                "Tier": "[parameters('sku')]",
                "Name": "[parameters('skuCode')]"
            }
        },
        {
            "apiVersion": "2015-05-01",
            "name": "AppMonitoredSite",
            "type": "microsoft.insights/components",
            "location": "West US 2",
            "properties": {
                "ApplicationId": "[parameters('name')]",
                "Request_Source": "IbizaWebAppExtensionCreate"
            }
        }
    ],
    "parameters": {
        "name": {
            "type": "string"
        },
        "hostingPlanName": {
            "type": "string"
        },
        "hostingEnvironment": {
            "type": "string"
        },
        "location": {
            "type": "string"
        },
        "sku": {
            "type": "string"
        },
        "skuCode": {
            "type": "string"
        },
        "workerSize": {
            "type": "string"
        },
        "serverFarmResourceGroup": {
            "type": "string"
        },
        "subscriptionId": {
            "type": "string"
        }
    },
    "$schema": "https://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0"
}
```

### <a name="enabling-through-powershell"></a>PowerShell aracılığıyla etkinleştirme

Uygulama izlemeyi PowerShell aracılığıyla etkinleştirmek için, yalnızca temeldeki uygulama ayarlarının değiştirilmesi gerekir. Aşağıda, "Appmonitortoredrg" kaynak grubunda "Appmonitortoredsite" adlı bir Web sitesi için uygulama izlemeye olanak tanıyan bir örnek verilmiştir ve "012345678-abcd-EF01-2345-6789abcd" izleme anahtarına gönderilecek verileri yapılandırır.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

```powershell
$app = Get-AzWebApp -ResourceGroupName "AppMonitoredRG" -Name "AppMonitoredSite" -ErrorAction Stop
$newAppSettings = @{} # case-insensitive hash map
$app.SiteConfig.AppSettings | %{$newAppSettings[$_.Name] = $_.Value} # preserve non Application Insights application settings.
$newAppSettings["APPINSIGHTS_INSTRUMENTATIONKEY"] = "012345678-abcd-ef01-2345-6789abcd"; # set the Application Insights instrumentation key
$newAppSettings["APPLICATIONINSIGHTS_CONNECTION_STRING"] = "InstrumentationKey=012345678-abcd-ef01-2345-6789abcd"; # set the Application Insights connection string
$newAppSettings["ApplicationInsightsAgent_EXTENSION_VERSION"] = "~2"; # enable the ApplicationInsightsAgent
$app = Set-AzWebApp -AppSettings $newAppSettings -ResourceGroupName $app.ResourceGroup -Name $app.Name -ErrorAction Stop
```

## <a name="upgrade-monitoring-extensionagent"></a>Yükseltme izleme uzantısı/Aracısı

### <a name="upgrading-from-versions-289-and-up"></a>2.8.9 ve up sürümlerinden yükseltme

2.8.9 sürümünden yükseltme, ek eylemler olmadan otomatik olarak gerçekleşir. Yeni izleme bitleri, hedef App Service 'e arka planda teslim edilir ve uygulama yeniden başlatıldığında alınır.

Hangi uzantının çalıştırdığınız sürümünü denetlemek için ziyaret edin `http://yoursitename.scm.azurewebsites.net/ApplicationInsights`

![URL yolunun ekran görüntüsü http://yoursitename.scm.azurewebsites.net/ApplicationInsights](./media/azure-web-apps/extension-version.png)

### <a name="upgrade-from-versions-100---265"></a>1.0.0-2.6.5 sürümlerinden yükseltme

Version 2.8.9 sürümünden itibaren önceden yüklenmiş site uzantısı kullanılır. Önceki bir sürümdeyse, iki şekilde güncelleştirebilirsiniz:

* [Portal üzerinden etkinleştirerek yükseltme](#enable-application-insights)yapın. (Azure App Service Application Insights uzantısı yüklü olsa da, UI yalnızca **Etkinleştir** düğmesini gösterir. Arka planda, eski özel site uzantısı kaldırılır.)

* [PowerShell aracılığıyla Yükselt](#enabling-through-powershell):

    1. Uygulama ayarlarını, önceden yüklenmiş site uzantısını Applicationınsimalsagent olacak şekilde ayarlayın. Bkz. [PowerShell aracılığıyla etkinleştirme](#enabling-through-powershell).
    2. Azure App Service için Application Insights uzantısı adlı özel site uzantısını el ile kaldırın.

Yükseltme, 2.5.1 ' den önceki bir sürümden yapılamıyorsa, uygulama bin klasöründen Applicationınsigyoze dll 'lerinin kaldırılıp kaldırılmadığını denetleyin. [sorun giderme adımları bölümüne bakın](#troubleshooting).

## <a name="troubleshooting"></a>Sorun giderme

Azure Uygulama Hizmetleri 'nde çalışan ASP.NET ve ASP.NET Core tabanlı uygulamalar için uzantı/aracı tabanlı izlemeye yönelik adım adım sorun giderme kılavuzumuz aşağıda verilmiştir.

> [!NOTE]
> Java uygulamalarını izlemek için önerilen yaklaşım, kodu değiştirmeden otomatik izleme kullanmaktır. Lütfen [Application Insights Java 3,0 Aracısı](./java-in-process-agent.md)için yönergeleri izleyin.


1. Uygulamasının aracılığıyla izlendiğinden emin olun `ApplicationInsightsAgent` .
    * `ApplicationInsightsAgent_EXTENSION_VERSION`Uygulama ayarının "~ 2" değerine ayarlanmış olduğunu denetleyin.
2. Uygulamanın izlenecek gereksinimleri karşıladığından emin olun.
    * `https://yoursitename.scm.azurewebsites.net/ApplicationInsights` adresine gidin

    ![https://yoursitename.scm.azurewebsites/applicationinsightsSonuç sayfasının ekran görüntüsü](./media/azure-web-apps/app-insights-sdk-status.png)

    * `Application Insights Extension Status`Olduğunu onaylayın`Pre-Installed Site Extension, version 2.8.12.1527, is running.` 
    * Çalışmıyorsa, [etkinleştirme Application Insights izleme yönergelerini](#enable-application-insights) izleyin

    * Durum kaynağının var olduğunu ve şu şekilde göründüğünü onaylayın: `Status source D:\home\LogFiles\ApplicationInsights\status\status_RD0003FF0317B6_4248_1.json`
        * Benzer bir değer yoksa, uygulamanın Şu anda çalışmadığı veya desteklenmediği anlamına gelir. Uygulamanın çalıştığından emin olmak için, uygulama URL 'si/uygulama uç noktalarını el ile ziyaret etmeyi deneyin, bu, çalışma zamanı bilgilerinin kullanılabilir hale gelmesini sağlar.

    * `IKeyExists`Olduğunu onaylayın`true`
        * Varsa `false` , `APPINSIGHTS_INSTRUMENTATIONKEY` `APPLICATIONINSIGHTS_CONNECTION_STRING` uygulama ayarlarınıza Ikey GUID 'nizi ekleyin.

    * , Ve için girdi olmadığını doğrulayın `AppAlreadyInstrumented` `AppContainsDiagnosticSourceAssembly` `AppContainsAspNetTelemetryCorrelationAssembly` .
        * Bu girdilerden herhangi biri mevcutsa, aşağıdaki paketleri uygulamanızdan kaldırın: `Microsoft.ApplicationInsights` , `System.Diagnostics.DiagnosticSource` ve `Microsoft.AspNet.TelemetryCorrelation` .
        * Yalnızca ASP.NET Core uygulamalar için: uygulamanızın herhangi bir Application Insights paketine başvurması durumunda, örneğin uygulamanızı [ASP.NET Core SDK 'sı](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core)ile daha önce (veya işaretleme girişiminde bulunursa), App Service tümleştirmenin etkin olmaması ve veriler Application Insights görünmeyebilir. Sorunu onarmak için Portal 'da "Application Insights SDK ile birlikte çalışma" seçeneğini açın ve verileri Application Insights görmeye başlayabilirsiniz 
        > [!IMPORTANT]
        > Bu işlevsellik önizlemededir 

        ![Mevcut uygulama ayarını etkinleştirin](./media/azure-web-apps/netcore-sdk-interop.png)

        Veriler artık Application Insights SDK 'nın ilk kullanılma veya kullanılmaya çalışsa bile kodsuz kullanacaksınız yaklaşımı kullanılarak gönderilecek.

        > [!IMPORTANT]
        > Uygulama herhangi bir telemetri göndermek için SDK Application Insights kullanıyorsa, bu tür Telemetri devre dışı bırakılır; örneğin, herhangi bir Izleme * () yöntemi gibi özel tüm ayarlar ve örnekleme gibi özel ayarlar devre dışı bırakılır. 


### <a name="php-and-wordpress-are-not-supported"></a>PHP ve WordPress desteklenmez

PHP ve WordPress siteleri desteklenmez. Şu anda bu iş yüklerinin sunucu tarafında izlenmesi için resmi olarak desteklenen bir SDK/aracı yoktur. Ancak, Web sayfalarınıza istemci tarafı JavaScript ekleyerek bir PHP veya WordPress sitesinde istemci tarafı işlemlerini el ile, [JavaScript SDK 'sı](./javascript.md)kullanılarak gerçekleştirebilirsiniz.

Aşağıdaki tabloda, bu değerlerin ne anlama geldiğini, temeldeki nedenleri ve önerilen düzeltmeleri verilmiştir:

|Sorun değeri|Açıklama|Düzeltme
|---- |----|---|
| `AppAlreadyInstrumented:true` | Bu değer, uzantının bir SDK 'nın bazı yönlerinin uygulamada zaten bulunduğunu algıladı ve geri dönecek. Bunun nedeni `System.Diagnostics.DiagnosticSource` , veya başvurusu olabilir `Microsoft.AspNet.TelemetryCorrelation``Microsoft.ApplicationInsights`  | Başvuruları kaldırın. Bu başvurulardan bazıları varsayılan olarak belirli Visual Studio şablonlarından eklenir ve Visual Studio 'nun eski sürümleri öğesine başvurular ekleyebilir `Microsoft.ApplicationInsights` .
|`AppAlreadyInstrumented:true` | Uygulama ASP.NET Core 2,1 veya 2,2 ' i hedefliyorsanız, bu değer uzantının bazı SDK yönlerinin uygulamada zaten bulunduğunu ve geri dönecek olduğunu belirtir | .NET Core 2.1, 2.2 ' deki müşterilerin yerine Microsoft. AspNetCore. app meta paketini kullanması [önerilir](https://github.com/aspnet/Announcements/issues/287) . Ayrıca, portalda "Application Insights SDK ile birlikte çalışabilirlik" seçeneğini açın (yukarıdaki yönergelere bakın).|
|`AppAlreadyInstrumented:true` | Bu değere, önceki bir dağıtımdan uygulama klasöründeki yukarıdaki dll 'lerin varlığı da neden olabilir. | Bu dll 'lerin kaldırıldığından emin olmak için uygulama klasörünü temizleyin. Hem yerel uygulamanızın bin dizinini hem de App Service Wwwroot dizinini denetleyin. (App Service Web uygulamanızın Wwwroot dizinini denetlemek için: Gelişmiş araçlar (kudu) > hata ayıklama konsolu > CMD > home\wwwroot).
|`AppContainsAspNetTelemetryCorrelationAssembly: true` | Bu değer, uzantının uygulamada başvuru algıladığını `Microsoft.AspNet.TelemetryCorrelation` ve geri dönecek olduğunu gösterir. | Başvuruyu kaldırın.
|`AppContainsDiagnosticSourceAssembly**:true`|Bu değer, uzantının uygulamada başvuru algıladığını `System.Diagnostics.DiagnosticSource` ve geri dönecek olduğunu gösterir.| ASP.NET için başvuruyu kaldırın. 
|`IKeyExists:false`|Bu değer, izleme anahtarının AppSetting 'de mevcut olmadığını gösterir `APPINSIGHTS_INSTRUMENTATIONKEY` . Olası nedenler: değerler yanlışlıkla kaldırılmış olabilir, Otomasyon betikindeki değerleri ayarlamayı unuttu, vb. | Ayarın App Service uygulama ayarlarında bulunduğundan emin olun.

### <a name="appinsights_javascript_enabled-and-urlcompression-is-not-supported"></a>APPINSIGHTS_JAVASCRIPT_ENABLED ve urlCompression desteklenmez

İçeriğin kodlandığı durumlarda APPINSIGHTS_JAVASCRIPT_ENABLED = true kullanırsanız, şöyle bir hata alabilirsiniz: 

- 500 URL yeniden yazma hatası
- 500,53 URL yeniden yazma hatası ileti giden yeniden yazma kuralları HTTP yanıtının içeriği kodlanmışsa (' gzip ') uygulanamaz. 

Bunun nedeni, APPINSIGHTS_JAVASCRIPT_ENABLED uygulama ayarı true olarak ayarlanmakta ve içerik kodlamasının aynı anda mevcut olmasını sağlar. Bu senaryo henüz desteklenmiyor. Geçici çözüm, APPINSIGHTS_JAVASCRIPT_ENABLED uygulama ayarlarından kaldırdır. Ne yazık ki, istemci/tarayıcı tarafı JavaScript izleme hâlâ gerekliyse, Web sayfalarınız için el ile SDK başvuruları gerekir. Lütfen JavaScript SDK 'Sı ile el ile izleme [yönergelerini](https://github.com/Microsoft/ApplicationInsights-JS#snippet-setup-ignore-if-using-npm-setup) izleyin.

Application Insights Aracısı/uzantısıyla ilgili en son bilgiler için, [sürüm notlarına](https://github.com/MohanGsk/ApplicationInsights-Home/blob/master/app-insights-web-app-extensions-releasenotes.md)göz atın.

### <a name="default-website-deployed-with-web-apps-does-not-support-automatic-client-side-monitoring"></a>Web Apps ile dağıtılan varsayılan Web sitesi otomatik istemci tarafı izlemeyi desteklemez

Azure Uygulama hizmetlerinde veya çalışma zamanları ile bir Web uygulaması oluşturduğunuzda, `ASP.NET` `ASP.NET Core` tek BIR statik HTML sayfasını Başlatıcı Web sitesi olarak dağıtır. Statik Web sayfası, IIS 'de ASP.NET yönetilen bir Web bölümü de yükler. Bu, kodsuz kullanacaksınız sunucu tarafı izlemenin test edilmesine olanak tanır, ancak otomatik istemci tarafı izlemeyi desteklemez.

Azure App Services Web uygulamasında ASP.NET veya ASP.NET Core için kodsuz kullanacaksınız sunucusunu ve istemci tarafı izlemeyi test etmek istiyorsanız, [bir ASP.NET Core Web uygulaması oluşturmak](../../app-service/quickstart-dotnetcore.md) ve [bir ASP.NET Framework Web uygulaması](../../app-service/quickstart-dotnet-framework.md) oluşturmak için resmi kılavuzlarınızın yanı sıra, izlemeyi etkinleştirmek için geçerli makaledeki yönergeleri kullanmanız önerilir.

### <a name="connection-string-and-instrumentation-key"></a>Bağlantı dizesi ve izleme anahtarı

Kodsuz kullanacaksınız izleme kullanıldığında yalnızca bağlantı dizesi gereklidir. Bununla birlikte, el ile izleme gerçekleştirilirken SDK 'nın eski sürümleriyle geriye dönük uyumluluğu korumak için de izleme anahtarını ayarlamayı öneririz.

## <a name="release-notes"></a>Sürüm notları

En son güncelleştirmeler ve hata düzeltmeleri için [sürüm notlarına bakın](./web-app-extension-release-notes.md).

## <a name="next-steps"></a>Sonraki adımlar
* [Canlı uygulamanızda profil oluşturucuyu çalıştırın](./profiler.md).
* [Azure İşlevleri](https://github.com/christopheranderson/azure-functions-app-insights-sample) - Application Insights ile Azure İşlevlerini izleyin
* Application Insights’a gönderilmek üzere [Azure tanılamayı etkinleştirin](../platform/diagnostics-extension-to-application-insights.md).
* Hizmetinizin kullanılabilir ve yanıt verir durumda oluğundan emin olmak için [hizmet durumu ölçümlerini izleyin](../platform/data-platform.md).
* İşletimsel olaylar gerçekleştiğinde ya da ölçümler bir eşiği aştığında [uyarı bildirimleri alın](../platform/alerts-overview.md).
* Bir web sayfasını ziyaret eden tarayıcılardan istemci telemetrisi toplamak istiyorsanız [JavaScript uygulamaları ve web sayfaları için Application Insights](javascript.md)’ı kullanın.
* Sitenizin kapalı olması durumunda uyarı almak istiyorsanız [Kullanılabilirlik web testleri](monitor-web-app-availability.md) ayarlayın.

