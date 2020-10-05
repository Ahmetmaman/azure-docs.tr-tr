---
title: "Hızlı başlangıç: Python Daemon 'dan Microsoft Graph çağırma | Mavisi"
titleSuffix: Microsoft identity platform
description: Bu hızlı başlangıçta, bir Python işleminin bir erişim belirtecini nasıl alabileceğinizi ve uygulamanın kendi kimliğini kullanarak Microsoft Identity platform uç noktası tarafından korunan bir API 'YI nasıl çağırabileceğinizi öğrenirsiniz.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: quickstart
ms.workload: identity
ms.date: 10/22/2019
ms.author: jmprieur
ms.custom: aaddev, identityplatformtop40, devx-track-python, scenarios:getting-started, languages:Python
ms.openlocfilehash: 2c280b8241819155f32942a399caa7f916db3827
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91257750"
---
# <a name="quickstart-acquire-a-token-and-call-microsoft-graph-api-from-a-python-console-app-using-apps-identity"></a>Hızlı başlangıç: uygulama kimliğini kullanarak bir Python konsol uygulamasından belirteç alma ve Microsoft Graph API çağırma

Bu hızlı başlangıçta, uygulamanın kimliğini kullanarak bir erişim belirteci alan bir Python uygulaması yazın ve ardından dizindeki [kullanıcıların listesini](/graph/api/user-list) göstermek IÇIN Microsoft Graph API 'sini çağırır. Bu senaryo, kullanıcının kimliği yerine gözetimsiz, katılımsız iş veya Windows hizmeti 'nin bir uygulama kimliğiyle çalışması gereken durumlar için yararlıdır.

> [!div renderon="docs"]
> ![Bu hızlı başlangıç tarafından oluşturulan örnek uygulamanın nasıl çalıştığını gösterir](media/quickstart-v2-netcore-daemon/netcore-daemon-intro.svg)

## <a name="prerequisites"></a>Önkoşullar

Bu örneği çalıştırmak için şunlar gerekir:

- [Python 2.7 +](https://www.python.org/downloads/release/python-2713) veya [Python 3 +](https://www.python.org/downloads/release/python-364/)
- [MSAL Python](https://github.com/AzureAD/microsoft-authentication-library-for-python)

> [!div renderon="docs"]
> ## <a name="register-and-download-your-quickstart-app"></a>Hızlı başlangıç uygulamanızı kaydetme ve indirme

> [!div renderon="docs" class="sxs-lookup"]
>
> Hızlı başlangıç uygulamanızı başlatmak için iki seçeneğiniz vardır: Express (aşağıdaki 1. seçenek) ve manuel (seçenek 2)
>
> ### <a name="option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample"></a>1. Seçenek: Uygulamanızı otomatik olarak kaydedip yapılandırın ve ardından kod örneğinizi indirin
>
> 1. Yeni [Azure Portal-uygulama kayıtları](https://portal.azure.com/?Microsoft_AAD_RegisteredApps=true#blade/Microsoft_AAD_RegisteredApps/applicationsListBlade/quickStartType/PythonDaemonQuickstartPage/sourceType/docs) bölmesine gidin.
> 1. Uygulamanız için bir ad girin ve **Kaydet**'i seçin.
> 1. Yönergeleri izleyerek yeni uygulamanızı yalnızca tek tıklamayla indirin ve otomatik olarak yapılandırın.
>
> ### <a name="option-2-register-and-manually-configure-your-application-and-code-sample"></a>2. Seçenek: Uygulamanızı ve kod örneğinizi el ile kaydetme ve yapılandırma

> [!div renderon="docs"]
> #### <a name="step-1-register-your-application"></a>1. Adım: Uygulamanızı kaydetme
> Uygulamanızı kaydetmek ve uygulama kayıt bilgilerinizi çözümünüze el ile eklemek için şu adımları izleyin:
>
> 1. [Azure Portal](https://portal.azure.com) iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
> 1. Hesabınız size birden fazla Azure AD kiracısına erişim sunuyorsa sağ üst köşeden hesabınızı seçin ve portal oturumunuzu istediğiniz Azure AD kiracısına ayarlayın.
> 1. Geliştiriciler için Microsoft Identity platformu [uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) sayfasına gidin.
> 1. **Yeni kayıt**seçeneğini belirleyin.
> 1. **Bir uygulamayı kaydet** sayfası göründüğünde, uygulamanızın kayıt bilgilerini girin.
> 1. **Ad** bölümünde, uygulamanın kullanıcılarına gösterilecek anlamlı bir uygulama adı girin, örneğin `Daemon-console` , uygulamayı oluşturmak için **Kaydet** ' i seçin.
> 1. Kaydolduktan sonra **sertifikalar & gizlilikler** menüsünü seçin.
> 1. **İstemci gizli**dizileri altında **+ yeni istemci parolası**' nı seçin. Bir ad verin ve **Ekle**' yi seçin. Parolayı güvenli bir konuma kopyalayın. Kodunuzda kullanmak için ihtiyacınız olacak.
> 1. Şimdi **API izinleri** menüsünü seçin **+ izin Ekle** düğmesini seçin, **Microsoft Graph**' yi seçin.
> 1. **Uygulama izinleri**' ni seçin.
> 1. **Kullanıcı** düğümü altında **User. Read. All**' ı seçin ve ardından **izin Ekle** ' yi seçin.

> [!div class="sxs-lookup" renderon="portal"]
> ### <a name="download-and-configure-your-quickstart-app"></a>Hızlı başlangıç uygulamanızı indirin ve yapılandırın
>
> #### <a name="step-1-configure-your-application-in-azure-portal"></a>1. Adım: Uygulamanızı Azure portalında yapılandırma
> Bu hızlı başlangıçta çalışması için kod örneği için, bir istemci parolası oluşturmanız ve Graph API **Kullanıcı. Read. All** uygulama iznini eklemeniz gerekir.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Bu değişiklikleri benim için yap]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Zaten yapılandırılmış](media/quickstart-v2-netcore-daemon/green-check.png) Uygulamanız bu özniteliklerle yapılandırılmış.

#### <a name="step-2-download-your-python-project"></a>2. Adım: Python projenizi Indirme

> [!div renderon="docs"]
> [Python Daemon projesini indirin](https://github.com/Azure-Samples/ms-identity-python-daemon/archive/master.zip)

> [!div renderon="portal" id="autoupdate" class="nextstepaction"]
> [Kod örneğini indirin](https://github.com/Azure-Samples/ms-identity-python-daemon/archive/master.zip)

> [!div class="sxs-lookup" renderon="portal"]
> > [!NOTE]
> > `Enter_the_Supported_Account_Info_Here`


> [!div renderon="docs"]
> #### <a name="step-3-configure-your-python-project"></a>3. Adım: Python projenizi yapılandırma
>
> 1. Zip dosyasını diskin köküne yakın bir yerel klasöre (örneğin **C:\Azure-Samples**) ayıklayın.
> 1. **1-Call-MsGraph-WithSecret "** adlı alt klasöre gidin.
> 1. **Üzerindeparameters.js** düzenleyin ve alanları `authority` , `client_id` ve değerlerini `secret` Aşağıdaki kod parçacığıyla değiştirin:
>
>    ```json
>    "authority": "https://login.microsoftonline.com/Enter_the_Tenant_Id_Here",
>    "client_id": "Enter_the_Application_Id_Here",
>    "secret": "Enter_the_Client_Secret_Here"
>    ```
>    Burada:
>    - `Enter_the_Application_Id_Here` - kaydettiğiniz uygulamanın **Uygulama (istemci) Kimliği** değeridir.
>    - `Enter_the_Tenant_Id_Here` -Bu değeri **Kiracı kimliği** veya **kiracı adıyla** değiştirin (örneğin, contoso.Microsoft.com)
>    - `Enter_the_Client_Secret_Here` -Bu değeri, 1. adımda oluşturulan istemci gizli anahtarı ile değiştirin.
>
> > [!TIP]
> > **Uygulama (istemci) kimliği**, **Dizin (kiracı) kimliği**değerlerini bulmak Için Azure Portal uygulamanın **genel bakış** sayfasına gidin. Yeni bir anahtar oluşturmak için **sertifikalar & gizlilikler** sayfasına gidin.

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-3-admin-consent"></a>3. Adım: yönetici onayı

> [!div renderon="docs"]
> #### <a name="step-4-admin-consent"></a>4. Adım: yönetici onayı

Bu noktada uygulamayı çalıştırmayı denerseniz, *HTTP 403-Yasak* hatası: ' ı alırsınız `Insufficient privileges to complete the operation` . Bu hata, *yalnızca uygulama Izni* yönetici onayı gerektirdiğinden oluşur: dizininizin genel Yöneticisi uygulamanıza onay vermelidir. Rolünüze bağlı olarak aşağıdaki seçeneklerden birini belirleyin:

##### <a name="global-tenant-administrator"></a>Genel Kiracı Yöneticisi

> [!div renderon="docs"]
> Küresel kiracı yöneticisiyseniz, Azure portalının uygulama kaydı 'nda (Önizleme) **API izinleri** sayfasına gidin ve **{Tenant Name} Için yönetici onayı ver** ' i seçin (burada {Tenant Name} dizininizin adıdır).

> [!div renderon="portal" class="sxs-lookup"]
> Genel yöneticiyseniz, **API izinleri** sayfasına gidin **Enter_the_Tenant_Name_Here Için yönetici onayı ver** ' i seçin
> > [!div id="apipermissionspage"]
> > [API Izinleri sayfasına gidin]()

##### <a name="standard-user"></a>Standart Kullanıcı

Kiracınızın standart bir kullanıcısı kullanıyorsanız, uygulamanız için yönetici onayı vermesini istemek üzere bir genel yönetici yapmanız gerekir. Bunu yapmak için, yöneticinize aşağıdaki URL 'YI verin:

```url
https://login.microsoftonline.com/Enter_the_Tenant_Id_Here/adminconsent?client_id=Enter_the_Application_Id_Here
```

> [!div renderon="docs"]
>> Burada:
>> * `Enter_the_Tenant_Id_Here` -Bu değeri **Kiracı kimliği** veya **kiracı adıyla** değiştirin (örneğin, contoso.Microsoft.com)
>> * `Enter_the_Application_Id_Here` - kaydettiğiniz uygulamanın **Uygulama (istemci) Kimliği** değeridir.

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-4-run-the-application"></a>4. Adım: uygulamayı çalıştırma

> [!div renderon="docs"]
> #### <a name="step-5-run-the-application"></a>5. Adım: uygulamayı çalıştırma

Bu örneğin bağımlılıklarını bir kez yüklemeniz gerekir

```console
pip install -r requirements.txt
```

Ardından, komut istemi veya konsolu aracılığıyla uygulamayı çalıştırın:

```console
python confidential_client_secret_sample.py parameters.json
```

Konsol çıkışında Azure AD dizininizde bulunan kullanıcıların listesini temsil eden bazı JSON parçaları ' nı görmeniz gerekir.

> [!IMPORTANT]
> Bu hızlı başlangıç uygulaması, kendisini gizli istemci olarak tanımlamak için bir istemci gizli anahtarı kullanır. İstemci parolası proje dosyalarınıza düz metin olarak eklendiğinden, güvenlik nedenleriyle, uygulamayı üretim uygulaması olarak düşünmeden önce istemci parolası yerine bir sertifika kullanmanız önerilir. Sertifika kullanma hakkında daha fazla bilgi için, bu örnek için aynı GitHub deposunda yer alan [yönergelere](https://github.com/Azure-Samples/ms-identity-python-daemon/blob/master/2-Call-MsGraph-WithCertificate/README.md) bakın, ancak ikinci klasör **2-Call-MSGraph-withcertificate**

## <a name="more-information"></a>Daha fazla bilgi

### <a name="msal-python"></a>MSAL Python

[Msal Python](https://github.com/AzureAD/microsoft-authentication-library-for-python) , kullanıcıların oturum açması ve Microsoft Identity platformu tarafından korunan bir API 'ye erişmek için kullanılan belirteçleri istemek için kullanılan bir kitaplıktır. Bu hızlı başlangıç, açıklandığı şekilde, uygulamanın temsilci izinleri yerine kendi kimliğini kullanarak belirteçleri ister. Bu örnekte kullanılan kimlik doğrulama akışı, *[istemci kimlik bilgileri OAuth akışı](v2-oauth2-client-creds-grant-flow.md)* olarak bilinir. MSAL Python 'u Daemon uygulamalarıyla kullanma hakkında daha fazla bilgi için [Bu makaleye](scenario-daemon-overview.md)bakın.

 Aşağıdaki PIP komutunu çalıştırarak MSAL Python uygulamasını yükleyebilirsiniz.

```powershell
pip install msal
```

### <a name="msal-initialization"></a>MSAL başlatma

Şu kodu ekleyerek MSAL başvurusunu ekleyebilirsiniz:

```Python
import msal
```

Sonra şu kodu kullanarak MSAL'yi başlatın:

```Python
app = msal.ConfidentialClientApplication(
    config["client_id"], authority=config["authority"],
    client_credential=config["secret"])
```

> | Burada: |Açıklama |
> |---------|---------|
> | `config["secret"]` | Azure portalında uygulama için istemci gizli dizisi oluşturulmuştur. |
> | `config["client_id"]` | **Uygulama (istemci) Kimliği**, Azure portalda kayıtlı uygulamadır. Bu değeri Azure portalda uygulamanın **Genel bakış** sayfasında bulabilirsiniz. |
> | `config["authority"]`    | Kimlik doğrulaması yapılacak kullanıcı için STS uç noktası. Genellikle `https://login.microsoftonline.com/{tenant}` {Tenant}, kiracınızın adı veya kiracı kimliğiniz olduğu genel bulut için.|

Daha fazla bilgi için lütfen [başvuru belgelerine `ConfidentialClientApplication` ](https://msal-python.readthedocs.io/en/latest/#confidentialclientapplication) bakın

### <a name="requesting-tokens"></a>Belirteç isteme

Uygulamanın kimliğini kullanarak bir belirteç istemek için `AcquireTokenForClient` yöntemi kullanın:

```Python
result = None
result = app.acquire_token_silent(config["scope"], account=None)

if not result:
    logging.info("No suitable token exists in cache. Let's get a new one from AAD.")
    result = app.acquire_token_for_client(scopes=config["scope"])
```

> |Burada:| Açıklama |
> |---------|---------|
> | `config["scope"]` | İstenen kapsamları içerir. Gizli istemciler için, `{Application ID URI}/.default` istenen kapsamların Azure portalında ayarlanmış uygulama nesnesi içinde statik olarak tanımlanmış olduğunu göstermek için şuna benzer biçimi kullanmalıdır (Microsoft Graph için, `{Application ID URI}` işaret eder `https://graph.microsoft.com` ). Özel Web API 'Leri için, `{Application ID URI}` Azure portalının uygulama kaydı 'nda (Önizleme) **bir API 'yi kullanıma** sunma bölümünde tanımlanmıştır. |

Daha fazla bilgi için lütfen [başvuru belgelerine `AcquireTokenForClient` ](https://msal-python.readthedocs.io/en/latest/#msal.ConfidentialClientApplication.acquire_token_for_client) bakın

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

Daemon uygulamaları hakkında daha fazla bilgi için bkz. senaryo giriş sayfası

> [!div class="nextstepaction"]
> [Web API 'Lerini çağıran Daemon uygulaması](scenario-daemon-overview.md)
