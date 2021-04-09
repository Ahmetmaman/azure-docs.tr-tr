---
title: 'Hızlı başlangıç: angular tek sayfalı uygulamalardaki kullanıcı oturum açma-Azure'
titleSuffix: Microsoft identity platform
description: Bu hızlı başlangıçta, angular uygulamasının Microsoft Identity platformu tarafından verilen erişim belirteçleri gerektiren bir API 'YI nasıl çağırabileceğinizi öğrenirsiniz.
services: active-directory
author: jasonnutter
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.custom: aaddev, identityplatformtop40, scenarios:getting-started, languages:JavaScript, devx-track-js
ms.topic: quickstart
ms.workload: identity
ms.date: 03/18/2020
ms.author: janutter
ms.openlocfilehash: 5ac6a19fc569d60f75f9da788629331a70b0a4c9
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2021
ms.locfileid: "103225056"
---
# <a name="quickstart-sign-in-users-and-get-an-access-token-in-an-angular-single-page-application"></a>Hızlı başlangıç: angular tek sayfalı uygulamada Kullanıcı oturumu açma ve erişim belirteci edinme

Bu hızlı başlangıçta, angular bir tek sayfalı uygulamanın (SPA) kullanıcılara nasıl oturum açıp Microsoft Graph çağırabileceğinizi gösteren bir kod örneği indirip çalıştırırsınız. Kod örneği, Microsoft Graph API 'sini veya herhangi bir Web API 'sini çağırmak için bir erişim belirtecinin nasıl alınacağını gösterir.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. [Ücretsiz bir tane oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* [Node.js](https://nodejs.org/en/download/).
* Projeyi çalıştırmak için proje dosyalarını veya [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/) 'yi düzenlemek [Visual Studio Code](https://code.visualstudio.com/download) .

> [!div renderon="docs"]
> ## <a name="register-and-download-the-quickstart-app"></a>Hızlı başlangıç uygulamasını kaydedin ve indirin
> Hızlı başlangıç uygulamasını başlatmak için aşağıdaki seçeneklerden birini kullanın.
>
> ### <a name="option-1-express-register-and-automatically-configure-the-app-and-then-download-the-code-sample"></a>Seçenek 1 (Express): uygulamayı kaydedin ve otomatik olarak yapılandırın ve ardından kod örneğini indirin
>
> 1. <a href="https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade/quickStartType/JavascriptSpaQuickstartPage/sourceType/docs" target="_blank">Azure portal uygulama kayıtları</a> hızlı başlangıç deneyimine gidin.
> 1. Uygulamanız için bir ad girin ve ardından **Kaydet**' i seçin.
> 1. Hızlı başlangıç bölmesine gidin ve angular hızlı başlangıcını görüntüleyin. Yeni uygulamanızı indirip otomatik olarak yapılandırmak için yönergeleri izleyin.
>
> ### <a name="option-2-manual-register-and-manually-configure-the-application-and-code-sample"></a>Seçenek 2 (el ile): uygulamayı ve kod örneğini kaydetme ve el ile yapılandırma
>
> #### <a name="step-1-register-the-application"></a>1. Adım: uygulamayı kaydetme
>
> 1. <a href="https://portal.azure.com/" target="_blank">Azure portalında</a> oturum açın.
> 1. Birden fazla kiracıya erişiminiz varsa, uygulamayı kaydetmek istediğiniz kiracıyı seçmek için üst menüdeki **Dizin + abonelik** filtresini kullanın :::image type="icon" source="./media/common/portal-directory-subscription-filter.png" border="false"::: .
> 1. Azure portal [tek sayfalı bir uygulamayı kaydetmek](./scenario-spa-app-registration.md) için yönergeleri izleyin.
> 1. Uygulama kaydlarınızın **kimlik doğrulama** bölmesine yeni bir platform ekleyin ve yenıden yönlendirme URI 'sini kaydedin: `http://localhost:4200/` .
> 1. Bu hızlı başlangıç, [örtük verme akışını](v2-oauth2-implicit-grant-flow.md)kullanır. **Örtük verme ve karma akışlar** bölümünde **Kimlik belirteçleri** ve **erişim belirteçleri**' ni seçin. KIMLIK belirteçleri ve erişim belirteçleri gereklidir çünkü bu uygulama kullanıcılar üzerinde oturum açar ve bir API çağırır.

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-1-configure-the-application-in-the-azure-portal"></a>1. Adım: uygulamayı Azure portal yapılandırma
> Bu hızlı başlangıçtaki kod örneğinin çalışması için, bir yeniden yönlendirme URI 'SI eklemeniz **http://localhost:4200/** ve **örtülü izni** etkinleştirmeniz gerekir.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Bu değişiklikleri benim için yap]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Zaten yapılandırılmış](media/quickstart-v2-javascript/green-check.png) Uygulamanız bu özniteliklerle yapılandırılmış.

#### <a name="step-2-download-the-code-sample"></a>2. Adım: kod örneğini Indirme
>[!div renderon="docs"]
>Node.js kullanarak projeyi bir Web sunucusuyla çalıştırmak için, [örnek depoyu kopyalayın](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-angular) veya [temel proje dosyalarını indirin](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-angular/archive/master.zip). Visual Studio Code gibi bir düzenleyiciyi kullanarak dosyaları açın.

> [!div renderon="portal" id="autoupdate" class="sxs-lookup nextstepaction"]
> [Kod örneğini indirin](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-angular/archive/master.zip)

> [!div renderon="docs"]
>#### <a name="step-3-configure-the-javascript-app"></a>3. Adım: JavaScript uygulamasını yapılandırma
>
> *Src/App* klasöründe *app. Module. TS* öğesini düzenleyin ve `clientId` `authority` altındaki ve değerlerini ayarlayın `MsalModule.forRoot` .
>
>```javascript
>MsalModule.forRoot({
>    auth: {
>        clientId: 'Enter_the_Application_Id_here', // This is your client ID
>        authority: 'https://login.microsoftonline.com/Enter_the_Tenant_Info_Here', // This is your tenant info
>        redirectUri: 'Enter_the_Redirect_Uri_Here' // This is your redirect URI
>    },
>    cache: {
>        cacheLocation: 'localStorage',
>        storeAuthStateInCookie: isIE, // set to true for IE 11
>    },
>},
> //... )
>```

> [!div class="sxs-lookup" renderon="portal"]
> > [!NOTE]
> > Enter_the_Supported_Account_Info_Here


> [!div renderon="docs"]
>
> Şu değerleri değiştirin:
>
>|Değer adı|Description|
>|---------|---------|
>|Enter_the_Application_Id_Here|Uygulama kaydlarınızın **genel bakış** sayfasında bu, **uygulamanızın (istemci) kimlik** değeridir. |
>|Enter_the_Cloud_Instance_Id_Here|Bu, Azure bulutunun örneğidir. Ana veya küresel Azure bulutu için girin **https://login.microsoftonline.com** . Ulusal bulutlar (örneğin, Çin) için bkz. [Ulusal bulutlar](./authentication-national-cloud.md).|
>|Enter_the_Tenant_Info_Here| Aşağıdaki seçeneklerden birine ayarlayın: uygulamanız *bu kuruluş dizinindeki hesapları* destekliyorsa, bu değeri dizin (KIRACı) kimliği veya kiracı adı (örneğin, **contoso.Microsoft.com**) ile değiştirin. Uygulamanız *herhangi bir kuruluş dizinindeki hesapları* destekliyorsa, bu değeri **kuruluşlar** ile değiştirin. Uygulamanız *herhangi bir kurumsal dizin ve kişisel Microsoft hesabında hesapları* destekliyorsa, bu değeri **ortak** ile değiştirin. *Yalnızca kişisel Microsoft hesaplarına* yönelik desteği kısıtlamak için bu değeri **tüketicilerle** değiştirin. |
>|Enter_the_Redirect_Uri_Here|İle değiştirin **http://localhost:4200** .|
>|Önbellekelocation  | Seçim Kimlik doğrulama durumu için tarayıcı depolamayı ayarlayın. Varsayılan değer **sessionStorage**' dır.   |
>|Storeauthstateıncookie  | Seçim Kimlik doğrulama isteği durumunu depolayan kitaplığı belirler. Tarayıcı tanımlama bilgilerinde kimlik doğrulama akışlarını doğrulamak için bu durum gereklidir. Bu tanımlama bilgisi, Internet Explorer ve Edge 'in bu iki tarayıcıyı kapsayacak şekilde ayarlanır. Daha fazla ayrıntı için bkz. [bilinen sorunlar](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Known-issues->on-IE-and-Edge-Browser#issues). |
>
> **Uygulama (istemci) Kimliği**, **Dizin (kiracı) Kimliği** ve **Desteklenen hesap türleri** değerlerini bulmak için Azure portalında uygulamanın **Genel bakış** sayfasına gidin.

Kullanılabilir yapılandırılabilir seçenekler hakkında daha fazla bilgi için bkz. [istemci uygulamalarını başlatma](msal-js-initializing-client-applications.md).

GitHub 'da, MSAL.js kitaplığının kaynak kodunu, [Azuread/Microsoft-Authentication-Library-for-js](https://github.com/AzureAD/microsoft-authentication-library-for-js) deposunda bulabilirsiniz.

>[!div class="sxs-lookup" renderon="portal"]
>#### <a name="step-3-run-the-project"></a>3. Adım: projeyi çalıştırma

>[!div renderon="docs"]
>#### <a name="step-4-run-the-project"></a>4. Adım: projeyi çalıştırma

Node.js kullanıyorsanız:

1. Proje dizininden aşağıdaki komutları çalıştırarak sunucuyu başlatın:

   ```console
   npm install
   npm start
   ```

1. **http://localhost:4200/** adresine göz atın.
1. **Oturum aç**' ı seçin.
1. Microsoft Graph çağırmak için **profil** ' i seçin.

Tarayıcı uygulamayı yükledikten sonra **oturum aç**' ı seçin. İlk kez oturum açmaya başladığınızda, uygulamanın profilinize erişmesine izin vermek için onayınızı sağlamanız ve oturumunuzu açmanız istenir. Başarıyla oturum açtıktan sonra **profil**' i seçin ve Kullanıcı profili bilgileriniz sayfada görüntülenir.

## <a name="how-the-sample-works"></a>Örneğin nasıl çalıştığı

![Bu hızlı başlangıçta örnek uygulamanın nasıl çalıştığını gösteren diyagram](./media/quickstart-v2-angular/diagram-auth-flow-spa-angular.svg)


## <a name="next-steps"></a>Sonraki adımlar

Ardından, bir kullanıcı oturumunu nasıl kullanabileceğinizi ve angular öğreticisinde belirteç edinmeyi öğrenin:

> [!div class="nextstepaction"]
> [Angular öğreticisi](./tutorial-v2-angular.md)