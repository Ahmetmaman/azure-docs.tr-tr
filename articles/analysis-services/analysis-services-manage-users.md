---
title: Azure Analysis Services kimlik doğrulaması ve Kullanıcı izinleri | Microsoft Docs
description: Bu makalede, Azure Analysis Services kimlik yönetimi ve Kullanıcı kimlik doğrulaması için Azure Active Directory (Azure AD) nasıl kullanıldığı açıklanmaktadır.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 05/19/2020
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: b40be802f30bac8438f10c4ab60e1c196c9f7164
ms.sourcegitcommit: 0a9df8ec14ab332d939b49f7b72dea217c8b3e1e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/18/2020
ms.locfileid: "94833801"
---
# <a name="authentication-and-user-permissions"></a>Kimlik doğrulaması ve kullanıcı izinleri

Azure Analysis Services, kimlik yönetimi ve Kullanıcı kimlik doğrulaması için Azure Active Directory (Azure AD) kullanır. Bir Azure Analysis Services sunucusuna oluşturan, yöneten veya bağlanan tüm kullanıcıların aynı abonelikte bir [Azure AD kiracısında](../active-directory/fundamentals/active-directory-whatis.md) geçerli bir kullanıcı kimliği olması gerekir.

Azure Analysis Services [Azure AD B2B işbirliğini](../active-directory/external-identities/what-is-b2b.md)destekler. B2B sayesinde kuruluş dışından gelen kullanıcılar, bir Azure AD dizininde Konuk Kullanıcı olarak davet edilebilir. Konuklar başka bir Azure AD kiracı dizininden veya geçerli herhangi bir e-posta adresinden olabilir. Davet edildikten ve Kullanıcı Azure 'dan e-posta ile gönderilen daveti kabul ettikten sonra, Kullanıcı kimliği kiracı dizinine eklenir. Bu kimlikler, güvenlik gruplarına veya bir sunucu yöneticisi veya veritabanı rolünün üyeleri olarak eklenebilir.

![Azure Analysis Services kimlik doğrulama mimarisi](./media/analysis-services-manage-users/aas-manage-users-arch.png)

## <a name="authentication"></a>Kimlik Doğrulaması

Tüm istemci uygulamaları ve araçları bir sunucuya bağlanmak için bir veya daha fazla Analysis Services [istemci kitaplığı](/analysis-services/client-libraries?view=azure-analysis-services-current) (amo, MSOLAP, ADOMD) kullanır. 

Üç istemci kitaplığı da Azure AD etkileşimli akışını ve etkileşimli olmayan kimlik doğrulama yöntemlerini destekler. Etkileşimli olmayan iki yöntem, Active Directory parola ve Active Directory tümleşik kimlik doğrulama yöntemleri, AMOMD ve MSOLAP kullanan uygulamalarda kullanılabilir. Bu iki yöntem hiçbir koşulda açılan iletişim kutularında sonuç vermez.

Excel ve Power BI Desktop gibi istemci uygulamaları ve Visual Studio için SSMS ve Analysis Services projeleri uzantısı gibi araçlar, en son sürüme güncelleştirildiği sırada kitaplıkların en son sürümlerini yükler. Power BI Desktop, SSMS ve Analysis Services projeleri uzantısı aylık olarak güncelleştirilir. Excel [Microsoft 365 ile güncelleştirilir](https://support.microsoft.com/office/when-do-i-get-the-newest-features-for-microsoft-365-da36192c-58b9-4bc9-8d51-bb6eed468516). Microsoft 365 güncelleştirmeleri daha az sıklıkla ve bazı kuruluşlar ertelenmiş kanalı kullanır, yani güncelleştirmeler üç aya kadar ertelenir.

Kullandığınız istemci uygulamasına veya araca bağlı olarak, kimlik doğrulama türü ve oturum açma farklı olabilir. Her uygulama, Azure Analysis Services gibi bulut hizmetlerine bağlanmak için farklı özellikleri destekleyebilir.

Power BI Desktop, Visual Studio ve SSMS desteği, Azure AD Multi-Factor Authentication (MFA) de destekleyen etkileşimli bir yöntem olan evrensel kimlik doğrulaması Active Directory. Azure AD MFA, basit bir oturum açma işlemi sağlarken verilere ve uygulamalara erişimi korumaya yardımcı olur. Çeşitli doğrulama seçenekleriyle (telefon araması, SMS mesajı, PIN ile akıllı kartlar veya mobil uygulama bildirimi) güçlü kimlik doğrulaması sağlar. Azure AD ile etkileşimli MFA, doğrulama için bir açılır iletişim kutusu ile sonuçlanabilir. **Evrensel kimlik doğrulaması önerilir**.

Azure 'da bir Windows hesabı kullanarak oturum açıyorsanız ve evrensel kimlik doğrulaması seçilmemiş ya da kullanılabilir (Excel), [Active Directory Federasyon Hizmetleri (AD FS) (AD FS)](/windows-server/identity/ad-fs/deployment/how-to-connect-fed-azure-adfs) gerekir. Federasyon ile Azure AD ve Microsoft 365 kullanıcıların kimliği, şirket içi kimlik bilgileri kullanılarak doğrulanır ve Azure kaynaklarına erişebilir.

### <a name="sql-server-management-studio-ssms"></a>SQL Server Management Studio (SSMS)

Azure Analysis Services sunucular, Windows kimlik doğrulaması, Active Directory parola kimlik doğrulaması ve Active Directory evrensel kimlik doğrulaması kullanarak [SSMS v 17.1](/sql/ssms/download-sql-server-management-studio-ssms) ve üzeri bağlantıları destekler. Genel olarak, Active Directory evrensel kimlik doğrulaması kullanmanız önerilir:

*  Etkileşimli ve etkileşimli olmayan kimlik doğrulama yöntemlerini destekler.

*  Azure 'a kiracı olarak davet edilen Azure B2B Konuk kullanıcılarını destekler. Bir sunucuya bağlanırken, konuk kullanıcıların sunucuya bağlanırken Active Directory evrensel kimlik doğrulaması ' nı seçmeniz gerekir.

*  Multi-Factor Authentication (MFA) destekler. Azure AD MFA; telefon araması, SMS mesajı, PIN ile akıllı kartlar veya mobil uygulama bildirimi ile veri ve uygulamalara erişimi korumaya yardımcı olur. Azure AD ile etkileşimli MFA, doğrulama için bir açılır iletişim kutusu ile sonuçlanabilir.

### <a name="visual-studio"></a>Visual Studio

Visual Studio, MFA desteğiyle Active Directory evrensel kimlik doğrulaması kullanarak Azure Analysis Services bağlanır. Kullanıcılardan ilk dağıtımda Azure 'da oturum açması istenir. Kullanıcıların, dağıttıkları sunucuda Sunucu Yöneticisi izinlerine sahip bir hesapla Azure 'da oturum açması gerekir. Azure 'da ilk kez oturum açarken bir belirteç atanır. Belirteç daha sonra yeniden bağlandığında, bellek içinde önbelleğe alınır.

### <a name="power-bi-desktop"></a>Power BI Desktop

Power BI Desktop, MFA desteğiyle Active Directory evrensel kimlik doğrulaması kullanarak Azure Analysis Services bağlanır. Kullanıcılardan ilk bağlantıda Azure 'da oturum açması istenir. Kullanıcıların, bir sunucu yöneticisi veya veritabanı rolüne dahil olan bir hesapla Azure 'da oturum açması gerekir.

### <a name="excel"></a>Excel

Excel kullanıcıları bir Windows hesabı, bir kuruluş KIMLIĞI (e-posta adresi) veya bir dış e-posta adresi kullanarak bir sunucuya bağlanabilir. Dış e-posta kimlikleri, Azure AD 'de Konuk Kullanıcı olarak bulunmalıdır.

## <a name="user-permissions"></a>Kullanıcı izinleri

**Sunucu yöneticileri** bir Azure Analysis Services sunucusu örneğine özgüdür. Veritabanları ekleme ve Kullanıcı rollerini yönetme gibi görevleri gerçekleştirmek için Azure portal, SSMS ve Visual Studio gibi araçlarla bağlanırlar. Varsayılan olarak, sunucuyu oluşturan Kullanıcı Analysis Services sunucu yöneticisi olarak otomatik olarak eklenir. Diğer yöneticiler, Azure portal veya SSMS kullanılarak eklenebilir. Sunucu yöneticilerinin aynı abonelikte Azure AD kiracısında bir hesabı olması gerekir. Daha fazla bilgi için bkz. [sunucu yöneticilerini yönetme](analysis-services-server-admins.md). 

**Veritabanı kullanıcıları** , Excel veya Power BI gibi istemci uygulamalarını kullanarak model veritabanlarına bağlanır. Kullanıcıların veritabanı rollerine eklenmesi gerekir. Veritabanı rolleri bir veritabanı için yönetici, işlem veya Okuma izinlerini tanımlar. Yönetici izinleri olan bir roldeki veritabanı kullanıcılarının sunucu yöneticilerinden farklı olduğunu anlamak önemlidir. Bununla birlikte, varsayılan olarak, sunucu yöneticileri de veritabanı yöneticileridir. Daha fazla bilgi için bkz. [veritabanı rollerini ve kullanıcıları yönetme](analysis-services-database-users.md).

**Azure Kaynak sahipleri**. Kaynak sahipleri bir Azure aboneliğine ait kaynakları yönetir. Kaynak sahipleri Azure portal veya Azure Resource Manager şablonlarıyla **erişim denetimi** kullanarak bir abonelik içindeki sahibine veya katkıda bulunan ROLLERE Azure AD Kullanıcı kimlikleri ekleyebilir. 

![Azure portal erişim denetimi](./media/analysis-services-manage-users/aas-manage-users-rbac.png)

Bu düzeydeki roller, portalda veya Azure Resource Manager şablonlar kullanılarak tamamlangerekebilecek görevler gerçekleştirmesi gereken kullanıcılara veya hesaplara uygulanır. Daha fazla bilgi için bkz. [Azure rol tabanlı erişim denetimi (Azure RBAC)](../role-based-access-control/overview.md). 

## <a name="database-roles"></a>Veritabanı rolleri

 Tablo modeli için tanımlanan roller veritabanı rolleridir. Diğer bir deyişle, roller, Azure AD kullanıcıları ve bu üyelerin bir model veritabanında uygulayabileceğiniz eylemi tanımlayan belirli izinlere sahip güvenlik gruplarından oluşan üyeleri içerir. Veritabanı rolü, veritabanında ayrı bir nesne olarak oluşturulur ve yalnızca bu rolün oluşturulduğu veritabanı için geçerli olur.   
  
 Varsayılan olarak, yeni bir tablosal model projesi oluşturduğunuzda model projesinin rolü yoktur. Roller, Visual Studio 'daki rol Yöneticisi iletişim kutusu kullanılarak tanımlanabilir. Model proje tasarımı sırasında roller tanımlandığında, bunlar yalnızca model çalışma alanı veritabanına uygulanır. Model dağıtıldığında, dağıtılan modele aynı roller uygulanır. Bir model dağıtıldıktan sonra sunucu ve veritabanı yöneticileri, SSD 'LERI kullanarak rolleri ve üyeleri yönetebilir. Daha fazla bilgi için bkz. [veritabanı rollerini ve kullanıcıları yönetme](analysis-services-database-users.md).
  
## <a name="next-steps"></a>Sonraki adımlar

[Azure Active Directory gruplarıyla kaynaklara erişimi yönetme](../active-directory/fundamentals/active-directory-manage-groups.md)   
[Veritabanı rollerini ve kullanıcılarını yönetme](analysis-services-database-users.md)  
[Sunucu yöneticilerini yönetme](analysis-services-server-admins.md)  
[Azure rol tabanlı erişim denetimi (Azure RBAC)](../role-based-access-control/overview.md)