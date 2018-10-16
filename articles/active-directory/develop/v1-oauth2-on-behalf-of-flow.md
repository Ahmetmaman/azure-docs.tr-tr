---
title: OAuth2.0 On-Behalf-Of taslak belirtimi kullanarak azure AD hizmet kimlik doğrulaması | Microsoft Docs
description: Bu makalede, HTTP iletileri OAuth2.0 On-Behalf-Of akışı kullanarak hizmet kimlik doğrulaması uygulamak için kullanmayı açıklar.
services: active-directory
documentationcenter: .net
author: navyasric
manager: mtillman
editor: ''
ms.assetid: 09f6f318-e88b-4024-9ee1-e7f09fb19a82
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/06/2017
ms.author: celested
ms.reviewer: hirsin, nacanuma
ms.custom: aaddev
ms.openlocfilehash: cf62d961d7bd2b6ff2cb03ee577368f2ee7b8452
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2018
ms.locfileid: "49318841"
---
# <a name="service-to-service-calls-using-delegated-user-identity-in-the-on-behalf-of-flow"></a>Hizmetten hizmete çağrılar kullanarak yönetici temsilcisi kullanıcı kimliği, On-Behalf-Of akışı
Bir uygulama hizmeti/sırayla başka çağırmak için gereken web API'si, burada çağırır kullanım örneği akış hizmet OAuth 2.0 On-Behalf-Of (OBO) hizmeti/web API'si. İstek zincirinin aracılığıyla izinleri ve yetkilendirilmiş kullanıcının kimlik yayılması olur. Orta katman hizmet kimliği doğrulanmış istekler aşağı akış hizmetinize, Azure Active Directory'den (Azure AD), bir erişim belirteci güvenliğini sağlamak kullanıcının adına olmalıdır.

> [!IMPORTANT]
> Mayıs 2018'den itibaren bir `id_token` kullanılamaz On-Behalf-Of akışı için - Spa'lar geçmesi gereken bir **erişim** OBO gerçekleştirmek için gizli bir istemci bir orta katman belirtecini akar. Bkz: [sınırlamaları](#client-limitations) üzerinde istemcileri On-Behalf-Of çağrıları gerçekleştirebilir daha fazla ayrıntı için.

## <a name="on-behalf-of-flow-diagram"></a>On-Behalf-Of akışı diyagramı
Kullanıcı bir uygulama kullanarak doğrulandıktan varsayar [OAuth 2.0 yetkilendirme kodu verme akışı](v1-protocols-oauth-code.md). Bu noktada, uygulama, kullanıcı talepleri ve orta katman web API'si (API A) erişmek için izniniz bir erişim belirteci (belirteç A) sahip. Şimdi, API bir aşağı akış web API'sine (API B) kimliği doğrulanmış bir istekte gerekir.

Aşağıdaki adımları On-Behalf-Of akışı oluşturan ve aşağıdaki diyagramda yardımıyla açıklanmıştır.

![OAuth2.0 üzerinde-Behalf-Of akışı](./media/v1-oauth2-on-behalf-of-flow/active-directory-protocols-oauth-on-behalf-of-flow.png)


1. İstemci uygulama bir API A A. belirteciyle istekte
2. API bir Azure AD belirteç yayınında uç noktaya kimliğini doğrular ve API B'nin erişmek için bir belirteç istekleri
3. Azure AD belirteç yayınında uç noktası, API A'ın kimlik belirteci A ile doğrular ve API B (belirteç B) için erişim belirteci verir.
4. ' % S'belirteci B API b isteğin yetkilendirme üst bilgisinde ayarlama
5. Güvenli kaynaktan veri API b tarafından döndürülür.

## <a name="register-the-application-and-service-in-azure-ad"></a>Uygulama ve hizmet, Azure AD'ye kaydetme
Hem istemci uygulaması hem de bir orta katman hizmet Azure AD'ye kaydetme.
### <a name="register-the-middle-tier-service"></a>Orta katman hizmet kaydı
1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Üst çubuğunda, hesabınızda ve altında tıklayın **dizin** listesinde, uygulamanızı kaydetmek istediğiniz Active Directory kiracısı seçin.
3. Tıklayarak **diğer hizmetler** , sol taraftaki gezinti ve **Azure Active Directory**.
4. Tıklayarak **uygulama kayıtları** ve **yeni uygulama kaydı**.
5. Uygulama için kolay bir ad girin ve uygulama türünü seçin. Uygulama türü kümesinde oturum açma URL'si veya yeniden yönlendirme URL'sine temel URL'yi temel. Tıklayarak **Oluştur** uygulama oluşturmak için.
6. Azure portalında hala, uygulamanızı seçin ve tıklayın **ayarları**. Ayarlar menüsünden **anahtarları** ve bir anahtar ekleyin - 1 yıl ya da 2 yıl önemli bir süre seçin. Bu sayfayı kaydedin, anahtar değeri görüntülenir, kopyalama ve değerin güvenli bir konuma kaydedin,-daha sonra uygulamanızda - uygulama ayarlarını yapılandırmak için bu anahtar gerekir, bu anahtar değeri diğer herhangi bir yolla yeniden görüntülenen ya da alınabilir olmaz , bu nedenle Lütfen Azure Portalı'ndan görünür duruma geldiği kaydedin.

### <a name="register-the-client-application"></a>İstemci uygulamayı kaydetme
1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Üst çubuğunda, hesabınızda ve altında tıklayın **dizin** listesinde, uygulamanızı kaydetmek istediğiniz Active Directory kiracısı seçin.
3. Tıklayarak **diğer hizmetler** , sol taraftaki gezinti ve **Azure Active Directory**.
4. Tıklayarak **uygulama kayıtları** ve **yeni uygulama kaydı**.
5. Uygulama için kolay bir ad girin ve uygulama türünü seçin. Uygulama türü kümesinde oturum açma URL'si veya yeniden yönlendirme URL'sine temel URL'yi temel. Tıklayarak **Oluştur** uygulama oluşturmak için.
6. İzinleri - ayarlar menüsünden uygulamanız için yapılandırma öğesini **gerekli izinler** bölümünde, tıklayarak **Ekle**, ardından **bir API seçin**ve adını yazın Orta katman hizmet metin kutusuna. Ardından **Select izinleri** seçip ' erişim *hizmet adı*'.

### <a name="configure-known-client-applications"></a>Bilinen istemci uygulamalarını yapılandırın
Bu senaryoda, kullanıcı etkileşimi olmadan aşağı akış API'ye erişmek için kullanıcı onayı almak için orta katman hizmet vardır. Bu nedenle, adım onay bir parçası olarak kimlik doğrulaması sırasında aşağı akış API'sine erişim izni seçeneği önceden sunulmalıdır.
Bunu başarmak için açıkça tek bir iletişim kutusuna istemci ve orta katman tarafından gerekli onay birleştirir orta katman hizmet kaydının ile Azure AD'de istemci uygulamasının kaydı bağlamak için aşağıdaki adımları izleyin.
1. Orta katman hizmet kaydı için gidin ve tıklayarak **bildirim** bildirim düzenleyicisini açın.
2. Bildiriminde bulun `knownClientApplications` dizi özelliği ve öğe olarak istemci uygulamasının istemci kimliği ekleyin.
3. Kaydet'e tıklayarak bildirim Kaydet düğmesi.

## <a name="service-to-service-access-token-request"></a>Hizmet erişim belirteci isteği için hizmeti
Bir HTTP POST yapmak için kiracıya özgü bir erişim belirteci istemek için aşağıdaki parametrelerle Azure AD uç noktası.

```
https://login.microsoftonline.com/<tenant>/oauth2/token
```
İstemci uygulaması paylaşılan bir gizli dizi veya bir sertifika tarafından güvenli hale seçti olup olmadığına bağlı olarak iki durum vardır.

### <a name="first-case-access-token-request-with-a-shared-secret"></a>İlk durumda: paylaşılan bir gizli dizi ile erişim belirteci isteği
Paylaşılan gizlilik kullanırken, hizmetten hizmete erişim belirteci isteği aşağıdaki parametreleri içerir:

| Parametre |  | Açıklama |
| --- | --- | --- |
| grant_type değeri |gerekli | Belirteç isteği türü. JWT'nin kullanarak bir istek için bir değer olmalıdır **urn: ietf:params:oauth:grant-türü: jwt-taşıyıcı**. |
| onaylama |gerekli | İstekte kullanılan belirteç değeri. |
| client_id |gerekli | Azure AD ile kayıt sırasında arama hizmete atanan uygulama kimliği. Uygulama Kimliği Azure Yönetim Portalı'nda bulmak için tıklatın **Active Directory**Dizin'e tıklayın ve ardından uygulama adına tıklayın. |
| client_secret |gerekli | Anahtar arama hizmeti için Azure AD'de kayıtlı. Bu değeri kayıt zamanında Not. |
| kaynak |gerekli | Uygulama Kimliği URI'si (güvenli kaynak) alma hizmeti. Uygulama Kimliği URI'si, Azure Yönetim Portalı'nda bulmak için tıklatın **Active Directory**, dizini tıklatın, uygulama adına tıklayın, tıklayın **tüm ayarlar** ve ardından **özellikleri**. |
| requested_token_use |gerekli | İsteğin nasıl işleneceğini belirtir. On-Behalf-Of akışı değer olmalıdır **on_behalf_of**. |
| scope |gerekli | Boşlukla ayrılmış belirteci isteği için kapsam listesi. Openıd Connect, kapsam için **openıd** belirtilmesi gerekir.|

#### <a name="example"></a>Örnek
Aşağıdaki HTTP POST istekleri için bir erişim belirteci https://graph.windows.net web API'si. `client_id` Erişim belirteci isteklerini hizmeti tanımlar.

```
// line breaks for legibility only

POST /oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=urn%3Aietf%3Aparams%3Aoauth%3Agrant-type%3Ajwt-bearer
&client_id=625391af-c675-43e5-8e44-edd3e30ceb15
&client_secret=0Y1W%2BY3yYb3d9N8vSjvm8WrGzVZaAaHbHHcGbcgG%2BoI%3D
&resource=https%3A%2F%2Fgraph.windows.net
&assertion=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCIsImtpZCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCJ9.eyJhdWQiOiJodHRwczovL2Rkb2JhbGlhbm91dGxvb2sub25taWNyb3NvZnQuY29tLzE5MjNmODYyLWU2ZGMtNDFhMy04MWRhLTgwMmJhZTAwYWY2ZCIsImlzcyI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzI2MDM5Y2NlLTQ4OWQtNDAwMi04MjkzLTViMGM1MTM0ZWFjYi8iLCJpYXQiOjE0OTM0MjMxNTIsIm5iZiI6MTQ5MzQyMzE1MiwiZXhwIjoxNDkzNDY2NjUyLCJhY3IiOiIxIiwiYWlvIjoiWTJaZ1lCRFF2aTlVZEc0LzM0L3dpQndqbjhYeVp4YmR1TFhmVE1QeG8yYlN2elgreHBVQSIsImFtciI6WyJwd2QiXSwiYXBwaWQiOiJiMzE1MDA3OS03YmViLTQxN2YtYTA2YS0zZmRjNzhjMzI1NDUiLCJhcHBpZGFjciI6IjAiLCJlX2V4cCI6MzAyNDAwLCJmYW1pbHlfbmFtZSI6IlRlc3QiLCJnaXZlbl9uYW1lIjoiTmF2eWEiLCJpcGFkZHIiOiIxNjcuMjIwLjEuMTc3IiwibmFtZSI6Ik5hdnlhIFRlc3QiLCJvaWQiOiIxY2Q0YmNhYy1iODA4LTQyM2EtOWUyZi04MjdmYmIxYmI3MzkiLCJwbGF0ZiI6IjMiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJzdWIiOiJEVXpYbkdKMDJIUk0zRW5pbDFxdjZCakxTNUllQy0tQ2ZpbzRxS1MzNEc4IiwidGlkIjoiMjYwMzljY2UtNDg5ZC00MDAyLTgyOTMtNWIwYzUxMzRlYWNiIiwidW5pcXVlX25hbWUiOiJuYXZ5YUBkZG9iYWxpYW5vdXRsb29rLm9ubWljcm9zb2Z0LmNvbSIsInVwbiI6Im5hdnlhQGRkb2JhbGlhbm91dGxvb2sub25taWNyb3NvZnQuY29tIiwidmVyIjoiMS4wIn0.R-Ke-XO7lK0r5uLwxB8g5CrcPAwRln5SccJCfEjU6IUqpqcjWcDzeDdNOySiVPDU_ZU5knJmzRCF8fcjFtPsaA4R7vdIEbDuOur15FXSvE8FvVSjP_49OH6hBYqoSUAslN3FMfbO6Z8YfCIY4tSOB2I6ahQ_x4ZWFWglC3w5mK-_4iX81bqi95eV4RUKefUuHhQDXtWhrSgIEC0YiluMvA4TnaJdLq_tWXIc4_Tq_KfpkvI004ONKgU7EAMEr1wZ4aDcJV2yf22gQ1sCSig6EGSTmmzDuEPsYiyd4NhidRZJP4HiiQh-hePBQsgcSgYGvz9wC6n57ufYKh2wm_Ti3Q
&requested_token_use=on_behalf_of
&scope=openid
```

### <a name="second-case-access-token-request-with-a-certificate"></a>İkinci durumda: bir sertifika ile erişim belirteci isteği
Bir sertifika ile hizmetten hizmete erişim belirteci isteği aşağıdaki parametreleri içerir:

| Parametre |  | Açıklama |
| --- | --- | --- |
| grant_type değeri |gerekli | Belirteç isteği türü. JWT'nin kullanarak bir istek için bir değer olmalıdır **urn: ietf:params:oauth:grant-türü: jwt-taşıyıcı**. |
| onaylama |gerekli | İstekte kullanılan belirteç değeri. |
| client_id |gerekli | Azure AD ile kayıt sırasında arama hizmete atanan uygulama kimliği. Uygulama Kimliği Azure Yönetim Portalı'nda bulmak için tıklatın **Active Directory**Dizin'e tıklayın ve ardından uygulama adına tıklayın. |
| client_assertion_type |gerekli |Değer olmalıdır `urn:ietf:params:oauth:client-assertion-type:jwt-bearer` |
| client_assertion |gerekli | Onaylama (bir JSON Web belirteci) oluşturmak ve sertifika ile imzalamak için gereken kimlik bilgileri olarak uygulamanız için kayıtlı. Hakkında bilgi edinin [sertifika kimlik bilgileri](active-directory-certificate-credentials.md) sertifikanız ve onaylama biçimi kaydetme hakkında bilgi edinmek için.|
| kaynak |gerekli | Uygulama Kimliği URI'si (güvenli kaynak) alma hizmeti. Uygulama Kimliği URI'si, Azure Yönetim Portalı'nda bulmak için tıklatın **Active Directory**, dizini tıklatın, uygulama adına tıklayın, tıklayın **tüm ayarlar** ve ardından **özellikleri**. |
| requested_token_use |gerekli | İsteğin nasıl işleneceğini belirtir. On-Behalf-Of akışı değer olmalıdır **on_behalf_of**. |
| scope |gerekli | Boşlukla ayrılmış belirteci isteği için kapsam listesi. Openıd Connect, kapsam için **openıd** belirtilmesi gerekir.|

Client_secret parametresi tarafından iki parametre değiştirilir dışında parametreler neredeyse aynı paylaşılan gizli diziyi isteğiyle olduğu gibi olduğuna dikkat edin: client_assertion_type ve client_assertion.

#### <a name="example"></a>Örnek
Aşağıdaki HTTP POST istekleri için bir erişim belirteci https://graph.windows.net bir sertifika ile web API'si. `client_id` Erişim belirteci isteklerini hizmeti tanımlar.

```
// line breaks for legibility only

POST /oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=urn%3Aietf%3Aparams%3Aoauth%3Agrant-type%3Ajwt-bearer
&client_id=625391af-c675-43e5-8e44-edd3e30ceb15
&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer
&client_assertion=eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNScUtqRlBuZDdSRnd2d1pJMCJ9.eyJ{a lot of characters here}M8U3bSUKKJDEg
&resource=https%3A%2F%2Fgraph.windows.net
&assertion=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCIsImtpZCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCJ9.eyJhdWQiOiJodHRwczovL2Rkb2JhbGlhbm91dGxvb2sub25taWNyb3NvZnQuY29tLzE5MjNmODYyLWU2ZGMtNDFhMy04MWRhLTgwMmJhZTAwYWY2ZCIsImlzcyI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzI2MDM5Y2NlLTQ4OWQtNDAwMi04MjkzLTViMGM1MTM0ZWFjYi8iLCJpYXQiOjE0OTM0MjMxNTIsIm5iZiI6MTQ5MzQyMzE1MiwiZXhwIjoxNDkzNDY2NjUyLCJhY3IiOiIxIiwiYWlvIjoiWTJaZ1lCRFF2aTlVZEc0LzM0L3dpQndqbjhYeVp4YmR1TFhmVE1QeG8yYlN2elgreHBVQSIsImFtciI6WyJwd2QiXSwiYXBwaWQiOiJiMzE1MDA3OS03YmViLTQxN2YtYTA2YS0zZmRjNzhjMzI1NDUiLCJhcHBpZGFjciI6IjAiLCJlX2V4cCI6MzAyNDAwLCJmYW1pbHlfbmFtZSI6IlRlc3QiLCJnaXZlbl9uYW1lIjoiTmF2eWEiLCJpcGFkZHIiOiIxNjcuMjIwLjEuMTc3IiwibmFtZSI6Ik5hdnlhIFRlc3QiLCJvaWQiOiIxY2Q0YmNhYy1iODA4LTQyM2EtOWUyZi04MjdmYmIxYmI3MzkiLCJwbGF0ZiI6IjMiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJzdWIiOiJEVXpYbkdKMDJIUk0zRW5pbDFxdjZCakxTNUllQy0tQ2ZpbzRxS1MzNEc4IiwidGlkIjoiMjYwMzljY2UtNDg5ZC00MDAyLTgyOTMtNWIwYzUxMzRlYWNiIiwidW5pcXVlX25hbWUiOiJuYXZ5YUBkZG9iYWxpYW5vdXRsb29rLm9ubWljcm9zb2Z0LmNvbSIsInVwbiI6Im5hdnlhQGRkb2JhbGlhbm91dGxvb2sub25taWNyb3NvZnQuY29tIiwidmVyIjoiMS4wIn0.R-Ke-XO7lK0r5uLwxB8g5CrcPAwRln5SccJCfEjU6IUqpqcjWcDzeDdNOySiVPDU_ZU5knJmzRCF8fcjFtPsaA4R7vdIEbDuOur15FXSvE8FvVSjP_49OH6hBYqoSUAslN3FMfbO6Z8YfCIY4tSOB2I6ahQ_x4ZWFWglC3w5mK-_4iX81bqi95eV4RUKefUuHhQDXtWhrSgIEC0YiluMvA4TnaJdLq_tWXIc4_Tq_KfpkvI004ONKgU7EAMEr1wZ4aDcJV2yf22gQ1sCSig6EGSTmmzDuEPsYiyd4NhidRZJP4HiiQh-hePBQsgcSgYGvz9wC6n57ufYKh2wm_Ti3Q
&requested_token_use=on_behalf_of
&scope=openid
```

## <a name="service-to-service-access-token-response"></a>Hizmet erişim belirteci yanıtı hizmetine
Başarılı yanıtı aşağıdaki parametrelerle bir JSON OAuth 2.0 yanıtındaki ' dir.

| Parametre | Açıklama |
| --- | --- |
| token_type |Belirteç türü değeri gösterir. Azure AD destekleyen tek tür **taşıyıcı**. Taşıyıcı belirteçleri hakkında daha fazla bilgi için bkz: [OAuth 2.0 yetkilendirme Framework: taşıyıcı belirteç kullanımı (RFC 6750)](http://www.rfc-editor.org/rfc/rfc6750.txt). |
| scope |Erişim belirtecinde verilen kapsam. |
| expires_in |Süre (saniye cinsinden) erişim belirteci geçerlidir. |
| expires_on |Erişim belirtecinin süresinin sona erdiği zaman. Tarih 1970'ten saniye sayısı temsil edilen-01-kadar süre sonu UTC 01T0:0:0Z. Bu değer, önbelleğe alınan belirteç ömrünü belirlemek için kullanılır. |
| kaynak |Uygulama Kimliği URI'si (güvenli kaynak) alma hizmeti. |
| access_token |İstenen erişim belirteci. Arama hizmeti, alıcı hizmetinde kimlik doğrulaması için bu belirteci kullanabilirsiniz. |
| id_token |İstenen kimlik belirteci. Arama Hizmeti kullanıcının kimliğini doğrulamak ve kullanıcıyı bir oturum başlatmak için bunu kullanabilirsiniz. |
| refresh_token |İstenen erişim belirtecini yenileme belirteci. Arama hizmeti geçerli erişim belirtecinin süresi dolduktan sonra başka bir erişim belirteci istemek için bu belirteci kullanabilirsiniz. |

### <a name="success-response-example"></a>Başarılı yanıt örneği
Aşağıdaki örnek, bir başarı isteğine yanıt olarak bir erişim belirteci için gösterir https://graph.windows.net web API'si.

```
{
    "token_type":"Bearer",
    "scope":"User.Read",
    "expires_in":"43482",
    "ext_expires_in":"302683",
    "expires_on":"1493466951",
    "not_before":"1493423168",
    "resource":"https://graph.windows.net",
    "access_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCIsImtpZCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCJ9.eyJhdWQiOiJodHRwczovL2dyYXBoLndpbmRvd3MubmV0IiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvMjYwMzljY2UtNDg5ZC00MDAyLTgyOTMtNWIwYzUxMzRlYWNiLyIsImlhdCI6MTQ5MzQyMzE2OCwibmJmIjoxNDkzNDIzMTY4LCJleHAiOjE0OTM0NjY5NTEsImFjciI6IjEiLCJhaW8iOiJBU1FBMi84REFBQUE1NnZGVmp0WlNjNWdBVWwrY1Z0VFpyM0VvV2NvZEoveWV1S2ZqcTZRdC9NPSIsImFtciI6WyJwd2QiXSwiYXBwaWQiOiI2MjUzOTFhZi1jNjc1LTQzZTUtOGU0NC1lZGQzZTMwY2ViMTUiLCJhcHBpZGFjciI6IjEiLCJlX2V4cCI6MzAyNjgzLCJmYW1pbHlfbmFtZSI6IlRlc3QiLCJnaXZlbl9uYW1lIjoiTmF2eWEiLCJpcGFkZHIiOiIxNjcuMjIwLjEuMTc3IiwibmFtZSI6Ik5hdnlhIFRlc3QiLCJvaWQiOiIxY2Q0YmNhYy1iODA4LTQyM2EtOWUyZi04MjdmYmIxYmI3MzkiLCJwbGF0ZiI6IjMiLCJwdWlkIjoiMTAwMzNGRkZBMTJFRDdGRSIsInNjcCI6IlVzZXIuUmVhZCIsInN1YiI6IjNKTUlaSWJlYTc1R2hfWHdDN2ZzX0JDc3kxa1l1ekZKLTUyVm1Zd0JuM3ciLCJ0aWQiOiIyNjAzOWNjZS00ODlkLTQwMDItODI5My01YjBjNTEzNGVhY2IiLCJ1bmlxdWVfbmFtZSI6Im5hdnlhQGRkb2JhbGlhbm91dGxvb2sub25taWNyb3NvZnQuY29tIiwidXBuIjoibmF2eWFAZGRvYmFsaWFub3V0bG9vay5vbm1pY3Jvc29mdC5jb20iLCJ1dGkiOiJ4Q3dmemhhLVAwV0pRT0x4Q0dnS0FBIiwidmVyIjoiMS4wIn0.cqmUVjfVbqWsxJLUI1Z4FRx1mNQAHP-L0F4EMN09r8FY9bIKeO-0q1eTdP11Nkj_k4BmtaZsTcK_mUygdMqEp9AfyVyA1HYvokcgGCW_Z6DMlVGqlIU4ssEkL9abgl1REHElPhpwBFFBBenOk9iHddD1GddTn6vJbKC3qAaNM5VarjSPu50bVvCrqKNvFixTb5bbdnSz-Qr6n6ACiEimiI1aNOPR2DeKUyWBPaQcU5EAK0ef5IsVJC1yaYDlAcUYIILMDLCD9ebjsy0t9pj_7lvjzUSrbMdSCCdzCqez_MSNxrk1Nu9AecugkBYp3UVUZOIyythVrj6-sVvLZKUutQ",
    "refresh_token":"AQABAAAAAABnfiG-mA6NTae7CdWW7QfdjKGu9-t1scy_TDEmLi4eLQMjJGt_nAoVu6A4oSu1KsRiz8XyQIPKQxSGfbf2FoSK-hm2K8TYzbJuswYusQpJaHUQnSqEvdaCeFuqXHBv84wjFhuanzF9dQZB_Ng5za9xKlUENrNtlq9XuLNVKzxEyeUM7JyxzdY7JiEphWImwgOYf6II316d0Z6-H3oYsFezf4Xsjz-MOBYEov0P64UaB5nJMvDyApV-NWpgklLASfNoSPGb67Bc02aFRZrm4kLk-xTl6eKE6hSo0XU2z2t70stFJDxvNQobnvNHrAmBaHWPAcC3FGwFnBOojpZB2tzG1gLEbmdROVDp8kHEYAwnRK947Py12fJNKExUdN0njmXrKxNZ_fEM33LHW1Tf4kMX_GvNmbWHtBnIyG0w5emb-b54ef5AwV5_tGUeivTCCysgucEc-S7G8Cz0xNJ_BOiM_4bAv9iFmrm9STkltpz0-Tftg8WKmaJiC0xXj6uTf4ZkX79mJJIuuM7XP4ARIcLpkktyg2Iym9jcZqymRkGH2Rm9sxBwC4eeZXM7M5a7TJ-5CqOdfuE3sBPq40RdEWMFLcrAzFvP0VDR8NKHIrPR1AcUruat9DETmTNJukdlJN3O41nWdZOVoJM-uKN3uz2wQ2Ld1z0Mb9_6YfMox9KTJNzRzcL52r4V_y3kB6ekaOZ9wQ3HxGBQ4zFt-2U0mSszIAA",
    "id_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiI2MjUzOTFhZi1jNjc1LTQzZTUtOGU0NC1lZGQzZTMwY2ViMTUiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC8yNjAzOWNjZS00ODlkLTQwMDItODI5My01YjBjNTEzNGVhY2IvIiwiaWF0IjoxNDkzNDIzMTY4LCJuYmYiOjE0OTM0MjMxNjgsImV4cCI6MTQ5MzQ2Njk1MSwiYW1yIjpbInB3ZCJdLCJmYW1pbHlfbmFtZSI6IlRlc3QiLCJnaXZlbl9uYW1lIjoiTmF2eWEiLCJpcGFkZHIiOiIxNjcuMjIwLjEuMTc3IiwibmFtZSI6Ik5hdnlhIFRlc3QiLCJvaWQiOiIxY2Q0YmNhYy1iODA4LTQyM2EtOWUyZi04MjdmYmIxYmI3MzkiLCJwbGF0ZiI6IjMiLCJzdWIiOiJEVXpYbkdKMDJIUk0zRW5pbDFxdjZCakxTNUllQy0tQ2ZpbzRxS1MzNEc4IiwidGlkIjoiMjYwMzljY2UtNDg5ZC00MDAyLTgyOTMtNWIwYzUxMzRlYWNiIiwidW5pcXVlX25hbWUiOiJuYXZ5YUBkZG9iYWxpYW5vdXRsb29rLm9ubWljcm9zb2Z0LmNvbSIsInVwbiI6Im5hdnlhQGRkb2JhbGlhbm91dGxvb2sub25taWNyb3NvZnQuY29tIiwidXRpIjoieEN3ZnpoYS1QMFdKUU9MeENHZ0tBQSIsInZlciI6IjEuMCJ9."
}
```

### <a name="error-response-example"></a>Hata yanıtı örneği
Aşağı Akış API ayarlayabilirsiniz çok faktörlü kimlik doğrulaması gibi bir koşullu erişim ilkesi varsa aşağı akış API'si için bir erişim belirteci almak çalışırken, bir hata yanıtı Azure AD belirteç uç noktası tarafından döndürülür. İstemci uygulaması koşullu erişim ilkesi karşılamak için kullanıcı etkileşimi sağlayabilmesi orta katman hizmet istemci uygulamayı bu hata ortaya.

```
{
    "error":"interaction_required",
    "error_description":"AADSTS50079: Due to a configuration change made by your administrator, or because you moved to a new location, you must enroll in multi-factor authentication to access 'bf8d80f9-9098-4972-b203-500f535113b1'.\r\nTrace ID: b72a68c3-0926-4b8e-bc35-3150069c2800\r\nCorrelation ID: 73d656cf-54b1-4eb2-b429-26d8165a52d7\r\nTimestamp: 2017-05-01 22:43:20Z",
    "error_codes":[50079],
    "timestamp":"2017-05-01 22:43:20Z",
    "trace_id":"b72a68c3-0926-4b8e-bc35-3150069c2800",
    "correlation_id":"73d656cf-54b1-4eb2-b429-26d8165a52d7",
    "claims":"{\"access_token\":{\"polids\":{\"essential\":true,\"values\":[\"9ab03e19-ed42-4168-b6b7-7001fb3e933a\"]}}}"
}
```

## <a name="use-the-access-token-to-access-the-secured-resource"></a>Güvenli kaynak erişimi için erişim belirteci kullanın
Orta katman hizmet belirteci ayarlayarak, Aşağı Akış web API'sine, kimliği doğrulanmış isteğinde bulunmak için yukarıda edinilen belirteci artık `Authorization` başlığı.

### <a name="example"></a>Örnek
```
GET /me?api-version=2013-11-08 HTTP/1.1
Host: graph.windows.net
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCIsImtpZCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCJ9.eyJhdWQiOiJodHRwczovL2dyYXBoLndpbmRvd3MubmV0IiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvMjYwMzljY2UtNDg5ZC00MDAyLTgyOTMtNWIwYzUxMzRlYWNiLyIsImlhdCI6MTQ5MzQyMzE2OCwibmJmIjoxNDkzNDIzMTY4LCJleHAiOjE0OTM0NjY5NTEsImFjciI6IjEiLCJhaW8iOiJBU1FBMi84REFBQUE1NnZGVmp0WlNjNWdBVWwrY1Z0VFpyM0VvV2NvZEoveWV1S2ZqcTZRdC9NPSIsImFtciI6WyJwd2QiXSwiYXBwaWQiOiI2MjUzOTFhZi1jNjc1LTQzZTUtOGU0NC1lZGQzZTMwY2ViMTUiLCJhcHBpZGFjciI6IjEiLCJlX2V4cCI6MzAyNjgzLCJmYW1pbHlfbmFtZSI6IlRlc3QiLCJnaXZlbl9uYW1lIjoiTmF2eWEiLCJpcGFkZHIiOiIxNjcuMjIwLjEuMTc3IiwibmFtZSI6Ik5hdnlhIFRlc3QiLCJvaWQiOiIxY2Q0YmNhYy1iODA4LTQyM2EtOWUyZi04MjdmYmIxYmI3MzkiLCJwbGF0ZiI6IjMiLCJwdWlkIjoiMTAwMzNGRkZBMTJFRDdGRSIsInNjcCI6IlVzZXIuUmVhZCIsInN1YiI6IjNKTUlaSWJlYTc1R2hfWHdDN2ZzX0JDc3kxa1l1ekZKLTUyVm1Zd0JuM3ciLCJ0aWQiOiIyNjAzOWNjZS00ODlkLTQwMDItODI5My01YjBjNTEzNGVhY2IiLCJ1bmlxdWVfbmFtZSI6Im5hdnlhQGRkb2JhbGlhbm91dGxvb2sub25taWNyb3NvZnQuY29tIiwidXBuIjoibmF2eWFAZGRvYmFsaWFub3V0bG9vay5vbm1pY3Jvc29mdC5jb20iLCJ1dGkiOiJ4Q3dmemhhLVAwV0pRT0x4Q0dnS0FBIiwidmVyIjoiMS4wIn0.cqmUVjfVbqWsxJLUI1Z4FRx1mNQAHP-L0F4EMN09r8FY9bIKeO-0q1eTdP11Nkj_k4BmtaZsTcK_mUygdMqEp9AfyVyA1HYvokcgGCW_Z6DMlVGqlIU4ssEkL9abgl1REHElPhpwBFFBBenOk9iHddD1GddTn6vJbKC3qAaNM5VarjSPu50bVvCrqKNvFixTb5bbdnSz-Qr6n6ACiEimiI1aNOPR2DeKUyWBPaQcU5EAK0ef5IsVJC1yaYDlAcUYIILMDLCD9ebjsy0t9pj_7lvjzUSrbMdSCCdzCqez_MSNxrk1Nu9AecugkBYp3UVUZOIyythVrj6-sVvLZKUutQ
```
## <a name="client-limitations"></a>İstemci sınırlamaları
Joker karakter yanıt URL'leri ile ortak istemcileri bir `id_token` OBO akışlar için. Ancak, gizli bir istemci yine de kullanmak **erişim** genel istemci bir joker karakter olsa bile örtük verme akışı edinilen belirteçleri yeniden yönlendirme URI'si kayıtlı.

## <a name="next-steps"></a>Sonraki adımlar
OAuth 2.0 protokolünü ve istemci kimlik bilgilerini kullanarak hizmet kimlik doğrulaması gerçekleştirmek için başka bir yöntem hakkında daha fazla bilgi edinin.
* [OAuth 2.0 istemci kimlik bilgileri verme Azure AD'de kullanarak hizmet kimlik doğrulama hizmetine](v1-oauth2-client-creds-grant-flow.md)
* [Azure AD'nin OAuth 2.0](v1-protocols-oauth-code.md)
