---
title: Azure AD dağıtım denetim listesi
description: Azure Active Directory özellik dağıtım denetim listesi
services: active-directory
ms.service: active-directory
ms.component: ''
ms.topic: conceptual
ms.date: 01/08/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: ''
ms.openlocfilehash: 1bfc97eb850cf81ff638f92bc628c856fa75681e
ms.sourcegitcommit: a512360b601ce3d6f0e842a146d37890381893fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2019
ms.locfileid: "54230671"
---
# <a name="azure-active-directory-feature-deployment-guide"></a>Azure Active Directory özelliği dağıtım kılavuzu

Kuruluşunuz için Azure Active Directory (Azure AD) dağıtma ve güvenli tutmaya göz korkutucu görünebilir. Bu makale, müşterilerin aşamalarında 60, 90 gün boyunca 30 tamamlamak yararlı veya aboneliklerin güvenliğini artırmak için daha fazla duruşunu, ortak görevler tanımlar. Azure AD zaten dağıtmış olan kuruluşlar bile, bunlar en iyi yatırımları aldıklarından emin olmak için bu kılavuzu kullanabilirsiniz.

İyi planlanmış ve yürütülen kimlik altyapısını güvenli erişim için üretkenlik iş yüklerini ve verileri tarafından bilinen kullanıcılara ve cihazlara yalnızca bir hazırlık niteliğindedir.

Müşteriler ayrıca denetleyebilirsiniz kendi [kimlik güvenli puanı](identity-secure-score.md) nasıl hizalanmış için en iyi Microsoft olduklarını görmek için. Önce güvenli puanınız denetleyin ve görmek için bu önerileri uyguladıktan sonra ne kadar iyi yapmakta olduğunuz sektörünüze bazılarında ve boyutunun diğer kuruluşlar için karşılaştırılır.

## <a name="prerequisites"></a>Önkoşullar

Bu kılavuzdaki önerilerin çoğu Azure AD ücretsiz, temel veya lisans ile hiç uygulanabilir. Lisans gerekli olduğu, biz bir görevi tamamlamak için en azından hangi lisans gereklidir durumu.

Lisanslama hakkında ek bilgiler aşağıdaki sayfalarda bulunabilir:

* [Azure AD lisanslama](https://azure.microsoft.com/pricing/details/active-directory/)
* [Microsoft 365 Kurumsal](https://www.microsoft.com/en-us/licensing/product-licensing/microsoft-365-enterprise)
* [Enterprise Mobility + Security](https://www.microsoft.com/en-us/licensing/product-licensing/enterprise-mobility-security)
* [Azure AD B2B lisanslama Kılavuzu](../b2b/licensing-guidance.md)

## <a name="phase-1-foundation-of-security"></a>1. Aşama: Güvenlik temeli

1. aşamada, yöneticilerin almak veya normal kullanıcı hesapları oluşturmadan önce Azure AD'de daha güvenli ve kullanımı kolay bir temel oluşturmak temel güvenlik özellikleri sağlar. Bu temel aşama başından itibaren daha güvenli bir durumda olduğunu ve kullanıcılarınız yalnızca bir kez için yeni kavramları tanıtılmak üzere olmasını sağlar.

| Görev | Ayrıntı | Gerekli lisans |
| ---- | ------ | ---------------- |
| [Birden fazla genel yönetici belirleyin](../users-groups-roles/directory-emergency-access.md) | Acil ise kullanmak için en az iki kalıcı yalnızca bulut genel yönetici hesabı atayın. Bu hesaplar olmayan günlük kullanılması ve uzun ve karmaşık parolalar sahip olmalıdır. | Azure AD Ücretsiz |
| [Mümkün olduğunda genel olmayan yönetici rollerini kullanın](../users-groups-roles/directory-assign-admin-roles.md) | Yöneticiler, yalnızca ihtiyaç duydukları erişim alanlara yalnızca ihtiyaç duydukları erişim verin. Tüm yöneticilerin genel yönetici olması gerekir. | Azure AD Ücretsiz |
| [Yönetici rolü kullanımını izleme için Privileged Identity Management'ı etkinleştir](../privileged-identity-management/pim-getting-started.md) | Yönetim rolü kullanımı izlemeye başlamak Privileged Identity Management'ı etkinleştirin. | Azure AD Premium P2 |
| [Self servis parola sıfırlamayı dağıtma](../authentication/howto-sspr-deployment.md) | Personel ilkelerini kullanarak kendi parolalarını sıfırlamasına izin vererek parola sıfırlamaları için Yardım Masası aramalarını azaltır, bir yönetici denetimi. | Azure AD Basic |
| [Bir kuruluşun belirli özel yasaklı parola listesi oluşturun](../authentication/howto-password-ban-bad-configure.md) | Kullanıcıların, genel sözcükleri veya tümcecikleri kuruluş veya alan dahil parolalar oluşturmasını engeller. | Azure AD Basic |
| [Azure AD parola koruması ile şirket içi tümleştirmeyi etkinleştir](../authentication/concept-password-ban-bad-on-premises.md) | Şirket küme parolaları da genel uyumlu olduğundan emin olmak için şirket içi dizininize yasaklı parola listesi genişletin ve parola listesi kiracıya özgü yasaklandı. | Azure AD Premium P1 |
| [Microsoft'un parola kılavuzunu etkinleştirin](https://www.microsoft.com/research/publication/password-guidance/) | Kullanıcıların parolalarını ayarlanan zamanlamada, karmaşıklık gereksinimlerini devre dışı bırakın ve kullanıcılarınızın gerektiren durdurma parolalarını unutmayın ve bunları güvenli bir şey tutun daha apt. | Azure AD Ücretsiz |
| [Düzenli parola sıfırlama için bulut tabanlı kullanıcı hesaplarına devre dışı bırak](../authentication/concept-sspr-policy.md#set-a-password-to-never-expire) | Düzenli parola sıfırlama mevcut parolalarını artırmak için kullanıcılarınızı teşvik edin. Microsoft'un parola Kılavuzu belgedeki yönergeleri kullanın ve yalnızca bulutta yer alan kullanıcılar için şirket içi ilkenizi yansıtır. | Azure AD Ücretsiz |
| [Azure Active Directory akıllı kilitleme özelleştirme](../authentication/howto-password-smart-lockout.md) | Buluttaki kullanıcıların şirket içi Active Directory Kullanıcıları için çoğaltılmasını kilitlenmelerden Durdur | Azure AD Basic |
| [AD FS için akıllı extranet kilitleme etkinleştir](/windows-server/identity/ad-fs/operations/configure-ad-fs-extranet-smart-lockout-protection) | AD FS extranet kilitleme saldırıları geçerli AD FS kullanıcı hesaplarını kullanmaya devam izin verirken tahmin yanılma parola karşı korur. | |
| [Koşullu erişim ilkeleri kullanarak Azure AD multi-Factor Authentication'ı dağıtma](../authentication/howto-mfa-getstarted.md) | Koşullu erişim ilkelerini kullanarak hassas uygulamalarına erişirken iki aşamalı doğrulamayı gerçekleştirmek kullanıcıların gerektirir. | Azure AD Premium P1 |
| [Azure Active Directory kimlik Koruması'nı etkinleştir](../identity-protection/enable.md) | Riskli oturum açma ve güvenliği aşılmış kimlik bilgileri, kuruluşunuzdaki kullanıcılar için izlemeyi etkinleştirin. | Azure AD Premium P2 |
| [Çok faktörlü kimlik doğrulaması ve parola değişiklikleri tetiklemek için risk olayları kullanın](../authentication/tutorial-risk-based-sspr-mfa.md) | Çok faktörlü kimlik doğrulaması, parola sıfırlama ve oturum açma riskine bağlı olarak, engelleme gibi olayları tetikleyebilir otomasyonunu etkinleştirme. | Azure AD Premium P2 |
| [Self Servis parola sıfırlama ve Azure AD multi-Factor Authentication (Önizleme) için yakınsanmış kaydını etkinleştirin](../authentication/concept-registration-mfa-sspr-converged.md) | Azure multi-Factor Authentication hem de Self Servis parola sıfırlama için bir ortak deneyiminden kaydedin açmasına imkan tanıyın. | Azure AD Premium P1 |

## <a name="phase-2-users-synchronization-and-devices"></a>2. Aşama: Kullanıcılar, eşitleme ve cihazlar

1. Aşama kullanıcılarımızın alma ve etkinleştirme düzenlenir Foundation ekleriz Aşama 2'de, Konuk erişimi için planlama ve ek işlevleri destekleyecek şekilde hazırlama eşitleme.

| Görev | Ayrıntı | Gerekli lisans |
| ---- | ------ | ---------------- |
| [Azure AD Connect'i yükleme](../connect/active-directory-aadconnect-select-installation.md) | Mevcut şirket içi dizininizi buluta kullanıcıların eşitlemek hazırlayın. | Azure AD Ücretsiz |
| [Parola karma eşitlemesi uygulama](../connect/active-directory-aadconnectsync-implement-password-hash-synchronization.md) | Çoğaltılacak parola değişiklikleri, hatalı parola algılama ve düzeltme ve sızdırılan kimlik bilgisi raporlama izin vermek için parola karmalarını eşitleyin. | Azure AD Premium P1 |
| [Parola geri yazma uygulama](../authentication/howto-sspr-writeback.md) | Parola değişiklikleri şirket içi Windows Server Active Directory ortamına geri yazılması için bulutta izin verin. | Azure AD Premium P1 |
| [Uygulama Azure AD Connect Health'i](../connect-health/active-directory-aadconnect-health.md) | Azure AD Connect sunucularınız, AD FS sunucuları ve etki alanı denetleyicileri için temel sistem durumu istatistiklerin izlemeyi etkinleştirin. | Azure AD Premium P1 |
| [Azure Active Directory'de Grup üyeliği kullanıcıları için lisans atama](../users-groups-roles/licensing-groups-assign.md) | Zaman ve çaba etkinleştirmek veya devre dışı özellikleri kullanıcı başına ayar yerine grup tarafından lisans grupları oluşturarak kaydedin. | |
| [Konuk kullanıcı erişimi için bir plan oluşturun](../b2b/what-is-b2b.md) | Konuk kullanıcılar ile bunları uygulamaları ve Hizmetleri, kendi iş, okul veya sosyal kimlik ile oturum açma vererek işbirliği yapın. | [Azure AD B2B lisanslama Kılavuzu](../b2b/licensing-guidance.md) |
| [Cihaz yönetim stratejinize karar verin](../devices/overview.md) | Kuruluşunuzun cihazları ile ilgili ne verdiği karar verin. Birleştirme, kendi cihazını Getir vs bir vs kaydetme şirket sağlanır. | |
| [Windows iş için Hello, kuruluşunuzda dağıtma](/windows/security/identity-protection/hello-for-business/hello-manage-in-organization) | Windows Hello'yu kullanarak parola olmadan kimlik doğrulaması için hazırlama | |

## <a name="phase-3-applications"></a>3. Aşama: Uygulamalar

Aşama 3'de, yöneticiler uygulamalarda eklemek için devam eden işlemi başlar.

| Görev | Ayrıntı | Gerekli lisans |
| ---- | ------ | ---------------- |
| Uygulamalarınızı tanımlayın | Kuruluşunuzun kullanılmakta olan uygulamaları belirlemek: şirket içi, bulutta SaaS uygulamaları ve diğer satır iş kolu uygulamaları. Bu uygulamalar ve Azure AD ile yönetilen varsa bu seçeneği belirleyin. | Lisansa gerek yoktur |
| [Galerideki desteklenen SaaS uygulamalarını tümleştirme](../manage-apps/add-application-portal.md) | Azure AD binlerce önceden tümleştirilmiş uygulamalar içeren bir galeri var. Kuruluşunuzun kullandığı uygulamalar büyük olasılıkla doğrudan Azure portalından erişilebilir galerisinde bazılarıdır. | Azure AD Ücretsiz |
| [Şirket içi uygulamalarını tümleştirmek için uygulama proxy'si kullanın](../manage-apps/application-proxy-add-on-premises-application.md) | Uygulama proxy'si, kullanıcıların kendi Azure AD hesabıyla oturum açarak şirket uygulamalarına erişmelerini sağlar. | Azure AD Basic |

## <a name="phase-4-privileged-identities-access-reviews-and-user-lifecycle"></a>4. Aşama: Ayrıcalıklı kimlikleri ve erişim gözden geçirmeleri kullanıcı yaşam döngüsü

4. aşaması, yöneticilerin yönetimi için en az ayrıcalık ilkeleri zorunlu, kendi ilk erişim gözden geçirmeleri tamamlanıyor ve yaygın kullanıcı yaşam döngüsü görevlerini otomasyonunu etkinleştirme görür.

| Görev | Ayrıntı | Gerekli lisans |
| ---- | ------ | ---------------- |
| [Privileged Identity Management kullanımını zorunlu kılma](../privileged-identity-management/pim-configure.md) | Yönetim rolleri normal gündelik kullanıcı hesaplarından kaldırın. Yönetici kullanıcılar kendi rol başarılı bir çok faktörlü kimlik doğrulama denetimi, bir iş gerekçesi sağlamak ya da belirlenmiş onaylayanlar onay isteme kullanmak uygun olarak belirleyemezsiniz. | Azure AD Premium P2 |
| [Azure AD dizin rollerini PIM için erişim değerlendirmesi tamamlama](../privileged-identity-management/pim-how-to-start-security-review.md) | Kuruluşunuzun ilkelerine bağlı olarak yönetici erişimi gözden geçirmek için bir erişim gözden geçirme ilkesi oluşturmak için güvenlik ve liderlik takımlarınızın birlikte çalışın. | Azure AD Premium P2 |
| [Uygulama dinamik grup üyeliği ilkeleri](../users-groups-roles/groups-dynamic-membership.md) | İK (veya bir kaynak sağlar), departman, başlık, bölge, bunların özniteliklerini ve diğer özniteliklerini göre gruplara otomatik olarak kullanıcılara atamak için dinamik grupları kullanın. |  |
| [Uygulama sağlama uygulama grubuna göre](../manage-apps/what-is-access-management.md) | Grup tabanlı erişim denetimi, otomatik olarak sağlama kullanıcılara SaaS uygulamaları için sağlama kullanın. |  |
| [Kullanıcı sağlamayı ve sağlama kaldırmayı otomatikleştirme](../manage-apps/user-provisioning.md) | Yetkisiz erişimi önlemek için çalışan hesabı döngüsü adımları el ile kaldırın. Getirilir (ik sistemine) kaynağınızdan Azure AD kimlikleri eşitleyin. |  |

## <a name="next-steps"></a>Sonraki adımlar

[Azure AD Lisanslama ve fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/active-directory/)

[Kimlik ve cihaz erişim yapılandırmaları](https://docs.microsoft.com/microsoft-365/enterprise/microsoft-365-policies-configurations)

[Ortak kimlik ve cihaz erişim ilkeleri önerilir.](https://docs.microsoft.com/microsoft-365/enterprise/identity-access-policies)
