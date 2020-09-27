---
title: Azure Bilişsel Arama günlüklerini ve ölçümlerini Power BI ile görselleştirin
description: Azure Bilişsel Arama günlüklerini ve ölçümlerini Power BI ile görselleştirin
manager: eladz
author: MarkHeff
ms.author: maheff
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 09/25/2020
ms.openlocfilehash: 90691fda7b0bf58768c7e9be6a78fb27a7807186
ms.sourcegitcommit: 4313e0d13714559d67d51770b2b9b92e4b0cc629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2020
ms.locfileid: "91400375"
---
# <a name="visualize-azure-cognitive-search-logs-and-metrics-with-power-bi"></a>Azure Bilişsel Arama günlüklerini ve ölçümlerini Power BI ile görselleştirin
[Azure bilişsel arama](./search-what-is-azure-search.md) , arama hizmetinize ilişkin işlem günlüklerini ve hizmet ölçümlerini bir Azure depolama hesabında depolamanıza olanak tanır. Bu sayfa, bu bilgileri bir Power BI şablonu uygulaması aracılığıyla görselleştirmenize ilişkin yönergeler sağlar. Uygulama arama, dizin oluşturma, Işlemler ve hizmet ölçümleri hakkında bilgiler de dahil olmak üzere arama hizmetiniz hakkında ayrıntılı öngörüler sağlar.

Azure Bilişsel Arama Power BI şablon uygulamasını bulabilirsiniz: [Power BI Apps marketi](https://appsource.microsoft.com/marketplace/apps)'nde **günlükleri ve ölçümleri analiz edin** .

## <a name="how-to-get-started-with-the-app"></a>Uygulama ile çalışmaya başlama

1. Arama hizmetiniz için ölçüm ve kaynak günlüğünü etkinleştirme:

    1. Günlükleri arşivleyebileceğiniz mevcut bir [Azure depolama hesabı](../storage/common/storage-account-create.md) oluşturun veya bunu yapın
    1. Azure portal Azure Bilişsel Arama hizmetinize gidin
    1. Sol sütundaki Izleme bölümü altında **Tanılama ayarları** ' nı seçin.

        :::image type="content" source="media/search-monitor-logs-powerbi/diagnostic-settings.png" alt-text="Azure Bilişsel Arama hizmetinin Izleme bölümünde tanılama ayarlarının nasıl seçileceğini gösteren ekran görüntüsü." border="false":::

    1. **+ Tanılama ayarı Ekle** ' yi seçin
    1. **Arşivi bir depolama hesabına**çekin, depolama hesabı bilgilerinizi sağlayın ve **Operationlogs** ve **allölçümlerini** denetleyin

        :::image type="content" source="media/search-monitor-logs-powerbi/add-diagnostic-setting.png" alt-text="Tanılama Ayarları sayfasında ölçümler ve kaynak günlüğü için nasıl seçim yapılacağını gösteren ekran görüntüsü.":::
    1. **Kaydet**’i seçin

1. Günlüğe kaydetme etkinleştirildikten sonra, günlükleri ve ölçümleri oluşturmaya başlamak için arama hizmetinizi kullanın. Kapsayıcıların bu günlüklere sahip blob depolamada görünmesi bir saat kadar sürer. Arama trafiği günlükleri için bir **Öngörüler-logs-operationlogs** kapsayıcısı ve ölçümler için bir **Öngörüler-ölçümler-pt1m** kapsayıcısı görürsünüz.

1. Azure Bilişsel Arama Power BI uygulamasını [Power BI Apps Market](https://appsource.microsoft.com/marketplace/apps) 'te bulun ve yeni bir çalışma alanına veya mevcut bir çalışma alanına yüklemeyin. Uygulamaya **Azure bilişsel arama adı verilir: günlükleri ve ölçümleri analiz edin**.

1. Uygulamayı yükledikten sonra Power BI uygulama listenizden uygulamayı seçin.

    :::image type="content" source="media/search-monitor-logs-powerbi/azure-search-app-tile.png" alt-text="Uygulama listesinden seçilecek Azure Bilişsel Arama uygulamasını gösteren ekran görüntüsü.":::

1. Verilerinizi bağlamak için **Bağlan** ' ı seçin

    :::image type="content" source="media/search-monitor-logs-powerbi/get-started-with-your-new-app.png" alt-text="Azure Bilişsel Arama uygulamasında verilerinize nasıl bağlanagösterileceğini gösteren ekran görüntüsü.":::

1. Günlüklerinizi ve ölçümlerinizi içeren depolama hesabının adını girin. Varsayılan olarak, uygulama son 10 güne kadar veri arayacaktır, ancak bu değer **günler** parametresiyle değiştirilebilir.

    :::image type="content" source="media/search-monitor-logs-powerbi/connect-to-storage-account.png" alt-text="Depolama hesabı adının nasıl ekleneceğini ve Azure Bilişsel Arama Bağlan sayfasında Sorgulanacak gün sayısını gösteren ekran görüntüsü.":::

1. Kimlik doğrulama yöntemi olarak **anahtar** ' ı seçin ve depolama hesabı anahtarınızı sağlayın. Gizlilik düzeyi olarak **özel** ' i seçin. Oturum Aç'a tıklayın ve yükleme işlemine başlayın.

    :::image type="content" source="media/search-monitor-logs-powerbi/connect-to-storage-account-step-two.png" alt-text="Azure Bilişsel Arama 'e Bağlan sayfasında kimlik doğrulama yönteminin, hesap anahtarının ve gizlilik düzeyinin nasıl girişi gösteren ekran görüntüsü.":::

1. Verilerin yenilenmesini bekleyin. Bu işlem, sahip olduğunuz veri miktarına bağlı olarak biraz zaman alabilir. Verilerin yine de aşağıdaki göstergeye göre yenilenip yenilenmediğini görebilirsiniz.

    :::image type="content" source="media/search-monitor-logs-powerbi/workspace-view-refreshing.png" alt-text="Veri yenileme sayfasındaki bilgilerin nasıl okunacağını gösteren ekran görüntüsü.":::

1. Veri yenileme işlemi tamamlandıktan sonra raporu görüntülemek için **Azure bilişsel arama raporu** ' nu seçin.

    :::image type="content" source="media/search-monitor-logs-powerbi/workspace-view-select-report.png" alt-text="Veri yenileme sayfasında Azure Bilişsel Arama raporunun nasıl seçileceğini gösteren ekran görüntüsü.":::![]()

1. Verilerle birlikte açılacak şekilde raporu açtıktan sonra sayfayı yenilediğinizden emin olun.

    :::image type="content" source="media/search-monitor-logs-powerbi/powerbi-search.png" alt-text="Azure Bilişsel Arama Power BI raporunun ekran görüntüsü.":::

## <a name="how-to-change-the-app-parameters"></a>Uygulama parametrelerini değiştirme
Farklı bir depolama hesabındaki verileri görselleştirmek veya verilerin sorgulanme gün sayısını değiştirmek isterseniz, **gün** ve **storageaccount** parametrelerini değiştirmek için aşağıdaki adımları izleyin.

1. Power BI uygulamalarınıza giderek Azure Bilişsel Arama uygulamanızı bulun ve **uygulamayı Düzenle** düğmesini seçerek çalışma alanını görüntüleyin.

    :::image type="content" source="media/search-monitor-logs-powerbi/azure-search-app-tile-edit.png" alt-text="Azure Bilişsel Arama uygulaması için uygulamayı Düzenle düğmesinin nasıl seçileceğini gösteren ekran görüntüsü.":::

1. Veri kümesi seçeneklerindeki **ayarları** seçin.

    :::image type="content" source="media/search-monitor-logs-powerbi/workspace-view-select-settings.png" alt-text="Azure Bilişsel Arama veri kümesi seçeneklerinden ayarların nasıl seçileceğini gösteren ekran görüntüsü.":::

1. Veri kümeleri sekmesinden parametre değerlerini değiştirin ve **Uygula**' yı seçin. Bağlantıyla ilgili bir sorun varsa, veri kaynağı kimlik bilgilerini aynı sayfada güncelleştirin.

1. Çalışma alanına geri gidin ve veri kümesi seçeneklerinde **Şimdi Yenile** ' yi seçin.

    :::image type="content" source="media/search-monitor-logs-powerbi/workspace-view-select-refresh-now.png" alt-text="Azure Bilişsel Arama veri kümesi seçeneklerinden şimdi yenilemeyi nasıl seçileceğini gösteren ekran görüntüsü.":::

1. Güncelleştirilmiş verileri görüntülemek için raporu açın. Ayrıca, en son verileri görüntülemek için raporu yenilemeniz gerekebilir.

## <a name="troubleshooting"></a>Sorun giderme
Verilerinizi göremiyorsanız, bu sorun giderme adımlarını izleyin:

1. Raporu açın ve en son verileri görüntülediğinizden emin olmak için sayfayı yenileyin. Raporda verileri yenilemek için bir seçenek vardır. En son verileri almak için bunu seçin.

1. Girdiğiniz depolama hesabı adı ve erişim anahtarının doğru olduğundan emin olun. Depolama hesabı adı, arama hizmeti günlüklerinizin yapılandırıldığı hesaba karşılık gelmelidir.

1. Depolama hesabınızın, içgörüler- **Günlükler-operationlogs** ve **Insights-ölçümler-pt1m** ve her kapsayıcının veri içerdiğini doğrulayın. Günlükler ve ölçümler, birkaç klasör katmanında yer alacak.

1. Veri kümesinin yenileme işlemi devam ettiğinden emin olun. Yenileme durumu göstergesi yukarıdaki 8. adımda gösterilmiştir. Yenileme işlemi devam ediyorsa, raporu açmak ve yenilemek için yenileme tamamlanana kadar bekleyin.

## <a name="next-steps"></a>Sonraki adımlar
[Azure Bilişsel Arama hakkında daha fazla bilgi](./index.yml)

[Power BI nedir?](/power-bi/fundamentals/power-bi-overview)

[Power BI hizmetinde tasarımcılara yönelik temel kavramlar](/power-bi/service-basic-concepts)