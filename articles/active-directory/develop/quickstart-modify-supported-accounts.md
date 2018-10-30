---
title: Microsoft kimlik platformu ile kaydedilmiş bir uygulama tarafından desteklenen hesapları değiştirme | Azure
description: Kimlerin veya hangi hesapların uygulamaya erişebileceğini değiştirmek için Microsoft kimlik platformuyla kayıtlı bir uygulama yapılandırın.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/25/2018
ms.author: celested
ms.custom: aaddev
ms.reviewer: lenalepa, sureshja
ms.openlocfilehash: 71c8b9cc05d15a4c4eadb6321de580ed9768a438
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2018
ms.locfileid: "49988895"
---
# <a name="quickstart-modify-the-accounts-supported-by-an-application-preview"></a>Hızlı Başlangıç: Bir uygulama tarafından desteklenen hesapları değiştirme (Önizleme)

Bir uygulamayı Microsoft kimlik platformunda kaydederken uygulamaya yalnızca kuruluşunuzdaki kullanıcılar tarafından erişim sağlanmasını isteyebilirsiniz. Alternatif olarak, uygulamanızın dış kuruluşlardaki kullanıcılar tarafından veya dış kuruluşlardaki kullanıcılar ve bir kuruluşun parçası olmayan kullanıcılar (kişisel hesaplar) tarafından da erişilebilir olmasını isteyebilirsiniz.

Bu hızlı başlangıçta, kimlerin veya hangi hesapların uygulamaya erişebileceğini değiştirmek için uygulamanızın yapılandırmasını değiştirmeyi öğreneceksiniz.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki önkoşulları tamamladığınızdan emin olun:

* Diğer kullanıcılar veya uygulamalar tarafından kullanılması gereken uygulamaları derleme konusunda önemli olan desteklenen [izinler ve onaylar](v2-permissions-and-consent.md) hakkında bilgi edinin.
* Uygulamaların kaydedilmiş olduğu bir kiracı kullanın.
  * Kayıtlı uygulama yoksa, [Microsoft kimlik platformu ile uygulamaları kaydetmeyi öğrenin](quickstart-register-app.md).
* Azure portalında uygulama kaydı için Önizleme deneyimine katılın. Bu hızlı başlangıçtaki adımlar yeni kullanıcı arabirimine karşılık gelir ve yalnızca Önizleme deneyimine geçmeyi kabul ettiyseniz çalışır.

## <a name="sign-in-to-the-azure-portal-and-select-the-app"></a>Azure portalında oturum açın ve uygulamayı seçin

Uygulamayı yapılandırmadan önce, aşağıdaki adımları izleyin:

1. Bir iş veya okul hesabı ya da kişisel Microsoft hesabınızı kullanarak [Azure portalında](https://portal.azure.com) oturum açın.
1. Hesabınız birden fazla Azure AD kiracısına erişim sunuyorsa sağ üst köşeden hesabınızı seçin ve portal oturumunuzu kullanmak istediğiniz Azure AD kiracısına ayarlayın.
1. Sol taraftaki gezinti bölmesinde **Azure Active Directory** hizmetini seçin ve ardından **Uygulama kayıtları (Önizleme)** seçeneğini belirleyin.
1. Yapılandırmak istediğiniz uygulamayı bulun ve seçin. Uygulamayı seçtiğinizde, uygulamanın **Genel Bakış** veya ana kayıt sayfasını görürsünüz.
1. [Uygulama kaydını farklı hesapları destekleyecek şekilde değiştirme](#change-the-application-registration-to-support-different-accounts) adımlarını izleyin.
1. Tek sayfalı bir uygulamanız varsa, [OAuth 2.0 örtük onayını etkinleştirin](#enable-oauth-20-implicit-grant-for-single-page-applications).

## <a name="change-the-application-registration-to-support-different-accounts"></a>Uygulama kaydını farklı hesapları destekleyecek şekilde değiştirme

Kuruluşunuzun dışındaki müşterilerinize veya iş ortaklarınıza sunmak istediğiniz bir uygulama yazıyorsanız Azure portalda uygulama tanımını güncelleştirmeniz gerekir.

> [!IMPORTANT]
> Azure AD, çok kiracılı uygulamaların Uygulama Kimliği URI'sinin genel olarak benzersiz olmasını gerektirir. Uygulama Kimliği URI'si, uygulamanın protokol iletileri içinde tanımlanması için kullanılan yollardan biridir. Tek kiracılı bir uygulamada Uygulama Kimliği URI'sinin kiracı içinde benzersiz olması yeterlidir. Azure AD'nin uygulamayı tüm kiracılar arasında bulabilmesi için çok kiracılı uygulamada bu değerin genel olarak benzersiz olması gerekir. Genel olarak benzersiz olma gereksinimi, Uygulama Kimliği URI'sinin Azure AD kiracısının doğrulanmış etki alanı ile eşleşen bir ana bilgisayar adına sahip olması şartıyla sağlanır. Örneğin kiracınızın adı contoso.onmicrosoft.com ise geçerli bir Uygulama Kimliği URI'si https://contoso.onmicrosoft.com/myapp şeklinde olacaktır. Kiracınızda contoso.com etki alanı doğrulanmışsa geçerli Uygulama Kimliği URI'si https://contoso.com/myapp şeklinde olacaktır. Uygulama Kimliği URI'si bu düzene uygun olmadığında uygulamayı çok kiracılı hale getirme işlemi başarısız olur.

### <a name="to-change-who-can-access-your-application"></a>Uygulamanızı erişebilecek kişileri değiştirmek için

1. Uygulamanın **Genel Bakış** sayfasında, **Kimlik doğrulaması** bölümünü seçin ve **Desteklenen hesap türleri** altında seçili değeri değiştirin.
    * Bir iş kolu uygulaması (LOB) oluşturuyorsanız **Yalnızca bu dizindeki hesaplar** seçeneğini belirleyin. Uygulama bir dizine kaydedilmiyorsa bu seçenek kullanılamaz.
    * Tüm iş ve eğitim kullanıcılarını hedeflemek istiyorsanız, **Herhangi bir kuruluş dizinindeki hesaplar** seçeneğini belirleyin.
    * En geniş müşteri kümesini hedeflemek için **Herhangi bir kuruluş dizinindeki hesaplar ve kişisel Microsoft hesapları** seçeneğini belirleyin.
1. **Kaydet**’i seçin.

## <a name="enable-oauth-20-implicit-grant-for-single-page-applications"></a>Tek sayfalı uygulamalar için OAuth 2.0 örtük onayını etkinleştirme

Tek sayfalı uygulamalar (SPA) genellikle tarayıcıda çalışan ve iş mantığını gerçekleştirmek için uygulamanın web API'sini çağıran ve yoğun JavaScript kullanan bir ön uca sahiptir. Azure AD'de barındırılan SPA'lar için OAuth 2.0 Örtük Onayını kullanarak kullanıcı kimliğini Azure AD ile doğrulayabilir ve uygulamanın JavaScript istemcisinden arka uç web API'sine güvenli çağrı göndermek için kullanabileceğiniz bir belirteç alabilirsiniz.

Kullanıcı onay verdikten sonra yine bu kimlik doğrulaması protokolünü kullanarak istemci ile uygulamada yapılandırılmış olan diğer web API'si kaynakları arasında güvenli çağrı göndermek için de belirteç alabilirsiniz. Örtük yetkilendirme onayı hakkında daha fazla bilgi almak ve uygulama senaryonuz için doğru seçenek olup olmadığını öğrenmek için Azure AD [v1.0](v1-oauth2-implicit-grant-flow.md) ve [v2.0](v2-oauth2-implicit-grant-flow.md) için OAuth 2.0 örtük onay akışı hakkında bilgi edinin.

OAuth 2.0 örtük onay özelliği uygulamalar için varsayılan olarak devre dışı bırakılır. Aşağıda özetlenen adımları izleyerek uygulamanız için OAuth 2.0 örtük onayını etkinleştirebilirsiniz.

### <a name="to-enable-oauth-20-implicit-grant"></a>OAuth 2.0 örtük onayını etkinleştirmek için

1. Uygulamanın **Genel Bakış** sayfasında, **Kimlik doğrulaması** bölümünü seçin.
1. **Gelişmiş ayarlar** altında, **Örtük onay** bölümünü bulun.
1. **Kimlik belirteçleri**, **Erişim belirteçleri** veya her ikisini birden seçin.
1. **Kaydet**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Uygulamalar için diğer ilgili uygulama yönetimi hızlı başlangıçları hakkında bilgi edinin:

* [Microsoft kimlik platformu ile uygulama kaydetme](quickstart-register-app.md)
* [Bir istemci uygulamasını web API'lerine erişecek şekilde yapılandırma](quickstart-configure-app-access-web-apis.md)
* [Bir uygulamayı web API'lerini kullanıma sunacak şekilde yapılandırma](quickstart-configure-app-expose-web-apis.md)
* [Microsoft kimlik platformu ile kaydedilmiş bir uygulamayı kaldırma](quickstart-remove-app.md)

Kayıtlı uygulamayı temsil eden iki Azure AD nesnesi ve aralarındaki ilişki hakkında daha fazla bilgi edinmek için bkz. [Uygulama nesneleri ve hizmet sorumlusu nesneleri](app-objects-and-service-principals.md).

Azure Active Directory ile uygulama geliştirirken kullanmanız gereken markalama yönergeleri hakkında daha fazla bilgi edinmek için bkz. [Uygulamalar için markalama yönergeleri](howto-add-branding-in-azure-ad-apps.md).