---
title: Azure Active Directory Sertifika tabanlı kimlik doğrulaması Android
description: Android cihazları ile desteklenen senaryolar ve sertifika tabanlı kimlik doğrulama işlemini yapılandırmayı çözümlerinde gereksinimleri hakkında bilgi edinin
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: article
ms.date: 01/15/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: annaba
ms.openlocfilehash: 3127d4d80368723d117226c98ad51637724906b1
ms.sourcegitcommit: 58dc0d48ab4403eb64201ff231af3ddfa8412331
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/26/2019
ms.locfileid: "55080949"
---
# <a name="azure-active-directory-certificate-based-authentication-on-android"></a>Azure Active Directory Sertifika tabanlı kimlik doğrulaması Android

Android cihazlara sertifika tabanlı kimlik doğrulaması (CBA) bağlanırken cihazlarında bir istemci sertifikası kullanarak Azure Active Directory kimlik doğrulaması için kullanabilirsiniz:

* Microsoft Outlook ve Microsoft Word gibi Office mobil uygulamaları
* Exchange ActiveSync (EAS) istemcileri

Bu özelliği yapılandıran bir kullanıcı adı ve parola birleşimini belirli e-posta ve Microsoft Office uygulamaları ile mobil Cihazınızda girilmesi gereğini ortadan kaldırır.

Bu konu, gereksinimler ve desteklenen senaryoları ile Office 365 Kurumsal, iş, eğitim, US Government, Çin kiracılar, kullanıcılar için iOS(Android) cihazında CBA yapılandırmak için sağlar ve Almanya planları.

Bu özellik, Office 365 ABD kamu savunma ve Federal planlarında önizlemede kullanılabilir.

## <a name="microsoft-mobile-applications-support"></a>Microsoft mobil uygulamaları desteği

| Uygulamalar | Destek |
| --- | --- |
| Azure Information Protection uygulaması |![İşaretli][1] |
| Intune Şirket portalı |![İşaretli][1] |
| Microsoft Teams |![İşaretli][1] |
| OneNote |![İşaretli][1] |
| OneDrive |![İşaretli][1] |
| Outlook |![İşaretli][1] |
| Power BI |![İşaretli][1] |
| Skype Kurumsal |![İşaretli][1] |
| Word / Excel / PowerPoint |![İşaretli][1] |
| Yammer |![İşaretli][1] |

### <a name="implementation-requirements"></a>Uygulama gereksinimleri

Cihaz işletim sistemi sürümü Android 5.0 (Lollipop) olmalıdır ve üstü.

Bir federasyon sunucusunun yapılandırılması gerekir.

Azure istemci sertifikasını iptal etmek için Active Directory, AD FS belirteci aşağıdaki talep sahip olmanız gerekir:

* `http://schemas.microsoft.com/ws/2008/06/identity/claims/<serialnumber>` (İstemci sertifikası seri sayısı)
* `http://schemas.microsoft.com/2012/12/certificatecontext/field/<issuer>` (İstemci sertifikası verenin dize)

Azure Active Directory, AD FS belirteci (veya başka bir SAML belirteci) kullanılabilir olmaları durumunda bu talepler için yenileme belirtecini ekler. Yenileme belirteci doğrulanmış gerektiğinde, bu bilgileri iptali denetlemek için kullanılır.

En iyi uygulama, kuruluşunuzun ADFS hata sayfalarını aşağıdaki bilgilerle güncelleştirmeniz gerekir:

* Android için Microsoft Authenticator'ı yüklemek için gereksinim.
* Bir kullanıcı sertifikası alma konusunda yönergeler.

Daha fazla bilgi için [AD FS oturum açma sayfalarını özelleştirme](https://technet.microsoft.com/library/dn280950.aspx).

Bazı Office uygulamalarında (modern kimlik doğrulaması) etkin Gönder '*oturum açma istemi =*' Azure AD'ye kendi isteği. Varsayılan olarak, Azure AD çevirir '*oturum açma istemi =*'isteğindeki ADFS'*wauth usernamepassworduri =*' (U/P kimlik doğrulaması yapmak için ADFS ister) ve '*wfresh = 0*' (AD FS için sorar SSO durumu yok saymak ve yeni bir kimlik doğrulaması yapın). Sertifika tabanlı kimlik doğrulaması bu uygulamalar için etkinleştirmek istiyorsanız, varsayılan Azure AD davranışını değiştirmeniz gerekir. Ayarla '*PromptLoginBehavior*'ın, Federasyon etki alanı ayarlarınızı'*devre dışı bırakılmış*'.
Kullanabileceğiniz [msoldomainfederationsettings komutunu](/powershell/module/msonline/set-msoldomainfederationsettings?view=azureadps-1.0) bu görevi gerçekleştirmek için cmdlet:

`Set-MSOLDomainFederationSettings -domainname <domain> -PromptLoginBehavior Disabled`

## <a name="exchange-activesync-clients-support"></a>Exchange ActiveSync istemcileri destekler

Belirli bir Exchange ActiveSync uygulamaları Android 5.0 (Lollipop) veya sonraki sürümlerde desteklenir. E-posta uygulamanızı bu özelliği destekleyen belirlemek için uygulama geliştiricisine başvurun.

## <a name="next-steps"></a>Sonraki adımlar

Sertifika tabanlı kimlik doğrulaması, ortamınızda yapılandırmak istiyorsanız, bkz. [Android sertifika tabanlı kimlik doğrulaması ile çalışmaya başlama](active-directory-certificate-based-authentication-get-started.md) yönergeler için.

<!--Image references-->
[1]: ./media/active-directory-certificate-based-authentication-android/ic195031.png
