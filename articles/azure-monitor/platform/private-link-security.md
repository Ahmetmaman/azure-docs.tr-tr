---
title: Ağları Azure İzleyici'ye güvenle bağlamak için Azure Özel Bağlantı'yı kullanma
description: Ağları Azure İzleyici'ye güvenle bağlamak için Azure Özel Bağlantı'yı kullanma
author: nkiest
ms.author: nikiest
ms.topic: conceptual
ms.date: 10/05/2020
ms.subservice: ''
ms.openlocfilehash: 9eac64eff8c87046fd1ce76ee71475fda79ac6f7
ms.sourcegitcommit: 03713bf705301e7f567010714beb236e7c8cee6f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/21/2020
ms.locfileid: "92329262"
---
# <a name="use-azure-private-link-to-securely-connect-networks-to-azure-monitor"></a>Ağları Azure İzleyici'ye güvenle bağlamak için Azure Özel Bağlantı'yı kullanma

[Azure özel bağlantı](../../private-link/private-link-overview.md) , Özel uç noktaları kullanarak Azure PaaS hizmetlerini sanal ağınıza güvenli bir şekilde bağlayabilmeniz için izin verir. Birçok hizmet için, yalnızca kaynak başına bir uç nokta ayarlarsınız. Bununla birlikte, Azure Izleyici, iş yüklerinizi izlemek için birlikte çalışan farklı bağlantılı hizmetlerden oluşan bir tutarlılık sağlar. Sonuç olarak, izleme ağınızın sınırlarını tanımlamanızı ve sanal ağınıza bağlanmanızı sağlayan bir Azure Izleyici özel bağlantı kapsamı (AMPLS) adlı bir kaynak geliştirdik. Bu makalede, ne zaman kullanılacağı ve Azure Izleyici özel bağlantı kapsamının nasıl ayarlanacağı ele alınmaktadır.

## <a name="advantages"></a>Avantajlar

Özel bağlantıyla birlikte şunları yapabilirsiniz:

- Herhangi bir genel ağ erişimi açmadan Azure Izleyici 'ye özel olarak bağlanma
- İzleme verilerinize yalnızca yetkili özel ağlar aracılığıyla erişildiğinden emin olun
- Özel uç noktanızla bağlanan belirli Azure Izleyici kaynaklarını tanımlayarak özel ağlarınızdan veri alımını önleyin
- ExpressRoute ve özel bağlantı kullanarak özel şirket içi ağınızı Azure Izleyici 'ye güvenle bağlayın
- Tüm trafiği Microsoft Azure omurga ağı içinde tut

Daha fazla bilgi için bkz.  [özel bağlantının önemli avantajları](../../private-link/private-link-overview.md#key-benefits).

## <a name="how-it-works"></a>Nasıl çalışır?

Azure Izleyici özel bağlantı kapsamı bir veya daha fazla özel bitiş noktasını (ve bu nedenle içerdikleri sanal ağları) bir veya daha fazla Azure Izleyici kaynağına bağlamak için bir gruplandırma kaynağıdır. Kaynaklar Log Analytics çalışma alanları ve Application Insights bileşenleri içerir.

![Kaynak topolojisi diyagramı](./media/private-link-security/private-link-topology-1.png)

> [!NOTE]
> Tek bir Azure Izleyici kaynağı birden fazla AMPLSs 'ye ait olabilir, ancak tek bir sanal ağı birden fazla AMPLS 'e bağlanamazsınız. 

## <a name="planning-based-on-your-network"></a>Ağınızı temel alan planlama

AMPLS kaynaklarınızı ayarlamadan önce ağ yalıtımı gereksinimlerinizi göz önünde bulundurun. Sanal ağlarınızın genel İnternet 'e erişimini ve Azure Izleyici kaynaklarınızın (yani Application Insights bileşenleri ve Log Analytics çalışma alanlarının) erişim kısıtlamalarını değerlendirin.

> [!NOTE]
> Hub ve bağlı ağ ağları ya da eşlenmiş ağların herhangi bir topolojisi, hub (ana) VNet ve her VNet üzerinde özel bir bağlantı kurmak yerine, ilgili Azure Izleyici kaynakları arasında özel bir bağlantı kurmak için kullanılabilir. Bu, özellikle bu ağlar tarafından kullanılan Azure Izleyici kaynakları paylaşılmışsa bu şekilde anlamlıdır. Ancak, her VNet 'in ayrı bir izleme kaynakları kümesine erişmesine izin vermek istiyorsanız, her ağ için adanmış bir AMPLS 'e özel bir bağlantı oluşturun.

### <a name="evaluate-which-virtual-networks-should-connect-to-a-private-link"></a>Hangi sanal ağların özel bir bağlantıya bağlanması gerektiğini değerlendirin

Sanal ağlarınızdan (VNet) hangisinin Internet 'e erişimi olduğunu değerlendirerek başlatın. Ücretsiz internet içeren VNET 'ler, Azure Izleyici kaynaklarınıza erişmek için özel bir bağlantı gerektirmeyebilir. Sanal ağlarınızın bağlanacağı izleme kaynakları gelen trafiği kısıtlayabilir ve özel bağlantı bağlantısı gerektirebilir (günlük alma veya sorgu için). Bu gibi durumlarda, genel internet erişimi olan bir sanal ağın bile, bu kaynaklara özel bir bağlantı üzerinden ve bir AMPLS aracılığıyla bağlanması gerekir.

### <a name="evaluate-which-azure-monitor-resources-should-have-a-private-link"></a>Hangi Azure Izleyici kaynaklarının özel bir bağlantıya sahip olması gerektiğini değerlendirin

Azure Izleyici kaynaklarınızın her birini gözden geçirin:

- Kaynak yalnızca belirli VNET 'lerde bulunan kaynaklardan günlüklerin içeri eklenmesine izin mı?
- Kaynak yalnızca belirli VNET 'lerde bulunan istemcilerle sorgulanmalıdır mi?

Bu sorulardan birine yanıt evet ise, kısıtlamaları Log Analytics çalışma alanlarını [yapılandırma](#configure-log-analytics) ve [Application Insights bileşenlerini yapılandırma](#configure-application-insights) ve bu kaynakları tek veya birkaç ampls ile ilişkilendirme bölümünde açıklandığı gibi ayarlayın. Bu izleme kaynaklarına erişmesi gereken sanal ağların ilgili AMPLS 'e bağlanan özel bir uç noktası olması gerekir.
Unutmayın: aynı çalışma alanlarını veya uygulamayı birden çok AMPLS 'e bağlayarak farklı ağlar tarafından ulaşılmalarına izin verebilirsiniz.

### <a name="group-together-monitoring-resources-by-network-accessibility"></a>Ağ erişilebilirliği ile birlikte izleme kaynaklarını gruplama

Her VNet yalnızca bir AMPLS kaynağına bağlanabildiğinden, aynı ağlar için erişilebilir olması gereken izleme kaynaklarını birlikte gruplandırmanız gerekir. Bu gruplamayı yönetmenin en kolay yolu, sanal ağ başına bir AMPLS oluşturmak ve bu ağa bağlanacak kaynakları seçmek şeklindedir. Ancak, kaynakları azaltmak ve yönetilebilirliği artırmak için ağlarda bir AMPLS 'yi yeniden kullanmak isteyebilirsiniz.

Örneğin, iç sanal ağlarınız VNet1 ve VNet2 çalışma alanları çalışma alanı1 ve Workspace2 Application Insights ve bileşen Application Insights 3 ' e bağlanıp, üç kaynağı da aynı AMPLS ile ilişkilendirin. VNet3 yalnızca çalışma alanı1 'e erişmesi gerekiyorsa, başka bir AMPLS kaynağı oluşturun, çalışma alanı1 ile ilişkilendirin ve Aşağıdaki diyagramlarda gösterildiği gibi VNet3 bağlayın:

![Bir topolojiden yükseltme diyagramı](./media/private-link-security/ampls-topology-a-1.png)

![AMPLS B topolojisi diyagramı](./media/private-link-security/ampls-topology-b-1.png)

### <a name="consider-limits"></a>Limitleri değerlendirin

Özel bağlantı kurulumunuzu planlarken göz önünde bulundurmanız gereken birkaç sınır vardır:

* VNet yalnızca 1 AMPLS nesnesine bağlanabilir. Bu, AMPLS nesnesinin, VNet 'in erişimi olması gereken tüm Azure Izleyici kaynaklarına erişim sağlaması gerektiği anlamına gelir.
* Bir Azure Izleyici kaynağı (çalışma alanı veya Application Insights bileşeni), en çok 5 AMPLSs 'ye bağlanabilir.
* Bir AMPLS nesnesi, en çok 50 Azure Izleyici kaynağına bağlanabilir.
* AMPLS nesnesi, en çok 10 özel uç noktaya bağlanabilir.

Aşağıdaki topolojide:
* Her VNet 1 AMPLS nesnesine bağlanır, bu nedenle diğer AMPLSs 'ye bağlanamaz.
* AMPLS B, 2 sanal ağa bağlanır: olası özel uç nokta bağlantılarında 2/10 kullanma.
* AMPLS A, 2 çalışma alanına ve 1 Application Insights bileşenine bağlanır: olası Azure Izleyici kaynaklarının 3/50 ' ünü kullanıyor.
* Çalışma alanı 2, AMPLS A ve AMPLS B 'ye bağlanır: olası AMPLS bağlantılarında 2/5 kullanma.

![AMPLS limitlerinin diyagramı](./media/private-link-security/ampls-limits.png)

## <a name="example-connection"></a>Örnek bağlantı

Azure Izleyici özel bağlantı kapsamı kaynağı oluşturarak başlayın.

1. Azure portal **bir kaynak oluştur ' a** gidin ve **Azure Izleyici özel bağlantı kapsamını**arayın.

   ![Azure Izleyici özel bağlantı kapsamını bulun](./media/private-link-security/ampls-find-1c.png)

2. **Oluştur**' a tıklayın.
3. Bir abonelik ve kaynak grubu seçin.
4. AMPLS 'e bir ad verin. Kapsamın, ağ güvenlik sınırlarını yanlışlıkla kesmesine izin vermek için, kapsamın kullanılacağı amacı ve güvenlik sınırını belirten bir ad kullanmak en iyisidir. Örneğin, "AppServerProdTelem".
5. **Gözden Geçir ve Oluştur**’a tıklayın. 

   ![Azure Izleyici özel bağlantı kapsamı oluştur](./media/private-link-security/ampls-create-1d.png)

6. Doğrulama geçişine izin verin ve ardından **Oluştur**' a tıklayın.

## <a name="connect-azure-monitor-resources"></a>Azure Izleyici kaynaklarını bağlama

AMPLS 'yi önce özel uç noktalara, sonra da Azure Izleyici kaynaklarına veya bunun tersini yapabilir, ancak Azure Izleyici kaynaklarınızla başladıysanız bağlantı işlemi daha hızlı ilerler. Azure Izleyici Log Analytics çalışma alanlarını ve Application Insights bileşenlerini bir AMPLS 'e nasıl bağlayacağız

1. Azure Izleyici özel bağlantı kapsamınızda, sol taraftaki menüden **Azure Izleyici kaynakları** ' na tıklayın. **Ekle** düğmesine tıklayın.
2. Çalışma alanını veya bileşeni ekleyin. **Ekle** düğmesine tıkladığınızda Azure izleyici kaynaklarını seçebileceğiniz bir iletişim kutusu açılır. Aboneliklerinize ve kaynak gruplarınıza göz atabilir veya bunları filtrelemek için ad yazabilirsiniz. Çalışma alanını veya bileşeni seçin ve kapsamlarınıza eklemek için **Uygula** ' ya tıklayın.

    ![Kapsam seçin UX ekran görüntüsü](./media/private-link-security/ampls-select-2.png)

### <a name="connect-to-a-private-endpoint"></a>Özel bir uç noktaya Bağlan

Şimdi, AMPLS 'larınızla bağlantılı kaynaklarınız olduğuna göre, ağımızı bağlamak için özel bir uç nokta oluşturun. Bu görevi, bu örnekte olduğu gibi [Azure Portal özel bağlantı merkezinde](https://portal.azure.com/#blade/Microsoft_Azure_Network/PrivateLinkCenterBlade/privateendpoints)veya Azure Izleyici özel bağlantı kapsamınızda yapabilirsiniz.

1. Kapsam kaynağında, sol taraftaki kaynak menüsünde **Özel uç nokta bağlantıları** ' na tıklayın. Uç nokta oluşturma işlemini başlatmak için **Özel uç noktaya** tıklayın. Ayrıca, özel bağlantı merkezinde başlatılan bağlantıları, seçerek ve **Onayla**' ya tıklayarak da onaylayabilirsiniz.

    ![Özel uç nokta bağlantıları UX görüntüsü](./media/private-link-security/ampls-select-private-endpoint-connect-3.png)

2. Abonelik, kaynak grubu ve uç noktanın adını ve üzerinde etkin olması gereken bölgeyi seçin. Bölgenin, bağlayacaksınız sanal ağ ile aynı bölgede olması gerekir.

3. **İleri: kaynak**' a tıklayın. 

4. Kaynak ekranında,

   a. Azure Izleyici özel kapsam kaynağınızı içeren **aboneliği** seçin. 

   b. **Kaynak türü**için **Microsoft. Insights/privatelinkscopes**' ı seçin. 

   c. **Kaynak** açılan listesinden daha önce oluşturduğunuz özel bağlantı kapsamınızı seçin. 

   d. Ileri ' ye tıklayın **: yapılandırma >**.
      ![Özel uç nokta oluştur Seç ekran görüntüsü](./media/private-link-security/ampls-select-private-endpoint-create-4.png)

5. Yapılandırma bölmesinde,

   a.    Azure Izleyici kaynaklarınıza bağlamak istediğiniz **sanal ağı** ve **alt ağı** seçin. 
 
   b.    **Özel DNS bölgesi Ile tümleştirme**için **Evet** ' i seçin ve otomatik olarak yeni bir özel DNS bölgesi oluşturmasını sağlayın. Gerçek DNS bölgeleri aşağıdaki ekran görüntüsünde gösterilenden farklı olabilir. 
 
   c.    **Gözden geçir ve oluştur**’a tıklayın.
 
   d.    Doğrulama geçişine izin verin. 
 
   e.    **Oluştur**’a tıklayın. 

    ![Create Private Endpoint2 Select ekran görüntüsü](./media/private-link-security/ampls-select-private-endpoint-create-5.png)

Artık bu Azure Izleyici özel bağlantı kapsamına bağlı yeni bir özel uç nokta oluşturdunuz.

## <a name="configure-log-analytics"></a>Log Analytics’i Yapılandır

Azure portala gidin. Log Analytics çalışma alanı kaynağında, sol taraftaki bir menü öğesi **ağ yalıtımı** vardır. Bu menüden iki farklı durumu kontrol edebilirsiniz. 

![LA ağ yalıtımı](./media/private-link-security/ampls-log-analytics-lan-network-isolation-6.png)

İlk olarak, bu Log Analytics kaynağını erişiminiz olan tüm Azure Izleyici özel bağlantı kapsamlarına bağlayabilirsiniz. **Ekle** ' ye tıklayın ve Azure Izleyici özel bağlantı kapsamını seçin.  Bağlanmak için **Uygula** ' ya tıklayın. Tüm bağlı kapsamlar Bu ekranda görünür. Bu bağlantının yapılması, bağlı sanal ağlardaki ağ trafiğinin bu çalışma alanına ulaşmasını sağlar. Bağlantıyı, [Azure izleyici kaynaklarını bağlıyoruz](#connect-azure-monitor-resources), kapsamdan bağlama ile aynı etkiye sahip hale getirme.  

İkincisi, bu kaynağa yukarıda listelenen özel bağlantı kapsamlarının dışından nasıl ulaşılırsa denetleyebilirsiniz. Alma **için genel ağ erişimine Izin ver** ' i **Hayır**olarak ayarlarsanız, bağlı kapsamların dışındaki makineler bu çalışma alanına veri yükleyebilir. **Sorgular için ortak ağ erişimine Izin ver** ' i **Hayır**olarak ayarlarsanız, kapsamların dışındaki makineler bu çalışma alanındaki verilere erişemez. Bu veriler çalışma kitaplarına, panolara, sorgu API tabanlı istemci deneyimlerini, Azure portal öngörüleri ve daha fazlasını içerir. Azure portal dışında çalışan deneyimler ve sorgu Log Analytics verileri özel bağlantılı VNET içinde de çalışıyor olması gerekir.

Erişimin bu şekilde kısıtlanması Azure Resource Manager uygulanmaz ve bu nedenle aşağıdaki sınırlamalara sahiptir:
* Verilere erişim-ortak ağlardan gelen sorguları engelleme, çoğu Log Analytics deneyim için geçerli olsa da, bazı deneyimler verileri Azure Resource Manager aracılığıyla sorgular ve bu nedenle özel bağlantı ayarları Kaynak Yöneticisi de (yakında kullanıma sunulacak özellik) uygulanmamışsa verileri sorgulayamaz. Bu, örneğin Azure Izleyici çözümlerini, çalışma kitaplarını ve öngörülerini ve LogicApp bağlayıcısını içerir.
* Çalışma alanı yönetimi-çalışma alanı ayarı ve yapılandırma değişiklikleri (Bu erişim ayarlarını açma veya kapatma dahil) Azure Resource Manager tarafından yönetilir. Uygun rolleri, izinleri, ağ denetimlerini ve denetimi kullanarak çalışma alanı yönetimine erişimi kısıtlayın. Daha fazla bilgi için bkz. [Azure Izleyici rolleri, izinleri ve güvenliği](roles-permissions-security.md).

> [!NOTE]
> [Tanılama ayarları](diagnostic-settings.md) aracılığıyla bir çalışma alanına yüklenen Günlükler ve ölçümler güvenli bir özel Microsoft kanalının üzerine gider ve bu ayarlar tarafından denetlenmez.

### <a name="log-analytics-solution-packs-download"></a>Log Analytics çözüm paketlerini indirme

Log Analytics aracısının çözüm paketlerini indirmesini sağlamak için, uygun tam etki alanı adlarını güvenlik duvarı izin verilenler listenize ekleyin. 


| Bulut ortamı | Aracı Kaynağı | Bağlantı noktaları | Yön |
|:--|:--|:--|:--|
|Azure Genel     | scadvisorcontent.blob.core.windows.net         | 443 | Outbound
|Azure Kamu | usbn1oicore.blob.core.usgovcloudapi.net | 443 |  Outbound
|Azure China 21Vianet      | mceast2oicore.blob.core.chinacloudapi.cn| 443 | Outbound

## <a name="configure-application-insights"></a>Application Insights Yapılandır

Azure portala gidin. Azure Izleyici Application Insights bileşen kaynağında, sol taraftaki bir menü öğesi **ağ yalıtımı** bulunur. Bu menüden iki farklı durumu kontrol edebilirsiniz.

![AI ağ yalıtımı](./media/private-link-security/ampls-application-insights-lan-network-isolation-6.png)

İlk olarak, bu Application Insights kaynağını, erişiminiz olan Azure Izleyici özel bağlantı kapsamlarına bağlayabilirsiniz. **Ekle** ' ye tıklayın ve **Azure Izleyici özel bağlantı kapsamını**seçin. Bağlanmak için Uygula ' ya tıklayın. Tüm bağlı kapsamlar Bu ekranda görünür. Bu bağlantının yapılması, bağlı sanal ağlardaki ağ trafiğinin bu bileşene ulaşmasını sağlar. Bağlantıyı, [Azure izleyici kaynaklarını bağlıyoruz](#connect-azure-monitor-resources), kapsamdan bağlama ile aynı etkiye sahip hale getirme. 

İkincisi, daha önce listelenen özel bağlantı kapsamlarının dışından bu kaynağa nasıl ulaşılırsa kontrol edebilirsiniz. Alma **için genel ağ erişimine Izin ver** ' i **Hayır**olarak ayarlarsanız, bağlı kapsamların dışındaki makineler veya SDK 'lar bu bileşene veri yükleyebilir. **Sorgular için ortak ağ erişimine Izin ver** ' i **Hayır**olarak ayarlarsanız, kapsamların dışındaki makineler bu Application Insights kaynaktaki verilere erişemez. Bu veriler APM günlüklerine, ölçümlere ve canlı ölçüm akışına erişimi, ayrıca çalışma kitapları, panolar, sorgu API tabanlı istemci deneyimleri, Azure portal içgörüler ve daha fazlası gibi en üstte oluşturulan deneyimlerden de oluşur. 

Portal dışı tüketim deneyimlerinin, izlenen iş yüklerini içeren özel bağlantılı VNET içinde de çalışıyor olması gerektiğini unutmayın. 

İzlenen iş yüklerini barındıran kaynakları özel bağlantıya eklemeniz gerekir. Bu, uygulama [Hizmetleri için bunu](../../app-service/networking/private-endpoint.md) nasıl yapaöğreneceksiniz.

Erişimin bu şekilde kısıtlanması yalnızca Application Insights kaynaktaki veriler için geçerlidir. Bu erişim ayarlarını açma veya kapatma dahil olmak üzere yapılandırma değişiklikleri Azure Resource Manager tarafından yönetilir. Bunun yerine, uygun rolleri, izinleri, ağ denetimlerini ve denetimi kullanarak Kaynak Yöneticisi erişimi kısıtlayın. Daha fazla bilgi için bkz. [Azure Izleyici rolleri, izinleri ve güvenliği](roles-permissions-security.md).

> [!NOTE]
> Çalışma alanı tabanlı Application Insights tamamen güvenli hale getirmek için, Application Insights kaynağa erişimin yanı sıra temel Log Analytics çalışma alanını da kapatmanız gerekir.
>
> Kod düzeyinde Tanılamalar (Profiler/Debugger), özel bağlantıyı desteklemek için kendi depolama hesabınızı sağlamanızı gerektirir. Bunun nasıl yapılacağını gösteren [Belgeler](../app/profiler-bring-your-own-storage.md) aşağıda verilmiştir.

## <a name="use-apis-and-command-line"></a>API 'Leri ve komut satırını kullanma

Azure Resource Manager şablonları ve komut satırı arabirimlerini kullanarak daha önce açıklanan işlemi otomatikleştirebilirsiniz.

Özel bağlantı kapsamları oluşturup yönetmek için [az Monitor Private-link-Scope](/cli/azure/monitor/private-link-scope?view=azure-cli-latest)kullanın. Bu komutu kullanarak kapsamlar oluşturabilir, Log Analytics çalışma alanlarını ve Application Insights bileşenlerini ilişkilendirebilir, Özel uç noktaları ekleyebilir/kaldırabilir/onaylayabilirsiniz.

Ağ erişimini yönetmek için, bayraklarını `[--ingestion-access {Disabled, Enabled}]` ve `[--query-access {Disabled, Enabled}]` [Log Analytics çalışma alanlarını](/cli/azure/monitor/log-analytics/workspace?view=azure-cli-latest) veya [Application Insights bileşenlerini](/cli/azure/ext/application-insights/monitor/app-insights/component?view=azure-cli-latest)kullanın.

## <a name="collect-custom-logs-over-private-link"></a>Özel bağlantı üzerinden özel günlükleri topla

Depolama hesapları özel günlüklerin alma işleminde kullanılır. Varsayılan olarak, hizmet tarafından yönetilen depolama hesapları kullanılır. Ancak özel bağlantılar üzerinde özel Günlükler almak için kendi depolama hesaplarınızı kullanmanız ve bunları Log Analytics çalışma alanı (ler) ile ilişkilendirmeniz gerekir. Bu hesapları [komut satırını](/cli/azure/monitor/log-analytics/workspace/linked-storage?view=azure-cli-latest)kullanarak ayarlama hakkında daha fazla ayrıntı için bkz..

Kendi depolama hesabınızı getirme hakkında daha fazla bilgi için bkz. [günlük alma Için müşterinin sahip olduğu depolama hesapları](private-storage.md)

## <a name="restrictions-and-limitations"></a>Kısıtlamalar ve sınırlamalar

### <a name="agents"></a>Aracılar

Windows ve Linux aracılarının en son sürümleri, Log Analytics çalışma alanlarına güvenli alma özelliğini etkinleştirmek için özel ağlarda kullanılmalıdır. Eski sürümler, izleme verilerini özel bir ağa karşıya yükleyemezsiniz.

**Log Analytics Windows aracısı**

Log Analytics Agent sürümü 10.20.18038.0 veya üstünü kullanın.

**Log Analytics Linux aracısı**

Aracı sürümü 1.12.25 veya üstünü kullanın. Bu şekilde, sanal makinenizde aşağıdaki komutları çalıştırın.

```cmd
$ sudo /opt/microsoft/omsagent/bin/omsadmin.sh -X
$ sudo /opt/microsoft/omsagent/bin/omsadmin.sh -w <workspace id> -s <workspace key>
```

### <a name="azure-portal"></a>Azure portal

Application Insights ve Log Analytics gibi Azure Izleyici portalı deneyimlerini kullanmak için, Azure portal ve Azure Izleyici uzantılarına özel ağlarda erişilebilir durumda izin vermeniz gerekir. Güvenlik duvarınızdan **AzureActiveDirectory**, **AzureResourceManager**, **Azurefrontkapısı. Firstpartisi**ve **Azurefrontkapısı. ön uç** [hizmeti etiketleri](../../firewall/service-tags.md) ekleyin.

### <a name="programmatic-access"></a>Programlı erişim

REST API, [CLI](/cli/azure/monitor?view=azure-cli-latest) veya PowerShell 'i özel ağlarda Azure izleyici ile kullanmak için,**AzureActiveDirectory** ve AzureResourceManager [hizmet etiketlerini](../../virtual-network/service-tags-overview.md)güvenlik duvarınızın **AzureResourceManager** içine ekleyin.  

Bu etiketlerin eklenmesi, Log Analytics çalışma alanlarını ve AI bileşenlerini sorgulama, oluşturma ve yönetme gibi eylemleri gerçekleştirmenize olanak tanır.

### <a name="application-insights-sdk-downloads-from-a-content-delivery-network"></a>Bir içerik teslim ağından SDK indirmelerini Application Insights

Tarayıcı bir CDN 'den kod indirmeyi denememesi için betikinizdeki JavaScript kodunu paketleyin. [GitHub](https://github.com/microsoft/ApplicationInsights-JS#npm-setup-ignore-if-using-snippet-setup) 'da bir örnek verilmiştir

### <a name="browser-dns-settings"></a>Tarayıcı DNS ayarları

Azure Izleyici kaynaklarınıza özel bir bağlantı üzerinden bağlanıyorsanız, bu kaynağa giden trafik ağınızda yapılandırılan özel uç noktadan sonra gelmelidir. Özel uç noktayı etkinleştirmek için DNS ayarlarınızı [özel bir uç noktaya bağlanma](#connect-to-a-private-endpoint)bölümünde açıklandığı gibi güncelleştirin. Bazı tarayıcılar, sizin ayarladığınız kullanıcılar yerine kendi DNS ayarlarını kullanır. Tarayıcı, Azure Izleyici genel uç noktalarına bağlanmayı ve özel bağlantıyı tamamen atlamayı deneyebilir. Tarayıcıların ayarlarının eski DNS ayarlarını geçersiz kılmayacağını veya önbelleğe aldığını doğrulayın. 

## <a name="next-steps"></a>Sonraki adımlar

- [Özel depolama](private-storage.md) hakkında bilgi edinin
