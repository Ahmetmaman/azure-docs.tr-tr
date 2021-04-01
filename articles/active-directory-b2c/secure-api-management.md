---
title: Azure Active Directory B2C kullanarak bir Azure API Management API 'sinin güvenliğini sağlama
description: Azure API Management API uç noktasının güvenliğini sağlamak için Azure Active Directory B2C tarafından verilen erişim belirteçlerini nasıl kullanacağınızı öğrenin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 07/30/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 60b7bb33dfbf29b7e448887ce992d03009133b2e
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "94953500"
---
# <a name="secure-an-azure-api-management-api-with-azure-ad-b2c"></a>Azure AD B2C ile Azure API Management API 'sinin güvenliğini sağlama

Azure API Management (APıM) API 'nize erişimi, Azure Active Directory B2C (Azure AD B2C) ile kimliği doğrulanan istemcilere nasıl kısıtlayacağınızı öğrenin. Bu makaledeki adımları izleyerek, erişimi yalnızca geçerli bir Azure AD B2C verilen erişim belirteci içeren isteklerle sınırlayan APıM 'de bir gelen ilke oluşturun ve test edin.

## <a name="prerequisites"></a>Önkoşullar

Bu makaledeki adımlarla devam etmeden önce aşağıdaki kaynaklara sahip olmanız gerekir:

* [Azure AD B2C kiracı](tutorial-create-tenant.md)
* Kiracınızda [kayıtlı uygulama](tutorial-register-applications.md)
* Kiracınızda [oluşturulan kullanıcı akışları](tutorial-create-user-flows.md)
* Azure API Management 'de [YAYıMLANMıŞ API](../api-management/import-and-publish.md)
* Güvenli erişimi test etmek için [Postman](https://www.getpostman.com/) (isteğe bağlı)

## <a name="get-azure-ad-b2c-application-id"></a>Azure AD B2C uygulama KIMLIĞI al

Azure API Management 'de Azure AD B2C bir API 'yi güvenli hale getirmeye çalıştığınızda, APıM içinde oluşturduğunuz [gelen ilke](../api-management/api-management-howto-policies.md) için birkaç değere ihtiyacınız vardır. İlk olarak, Azure AD B2C kiracınızda daha önce oluşturduğunuz bir uygulamanın uygulama KIMLIĞINI kaydedin. Önkoşullarda oluşturduğunuz uygulamayı kullanıyorsanız, *webbapp1* IÇIN uygulama kimliği ' ni kullanın.

Bir uygulamayı Azure AD B2C kiracınıza kaydetmek için yeni Birleşik **uygulama kayıtları** deneyimimizi veya eski  **uygulamalarımız (eski)** deneyimimizi kullanabilirsiniz. [Yeni deneyim hakkında daha fazla bilgi edinin](./app-registrations-training-guide.md).

#### <a name="app-registrations"></a>[Uygulama kayıtları](#tab/app-reg-ga/)

1. [Azure portalında](https://portal.azure.com) oturum açın.
1. Üst menüden **Dizin + abonelik** filtresi ' ni seçin ve ardından Azure AD B2C kiracınızı içeren dizini seçin.
1. Sol menüden **Azure AD B2C**' yi seçin. Ya da **tüm hizmetler** ' i seçin ve **Azure AD B2C** seçin.
1. **Uygulama kayıtları**' yi seçin ve ardından **sahip olunan uygulamalar** sekmesini seçin.
1. Değeri, *WebApp1* için **uygulama (istemci) kimliği** sütununa veya daha önce oluşturduğunuz başka bir uygulamaya kaydedin.

#### <a name="applications-legacy"></a>[Uygulamalar (eski)](#tab/applications-legacy/)

1. [Azure portalında](https://portal.azure.com) oturum açın.
1. Üst menüden **Dizin + abonelik** filtresi ' ni seçin ve ardından Azure AD B2C kiracınızı içeren dizini seçin.
1. Sol menüden **Azure AD B2C**' yi seçin. Ya da **tüm hizmetler** ' i seçin ve **Azure AD B2C** seçin.
1. **Yönet** altında uygulamalar ' ı **(eski)** seçin.
1. Değeri *WebApp1* veya daha önce oluşturduğunuz başka bir uygulama IÇIN **uygulama kimliği** sütununa kaydedin.

* * *

## <a name="get-token-issuer-endpoint"></a>Belirteç Verenin uç noktasını al

Sonra, Azure AD B2C Kullanıcı akışlarınızdan biri için iyi bilinen yapılandırma URL 'sini alın. Ayrıca, Azure API Management 'da desteklemek istediğiniz belirteç verenin uç nokta URI 'sine ihtiyacınız vardır.

1. [Azure portal](https://portal.azure.com)Azure AD B2C kiracınıza gidin.
1. **İlkeler** altında **Kullanıcı akışları**' nı seçin.
1. Mevcut bir ilkeyi seçin (örneğin *B2C_1_signupsignin1*) ve ardından **Kullanıcı akışını Çalıştır**' ı seçin.
1. Sayfanın üst kısmındaki **Kullanıcı akış** başlığının altında görüntülenen köprüye URL 'yi kaydedin. Bu URL, Kullanıcı akışına yönelik OpenID Connect iyi bilinen bulma uç noktasıdır ve Azure API Management gelen ilkesini yapılandırırken sonraki bölümde kullanılır.

    ![Azure portal Şimdi Çalıştır sayfasında iyi bilinen URI Köprüsü](media/secure-apim-with-b2c-token/portal-01-policy-link.png)

1. OpenID Connect iyi bilinen yapılandırma sayfasına gitmek için köprüyü seçin.
1. Tarayıcınızda açılan sayfada `issuer` değeri kaydedin, örneğin:

    `https://<tenant-name>.b2clogin.com/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/v2.0/`

    Bu değeri, API 'nizi Azure API Management yapılandırırken bir sonraki bölümde kullanırsınız.

Artık bir sonraki bölümde kullanılmak üzere kaydedilmiş iki URL 'ye sahip olmanız gerekir: OpenID Connect iyi bilinen yapılandırma uç noktası URL 'SI ve veren URI. Örnek:

```
https://<tenant-name>.b2clogin.com/<tenant-name>.onmicrosoft.com/B2C_1_signupsignin1/v2.0/.well-known/openid-configuration
https://<tenant-name>.b2clogin.com/99999999-0000-0000-0000-999999999999/v2.0/
```

## <a name="configure-inbound-policy-in-azure-api-management"></a>Azure API Management gelen ilkesini yapılandırma

Artık, API çağrılarını doğrulayan Azure API Management gelen ilkesini eklemeye hazırsınız. Erişim belirtecinde hedef kitleyi ve veren 'i doğrulayan bir [JWT doğrulama](../api-management/api-management-access-restriction-policies.md#ValidateJWT) ilkesi ekleyerek, yalnızca geçerli BELIRTECE sahip API çağrılarının kabul edilmesini sağlayabilirsiniz.

1. [Azure Portal](https://portal.azure.com)Azure API Management örneğinizi inceleyin.
1. **API’ler** seçeneğini belirleyin.
1. Azure AD B2C güvenliğini sağlamak istediğiniz API 'YI seçin.
1. **Tasarım** sekmesini seçin.
1. **Gelen işlem** altında, **\</\>** ilke kodu düzenleyicisini açmak için seçin.
1. Aşağıdaki `<validate-jwt>` etiketi ilkenin içine yerleştirin `<inbound>` .

    1. `url` `<openid-config>` Öğesindeki değeri ilkenizin TANıNMıŞ yapılandırma URL 'siyle güncelleştirin.
    1. `<audience>`Öğesini daha önce B2C kiracınızda oluşturduğunuz uygulamanın uygulama kimliğiyle güncelleştirin (örneğin, *WebApp1*).
    1. `<issuer>`Öğesini daha önce kaydettiğiniz belirteç veren uç noktasıyla güncelleştirin.

    ```xml
    <policies>
        <inbound>
            <validate-jwt header-name="Authorization" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Access token is missing or invalid.">
                <openid-config url="https://<tenant-name>.b2clogin.com/<tenant-name>.onmicrosoft.com/B2C_1_signupsignin1/v2.0/.well-known/openid-configuration" />
                <audiences>
                    <audience>44444444-0000-0000-0000-444444444444</audience>
                </audiences>
                <issuers>
                    <issuer>https://<tenant-name>.b2clogin.com/99999999-0000-0000-0000-999999999999/v2.0/</issuer>
                </issuers>
            </validate-jwt>
            <base />
        </inbound>
        <backend> <base /> </backend>
        <outbound> <base /> </outbound>
        <on-error> <base /> </on-error>
    </policies>
    ```

## <a name="validate-secure-api-access"></a>Güvenli API erişimini doğrulama

API 'nize yalnızca kimliği doğrulanmış çağıranlar erişebildiğinden emin olmak için, API 'yi [Postman](https://www.getpostman.com/)Ile çağırarak Azure API Management yapılandırmanızı doğrulayabilirsiniz.

API 'yi çağırmak için hem Azure AD B2C tarafından verilen bir erişim belirtecine hem de bir APıM abonelik anahtarına ihtiyacınız vardır.

### <a name="get-an-access-token"></a>Bir erişim belirteci alma

İlk olarak, `Authorization` Postman 'daki üst bilgide kullanmak üzere Azure AD B2C tarafından verilen bir belirtece ihtiyacınız vardır. Kaydolma/oturum açma Kullanıcı akışınız için **Şimdi Çalıştır** özelliğini kullanarak bir tane, önkoşullardan biri olarak oluşturmuş olmanız gerekir.

1. [Azure portal](https://portal.azure.com)Azure AD B2C kiracınıza gidin.
1. **İlkeler** altında **Kullanıcı akışları**' nı seçin.
1. Mevcut bir kaydolma/oturum açma Kullanıcı akışı seçin, örneğin *B2C_1_signupsignin1*.
1. **Uygulama** için *WebApp1* öğesini seçin.
1. **Yanıt URL 'si** için öğesini seçin `https://jwt.ms` .
1. **Kullanıcı akışını Çalıştır**' ı seçin.

    ![Azure portal oturum açmak için Kullanıcı akış sayfasını Çalıştır](media/secure-apim-with-b2c-token/portal-03-user-flow.png)

1. Oturum açma işlemini tamamlayın. Uygulamasına yönlendirilmelisiniz `https://jwt.ms` .
1. Tarayıcınızda görünen kodlanmış belirteç değerini kaydedin. Bu belirteç değerini Postman 'daki yetkilendirme üst bilgisi için kullanırsınız.

    ![Jwt.ms üzerinde görünen kodlanmış belirteç değeri](media/secure-apim-with-b2c-token/jwt-ms-01-token.png)

### <a name="get-api-subscription-key"></a>API abonelik anahtarını al

Yayımlanmış bir API 'yi çağıran bir istemci uygulaması (Bu durumda Postman), HTTP isteklerinde API 'ye geçerli bir API Management abonelik anahtarı içermelidir. Postman HTTP isteğinize dahil edilecek bir abonelik anahtarı almak için:

1. [Azure Portal](https://portal.azure.com)Azure API Management hizmet örneğinize gidin.
1. **Abonelikler**'i seçin.
1. **Ürün** için üç noktayı seçin ve ardından **anahtarları göster/gizle**' yi seçin.
1. Ürünün **BIRINCIL anahtarını** kaydedin. Bu anahtarı `Ocp-Apim-Subscription-Key` Postman 'DAKI http talebinizdeki üst bilgi için kullanırsınız.

![Azure portal anahtarları göster/gizle seçiliyken abonelik anahtarı sayfası](media/secure-apim-with-b2c-token/portal-04-api-subscription-key.png)

### <a name="test-a-secure-api-call"></a>Güvenli bir API çağrısını test etme

Erişim belirteci ve APıM abonelik anahtarı kaydedildiğinde, artık API 'ye güvenli erişimi doğru şekilde yapılandırıp yapılandırmadığınızı test etmeye hazırsınız demektir.

1. `GET` [Postman](https://www.getpostman.com/)'da yeni bir istek oluşturun. İstek URL 'SI için, önkoşullardan biri olarak yayımladığınız API 'nin hoparlör listesi uç noktasını belirtin. Örnek:

    `https://contosoapim.azure-api.net/conference/speakers`

1. Sonra, aşağıdaki üst bilgileri ekleyin:

    | Anahtar | Değer |
    | --- | ----- |
    | `Authorization` | Daha önce kaydettiğiniz kodlanmış belirteç değeri, ön eki eklenmiş `Bearer ` ("taşıyıcı" den sonra boşluk dahil) |
    | `Ocp-Apim-Subscription-Key` | Daha önce kaydettiğiniz APıM abonelik anahtarı |

    **Get** Request URL 'Si ve **başlıklarınız** şuna benzer görünmelidir:

    ![GET isteği URL 'sini ve üstbilgilerini gösteren Postman Kullanıcı arabirimi](media/secure-apim-with-b2c-token/postman-01-headers.png)

1. İsteği yürütmek için Postman 'daki **Gönder** düğmesini seçin. Her şeyi doğru şekilde yapılandırdıysanız, bir konferans hoparlörleri koleksiyonu ile bir JSON yanıtı (burada kesilmiş olarak gösterilir) sunulur:

    ```json
    {
      "collection": {
        "version": "1.0",
        "href": "https://conferenceapi.azurewebsites.net:443/speakers",
        "links": [],
        "items": [
          {
            "href": "https://conferenceapi.azurewebsites.net/speaker/1",
            "data": [
              {
                "name": "Name",
                "value": "Scott Guthrie"
              }
            ],
            "links": [
              {
                "rel": "http://tavis.net/rels/sessions",
                "href": "https://conferenceapi.azurewebsites.net/speaker/1/sessions"
              }
            ]
          },
    [...]
    ```

### <a name="test-an-insecure-api-call"></a>Güvenli olmayan bir API çağrısını test etme

Başarılı *bir istek* YAPTıK, API 'nize yapılan çağrıların beklenen şekilde reddedildiğini sağlamak için hata durumunu test edin. Testi gerçekleştirmenin bir yolu, belirteç değerindeki birkaç karakteri eklemek veya değiştirmek, ardından aynı isteği daha sonra çalıştırmak `GET` .

1. Geçersiz bir belirtecin benzetimini yapmak için belirteç değerine birkaç karakter ekleyin. Örneğin, belirteç değerine "GEÇERSIZ" ekleyin:

    ![Postman Kullanıcı arabiriminin üstbilgiler bölümü, GEÇERSIZ belirtece eklenmiş olarak gösteriliyor](media/secure-apim-with-b2c-token/postman-02-invalid-token.png)

1. İsteği yürütmek için **Gönder** düğmesini seçin. Geçersiz bir belirteçle, beklenen sonuç `401` yetkisiz bir durum kodudur:

    ```json
    {
        "statusCode": 401,
        "message": "Unauthorized. Access token is missing or invalid."
    }
    ```

`401`Durum kodunu görürseniz, yalnızca Azure AD B2C tarafından verilen geçerli erişim belirtecine sahip çağıranların Azure API Management API 'nize başarılı istekler yapacadığını doğruladınız.

## <a name="support-multiple-applications-and-issuers"></a>Birden çok uygulamayı ve verenler destekleme

Birçok uygulama genellikle tek bir REST API etkileşim kurar. API 'nizin birden çok uygulama için tasarlanan belirteçleri kabul etmesine olanak tanımak için, uygulama kimliklerini `<audiences>` APIM gelen ilkesindeki öğesine ekleyin.

```xml
<!-- Accept tokens intended for these recipient applications -->
<audiences>
    <audience>44444444-0000-0000-0000-444444444444</audience>
    <audience>66666666-0000-0000-0000-666666666666</audience>
</audiences>
```

Benzer şekilde, birden çok belirteç verenler desteklemek için, `<issuers>` APIM gelen ilkesindeki öğesine kendi uç nokta URI 'lerini ekleyin.

```xml
<!-- Accept tokens from multiple issuers -->
<issuers>
    <issuer>https://<tenant-name>.b2clogin.com/99999999-0000-0000-0000-999999999999/v2.0/</issuer>
    <issuer>https://login.microsoftonline.com/99999999-0000-0000-0000-999999999999/v2.0/</issuer>
</issuers>
```

## <a name="migrate-to-b2clogincom"></a>B2clogin.com 'e geçiş

Eski uç nokta tarafından verilen belirteçleri doğrulayan bir APıM API 'SI varsa `login.microsoftonline.com` , API 'yi ve bunu çağıran uygulamaları [b2clogin.com](b2clogin.md)tarafından verilen belirteçleri kullanacak şekilde geçirmeniz gerekir.

Aşamalı bir geçiş gerçekleştirmek için bu genel işlemi izleyebilirsiniz:

1. Hem b2clogin.com hem de login.microsoftonline.com tarafından verilen belirteçler için APıM gelen ilkenize destek ekleyin.
1. Uygulamalarınızı b2clogin.com uç noktasından belirteçleri almak için tek seferde güncelleştirin.
1. Tüm uygulamalarınız b2clogin.com ' dan belirteçleri doğru bir şekilde aldıktan sonra, API 'den login.microsoftonline.com tarafından verilen belirteçler desteğini kaldırın.

Aşağıdaki örnek APıM gelen ilkesi, hem b2clogin.com hem de login.microsoftonline.com tarafından verilen belirteçlerin nasıl kabul edileceği gösterilmektedir. Ayrıca, iki uygulamadan API isteklerini destekler.

```xml
<policies>
    <inbound>
        <validate-jwt header-name="Authorization" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Access token is missing or invalid.">
            <openid-config url="https://<tenant-name>.b2clogin.com/<tenant-name>.onmicrosoft.com/B2C_1_signupsignin1/v2.0/.well-known/openid-configuration" />
            <audiences>
                <audience>44444444-0000-0000-0000-444444444444</audience>
                <audience>66666666-0000-0000-0000-666666666666</audience>
            </audiences>
            <issuers>
                <issuer>https://login.microsoftonline.com/99999999-0000-0000-0000-999999999999/v2.0/</issuer>
                <issuer>https://<tenant-name>.b2clogin.com/99999999-0000-0000-0000-999999999999/v2.0/</issuer>
            </issuers>
        </validate-jwt>
        <base />
    </inbound>
    <backend> <base /> </backend>
    <outbound> <base /> </outbound>
    <on-error> <base /> </on-error>
</policies>
```

## <a name="next-steps"></a>Sonraki adımlar

Azure API Management ilkeleri hakkında daha fazla bilgi için [APIM ilke başvurusu dizinine](../api-management/api-management-policies.md)bakın.

Owin tabanlı Web API 'Lerini ve uygulamalarını b2clogin.com 'ye geçirme hakkında bilgi edinmek için [OWIN tabanlı Web API 'sini b2clogin.com 'e geçirin](multiple-token-endpoints.md).