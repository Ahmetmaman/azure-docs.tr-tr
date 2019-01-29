---
title: Azure Active Directory v2.0 kimlik doğrulama kitaplıkları | Microsoft Docs
description: Uyumlu bir istemci kitaplıkları ve sunucusu ara yazılım kitaplıkları ve ilgili kitaplık, kaynak ve örnekleri bağlantılar, Azure Active Directory v2.0 uç noktası için.
services: active-directory
documentationcenter: ''
author: negoe
manager: mtillman
editor: ''
ms.assetid: 19cec615-e51f-4141-9f8c-aaf38ff9f746
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/14/2018
ms.author: negoe
ms.reviewer: jmprieur, saeeda
ms.custom: aaddev
ms.openlocfilehash: a7745a0c8a53a0726a27dc1e2642733bafeb8f30
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2019
ms.locfileid: "55104583"
---
# <a name="azure-active-directory-v20-authentication-libraries"></a>Azure Active Directory v2.0 kimlik doğrulama kitaplıkları

[Azure Active Directory (Azure AD) v2.0 uç noktası](active-directory-v2-compare.md) endüstri standardı OAuth 2.0 ve Openıd Connect 1.0 protokollerini destekler. Microsoft Authentication Library (MSAL), Azure AD v2.0 uç noktası ile çalışacak şekilde tasarlanmıştır. OAuth 2.0 ve Openıd Connect 1.0 destekleyen açık kaynak kitaplıkları kullanmak mümkündür.

Bir güvenlik geliştirme yaşam döngüsü (SDL) yöntemleri gibi izleyen Protokolü etki alanı uzmanları tarafından yazılmış kitaplıkları kullanmanızı tavsiye edilir [biri Microsoft tarafından izlenen][Microsoft-SDL]. Elle kod protokoller için karar verirseniz, Microsoft'un SDL ve ödeme standartları özellikleri her protokol için güvenlik konuları dikkat kapatmak gibi bir Metodoloji izlemelidir.

> [!NOTE]
> Azure AD için sık sorulan sorular v1.0 kitaplığı (ADAL) mi arıyorsunuz? Kullanıma alma [ADAL kitaplığını Kılavuzu](active-directory-authentication-libraries.md).

## <a name="types-of-libraries"></a>Tür kitaplıkları

Azure AD v2.0 uç noktası, iki tür kitaplıkları ile çalışır:

* **İstemci kitaplıkları**: Yerel istemcileri ve sunucuları Microsoft Graph gibi bir kaynak çağırmak için erişim belirteçlerini almak için istemci kitaplıkları'nı kullanın.
* **Server ara yazılım kitaplıklarını**: Web uygulamaları, kullanıcı oturum açma için server ara yazılım kitaplıklarını kullanın. Web API'leri, diğer sunucuları veya yerel istemciler tarafından gönderilen belirteçleri doğrulamak için sunucu ara yazılım kitaplıkları kullanın.

## <a name="library-support"></a>Kitaplık desteği

Kitaplıkları iki destek kategoriye ayrılır:

* **Microsoft tarafından desteklenen**: Microsoft bu kitaplıklar için düzeltme sağlayan ve SDL işlemi yapana aksaklıkla Bu kitaplıklar üzerinde.
* **Uyumlu**: Microsoft, temel senaryolarda bu kitaplıklar test ve çalıştıkları v2.0 uç noktası ile onaylandı. Bu kitaplıklar için düzeltmeler ve bu kitaplıklar incelenmesi yapmış değil Microsoft sağlamaz. Sorunları ve özellik istekleri kitaplığın açık kaynak projesine yönlendirilebilir.

V2.0 uç nokta ile çalışmaya kitaplıkları listesi için bu makalenin sonraki bölümlere bakın.

## <a name="microsoft-supported-client-libraries"></a>Microsoft tarafından desteklenen istemci kitaplıkları

Korumalı bir Web API'sini çağırmak için bir belirteç almak için kullanılan istemci kimlik doğrulama kitaplıkları

| Platform | Kitaplık | İndirme | Kaynak kod | Örnek | Başvuru | Kavramsal belge | Yol Haritası |
| --- | --- | --- | --- | --- | --- | --- | ---|
| ![JavaScript](media/sample-v2-code/logo_js.png) | MSAL.js (Önizleme) | [NPM](https://www.npmjs.com/package/msal) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angularjs/README.md) |  [Tek sayfalı uygulama](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi-v2) |  | [Wiki](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki)|
|![Angular JS](media/sample-v2-code/logo_angular.png) | MSAL Angular JS | [NPM](https://www.npmjs.com/package/@azure/msal-angularjs) | [GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angularjs/README.md) |  |  | |
![Angular](media/sample-v2-code/logo_angular.png) | MSAL Angular(Preview) | [NPM](https://www.npmjs.com/package/@azure/msal-angular) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) | | | |
| ![.NET Framework](media/sample-v2-code/logo_NET.png) ![UWP](media/sample-v2-code/logo_windows.png) ![Xamarin](media/sample-v2-code/logo_xamarin.png) | MSAL .NET (Önizleme) |[NuGet](https://www.nuget.org/packages/Microsoft.Identity.Client) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) | [Masaüstü uygulaması](guidedsetups/active-directory-mobileanddesktopapp-windowsdesktop-intro.md) | [MSAL.NET](https://docs.microsoft.com/dotnet/api/microsoft.identity.client?view=azure-dotnet-preview) |[Wiki](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki#conceptual-documentation) | [Yol Haritası](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki#roadmap)
| ![iOS / Objective C ya da swift'te](media/sample-v2-code/logo_iOS.png) | MSAL obj_c (Önizleme) | [GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-objc) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-objc) | [iOS uygulaması](https://github.com/Azure-Samples/active-directory-msal-ios-swift) |  |
|![Android / Java](media/sample-v2-code/logo_Android.png) | MSAL (Önizleme) | [ Merkezi depo](https://repo1.maven.org/maven2/com/microsoft/identity/client/msal/) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-android) | [Android uygulaması](quickstart-v2-android.md) | [JavaDocs](https://javadoc.io/doc/com.microsoft.identity.client/msal) | | |

## <a name="microsoft-supported-server-middleware-libraries"></a>Microsoft tarafından desteklenen sunucusu ara yazılım kitaplıkları

Ara yazılım kitaplıkları, Web uygulamaları ve Web API'leri korumak için kullanılır. Web uygulaması veya ASP.NET veya ASP.NET Core ile yazılmış Web API'si için ara yazılım kitaplıkları ASP.NET tarafından kullanılan / ASP.NET Core

| Platform | Kitaplık | İndirme | Kaynak kodu | Örnek | Başvuru
| --- | --- | --- | --- | --- | --- |
| ![.NET](media/sample-v2-code/logo_NET.png) ![.NET Core](media/sample-v2-code/logo_NETcore.png) | ASP.NET güvenliği |[NuGet](https://www.nuget.org/packages/Microsoft.AspNet.Mvc/) |[ASP.NET güvenlik (GitHub)](https://github.com/aspnet/Security) |[MVC uygulaması](quickstart-v2-aspnet-webapp.md) |[ASP.NET API Başvurusu](https://docs.microsoft.com/dotnet/api/?view=aspnetcore-2.0) |
| ![.NET](media/sample-v2-code/logo_NET.png)| .NET için IdentityModel uzantıları| |[GitHub](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet) | [MVC uygulaması](quickstart-v2-aspnet-webapp.md) |[Başvuru](https://docs.microsoft.com/dotnet/api/overview/azure/activedirectory/client?view=azure-dotnet) |
| ![Node.js](media/sample-v2-code/logo_nodejs.png) | Azure AD Passport |[NPM](https://www.npmjs.com/package/passport-azure-ad) |[GitHub](https://github.com/AzureAD/passport-azure-ad) | [Web uygulaması](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs) | |

## <a name="compatible-client-libraries"></a>Uyumlu bir istemci kitaplıkları

| Platform | Kitaplık adı | Test edilen sürüm | Kaynak kod | Örnek |
|:---:|:---:|:---:|:---:|:---:|
|![JavaScript](media/sample-v2-code/logo_js.png)|[Hello.js](https://adodson.com/hello.js/) |1.13.5 |[Hello.js](https://github.com/MrSwitch/hello.js) |[SPA](https://github.com/Azure-Samples/active-directory-javascript-graphapi-web-v2) |
| ![Java](media/sample-v2-code/logo_java.png) | [Java içerdiğini açıklayın](https://github.com/scribejava/scribejava) | [Sürüm 3.2.0](https://github.com/scribejava/scribejava/releases/tag/scribejava-3.2.0) | [ScribeJava](https://github.com/scribejava/scribejava/) | |
| ![PHP](media/sample-v2-code/logo_php.png) | [PHP ligi oauth2 istemci](https://github.com/thephpleague/oauth2-client) | [Sürüm 1.4.2](https://github.com/thephpleague/oauth2-client/releases/tag/1.4.2) | [oauth2 istemci](https://github.com/thephpleague/oauth2-client/) | |
| ![Ruby](media/sample-v2-code/logo_ruby.png) |[OmniAuth](https://github.com/omniauth/omniauth/wiki) |omniauth:1.3.1<br />omniauth-oauth2:1.4.0 |[OmniAuth](https://github.com/omniauth/omniauth)<br />[OmniAuth OAuth2](https://github.com/intridea/omniauth-oauth2) |  |
![iOS](media/sample-v2-code/logo_iOS.png) |[NXOAuth2Client](https://github.com/nxtbgthng/OAuth2Client) |1.2.8 |[NXOAuth2Client](https://github.com/nxtbgthng/OAuth2Client) |[Yerel uygulama örneği](active-directory-v2-devquickstarts-ios.md) |

İçin standartlara uygun bir kitaplık v2.0 uç noktası kullanabilirsiniz. böylece desteği için nereye bilmek önemlidir.

* Sorunlar ve yeni özellik istekleriniz kitaplığı kod için kitaplık sahibine başvurun.
* Sorunlar ve Hizmet tarafı protokolünü uygulamasındaki yeni özellik istekleri için Microsoft ile iletişime geçin.
* [Bir özellik isteği](https://feedback.azure.com/forums/169401-azure-active-directory) protokolünde görmek istediğiniz ek özellikler.
* [Bir destek isteği oluşturma](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request) bir sorun bulursanız burada Azure AD v2.0 uç noktası OAuth 2.0 veya Openıd Connect 1.0 ile uyumlu değildir.

## <a name="related-content"></a>İlgili içerik

Azure AD v2.0 uç noktası hakkında daha fazla bilgi için bkz: [Azure AD uygulama modeli v2.0 genel bakış][AAD-App-Model-V2-Overview].

<!--Image references-->

<!--Reference style links -->
[AAD-App-Model-V2-Overview]: v2-overview.md
[ClientLib-NET-Lib]: https://www.nuget.org/packages/Microsoft.Identity.Client
[ClientLib-NET-Repo]: https://github.com/AzureAD/microsoft-authentication-library-for-dotnet
[ClientLib-NET-Sample]: active-directory-v2-devquickstarts-wpf.md
[ClientLib-Node-Lib]: https://www.npmjs.com/package/passport-azure-ad
[ClientLib-Node-Repo]: https://github.com/AzureAD/passport-azure-ad
[ClientLib-Node-Sample]:/
[ClientLib-Iosmac-Lib]:/
[ClientLib-Iosmac-Repo]:/
[ClientLib-Iosmac-Sample]:/
[ClientLib-Android-Lib]:/
[ClientLib-Android-Repo]:/
[ClientLib-Android-Sample]:/
[ClientLib-Js-Lib]:/
[ClientLib-Js-Repo]:/
[ClientLib-Js-Sample]:/

[Microsoft-SDL]: https://www.microsoft.com/sdl/default.aspx
[ServerLib-Net4-Owin-Oidc-Lib]: https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect/
[ServerLib-Net4-Owin-Oidc-Repo]: https://katanaproject.codeplex.com/
[ServerLib-Net4-Owin-Oidc-Sample]: active-directory-v2-devquickstarts-dotnet-web.md
[ServerLib-Net4-Owin-Oauth-Lib]: https://www.nuget.org/packages/Microsoft.Owin.Security.OAuth/
[ServerLib-Net4-Owin-Oauth-Repo]: https://katanaproject.codeplex.com/
[ServerLib-Net4-Owin-Oauth-Sample]: https://azure.microsoft.com/documentation/articles/active-directory-v2-devquickstarts-dotnet-api/
[ServerLib-Net-Jwt-Lib]: https://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt
[ServerLib-Net-Jwt-Repo]: https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet
[ServerLib-Net-Jwt-Sample]:/
[ServerLib-NetCore-Owin-Oidc-Lib]: https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.OpenIdConnect/
[ServerLib-NetCore-Owin-Oidc-Repo]: https://github.com/aspnet/Security
[ServerLib-NetCore-Owin-Oidc-Sample]: https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect-aspnetcore-v2
[ServerLib-NetCore-Owin-Oauth-Lib]: https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.OAuth/
[ServerLib-NetCore-Owin-Oauth-Repo]: https://github.com/aspnet/Security
[ServerLib-NetCore-Owin-Oauth-Sample]:/
[ServerLib-Node-Lib]: https://www.npmjs.com/package/passport-azure-ad
[ServerLib-Node-Repo]: https://github.com/AzureAD/passport-azure-ad/
[ServerLib-Node-Sample]: https://azure.microsoft.com/documentation/articles/active-directory-v2-devquickstarts-node-web/
