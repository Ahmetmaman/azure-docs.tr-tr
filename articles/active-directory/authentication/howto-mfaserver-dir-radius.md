---
title: RADIUS and Azure MFA Server - Azure Active Directory
description: RADIUS Kimlik Doğrulaması ve Azure Multi-Factor Authentication Sunucusu dağıtılıyor.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 11/21/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: f7f773914fcdd205e9cb6b7e3505904eb9550f16
ms.sourcegitcommit: dd0304e3a17ab36e02cf9148d5fe22deaac18118
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2019
ms.locfileid: "74404269"
---
# <a name="integrate-radius-authentication-with-azure-multi-factor-authentication-server"></a>RADIUS kimlik doğrulamasını ve Azure Multi-Factor Authentication Sunucusuyla tümleştirme

RADIUS, kimlik doğrulama isteklerini kabul etmek ve bu istekleri işlemek için standart bir protokoldür. Azure Multi-Factor Authentication Sunucusu bir RADIUS sunucusu olarak görev yapabilir. İki aşamalı doğrulamayı eklemek için RADIUS istemciniz (VPN gereci) ile kimlik doğrulama hedefiniz arasına eklersiniz. Kimlik doğrulama hedefiniz, Active Directory, LDAP dizini ya da başka bir RADIUS sunucusu olabilir. Azure Multi-Factor Authentication’ın (MFA) çalışması için Azure MFA Sunucusu’nu hem istemci sunucuları hem de kimlik doğrulama hedefi ile iletişim kurabilecek şekilde yapılandırmalısınız. Azure MFA Sunucusu, RADIUS istemcisinden gelen istekleri kabul eder, kimlik doğrulama hedefine göre kimlik bilgilerini doğrular, Azure Multi-Factor Authentication ekler ve RADIUS istemcisine bir yanıt döndürür. Kimlik doğrulama isteği yalnızca hem birincil kimlik doğrulamasının hem de Azure Multi-Factor Authentication’ın başarılı olması durumunda başarılı olur.

> [!IMPORTANT]
> As of July 1, 2019, Microsoft will no longer offer MFA Server for new deployments. New customers who would like to require multi-factor authentication from their users should use cloud-based Azure Multi-Factor Authentication. Existing customers who have activated MFA Server prior to July 1 will be able to download the latest version, future updates and generate activation credentials as usual.

> [!NOTE]
> MFA sunucusu, RADIUS sunucusu olarak hareket ederken, yalnızca PAP (parola kimlik doğrulama protokolü) ve MSCHAPv2 (Microsoft Karşılıklı Kimlik Doğrulama Protokolü ) RADIUS protokollerini destekler.  MFA sunucusu bu protokolü destekleyen başka bir RADIUS sunucusu için RADIUS proxy işlevi görüyorsa, EAP (genişletilebilir kimlik doğrulama protokolü) gibi diğer protokoller kullanılabilir.
>
> Bu yapılandırmada, MFA Sunucusu alternatif protokolleri kullanarak başarılı bir RADIUS Sınama yanıtı başlatamadığından, tek yönlü SMS ve OATH belirteçleri çalışmaz.

![Radius Authentication in MFA Server](./media/howto-mfaserver-dir-radius/radius.png)

## <a name="add-a-radius-client"></a>RADIUS istemcisi ekleme

RADIUS kimlik doğrulamasını yapılandırmak için, bir Windows sunucusuna Azure Multi-Factor Authentication Sunucusu yükleyin. Bir Active Directory ortamınız varsa, sunucu ağ içindeki etki alanına eklenmelidir. Azure Multi-Factor Authentication Sunucusu’nu yapılandırmak için aşağıdaki yordamı uygulayın:

1. Azure Multi-Factor Authentication Sunucusu’nda, soldaki menüde RADIUS Kimlik Doğrulaması simgesine tıklayın.
2. **RADIUS kimlik doğrulamasını etkinleştir** onay kutusunu işaretleyin.
3. Azure MFA RADIUS hizmetinin RADIUS isteklerini standart olmayan bağlantı noktalarında dinlemesi gerekiyorsa, İstemciler sekmesinden Kimlik Doğrulama ve Hesap Oluşturma bağlantı noktalarını değiştirin.
4. **Ekle**'ye tıklayın.
5. Azure Multi-Factor Authentication Sunucusu için kimlik doğrulaması yapacak gerecin/sunucunun IP adresini, bir uygulama adı (isteğe bağlı) ve paylaşılan bir gizli dizi girin.

   Uygulama adı raporlarda görünür ve SMS veya mobil uygulama kimlik doğrulama iletilerinde görüntülenebilir.

   Paylaşılan gizli dizinin Azure Multi-Factor Authentication Sunucusu’nda ve gereçte/sunucuda aynı olması gerekir.

6. Tüm kullanıcılar Sunucu’ya aktarılmışsa ve multi-factor authentication’a tabi olacaksa, **Multi-Factor Authentication İste kullanıcı eşleme** kutusunu işaretleyin. Sunucu’ya henüz aktarılmamış veya iki aşamalı doğrulamadan muaf tutulacak çok sayıda kullanıcı varsa kutunun işaretini kaldırın.
7. Mobil doğrulama uygulamalarınızdan edindiğiniz OATH parolalarını yedekleme yöntemi olarak kullanmak istiyorsanız **Yedek OATH belirtecini etkinleştir** kutusunu işaretleyin.
8. **Tamam**’a tıklayın.

4 adımdan 8 adıma kadar yapılan işlemleri tekrarlayarak dilediğiniz kadar RADIUS istemcisi ekleyebilirsiniz.

## <a name="configure-your-radius-client"></a>RADIUS istemcinizi yapılandırma

1. **Hedef** sekmesine tıklayın.
   * If the Azure MFA Server is installed on a domain-joined server in an Active Directory environment, select **Windows domain**.
   * Kullanıcılara LDAP dizinine göre kimlik doğrulaması uygulanması gerekiyorsa **LDAP bağlaması**’nı seçin.
      Sunucu’nun dizininize bağlanabilmesi için Dizin Tümleştirme simgesine tıklayın ve Ayarlar sekmesinde LDAP yapılandırmasını düzenleyin. LDAP yapılandırma yönergeleri [LDAP Proxy yapılandırma kılavuzunda](howto-mfaserver-dir-ldap.md) bulunabilir.
   * If users should be authenticated against another RADIUS server, select **RADIUS server(s)** .
1. Azure MFA Sunucusu’nun RADIUS istekleri için proxy olarak kullanacağı sunucuyu yapılandırmak için **Ekle**’ye tıklayın.
1. RADIUS Sunucusu Ekle iletişim kutusuna RADIUS sunucusunun IP adresi İle paylaşılan bir gizli dizi girin.

   Paylaşılan gizli dizinin Azure Multi-Factor Authentication Sunucusu’nda ve RADIUS sunucusunda aynı olması gerekir. RADIUS sunucusu tarafından farklı bağlantı noktaları kullanılıyorsa, Kimlik Doğrulama bağlantı noktasını ve Hesap bağlantı noktasını değiştirin.

1. **Tamam**’a tıklayın.
1. Azure MFA Sunucusu’ndan gönderilen erişim isteklerini işleyebilmesi için Azure MFA Sunucusu’nu başka bir RADIUS sunucusunda RADIUS istemcisi olarak ekleyin. Azure Multi-Factor Authentication Sunucusu’nda yapılandırılanla aynı paylaşılan gizli diziyi kullanın.

Başka RADIUS sunucuları eklemek için bu adımları yineleyin. **Yukarı Taşı** ve **Aşağı Taşı** düğmeleriyle Azure MFA Sunucusu’nun bunları çağıracağı sırayı yapılandırabilirsiniz.

Azure Multi-Factor Authentication Sunucusu’nu başarıyla yapılandırdınız. Sunucu artık yapılandırılan istemcilerden gelen RADIUS erişim istekleri için yapılandırılan bağlantı noktalarını dinler.

## <a name="radius-client-configuration"></a>RADIUS İstemcisi yapılandırması

RADIUS istemcisini yapılandırmak için yönergeleri kullanın:

* Gerecinizi/sunucunuzu, RADIUS sunucusu olarak görev yapan Azure Multi-Factor Authentication Sunucusu’nun IP adresi için RADIUS aracılığıyla kimlik doğrulaması yapacak şekilde yapılandırın.
* Daha önce yapılandırılanla aynı paylaşılan gizli diziyi kullanın.
* Kullanıcının kimlik bilgilerini doğrulama, iki aşamalı kimlik doğrulaması gerçekleştirme, bunların yanıtını alma ve sonra RADIUS erişim isteğini yanıtlamaya yetecek kadar zaman olması için RADIUS zaman aşımını 30-60 saniye olarak yapılandırın.

## <a name="next-steps"></a>Sonraki adımlar

Bulutta Azure Multi-Factor Authentication kullanıyorsanız [RADIUS kimlik doğrulaması ile tümleştirmeyi](howto-mfa-nps-extension.md) öğrenin. 
