---
title: include dosyası
description: include dosyası
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/18/2018
ms.author: andret
ms.custom: include file
ms.openlocfilehash: d4ba15e4ad46044c04c242c8805af9f320e95150
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2018
ms.locfileid: "46368467"
---
## <a name="use-msal-to-get-a-token-for-the-microsoft-graph-api"></a>Microsoft Graph API'si için bir belirteç almak için MSAL kullanın

Bu bölümde, Microsoft Graph API'si için bir belirteç almak için MSAL kullanın.

1.  İçinde *MainWindow.xaml.cs* başvuru için MSAL sınıfı ekleyin:

    ```csharp
    using Microsoft.Identity.Client;
    ```

2. Değiştirin `MainWindow` kodu şu kodla sınıfı:

    ```csharp
    public partial class MainWindow : Window
    {
        //Set the API Endpoint to Graph 'me' endpoint
        string _graphAPIEndpoint = "https://graph.microsoft.com/v1.0/me";

        //Set the scope for API call to user.read
        string[] _scopes = new string[] { "user.read" };

        public MainWindow()
        {
            InitializeComponent();
        }

        /// <summary>
        /// Call AcquireTokenAsync - to acquire a token requiring user to sign-in
        /// </summary>
        private async void CallGraphButton_Click(object sender, RoutedEventArgs e)
        {
            AuthenticationResult authResult = null;

            var app = App.PublicClientApp;
            ResultText.Text = string.Empty;
            TokenInfoText.Text = string.Empty;

            var accounts = await app.GetAccountsAsync();

            try
            {
                authResult = await app.AcquireTokenSilentAsync(_scopes, accounts.FirstOrDefault());
            }
            catch (MsalUiRequiredException ex)
            {
                // A MsalUiRequiredException happened on AcquireTokenSilentAsync. This indicates you need to call AcquireTokenAsync to acquire a token
                System.Diagnostics.Debug.WriteLine($"MsalUiRequiredException: {ex.Message}");

                try
                {
                    authResult = await App.PublicClientApp.AcquireTokenAsync(_scopes);
                }
                catch (MsalException msalex)
                {
                    ResultText.Text = $"Error Acquiring Token:{System.Environment.NewLine}{msalex}";
                }
            }
            catch (Exception ex)
            {
                ResultText.Text = $"Error Acquiring Token Silently:{System.Environment.NewLine}{ex}";
                return;
            }

            if (authResult != null)
            {
                ResultText.Text = await GetHttpContentWithToken(_graphAPIEndpoint, authResult.AccessToken);
                DisplayBasicTokenInfo(authResult);
                this.SignOutButton.Visibility = Visibility.Visible;
            }
        }
    }
    ```

<!--start-collapse-->
### <a name="more-information"></a>Daha fazla bilgi

#### <a name="get-a-user-token-interactively"></a>Etkileşimli bir kullanıcı belirteci alma

Çağırma `AcquireTokenAsync` oturum açmak için kullanıcıların ister bir pencere yöntemi sonuçlanıyor. Uygulamalar genellikle etkileşimli olarak korunan bir kaynağa erişmek için ihtiyaç duydukları ilk kez oturum açmalarını gerektirir. Bunlar (örneğin, bir kullanıcının parolasını süresi dolduğunda), bir belirteç almak için sessiz bir işlemi başarısız olduğunda oturum açmanız gerekebilir.

#### <a name="get-a-user-token-silently"></a>Bir kullanıcı sessizce belirteci alma

`AcquireTokenSilentAsync` Yöntemi, belirteç edinme ve herhangi bir kullanıcı etkileşimi olmadan yenilemeler işler. Sonra `AcquireTokenAsync` ilk kez yürütülür `AcquireTokenSilentAsync` veya belirteçleri yenileme isteği için çağrıları sessizce yapılan sonraki çağrılar için korunan kaynaklara erişim belirteçleri elde etmek için kullanılacak normal yöntemidir.

Sonuç olarak, `AcquireTokenSilentAsync` yöntemi başarısız olur. Hatanın nedenlerini kullanıcı ya da oturumunuz veya başka bir cihazda kendi parola değiştirildi emin olabilir. MSAL etkileşimli bir eylem gerektirerek sorun çözülebilir, harekete algıladığında bir `MsalUiRequiredException` özel durum. Uygulamanız, bu özel durumun iki şekilde işleyebilir:

* Bir çağrısı yapabilirsiniz `AcquireTokenAsync` hemen. Bu çağrı, kullanıcının oturum açmasını isteyen içinde sonuçlanır. Bu düzen, genellikle çevrimiçi uygulamalarda kullanılır kullanıcı için çevrimdışı kullanılabilir içerik olduğunda. Bu Kılavuzlu kurulum tarafından oluşturulan örnek örnek yürütme eylemi ilk sürede görebileceğiniz gibi bu desenini izler. 

* Hiçbir kullanıcı uygulama kullanıldığından `PublicClientApp.Users.FirstOrDefault()` bir null değer içeriyor ve bir `MsalUiRequiredException` özel durumu oluşturulur. 

* Ardından kod çağırarak özel durumu işleyen `AcquireTokenAsync`, kullanıcının oturum açmasını isteyen içinde sonuçlanır.

* Oturum açmak için doğru zamanda seçebilmeniz için görsel gösterimi bunun yerine bir etkileşimli oturum açma gerekli olan kullanıcılara sunabilirsiniz. Veya uygulama yeniden deneyebilirsiniz `AcquireTokenSilentAsync` daha sonra. Bu düzen, çevrimdışı içerik uygulamada mevcut olduğunda kullanıcıların diğer uygulama işlevleri kesintiye uğratmadan--Örneğin, ne zaman kullanabileceğini sık kullanılır. Korumalı kaynağa ya da eski bilgileri Yenile oturum açmaya istediğinizde, bu durumda, kullanıcılar karar verebilirsiniz. Alternatif olarak, uygulama denemeye karar verebilirsiniz `AcquireTokenSilentAsync` ağ zaman geri geçici olarak devre dışı olan sonra.
<!--end-collapse-->

## <a name="call-the-microsoft-graph-api-by-using-the-token-you-just-obtained"></a>Yalnızca edinilen belirteçle'ı kullanarak Microsoft Graph API çağırma

Aşağıdaki yeni yöntemi ekleyin, `MainWindow.xaml.cs`. Yöntem yapmak için kullanılan bir `GET` Authorize üstbilgi kullanarak Graph API'si isteği:

```csharp
/// <summary>
/// Perform an HTTP GET request to a URL using an HTTP Authorization header
/// </summary>
/// <param name="url">The URL</param>
/// <param name="token">The token</param>
/// <returns>String containing the results of the GET operation</returns>
public async Task<string> GetHttpContentWithToken(string url, string token)
{
    var httpClient = new System.Net.Http.HttpClient();
    System.Net.Http.HttpResponseMessage response;
    try
    {
        var request = new System.Net.Http.HttpRequestMessage(System.Net.Http.HttpMethod.Get, url);
        //Add the token in Authorization header
        request.Headers.Authorization = new System.Net.Http.Headers.AuthenticationHeaderValue("Bearer", token);
        response = await httpClient.SendAsync(request);
        var content = await response.Content.ReadAsStringAsync();
        return content;
    }
    catch (Exception ex)
    {
        return ex.ToString();
    }
}
```

<!--start-collapse-->
### <a name="more-information-about-making-a-rest-call-against-a-protected-api"></a>Karşı korumalı bir API REST çağrısı yapma hakkında daha fazla bilgi

Bu örnek uygulamasında kullandığınız `GetHttpContentWithToken` yöntemi bir HTTP yapmak `GET` istemek için bir belirteç gerekiyor korunan bir kaynağa karşı ve ardından içeriği çağırana döndürmesi. Bu yöntem, HTTP yetkilendirme üst bilgisinde alınan belirteç ekler. Bu örnek için Microsoft Graph API kaynaktır *bana* uç noktası için kullanıcı profili bilgilerini görüntüler.
<!--end-collapse-->

## <a name="add-a-method-to-sign-out-a-user"></a>Bir kullanıcının oturumunu kapatmaz için yöntem ekleme

Bir kullanıcının oturumunu kapatmaz, aşağıdaki yöntemi ekleyin, `MainWindow.xaml.cs` dosyası:

```csharp
/// <summary>
/// Sign out the current user
/// </summary>
private async void SignOutButton_Click(object sender, RoutedEventArgs e)
{
    var accounts = await App.PublicClientApp.GetAccountsAsync(); 

    if (accounts.Any())
    {
        try
        {
            await App.PublicClientApp.RemoveAsync(accounts.FirstOrDefault()); 
            this.ResultText.Text = "User has signed-out";
            this.CallGraphButton.Visibility = Visibility.Visible;
            this.SignOutButton.Visibility = Visibility.Collapsed;
        }
        catch (MsalException ex)
        {
            ResultText.Text = $"Error signing-out user: {ex.Message}";
        }
    }
}
```

<!--start-collapse-->
### <a name="more-information-about-user-sign-out"></a>Kullanıcı oturum kapatma hakkında daha fazla bilgi

`SignOutButton_Click` Yöntemi kullanıcılar yalnızca etkileşimli olarak yapılırsa, bir belirteç almak için gelecekteki bir istek başarılı olur böylece geçerli kullanıcının unutmak çok MSAL etkili bir şekilde söyler MSAL kullanıcı önbellekten kaldırır.

Bu örnek uygulamasında tek kullanıcılı desteklese de MSAL burada birden çok hesap aynı zamanda oturum senaryolarını destekler. Örnek bir kullanıcı birden çok hesaba sahip olduğu bir e-posta uygulamasıdır.
<!--end-collapse-->

## <a name="display-basic-token-information"></a>Temel belirteç bilgilerini görüntüle

Belirteç ile ilgili temel bilgileri görüntülemek için aşağıdaki yöntemi ekleyin, *MainWindow.xaml.cs* dosyası:

```csharp
/// <summary>
/// Display basic information contained in the token
/// </summary>
private void DisplayBasicTokenInfo(AuthenticationResult authResult)
{
    TokenInfoText.Text = "";
    if (authResult != null)
    {
        TokenInfoText.Text += $"Username: {authResult.Account.Username}" + Environment.NewLine;
        TokenInfoText.Text += $"Token Expires: {authResult.ExpiresOn.ToLocalTime()}" + Environment.NewLine;
        TokenInfoText.Text += $"Access Token: {authResult.AccessToken}" + Environment.NewLine;
    }
}
```

<!--start-collapse-->
### <a name="more-information"></a>Daha fazla bilgi

Kullanıcı oturum açtığında sonra Microsoft Graph API'sini çağırmak için kullanılan erişim belirteci yanı sıra MSAL, ayrıca bir kimlik belirteci alır. Bu belirteç, küçük bir alt kümesini kullanıcılara ilgili bilgileri içerir. `DisplayBasicTokenInfo` Yöntemi belirtecinde yer alan temel bilgileri görüntüler. Örneğin, kullanıcının görünen adı ve kimliği yanı sıra belirteç sona erme tarihini ve erişim belirteci temsil eden dize görüntüler. Seçebileceğiniz *Microsoft Graph API çağrısı* düğmesine birden çok kez ve sonraki istekleri için aynı belirteci yeniden olduğunu görebilirsiniz. MSAL çalışmanın kapılarını açtığında Genişletilmekte olan belirteci yenileme süresi sona erme tarihi de görebilirsiniz.
<!--end-collapse-->

