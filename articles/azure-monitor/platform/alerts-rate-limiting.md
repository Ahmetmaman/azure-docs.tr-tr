---
title: SMS, e-postalar, Azure uygulaması anında iletme bildirimleri ve Web kancaları için sınırlama oranı
description: Nasıl Azure olası SMS, e-posta, Azure uygulaması anında iletme veya Web kancası bildirimleri bir eylem grubu sayısını sınırlayan anlayın.
author: dkamstra
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 3/12/2018
ms.author: dukek
ms.subservice: alerts
ms.openlocfilehash: cb70dd30d2c3218ac0ad6ef4bd7b71108845f8cb
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54476453"
---
# <a name="rate-limiting-for-voice-sms-emails-azure-app-push-notifications-and-webhook-posts"></a>Ses, SMS, e-postalar, Azure uygulaması anında iletme bildirimleri ve Web kancası gönderileri sınırlama oranı
Hız sınırlaması, çok fazla belirli bir telefon numarası, e-posta adresi veya cihaz gönderildiğinde oluşan bir askıya alma bildirim olduğu. Hız sınırlaması uyarılar yönetilebilir ve işlem yapılabilir olmasını sağlar.

Hızı sınırı eşikler şunlardır:

 - **SMS**: En fazla 1 SMS 5 dakikada bir.
 - **Ses**: Sesli çağrı her 5 dakikada en fazla 1.
 - **e-posta**: En fazla 100 e-postaları bir saat içinde.
 
 Diğer Eylemler oranı sınırlı değildir.

## <a name="rate-limit-rules"></a>Hızı sınırı kuralları
- Belirli bir telefon numarası veya e-posta eşiği izin verdiğinden daha fazla ileti aldığında sınırlı oranıdır.
- Bir telefon numarası veya e-posta eylem gruplarının bir parçası, birçok farklı abonelikler arasında olabilir. Oran sınırlandırma için tüm abonelikler arasında geçerlidir. Eşiğine ulaşılmadığı sürece birden fazla aboneliklerden gönderilen iletileri bile geçerlidir.
- Bir e-posta adresi oranı sınırlı olduğunda, oran sınırlandırma iletişim kurmak için başka bir bildirim gönderilir. E-posta, oran sınırlandırma süresi dolduğunda durumları.

## <a name="next-steps"></a>Sonraki adımlar ##
* Daha fazla bilgi edinin [SMS uyarısı davranışı](alerts-sms-behavior.md).
* Alma bir [etkinlik günlüğü uyarılarına genel bakış](alerts-overview.md)ve uyarıları alma hakkında bilgi edinin.  
* Bilgi edinmek için nasıl [hizmet durumu bildirimi gönderilen her uyarıları yapılandırma](../../azure-monitor/platform/alerts-activity-log-service-notifications.md).

