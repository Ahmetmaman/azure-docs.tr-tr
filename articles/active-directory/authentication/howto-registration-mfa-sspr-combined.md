---
title: Azure AD SSPR ve mfa'yı (Önizleme) için birleşik kaydına Başlarken
description: Azure AD multi-Factor Authentication etkin birleştirilir ve Self Servis parola sıfırlama kaydı (Önizleme)
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 03/18/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2a6896e2b9633b8de679e8d14a7957dc0e3229e7
ms.sourcegitcommit: 12d67f9e4956bb30e7ca55209dd15d51a692d4f6
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58226734"
---
# <a name="enable-combined-security-information-registration-preview"></a>Birleştirilmiş etkinleştir güvenlik bilgileri kayıt (Önizleme)

Yeni deneyimi etkinleştirmeden önce makalesini gözden geçirin [güvenlik bilgileri kayıt (Önizleme) birleştirilmiş](concept-registration-mfa-sspr-combined.md) işlevleri ve bu özellik etkisini anladığınızdan emin olmak için.

![Birleşik güvenlik bilgileri geliştirilmiş kayıt deneyimi oturum açma sırasında daha fazla bilgi isteniyor. İlk yöntem Microsoft Authenticator uygulamasını kaydetme örnek gösterir.](media/howto-registration-mfa-sspr-combined/combined-security-info-more-required.png)

|     |
| --- |
| Azure multi-Factor Authentication ve Azure AD Self Servis parola sıfırlama için birleşik güvenlik bilgileri kayıt Azure Active Directory genel Önizleme özelliğidir. Önizlemeler hakkında daha fazla bilgi için bkz: [ek kullanım koşulları Microsoft Azure önizlemeleri için](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

## <a name="enable-combined-registration"></a>Birleşik kaydı etkinleştirme

Birleşik kaydını etkinleştirmek için aşağıdaki adımları tamamlayın:

1. Azure portalına genel yönetici veya Kullanıcı Yöneticisi olarak oturum açın.
2. Gözat **Azure Active Directory** > **kullanıcı ayarları** > **erişim paneli Önizleme özellikleri için ayarları yönetme**.
3. Altında **kullanıcılar Önizleme özellikleri kaydediliyor ve güvenlik bilgilerinizi yönetmek için - yenileme**, için etkinleştirmeyi seçerseniz bir **seçili** için kullanıcı ve grup **tüm** kullanıcılar.

![Azure AD portalında tüm kullanıcıları için birleşik güvenlik bilgisi Önizleme deneyimini etkinleştirmek](media/howto-registration-mfa-sspr-combined/combined-security-info-enable.png)

> [!IMPORTANT]
> Telefon araması seçenekleri 2019'ın Mart başlangıç MFA ve SSPR kullanıcıları Azure AD ücretsiz/deneme kiracıları için kullanılabilir olmayacak. SMS iletileri, bu değişiklikten etkilenmez. Telefon Araması'nda kullanıcılara kullanılabilir Ücretli Azure AD kiracılarıyla devam eder. Bu değişiklik, yalnızca Azure AD ücretsiz/deneme kiracıları etkiler.

> [!NOTE]
> Bir kez birleşik kayıt kaydetmek veya telefon numarası ya da yeni deneyim aracılığıyla mobil uygulama bunları MFA ve SSPR, kullanabileceğiniz bu yöntemler MFA ve SSPR ilkelerinde etkinleştirilip etkinleştirilmediğini onaylayın kullanıcıları etkinleştirin. Bu deneyim devre dışı bırakırsanız, önceki SSPR kaydı için Git Kullanıcılar sayfasında `https:/aka.ms/ssprsetup` sayfa erişebilmeniz için önce çok faktörlü kimlik doğrulaması gerçekleştirmek için gerekli.

Internet Explorer'da siteyi bölgeye ataması Listesi'ni yapılandırdıysanız, aşağıdaki siteleri aynı bölgede olması gerekir:

* [https://login.microsoftonline.com](https://login.microsoftonline.com)
* [https://mysignins.microsoft.com](https://mysignins.microsoft.com)
* [https://account.activedirectory.windowsazure.com](https://account.activedirectory.windowsazure.com)

## <a name="next-steps"></a>Sonraki adımlar

[MFA ve SSPR için kullanılabilen yöntemler](concept-authentication-methods.md)

[Self Servis parola sıfırlamayı yapılandırın](howto-sspr-deployment.md)

[Azure çok faktörlü kimlik doğrulamasını yapılandırma](howto-mfa-getstarted.md)

[Güvenlik bilgileri kayıt birleştirilmiş sorunlarını giderme](howto-registration-mfa-sspr-combined-troubleshoot.md)