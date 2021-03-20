---
title: Genel koşullu erişim ilkeleri-Azure Active Directory
description: Kuruluşlar için yaygın olarak kullanılan koşullu erişim ilkeleri
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 07/02/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: bdaaed72b85bc3c0196a8897be311efa5fb714e2
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "92366352"
---
# <a name="common-conditional-access-policies"></a>Sık kullanılan Koşullu Erişim İlkeleri

[Güvenlik Varsayılanları](../fundamentals/concept-fundamentals-security-defaults.md) , bazı bazı kuruluşlar için çok daha fazla esneklik sağlar. Örneğin, birçok kuruluşun, çok faktörlü kimlik doğrulaması gerektiren koşullu erişim ilkelerinden acil erişim veya kesme camı yönetim hesapları gibi belirli hesapları hariç tutma yeteneğine ihtiyacı vardır. Bu kuruluşlar için, bu makalede başvurulan ortak ilkeler kullanılabilir.

![Azure portal koşullu erişim ilkeleri](./media/concept-conditional-access-policy-common/conditional-access-policies-azure-ad-listing.png)

## <a name="emergency-access-accounts"></a>Acil durum erişim hesapları

Acil durum erişim hesapları ve neden önemli oldukları hakkında daha fazla bilgi aşağıdaki makalelerde bulunabilir: 

* [Azure AD'deki acil durum erişim hesaplarını yönetme](../roles/security-emergency-access.md)
* [Azure Active Directory ile dayanıklı bir erişim denetimi yönetim stratejisi oluşturma](../authentication/concept-resilient-controls.md)

## <a name="typical-policies-deployed-by-organizations"></a>Kuruluşlar tarafından dağıtılan tipik ilkeler

* [Eski kimlik doğrulamasını engelle](howto-conditional-access-policy-block-legacy.md)\*
* [Yöneticiler için MFA gerektir](howto-conditional-access-policy-admin-mfa.md)\*
* [Azure yönetimi için MFA gerektir](howto-conditional-access-policy-azure-management.md)\*
* [Tüm kullanıcılar için MFA gerektir](howto-conditional-access-policy-all-users-mfa.md)\*

\* Bu dört ilke birlikte yapılandırıldığında, [güvenlik Varsayılanları](../fundamentals/concept-fundamentals-security-defaults.md)tarafından etkinleştirilen işlevselliği taklit edin.

## <a name="additional-policies"></a>Ek ilkeler

* [Oturum açma riski tabanlı koşullu erişim (Azure AD Premium P2 gerektirir)](howto-conditional-access-policy-risk.md)
* [Kullanıcı riskli tabanlı koşullu erişim (Azure AD Premium P2 gerektirir)](howto-conditional-access-policy-risk-user.md)
* [MFA kaydı için güvenilen konum gerektirme](howto-conditional-access-policy-registration.md)
* [Konuma göre erişimi engelle](howto-conditional-access-policy-location.md)
* [Uyumlu cihaz gerektirme](howto-conditional-access-policy-compliant-device.md)
* [Belirli uygulamalar haricinde erişimi engelle](howto-conditional-access-policy-block-access.md)

## <a name="next-steps"></a>Sonraki adımlar

- [Koşullu erişim What If aracını kullanarak oturum açma davranışının benzetimini yapın.](troubleshoot-conditional-access-what-if.md)

- [Yeni ilke kararlarının etkisini öğrenmek için koşullu erişim için yalnızca rapor modunu kullanın.](concept-conditional-access-report-only.md)
