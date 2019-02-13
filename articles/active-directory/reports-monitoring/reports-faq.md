---
title: Azure Active Directory rapor ile ilgili SSS | Microsoft Docs
description: Azure Active Directory raporlarını geçici quesitons sık sorulan.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: daveba
ms.assetid: 534da0b1-7858-4167-9986-7a62fbd10439
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: b6b8587313a4e98bfefa6489d9698052d312a6d3
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2019
ms.locfileid: "56194555"
---
# <a name="frequently-asked-questions-around-azure-active-directory-reports"></a>Sık sorulan sorular Azure Active Directory raporlarını geçici bir çözüm

Bu makale, Azure Active Directory (Azure AD) hakkında sık sorulan sorular raporlama yanıtlarını içerir. Daha fazla bilgi için bkz. [Azure Active Directory raporlaması](overview-reports.md). 

## <a name="getting-started"></a>Başlarken 

**S: Şu anda kullanın https://graph.windows.net/&lt; Kiracı adı&gt;/reports/ uç nokta API'leri çekme Azure AD denetim ve tümleşik uygulama kullanım için raporlama sistemlerimizde programlı olarak bildirir. Ne için geçiş miyim?**

**Y:** Arama [API Başvurusu](https://developer.microsoft.com/graph/) öğrenmek için [etkinliği raporlarına erişmek için API'leri kullanan](concept-reporting-api.md). Bu uç noktaya sahip iki rapor (**denetim** ve **oturum açma işlemleri**) eski API uç noktası aldığınız tüm verileri sağlar. Bu yeni uç nokta da, uygulama kullanımını, Aygıt kullanımı ve kullanıcı oturum açma bilgilerini almak için kullanabileceğiniz bir Azure AD Premium lisansına sahip bir oturum açma işlemleri raporu vardır.

--- 

**S: Şu anda kullanın https://graph.windows.net/&lt; Kiracı adı&gt;/reports/ uç nokta API'leri, Azure AD güvenlik raporları (belirli tür algılamadan dışlanmasını, sızan kimlik bilgileri veya anonim IP adreslerinden oturum açma işlemleri gibi) raporlama sistemlerimizde çekmek için Program aracılığıyla. Ne için geçiş miyim?**

**Y:** Kullanabileceğiniz [kimlik koruması risk olayları API](../identity-protection/graph-get-started.md) Microsoft Graph üzerinden erişim güvenlik algılamaları için. Bu yeni biçim, Gelişmiş filtreleme, alan seçimi ve diğer verileri nasıl Sorgulayabileceğiniz büyük esneklik sağlar ve daha kolay tümleştirmeye Sıem'lerden ve diğer veri toplama araçları için bir tür içinde risk olayları standartlaştırır. Verileri farklı bir biçimde olduğundan, yeni bir sorgu için eski sorgularınızı yerine geçemez. Ancak, [Microsoft Graph yeni API'yi kullanan](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/identityriskevent), olduğu gibi API'ler olarak O365 veya Azure AD için Microsoft standart. MS Graph mevcut yatırımlarınızdan veya Yardım ya da genişletebilirsiniz iş gerekli şekilde bu yeni standart platformu geçişinizi başlayın.

--- 

**S: Bir premium lisansı nasıl alabilirim?**

**Y:** Bkz: [Azure Active Directory Premium ile çalışmaya başlama](../fundamentals/active-directory-get-started-premium.md) , Azure Active Directory sürümünü yükseltmek için.

---

**S: Ne kadar kısa sürede etkinliği verileri bir premium lisansı aldıktan sonra görüyorum?**

**Y:** Etkinlikler verileri ücretsiz lisans olarak zaten varsa, daha sonra bunu hemen görebilirsiniz. Herhangi bir veri yoksa, veriler raporlarda görünmesi için bir veya iki gün sürebilir.

---

**S: Son aya ilişkin verileri bir Azure AD premium lisansı aldıktan sonra görebilir miyim?**

**Y:** Yakın zamanda (deneme sürümü dahil) bir Premium sürümüne çalıştıysanız, veri yedekleme 7 gün için başlangıçta görebilirsiniz. Verileri toplanır, son 30 güne ilişkin verileri görebilirsiniz.

---

**S: Azure portalında etkinlik oturum açma işlemleri görmek veya API üzerinden veri almak için genel yönetici olmanız gerekiyor mu?**

**Y:** Eğer Hayır, da raporlama verileri portalı veya API üzerinden erişebilirsiniz bir **güvenlik okuyucusu** veya **Güvenlik Yöneticisi** Kiracı. Elbette, **genel Yöneticiler** de bu verilere erişebilir.

---


## <a name="activity-logs"></a>Etkinlik günlükleri


**S: Azure portalında etkinlik günlüklerini (Denetim ve oturum açma) için veri saklama nedir?** 

**Y:** Veri saklama süresi etkinlik günlükleri için aşağıdaki tabloda listelenmektedir. Daha fazla bilgi için [veri bekletme ilkeleri için Azure AD raporlar](reference-reports-data-retention.md).

| Rapor                 | Azure AD Ücretsiz | Azure AD Premium P1 | Azure AD Premium P2 |
| :--                    | :--           | :--                 | :--                 |
| Denetim günlükleri             | 7 gün        | 30 gün             | 30 gün             |
| Oturum açma işlemleri               | Yok           | 30 gün             | 30 gün             |
| Azure MFA kullanımı        | 30 gün       | 30 gün             | 30 gün             |

--- 

**S: My görevi tamamladığınızda etkinlik verileri görene kadar ne kadar sürer?**

**Y:** Denetim günlükleri, 15 dakika veya saat arasında bir gecikme süresine sahiptir. Oturum açma etkinlik günlüklerini 15 dakika veya bazı kayıtları 2 saate kadar sürebilir.

---

**S: Azure Portalı aracılığıyla Office 365 etkinlik günlüğü bilgilerini alabilir miyim?**

**Y:** Office 365 ve Azure AD etkinlik günlükleri dizin kaynaklarının çoğunu paylaşır, ancak Office 365 etkinlik günlüklerinin tam bir görünümüne sahip olmak istiyorsanız Office 365 Yönetim Merkezine giderek Office 365 Etkinlik günlüğü bilgilerini edinmeniz gerekir.

---

**S: Office 365 etkinlik günlükleri hakkında daha fazla bilgi almak için hangi API'leri kullanabilir?**

**Y:** Kullanım [Office 365 Yönetim API'leri](https://docs.microsoft.com/office/office-365-management-api/office-365-management-apis-overview) bir API aracılığıyla Office 365 etkinlik günlüklerine erişmek için.

---

**S: Kaç tane kayıtlarını Azure portalından indirebilirim?**

**Y:** En fazla 5000 kayıtlarını Azure portalından indirebilirsiniz. Kayıtları göre sıralanır *en son* ve varsayılan olarak, en son 5000 kayıtları alın.

---

## <a name="risky-sign-ins"></a>Riskli oturum açma işlemleri

**S: Kimlik koruması risk olayı yoktur, ancak karşılık gelen oturum açma, oturum açma işlemleri raporu görüyorum değil. Bu beklenen bir durumdur?**

**Y:** Evet, kimlik koruması, etkileşimli veya etkileşimli olmayan tüm kimlik doğrulama akışları için risk değerlendirir. Ancak, tüm oturum açma işlemleri yalnızca rapor yalnızca etkileşimli oturum açma işlemleri gösterir.

---

**S: Neden bir oturum açma veya bir kullanıcıya Azure portalında riskli bayrak eklenmiş olan olduğunu nasıl öğrenebilirim?**

**Y:** Varsa bir **Azure AD Premium** abonelik edinebilirsiniz temel risk olayları hakkında daha fazla kullanıcı seçerek **risk için işaretlenen kullanıcılar** veya bir kaydı seçmesini **riskli oturum açma işlemleri** rapor. Varsa bir **ücretsiz** veya **temel** abonelik olduktan sonra kullanıcılar, risk ve riskli oturum açma işlemleri raporları görüntüleyebilir, ancak temel alınan risk olayı bilgilerini göremez.

---

**S: IP adresleri oturum açma ve riskli oturum açma işlemleri raporu nasıl hesaplanır?**

**Y:** IP adresleri, bir IP adresi ve bilgisayarın bu adresine sahip fiziksel olarak bulunduğu yeri arasında kesin bağlantı yoktur şekilde verilir. IP adreslerini eşleme daha fazla mobil sağlayıcıları ve VPN istemci cihaz gerçekten kullanıldığı gölgeden uzak IP adresleri merkezi havuzlarından genellikle çok verme gibi faktörlere tarafından karmaşık. Şu anda Azure AD raporlarında, IP adresi fiziksel bir konuma dönüştürme izlemeleri, kayıt defteri verisi, geriye doğru arama ve diğer bilgilere göre en iyi çaba ilkesi var. 

---

**S: "Oturum açma algılandı ek risk içeren" geldiğiniz risk olayı nedir?**

**Y:** Oturum açma işlemleri için Azure AD kimlik koruması aboneleri için özel olan algılama için yer tutucu olarak ortamınızda riskli oturum açma işlemleri, "Oturum açma algılandı ek risk içeren" işlevleri hakkında bilgi vermek için.

---

## <a name="conditional-access"></a>Koşullu erişim

**S: Bu özellik ile yenilikler nelerdir?**

**Y:** Müşteriler artık tüm oturum açma işlemleri raporu koşullu erişim ilkeleriyle giderebilirsiniz. Müşteriler, oturum açma ve sonucu her ilke için uygulanan tüm ilkeler ayrıntılarını inceleyin ve koşullu erişim durumunu gözden geçirebilirsiniz.

**S: Nasıl kullanmaya başlarım?**

**Y:** Kullanmaya başlamak için:
    * Oturum açma işlemleri raporu gidin [Azure portalında](https://portal.azure.com). 
    * Sorun giderme istediğiniz oturum üzerinde tıklayın.
    * Gidin **koşullu erişim** sekmesi. Burada, oturum açma ve her ilke için sonucu etkilenen tüm ilkelerde görüntüleyebilirsiniz. 
    
**S: Koşullu erişim durumu için tüm olası değerler nelerdir?**

**Y:** Koşullu erişim durumu, aşağıdaki değerlere sahip olabilir:
    * **Uygulanmamış**: Bu, CA ilke kullanıcı ve uygulamanın kapsamında olduğu anlamına gelir. 
    * **Başarı**: Bu kullanıcı ve uygulamanın kapsamında olan bir CA ilke vardı ve CA ilkeleri başarıyla karşılandı anlamına gelir. 
    * **Hata**: Bu kullanıcı ve uygulamanın kapsamında olan bir CA ilke vardı ve CA ilkeleri karşılanmadı anlamına gelir. 
    
**S: Koşullu erişim ilkesi sonucu için tüm olası değerler nelerdir?**

**Y:** Koşullu erişim ilkesi, aşağıdaki sonuçları olabilir:
    * **Başarı**: İlke başarıyla gerçekleşmiş.
    * **Hata**: İlke karşılanmadı.
    * **Uygulanmamış**: İlke koşullarını karşılamıyor, çünkü bu olabilir.
    * **Etkin**: Bu ilkeyi devre dışı durumda kaynaklanır. 
    
**S: Rapordaki tüm oturum açma ilke adını, CA ilkesi adı eşleşmiyor. Neden?**

**Y:** Rapordaki tüm oturum açma ilkesi adı, oturum açma zaman CA ilke adına bağlıdır. İlke adına daha sonra diğer bir deyişle, oturum açma işleminden sonra güncelleştirdiyseniz bu CA ilkesi adı ile tutarsız olabilir.

**S: Koşullu erişim ilkesi nedeniyle My oturum açma engellendi, ancak oturum açma etkinliği raporunu, oturum açma başarılı olduğunu gösterir. Neden?**

**Y:** Şu anda oturum açma raporu, koşullu erişim uygulandığında, Exchange ActiveSync senaryoları için daha doğru sonuçlar gösterilmeyebilir. Bir oturum açma başarılı oturum açma sonuç raporunda gösterdiği durumlarda durumları olabilir, ancak oturum açma aslında bir koşullu erişim ilkesi nedeniyle başarısız oldu. 
