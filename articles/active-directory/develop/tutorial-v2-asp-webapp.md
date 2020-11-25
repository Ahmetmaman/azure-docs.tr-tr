---
title: 'Öğretici: kimlik doğrulaması için Microsoft Identity platformunu kullanan bir ASP.NET Web uygulaması oluşturma | Mavisi'
titleSuffix: Microsoft identity platform
description: Bu öğreticide, Kullanıcı oturum açmayı etkinleştirmek için Microsoft Identity platformu ve OWıN ara yazılımı kullanan bir ASP.NET Web uygulaması oluşturacaksınız.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: tutorial
ms.workload: identity
ms.date: 08/28/2019
ms.author: jmprieur
ms.custom: devx-track-csharp, aaddev, identityplatformtop40
ms.openlocfilehash: 0dad7add63102d462a2111f1ecf12ae43dae123c
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "95996194"
---
# <a name="tutorial-add-sign-in-to-microsoft-to-an-aspnet-web-app"></a>Öğretici: Microsoft 'a bir ASP.NET Web uygulamasına oturum açma ekleme

Bu öğreticide, .NET (OWıN) ara yazılımı ve Microsoft Identity platformu için açık Web arabirimi kullanarak kullanıcılara oturum açan bir ASP.NET MVC web uygulaması oluşturacaksınız.

Bu kılavuzu tamamladığınızda, uygulamanız kişisel hesapların oturum açma işlemlerini outlook.com ve live.com beğeni kabul edebilir. Ayrıca, Microsoft Identity platformu ile tümleştirilen herhangi bir şirketten veya kuruluştan iş ve okul hesapları, uygulamanızda oturum açabiliyor.

Bu öğreticide:

> [!div class="checklist"]
> * Visual Studio 'da *ASP.NET Web uygulaması* projesi oluşturma
> * .NET (OWıN) ara yazılım bileşenleri için açık Web arabirimi ekleme
> * Kullanıcı oturum açma ve oturum kapatma desteği için kod ekleme
> * Uygulamayı Azure portal kaydetme
> * Uygulamayı test etme

## <a name="prerequisites"></a>Önkoşullar

* **ASP.net ve Web geliştirme** iş yükü yüklü [Visual Studio 2019](https://visualstudio.microsoft.com/vs/)

## <a name="how-the-sample-app-generated-by-this-guide-works"></a>Bu kılavuz tarafından oluşturulan örnek uygulamanın nasıl çalıştığı

![Bu öğretici tarafından oluşturulan örnek uygulamanın nasıl çalıştığını gösterir](media/active-directory-develop-guidedsetup-aspnetwebapp-intro/aspnetbrowsergeneral.svg)

Oluşturduğunuz örnek uygulama, kullanıcının oturum açma düğmesiyle kimlik doğrulaması yapmasını isteyen bir ASP.NET Web sitesine erişmek için tarayıcıyı kullandığınız bir senaryoya dayalıdır. Bu senaryoda web sayfasını oluşturma işlemlerinin çoğu sunucu tarafında gerçekleştirilmektedir.

## <a name="libraries"></a>Kitaplıklar

Bu kılavuz aşağıdaki kitaplıkları kullanır:

|Kitaplık|Description|
|---|---|
|[Microsoft.Owin.Security.OpenIdConnect](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect/)|Uygulamanın kimlik doğrulaması için OpenIdConnect kullanmasını sağlayan ara yazılım|
|[Microsoft.Owin.Security.Cookies](https://www.nuget.org/packages/Microsoft.Owin.Security.Cookies)|Bir uygulamanın tanımlama bilgilerini kullanarak bir Kullanıcı oturumu korumasına olanak tanıyan ara yazılım|
|[Microsoft.Owin.Host.SystemWeb](https://www.nuget.org/packages/Microsoft.Owin.Host.SystemWeb)|ASP.NET istek işlem hattını kullanarak OWıN tabanlı uygulamaların Internet Information Services (IIS) üzerinde çalışmasını sağlayan ara yazılım|

## <a name="set-up-your-project"></a>Projenizi ayarlama

Bu bölümde, OpenID Connect kullanılarak bir ASP.NET projesine OWıN ara yazılımı aracılığıyla kimlik doğrulama işlem hattının nasıl yükleneceği ve yapılandırılacağı açıklanmaktadır.

> Bunun yerine bu örneğin Visual Studio projesini indirmeyi tercih etmek istiyor musunuz? [Bir proje indirin](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-DotNet/archive/master.zip) ve yürütmeden önce kod örneğini yapılandırmak Için [Uygulamanızı kaydetme](#register-your-application) bölümüne atlayın.

### <a name="create-your-aspnet-project"></a>ASP.NET projenizi oluşturma

1. Visual Studio 'da: **Dosya**  >  **Yeni**  >  **Proje**' ye gidin.
2. **Visual C#\Web** bölümünde **ASP.NET Web Uygulaması (.NET Framework)** girişini seçin.
3. Uygulamanıza bir ad verin ve **Tamam**'ı seçin.
4. **Boş**' ı seçin ve ardından **MVC** başvurularını eklemek için onay kutusunu işaretleyin.

## <a name="add-authentication-components"></a>Kimlik doğrulama bileşenleri ekleme

1. Visual Studio 'da: **Araçlar**  >  **NuGet Paket Yöneticisi**  >  **Paket Yöneticisi konsolu**' na gidin.
2. Paket Yöneticisi Konsolu penceresinde aşağıdakileri yazarak *OWIN ara yazılım NuGet paketlerini* ekleyin:

    ```powershell
    Install-Package Microsoft.Owin.Security.OpenIdConnect
    Install-Package Microsoft.Owin.Security.Cookies
    Install-Package Microsoft.Owin.Host.SystemWeb
    ```

### <a name="about-these-libraries"></a>Bu kitaplıklar hakkında
Bu kitaplıklar, tanımlama bilgisi tabanlı kimlik doğrulaması aracılığıyla OpenID Connect kullanarak çoklu oturum açmayı (SSO) etkinleştirir. Kimlik doğrulaması tamamlandıktan ve kullanıcıyı temsil eden belirteç uygulamanıza gönderildikten sonra OWIN ara yazılımı bir oturum tanımlama bilgisi oluşturur. Daha sonra tarayıcı bu tanımlama bilgisini sonraki isteklerde kullanır, böylece Kullanıcı parolayı yeniden yazması gerekmez ve ek bir doğrulama gerekmez.

## <a name="configure-the-authentication-pipeline"></a>Kimlik doğrulama işlem hattını yapılandırma

Aşağıdaki adımlar, OpenID Connect kimlik doğrulamasını yapılandırmak için bir OWıN ara yazılım başlangıç sınıfı oluşturmak için kullanılır. Bu sınıf, IIS işleminiz başladığında otomatik olarak yürütülür.

> [!TIP]
> Projenizin kök klasöründe `Startup.cs` adlı bir dosya yoksa:
> 1. Projenin kök klasörüne sağ tıklayın ve ardından **Add**  >  **Yeni öğe** Ekle  >  **Başlangıç sınıfı**' nı seçin.<br/>
> 2. **Startup.cs** olarak adlandırın.
>
>> Seçilen sınıfın bir OWıN başlangıç sınıfı olduğundan ve standart C# sınıfı olmadığından emin olun. [Assembly: Owınstartup (typeof ({NameSpace}) gördüğünüz doğrulayarak bunu onaylayın. Başlangıç))] ad alanı üzerinde.

1. Startup.cs 'e *Owın* ve *Microsoft. IdentityModel* başvuruları ekleyin:

    ```csharp
    using Microsoft.Owin;
    using Owin;
    using Microsoft.IdentityModel.Protocols.OpenIdConnect;
    using Microsoft.IdentityModel.Tokens;
    using Microsoft.Owin.Security;
    using Microsoft.Owin.Security.Cookies;
    using Microsoft.Owin.Security.OpenIdConnect;
    using Microsoft.Owin.Security.Notifications;
    ```

2. Başlangıç sınıfını aşağıdaki kodla değiştirin:

    ```csharp
    public class Startup
    {
        // The Client ID is used by the application to uniquely identify itself to Microsoft identity platform.
        string clientId = System.Configuration.ConfigurationManager.AppSettings["ClientId"];

        // RedirectUri is the URL where the user will be redirected to after they sign in.
        string redirectUri = System.Configuration.ConfigurationManager.AppSettings["RedirectUri"];

        // Tenant is the tenant ID (e.g. contoso.onmicrosoft.com, or 'common' for multi-tenant)
        static string tenant = System.Configuration.ConfigurationManager.AppSettings["Tenant"];

        // Authority is the URL for authority, composed by Microsoft identity platform endpoint and the tenant name (e.g. https://login.microsoftonline.com/contoso.onmicrosoft.com/v2.0)
        string authority = String.Format(System.Globalization.CultureInfo.InvariantCulture, System.Configuration.ConfigurationManager.AppSettings["Authority"], tenant);

        /// <summary>
        /// Configure OWIN to use OpenIdConnect
        /// </summary>
        /// <param name="app"></param>
        public void Configuration(IAppBuilder app)
        {
            app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

            app.UseCookieAuthentication(new CookieAuthenticationOptions());
            app.UseOpenIdConnectAuthentication(
                new OpenIdConnectAuthenticationOptions
                {
                    // Sets the ClientId, authority, RedirectUri as obtained from web.config
                    ClientId = clientId,
                    Authority = authority,
                    RedirectUri = redirectUri,
                    // PostLogoutRedirectUri is the page that users will be redirected to after sign-out. In this case, it is using the home page
                    PostLogoutRedirectUri = redirectUri,
                    Scope = OpenIdConnectScope.OpenIdProfile,
                    // ResponseType is set to request the id_token - which contains basic information about the signed-in user
                    ResponseType = OpenIdConnectResponseType.IdToken,
                    // ValidateIssuer set to false to allow personal and work accounts from any organization to sign in to your application
                    // To only allow users from a single organizations, set ValidateIssuer to true and 'tenant' setting in web.config to the tenant name
                    // To allow users from only a list of specific organizations, set ValidateIssuer to true and use ValidIssuers parameter
                    TokenValidationParameters = new TokenValidationParameters()
                    {
                        ValidateIssuer = false // This is a simplification
                    },
                    // OpenIdConnectAuthenticationNotifications configures OWIN to send notification of failed authentications to OnAuthenticationFailed method
                    Notifications = new OpenIdConnectAuthenticationNotifications
                    {
                        AuthenticationFailed = OnAuthenticationFailed
                    }
                }
            );
        }

        /// <summary>
        /// Handle failed authentication requests by redirecting the user to the home page with an error in the query string
        /// </summary>
        /// <param name="context"></param>
        /// <returns></returns>
        private Task OnAuthenticationFailed(AuthenticationFailedNotification<OpenIdConnectMessage, OpenIdConnectAuthenticationOptions> context)
        {
            context.HandleResponse();
            context.Response.Redirect("/?errormessage=" + context.Exception.Message);
            return Task.FromResult(0);
        }
    }
    ```

> [!NOTE]
> Ayar `ValidateIssuer = false` , bu hızlı başlangıç için bir basitleştirmesi. Gerçek uygulamalarda, sertifikayı vereni doğrulamanız gerekir.
> Bunun nasıl yapılacağını öğrenmek için örneklere bakın.

### <a name="more-information"></a>Daha fazla bilgi

*Openıdconnectauthenticationoptions* içinde sağladığınız parametreler, uygulamanın Microsoft Identity platformu ile iletişim kurması için koordinatlar olarak görev yapar. OpenID Connect ara yazılımı arka planda tanımlama bilgileri kullandığından, önceki kodun gösterdiği gibi tanımlama bilgisi kimlik doğrulamasını da ayarlamanız gerekir. *Validateıssuer* değeri, Openıdconnect 'in belirli bir kuruluşa erişimi kısıtlayamayacağını söyler.

## <a name="add-a-controller-to-handle-sign-in-and-sign-out-requests"></a>Oturum açma ve oturum kapatma isteklerini işlemek için bir denetleyici ekleme

Oturum açma ve oturum kapatma yöntemlerini kullanıma sunmak üzere yeni bir denetleyici oluşturmak için aşağıdaki adımları izleyin:

1.  **Denetleyiciler** klasörüne sağ tıklayın ve denetleyici **Ekle**' yi seçin  >  **Controller**.
2.  **MVC (.NET sürümü) Denetleyici – Boş** girişini seçin.
3.  **Ekle**’yi seçin.
4.  **HomeController** olarak adlandırın ve **Ekle**' yi seçin.
5.  Sınıfa OWıN başvuruları ekleyin:

    ```csharp
    using Microsoft.Owin.Security;
    using Microsoft.Owin.Security.Cookies;
    using Microsoft.Owin.Security.OpenIdConnect;
    ```

6. Oturum açmayı işlemek ve bir kimlik doğrulama sınaması başlatarak denetleyicinize kaydolmak için aşağıdaki iki yöntemi ekleyin:

    ```csharp
    /// <summary>
    /// Send an OpenID Connect sign-in request.
    /// Alternatively, you can just decorate the SignIn method with the [Authorize] attribute
    /// </summary>
    public void SignIn()
    {
        if (!Request.IsAuthenticated)
        {
            HttpContext.GetOwinContext().Authentication.Challenge(
                new AuthenticationProperties{ RedirectUri = "/" },
                OpenIdConnectAuthenticationDefaults.AuthenticationType);
        }
    }

    /// <summary>
    /// Send an OpenID Connect sign-out request.
    /// </summary>
    public void SignOut()
    {
        HttpContext.GetOwinContext().Authentication.SignOut(
                OpenIdConnectAuthenticationDefaults.AuthenticationType,
                CookieAuthenticationDefaults.AuthenticationType);
    }
    ```

## <a name="create-the-apps-home-page-for-user-sign-in"></a>Kullanıcının oturum açması için uygulamanın giriş sayfasını oluşturma

Visual Studio 'da, oturum açma düğmesini eklemek ve kimlik doğrulamasından sonra Kullanıcı bilgilerini görüntülemek için yeni bir görünüm oluşturun:

1.  **Görünümler\Giriş Sayfası** klasörüne sağ tıklayın ve **Görünüm Ekle**'yi seçin.
2.  Yeni Görünüm **dizinini** adlandırın.
3.  Oturum açma düğmesini de içeren aşağıdaki HTML kodunu dosyaya ekleyin:

    ```html
    <html>
    <head>
        <meta name="viewport" content="width=device-width" />
        <title>Sign in with Microsoft Guide</title>
    </head>
    <body>
    @if (!Request.IsAuthenticated)
    {
        <!-- If the user is not authenticated, display the sign-in button -->
        <a href="@Url.Action("SignIn", "Home")" style="text-decoration: none;">
            <svg xmlns="http://www.w3.org/2000/svg" xml:space="preserve" width="300px" height="50px" viewBox="0 0 3278 522" class="SignInButton">
            <style type="text/css">.fil0:hover {fill: #4B4B4B;} .fnt0 {font-size: 260px;font-family: 'Segoe UI Semibold', 'Segoe UI'; text-decoration: none;}</style>
            <rect class="fil0" x="2" y="2" width="3174" height="517" fill="black" />
            <rect x="150" y="129" width="122" height="122" fill="#F35325" />
            <rect x="284" y="129" width="122" height="122" fill="#81BC06" />
            <rect x="150" y="263" width="122" height="122" fill="#05A6F0" />
            <rect x="284" y="263" width="122" height="122" fill="#FFBA08" />
            <text x="470" y="357" fill="white" class="fnt0">Sign in with Microsoft</text>
            </svg>
        </a>
    }
    else
    {
        <span><br/>Hello @System.Security.Claims.ClaimsPrincipal.Current.FindFirst("name").Value;</span>
        <br /><br />
        @Html.ActionLink("See Your Claims", "Index", "Claims")
        <br /><br />
        @Html.ActionLink("Sign out", "SignOut", "Home")
    }
    @if (!string.IsNullOrWhiteSpace(Request.QueryString["errormessage"]))
    {
        <div style="background-color:red;color:white;font-weight: bold;">Error: @Request.QueryString["errormessage"]</div>
    }
    </body>
    </html>
    ```

### <a name="more-information"></a>Daha fazla bilgi
 Bu sayfa siyah arka plan üzerinde SVG biçiminde bir oturum açma düğmesi ekler:<br/>![Microsoft hesabıyla oturum açın düğmesi](media/active-directory-develop-guidedsetup-aspnetwebapp-use/aspnetsigninbuttonsample.png)<br/> Daha fazla oturum açma düğmesi için [marka yönergelerine](./howto-add-branding-in-azure-ad-apps.md "Marka yönergeleri")gidin.

## <a name="add-a-controller-to-display-users-claims"></a>Kullanıcının taleplerini göstermek için bir denetleyici ekleme
Bu denetleyici bir denetleyiciyi koruma amacıyla `[Authorize]` özniteliğini kullanma şeklini gösterir. Bu öznitelik yalnızca kimliği doğrulanmış kullanıcılara izin vererek denetleyiciye erişimi kısıtlar. Aşağıdaki kod, oturum açma kapsamında alınan kullanıcı taleplerini göstermek için özniteliğini kullanır:

1.  **Denetleyiciler** klasörüne sağ tıklayın ve ardından denetleyici **Ekle**' yi seçin  >  **Controller**.
2.  **MVC {sürüm} Denetleyici – Boş** girişini seçin.
3.  **Ekle**’yi seçin.
4.  **ClaimsController** olarak adlandırın.
5.  Denetleyici sınıfınızın kodunu aşağıdaki kodla değiştirin. Bu `[Authorize]` özniteliği sınıfına ekler:

    ```csharp
    [Authorize]
    public class ClaimsController : Controller
    {
        /// <summary>
        /// Add user's claims to viewbag
        /// </summary>
        /// <returns></returns>
        public ActionResult Index()
        {
            var userClaims = User.Identity as System.Security.Claims.ClaimsIdentity;

            //You get the user's first and last name below:
            ViewBag.Name = userClaims?.FindFirst("name")?.Value;

            // The 'preferred_username' claim can be used for showing the username
            ViewBag.Username = userClaims?.FindFirst("preferred_username")?.Value;

            // The subject/ NameIdentifier claim can be used to uniquely identify the user across the web
            ViewBag.Subject = userClaims?.FindFirst(System.Security.Claims.ClaimTypes.NameIdentifier)?.Value;

            // TenantId is the unique Tenant Id - which represents an organization in Azure AD
            ViewBag.TenantId = userClaims?.FindFirst("http://schemas.microsoft.com/identity/claims/tenantid")?.Value;

            return View();
        }
    }
    ```

### <a name="more-information"></a>Daha fazla bilgi
Özniteliğin kullanımı nedeniyle `[Authorize]` , bu denetleyicideki tüm yöntemler yalnızca kullanıcının kimliği doğrulandıysa çalıştırılabilir. Kullanıcı kimliği doğrulanmazsa ve denetleyiciye erişmeye çalışırsa, OWıN bir kimlik doğrulama sınaması başlatır ve kullanıcıyı kimlik doğrulamaya zorlar. Yukarıdaki kod, kullanıcının KIMLIK belirtecine dahil edilen belirli kullanıcı özniteliklerinin talepler listesine bakar. Bu öznitelikler, kullanıcının tam adı ve Kullanıcı adının yanı sıra genel kullanıcı tanımlayıcısı konusunu içerir. Ayrıca, kullanıcının kuruluşunun KIMLIĞINI temsil eden *KIRACı kimliğini* de içerir.

## <a name="create-a-view-to-display-the-users-claims"></a>Kullanıcının taleplerini görüntülemek için bir görünüm oluşturun

Visual Studio'da kullanıcının taleplerini bir web sayfasında görüntülemek için yeni bir görünüm oluşturun:

1.  **Views\claim** klasörüne sağ tıklayın ve ardından **Görünüm Ekle**' yi seçin.
2.  Yeni Görünüm **dizinini** adlandırın.
3.  Aşağıdaki HTML kodunu dosyaya ekleyin:

    ```html
    <html>
    <head>
        <meta name="viewport" content="width=device-width" />
        <title>Sign in with Microsoft Sample</title>
        <link href="@Url.Content("~/Content/bootstrap.min.css")" rel="stylesheet" type="text/css" />
    </head>
    <body style="padding:50px">
        <h3>Main Claims:</h3>
        <table class="table table-striped table-bordered table-hover">
            <tr><td>Name</td><td>@ViewBag.Name</td></tr>
            <tr><td>Username</td><td>@ViewBag.Username</td></tr>
            <tr><td>Subject</td><td>@ViewBag.Subject</td></tr>
            <tr><td>TenantId</td><td>@ViewBag.TenantId</td></tr>
        </table>
        <br />
        <h3>All Claims:</h3>
        <table class="table table-striped table-bordered table-hover table-condensed">
        @foreach (var claim in System.Security.Claims.ClaimsPrincipal.Current.Claims)
        {
            <tr><td>@claim.Type</td><td>@claim.Value</td></tr>
        }
        </table>
        <br />
        <br />
        @Html.ActionLink("Sign out", "SignOut", "Home", null, new { @class = "btn btn-primary" })
    </body>
    </html>
    ```

## <a name="register-your-application"></a>Uygulamanızı kaydetme

Uygulamanızı kaydetmek ve uygulama kayıt bilgilerinizi çözümünüze eklemek için iki seçeneğiniz vardır:

### <a name="option-1-express-mode"></a>Seçenek 1: hızlı mod

Uygulamanızı hızlı bir şekilde kaydetmek için şu adımları izleyin:

1. Yeni  [Azure Portal-uygulama kayıtları](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/applicationsListBlade/quickStartType/AspNetWebAppQuickstartPage/sourceType/docs) bölmesine gidin.
1. Uygulamanız için bir ad girin ve **Kaydet**'i seçin.
1. Yeni uygulamanızı tek bir tıklama ile indirip otomatik olarak yapılandırmak için yönergeleri izleyin.

### <a name="option-2-advanced-mode"></a>Seçenek 2: Gelişmiş mod

Uygulamanızı kaydetmek ve uygulama kayıt bilgilerinizi çözümünüze el ile eklemek için şu adımları izleyin:

1. Visual Studio 'yu açın ve ardından:
   1. Çözüm Gezgini, projeyi seçin ve Özellikler penceresi görüntüleyin (Özellikler penceresi görmüyorsanız F4 tuşuna basın).
   1. SSL etkin olarak değiştirin `True` .
   1. Visual Studio 'da projeye sağ tıklayın, **Özellikler**' i seçin ve ardından **Web** sekmesini seçin. **Sunucular** bölümünde, **proje URL** ayarını **SSL URL 'si** olarak değiştirin.
   1. SSL URL 'sini kopyalayın. Bu URL 'YI, sonraki adımda, kayıt portalının yeniden yönlendirme URL 'leri listesinde bulunan yeniden yönlendirme URL 'Leri listesine ekleyeceksiniz.<br/><br/>![Proje özellikleri](media/active-directory-develop-guidedsetup-aspnetwebapp-configure/vsprojectproperties.png)<br />
1. Bir iş veya okul hesabı kullanarak veya kişisel bir Microsoft hesabı kullanarak [Azure Portal](https://portal.azure.com) oturum açın.
1. Hesabınız birden fazla kiracıya erişim veriyorsa, sağ üst köşede hesabınızı seçin ve Portal oturumunuzu istediğiniz Azure AD kiracısına ayarlayın.
1. Geliştiriciler için Microsoft Identity platformu [uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) sayfasına gidin.
1. **Yeni kayıt** seçeneğini belirleyin.
1. **Bir uygulamayı kaydet** sayfası göründüğünde, uygulamanızın kayıt bilgilerini girin:
   1. **Ad** bölümünde, uygulama kullanıcılarına, **ASPNET-öğreticisi** gibi görüntülenecek anlamlı bir uygulama adı girin.
   1. Adım 1 ' de (örneğin,), yanıt URL 'sindeki Visual Studio 'dan kopyaladığınız SSL URL 'sini ekleyin `https://localhost:44368/` ve **Kaydet**' i seçin. **Reply URL**
1. **Kimlik doğrulama** menüsünü seçin, **örtük izin** altında **Kimlik belirteçleri** ' ni seçin ve ardından **Kaydet**' i seçin.
1. Aşağıdaki, bölümündeki kök klasörde bulunan web.config dosyasına aşağıdakileri ekleyin `configuration\appSettings` :

    ```xml
    <add key="ClientId" value="Enter_the_Application_Id_here" />
    <add key="redirectUri" value="Enter_the_Redirect_URL_here" />
    <add key="Tenant" value="common" />
    <add key="Authority" value="https://login.microsoftonline.com/{0}/v2.0" />
    ```

1. `ClientId`Yeni kaydettiğiniz uygulama kimliğiyle değiştirin.
1. `redirectUri`PROJENIZIN SSL URL 'siyle değiştirin.

## <a name="test-your-code"></a>Kodunuza test etme

Visual Studio 'da uygulamanızı test etmek için F5 'e basarak projenizi çalıştırın. Tarayıcı, http:// <span></span> localhost: {Port} konumunda açılır ve **Microsoft hesabıyla oturum açın** düğmesini görürsünüz. Oturum açma işlemini başlatmak için düğmeyi seçin.

Testinizi çalıştırmaya hazırsanız bir Azure AD hesabı (iş veya okul hesabı) veya kişisel Microsoft hesabı (canlı) kullanın<span>.</span> com veya <span>Outlook.</span> com) oturum açın.

![Tarayıcıda tarayıcı oturum açma sayfasında gösterilen Microsoft hesabıyla oturum açın düğmesi](media/active-directory-develop-guidedsetup-aspnetwebapp-test/aspnetbrowsersignin.png)
<br/><br/>
![Microsoft hesabı oturum açın](media/active-directory-develop-guidedsetup-aspnetwebapp-test/aspnetbrowsersignin2.png)

#### <a name="permissions-and-consent-in-the-microsoft-identity-platform-endpoint"></a>Microsoft kimlik platformu uç noktasında izinler ve onay

Microsoft Identity platformu ile tümleştirilen uygulamalar, kullanıcılara ve yöneticilere verilere nasıl erişilebileceği üzerinde denetim sağlayan bir yetkilendirme modeli izler. Bir Kullanıcı bu uygulamaya erişmek için Microsoft Identity platform ile kimlik doğrulamasından geçtikten sonra, uygulama tarafından istenen izinleri onaylaması istenir ("temel profilinizi görüntüleyin" ve "erişim vermiş olduğunuz verilere erişimi korur"). Bu izinleri kabul ettikten sonra, Kullanıcı uygulama sonuçlarına devam edecektir. Bununla birlikte, aşağıdakilerden biri gerçekleştiğinde kullanıcıya **Yönetici onay sayfası gereksinimi** istenebilir:

- Uygulama geliştiricisi, **yönetici onayı** gerektiren herhangi bir ek izin ekler.
- Ya da kiracı, kullanıcıların kendi adına şirket verilerine erişen uygulamalara izin veremediği ( **Kurumsal uygulamalarda > Kullanıcı ayarları**) yapılandırılır.

Daha fazla bilgi için [Microsoft Identity platform uç noktasındaki izinler ve onay](./v2-permissions-and-consent.md)bölümüne bakın.

### <a name="view-application-results"></a>Uygulama sonuçlarını görüntüle

Oturum açtıktan sonra, Kullanıcı Web sitenizin giriş sayfasına yönlendirilir. Giriş sayfası, Microsoft uygulama kayıt portalındaki uygulama kayıt bilgilerinizdeki belirtilen HTTPS URL 'sidir. Giriş sayfası bir *"Merhaba \<user> "* hoş geldiniz iletisi, oturumu kapatma bağlantısı ve kullanıcının taleplerini görüntülemek için bir bağlantı içerir. Kullanıcı taleplerinin bağlantısı, daha önce oluşturduğunuz talep denetleyicisine bağlanır.

### <a name="view-the-users-claims"></a>Kullanıcının taleplerini görüntüleme

Kullanıcının taleplerini görüntülemek için, yalnızca kimliği doğrulanmış kullanıcılar için kullanılabilen denetleyici görünümüne gitmek için bağlantıyı seçin.

#### <a name="view-the-claims-results"></a>Talep sonuçlarını görüntüleme

Denetleyici görünümüne gözatdıktan sonra, kullanıcının temel özelliklerini içeren bir tablo görmeniz gerekir:

|Özellik |Değer |Açıklama |
|---|---|---|
|**Ad** |Kullanıcının tam adı | Kullanıcının adı ve soyadı
|**Kullanıcı adı** |kullanıcısını<span>@domain.com</span> | Kullanıcıyı tanımlamak için kullanılan Kullanıcı adı|
|**Konu** |Konu |Kullanıcıyı web genelinde benzersiz şekilde tanımlayan bir dize|
|**Kiracı Kimliği** |Guid | Kullanıcının Azure AD organizasyonunu benzersiz bir şekilde temsil eden bir **GUID**|

Ayrıca, kimlik doğrulama isteğinde olan tüm taleplerin bir tablosunu görmeniz gerekir. Daha fazla bilgi için, [BIR kimlik belirtecinde olan taleplerin listesine](./id-tokens.md)bakın.

### <a name="test-access-to-a-method-that-has-an-authorize-attribute-optional"></a>Yetkilendir özniteliğine sahip bir yönteme erişimi test etme (isteğe bağlı)

Özniteliği tarafından korunan bir denetleyicide anonim kullanıcı olarak erişimi test etmek için `Authorize` şu adımları izleyin:

1. Kullanıcının oturumunu kapatmak için bağlantıyı seçin ve oturum kapatma sürecini doldurun.
2. Tarayıcınızda, <span></span> özniteliğiyle korunan denetleyicinize erişmek için http://localhost: {Port} veya talepler yazın `Authorize` .

#### <a name="expected-results-after-access-to-a-protected-controller"></a>Korumalı denetleyiciye erişimi sonrasında beklenen sonuçlar

Korumalı denetleyici görünümünü kullanmak için kimlik doğrulamanız istenir.

## <a name="advanced-options"></a>Gelişmiş seçenekler

### <a name="protect-your-entire-website"></a>Web sitenizin tamamını koruyun

Web sitenizin tamamını korumak için, **Global. asax** dosyasında, `AuthorizeAttribute` `GlobalFilters` yöntemi içindeki filtreye özniteliğini ekleyin `Application_Start` :

```csharp
GlobalFilters.Filters.Add(new AuthorizeAttribute());
```

### <a name="restrict-who-can-sign-in-to-your-application"></a>Uygulamanızda oturum açabilen kişileri kısıtla

Varsayılan olarak, bu kılavuz tarafından oluşturulan uygulamayı yapılandırdığınızda, uygulamanız kişisel hesapların (outlook.com, live.com ve diğerleri dahil) oturum açma işlemlerini kabul eder ve Microsoft Identity platformu ile tümleştirilen herhangi bir şirketten veya kuruluştan iş ve okul hesapları kabul eder. Bu, SaaS uygulamaları için önerilen bir seçenektir.

Uygulamanıza yönelik kullanıcı oturum açma erişimini kısıtlamak için birden çok seçenek mevcuttur.

#### <a name="option-1-restrict-users-from-only-one-organizations-active-directory-instance-to-sign-in-to-your-application-single-tenant"></a>Seçenek 1: kullanıcıların uygulamanızda oturum açmasını sağlamak için yalnızca bir kuruluşun Active Directory örneğinden kullanıcıları kısıtlayın (tek kiracılı)

Bu seçenek *LOB uygulamaları* için sık sık kullanılır: uygulamanızın yalnızca belirli BIR Azure AD örneğine ait olan hesaplardan (Bu örneğin *Konuk hesapları* dahil) oturum açma işlemlerini kabul etmesini istiyorsanız, şu adımları izleyin:

1. web.config dosyasında, parametresi için değerini `Tenant` `Common` , kuruluşun kiracı adıyla değiştirin (örneğin,) `contoso.onmicrosoft.com` .
2. [Owın başlangıç sınıfınız](#configure-the-authentication-pipeline)içinde, `ValidateIssuer` bağımsız değişkenini olarak ayarlayın `true` .

#### <a name="option-2-restrict-access-to-users-in-a-specific-list-of-organizations"></a>Seçenek 2: belirli bir kuruluş listesindeki kullanıcılara erişimi kısıtlama

Oturum açma erişimini yalnızca, izin verilen kuruluşlar listesindeki bir Azure AD kuruluşunda bulunan Kullanıcı hesaplarıyla kısıtlayabilirsiniz:
1. [Owın başlangıç sınıfınız](#configure-the-authentication-pipeline)içinde, `ValidateIssuer` bağımsız değişkenini olarak ayarlayın `true` .
2. `ValidIssuers`Parametresinin değerini izin verilen kuruluşlar listesine ayarlayın.

#### <a name="option-3-use-a-custom-method-to-validate-issuers"></a>Seçenek 3: verenler doğrulamak için özel bir yöntem kullanma

**Issuervalidator** parametresini kullanarak verenler doğrulamak için özel bir yöntem uygulayabilirsiniz. Bu parametrenin nasıl kullanılacağı hakkında daha fazla bilgi için bkz. [Tokenvalidationparameters](/dotnet/api/microsoft.identitymodel.tokens.tokenvalidationparameters) sınıfı.

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

Microsoft Identity platformu ile Web Apps 'ten korumalı Web API 'Lerini çağırma hakkında bilgi edinin:

> [!div class="nextstepaction"]
> [Web API 'Lerini çağıran Web uygulamaları](scenario-web-app-sign-user-overview.md)
