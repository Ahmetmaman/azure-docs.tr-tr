---
title: 'Öğretici: bir Web uygulamasında kimlik doğrulamasını etkinleştirme'
titleSuffix: Azure AD B2C
description: Bir ASP.NET web uygulamasında kullanıcının oturum açmasını sağlamak için Azure Active Directory B2C’nin nasıl kullanılacağını gösteren öğretici.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.author: mimart
ms.date: 10/02/2020
ms.custom: devx-track-csharp, mvc
ms.topic: tutorial
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: 9c3c63b6116e02e8a742b69e90c11e182d72ab2e
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "94953041"
---
# <a name="tutorial-enable-authentication-in-a-web-application-using-azure-active-directory-b2c"></a>Öğretici: Azure Active Directory B2C kullanarak bir Web uygulamasında kimlik doğrulamasını etkinleştirme

Bu öğreticide, bir ASP.NET Web uygulamasındaki kullanıcıları oturum açmak ve kaydolmak için Azure Active Directory B2C (Azure AD B2C) nasıl kullanılacağı gösterilmektedir. Azure AD B2C, uygulamalarınızın, açık standart protokoller kullanarak sosyal hesaplar, kurumsal hesaplar ve Azure Active Directory hesaplar için kimlik doğrulaması yapmasına olanak sağlar.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure AD B2C içinde uygulamayı güncelleştirme
> * Uygulamayı kullanmak için örneği yapılandırma
> * Kullanıcı akışını kullanarak kaydolma

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

> [!NOTE]
> Bu öğretici, ASP.NET örnek bir Web uygulaması kullanır. Diğer örnek uygulamalar (ASP.NET Core, Node.js, Python ve daha fazlası dahil) için bkz. [Azure Active Directory B2C Code Samples](code-samples.md).

## <a name="prerequisites"></a>Önkoşullar

* Uygulamanızdaki kullanıcı deneyimlerini etkinleştirmek için [Kullanıcı akışları oluşturun](tutorial-create-user-flows.md) .
* **ASP.net ve Web geliştirme** iş yüküyle [Visual Studio 2019](https://www.visualstudio.com/downloads/) ' i yükledikten sonra.

## <a name="update-the-application-registration"></a>Uygulama kaydını güncelleştirme

Önkoşulların bir parçası olarak tamamladığınız öğreticide, Azure AD B2C bir Web uygulaması kaydettiniz. Bu öğreticideki örnekle iletişimi etkinleştirmek için bir yeniden yönlendirme URI 'SI eklemeniz ve kayıtlı uygulama için bir istemci gizli anahtarı (anahtar) oluşturmanız gerekir.

### <a name="add-a-redirect-uri-reply-url"></a>Yeniden yönlendirme URI 'SI (yanıt URL 'SI) ekleme

Azure AD B2C kiracınızdaki bir uygulamayı güncelleştirmek için yeni Birleşik **uygulama kayıtları** deneyimimizi veya eski  **uygulamalarımız (eski)** deneyimimizi kullanabilirsiniz. [Yeni deneyim hakkında daha fazla bilgi edinin](./app-registrations-training-guide.md).

#### <a name="app-registrations"></a>[Uygulama kayıtları](#tab/app-reg-ga/)

1. [Azure portalında](https://portal.azure.com) oturum açın.
1. Üst menüden **Dizin + abonelik** filtresi ' ni seçin ve ardından Azure AD B2C kiracınızı içeren dizini seçin.
1. Sol menüden **Azure AD B2C**' yi seçin. Ya da **tüm hizmetler** ' i seçin ve **Azure AD B2C** seçin.
1. **Uygulama kayıtları** öğesini seçin, **sahip olunan uygulamalar** sekmesini seçin ve ardından *WebApp1* uygulamasını seçin.
1. **Web** altında **URI Ekle** bağlantısını seçin, girin `https://localhost:44316` ve ardından **Kaydet**' i seçin.
1. **Genel bakış**'ı seçin.
1. Web uygulamasını yapılandırırken daha sonraki bir adımda kullanmak üzere **uygulama (istemci) kimliğini** kaydedin.

#### <a name="applications-legacy"></a>[Uygulamalar (eski)](#tab/applications-legacy/)

1. [Azure portalında](https://portal.azure.com) oturum açın.
1. Üst menüdeki **Dizin + abonelik** filtresini seçip kiracınızı içeren dizini seçerek Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun.
1. Azure portal sol üst köşesindeki **tüm hizmetler** ' i seçin ve ardından **Azure AD B2C**' i arayıp seçin.
1. **Uygulamalar (eski)** öğesini seçin ve ardından *WebApp1* uygulamasını seçin.
1. **Yanıt URL 'si** altında, ekleyin `https://localhost:44316` .
1. **Kaydet**’i seçin.
1. Özellikler sayfasında, Web uygulamasını yapılandırırken daha sonraki bir adımda kullanmak üzere uygulama KIMLIĞINI kaydedin.

* * *

### <a name="create-a-client-secret"></a>İstemci parolası oluşturma

Sonra, kayıtlı Web uygulaması için bir istemci gizli anahtarı oluşturun. Web uygulaması kod örneği, belirteç istenirken kimliğini kanıtlamak için bunu kullanır.

[!INCLUDE [active-directory-b2c-client-secret](../../includes/active-directory-b2c-client-secret.md)]

## <a name="configure-the-sample"></a>Örneği yapılandırma

Bu öğreticide, GitHub 'dan indirebileceğiniz bir örnek yapılandırırsınız. Örnek, basit bir yapılacaklar listesi sağlamak için ASP.NET kullanır. Örnek, [Microsoft OWIN ara yazılım bileşenlerini](/aspnet/aspnet/overview/owin-and-katana/)kullanır. GitHub’dan [zip dosyasını indirin](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi/archive/master.zip) veya örneği kopyalayın. Örnek dosyayı, yolunun toplam karakter uzunluğu 260'tan az olan bir klasöre ayıkladığınızdan emin olun.

```
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi.git
```

Aşağıdaki iki proje örnek çözümde bulunur:

* **Taskwebapp** -bir görev listesi oluşturun ve düzenleyin. Örnek, kullanıcıların kaydolması ve oturum açması için kaydolma **veya oturum açma** Kullanıcı akışını kullanır.
* **Taskservice** -oluşturma, okuma, güncelleştirme ve silme görev listesi işlevlerini destekler. API Azure AD B2C tarafından korunur ve TaskWebApp tarafından çağırılır.

Örnek, kiracınızda kayıtlı olan uygulamayı kullanacak şekilde, uygulama KIMLIĞI ve daha önce kaydettiğiniz anahtar dahil olmak üzere değiştirirsiniz. Ayrıca, oluşturduğunuz Kullanıcı akışlarını da yapılandırırsınız. Örnek, *Web.config* dosyasında yapılandırma değerlerini ayarlar olarak tanımlar.

Web.config dosyasındaki ayarları Kullanıcı akışınız ile çalışacak şekilde güncelleştirin:

1. **B2C-WebAPI-DotNet** çözümünü Visual Studio’da açın.
1. **Taskwebapp** projesinde **Web.config** dosyasını açın.
    1. Ve değerlerini, `ida:Tenant` `ida:AadInstance` oluşturduğunuz Azure AD B2C kiracının adıyla güncelleştirin. Örneğin, ile değiştirin `fabrikamb2c` `contoso` .
    1. Değerini, `ida:TenantId` Azure B2C kiracınız için özelliklerde bulabileceğiniz Dizin kimliğiyle değiştirin ( **Azure Active Directory**  >  **Properties**  >  **dizin kimliği** altındaki Azure Portal).
    1. Değerini, `ida:ClientId` kaydettiğiniz uygulama kimliğiyle değiştirin.
    1. `ida:ClientSecret` değerini kaydettiğiniz anahtarla değiştirin. İstemci gizli dizisi, örneğin küçüktür ( `<` ), büyüktür () `>` , ampersan () veya çift tırnak () gibi önceden tanımlanmış XML varlıkları içeriyorsa `&` `"` , bu karakterleri, Web.config eklemeden önce, istemci gizli anahtarını XML kodlayarak kaçış yapmanız gerekir.
    1. Değerini `ida:SignUpSignInPolicyId` ile değiştirin `b2c_1_signupsignin1` .
    1. Değerini `ida:EditProfilePolicyId` ile değiştirin `b2c_1_profileediting1` .
    1. Değerini `ida:ResetPasswordPolicyId` ile değiştirin `b2c_1_passwordreset1` .

## <a name="run-the-sample"></a>Örneği çalıştırma

1. Çözüm Gezgini, **Taskwebapp** projesine sağ tıklayın ve ardından **Başlangıç projesi olarak ayarla**' ya tıklayın.
1. **F5** tuşuna basın. Varsayılan tarayıcı, `https://localhost:44316/` olan yerel web sitesi adresini açar.

### <a name="sign-up-using-an-email-address"></a>E-posta adresi kullanarak kaydolma

1. Uygulamanın bir kullanıcısı olarak kaydolmak için **kaydolun/oturum aç '** ı seçin. **B2c_1_signupsignin1** Kullanıcı akışı kullanılır.
1. Azure AD B2C, kayıt bağlantısı içeren bir oturum açma sayfası sunar. Henüz bir hesabınız olmadığından **Şimdi kaydolun**' ı seçin. Kaydolma iş akışında, kullanıcı kimliğini bir e-posta adresi kullanarak toplamak ve doğrulamak için bir sayfa sunar. Kaydolma iş akışı, kullanıcının parolasını ve Kullanıcı akışında tanımlanan istenen öznitelikleri de toplar.
1. Geçerli bir e-posta adresi kullanın ve doğrulama kodunu kullanarak doğrulamayı gerçekleştirin. Parola ayarlayın. İstenen öznitelikler için değerleri girin.

    ![Oturum açma/kaydolma iş akışının bir parçası olarak gösterilen kaydolma sayfası](./media/tutorial-web-app-dotnet/sign-up-workflow.PNG)

1. Azure AD B2C kiracısında yerel hesap oluşturmak için **Oluştur** ' u seçin.

Uygulama kullanıcısı artık kendi e-posta adreslerini kullanarak oturum açabilir ve Web uygulamasını kullanabilir.

Ancak, bu yapılacaklar **listesi** özelliği, serideki bir sonraki öğreticiyi tamamlayana kadar çalışmaz, [öğretici: bir ASP.NET Web API 'sini korumak için Azure AD B2C kullanın](tutorial-web-api-dotnet.md).

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Azure AD B2C içinde uygulamayı güncelleştirme
> * Uygulamayı kullanmak için örneği yapılandırma
> * Kullanıcı akışını kullanarak kaydolma

Şimdi, Web uygulamasının Yapılacaklar **listesi** özelliğini etkinleştirmek için sonraki öğreticiye geçin. Bu durumda, bir Web API uygulamasını kendi Azure AD B2C kiracınıza kaydeder ve sonra API kimlik doğrulaması için kiracınızı kullanacak şekilde kod örneğini değiştirirsiniz.

> [!div class="nextstepaction"]
> [Öğretici: bir ASP.NET Web API 'sini korumak için Azure Active Directory B2C kullanın >](tutorial-web-api-dotnet.md)