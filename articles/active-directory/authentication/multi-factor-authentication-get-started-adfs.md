---
title: İki aşamalı doğrulama ve AD FS - Azure MFA | Microsoft Docs
description: Bu, nasıl Azure MFA ve AD FS kullanmaya başlayacağınızı açıklayan Azure Multi-Factor Authentication sayfasıdır.
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.openlocfilehash: 0e862f41790dff151a9f0476e6ecff3b00f86bce
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2019
ms.locfileid: "54426905"
---
# <a name="getting-started-with-azure-multi-factor-authentication-and-active-directory-federation-services"></a>Azure Multi-Factor Authentication ve Active Directory Federasyon Hizmetlerini kullanmaya başlama

<center>![Bulut](./media/multi-factor-authentication-get-started-adfs/adfs.png)</center>

Kuruluşunuz şirket için Active Directory’nizi AD FS kullanan Azure Active Directory ile birleştirdiyse Azure Multi-Factor Authentication’ın kullanılması için iki seçenek mevcuttur.

* Azure Multi-Factor Authentication veya Active Directory Federasyon Hizmetleri’ni kullanarak bulut kaynaklarını güvenli hale getirme
* Azure Multi-Factor Authentication Sunucusu kullanarak bulut ve şirket içi kaynakları güvenli hale getirme

Aşağıdaki tabloda, kaynakların Azure Multi-Factor Authentication ve AD FS ile güvenliğini sağlama arasındaki doğrulama deneyimi özetlenmiştir

| Doğrulama Deneyimi - Tarayıcı Tabanlı Uygulamalar | Doğrulama Deneyimi - Tarayıcı Tabanlı Olmayan Uygulamalar |
|:--- |:--- |:--- |
| Azure AD’yi Azure Multi-Factor Authentication kullanarak güvenli hale getirme |<li>İlk doğrulama adımı, AD FS kullanılarak şirket içinde gerçekleştirilir.</li> <li>İkinci adım, bulut kimlik doğrulaması kullanılarak yapılan telefon tabanlı yöntemdir.</li> |
| Active Directory Federasyon Hizmetleri’ni kullanarak Azure AD kaynaklarını güvenli hale getirme |<li>İlk doğrulama adımı, AD FS kullanılarak şirket içinde gerçekleştirilir.</li><li>İkinci adım, talebin onaylanmasıyla şirket içinde gerçekleştirilir.</li> |

Federasyon kullanıcıları için uygulama parolaları ile ilgili uyarılar:

* Uygulama parolaları, bulut kimlik doğrulaması kullanılarak doğrulanır ve bu nedenle federasyonu atlar. Federasyon, yalnızca uygulama parolası ayarlanırken etkin şekilde kullanılır.
* Şirket içi İstemci Erişim Denetimi ayarları, uygulama parolaları tarafından onaylanmaz.
* Uygulama parolaları için şirket içi kimlik doğrulaması günlüğe kaydetme özelliğini kaybedersiniz.
* Hesabı devre dışı bırakma/silme işlemi, bulut kimliğinde dizin eşitlemesi ve uygulama parolalarının devre dışı bırakılması/silinmesi nedeniyle üç saate kadar sürebilir.

Azure Multi-Factor Authentication ya da AD FS ile Azure Multi-Factor Authentication Sunucusu kurulumu hakkında bilgi edinmek için aşağıdaki makalelere bakın:

* [Azure Multi-Factor Authentication ve AD FS kullanarak bulut kaynaklarını güvenli hale getirme](howto-mfa-adfs.md)
* [Windows Server 2012 R2 AD FS ile Azure Multi-Factor Authentication Sunucusu kullanarak bulut ve şirket içi kaynakları güvenli hale getirme](howto-mfaserver-adfs-2012.md)
* [AD FS 2.0 ile Azure Multi-Factor Authentication Sunucusu kullanarak bulut ve şirket içi kaynakları güvenli hale getirme](howto-mfaserver-adfs-2.md)
