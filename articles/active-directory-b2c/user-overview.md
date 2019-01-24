---
title: Genel Bakış kullanıcı hesapları Azure Active Directory B2C'de | Microsoft Docs
description: Kullanıcı hesapları Azure Active Directory B2C hakkında bilgi edinin.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 4bd3eba54d70ae065ce16845dd4fb9351c069f2d
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54853315"
---
# <a name="overview-of-user-accounts-in-azure-active-directory-b2c"></a>Kullanıcı hesapları Azure Active Directory B2C, genel bakış

Azure Active Directory (Azure AD) B2C'de, farklı hesap türlerinin kullanabilirsiniz. Azure Active Directory, Azure Active Directory B2B ve Azure Active Directory B2C kullanılabilen kullanıcı hesaplarını türlerinde paylaşın.

Hesap aşağıdaki türleri kullanılabilir:

- **İş hesabı** - bir iş hesabı, bir kiracı kaynaklara erişebilir ve sahip bir yönetici rolü, kiracılar yönetebilir.
- **Konuk hesabı** -bir Konuk hesabı yalnızca bir Microsoft hesabı veya uygulamalara erişmek veya kiracılar yönetmek için kullanılan bir Azure Active Directory kullanıcısı olabilir. 
- **Tüketici hesabı** -bir tüketici hesabı bir Azure AD B2C uygulamasında kaydolma kullanıcı akışı aracılığıyla veya Azure AD Graph API'si kullanılarak oluşturulur ve Azure AD B2C'ye kayıtlı uygulamalar kullanıcılar tarafından kullanılır. 

## <a name="work-account"></a>İş hesabı

Bir iş hesabı, Azure AD temelinde tüm kiracılar için aynı şekilde oluşturulur. Bir iş hesabı oluşturmak için bilgileri kullanabilirsiniz. [hızlı başlangıç: Azure Active Directory'ye yeni kullanıcı ekleme](../active-directory/fundamentals/add-users-azure-active-directory.md). Bir iş hesabı kullanılarak oluşturulan **yeni kullanıcı** Azure portalında seçim.

Yeni bir iş hesabı eklediğinizde, aşağıdaki yapılandırma ayarları dikkate almanız gerekir:

- **Adı** ve **kullanıcı adı** - **adı** özelliği içeren belirli ve kullanıcının soyadı. **Kullanıcı adı** kullanıcı oturum açarken girdiği tanımlayıcıdır. Kullanıcı adı tam etki alanı içerir. Kullanıcı adı etki alanı adı kısmı ya da ilk varsayılan etki alanı adı olmalıdır *-alanınız.onmicrosoft.com*, veya doğrulanmış, Federasyon olmayan [özel etki alanı](../active-directory/fundamentals/add-custom-domain.md) gibi ad  *contoso.com*.
- **Profili** -hesap, kullanıcı verilerinin bir profille ayarlanır. Bir ad, Soyadı, iş unvanı ve bölüm adı girin olanağına sahiptir. Hesap oluşturulduktan sonra profili düzenleyebilirsiniz.
- **Grupları** -bir kullanıcı veya cihaz sayısı için aynı anda lisans veya izin atama gibi yönetim görevlerini gerçekleştirmek için bir grubu kullanın. Yeni hesap varolan içine koyabilirsiniz [grubu](../active-directory/fundamentals/active-directory-groups-create-azure-portal.md) kiracınızdaki. 
- **Dizin rolü** -kullanıcı hesabının kiracınız içindeki kaynaklara sahip erişim düzeyini belirtmeniz gerekir. Aşağıdaki izin düzeyleri kullanılabilir:

    - **Kullanıcı** -kullanıcılar atanan kaynaklara erişebilir ancak çoğu Kiracı kaynaklarını yönetemez.
    - **Genel yönetici** -genel Yöneticiler, tüm Kiracı kaynaklar üzerinde tam denetime sahiptir.
    - **Sınırlı yönetici** -Yönetici rolü veya kullanıcı için rolleri seçin. Seçilebilecek rolleri hakkında daha fazla bilgi için bkz. [Azure Active Directory'de yönetici rolleri atama](../active-directory/users-groups-roles/directory-assign-admin-roles.md). 

### <a name="create-a-work-account"></a>Bir iş hesabı oluşturma

Yeni bir iş hesabı oluşturmak için aşağıdaki bilgileri kullanabilirsiniz:

- [Azure portal](../active-directory/fundamentals/add-users-azure-active-directory.md)
- [Microsoft Graph](https://developer.microsoft.com/en-us/graph/docs/api-reference/v1.0/api/user_post_users)

### <a name="update-a-user-profile"></a>Bir kullanıcı profilini güncelleştirme

Bir kullanıcının profilini güncelleştirmek için aşağıdaki bilgileri kullanabilirsiniz:

- [Azure portal](../active-directory/fundamentals/active-directory-users-profile-azure-portal.md)
- [Microsoft Graph](https://developer.microsoft.com/en-us/graph/docs/api-reference/v1.0/api/user_update)

### <a name="reset-a-password-for-a-user"></a>Bir kullanıcı için bir parola sıfırlama

Bir kullanıcının parolasını sıfırlamak için aşağıdaki bilgileri kullanabilirsiniz: 

- [Azure portal](../active-directory/fundamentals/active-directory-users-reset-password-azure-portal.md)
- [Microsoft Graph](https://developer.microsoft.com/en-us/graph/docs/api-reference/v1.0/api/user_update)

## <a name="guest-user"></a>Konuk kullanıcı

Dış kullanıcılar kiracınıza Konuk kullanıcı olarak davet edebilirsiniz. Azure AD B2C kiracınıza Konuk kullanıcı davet için tipik bir senaryo, yönetim sorumlulukları paylaşmaktır. Bir Konuk hesabı kullanarak bir örnek için bkz: [bir Azure Active Directory B2B işbirliği kullanıcısı özelliklerini](../active-directory/b2b/user-properties.md).

Kiracınıza Konuk kullanıcı davet ettiğinizde, alıcı ve daveti açıklayan bir ileti e-posta adresini sağlayın. Onay sayfası kullanıcıyı davet bağlantısını götürür burada **Başlarken** düğmesi seçili ve izinleri gözden geçirmesini kabul edilir. Bir gelen kutusu e-posta adresine bağlı değilse, davet edilen kimlik bilgilerini kullanarak Microsoft sayfasına giderek bir onay sayfasına gidebilirsiniz. Kullanıcı e-postadaki bağlantıya tıklayarak olarak aynı şekilde davetini zorlanır. Örneğin: `https://myapps.microsoft.com/B2CTENANTNAME`.

Ayrıca [Microsoft Graph API](https://developer.microsoft.com/en-us/graph/docs/api-reference/beta/api/invitation_post) Konuk kullanıcı davet etme.

## <a name="consumer-user"></a>Tüketici kullanıcı

Tüketici kullanıcı Azure AD B2C ile güvenliği sağlanan uygulamalar için oturum açabilirsiniz, ancak Azure portalı gibi Azure kaynaklarına erişemez.  Tüketici kullanıcı yerel bir hesap veya Facebook veya Twitter gibi Federasyon hesapları kullanabilirsiniz. Bir tüketici hesabı kullanılarak oluşturulan bir [kaydolma veya oturum açma kullanıcı akışı](../active-directory-b2c/active-directory-b2c-reference-policies.md).

Özel kullanıcı öznitelikleri kullanarak bir tüketici kullanıcı hesabı oluşturulduğunda, toplanan verileri belirtebilirsiniz. Daha fazla bilgi için [Azure Active Directory B2C'de özel öznitelikleri tanımlamak](../active-directory-b2c/active-directory-b2c-reference-custom-attr.md).

Konusundaki bilgileri kullanabilirsiniz **tüketici kullanıcı hesapları oluşturma** bölümünü [Azure AD Graph API'sini](active-directory-b2c-devquickstarts-graph-dotnet.md) bir Azure AD B2C tüketici hesabı oluşturmak için. Bilgiler, ayrıca kullanabileceğiniz **güncelleştirme tüketici kullanıcı hesaplarını** hesabının özelliklerini yönetmek için aynı makaledeki bir bölüm.

### <a name="migrate-consumer-user-accounts"></a>Tüketici kullanıcı hesapları geçirme

Mevcut tüketici kullanıcı hesaplarını herhangi bir kimlik sağlayıcısından Azure AD B2C'ye geçirmek için bir gereksinim olabilir. Daha fazla bilgi için [kullanıcı geçişi](active-directory-b2c-user-migration.md) veya [sosyal medya kimliklerinden kullanıcıları geçirme](active-directory-b2c-social-migration.md).