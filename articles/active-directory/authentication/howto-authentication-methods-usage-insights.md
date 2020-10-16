---
title: Kimlik doğrulama yöntemleri kullanım & Öngörüler-Azure Active Directory
description: Azure AD self servis parola sıfırlama ve Multi-Factor Authentication kimlik doğrulama yöntemi kullanımı hakkında raporlama
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 11/21/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: 03989e37ac05228dade2fdcda43856e8a5240865
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/13/2020
ms.locfileid: "91964919"
---
# <a name="authentication-methods-usage--insights-preview"></a>Kimlik doğrulama yöntemleri kullanım & öngörüleri (Önizleme)

Kullanım & öngörüleri, Azure Multi-Factor Authentication ve self servis parola sıfırlama gibi özelliklerin kimlik doğrulama yöntemlerinin kuruluşunuzda nasıl çalıştığını anlamanıza olanak sağlar. Bu raporlama özelliği, kuruluşunuzda hangi yöntemlerin kaydedildiğini ve bunların nasıl kullanıldığını anlamak için gerekenleri sağlar.

## <a name="permissions-and-licenses"></a>İzinler ve lisanslar

Aşağıdaki roller kullanım ve öngörülere erişebilir:

- Genel Yönetici
- Güvenlik Okuyucusu
- Güvenlik Yöneticisi
- Rapor okuyucu

Kullanım ve Öngörüler 'e erişmek için ek lisans gerekmez. Azure Multi-Factor Authentication ve self servis parola sıfırlama (SSPR) lisanslama bilgileri [Azure Active Directory fiyatlandırma sitesinde](https://azure.microsoft.com/pricing/details/active-directory/)bulunabilir.

## <a name="how-it-works"></a>Nasıl çalışır?

Kimlik doğrulama yöntemi kullanımı ve öngörülerine erişmek için:

1. [Azure Portal](https://portal.azure.com)gidin.
1. **Azure Active Directory**  >  **parola sıfırlama**  >  **kullanımı & öngörülerine**gidin.
1. **Kayıt** veya **kullanım** genel bakışlarından, gereksinimlerinize göre filtrelemek için önceden filtrelenmiş raporları açmayı seçebilirsiniz.

![Kullanım & öngörülerine genel bakış](./media/howto-authentication-methods-usage-insights/usage-insights-overview.png)

Kullanım & öngörülerini doğrudan erişmek için adresine gidin [https://portal.azure.com/#blade/Microsoft_AAD_IAM/AuthMethodsOverviewBlade](https://portal.azure.com/#blade/Microsoft_AAD_IAM/AuthMethodsOverviewBlade) . Bu bağlantı sizi kayda genel bakış alanına getirir.

Kayıtlı kullanıcılar, kullanıcılar etkin ve kullanıcılara uygun Kutucuklar, kullanıcılarınız için aşağıdaki kayıt verilerini gösterir:

- Kayıtlı: bir Kullanıcı, kuruluşunuzun SSPR veya Multi-Factor Authentication ilkesini karşılamak için yeterli kimlik doğrulama yöntemine kaydolduklarında (veya bir yönetici), kayıtlı kabul edilir.
- Etkin: SSPR ilkesi kapsamınlarsa Kullanıcı etkin kabul edilir. Bir grup için SSPR etkinse, bu grupta olmaları durumunda Kullanıcı etkin kabul edilir. SSPR tüm kullanıcılar için etkinleştirilirse, Kiracıdaki tüm kullanıcılar (konukları hariç) etkin kabul edilir.
- Uyumlu: bir Kullanıcı, hem kaydolduklarında hem de etkinse yetenekli olarak değerlendirilir. Bu durum, gerekirse SSPR 'yi dilediğiniz zaman gerçekleştirebilecekleri anlamına gelir.

Bu kutucukların herhangi birine veya bunlarda gösterilen öngörülere tıkladığınızda, kayıt ayrıntılarının önceden filtrelenmiş bir listesi görüntülenir.

**Kayıt** sekmesindeki **kayıtlar** grafiği, kimlik doğrulama yöntemine göre başarılı ve başarısız kimlik doğrulama yöntemi kaydı sayısını gösterir. **Kullanım** sekmesindeki grafiği **sıfırlar** , kimlik doğrulama yöntemine göre parola sıfırlama akışı sırasında başarılı ve başarısız kimlik doğrulama sayısını gösterir.

Grafiklerden birine tıkladığınızda, sizi önceden filtrelenmiş bir kayıt listesi veya sıfırlama olayları görüntülenir.

Sağ üst köşedeki denetimi kullanarak, kayıtlarda gösterilen denetim verilerinin tarih aralığını değiştirebilir ve grafikleri 24 saat, 7 gün veya 30 gün olarak sıfırlar.

### <a name="registration-details"></a>Kayıt ayrıntıları

**Kayıtlı kullanıcılar**, **etkin**kullanıcılar veya **kullanıcılara** uygun kutucuk ya da Öngörüler ' e tıkladığınızda kayıt ayrıntılarına bu bilgiler gönderilir.

Kayıt ayrıntıları raporu, her kullanıcı için aşağıdaki bilgileri gösterir:

- Name
- Kullanıcı adı
- Kayıt durumu (tümü, kayıtlı, kayıtlı değil)
- Etkin durum (tümü, etkin, etkin değil)
- Özellikli durum (tümü, uyumlu değil)
- Yöntemler (uygulama bildirimi, uygulama kodu, telefon araması, SMS, e-posta, güvenlik soruları)

Listenin en üstündeki denetimleri kullanarak bir Kullanıcı arayabilir ve gösterilen sütunlara göre Kullanıcı listesini filtreleyebilirsiniz.

### <a name="reset-details"></a>Ayrıntıları Sıfırla

Kayıt veya sıfırlama grafiklerini tıklatmak sizi sıfırlama ayrıntılarına getirir.

Ayrıntıları Sıfırla raporu, son 30 günden aşağıdakiler dahil olmak üzere kayıt ve sıfırlama olaylarını gösterir:

- Name
- Kullanıcı adı
- Özellik (tümü, kayıt, sıfırlama)
- Kimlik doğrulama yöntemi (uygulama bildirimi, uygulama kodu, telefon görüşmesi, Office çağrısı, SMS, e-posta, güvenlik soruları)
- Durum (tümü, başarı, hata)

Listenin en üstündeki denetimleri kullanarak bir Kullanıcı arayabilir ve gösterilen sütunlara göre Kullanıcı listesini filtreleyebilirsiniz.

## <a name="limitations"></a>Sınırlamalar

Bu raporlarda gösterilen veriler 60 dakikaya kadar ertelenecek. Verilerinizin en son ne olduğunu belirlemek için Azure portal bir "Son yenilenme" alanı bulunur.

Kullanım ve Öngörüler verileri Azure Multi-Factor Authentication etkinlik raporlarının veya Azure AD oturum açma raporu 'nda bulunan bilgilerin yerini almaz.

Rapor şu anda dış kullanıcıları dışarıda bırakacak şekilde filtrelenebilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Kimlik doğrulama yöntemleri kullanım raporu API 'SI ile çalışma](/graph/api/resources/authenticationmethods-usage-insights-overview?view=graph-rest-beta)
- [Kuruluşunuz için kimlik doğrulama yöntemlerini seçme](concept-authentication-methods.md)
- [Birleşik kayıt deneyimi](concept-registration-mfa-sspr-combined.md)