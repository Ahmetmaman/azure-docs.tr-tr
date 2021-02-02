---
title: Azure Active Directory Uygulama Ara Sunucusu sık sorulan sorular
description: Şirket içi uygulamaları uzak kullanıcılara yayımlamak için Azure AD Uygulama Ara Sunucusu kullanma hakkında sık sorulan soruların (SSS) yanıtlarını öğrenin.
services: active-directory
author: kenwith
manager: daveba
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: reference
ms.date: 07/23/2020
ms.author: kenwith
ms.reviewer: japere
ms.custom: contperf-fy21q2
ms.openlocfilehash: 121dcdf51374f625ad7393bb181b1be215775a0b
ms.sourcegitcommit: d49bd223e44ade094264b4c58f7192a57729bada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/02/2021
ms.locfileid: "99257786"
---
# <a name="active-directory-azure-ad-application-proxy-frequently-asked-questions"></a>Active Directory (Azure AD) uygulama proxy 'Si hakkında sık sorulan sorular

Bu sayfa Azure Active Directory (Azure AD) uygulama proxy 'Si hakkında sıkça sorulan soruları yanıtlar.

## <a name="enabling-azure-ad-application-proxy"></a>Azure AD Uygulama Ara Sunucusu etkinleştiriliyor

### <a name="what-license-is-required-to-use-azure-ad-application-proxy"></a>Azure AD Uygulama Ara Sunucusu kullanmak için hangi lisans gerekir?

Azure AD Uygulama Ara Sunucusu kullanmak için bir Azure AD Premium P1 veya P2 lisansına sahip olmanız gerekir. Lisanslama hakkında daha fazla bilgi için bkz. [Azure Active Directory fiyatlandırması](https://azure.microsoft.com/pricing/details/active-directory/)

### <a name="what-happens-to-azure-ad-application-proxy-in-my-tenant-if-my-license-expires"></a>Azure AD Uygulama Ara Sunucusu, Kiracımın süresi dolarsa ne olur?
Lisansınızın süresi dolarsa, uygulama proxy 'Si otomatik olarak devre dışı bırakılır. Uygulama bilgileriniz bir yıla kadar kaydedilir.

### <a name="why-is-the-enable-application-proxy-button-grayed-out"></a>"Uygulama proxy 'Sini etkinleştir düğmesi neden gri renkte?

En az bir Azure AD Premium P1 veya P2 lisansına sahip olduğunuzdan ve Azure AD Uygulama Ara Sunucusu bağlayıcısının yüklü olduğundan emin olun. İlk bağlayıcınızı başarıyla yükledikten sonra Azure AD Uygulama Ara Sunucusu hizmeti otomatik olarak etkinleştirilir.

## <a name="connector-configuration"></a>Bağlayıcı yapılandırması

### <a name="can-application-proxy-connector-services-run-in-a-different-user-context-than-the-default"></a>Uygulama proxy Bağlayıcısı Hizmetleri varsayılandan farklı bir kullanıcı bağlamında çalıştırılabilir mi?

Hayır, bu senaryo desteklenmiyor. Varsayılan ayarlar şunlardır:

- Microsoft AAD uygulama proxy Bağlayıcısı-WAPCSvc-Network Service
- Microsoft AAD Uygulama Ara Sunucusu Bağlayıcı Güncelleştiricisi-WAPCUpdaterSvc-NT Authorıty\system

### <a name="can-a-guest-user-with-the-global-administrator-or-the-application-administrator-role-register-the-connector-for-the-guest-tenant"></a>Küresel yönetici ile bir Konuk Kullanıcı veya uygulama Yöneticisi rolü, (konuk) kiracısı bağlayıcısını kaydedebilir mi?

Hayır, şu anda mümkün değildir. Kayıt denemesi her zaman kullanıcının giriş kiracısında yapılır.

### <a name="my-back-end-application-is-hosted-on-multiple-web-servers-and-requires-user-session-persistence-stickiness-how-can-i-achieve-session-persistence"></a>Arka uç Uygulamam birden çok Web sunucusunda barındırılır ve Kullanıcı oturumu kalıcılığı (süreklilik) gerektirir. Oturum kalıcılığını nasıl sağlayabilirim? 

Öneriler için bkz. [uygulama proxy bağlayıcılarınızın ve uygulamalarınızın yüksek kullanılabilirliği ve yük dengelemesi](application-proxy-high-availability-load-balancing.md).

### <a name="is-tls-termination-tlshttps-inspection-or-acceleration-on-traffic-from-the-connector-servers-to-azure-supported"></a>Bağlayıcı sunuculardan Azure 'a giden trafik için TLS sonlandırması (TLS/HTTPS denetimi veya hızlandırma) mi kullanıyorsunuz?

Uygulama proxy Bağlayıcısı, Azure 'da sertifika tabanlı kimlik doğrulaması gerçekleştirir. TLS sonlandırma (TLS/HTTPS denetimi veya hızlandırma) bu kimlik doğrulama yöntemini keser ve desteklenmez. Bağlayıcıdan Azure 'a giden trafiğin TLS sonlandırmasını gerçekleştiren tüm cihazları atlaması gerekir.  

### <a name="is-tls-12-required-for-all-connections"></a>TLS 1,2 tüm bağlantılar için gereklidir mi?
Evet. Müşterilerimize en iyi sınıf şifrelemeyi sağlamak için, uygulama proxy hizmeti yalnızca TLS 1,2 protokollerine erişimi sınırlandırır. Bu değişiklikler 31 Ağustos 2019 tarihinden itibaren kademeli olarak alındı ve geçerli. Tüm istemci sunucu ve tarayıcı-sunucu birleşimlerinizin, uygulama proxy hizmeti ile bağlantı sağlamak için TLS 1,2 kullanacak şekilde güncelleştirildiğinden emin olun. Bunlar, kullanıcılarınızın uygulama proxy 'Si aracılığıyla yayımlanan uygulamalara erişmek için kullandığı istemcileri içerir. Faydalı başvurular ve kaynaklar için bkz. [Office 365 ' de TLS 1,2](/microsoft-365/compliance/prepare-tls-1.2-in-office-365) için hazırlanma.

### <a name="can-i-place-a-forward-proxy-device-between-the-connector-servers-and-the-back-end-application-server"></a>Bağlayıcı sunucu (ler) ve arka uç uygulama sunucusu arasında bir iletme proxy cihazı yerleştirebilir miyim?
Evet, bu senaryo bağlayıcı sürümü 1.5.1526.0 'dan başlayarak desteklenir. Bkz. [mevcut şirket içi proxy sunucularıyla çalışma](application-proxy-configure-connectors-with-proxy-servers.md).

### <a name="should-i-create-a-dedicated-account-to-register-the-connector-with-azure-ad-application-proxy"></a>Bağlayıcıyı Azure AD Uygulama Ara Sunucusu kaydetmek için adanmış bir hesap oluşturmalı mıyım?

Bunun için bir neden yoktur. Herhangi bir genel yönetici veya uygulama yöneticisi hesabı çalışacaktır. Yükleme sırasında girilen kimlik bilgileri kayıt işleminden sonra kullanılmaz. Bunun yerine, bu noktadan kimlik doğrulaması için kullanılan bağlayıcıya bir sertifika verilir.

### <a name="how-can-i-monitor-the-performance-of-the-azure-ad-application-proxy-connector"></a>Azure AD Uygulama Ara Sunucusu bağlayıcısının performansını nasıl izleyebilirim?

Bağlayıcıyla birlikte yüklenen performans Izleme sayaçları vardır. Bunları görüntülemek için:  

1. **Başlat**' ı seçin, "perfmon" YAZıN ve ENTER tuşuna basın.
2. **Performans İzleyicisi** ' ni seçin ve yeşil **+** simgesine tıklayın.
3. İzlemek istediğiniz **MICROSOFT AAD uygulama proxy Bağlayıcısı** sayaçlarını ekleyin.

### <a name="does-the-azure-ad-application-proxy-connector-have-to-be-on-the-same-subnet-as-the-resource"></a>Azure AD Uygulama Ara Sunucusu bağlayıcısının kaynakla aynı alt ağda olması gerekir mi?

Bağlayıcının aynı alt ağda olması gerekmez. Ancak, kaynak için ad çözümlemesi (DNS, ana bilgisayar dosyası) ve gerekli ağ bağlantısı (kaynağa yönlendirme, kaynakta açık bağlantı noktaları, vb.) gerekir. Öneriler için [Azure Active Directory uygulama ara sunucusu kullanırken ağ topolojisi konuları](application-proxy-network-topology.md)bölümüne bakın.

### <a name="what-versions-of-windows-server-can-i-install-a-connector-on"></a>Windows Server 'ın hangi sürümlerini üzerine bir bağlayıcı yükleyebilirim?

Uygulama proxy 'Si Windows Server 2012 R2 veya üstünü gerektirir. Windows Server 2019 için HTTP2 üzerinde şu anda bir sınırlama vardır. Bağlayıcıyı Windows Server 2019 ' de başarılı bir şekilde kullanabilmek için aşağıdaki kayıt defteri anahtarını eklemeniz ve sunucuyu yeniden başlatmanız gerekir:

```
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Internet Settings\WinHttp\EnableDefaultHttp2 (DWORD) Value: 0 
```

## <a name="application-configuration"></a>Uygulama yapılandırması

### <a name="i-am-receiving-an-error-about-an-invalid-certificate-or-possible-wrong-password"></a>Geçersiz bir sertifika veya hatalı parola ile ilgili bir hata alıyorum

SSL sertifikasını karşıya yükledikten sonra portalda "geçersiz sertifika, yanlış parola" iletisini alırsınız.

Bu hatayı gidermek için bazı ipuçları aşağıda verilmiştir:
- Sertifikayla ilgili sorunları kontrol edin. Yerel bilgisayarınıza yükler. Herhangi bir sorunla karşılaşmıyorsanız, sertifika iyidir.
- Parolanın özel karakter içermediğinden emin olun. Sınama için parola yalnızca 0-9, A-Z ve A-z karakterlerini içermelidir.
- Sertifika Microsoft yazılım anahtarı depolama sağlayıcısı ile oluşturulduysa, RSA algoritması kullanılmalıdır.

### <a name="what-is-the-length-of-the-default-and-long-back-end-timeout-can-the-timeout-be-extended"></a>Varsayılan ve "uzun" arka uç zaman aşımı uzunluğu nedir? Zaman aşımı genişletilmiş olabilir mi?

Varsayılan uzunluk 85 saniyedir. "Long" ayarı 180 saniyedir. Zaman aşımı sınırı genişletilemiyor.

### <a name="can-a-service-principal-manage-application-proxy-using-powershell-or-microsoft-graph-apis"></a>Bir hizmet sorumlusu, PowerShell veya Microsoft Graph API 'Leri kullanarak uygulama ara sunucusunu yönetebilir mi?

Hayır, bu şu anda desteklenmiyor.

### <a name="how-do-i-change-the-landing-page-my-application-loads"></a>Uygulamamın giriş sayfasını Nasıl yaparım? değiştirmek mi?

Uygulama kayıtları sayfasında, giriş sayfasının istenen dış URL 'si için giriş sayfası URL 'sini değiştirebilirsiniz. Uygulama Uygulamalarım veya Office 365 portalından başlatıldığında belirtilen sayfa yüklenir. Yapılandırma adımları için bkz. [Azure AD uygulama ara sunucusu ile yayımlanan uygulamalar için özel bir giriş sayfası ayarlama](./application-proxy-configure-custom-home-page.md)

### <a name="can-only-iis-based-applications-be-published-what-about-web-applications-running-on-non-windows-web-servers-does-the-connector-have-to-be-installed-on-a-server-with-iis-installed"></a>Yalnızca IIS tabanlı uygulamalar yayımlanabilir mi? Windows dışı Web sunucularında çalışan Web uygulamaları nelerdir? Bağlayıcının IIS yüklü bir sunucuda yüklü olması gerekir mi?

Hayır, yayımlanan uygulamalar için IIS gereksinimi yoktur. Windows Server dışındaki sunucularda çalışan Web uygulamalarını yayımlayabilirsiniz. Ancak, Web sunucusunun anlaşmayı (Kerberos kimlik doğrulaması) destekleyip desteklemediğine bağlı olarak, Windows dışı bir sunucu ile ön kimlik doğrulaması kullanmeyebilirsiniz. Bağlayıcının yüklü olduğu sunucuda IIS gerekli değildir.

### <a name="can-i-configure-application-proxy-to-add-the-hsts-header"></a>Uygulama proxy 'sini HSTS üst bilgisini eklemek için yapılandırabilir miyim?
Uygulama proxy 'Si HTTP Strict-Transport-Security üst bilgisini HTTPS yanıtlarına otomatik olarak eklemez, ancak yayımlanan uygulama tarafından gönderilen özgün yanıt ise üstbilgiyi korur. Bu işlevi etkinleştirmek için bir ayarı sağlamak yol haritasında bulunur. Bunu yanıtlara eklemeyi sağlayan bir önizleme ile ilgileniyorsanız, Ayrıntılar için ' e ulaşın aadapfeedback@microsoft.com .

## <a name="integrated-windows-authentication"></a>Tümleşik Windows Kimlik Doğrulaması

### <a name="when-should-i-use-the-principalsallowedtodelegatetoaccount-method-when-setting-up-kerberos-constrained-delegation-kcd"></a>Kerberos kısıtlı temsilci (KCD) ayarlarken Principalsallodilimtodelegatetoaccount metodunu ne zaman kullanmalıyım?

Bağlayıcı sunucular Web uygulaması hizmet hesabından farklı bir etki alanında olduğunda Principalsallodilimtodelegatetoaccount yöntemi kullanılır. Kaynak tabanlı kısıtlanmış temsilin kullanılmasını gerektirir.
Bağlayıcı sunucuları ve Web uygulaması hizmet hesabı aynı etki alanında yer alıyorsa, her bağlayıcı makine hesabında temsilci ayarlarını yapılandırmak için Active Directory Kullanıcıları ve bilgisayarları kullanabilirsiniz, böylece hedef SPN 'nin temsilciliğini alabilir.

Bağlayıcı sunucuları ve Web uygulaması hizmet hesabı farklı etki alanlarındaysa, kaynak tabanlı temsili kullanılır. Temsili izinleri hedef Web sunucusunda ve Web uygulaması hizmeti hesabında yapılandırılır. Bu kısıtlı yetkilendirme yöntemi nispeten yenidir. Yöntemi, kaynak (Web hizmeti) sahibinin hangi makine ve hizmet hesaplarının temsil edebilir olduğunu denetlemesine izin vererek etki alanları arası temsilciyi destekleyen Windows Server 2012 ' de kullanıma sunulmuştur. Bu yapılandırmaya yardımcı olacak bir kullanıcı arabirimi yoktur, bu nedenle PowerShell kullanmanız gerekir.
Daha fazla bilgi için bkz. [uygulama proxy 'si Ile Kerberos kısıtlanmış temsilciyi anlama](https://aka.ms/kcdpaper).

### <a name="does-ntlm-authentication-work-with-azure-ad-application-proxy"></a>NTLM kimlik doğrulaması Azure AD Uygulama Ara Sunucusu ile çalışıyor mu?

NTLM kimlik doğrulaması, ön kimlik doğrulama veya çoklu oturum açma yöntemi olarak kullanılamaz. NTLM kimlik doğrulaması yalnızca istemci ile yayınlanan web uygulaması arasında doğrudan anlaşılırken kullanılabilir. NTLM kimlik doğrulamasının kullanılması genellikle tarayıcıda bir oturum açma isteminin görünmesine neden olur.

### <a name="can-i-use-the-logon-identity-on-premises-user-principal-name-or-on-premises-sam-account-name-in-a-b2b-iwa-single-sign-on-scenario"></a>"Şirket içi Kullanıcı asıl adı" veya "Şirket içi SAM hesap adı" oturum açma kimliğini bir B2B ıWA çoklu oturum açma senaryosunda kullanabilir miyim?

Hayır, Azure AD 'deki bir Konuk Kullanıcı yukarıda belirtilen oturum açma kimliklerinden herhangi biri için gerekli olan özniteliğe sahip olmadığından bu işe yaramayacaktır.

Bu durumda, "Kullanıcı asıl adı" na geri dönüş olur. B2B senaryosu hakkında daha fazla bilgi için lütfen [Azure AD 'de B2B kullanıcılarına şirket içi uygulamalarınıza erişim verme](../external-identities/hybrid-cloud-to-on-premises.md)konusunu okuyun.

## <a name="pass-through-authentication"></a>Doğrudan kimlik doğrulama

### <a name="can-i-use-conditional-access-policies-for-applications-published-with-pass-through-authentication"></a>Doğrudan kimlik doğrulamasıyla yayımlanan uygulamalar için koşullu erişim Ilkelerini kullanabilir miyim?

Koşullu erişim Ilkeleri yalnızca Azure AD 'de başarıyla önceden kimliği doğrulanmış kullanıcılar için zorlanır. Doğrudan kimlik doğrulaması Azure AD kimlik doğrulamasını tetiklemez, bu nedenle koşullu erişim Ilkeleri zorlanamaz. Doğrudan kimlik doğrulaması ile, MFA ilkelerinin şirket içi sunucuda, mümkünse veya Azure AD Uygulama Ara Sunucusu ön kimlik doğrulaması etkinleştirilerek uygulanması gerekir.

### <a name="can-i-publish-a-web-application-with-client-certificate-authentication-requirement"></a>İstemci sertifikası kimlik doğrulaması gereksinimiyle bir Web uygulaması yayımlayabilir miyim?

Hayır, uygulama proxy 'Si TLS trafiğini sonlandıracağından bu senaryo desteklenmez.  

## <a name="remote-desktop-gateway-publishing"></a>Uzak Masaüstü Ağ Geçidi yayımlama

### <a name="how-can-i-publish-remote-desktop-gateway-over-azure-ad-application-proxy"></a>Azure AD Uygulama Ara Sunucusu üzerinden Uzak Masaüstü Ağ Geçidi nasıl yayımlayabilirim?

[Azure AD uygulama ara sunucusu uzak masaüstünü yayımlama](application-proxy-integrate-with-remote-desktop-services.md)konusuna bakın.

### <a name="can-i-use-kerberos-constrained-delegation-single-sign-on---windows-integrated-authentication-in-the-remote-desktop-gateway-publishing-scenario"></a>Uzak Masaüstü Ağ Geçidi yayımlama senaryosunda Kerberos kısıtlanmış temsilciyi (tek Sign-On-Windows tümleşik kimlik doğrulaması) kullanabilir miyim?

Hayır, bu senaryo desteklenmiyor.  

### <a name="my-users-dont-use-internet-explorer-11-and-the-pre-authentication-scenario-doesnt-work-for-them-is-this-expected"></a>Kullanıcılarım Internet Explorer 11 kullanmıyor ve ön kimlik doğrulama senaryosu bunlar için çalışmıyor. Bu beklensin mi?

Evet, bekleniyor. Ön kimlik doğrulama senaryosu, üçüncü taraf tarayıcılarda desteklenmeyen bir ActiveX denetimi gerektirir.

### <a name="is-the-remote-desktop-web-client-html5-supported"></a>Uzak Masaüstü Web Istemcisi (HTML5) destekleniyor mu?

Evet, bu senaryo şu anda genel önizlemededir. [Azure AD uygulama ara sunucusu uzak masaüstünü yayımlama](application-proxy-integrate-with-remote-desktop-services.md)konusuna bakın.

### <a name="after-i-configured-the-pre-authentication-scenario-i-realized-that-the-user-has-to-authenticate-twice-first-on-the-azure-ad-sign-in-form-and-then-on-the-rdweb-sign-in-form-is-this-expected-how-can-i-reduce-this-to-one-sign-in"></a>Ön kimlik doğrulama senaryosunu yapılandırdıktan sonra, kullanıcının kimlik doğrulamasının iki kez olduğunu fark ediyorum: ilk olarak Azure AD oturum açma formunda ve ardından RDWeb oturum açma formunda. Bu beklensin mi? Bunu bir oturum açma ile nasıl azaltabilirim?

Evet, bekleniyor. Kullanıcının bilgisayarı Azure AD 'ye katılırsa, Kullanıcı Azure AD 'de otomatik olarak oturum açar. Kullanıcının kimlik bilgilerini yalnızca RDWeb oturum açma formunda sağlaması gerekir.

## <a name="sharepoint-publishing"></a>SharePoint yayımlaması

### <a name="how-can-i-publish-sharepoint-over-azure-ad-application-proxy"></a>Azure AD Uygulama Ara Sunucusu SharePoint üzerinden nasıl yayımlayabilirim?

[Azure AD uygulama ara sunucusu Ile SharePoint 'e uzaktan erişimi etkinleştirme](application-proxy-integrate-with-sharepoint-server.md)bölümüne bakın.

### <a name="can-i-use-the-sharepoint-mobile-app-ios-android-to-access-a-published-sharepoint-server"></a>SharePoint mobil uygulamasını (iOS/Android), yayımlanmış bir SharePoint sunucusuna erişmek için kullanabilir miyim?

[SharePoint mobil uygulaması](/sharepoint/administration/supporting-the-sharepoint-mobile-apps-online-and-on-premises) şu anda Azure Active Directory ön kimlik doğrulamasını desteklemez.

## <a name="active-directory-federation-services-ad-fs-publishing"></a>Active Directory Federasyon Hizmetleri (AD FS) (AD FS) yayımlama 

### <a name="can-i-use-azure-ad-application-proxy-as-ad-fs-proxy-like-web-application-proxy"></a>Azure AD Uygulama Ara Sunucusu, AD FS proxy (Web uygulaması ara sunucusu gibi) olarak kullanabilir miyim?

Hayır. Azure AD Uygulama Ara Sunucusu, Azure AD ile çalışacak şekilde tasarlanmıştır ve bir AD FS proxy 'si görevi gören gereksinimleri yerine getirmez.

## <a name="websocket"></a>WebSocket

### <a name="does-websocket-support-work-for-applications-other-than-qliksense"></a>WebSocket desteği QlikSense dışındaki uygulamalar için çalışır mı?

Şu anda WebSocket protokol desteği hala genel önizlemede ve diğer uygulamalar için çalışmayabilir. Bazı müşterilerin WebSocket protokolünü diğer uygulamalarla kullanarak karma başarısı vardı. Bu senaryoları test ediyorsanız, sonuçlarınızı duymak isteriz. Lütfen görüşlerinizi bize gönderin aadapfeedback@microsoft.com .

Windows Yönetim Merkezi (WAC) veya Uzak Masaüstü Web Istemcisi (HTML5) içindeki Özellikler (Eventlogs, PowerShell ve Uzak Masaüstü Hizmetleri) Şu anda Azure AD Uygulama Ara Sunucusu üzerinden çalışmaz.

## <a name="link-translation"></a>Bağlantı çevirisi

### <a name="does-using-link-translation-affect-performance"></a>Bağlantı çevirisi kullanımı performansı etkiler mi?

Evet. Bağlantı çevirisi performansı etkiler. Uygulama proxy hizmeti, uygulamayı sabit kodlanmış bağlantılar için tarar ve bunları kullanıcıya sunmadan önce ilgili, yayımlanan dış URL 'lerle değiştirir. 

En iyi performansı elde etmek için, [özel etki alanlarını](./application-proxy-configure-custom-domain.md)yapılandırarak özdeş iç ve dış URL 'ler kullanmanızı öneririz. Özel etki alanlarının kullanılması mümkün değilse, mobil uygulama veya Microsoft Edge tarayıcısı olan uygulamalarım güvenli oturum açma uzantısı ' nı kullanarak bağlantı çevirisi performansını geliştirebilirsiniz. Bkz. [Azure AD uygulama ara sunucusu ile yayımlanan uygulamalar Için yeniden kodlanmış bağlantıları yeniden yönlendirme](application-proxy-configure-hard-coded-link-translation.md).

## <a name="wildcards"></a>Joker karakterler

### <a name="how-do-i-use-wildcards-to-publish-two-applications-with-the-same-custom-domain-name-but-with-different-protocols-one-for-http-and-one-for-https"></a>Aynı özel etki alanı adına sahip, ancak biri HTTP ve HTTPS için olmak üzere farklı protokollerle iki uygulama yayımlamak için joker karakterler kullanın Nasıl yaparım?

Bu senaryo doğrudan desteklenmez. Bu senaryoya ilişkin seçenekleriniz şunlardır:

1. Hem HTTP hem de HTTPS URL 'Lerini joker karakterle ayrı uygulamalar olarak yayımlayın, ancak her birine farklı bir özel etki alanı verin. Bu yapılandırma, farklı dış URL 'ler olduğundan çalışacaktır.

2. HTTPS URL 'sini bir joker uygulama aracılığıyla yayımlayın. HTTP uygulamalarını bu uygulama proxy PowerShell cmdlet 'lerini kullanarak ayrı ayrı yayımlayın:
   - [Uygulama proxy 'Si uygulama yönetimi](/powershell/module/azuread/#application_proxy_application_management&preserve-view=true)
   - [Uygulama proxy Bağlayıcısı yönetimi](/powershell/module/azuread/#application_proxy_connector_management&preserve-view=true)
