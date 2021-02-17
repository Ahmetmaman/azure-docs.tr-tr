---
title: Azure Izleyici 'de izleme & klasik uyarı güncelleştirmesi
description: Daha önce uyarılar altında Azure portal gösterildiği gibi, klasik izleme hizmetleri ve işlevlerinin kullanımdan kaldırılması açıklaması (klasik).
author: yanivlavi
services: azure-monitor
ms.topic: conceptual
ms.date: 2/7/2019
ms.author: yalavi
ms.subservice: alerts
ms.openlocfilehash: 5fe33fc85768bec53bfb2c998c2e0518f79b2236
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100622513"
---
# <a name="unified-alerting--monitoring-in-azure-monitor-replaces-classic-alerting--monitoring"></a>Azure Izleyici 'de izleme & birleştirilmiş uyarı, klasik uyarı & izlemeyi değiştirir

Azure Izleyici artık kaynakların tamamında ' tek ölçüm ' ve ' bir uyarı ' desteği sunan birleştirilmiş bir tam yığın izleme hizmeti haline geldi; daha fazla bilgi için [yeni Azure izleyici 'de blog gönderimize](https://azure.microsoft.com/blog/new-full-stack-monitoring-capabilities-in-azure-monitor/)bakın. Yeni Azure izleme ve uyarı platformları daha hızlı, daha akıllı ve genişletilebilir olacak şekilde geliştirilmiştir; bulut bilgi işlem ve Microsoft Akıllı bulut felseonuna göre daha fazla bilgi almak üzere tasarlanmıştır.

Yeni Azure izleme ve uyarı platformu sayesinde, Azure Izleyici 'deki klasik uyarılar, genel bulut kullanıcıları için devre dışı bırakılır, ancak henüz yeni uyarıları desteklemeyen kaynaklar için sınırlı kullanımda. Bu uyarıların devre dışı bırakılması tarihi daha fazla genişletildi. Kalan uyarılar geçişi, [Azure Kamu Bulutu](../../azure-government/documentation-government-welcome.md)ve [Azure Çin 21Vianet](https://docs.azure.cn/)için yakında yeni bir tarih duyurulacaktır.

 ![Azure portal 'de klasik uyarı](media/monitoring-classic-retirement/monitor-alert-screen2.png) 

Başlamanızı ve yeni platformda uyarılarınızı yeniden oluşturmayı öneririz.

> [!IMPORTANT]
> Etkinlik günlüğünde oluşturulan klasik uyarı kuralları kullanım dışı olmayacaktır veya geçirilmez. Etkinlik günlüğünde oluşturulan tüm klasik uyarı kuralları, yeni Azure Izleyici-uyarılarından olduğu gibi erişilebilir ve kullanılabilir. Daha fazla bilgi için bkz. [Azure izleyici kullanarak etkinlik günlüğü uyarılarını oluşturma, görüntüleme ve yönetme](./alerts-activity-log.md). Benzer şekilde, hizmet durumundaki uyarılara erişilebilir ve yeni hizmet durumu bölümünde olduğu gibi kullanılabilirler. Ayrıntılar için bkz. [hizmet durumu bildirimleri uyarıları](../../service-health/alerts-activity-log-service-notifications-portal.md).

## <a name="unified-metrics-and-alerts-in-application-insights"></a>Application Insights 'daki birleştirilmiş ölçümler ve uyarılar

Azure Izleyici 'nin daha yeni ölçüm platformu, şimdi Application Insights 'tan geliyor. Bu taşıma Işlemi, yalnızca önceki e-posta ve Web kancası çağrılarından çok daha fazla seçeneğe izin veren Application Insights eylem gruplarına kanca anlamına gelir. Uyarılar artık ServiceNow ve Automation runbook 'Ları gibi sesli aramalar, Azure Işlevleri, Logic Apps, SMS ve ıTSM araçları tetiklenebilir. Neredeyse gerçek zamanlı izleme ve uyarı sayesinde yeni platform, Application Insights kullanıcıların diğer Azure kaynakları genelinde izlemeyi ve Microsoft ürünlerinin izlenmesini yeniden dağıtmalarını sağlar.

Application Insights için yeni Birleşik Izleme ve uyarı şu şekilde alınacaktır:

- **Application Insights platform ölçümleri** : Application Insights üründen popüler ön derlenmiş ölçümler sağlar. Daha fazla bilgi için, [yeni Azure izleyici Application Insights Için platform ölçümlerini](../app/pre-aggregated-metrics-log-metrics.md#pre-aggregated-metrics)kullanma hakkında bu makaleye bakın.
- **Application Insights kullanılabilirliği ve Web testi** , Web uygulamanızın veya sunucunuzun yanıt hızını ve kullanılabilirliğini değerlendirmenizi sağlar. Daha fazla bilgi için, [yeni Azure izleyici Application Insights kullanılabilirlik testlerini ve uyarılarını](../app/monitor-web-app-availability.md)kullanma hakkında bu makaleye bakın.
- İzleme ve uyarılar için kendi ölçümlerini tanımlamanızı ve yaymanızı sağlayan **özel ölçümler Application Insights** . Daha fazla bilgi için, [yeni Azure izleyici Application Insights Için özel ölçüm](../app/pre-aggregated-metrics-log-metrics.md#custom-metrics-dimensions-and-pre-aggregation)kullanma hakkında bu makaleye bakın.
- **Application Insights hata bozukluileri (akıllı algılama 'nın parçası)** : Web UYGULAMANıZ başarısız http istekleri veya bağımlılık çağrıları ücretlerinde anormal bir artış yaşıyorsa sizi gerçek zamanlı olarak neredeyse gerçek zamanlı olarak bildirir. Daha fazla bilgi için [akıllı algılama-hata Anormallarını](../app/proactive-failure-diagnostics.md)kullanma hakkında bu makaleye bakın.

## <a name="unified-metrics-and-alerts-for-other-azure-resources"></a>Diğer Azure kaynakları için birleştirilmiş ölçümler ve uyarılar

Mart 2018 ' den itibaren Azure kaynakları için bir sonraki nesil uyarı ve çok boyutlu izleme kullanılabilirliği vardır. Artık yeni ölçüm platformu ve uyarı, neredeyse gerçek zamanlı yetenekler sayesinde daha hızlıdır. Daha da önemlisi, daha yeni platform, belirli bir değer birleşimine, koşula veya işleme dilimlemenize ve filtrelemenize olanak sağlayan boyutlar seçeneğini içerdiği için daha yeni ölçüm platformu uyarıları daha fazla ayrıntı sağlar. Yeni Azure Izleyici 'deki tüm uyarılar gibi, daha yeni ölçüm uyarıları ActionGroups kullanımı ile daha geniştir. bildirimler, e-posta veya Web kancasının ötesinde SMS, Voice, Azure Işlevi, Otomasyon Runbook 'U ve daha fazlasını genişletmeye olanak tanır. Daha fazla bilgi için bkz. [Azure izleyici kullanarak ölçüm uyarıları oluşturma, görüntüleme ve yönetme](./alerts-metric.md).
Azure kaynakları için yeni ölçümler şu şekilde kullanılabilir:

- **Azure Izleyici standart platform ölçümleri** : çeşitli Azure hizmetlerinden ve ürünlerinden daha önceden doldurulan popüler ölçümler sağlar. Daha fazla bilgi için [Azure izleyici 'de desteklenen ölçümler](./alerts-metric-near-real-time.md#metrics-and-dimensions-supported) ve [Azure izleyici 'de ölçüm uyarılarını destekleme](./alerts-metric-overview.md#supported-resource-types-for-metric-alerts)hakkında bu makaleye bakın.
- **Azure Izleyici özel ölçümleri** : Azure tanılama Aracısı da dahil olmak üzere Kullanıcı tarafından yönetilen kaynaklardan ölçümler sağlar. Daha fazla bilgi için bkz. [Azure izleyici 'de özel ölçümler](../essentials/metrics-custom-overview.md)hakkında bu makaleye bakın. Özel ölçümleri kullanarak, [Windows Azure tanılama Aracısı](../essentials/collect-custom-metrics-guestos-resource-manager-vm.md) ve [etkileyen telegraf Aracısı](../essentials/collect-custom-metrics-linux-telegraf.md)tarafından toplanan ölçümleri de yayımlayabilirsiniz.

## <a name="retirement-of-classic-monitoring-and-alerting-platform"></a>Klasik izleme ve uyarı platformunun kullanımdan kaldırılması

Daha önce belirtildiği gibi, eski klasik izleme ve uyarı genel bulut kullanıcıları için devre dışı bırakılır; ilgili API 'lerin kapatılmasını, Azure portal arabirimi ve Hizmetleri, ancak henüz yeni uyarıları desteklemeyen kaynaklar için sınırlı kullanımda de dahil olmak üzere. Özellikle, bu özellikler kullanım dışı olacaktır:

- Azure kaynakları için daha eski (klasik) ölçümler ve uyarılar Azure portal, [Uyarılar (klasik) bölümünde](./alerts-classic.overview.md) Şu anda kullanılabilir; [Microsoft. Insights/alertrules](/rest/api/monitor/alertrules) kaynağı olarak erişilebilir
- Application Insights için eski (klasik) platform ve özel ölçümler, Azure portal [Uyarılar (klasik) bölümünde](./alerts-classic.overview.md) Şu anda kullanılabilir olarak ve [Microsoft. Insights/alertrules](/rest/api/monitor/alertrules) kaynağı olarak erişilebilir olarak uyarma
- Daha eski (klasik) hata anomali uyarısı Şu anda Azure portal [Application Insights Içinde akıllı algılama](../app/proactive-diagnostics.md) olarak sunulmaktadır; Azure portal [Uyarılar (klasik) bölümünde](./alerts-classic.overview.md) gösterilen uyarılarla yapılandırılır

Diğer bir deyişle:

- Klasik izleme ve uyarılar hizmeti kullanımdan kaldırılacak ve yeni uyarı kuralları oluşturmak için artık kullanılamayacak.
- Uyarılarda (klasik) devam eden herhangi bir uyarı kuralı, bildirimler yürütülmeye ve tetiklenmesine devam edecektir.
- Klasik izleme & uyarılarındaki uyarı kuralları, taşınabilecek bir şekilde Microsoft tarafından, birkaç hafta yayılan aşamalarda yeni Azure izleyici platformunda eşdeğer bir şekilde otomatik olarak taşınır. İşlem herhangi bir kesinti olmadan sorunsuz olacaktır ve müşterilerin izleme kapsamında hiçbir kaybı olmayacaktır.
- Yeni uyarılar platformuna geçirilen uyarı kuralları, izleme kapsamını daha önce olduğu gibi sağlar, ancak yeni yüklerle bildirim tetikleyecektir. Klasik uyarı kuralıyla ilişkili herhangi bir e-posta adresi, Web kancası uç noktası veya mantıksal uygulama bağlantısı, geçirildiğinde ileri taşınır, ancak uyarı yükü yeni platformda farklı olacak şekilde çalışmayabilir.
- [Otomatik olarak geçirilemeyen](../alerts/alerts-understand-migration.md#manually-migrating-classic-alerts-to-newer-alerts) ve kullanıcılardan el ile eylem gerektiren bazı klasik uyarı kuralları çalışmaya devam edecektir.

> [!IMPORTANT]
> Microsoft Azure Izleyicisi, bazı bilgisayarlarda klasik uyarı kurallarını yeni platforma yakında [geçirmek için aşamalar aracında](../alerts/alerts-using-migration-tool.md) kullanıma alındı. Ve, hala var olan ve geçirilebilecek olan tüm klasik uyarı kurallarını zorlayarak çalıştırın. Müşterilerin, klasik uyarı kurallarının Application Insights veya [Birleşik ölçümler ve uyarılarından](#unified-metrics-and-alerts-for-other-azure-resources)oluşan [Birleşik ölçümler ve](#unified-metrics-and-alerts-in-application-insights) uyarılardan yeni yükü işlemek için, klasik uyarı kuralı yükünün otomatikleştirilmesini sağlamak için uyarlanmasını sağlamak gerekecektir. Daha fazla bilgi için bkz. [Klasik uyarı kuralı geçişine hazırlanma](../alerts/alerts-prepare-migration.md)

Bu makale yeni Azure izleme & uyarı işlevselliğiyle ilgili ayrıntıların & ayrıntılarla ve kullanıcılara yeni Azure Izleyici platformunu benimseme konusunda yardımcı olan araçların kullanılabilirliğine yönelik bağlantılarla birlikte sürekli olarak güncelleştirilecektir.

## <a name="pricing-for-migrated-alert-rules"></a>Geçirilen uyarı kuralları fiyatlandırması

Azure Monitor [Klasik uyarılarınızı](./alerts-classic.overview.md) yeni uyarılar deneyimine geçirmenize yardımcı olmak için bir geçiş aracı kullanıma sunuyoruz. Geçirilen uyarı kuralları ve karşılık gelen geçirilmiş eylem grupları (e-posta, Web kancası veya LogicApp) ücretsiz olarak kalır. Eşiği, toplama türünü ve toplama ayrıntı düzeyini düzenleme özelliği de dahil olmak üzere klasik uyarılarla sahip olduğunuz işlevsellik, geçirilen uyarı kuralınız ile ücretsiz olarak kullanılabilir olmaya devam edecektir. Ancak, yeni uyarı platformu özelliklerinden herhangi birini, bildirimleri veya eylem türlerini kullanmak için geçirilmiş uyarı kuralını düzenlerseniz, buna karşılık gelen bir ücret uygulanır. Uyarı kuralları ve bildirimlerin fiyatlandırmasıyla ilgili daha fazla bilgi için bkz. [Azure Monitor fiyatlandırması](https://azure.microsoft.com/pricing/details/monitor/).

Aşağıda, uyarı kuralınız için ücret ödemeniz gereken durumların örnekleri verilmiştir:

- Yeni Azure Izleyici platformunda ücretsiz birimlerin ötesinde oluşturulan yeni (geçirilmeyen) uyarı kuralı
- Azure Izleyici tarafından dahil edilen ve ücretsiz birimlerin ötesinde saklanan tüm veriler
- Application Insights tarafından yürütülen tüm çoklu test Web testleri
- Azure Izleyici 'de yer alan ücretsiz birimlerin ötesinde depolanan özel ölçümler
- Sıklık, birden çok kaynak/boyut, [dinamik eşikler](../alerts/alerts-dynamic-thresholds.md), kaynak/sinyal değiştirme vb. gibi daha yeni ölçüm uyarısı özelliklerini kullanmak için düzenlenen tüm geçirilmiş uyarı kuralları.
- Daha yeni bildirimleri veya SMS, sesli çağrı ve/veya ıTSM tümleştirmesi gibi eylem türlerini kullanmak için düzenlenen geçirilmiş eylem grupları.

## <a name="next-steps"></a>Sonraki adımlar

* [Yeni Birleşik Azure İzleyicisi](../overview.md)hakkında bilgi edinin.
* Yeni [Azure uyarıları](../platform/alerts-overview.md)hakkında daha fazla bilgi edinin.

