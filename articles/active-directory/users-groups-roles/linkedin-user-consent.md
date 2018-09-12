---
title: LinkedIn hesabı bağlantıları için veri paylaşımı Azure Active Directory'de kullanıcı onayı | Microsoft Docs
description: Açıklayan nasıl LinkedIn hesabı bağlantıları Microsoft uygulamaları Azure Active Directory'de aracılığıyla veri paylaşma
services: active-directory
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 09/11/2018
ms.author: curtand
ms.reviewer: beengen
ms.custom: it-pro
ms.openlocfilehash: b6b7f6f51e848223adcf618ebbc47df08e7ea7ef
ms.sourcegitcommit: 794bfae2ae34263772d1f214a5a62ac29dcec3d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2018
ms.locfileid: "44392293"
---
# <a name="user-consent-and-linkedin-account-connections-data-sharing"></a>Kullanıcı onayı ve LinkedIn hesabı bağlantıları veri paylaşımı

Azure Active Directory (Azure AD) yönetici olarak, Microsoft iş veya Okul hesabı LinkedIn hesabıyla onay verme, kuruluşunuzdaki kullanıcıların etkinleştirebilirsiniz. Kullanıcıların hesaplarını bağlandığınızda, bilgi ve LinkedIn Öne çıkanlar bazı Microsoft uygulamaları ve Hizmetleri kullanılabilir. Kullanıcılar, geliştirilmiş ve Microsoft gelen bilgilerle zenginleştirilmiş LinkedIn'de ağ deneyimlerini de bekleyebilirsiniz.

Microsoft uygulamaları ve Hizmetleri içinde LinkedIn bilgilerini görmek için kullanıcılar kendi Microsoft ve LinkedIn hesaplarınızı bağlamanıza olanak cconsent gerekir. Kullanıcılar, Outlook, OneDrive veya SharePoint Online bir profil kartında birinin LinkedIn bilgilerini görmek için tıklayın ilk kez kullanıcıların hesaplarını bağlamaları istenir. Deneyimi ve hesaplarına bağlanmak için onay kadar LinkedIn hesabı bağlantıları kullanıcılarınız için tam olarak etkinleştirilmedi.

[!INCLUDE [active-directory-gdpr-note](../../../includes/gdpr-hybrid-note.md)]

## <a name="benefits-of-sharing-linkedin-information"></a>LinkedIn bilgi paylaşımı avantajları

Microsoft uygulamaları ve Hizmetleri içinde LinkedIn bilgilerine erişim, kullanıcılarınızın bağlanmak, etkileşim kurun ve iş arkadaşlarınız, müşteriler ve iş ortakları içinden ve kuruluşunuz dışından ile profesyonel ilişkileri oluşturmak kolaylaştırır. Yeni kullanıcılar daha hızlı iş arkadaşlarınızla bağlanma, bunları hakkında daha fazla bilgi ve daha fazla bilgi kolayca erişmesini hız elde edebilirsiniz. Profil Kartı Microsoft uygulamalarında LinkedIn bilgilerini nasıl görünür bir örnek aşağıda verilmiştir:

![LinkedIn hesabı bağlantıları etkinleştirme](./media/linkedin-user-consent/display-example-02.png)

## <a name="enable-and-announce-linkedin-account-connections"></a>Etkinleştirme ve LinkedIn hesabı bağlantıları Duyurusu

Azure Active Directory ayarını kuruluşunuz için yönetmek için yönetici olmanız gerekir. Tüm kullanıcılar için veya belirli bir kullanıcı kümesi için etkinleştirebilirsiniz.

1. Etkinleştirmek veya hesap bağlantıları tümleştirmesini devre dışı bırakmak için adımları izleyin. [LinkedIn hesabı bağlantıları](linkedin-integration.md).
2. Kuruluşunuzda LinkedIn ile tümleştirmeyi duyurmaktan, kullanıcılarınız hakkında SSS noktası [Microsoft uygulamaları ve Hizmetleri içinde LinkedIn bilgi](https://support.office.com/article/about-linkedin-information-and-features-in-microsoft-apps-and-services-dc81cc70-4d64-4755-9f1c-b9536e34d381). Makale nerede hakkında bilgi sağlar ve hesapları bağlanma, LinkedIn bilgilerini gösterir.

## <a name="user-consent-for-data-access-in-microsoft-and-linkedin"></a>Kullanıcı onay için Microsoft ve LinkedIn veri erişimi

Microsoft Hizmetleri LinkedIn ' erişilen veriler kalıcı olarak depolanmaz. Kişiler dışında LinkedIn ile Microsoft tarafından erişilen veriler kalıcı olarak depolanmaz. Microsoft Contacts depolanan LinkedIn'de kullanıcılar bunları kaldırana kadar açıklandığı [LinkedIn alınan kişileri silme](https://www.linkedin.com/help/linkedin/answer/43377).

Kullanıcı hesaplarını bağlandığında, bilgi ve LinkedIn ınsights'tan profil kartı gibi bazı Microsoft uygulamaları içinde kullanılabilir. Kullanıcılar, geliştirilmiş ve Microsoft gelen bilgilerle zenginleştirilmiş LinkedIn'de ağ deneyimlerini de bekleyebilirsiniz.
Bunlar, kuruluşunuzdaki kullanıcılar bağlanmak LinkedIn ile Microsoft iş veya Okul hesapları, iki seçeneğiniz vardır:

* Her iki hesaplarından erişilecek veri iznini verin. Bu, Microsoft için izin verin veya iş hesabı verilerine erişmek için LinkedIn hesaplarından ve için anlamına gelir [verilere Microsoft iş veya Okul hesabına LinkedIn hesabını](https://www.linkedin.com/help/linkedin/answer/84077).
* İzin vermek için Microsoft tarafından erişilecek yalnızca LinkedIn verileri iş ve Okul hesabı.

Kullanıcı hesapları bağlantısını kesmek ve herhangi bir zamanda veri erişimi izinlerini kaldırın ve [denetim LinkedIn profillerini nasıl görüntülenen](https://www.linkedin.com/help/linkedin/answer/83), dahil Microsoft uygulamalarında profillerini olup olmadığı görüntülenebilir.

### <a name="linkedin-account-data"></a>LinkedIn hesabı verileri

Microsoft ve LinkedIn hesaplarınızı bağladığınızda, Microsoft'a göndermenizi aşağıdaki veriler LinkedIn izin ver:

* Profil verileri - LinkedIn kimliğinizi ve kişi bilgilerini, başkalarıyla paylaşmak hakkında bilgiler içerir, [LinkedIn profil](https://www.linkedin.com/help/linkedin/answer/15493).
* Veri çeken - LinkedIn, ilgi ve gibi'yi kişiler ve konular, kursları grupları, izleyin ve içerik paylaşmak gibi içerir.
* Abonelikleri veriler - LinkedIn uygulamalara ve hizmetlere ilişkili verilerle birlikte abonelikleri içerir. 
* Veri bağlantıları - içerir, [LinkedIn ağ](https://www.linkedin.com/help/linkedin/answer/110) profilleri ve 1. derece bağlantılarınızın iletişim bilgileri dahil.

Microsoft Hizmetleri LinkedIn ' erişilen veriler kalıcı olarak depolanmaz. Kişisel verileri Microsoft'un kullanımı hakkında daha fazla bilgi için bkz. [Microsoft gizlilik bildirimi](https://privacy.microsoft.com/privacystatement/).

### <a name="microsoft-work-or-school-account-data"></a>Microsoft iş veya Okul hesabı verileri

Microsoft ve LinkedIn hesaplarınızı bağladığınızda, aşağıdaki veriler LinkedIn için Microsoft'a izin ver:

* Profil verileri - adınızı, son adı, profil fotoğrafınız, e-posta adresi, Yöneticisi'ni ve yönettiğiniz kişiler gibi bilgileri içerir.
* Takvim veri - toplantıları, takvimlerinizde etkinlik, süreleri, konumları ve katılımcıların iletişim bilgilerini içerir. Takvim verileri Gündem, içeriği veya toplantı başlığı gibi toplantı hakkında bilgi dahil edilmez.
* Veri çeken - Cortana ve iş için Bing gibi Microsoft Hizmetleri kullanımınıza yönelik temel hesabınızla ilişkili ilgi alanları içerir.
* Abonelikleri veriler - Microsoft uygulamaları ve Office 365 gibi hizmetler için kuruluşunuz tarafından sağlanan abonelikleri içerir.
* Veri kişiler - kişi listelerini, Outlook, Skype ve sık iletişim veya işbirliği kişiler için kişi bilgilerini dahil olmak üzere diğer Microsoft hesabı hizmetlerini içerir. Kişiler depolanan, düzenli olarak içeri aktarılan ve LinkedIn tarafından kullanılan bağlantı önermek daha fazla örnek için klasörlere ve güncelleştirmelerle ilgili kişileri göster yardımcı.

Kişiler dışında LinkedIn ile Microsoft tarafından erişilen veriler kalıcı olarak depolanmaz. Kullanıcılar kaldırılıncaya kadar Microsoft Contacts LinkedIn'de depolanır. Daha fazla bilgi edinin [LinkedIn alınan kişileri silme](https://www.linkedin.com/help/linkedin/answer/43377).

Kişisel verilerin LinkedIn'ın kullanımı hakkında daha fazla bilgi için bkz. [LinkedIn gizlilik ilkesi](https://www.linkedin.com/legal/privacy-policy). LinkedIn Hizmetleri, veri aktarımı ve depolama için veri Avrupa Birliği ' Amerika Birleşik Devletleri ve arka akabilir ve gizlilik uyguladıktan olarak korunuyor [Avrupa Birliği veri aktarımları](https://www.linkedin.com/help/linkedin/answer/62533).

## <a name="next-steps"></a>Sonraki adımlar

* [İş veya Okul hesabınızla Microsoft uygulamalarında LinkedIn](https://www.linkedin.com/help/linkedin/answer/84077)
