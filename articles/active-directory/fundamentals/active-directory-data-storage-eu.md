---
title: Avrupalı müşteriler - Azure Active Directory için kimlik verileri depolama | Microsoft Docs
description: Azure Active Directory kimlik doğrulamayla ilgili veriler, Avrupalı müşteriler için depoladığı hakkında bilgi edinin.
services: active-directory
author: eross-msft
manager: daveba
ms.author: lizross
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 05/17/2018
ms.custom: it-pro, seodec18
ms.openlocfilehash: 4e231d1e5af2107367a6fa2b72342bf1667e9ce1
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2019
ms.locfileid: "55102791"
---
# <a name="identity-data-storage-for-european-customers-in-azure-active-directory"></a>Kimlik Azure Active Directory'de Avrupalı müşteriler için veri depolama
Azure Active Directory (Azure AD), kullanıcı kimliklerini yönetmenize ve kuruluşunuzun kaynaklarına güvenli hale zeka tabanlı erişim ilkeleri oluşturmak için yardımcı olur. Kimlik verileri, hizmete abone olurken kuruluşunuzun sağladığı adrese göre belirlenen bir konumda depolanır. Örnek olarak Office 365 veya Azure abonelikleri verilebilir. Kimlik verilerinizin depolandığı yer hakkında belirli bilgiler için Microsoft Güven Merkezi'nin [Verileriniz nerede bulunur?](https://www.microsoft.com/trustcenter/privacy/where-your-data-is-located) bölümünü inceleyebilirsiniz.

Azure AD ile ilgili Avrupa kaynaklı kimlik verilerinin çoğu Avrupa veri merkezlerinde depolanıyor olsa da genellikle ABD veri merkezlerinde depolanan beş kullanıcı özniteliği vardır. Bunlar GivenName, Surname, userPrincipalName, Domain, ve PasswordHash özniteliğidir. PasswordHash değerinin Azure AD ile eşitlenmesini durduran şirket içi, federasyon oturumu açma yöntemi kullanıldığında PasswordHash özniteliği özel durum oluşturabilir ve ABD veri merkezlerinde depolanmayabilir. Ayrıca Azure AD'nin normal bir şekilde çalışması için ABD'de depolanan ve kişisel veri içermeyen işleme ve hizmete özgü veriler de bulunur.

## <a name="data-stored-outside-of-european-datacenters-for-european-customers"></a>Avrupalı müşteriler için Avrupa veri merkezleri dışında depolanan veriler

Avrupa adresine sahip kuruluşların Azure AD ile ilgili Avrupa kaynaklı kimlik verileri Avrupa veri merkezlerinde kalır. Avrupa veri merkezlerinde depolanır ve aynı zamanda Amerika Birleşik Devletleri veri merkezleri için çoğaltılan azure AD veri içerir:

- **Kimlikle ilgili öznitelikler**

    Aşağıdaki kimlikle ilgili öznitelikler ABD'de çoğaltılır:

    - GivenName
    - Soyadı
    - userPrincipalName
    - Domain
    - PasswordHash
    - SourceAnchor
    - AccountEnabled
    - PasswordPolicies
    - StrongAuthenticationRequirement
    - ApplicationPassword
    - PUID

- **Microsoft Azure Multi-Factor Authentication (MFA) ve Azure AD self servis parola sıfırlama (SSPR)**
    
    MFA, bekleme durumundaki tüm kullanıcı verilerini Avrupa veri merkezlerinde depolar. Ancak aşağıdakiler dahil olmak üzere MFA hizmetine özgü bazı veriler ABD'de depolanır:
    
    - MFA veya SSPR kullanmanız halinde iki öğeli kimlik doğrulama ve ilgili kişisel veriler ABD'de depolanabilir.
        - Telefon araması veya SMS ile gerçekleştirilen iki öğeli kimlik doğrulama işlemlerinin tamamı ABD'li operatörler tarafından gerçekleştirilebilir.
        - Microsoft Authenticator uygulamasını kullanan anında iletme bildirimleri için üreticinin bildirim hizmetinden (Apple veya Google) bildirim gönderilmesi gerekir ve bu da Avrupa dışından gerçekleştirilebilir.
        - OATH kodları her zaman ABD'de doğrulanır. 
    - Kimlik doğrulaması türünden bağımsız olarak bazı MFA ve SSPR günlükleri 30 gün boyunca ABD'de depolanır.

- **Microsoft Azure Active Directory B2C (Azure AD B2C)**

    Azure AD B2C, bekleme durumundaki tüm kullanıcı verilerini Avrupa veri merkezlerinde depolar. Ancak işlem günlükleri (kişisel veriler kaldırılmış halde) hizmete erişen kişinin bulunduğu konumda kalır. Örneğin bir B2C kullanıcısı hizmete ABD'den erişirse işlem günlükleri ABD'de kalır. Ayrıca kişisel veri içermeyen tüm ilke yapılandırma verileri yalnızca ABD'de depolanır. İlke yapılandırmaları hakkında daha fazla bilgi için bkz. [Azure Active Directory B2C: Yerleşik ilkeler](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-policies) makalesi.

- **Microsoft Azure Active Directory B2B (Azure AD B2B)** 
    
    Azure AD B2B, bekleme durumundaki tüm kullanıcı verilerini Avrupa veri merkezlerinde depolar. Ancak B2B, kişisel olmayan meta verileri ABD veri merkezlerindeki tablolarda depolar. Bu tabloda redeemUrl, invitationTicket, resource tenant Id, InviteRedirectUrl ve InviterAppId gibi alanlar bulunur.

- **Microsoft Azure Active Directory Domain Services (Azure AD DS)**

    Azure AD DS, kullanıcı verilerini müşterinin seçtiği Azure Sanal Ağı ile aynı konumda depolar. Ağınız Avrupa dışındaysa veriler Avrupa dışında çoğaltılır ve depolanır.

- **Azure AD ile tümleştirilen hizmetler ve uygulamalar**

    Azure AD ile tümleştirilen hizmetler ve uygulamalar, kimlik verilerine erişebilir. Her bir hizmeti ve uygulamayı ayrı ayrı değerlendirerek ilgili hizmet veya uygulama tarafından kimlik verilerinin işlenme şeklini ve şirketinizin veri depolama gereksinimlerine uygun olup olmadığını belirleyebilirsiniz.

    Microsoft hizmetlerinin verileri depoladığı konumlar hakkında daha fazla bilgi için Microsoft Güven Merkezi'nin [Verileriniz nerede bulunur?](https://www.microsoft.com/trustcenter/privacy/where-your-data-is-located) bölümünü inceleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Özellikler ve İşlevler yukarıda açıklanan ilgili daha fazla bilgi için şu makalelere bakın:
- [Multi-Factor Authentication nedir?](https://docs.microsoft.com/azure/active-directory/authentication/multi-factor-authentication)
- [Azure AD self servis parola sıfırlama](https://docs.microsoft.com/azure/active-directory/authentication/active-directory-passwords-overview)
- [Azure Active Directory B2C nedir?](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-overview)
- [Azure AD B2B işbirliği nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b)
- [Azure Active Directory (AD) Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-overview)
