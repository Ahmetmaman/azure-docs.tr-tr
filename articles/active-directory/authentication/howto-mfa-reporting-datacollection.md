---
title: Azure AD MFA Kullanıcı verileri koleksiyonu-Azure Active Directory
description: Azure AD Multi-Factor Authentication tarafından kullanıcıların kimliğini doğrulamaya yardımcı olmak için hangi bilgiler kullanılır?
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 11/21/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: ed0d4b754911dda49776379fb318390eae411000
ms.sourcegitcommit: 0a9df8ec14ab332d939b49f7b72dea217c8b3e1e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/18/2020
ms.locfileid: "94839021"
---
# <a name="azure-ad-multi-factor-authentication-user-data-collection"></a>Azure AD Multi-Factor Authentication Kullanıcı verisi koleksiyonu

Bu belgede, kaldırmak istediğiniz olayda Azure Multi-Factor Authentication Sunucusu (MFA sunucusu) ve Azure AD MFA (bulut tabanlı) tarafından toplanan kullanıcı bilgilerinin nasıl bulunacağı açıklanmaktadır.

[!INCLUDE [gdpr-hybrid-note](../../../includes/gdpr-hybrid-note.md)]

## <a name="information-collected"></a>Toplanan bilgiler

MFA sunucusu, NPS uzantısı ve Windows Server 2016 Azure AD MFA AD FS bağdaştırıcısı, aşağıdaki bilgileri toplar ve 90 gün boyunca depolar.

Kimlik doğrulama denemeleri (Raporlama ve sorun giderme için kullanılır):

- Zaman damgası
- Kullanıcı adı
- Ad
- Soyadı
- E-posta Adresi
- Kullanıcı grubu
- Kimlik doğrulama yöntemi (telefon araması, SMS mesajı, mobil uygulama, OATH belirteci)
- Telefon araması modu (Standart, PIN)
- Kısa mesaj yönü (tek yönlü, çift yönlü)
- Kısa mesaj modu (OTP, OTP + PIN)
- Mobil uygulama modu (Standart, PIN)
- OATH belirteci modu (Standart, PIN)
- Kimlik Doğrulama Türü
- Uygulama Adı
- Birincil çağrı ülke kodu
- Birincil arama telefon numarası
- Birincil çağrı uzantısı
- Birincil çağrının kimliği doğrulandı
- Birincil çağrı sonucu
- Yedekleme çağrısı ülke kodu
- Yedekleme araması telefon numarası
- Yedekleme çağrısı uzantısı
- Yedekleme çağrısının kimliği doğrulandı
- Yedekleme çağrısı sonucu
- Genel kimlik doğrulamalı
- Genel sonuç
- Sonuçlar
- Denetiminden
- Sonuç
- IP adresi başlatılıyor
- Cihazlar
- Cihaz belirteci
- Cihaz Türü
- Mobil uygulama sürümü
- İşletim Sistemi Sürümü
- Sonuç
- Bildirim için kullanılan denetim

Etkinleştirmeler (Microsoft Authenticator mobil uygulamada bir hesabı etkinleştirmeye çalışır):
- Kullanıcı adı
- Hesap Adı
- Zaman damgası
- Etkinleştirme kodu sonucunu al
- Başarıyı etkinleştir
- Etkinleştirme hatası
- Etkinleştirme durumu sonucu
- Cihaz adı
- Cihaz Türü
- Uygulama Sürümü
- OATH belirteci etkin

Bloklar (engellenen durumu ve raporlamayı tespit etmek için kullanılır):

- Engelleme zaman damgası
- Kullanıcı adına göre engelle
- Kullanıcı adı
- Ülke Kodu
- Telefon Numarası
- Telefon numarası biçimli
- Uzantı
- Uzantıyı temizle
- Engellendi
- Engelleme nedeni
- Tamamlama zaman damgası
- Tamamlanma nedeni
- Hesap Kilitli
- Sahtekarlık uyarısı
- Sahtekarlık uyarısı engellenmedi
- Dil

Atlanır (Raporlama için kullanılır):

- Zaman damgasını atla
- Atlama Saniyeler
- Kullanıcı adına göre atla
- Kullanıcı adı
- Ülke Kodu
- Telefon Numarası
- Telefon numarası biçimli
- Uzantı
- Uzantıyı temizle
- Atlama nedeni
- Tamamlama zaman damgası
- Tamamlanma nedeni
- Atlama kullanıldı

Değişiklikler (Kullanıcı değişikliklerini MFA sunucusu veya Azure AD ile eşitlemek için kullanılır):

- Zaman damgasını Değiştir
- Kullanıcı adı
- Yeni ülke kodu
- Yeni telefon numarası
- Yeni uzantı
- Yeni yedek ülke kodu
- Yeni yedek telefon numarası
- Yeni yedekleme uzantısı
- Yeni PIN
- PIN değişikliği gerekiyor
- Eski cihaz belirteci
- Yeni cihaz belirteci

## <a name="gather-data-from-mfa-server"></a>MFA sunucusundan veri toplama

MFA sunucusu sürüm 8,0 veya üzeri için aşağıdaki işlem, yöneticilerin kullanıcılar için tüm verileri dışarı aktaralmasına izin verir:

- MFA sunucunuzda oturum açın, **Kullanıcılar** sekmesine gidin, söz konusu kullanıcıyı seçin ve **Düzenle** düğmesine tıklayın. Kullanıcıya geçerli MFA ayarlarını sağlamak için her sekmenin ekran görüntülerini (alt-PrtScn) alın.
- MFA sunucusunun komut satırından, `C:\Program Files\Multi-Factor Authentication Server\MultiFactorAuthGdpr.exe export <username>` JSON biçimli bir dosya oluşturmak için yolu yüklemenize göre değiştirerek aşağıdaki komutu çalıştırın.
- Yöneticiler, belirli bir kullanıcı için toplanan veya daha büyük bir raporlama çözümüne dahil olan tüm MFA bulut hizmeti bilgilerini dışarı aktarma seçeneği olarak Web hizmeti SDK GetUserGdpr işlemini de kullanabilir.
- `C:\Program Files\Multi-Factor Authentication Server\Logs\MultiFactorAuthSvc.log` \<username> Eklenen veya değiştirilen Kullanıcı kaydının tüm örneklerini bulmak için, "" için yedekleme ve tüm yedeklemeler (aramada tırnak işareti dahil) arayın.
   - Bu kayıtlar MFA sunucusu UX, günlük bölümü, günlük dosyaları sekmesindeki **"Kullanıcı değişikliklerini günlüğe kaydet"** seçeneğinin işaretini kaldırarak sınırlı olabilir (ancak ortadan kaldırılamaz).
   - Syslog yapılandırıldıysa ve **"Kullanıcı değişikliklerini günlüğe kaydet"** , MFA sunucusu UX, günlük bölümü, syslog sekmesinde işaretlenirse, bunun yerine günlük girdileri Syslog 'dan toplanabilir.
- MultiFactorAuthSvc. log ve kimlik doğrulama girişimleriyle ilgili diğer MFA sunucusu günlük dosyalarındaki Kullanıcı adının diğer oluşumları işletimsel kabul edilir ve MultiFactorAuthGdpr.exe Export veya Web Service SDK GetUserGdpr kullanılarak verilen bilgilere sahiptir.

## <a name="delete-data-from-mfa-server"></a>MFA sunucusundan verileri silme

MFA sunucusunun komut satırından, `C:\Program Files\Multi-Factor Authentication Server\MultiFactorAuthGdpr.exe delete <username>` bu kullanıcı için toplanan tüm MFA bulut hizmeti bilgilerini silmek için yolu, yüklemenize göre değiştirerek aşağıdaki komutu çalıştırın.

- Dışarı aktarmaya dahil edilen veriler gerçek zamanlı olarak silinir, ancak işlemsel veya yinelenen verilerin tamamen kaldırılması 30 güne kadar sürebilir.
- Yöneticiler, belirli bir kullanıcı için toplanan veya daha büyük bir raporlama çözümüne dahil olan tüm MFA bulut hizmeti bilgilerini silme seçeneği olarak Web hizmeti SDK DeleteUserGdpr işlemini de kullanabilir.

## <a name="gather-data-from-nps-extension"></a>NPS uzantısından veri toplama

Dışarı aktarma isteği yapmak için [Microsoft Gizlilik portalı](https://portal.azure.com/#blade/Microsoft_Azure_Policy/UserPrivacyMenuBlade/Overview) 'nı kullanın.

- MFA bilgileri dışarı aktarmaya dahildir, bu da tamamlanması saat veya gün olabilir.
- AzureMfa/AuthN/AuthNOptCh, AzureMfa/AuthZ/AuthZAdminCh ve AzureMfa/AuthZ/AuthZOptCh olay günlüklerinde Kullanıcı adının oluşumları işletimsel kabul edilir ve dışarı aktarma sırasında verilen bilgilere karşı silinir.

## <a name="delete-data-from-nps-extension"></a>NPS uzantısından verileri silme

Bu Kullanıcı için toplanan tüm MFA bulut hizmeti bilgilerini silmeye yönelik bir hesap isteğinin kapatılmasını sağlamak için [Microsoft Gizlilik portalı](https://portal.azure.com/#blade/Microsoft_Azure_Policy/UserPrivacyMenuBlade/Overview) 'nı kullanın.

- Verilerin tam olarak kaldırılması 30 güne kadar sürebilir.

## <a name="gather-data-from-windows-server-2016-azure-ad-mfa-ad-fs-adapter"></a>Windows Server 2016 Azure AD MFA AD FS bağdaştırıcıdan veri toplama

Dışarı aktarma isteği yapmak için [Microsoft Gizlilik portalı](https://portal.azure.com/#blade/Microsoft_Azure_Policy/UserPrivacyMenuBlade/Overview) 'nı kullanın. 

- MFA bilgileri dışarı aktarmaya dahildir, bu da tamamlanması saat veya gün olabilir.
- AD FS Izleme/hata ayıklama olay günlüklerinde (etkinse) Kullanıcı adının oluşumları işletimsel kabul edilir ve dışarı aktarma sırasında sağlanan bilgilere göre tekrarlanmaktadır.

## <a name="delete-data-from-windows-server-2016-azure-ad-mfa-ad-fs-adapter"></a>Windows Server 2016 Azure AD MFA AD FS bağdaştırıcıdan verileri silme

Bu Kullanıcı için toplanan tüm MFA bulut hizmeti bilgilerini silmeye yönelik bir hesap isteğinin kapatılmasını sağlamak için [Microsoft Gizlilik portalı](https://portal.azure.com/#blade/Microsoft_Azure_Policy/UserPrivacyMenuBlade/Overview) 'nı kullanın.

- Verilerin tam olarak kaldırılması 30 güne kadar sürebilir.

## <a name="gather-data-for-azure-ad-mfa"></a>Azure AD MFA için veri toplama

Dışarı aktarma isteği yapmak için [Microsoft Gizlilik portalı](https://portal.azure.com/#blade/Microsoft_Azure_Policy/UserPrivacyMenuBlade/Overview) 'nı kullanın.

- MFA bilgileri dışarı aktarmaya dahildir, bu da tamamlanması saat veya gün olabilir.

## <a name="delete-data-for-azure-ad-mfa"></a>Azure AD MFA için verileri silme

Bu Kullanıcı için toplanan tüm MFA bulut hizmeti bilgilerini silmeye yönelik bir hesap isteğinin kapatılmasını sağlamak için [Microsoft Gizlilik portalı](https://portal.azure.com/#blade/Microsoft_Azure_Policy/UserPrivacyMenuBlade/Overview) 'nı kullanın.

- Verilerin tam olarak kaldırılması 30 güne kadar sürebilir.

## <a name="next-steps"></a>Sonraki adımlar

[MFA sunucusu raporlama](howto-mfa-reporting.md)
