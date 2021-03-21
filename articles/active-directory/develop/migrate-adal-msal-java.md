---
title: ADAL to MSAL geçiş kılavuzu (MSAL4j) | Mavisi
titleSuffix: Microsoft identity platform
description: Azure Active Directory kimlik doğrulaması kitaplığı (ADAL) Java uygulamanızı Microsoft kimlik doğrulama kitaplığı 'na (MSAL) geçirmeyi öğrenin.
services: active-directory
author: sangonzal
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.tgt_pltfrm: Java
ms.workload: identity
ms.date: 11/04/2019
ms.author: sagonzal
ms.reviewer: nacanuma, twhitney
ms.custom: aaddev, devx-track-java
ms.openlocfilehash: 620f77655f8281919ba0831a7e53af8cd28bc5f2
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "99584002"
---
# <a name="adal-to-msal-migration-guide-for-java"></a>Java için ADAL MSAL geçiş kılavuzu

Bu makalede, Microsoft kimlik doğrulama kitaplığı 'nı (ADAL) Azure Active Directory kullanan bir uygulamayı Microsoft kimlik doğrulama kitaplığı 'nı (MSAL) kullanmak üzere geçirmek için gereken değişiklikler vurgulanmaktadır.

Hem Java için Microsoft kimlik doğrulama kitaplığı (MSAL4J) hem de Java için Azure AD kimlik doğrulama kitaplığı (ADAL4J), Azure AD varlıklarının kimliğini doğrulamak ve Azure AD 'de belirteçleri istemek için kullanılır. Bu aşamada, çoğu geliştirici Azure AD kimlik doğrulama kitaplığı (ADAL) kullanarak belirteçleri isteyerek Azure AD kimliklerinin (iş ve okul hesapları) kimliğini doğrulamak için Azure AD 'de geliştiriciler platformu (v 1.0) ile çalıştık.

MSAL aşağıdaki avantajları sunar:

- Daha yeni Microsoft Identity platformunu kullandığından, Azure AD Iş ve tüketici (B2C) aracılığıyla Azure AD kimlikleri, Microsoft hesapları ve sosyal ve yerel hesaplar gibi daha geniş bir dizi Microsoft kimliği için kimlik doğrulaması yapabilirsiniz.
- Kullanıcılarınız en iyi çoklu oturum açma deneyimini alacak.
- Uygulamanız artımlı onayı etkinleştirebilir ve Koşullu erişimin desteklenmesi daha kolay olabilir.

Java için MSAL, Microsoft Identity platformu ile kullanmanızı önerdiğimiz kimlik doğrulama kitaplığıdır. ADAL4J üzerinde yeni özellik uygulanmayacak. Öne çıkan tüm çabalar MSAL iyileştirmeye odaklanılmıştır.

## <a name="differences"></a>Farklılıklar

Geliştiriciler için Azure AD (v 1.0) uç noktası (ve ADAL4J) ile çalışıyorsanız, [Microsoft Identity platformu hakkında ne kadar farklı olduğunu](../azuread-dev/azure-ad-endpoint-comparison.md)okumak isteyebilirsiniz.

## <a name="scopes-not-resources"></a>Kapsam kaynakları değil

ADAL4J, for Java için MSAL for Java için belirteçleri edinme Java sınıfları için bir dizi MSAL, bir kapsamlar parametresi gerektirir. Bu parametre, istenen izinleri ve istenen kaynakları bildiren dizelerin bir listesidir. Örnek kapsamları görmek için [Microsoft Graph kapsamlarına](/graph/permissions-reference) bakın.

`/.default`UYGULAMALARıNıZı adal 'DAN msal 'e geçirmeye yardımcı olması için kapsam sonekini kaynağa ekleyebilirsiniz. Örneğin, kaynak değeri için `https://graph.microsoft.com` , eşdeğer kapsam değeri `https://graph.microsoft.com/.default` .  Kaynak URL biçiminde değilse, ancak formun kaynak KIMLIĞI ise `XXXXXXXX-XXXX-XXXX-XXXXXXXXXXXX` , kapsam değerini kullanmaya devam edebilirsiniz `XXXXXXXX-XXXX-XXXX-XXXXXXXXXXXX/.default` .

Farklı kapsam türleri hakkında daha fazla bilgi için [Microsoft Identity platformunda izinler ve onay](./v2-permissions-and-consent.md) ' i ve [v 1.0 belirteçleri 'ni kabul eden bir Web API 'si için kapsamları](./msal-v1-app-scopes.md) inceleyin.

## <a name="core-classes"></a>Çekirdek sınıflar

ADAL4J ' de, `AuthenticationContext` sınıfı güvenlik belirteci hizmeti (STS) veya yetkilendirme sunucusu ile bir yetkili aracılığıyla bağlantınızı temsil eder. Ancak, Java için MSAL istemci uygulamaları etrafında tasarlanmıştır. İki ayrı sınıf sağlar: `PublicClientApplication` ve `ConfidentialClientApplication` istemci uygulamalarını temsil eder.  İkinci olarak, `ConfidentialClientApplication` bir Daemon uygulaması için uygulama tanımlayıcısı gibi bir gizli anahtarı güvenli bir şekilde korumak için tasarlanan bir uygulamayı temsil eder.

Aşağıdaki tabloda, ADAL4J işlevlerinin Java işlevleri için yeni MSAL nasıl eşlendiğini gösterilmektedir:

| ADAL4J yöntemi| MSAL4J yöntemi|
|------|-------|
|acquireToken (dize kaynağı, ClientCredential kimlik bilgileri, AuthenticationCallback geri çağırma) | acquireToken (ClientCredentialParameters)|
|acquireToken (dize kaynağı, ClientAssertion onaylama, AuthenticationCallback geri çağırma)|acquireToken (ClientCredentialParameters)|
|acquireToken (dize kaynağı, AsymmetricKeyCredential kimlik bilgileri, AuthenticationCallback geri çağırma)|acquireToken (ClientCredentialParameters)|
|acquireToken (dize kaynağı, dize ClientID, dize Kullanıcı adı, dize parolası, AuthenticationCallback geri çağırma)| acquireToken (UsernamePasswordParameters)|
|acquireToken (dize kaynağı, dize ClientID, dize Kullanıcı adı, dize parolası = null, AuthenticationCallback geri çağırma)|acquireToken (IntegratedWindowsAuthenticationParameters)|
|acquireToken (dize kaynağı, Kullanıcı onaylama Kullanıcı onaylama, ClientCredential kimlik bilgileri, AuthenticationCallback geri çağırma)| acquireToken (OnBehalfOfParameters)|
|acquireTokenByAuthorizationCode () | acquireToken (AuthorizationCodeParameters) |
| acquireDeviceCode () ve acquireTokenByDeviceCode ()| acquireToken (DeviceCodeParameters)|
|acquireTokenByRefreshToken ()| Acquiretokensessizce (SilentParameters)|

## <a name="iaccount-instead-of-iuser"></a>IUser yerine IAccount

ADAL4J tarafından yönetilen kullanıcılar. Bir kullanıcı tek bir insan veya yazılım aracısını temsil ediyorsa, Microsoft Identity sisteminde bir veya daha fazla hesabı olabilir. Örneğin, bir kullanıcının birkaç Azure AD, Azure AD B2C veya Microsoft Kişisel hesabı olabilir.

Java için MSAL, arabirim aracılığıyla hesap kavramını tanımlar `IAccount` . Bu, ADAL4J ' den önemli bir değişiklik olmakla kalmaz, ancak aynı kullanıcının çeşitli hesaplara sahip olabileceğinden ve belki de farklı Azure AD dizinlerinde bile olduğu gerçeğini yakaladığı için bu iyi bir yoldur. Ana hesap bilgileri sağlandığı için Java için MSAL, Konuk senaryolarında daha iyi bilgiler sağlar.

## <a name="cache-persistence"></a>Önbellek kalıcılığı

ADAL4J belirteç önbelleği için desteğe sahip değildi.
Java için MSAL, mümkün olduğunda süresi geçen belirteçleri otomatik olarak yenileyerek ve mümkün olduğunda kullanıcının kimlik bilgilerini sağlaması için gereksiz istemler yaparak belirteç yaşam sürelerini yönetmeyi kolaylaştırmak için bir [belirteç önbelleği](msal-acquire-cache-tokens.md) ekler.

## <a name="common-authority"></a>Ortak yetkili

V 1.0 'da, yetkili kullanırsanız, `https://login.microsoftonline.com/common` Kullanıcılar herhangi bir Azure Active Directory (AAD) hesabıyla oturum açabilirler (herhangi bir kuruluş için).

`https://login.microsoftonline.com/common`Yetkilisi 'yi v 2.0 'da kullanıyorsanız, kullanıcılar herhangi BIR AAD organizasyonu ile veya hatta bir Microsoft Kişisel hesabı (MSA) ile oturum açabilirler. Java için MSAL içinde herhangi bir AAD hesabıyla oturum açmayı kısıtlamak istiyorsanız, `https://login.microsoftonline.com/organizations` yetkilisini (ADAL4J ile aynı davranış olan) kullanın. Bir yetkili belirtmek için, `authority` sınıfınızı oluştururken [Publicclientapplication. Builder](https://javadoc.io/doc/com.microsoft.azure/msal4j/1.0.0/com/microsoft/aad/msal4j/PublicClientApplication.Builder.html) yönteminde parametresini ayarlayın `PublicClientApplication` .

## <a name="v10-and-v20-tokens"></a>v 1.0 ve v 2.0 belirteçleri

V 1.0 uç noktası (ADAL tarafından kullanılan) yalnızca v 1.0 belirteçlerini yayar.

V 2.0 uç noktası (MSAL tarafından kullanılan), v 1.0 ve v 2.0 belirteçlerini yayabilir. Web API 'sinin uygulama bildiriminin bir özelliği, geliştiricilerin hangi belirteç sürümünün kabul edildiğini seçmesini sağlar. Bkz `accessTokenAcceptedVersion` . [uygulama bildirimi](./reference-app-manifest.md) başvuru belgeleri.

V 1.0 ve v 2.0 belirteçleri hakkında daha fazla bilgi için bkz. [Azure Active Directory erişim belirteçleri](./access-tokens.md).

## <a name="adal-to-msal-migration"></a>ADAL to MSAL geçişi

ADAL4J ' de, yenileme belirteçleri gösterilir ve izin verilen geliştiricilerin onları önbelleğe alma izni vardır. Daha sonra `AcquireTokenByRefreshToken()` , Kullanıcı artık bağlı olmadığında Kullanıcı adına panoları yenileyen uzun süreli Hizmetleri uygulama gibi çözümleri etkinleştirmek için kullanırlar.

Java için MSAL, güvenlik nedenleriyle yenileme belirteçleri sunmaz. Bunun yerine, MSAL belirteçleri sizin için yenilemeyi işler.

Java için MSAL, ADAL4j ile elde ettiğiniz yenileme belirteçlerini ClientApplication: [Acquiretoken (RefreshTokenParameters)](https://javadoc.io/static/com.microsoft.azure/msal4j/1.0.0/com/microsoft/aad/msal4j/PublicClientApplication.html#acquireToken-com.microsoft.aad.msal4j.RefreshTokenParameters-)öğesine geçirmenize olanak tanıyan bir API içerir. Bu yöntemde, daha önce kullanılan yenileme belirtecini istediğiniz kapsamlar (kaynaklar) ile birlikte sağlayabilirsiniz. Yenileme belirteci yeni bir tane için değiştirilir ve uygulamanız tarafından kullanılmak üzere önbelleğe alınır.

Aşağıdaki kod parçacığı, bir gizli istemci uygulamasındaki bazı geçiş kodlarını gösterir:

```java
String rt = GetCachedRefreshTokenForSignedInUser(); // Get refresh token from where you have them stored
Set<String> scopes = Collections.singleton("SCOPE_FOR_REFRESH_TOKEN");

RefreshTokenParameters parameters = RefreshTokenParameters.builder(scopes, rt).build();

PublicClientApplication app = PublicClientApplication.builder(CLIENT_ID) // ClientId for your application
                .authority(AUTHORITY)  //plug in your authority
                .build();

IAuthenticationResult result = app.acquireToken(parameters);
```

, `IAuthenticationResult` Yeni yenileme belirteciniz önbellekte depolandığından, bir erişim belirteci ve kimlik belirteci döndürür.
Uygulama artık bir IAccount da içerecektir:

```java
Set<IAccount> accounts =  app.getAccounts().join();
```

Şu anda önbellekte olan belirteçleri kullanmak için şunu çağırın:

```java
SilentParameters parameters = SilentParameters.builder(scope, accounts.iterator().next()).build();
IAuthenticationResult result = app.acquireToken(parameters);
```
