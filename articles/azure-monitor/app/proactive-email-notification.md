---
title: Akıllı algılama bildirim değişikliği-Azure Application Insights
description: Akıllı algılamada varsayılan bildirim alıcılarına geçiş yapın. Akıllı algılama, izleme telemetride olağandışı desenler için Azure Application Insights ile uygulama izlemelerini izlemenize olanak tanır.
ms.topic: conceptual
author: harelbr
ms.author: harelbr
ms.date: 03/13/2019
ms.reviewer: mbullwin
ms.openlocfilehash: 0bf3a95814aadf687efcc27294b760e14f7d45d8
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100585634"
---
# <a name="smart-detection-e-mail-notification-change"></a>Akıllı algılama e-posta bildirimi değişikliği

Müşteri geri bildirimlerine bağlı olarak, 1 Nisan 2019 ' de, akıllı algılamada e-posta bildirimleri alan varsayılan rolleri değiştiriyoruz.

## <a name="what-is-changing"></a>Ne değişiyor?

Şu anda, akıllı algılama e-posta bildirimleri varsayılan olarak _abonelik sahibine_, _abonelik katkıya_ ve _abonelik okuyucu_ rollerine gönderilir. Bu roller genellikle izlemeye dahil olmayan kullanıcıları içerir. Bu, bu kullanıcıların çoğunun gereksiz bildirimleri almasına neden olur. Bu deneyimi geliştirmek için, e-posta bildirimlerinin yalnızca [Izleme okuyucusuna](../../role-based-access-control/built-in-roles.md#monitoring-reader) gitmesi ve varsayılan olarak [katkıda bulunan rollerinin izlenmesi](../../role-based-access-control/built-in-roles.md#monitoring-contributor) için bir değişiklik yapıyoruz.

## <a name="scope-of-this-change"></a>Bu değişikliğin kapsamı

Bu değişiklik, tüm akıllı algılama kurallarını etkiler ve aşağıdakiler hariç olur:

* Önizleme olarak işaretlenen akıllı algılama kuralları. Bu akıllı algılama kuralları, e-posta bildirimlerini bugün desteklemez.

* Hata bozuklukları kuralı. Bu kural, klasik bir uyarıdan birleştirilmiş uyarılar platformuna geçirildikten sonra yeni varsayılan rolleri hedeflemeye başlar ( [burada](../alerts/monitoring-classic-retirement.md)daha fazla bilgi bulunur.)

## <a name="how-to-prepare-for-this-change"></a>Bu değişiklik için nasıl hazırlandınız?

Akıllı algılamada e-posta bildirimlerinin ilgili kullanıcılara gönderilmesini sağlamak için, bu kullanıcıların [Izleme okuyucusuna](../../role-based-access-control/built-in-roles.md#monitoring-reader) atanması veya aboneliğin [katkıda bulunan rollerinin izlenmesi](../../role-based-access-control/built-in-roles.md#monitoring-contributor) gerekir.

Kullanıcıları Izleme okuyucusuna atamak veya Azure portal ile katkıda bulunan rolleri Izlemek için, [Azure rolleri atama](../../role-based-access-control/role-assignments-portal.md) makalesinde açıklanan adımları izleyin. _Izleme okuyucusunu_ veya _izleme katkı izlemeyi_ kullanıcıların atandığı rol olarak seçtiğinizden emin olun.

> [!NOTE]
> Kural ayarlarındaki _ek e-posta alıcıları_ seçeneği kullanılarak yapılandırılmış akıllı algılama bildirimlerinin belirli alıcıları bu değişiklikten etkilenmez. Bu alıcılar e-posta bildirimlerini almaya devam edecektir.

Bu değişiklik hakkında sorularınız veya endişeleriniz varsa [bizimle iletişime](mailto:smart-alert-feedback@microsoft.com)geçin.

## <a name="next-steps"></a>Sonraki adımlar

Akıllı algılama hakkında daha fazla bilgi edinin:

- [Hata anomalileri](./proactive-failure-diagnostics.md)
- [Bellek sızıntıları](./proactive-potential-memory-leak.md)
- [Performans anomalileri](./proactive-performance-diagnostics.md)

