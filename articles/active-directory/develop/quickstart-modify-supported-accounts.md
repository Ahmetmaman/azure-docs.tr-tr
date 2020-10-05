---
title: 'Hızlı başlangıç: Microsoft SDK Platformu uygulama hesaplarını değiştirme | Mavisi'
description: Bu hızlı başlangıçta, uygulamaya kimlerin erişebileceğini veya hangi hesapların erişebileceğini değiştirmek için Microsoft Identity platformu ile kaydedilen bir uygulamayı yapılandırırsınız.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: quickstart
ms.workload: identity
ms.date: 05/08/2019
ms.author: ryanwi
ms.custom: aaddev
ms.reviewer: aragra, lenalepa, sureshja
ms.openlocfilehash: d143bde9c22bc726f00b5c209d1b7fbc131905b0
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91258022"
---
# <a name="quickstart-modify-the-accounts-supported-by-an-application"></a>Hızlı başlangıç: bir uygulama tarafından desteklenen hesapları değiştirme

Bir uygulamayı Microsoft kimlik platformunda kaydederken uygulamaya yalnızca kuruluşunuzdaki kullanıcılar tarafından erişim sağlanmasını isteyebilirsiniz. Alternatif olarak, uygulamanızın dış kuruluşlardaki kullanıcılar tarafından veya dış kuruluşlardaki kullanıcılar ve bir kuruluşun parçası olmayan kullanıcılar (kişisel hesaplar) tarafından da erişilebilir olmasını isteyebilirsiniz.

Bu hızlı başlangıçta, kimlerin veya hangi hesapların uygulamaya erişebileceğini değiştirmek için uygulamanızın yapılandırmasını değiştirmeyi öğreneceksiniz.

## <a name="prerequisites"></a>Önkoşullar

* [Hızlı başlangıç: Microsoft Identity platformu ile bir uygulamayı kaydetme](quickstart-register-app.md)

## <a name="sign-in-to-the-azure-portal-and-select-the-app"></a>Azure portalında oturum açın ve uygulamayı seçin

Uygulamayı yapılandırmadan önce, aşağıdaki adımları izleyin:

1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalında](https://portal.azure.com) oturum açın.
1. Hesabınız size birden fazla Azure AD kiracısına erişim sunuyorsa sağ üst köşeden hesabınızı seçin ve portal oturumunuzu istediğiniz Azure AD kiracısına ayarlayın.
1. Sol taraftaki Gezinti bölmesinde **Azure Active Directory** hizmetini seçin ve **uygulama kayıtları**' i seçin.
1. Yapılandırmak istediğiniz uygulamayı bulun ve seçin. Uygulamayı seçtiğinizde, uygulamanın **Genel Bakış** veya ana kayıt sayfasını görürsünüz.
1. [Uygulama kaydını farklı hesapları destekleyecek şekilde değiştirme](#change-the-application-registration-to-support-different-accounts) adımlarını izleyin.
1. Tek sayfalı bir uygulamanız varsa, [OAuth 2.0 örtük onayını etkinleştirin](#enable-oauth-20-implicit-grant-for-single-page-applications).

## <a name="change-the-application-registration-to-support-different-accounts"></a>Uygulama kaydını farklı hesapları destekleyecek şekilde değiştirme

Kuruluşunuzun dışındaki müşterilerinize veya iş ortaklarınıza sunmak istediğiniz bir uygulama yazıyorsanız Azure portalda uygulama tanımını güncelleştirmeniz gerekir.

> [!IMPORTANT]
> Azure AD, çok kiracılı uygulamaların Uygulama Kimliği URI'sinin genel olarak benzersiz olmasını gerektirir. Uygulama Kimliği URI'si, uygulamanın protokol iletileri içinde tanımlanması için kullanılan yollardan biridir. Tek kiracılı bir uygulamada Uygulama Kimliği URI'sinin kiracı içinde benzersiz olması yeterlidir. Azure AD'nin uygulamayı tüm kiracılar arasında bulabilmesi için çok kiracılı uygulamada bu değerin genel olarak benzersiz olması gerekir. Genel olarak benzersiz olma gereksinimi, Uygulama Kimliği URI'sinin Azure AD kiracısının doğrulanmış etki alanı ile eşleşen bir ana bilgisayar adına sahip olması şartıyla sağlanır. Örneğin kiracınızın adı contoso.onmicrosoft.com ise geçerli bir Uygulama Kimliği URI'si `https://contoso.onmicrosoft.com/myapp` şeklinde olacaktır. Kiracınızda contoso.com etki alanı doğrulanmışsa geçerli Uygulama Kimliği URI'si `https://contoso.com/myapp` şeklinde olacaktır. Uygulama Kimliği URI'si bu düzene uygun olmadığında uygulamayı çok kiracılı hale getirme işlemi başarısız olur.

### <a name="to-change-who-can-access-your-application"></a>Uygulamanızı erişebilecek kişileri değiştirmek için

1. Uygulamanın **Genel Bakış** sayfasında, **Kimlik doğrulaması** bölümünü seçin ve **Desteklenen hesap türleri** altında seçili değeri değiştirin.
    * Bir iş kolu uygulaması (LOB) oluşturuyorsanız **Yalnızca bu dizindeki hesaplar** seçeneğini belirleyin. Uygulama bir dizine kaydedilmiyorsa bu seçenek kullanılamaz.
    * Tüm iş ve eğitim kullanıcılarını hedeflemek istiyorsanız, **Herhangi bir kuruluş dizinindeki hesaplar** seçeneğini belirleyin.
    * En geniş müşteri kümesini hedeflemek için **Herhangi bir kuruluş dizinindeki hesaplar ve kişisel Microsoft hesapları** seçeneğini belirleyin.
1. **Kaydet**’i seçin.

## <a name="enable-oauth-20-implicit-grant-for-single-page-applications"></a>Tek sayfalı uygulamalar için OAuth 2.0 örtük onayını etkinleştirme

Tek sayfalı uygulamalar (SPA) genellikle tarayıcıda çalışan ve iş mantığını gerçekleştirmek için uygulamanın web API'sini çağıran ve yoğun JavaScript kullanan bir ön uca sahiptir. Azure AD'de barındırılan SPA'lar için OAuth 2.0 Örtük Onayını kullanarak kullanıcı kimliğini Azure AD ile doğrulayabilir ve uygulamanın JavaScript istemcisinden arka uç web API'sine güvenli çağrı göndermek için kullanabileceğiniz bir belirteç alabilirsiniz.

Kullanıcı onay verdikten sonra yine bu kimlik doğrulaması protokolünü kullanarak istemci ile uygulamada yapılandırılmış olan diğer web API'si kaynakları arasında güvenli çağrı göndermek için de belirteç alabilirsiniz. Örtük yetkilendirme onayı hakkında daha fazla bilgi almak ve uygulama senaryonuz için doğru seçenek olup olmadığını öğrenmek için Azure AD [v1.0](../azuread-dev/v1-oauth2-implicit-grant-flow.md) ve [v2.0](v2-oauth2-implicit-grant-flow.md) için OAuth 2.0 örtük onay akışı hakkında bilgi edinin.

OAuth 2.0 örtük onay özelliği uygulamalar için varsayılan olarak devre dışı bırakılır. Aşağıda özetlenen adımları izleyerek uygulamanız için OAuth 2.0 örtük onayını etkinleştirebilirsiniz.

### <a name="to-enable-oauth-20-implicit-grant"></a>OAuth 2.0 örtük onayını etkinleştirmek için

1. Sol taraftaki Gezinti bölmesinde **Azure Active Directory** hizmetini seçin ve **uygulama kayıtları**' i seçin.
1. Yapılandırmak istediğiniz uygulamayı bulun ve seçin. Uygulamayı seçtiğinizde, uygulamanın **Genel Bakış** veya ana kayıt sayfasını görürsünüz.
1. Uygulamanın **Genel Bakış** sayfasında, **Kimlik doğrulaması** bölümünü seçin.
1. **Gelişmiş ayarlar** altında, **Örtük onay** bölümünü bulun.
1. **Kimlik belirteçleri**, **Erişim belirteçleri** veya her ikisini birden seçin.
1. **Kaydet**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Uygulamalar için markalama yönergeleri](howto-add-branding-in-azure-ad-apps.md)
