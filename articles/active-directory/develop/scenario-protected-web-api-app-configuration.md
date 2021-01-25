---
title: Korumalı Web API uygulamalarını yapılandırma | Mavisi
titleSuffix: Microsoft identity platform
description: Korumalı bir Web API 'SI oluşturmayı ve uygulamanızın kodunu yapılandırmayı öğrenin.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 07/15/2020
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 3a26157949ff6ef69c9c009dfdd40781b47bc761
ms.sourcegitcommit: 5cdd0b378d6377b98af71ec8e886098a504f7c33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/25/2021
ms.locfileid: "98753576"
---
# <a name="protected-web-api-code-configuration"></a>Korumalı Web API 'SI: kod yapılandırması

Korunan Web API 'niz için kodu yapılandırmak üzere şunları anlamanız gerekir:

- API 'Leri korumalı olarak tanımlar.
- Bir taşıyıcı belirtecini yapılandırma.
- Belirteci doğrulama.

## <a name="what-defines-aspnet-and-aspnet-core-apis-as-protected"></a>ASP.NET ve ASP.NET Core API 'Leri korunan olarak tanımlar?

Web Apps gibi, ASP.NET ve ASP.NET Core Web API 'Leri, denetleyici eylemlerine **[Yetkilendir]** özniteliği ön eki eklendiği için korunur. Denetleyici eylemleri yalnızca API bir yetkili kimlikle çağrılırsa çağrılabilir.

Aşağıdaki soruları göz önünde bulundurun:

- Yalnızca bir uygulama, bir Web API 'sini çağırabilir. API bu uygulamayı çağıran uygulamanın kimliğini nasıl bilir?
- Uygulama bir kullanıcı adına API 'yi çağırırsa, kullanıcının kimliği nedir?

## <a name="bearer-token"></a>Taşıyıcı belirteci

Uygulama çağrıldığında üst bilgide ayarlanan taşıyıcı belirteç, uygulama kimliği hakkında bilgi içerir. Web uygulaması, bir Daemon uygulamasından hizmetten hizmete çağrılar kabul etmediği takdirde, kullanıcı hakkındaki bilgileri de barındırır.

Aşağıda, .NET için Microsoft kimlik doğrulama kitaplığı (MSAL.NET) ile bir belirteç aldıktan sonra API 'YI çağıran bir istemciyi gösteren bir C# kod örneği verilmiştir:

```csharp
var scopes = new[] {$"api://.../access_as_user"};
var result = await app.AcquireToken(scopes)
                      .ExecuteAsync();

httpClient = new HttpClient();
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);

// Call the web API.
HttpResponseMessage response = await _httpClient.GetAsync(apiUri);
```

> [!IMPORTANT]
> İstemci uygulaması, *Web API 'si Için* Microsoft Identity platformu için taşıyıcı belirtecini ister. Web API 'SI, belirtecin doğrulanması ve içerdiği talepleri görüntülemesi gereken tek uygulamadır. İstemci uygulamaları, belirteçlerdeki talepleri incelemeye asla denememelidir.
>
> Gelecekte, Web API 'SI belirtecin şifrelenmesini gerektirebilir. Bu gereksinim, erişim belirteçlerini görüntüleyebilen istemci uygulamalarına erişimi engeller.

## <a name="jwtbearer-configuration"></a>Jwttaşıyıcı yapılandırması

Bu bölümde, bir taşıyıcı belirtecinin nasıl yapılandırılacağı açıklanmaktadır.

### <a name="config-file"></a>Yapılandırma dosyası

```Json
{
  "AzureAd": {
    "Instance": "https://login.microsoftonline.com/",
    "ClientId": "[Client_id-of-web-api-eg-2ec40e65-ba09-4853-bcde-bcb60029e596]",
    /*
      You need specify the TenantId only if you want to accept access tokens from a single tenant
     (line-of-business app).
      Otherwise, you can leave them set to common.
      This can be:
      - A GUID (Tenant ID = Directory ID)
      - 'common' (any organization and personal accounts)
      - 'organizations' (any organization)
      - 'consumers' (Microsoft personal accounts)
    */
    "TenantId": "common"
  },
  "Logging": {
    "LogLevel": {
      "Default": "Warning"
    }
  },
  "AllowedHosts": "*"
}
```

#### <a name="case-where-you-used-a-custom-app-id-uri-for-your-web-api"></a>Web API 'niz için özel bir uygulama KIMLIĞI URI 'SI kullandığınız durumlar

Uygulama kayıt portalı tarafından önerilen uygulama KIMLIĞI URI 'sini kabul ettiyseniz, hedef kitleyi belirtmeniz gerekmez (bkz. [uygulama kimliği URI 'si ve kapsamlar](scenario-protected-web-api-app-registration.md#application-id-uri-and-scopes)). Aksi takdirde, `Audience` değeri, Web API 'niz Için uygulama kimliği URI 'si olan bir özellik eklemelisiniz.

```Json
{
  "AzureAd": {
    "Instance": "https://login.microsoftonline.com/",
    "ClientId": "[Client_id-of-web-api-eg-2ec40e65-ba09-4853-bcde-bcb60029e596]",
    "TenantId": "common",
    "Audience": "custom App ID URI for your web API"
  },
  // more lines
}
```

### <a name="code-initialization"></a>Kod başlatma

Bir uygulama bir **[Yetkilendir]** özniteliği tutan bir denetleyici eyleminde çağrıldığında, ASP.NET ve ASP.NET Core erişim belirtecini yetkilendirme üstbilgisinin taşıyıcı belirtecinden ayıklayın. Daha sonra erişim belirteci, .NET için Microsoft IdentityModel uzantıları 'nı çağıran Jwttaşıyıcı ara yazılıma iletilir.

#### <a name="microsoftidentityweb"></a>Microsoft. Identity. Web

Microsoft, bir Web API 'sini ASP.NET Core geliştirilirken [Microsoft. Identity. Web](https://www.nuget.org/packages/Microsoft.Identity.Web) NuGet paketini kullanmanızı önerir.

_Microsoft. Identity. Web_ , .net için ASP.NET Core, kimlik doğrulama ara yazılımı ve [Microsoft kimlik doğrulama kitaplığı (msal)](msal-overview.md) arasında Tutkallamayı sağlar. Daha net, daha güçlü bir geliştirici deneyimi sağlar ve Microsoft Identity platform ve Azure AD B2C gücünden yararlanır.

#### <a name="using-microsoftidentityweb-templates"></a>Microsoft. Identity. Web şablonlarını kullanma

Microsoft. Identity. Web proje şablonlarını kullanarak sıfırdan bir Web API 'SI oluşturabilirsiniz. Ayrıntılar için bkz. [Microsoft. Identity. Web-Web API proje şablonu](https://aka.ms/ms-id-web/webapi-project-templates).

#### <a name="starting-from-an-existing-aspnet-core-31-application"></a>Mevcut bir ASP.NET Core 3,1 uygulamasından başlayarak

Günümüzde ASP.NET Core 3,1, Microsoft. AspNetCore. AzureAD. UI kitaplığını kullanır. Ara yazılım Startup.cs dosyasında başlatılır.

```csharp
using Microsoft.AspNetCore.Authentication.JwtBearer;
```

Bu yönerge, ara yazılım Web API 'sine eklenir:

```csharp
// This method gets called by the runtime. Use this method to add services to the container.
public void ConfigureServices(IServiceCollection services)
{
  services.AddAuthentication(AzureADDefaults.JwtBearerAuthenticationScheme)
          .AddAzureADBearer(options => Configuration.Bind("AzureAd", options));
}
```

 Şu anda ASP.NET Core şablonları, kuruluşunuzdaki veya herhangi bir kuruluşun içindeki kullanıcıların oturum açmasını sağlayan Azure Active Directory (Azure AD) Web API 'Leri oluşturur. Kişisel hesaplarla kullanıcıları oturum açtıklarında oturum açabilirler. Ancak, *Startup.cs*' deki kodu değiştirerek Microsoft [. Identity. Web](https://www.nuget.org/packages/Microsoft.Identity.Web) kullanarak şablonları Microsoft Identity platformu kullanacak şekilde değiştirebilirsiniz:

```csharp
using Microsoft.Identity.Web;
```

```csharp
public void ConfigureServices(IServiceCollection services)
{
 // Adds Microsoft Identity platform (AAD v2.0) support to protect this API
 services.AddMicrosoftIdentityWebApiAuthentication(Configuration, "AzureAd");

 services.AddControllers();
}
```

Ayrıca, şunu yazabilirsiniz (eşdeğer)

```csharp
public void ConfigureServices(IServiceCollection services)
{
 // Adds Microsoft Identity platform (AAD v2.0) support to protect this API
 services.AddAuthentication(AzureADDefaults.JwtBearerAuthenticationScheme)
             .AddMicrosoftIdentityWebApi(Configuration, "AzureAd");

services.AddControllers();
}
```

> [!NOTE]
> Microsoft. Identity. Web kullanıyorsanız ve ' `Audience` de *appsettings.js*' yi ayarlamazsanız, aşağıdakiler kullanılır:
> -  `$"{ClientId}"`[erişim belirtecinin kabul edilen sürümünü](scenario-protected-web-api-app-registration.md#accepted-token-version) `2` veya Azure AD B2C Web API 'lerini olarak ayarladıysanız.
> - `$"api://{ClientId}` diğer tüm durumlarda (v 1.0 [erişim belirteçleri](access-tokens.md)için).
> Ayrıntılar için bkz. Microsoft. Identity. Web [kaynak kodu](https://github.com/AzureAD/microsoft-identity-web/blob/d2ad0f5f830391a34175d48621a2c56011a45082/src/Microsoft.Identity.Web/Resource/RegisterValidAudience.cs#L70-L83).

Yukarıdaki kod parçacığı [ASP.NET Core Web API 'si artımlı öğreticiden](https://github.com/Azure-Samples/active-directory-dotnet-native-aspnetcore-v2/blob/63087e83326e6a332d05fee6e1586b66d840b08f/1.%20Desktop%20app%20calls%20Web%20API/TodoListService/Startup.cs#L23-L28)ayıklanır. **AddMicrosoftIdentityWebApiAuthentication** ayrıntısı [Microsoft. Identity. Web](microsoft-identity-web.md)içinde kullanılabilir. Bu yöntem, kendi ara yazılım tarafından belirtecin nasıl doğrulandığını bildiren [Addmicrosoftidentitywebapi](/dotnet/api/microsoft.identity.web.microsoftidentitywebapiauthenticationbuilderextensions.addmicrosoftidentitywebapi?preserve-view=true&view=azure-dotnet-preview)' i çağırır.

## <a name="token-validation"></a>Belirteç doğrulama

Yukarıdaki kod parçacığında, Web Apps 'teki OpenID Connect ara yazılımı gibi Jwttaşıyıcı ara yazılımı, belirtecini değerine göre doğrular `TokenValidationParameters` . Gerektiğinde belirtecin şifresi çözülür, talepler ayıklanır ve imza doğrulanır. Bu durumda, ara yazılım bu verileri denetleyerek belirteci doğrular:

- Hedef kitle: belirteç Web API 'sine yöneliktir.
- Sub: Web API 'sini çağırmaya izin verilen bir uygulama için verildi.
- Veren: bir güvenilen güvenlik belirteci hizmeti (STS) tarafından verilmiş.
- Süre sonu: ömrü Aralık içinde.
- İmza: ile oynanmadı.

Özel doğrulamalar de olabilir. Örneğin, bir belirteçte gömülü olduğunda imzalama anahtarlarının güvenilir olduğunu ve belirtecin yeniden çalınmayacağını doğrulamak mümkündür. Son olarak, bazı protokoller belirli doğrulamalar gerektirir.

### <a name="validators"></a>Metninin

Doğrulama adımları, .NET açık kaynak kitaplığı [Için Microsoft IdentityModel uzantıları](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet) tarafından belirtilen doğrulayıcılar içinde yakalanır. Doğrulayıcılar, [Microsoft. IdentityModel. Tokens/Doğrulayıcı. cs](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet/blob/master/src/Microsoft.IdentityModel.Tokens/Validators.cs)kitaplık kaynak dosyasında tanımlanmıştır.

Bu tabloda doğrulayıcılar açıklanmaktadır:

| Doğrulayıcı | Açıklama |
|---------|---------|
| **ValidateAudience** | Belirtecin sizin için belirteci doğrulayan uygulamanın olduğundan emin olur. |
| **Validateıssuer** | Belirtecin güvenilir bir STS tarafından verildiğini ve güvendiğiniz bir kişiden geldiğini sağlar. |
| **Validateıssuersigningkey** | Belirteç, belirteci imzalamak için kullanılan anahtara güvendiğini belirten uygulamayı sağlar. Anahtarın belirtece gömülü olduğu özel bir durum vardır. Ancak bu durum genellikle ortaya çıkar. |
| **ValidateLifetime** | Belirtecin hala veya zaten geçerli olduğundan emin olur. Doğrulayıcı, belirtecin kullanım ömrünün **NotBefore** ve **Expires** talepleri tarafından belirtilen aralıkta olup olmadığını denetler. |
| **ValidateSignature** | Belirtecin oynanmamasını sağlar. |
| **ValidateTokenReplay** | Belirtecin yeniden çalınmamasını sağlar. Bazı Onetime kullanım protokolleri için özel bir durum vardır. |

#### <a name="customizing-token-validation"></a>Belirteç doğrulamayı özelleştirme

Doğrulayıcılar **Tokenvalidationparameters** sınıfının özellikleriyle ilişkilendirilir. Özellikler, ASP.NET ve ASP.NET Core yapılandırmasından başlatılır.

Çoğu durumda, parametreleri değiştirmeniz gerekmez. Tek kiracılar olmayan uygulamalar özel durumlardır. Bu Web Apps kullanıcıları herhangi bir kuruluştan veya kişisel Microsoft hesaplarından kabul eder. Bu durumda verenler doğrulanması gerekir. Microsoft. Identity. Web, veren doğrulamasının yanı sıra ele alır. Ayrıntılar için bkz. Microsoft. Identity. Web [Aadıssuervalidator](https://github.com/AzureAD/microsoft-identity-web/blob/master/src/Microsoft.Identity.Web/Resource/AadIssuerValidator.cs).

ASP.NET Core, belirteç doğrulama parametrelerini özelleştirmek istiyorsanız, *Startup.cs* içinde aşağıdaki kod parçacığını kullanın:

```c#
services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
        .AddMicrosoftIdentityWebApi(Configuration);
services.Configure<JwtBearerOptions>(JwtBearerDefaults.AuthenticationScheme, options =>
{
  var existingOnTokenValidatedHandler = options.Events.OnTokenValidated;
  options.Events.OnTokenValidated = async context =>
  {
       await existingOnTokenValidatedHandler(context);
      // Your code to add extra configuration that will be executed after the current event implementation.
      options.TokenValidationParameters.ValidIssuers = new[] { /* list of valid issuers */ };
      options.TokenValidationParameters.ValidAudiences = new[] { /* list of valid audiences */};
  }
});
```

ASP.NET MVC için aşağıdaki kod örneği, özel belirteç doğrulamanın nasıl yapılacağını göstermektedir:

https://github.com/azure-samples/active-directory-dotnet-webapi-manual-jwt-validation

## <a name="token-validation-in-azure-functions"></a>Azure Işlevlerinde belirteç doğrulama

Ayrıca, Azure Işlevlerinde gelen erişim belirteçlerini doğrulayabilirsiniz. GitHub 'da aşağıdaki kod örneklerinde bu doğrulamanın örneklerini bulabilirsiniz:

- .NET: [Azure-Samples/MS-Identity-DotNet-WebApi-azurefunctions](https://github.com/Azure-Samples/ms-identity-dotnet-webapi-azurefunctions)
- Node.js: [Azure-Samples/MS-Identity-NodeJS-WebApi-azurefunctions](https://github.com/Azure-Samples/ms-identity-nodejs-webapi-azurefunctions)
- Python: [Azure-Samples/MS-Identity-Python-WebApi-azurefunctions)](https://github.com/Azure-Samples/ms-identity-python-webapi-azurefunctions)

## <a name="next-steps"></a>Sonraki adımlar

Bu senaryodaki sonraki makaleye geçin, [kodunuzda kapsamları ve uygulama rollerini doğrulayın](scenario-protected-web-api-verification-scope-app-roles.md).