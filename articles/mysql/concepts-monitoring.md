---
title: MySQL için Azure veritabanı'nda izleme
description: Bu makalede izleme ve CPU, depolama ve bağlantı istatistikleri de dahil olmak üzere MySQL için Azure veritabanı için uyarı ölçümleri açıklanır.
services: mysql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.topic: article
ms.date: 11/01/2018
ms.openlocfilehash: 0796ab0a0923d9a04e07542c576883c004c63263
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/02/2018
ms.locfileid: "50957761"
---
# <a name="monitoring-in-azure-database-for-mysql"></a>MySQL için Azure veritabanı'nda izleme
İzleme verilerini sunucularınız hakkında sorun giderme ve iş yükünüz için iyileştirmenize yardımcı olur. MySQL için Azure veritabanı sunucunuzu davranışını öngörü sunan çeşitli ölçümleri sağlar.

## <a name="metrics"></a>Ölçümler
Tüm Azure ölçümleri bir dakikalık sıklığı, ve 30 günlük geçmişi her ölçüm sağlar. Ölçümler üzerinde uyarılar yapılandırabilirsiniz. Adım adım yönergeler için bkz. [uyarıları ayarlamak nasıl](howto-alert-on-metric.md). Diğer görevler otomatik eylemleri ayarlama, Gelişmiş analiz gerçekleştirme ve geçmiş arşivleme içerir. Daha fazla bilgi için [Azure ölçümlerine genel bakış](../monitoring-and-diagnostics/monitoring-overview-metrics.md).

### <a name="list-of-metrics"></a>Ölçümlerin listesi
Bu ölçümler, MySQL için Azure veritabanı için kullanılabilir:

|Ölçüm|Ölçüm görünen adı|Birim|Açıklama|
|---|---|---|---|---|
|cpu_percent|CPU yüzdesi|Yüzde|CPU yüzdesi kullanılıyor.|
|memory_percent|Bellek yüzdesi|Yüzde|Kullanılan bellek yüzdesi.|
|io_consumption_percent|G/ç yüzdesi|Yüzde|G/ç yüzdesi kullanılıyor.|
|storage_percent|Depolama yüzdesi|Yüzde|En fazla depolama sunucusu dışında kullanılan yüzdesi.|
|storage_used|Kullanılan depolama|Bayt|Kullanılan depolama miktarı. Hizmet tarafından kullanılan depolama, veritabanı dosyaları, işlem günlükleri ve sunucu günlüklerini içerebilir.|
|serverlog_storage_percent|Sunucu günlüğü depolama yüzdesi|Yüzde|Sunucunun en fazla sunucu günlük depolama dışında kullanılan sunucu günlük depolama yüzdesi.|
|serverlog_storage_usage|Kullanılan sunucu günlük depolama alanı|Bayt|Sunucu günlüğü depolama miktarı.|
|serverlog_storage_limit|Sunucu günlük depolama sınırı|Bayt|Bu sunucu için en fazla sunucu günlük depolama.|
|storage_limit|Depolama sınırı|Bayt|Bu sunucu için en fazla depolama alanı.|
|active_connections|Etkin bağlantılar|Sayı|Sunucu için etkin bağlantı sayısı.|
|connections_failed|Başarısız Bağlantılar|Sayı|Başarısız bağlantılar sunucu sayısı.|
|seconds_behind_master|Saniyeler içinde çoğaltma gecikmesi|Sayı|Çoğaltma sunucusu, ana sunucu karşı geciken saniye sayısı.|
|network_bytes_egress|Ağ Çıkışı|Bayt|Ağ çıkışı arasında etkin bağlantılar.|
|network_bytes_ingress|Ağ Girişi|Bayt|Ağ içinde arasında etkin bağlantılar.|
|backup_storage_used|Kullanılan yedekleme depolama alanı|Bayt|Kullanılan yedekleme depolama alanı miktarı.|

## <a name="server-logs"></a>Sunucu günlükleri
Yavaş sorgu günlüğü sunucunuzda etkinleştirebilirsiniz. Bu günlükler, OMS Log Analytics, olay hub'ları ve depolama hesabı Azure tanılama günlükleri aracılığıyla da kullanılabilir. Günlüğe kaydetme hakkında daha fazla bilgi edinmek için [sunucu günlükleri](concepts-server-logs.md) sayfası.

## <a name="next-steps"></a>Sonraki adımlar
- Bkz: [uyarıları ayarlamak nasıl](howto-alert-on-metric.md) bir ölçüme göre bir uyarı oluşturma hakkında yönergeler için.
- Erişim ve Azure portalı, REST API veya CLI kullanarak ölçümleri dışarı aktarma hakkında daha fazla bilgi için bkz. [Azure ölçümlerine genel bakış](../monitoring-and-diagnostics/monitoring-overview-metrics.md).
