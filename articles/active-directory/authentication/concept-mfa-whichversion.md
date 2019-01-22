---
title: Azure MFA sunucusu veya hizmeti, şirket içi veya bulutta?
description: Azure AD Yöneticisi olarak, MFA'ın hangi sürümünü dağıtabilirim anlamak için değiştirmem gerekiyor?
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.openlocfilehash: 50340218f80dfb0b443075a3a4f85e7a5ed86159
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2019
ms.locfileid: "54433287"
---
# <a name="which-version-of-azure-mfa-is-right-for-my-organization"></a>Kuruluşum için hangi Azure MFA'ın sürümü hangisi?

Nerede ve nasıl karar vermeden önce çok faktörlü kimlik doğrulaması (MFA) dağıtmak için üç temel soruları yanıtlamanız gerekir.

* [Neyi güvenli hale getirmeye çalışıyorum?](#what-am-i-trying-to-secure)
* [Kullanıcılar nerede bulunuyor?](#where-are-the-users-located)
* [Hangi özelliklere ihtiyacım var?](#what-features-do-i-need)

Aşağıdaki bölümlerde yukarıdaki soruları yanıtlamanıza yardımcı olacak Ayrıntılar verilmiştir.

## <a name="what-am-i-trying-to-secure"></a>Neyi güvenli hale getirmeye çalışıyorum?

Doğru iki aşamalı doğrulama çözümünü belirlemek için önce neyi, ek bir kimlik doğrulama faktörü ile güvenli hale getirmeye sorusunu yanıtlamalıyız. Bu, Azure’da olan bir uygulama mi? Veya uzaktan erişim sistemi mi? Hangi, güvenli hale getirmeye çalıştığımızı belirleyerek, ın multi-Factor Authentication nerede etkinleştirilmesi gerektiği sorusunu yanıtlayabiliriz.

| Neyi güvenli hale getirmeye çalışıyorsunuz? | Bulutta MFA | MFA Sunucusu |
| --- |:---:|:---:|
| Birinci taraf Microsoft uygulamaları |● |● |
| Uygulama galerisinde SaaS uygulamaları |● |  |
| Azure AD Uygulaması Proxy üzerinden yayımlanan web uygulamaları |● |  |
| Azure AD Uygulaması Proxy üzerinden yayımlanmayan IIS uygulamaları | |● |
| VPN, RDG gibi uzaktan erişim | ● | ● |

## <a name="where-are-the-users-located"></a>Kullanıcılar nerede bulunuyor?

Ardından, kuruluşunuzun kullanıcıları bulunan, bulutta veya şirket içinde MFA sunucusu kullanan olmadığını kullanılacak doğru çözümün belirlenmesine yardımcı olduğu belirleyin.

| Kullanıcı Konumu | Bulutta MFA | MFA Sunucusu |
| --- |:---:|:---:|
| Azure Active Directory |● | |
| AD FS ile federasyon kullanana Azure AD ve şirket içi AD |● |● |
| Azure AD ve şirket içi Azure AD Connect - hiçbir parola karması eşitleme veya doğrudan kimlik doğrulama kullanan |● |● |
| Azure AD ve şirket içi parola karması eşitleme veya doğrudan kimlik doğrulaması ile Azure AD Connect - kullanan |● | |
| Şirket içi Active Directory | |● |

## <a name="what-features-do-i-need"></a>Hangi özelliklere ihtiyacım var?

Aşağıdaki tabloda, bulutta Multi-Factor Authentication ile kullanılabilen özellikler ve Multi-Factor Authentication Sunucusu ile kullanılabilen özellikler karşılaştırılmıştır.

| Özellik | Bulutta MFA | MFA Sunucusu |
| --- |:---:|:---:|
| İkinci öğe olarak mobil uygulama bildirimi | ● | ● |
| İkinci öğe olarak mobil uygulama doğrulama kodu | ● | ● |
| İkinci öğe olarak telefon araması | ● | ● |
| İkinci öğe olarak tek yönlü SMS | ● | ● |
| İkinci öğe olarak Donanım Belirteçleri | ● (genel Önizleme) | ● |
| MFA'yı desteklemeyen Office 365 istemcileri için uygulama parolaları | ● | |
| Kimlik doğrulama yöntemleri üzerinde yönetici denetimi | ● | ● |
| PIN modu | | ● |
| Sahtekarlık uyarısı | ● | ● |
| MFA Raporları | ● | ● |
| Bir Kerelik Atlama | | ● |
| Telefon aramaları için özel karşılama | ● | ● |
| Telefon aramaları için özelleştirilebilir arayan kimliği | ● | ● |
| Güvenilen IP'ler | ● | ● |
| Güvenilen cihazlar için MFA hatırlama | ● | |
| Koşullu erişim | ● | ● |
| Önbellek |  | ● |

## <a name="next-steps"></a>Sonraki adımlar

Bulutta ve şirket içindeki MFA Sunucusunda Azure Multi-Factor Authentication kullanmanın farklarını gördüğümüze göre artık Azure Multi-Factor Authentication’ı ayarlama ve kullanma zamanı gelmiş demektir. **Senaryonuzu temsil eden simgeyi seçin**

<center>

[![Bulutta MFA](./media/concept-mfa-whichversion/cloud2.png)](howto-mfa-getstarted.md)  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; [![MFA sunucusu](./media/concept-mfa-whichversion/server2.png)](howto-mfaserver-deploy.md) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </center>
