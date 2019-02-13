---
title: Güvenlik Uyarıları Azure kaynak rolleri için PIM içinde yapılandırma | Microsoft Docs
description: Azure AD Privileged Identity Management (PIM) Azure kaynak rolleri için güvenlik uyarıları yapılandırmayı öğrenin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: pim
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 538661df85ca0960992cb40a53b0e149c3b566b6
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2019
ms.locfileid: "56189540"
---
# <a name="configure-security-alerts-for-azure-resource-roles-in-pim"></a>Güvenlik Uyarıları Azure kaynak rolleri için PIM içinde yapılandırma
Azure kaynakları için privileged Identity Management (PIM), ortamınızda şüpheli veya güvenli olmayan bir etkinlik olduğunda uyarılar oluşturur. Bir uyarı tetiklendiğinde, bunu bir uyarılar sayfasında gösterilir. 

![Uyarılar sayfası](media/azure-pim-resource-rbac/RBAC-alerts-home.png)

## <a name="review-alerts"></a>Uyarıları gözden geçirin
Düzeltme önerileri ile birlikte, uyarıyı tetikleyen rollerin listeleyen bir rapor görmek için bir uyarı seçin.

![Uyarı raporu](media/azure-pim-resource-rbac/rbac-alert-info.png)

## <a name="alerts"></a>Uyarılar
| Uyarı | Severity | Tetikleyici | Öneri |
| --- | --- | --- | --- |
| **Bir kaynağa çok fazla sahip atandı** |Orta |Çok sayıda kullanıcı, sahip rolünün sahip. |Listedeki kullanıcıları gözden geçirin ve bazı az ayrıcalıklı roller için yeniden atayın. |
| **Bir kaynağa çok fazla kalıcı sahip atandı** |Orta |Çok sayıda kullanıcı, bir rol kalıcı olarak atanır. |Listedeki kullanıcıları gözden geçirin ve bazı etkinleştirme rol kullanması için yeniden atayın. |
| **Yinelenen rol oluşturuldu** |Orta |Birden çok rol aynı ölçütlerine sahiptirler. |Bu roller yalnızca birini kullanın. |


### <a name="severity"></a>Severity
* **Yüksek**: Acil eylem ilke ihlali nedeniyle gerektirir. 
* **Orta**: Acil eylem gerektirmeyen ancak olası bir ilke ihlali bildirir.
* **Düşük**: Acil eylem gerektirmeyen ancak tercih edilen ilke değişikliğini önerir.

## <a name="configure-security-alert-settings"></a>Güvenlik Uyarısı Ayarları yapılandırma
Uyarılar sayfasından Git **ayarları**.
![Ayarlar](media/azure-pim-resource-rbac/rbac-navigate-settings.png)

Güvenlik hedefleri ve ortam ile çalışmak için farklı uyarıların ayarlarını özelleştirin.
![Ayarları özelleştirme](media/azure-pim-resource-rbac/rbac-alert-settings.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Güvenlik Uyarıları Azure kaynak rolleri için PIM içinde yapılandırma](pim-resource-roles-configure-alerts.md)
