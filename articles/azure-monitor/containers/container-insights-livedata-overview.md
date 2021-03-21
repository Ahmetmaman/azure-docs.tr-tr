---
title: Kapsayıcı öngörüleri ile canlı verileri görüntüleme | Microsoft Docs
description: Bu makalede, kapsayıcı öngörülerinde kubectl kullanılmadan Kubernetes günlüklerinin, olaylarının ve pod ölçümlerinin gerçek zamanlı görünümü açıklanmaktadır.
ms.topic: conceptual
ms.date: 03/04/2021
ms.custom: references_regions
ms.openlocfilehash: 5277f5051e291e9058255d8920ac0be950389704
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102203207"
---
# <a name="how-to-view-kubernetes-logs-events-and-pod-metrics-in-real-time"></a>Kubernetes günlüklerini, olayları ve pod ölçümlerini gerçek zamanlı olarak görüntüleme

Kapsayıcı öngörüleri, Azure Kubernetes Service (AKS) kapsayıcı günlüklerine (stdout/stderror), olaylara ve pod ölçümlere doğrudan erişim sağlayan gelişmiş bir tanılama özelliği olan canlı veri özelliğini içerir. `kubectl logs -c`, Ve olaylarına doğrudan erişim sunar `kubectl get` `kubectl top pods` . Konsol bölmesi, sorun giderme sorunlarını gerçek zamanlı olarak daha fazla yardımcı olmak için kapsayıcı altyapısı tarafından oluşturulan günlükleri, olayları ve ölçümleri gösterir.

Bu makale ayrıntılı bir genel bakış sağlar ve bu özelliğin nasıl kullanılacağını anlamanıza yardımcı olur.

Canlı veri özelliğini ayarlama veya sorunlarını gidermeyle ilgili yardım için [Kurulum kılavuzumuzu](container-insights-livedata-setup.md)gözden geçirin. Bu özellik, Kubernetes API 'sine doğrudan erişir ve kimlik doğrulama modeliyle ilgili ek bilgilere [buradan](https://kubernetes.io/docs/concepts/overview/kubernetes-api/)ulaşabilirsiniz.

## <a name="view-aks-resource-live-logs"></a>AKS kaynak canlı günlüklerini görüntüle
AKS kaynak görünümünden kapsayıcı içgörüler, dağıtımlar ve çoğaltma kümelerinin canlı günlüklerini görüntülemek için aşağıdaki yordamı kullanın.

1. Azure portal, AKS kümesi kaynak grubuna gidin ve AKS kaynağınızı seçin.

2. Menüdeki **Kubernetes kaynakları** bölümünde **iş yükleri** ' ni seçin.

3. İlgili sekmeden bir pod, dağıtım, çoğaltma kümesi seçin.

4. Kaynağın menüsünde **canlı Günlükler** ' i seçin.

5. Canlı verilerin toplanmasını başlatmak için bir pod seçin.

    [![Canlı dağıtım günlükleri](./media/container-insights-livedata-overview/live-data-deployment.png)](./media/container-insights-livedata-overview/live-data-deployment.png#lightbox)

## <a name="view-logs"></a>Günlükleri görüntüleme

Gerçek zamanlı günlük verilerini **düğümler**, **denetleyiciler** ve **kapsayıcılar** görünümünden kapsayıcı altyapısı tarafından oluşturulan şekilde görüntüleyebilirsiniz. Günlük verilerini görüntülemek için aşağıdaki adımları gerçekleştirin.

1. Azure portal, AKS kümesi kaynak grubuna gidin ve AKS kaynağınızı seçin.

2. AKS kümesi panosunda, sol taraftaki **izleme** altında **Öngörüler**' i seçin.

3. **Düğümler**, **denetleyiciler** veya **kapsayıcılar** sekmesinden birini seçin.

4. Performans kılavuzundan bir nesne seçin ve sağ tarafta bulunan Özellikler bölmesinde **canlı verileri görüntüle** seçeneğini belirleyin. AKS kümesi, Azure AD kullanarak çoklu oturum açma ile yapılandırıldıysa, bu tarayıcı oturumu sırasında ilk kullanımda kimlik doğrulaması yapmanız istenir. Hesabınızı seçin ve Azure ile kimlik doğrulamayı doldurun.

    >[!NOTE]
    >Özellikler bölmesinde **Analytics 'Te görünüm** seçeneğini belirleyerek Log Analytics çalışma alanınızdaki verileri görüntülerken, günlük araması sonuçları potansiyel olarak mevcut olmayan **düğümleri**, **Daemon kümelerini**, **çoğaltma kümelerini**, **Işleri**, **cron işleri**, **pods** ve **kapsayıcıları** gösterir. İçinde kullanılamayan bir kapsayıcı için günlüklere arama girişimi `kubectl` de burada başarısız olur. Geçmiş günlükleri, olayları ve ölçümleri görüntüleme hakkında daha fazla bilgi edinmek için [Analytics özelliğindeki görünümü](container-insights-log-search.md#search-logs-to-analyze-data) gözden geçirin.

Başarıyla kimlik doğrulamasından geçtikten sonra, canlı veri konsolu bölmesi, günlük verilerini sürekli bir akışta görüntüleyebileceğiniz performans verileri kılavuzunun altında görünür. Getirme durumu göstergesi, bölmenin en sağında yer alan yeşil bir onay işareti gösteriyorsa, verilerin alınabilmesi ve konsolunuza akışa başlaması anlamına gelir.

![Düğüm özellikleri bölmesi görünüm verileri seçeneği](./media/container-insights-livedata-overview/node-properties-pane.png)

Bölme başlığı, kapsayıcının gruplandırıldığı Pod 'ın adını gösterir.

## <a name="view-events"></a>Etkinlikleri görüntüleme

Bir kapsayıcı, Pod, Node, ReplicaSet, DaemonSet, Job, CronJob veya Deployment seçildiğinde, kapsayıcı motoru tarafından **düğümler**, **denetleyiciler**, **kapsayıcılar** ve **dağıtımlar** görünümünden oluşturulan gerçek zamanlı olay verilerini görüntüleyebilirsiniz. Olayları görüntülemek için aşağıdaki adımları gerçekleştirin.

1. Azure portal, AKS kümesi kaynak grubuna gidin ve AKS kaynağınızı seçin.

2. AKS kümesi panosunda, sol taraftaki **izleme** altında **Öngörüler**' i seçin.

3. **Düğümler**, **denetleyiciler**, **kapsayıcılar** ya da **dağıtımlar** sekmesini seçin.

4. Performans kılavuzundan bir nesne seçin ve sağ tarafta bulunan Özellikler bölmesinde **canlı verileri görüntüle** seçeneğini belirleyin. AKS kümesi, Azure AD kullanarak çoklu oturum açma ile yapılandırıldıysa, bu tarayıcı oturumu sırasında ilk kullanımda kimlik doğrulaması yapmanız istenir. Hesabınızı seçin ve Azure ile kimlik doğrulamayı doldurun.

    >[!NOTE]
    >Özellikler bölmesinde **Analytics 'Te görünüm** seçeneğini belirleyerek Log Analytics çalışma alanınızdaki verileri görüntülerken, günlük araması sonuçları potansiyel olarak mevcut olmayan **düğümleri**, **Daemon kümelerini**, **çoğaltma kümelerini**, **Işleri**, **cron işleri**, **pods** ve **kapsayıcıları** gösterir. İçinde kullanılamayan bir kapsayıcı için günlüklere arama girişimi `kubectl` de burada başarısız olur. Geçmiş günlükleri, olayları ve ölçümleri görüntüleme hakkında daha fazla bilgi edinmek için [Analytics özelliğindeki görünümü](container-insights-log-search.md#search-logs-to-analyze-data) gözden geçirin.

Başarıyla kimlik doğrulamasından geçtikten sonra, canlı veri konsolu bölmesi performans verileri kılavuzunun altında görünür. Getirme durumu göstergesi, bölmenin en sağında yer alan yeşil bir onay işareti gösteriyorsa, verilerin alınabilmesi ve konsolunuza akışa başlaması anlamına gelir.

Seçtiğiniz nesne bir kapsayıcı ise, bölmedeki **Olaylar** seçeneğini belirleyin. Bir düğüm, Pod veya denetleyici seçtiyseniz, olayları görüntülemek otomatik olarak seçilir.

![Denetleyici özellikleri bölmesi olayları görüntüle](./media/container-insights-livedata-overview/controller-properties-live-event.png)

Bölme başlığı, kapsayıcının gruplandırıldığı Pod 'ın adını gösterir.

### <a name="filter-events"></a>Olayları filtreleme

Olayları görüntülerken, arama çubuğunun sağında bulunan **filtre** hap 'i kullanarak sonuçları da sınırlayabilirsiniz. Seçtiğiniz kaynağa bağlı olarak, hap, arasından seçim yapmak için bir pod, Namespace veya Cluster listeler.

## <a name="view-metrics"></a>Ölçümleri görüntüle

Gerçek zamanlı ölçüm verilerini, **düğüm** veya **denetleyiciler** görünümünden yalnızca bir **Pod** seçildiğinde kapsayıcı motoru tarafından oluşturulan şekilde görüntüleyebilirsiniz. Ölçümleri görüntülemek için aşağıdaki adımları gerçekleştirin.

1. Azure portal, AKS kümesi kaynak grubuna gidin ve AKS kaynağınızı seçin.

2. AKS kümesi panosunda, sol taraftaki **izleme** altında **Öngörüler**' i seçin.

3. **Düğümler** ya da **denetleyiciler** sekmesini seçin.

4. Performans kılavuzundan bir **Pod** nesnesi seçin ve sağ tarafta bulunan Özellikler bölmesinde **canlı verileri görüntüle** seçeneğini belirleyin. AKS kümesi, Azure AD kullanarak çoklu oturum açma ile yapılandırıldıysa, bu tarayıcı oturumu sırasında ilk kullanımda kimlik doğrulaması yapmanız istenir. Hesabınızı seçin ve Azure ile kimlik doğrulamayı doldurun.

    >[!NOTE]
    >Özellikler bölmesinde **Analytics 'Te görünüm** seçeneğini belirleyerek Log Analytics çalışma alanınızdaki verileri görüntülerken, günlük araması sonuçları potansiyel olarak mevcut olmayan **düğümleri**, **Daemon kümelerini**, **çoğaltma kümelerini**, **Işleri**, **cron işleri**, **pods** ve **kapsayıcıları** gösterir. İçinde kullanılamayan bir kapsayıcı için günlüklere arama girişimi `kubectl` de burada başarısız olur. Geçmiş günlükleri, olayları ve ölçümleri görüntüleme hakkında daha fazla bilgi edinmek için [Analytics özelliğindeki görünümü](container-insights-log-search.md#search-logs-to-analyze-data) gözden geçirin.

Başarıyla kimlik doğrulamasından geçtikten sonra, canlı veri konsolu bölmesi performans verileri kılavuzunun altında görünür. Ölçüm verileri alınır ve iki grafikte sunum için konsolunuza akışa başlar. Bölme başlığı, kapsayıcının gruplandırıldığı Pod 'ın adını gösterir.

![Pod ölçümleri örneğini görüntüle](./media/container-insights-livedata-overview/pod-properties-live-metrics.png)

## <a name="using-live-data-views"></a>Canlı Veri Görünümlerini kullanma
Aşağıdaki bölümlerde, farklı canlı veri görünümlerinde kullanabileceğiniz işlevler açıklanır.

### <a name="search"></a>Arayın
Canlı veri özelliği, arama işlevlerini içerir. **Arama** alanında, önemli bir sözcük veya terim yazarak sonuçları filtreleyebilirsiniz ve tüm eşleşen sonuçlar hızlı incelemeye izin verecek şekilde vurgulanır. Olayları görüntülerken, arama çubuğunun sağında bulunan **filtre** hap 'i kullanarak sonuçları da sınırlayabilirsiniz. Seçtiğiniz kaynağa bağlı olarak, hap, arasından seçim yapmak için bir pod, Namespace veya Cluster listeler.

![Canlı veri konsol bölmesi filtre örneği](./media/container-insights-livedata-overview/livedata-pane-filter-example.png)

![Dağıtım için canlı veri konsol bölmesi filtresi örneği](./media/container-insights-livedata-overview/live-data-deployment-search.png)

### <a name="scroll-lock-and-pause"></a>Scroll Lock ve Duraklat

Otomatik kaydırmayı askıya almak ve bölmenin davranışını denetlemek için, yeni verileri okuma bölümünde el ile kaydırabilmenizi sağlayan **kaydırma** seçeneğini kullanabilirsiniz. Otomatik kaydırmayı yeniden etkinleştirmek için **kaydırma** seçeneğini tekrar seçmeniz yeterlidir. Ayrıca, **Duraklat** seçeneğini belirleyerek günlük veya olay verilerinin alınmasını da duraklatabilirsiniz ve devam etmek Için hazırsanız **oynat**' ı seçmeniz yeterlidir.

![Canlı veri konsol bölmesi canlı görünümü Duraklat](./media/container-insights-livedata-overview/livedata-pane-scroll-pause-example.png)

![Canlı veri konsol bölmesi dağıtım için canlı görünümü Duraklat](./media/container-insights-livedata-overview/live-data-deployment-pause.png)



>[!IMPORTANT]
>Bir sorunu giderirken kısa bir süre boyunca yalnızca otomatik kaydırma askıya alıp duraklamasını öneririz. Bu istekler, kümenizde Kubernetes API 'sinin kullanılabilirliğini ve azaltmasını etkileyebilir.

>[!IMPORTANT]
>Bu özelliğin çalışması sırasında hiçbir veri kalıcı olarak depolanmaz. Tarayıcınızı kapattığınızda veya uygulamadan çıktığınızda, oturum sırasında yakalanan tüm bilgiler silinir. Veriler yalnızca ölçüm özelliğinin beş dakikalık penceresinde görselleştirme için mevcut olmaya devam eder; beş dakikadan daha eski olan ölçümler de silinir. Canlı veri arabelleği, makul bellek kullanım sınırları içinde sorgular.

## <a name="next-steps"></a>Sonraki adımlar

- Azure Izleyici 'yi kullanmayı ve AKS kümenizin diğer yönlerini izlemeyi öğrenmeye devam etmek için bkz. [Azure Kubernetes hizmet durumunu görüntüleme](container-insights-analyze.md).

- Önceden tanımlanmış sorguları ve uyarı oluşturma, görselleştirmeler oluşturmak veya kümelerinizde daha fazla analiz yapmak için örnekleri görmek için [günlük sorgusu örneklerini](container-insights-log-search.md#search-logs-to-analyze-data) görüntüleyin.
