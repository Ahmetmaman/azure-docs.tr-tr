---
title: Yenilikler Sürüm notları-Azure Active Directory | Microsoft Docs
description: Azure Active Directory yenilikleri öğrenin; en son sürüm notları, bilinen sorunlar, hata düzeltmeleri, kullanım dışı işlevsellik ve yaklaşan değişiklikler gibi.
services: active-directory
author: ajburnle
manager: daveba
featureFlags:
- clicktale
ms.assetid: 06a149f7-4aa1-4fb9-a8ec-ac2633b031fb
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 10/06/2020
ms.author: ajburnle
ms.reviewer: dhanyahk
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6b076c757c8f86941c79da4f0be598aaa5ea6761
ms.sourcegitcommit: ce8eecb3e966c08ae368fafb69eaeb00e76da57e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/21/2020
ms.locfileid: "92317955"
---
# <a name="whats-new-in-azure-active-directory"></a>Azure Active Directory yenilikler nelerdir?

>Bu URL 'YI kopyalayarak ve yapıştırarak bu sayfanın ne zaman yeniden ziyaret ettikleridir hakkında bildirim alın: `https://docs.microsoft.com/api/search/rss?search=%22Release+notes+-+Azure+Active+Directory%22&locale=en-us` ![ RSS akışı okuyucu simgesi ](./media/whats-new/feed-icon-16x16.png) akış okuyucusuna.

Azure AD, iyileştirmeleri sürekli olarak alır. En son gelişmelerden haberdar olmak için, bu makalede hakkında bilgi verilmektedir:

- En son yayınlar
- Bilinen sorunlar
- Hata düzeltmeleri
- Kullanım dışı işlevsellik
- Değişiklik planları

Bu sayfa aylık olarak güncelleştirildiğinden düzenli olarak yeniden ziyaret edin. Altı aydan eski olan öğeleri arıyorsanız, [Azure Active Directory yenilikleri Için arşiv](whats-new-archive.md)halinde bulabilirsiniz.

---

## <a name="september-2020"></a>Eylül 2020

### <a name="new-provisioning-connectors-in-the-azure-ad-application-gallery---september-2020"></a>Azure AD uygulama galerisinde yeni sağlama bağlayıcıları-Eylül 2020

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Uygulama sağlama  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi
 
Artık yeni tümleştirilmiş uygulamalar için Kullanıcı hesapları oluşturmayı, güncellemeyi ve silmeyi otomatik hale getirebilirsiniz:

- [Coda](../saas-apps/coda-provisioning-tutorial.md)
- [Cofense Alıcı Eşitleme](../saas-apps/cofense-provision-tutorial.md)
- [InVision](../saas-apps/invision-provisioning-tutorial.md)
- [myday](../saas-apps/myday-provision-tutorial.md)
- [SAP Analytics Cloud](../saas-apps/sap-analytics-cloud-provisioning-tutorial.md)
- [Webroot güvenlik tanıma](../saas-apps/webroot-security-awareness-training-provisioning-tutorial.md)

Otomatik Kullanıcı hesabı sağlamayı kullanarak kuruluşunuzun daha iyi güvenliğini sağlama hakkında daha fazla bilgi için bkz. [Azure AD Ile SaaS uygulamalarına Kullanıcı sağlamayı otomatikleştirme](../app-provisioning/user-provisioning.md).
 
---
### <a name="cloud-provisioning-public-preview-refresh"></a>Bulut sağlama genel önizlemeyi yenileme

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Azure AD bulut sağlama **ürün yeteneği:** kimlik yaşam döngüsü yönetimi
 
Azure AD Connect bulut sağlama genel önizleme yenileme özellikleri müşteri geri bildirimlerinden geliştirilen iki önemli geliştirmelere sahiptir: 

- Azure Portal aracılığıyla öznitelik eşleme deneyimi

    Bu özellikle, BT yöneticileri Kullanıcı, Grup veya iletişim özniteliklerini AD 'den Azure AD 'ye eşleyerek bugün mevcut çeşitli eşleme türlerini kullanarak eşleyebilirsiniz. Öznitelik eşleme, Active Directory ' den Azure Active Directory akan özniteliklerin değerlerini standartlaştırarak kullanılan bir özelliktir. Bunlardan biri, AD 'den Azure AD 'ye ait olan öznitelik değerinin doğrudan eşlenip eşlenmeyeceğini belirleyebilir veya kullanıcıları sağlarken öznitelik değerlerini dönüştürmek için ifadeler kullanın. [Daha fazla bilgi edinin](../cloud-provisioning/how-to-attribute-mapping.md)

- İsteğe bağlı sağlama veya test Kullanıcı deneyimi

    Yapılandırmanızı ayarladıktan sonra, Kullanıcı dönüşümünün kapsamdaki tüm kullanıcılarınıza uygulamadan önce beklendiği gibi çalışıp çalışmadığını görmek için test etmek isteyebilirsiniz. İsteğe bağlı sağlama sayesinde, BT yöneticileri bir AD kullanıcısının ayırt edici adını (DN) girebilir ve beklenen şekilde eşitlenip eşitlenmediğini görebilirler. İsteğe bağlı sağlama, daha önce beklenen şekilde çalışan öznitelik eşleştirmelerinin sağlanması için harika bir yol sağlar. [Daha Fazla Bilgi](../cloud-provisioning/how-to-on-demand-provision.md)
 
---

### <a name="audited-bitlocker-recovery-in-azure-ad---public-preview"></a>Azure AD 'de denetlenen BitLocker kurtarma-genel önizleme

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Cihaz erişim yönetimi  
**Ürün yeteneği:** Cihaz yaşam döngüsü yönetimi
 
BT yöneticileri veya son kullanıcılar erişimi olan BitLocker kurtarma anahtarlarını okuduklarında, Azure Active Directory artık kurtarma anahtarına kimin eriştiğini yakalayan bir denetim günlüğü oluşturur. Aynı denetim, BitLocker anahtarının ilişkilendirildiği cihazın ayrıntılarını sağlar.

Son kullanıcılar [, kendi Kurtarma anahtarlarına Hesabım aracılığıyla erişebilirler](../user-help/my-account-portal-devices-page.md#view-a-bitlocker-key). BT yöneticileri, [Beta 'Daki BitLocker kurtarma anahtarı API 'si](/graph/api/resources/bitlockerrecoverykey?view=graph-rest-beta) veya Azure AD portalı aracılığıyla Kurtarma anahtarlarına erişebilir. Daha fazla bilgi için bkz. [Azure AD portalındaki BitLocker anahtarlarını görüntüleme veya kopyalama](../devices/device-management-azure-portal.md#view-or-copy-bitlocker-keys).

---

### <a name="teams-devices-administrator-built-in-role"></a>Ekip cihazları yönetici yerleşik rolü

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** RAPORDAKI  
**Ürün yeteneği:** Access Control
 
[Takımlar cihazları yönetici](../users-groups-roles/directory-assign-admin-roles.md#teams-devices-administrator) rolüne sahip kullanıcılar takımlar yönetim merkezinden [ekipten sertifikalı cihazları](https://www.microsoft.com/microsoft-365/microsoft-teams/across-devices/devices) yönetebilir. 

Bu rol, kullanıcının tüm cihazları tek bakışta görüntülemesine olanak tanır ve bu sayede cihazları arayabilir ve filtreleyebilirsiniz. Kullanıcı oturum açma hesabı ve cihazın marka ve modeli dahil olmak üzere her bir cihazın ayrıntılarını da denetleyebilir. Kullanıcı cihazdaki ayarları değiştirebilir ve yazılım sürümlerini güncelleştirebilir. Bu rol, ekip etkinliklerini denetlemek ve cihazın kalitesini çağırmak için izinler vermez.
 
---

### <a name="advanced-query-capabilities-for-directory-objects"></a>Dizin nesneleri için Gelişmiş sorgu özellikleri

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** MS grafiği  
**Ürün yeteneği:** Geliştirici deneyimi
 
Azure AD API 'Lerinde dizin nesneleri için sunulan tüm yeni sorgu özellikleri artık v 1.0 uç noktasında ve üretime hazır olarak sunulmaktadır. Geliştiriciler, Standart OData işleçlerini kullanarak Dizin nesnelerini ve ilgili bağlantıları sayabilir, arayabilir, filtreleyip sıralayabilir.

Daha fazla bilgi edinmek için [buradaki](https://aka.ms/BlogPostMezzoGA)belgelere bakın ve bu [kısa anketle](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR_yN8EPoGo5OpR1hgmCp1XxUMENJRkNQTk5RQkpWTE44NEk2U0RIV0VZRy4u)de geri bildirim gönderebilirsiniz.
 
---

### <a name="public-preview-continuous-access-evaluation-for-tenants-who-configured-conditional-access-policies"></a>Genel Önizleme: koşullu erişim ilkelerini yapılandıran kiracılar için sürekli erişim değerlendirmesi

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kimlik doğrulamaları (oturum açma)  
**Ürün yeteneği:** Kimlik güvenliği & koruması
 
Artık sürekli erişim değerlendirmesi (CAE), koşullu erişim ilkelerine sahip Azure AD kiracılar için genel önizlemede kullanıma sunulmuştur. CAE ile kritik güvenlik olayları ve ilkeleri gerçek zamanlı olarak değerlendirilir. Buna hesap devre dışı bırakma, parola sıfırlama ve konum değişikliği dahildir. Daha fazla bilgi için bkz. [sürekli erişim değerlendirmesi](../conditional-access/concept-continuous-access-evaluation.md).

---

### <a name="public-preview-ask-users-requesting-an-access-package-additional-questions-to-improve-approval-decisions"></a>Genel Önizleme: kullanıcılardan bir erişim paketi talep etmesini isteyin onay kararlarını artırmak için ek sorular

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kullanıcı erişim yönetimi  
**Ürün yeteneği:** Yetkilendirme Yönetimi
 
Yöneticiler artık bir erişim paketi isteyen kullanıcıların, Azure AD Yetkilendirme Yönetimi 'nin erişim portalından yalnızca iş gerekçesinin ötesinde ek sorulara yanıt yanıtlamasını gerektirebilir. Daha sonra bu kullanıcıların yanıtları daha doğru bir erişim onayı kararı vermek için onaylayanlara gösterilir. Daha fazla bilgi edinmek için bkz. [onay için ek istek sahibi bilgileri toplama (Önizleme)](../governance/entitlement-management-access-package-approval-policy.md#collect-additional-requestor-information-for-approval-preview).
 
---

### <a name="public-preview-enhanced-user-management"></a>Genel Önizleme: Gelişmiş Kullanıcı Yönetimi

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kullanıcı Yönetimi  
**Ürün yeteneği:** Kullanıcı Yönetimi
 

Azure AD portalı, tüm kullanıcılar ve silinen kullanıcılar sayfalarındaki kullanıcıları bulmayı kolaylaştıracak şekilde güncelleştirilmiştir. Önizlemedeki değişiklikler şunları içerir: 
- Nesne KIMLIĞI, Dizin eşitleme durumu, oluşturma türü ve kimlik veren gibi daha görünür Kullanıcı özellikleri.
- Arama şimdi ad, e-posta ve nesne kimliklerinin birleştirilmiş aramasına izin veriyor.
- Kullanıcı türüne göre Gelişmiş filtreleme (üye, konuk ve yok), Dizin eşitleme durumu, oluşturma türü, şirket adı ve etki alanı adı.
- Ad, Kullanıcı asıl adı ve silinme tarihi gibi özelliklerde yeni sıralama özellikleri.
- Yeni bir Toplam Kullanıcı sayısı, tüm aramalarla veya filtrelerle güncelleştirilir.

Daha fazla bilgi için lütfen [Azure Active Directory Içindeki Kullanıcı yönetimi geliştirmeleri (Önizleme)](../users-groups-roles/users-search-enhanced.md)bölümüne bakın.

---

### <a name="new-notes-field-for-enterprise-applications"></a>Kurumsal uygulamalar için yeni notlar alanı

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar **ürün yeteneği:** SSO

Kurumsal uygulamalara ücretsiz metin notları ekleyebilirsiniz. Kurumsal uygulamalar altındaki uygulamaları yönetici yapmanıza yardımcı olacak ilgili bilgileri ekleyebilirsiniz. Daha fazla bilgi için bkz. [hızlı başlangıç: Azure Active Directory (Azure AD) kiracınızdaki bir uygulamanın özelliklerini yapılandırma](../manage-apps/add-application-portal-configure.md). 

---

### <a name="new-federated-apps-available-in-azure-ad-application-gallery---september-2020"></a>Azure AD uygulama galerisinde yeni Federasyon uygulamaları kullanılabilir-Eylül 2020

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi

Eylül 2020 ' de, federasyon desteğiyle App Gallery 'de aşağıdaki 34 yeni uygulamaları ekledik:

[VMware ufuk-birleştirilmiş erişim ağ geçidi](), [Pulse Secure PC](../saas-apps/vmware-horizon-unified-access-gateway-tutorial.md), [Inventory360](../saas-apps/pulse-secure-pcs-tutorial.md), [innetleştirilmiş](https://services.enteksystems.de/sso/microsoft/signup), [bookpencere](https://www.bookwidgets.com/sso/office365), [ZVD_Server](https://zaas.zenmutech.com/user/signin), [Iş için HashData](https://hashdata.app/login.xhtml), [securelogin](https://securelogin.securelogin.nu/sso/azure/login), [cybersolutions MAILBASEΣ/CMSS](../saas-apps/cybersolutions-mailbase-tutorial.md), [sibsolutions CYBERMAILΣ](../saas-apps/cybersolutions-cybermail-tutorial.md), [limbpacmms](https://auth.limblecmms.com/), [Glint Inc](../saas-apps/glint-inc-tutorial.md), MB [Yükseklik](../saas-apps/zeroheight-tutorial.md), [cinsiyet uygunluk](https://app.genderfitness.com/), [COEO portalı](https://my.coeo.com/), [Grammarly](../saas-apps/grammarly-tutorial.md), [fivetran](../saas-apps/fivetran-tutorial.md), [Kumolus](../saas-apps/kumolus-tutorial.md), [RSA dever Suite](../saas-apps/rsa-archer-suite-tutorial.md), [teamzbeceri](../saas-apps/teamzskill-tutorial.md), [raumfürraum](../saas-apps/raumfurraum-tutorial.md), [Saviynt](../saas-apps/saviynt-tutorial.md), [BIZMERLINHR](https://marketplace.bizmerlin.net/bmone/signup), [Mobil dolap](../saas-apps/mobile-locker-tutorial.md), [zengine](../saas-apps/zengine-tutorial.md), [CLOUDCADı](https://app.cloudcadi.com/login), [sıtem](https://simfonianalytics.com/accounts/microsoft/login/), [bir kimlik & erişim yönetimi](https://my.priva.com/), [Nitro Pro](https://www.gonitro.com/nps/product-details/downloads), [eventfinity](../saas-apps/eventfinity-tutorial.md), [fexa](../saas-apps/fexa-tutorial.md), [güvenli imzalama Enterprise Portal](https://www.securedsigning.com/aad/Auth/ExternalLogin/AdminPortal), [güvenli imzalama Enterprise Portal AAD kurulumu](https://www.securedsigning.com/aad/Auth/ExternalLogin/AdminPortal), [wınfc çevrimiçi](https://wisteconline.com/auth/oidc), [Oracle PeopleSoft tarafından korunan, F5 BIG-IP APM](../saas-apps/oracle-peoplesoft-protected-by-f5-big-ip-apm-tutorial.md)

Ayrıca, tüm uygulamaların belgelerini buradan bulabilirsiniz: https://aka.ms/AppsTutorial .

Uygulamanızı Azure AD uygulama galerisinde listelemek için buradaki ayrıntıları okuyun: https://aka.ms/AzureADAppRequest .

---

### <a name="new-delegation-role-in-azure-ad-entitlement-management-access-package-assignment-manager"></a>Azure AD Yetkilendirme Yönetimi 'nde yeni temsili rolü: paket atama Yöneticisi 'ne erişim

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kullanıcı erişim yönetimi  
**Ürün yeteneği:** Yetkilendirme Yönetimi
 
Atamaları yönetmek için ayrıntılı izinler sağlamak üzere Azure AD Yetkilendirme Yönetimi 'ne yeni bir erişim paketi atama Yöneticisi rolü eklenmiştir. Artık bu roldeki bir kullanıcıya görevler temsilcisinden, bir erişim paketinin atama yönetimini iş sahibine devredebilir. Ancak, Access Package atama Yöneticisi, erişim paketi ilkelerini veya yöneticiler tarafından ayarlanan diğer özellikleri değiştiremez. 

Bu yeni rolle, atamaların yönetimini devretmek ve diğer tüm erişim paketi yapılandırmalarında yönetim denetimini sürdürmek için gereken en az ayrıcalıklardan yararlanabilirsiniz. Daha fazla bilgi için bkz. [Yetkilendirme Yönetimi rolleri](../governance/entitlement-management-delegate.md#entitlement-management-roles).
 
---

### <a name="changes-to-privileged-identity-managements-onboarding-flow"></a>Privileged Identity Management ekleme akışında yapılan değişiklikler

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Privileged Identity Management  
**Ürün yeteneği:** Privileged Identity Management
 
Daha önce, Privileged Identity Management (PıM) gerekli Kullanıcı Onayı ' na ekleme ve PıM 'nin dikey penceresinde Azure MFA 'ya kayıt dahil bir ekleme akışı. Azure AD rolleri ve yöneticiler dikey penceresinde PıM deneyiminin son tümleştirmesi ile bu deneyimi kaldırdık. Geçerli P2 lisansına sahip herhangi bir kiracı, PıM 'ye otomatik olarak eklendi.

PıM 'ye ekleme, kiracınızda herhangi bir dolaysız olumsuz etkiye sahip değildir. Aşağıdaki değişiklikleri bekleyebilir:
- Etkin vs. gibi ek atama seçenekleri, PıM veya Azure AD rolleri ve yöneticiler dikey penceresinde atama yaptığınızda başlangıç ve bitiş zamanına uygundur. 
- Doğrudan atama deneyimine getirilen yönetim birimleri ve özel roller gibi ek kapsam mekanizmaları. 
- Genel yönetici veya ayrıcalıklı rol yöneticisiyseniz, PıM haftalık Özeti gibi birkaç ek e-posta almaya başlayabilirsiniz. 
- Ayrıca, rol atamasıyla ilgili denetim günlüğünde MS-PIM hizmet sorumlusu ' nı da görebilirsiniz. Bu beklenen değişikliğin normal iş akışınızı etkilememesi gerekir.

 Daha fazla bilgi için bkz. [Privileged Identity Management kullanmaya başlama](../privileged-identity-management/pim-getting-started.md).

---

### <a name="azure-ad-entitlement-management-the-select-pane-of-access-package-resources-now-shows-by-default-the-resources-currently-in-the-selected-catalog"></a>Azure AD Yetkilendirme Yönetimi: erişim paketi kaynaklarının seçme bölmesi artık varsayılan olarak seçili katalogdaki kaynakları gösterir

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Kullanıcı erişim yönetimi  
**Ürün yeteneği:** Yetkilendirme Yönetimi
 

Erişim paketi oluşturma akışında, kaynak rolleri sekmesinde, seçim bölmesi davranışı değişiyor. Şu anda varsayılan davranış, seçilen kataloğa eklenen kullanıcının ve kaynakların sahip olduğu tüm kaynakları göstermek olur. 

Bu deneyim, kullanıcıların katalogdan kolayca kaynak seçebilmek için varsayılan olarak katalogda zaten eklenen kaynakları görüntüleyecek şekilde değiştirilecektir. Güncelleştirme, erişim paketlerine eklenecek kaynakların keşfediliğine yardımcı olur ve kataloğun parçası olmayan kullanıcıya ait kaynakları yanlışlıkla ekleme riskini azaltır. Daha fazla bilgi edinmek için bkz. [Azure AD yetkilendirme yönetiminde yeni bir erişim paketi oluşturma](../governance/entitlement-management-access-package-create.md#resource-roles).
 
---

## <a name="august-2020"></a>Ağustos 2020 
 
### <a name="updates-to-azure-multi-factor-authentication-server-firewall-requirements"></a>Azure Multi-Factor Authentication Sunucusu Güvenlik Duvarı gereksinimlerine yönelik güncelleştirmeler

**Şunu yazın:** Değişiklik planı  
**Hizmet kategorisi:** MFA  
**Ürün yeteneği:** Kimlik güvenliği & koruması
 
1 Ekim 2020 ' den itibaren Azure MFA sunucusu güvenlik duvarı gereksinimleri ek IP aralıkları gerektirecektir.

Kuruluşunuzda giden güvenlik duvarı kurallarınız varsa, MFA sunucularınızın tüm gerekli IP aralıklarıyla iletişim kurabilmesi için kuralları güncelleştirin. IP aralıkları [Azure Multi-Factor Authentication sunucusu güvenlik duvarı gereksinimleriyle](../authentication/howto-mfaserver-deploy.md#azure-multi-factor-authentication-server-firewall-requirements)belgelenmiştir.

---

### <a name="upcoming-changes-to-user-experience-in-identity-secure-score"></a>Kimlik güvenli puanı 'nda Kullanıcı deneyimine yaklaşan değişiklikler

**Şunu yazın:** Değişiklik planı  
**Hizmet kategorisi:** Kimlik koruması **ürün yeteneği:** kimlik güvenliği & koruması

Microsoft güvenli puanınızın [yeni sürümünde](/microsoft-365/security/mtp/microsoft-secure-score-whats-new?view=o365-worldwide)tanıtılan değişikliklerle uyum sağlamak Için kimlik güvenli puan portalı 'nı güncelleştiriyoruz. 

Değişikliklerle birlikte önizleme sürümü, Eylül ayının başlangıcında kullanılabilir olacaktır. Önizleme sürümündeki değişiklikler şunları içerir:
- Microsoft güvenli puanı ile marka hizalaması için "kimlik güvenli puanı", "kimlik için güvenli skor" olarak yeniden adlandırıldı
- Standart ölçeğe normalleştirilmiş ve işaret yerine yüzde cinsinden bildirilen noktaları

Bu önizlemede, müşteriler mevcut deneyim ve yeni deneyim arasında geçiş yapabilir. Bu önizleme, Kasım 2020 sonuna kadar son olacak. Önizleme sonrasında, müşteriler otomatik olarak yeni UX deneyimine yönlendirilir.

---

### <a name="new-restricted-guest-access-permissions-in-azure-ad---public-preview"></a>Azure AD 'de yeni kısıtlı Konuk erişim Izinleri-genel önizleme

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Access Control   
**Ürün yeteneği:** Kullanıcı Yönetimi

Konuk kullanıcılar için dizin düzeyindeki izinleri güncelleştirdik. Bu izinler, yöneticilerin dış Konuk Kullanıcı erişimi üzerinde ek kısıtlamalar ve denetimler gerektirmesini sağlar. Yöneticiler artık dış konukların Kullanıcı ve grupların profiline ve üyelik bilgilerine erişimi için ek kısıtlamalar ekleyebilir. Bu genel önizleme özelliğiyle, müşteriler, konuk kullanıcıların içinde bulundukları grup üyeliklerini görmesini kısıtlama dahil olmak üzere grup üyeliklerini kararlayarak, dış Kullanıcı erişimini ölçekte yönetebilir.

Daha fazla bilgi için bkz. [kısıtlı Konuk erişim izinleri](../users-groups-roles/users-restrict-guest-permissions.md) ve [Kullanıcıların varsayılan izinleri](./users-default-permissions.md).
 
---

### <a name="general-availability-of-delta-queries-for-service-principals"></a>Hizmet sorumluları için Delta sorgularının genel kullanılabilirliği

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** MS grafiği  
**Ürün yeteneği:** Geliştirici deneyimi
 
Microsoft Graph Delta sorgusu artık v 1.0 'da kaynak türünü destekliyor:
- Hizmet Sorumlusu

Artık istemciler bu kaynaklardaki değişiklikleri etkili bir şekilde izleyebilir ve değişiklikleri yerel bir veri deposuyla eşitlemeye yönelik en iyi çözümü sağlar. Bu kaynakları bir sorguda nasıl yapılandıracağınızı öğrenmek için bkz. [Microsoft Graph verilerdeki değişiklikleri izlemek için Delta sorgu kullanma](/graph/delta-query-overview).
 
---

### <a name="general-availability-of-delta-queries-for-oauth2permissiongrant"></a>OAuth2PermissionGrant için Delta sorgularının genel kullanılabilirliği

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** MS grafiği  
**Ürün yeteneği:** Geliştirici deneyimi

Microsoft Graph Delta sorgusu artık v 1.0 'da kaynak türünü destekliyor:
- OAuth2PermissionGrant

İstemciler artık bu kaynaklardaki değişiklikleri etkili bir şekilde izleyebilir ve değişiklikleri yerel bir veri deposuyla eşitlemeye yönelik en iyi çözümü sağlar. Bu kaynakları bir sorguda nasıl yapılandıracağınızı öğrenmek için bkz. [Microsoft Graph verilerdeki değişiklikleri izlemek için Delta sorgu kullanma](/graph/delta-query-overview).

---

### <a name="new-federated-apps-available-in-azure-ad-application-gallery---august-2020"></a>Azure AD uygulama galerisinde yeni Federasyon uygulamaları kullanılabilir-Ağustos 2020

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi

Ağustos 2020 ' de, federasyon desteğiyle App Gallery 'de aşağıdaki 25 yeni uygulamayı ekledik:

[Backup365](https://portal.backup365.io/login), [Soapbox](https://app.soapboxhq.com/create?step=auth&provider=azure-ad2-oauth2), [alma SIS](https://almau.getalma.com/), [enlyft Dynamics 365 Bağlayıcısı](http://enlyft.com/), [serseview Space kullanımı yazılım çözümleri](../saas-apps/serraview-space-utilization-software-solutions-tutorial.md), [Uniq](https://web.uniq.app/), [Visi,](../saas-apps/visibly-tutorial.md) [Zylo](../saas-apps/zylo-tutorial.md), [edmentum-Courseware değerlendirmesi tam yolu](https://auth.edmentum.com/elf/login), [Sivlab](https://cyberlab.evolvesecurity.com/#/welcome), [Altamıra HRM](../saas-apps/altamira-hrm-tutorial.md), [kablotekler](../saas-apps/wirewheel-tutorial.md), [ZX uyumluluk ve yakalama](https://sminstall.zixcorp.com/teams/teams.php?install_request=true&tenant_id=common), Doğa, cıklü [kurumsal iş denetimleri platformu](../saas-apps/greenlight-enterprise-business-controls-platform-tutorial.md), [Genetec payını](https://www.clearance.network/), [ISAMs](../saas-apps/isams-tutorial.md), [verasmart](../saas-apps/verasmart-tutorial.md), [AMIKO](https://amiko.web.rivero.app/), [twingate](https://auth.twingate.com/signup), [huni Kiralama](https://nestiolistings.com/sso/oidc/azure/authorize/), [scalefyayılması](https://scalefusion.com/users/sign_in/), [bpanda](https://goto.bpanda.com/login), [vivun takvim bağlantısı](https://app.vivun.com/dashboard/calendar/connect), [Fortigate SSL VPN](../saas-apps/fortigate-ssl-vpn-tutorial.md), [wandera Son Kullanıcı](https://www.wandera.com/)

Ayrıca, tüm uygulamaların belgelerini buradan bulabilirsiniz. https://aka.ms/AppsTutorial

Uygulamanızı Azure AD uygulama galerisinde listelemek için buradaki ayrıntıları okuyun https://aka.ms/AzureADAppRequest

---

### <a name="resource-forests-now-available-for-azure-ad-ds"></a>Kaynak ormanları artık Azure AD DS için kullanılabilir 

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** Azure AD Domain Services   
**Ürün yeteneği:** Azure AD Domain Services
 
Azure AD Domain Services içindeki kaynak ormanların özelliği artık genel kullanıma sunulmuştur. Artık, akıllı kart yetkilendirmesi de dahil olmak üzere Azure AD Domain Services kullanmak için Parola karması eşitlemesi olmadan yetkilendirmeyi etkinleştirebilirsiniz. Daha fazla bilgi edinmek için bkz. [Azure Active Directory Domain Services (Önizleme) Için çoğaltma kümesi kavramları ve özellikleri](../../active-directory-domain-services/concepts-replica-sets.md).
 
---

### <a name="regional-replica-support-for-azure-ad-ds-managed-domains-now-available"></a>Azure AD DS yönetilen etki alanları için bölgesel çoğaltma desteği artık kullanılabilir

**Şunu yazın:** Yeni Özellik   
**Hizmet kategorisi:** Azure AD Domain Services  
**Ürün yeteneği:** Azure AD Domain Services
 
Yönetilen bir etki alanını, Azure AD kiracısı başına birden fazla çoğaltma kümesine sahip olacak şekilde genişletebilirsiniz. Çoğaltma kümeleri, Azure AD Domain Services destekleyen herhangi bir Azure bölgesindeki eşlenen herhangi bir sanal ağa eklenebilir. Farklı Azure bölgelerindeki ek çoğaltma kümeleri, bir Azure bölgesi çevrimdışı kalırsa eski uygulamalar için coğrafi olağanüstü durum kurtarma sağlar. Daha fazla bilgi edinmek için bkz. [Azure Active Directory Domain Services (Önizleme) Için çoğaltma kümesi kavramları ve özellikleri](../../active-directory-domain-services/concepts-replica-sets.md).

---

### <a name="general-availability-of-azure-ad-my-sign-ins"></a>Azure AD My Sign-Ins genel kullanılabilirliği

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kimlik doğrulamaları (oturum açma)  
**Ürün yeteneği:** Son Kullanıcı deneyimleri
 
Azure AD My Sign-Ins, kurumsal kullanıcıların olağan dışı etkinlikleri denetlemek için oturum açma geçmişini gözden geçirmesine olanak sağlayan yeni bir özelliktir. Ayrıca, bu özellik, son kullanıcıların şüpheli etkinlikler üzerinde "Bu ben değilme" veya "Bu was Me" olarak raporistemelerine izin verir. Bu özelliği kullanma hakkında daha fazla bilgi edinmek için bkz. [My Sign-Ins sayfasından en son oturum açma etkinliğinizi görüntüleme ve arama](../user-help/my-account-portal-sign-ins-page.md#confirm-unusual-activity).
 
---

### <a name="sap-successfactors-hr-driven-user-provisioning-to-azure-ad-is-now-generally-available"></a>Azure AD 'de SAP başarılı bir şekilde yönetilen Kullanıcı hazırlama, genel kullanıma sunuldu

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Uygulama sağlama  
**Ürün yeteneği:** Kimlik yaşam döngüsü yönetimi
 
Artık Azure AD ile SAP başarılı çarpanlarını yetkili kimlik kaynağı olarak tümleştirebilir ve Azure AD 'de hesapların sağlanması ve ön sağlamasını yapmak için yeni işe alımlar ve sonlandırmalar gibi HR olaylarını kullanarak uçtan uca kimlik yaşam döngüsünü otomatik hale getirebilirsiniz. 

SAP başarılı bir şekilde Azure AD 'nin sağlamasını yapılandırma hakkında daha fazla bilgi edinmek için bkz. [SAP başarılı faktörleri Active Directory için Kullanıcı sağlamayı yapılandırma](../saas-apps/sap-successfactors-inbound-provisioning-tutorial.md).
 
---

### <a name="custom-open-id-connect-ms-graph-api-support-for-azure-ad-b2c"></a>Özel açık KIMLIK Azure AD B2C için MS Graph API desteğini bağlama

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** B2C-tüketici kimlik yönetimi  
**Ürün yeteneği:** B2B/B2C
 
Daha önce özel açık KIMLIK bağlama sağlayıcıları yalnızca Azure portal aracılığıyla eklenebilir veya yönetilebilir. Azure AD B2C müşteriler artık Microsoft Graph API beta sürümü aracılığıyla bunları ekleyebilir ve yönetebilir. Bu kaynağı API 'lerle yapılandırma hakkında bilgi edinmek için bkz. [IdentityProvider kaynak türü](/graph/api/resources/identityprovider?view=graph-rest-beta).
 
---

### <a name="assign-azure-ad-built-in-roles-to-cloud-groups"></a>Azure AD yerleşik rollerini bulut gruplarına atama

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Azure AD rolleri  
**Ürün yeteneği:** Access Control

Artık bu yeni özelliği kullanarak, Azure AD yerleşik rollerini bulut gruplarına atayabilirsiniz. Örneğin, SharePoint Yöneticisi rolünü Contoso_SharePoint_Admins grubuna atayabilirsiniz. Ayrıca, bir gruba bir rolün uygun bir üyesini sağlamak için PıM 'yi kullanarak, bekleyen erişim izni verebilirsiniz. Bu özelliği nasıl yapılandıracağınızı öğrenmek için bkz. [Azure Active Directory (Önizleme) içinde rol atamalarını yönetmek için bulut gruplarını kullanma](../users-groups-roles/roles-groups-concept.md).
 
---

### <a name="insights-business-leader-built-in-role-now-available"></a>Öngörüler Iş lideri yerleşik rolü artık kullanılabilir

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Azure AD rolleri  
**Ürün yeteneği:** Access Control
 
Öngörüler Iş lideri rolündeki kullanıcılar, [M365 Insights uygulaması](https://www.microsoft.com/microsoft-365/partners/workplaceanalytics)aracılığıyla bir panolar ve Öngörüler kümesine erişebilir. Bu, tüm panolara ve sunulan Öngörüler ve veri araştırma işlevlerine tam erişim içerir. Ancak, bu roldeki kullanıcılar, Öngörüler Yöneticisi rolünün sorumluluğu olan ürün yapılandırma ayarlarına erişemez. Bu rol hakkında daha fazla bilgi için, bkz. [Azure Active Directory yönetici rolü izinleri](../users-groups-roles/directory-assign-admin-roles.md#insights-business-leader)
 
---

### <a name="insights-administrator-built-in-role-now-available"></a>Öngörüler Yöneticisi yerleşik rolü artık kullanılabilir

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Azure AD rolleri  
**Ürün yeteneği:** Access Control
 
Öngörüler yönetici rolündeki kullanıcılar, [M365 Insights uygulamasındaki](https://www.microsoft.com/microsoft-365/partners/workplaceanalytics)tüm yönetim özelliklerine erişebilir. Bu roldeki bir Kullanıcı dizin bilgilerini okuyabilir, hizmet durumunu izleyebilir, dosya destek biletlerini izleyebilir ve Öngörüler yönetici ayarları yönlerini erişebilir. Bu rol hakkında daha fazla bilgi için, bkz. [Azure Active Directory yönetici rolü izinleri](../users-groups-roles/directory-assign-admin-roles.md#insights-administrator)
 
--- 

### <a name="application-admin-and-cloud-application-admin-can-manage-extension-properties-of-applications"></a>Uygulama Yöneticisi ve bulut uygulaması Yöneticisi, uygulamaların uzantı özelliklerini yönetebilir

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Azure AD rolleri  
**Ürün yeteneği:** Access Control
 
Daha önce, [uzantı özelliğini](/graph/api/application-post-extensionproperty?view=graph-rest-beta&tabs=http)yalnızca genel yönetici yönetebilir. Artık uygulama Yöneticisi ve bulut uygulaması Yöneticisi için de bu özelliği etkinleştiriyoruz.
 
---

### <a name="mim-2016-sp2-hotfix-462630-and-connectors-1113010"></a>MıM 2016 SP2 düzeltme 4.6.263.0 ve bağlayıcılar 1.1.1301.0

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Microsoft Identity Manager  
**Ürün yeteneği:** Kimlik yaşam döngüsü yönetimi

Microsoft Identity Manager (MıM) 2016 Service Pack 2 (SP2) için bir [düzeltme paketi paketi (Build 4.6.263.0)](https://support.microsoft.com/help/4576473/hotfix-rollup-package-build-4-6-263-0-is-available-for-microsoft-ident) kullanılabilir. Bu toplu paket MIM CM, MıM Eşitleme Yöneticisi ve PAM bileşenleri için güncelleştirmeleri içerir. Ayrıca, MıM genel bağlayıcıları derleme 1.1.1301.0, Graph Bağlayıcısı için güncelleştirmeleri içerir.

---
 
## <a name="july-2020"></a>Temmuz 2020

### <a name="as-an-it-admin-i-want-to-target-client-apps-using-conditional-access"></a>BT Yöneticisi olarak, koşullu erişim kullanarak istemci uygulamalarını hedeflemek istiyorum

**Şunu yazın:** Değişiklik planı   
**Hizmet kategorisi:** Koşullu erişim  
**Ürün yeteneği:** Kimlik güvenliği & koruması
 
Koşullu erişimde istemci uygulamaları koşulunun GA sürümü ile, yeni ilkeler artık varsayılan olarak tüm istemci uygulamalarına uygulanır. Buna eski kimlik doğrulama istemcileri dahildir. Mevcut ilkeler değişmeden kalır, ancak ilke tarafından hangi istemci uygulamalarının uygulanacağını kolayca görmek için, var olan ilkelerden *yapılandırma Evet/Hayır* geçişi kaldırılır. 

Yeni bir ilke oluştururken, hala eski kimlik doğrulaması kullanan kullanıcıları ve hizmet hesaplarını dışlediğinizden emin olun; Bunu yapmazsanız, engellenecektir. [Daha fazla bilgi edinin](../conditional-access/concept-conditional-access-conditions.md).
 
---

### <a name="upcoming-scim-compliance-fixes"></a>Yaklaşan SCıM uyumluluk düzeltmeleri

**Şunu yazın:** Değişiklik planı  
**Hizmet kategorisi:** Uygulama sağlama  
**Ürün yeteneği:** Kimlik yaşam döngüsü yönetimi
 
Azure AD sağlama hizmeti, uygulamalarla tümleştirme için SCıM standardını kullanır. SCıM standardı uygulamamız gelişiyor ve bir kaynaktaki "etkin" özelliğini ayarlama ve düzeltme eki gerçekleştirme davranışımızın bir üzerinde değişiklik yapmayı bekledik. [Daha fazla bilgi edinin](../app-provisioning/application-provisioning-config-problem-scim-compatibility.md).
 
---

### <a name="group-owner-setting-on-azure-admin-portal-will-be-changed"></a>Azure yönetim portalı 'nda Grup sahibi ayarı değiştirilecek

**Şunu yazın:** Değişiklik planı  
**Hizmet kategorisi:** Grup Yönetimi  
**Ürün yeteneği:** İş

Gruplar genel ayar sayfasında sahip ayarları, Azure yönetim portalı ve erişim panelindeki sınırlı bir kullanıcı grubuyla sahip atama ayrıcalıklarını kısıtlayacak şekilde yapılandırılabilir. Yakında, bu iki UX portalında değil de grup sahibi ayrıcalığı atama imkanına sahip olmakla kalmaz, uç noktalar arasında PowerShell ve Microsoft Graph gibi tutarlı davranışlar sağlamak için de arka uçta ilke uygulayabilir. 

Kullanmayan müşteriler için geçerli ayarı devre dışı bırakacağız ve bir sonraki birkaç ayda Grup sahibi ayrıcalığı için kullanıcıları kapsam seçeneğiyle bir seçenek sunacak. Grup ayarlarını güncelleştirme hakkında yönergeler için bkz. [Azure Active Directory](./active-directory-groups-settings-azure-portal.md?context=azure%2factive-directory%2fusers-groups-roles%2fcontext%2fugr-context)kullanarak Grup bilgilerinizi düzenleme.

---

### <a name="azure-active-directory-registration-service-is-ending-support-for-tls-10-and-11"></a>Azure Active Directory kayıt hizmeti, TLS 1,0 ve 1,1 için destek bitidir

**Şunu yazın:** Değişiklik planı  
**Hizmet kategorisi:** Cihaz kaydı ve yönetimi  
**Ürün yeteneği:** Platformunun

Aktarım Katmanı Güvenliği (TLS) 1,2 ve güncelleştirme sunucuları ve istemcileri yakında Azure Active Directory Cihaz Kaydı hizmetiyle iletişim kuracak. Azure AD cihaz kayıt hizmeti ile iletişim için TLS 1,0 ve 1,1 desteği devre dışı bırakılacak:
- 31 Ağustos 2020 ' de tüm bağımsız bulutlarda (GCC High, DoD vb.)
- 30 Ekim 2020 ' de tüm ticari bulutlarda

Azure AD kayıt hizmeti için TLS 1,2 hakkında [daha fazla bilgi edinin](../devices/reference-device-registration-tls-1-2.md) .

---

### <a name="windows-hello-for-business-sign-ins-visible-in-azure-ad-sign-in-logs"></a>Azure AD oturum açma günlüklerinde görünen Iş için Windows Hello

**Şunu yazın:** Düzenle  
**Hizmet kategorisi:** Rapor  
**Ürün yeteneği:** & raporlamayı izleme
 
Iş için Windows Hello, son kullanıcıların Windows makinelerde bir hareket (PIN veya biyometri gibi) oturum açmasına olanak tanır. Azure AD yöneticileri, şirket içi oturum açma işlemleri için Windows Hello 'Yu, bir kuruluşun yolculuğunun bir parçası olarak passwordless kimlik doğrulamasına ayırmak isteyebilir. 

Yöneticiler artık, Azure portal Azure AD Sign-Ins dikey penceresinde bir Windows oturum açma olayı için kimlik doğrulama ayrıntıları sekmesini denetleyerek Windows kimlik doğrulamasının Iş için Windows Hello kullanıp kullanmadığını görebilir. Iş için Windows Hello kimlik doğrulamaları, kimlik doğrulama yöntemi alanına "WindowsHelloForBusiness" içerecektir. Sign-In günlüklerini yorumlama hakkında daha fazla bilgi için lütfen [oturum açma günlükleri belgelerine](../reports-monitoring/concept-sign-ins.md)bakın.
 
---

### <a name="fixes-to-group-deletion-behavior-and-performance-improvements"></a>Grup silme davranışı ve performans iyileştirmeleri için düzeltmeler

**Şunu yazın:** Düzenle  
**Hizmet kategorisi:** Uygulama sağlama  
**Ürün yeteneği:** Kimlik yaşam döngüsü yönetimi
 
Daha önce, bir grup "kapsam içi" iken "kapsam dışı" olarak değiştirildiğinde ve değişiklik tamamlanmadan önce bir yöneticinin tıklamış olması durumunda, Grup nesnesi silinmeyecektir. Artık grup nesnesi kapsam dışına geçtiğinde (devre dışı, silinmiş, atanmamış veya kapsam filtresi geçirmediği) hedef uygulamadan silinecek. [Daha fazla bilgi edinin](../app-provisioning/how-provisioning-works.md#incremental-cycles).
 
---

### <a name="public-preview-admins-can-now-add-custom-content-in-the-email-to-reviewers-when-creating-an-access-review"></a>Genel Önizleme: Yöneticiler artık erişim incelemesi oluştururken gözden geçirenlere e-postaya özel içerik ekleyebilir

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Erişim Incelemeleri  
**Ürün yeteneği:** Kimlik yönetimi
 
Yeni bir erişim incelemesi oluşturulduğunda, gözden geçiren, erişim incelemesini tamamlaması için isteyen bir e-posta alır. Müşterilerimizin birçoğu, e-postaya iletişim bilgileri gibi özel içerik ekleme ve gözden geçiriciye kılavuzluk etmek için diğer ek destekleyici içerik gibi bir özellik istiyor. 

Artık genel önizlemede sunulan Yöneticiler, Azure AD erişim Incelemelerinin "Gelişmiş" bölümünde içerik ekleyerek gözden geçirenlere gönderilen e-postada özel içerik belirtebilir. Erişim gözden geçirmeleri oluşturma hakkında yönergeler için bkz. [Azure AD erişim gözden geçirmeleri içindeki gruplar ve uygulamalar için erişim gözden geçirmesi oluşturma](../governance/create-access-review.md).
 
---

### <a name="authorization-code-flow-for-single-page-apps-available"></a>Tek sayfalı uygulamalar için yetkilendirme kodu akışı kullanılabilir

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kimlik doğrulamaları (oturum açma)  
**Ürün yeteneği:** Geliştirici deneyimi
 
Safari ıOP gibi modern tarayıcı tanımlama bilgisi kısıtlamaları nedeniyle, maça 'ın SSO 'yu korumak için örtük akış yerine yetkilendirme kodu akışını kullanması ve MSAL.js v 2. x de yetkilendirme kodu akışını desteklemeye devam edecektir. 

SPA 'nizi "Spa" türüne ve kimlik doğrulama kod akışını kullanacak şekilde güncelleştirebilmeniz için Azure portal ilgili güncelleştirmeler vardır. Daha fazla bilgi için bkz. [oturum açma kullanıcıları ve kimlik doğrulama kodu akışını kullanarak bir JAVASCRIPT Spa 'da erişim belirteci edinme](../develop/quickstart-v2-javascript-auth-code.md) .
 
---

### <a name="azure-ad-application-proxy-now-supports-the-remote-desktop-services-web-client"></a>Azure AD Uygulama Ara Sunucusu artık Uzak Masaüstü Hizmetleri Web Istemcisini destekliyor

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Uygulama proxy 'Si  
**Ürün yeteneği:** Access Control

Azure AD Uygulama Ara Sunucusu artık Uzak Masaüstü Hizmetleri (RDS) Web Istemcisini desteklemektedir. RDS Web istemcisi, kullanıcıların Microsoft Edge, Internet Explorer 11, Google Chrome vb. gibi HTLM5 özellikli bir tarayıcıdan uzak masaüstü altyapısına erişmesini sağlar. Kullanıcılar, uzak uygulamalarla veya masaüstü bilgisayarlardan her yerden yerel bir cihazla etkileşim kurabilir. Azure AD Uygulama Ara Sunucusu kullanarak, tüm zengin istemci uygulamaları için ön kimlik doğrulama ve koşullu erişim ilkelerini zorunlu tutarak RDS dağıtımınızın güvenliğini artırabilirsiniz. Rehberlik için bkz. [Azure AD uygulama ara sunucusu Ile uzak masaüstü yayımlama](../manage-apps/application-proxy-integrate-with-remote-desktop-services.md).
 
---

### <a name="next-generation-azure-ad-b2c-user-flows-in-public-preview"></a>Yeni nesil Azure AD B2C Kullanıcı akışları genel önizlemede

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** B2C-tüketici kimlik yönetimi  
**Ürün yeteneği:** B2B/B2C
 
Basitleştirilmiş Kullanıcı akış deneyimi, önizleme özellikleriyle Özellik eşliği sağlar ve tüm yeni özelliklerin ana adresidir. Kullanıcılar aynı kullanıcı akışındaki yeni özellikleri etkinleştirerek, her yeni özellik sürümüyle birden çok sürüm oluşturma gereksinimini azaltır. Son olarak, yeni, Kullanıcı dostu UX, Kullanıcı akışlarının seçimini ve oluşturulmasını basitleştirir. Şimdi [bir Kullanıcı akışı oluşturarak](../../active-directory-b2c/tutorial-create-user-flows.md)deneyin. 

Kullanıcılar akışları hakkında daha fazla bilgi için, [Azure Active Directory B2C Içindeki Kullanıcı akışı sürümleri](../../active-directory-b2c/user-flow-versions.md#:~:text=    User flow  ,account. Usi ...  1 more rows )bölümüne bakın.

---

### <a name="new-federated-apps-available-in-azure-ad-application-gallery---july-2020"></a>Azure AD uygulama galerisinde yeni Federasyon uygulamaları kullanılabilir-Temmuz 2020

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi
 
Haziran 2020 ' de, Federasyon desteği ile App Gallery 'de aşağıdaki 55 yeni uygulamaları ekledik:

[Clap, uygulamalı](http://www.rmit.com.ar/), [appreiz](https://microsoftteams.appreiz.com/), [inextor Kasası](https://inexto.com/inexto-suite/inextor), [Beekast](https://my.beekast.com/), [templamy OpenID Connect](https://app.templafy.com/), [peterconnect Alım](https://msteams.peterconnects.com/), [alogüm](https://appfusions.alohacloud.com/auth), güetler, [DENETIM Kulesi](https://bpm.tnxcorp.com/sso/microsoft), [Cocoom](https://start.cocoom.com/), [Coins oluşturma bulutu](https://sso.coinsconstructioncloud.com/#login/), [medxnote MT](https://task.teamsmain.medx.im/authorization), [yansıma kt](https://reflekt.konsolute.com/login), [rever](https://app.reverscore.net/access), [mycompanyarchive](https://login.mycompanyarchive.com/), [greminders](https://app.greminders.com/o365-oauth), [titandosya](../saas-apps/titanfile-tutorial.md), [WOOTRIC](../saas-apps/wootric-tutorial.md), [Solarwinds cumon](https://support.solarwinds.com/SuccessCenter/s/orion-platform?language=en_US),  [OpenText Dizin Hizmetleri](../saas-apps/opentext-directory-services-tutorial.md), [Datasite](../saas-apps/datasite-tutorial.md), [Blogin](../saas-apps/blogin-tutorial.md), [intsıghts](../saas-apps/intsights-tutorial.md), [kpifire](../saas-apps/kpifire-tutorial.md), [TextLine](../saas-apps/textline-tutorial.md), [bulut akademik My-SSO](../saas-apps/cloud-academy-sso-tutorial.md), [topluluk Spark](../saas-apps/community-spark-tutorial.md), [ChatWork](../saas-apps/chatwork-tutorial.md), [cloudsign](../saas-apps/cloudsign-tutorial.md), [C3M Cloud Control](../saas-apps/c3m-cloud-control-tutorial.md), [smarthr](https://smarthr.jp/), [Numlyengage™](../saas-apps/numlyengage-tutorial.md), [Michigan Data Hub çoklu oturum açma](../saas-apps/michigan-data-hub-single-sign-on-tutorial.md), [Çıkış](../saas-apps/egress-tutorial.md), [sendgüvenle](../saas-apps/sendsafely-tutorial.md), [eletive](https://app.eletive.com/), [sağ taraftaki siber güvenlik adı](https://right-hand.ai/), [EYC kurumsal kimlik doğrulaması](https://enterprise.fyde.com/), verme [,](../saas-apps/verme-tutorial.md) [lenses.io](../saas-apps/lensesio-tutorial.md), [momenta](../saas-apps/momenta-tutorial.md), [Uprise](https://app.uprise.co/sign-in), [Q](https://q.moduleq.com/login), [cloudcords](../saas-apps/cloudcords-tutorial.md), [Tellme bot](https://tellme365liteweb.azurewebsites.net/), [feire](https://app.inspiresoftware.com/), [Maverics Identity Orchestrator SAML Bağlayıcısı](https://www.strata.io/identity-fabric/), [smartokul (okul yönetim sistemi)](https://smart-schoolapp.com/frmLoginForm), [Zepto-Intelligent timesaklanması](https://user.zepto-ai.com/signin), [Studi.ly](https://studi.ly/), [Trackplan](http://www.trackplanfm.com/), [skedda](../saas-apps/skedda-tutorial.md), [whosonlocation](../saas-apps/whos-on-location-tutorial.md), [Coire](../saas-apps/coggle-tutorial.md), [Kemp Loadmaster](https://kemptechnologies.com/cloud-load-balancer/), [BrowserStack çoklu oturum açma](../saas-apps/browserstack-single-sign-on-tutorial.md)

Ayrıca, tüm uygulamaların belgelerini buradan bulabilirsiniz. https://aka.ms/AppsTutorial

Uygulamanızı Azure AD uygulama galerisinde listelemek için lütfen buradaki ayrıntıları okuyun https://aka.ms/AzureADAppRequest

---

### <a name="new-provisioning-connectors-in-the-azure-ad-application-gallery---july-2020"></a>Azure AD uygulama galerisinde yeni sağlama bağlayıcıları-Temmuz 2020

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Uygulama sağlama  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi

Artık yeni tümleşik App [LinkedIn öğrenimi](../saas-apps/linkedin-learning-provisioning-tutorial.md)için Kullanıcı hesaplarını oluşturmayı, güncelleştirmeyi ve silmeyi otomatik hale getirebilirsiniz.

Otomatik Kullanıcı hesabı sağlamayı kullanarak kuruluşunuzun daha iyi güvenliğini sağlama hakkında daha fazla bilgi için bkz. [Azure AD Ile SaaS uygulamalarına Kullanıcı sağlamayı otomatikleştirme](../app-provisioning/user-provisioning.md).

---

### <a name="view-role-assignments-across-all-scopes-and-ability-to-download-them-to-a-csv-file"></a>Tüm kapsamlar genelinde rol atamalarını görüntüleme ve bunları bir CSV dosyasına indirme yeteneği

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Azure AD rolleri  
**Ürün yeteneği:** Access Control
 
Artık Azure AD portalındaki "roller ve Yöneticiler" sekmesinde bir rolün tüm kapsamları genelinde rol atamalarını görüntüleyebilirsiniz. Ayrıca, her rol için bu rol atamalarını bir CSV dosyasına indirebilirsiniz. Rol atamalarını görüntüleme ve ekleme hakkında yönergeler için, bkz. [Azure Active Directory yönetici rollerini görüntüleme ve atama](../users-groups-roles/directory-manage-roles-portal.md).
 
---

### <a name="azure-multi-factor-authentication-software-development-azure-mfa-sdk-deprecation"></a>Azure Multi-Factor Authentication yazılım geliştirme (Azure MFA SDK) kullanımdan kaldırılıyor

**Şunu yazın:** Kullanım dışı  
**Hizmet kategorisi:** MFA  
**Ürün yeteneği:** Kimlik güvenliği & koruması
 
Azure Multi-Factor Authentication yazılım geliştirme (Azure MFA SDK), 14 Kasım 2018 ' 2017 de ilk kez duyuruldu. Microsoft, 30 Eylül 2020 tarihinde geçerli olan SDK hizmetini kapatacak. SDK 'ya yapılan çağrılar başarısız olur.

Kuruluşunuz Azure MFA SDK 'sını kullanıyorsa, 30 Eylül 2020 ' e kadar geçiş yapmanız gerekir:
- MıM için Azure MFA SDK: SDK 'Yı MıM ile kullanıyorsanız, bu [yönergeleri](/microsoft-identity-manager/working-with-mfaserver-for-mim)IZLEYEREK Azure MFA sunucusuna geçiş yapmanız ve PRIVILEGED Access Management (Pam) etkinleştirmeniz gerekir.   
- Özelleştirilmiş uygulamalar için Azure MFA SDK 'Sı: uygulamanızı Azure AD 'ye tümleştirmeyi düşünün ve MFA 'yı zorlamak için koşullu erişim kullanın. Başlamak için bu [sayfayı](../manage-apps/plan-an-application-integration.md)gözden geçirin. 

---

## <a name="june-2020"></a>Haziran 2020 

### <a name="user-risk-condition-in-conditional-access-policy"></a>Koşullu erişim ilkesinde Kullanıcı risk koşulu

**Şunu yazın:** Değişiklik planı  
**Hizmet kategorisi:** Koşullu erişim  
**Ürün yeteneği:** Kimlik güvenliği & koruması
 

Azure AD koşullu erişim ilkesinde Kullanıcı risk desteği, birden çok Kullanıcı risk tabanlı ilke oluşturmanıza olanak sağlar. Farklı kullanıcılar ve uygulamalar için farklı en düşük Kullanıcı risk düzeyleri gerekli olabilir. Kullanıcı riskine bağlı olarak, erişimi engellemek, Multi-Factor Authentication, güvenli parola değişikliği yapmak veya ek denetim gibi oturum ilkesini zorlamak için Microsoft Cloud App Security yeniden yönlendirmek üzere ilkeler oluşturabilirsiniz.

Kullanıcı risk koşulu, bir P2 teklifi olan Azure kimlik koruması kullandığından Azure AD Premium P2 gerektirir. Koşullu erişim hakkında daha fazla bilgi için [Azure AD koşullu erişim belgelerine](../conditional-access/index.yml)bakın.

---

### <a name="saml-sso-now-supports-apps-that-require-spnamequalifier-to-be-set-when-requested"></a>SAML SSO artık istendiğinde, SPNameQualifier 'nin ayarlanması gereken uygulamaları destekliyor

**Şunu yazın:** Düzenle  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** SSO
 
Bazı SAML uygulamaları, istendiğinde onay konusunun döndürülmesini gerektirir. İstek NameID ilkesinde bir SPNameQualifier istendiğinde Azure AD doğru şekilde yanıt veriyor. Bu, SP tarafından başlatılan oturum açma için de çalışır ve IDP tarafından başlatılan oturum açma işlemi uygulanır.  Azure Active Directory 'de SAML protokolü hakkında daha fazla bilgi edinmek için bkz. [tek Sign-On SAML Protokolü](../develop/single-sign-on-saml-protocol.md).

---

### <a name="azure-ad-b2b-collaboration-supports-inviting-msa-and-google-users-in-azure-government-tenants"></a>Azure AD B2B Işbirliği, Azure Kamu kiracılarında MSA ve Google kullanıcılarını davet etme desteği

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** EAı  
**Ürün yeteneği:** B2B/B2C
 

B2B işbirliği özelliklerini kullanan Azure Kamu kiracılar artık Microsoft veya Google hesabı olan kullanıcıları davet edebilir. Kiracınızın bu özellikleri kullanıp kullanmızdan emin olmak için, [Azure ABD kamu kiracımda B2B işbirliğinin kullanılabilir olup olmadığını nasıl anlayabilirim?](../external-identities/current-limitations.md#how-can-i-tell-if-b2b-collaboration-is-available-in-my-azure-us-government-tenant) konusundaki yönergeleri izleyin.

 
---
 
### <a name="user-object-in-ms-graph-v1-now-includes-externaluserstate-and-externaluserstatechangeddatetime-properties"></a>MS Graph v1 'deki Kullanıcı nesnesi artık externalUserState ve externalUserStateChangedDateTime özelliklerini içerir

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** EAı  
**Ürün yeteneği:** B2B/B2C
 

ExternalUserState ve externalUserStateChangedDateTime özellikleri, davetlerini henüz kabul etmemiş olan davet edilen B2B konuklarının yanı sıra bazı gün sayısından sonra davetlerini kabul edilmeyen kullanıcıları silme gibi derleme Otomasyonu 'nu bulmak için de kullanılabilir. Bu özellikler artık MS Graph v1 'de kullanılabilir. Bu özellikleri kullanma hakkında rehberlik için [Kullanıcı kaynak türü](/graph/api/resources/user?view=graph-rest-1.0)' ne bakın.
 
---

### <a name="manage-authentication-sessions-in-azure-ad-conditional-access-is-now-generally-available"></a>Azure AD koşullu erişim 'de kimlik doğrulama oturumlarını yönetme genel kullanıma sunuldu

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Koşullu erişim  
**Ürün yeteneği:** Kimlik güvenliği & koruması
 
Kimlik doğrulama oturumu yönetimi özellikleri, kullanıcılarınızın ortamınızda daha fazla güvenlik ve esneklik sunmak için oturum açma kimlik bilgilerini ve tarayıcıları kapatıp yeniden açtıktan sonra kimlik bilgileri sağlamaları gerekip gerekmediğini yapılandırmanıza olanak tanır.
 
Ayrıca, kimlik doğrulama oturumu yönetimi yalnızca Azure AD 'ye katılmış, hibrit Azure AD 'ye katılmış ve Azure AD 'ye kayıtlı cihazlarda yalnızca birinci faktör kimlik doğrulamasına uygulamak için kullanılır. Artık, kimlik doğrulama oturumu yönetimi MFA için de geçerlidir. Daha fazla bilgi için bkz. [koşullu erişimle kimlik doğrulama oturumu yönetimini yapılandırma](../conditional-access/howto-conditional-access-session-lifetime.md).

---

### <a name="new-federated-apps-available-in-azure-ad-application-gallery---june-2020"></a>Azure AD uygulama galerisinde yeni Federasyon uygulamaları kullanılabilir-Haziran 2020

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi
 
Haziran 2020 ' de, federasyon desteğiyle App Gallery 'de aşağıdaki 29 yeni uygulamayı ekledik:

[Shopify Plus](../saas-apps/shopify-plus-tutorial.md), [Ekarda](../saas-apps/ekarda-tutorial.md), [MailGates](../saas-apps/mailgates-tutorial.md), [BullseyeTDP](../saas-apps/bullseyetdp-tutorial.md), [finketa](../saas-apps/raketa-tutorial.md), [segment](../saas-apps/segment-tutorial.md), [AI denetleyicisi](https://www.mindbridge.ai/products/ai-auditor/), [Pobuca Connect](https://app.pobu.ca/), [proto.io](../saas-apps/proto.io-tutorial.md), [Gatekeeper](https://www.gatekeeperhq.com/), [Merkez planlayıcısı](../saas-apps/hub-planner-tutorial.md), [ansıra-iş ortağı go-to-market](https://ansira.com/technology/channel-engagement), [bulutta IBM dijital iş Otomasyonu](../saas-apps/ibm-digital-business-automation-on-cloud-tutorial.md), [kişi fiziksel güvenliği](../saas-apps/kisi-physical-security-tutorial.md), [viewpointon](https://team.viewpoint.com/), [ıntelligencebank](../saas-apps/intelligencebank-tutorial.md), [pyölçümler](../saas-apps/pymetrics-tutorial.md), [sıfır](https://www.teamzero.com/), [ınstation](https://instation.invillia.com/), [EDX for Business SAML 2,0 tümleştirmesi](../saas-apps/edx-for-business-saml-integration-tutorial.md), [MOOC Office 365](https://mooc.office365-training.com/en/), [SmartKargo](../saas-apps/smartkargo-tutorial.md), [pkııgmpplatform](https://platform.pkisigning.nl/), [siteıntel](../saas-apps/siteintel-tutorial.md), [alan kimliği](../saas-apps/field-id-tutorial.md), [Curricula SAML](../saas-apps/curricula-saml-tutorial.md), [Perforce Helix Core-Helix Authentication Service](../saas-apps/perforce-helix-core-tutorial.md), [myuyumluluk bulutu](https://cloud.metacompliance.com/), [smallstep SSH](https://smallstep.com/sso-ssh/)  

Ayrıca, tüm uygulamaların belgelerini buradan bulabilirsiniz https://aka.ms/AppsTutorial . Uygulamanızı Azure AD uygulama galerisinde listelemek için lütfen buradaki ayrıntıları okuyun: https://aka.ms/AzureADAppRequest .

---

### <a name="api-connectors-for-external-identities-self-service-sign-up-are-now-in-public-preview"></a>Dış kimlikler self servis kaydolma için API bağlayıcıları artık genel önizlemede

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** EAı  
**Ürün yeteneği:** B2B/B2C
 
Dış kimlikler API bağlayıcıları, self servis kaydolma 'yı dış bulut sistemleriyle tümleştirmenizi sağlayan Web API 'Lerinden yararlanmanızı sağlar. Bu, artık bulut tabanlı özel iş akışlarını tetiklemek için bir kayıt akışında özel adımlar olarak Web API 'Leri çağırabileceğiniz anlamına gelir. Örneğin, API bağlayıcılarını kullanarak şunları yapabilirsiniz:

- Özel bir onay iş akışlarıyla tümleştirin.
- Kimlik doğrulama gerçekleştir
- Kullanıcı giriş verilerini doğrulama
- Kullanıcı özniteliklerinin üzerine yaz
- Özel iş mantığını Çalıştır

API bağlayıcılarıyla ilgili tüm deneyimler hakkında daha fazla bilgi için bkz. [API bağlayıcılarını özelleştirmek ve genişletmek için API bağlayıcıları kullanma](../external-identities/api-connectors-overview.md)veya [Web API 'si tümleştirmelerine sahip dış kimlikler self servis kaydolma](https://techcommunity.microsoft.com/t5/azure-active-directory-identity/customize-external-identities-self-service-sign-up-with-web-api/ba-p/1257364#.XvNz2fImuQg.linkedin).
 
---

### <a name="provision-on-demand-and-get-users-into-your-apps-in-seconds"></a>İsteğe bağlı sağlayın ve uygulamaları saniyeler içinde alın

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Uygulama sağlama  
**Ürün yeteneği:** Kimlik yaşam döngüsü yönetimi
 
Azure AD sağlama hizmeti şu anda döngüsel olarak çalışır. Hizmet, her 40 dakikada bir çalışır. [İsteğe bağlı sağlama özelliği](https://aka.ms/provisionondemand) , bir Kullanıcı seçmenizi ve saniyeler içinde sağlamanızı sağlar. Bu özellik, sağlama döngüsünün yeniden başlatılmasına zorlamak için yeniden başlatma işlemi yapmanıza gerek kalmadan sağlama sorunlarını hızla giderebilmenizi sağlar. 
 
---

### <a name="new-permission-for-using-azure-ad-entitlement-management-in-graph"></a>Graph 'ta Azure AD Yetkilendirme Yönetimi 'ni kullanmaya yönelik yeni izin

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Farklı  
**Ürün yeteneği:** Yetkilendirme Yönetimi
 
Yeni bir Temsilcili izin EntitlementManagement. Read. All artık Microsoft Graph Beta 'da yetkilendirme yönetimi API 'SI ile kullanılabilir. Kullanılabilir API 'Ler hakkında daha fazla bilgi edinmek için bkz. [Azure AD yetkilendirme yönetimi API 'si Ile çalışma](/graph/api/resources/entitlementmanagement-root?view=graph-rest-beta).

---

### <a name="identity-protection-apis-available-in-v10"></a>V 1.0 'da kimlik koruması API 'Leri kullanılabilir

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kimlik koruması  
**Ürün yeteneği:** Kimlik güvenliği & koruması
 
RiskyUsers ve riskDetections Microsoft Graph API 'Leri artık genel kullanıma sunulmuştur. Artık v 1.0 uç noktasında kullanılabilir olduğuna göre, bunları üretimde kullanmak için davet ediyoruz. Daha fazla bilgi için lütfen [Microsoft Graph belgelerine](/graph/api/resources/identityprotectionroot?view=graph-rest-1.0)göz atın.
 
---

### <a name="sensitivity-labels-to-apply-policies-to-microsoft-365-groups-is-now-generally-available"></a>Microsoft 365 gruplara ilke uygulamak için duyarlılık etiketleri genel kullanıma sunuldu

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Grup Yönetimi  
**Ürün yeteneği:** İş
 

Artık duyarlılık etiketleri oluşturabilir ve Gizlilik (genel veya özel) ve dış Kullanıcı erişimi ilkesi gibi Microsoft 365 gruplarına ilke uygulamak için etiket ayarlarını kullanabilirsiniz. Gizli olması için Gizlilik ilkesiyle birlikte bir etiket ve Konuk kullanıcılar eklemeye izin vermek için ise harici Kullanıcı erişim ilkesi oluşturabilirsiniz. Bir Kullanıcı bu etiketi bir gruba uygularsa, Grup özel olur ve gruba hiçbir Konuk kullanıcının eklenmesine izin verilmez. 

Duyarlılık etiketleri, iş açısından kritik verilerinizi korumak ve grupları uyumlu ve güvenli bir biçimde yönetmenize olanak tanımak için önemlidir. Duyarlılık etiketlerini kullanma hakkında yönergeler için, [Azure Active Directory (Önizleme) Microsoft 365 gruplara duyarlılık etiketleri atama](../users-groups-roles/groups-assign-sensitivity-labels.md)bölümüne bakın.
 
---

### <a name="updates-to-support-for-microsoft-identity-manager-for-azure-ad-premium-customers"></a>Azure AD Premium müşterileri için Microsoft Identity Manager Destek güncelleştirmeleri

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Microsoft Identity Manager  
**Ürün yeteneği:** Kimlik yaşam döngüsü yönetimi
 
Azure desteği, Microsoft Identity Manager 2016 için genişletilmiş destek sonuna kadar Microsoft Identity Manager 2016 ' nin Azure AD tümleştirme bileşenleri ile sunulmaktadır. [Microsoft Identity Manager kullanarak Azure AD Premium müşterileri Için destek güncelleştirmesinde](/microsoft-identity-manager/support-update-for-azure-active-directory-premium-customers)daha fazla bilgi edinin.

---

### <a name="the-use-of-group-membership-conditions-in-sso-claims-configuration-is-increased"></a>SSO talep yapılandırmasındaki grup üyeliği koşullarının kullanımı artırıldı

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** SSO
 
Daha önce, tek bir uygulama yapılandırması içindeki grup üyeliğine göre talepleri koşullu olarak değiştirdiğinizde kullanabileceğiniz grupların sayısı 10 ile sınırlıdır. SSO talep yapılandırmasındaki grup üyeliği koşullarının kullanılması artık maksimum 50 gruba yükselmiştir. Talepleri yapılandırma hakkında daha fazla bilgi için bkz. [Kurumsal uygulamalar SSO talep yapılandırması](../develop/active-directory-saml-claims-customization.md#emitting-claims-based-on-conditions). 

---

### <a name="enabling-basic-formatting-on-the-sign-in-page-text-component-in-company-branding"></a>Şirket markalaması 'nda oturum açma sayfası metin bileşeninde temel biçimlendirmeyi etkinleştirme.

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Kimlik doğrulamaları (oturum açma)  
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması
 
Azure AD/Microsoft 365 oturum açma deneyiminde Şirket markası işlevselliği, müşterinin kalın yazı tipi, altı çizili ve italik gibi köprü ve basit biçimlendirme eklemesine izin verecek şekilde güncelleştirilmiştir. Bu işlevi kullanma hakkında yönergeler için bkz. [kuruluşunuzun Azure Active Directory oturum açma sayfasına marka ekleme](./customize-branding.md).

---

### <a name="provisioning-performance-improvements"></a>Performans iyileştirmeleri sağlama

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Uygulama sağlama  
**Ürün yeteneği:** Kimlik yaşam döngüsü yönetimi
 
Sağlama Hizmeti, [artımlı bir döngüyü](../app-provisioning/how-provisioning-works.md#incremental-cycles) tamamlamaya yönelik süreyi azaltacak şekilde güncelleştirilmiştir. Bu, kullanıcıların ve grupların uygulamalarına daha hızlı sağlandığı anlamına gelir. 6/10/2020 sonra oluşturulan tüm yeni sağlama işleri, performans iyileştirmelerinden otomatik olarak faydalanır. Performans iyileştirmelerinden faydalanmak için 6/10/2020 ' den önce sağlama için yapılandırılmış tüm uygulamaların, 6/10/2020 ' den sonra yeniden başlatılması gerekir. 

---

### <a name="announcing-the-deprecation-of-adal-and-ms-graph-parity"></a>ADAL ve MS Graf eşliği kullanımdan kaldırılması duyurusu

**Şunu yazın:** Kullanım dışı  
**Hizmet kategorisi:** yok  
**Ürün yeteneği:** Cihaz yaşam döngüsü yönetimi

Artık Microsoft kimlik doğrulama kitaplıkları (MSAL) kullanılabilir olduğuna göre, artık Azure Active Directory kimlik doğrulama kitaplıklarına (ADAL) yeni özellikler ekleyeceğiz ve 30 Haziran 2022 ' de güvenlik düzeltme eklerini sona erdirmek için. MSAL ' ye geçiş hakkında daha fazla bilgi için bkz. [uygulamaları Microsoft kimlik doğrulama kitaplığı 'Na geçirme (msal)](../develop/msal-migration.md).

Ayrıca, tüm Azure AD Graph işlevlerini MS Graph aracılığıyla kullanılabilir hale getirmek için çalışmayı tamamladık. Bu nedenle, Azure AD Graph API 'Leri 30 Haziran 2022 ' e kadar yalnızca bugdüzeltmesini ve güvenlik düzeltmelerini alacak. Daha fazla bilgi için bkz. [Microsoft kimlik doğrulama kitaplığı ve apı Microsoft Graph kullanmak için uygulamalarınızı güncelleştirme](https://techcommunity.microsoft.com/t5/azure-active-directory-identity/update-your-applications-to-use-microsoft-authentication-library/ba-p/1257363)
 
---
 
## <a name="may-2020"></a>Mayıs 2020

### <a name="retirement-of-properties-in-signins-riskyusers-and-riskdetections-apis"></a>SignIns, riskyUsers ve riskDetections API 'Lerinde özelliklerin kullanımdan kaldırılması

**Şunu yazın:** Değişiklik planı  
**Hizmet kategorisi:** Kimlik koruması  
**Ürün yeteneği:** Kimlik güvenliği & koruması

Şu anda numaralandırılmış türler, riskType özelliğini hem riskDetections API hem de Riskyuseriztem (önizlemede) içinde temsil etmek için kullanılır. Numaralandırılmış türler Ayrıca signIns API 'sindeki riskEventTypes özelliği için de kullanılır. İleri giderek bu özellikleri dizeler olarak temsil edeceğiz. 

Müşteriler Beta riskDetections ve Riskyuseriztem API 'sindeki riskEventType özelliğine ve 9 Eylül 2020 ' e kadar Beta signIns API 'sindeki riskEventTypes_v2 özelliğine geçiş yapılmalıdır. Bu tarihte, geçerli riskType ve riskEventTypes özelliklerini devre dışı bırakılacağız. Daha fazla bilgi için [Microsoft Graph risk olayı özelliklerine ve kimlik koruması API 'lerine yönelik değişikliklere](https://developer.microsoft.com/graph/blogs/changes-to-risk-event-properties-and-identity-protection-apis-on-microsoft-graph/)bakın.

--- 

### <a name="deprecation-of-riskeventtypes-property-in-signins-v10-api-on-microsoft-graph"></a>SignIns v 1.0 API 'sindeki riskEventTypes özelliğinin kullanımdan kaldırılması Microsoft Graph

**Şunu yazın:** Değişiklik planı  
**Hizmet kategorisi:** Rapor  
**Ürün yeteneği:** Kimlik güvenliği & koruması

Numaralandırılmış türler Microsoft Graph Eylül 2020 ' deki risk olayı özelliklerini temsil edildiğinde dize türlerine geçiş yapar. Bu değişiklik, önizleme API 'Lerini etkilemenin yanı sıra üretim içi signIns API 'sini de etkileyecektir.

SignIns v 1.0 API 'sine yeni bir riskEventsTypes_v2 (String) özelliği ekledik. Geçerli riskEventTypes (enum) özelliğini 11 Haziran 2022 ' de kullanımdan kaldıracağız ve Microsoft Graph kullanım dışı bırakma ilkenize uygun şekilde devre dışı bırakacağız. Müşteriler, 11 Haziran 2022 ' e kadar v 1.0 signIns API 'sindeki riskEventTypes_v2 özelliğine geçiş yapılmalıdır. Daha fazla bilgi için [Microsoft Graph signIns v 1.0 API 'Sindeki riskEventTypes özelliğinin kullanımdan kaldırılması](https://developer.microsoft.com/graph/blogs/deprecation-of-riskeventtypes-property-in-signins-v1-0-api-on-microsoft-graph//)bölümüne bakın.

--- 

### <a name="upcoming-changes-to-mfa-email-notifications"></a>MFA e-posta bildirimlerinde yaklaşan değişiklikler

**Şunu yazın:** Değişiklik planı  
**Hizmet kategorisi:** MFA  
**Ürün yeteneği:** Kimlik güvenliği & koruması
 

Cloud MFA için e-posta bildirimlerinde aşağıdaki değişiklikleri yapıyoruz:

E-posta bildirimleri şu adresten gönderilir: azure-noreply@microsoft.com ve msonlineservicesteam@microsoftonline.com . Dolandırıcıların engellemesini kaldırmak için gerekli adımları daha iyi göstermek üzere sahtekarlık uyarı e-postalarının içeriğini güncelleştiriyoruz.

---

### <a name="new-self-service-sign-up-for-users-in-federated-domains-who-cant-access-microsoft-teams-because-they-arent-synced-to-azure-active-directory"></a>Azure Active Directory eşitlenmediği için, Microsoft ekiplerine erişemeyen Federasyon etki alanlarında bulunan kullanıcılar için yeni self servis kaydolun.

**Şunu yazın:** Değişiklik planı  
**Hizmet kategorisi:** Kimlik doğrulamaları (oturum açma)  
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması
 

Şu anda, Azure AD 'de Federasyon etki alanlarında bulunan ancak kiracı ile eşitlenmemiş olan kullanıcılar takımlara erişemez. Haziran 'un sonundan itibaren, bu yeni özellik, var olan e-posta doğrulanmış kaydolma özelliğini genişleterek bunu yapabilmesini sağlayacak. Bu, Federasyon IDP 'de oturum açabilen ancak henüz Azure KIMLIĞINDE bir kullanıcı nesnesine sahip olmayan ve takımlar için otomatik olarak oluşturulmuş bir kullanıcı nesnesine sahip olmayan kullanıcılara izin verir. Kullanıcı nesneleri, "self servis kaydolma" olarak işaretlenir. Bu, yönetilen etki alanındaki kullanıcıların yapabileceği ve aynı bayrak kullanılarak denetlenebileceği, var olan bir e-posta doğrulama özelliğinin bir uzantısıdır. Bu değişiklik, aşağıdaki iki ay boyunca kullanıma alınır. Belge güncelleştirmelerini [buradan](../users-groups-roles/directory-self-service-signup.md)izleyin.
 
---

### <a name="upcoming-fix-the-oidc-discovery-document-for-the-azure-government-cloud-is-being-updated-to-reference-the-correct-graph-endpoints"></a>Yaklaşan Düzeltme: Azure Kamu Bulutu için OıDC keşif belgesi, doğru grafik uç noktalarına başvuracak şekilde güncelleştiriliyor.

**Şunu yazın:** Değişiklik planı  
**Hizmet kategorisi:** Bağımsız bulutlar  
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması
 
Haziran 'dan itibaren, [Azure Kamu Bulutu](../develop/authentication-national-cloud.md) uç noktasındaki (login.microsoftonline.us) OIDC keşif belgesi [Microsoft Identity platform ve OpenID Connect Protokolü](../develop/v2-protocols-oidc.md) , doğru [National Cloud Graph](/graph/deployments) uç noktasını ( https://graph.microsoft.us veya https://dod-graph.microsoft.us) belirtilen kiracıya göre) döndürmeye başlayacaktır.  Şu anda hatalı grafik uç noktası (graph.microsoft.com) "msgraph_host" alanı sağlar.  

Bu hata düzeltmesinin süresi yaklaşık 2 aydan fazla olacak şekilde alınacaktır.  

---

### <a name="azure-government-users-will-no-longer-be-able-to-sign-in-on-loginmicrosoftonlinecom"></a>Azure Kamu kullanıcıları artık login.microsoftonline.com üzerinde oturum açamaz

**Şunu yazın:** Değişiklik planı  
**Hizmet kategorisi:** Bağımsız bulutlar  
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması
 
1 Haziran 2018 ' de, Azure Kamu için resmi Azure Active Directory (Azure AD) yetkilisi ' den ' a değiştirilir https://login-us.microsoftonline.com https://login.microsoftonline.us . Bir Azure Kamu kiracısı içindeki bir uygulamaya sahipseniz, uygulamanızı. us uç noktasındaki kullanıcıları imzalamak için güncelleştirmeniz gerekir.

Azure AD, Mayıs 5 ' ten itibaren, Azure Kamu kullanıcılarının genel uç nokta (microsoftonline.com) kullanılarak Azure Kamu kiracılarında barındırılan uygulamalarda oturum açmasını engelleyen bitiş noktası değişikliğini zorlamaya başlayacak. Etkilenen uygulamalar, AADSTS900439-USGClientNotSupportedOnPublicEndpoint hatasını görmeye başlayacaktır. 

Bu değişikliğin, Haziran 2020 ' de tüm uygulamalarda tamamlanamamış olması beklendiğinden aşamalı olarak bir dağıtım olması gerekir. Daha fazla ayrıntı için lütfen [Azure Kamu blog gönderisine](https://devblogs.microsoft.com/azuregov/azure-government-aad-authority-endpoint-update/)bakın.

---

### <a name="saml-single-logout-request-now-sends-nameid-in-the-correct-format"></a>SAML çoklu oturum kapatma isteği artık NameID 'yi doğru biçimde gönderiyor

**Şunu yazın:** Düzenle  
**Hizmet kategorisi:** Kimlik doğrulamaları (oturum açma)  
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması
 
Kullanıcı, oturum kapatma (örneğin, Uygps portalındaki) oturumunu tıklattığında, Azure AD kullanıcı oturumunda etkin olan ve oturum kapatma URL 'SI yapılandırılmış her uygulamaya bir SAML Single Logout iletisi gönderir. Bu iletiler kalıcı biçimde bir NameID içerir.

Özgün SAML oturum açma belirteci, NameID (örn. e-posta/UPN) için farklı bir biçim kullanıyorsa, SAML uygulaması oturum kapatma iletisindeki NameID 'yi mevcut bir oturumla ilişkilendiremiyor (her iki iletide kullanılan Nameıds farklı olduğu için), bu da oturum kapatma iletisinin SAML uygulaması ve Kullanıcı oturum açmış durumda kalmasını sağlar. Bu onarım, oturum kapatma iletisinin uygulama için yapılandırılan NameID ile tutarlı olmasını sağlar.

---

### <a name="hybrid-identity-administrator-role-is-now-available-with-cloud-provisioning"></a>Karma kimlik yöneticisi rolü artık bulut sağlaması ile kullanılabilir

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Azure AD bulutu sağlama  
**Ürün yeteneği:** Kimlik yaşam döngüsü yönetimi
 
BT yöneticileri yeni "karma yönetici" rolünü, Azure ADConnect bulut sağlamasını ayarlamaya yönelik en düşük ayrıcalıklı rol olarak kullanmaya başlayabilir. Bu yeni rolle, artık bulut sağlamasını ayarlamak ve yapılandırmak için genel yönetici rolünü kullanmanız gerekmez. [Daha fazla bilgi edinin](../users-groups-roles/roles-delegate-by-task.md#connect).
 
---

### <a name="new-federated-apps-available-in-azure-ad-application-gallery---may-2020"></a>Azure AD uygulama galerisinde yeni Federasyon uygulamaları kullanılabilir-Mayıs 2020

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi
 
2020 Mayıs 'ta, federasyon desteğiyle uygulama galerimize aşağıdaki 36 yeni uygulamaları ekledik:

[Moula](https://moula.com.au/pay/merchants), [gözetimi ypal](https://www.surveypal.com/app), [Kbot365](https://www.konverso.ai/virtual-assistant-digital-workplace/), [tacklebox](http://www.tacklebox.app/), [Powell ekipleri](https://powell-software.com/en/powell-teams-en/), [talentsoft Yardımcısı](https://msteams.talent-soft.com/), [ASC kayıt öngörüleri](https://teams.asc-recording.app/product), [GO1](https://www.go1.com/), [B-bağlı](https://b-engaged.se/), [Competella Contact Center çalışma grubu](http://www.competella.com/), [asite](http://www.asite.com/), [ımavals kimliği](https://identity.imagesoftinc.com/), [ibısworld](https://identity.imagesoftinc.com/), [ınsuite](../saas-apps/insuite-tutorial.md), [değişiklik işlemi yönetimi](../saas-apps/change-process-management-tutorial.md), [Cyara CX güvence platformu](../saas-apps/cyara-cx-assurance-platform-tutorial.md), [Smart Global idare](../saas-apps/smart-global-governance-tutorial.md), [PREZI](../saas-apps/prezi-tutorial.md), [Mapbox](../saas-apps/mapbox-tutorial.md), [datava Enterprise Service platform](../saas-apps/datava-enterprise-service-platform-tutorial.md), [Whimsical](../saas-apps/whimsical-tutorial.md), [trelica](../saas-apps/trelica-tutorial.md), [Tsysso](../saas-apps/easysso-for-confluence-tutorial.md)for the [Bitbucket](../saas-apps/easysso-for-bitbucket-tutorial.md)için easysso, [Bamboo](../saas-apps/easysso-for-bamboo-tutorial.md), [Torii](../saas-apps/torii-tutorial.md), [axiad bulutu](../saas-apps/axiad-cloud-tutorial.md), [humanage](../saas-apps/humanage-tutorial.md), [colortokens ztna](../saas-apps/colortokens-ztna-tutorial.md), [CCH Tagetik](../saas-apps/cch-tagetik-tutorial.md), [sharekasa,](../saas-apps/sharevault-tutorial.md) [Vyond](../saas-apps/vyond-tutorial.md), [textexpander](../saas-apps/textexpander-tutorial.md), bir [Ev CRM](../saas-apps/anyone-home-crm-tutorial.md) [,](../saas-apps/ice-contact-center-tutorial.md) [askbağlı](../saas-apps/askspoke-tutorial.md)

Ayrıca, tüm uygulamaların belgelerini buradan bulabilirsiniz https://aka.ms/AppsTutorial .

Uygulamanızı Azure AD uygulama galerisinde listelemek için lütfen buradaki ayrıntıları okuyun https://aka.ms/AzureADAppRequest .

---

### <a name="report-only-mode-for-conditional-access-is-now-generally-available"></a>Koşullu erişim için yalnızca rapor modu genel kullanıma sunuldu

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Koşullu erişim  
**Ürün yeteneği:** Kimlik güvenliği & koruması

[Azure AD koşullu erişim için yalnızca rapor modu](../conditional-access/concept-conditional-access-report-only.md) , erişim denetimlerini zorlamadan bir ilkenin sonucunu değerlendirmenizi sağlar. Kuruluşunuzda yalnızca rapor ilkelerini test edebilir ve etkinleştirilmeden önce etkilerini anlayabilmeniz, dağıtımın daha güvenli ve kolay olmasını sağlayabilirsiniz. Son birkaç aya göre yalnızca rapor modunun güçlü şekilde benimsenmesini gördük. 26 ' dan fazla kullanıcı zaten rapor ilkesi kapsamında. Günümüzde duyuru sayesinde, yeni Azure AD koşullu erişim ilkeleri varsayılan olarak yalnızca rapor modunda oluşturulur. Bu, ilkelerinizin oluşturuldukları andan itibaren etkilerini izleyebilmeniz anlamına gelir. Ayrıca, MS Graph API 'Lerini kullananlar için [yalnızca rapor ilkelerini](/graph/api/resources/conditionalaccesspolicy?view=graph-rest-beta) de daha fazla yönetebilirsiniz. 

---

### <a name="self-service-sign-up-for-guest-users"></a>Konuk kullanıcılar için self servis kaydolma

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** EAı  
**Ürün yeteneği:** B2B/B2C
 
Azure AD 'de dış kimliklerle, kuruluşunuzun dışındaki kişilerin, tercih ettikleri kimliği kullanarak oturum açmalarına izin vererek uygulamalarınıza ve kaynaklarınıza erişmesini sağlayabilirsiniz. Bir uygulamayı dış kullanıcılarla paylaşırken, uygulamaya erişmesi gereken her zaman önceden bilmiyor olabilirsiniz. [Self servis kaydolma](../external-identities/self-service-sign-up-overview.md)sayesinde, konuk kullanıcıların kaydolup iş kolu (LOB) Uygulamalarınız için bir Konuk hesabı elde etmesine olanak sağlayabilirsiniz. Kaydolma akışı, Azure AD ve sosyal kimlik kimliklerini destekleyecek şekilde oluşturulabilir ve özelleştirilebilir. Kayıt sırasında Kullanıcı hakkında ek bilgiler de toplayabilirsiniz.

---

 ### <a name="conditional-access-insights-and-reporting-workbook-is-generally-available"></a>Koşullu erişim öngörüleri ve raporlama çalışma kitabı genel kullanıma sunuldu

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Koşullu erişim  
**Ürün yeteneği:** Kimlik güvenliği & koruması

[Öngörüler ve raporlama çalışma kitabı](../conditional-access/howto-conditional-access-insights-reporting.md) , yöneticilere kiracınızda Azure AD koşullu erişiminin Özet görünümünü sağlar. Tek bir ilke seçme özelliği sayesinde, yöneticiler her bir ilkenin ne yaptığını daha iyi anlayabilir ve değişiklikleri gerçek zamanlı olarak izleyebilir. Çalışma kitabı, Azure Izleyici 'de depolanan verileri akışlar ve [Bu yönergeleri izleyerek](../reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md)birkaç dakika içinde ayarlayabilirsiniz. Panoyu daha fazla bulunabilir hale getirmek için Azure AD koşullu erişim menüsündeki yeni Öngörüler ve Raporlama sekmesine taşıdık.

---

### <a name="policy-details-blade-for-conditional-access-is-in-public-preview"></a>Koşullu erişimin ilke ayrıntıları dikey penceresi genel önizlemede

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Koşullu erişim  
**Ürün yeteneği:** Kimlik güvenliği & koruması

Yeni [ilke ayrıntıları dikey](../conditional-access/troubleshoot-conditional-access.md) penceresi, koşullu erişim ilkesi değerlendirmesi sırasında bulunan atamaları, koşulları ve denetimleri görüntüler. Oturum açma ayrıntılarının koşullu erişim veya yalnızca rapor sekmelerinde bulunan bir satırı seçerek dikey pencereye erişebilirsiniz.

---

### <a name="new-query-capabilities-for-directory-objects-in-microsoft-graph-are-in-public-preview"></a>Microsoft Graph dizin nesneleri için yeni sorgu özellikleri genel önizlemede

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** MS Graph **ürün yeteneği:** geliştirici deneyimi

Microsoft Graph dizin nesneleri API 'Leri için yeni yetenekler tanıtılmakta, sayı, arama, filtre ve sıralama işlemleri etkinleştiriliyor. Bu, geliştiricilere bellek içi filtreleme ve sıralama gibi geçici çözümler olmadan dizin nesnelerinizi hızlı bir şekilde sorgulama olanağı sunar. Bu [blog gönderisine](https://aka.ms/CountFilterMSGraphAAD)daha fazla bilgi edinin.

Şu anda genel önizleme aşamasındadır ve geri bildirim aranıyor. Lütfen bu [kısa anketle](https://aka.ms/MsGraphAADSurveyDocs)yorumlarınızı gönderin.

---

### <a name="configure-saml-based-single-sign-on-using-microsoft-graph-api-beta"></a>Microsoft Graph API (Beta) kullanarak SAML tabanlı çoklu oturum açmayı yapılandırma

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** SSO
 
Beta 'daki MS Graph API 'Leri kullanılarak Azure AD galerisinden bir uygulama oluşturma ve yapılandırma desteği artık kullanılabilir. Bir uygulamanın birden çok örneği için SAML tabanlı çoklu oturum açmayı ayarlamanız gerekiyorsa, [SAML tabanlı çoklu oturum açma yapılandırmasını otomatikleştirmek](/graph/application-saml-sso-configure-api)Için Microsoft Graph API 'lerini kullanarak zamandan tasarruf edin.
 
---

### <a name="new-provisioning-connectors-in-the-azure-ad-application-gallery---may-2020"></a>Azure AD uygulama galerisinde yeni sağlama bağlayıcıları-Mayıs 2020

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Uygulama sağlama  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi
 
Artık yeni tümleştirilmiş uygulamalar için Kullanıcı hesapları oluşturmayı, güncellemeyi ve silmeyi otomatik hale getirebilirsiniz:

* [8x8](../saas-apps/8x8-provisioning-tutorial.md)
* [Juno Journey](../saas-apps/juno-journey-provisioning-tutorial.md)
* [MediusFlow](../saas-apps/mediusflow-provisioning-tutorial.md)
* [Kuruluşa göre New Relic](../saas-apps/new-relic-by-organization-provisioning-tutorial.md)
* [Oracle Cloud Infrastructure Console](../saas-apps/oracle-cloud-infratstructure-console-provisioning-tutorial.md)

Otomatik Kullanıcı hesabı sağlamayı kullanarak kuruluşunuzun daha iyi güvenliğini sağlama hakkında daha fazla bilgi için bkz. [Azure AD Ile SaaS uygulamalarına Kullanıcı sağlamayı otomatikleştirme](../app-provisioning/user-provisioning.md).

---

### <a name="saml-token-encryption-is-generally-available"></a>SAML belirteci şifrelemesi genel kullanıma sunuldu

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** SSO
 
[SAML belirteci şifrelemesi](../manage-apps/howto-saml-token-encryption.md) , UYGULAMALARıN şifreli SAML onayları alacak şekilde yapılandırılmasını sağlar. Özelliği artık tüm bulutlarda genel kullanıma sunulmuştur.
 
---

### <a name="group-name-claims-in-application-tokens-is-generally-available"></a>Uygulama belirteçlerinde grup adı talepleri genel kullanıma sunuldu

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** SSO
 
Bir belirteçte verilen grup talepleri artık yalnızca uygulamaya atanan gruplarla sınırlı olabilir.  Bu özellikle, kullanıcılar çok sayıda grubun üyesi olduğunda ve belirteç boyutu sınırlarını aşmamak açısından önemlidir. Bu yeni özellik hazır olduğunda, [belirteçlere grup adları ekleme](../hybrid/how-to-connect-fed-group-claims.md) özelliği genel kullanıma sunulmuştur.
 
---

### <a name="workday-writeback-now-supports-setting-work-phone-number-attributes"></a>Workday geri yazma özelliği artık iş telefonu numarası özniteliklerinin ayarlanmasını destekliyor

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Uygulama sağlama  
**Ürün yeteneği:** Kimlik yaşam döngüsü yönetimi
 
İş günü geri yazma sağlama uygulamasının artık iş telefonu numarası ve mobil numara özniteliklerinin geri yazma işlemlerini desteklemesi için geliştirilmiştir. E-posta ve Kullanıcı adının yanı sıra, artık Workday geri yazma sağlama uygulamasını Azure AD 'den Workday 'e telefon numarası değerlerini Flow şekilde yapılandırabilirsiniz. Telefon numarası geri yazma özelliğinin nasıl yapılandırılacağı hakkında daha fazla bilgi için, [Workday geri yazma](../saas-apps/workday-writeback-tutorial.md) uygulaması öğreticisine bakın. 

---

### <a name="publisher-verification-preview"></a>Yayımcı doğrulaması (Önizleme)

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Farklı  
**Ürün yeteneği:** Geliştirici deneyimi
 
Yayımcı doğrulaması (Önizleme), yöneticilerin ve son kullanıcıların Microsoft Identity platformu ile tümleştirerek uygulama geliştiricilerinin orijinalliğini anlamalarına yardımcı olur. Ayrıntılar için [Yayımcı doğrulama (Önizleme)](../develop/publisher-verification-overview.md)bölümüne bakın.
 
---

### <a name="authorization-code-flow-for-single-page-apps"></a>Tek sayfalı uygulamalar için yetkilendirme kodu akışı

**Şunu yazın:** Değiştirilen özellik **hizmeti kategorisi:** kimlik doğrulama **ürün yeteneği:** geliştirici deneyimi

[Safari IOP gibi modern tarayıcı 3 taraf tanımlama bilgisi kısıtlamaları](../develop/reference-third-party-cookies-spas.md)nedeniyle, maça 'ın SSO 'yu korumak için örtük akış yerine yetkilendirme kodu akışını kullanması gerekecektir; MSAL.js v 2. x artık yetkilendirme kodu akışını destekleyecektir. Azure portal, SPA 'nizi "Spa" türüne ve kimlik doğrulama kod akışını kullanacak şekilde güncelleştirebilmeniz için bunlara karşılık gelen güncelleştirmeler de vardır. Rehberlik için [hızlı başlangıç: Kullanıcı oturum açma ve kimlik doğrulama kod akışını kullanarak bir JAVASCRIPT Spa 'da erişim belirteci edinme](../develop/quickstart-v2-javascript-auth-code.md)bölümüne bakın.

---

### <a name="improved-filtering-for-devices-is-in-public-preview"></a>Cihazlar için geliştirilmiş filtreleme genel önizlemede

**Şunu yazın:** Değiştirilen özellik   
**Hizmet kategorisi:** Cihaz Yönetimi **ürün yeteneği:** cihaz yaşam döngüsü yönetimi
 
Daha önce, kullanabileceğiniz tek filtreler "etkinleştirildi" ve "etkinlik tarihi" olarak değiştirildi. Artık, işletim sistemi türü, JOIN türü, uyumluluk ve daha fazlası dahil olmak üzere [daha fazla özellik için cihaz listenizi filtreleyebilirsiniz](../devices/device-management-azure-portal.md#device-list-filtering-preview). Bu eklemeler belirli bir cihazı konumlandırmayı basitleştirir.

---

### <a name="the-new-app-registrations-experience-for-azure-ad-b2c-is-now-generally-available"></a>Azure AD B2C için yeni Uygulama kayıtları deneyimi artık genel kullanıma sunuldu

**Şunu yazın:** Değiştirilen özellik   
**Hizmet kategorisi:** B2C-tüketici kimlik yönetimi  
**Ürün yeteneği:** Kimlik yaşam döngüsü yönetimi
 
Azure AD B2C için yeni Uygulama kayıtları deneyimi artık genel kullanıma sunulmuştur. 

Daha önce, B2C tüketicisine yönelik uygulamalarınızı eski ' uygulamalar ' deneyimini kullanarak uygulamalarınızın geri kalanından ayrı olarak yönetmeniz gerekiyordu. Bu, Azure 'daki farklı yerlerde farklı uygulama oluşturma deneyimleri sunmaktır.

Yeni deneyim tüm B2C uygulama kayıtlarını ve Azure AD uygulama kayıtlarını tek bir yerde gösterir ve bunları yönetmek için tutarlı bir yol sağlar. Azure AD B2C kaynaklarını programlı bir şekilde yönetmek için Microsoft Graph erişimi olan bir uygulamayı veya bir uygulamayı yönetmeniz gerekip gerekmediğini öğrenmek için yalnızca bir yol yapmanız gerekir.

Azure AD B2C hizmetinde gezinerek ve Uygulama kayıtları dikey penceresini seçerek yeni deneyimle ulaşabilirsiniz. Deneyim Azure Active Directory hizmetinden de erişilebilir.

Azure AD B2C Uygulama kayıtları deneyimi, Azure AD kiracılarına yönelik genel [uygulama kayıt deneyimini](https://developer.microsoft.com/identity/blogs/new-app-registrations-experience-is-now-generally-available/) temel alır, ancak Azure AD B2C için tasarlanmıştır. Eski "uygulamalar" deneyimi gelecekte kullanım dışı olacaktır.

Daha fazla bilgi için [Azure AD B2C Için yeni uygulama kaydı deneyimini](../../active-directory-b2c/app-registrations-training-guide.md)ziyaret edin.

---

## <a name="april-2020"></a>Nisan 2020

### <a name="combined-security-info-registration-experience-is-now-generally-available"></a>Birleşik güvenlik bilgileri kayıt deneyimi artık genel kullanıma sunuldu

**Şunu yazın:** Yeni Özellik

**Hizmet kategorisi:** Kimlik doğrulamaları (oturum açma)

**Ürün yeteneği:** Kimlik güvenliği & koruması

Multi-Factor Authentication (MFA) ve Self-Service parola sıfırlama (SSPR) için Birleşik kayıt deneyimi artık genel kullanıma sunulmuştur. Bu yeni kayıt deneyimi, kullanıcıların MFA ve SSPR 'ye tek bir adım adım işlem halinde kaydolmalarını sağlar. Kuruluşunuz için yeni deneyimi dağıttığınızda, kullanıcılar daha az zaman ve daha az kurtulur kaydedebilir. Blog gönderisine [buradan](https://bit.ly/3etiRyQ)göz atın.

---

### <a name="continuous-access-evaluation"></a>Sürekli erişim değerlendirmesi

**Şunu yazın:** Yeni Özellik

**Hizmet kategorisi:** Kimlik doğrulamaları (oturum açma)

**Ürün yeteneği:** Kimlik güvenliği & koruması

Sürekli erişim değerlendirmesi, Azure AD 'de (Kullanıcı hesabı silme gibi) olaylar gerçekleştiğinde Azure AD erişim belirteçlerini kullanan bağlı olan taraflar üzerinde neredeyse gerçek zamanlı ilke uygulanmasını sağlayan yeni bir güvenlik özelliğidir. Bu özelliği ilk olarak takımlar ve Outlook istemcileri için kullanıma sunuyoruz. Daha fazla ayrıntı için lütfen [blogumuzu](https://techcommunity.microsoft.com/t5/azure-active-directory-identity/moving-towards-real-time-policy-and-security-enforcement/ba-p/1276933) ve  [belgelerimizi](../conditional-access/concept-continuous-access-evaluation.md)okuyun.

---

### <a name="sms-sign-in-firstline-workers-can-sign-in-to-azure-ad-backed-applications-with-their-phone-number-and-no-password"></a>SMS oturum açma: Firstline çalışanları, telefon numarası ve parola olmadan Azure AD ile desteklenen uygulamalarda oturum açabilir

**Şunu yazın:** Yeni Özellik

**Hizmet kategorisi:** Kimlik doğrulamaları (oturum açma)

**Ürün yeteneği:** Kullanıcı kimlik doğrulaması

Office, geleneksel olmayan kuruluşlara ve birincil iletişim yöntemi olarak e-posta kullanmayan büyük kuruluşlardaki çalışanlara yönelik bir dizi mobil ilk iş uygulaması başlatıyor. Bu uygulamalar, kendi işverenlerinden bir e-posta adresi alamaz, bir bilgisayara veya bu uygulamalara erişim sahibi olmayan çalışanlar, deskuz çalışanları, alan aracılarını veya perakende çalışanları hedefleyebilir. Bu proje, bu çalışanların bir telefon numarası girerek ve bir kodu aşağı girerken iş uygulamalarında oturum açmalarına olanak tanır. Daha fazla ayrıntı için lütfen [yönetici belgelerimize](../authentication/howto-authentication-sms-signin.md) ve [Son Kullanıcı belgelerinize](../user-help/sms-sign-in-explainer.md)bakın.

---

### <a name="invite-internal-users-to-use-b2b-collaboration"></a>B2B işbirliği kullanan dahili kullanıcıları davet etme

**Şunu yazın:** Yeni Özellik

**Hizmet kategorisi:** EAı

**Ürün yeteneği:**

Mevcut iç hesapların, daha sonra B2B işbirliği kimlik bilgilerini kullanacak şekilde davet edilmesine izin vermek için B2B davetiyesi özelliğini genişlettik. Bu, davet edilen e-posta adresi gibi tipik parametrelere ek olarak Kullanıcı nesnesi davet API 'sine geçirilerek yapılır. Kullanıcının nesne KIMLIĞI, UPN, Grup üyeliği, uygulama ataması, vb. değişmeden kalır, ancak iletme, bir yandan, Davetten önce kullandıkları iç kimlik bilgileri yerine kendi giriş Kiracı kimlik bilgileriyle kimlik doğrulaması yapmak için B2B kullanır. Ayrıntılar için [belgelerine](../external-identities/invite-internal-users.md)bakın.

---

### <a name="report-only-mode-for-conditional-access-is-now-generally-available"></a>Koşullu erişim için yalnızca rapor modu genel kullanıma sunuldu

**Şunu yazın:** Yeni Özellik

**Hizmet kategorisi:** Koşullu erişim

**Ürün yeteneği:** Kimlik güvenliği & koruması

[Azure AD koşullu erişim için yalnızca rapor modu](../conditional-access/concept-conditional-access-report-only.md) , erişim denetimlerini zorlamadan bir ilkenin sonucunu değerlendirmenizi sağlar. Kuruluşunuzda yalnızca rapor ilkelerini test edebilir ve etkinleştirilmeden önce etkilerini anlayabilmeniz, dağıtımın daha güvenli ve kolay olmasını sağlayabilirsiniz. Son birkaç ay içinde, yalnızca rapor ilkesi kapsamında, 26 ' dan fazla Kullanıcı önceden rapor modunun güçlü şekilde benimsenmesini gördük. Bu duyuru ile yeni Azure AD koşullu erişim ilkeleri varsayılan olarak yalnızca rapor modunda oluşturulur. Bu, ilkelerinizin oluşturuldukları andan itibaren etkilerini izleyebilmeniz anlamına gelir. Ayrıca, MS Graph API 'Lerini kullananlar için, [yalnızca rapor ilkelerini programlı](/graph/api/resources/conditionalaccesspolicy?view=graph-rest-beta)bir şekilde yönetebilirsiniz. 

---

### <a name="conditional-access-insights-and-reporting-workbook-is-generally-available"></a>Koşullu erişim öngörüleri ve raporlama çalışma kitabı genel kullanıma sunuldu

**Şunu yazın:** Yeni Özellik

**Hizmet kategorisi:** Koşullu erişim

**Ürün yeteneği:** Kimlik güvenliği & koruması

Koşullu erişim [öngörüleri ve raporlama çalışma kitabı](../conditional-access/howto-conditional-access-insights-reporting.md) , yöneticilere kiracınızda Azure AD koşullu erişiminin Özet görünümünü sağlar. Tek bir ilke seçme özelliği sayesinde, yöneticiler her bir ilkenin ne yaptığını daha iyi anlayabilir ve değişiklikleri gerçek zamanlı olarak izleyebilir. Çalışma kitabı, Azure Izleyici 'de depolanan verileri akışlar ve [Bu yönergeleri izleyerek](../reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md)birkaç dakika içinde ayarlayabilirsiniz. Panoyu daha fazla bulunabilir hale getirmek için Azure AD koşullu erişim menüsündeki yeni Öngörüler ve Raporlama sekmesine taşıdık.

---

### <a name="policy-details-blade-for-conditional-access-is-in-public-preview"></a>Koşullu erişimin ilke ayrıntıları dikey penceresi genel önizlemede

**Şunu yazın:** Yeni Özellik

**Hizmet kategorisi:** Koşullu erişim

**Ürün yeteneği:** Kimlik güvenliği & koruması

Yeni [ilke ayrıntıları dikey](../conditional-access/troubleshoot-conditional-access.md) penceresi, koşullu erişim ilkesi değerlendirmesi sırasında hangi atamaların, koşulların ve denetimlerin karşılanmasını görüntüler. Oturum açma ayrıntılarının **koşullu erişim** veya **yalnızca rapor** sekmelerinde bulunan bir satırı seçerek dikey pencereye erişebilirsiniz.

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---april-2020"></a>Azure AD Uygulaması galerisinde yeni Federasyon uygulamaları kullanılabilir-Nisan 2020

**Şunu yazın:** Yeni Özellik

**Hizmet kategorisi:** Kurumsal uygulamalar

**Ürün yeteneği:** üçüncü taraf tümleştirmesi

2020 Nisan 'da, uygulama galerisine federasyon desteği olan bu 31 yeni uygulamayı ekledik: 

Bol [Ropool Apps](https://www.sincropool.com/), [smartdb](https://hibiki.dreamarts.co.jp/smartdb/trial/), [float](../saas-apps/float-tutorial.md), [LMS365](https://lms.365.systems/), [IWT tedarik Suite](../saas-apps/iwt-procurement-suite-tutorial.md), [lunnı](https://lunni.fi/), [easysso, jira](../saas-apps/easysso-for-jira-tutorial.md), [sanal eğitim akademik My](https://vta.c3p.ca/app/en/openid?authenticate_with=microsoft), [Meraki panosu](../saas-apps/meraki-dashboard-tutorial.md), [Microsoft 365 taşıyıcısı](https://app.mover.io/login), [Konuşmacı](https://speakerengage.com/login.php)katılım, [his](../saas-apps/honestly-tutorial.md), [önemli](../saas-apps/ally-tutorial.md), [DutyFlow](https://app.dutyflow.nl/), [Alertmedia](../saas-apps/alertmedia-tutorial.md), [gr8 kişileri](../saas-apps/gr8-people-tutorial.md), [pendo](../saas-apps/pendo-tutorial.md), [üst](../saas-apps/highground-tutorial.md)düzey, [uyum](../saas-apps/harmony-tutorial.md), [timetabling çözümleri](../saas-apps/timetabling-solutions-tutorial.md), [Synchronet tıklama](../saas-apps/synchronet-click-tutorial.md), [Empower](https://www.made-in-office.com/en/), [ktes değişiklik bulutu](../saas-apps/fortes-change-cloud-tutorial.md), [Litmus](../saas-apps/litmus-tutorial.md), [grouptalk](https://recorder.grouptalk.com/), [mintify](../saas-apps/frontify-tutorial.md), [MongoDB Cloud](../saas-apps/mongodb-cloud-tutorial.md), [tickitlms öğreni](../saas-apps/tickitlms-learn-tutorial.md), [Coco](https://hexaware.com/partnerships-and-alliances/digital-transformation-using-microsoft-azure/), [Nitro üretkenlik Suite](../saas-apps/nitro-productivity-suite-tutorial.md) , [Trend Micro Web Security (tmws)](https://review.docs.microsoft.com/azure/active-directory/saas-apps/trend-micro-tutorial)

Uygulamalar hakkında daha fazla bilgi için, bkz. [Azure Active Directory SaaS uygulama tümleştirmesi](../saas-apps/tutorial-list.md). Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi için, [Azure Active Directory Uygulama galerisinde uygulamanızı](../azuread-dev/howto-app-gallery-listing.md)listeleme bölümüne bakın.

---

### <a name="microsoft-graph-delta-query-support-for-oauth2permissiongrant-available-for-public-preview"></a>OAuth2PermissionGrant için Delta sorgu desteği genel önizlemeye sunuldu Microsoft Graph

**Şunu yazın:** Yeni Özellik

**Hizmet kategorisi:** MS grafiği

**Ürün yeteneği:** Geliştirici deneyimi

OAuth2PermissionGrant için Delta sorgusu genel önizleme için kullanılabilir! Artık Microsoft Graph sürekli olarak yoklama yapmadan değişiklikleri izleyebilirsiniz. [Daha fazla bilgi edinin.](/graph/api/oAuth2PermissionGrant-delta?tabs=http&view=graph-rest-beta)

---

### <a name="microsoft-graph-delta-query-support-for-organizational-contact-generally-available"></a>Kurumsal iletişim için Microsoft Graph Delta sorgu desteği genel kullanıma sunuldu

**Şunu yazın:** Yeni Özellik

**Hizmet kategorisi:** MS grafiği

**Ürün yeteneği:** Geliştirici deneyimi

Kurumsal kişiler için Delta sorgusu genel kullanıma sunulmuştur! Artık, Microsoft Graph sürekli olarak yoklama yapmadan üretim uygulamalarındaki değişiklikleri izleyebilirsiniz. Performansı önemli ölçüde artırmak için, orgContact verilerini Delta sorgusuna sürekli yoklayan mevcut herhangi bir kodu değiştirin. [Daha fazla bilgi edinin.](/graph/api/orgcontact-delta?tabs=http&view=graph-rest-1.0)

---

### <a name="microsoft-graph-delta-query-support-for-application-generally-available"></a>Uygulama için Microsoft Graph Delta sorgu desteği genel kullanıma sunuldu

**Şunu yazın:** Yeni Özellik

**Hizmet kategorisi:** MS grafiği

**Ürün yeteneği:** Geliştirici deneyimi

Uygulamalar için Delta sorgusu genel kullanıma sunulmuştur! Artık, Microsoft Graph sürekli olarak yoklama yapmadan üretim uygulamalarındaki değişiklikleri izleyebilirsiniz. Performansı önemli ölçüde artırmak için uygulama verilerini Delta sorgusuna göre sürekli yoklayan mevcut herhangi bir kodu değiştirin. [Daha fazla bilgi edinin.](/graph/api/application-delta?view=graph-rest-1.0)

---

### <a name="microsoft-graph-delta-query-support-for-administrative-units-available-for-public-preview"></a>Genel önizleme için kullanılabilen yönetim birimlerine yönelik Delta sorgu desteğini Microsoft Graph

**Şunu yazın:** Yeni Özellik

**Hizmet kategorisi:** MS grafiği

**Ürün yeteneği:** Yönetim birimleri için geliştirici deneyimi Delta sorgusu genel önizleme için kullanılabilir! Artık Microsoft Graph sürekli olarak yoklama yapmadan değişiklikleri izleyebilirsiniz. [Daha fazla bilgi edinin.](/graph/api/administrativeunit-delta?tabs=http&view=graph-rest-beta)

---

### <a name="manage-authentication-phone-numbers-and-more-in-new-microsoft-graph-beta-apis"></a>Yeni Microsoft Graph Beta API 'Lerinde kimlik doğrulama telefon numaralarını ve daha fazlasını yönetme

**Şunu yazın:** Yeni Özellik

**Hizmet kategorisi:** MS grafiği

**Ürün yeteneği:** Geliştirici deneyimi

Bu API 'Ler, kullanıcılarınızın kimlik doğrulama yöntemlerini yönetmeye yönelik bir temel araçtır. Artık MFA ve self servis parola sıfırlama (SSPR) için kullanılan kimlik doğrulayıcılar aracılığıyla programlı olarak kayıt yapabilir ve yönetebilirsiniz. Bu, Azure MFA, SSPR ve Microsoft Graph alanlarında en çok istenen özelliklerden biridir. Bu dalgaya yayımladığımız yeni API 'Ler şunları yapabilmenizi sağlar:

- Kullanıcının kimlik doğrulama telefonlarını okuma, ekleme, güncelleştirme ve kaldırma
- Kullanıcının parolasını sıfırlama
- SMS 'yi açma ve kapatma-oturum açma

Daha fazla bilgi için bkz. [Azure AD kimlik doğrulama yöntemleri API 'sine genel bakış](/graph/api/resources/authenticationmethods-overview?view=graph-rest-beta).

---

### <a name="administrative-units-public-preview"></a>Yönetim birimleri genel önizleme

**Şunu yazın:** Yeni Özellik

**Hizmet kategorisi:** Azure AD rolleri

**Ürün yeteneği:** Access Control

Yönetim birimleri bir departman, bölge veya tanımladığınız kuruluşunuzun diğer kesimiyle kısıtlanan yönetici izinleri vermenizi sağlar. Bölgesel yöneticilerle izinleri devretmek veya ayrıntılı bir düzeyde ilke ayarlamak için yönetim birimlerini kullanabilirsiniz. Örneğin, bir kullanıcı hesabı Yöneticisi profil bilgilerini güncelleştirebilir, parolaları sıfırlayabilir ve yalnızca kendi yönetim biriminde kullanıcılar için lisans atayabilir.

Yönetim birimlerini kullanarak bir merkezi yönetici şunları sağlayabilir:

- Kaynakların merkezileşmemiş yönetimi için yönetim birimi oluşturma
- Yönetici izinlerine sahip bir rolü yalnızca Azure AD kullanıcıları üzerinde bir yönetim biriminde atama
- Yönetim birimlerini Kullanıcı ve gruplarla gerektiği şekilde doldurma

Daha fazla bilgi için bkz. [Azure Active Directory (Önizleme) Içinde yönetim birimleri yönetimi](../users-groups-roles/directory-administrative-units.md).

---

### <a name="printer-administrator-and-printer-technician-built-in-roles"></a>Yazıcı Yöneticisi ve yazıcı teknisyeni yerleşik rolleri

**Şunu yazın:** Yeni Özellik

**Hizmet kategorisi:** Azure AD rolleri

**Ürün yeteneği:** Access Control

**Yazıcı Yöneticisi**: Bu role sahip kullanıcılar, Evrensel Yazdırma Bağlayıcısı ayarları dahil olmak üzere Microsoft Evrensel Yazdırma çözümünde tüm yazıcı yapılandırmalarının yazıcılarını kaydedebilir ve tüm yönlerini yönetebilir. Tüm temsilci atanmış yazdırma izni isteklerini kabul edebilirler. Yazıcı yöneticilerinin Ayrıca raporları yazdırma erişimi de vardır. 

**Yazıcı teknisyeni**: Bu role sahip kullanıcılar Microsoft Evrensel Yazdırma çözümünde yazıcı kaydedebilir ve yazıcı durumunu yönetebilir. Ayrıca, tüm bağlayıcı bilgilerini okuyabilir. Ana görevler bir yazıcı teknisyeni, yazıcılar ve paylaşım yazıcılarında Kullanıcı izinlerini ayarlar. [Daha fazla bilgi edinin.](../users-groups-roles/directory-assign-admin-roles.md#printer-administrator)

---

### <a name="hybrid-identity-admin-built-in-role"></a>Karma kimlik yöneticisi yerleşik rolü

**Şunu yazın:** Yeni Özellik

**Hizmet kategorisi:** Azure AD rolleri

**Ürün yeteneği:** Access Control

Bu roldeki kullanıcılar, Azure AD 'de karma kimliği etkinleştirme ile ilgili hizmetleri ve ayarları etkinleştirebilir, yapılandırabilir ve yönetebilir. Bu rol, Azure AD 'yi desteklenen üç kimlik doğrulama yönteminden birine&#8212;, Parola karması eşitlemesi (PHS), geçişli kimlik doğrulaması (PTA) veya Federasyon (AD FS ya da 3. taraf Federasyon sağlayıcısı) &#8212;ve bunları etkinleştirmek üzere ilgili şirket içi altyapıyı dağıtmak için yapılandırma olanağı verir. Şirket içi altyapı, sağlama ve PTA aracılarını içerir. Bu rol, Windows 10 olmayan cihazlarda veya Windows Server 2016 olmayan bilgisayarlarda sorunsuz kimlik doğrulamayı etkinleştirmek için sorunsuz tek Sign-On (S-SSO) etkinleştirebilme olanağı verir. Ayrıca, bu rol, oturum açma günlüklerini görüntüleme ve izleme ve sorun giderme amacıyla sistem durumuna ve analizine erişme olanağı verir. [Daha fazla bilgi edinin.](../users-groups-roles/directory-assign-admin-roles.md#hybrid-identity-administrator)

---

### <a name="network-administrator-built-in-role"></a>Ağ Yöneticisi yerleşik rolü

**Şunu yazın:** Yeni Özellik

**Hizmet kategorisi:** Azure AD rolleri

**Ürün yeteneği:** Access Control

Bu role sahip olan kullanıcılar, Microsoft 'tan gelen ve Kullanıcı konumlarından ağ telemetrisini temel alan ağ çevre mimarisi önerilerini gözden geçirebilir. Microsoft 365 ağ performansı, genellikle kullanıcı konumlarına özgü olan dikkatli kurumsal müşteri ağ çevre mimarisine dayanır. Bu rol, geliştirilmiş telemetri ölçümlerini ve tasarım önerilerini kolaylaştırmak amacıyla, bulunan Kullanıcı konumlarının düzenlenmesine ve bu konumların ağ parametrelerinin yapılandırılmasını sağlar. [Daha fazla bilgi edinin.](../users-groups-roles/directory-assign-admin-roles.md#network-administrator)

---

### <a name="bulk-activity-and-downloads-in-the-azure-ad-admin-portal-experience"></a>Azure AD Yönetici portalı deneyiminde toplu etkinlik ve İndirmeler

**Şunu yazın:** Yeni Özellik

**Hizmet kategorisi:** Kullanıcı Yönetimi

**Ürün yeteneği:** Dizinden

Artık Azure AD Yönetim Portalı deneyimine bir CSV dosyası yükleyerek Azure AD 'de kullanıcılar ve gruplar üzerinde toplu etkinlikler gerçekleştirebilirsiniz. Kullanıcılar oluşturabilir, kullanıcıları silebilir ve konuk kullanıcıları davet edebilirsiniz. Ve bir gruba üye ekleyebilir ve kaldırabilirsiniz.

Azure AD kaynakları listesini Azure AD Yönetici portalı deneyiminden de indirebilirsiniz. Dizindeki Kullanıcı listesini, dizindeki grupların listesini ve belirli bir grubun üyelerini indirebilirsiniz.

Daha fazla bilgi için aşağıdakileri gözden geçirin:

- [Kullanıcı oluşturma](../users-groups-roles/users-bulk-add.md) veya [Konuk kullanıcıları davet etme](../external-identities/tutorial-bulk-invite.md)
- [Kullanıcıları silme](../users-groups-roles/users-bulk-delete.md) veya [silinen kullanıcıları geri yükleme](../users-groups-roles/users-bulk-restore.md)
- [Kullanıcı listesini indir](../users-groups-roles/users-bulk-download.md) veya [Grup listesini indir](../users-groups-roles/groups-bulk-download.md)
- Üye [ekleme (içeri aktarma)](../users-groups-roles/groups-bulk-import-members.md) veya üye [kaldırma](../users-groups-roles/groups-bulk-remove-members.md) ya da bir grup için [üye listesini indirme](../users-groups-roles/groups-bulk-download-members.md)

---

### <a name="my-staff-delegated-user-management"></a>Personelin Temsilcili Kullanıcı Yönetimi

**Şunu yazın:** Yeni Özellik

**Hizmet kategorisi:** Kullanıcı Yönetimi

**Ürün yeteneği:**

Personeliniz, personel üyelerinin Azure AD hesaplarına erişebildiğinden emin olmak için mağaza yöneticisi gibi Firstline yöneticileri sağlar. Kuruluşlar, merkezi bir yardım masasına güvenmek yerine, parolaları sıfırlama veya telefon numaralarını değiştirme gibi genel görevleri bir Firstline Manager 'a devredebilir. Personeli sayesinde, hesaplarına erişemesiz bir Kullanıcı, yardım masası veya BT personeli gerekmeden yalnızca birkaç tıklamayla erişim elde edebilir. Daha fazla bilgi için bkz. [kullanıcılarımızla kullanıcılarınızı yönetme (Önizleme)](../users-groups-roles/my-staff-configure.md) ve [Kullanıcılarım Ile Kullanıcı yönetimi temsilcisi seçme (Önizleme)](../user-help/my-staff-team-manager.md).

---

### <a name="an-upgraded-end-user-experience-in-access-reviews"></a>Erişim incelemeleriyle yükseltilen Son Kullanıcı deneyimi

**Şunu yazın:** Değiştirilen özellik

**Hizmet kategorisi:** Erişim Incelemeleri

**Ürün yeteneği:** Kimlik yönetimi

My Apps portalındaki Azure AD erişim gözden geçirmeleriyle ilgili gözden geçiren deneyimini güncelleştirdik. Nisan 'un sonunda, Azure AD erişim gözden geçirmeleri gözden geçirmesinde oturum açmış olan gözden geçirenler, erişimimde güncelleştirilmiş deneyimi denemeye imkan tanıyan bir başlık görülecektir. Lütfen güncelleştirilmiş erişim İncelemeleri deneyiminin geçerli deneyimle aynı işlevleri sunduğunu, ancak kullanıcılarınızın üretken olmalarını sağlamak için yeni yetenekler üzerinde geliştirilmiş bir kullanıcı arabirimi olduğunu unutmayın. [Burada güncelleştirilmiş deneyim hakkında daha fazla bilgi](../governance/perform-access-review.md)edinebilirsiniz. Bu genel önizleme, 2020 Temmuz 'un sonuna kadar son olacak. Temmuz 'un sonunda, önizleme deneyimine karar verilmeyen gözden geçirenler, erişim gözden geçirmeleri gerçekleştirmek için erişimime otomatik olarak yönlendirilir. Gözden geçirenlerden şimdi erişimimde önizleme deneyimine geçiş yapmasını istiyorsanız, [lütfen buradan bir istek yapın](https://forms.microsoft.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR5dv-S62099HtxdeKIcgO-NUOFJaRDFDWUpHRk8zQ1BWVU1MMTcyQ1FFUi4u).

---

### <a name="workday-inbound-user-provisioning-and-writeback-apps-now-support-the-latest-versions-of-workday-web-services-api"></a>İşgünü gelen Kullanıcı hazırlama ve geri yazma uygulamaları artık Workday Web Hizmetleri API 'sinin en son sürümlerini desteklemektedir

**Şunu yazın:** Değiştirilen özellik

**Hizmet kategorisi:** Uygulama sağlama

**Ürün yeteneği:** 

Müşteri geri bildirimlerine bağlı olarak, Workday Web Hizmetleri (WWS) API 'sinin en son sürümlerini desteklemek üzere kurumsal uygulama galerisinde iş günü gelen Kullanıcı hazırlama ve geri yazma uygulamalarını güncelleştirdik. Bu değişiklik ile müşteriler, bağlantı dizesinde kullanmak istedikleri WWS API sürümünü belirtebilir. Bu, müşterilere Workday 'in sürümlerindeki daha fazla ık özniteliği alma olanağı sağlar. Workday geri yazma uygulaması artık Maintain_Contact_Info sınırlamalarını aşmak için önerilen Change_Work_Contact_Info Workday Web hizmetini kullanmaktadır.

Bağlantı dizesinde sürüm belirtilmemişse, varsayılan olarak, Workday gelen sağlama uygulamaları, gelen Kullanıcı sağlaması için en son Workday API 'Lerine geçiş yapmak üzere WWS v 21.1 kullanmaya devam eder, müşterilerin bağlantı dizesini [öğreticide](../saas-apps/workday-inbound-tutorial.md#which-workday-apis-does-the-solution-use-to-query-and-update-workday-worker-profiles) belgelendiği şekilde güncelleştirmesi ve ayrıca Workday [özniteliği başvuru kılavuzu](../app-provisioning/workday-attribute-reference.md#xpath-values-for-workday-web-services-wws-api-v30)'Nda belgelendiği gibi Workday öznitelikleri için kullanılan XPaths 'ı güncelleştirmesi gerekir. 

Yeni API 'yi geri yazma için kullanmak üzere, Workday geri yazma sağlama uygulamasında hiçbir değişiklik yapılması gerekmez. Workday tarafında, Workday Integration System User (ıSU) hesabının öğretici bölümünde belgelendiği gibi Change_Work_Contact iş sürecini çağırma izinlerine sahip olduğundan emin olun, [iş süreci güvenlik ilkesi Izinlerini yapılandırın](../saas-apps/workday-inbound-tutorial.md#configuring-business-process-security-policy-permissions). 

[Öğretici kılavuzumuzu](../saas-apps/workday-inbound-tutorial.md) yeni API sürümü desteğini yansıtacak şekilde güncelleştirdik.

---

### <a name="users-with-default-access-role-are-now-in-scope-for-provisioning"></a>Varsayılan erişim rolüne sahip kullanıcılar artık sağlama için kapsamdadır

**Şunu yazın:** Değiştirilen özellik

**Hizmet kategorisi:** Uygulama sağlama

**Ürün yeteneği:** Kimlik yaşam döngüsü yönetimi

Geçmişte, varsayılan erişim rolüne sahip olan kullanıcılar sağlama için kapsam dışındaydı. Müşterilerin bu rolü olan kullanıcıların sağlama kapsamında olmasını istediğini belirten geri bildirimde bulunduk. 16 Nisan 2020 itibariyle, tüm yeni sağlama yapılandırmalarının varsayılan erişim rolüne sahip kullanıcıların sağlanması sağlanır. Kademeli olarak, mevcut sağlama yapılandırmalarının davranışını bu rolle kullanıcıları sağlamayı destekleyecek şekilde değiştirecek. [Daha fazla bilgi edinin.](../app-provisioning/application-provisioning-config-problem-no-users-provisioned.md)

---

### <a name="updated-provisioning-ui"></a>Hazırlama Kullanıcı arabirimi güncelleştirildi

**Şunu yazın:** Değiştirilen özellik

**Hizmet kategorisi:** Uygulama sağlama

**Ürün yeteneği:** Kimlik yaşam döngüsü yönetimi

Daha odaklanmış bir yönetim görünümü oluşturmak için sağlama deneyimimizi yeniledik. Zaten yapılandırılmış bir kurumsal uygulama için sağlama dikey penceresine gittiğinizde, sağlama, durdurma ve yeniden başlatma gibi eylemleri sağlama ve yönetme sürecini kolayca izleyebilirsiniz. [Daha fazla bilgi edinin.](../app-provisioning/configure-automatic-user-provisioning-portal.md)

---

### <a name="dynamic-group-rule-validation-is-now-available-for-public-preview"></a>Dinamik grup kuralı doğrulaması artık genel önizleme için kullanılabilir

**Şunu yazın:** Değiştirilen özellik

**Hizmet kategorisi:** Grup Yönetimi

**Ürün yeteneği:** İş

Azure Active Directory (Azure AD), artık dinamik grup kurallarını doğrulama araçlarını sağlıyor. Kuralların beklenen şekilde çalıştığını onaylamak için **kuralları doğrula** sekmesinde, dinamik kuralınızı örnek grup üyelerine doğrulayabilirsiniz. Yöneticiler, dinamik grup kuralları oluştururken veya güncelleştirirken, bir kullanıcının veya cihazın grubun üyesi olup olmayacağını bilmesini ister. Bu, bir kullanıcının veya cihazın, üyelik beklenmediği zaman sorun gidermede kural ölçütlerini ve yardımları karşılayıp karşılamadığını değerlendirmenize yardımcı olur.

Daha fazla bilgi için bkz. [dinamik grup üyeliği kuralını doğrulama (Önizleme)](../users-groups-roles/groups-dynamic-rule-validation.md).

---

### <a name="identity-secure-score---security-defaults-and-mfa-improvement-action-updates"></a>Kimlik güvenli puanı-güvenlik Varsayılanları ve MFA geliştirme eylem güncelleştirmeleri

**Şunu yazın:** Değiştirilen özellik

**Hizmet kategorisi:** yok

**Ürün yeteneği:** Kimlik güvenliği & koruması

**Azure AD geliştirme eylemleri için güvenlik varsayılanlarını destekleme:** Microsoft güvenli puanı, [Azure AD 'deki güvenlik varsayılanlarını](./concept-fundamentals-security-defaults.md)desteklemek için geliştirme eylemlerini güncelleyecektir. Bu, kuruluşunuzun yaygın saldırılar için önceden yapılandırılmış güvenlik ayarlarıyla korunmasını kolaylaştırır. Bu, aşağıdaki geliştirme eylemlerini etkiler:

- Tüm kullanıcıların güvenli erişim için Multi-Factor Authentication 'ı tamamlayabilmesi için
- Yönetim rolleri için MFA gerektir
- Eski kimlik doğrulamasını engellemek için ilkeyi etkinleştir
 
**MFA geliştirme eylemi güncelleştirmeleri:** İşletmelerin, işletmelerle çalışan ilkeleri uygularken en çok güvenliği güvence altına aldığından emin olmak için, Microsoft güvenli puanı, Multi-Factor Authentication ' ın çevresinde ortalanmış üç geliştirme eylemini kaldırmış ve iki eklenmiştir.

Geliştirme eylemleri kaldırıldı:

- Multi-Factor Authentication için tüm kullanıcıları kaydet
- Tüm kullanıcılar için MFA gerektirme
- Azure AD ayrıcalıklı rolleri için MFA gerektir

Geliştirme eylemleri eklendi:

- Tüm kullanıcıların güvenli erişim için Multi-Factor Authentication 'ı tamamlayabilmesi için
- Yönetim rolleri için MFA gerektir

Bu yeni geliştirme eylemleri, kullanıcılarınızı veya yöneticilerinizi, dizininiz genelinde Multi-Factor Authentication (MFA) için kaydetmeyi ve kuruluşunuzun gereksinimlerine uygun olan doğru ilke kümesini oluşturmayı gerektirir. Ana amaç, tüm kullanıcılarınızın ve yöneticilerinin birden çok faktörle veya risk tabanlı kimlik doğrulama istemlerinde kimlik doğrulaması yapabilmesini sağlarken esneklik sağlamaktır. Bu, kapsamlı kararlar uygulayan veya güvenlik varsayılanlarını (16 Mart itibariyle) ayarlayarak, Microsoft 'un MFA için kullanıcılara ne zaman zorluk cağına karar vermesine olanak tanıyan birden çok ilke olmasını sağlayabilir. [Microsoft güvenli puandaki yenilikler hakkında daha fazla bilgi edinin](/microsoft-365/security/mtp/microsoft-secure-score?view=o365-worldwide#whats-new).

---