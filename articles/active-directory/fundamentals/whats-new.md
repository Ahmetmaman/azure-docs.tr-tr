---
title: Yenilikler Azure AD için sürüm notları | Microsoft Docs
description: Bilinen sorunlar, hata düzeltmeleri, kullanım dışı bırakılan işlevsellik ve yaklaşan değişiklikleri en son sürüm notları gibi Azure Active Directory (Azure AD) ile yenilikleri öğrenin.
services: active-directory
author: eross-msft
manager: mtillman
featureFlags:
- clicktale
ms.assetid: 06a149f7-4aa1-4fb9-a8ec-ac2633b031fb
ms.service: active-directory
ms.component: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: lizross
ms.reviewer: dhanyahk
ms.openlocfilehash: e62dd48af69dd54abd26c21d7510ec872500e274
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/17/2018
ms.locfileid: "45737439"
---
# <a name="whats-new-in-azure-active-directory"></a>Azure Active Directory'deki yenilikler nelerdir?

>Bu ekleyerek güncelleştirmeler için bu sayfayı yeniden ziyaret etmeniz ne zaman hakkında bildirim almak [URL](https://docs.microsoft.com/api/search/rss?search=%22whats%20new%20in%20azure%20active%20directory%22&locale=docs.microsoft.com/) için ![RSS simgesi](./media/whats-new/feed-icon-16x16.png) okuyucu akış.

Azure AD iyileştirmeleri düzenli olarak alır. İle en son gelişmeleri güncel kalmak için bu makalede, ile hakkında bilgi sağlar:

- En son sürümleri
- Bilinen sorunlar
- Hata düzeltmeleri
- Kullanım dışı işlev
- Değişiklikleri planları

Bu sayfaya ay güncelleştirilir, böylece bunu düzenli olarak tekrar ziyaret.

---
## <a name="august-2018"></a>Ağustos 2018

### <a name="changes-to-azure-active-directory-ip-address-ranges"></a>Azure Active Directory IP adresi aralıklarını değişiklikler

**Türü:** değişiklik planı  
**Hizmet kategorisi:** diğer  
**Ürün özelliği:** platformu

Daha büyük IP aralıklarını, güvenlik duvarları, yönlendiriciler veya ağ güvenlik grupları, Azure AD IP adresi aralıklarını yapılandırdığınız anlamına güncelleştirmeniz gerekecektir Azure AD'ye kullanıma sunduğumuz. Azure AD yeni uç nokta eklediğinde, güvenlik duvarı, yönlendirici veya ağ güvenlik grupları IP aralığı yapılandırmaları yeniden değiştirmek zorunda kalmamanız için bu güncelleştirmeyi yapıyoruz. 

Ağ trafiği için bu yeni aralıklar sonraki iki ay içinde taşınıyor. Kesintisiz hizmet ile devam etmek için bu güncelleştirilmiş değerleri 10 Eylül 2018'den önce IP adreslerinizi eklemeniz gerekir:

- 20.190.128.0/18 

- 40.126.0.0/18 

Tüm ağ trafiğinizin taşındı kadar yeni aralıklar için eski IP adres aralıklarını kaldırılmıyor kesinlikle öneririz. Güncelleştirmeler taşıma hakkında ve ne zaman eski aralıkları kaldırabilirsiniz öğrenmek için bkz [Office 365 URL'leri ve IP adresi aralıkları](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2).

---

### <a name="change-notice-authorization-codes-will-no-longer-be-available-for-reuse"></a>Değiştirme dikkat edin: Yetkilendirme kodları artık yeniden kullanmak üzere kullanıma sunulacaktır 

**Türü:** değişiklik planı  
**Hizmet kategorisi:** kimlik doğrulamaları (oturum açma bilgileri)  
**Ürün özelliği:** kullanıcı kimlik doğrulaması
 
10 Ekim 2018 tarihinden itibaren Azure AD yeni uygulamalar için daha önce kullanılan kimlik doğrulama kodları kabul durdurur. 10 Ekim 2018'den önce oluşturulan herhangi bir uygulama kimlik doğrulama kodları yeniden kullanmaya devam edebilir. Bu güvenlik değişiklik v1 ve v2 Uç noktalara zorlanmasını sağlar ve Azure AD OAuth belirtimi ayarlarına uygun olarak çıkarmak yardımcı olur.

Uygulamanız için birden fazla kaynak belirteçlerini almak için yetkilendirme kodları yeniden kullanır, bir yenileme belirteci almak için kodu kullanın ve ardından diğer kaynaklar için ek belirteçlerini almak için yenileme belirtecini kullanmak öneririz. Yetkilendirme kodları yalnızca bir kez kullanılabilir, ancak yenileme belirteçleri birden fazla kaynak arasında birden çok kez kullanılabilir. Kimlik doğrulaması kodu OAuth kod akışı sırasında yeniden başlatmayı deneyen herhangi yeni bir uygulama, yinelenen kod kullanarak alındı önceki bir yenileme belirteci iptal ediliyor invalid_grant hata alırsınız.

Yenileme belirteçleri hakkında daha fazla bilgi için bkz: [erişim belirteçlerini yenileme](https://docs.microsoft.com/azure/active-directory/develop/v1-protocols-oauth-code#refreshing-the-access-tokens).
 
---

### <a name="converged-security-info-management-for-self-service-password-sspr-and-multi-factor-authentication-mfa"></a>Self Servis parola (SSPR) ve multi-Factor Authentication (MFA) için yakınsanmış güvenlik bilgileri yönetimi

**Türü:** yeni özellik  
**Hizmet kategorisi:** SSPR  
**Ürün özelliği:** kullanıcı kimlik doğrulaması
 
Bu yeni özellik SSPR ve mfa'yı tek bir konum ve deneyimi için güvenlik bilgilerini (örneğin, telefon numarası, mobil uygulama ve benzeri) yönetme kişiler yardımcı olur; kıyasla daha önce burada iki farklı konumlarda bitti dedik.

Bu yakınsanmış deneyimi de SSPR'ı veya mfa'yı kullanan kişiler için çalışır. Ayrıca, kuruluşunuz MFA veya SSPR kayıt zorunlu değil, kişiler uygulamalarım portalında kuruluşunuz tarafından izin verilen tüm MFA veya SSPR güvenlik bilgisi yöntemleri yine de kaydedebilirsiniz.

Bu bir katılım genel önizlemesidir. Yöneticilerin yeni deneyimi (istenirse) bir kiracıdaki tüm kullanıcılar veya seçili bir grup için kapatabilirsiniz. Yakınsanmış deneyimi hakkında daha fazla bilgi için bkz. [Yakınsanan experience blog'a](https://cloudblogs.microsoft.com/enterprisemobility/2018/08/06/mfa-and-sspr-updates-now-in-public-preview/)

---

### <a name="new-http-only-cookies-setting-in-azure-ad-application-proxy-apps"></a>Azure AD uygulama ara sunucusu uygulamaları ayarlama, yeni yalnızca HTTP tanımlama bilgileri

**Türü:** yeni özellik  
**Hizmet kategorisi:** uygulama ara sunucusu  
**Ürün özelliği:** erişim denetimi

Çağrılan, bir yeni ayar yoktur **azaltmaya** uygulamalarınızdaki uygulama proxy'si. Bu ayar, her iki uygulama ara sunucusu erişim ve oturum tanımlama bilgileri HTTP yanıt üst bilgisinde HTTPOnly bayrağı dahil olmak üzere, erişim tanımlama bilgisine istemci tarafı komut dosyasından durduruluyor ve daha da kopyalama gibi eylemleri engelleyen ek güvenlik sağlamaya yardımcı olur veya tanımlama bilgisi değiştiriliyor. Bu bayrak daha önce kullanılmamış olsa da, tanımlama bilgilerinizi her zaman silinmiş şifrelenir ve aktarılan yanlış değişikliklere karşı korumaya yardımcı olmak için bir SSL bağlantısı kullanma.

Bu ayarı, Uzak Masaüstü gibi ActiveX denetimlerini kullanan uygulamalar ile uyumlu değildir. Bu durumda kullanıyorsanız, bu ayar devre dışı olmasını öneririz.

Azaltmaya ayarı hakkında daha fazla bilgi için bkz. [Azure AD uygulama ara sunucusu kullanarak uygulama yayımlama](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-publish-azure-portal).

---

### <a name="privileged-identity-management-pim-for-azure-resources-supports-management-group-resource-types"></a>Azure kaynakları için Privileged Identity Management (PIM) yönetim grubu kaynak türlerini destekler.

**Türü:** yeni özellik  
**Hizmet kategorisi:** Privileged Identity Management  
**Ürün özelliği:** Privileged Identity Management
 
Just-In-Time etkinleştirme ve atama ayarları şimdi zaten abonelik, kaynak grupları ve kaynakları (örneğin, VM'ler, uygulama hizmetleri ve diğer) için yaptığınız gibi yönetim grubu kaynak türleri için de uygulanabilir. Ayrıca, bir yönetim grubu için yönetici erişimi sağlayan bir rolü olan herkes bulabilir ve bu kaynağın PIM yönetme.

PIM ve Azure kaynakları hakkında daha fazla bilgi için bkz. [bulma ve Privileged Identity Management'ı kullanarak Azure kaynaklarını yönetme](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-resource-roles-discover-resources)
 
---

### <a name="application-access-preview-provides-faster-access-to-the-azure-ad-portal"></a>Uygulama erişimi (Önizleme) Azure AD portalı hızlı erişim sağlar.

**Türü:** yeni özellik  
**Hizmet kategorisi:** Privileged Identity Management  
**Ürün özelliği:** Privileged Identity Management
 
Bugün, PIM kullanarak rol etkinleştirme yapılırken, üzerinde etkili olması izinler için 10 dakika sürebilir. Şu anda genel Önizleme aşamasında olan uygulama erişimi kullanmayı tercih ederseniz Yöneticiler etkinleştirme isteği tamamlandıktan hemen sonra Azure AD portalına erişebilir.

Şu anda uygulama erişimi yalnızca Azure kaynakları ve Azure AD portalı deneyiminin destekler. PIM ve uygulama erişimi hakkında daha fazla bilgi için bkz: [Azure AD Privileged Identity Management nedir?](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-configure)
 
---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---august-2018"></a>Yeni Federasyon uygulamaları kullanılabilir Azure AD uygulama galerisinde - Ağustos 2018

**Türü:** yeni özellik  
**Hizmet kategorisi:** kurumsal uygulamalar  
**Ürün özelliği:** 3. taraf tümleştirme
 
Ağustos 2018'de Federasyon ile 16 Bu yeni uygulamalar için uygulama Galerisi desteği ekledik:

[Hornbill](https://docs.microsoft.com/azure/active-directory/saas-apps/hornbill-tutorial), [ilişkisiz Bridgeline](https://docs.microsoft.com/azure/active-directory/saas-apps/bridgelineunbound-tutorial), [Tariflerinizden Labs - mobil ve Web testi](https://docs.microsoft.com/azure/active-directory/saas-apps/saucelabs-mobileandwebtesting-tutorial), [Meta ağları bağlayıcı](https://docs.microsoft.com/azure/active-directory/saas-apps/metanetworksconnector-tutorial), [yaptığımız şekilde](https://docs.microsoft.com/azure/active-directory/saas-apps/waywedo-tutorial), [Spotinst](https://docs.microsoft.com/azure/active-directory/saas-apps/spotinst-tutorial), [(tarafından Inlogik) ProMaster](https://docs.microsoft.com/azure/active-directory/saas-apps/promaster-tutorial), SchoolBooking, [4me](https://docs.microsoft.com/azure/active-directory/saas-apps/4me-tutorial), [Dossier](https://docs.microsoft.com/azure/active-directory/saas-apps/DOSSIER-tutorial), [N2F - gider raporları](https://docs.microsoft.com/azure/active-directory/saas-apps/n2f-expensereports-tutorial), [Comm100 canlı sohbet](https://docs.microsoft.com/azure/active-directory/saas-apps/comm100livechat-tutorial), [SafeConnect](https://docs.microsoft.com/azure/active-directory/saas-apps/safeconnect-tutorial), [ZenQMS](https://docs.microsoft.com/azure/active-directory/saas-apps/zenqms-tutorial), [eLuminate](https://docs.microsoft.com/azure/active-directory/saas-apps/eluminate-tutorial), [ Dovetale](https://docs.microsoft.com/azure/active-directory/saas-apps/dovetale-tutorial).

Uygulamalar hakkında daha fazla bilgi için bkz. [Azure Active Directory ile SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Azure AD uygulama galerisinde uygulamanızı listeleme hakkında daha fazla bilgi için bkz. [uygulamanızı Azure Active Directory Uygulama galerisinde listeleyin](https://aka.ms/azureadapprequest).

---

### <a name="native-tableau-support-is-now-available-in-azure-ad-application-proxy"></a>Azure AD uygulama proxy'si yerel Tableau desteği artık kullanılabilir

**Türü:** değiştirilen özellik  
**Hizmet kategorisi:** uygulama ara sunucusu  
**Ürün özelliği:** erişim denetimi

Bizim güncelleştirmesi ile Openıd Connect bizim ön kimlik doğrulama protokolü için OAuth 2.0 kodu verme protokolü, artık Tableau uygulaması Ara sunucusu ile kullanılacak tüm ek yapılandırma adımları gerekir. Bu protokol değişiklik uygulama proxy'si, JavaScript ve HTML etiketleri yaygın olarak desteklenen HTTP yönlendirmeleri kullanarak daha modern uygulamalar daha iyi destek de yardımcı olur.

Tableau için yerel destek hakkında daha fazla bilgi için bkz: [artık yerel Tableau desteği ile Azure AD uygulama proxy'si](https://blogs.technet.microsoft.com/applicationproxyblog/2018/08/14/azure-ad-application-proxy-now-with-native-tableau-support).

---

### <a name="new-support-to-add-google-as-an-identity-provider-for-b2b-guest-users-in-azure-active-directory-preview"></a>Google (Önizleme) Azure Active Directory B2B Konuk kullanıcılar için kimlik sağlayıcısı eklemek için yeni destek

**Türü:** yeni özellik  
**Hizmet kategorisi:** B2B  
**Ürün özelliği:** B2B/B2C

Google ile bir Federasyon kuruluşunuzdaki ayarlayarak, paylaşılan uygulamaları ve kişisel bir Microsoft Account (MSA) veya bir Azure AD hesabı oluşturmak zorunda kalmadan, var olan Google hesaplarını kullanarak kaynakları için oturum açın, davet edilen Gmail kullanıcılar izin verebilirsiniz.

Bu bir katılım genel önizlemesidir. Google Federasyon hakkında daha fazla bilgi için bkz: [B2B Konuk kullanıcılar için kimlik sağlayıcısı ekle Google](https://docs.microsoft.com/azure/active-directory/b2b/google-federation).

---

## <a name="july-2018"></a>Temmuz 2018

### <a name="improvements-to-azure-active-directory-email-notifications"></a>Azure Active Directory e-posta bildirimleri iyileştirmeleri

**Türü:** değiştirilen özellik  
**Hizmet kategorisi:** diğer  
**Ürün özelliği:** kimlik yaşam döngüsü yönetimi
 
Azure Active Directory (Azure AD) e-postaları artık gönderen e-posta adresi ve gönderen görünen adı, değişiklikleri yanı sıra, güncelleştirilmiş bir tasarım aşağıdaki hizmetlerinden gönderilen zaman özellik:
 
- Azure AD erişim gözden geçirmeleri
- Azure AD Connect Health 
- Azure AD Kimlik Koruması 
- Azure AD Privileged Identity Management
- Kurumsal uygulama süresi dolan sertifika bildirimleri
- Kurumsal uygulama sağlama hizmet bildirimleri
 
E-posta bildirimleri şu e-posta adresinden gönderilir ve görünen ad:

- E-posta adresi: azure-noreply@microsoft.com
- Görünen ad: Microsoft Azure
 
Bir örnek, bazı yeni e-posta tasarımları ve daha fazla bilgi için bkz [e-posta bildirimleri Azure AD PIM](https://go.microsoft.com/fwlink/?linkid=2005832).

---

### <a name="azure-ad-activity-logs-are-now-available-through-azure-monitor"></a>Azure AD Etkinlik Günlükleri artık Azure İzleyici'de kullanılabilir

**Türü:** yeni özellik  
**Hizmet kategorisi:** raporlama  
**Ürün özelliği:** izleme ve Raporlama

Azure AD etkinlik günlüklerini Azure İzleyicisi'ni (Azure'nın platform genelinde izleme hizmeti) için genel önizlemede kullanıma sunulmuştur. Azure İzleyici, uzun süreli saklama ve bu geliştirmeler yanı sıra sorunsuz tümleştirme sunar:

- Günlük dosyalarınızın kendi Azure depolama hesabına yönlendirme ile uzun süreli saklama için.

- Yazma veya özel komut dosyaları korumak gerektirmeden sorunsuz SIEM tümleştirmesi.

- Kendi özel çözümler, analiz araçları ve Olay yönetimi çözümleri ile sorunsuz tümleştirme sağlar.

Blogumuzu bu yeni özellikler hakkında daha fazla bilgi için bkz. [Azure AD etkinlik günlükleri Tanılama, artık genel önizlemede Azure İzleyicisi'nde](https://cloudblogs.microsoft.com/enterprisemobility/2018/07/26/azure-ad-activity-logs-in-azure-monitor-diagnostics-now-in-public-preview/) ve Belgelerimizi, [Azure'da Azure Active Directory etkinlik günlükleri İzleyici (Önizleme)](https://docs.microsoft.com/azure/active-directory/reporting-azure-monitor-diagnostics-overview).

---

### <a name="conditional-access-information-added-to-the-azure-ad-sign-ins-report"></a>Koşullu erişim bilgileri Azure AD oturum açma raporuna eklendi

**Türü:** yeni özellik  
**Hizmet kategorisi:** raporlama  
**Ürün özelliği:** kimlik güvenliği ve koruması
 
Bu güncelleştirme, hangi ilkelerin ilke sonucu ile birlikte bir kullanıcı oturum açtığı zaman değerlendirilen görmenize olanak tanır. Ayrıca, rapor artık eski protokol trafiğini belirleyebilmeniz kullanıcı tarafından kullanılan istemci uygulaması türünü içerir. Rapor girişleri kullanıcıya yönelik hata iletisinde bulunabilir ve belirleme ve giderme eşleşen oturum açma isteği için kullanılan bir bağıntı kimliği için artık aranabilir.

---

### <a name="view-legacy-authentications-through-sign-ins-activity-logs"></a>Oturum açma etkinlik günlükleri ile eski kimlik doğrulamalarını görüntüleyin

**Türü:** yeni özellik  
**Hizmet kategorisi:** raporlama  
**Ürün özelliği:** izleme ve Raporlama
 
Sunulmasıyla birlikte **istemci uygulaması** alan oturum açma etkinlik günlükleri, eski kimlik doğrulama kullanarak müşterilerin can artık bkz: kullanıcılar. Müşteriler, oturum açma MS Graph API'sini kullanarak bu bilgilere erişmek mümkün olacaktır veya oturum açma kullanabileceğiniz Azure AD'ye portalda etkinlik günlükleri **istemci uygulaması** üzerinde eski kimlik doğrulamaları filtrelemek için denetimi. Daha fazla ayrıntı için belgeleri gözden geçirin.

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---july-2018"></a>Azure AD uygulama galerisinde yeni Federasyon Uygulamaları kullanılabilir - Temmuz 2018

**Türü:** yeni özellik  
**Hizmet kategorisi:** kurumsal uygulamalar  
**Ürün özelliği:** 3. taraf tümleştirme
 
Temmuz 2018'de Federasyon ile 16 Bu yeni uygulamalar için uygulama Galerisi desteği ekledik:

[Yenilik Hub](https://docs.microsoft.com/azure/active-directory/saas-apps/innovationhub-tutorial), [Leapsome](https://docs.microsoft.com/azure/active-directory/saas-apps/leapsome-tutorial), [belirli yönetici SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/certainadminsso-tutorial), PSUC hazırlama [iPass SmartConnect](https://docs.microsoft.com/azure/active-directory/saas-apps/ipasssmartconnect-tutorial), [yayını-O-Matic](https://docs.microsoft.com/azure/active-directory/saas-apps/screencast-tutorial) , PowerSchool sınıf, birleşik [Eli ekleme](https://docs.microsoft.com/azure/active-directory/saas-apps/elionboarding-tutorial), [Bomgar Uzaktan Destek](https://docs.microsoft.com/azure/active-directory/saas-apps/bomgarremotesupport-tutorial), [Nimblex](https://docs.microsoft.com/azure/active-directory/saas-apps/nimblex-tutorial), [Imagineer WebVision](https://docs.microsoft.com/azure/active-directory/saas-apps/imagineerwebvision-tutorial) , [Insight4GRC](https://docs.microsoft.com/azure/active-directory/saas-apps/insight4grc-tutorial), [SecureW2 JoinNow bağlayıcı](https://docs.microsoft.com/azure/active-directory/saas-apps/securejoinnow-tutorial), [Kanbanize](https://review.docs.microsoft.com/azure/active-directory/saas-apps/kanbanize-tutorial), [SmartLPA](https://review.docs.microsoft.com/azure/active-directory/saas-apps/smartlpa-tutorial), [temel becerileri](https://docs.microsoft.com/azure/active-directory/saas-apps/skillsbase-tutorial)

Uygulamalar hakkında daha fazla bilgi için bkz. [Azure Active Directory ile SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Azure AD uygulama galerisinde uygulamanızı listeleme hakkında daha fazla bilgi için bkz. [uygulamanızı Azure Active Directory Uygulama galerisinde listeleyin](https://aka.ms/azureadapprequest).

---
 
### <a name="new-user-provisioning-saas-app-integrations---july-2018"></a>Yeni kullanıcı sağlama SaaS uygulama tümleştirmeleri - Temmuz 2018

**Türü:** yeni özellik  
**Hizmet kategorisi:** uygulama sağlama  
**Ürün özelliği:** 3. taraf tümleştirme
 
Azure AD, oluşturma, Bakım ve Dropbox, Salesforce, ServiceNow ve diğer SaaS uygulamalarına kullanıcı kimliklerini kaldırılmasını otomatik hale getirmenizi sağlar. Kullanıcı Azure AD uygulama galerisinde şu uygulamalar için destek sağlama Temmuz 2018 tarihinden itibaren ekledik:

- [Cisco Spark](https://docs.microsoft.com/azure/active-directory/saas-apps/cisco-spark-provisioning-tutorial)

- [Cisco WebEx](https://docs.microsoft.com/azure/active-directory/saas-apps/cisco-webex-provisioning-tutorial)

- [Bonusly](https://docs.microsoft.com/azure/active-directory/saas-apps/bonusly-provisioning-tutorial)

Azure AD Galerisi'nde kullanıcı sağlamayı destekleyen tüm uygulamalar listesi için bkz. [Azure Active Directory ile SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial).

---

### <a name="connect-health-for-sync---an-easier-way-to-fix-orphaned-and-duplicate-attribute-sync-errors"></a>Eşitleme için Connect Health - Yalnız bırakılmış ve çoğaltışmış öznitelik eşitleme hatalarını kolaylıkla düzeltme

**Türü:** yeni özellik  
**Hizmet kategorisi:** AD Bağlan  
**Ürün özelliği:** izleme ve Raporlama
 
Azure AD Connect Health kendi kendine düzeltme vurgulayın ve eşitleme hatalarını düzeltmeyi yardımcı olması için ortaya çıkarır. Bu özellik, yinelenen öznitelik Eşitleme hataları giderir ve yalnız bırakılmış nesneler, Azure AD'den giderir. Bu tanılama aşağıdaki faydaları sağlar:

- Belirli düzeltmeler sağlama yinelenen öznitelik Eşitleme hataları daraltır

- Azure AD senaryoları, tek bir adımda hatalarını çözümlemek için bir düzeltme uygular

- Yükseltme ya da yapılandırma'yı açmak ve bu özelliği kullanmak için gereklidir

Daha fazla bilgi için [Tanıla ve yinelenen öznitelik eşitleme hatalarını Düzelt](https://docs.microsoft.com/azure/active-directory/connect-health/active-directory-aadconnect-health-diagnose-sync-errors)

---

### <a name="visual-updates-to-the-azure-ad-and-msa-sign-in-experiences"></a>Azure AD ve MSA oturum açma deneyimleri için görsel güncelleştirmeler

**Türü:** değiştirilen özellik  
**Hizmet kategorisi:** Azure AD  
**Ürün özelliği:** kullanıcı kimlik doğrulaması

Microsoft online services oturum açma deneyimi için kullanıcı Arabirimi gibi Office 365 ve Azure için güncelleştirdik. Bu değişiklik ekranları, daha az anlaşılamayacak ve daha basit hale getirir. Bu değişiklik hakkında daha fazla bilgi için bkz. [yaklaşan geliştirmeler Azure AD oturum açma deneyimini](https://cloudblogs.microsoft.com/enterprisemobility/2018/04/04/upcoming-improvements-to-the-azure-ad-sign-in-experience/) blogu.

---

### <a name="new-release-of-azure-ad-connect---july-2018"></a>Yeni Azure AD Connect sürümü - Temmuz 2018

**Türü:** değiştirilen özellik  
**Hizmet kategorisi:** uygulama sağlama  
**Ürün özelliği:** kimlik yaşam döngüsü yönetimi

Azure AD Connect'in en son sürümünü içerir: 

- Hata düzeltmeleri ve desteklenebilirliği güncelleştirmeleri 

- Genel kullanılabilirlik Ping federasyona tümleştirme

- En son SQL 2012 istemci güncelleştirmeleri 

Bu güncelleştirme hakkında daha fazla bilgi için bkz. [Azure AD Connect: sürüm yayımlama geçmişi](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-version-history)

---

### <a name="updates-to-the-terms-of-use-tou-end-user-ui"></a>Kullanım Koşulları (ToU) son kullanıcı arabirimi güncelleştirmeleri

**Türü:** değiştirilen özellik  
**Hizmet kategorisi:** kullanım koşulları  
**Ürün özelliği:** idare

Kabul dize TOU son kullanıcı arabiriminde güncelleştiriyoruz.

**Geçerli metin.** [Kiracıadı] kaynaklara erişmek için kullanım koşullarını kabul etmeniz gerekir.<br>**Yeni metin.** [Kiracıadı] kaynağa erişebilmeniz için kullanım koşullarını okuyun gerekir.

**Geçerli metin:** tüm yukarıdaki kullanım koşullarını kabul etmiş olursunuz kabul etmek seçme.<br>**Yeni metin:** Lütfen okudum ve anladım kullanım koşullarını onaylamak üzere kabul Et'E tıklayın.

---
 
### <a name="pass-through-authentication-supports-legacy-protocols-and-applications"></a>Doğrudan kimlik doğrulama eski protokolleri ve uygulamaları destekliyor

**Türü:** değiştirilen özellik  
**Hizmet kategorisi:** kimlik doğrulamaları (oturum açma bilgileri)  
**Ürün özelliği:** kullanıcı kimlik doğrulaması
 
Geçişli kimlik doğrulaması artık eski protokolleri ve uygulamaları destekler. Aşağıdaki sınırlamalar artık tam olarak desteklenir:

- Modern kimlik doğrulaması gerektirmeden eski Office istemci uygulamaları, Office 2010 ve Office 2013, kullanıcı oturum açma işlemleri.

- Takvim Paylaşımı ve ücretsiz/meşgul bilgileri yalnızca karma ortamlarda Office 2010 Exchange erişim.

- Kullanıcı oturum açma işlemleri Skype için modern kimlik doğrulaması gerektirmeden iş istemci uygulamaları için.

- PowerShell sürüm 1.0 için kullanıcı oturum açma işlemleri.

- Apple cihaz kaydı iOS Kurulum Yardımcısı'nı kullanarak programı (Apple DEP). 

---
 
### <a name="converged-security-info-management-for-self-service-password-reset-and-multi-factor-authentication"></a>Self servis parola sıfırlama ve Multi-Factor Authentication için yakınsanmış güvenlik bilgileri yönetimi

**Türü:** yeni özellik  
**Hizmet kategorisi:** SSPR  
**Ürün özelliği:** kullanıcı kimlik doğrulaması

Bu yeni özellik için güvenlik bilgilerini (örneğin, telefon numaranız, e-posta adresi, mobil uygulama ve benzeri) Self Servis parola sıfırlama (SSPR) ve multi-Factor Authentication (MFA) tek bir deneyimle yönetme olanağı sunar. Kullanıcıların SSPR ve MFA için iki farklı deneyimlerinde aynı güvenlik bilgilerini kaydetmek artık gerekir. Bu yeni deneyim, SSPR'ı veya MFA sahip kullanıcılar için de geçerlidir.

Bir kuruluş, MFA veya SSPR kayıt zorunlu değil, kullanıcılar aracılığıyla güvenlik bilgileri kaydedebilirsiniz **uygulamalarım** portalı. Burada, kullanıcılar için mfa'yı veya SSPR etkin herhangi bir yöntem kaydedebilirsiniz. 

Bu bir katılım genel önizlemesidir. Yöneticilerin yeni deneyimi (istenirse) seçili bir grup kullanıcı ya da bir kiracıdaki tüm kullanıcılar için etkinleştirebilirsiniz.

---
 
### <a name="use-the-microsoft-authenticator-app-to-verify-your-identity-when-you-reset-your-password"></a>Parolanızı sıfırlarken kimliğinizi doğrulamak için Microsoft Authenticator uygulamasını kullanın

**Türü:** değiştirilen özellik  
**Hizmet kategorisi:** SSPR  
**Ürün özelliği:** kullanıcı kimlik doğrulaması

Bu özellik, bir bildirim veya koddan Microsoft Authenticator'ı (ya da herhangi bir kimlik doğrulayıcı uygulama) kullanarak bir parolayı sıfırlarken kimliklerini doğrulama yönetici olmayanlar olanak sağlar. Yöneticiler bu Self Servis etkinleştirdikten sonra yöntemi, bir mobil uygulama aracılığıyla aka.ms/mfasetup kaydolan kullanıcılar parola sıfırlama veya aka.ms/setupsecurityinfo hazırladıkları mobil uygulama doğrulama yöntemi olarak, parola sıfırlama sırasında kullanabilirsiniz.

Mobil uygulama bildirimi, parolanızı sıfırlamak için iki yöntem gerektiren bir ilke bir parçası olarak yalnızca açılabilir.

---

## <a name="june-2018"></a>Haziran 2018

### <a name="change-notice-security-fix-to-the-delegated-authorization-flow-for-apps-using-azure-ad-activity-logs-api"></a>Değişiklik bildirimi: Azure AD Etkinlik Günlükleri API'sini kullanan uygulamaların yetkilendirme akışı temsilcisi için güvenlik düzeltmesi

**Türü:** değişiklik planı  
**Hizmet kategorisi:** raporlama  
**Ürün özelliği:** izleme ve Raporlama

Bizim daha güçlü güvenlik zorlama nedeniyle, şu izinleri erişmek için Temsilcili yetkilendirme akışı kullanan uygulamalar için değişiklik vardı [Azure AD etkinlik günlüklerini API'lerini](https://aka.ms/aadreportsapi). Bu değişiklik tarafından ortaya çıkar **26 Haziran 2018'e**.

Tüm uygulamalarınızın Azure AD etkinlik günlüğü API'lerini kullanırsanız, değişiklik yapıldıktan sonra app kesmeyecektir emin olmak için aşağıdaki adımları izleyin.

**Uygulama izinleri güncelleştirmek için**

1. Azure portalında oturum, select **Azure Active Directory**ve ardından **uygulama kayıtları**.
2. Azure AD etkinlik günlüklerini API'si, select kullanan uygulamanızı seçin **ayarları**seçin **gerekli izinler**ve ardından **Windows Azure Active Directory** API.
3. İçinde **temsilci izinleri** alanının **erişimi etkinleştirme** dikey penceresinde yanındaki kutuyu işaretleyin **okuma directory** veri tıklayın ve ardından **Kaydet**.
4. Seçin **izinleri verin**ve ardından **Evet**.
    
    >[!Note]
    >Uygulama izinlerini vermek için genel yönetici olması gerekir.

Daha fazla bilgi için [izinleri verin](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-prerequisites-azure-portal#grant-permissions) alan Azure AD raporlama API'si makaleye erişmek için Önkoşullar.

---

### <a name="configure-tls-settings-to-connect-to-azure-ad-services-for-pci-dss-compliance"></a>PCI DSS uyumluluğu için Azure AD hizmetlerine bağlanmak amacıyla TLS ayarlarını yapılandırma

**Türü:** yeni özellik  
**Hizmet kategorisi:** yok  
**Ürün özelliği:** platformu

Aktarım Katmanı Güvenliği (TLS) arasında iki iletişim kuran uygulamaları gizlilik ve veri bütünlüğü sağlar ve Günümüzde kullanılan en yaygın olarak dağıtılan güvenlik protokolü bir protokoldür.

[PCI güvenlik standartları Council](https://www.pcisecuritystandards.org/) TLS Güvenli Yuva Katmanı (SSL) ve önceki sürümlerinde uyumluluk başlangıç ile yeni ve daha güvenli uygulama protokolleri etkinleştirme ile değiştiriliyor devre dışı bırakılmalıdır olduğunu belirledi **30 Haziran 2018**. Bu değişiklik, Azure AD hizmetlerine bağlanmak ve PCI DSS uyumluluğu gerektir, TLS 1.0 mamtelemetrydisabled anlamına gelir. TLS birden çok sürümü kullanılabilir ancak TLS 1.2 en son sürümü Azure Active Directory Hizmetleri için kullanılabilir. Hem istemci/sunucu hem de tarayıcı/sunucu birleşimleri için doğrudan TLS 1.2 taşınması önerilir.

Güncel olmayan tarayıcılar, TLS 1.2 gibi yeni TLS sürümlerini desteklemiyor olabilir. TLS hangi sürümlerinin tarayıcınız tarafından desteklendiğini görmek için Git [Qualys SSL Labs](https://www.ssllabs.com/) tıklayın ve site **tarayıcınızı Test**. Öneririz web tarayıcınızın en son sürümüne yükseltin ve tercihen yalnızca TLS 1.2 etkinleştirin.

**TLS 1.2, tarayıcı tarafından etkinleştirmek için**

- **Microsoft Edge ve Internet Explorer'ın (hem de Internet Explorer'ı kullanarak ayarlanır)**

    1. Internet Explorer'ı açın, **Araçları** > **Internet Seçenekleri** > **Gelişmiş**.
    2. İçinde **güvenlik** alanında **TLS 1.2 kullanmak**ve ardından **Tamam**.
    3. Tüm tarayıcı pencerelerini kapatın ve Internet Explorer'ı yeniden başlatın. 

- **Google Chrome**

    1. Açık Google Chrome, türü *chrome://settings/* tuşuna basın ve adres çubuğuna **Enter**.
    2. Genişletin **Gelişmiş** seçenekleri, Git **sistem** alan ve seçim **proxy ayarları**.
    3. İçinde **Internet Özellikleri** kutusunda **Gelişmiş** sekmesine gidin **güvenlik** alanında **TLS 1.2 kullanmak**ve ardından seçin **Tamam**.
    4. Tüm tarayıcı pencerelerini kapatın ve Google Chrome'u yeniden başlatın.

- **Mozilla Firefox**

    1. Açık Firefox, türü *hakkında: yapılandırma* ENTER tuşuna basın ve adres çubuğuna **Enter**.
    2. Arama terimi için *TLS*ve ardından **security.tls.version.max** girişi.
    3. Değerine **3** TLS 1.2 sürümü için kullanır ve ardından için tarayıcıyı zorlamak için **Tamam**.

        >[!NOTE]
        >Firefox sürümü 60,0 TLS 1.3 destekler böylece de security.tls.version.max değeri ayarlayabilirsiniz **4**.

    4. Tüm tarayıcı pencerelerini kapatın ve Mozilla Firefox yeniden başlatın.

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---june-2018"></a>Azure AD uygulama galerisinde yeni Federasyon Uygulamaları kullanılabilir - Haziran 2018

**Türü:** yeni özellik  
**Hizmet kategorisi:** kurumsal uygulamalar  
**Ürün özelliği:** 3. taraf tümleştirme
 
Haziran 2018'de Federasyon ile 15 bu yeni uygulamalar için uygulama Galerisi desteği ekledik:

[Skytap](https://docs.microsoft.com/azure/active-directory/active-directory-saas-skytap-tutorial), [müzik sonlandırma](https://docs.microsoft.com/azure/active-directory/active-directory-saas-settlingmusic-tutorial), [SAML 1.1 belirteci etkin LOB uygulaması](https://docs.microsoft.com/azure/active-directory/active-directory-saas-saml-tutorial), [Supermood](https://docs.microsoft.com/azure/active-directory/active-directory-saas-supermood-tutorial), [Autotask](https://docs.microsoft.com/azure/active-directory/active-directory-saas-autotaskendpointbackup-tutorial), [ Uç nokta yedekleme](https://docs.microsoft.com/azure/active-directory/active-directory-saas-autotaskendpointbackup-tutorial), [Skyhigh ağları](https://docs.microsoft.com/azure/active-directory/active-directory-saas-skyhighnetworks-tutorial), Smartway2, [TonicDM](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tonicdm-tutorial), [Moconavi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-moconavi-tutorial), [Zoho bir](https://docs.microsoft.com/azure/active-directory/active-directory-saas-zohoone-tutorial), [ SharePoint şirket içi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-sharepoint-on-premises-tutorial), [CX Suite Öngörüyor](https://docs.microsoft.com/azure/active-directory/active-directory-saas-foreseecxsuite-tutorial), [Vidyard](https://docs.microsoft.com/azure/active-directory/active-directory-saas-vidyard-tutorial), [ChronicX](https://docs.microsoft.com/azure/active-directory/active-directory-saas-chronicx-tutorial)

Uygulamalar hakkında daha fazla bilgi için bkz. [Azure Active Directory ile SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Azure AD uygulama galerisinde uygulamanızı listeleme hakkında daha fazla bilgi için bkz. [uygulamanızı Azure Active Directory Uygulama galerisinde listeleyin](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing). 

---

### <a name="azure-ad-password-protection-is-available-in-public-preview"></a>Azure AD Parola Koruması ortak önizlemede kullanılabilir

**Türü:** yeni özellik  
**Hizmet kategorisi:** kimlik koruması  
**Ürün özelliği:** kullanıcı kimlik doğrulaması

Ortamınızdaki parolalar kolayca tahmin gidermeye yardımcı olması için Azure AD parola koruması kullanın. Bu parolalar ortadan güvenliğinin aşılması riskine karşı bir saldırı parola ilaç türü riskini azaltmak için yardımcı olur.

Özellikle, Azure AD parola koruması, yardımcı olur:

- Hem Azure AD'de kuruluşunuzun hesapları korumak ve Windows Server Active Directory (AD). 
- Kullanıcılarınızın parolaları listesi en sık kullanılan parolaların 500'den fazla ve bu parolaları 1 milyon karakter değiştirme çözümlenmeyebileceği kullanarak durdurur.
- Her iki Azure AD için Azure AD parola koruması, Azure AD portalında tek bir konumdan yönetmek ve şirket içi Windows Server AD.

Azure AD parola koruması hakkında daha fazla bilgi için bkz. [yanlış parolalar, kuruluşunuzdaki ortadan](https://aka.ms/aadpasswordprotectiondocs).

---

### <a name="new-all-guests-conditional-access-policy-template-created-during-terms-of-use-tou-creation"></a>Kullanım Koşulları (ToU) oluşturulurken elde edilen yeni "tüm konuklar" koşullu erişim ilkesi şablonu

**Türü:** yeni özellik  
**Hizmet kategorisi:** kullanım koşulları  
**Ürün özelliği:** idare

Kullanım koşulları (ToU) oluşturma sırasında "tüm konuklar" ve "tüm uygulamalar" için yeni koşullu erişim ilkesi şablonu da oluşturulur. Bu yeni bir ilke şablonu oluşturma ve zorlama işlemi konuklar için hızlandırma, yeni oluşturulan ToU geçerlidir.

Daha fazla bilgi için [kullanım özelliği, Azure Active Directory koşulları](https://docs.microsoft.com/azure/active-directory/active-directory-tou).

---

### <a name="new-custom-conditional-access-policy-template-created-during-terms-of-use-tou-creation"></a>Kullanım Koşulları (ToU) oluşturulurken elde edilen yeni "özel" koşullu erişim ilkesi şablonu

**Türü:** yeni özellik  
**Hizmet kategorisi:** kullanım koşulları  
**Ürün özelliği:** idare

Kullanım koşulları (ToU) oluşturma sırasında ayrıca yeni bir "özel" koşullu erişim ilkesi şablonu oluşturulur. Bu yeni bir ilke şablonu ToU oluşturmanızı ve el ile portal üzerinden gitmek zorunda kalmadan koşullu erişim ilkesi oluşturma dikey penceresine hemen Git sağlar.

Daha fazla bilgi için [kullanım özelliği, Azure Active Directory koşulları](https://docs.microsoft.com/azure/active-directory/active-directory-tou).

---

### <a name="new-and-comprehensive-guidance-about-deploying-azure-multi-factor-authentication"></a>Azure Multi-Factor Authentication'ı dağıtma hakkında yeni ve kapsamlı kılavuz

**Türü:** yeni özellik  
**Hizmet kategorisi:** diğer  
**Ürün özelliği:** kimlik güvenliği ve koruması
 
Yeni bir Azure multi-Factor Authentication (MFA) kuruluşunuzda dağıtma hakkında adım adım kılavuz yayımladık.

MFA Dağıtım Kılavuzu'nu görüntülemek için Git [kimlik dağıtım kılavuzları](https://aka.ms/DeploymentPlans) github deposu. Dağıtım kılavuzları hakkında geri bildirim sağlamak için kullanın [dağıtım planı geri bildirim formu](https://aka.ms/deploymentplanfeedback). Dağıtım kılavuzları hakkında sorularınız varsa, adresinden bize başvurun [IDGitDeploy](mailto:idgitdeploy@microsoft.com).

---

### <a name="azure-ad-delegated-app-management-roles-are-in-public-preview"></a>Azure AD uygulama yönetimi temsilcisi rolleri ortak önizlemede

**Türü:** yeni özellik  
**Hizmet kategorisi:** kurumsal uygulamalar  
**Ürün özelliği:** erişim denetimi

Yöneticiler, artık genel yönetici rolü atama olmadan uygulama yönetim görevlerini devredebilirsiniz. Yeni rol ve özellikleri şunlardır:

- **Yeni bir standart Azure AD yönetim rolleri:**

    - **Uygulama Yöneticisi.** Kayıt, SSO ayarlarını, uygulama atamalarını ve lisanslama, uygulama proxy'si ayarları ve onay dahil tüm uygulamalar'ın tüm yönlerini yönetme olanağı veren (Azure AD kaynaklarını hariç).

    - **Bulut uygulaması Yöneticisi.** Şirket içi erişim sağlamaz çünkü uygulama Yöneticisi becerileri, uygulama ara sunucusu hariç tüm verir.

    - **Uygulama geliştiricisi.** Uygulama kayıtları oluşturma yeteneği verir bile **uygulamaları kaydetmesine izin verin** seçeneği devre dışı bırakılmış durumdadır.

- **Sahiplik (uygulama başına kayıt ve kurumsal uygulama, Grup sahipliği işleme benzer ayarlayın:**
 
    - **Uygulama kaydı sahibi.** Sahip olunan bir uygulama kaydı, ek sahipleri ekleme ve uygulama bildirimi dahil olmak üzere tüm yönlerini yönetmek için yeteneği verir.

    - **Kurumsal uygulama sahibi.** Sahip olunan Kurumsal uygulamaları, SSO ayarlarını, uygulama atamalarını ve onay dahil olmak üzere birçok yönden yönetme olanağı veren (Azure AD kaynaklarını hariç).

Genel önizleme hakkında daha fazla bilgi için bkz: [Azure AD rolleri, genel Önizleme aşamasında olan uygulama yönetimi temsilcisi!](https://cloudblogs.microsoft.com/enterprisemobility/2018/06/13/hallelujah-azure-ad-delegated-application-management-roles-are-in-public-preview/) blog. Rolleri ve izinleri hakkında daha fazla bilgi için bkz: [Azure Active Directory'de yönetici rolleri atama](https://docs.microsoft.com/azure/active-directory/active-directory-assign-admin-roles-azure-portal).

---

## <a name="may-2018"></a>Mayıs 2018

### <a name="expressroute-support-changes"></a>ExpressRoute desteği değişiklikleri

**Türü:** değişiklik planı  
**Hizmet kategorisi:** kimlik doğrulamaları (oturum açma bilgileri)  
**Ürün özelliği:** platformu  

Doğrudan Internet gidip ExpressRoute veya diğer özel VPN tünelleri gerek kalmadan iyi çalışacak şekilde tasarlanan Azure Active Directory (Azure AD) gibi bir hizmet olarak yazılım. Bu nedenle, üzerinde **1 Ağustos 2018**, size Azure AD Hizmetleri kullanarak Azure ortak eşleme ve Microsoft eşlemesi içinde Azure topluluklar için ExpressRoute destekleme durdurur. Bu değişiklikten etkilenen tüm hizmetler Azure fark edebilirsiniz AD trafik ExpressRoute Internet'e giderek kaydırma.

Ayrıca var. biliyoruz ki, destek değiştirmiyorsanız olan gerek duyduğunuz senaryolara hala durumlarda kullanırken devreler adanmış bir dizi, kimlik doğrulama trafiği için. Bu nedenle, Azure AD Kiracı başına ExpressRoute ve Hizmetleri zaten "Diğer Office 365 Çevrimiçi Hizmetleri" toplulukla Microsoft eşlemesi üzerinde kullanarak IP aralığı kısıtlamalarını desteklemeye devam edecektir. Hizmetlerinizi etkilenir, ancak ExpressRoute gerektirir, aşağıdakileri yapmanız gerekir:

- **Azure ortak eşleme üzerinde kullanıyorsanız.** Microsoft eşlemesi için taşıma ve kaydolun **diğer Office 365 Çevrimiçi Hizmetleri (12076:5100)** topluluğu. Azure genel Microsoft eşlemesi için eşlemeden taşıma hakkında daha fazla bilgi için bkz. [Microsoft eşlemesi için genel eşleme taşıma](https://docs.microsoft.com/azure/expressroute/how-to-move-peering) makalesi.

- **Microsoft eşlemesi üzerinde kullanıyorsanız.** Kaydolun **diğer Office 365 çevrimiçi hizmet (12076:5100)** topluluğu. Yönlendirme gereksinimleri hakkında daha fazla bilgi için bkz. [BGP toplulukları bölümü için destek](https://docs.microsoft.com/azure/expressroute/expressroute-routing#bgp) ExpressRoute yönlendirme gereksinimleri makalesi.

Adanmış devreler kullanmaya devam etmeniz gerekiyorsa Microsoft Account ekibinize kullanılacak yetkilendirme edinme hakkında konuşmak gerekecektir **diğer Office 365 çevrimiçi hizmet (12076:5100)** topluluğu. MS Office yönetilen gözden geçirme panonun bu devreler gerekir ve başvurularınızı teknik sonuçlarını anladığınızdan emin olup olmadığını doğrular. Yetkisiz abonelikleri Office 365 için rota filtreleri oluşturulmaya çalışılırken bir hata iletisi alırsınız. 
 
---

### <a name="microsoft-graph-apis-for-administrative-scenarios-for-tou"></a>TOU yönetim senaryoları için Microsoft Graph API'leri

**Türü:** yeni özellik  
**Hizmet kategorisi:** kullanım koşulları  
**Ürün özelliği:** geliştirici deneyimi
 
Azure AD kullanım koşulları için yönetim işlemi Microsoft Graph API'leri ekledik. Oluştur, Güncelleştir, kullanım koşulları nesneyi silmek kullanabilirsiniz.

---

### <a name="add-azure-ad-multi-tenant-endpoint-as-an-identity-provider-in-azure-ad-b2c"></a>Azure AD çok kiracılı uç noktayı Azure AD B2C'de bir kimlik sağlayıcısı olarak ekle

**Türü:** yeni özellik  
**Hizmet kategorisi:** B2C - tüketici Kimlik Yönetimi  
**Ürün özelliği:** B2B/B2C
 
Özel ilkeleri kullanarak, Azure AD genel uç noktası kimlik sağlayıcısı olarak Azure AD B2C artık ekleyebilirsiniz. Bu, tek bir uygulamalarınızda oturum açtığınız tüm Azure AD kullanıcıları için giriş noktası olmasını sağlar. Daha fazla bilgi için [Azure Active Directory B2C: özel ilkeleri kullanarak çok kiracılı Azure AD kimlik sağlayıcısı için oturum açmasına imkan tanıyın](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-commonaad-custom).

---

### <a name="use-internal-urls-to-access-apps-from-anywhere-with-our-my-apps-sign-in-extension-and-the-azure-ad-application-proxy"></a>Uygulamalarım Oturum Açma Uzantısı ve Azure AD Uygulama Proxy'si ile İç URL'ler kullanarak uygulamalarınıza herhangi bir yerden erişin

**Türü:** yeni özellik  
**Hizmet kategorisi:** uygulamalarım  
**Ürün özelliği:** SSO
 
Kullanıcılar artık uygulamalar üzerinden şirket ağınıza olduğunda bile dışında İç URL My Apps güvenli oturum açma uzantısı için Azure AD'yi kullanarak erişebilirsiniz. Bu, Azure AD uygulama ara sunucusu, ayrıca erişim paneli tarayıcı uzantısının yüklü olan herhangi bir tarayıcı üzerinde kullanarak yayımladığınız herhangi bir uygulama ile çalışır. URL yeniden yönlendirme işlevi, uzantısı'na bir kullanıcı oturumu sonra otomatik olarak etkinleştirilir. Uzantı, ücretsiz olarak kullanılabilir [Edge](https://go.microsoft.com/fwlink/?linkid=845176), [Chrome](https://go.microsoft.com/fwlink/?linkid=866367), ve [Firefox](https://go.microsoft.com/fwlink/?linkid=866366).

---
 
### <a name="azure-active-directory---data-in-europe-for-europe-customers"></a>Azure Active Directory - Avrupa müşterileri için Avrupa'daki veriler

**Türü:** yeni özellik  
**Hizmet kategorisi:** diğer  
**Ürün özelliği:** GoLocal

Avrupa'da bulunan müşteriler verilerine Avrupa'da kalmasını gerektirir ve toplantı gizliliği ve Avrupa yasaları Avrupa veri merkezleri dışına çoğaltılmamış. Bu [makale](https://go.microsoft.com/fwlink/?linkid=872328) hangi kimlik bilgilerini belirli ayrıntıları Avrupa içinde depolanır ve ayrıca depolanan dış Avrupa veri merkezlerinde olacak bilgileri hakkında ayrıntılı bilgi sağlar. sağlar. 

---
 
### <a name="new-user-provisioning-saas-app-integrations---may-2018"></a>Yeni kullanıcı sağlama SaaS uygulama tümleştirmeleri - Mayıs 2018

**Türü:** yeni özellik  
**Hizmet kategorisi:** uygulama sağlama  
**Ürün özelliği:** 3. taraf tümleştirme
 
Azure AD, oluşturma, Bakım ve Dropbox, Salesforce, ServiceNow ve diğer SaaS uygulamalarına kullanıcı kimliklerini kaldırılmasını otomatik hale getirmenizi sağlar. Mayıs 2018 için kullanıcı sağlama desteği aşağıdaki uygulamalar Azure AD uygulama galerisinde ekledik:

- [BlueJeans](https://docs.microsoft.com/azure/active-directory/active-directory-saas-bluejeans-provisioning-tutorial)

- [Temel taşıdır OnDemand](https://docs.microsoft.com/azure/active-directory/active-directory-saas-cornerstone-ondemand-provisioning-tutorial)

- [Zendesk](https://docs.microsoft.com/azure/active-directory/active-directory-saas-zendesk-provisioning-tutorial)

Azure AD Galerisi'nde kullanıcı sağlamayı destekleyen tüm uygulamalar listesi için bkz. [ https://aka.ms/appstutorial ](https://aka.ms/appstutorial).

---
 
### <a name="azure-ad-access-reviews-of-groups-and-app-access-now-provides-recurring-reviews"></a>Gruplar ve uygulama erişimi için Azure AD erişim gözden geçirmeleri artık yinelenen gözden geçirmeler sağlar

**Türü:** yeni özellik  
**Hizmet kategorisi:** erişim gözden geçirmeleri  
**Ürün özelliği:** idare
 
Erişim gözden geçirmesi grupları ve uygulamaları şu anda genel kullanımdadır Azure AD Premium P2 bir parçası olarak.  Yöneticiler, erişim gözden geçirmeleri grup üyeliklerini ve uygulama atamaları aylık veya üç aylık gibi düzenli aralıklarla otomatik olarak yinelenmesini yapılandırmanız mümkün olacaktır.

---

### <a name="azure-ad-activity-logs-sign-ins-and-audit-are-now-available-through-ms-graph"></a>Azure AD Etkinlik günlükleri (oturum açma ve denetim) artık MS Graph üzerinden kullanılabilir

**Türü:** yeni özellik  
**Hizmet kategorisi:** raporlama  
**Ürün özelliği:** izleme ve Raporlama
 
Oturum açma işlemleri ve Denetim günlükleri'nı içeren azure AD etkinlik günlüklerini MS Graph üzerinden kullanıma sunulmuştur. Biz bu günlüklerine erişmek için MS Graph ile iki bitiş noktası kullanıma sunması. Kullanıma sunduğumuz [belgeleri](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started-azure-portal) programlı erişim için Azure AD raporlama kullanmaya başlamak için API'leri. 

---
 
### <a name="improvements-to-the-b2b-redemption-experience-and-leave-an-org"></a>B2B kullanım deneyimi ve bir kuruluştan ayrılmaya yönelik iyileştirmeler

**Türü:** yeni özellik  
**Hizmet kategorisi:** B2B  
**Ürün özelliği:** B2B/B2C

**Kullanım zaman içinde yalnızca:** sonra bir kaynak B2B API'sini kullanarak bir konuk kullanıcıyla paylaştığınız – özel davet e-posta gönderir gerekmez. Çoğu durumda, Konuk kullanıcı kaynaklara erişebilir ve zamanında kullanım deneyimi üzerinden gerçekleştirilmez. Daha fazla etkisi nedeniyle eksik e-postaları. "Gönderdiğiniz sistem, kullanım bağlantıya tıklayın muydunuz?", Konuk kullanıcılar artık sorma. Bunun anlamı sonra SPO davet manager'ın kullandığı – cloudy ekleri – iç ve dış – faydalarınız, herhangi bir durumdaki tüm kullanıcılar için kurallı aynı URL'ye sahip olabilir.

**Modern bir kullanım deneyimi:** artık giriş sayfası ekran kullanım bölün. Kullanıcıların modern görürsünüz üçüncü taraf uygulamaları için yaptıkları gibi davet eden kuruluşunuzun gizlilik bildirimini deneyimle onayı.

**Konuk kullanıcılar, kuruluş bırakabilirsiniz:** bir kuruluş ile bir kullanıcının ilişki bittikten sonra bunlar kuruluştan ayrılmadan Self Servis. Artık ", daha fazla yükseltmeyi destek biletlerini kaldırılacak" davet edildikleri kuruluştan 's yöneticinin çağrılıyor.

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---may-2018"></a>Azure AD uygulama galerisinde yeni Federasyon Uygulamaları kullanılabilir - Mayıs 2018

**Türü:** yeni özellik  
**Hizmet kategorisi:** kurumsal uygulamalar  
**Ürün özelliği:** 3. taraf tümleştirme
 
Mayıs 2018'de uygulama galerimizden Federasyon ile 18 Bu yeni uygulamaları desteği ekledik:

[AwardSpring](https://docs.microsoft.com/azure/active-directory/active-directory-saas-awardspring-tutorial), Infogix Data3Sixty yöneten, [Yodeck](https://docs.microsoft.com/azure/active-directory/active-directory-saas-infogix-tutorial), [Jamf Pro](https://docs.microsoft.com/azure/active-directory/active-directory-saas-jamfprosamlconnector-tutorial), [KnowledgeOwl](https://docs.microsoft.com/azure/active-directory/active-directory-saas-knowledgeowl-tutorial), [Envi MMIS](https://docs.microsoft.com/azure/active-directory/active-directory-saas-envimmis-tutorial), [LaunchDarkly](https://docs.microsoft.com/azure/active-directory/active-directory-saas-launchdarkly-tutorial), [Adobe büyüleyecektir asal](https://docs.microsoft.com/azure/active-directory/active-directory-saas-adobecaptivateprime-tutorial), [çevrimiçi montaj](https://docs.microsoft.com/azure/active-directory/active-directory-saas-montageonline-tutorial),[まなびポケット](https://docs.microsoft.com/azure/active-directory/active-directory-saas-manabipocket-tutorial), OpenReel, [yay yayımlama - SSO ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-arc-tutorial), [PlanGrid](https://docs.microsoft.com/azure/active-directory/active-directory-saas-plangrid-tutorial), [iWellnessNow](https://docs.microsoft.com/azure/active-directory/active-directory-saas-iwellnessnow-tutorial), [Proxyclick](https://docs.microsoft.com/azure/active-directory/active-directory-saas-proxyclick-tutorial), [Riskware](https://docs.microsoft.com/azure/active-directory/active-directory-saas-riskware-tutorial), [Flock](https://docs.microsoft.com/azure/active-directory/active-directory-saas-flock-tutorial), [Reviewsnap](https://docs.microsoft.com/azure/active-directory/active-directory-saas-reviewsnap-tutorial)

Uygulamalar hakkında daha fazla bilgi için bkz. [Azure Active Directory ile SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial).

Azure AD uygulama galerisinde uygulamanızı listeleme hakkında daha fazla bilgi için bkz. [uygulamanızı Azure Active Directory Uygulama galerisinde listeleyin](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing).

---
 
### <a name="new-step-by-step-deployment-guides-for-azure-active-directory"></a>Azure Active Directory için yeni adım adım dağıtım kılavuzları

**Türü:** yeni özellik  
**Hizmet kategorisi:** diğer  
**Ürün özelliği:** dizini
 
Azure Active Directory (Azure AD) gibi Self Servis parola sıfırlama (SSPR) dağıtmak nasıl yeni, adım adım yönergeler çoklu oturum açma (SSO), koşullu erişim (CA), kullanıcı hazırlama, Active Directory Federasyon Hizmetleri (ADFS) için uygulama ara sunucusu Geçişli kimlik doğrulaması (PTA) ve ADFS için parola karması eşitlemesi (PHS).

Dağıtım kılavuzlarını görüntülemek için Git [kimlik dağıtım kılavuzları](https://aka.ms/DeploymentPlans) github deposu. Dağıtım kılavuzları hakkında geri bildirim sağlamak için kullanın [dağıtım planı geri bildirim formu](https://aka.ms/deploymentplanfeedback). Dağıtım kılavuzları hakkında sorularınız varsa, adresinden bize başvurun [IDGitDeploy](mailto:idgitdeploy@microsoft.com).

---

### <a name="enterprise-applications-search---load-more-apps"></a>Kurumsal Uygulamalar Arama - Daha Fazla Uygulama Yükle

**Türü:** yeni özellik  
**Hizmet kategorisi:** kurumsal uygulamalar  
**Ürün özelliği:** SSO
 
Hizmet sorumluları / uygulamalarınızı bulma konusunda sorun mu yaşıyorsunuz? Tüm uygulamalar listesini, Kurumsal uygulamalarınızda daha fazla uygulama yükleme olanağı ekledik. Varsayılan olarak 20 uygulamaları göstereceğiz. Artık tıklatabilir, **daha fazla Yükle** ek uygulamaları görüntülemek için. 

---
 
### <a name="the-may-release-of-aadconnect-contains-a-public-preview-of-the-integration-with-pingfederate-important-security-updates-many-bug-fixes-and-new-great-new-troubleshooting-tools"></a>AADConnect Mayıs sürümü PingFederate tümleştirmesinin genel önizlemesi, önemli güvenlik güncelleştirmeleri, birçok hata düzeltmesi ve yeni ve harika sorun giderme araçlarını içerir. 

**Türü:** değiştirilen özellik  
**Hizmet kategorisi:** AD Bağlan  
**Ürün özelliği:** kimlik yaşam döngüsü yönetimi
 
AADConnect Mayıs sürümü PingFederate tümleştirmesinin genel önizlemesi, önemli güvenlik güncelleştirmeleri, birçok hata düzeltmesi ve yeni ve harika sorun giderme araçlarını içerir. Sürüm notlarında bulabilirsiniz [burada](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-version-history#118190).

---

### <a name="azure-ad-access-reviews-auto-apply"></a>Azure AD erişim gözden geçirmeleri: otomatik olarak uygula

**Türü:** değiştirilen özellik  
**Hizmet kategorisi:** erişim gözden geçirmeleri  
**Ürün özelliği:** idare

Erişim gözden geçirmeleri, grupları ve uygulamaları sunulmuştur Azure AD Premium P2 bir parçası olarak. Bir yönetici, erişim gözden geçirmesi tamamlandığında otomatik olarak gözden geçirenin değişiklikler, Grup veya uygulama uygulamak üzere yapılandırabilirsiniz. Yönetici kullanıcının sürekli erişim gözden geçirenler olmadı yanıt, erişimini kaldırmak, erişim tutun veya sistem önerileri ele ne de belirtebilirsiniz. 

---

### <a name="id-tokens-can-no-longer-be-returned-using-the-query-responsemode-for-new-apps"></a>Kimlik belirteçleri artık yeni uygulamalar için query response_mode kullanılarak döndürülemez. 

**Türü:** değiştirilen özellik  
**Hizmet kategorisi:** kimlik doğrulamaları (oturum açma bilgileri)  
**Ürün özelliği:** kullanıcı kimlik doğrulaması
 
Ya da 25 Nisan 2018'den sonra oluşturulan uygulamaların artık oluşturabileceksiniz istemek bir **id_token** kullanarak **sorgu** response_mode.  Yardımcı uygulama saldırı yüzeyinizi azaltmak ve bu Azure AD satır içi OIDC belirtimleri ile getirir.  25 Nisan 2018 tarihinden önce oluşturulan uygulamaların, kullanımından değil engellenir **sorgu** ile bir response_type response_mode **id_token**.  Döndürülen bir id_token, AAD'den istenirken hata **AADSTS70007: 'query' değil 'response_mode' ın desteklenen bir değer bir belirteç isterken**.

**Parça** ve **form_post** response_modes - yeni uygulama nesneleri oluşturma (örneğin, uygulama proxy'si kullanım için) emin olun, bu response_modes birini kullanmak bunlar oluşturmadan önce çalışmaya devam bir Yeni uygulama.  
 
---
 
## <a name="april-2018"></a>Nisan 2018 

### <a name="azure-ad-b2c-access-token-are-ga"></a>Azure AD B2C Erişim Belirteci: GA

**Türü:** yeni özellik  
**Hizmet kategorisi:** B2C - tüketici Kimlik Yönetimi  
**Ürün özelliği:** B2B/B2C 

Azure AD B2C ile güvenliği sağlanan Web API'leri artık erişebilirsiniz erişim belirteçleri kullanarak. Özellik genel Önizleme için büyüyecek taşınıyor Azure AD B2C uygulamaları ve web API'leri yapılandırmak için kullanıcı Arabirimi deneyimi İyileştirildi ve başka küçük geliştirmeler yapıldı.
 
Daha fazla bilgi için [Azure AD B2C: istenen erişim belirteçleri](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-access-tokens).

---

### <a name="test-single-sign-on-configuration-for-saml-based-applications"></a>SAML tabanlı uygulamalar için çoklu oturum açma yapılandırmasını test edin

**Türü:** yeni özellik  
**Hizmet kategorisi:** kurumsal uygulamalar  
**Ürün özelliği:** SSO

SAML tabanlı SSO uygulamaları yapılandırırken, yapılandırma sayfasında tümleştirme test edebilirsiniz. Oturum açma sırasında bir hatayla karşılaşırsanız, hata test deneyiminde sağlayabilir ve Azure AD ile çözüm adımları belirli sorunu çözmek için sağlar.

Daha fazla bilgi için bkz.

- [Azure Active Directory uygulama galerisinde bulunmayan uygulamalar için çoklu oturum açmayı yapılandırma](https://docs.microsoft.com/azure/active-directory/active-directory-saas-custom-apps)
- [Azure Active Directory'de uygulamalar için SAML tabanlı çoklu oturum açma hata ayıklama](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-debugging)

---
 
### <a name="azure-ad-terms-of-use-now-has-per-user-reporting"></a>Azure AD Kullanım Koşulları artık kullanıcı başına raporlama içeriyor

**Türü:** yeni özellik  
**Hizmet kategorisi:** kullanım koşulları  
**Ürün özelliği:** uyumluluk
 
Yöneticiler artık belirli bir ToU'ı seçin ve ToU ve hangi, tarih yer aldığı onay tüm kullanıcılar görür.

Daha fazla bilgi için [Azure AD kullanım koşulları özelliği](https://docs.microsoft.com/azure/active-directory/active-directory-tou).

---
 
### <a name="azure-ad-connect-health-risky-ip-for-ad-fs-extranet-lockout-protection"></a>Azure AD Connect Health: AD FS extranet kilitleme koruması için riskli IP 

**Türü:** yeni özellik  
**Hizmet kategorisi:** diğer  
**Ürün özelliği:** izleme ve Raporlama

Bir saatlik veya günlük olarak hatalı U/P oturum açma eşiğini aşan IP algılama özelliğine destekler adresleri artık connect Health. Bu özellik tarafından sağlanan özellikler şunlardır:

- IP adresi ve bir saatlik/günlük olarak özelleştirilebilir eşik ile oluşturulan başarısız oturum açma sayısını gösteren kapsamlı bir rapor.
- E-posta tabanlı uyarılar belirli bir IP adresi, gösteren saatlik/günlük olarak hatalı U/P oturum açma eşiğini aştı.
- Verilerin ayrıntılı bir analiz yapmak için bir yükleme seçeneği

Daha fazla bilgi için [riskli IP raporu](https://aka.ms/aadchriskyip).

---
 
### <a name="easy-app-config-with-metadata-file-or-url"></a>Meta veri dosyası veya URL ile kolay uygulama yapılandırması

**Türü:** yeni özellik  
**Hizmet kategorisi:** kurumsal uygulamalar  
**Ürün özelliği:** SSO

Kurumsal uygulamalar sayfasında Yöneticiler SAML tabanlı oturum açma AAD galeri ve galeri dışı bir uygulama için yapılandırmak için bir SAML meta veri dosyasını karşıya yükleyebilirsiniz.

Ayrıca, Azure AD uygulama Federasyon meta veri URL'si ile hedeflenen uygulamaya SSO yapılandırmak için kullanabilirsiniz.

Daha fazla bilgi için [Azure Active Directory Uygulama galerisinde bulunmayan uygulamalar için çoklu oturum açma yapılandırma](https://docs.microsoft.com/azure/active-directory/active-directory-saas-custom-apps).

---

### <a name="azure-ad-terms-of-use-now-generally-available"></a>Azure AD Kullanım koşulları artık genel olarak kullanılabilir

**Türü:** yeni özellik  
**Hizmet kategorisi:** kullanım koşulları  
**Ürün özelliği:** uyumluluk
 

Azure AD kullanım koşulları, genel Önizleme için kullanıma sunuldu taşıdık.

Daha fazla bilgi için [Azure AD kullanım koşulları özelliği](https://docs.microsoft.com/azure/active-directory/active-directory-tou).

---

### <a name="allow-or-block-invitations-to-b2b-users-from-specific-organizations"></a>Belirli kuruluşlardan B2B kullanıcılarına gelen davetlere izin verme veya bu davetleri engelleme

**Türü:** yeni özellik  
**Hizmet kategorisi:** B2B  
**Ürün özelliği:** B2B/B2C
 

Paylaşın ve Azure AD B2B işbirliği birlikte çalışmak istediğiniz hangi iş ortağı kuruluşlar artık belirtebilirsiniz. Bunu yapmak için özel bir listesini oluşturmak seçebileceğiniz izin ver veya Reddet etki alanları. Bu özellikleri kullanılarak bir etki alanı engellendiğinde, çalışanlar artık bu etki alanında davetleri kişilere gönderebilirsiniz.

Bu, onaylanan kullanıcılar için sorunsuz bir deneyim etkinleştirirken, kaynaklarınıza erişimi denetlemenize yardımcı olur.

B2B işbirliği bu özellik tüm Azure Active Directory müşterileri tarafından kullanılabilir ve koşullu erişim ve kimlik koruma gibi Azure AD Premium özellikleri ile birlikte daha ayrıntılı denetim için zaman ve dış iş kullanıcılar oturum nasıl kullanılabilir içinde ve erişim.

Daha fazla bilgi için [B2B kullanıcıları için izin verilenler veya Engellenenler davetleri belirli kuruluşlardan](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-allow-deny-list).

---
 
### <a name="new-federated-apps-available-in-azure-ad-app-gallery"></a>Azure AD uygulama galerisinde yeni federasyon uygulamaları kullanılabilir

**Türü:** yeni özellik  
**Hizmet kategorisi:** kurumsal uygulamalar  
**Ürün özelliği:** 3. taraf tümleştirme

Nisan 2018'de uygulama galerimizden Federasyon ile 13 bu yeni uygulamaları desteği ekledik:

Ölçüt HCM, [FiscalNote](https://docs.microsoft.com/azure/active-directory/active-directory-saas-fiscalnote-tutorial), [gizli sunucusu (şirket içi)](https://docs.microsoft.com/azure/active-directory/active-directory-saas-secretserver-on-premises-tutorial), [dinamik sinyal](https://docs.microsoft.com/azure/active-directory/active-directory-saas-dynamicsignal-tutorial), [mindWireless](https://docs.microsoft.com/azure/active-directory/active-directory-saas-mindwireless-tutorial), [Kuruluş Şeması Artık](https://docs.microsoft.com/azure/active-directory/active-directory-saas-orgchartnow-tutorial), [Ziflow](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ziflow-tutorial), [AppNeta Performans İzleyicisi'ni](https://docs.microsoft.com/azure/active-directory/active-directory-saas-appneta-tutorial), [Elium](https://docs.microsoft.com/azure/active-directory/active-directory-saas-elium-tutorial) , [Fluxx Labs](https://docs.microsoft.com/azure/active-directory/active-directory-saas-fluxxlabs-tutorial), [ Cisco bulut](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ciscocloud-tutorial), raf, [SafetyNet](https://docs.microsoft.com/azure/active-directory/active-directory-saas-safetynet-tutorial)

Uygulamalar hakkında daha fazla bilgi için bkz. [Azure Active Directory ile SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial).

Azure AD uygulama galerisinde uygulamanızı listeleme hakkında daha fazla bilgi için bkz. [uygulamanızı Azure Active Directory Uygulama galerisinde listeleyin](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing).

---
 
### <a name="grant-b2b-users-in-azure-ad-access-to-your-on-premises-applications-public-preview"></a>Azure AD'deki B2B kullanıcılarına, şirket içi uygulamalarınıza erişim izni verin (genel önizleme)

**Türü:** yeni özellik  
**Hizmet kategorisi:** B2B  
**Ürün özelliği:** B2B/B2C

Azure ad iş ortağı kuruluşlardan Konuk kullanıcıları davet etmek için Azure Active Directory (Azure AD) B2B işbirliği özelliklerini kullanan bir kuruluş, artık bu B2B kullanıcıları şirket içi uygulamalara erişim sağlayabilir. Bu şirket içi uygulamaları, Kerberos Kısıtlı temsilci (KCD) ile SAML tabanlı kimlik doğrulaması veya tümleşik Windows kimlik doğrulaması (IWA) kullanabilirsiniz.

Daha fazla bilgi için [verme B2B kullanıcıları Azure AD'de, şirket içi uygulamalarınıza erişim](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-hybrid-cloud-to-on-premises).

---
 
### <a name="get-sso-integration-tutorials-from-the-azure-marketplace"></a>Azure Marketi'nden SSO tümleştirme öğreticilerini alın

**Türü:** değiştirilen özellik  
**Hizmet kategorisi:** diğer  
**Ürün özelliği:** 3. taraf tümleştirme

Listelenen bir uygulama bildirimi [Azure Market](https://azuremarketplace.microsoft.com/marketplace/apps/category/azure-active-directory-apps?page=1) destekleyen SAML tabanlı çoklu tıklayarak oturum, **şimdi edinin** bu uygulamayla ilişkili tümleştirme öğreticisiyle sağlar. 

---

### <a name="faster-performance-of-azure-ad-automatic-user-provisioning-to-saas-applications"></a>SaaS uygulamalarına daha yüksek Azure AD otomatik kullanıcı sağlama performansı

**Türü:** değiştirilen özellik  
**Hizmet kategorisi:** uygulama sağlama  
**Ürün özelliği:** 3. taraf tümleştirme
 
Daha önce Azure AD kiracıları 100. 000'den birleşik kullanıcılar içeriyorsa sağlama bağlayıcılar (örneğin Salesforce, ServiceNow ve kutusu) SaaS uygulamaları için Azure Active Directory kullanıcı kullanan müşteriler yavaş performans karşılaşması ve Gruplar ve hangi kullanıcıların sağlanan belirlemek için kullanıcı ve Grup atamaları kullanıyordu.

2 Nisan 2018'de, Azure Active Directory ve hedef SaaS uygulamaları arasında ilk eşitlemeler gerçekleştirmek için gereken süreyi büyük ölçüde azaltmak Azure AD sağlama hizmeti için önemli performans geliştirmeleri dağıtıldı.

Sonuç olarak, birçok müşteri, ilk eşitleme geçen gün sayısı veya hiçbir zaman tamamlandı, artık yalnızca birkaç dakika veya saat içinde tamamlıyorsanız uygulamalara vardı.

Daha fazla bilgi için [sağlama sırasında ne olur?](https://docs.microsoft.com/azure/active-directory/active-directory-saas-app-provisioning#what-happens-during-provisioning)

---

### <a name="self-service-password-reset-from-windows-10-lock-screen-for-hybrid-azure-ad-joined-machines"></a>Hibrit Azure AD'ye katılan makineler için Windows 10 kilit ekranından self servis parola sıfırlama

**Türü:** değiştirilen özellik  
**Hizmet kategorisi:** Self Servis parola sıfırlama  
**Ürün özelliği:** kullanıcı kimlik doğrulaması
 
Hibrit Azure AD'ye katılmış olan makineler için destek eklemek için Windows 10 SSPR özelliğini güncelleştirdik. Bu özellik kullanılabilir Windows 10 RS4 bir Windows 10 makinenin kilit ekranında parolalarını sıfırlamalarına olanak tanır. Etkin ve Self Servis parola sıfırlama için kaydolan kullanıcılar, bu özelliği kullanabilir.

Daha fazla bilgi için [Azure AD parola sıfırlama oturum açma ekranından](https://docs.microsoft.com/azure/active-directory/authentication/tutorial-sspr-windows).
 
---

## <a name="march-2018"></a>Mart 2018
 
### <a name="certificate-expire-notification"></a>Sertifika sona bildirimi

**Türü:** düzeltildi  
**Hizmet kategorisi:** kurumsal uygulamalar  
**Ürün özelliği:** SSO
 
Galeri dışı uygulama dolmak üzere olduğu ya da Azure AD galeri için bir sertifika olduğunda bir bildirim gönderir. 

Bazı kullanıcılar, SAML tabanlı çoklu oturum açma için yapılandırılmış kurumsal uygulamalar için bildirim almadı. Bu sorun çözüldü. Azure AD 7, 30 ve 60 gün içinde sona erecek sertifikaları için bildirim gönderir. Bu olay denetim günlüklerinde görebilirsiniz. 

Daha fazla bilgi için bkz.

- [Federasyon çoklu oturum açma için Azure Active Directory'de sertifikaları yönetme](https://docs.microsoft.com/azure/active-directory/active-directory-sso-certs)
- [Azure Active Directory portalındaki denetim etkinlik raporları](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs)
 
---
 
### <a name="twitter-and-github-identity-providers-in-azure-ad-b2c"></a>Azure AD B2C, twitter ve GitHub kimlik sağlayıcıları

**Türü:** yeni özellik  
**Hizmet kategorisi:** B2C - tüketici Kimlik Yönetimi  
**Ürün özelliği:** B2B/B2C
 
Artık, bir Azure AD B2C kimlik sağlayıcısı olarak Twitter veya GitHub ekleyebilirsiniz. Twitter büyüyecek için genel preview'dan taşınıyor GitHub genel önizlemede yayınlanır.

Daha fazla bilgi için [Azure AD B2B işbirliği nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b).
 
---

### <a name="restrict-browser-access-using-intune-managed-browser-with-azure-ad-application-based-conditional-access-for-ios-and-android"></a>Azure AD uygulama tabanlı koşullu erişimle, iOS ve Android için Intune Managed Browser kullanarak tarayıcı erişimi kısıtlama

**Türü:** yeni özellik  
**Hizmet kategorisi:** koşullu erişim  
**Ürün özelliği:** kimlik güvenliği ve koruması
 
**Artık genel önizlemede!**

**Intune Managed Browser'da SSO:** çalışanlarınızın çoklu oturum açma (Microsoft Outlook gibi) için yerel istemcileri ve Intune Managed Browser tüm Azure AD bağlantılı uygulamalar için kullanabilirsiniz.

**Intune Managed Browser koşullu erişim desteği:** artık uygulama tabanlı koşullu erişim ilkelerini kullanarak Intune Managed browser'ı kullanmak çalışanların gerektirebilir.

Bu konuda hakkında daha fazla bilgiyi bizim [blog gönderisi](https://cloudblogs.microsoft.com/enterprisemobility/2018/03/15/the-intune-managed-browser-now-supports-azure-ad-sso-and-conditional-access/).

Daha fazla bilgi için bkz.

- [Uygulama tabanlı koşullu erişim Kurulumu](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)

- [Yönetilen tarayıcı ilkelerini yapılandırma](https://aka.ms/managedbrowser)  

---
 
### <a name="app-proxy-cmdlets-in-powershell-ga-module"></a>Uygulama proxy'si GA Powershell modülündeki cmdlet'leri

**Türü:** yeni özellik  
**Hizmet kategorisi:** uygulama ara sunucusu  
**Ürün özelliği:** erişim denetimi
 
Uygulama proxy'si cmdlet'leri için desteği Powershell GA modülünde sunuldu! Bu Powershell modülleri - daha fazla bir yıl, bazı cmdlet'ler hale gelirse çalışmayı durdurabilir güncel gerektirir. 

Daha fazla bilgi için [AzureAD](https://docs.microsoft.com/powershell/module/Azuread/?view=azureadps-2.0).
 
---
 
### <a name="office-365-native-clients-are-supported-by-seamless-sso-using-a-non-interactive-protocol"></a>Office 365 için yerel istemcileri tarafından etkileşimli olmayan bir protokol kullanarak sorunsuz SSO desteklenir

**Türü:** yeni özellik  
**Hizmet kategorisi:** kimlik doğrulamaları (oturum açma bilgileri)  
**Ürün özelliği:** kullanıcı kimlik doğrulaması
 
Office 365 yerel istemci kullanarak kullanıcı (sürüm 16.0.8730.xxxx ve üstü) sorunsuz çoklu oturum açma kullanarak sessiz oturum açma deneyimi alın. Bu destek, Azure AD'ye etkileşimli olmayan bir protokol (WS-Trust) ekleyerek sağlanır.

Daha fazla bilgi için [nasıl sorunsuz çoklu oturum açma çalışma ile yerel bir istemcide oturum?](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso-how-it-works#how-does-sign-in-on-a-native-client-with-seamless-sso-work)
 
---

### <a name="users-get-a-silent-sign-on-experience-with-seamless-sso-if-an-application-sends-sign-in-requests-to-azure-ads-tenant-endpoints"></a>Bir uygulamayı Azure AD'nin Kiracı uç noktalar için oturum açma isteği gönderirse bir sessiz oturum açma deneyimini sorunsuz SSO ile kullanıcılar Al

**Türü:** yeni özellik  
**Hizmet kategorisi:** kimlik doğrulamaları (oturum açma bilgileri)  
**Ürün özelliği:** kullanıcı kimlik doğrulaması
 
Kullanıcılar bir sessiz oturum açma deneyimini sorunsuz SSO ile bir uygulama bildirimi alır (örneğin, `https://contoso.sharepoint.com`) oturum açma istekleri için Azure AD'nin Kiracı uç noktası; diğer bir deyişle, gönderir `https://login.microsoftonline.com/contoso.com/<..>` veya `https://login.microsoftonline.com/<tenant_ID>/<..>` - Azure AD'nin ortak uç nokta yerine (`https://login.microsoftonline.com/common/<...>`).

Daha fazla bilgi için [Azure Active Directory sorunsuz çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso). 

---
 
### <a name="need-to-add-only-one-azure-ad-url-instead-of-two-urls-previously-to-users-intranet-zone-settings-to-roll-out-seamless-sso"></a>Sorunsuz çoklu oturum açma almak için kullanıcıların Intranet bölgesi ayarlarını daha önce iki URL yerine yalnızca bir Azure AD URL'sini eklemeniz gerekir

**Türü:** yeni özellik  
**Hizmet kategorisi:** kimlik doğrulamaları (oturum açma bilgileri)  
**Ürün özelliği:** kullanıcı kimlik doğrulaması
 
Kullanıcılarınız için sorunsuz SSO alma için yalnızca bir Azure AD URL kullanıcıların intranet bölgesi ayarlarını Active Directory'de Grup İlkesi'ni kullanarak eklemeniz gerekir: `https://autologon.microsoftazuread-sso.com`. Daha önce müşterilerin iki URL'si eklemek için zorunluluğundaydı.

Daha fazla bilgi için [Azure Active Directory sorunsuz çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso). 
 
---
 
### <a name="new-federated-apps-available-in-azure-ad-app-gallery"></a>Yeni Federasyon uygulamaları kullanılabilir Azure AD uygulama Galerisi

**Türü:** yeni özellik  
**Hizmet kategorisi:** kurumsal uygulamalar  
**Ürün özelliği:** 3. taraf tümleştirme

Mart 2018'de bu 15 yeni Federasyon uygulamalarla uygulama galerimizden desteği ekledik:

[Boxcryptor](https://docs.microsoft.com/azure/active-directory/active-directory-saas-boxcryptor-tutorial), [CylancePROTECT](https://docs.microsoft.com/azure/active-directory/active-directory-saas-cylanceprotect-tutorial), Wrike, [SignalFx](https://docs.microsoft.com/azure/active-directory/active-directory-saas-signalfx-tutorial), Yardımcısı tarafından FirstAgenda, [YardiOne](https://docs.microsoft.com/azure/active-directory/active-directory-saas-yardione-tutorial), Vtiger CRM, inwink, [Genliğe](https://docs.microsoft.com/azure/active-directory/active-directory-saas-amplitude-tutorial), [Spacio](https://docs.microsoft.com/azure/active-directory/active-directory-saas-spacio-tutorial), [ContractWorks](https://docs.microsoft.com/azure/active-directory/active-directory-saas-contractworks-tutorial), [Bersin](https://docs.microsoft.com/azure/active-directory/active-directory-saas-bersin-tutorial), [Mercell](https://docs.microsoft.com/azure/active-directory/active-directory-saas-mercell-tutorial), [Trisotech dijital Kuruluş Sunucusu](https://docs.microsoft.com/azure/active-directory/active-directory-saas-trisotechdigitalenterpriseserver-tutorial), [Qumu bulut](https://docs.microsoft.com/azure/active-directory/active-directory-saas-qumucloud-tutorial).
 
Uygulamalar hakkında daha fazla bilgi için bkz. [Azure Active Directory ile SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial).

Azure AD uygulama galerisinde uygulamanızı listeleme hakkında daha fazla bilgi için bkz. [uygulamanızı Azure Active Directory Uygulama galerisinde listeleyin](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing). 

---
 
### <a name="pim-for-azure-resources-is-generally-available"></a>Azure Kaynakları için PIN genel olarak kullanılabilir

**Türü:** yeni özellik  
**Hizmet kategorisi:** Privileged Identity Management  
**Ürün özelliği:** Privileged Identity Management
 
Dizin rolleri için Azure AD Privileged Identity Management kullanıyorsanız, artık PIM'ın süresi sınırlı erişim ve atama özellikleri abonelik, kaynak grupları, sanal makineler ve desteklenen herhangi bir kaynak gibi Azure kaynak rolleri için kullanabilirsiniz Azure Resource Manager tarafından. Just-ın-Time rolleri etkinleştirirken multi-Factor Authentication yürürlüğe ve onaylanan değişiklik windows ile koordinasyon halinde etkinleştirmeleri zamanlayabilirsiniz. Ayrıca, bu sürüm geliştirmeleri güncelleştirilmiş bir kullanıcı Arabirimi, onay iş akışları ve özelliği yakında sona erecek rolleri genişletmek ve süresi dolan rolleri yenileme dahil olmak üzere genel Önizleme sırasında kullanılabilir değil ekler.

Daha fazla bilgi için [(Önizleme) Azure kaynakları için PIM](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac)
 
---
 
### <a name="adding-optional-claims-to-your-apps-tokens-public-preview"></a>Uygulama belirteçlerinize İsteğe Bağlı Talepler ekleniyor (genel önizleme)

**Türü:** yeni özellik  
**Hizmet kategorisi:** kimlik doğrulamaları (oturum açma bilgileri)  
**Ürün özelliği:** kullanıcı kimlik doğrulaması
 
Özel veya isteğe bağlı isteği Jwt'ler veya SAML belirteçlerini talep artık Azure AD uygulamanızı geçirebilirsiniz.  Bu, varsayılan boyutu veya Uygulanabilirlik kısıtlamaları nedeniyle bir belirteç olarak bulunmayan bir kullanıcı ya da Kiracı hakkında taleplerdir.  Şu anda genel önizlemede Azure AD uygulamaları v1.0 ve v2.0 uç noktalarda budur.  Bilgi için bkz. hangi talepleri temel eklenebilir ve bunları istemek için uygulama bildirimini düzenleme.  

Daha fazla bilgi için [isteğe bağlı Azure AD'de talep](https://docs.microsoft.com/azure/active-directory/develop/active-directory-optional-claims).
 
---
 
### <a name="azure-ad-supports-pkce-for-more-secure-oauth-flows"></a>Azure AD, daha güvenli OAuth akışları için PKCE'yi destekler

**Türü:** yeni özellik  
**Hizmet kategorisi:** kimlik doğrulamaları (oturum açma bilgileri)  
**Ürün özelliği:** kullanıcı kimlik doğrulaması
 
Azure AD belgeleri, OAuth 2.0 yetkilendirme kodu verme akışı sırasında daha güvenli iletişime izin veren PKCE desteği Not güncelleştirildi.  S256 hem düz metin code_challenges v1.0 ve v2.0 Uç noktalara desteklenir. 

Daha fazla bilgi için [bir yetkilendirme kodu istek](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-protocols-oauth-code#request-an-authorization-code). 
 
---
 
### <a name="support-for-provisioning-all-user-attribute-values-available-in-the-workday-getworkers-api"></a>Workday Get_Workers API'sinde kullanılabilir olan tüm kullanıcı öznitelik değerlerini sağlama desteği

**Türü:** yeni özellik  
**Hizmet kategorisi:** uygulama sağlama  
**Ürün özelliği:** 3. taraf tümleştirme
 
Gelen Workday'den Active Directory artık Azure AD için sağlama ve genel Önizleme çıkarma ve sağlama Workday Get_Workers API'SİNDE kullanılabilir tüm öznitelik değerleri destekler. Bu ek standart yüzlerce destekler ekler ve ilk sürümü Workday ile birlikte gelen olanlar dışında özel öznitelikler gelen bağlayıcı sağlama.

Daha fazla bilgi için bkz: [Workday kullanıcı özniteliklerinin listesi özelleştirme](https://docs.microsoft.com/azure/active-directory/active-directory-saas-workday-inbound-tutorial#customizing-the-list-of-workday-user-attributes)

---

### <a name="changing-group-membership-from-dynamic-to-static-and-vice-versa"></a>Dinamik olan grup üyeliğini statik olarak (veya tam tersi) değiştirme

**Türü:** yeni özellik  
**Hizmet kategorisi:** Grup Yönetimi  
**Ürün özelliği:** işbirliği
 
Üyelik bir gruba nasıl yönetilir değiştirmek mümkündür. Bu gruba varolan herhangi bir başvuruyu hala geçerli olduğundan sistemde aynı grubu adını ve Kimliğini tutmak istediğinizde kullanışlıdır. Yeni grup oluşturma, bu başvuruları güncelleştirilmesi gerekir.
Bu işlevselliği desteklemek için Azure AD yönetim merkezini güncelleştirdik. Şimdi, müşterilerin var olan grupları dinamik üyelik atanan üyelik ve tersi dönüştürebilirsiniz. Mevcut PowerShell cmdlet'leri de yine kullanılabilir durumdadır.

Daha fazla bilgi için [statik ve tersi için dinamik üyeliği değiştirme](https://docs.microsoft.com/azure/active-directory/active-directory-groups-dynamic-membership-azure-portal#changing-dynamic-membership-to-static-and-vice-versa)

---

### <a name="improved-sign-out-behavior-with-seamless-sso"></a>Sorunsuz Çoklu Oturum Açma ile gelişmiş oturum kapatma davranışı

**Türü:** değiştirilen özellik  
**Hizmet kategorisi:** kimlik doğrulamaları (oturum açma bilgileri)  
**Ürün özelliği:** kullanıcı kimlik doğrulaması
 
Kullanıcılar, Azure AD tarafından güvenli hale getirilmiş bir uygulamaya dışında açıkça imzalanmış olsa bile, daha önce bunlar otomatik olarak etki alanına katılmış cihazlarından bir Azure AD uygulaması, corpnet içinde yeniden erişmeye çalışıyorsanız geri sorunsuz SSO kullanarak oturum açmış olmanız. Bu değişiklik, oturumunuzu desteklenir.  Bu aynı veya farklı Azure seçmelerini sağlar geri, otomatik olarak sorunsuz SSO kullanarak oturum açmış yerine oturum açmak için bir AD hesabı.

Daha fazla bilgi için [Azure Active Directory sorunsuz çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso)
 
---
 
### <a name="application-proxy-connector-version-154020-released"></a>Uygulama Ara Sunucusu Bağlayıcısı 1.5.402.0 Sürümü Yayımlandı

**Türü:** değiştirilen özellik  
**Hizmet kategorisi:** uygulama ara sunucusu  
**Ürün özelliği:** kimlik güvenliği ve koruması
 
Bu bağlayıcı sürüm kademeli olarak Kasım kullanıma sunulacaktır. Bu yeni bağlayıcı sürüm, aşağıdaki değişiklikleri içerir:

- Bağlayıcı artık etki alanı düzeyi tanımlama bilgilerini bunun yerine alt etki alanı düzeyini ayarlar. Bu daha sorunsuz SSO bir deneyim sağlar ve yedek kimlik doğrulama istemleri önler.
- Öbekli kodlama istekleri için destek
- Geliştirilmiş bağlayıcı sistem durumu izleme 
- Birçok hata düzeltmesi ve kararlılık geliştirmeleri

Daha fazla bilgi için [anlamak Azure AD uygulama ara sunucusu bağlayıcıları](https://docs.microsoft.com/azure/active-directory/application-proxy-understand-connectors).
 
---

## <a name="february-2018"></a>Şubat 2018
 
### <a name="improved-navigation-for-managing-users-and-groups"></a>Kullanıcılar ve grupları yönetmek için Gezinti geliştirildi

**Türü:** değişiklik planı  
**Hizmet kategorisi:** dizin yönetimi  
**Ürün özelliği:** dizini

Kullanıcılar ve grupları yönetmek için gezinti deneyimini basitleştirilmiştir. Artık dizine genel bakış'tan doğrudan silinen kullanıcıların listesine daha kolay erişimi olan tüm kullanıcıların listesini gidebilirsiniz. Grup Yönetimi ayarlarına daha kolay erişim ile tüm grupların listesine doğrudan dizin genel bakış'tan da gidebilirsiniz. Ve ayrıca dizine genel bakış sayfasından bir kullanıcı, Grup, Kurumsal uygulama veya uygulama kaydını arayabilirsiniz. 

---

### <a name="availability-of-sign-ins-and-audit-reports-in-microsoft-azure-operated-by-21vianet-azure-china-21vianet"></a>Oturum açma işlemleri ve denetim kullanılabilirliğini raporlar 21Vianet tarafından işletilen Microsoft azure'da (Azure China 21Vianet)

**Türü:** yeni özellik  
**Hizmet kategorisi:** Azure Stack  
**Ürün özelliği:** izleme ve Raporlama

Azure AD etkinlik günlük raporlar tarafından (Azure China 21Vianet) örnekleri 21Vianet tarafından işletilen Microsoft azure'da kullanıma sunulmuştur. Aşağıdaki günlüklere dahil edilir:

- **Oturum açma işlemleri etkinlik günlüklerini** -tüm oturum açma işlemleri içerir, Kiracı ile ilişkilendirilen günlükleri.

- **Self Servis parola denetim günlükleri** -SSPR denetim günlükleri içerir.

- **Dizin Yönetimi denetim günlüklerini** -kullanıcı yönetimi, uygulama yönetimi ve diğerleri gibi tüm dizin yönetimiyle ilgili denetim günlükleri içerir.

Bu günlükleri ile ortamınızın nasıl çalıştığını içine öngörü sahibi olabilir. Sağlanan verilerle:

- Uygulama ve hizmetlerinizin kullanıcılarınız tarafından nasıl kullanıldığını saptayabilirsiniz.

- Kullanıcılarınızın işlerini yapmalarını engelleyen sorunları giderin.

Bu raporların nasıl kullanılacağı hakkında daha fazla bilgi için bkz. [Azure Active Directory raporlama](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal).

---

### <a name="use-report-reader-role-non-admin-role-to-view-azure-ad-activity-reports"></a>Azure AD Etkinlik raporlarını görüntülemek için "Rapor okuyucu" rolünü (yönetici olmayan rol) kullanın

**Türü:** yeni özellik  
**Hizmet kategorisi:** raporlama  
**Ürün özelliği:** izleme ve Raporlama

Yönetici olmayan rollerin Azure AD etkinlik erişmesini etkinleştirmek için müşterilerin geri bildirim parçası günlükleri gibi erişim oturum açma ve Azure portalı yanı sıra Graph Apı'lerimizi kullanarak içinde denetim etkinliği için "Rapor okuyucu" rolündeki kullanıcılar için özelliğini etkinleştirdik. 

Daha fazla bilgi için bu raporları kullanmak üzere nasıl [Azure Active Directory raporlama](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal). 

---

### <a name="employeeid-claim-available-as-user-attribute-and-user-identifier"></a>Kullanıcı özniteliği ve kullanıcı tanımlayıcısı olarak kullanılabilir EmployeeID talebi

**Türü:** yeni özellik  
**Hizmet kategorisi:** kurumsal uygulamalar  
**Ürün özelliği:** SSO
 
Yapılandırabileceğiniz **EmployeeID** üye kullanıcılar ve B2B Konukları uygulamalarda Kurumsal Uygulama UI'dan SAML tabanlı oturum açma için kullanıcı özniteliği ve kullanıcı tanımlayıcısı olarak.

Daha fazla bilgi için [Azure Active Directory'de kurumsal uygulamalar için SAML belirtecinde verilen talepleri özelleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization).

---

### <a name="simplified-application-management-using-wildcards-in-azure-ad-application-proxy"></a>Azure AD uygulama ara Sunucusu'nda joker kullanarak Basitleştirilmiş uygulama yönetimi

**Türü:** yeni özellik  
**Hizmet kategorisi:** uygulama ara sunucusu  
**Ürün özelliği:** kullanıcı kimlik doğrulaması
 
Uygulama dağıtımını kolaylaştırın ve yönetim yükünüzü azaltmak için artık joker karakter kullanan uygulamalar yayımlama olanağı desteklenmektedir. Bir joker uygulama yayımlamak için standart bir uygulama yayımlama akışını takip edin, ancak iç ve dış URL'lerinde bir joker karakter kullanın.

Daha fazla bilgi için [joker karakteri uygulamalarında Azure Active Directory Uygulama proxy'si](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-wildcard)

---

### <a name="new-cmdlets-to-support-configuration-of-application-proxy"></a>Uygulama Ara Sunucusu'nun yapılandırılmasını desteklemeye yönelik yeni cmdlet'ler

**Türü:** yeni özellik  
**Hizmet kategorisi:** uygulama ara sunucusu  
**Ürün özelliği:** platformu

AzureAD PowerShell Önizleme modülünün en son sürümü, müşterilerin PowerShell kullanarak uygulama Proxy uygulamaları yapılandırmanıza imkan tanıyan yeni cmdlet'ler içerir.

Yeni cmdlet'ler şunlardır: 

- Get-AzureADApplicationProxyApplication
- Get-AzureADApplicationProxyApplicationConnectorGroup
- Get-AzureADApplicationProxyConnector
- Get-AzureADApplicationProxyConnectorGroup
- Get-AzureADApplicationProxyConnectorGroupMembers
- Get-AzureADApplicationProxyConnectorMemberOf
- New-AzureADApplicationProxyApplication
- New-AzureADApplicationProxyConnectorGroup
- Remove-AzureADApplicationProxyApplication
- Remove-AzureADApplicationProxyApplicationConnectorGroup
- Remove-AzureADApplicationProxyConnectorGroup
- Set-AzureADApplicationProxyApplication
- Set-AzureADApplicationProxyApplicationConnectorGroup
- Set-AzureADApplicationProxyApplicationCustomDomainCertificate
- Set-AzureADApplicationProxyApplicationSingleSignOn
- Set-AzureADApplicationProxyConnector
- Set-AzureADApplicationProxyConnectorGroup

---
 
### <a name="new-cmdlets-to-support-configuration-of-groups"></a>Grupların yapılandırılmasını desteklemeye yönelik yeni cmdlet'ler

**Türü:** yeni özellik  
**Hizmet kategorisi:** uygulama ara sunucusu  
**Ürün özelliği:** platformu

AzureAD PowerShell modülünün en son sürümü Azure AD'de grupları yönetmek için cmdlet'leri içerir. Bu cmdlet'ler, daha önce AzureADPreview modülünde kullanılabilen ve şimdi AzureAD modülüne eklendi

Şimdi genel kullanılabilirlik sürümü olan Grup cmdlet'ler şunlardır: 

- Get-AzureADMSGroup
- New-AzureADMSGroup
- Remove-AzureADMSGroup
- Set-AzureADMSGroup
- Get-AzureADMSGroupLifecyclePolicy
- Yeni-AzureADMSGroupLifecyclePolicy
- Remove-AzureADMSGroupLifecyclePolicy
- Ekle-AzureADMSLifecyclePolicyGroup
- Remove-AzureADMSLifecyclePolicyGroup
- Reset-AzureADMSLifeCycleGroup   
- Get-AzureADMSLifecyclePolicyGroup

---
 
### <a name="a-new-release-of-azure-ad-connect-is-available"></a>Azure AD Connect'in yeni bir sürüm kullanılabilir

**Türü:** yeni özellik  
**Hizmet kategorisi:** AD eşitleme  
**Ürün özelliği:** platformu
 
Azure AD Connect, Azure AD arasında ve Windows Server Active Directory ve LDAP dahil olmak üzere şirket içi veri kaynakları verileri eşitlemek için tercih edilen bir araçtır.

>[!Important]
>Bu derleme şema ve eşitleme tanıtır kural değişiklikler. Azure AD Connect eşitleme hizmeti bir yükseltmeden sonra bir tam içeri aktarma ve tam eşitleme adımları tetikler. Bu davranışı değiştirmek konusunda daha fazla bilgi için bkz: [yükseltmeden sonra tam eşitleme erteleneceği nasıl](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-upgrade-previous-version#how-to-defer-full-synchronization-after-upgrade).

Bu sürüm aşağıdaki güncelleştirmeleri ve değişiklikleri sahiptir:

**Giderilen sorunlar**

- Sonraki sayfaya geçiş yaparken bölüm filtreleme sayfası için arka plan görevleri zamanlama penceresi düzeltmesi.

- Yapılandırmaveritabanı özel eylemi sırasında erişim ihlaline neden bir hata düzeltildi.

- SQL bağlantı zaman aşımı kurtarmak için bir hata düzeltildi.

- Burada SAN joker karakterler sertifikalarla Önkoşul denetimi başarısız bir hata düzeltildi.

- AAD bağlayıcı dışarı aktarma sırasında miiserver.exe kilitlenmeye neden olan bir hata düzeltildi.

- DC üzerinde çalışan AAD neden olduğunda günlüğe bir hatalı parola denemesi yapılandırmasını değiştirmek için Sihirbazı eriştikleri bir hata düzeltildi

**Yeni özellikler ve geliştirmeler**
 
- Uygulama telemetrisini - Yöneticiler, bu sınıf Aç/Kapat verilerinin geçiş yapabilirsiniz.

- Azure AD sistem durumu verilerini - yöneticiler kendi sistem durumu ayarlarını denetlemek için sistem durumu portalı ziyaret etmeniz gerekir. Hizmet İlkesi değiştirildi sonra aracılar okuyun ve onu zorla.

- Cihaz geri yazmayı yapılandırma eylemleri ve sayfa başlatma için bir ilerleme çubuğu ekledik.

- Geliştirilmiş genel tanılama HTML rapor ve tam veri toplama ZIP metin / HTML raporu.

- Geliştirilmiş otomatik güvenilirliğini yükseltin ve sunucusunun sistem durumunu belirlenebilir emin olmak için ek telemetri eklendi.

- Kullanılabilir AD Bağlayıcısı hesabındaki ayrıcalıklı hesaplara kısıtlayın. Yeni yüklemeler için sihirbazın ayrıcalıklı hesapları izinleri kısıtlar MSOL hesabı oluşturduktan sonra MSOL hesabına sahip. Değişiklikler express yüklemeleri ve otomatik olarak oluşturma hesabıyla özel yüklemeleri etkiler.

- Yükleyici SA ayrıcalık AADConnect temiz yüklemesini zorunlu tutulmayacak şekilde değiştirildi.

- Belirli bir nesnesi için eşitleme sorunlarını gidermek için yeni yardımcı program'ı seçin. Şu anda aşağıdakiler için yardımcı program denetler:

    - Azure AD kiracısında kullanıcı hesabı ile eşitlenmiş kullanıcı nesnesi arasındaki UserPrincipalName uyuşmazlığı.
  
    - Nesne filtreleme etki alanı nedeniyle eşitleme filtrelediyseniz
  
    - Nesne eşitleme filtreleme kuruluş birimi (OU) nedeniyle filtrelediyseniz

- Belirli bir kullanıcı hesabı için şirket içi Active Directory içinde depolanan geçerli parola karması eşitleme için yeni yardımcı program'ı seçin. Yardımcı programı, parola değişikliği gerektirmez. 

---
 
### <a name="applications-supporting-intune-app-protection-policies-added-for-use-with-azure-ad-application-based-conditional-access"></a>Azure AD uygulama tabanlı koşullu erişim ile destek Intune uygulama koruma ilkeleri için eklenen uygulamalar kullanın

**Türü:** değiştirilen özellik  
**Hizmet kategorisi:** koşullu erişim  
**Ürün özelliği:** kimlik güvenliği ve koruması

Uygulama tabanlı koşullu erişimi destekleyen daha fazla uygulama ekledik. Artık, Office 365 ve bu onaylı istemci uygulamalarını kullanarak diğer Azure AD bağlantılı bulut uygulamalarınıza erişimi alabilirsiniz.

Aşağıdaki uygulamalar Şubat sonuna eklenecek:

- Microsoft Power BI

- Microsoft Başlatıcısı

- Microsoft faturalama

Daha fazla bilgi için bkz.

- [Onaylı istemci uygulama gereksinimi](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement)
- [Azure AD uygulama tabanlı koşullu erişim](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)

---

### <a name="terms-of-use-update-to-mobile-experience"></a>Mobil deneyimde kullanım koşulları güncelleştirmesi 

**Türü:** değiştirilen özellik  
**Hizmet kategorisi:** kullanım koşulları  
**Ürün özelliği:** uyumluluk

Kullanım koşulları görüntülendiğinde artık tıklayabilirsiniz **görüntüleme konusunda sorun mu yaşıyorsunuz? Buraya**. Bu bağlantıya tıkladığınızda kullanım Cihazınızda yerel olarak açılır. Belgenin yazı tipi boyutu veya cihazın ekran boyutu ne olursa olsun yakınlaştırma ve gerektiğinde belgeyi okuyalım. 

---
 
## <a name="january-2018"></a>Ocak 2018
 
### <a name="new-federated-apps-available-in-azure-ad-app-gallery"></a>Yeni Federasyon uygulamaları kullanılabilir Azure AD uygulama Galerisi 

**Türü:** yeni özellik  
**Hizmet kategorisi:** kurumsal uygulamalar  
**Ürün özelliği:** 3. taraf tümleştirme

Ocak 2018'de app Galerisi'nde aşağıdaki yeni uygulamaları ile Federasyonu Desteği eklendi:

[IBM OpenPages](https://go.microsoft.com/fwlink/?linkid=864698), [OneTrust gizlilik yönetim yazılımı](https://go.microsoft.com/fwlink/?linkid=861660), [Dealpath](https://go.microsoft.com/fwlink/?linkid=863526), [IriusRisk Federasyon dizin ve [uygunluk NetBenefits](https://go.microsoft.com/fwlink/?linkid=864701).

Uygulamalar hakkında daha fazla bilgi için bkz. [Azure Active Directory ile SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial).

Azure AD uygulama galerisinde uygulamanızı listeleme hakkında daha fazla bilgi için bkz. [uygulamanızı Azure Active Directory Uygulama galerisinde listeleyin](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing). 

---
 
### <a name="sign-in-with-additional-risk-detected"></a>Ek risk algılandı bilgilerinizle oturum açın

**Türü:** yeni özellik  
**Hizmet kategorisi:** kimlik koruması  
**Ürün özelliği:** kimlik güvenliği ve koruması

Algılanan risk olayı için alma öngörü için Azure AD aboneliğiniz bağlıdır. Azure AD Premium P2 sürümü ile temel alınan tüm algılamalar hakkında en ayrıntılı bilgileri alın.

Azure AD Premium P1 edition ile lisansınızı tarafından kapsanmaz algılamalar algılanan ek risk içeren oturum açma risk olayı olarak görünür.

Daha fazla bilgi için bkz. [Azure Active Directory risk olayları](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-risk-events).
 
---

### <a name="hide-office-365-applications-from-end-users-access-panels"></a>Son kullanıcı erişim panellerinde Office 365 uygulamalarından gizleme

**Türü:** yeni özellik  
**Hizmet kategorisi:** uygulamalarım  
**Ürün özelliği:** SSO

Artık, nasıl Office 365 uygulamaları üzerinde yeni bir kullanıcı ayarı ile kullanıcı erişim panellerinde görünmesi daha iyi yönetebilirsiniz. Bu seçenek Office uygulamalarını yalnızca Office Portalı'nda gösterilecek tercih ederseniz bir kullanıcının erişim panellerinde uygulamaları sayısını azaltmak için yararlıdır. Ayar bulunan **kullanıcı ayarları** ve olarak etiketlenen **kullanıcılar yalnızca Office 365 uygulamaları Office 365 portalında görebilir**.

Daha fazla bilgi için [bir uygulamadan Azure Active Directory'de kullanıcı deneyimini Gizle](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-hide-third-party-app).

---
 
### <a name="seamless-sign-into-apps-enabled-for-password-sso-directly-from-apps-url"></a>Uygulamanın URL'sini doğrudan parola SSO için etkinleştirildiği uygulamalar sorunsuz oturum 

**Türü:** yeni özellik  
**Hizmet kategorisi:** uygulamalarım  
**Ürün özelliği:** SSO

Uygulamalarım tarayıcı uzantısı aracılığıyla uygulamalarım çoklu oturum tarayıcınızda bir kısayol olarak özellik hakkında size uygun bir aracı kullanıma sunuldu. Kullanıcının yükledikten sonra tarayıcılarında bunları uygulamalarına hızlı erişim sağlayan bir waffle menüsü simgesi görürsünüz. Kullanıcılar artık yararlanabilirsiniz:

- Özelliği doğrudan uygulamanın oturum açma sayfasında parola tabanlı SSO uygulamaları için oturum açın
- Hızlı arama özelliğini kullanarak herhangi bir uygulama başlatın
- Uzantı son kullanılan uygulamalar için kısayollar
- Uzantı, Edge, Chrome ve Firefox için kullanılabilir.
 
Daha fazla bilgi için [My Apps güvenli oturum açma uzantısı](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction#my-apps-secure-sign-in-extension).

---

### <a name="azure-ad-administration-experience-in-azure-classic-portal-has-been-retired"></a>Azure AD yönetim Klasik Azure portalı deneyiminde Çekildi

**Türü:** kullanım dışı   
**Hizmet kategorisi:** Azure AD  
**Ürün özelliği:** dizini

8 Ocak 2018'den itibaren Azure AD Yönetim'den itibaren Klasik Azure portalı deneyiminde Çekildi. Bu, Azure Klasik Portalı'nın kendisini kullanımdan kaldırma ile birlikte yapıldı. Gelecekte, kullanmanız gereken [Azure AD yönetim merkezini](https://aad.portal.azure.com) tüm, portal tabanlı yönetim için Azure ad.
 
---

### <a name="the-phonefactor-web-portal-has-been-retired"></a>PhoneFactor web portalı kullanımdan kaldırıldı

**Türü:** kullanım dışı  
**Hizmet kategorisi:** Azure AD  
**Ürün özelliği:** dizini
 
8 Ocak 2018'den itibaren PhoneFactor web portalı kullanımdan kaldırıldı. Bu portalı MFA sunucusunun yönetimi için kullanılan, ancak bu işlevleri portal.azure.com adresindeki Azure portalında taşınmıştır. 

MFA yapılandırma şu konumdadır: **Azure Active Directory \> MFA sunucusu**
 
---
 
### <a name="deprecate-azure-ad-reports"></a>Azure AD raporlar kullanımdan kaldırma

**Türü:** kullanım dışı  
**Hizmet kategorisi:** raporlama  
**Ürün özelliği:** kimlik yaşam döngüsü yönetimi  


Genel kullanılabilirlik yeni API'ler hem etkinlik ve güvenlik raporları için rapor API'leri kullanıma sunuldu ve yeni Azure Active Directory Yönetim Konsolu ile altında "/ reports" uç nokta son 31 Aralık 2017 itibariyle devre dışı bırakılmış.

**Kullanılabilir nedir?**

Yeni Yönetici Konsolu geçiş işleminin bir parçası olarak 2 yeni API'ler Azure AD etkinlik günlüklerini almak için kullanılabilir gerçekleştirdik. Yeni API'leri daha zengin filtreleme ve sıralama daha zengin denetim ve oturum açma etkinlikleri sağlamaya ek olarak işlevselliği sunmaktadır. Güvenlik raporları ile önceden kullanılabilen veri artık Microsoft Graph API'si kimlik koruması risk olayları aracılığıyla erişilebilir.

Daha fazla bilgi için bkz.

- [Azure Active Directory raporlama API'SİYLE çalışmaya başlama](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started-azure-portal)

- [Microsoft Graph ve Azure Active Directory kimlik koruması ile çalışmaya başlama](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection-graph-getting-started)

---

## <a name="december-2017"></a>Aralık 2017

### <a name="terms-of-use-in-the-access-panel"></a>Erişim panelinde kullanım koşulları

**Türü:** yeni özellik  
**Hizmet kategorisi:** kullanım koşulları  
**Ürün özelliği:** uyumluluk
 
Artık erişim Masası'na gidin ve daha önce kabul ettiğiniz kullanım koşullarını görüntüleyin.

Şu adımları uygulayın:

1. Git [MyApps portalında](https://myapps.microsoft.com)ve oturum açın.

2. Sağ üst köşede, adınızı seçerek ve ardından **profili** listeden. 

3. Üzerinde **profili**seçin **kullanım koşullarını gözden geçir**. 

4. Kullanım koşullarını gözden geçirebilir, kabul edildi. 

Daha fazla bilgi için [kullanım özelliği (Önizleme) Azure AD kullanım koşulları](https://docs.microsoft.com/azure/active-directory/active-directory-tou).
 
---
 
### <a name="new-azure-ad-sign-in-experience"></a>Yeni Azure AD oturum açma deneyimi

**Türü:** yeni özellik  
**Hizmet kategorisi:** Azure AD  
**Ürün özelliği:** kullanıcı kimlik doğrulaması
 
Azure AD ve Microsoft hesabı kimlik sistemi kullanıcı arabirimleri, tutarlı bir görünüm olduğuna şekilde tasarlanmıştır. Ayrıca, Azure AD oturum açma sayfasının kullanıcı adının ilk olarak, ikinci bir ekranda kimlik bilgisi tarafından izlenen toplar.

Daha fazla bilgi için [yeni Azure AD oturum açma deneyiminin genel önizlemeye sunuldu](https://cloudblogs.microsoft.com/enterprisemobility/2017/08/02/the-new-azure-ad-signin-experience-is-now-in-public-preview/).
 
---
 
### <a name="fewer-sign-in-prompts-a-new-keep-me-signed-in-experience-for-azure-ad-sign-in"></a>Daha az oturum açma istemi: yeni "Oturumumu açık bırak" bir deneyimi için Azure AD oturum açma

**Türü:** yeni özellik  
**Hizmet kategorisi:** Azure AD  
**Ürün özelliği:** kullanıcı kimlik doğrulaması
 
**Oturumumu açık bırak** Azure AD oturum açma sayfasında onay kutusunu başarıyla kimlik doğrulama işleminden sonra yedekleme gösteren yeni bir İstemi ile değiştirildi. 

Yanıt, **Evet** bu istemi için hizmeti, bir sürekli yenileme belirteci sağlar. Bu seçili olduğunda aynı, davranıştır **Oturumumu açık bırak** eski deneyimi onay kutusu. Federasyon Hizmeti ile başarıyla kimlik doğrulama işleminden sonra Federasyon kiracıları için bu istemi gösterir.

Daha fazla bilgi için [daha az oturum açma istemi: yeni "Oturumumu açık bırak" deneyimi için Azure AD Önizleme aşamasındadır](https://cloudblogs.microsoft.com/enterprisemobility/2017/09/19/fewer-login-prompts-the-new-keep-me-signed-in-experience-for-azure-ad-is-in-preview/). 

---

### <a name="add-configuration-to-require-the-terms-of-use-to-be-expanded-prior-to-accepting"></a>Kabul etmeden önce genişletilecek kullanım koşullarını gerektirecek şekilde yapılandırması Ekle

**Türü:** yeni özellik  
**Hizmet kategorisi:** kullanım koşulları  
**Ürün özelliği:** uyumluluk
 
Yöneticiler için bir seçenek, kullanıcıların koşulları kabul etmeden önce kullanım koşullarını genişletmesini gerektirir.

Şunlardan birini seçin **üzerinde** veya **kapalı** kullanıcıların kullanım koşullarını genişletmesini gerekli kıl için. **Üzerinde** ayarı kabul etmeden önce kullanım koşullarını görüntülemek kullanıcıların gerektirir.

Daha fazla bilgi için [kullanım özelliği (Önizleme) Azure AD kullanım koşulları](https://docs.microsoft.com/azure/active-directory/active-directory-tou).
 
---

### <a name="scoped-activation-for-eligible-role-assignments"></a>Uygun rol atamaları için kapsamlı etkinleştirme

**Türü:** yeni özellik  
**Hizmet kategorisi:** Privileged Identity Management  
**Ürün özelliği:** Privileged Identity Management
 
Kapsamlı etkinleştirme, özgün atama varsayılan değerinden daha az bağımsız çalışma sınırı ile uygun bir Azure kaynak rol atamalarını etkinleştirmek için kullanabilirsiniz. Kiracınızda atadığınız bir abonelik sahibi olarak bir örnektir. Kapsamlı etkinleştirme'yle (örneğin, kaynak grupları ve sanal makineler) abonelik kapsamında yer alan en fazla beş kaynakları için sahip rolü etkinleştirebilirsiniz. Aktivasyon kapsamı, kritik Azure kaynakları için istenmeyen değişiklikleri yürütülmeden olasılığını düşürebilir.

Daha fazla bilgi için [Azure AD Privileged Identity Management nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure).
 
---
 
### <a name="new-federated-apps-in-the-azure-ad-app-gallery"></a>Azure AD uygulama Galerisi'nde yeni Federasyon uygulamaları

**Türü:** yeni özellik  
**Hizmet kategorisi:** kurumsal uygulamalar  
**Ürün özelliği:** 3. taraf tümleştirme

Aralık 2017'de, bu yeni Federasyon uygulamalarla uygulama galerimizden desteği ekledik:

[Accredible](https://go.microsoft.com/fwlink/?linkid=863523), Adobe Experience Manager [EFI dijital mağaza](https://go.microsoft.com/fwlink/?linkid=861685), [Communifire](https://go.microsoft.com/fwlink/?linkid=861676) CybSafe, [FactSet](https://go.microsoft.com/fwlink/?linkid=863525), [görüntü WORKS](https://go.microsoft.com/fwlink/?linkid=863517), [MOBI](https://go.microsoft.com/fwlink/?linkid=863521), [MobileIron Azure AD tümleştirme](https://go.microsoft.com/fwlink/?linkid=858027), [Reflektive](https://go.microsoft.com/fwlink/?linkid=863518), [GmbH çözünürlüğün Bamboo için SAML SSO](https://go.microsoft.com/fwlink/?linkid=863520), [GmbH çözünürlüğün Bitbucket için SAML SSO](https://go.microsoft.com/fwlink/?linkid=863519), [Vodeclic](https://go.microsoft.com/fwlink/?linkid=863522), WebHR, Zenegy Azure AD tümleştirmesi.

Uygulamalar hakkında daha fazla bilgi için bkz. [Azure Active Directory ile SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial).

Azure AD uygulama galerisinde uygulamanızı listeleme hakkında daha fazla bilgi için bkz. [uygulamanızı Azure Active Directory Uygulama galerisinde listeleyin](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing). 
 
---

### <a name="approval-workflows-for-azure-ad-directory-roles"></a>Azure AD Dizin rolleri için onay iş akışları

**Türü:** değiştirilen özellik  
**Hizmet kategorisi:** Privileged Identity Management  
**Ürün özelliği:** Privileged Identity Management
 
Azure AD Dizin rolleri için onay iş akışı genel kullanıma sunulmuştur.

Ayrıcalıklı rol kullanabilmeniz için önce onay iş akışı ile ayrıcalıklı rol yöneticileri rol etkinleştirme isteği uygun rolü üyeleri gerektirebilir. Birden çok kullanıcı ve grupları temsilcisi onay sorumlulukları olabilir. Uygun Rol üyeleri onayı tamamlandı ve rollerine etkin olduğunda bildirim alırsınız.

---
 
### <a name="pass-through-authentication-skype-for-business-support"></a>Geçişli kimlik doğrulaması: Skype for Business desteği

**Türü:** değiştirilen özellik  
**Hizmet kategorisi:** kimlik doğrulamaları (oturum açma bilgileri)  
**Ürün özelliği:** kullanıcı kimlik doğrulaması

Geçişli kimlik doğrulaması artık kullanıcı oturum açma işlemleri Skype Kurumsal çevrimiçi içeren modern kimlik doğrulamasını destekleyen iş istemci uygulamalarını ve karma topolojiler için destekler. 

Daha fazla bilgi için [Skype için modern kimlik doğrulaması ile desteklenen iş topolojiler](https://technet.microsoft.com/library/mt803262.aspx).
 
---

### <a name="updates-to-azure-ad-privileged-identity-management-for-azure-rbac-preview"></a>Azure RBAC (Önizleme) için Azure AD Privileged Identity Management güncelleştirmeleri

**Türü:** değiştirilen özellik  
**Hizmet kategorisi:** Privileged Identity Management  
**Ürün özelliği:** Privileged Identity Management
 
Azure AD Privileged Identity Management (PIM) Azure rol tabanlı Access Control (RBAC) için genel Önizleme yenileme ile şunları yapabilirsiniz:

* Yeterli yönetim kullanın.
* Kaynak rolleri etkinleştirmek için onay gerektirir.
* Azure RBAC rolleri ile hem Azure AD için onay gerektiren bir rolü ve gelecekteki bir etkinleştirme zamanlayın.
 
Daha fazla bilgi için [(Önizleme) Azure kaynakları için Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac).

---
 
## <a name="november-2017"></a>Kasım 2017
 
### <a name="access-control-service-retirement"></a>Erişim denetimi hizmeti devre dışı bırakma

**Türü:** değişiklik planı  
**Hizmet kategorisi:** erişim denetimi hizmeti  
**Ürün özelliği:** erişim denetimi hizmeti 

Azure Active Directory erişim denetimi (erişim denetimi hizmeti olarak da bilinir) geç 2018'de kullanımdan kaldırılacaktır. Önümüzdeki birkaç hafta içinde ayrıntılı zamanlama ve üst düzey geçiş kılavuzunu içeren daha fazla bilgi sağlanacaktır. Erişim denetimi hizmeti hakkında sorular ile bu sayfada açıklamaları bırakabilirsiniz ve bir takım üyesi yanıt.

---

### <a name="restrict-browser-access-to-the-intune-managed-browser"></a>Intune Managed Browser için tarayıcı erişimi kısıtlama 

**Türü:** değişiklik planı  
**Hizmet kategorisi:** koşullu erişim  
**Ürün özelliği:** kimlik güvenliği ve koruması

Onaylanmış bir uygulama Intune Managed Browser'ı kullanarak, Office 365 ve diğer Azure AD bağlantılı bulut uygulamaları için tarayıcı erişimini kısıtlayabilirsiniz. 

Artık uygulama tabanlı koşullu erişim için aşağıdaki koşul yapılandırabilirsiniz:

**İstemci uygulamaları:** tarayıcı

**Değişikliğin etkilerini nedir?**

Günümüzde bu durum kullandığınızda erişim engellenir. Önizleme mevcut değil, tüm erişim yönetilen tarayıcı uygulaması kullanımını gerektirir. 

Bu özellik ve yaklaşan blogları ve sürüm notları hakkında daha fazla bilgi arayın. 

Daha fazla bilgi için [koşullu erişim, Azure AD'de](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal).
 
---

### <a name="new-approved-client-apps-for-azure-ad-app-based-conditional-access"></a>Azure AD uygulama tabanlı koşullu erişim için yeni onaylı istemci uygulamaları

**Türü:** değişiklik planı  
**Hizmet kategorisi:** koşullu erişim  
**Ürün özelliği:** kimlik güvenliği ve koruması

Aşağıdaki uygulamalar listede yer [onaylı istemci uygulamalar](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement):

- [Microsoft Kaizala](https://www.microsoft.com/garage/profiles/kaizala/)
- [Microsoft StaffHub](https://staffhub.office.com/what-it-is)

Daha fazla bilgi için bkz.

- [Onaylı istemci uygulama gereksinimi](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement)
- [Azure AD uygulama tabanlı koşullu erişim](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)

---

### <a name="terms-of-use-support-for-multiple-languages"></a>Kullanım koşulları birden çok dil desteği

**Türü:** yeni özellik    
**Hizmet kategorisi:** kullanım koşulları  
**Ürün özelliği:** uyumluluk

Yöneticiler artık birden çok PDF belgeleri içeren yeni kullanım koşulları oluşturabilirsiniz. Bu PDF belgeleri karşılık gelen bir dil ile etiketleyebilirsiniz. Kullanıcılara, PDF, eşleşen dil tercihlerine göre ile gösterilir. Eşleşme yoksa varsayılan dilde gösterilir.

---
 

### <a name="real-time-password-writeback-client-status"></a>Gerçek zamanlı parola geri yazma istemcinizin durumunu

**Türü:** yeni özellik  
**Hizmet kategorisi:** Self Servis parola sıfırlama  
**Ürün özelliği:** kullanıcı kimlik doğrulaması

Artık, şirket içi parola geri yazma istemcinizin durumunu gözden geçirebilirsiniz. Bu seçenek kullanılabilir **şirket içi tümleştirme** bölümünü [parola sıfırlama](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/PasswordReset) sayfası. 

Şirket içi geri yazma istemciniz için bağlantınızda bir sorun varsa, size sağlayan bir hata iletisi görürsünüz:

- Şirket içi geri yazma istemcinize neden bağlanamıyorsunuz bilgi.
- Sorunun çözümlenmesinde yardımcı belgeleri bağlantısı. 

Daha fazla bilgi için [şirket tümleştirme](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-how-it-works#on-premises-integration).

---

### <a name="azure-ad-app-based-conditional-access"></a>Azure AD uygulama tabanlı koşullu erişim 
 
**Türü:** yeni özellik  
**Hizmet kategorisi:** Azure AD  
**Ürün özelliği:** kimlik güvenliği ve koruması

Artık Office 365 ve diğer Azure AD bağlantılı bulut uygulamaları için erişimi kısıtlayabilirsiniz [onaylı istemci uygulamalar](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement) kullanarak Intune uygulama koruma ilkelerini destekleyen [Azure AD uygulama tabanlı koşullu erişim](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access). Intune uygulama koruma ilkelerini yapılandırma ve bu istemci uygulamalarını üzerindeki şirket verilerini korumak için kullanılır.

Birleştirme tarafından [uygulama tabanlı](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access) ile [cihaz tabanlı](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-policy-connected-applications) koşullu erişim ilkeleri, kişisel verileri ve şirket cihazları korumak için esnekliğe sahip.

Aşağıdaki koşullar ve denetimleri artık uygulama tabanlı koşullu erişim ile kullanılmak üzere mevcuttur:

**Desteklenen platform koşulu**

- iOS
- Android

**İstemci uygulamaları koşulu**

- Mobil uygulamalar ve masaüstü istemcileri

**Erişim denetimi**

- Onaylı istemci uygulaması gerektir

Daha fazla bilgi için [Azure AD uygulama tabanlı koşullu erişim](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access).
 
---

### <a name="manage-azure-ad-devices-in-the-azure-portal"></a>Azure Portal'da Azure AD cihazları yönetme

**Türü:** yeni özellik  
**Hizmet kategorisi:** cihaz kaydı ve Yönetimi  
**Ürün özelliği:** kimlik güvenliği ve koruması

Artık tüm cihazlarınızı Azure AD'ye bağlı bulabilirsiniz ve cihaz ile ilgili etkinlikleri tek bir yerde. Tüm cihaz kimliklerini ve Azure Portalı'ndaki ayarları yönetmek için yeni bir yönetim deneyimi vardır. Bu sürümde, şunları yapabilirsiniz:

- Azure AD'de koşullu erişim için kullanılabilir olan tüm cihazlarınızı görüntüleyin.
- Azure hibrit içeren özelliklerini görüntüleme AD alanına katılmış cihazlar.
- Azure AD'ye katılmış cihazlar için BitLocker anahtarlarını bulmak, Cihazınızı ve Intune ile yönetme.
- Azure AD cihaz ile ilgili ayarları yönetin.

Daha fazla bilgi için [Azure portalını kullanarak cihazları yönetme](https://docs.microsoft.com/azure/active-directory/device-management-azure-portal).

---

### <a name="support-for-macos-as-a-device-platform-for-azure-ad-conditional-access"></a>MacOS cihaz platformu için Azure AD koşullu erişim desteği 

**Türü:** yeni özellik    
**Hizmet kategorisi:** koşullu erişim  
**Ürün özelliği:** kimlik güvenliği ve koruması 

Artık içerir (hariç macOS cihaz platformu koşul olarak Azure AD koşullu erişim ilkenizi veya istediğiniz). Desteklenen cihaz platformları için macOS'ın eklenmesiyle, şunları yapabilirsiniz:

- **Kaydetme ve Intune kullanarak macOS cihazlarını yönetin.** Benzer şekilde, iOS ve Android gibi diğer platformlarda, Şirket portalı uygulamasını birleşik kayıtları yapmak için macOS için kullanılabilir. Yeni Şirket portalı uygulaması macOS için Intune ile cihaz kaydetme ve Azure AD ile kaydetmek için kullanabilirsiniz.
- **MacOS cihazları Intune'da tanımlanan kuruluşunuzun uyumluluk ilkelerine uymaları emin olun.** Azure portalında Intune'da artık macOS cihazları için Uyumluluk ilkelerini ayarlayabilirsiniz. 
- **Yalnızca uyumlu macOS cihazlar için Azure AD'de, uygulamalara erişimi kısıtlayın.** MacOS koşullu erişim ilkesi yazma ayrı cihaz platformu seçeneği olarak sahiptir. Artık Azure'da ayarlanan hedeflenen uygulama için macOS özel koşullu erişim ilkeleri yazabilirsiniz.

Daha fazla bilgi için bkz.

- [Intune ile macOS cihazları için cihaz uyumluluğu ilkesi oluşturma](https://aka.ms/macoscompliancepolicy)
- [Azure AD'de koşullu erişim](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)
 
---

### <a name="network-policy-server-extension-for-azure-multi-factor-authentication"></a>Azure multi-Factor Authentication için ağ ilkesi sunucusu uzantısı 

**Türü:** yeni özellik    
**Hizmet kategorisi:** çok faktörlü kimlik doğrulaması  
**Ürün özelliği:** kullanıcı kimlik doğrulaması

Azure multi-Factor Authentication için ağ ilkesi sunucusu uzantısı, mevcut sunucularınızda kullanarak bulut tabanlı çok faktörlü kimlik doğrulama özellikleri kimlik doğrulama altyapınızı ekler. Ağ İlkesi Sunucusu uzantısı ile mevcut kimlik doğrulama akışınıza telefon araması, SMS mesajı ve telefon uygulaması doğrulama ekleyebilirsiniz. Yükleme, yapılandırma ve yeni sunuculara bakım gerekmez. 

Bu uzantı, sanal özel ağ bağlantılarıyla Azure multi-Factor Authentication Sunucusu'nu dağıtmadan korumak istediğiniz kuruluşlar için oluşturuldu. Ağ ilkesi sunucusu arasında RADIUS ve Azure multi-Factor Authentication'ı bulut tabanlı bir ikinci faktör kimlik doğrulaması sağlamak için bir bağdaştırıcı uzantısı görür federasyona eklenenler veya eşitlenmiş kullanıcıları.


Daha fazla bilgi için [mevcut ağ ilkesi sunucusu altyapınızı Azure multi-Factor Authentication ile tümleştirme](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-nps-extension).

 
---

### <a name="restore-or-permanently-remove-deleted-users"></a>Geri yükleme ya da kalıcı olarak silinen Kullanıcıları Kaldır

**Türü:** yeni özellik    
**Hizmet kategorisi:** kullanıcı yönetimi  
**Ürün özelliği:** dizini 


Azure AD Yönetim merkezinde artık şunları yapabilirsiniz:

- Silinen bir kullanıcıyı geri yükleyin. 
- Kullanıcı kalıcı olarak siler.


**Denemek için:**

1. Azure AD Yönetim merkezinde seçin [tüm kullanıcılar](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/All) içinde **Yönet** bölümü. 

2. Gelen **Göster** listesinden **kullanıcılar'yakın zamanda silinip**. 

3. Yakın zamanda silinen kullanıcılar, bir veya daha fazla seçin ve ardından ya da geri yükleyebilir veya kalıcı olarak silebilirsiniz.

 
---

### <a name="new-approved-client-apps-for-azure-ad-app-based-conditional-access"></a>Azure AD uygulama tabanlı koşullu erişim için yeni onaylı istemci uygulamaları

 
**Türü:** değiştirilen özellik  
**Hizmet kategorisi:** koşullu erişim  
**Ürün özelliği:** kimlik güvenliği ve koruması


Aşağıdaki uygulamalar listesine eklenmiş olmasından [onaylı istemci uygulamalar](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement):

- Microsoft Planner
- Azure Information Protection 


Daha fazla bilgi için bkz.

- [Onaylı istemci uygulama gereksinimi](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement)
- [Azure AD uygulama tabanlı koşullu erişim](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)


---

### <a name="use-or-between-controls-in-a-conditional-access-policy"></a>Kullanım "Veya" arasında bir koşullu erişim ilkesi denetimleri 


**Türü:** değiştirilen özellik    
**Hizmet kategorisi:** koşullu erişim  
**Ürün özelliği:** kimlik güvenliği ve koruması

 
Artık kullanabilir "veya" (Seçili denetimlerden birini gerektir) için koşullu erişim denetimleri. "Veya" arasında erişim denetim ilkeleri oluşturmak için bu özelliği kullanabilirsiniz. Örneğin, bir kullanıcı çok faktörlü kimlik doğrulaması kullanarak oturum açın "veya" uyumlu bir cihaz üzerinde olmasını gerektiren bir ilke oluşturmak için bu özelliği kullanabilirsiniz.

Daha fazla bilgi için [Azure AD koşullu erişim denetimleri](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-controls).

 
---

### <a name="aggregation-of-real-time-risk-events"></a>Gerçek zamanlı risk olaylarını toplama


**Türü:** değiştirilen özellik    
**Hizmet kategorisi:** kimlik koruması  
**Ürün özelliği:** kimlik güvenliği ve koruması


Azure AD kimlik koruması belirli bir gün aynı IP adresinden kaynaklanan tüm gerçek zamanlı risk olayları artık her bir risk olayı türü için toplanır. Bu değişiklik, risk olaylarının herhangi bir değişiklik kullanıcı güvenlik gösterilen birim sınırlar.

Temel alınan gerçek zamanlı algılama, kullanıcı oturum açtığı her zaman çalışır. Çok faktörlü kimlik doğrulaması veya erişimi engelleme için ayarlanmış bir oturum açma riski ilkesi varsa, yine de her riskli oturum açma sırasında tetiklenir.

 
---
 




## <a name="october-2017"></a>Ekim 2017


### <a name="deprecate-azure-ad-reports"></a>Azure AD raporlar kullanımdan kaldırma


**Türü:** değişiklik planı  
**Hizmet kategorisi:** raporlama  
**Ürün özelliği:** kimlik yaşam döngüsü yönetimi  



Azure portalı ile sağlar:

- Yeni bir Azure AD Yönetim Konsolu.
- Yeni API'ler için etkinlik ve güvenlik raporları.
 
Bu yeni özellikler nedeniyle/Reports uç nokta altında API'ler rapor kullanımdan kaldırılmıştır 10 Aralık 2017'de. 

---

### <a name="automatic-sign-in-field-detection"></a>Otomatik oturum açma alanı algılaması


**Türü:** düzeltildi   
**Hizmet kategorisi:** uygulamalarım  
**Ürün özelliği:** çoklu oturum açma  



Azure AD, bir HTML kullanıcı adı ve parola alanı işlemek uygulamalar için otomatik oturum açma alanı algılaması destekler. Bu adımları bölümünde belgelendirilen [nasıl otomatik olarak bir uygulama için oturum açma alanlarını Yakala](https://docs.microsoft.com/azure/active-directory/application-config-sso-problem-configure-password-sso-non-gallery#how-to-manually-capture-sign-in-fields-for-an-application). Bu özellik ekleyerek bulabilirsiniz bir *galeri dışı* uygulaması **kurumsal uygulamalar** sayfasını [Azure portalında](http://aad.portal.azure.com). Buna ek olarak, yapılandırabileceğiniz **çoklu oturum açma** bu yeni uygulama modu **parola tabanlı çoklu oturum açma**web URL'si girin ve ardından sayfanın kaydedin.
 
Bir hizmet sorunu nedeniyle bu işlevselliği geçici olarak devre dışı bırakıldı. Sorunun çözülüp çözülmediğini ve otomatik oturum açma alanı algılaması yeniden kullanılabilir.

---

### <a name="new-multi-factor-authentication-features"></a>Yeni çok faktörlü kimlik doğrulaması özellikleri


**Türü:** yeni özellik  
**Hizmet kategorisi:** çok faktörlü kimlik doğrulaması  
**Ürün özelliği:** kimlik güvenliği ve koruması  



Çok faktörlü kimlik doğrulaması (MFA), kuruluşunuzun koruma önemli bir parçasıdır. Kimlik bilgileri daha Uyarlamalı ve deneyimi daha sorunsuz hale getirmek için aşağıdaki özellikleri eklendi: 

- Çok faktörlü sınama sonuçları, MFA sonuçları programlı erişim içeren Azure AD oturum açma raporu doğrudan bütünleştirilmiştir.
- MFA yapılandırmanın daha derin bir şekilde Azure AD'ye yapılandırmaya tümleşik deneyimi Azure Portalı'nda.

Bu genel Önizleme ile MFA yönetim ve raporlama çekirdek Azure AD'ye yapılandırma deneyimi tümleşik bir parçası olan. Artık Azure AD deneyimi içinde MFA Yönetim Portalı işlevlerini yönetebilirsiniz.

Daha fazla bilgi için [MFA Azure Portalı'nda raporlama için başvuru](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-sign-ins-mfa). 


---

### <a name="terms-of-use"></a>Kullanım koşulları



**Türü:** yeni özellik  
**Hizmet kategorisi:** kullanım koşulları  
**Ürün özelliği:** uyumluluk  



Azure AD kullanım koşulları yasal gereksinimler veya uyumluluk gereksinimleriyle ilgili bildirimleri gibi bilgiler sunmak için kullanabilirsiniz.

Azure AD kullanım koşulları aşağıdaki senaryolarda kullanabilirsiniz:

- Genel, kuruluşunuzdaki tüm kullanıcılar için kullanım koşulları
- Belirli kullanıcı özniteliklerine (doktorlarla nurses karşılaştırması gibi) ya da yurtiçi ve uluslararası çalışanlara, dinamik gruplar tarafından yapılan temel kullanım koşulları
- Salesforce gibi yüksek etkili iş uygulamaları erişmek için belirli kullanım koşulları

Daha fazla bilgi için [Azure AD kullanım koşulları](https://docs.microsoft.com/azure/active-directory/active-directory-tou).


---

### <a name="enhancements-to-privileged-identity-management"></a>Privileged Identity Management geliştirmeleri


**Türü:** yeni özellik  
**Hizmet kategorisi:** Privileged Identity Management  
**Ürün özelliği:** Privileged Identity Management  


Azure AD Privileged Identity Management ile yönetebilir, denetleyebilir ve kuruluşunuz içinde (Önizleme) Azure kaynaklarına erişimi izleyin:

- Abonelikler
- Kaynak grupları
- Sanal makineler 

Azure RBAC işlevselliği kullandığı tüm kaynakları Azure portalındaki güvenlik ve Azure AD Privileged Identity Management sunduğu yaşam döngüsü yönetim özellikleri yararlanabilirsiniz.

Daha fazla bilgi için [Azure kaynakları için Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac).


---

### <a name="access-reviews"></a>Erişim gözden geçirmeleri


**Türü:** yeni özellik  
**Hizmet kategorisi:** erişim gözden geçirmeleri  
**Ürün özelliği:** uyumluluk  



Kuruluşların grup üyeliklerini ve kurumsal uygulamalara erişimi etkili bir şekilde yönetmek için erişim gözden geçirmeleri (Önizleme) kullanabilirsiniz: 

- Konuk kullanıcıların uygulamalara ve grup üyeliklerine erişimlerine ait erişim gözden geçirmelerini kullanarak bu kullanıcıların erişimini yeniden onaylayabilirsiniz. Gözden geçirenler, verimli bir şekilde erişim gözden geçirmeleri tarafından sağlanan bilgiler dayalı olarak erişimi Konukları izin vermek isteyip devam karar verebilirsiniz.
- Erişim gözden geçirmeleri ile çalışanların uygulamalara erişimini ve grup üyeliklerini yeniden onaylayabilirsiniz.

Erişim gözden geçirmesi denetimlerini kuruluşunuza uygun programlarda toplayarak, uyumluluk veya riske duyarlı uygulamalar için gözden geçirmeleri takip edebilirsiniz.

Daha fazla bilgi için [Azure AD erişim gözden geçirmeleri](https://docs.microsoft.com/azure/active-directory/active-directory-azure-ad-controls-access-reviews-overview).


---

### <a name="hide-third-party-applications-from-my-apps-and-the-office-365-app-launcher"></a>Üçüncü taraf uygulamalardan uygulamalarım ve Office 365 uygulama başlatıcısında Gizle



**Türü:** yeni özellik  
**Hizmet kategorisi:** uygulamalarım  
**Ürün özelliği:** çoklu oturum açma  



Şimdi yeni bir aracılığıyla kullanıcılarınızın portalı üzerinde görünen uygulamalar daha iyi yönetebilirsiniz **uygulama Gizle** özelliği. Arka uç hizmetlerine veya yinelenen kutucukları ve dağınıklık kullanıcıların uygulama launchers uygulama kutucuklarına burada görünmesi durumlarda yardımcı olması için uygulamanızı gizleyebilirsiniz. İki durumlu düğme bulunduğu **özellikleri** üçüncü taraf uygulama bölümünü ve etiketli **kullanıcıya görünür?** Ayrıca PowerShell üzerinden program aracılığıyla uygulama gizleyebilirsiniz. 

Daha fazla bilgi için [üçüncü taraf bir uygulamanın Azure AD'de kullanıcı deneyiminden gizleme](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-hide-third-party-app). 


**Kullanılabilir nedir?**

 Yeni Yönetici Konsolu, iki yeni API geçişi kapsamında günlüklerine Azure AD etkinlik almak için kullanılabilir. Yeni API'leri daha zengin filtreleme ve sıralama daha zengin denetim ve oturum açma etkinlikleri sağlamaya ek olarak işlevselliği sunmaktadır. Artık güvenlik raporları ile önceden kullanılabilen veri, Microsoft Graph kimlik koruma Risk olayları API aracılığıyla erişilebilir.


## <a name="september-2017"></a>Eylül 2017

### <a name="hotfix-for-identity-manager"></a>Identity Manager için düzeltme


**Türü:** değiştirilen özellik  
**Hizmet kategorisi:** Identity Manager  
**Ürün özelliği:** kimlik yaşam döngüsü yönetimi  



Bir düzeltme dökümü paket (derleme 4.4.1642.0), 25 Eylül 2017 Identity Manager 2016 Service Pack 1'den itibaren kullanılabilir. Bu döküm paketi:

- Sorunları giderir ve geliştirmeleri içerir.
- Identity Manager 2016 için derleme 4.4.1459.0 kadar tüm Identity Manager 2016 Service Pack 1 güncelleştirmeleri değiştirir toplu bir güncelleştirmesidir. 
- Identity Manager 2016 4.4.1302.0 derleme olmasını gerektirir. 

Daha fazla bilgi için [Identity Manager 2016 Service Pack 1 için düzeltme paketi (derleme 4.4.1642.0) kullanılabilir](https://support.microsoft.com/help/4021562). 

---
