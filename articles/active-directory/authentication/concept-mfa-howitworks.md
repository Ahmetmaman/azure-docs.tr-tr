---
title: Azure Multi-Factor Authentication genel bakış
description: Azure Multi-Factor Authentication, kullanıcıların basit bir oturum açma işlemi taleplerini karşılaırken veri ve uygulamalara erişimi korumaya nasıl yardımcı olduğunu öğrenin.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/14/2020
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: b5f8d08f0814ec69719c002ea9efd39bb38b7d16
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "88718025"
---
# <a name="how-it-works-azure-multi-factor-authentication"></a>Nasıl çalışır: Azure Multi-Factor Authentication

Çok faktörlü kimlik doğrulaması bir kullanıcıdan oturum açma sürecinde ek kimlik bilgisi istenmesidir. Bu bilgi, kullanıcının cep telefonuna gönderilen bir kod veya parmak izi taraması olabilir.

Yalnızca bir kullanıcının kimliğini doğrulamak için parola kullanıyorsanız, saldırı için güvenli olmayan bir vektör bırakır. Parola zayıfmıdır veya başka bir yerde sunulduktan sonra Kullanıcı Kullanıcı adı ve parolayla oturum açmasını gerçekten mi, yoksa bir saldırgan mi? İkinci bir kimlik doğrulaması formu gerektirdiğinde, bu ek faktörün bir saldırganın alması veya yinelemesi kolay olmadığı için güvenlik artar.

![Multi-Factor Authentication 'ın farklı biçimlerinin kavramsal resmi](./media/concept-mfa-howitworks/methods.png)

Azure Multi-Factor Authentication, aşağıdaki kimlik doğrulama yöntemlerinin iki veya daha fazlasını gerektirerek işe yarar:

* Bildiğiniz bir şey, genellikle bir paroladır.
* Telefon veya donanım anahtarı gibi kolayca tekrarlanmayan güvenilir bir cihaz gibi sahip olduğunuz bir şey.
* Parmak izi veya yüz tarama gibi bir biyometri vardır.

Kullanıcılar kendi self servis parola sıfırlama ve Azure Multi-Factor Authentication için kendilerini tek bir adımda kaydedebilir ve böylece yerleşik deneyimi basitleştirir. Yöneticiler, hangi ikincil kimlik doğrulama biçimlerinin kullanılabileceğini tanımlayabilir. Ayrıca, kullanıcılar bu işlemi daha güvenli hale getirmek için bir self servis parola sıfırlaması gerçekleştirirken Azure Multi-Factor Authentication de gerekebilir.

![Oturum açma ekranında kullanılan kimlik doğrulama yöntemleri](media/concept-authentication-methods/overview-login.png)

Azure Multi-Factor Authentication, kullanıcılar için basitlik sağlarken veri ve uygulamalara erişimin korunmasına yardımcı olur. İkinci bir kimlik doğrulama biçimi gerektirerek ek güvenlik sağlar ve bir dizi kullanımı kolay [kimlik doğrulama yöntemi](concept-authentication-methods.md)aracılığıyla güçlü kimlik doğrulaması sunar. Kullanıcılar, bir yöneticinin yaptığı yapılandırma kararlarından dolayı MFA 'ya yönelik olarak istenebilir veya olmayabilir.

Uygulamalarınızın veya hizmetlerinizin Azure Multi-Factor Authentication kullanmak için herhangi bir değişiklik yapması gerekmez. Doğrulama istemleri, Azure AD oturum açma olayının bir parçasıdır. Bu, gerektiğinde MFA sınamasını otomatik olarak ister ve işler.

## <a name="available-verification-methods"></a>Kullanılabilir doğrulama yöntemleri

Bir Kullanıcı bir uygulama veya hizmette oturum açtığında ve bir MFA istemi aldıktan sonra, kayıtlı başka bir doğrulama formlarından birini seçebilirler. Bir yönetici bu Azure Multi-Factor Authentication doğrulama yöntemlerinin kaydını gerektirebilir veya Kullanıcı doğrulama yöntemlerini düzenlemek veya eklemek için kendi [profilinize](https://myprofile.microsoft.com) erişebilir.

Aşağıdaki ek doğrulama biçimleri Azure Multi-Factor Authentication ile birlikte kullanılabilir:

* Microsoft Authenticator uygulaması
* OATH donanım belirteci
* SMS
* Sesli arama

## <a name="how-to-enable-and-use-azure-multi-factor-authentication"></a>Azure Multi-Factor Authentication etkinleştirme ve kullanma

Kullanıcılar ve gruplar, oturum açma olayı sırasında daha fazla doğrulama istemek için Azure Multi-Factor Authentication için etkinleştirilebilir. Tüm Azure AD kiracılarında [güvenlik Varsayılanları](../fundamentals/concept-fundamentals-security-defaults.md) , tüm kullanıcılar için Microsoft Authenticator uygulamasının kullanımını hızlıca etkinleştirecek şekilde kullanılabilir.

Daha ayrıntılı denetimler için, [koşullu erişim](../conditional-access/overview.md) ilkeleri MFA gerektiren olayları veya uygulamaları tanımlamak için kullanılabilir. Bu ilkeler, Kullanıcı şirket ağı veya kayıtlı bir cihaz üzerinde olduğunda normal oturum açma olaylarına izin verebilir, ancak uzak veya kişisel bir cihaz üzerinde ek doğrulama faktörleri ister.

![Koşullu erişimin, oturum açma işleminin güvenliğini sağlamak için nasıl çalıştığı hakkında genel bakış Diyagramı](media/tutorial-enable-azure-mfa/conditional-access-overview.png)

## <a name="next-steps"></a>Sonraki adımlar

Lisanslama hakkında bilgi edinmek için bkz. [Azure Multi-Factor Authentication Için Özellikler ve lisanslar](concept-mfa-licensing.md).

MFA 'yı eylemde görmek için, aşağıdaki öğreticide bir test kullanıcıları kümesi için Azure Multi-Factor Authentication etkinleştirin:

> [!div class="nextstepaction"]
> [Azure Multi-Factor Authentication’ı etkinleştirme](./tutorial-enable-azure-mfa.md)