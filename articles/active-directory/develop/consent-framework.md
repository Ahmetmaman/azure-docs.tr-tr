---
title: Azure AD onay çerçevesi
titleSuffix: Microsoft identity platform
description: Azure Active Directory 'de onay çerçevesi ve çok kiracılı web ve yerel istemci uygulamaları geliştirmeyi nasıl kolaylaştırdığını öğrenin.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 10/21/2020
ms.author: ryanwi
ms.reviewer: zachowd, lenalepa, jesakowi
ms.openlocfilehash: 74321bc75fa760727e7896f47cdfc5b2929047e5
ms.sourcegitcommit: 28c5fdc3828316f45f7c20fc4de4b2c05a1c5548
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/22/2020
ms.locfileid: "92366097"
---
# <a name="azure-active-directory-consent-framework"></a>Azure Active Directory onay çerçevesi

Azure Active Directory (Azure AD) onay çerçevesi, çok kiracılı web ve yerel istemci uygulamaları geliştirmeyi kolaylaştırır. Bu uygulamalar, bir Azure AD kiracısından, uygulamanın kaydedildiği bilgisayardan farklı olan kullanıcı hesaplarına oturum açma izni verir. Ayrıca, kendi Web API 'lerinize ek olarak, Microsoft Graph API 'SI (Microsoft 365 Azure AD, Intune ve hizmetlere erişmek için) ve diğer Microsoft Hizmetleri API 'Leri gibi Web API 'Lerine erişim de gerekebilir.

Çerçeve, dizin verilerine erişmeyi gerektirebilecek, kendi dizinine kaydedilmesini isteyen bir uygulamaya onay veren bir kullanıcı veya yöneticiye dayalıdır. Örneğin, bir Web istemcisi uygulamasının kullanıcı hakkındaki takvim bilgilerini Microsoft 365 okuması gerekiyorsa, önce bu kullanıcının istemci uygulamaya onay verilmesi gerekir. Onay verildikten sonra, istemci uygulaması kullanıcı adına Microsoft Graph API 'sini çağırabilir ve gerektiğinde takvim bilgilerini kullanabilir. [Microsoft Graph API 'si](https://developer.microsoft.com/graph) , Microsoft 365 verileri (örneğin, Exchange 'deki takvimler ve Iletiler, SharePoint 'ten gelen siteler ve listeler, OneDrive 'daki belgeler, Microsoft 'un Not defterleri, planlayıcılardan görevler ve Excel 'deki çalışma kitapları) ve diğer Microsoft bulut hizmetlerinden diğer veri nesnelerinden gelen kullanıcılar ve gruplar gibi erişim sağlar.

Onay çerçevesi, OAuth 2,0 ' de ve yetkilendirme kodu verme ve istemci kimlik bilgileri gibi çeşitli akışlarda, genel veya gizli istemciler kullanılarak oluşturulmuştur. Azure AD, OAuth 2,0 kullanarak bir telefon, tablet, sunucu veya Web uygulaması gibi birçok farklı türde istemci uygulaması oluşturmayı mümkün kılar ve gerekli kaynaklara erişim elde edebilir.

Onay çerçevesini OAuth 2.0 yetkilendirmesi ile kullanma hakkında daha fazla bilgi için bkz. [Azure AD Için](./authentication-vs-authorization.md) [OAuth 2,0 ve Azure AD ve kimlik doğrulama senaryolarını kullanarak Web uygulamalarına erişim yetkisi verme](v2-oauth2-auth-code-flow.md) . Microsoft Graph üzerinden Microsoft 365 yetki erişimi alma hakkında bilgi için bkz. [Microsoft Graph Ile uygulama kimlik doğrulaması](/graph/).

## <a name="consent-experience---an-example"></a>Onay deneyimi-bir örnek

Aşağıdaki adımlarda, onay deneyiminin hem uygulama geliştiricisi hem de Kullanıcı için nasıl çalıştığı gösterilmektedir.

1. Bir kaynağa/API 'ye erişmek için belirli izinler istemesi gereken bir Web istemci uygulamanız olduğunu varsayalım. Bir sonraki bölümde bu yapılandırmayı nasıl yapacağınızı öğreneceksiniz, ancak temel olarak Azure portal yapılandırma zamanında izin isteklerini bildirmek için kullanılır. Diğer yapılandırma ayarları gibi, uygulamanın Azure AD kaydının bir parçası haline gelir:

    ![Diğer uygulamalar için izinler](./media/consent-framework/permissions.png)

1. Uygulamanızın izinlerinin güncelleştirildiğini, uygulamanın çalıştığını ve bir kullanıcının ilk kez kullanmak üzere olduğunu göz önünde bulundurun. İlk olarak, uygulamanın Azure AD uç noktasından bir yetkilendirme kodu alması gerekir `/authorize` . Yetkilendirme kodu daha sonra yeni bir erişim ve yenileme belirteci almak için kullanılabilir.

1. Kullanıcının kimliği doğrulanmıyorsa, Azure AD `/authorize` uç noktası kullanıcıdan oturum açmasını ister.

    ![Kullanıcı veya yönetici Azure AD 'de oturum açın](./media/consent-framework/usersignin.png)

1. Kullanıcı oturum açtıktan sonra, Azure AD kullanıcının bir onay sayfası gösterilmesi gerekip gerekmediğini tespit eder. Bu belirleme, kullanıcının (veya kuruluşun yöneticisinin) uygulama iznini zaten vermiş olup olmadığına bağlıdır. İzin verilmemişse, Azure AD kullanıcıya onay ister ve çalışması gereken gerekli izinleri görüntüler. Onay iletişim kutusunda görüntülenen izin kümesi, Azure portal **temsilci izinleri** içinde seçili olanlarla eşleşir.

    ![Onay iletişim kutusunda gösterilen izinlere bir örnek gösterir](./media/consent-framework/consent.png)

1. Kullanıcı onay verdikten sonra uygulamanıza bir erişim belirteci ve yenileme belirteci almak için kullanılan bir yetkilendirme kodu döndürülür. Bu akış hakkında daha fazla bilgi için bkz. [OAuth 2,0 yetkilendirme kodu akışı](v2-oauth2-auth-code-flow.md).

1. Yönetici olarak, kiracınızdaki tüm kullanıcılar adına bir uygulamanın temsilci izinlerini de kabul edebilirsiniz. Yönetici onayı, izin iletişim kutusunun Kiracıdaki her kullanıcı için görünmesini engeller ve yönetici rolüne sahip kullanıcılar tarafından [Azure Portal](https://portal.azure.com) yapılabilir. Hangi Yönetici rollerinin temsilci izinleri onaylamasına izin verebileceğini öğrenmek için bkz. [Azure AD 'de yönetici rolü izinleri](../roles/permissions-reference.md).

    **Uygulamanın temsilci izinlerini kabul etmek için**

   1. Uygulamanızın **API izinleri** sayfasına gidin
   1. **Yönetici onayı ver** düğmesine tıklayın.

      ![Açık yönetici onayı için izin verme](./media/consent-framework/grant-consent.png)

   > [!IMPORTANT]
   > **Izin verme** düğmesi kullanılarak açık onay verilmesi şu anda ADAL.js kullanan tek sayfalı uygulamalar (Spa) için gereklidir. Aksi takdirde, erişim belirteci istendiğinde uygulama başarısız olur.

## <a name="next-steps"></a>Sonraki adımlar

Bkz. [bir uygulamayı çok kiracılı olarak dönüştürme](howto-convert-app-to-be-multi-tenant.md)
