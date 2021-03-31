---
title: Günlükler-Hyperscale (Citus)-PostgreSQL için Azure veritabanı
description: PostgreSQL için Azure veritabanı 'na yönelik veritabanı günlüklerine erişme-hiper ölçek (Citus)
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 7/13/2020
ms.openlocfilehash: ca3cc2873fbc6db72b10c80daecddf1471e30ff4
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "100577072"
---
# <a name="logs-in-azure-database-for-postgresql---hyperscale-citus"></a>PostgreSQL için Azure veritabanı 'nda Günlükler-hiper ölçek (Citus)

PostgreSQL günlükleri, bir hiper ölçek (Citus) sunucu grubunun her düğümünde kullanılabilir. Günlükleri bir depolama sunucusuna veya bir analiz hizmetine gönderebilirsiniz. Günlükler, yapılandırma hatalarını belirlemek, sorunlarını gidermek ve onarmak ve performans performansını düzeltmek için kullanılabilir.

## <a name="accessing-logs"></a>Günlüklere erişme

Hiper ölçek (Citus) Düzenleyicisi veya çalışan düğümü için PostgreSQL günlüklerine erişmek için, Azure portal düğümü açın:

:::image type="content" source="media/howto-hyperscale-logging/choose-node.png" alt-text="düğümlerin listesi":::

Seçili düğüm için **Tanılama ayarları**' nı açın ve **+ Tanılama ayarı Ekle**' ye tıklayın.

:::image type="content" source="media/howto-hyperscale-logging/diagnostic-settings.png" alt-text="Tanılama Ayarları Ekle düğmesi":::

Yeni Tanılama ayarları için bir ad seçin ve **Postgressqllogs** kutusunu işaretleyin.  Hangi hedefin günlükleri alacağını seçin.

:::image type="content" source="media/howto-hyperscale-logging/diagnostic-create-setting.png" alt-text="PostgreSQL günlüklerini seçin":::

## <a name="next-steps"></a>Sonraki adımlar

- [Log Analytics sorgularını kullanmaya başlama](../azure-monitor/logs/log-analytics-tutorial.md)
- [Azure Olay Hub 'ları](../event-hubs/event-hubs-about.md) hakkında bilgi edinin