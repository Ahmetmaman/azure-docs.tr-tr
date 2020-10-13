---
title: İzleme-MySQL için Azure veritabanı
description: Bu makalede CPU, depolama ve bağlantı istatistikleri gibi MySQL için Azure veritabanı için izleme ve uyarı verme ölçümleri açıklanmaktadır.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.custom: references_regions
ms.date: 8/13/2020
ms.openlocfilehash: 9e1bd3f555873503aa1f6ed9c804aced3620fb9e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91627525"
---
# <a name="monitoring-in-azure-database-for-mysql"></a>MySQL için Azure veritabanı 'nda izleme
Sunucularınız hakkındaki izleme verileri, iş yükünüz için sorun gidermenize ve iyileştirmenize yardımcı olur. MySQL için Azure veritabanı, sunucunuzun davranışına ilişkin Öngörüler sağlayan çeşitli ölçümler sağlar.

## <a name="metrics"></a>Ölçümler
Tüm Azure ölçümlerinin bir dakikalık sıklığı vardır ve her ölçüm 30 gün geçmiş sağlar. Ölçümler üzerinde uyarılar yapılandırabilirsiniz. Adım adım yönergeler için bkz. [uyarıları ayarlama](howto-alert-on-metric.md). Diğer görevler otomatik eylemleri ayarlamayı, gelişmiş analiz gerçekleştirmeyi ve arşivleme geçmişini içerir. Daha fazla bilgi için bkz. [Azure ölçümlerine genel bakış](../monitoring-and-diagnostics/monitoring-overview-metrics.md).

### <a name="list-of-metrics"></a>Ölçüm listesi
MySQL için Azure veritabanı 'nda bu ölçümler mevcuttur:

|Ölçüm|Ölçüm görünen adı|Birim|Açıklama|
|---|---|---|---|
|cpu_percent|CPU yüzdesi|Yüzde|Kullanımdaki CPU yüzdesi.|
|memory_percent|Bellek yüzdesi|Yüzde|Kullanımdaki belleğin yüzdesi.|
|io_consumption_percent|GÇ yüzdesi|Yüzde|Kullanımdaki GÇ yüzdesi. (Temel katman sunucuları için geçerli değildir)|
|storage_percent|Depolama yüzdesi|Yüzde|Sunucunun en yüksek sınırının dışında kullanılan depolama alanı yüzdesi.|
|storage_used|Kullanılan depolama alanı|Bayt|Kullanımdaki depolama miktarı. Hizmet tarafından kullanılan depolama alanı, veritabanı dosyalarını, işlem günlüklerini ve sunucu günlüklerini içerebilir.|
|serverlog_storage_percent|Sunucu günlüğü depolama yüzdesi|Yüzde|Sunucunun en yüksek sunucu günlük depolama alanı dışında kullanılan sunucu günlük depolama alanı yüzdesi.|
|serverlog_storage_usage|Kullanılan sunucu günlüğü depolaması|Bayt|Kullanımdaki sunucu günlüğü depolama miktarı.|
|serverlog_storage_limit|Sunucu günlüğü depolama sınırı|Bayt|Bu sunucu için en fazla sunucu günlük depolama alanı.|
|storage_limit|Depolama sınırı|Bayt|Bu sunucu için en fazla depolama alanı.|
|active_connections|Etkin Bağlantılar|Sayı|Sunucuya etkin bağlantı sayısı.|
|connections_failed|Başarısız Bağlantılar|Sayı|Sunucuya yönelik başarısız bağlantı sayısı.|
|seconds_behind_master|Saniye cinsinden çoğaltma gecikmesi|Sayı|Çoğaltma sunucusunun kaynak sunucuya karşı gecikme saniye sayısı. (Temel katman sunucuları için geçerli değildir)|
|network_bytes_egress|Ağ Çıkışı|Bayt|Etkin bağlantılar arasında ağ çıkışı.|
|network_bytes_ingress|Ağ Girişi|Bayt|Etkin bağlantılar genelinde ağ.|
|backup_storage_used|Kullanılan yedekleme depolama alanı|Bayt|Kullanılan yedekleme depolama alanı miktarı. Bu ölçüm, tüm tam veritabanı yedeklemeleri, fark yedeklemeleri ve sunucu için ayarlanan yedekleme Bekletme dönemi temel alınarak korunan depolama alanının toplamını temsil eder. Yedeklemelerin sıklığı hizmet olarak yönetilir ve [Kavramlar makalesinde](concepts-backup.md)açıklanmıştır. Coğrafi olarak yedekli depolama için, yedekleme depolama alanı kullanımı yerel olarak yedekli depolama alanının iki katından oluşur.|

## <a name="server-logs"></a>Sunucu günlükleri
Sunucunuzda yavaş sorgu ve denetim günlüğünü etkinleştirebilirsiniz. Bu Günlükler Azure Izleyici günlükleri, Event Hubs ve depolama hesabı 'ndaki Azure tanılama günlükleri aracılığıyla da kullanılabilir. Günlüğe kaydetme hakkında daha fazla bilgi edinmek için [Denetim günlükleri](concepts-audit-logs.md) ' ni ve [yavaş sorgu günlüğü](concepts-server-logs.md) makalelerini ziyaret edin.

## <a name="query-store"></a>Sorgu Deposu
[Sorgu deposu](concepts-query-store.md) sorgu çalışma zamanı istatistikleri ve bekleme olayları dahil olmak üzere zaman içinde sorgu performansını izlemeyi tutan bir özelliktir. Özellik, **MySQL** şemasında çalışma zamanı performans bilgilerini sorgu ile devam ettirir. Verilerin toplanmasını ve depolanmasını çeşitli yapılandırma düğmelerini kullanarak denetleyebilirsiniz.

## <a name="query-performance-insight"></a>Sorgu Performansı İçgörüleri
[Sorgu performansı içgörüleri](concepts-query-performance-insight.md) , Azure Portal erişilebilir görselleştirmeler sağlamak üzere sorgu deposuyla birlikte çalışarak. Bu grafikler, performansı etkileyen anahtar sorgularını tanımlamanızı sağlar. Sorgu Performansı İçgörüleri, MySQL için Azure veritabanı sunucunuzun portal sayfasındaki **akıllı performans** bölümünde erişilebilir.

## <a name="performance-recommendations"></a>Performans Önerileri
[Performans önerileri](concepts-performance-recommendations.md) özelliği, iş yükü performansını artırmaya yönelik fırsatları tanımlar. Performans önerileri, iş yüklerinizin performansını geliştirmek için potansiyel olarak yeni dizinler oluşturmaya yönelik öneriler sağlar. Dizin önerileri oluşturmak için, özelliği şema ve sorgu deposu tarafından bildirilen iş yükü dahil çeşitli veritabanı özelliklerini dikkate alır. Herhangi bir performans önerisi uygulandıktan sonra, müşteriler bu değişikliklerin etkisini değerlendirmek için performansı test etmelidir.

## <a name="planned-maintenance-notification"></a>Planlı bakım bildirimi

**Planlı bakım bildirimleri** MySQL Için Azure veritabanınıza yaklaşan planlı bakım için uyarı almanızı sağlar. Bu bildirimler [hizmet durumunun](../service-health/overview.md) planlanmış bakımıyla tümleşiktir ve abonelikleriniz için tüm zamanlanmış bakımı tek bir yerden görüntülemenize olanak sağlar. Ayrıca, farklı kaynaklardan sorumlu farklı kişileriniz olabileceğinden farklı kaynak grupları için bildirimin doğru kitlelere ölçeklendirilmesine de yardımcı olur. Olaydan önce yaklaşan bakım 72 saati hakkında bildirim alacaksınız.

Planlı bakım sırasında sunucunuzun yeniden başlatılmasını bekleyebilir ve [geçici hatalar](concepts-connectivity.md#transient-errors) oluşabilir. Bu olayların çoğu, sistem tarafından 60 saniyeden daha az bir süre içinde otomatik olarak azalır.

> [!IMPORTANT]
> Planlı bakım bildirimleri şu anda tüm bölgelerde genel önizlemede bulunabilir Orta Batı ABD **hariç**

### <a name="to-receive-planned-maintenance-notification"></a>Planlı bakım bildirimi almak için

1. [Portalda](https://portal.azure.com) **hizmet durumu**' nu seçin.
2. **Uyarılar** bölümünde **sistem durumu uyarıları**' nı seçin.
3. **+ Hizmet sistem durumu uyarısı Ekle** ' yi seçin ve alanları girin.
4. Gerekli alanları doldurun. 
5. **Olay türünü**seçin, **Planlı bakım** ' ı seçin veya **tümünü seçin**
6. **Eylem gruplarında** , uyarıyı nasıl almak istediğinizi tanımlar (bir e-posta alın, mantıksal uygulama tetiklemesi vb.)  
7. Oluşturma sonrasında kuralın etkinleştir ' in Evet olarak ayarlandığından emin olun.
8. Uyarınızı gerçekleştirmek için **Uyarı kuralı oluştur** ' u seçin

**Hizmet durumu uyarıları**oluşturma hakkında ayrıntılı adımlar için, [hizmet bildirimlerinde etkinlik günlüğü uyarıları oluşturma](../service-health/alerts-activity-log-service-notifications.md)konusuna bakın.

> [!Note]
> Tüm olaylar için **Planlı bakım bildirimi** 72 saat bildirimi sağlamak için her girişimde bulunacağız. Bununla birlikte, kritik veya güvenlik düzeltme ekleri durumlarında, bildirim olaya yakın bir şekilde gönderilebilir veya atlanamaz.

## <a name="next-steps"></a>Sonraki adımlar
- Bir ölçüm üzerinde uyarı oluşturma konusunda rehberlik için [uyarıları ayarlama](howto-alert-on-metric.md) bölümüne bakın.
- Azure portal, REST API veya CLı kullanarak ölçümlere erişme ve dışarı aktarma hakkında daha fazla bilgi için bkz. [Azure ölçümleri 'Ne genel bakış](../monitoring-and-diagnostics/monitoring-overview-metrics.md).
- [Sunucunuzu izlemek için en iyi uygulamalardan](https://azure.microsoft.com/blog/best-practices-for-alerting-on-metrics-with-azure-database-for-mysql-monitoring/)blogumuzu okuyun.
