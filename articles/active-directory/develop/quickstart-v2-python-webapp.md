---
title: "Hızlı başlangıç: Microsoft 'a Python web uygulamasına oturum açma ekleme | Mavisi"
titleSuffix: Microsoft identity platform
description: Bu hızlı başlangıçta, bir Python web uygulamasının kullanıcılara nasıl oturum açabileceğinizi, Microsoft Identity platformundan bir erişim belirteci nasıl alabileceğinizi ve Microsoft Graph API 'yi çağıracağınızı öğrenin.
services: active-directory
author: abhidnya13
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: quickstart
ms.workload: identity
ms.date: 09/25/2019
ms.author: abpati
ms.custom: aaddev, devx-track-python, scenarios:getting-started, languages:Python
ms.openlocfilehash: 1a8d851d2e70850155950786c6aa67c1d5086eb2
ms.sourcegitcommit: 1bf144dc5d7c496c4abeb95fc2f473cfa0bbed43
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/24/2020
ms.locfileid: "95993882"
---
# <a name="quickstart-add-sign-in-with-microsoft-to-a-python-web-app"></a>Hızlı başlangıç: Microsoft 'a Python web uygulamasına oturum açma ekleme

Bu hızlı başlangıçta, bir Python web uygulamasının kullanıcılara nasıl oturum açıp Microsoft Graph API 'yi çağırmak için bir erişim belirteci edindiğini gösteren bir kod örneği indirip çalıştırırsınız. Kişisel Microsoft hesabı veya herhangi bir Azure Active Directory (Azure AD) kuruluşunda bir hesabı olan kullanıcılar uygulamada oturum açabilir.

Örneğin bir çizim için [nasıl çalıştığını](#how-the-sample-works) görün.

## <a name="prerequisites"></a>Önkoşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- [Python 2.7 +](https://www.python.org/downloads/release/python-2713) veya [Python 3 +](https://www.python.org/downloads/release/python-364/)
- [Flask](http://flask.pocoo.org/), [Flask-oturum](https://pypi.org/project/Flask-Session/), [istekler](https://requests.kennethreitz.org/en/master/)
- [MSAL Python](https://github.com/AzureAD/microsoft-authentication-library-for-python)

> [!div renderon="docs"]
>
> ## <a name="register-and-download-your-quickstart-app"></a>Hızlı başlangıç uygulamanızı kaydetme ve indirme
>
> Hızlı başlangıç uygulamanızı başlatmak için iki seçeneğiniz vardır: Express (seçenek 1) ve el ile (seçenek 2)
>
> ### <a name="option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample"></a>1. Seçenek: Uygulamanızı otomatik olarak kaydedip yapılandırın ve ardından kod örneğinizi indirin
>
> 1. [Azure portal uygulama kayıtları](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/applicationsListBlade/quickStartType/PythonQuickstartPage/sourceType/docs)gidin.
> 1. Uygulamanız için bir ad girin ve **Kaydet**'i seçin.
> 1. Yeni uygulamanızı indirip otomatik olarak yapılandırmak için yönergeleri izleyin.
>
> ### <a name="option-2-register-and-manually-configure-your-application-and-code-sample"></a>2. Seçenek: Uygulamanızı ve kod örneğinizi el ile kaydetme ve yapılandırma
>
> #### <a name="step-1-register-your-application"></a>1. Adım: Uygulamanızı kaydetme
>
> Uygulamanızı kaydetmek ve uygulama kayıt bilgilerinizi çözümünüze el ile eklemek için şu adımları izleyin:
>
> 1. [Azure Portal](https://portal.azure.com) iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
> 1. Hesabınız size birden fazla Azure AD kiracısına erişim sunuyorsa sağ üst köşeden hesabınızı seçin ve portal oturumunuzu istediğiniz Azure AD kiracısına ayarlayın.
> 1. Geliştiriciler için Microsoft Identity platformu [uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) sayfasına gidin.
> 1. **Yeni kayıt** seçeneğini belirleyin.
> 1. **Bir uygulamayı kaydet** sayfası göründüğünde, uygulamanızın kayıt bilgilerini girin:
>      - **Ad** bölümünde, örneğin, uygulamanın kullanıcılarına görüntülenecek anlamlı bir uygulama adı girin `python-webapp` .
>      - **Desteklenen hesap türleri** altında, **herhangi bir kurumsal dizin ve kişisel Microsoft hesabında hesaplar**' ı seçin.
>      - **Kaydet**’i seçin.
>      - Uygulamaya **genel bakış** sayfasında, daha sonra kullanılmak üzere **uygulama (istemci) kimliği** değerini aklınızda edin.
> 1. Menüden **kimlik doğrulamasını** seçin ve ardından aşağıdaki bilgileri ekleyin:
>    - **Web** platformu yapılandırmasını ekleyin. `http://localhost:5000/getAToken` **Yeniden yönlendirme URI 'leri** olarak ekleyin.
>    - **Kaydet**'i seçin.
> 1. Sol taraftaki menüde **sertifikalar & gizlilikler** ' ı seçin ve **istemci** gizli dizileri bölümünde **yeni istemci parolası** ' na tıklayın:
>
>      - Bir anahtar açıklaması (örnek uygulama gizli anahtarı) yazın.
>      - **1 yılda** bir anahtar süresi seçin.
>      - **Ekle**' ye tıkladığınızda, anahtar değeri görüntülenecektir.
>      - Anahtarın değerini kopyalayın. Buna daha sonra ihtiyacınız olacak.
> 1. **API izinleri** bölümünü seçin
>
>      - **Izin Ekle** düğmesine tıklayın ve ardından
>      - **Microsoft API 'leri** sekmesinin seçili olduğundan emin olun
>      - *Yaygın olarak kullanılan Microsoft API 'leri* bölümünde, **Microsoft Graph** ' ye tıklayın.
>      - **Temsilci izinleri** bölümünde, doğru izinlerin işaretli olduğundan emin olun: **User. Readbasic. All**. Gerekirse arama kutusunu kullanın.
>      - **Izin Ekle** düğmesini seçin
>
> [!div class="sxs-lookup" renderon="portal"]
>
> #### <a name="step-1-configure-your-application-in-azure-portal"></a>1. Adım: Uygulamanızı Azure portalında yapılandırma
>
> Bu hızlı başlangıçta çalışması için kod örneği için şunları yapmanız gerekir:
>
> 1. Olarak bir yanıt URL 'SI ekleyin `http://localhost:5000/getAToken` .
> 1. Bir Istemci gizli dizisi oluşturun.
> 1. Microsoft Graph API 'nin User. ReadBasic. All temsilci izinleri ekleyin.
>
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Bu değişiklikleri benim için yap]()
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Zaten yapılandırılmış](media/quickstart-v2-aspnet-webapp/green-check.png) Uygulamanız bu öznitelikle yapılandırılmış

#### <a name="step-2-download-your-project"></a>2. Adım: Projenizi indirme
> [!div renderon="docs"]
> [Kod örneğini indirin](https://github.com/Azure-Samples/ms-identity-python-webapp/archive/master.zip)

> [!div class="sxs-lookup" renderon="portal"]
> Projeyi indirin ve ZIP dosyasını kök klasöre daha yakın bir yerel klasöre ayıklayın. Örneğin, **C:\Azure-Samples**
> [!div class="sxs-lookup" renderon="portal" id="autoupdate" class="nextstepaction"]
> [Kod örneğini indirin](https://github.com/Azure-Samples/ms-identity-python-webapp/archive/master.zip)

> [!div class="sxs-lookup" renderon="portal"]
> > [!NOTE]
> > `Enter_the_Supported_Account_Info_Here`

> [!div renderon="docs"]
> #### <a name="step-3-configure-the-application"></a>3. Adım: uygulamayı yapılandırma
>
> 1. Zip dosyasını kök klasöre yakın bir yerel klasöre (örneğin **C:\Azure-Samples**) açın
> 1. Tümleşik bir geliştirme ortamı kullanıyorsanız, örneği en sevdiğiniz IDE (isteğe bağlı) içinde açın.
> 1. Kök klasörde bulunan **app_config. Kopyala** dosyasını açın ve aşağıdaki kod parçacığı ile değiştirin:
>
> ```python
> CLIENT_ID = "Enter_the_Application_Id_here"
> CLIENT_SECRET = "Enter_the_Client_Secret_Here"
> AUTHORITY = "https://login.microsoftonline.com/Enter_the_Tenant_Name_Here"
> ```
> Konum:
>
> - `Enter_the_Application_Id_here` - Kaydettiğiniz uygulamanın Uygulama Kimliği değeridir.
> - `Enter_the_Client_Secret_Here` -Sertifikalar 'da oluşturduğunuz **Istemci gizli anahtarı** , kaydettiğiniz uygulamanın **gizli dizileri &**  .
> - `Enter_the_Tenant_Name_Here` -kaydettiğiniz uygulamanın **Dizin (kiracı) kimliği** değeridir.

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-3-run-the-code-sample"></a>3. Adım: kod örneğini çalıştırma

> [!div renderon="docs"]
> #### <a name="step-4-run-the-code-sample"></a>4. Adım: kod örneğini çalıştırma

1. Sunucu tarafı oturum yönetimi için Flask-Sessions MSAL Python kitaplığı, Flask çerçevesi, aşağıdaki gibi PIP kullanarak istekler yüklemeniz gerekir:

    ```Shell
    pip install -r requirements.txt
    ```

2. App.py from Shell veya komut satırından Çalıştır:

    ```Shell
    python app.py
    ```
   > [!IMPORTANT]
   > Bu hızlı başlangıç uygulaması, kendisini gizli istemci olarak tanımlamak için bir istemci gizli anahtarı kullanır. İstemci parolası proje dosyalarınıza düz metin olarak eklendiğinden, güvenlik nedenleriyle, uygulamayı üretim uygulaması olarak düşünmeden önce istemci parolası yerine bir sertifika kullanmanız önerilir. Sertifika kullanma hakkında daha fazla bilgi için [Bu yönergelere](./active-directory-certificate-credentials.md)bakın.

## <a name="more-information"></a>Daha fazla bilgi

### <a name="how-the-sample-works"></a>Örneğin nasıl çalıştığı
![Bu hızlı başlangıç tarafından oluşturulan örnek uygulamanın nasıl çalıştığını gösterir](media/quickstart-v2-python-webapp/python-quickstart.svg)

### <a name="getting-msal"></a>MSAL alma
MSAL, kullanıcıların oturum açması ve Microsoft Identity platformu tarafından korunan bir API 'ye erişmek için kullanılan belirteçleri istemek için kullanılan bir kitaplıktır.
PIP kullanarak uygulamanıza MSAL Python ekleyebilirsiniz.

```Shell
pip install msal
```

### <a name="msal-initialization"></a>MSAL başlatma
MSAL kullanacağınız dosyanın en üstüne aşağıdaki kodu ekleyerek MSAL Python öğesine başvuru ekleyebilirsiniz:

```Python
import msal
```

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

Çok parçalı senaryo serimizin kullanıcılarına oturum açmak için Web Apps hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [Senaryo: kullanıcılarda oturum açan Web uygulaması](scenario-web-app-sign-user-overview.md)
