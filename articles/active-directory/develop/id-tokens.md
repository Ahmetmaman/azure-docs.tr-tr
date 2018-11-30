---
title: Azure Active Directory kimliği belirteç başvurusu | Microsoft Docs
description: Azure AD tarafından v1.0 ve v2.0 uç yayılan id_tokens kullanmayı öğrenin.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/05/2018
ms.author: celested
ms.reviewer: hirsin
ms.custom: aaddev
ms.openlocfilehash: ab2c0f671eaf6147baad24b426c4a527f07e136f
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/27/2018
ms.locfileid: "52422414"
---
# <a name="id-tokens"></a>Kimlik belirteçleri

`id_tokens` istemci uygulama bir parçası olarak gönderilen bir [Openıd Connect](v1-protocols-openid-connect-code.md) akış. Bunlar yanı sıra veya bir erişim belirteci yerine gönderilebilir ve kullanıcının kimliğini doğrulamak için istemci tarafından kullanılır. 

## <a name="using-the-idtoken"></a>İd_token kullanma

Bir kullanıcı olup olmadığını kimin olabilir ve bunlar hakkında başka yararlı bilgiler almak talep etmek - yerine yetkilendirme için kullanılmamalıdır doğrulamak için kimlik belirteçlerini kullanılmalıdır bir [erişim belirteci](access-tokens.md). Sağladığı talep UX için bir veritabanı anahtarlama ve istemci uygulamaya erişim sağlayarak uygulamanızın içinde kullanılabilir. 

## <a name="claims-in-an-idtoken"></a>Bir id_token talepleri

`id_tokens` Microsoft kimlik olan [Jwt'ler](https://tools.ietf.org/html/rfc7519), yani bunlar bir üst bilgi, yükü ve imza bölümü oluşur. Üst bilgisi ve yük yükü, istemci tarafından istenen kullanıcı hakkındaki bilgileri içerirken belirteci özgünlüğünü doğrulamak için kullanabilirsiniz. Belirtilenler burada listelenen tüm talepler v1.0 hem v2.0 belirteçleri görüntülenir.

### <a name="v10"></a>v1.0

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6IjdfWnVmMXR2a3dMeFlhSFMzcTZsVWpVWUlHdyIsImtpZCI6IjdfWnVmMXR2a3dMeFlhSFMzcTZsVWpVWUlHdyJ9.eyJhdWQiOiJiMTRhNzUwNS05NmU5LTQ5MjctOTFlOC0wNjAxZDBmYzljYWEiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC9mYTE1ZDY5Mi1lOWM3LTQ0NjAtYTc0My0yOWYyOTU2ZmQ0MjkvIiwiaWF0IjoxNTM2Mjc1MTI0LCJuYmYiOjE1MzYyNzUxMjQsImV4cCI6MTUzNjI3OTAyNCwiYWlvIjoiQVhRQWkvOElBQUFBcXhzdUIrUjREMnJGUXFPRVRPNFlkWGJMRDlrWjh4ZlhhZGVBTTBRMk5rTlQ1aXpmZzN1d2JXU1hodVNTajZVVDVoeTJENldxQXBCNWpLQTZaZ1o5ay9TVTI3dVY5Y2V0WGZMT3RwTnR0Z2s1RGNCdGsrTExzdHovSmcrZ1lSbXY5YlVVNFhscGhUYzZDODZKbWoxRkN3PT0iLCJhbXIiOlsicnNhIl0sImVtYWlsIjoiYWJlbGlAbWljcm9zb2Z0LmNvbSIsImZhbWlseV9uYW1lIjoiTGluY29sbiIsImdpdmVuX25hbWUiOiJBYmUiLCJpZHAiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83MmY5ODhiZi04NmYxLTQxYWYtOTFhYi0yZDdjZDAxMWRiNDcvIiwiaXBhZGRyIjoiMTMxLjEwNy4yMjIuMjIiLCJuYW1lIjoiYWJlbGkiLCJub25jZSI6IjEyMzUyMyIsIm9pZCI6IjA1ODMzYjZiLWFhMWQtNDJkNC05ZWMwLTFiMmJiOTE5NDQzOCIsInJoIjoiSSIsInN1YiI6IjVfSjlyU3NzOC1qdnRfSWN1NnVlUk5MOHhYYjhMRjRGc2dfS29vQzJSSlEiLCJ0aWQiOiJmYTE1ZDY5Mi1lOWM3LTQ0NjAtYTc0My0yOWYyOTU2ZmQ0MjkiLCJ1bmlxdWVfbmFtZSI6IkFiZUxpQG1pY3Jvc29mdC5jb20iLCJ1dGkiOiJMeGVfNDZHcVRrT3BHU2ZUbG40RUFBIiwidmVyIjoiMS4wIn0=.UJQrCA6qn2bXq57qzGX_-D3HcPHqBMOKDPx4su1yKRLNErVD8xkxJLNLVRdASHqEcpyDctbdHccu6DPpkq5f0ibcaQFhejQNcABidJCTz0Bb2AbdUCTqAzdt9pdgQvMBnVH1xk3SCM6d4BbT4BkLLj10ZLasX7vRknaSjE_C5DI7Fg4WrZPwOhII1dB0HEZ_qpNaYXEiy-o94UJ94zCr07GgrqMsfYQqFR7kn-mn68AjvLcgwSfZvyR_yIK75S_K37vC3QryQ7cNoafDe9upql_6pB2ybMVlgWPs_DmbJ8g0om-sPlwyn74Cc1tW3ze-Xptw_2uVdPgWyqfuWAfq6Q
```

Bu v1.0 örnek belirtecinde görüntülemek [jwt.ms](https://jwt.ms/#id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6IjdfWnVmMXR2a3dMeFlhSFMzcTZsVWpVWUlHdyIsImtpZCI6IjdfWnVmMXR2a3dMeFlhSFMzcTZsVWpVWUlHdyJ9.eyJhdWQiOiJiMTRhNzUwNS05NmU5LTQ5MjctOTFlOC0wNjAxZDBmYzljYWEiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC9mYTE1ZDY5Mi1lOWM3LTQ0NjAtYTc0My0yOWYyOTU2ZmQ0MjkvIiwiaWF0IjoxNTM2Mjc1MTI0LCJuYmYiOjE1MzYyNzUxMjQsImV4cCI6MTUzNjI3OTAyNCwiYWlvIjoiQVhRQWkvOElBQUFBcXhzdUIrUjREMnJGUXFPRVRPNFlkWGJMRDlrWjh4ZlhhZGVBTTBRMk5rTlQ1aXpmZzN1d2JXU1hodVNTajZVVDVoeTJENldxQXBCNWpLQTZaZ1o5ay9TVTI3dVY5Y2V0WGZMT3RwTnR0Z2s1RGNCdGsrTExzdHovSmcrZ1lSbXY5YlVVNFhscGhUYzZDODZKbWoxRkN3PT0iLCJhbXIiOlsicnNhIl0sImVtYWlsIjoiYWJlbGlAbWljcm9zb2Z0LmNvbSIsImZhbWlseV9uYW1lIjoiTGluY29sbiIsImdpdmVuX25hbWUiOiJBYmUiLCJpZHAiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83MmY5ODhiZi04NmYxLTQxYWYtOTFhYi0yZDdjZDAxMWRiNDcvIiwiaXBhZGRyIjoiMTMxLjEwNy4yMjIuMjIiLCJuYW1lIjoiYWJlbGkiLCJub25jZSI6IjEyMzUyMyIsIm9pZCI6IjA1ODMzYjZiLWFhMWQtNDJkNC05ZWMwLTFiMmJiOTE5NDQzOCIsInJoIjoiSSIsInN1YiI6IjVfSjlyU3NzOC1qdnRfSWN1NnVlUk5MOHhYYjhMRjRGc2dfS29vQzJSSlEiLCJ0aWQiOiJmYTE1ZDY5Mi1lOWM3LTQ0NjAtYTc0My0yOWYyOTU2ZmQ0MjkiLCJ1bmlxdWVfbmFtZSI6IkFiZUxpQG1pY3Jvc29mdC5jb20iLCJ1dGkiOiJMeGVfNDZHcVRrT3BHU2ZUbG40RUFBIiwidmVyIjoiMS4wIn0=.UJQrCA6qn2bXq57qzGX_-D3HcPHqBMOKDPx4su1yKRLNErVD8xkxJLNLVRdASHqEcpyDctbdHccu6DPpkq5f0ibcaQFhejQNcABidJCTz0Bb2AbdUCTqAzdt9pdgQvMBnVH1xk3SCM6d4BbT4BkLLj10ZLasX7vRknaSjE_C5DI7Fg4WrZPwOhII1dB0HEZ_qpNaYXEiy-o94UJ94zCr07GgrqMsfYQqFR7kn-mn68AjvLcgwSfZvyR_yIK75S_K37vC3QryQ7cNoafDe9upql_6pB2ybMVlgWPs_DmbJ8g0om-sPlwyn74Cc1tW3ze-Xptw_2uVdPgWyqfuWAfq6Q).

### <a name="v20"></a>v2.0

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6IjFMVE16YWtpaGlSbGFfOHoyQkVKVlhlV01xbyJ9.eyJ2ZXIiOiIyLjAiLCJpc3MiOiJodHRwczovL2xvZ2luLm1pY3Jvc29mdG9ubGluZS5jb20vOTE4ODA0MGQtNmM2Ny00YzViLWIxMTItMzZhMzA0YjY2ZGFkL3YyLjAiLCJzdWIiOiJBQUFBQUFBQUFBQUFBQUFBQUFBQUFJa3pxRlZyU2FTYUZIeTc4MmJidGFRIiwiYXVkIjoiNmNiMDQwMTgtYTNmNS00NmE3LWI5OTUtOTQwYzc4ZjVhZWYzIiwiZXhwIjoxNTM2MzYxNDExLCJpYXQiOjE1MzYyNzQ3MTEsIm5iZiI6MTUzNjI3NDcxMSwibmFtZSI6IkFiZSBMaW5jb2xuIiwicHJlZmVycmVkX3VzZXJuYW1lIjoiQWJlTGlAbWljcm9zb2Z0LmNvbSIsIm9pZCI6IjAwMDAwMDAwLTAwMDAtMDAwMC02NmYzLTMzMzJlY2E3ZWE4MSIsInRpZCI6IjMzMzgwNDBkLTZjNjctNGM1Yi1iMTEyLTM2YTMwNGI2NmRhZCIsIm5vbmNlIjoiMTIzNTIzIiwiYWlvIjoiRGYyVVZYTDFpeCFsTUNXTVNPSkJjRmF0emNHZnZGR2hqS3Y4cTVnMHg3MzJkUjVNQjVCaXN2R1FPN1lXQnlqZDhpUURMcSFlR2JJRGFreXA1bW5PcmNkcUhlWVNubHRlcFFtUnA2QUlaOGpZIn0=.1AFWW-Ck5nROwSlltm7GzZvDwUkqvhSQpm55TQsmVo9Y59cLhRXpvB8n-55HCr9Z6G_31_UbeUkoz612I2j_Sm9FFShSDDjoaLQr54CreGIJvjtmS3EkK9a7SJBbcpL1MpUtlfygow39tFjY7EVNW9plWUvRrTgVk7lYLprvfzw-CIqw3gHC-T7IK_m_xkr08INERBtaecwhTeN4chPC4W3jdmw_lIxzC48YoQ0dB1L9-ImX98Egypfrlbm0IBL5spFzL6JDZIRRJOu8vecJvj1mq-IUhGt0MacxX8jdxYLP-KUu2d9MbNKpCKJuZ7p8gwTL5B7NlUdh_dmSviPWrw
```

Bu v2.0 örnek belirtecinde görüntülemek [jwt.ms](https://jwt.ms/#id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6IjFMVE16YWtpaGlSbGFfOHoyQkVKVlhlV01xbyJ9.eyJ2ZXIiOiIyLjAiLCJpc3MiOiJodHRwczovL2xvZ2luLm1pY3Jvc29mdG9ubGluZS5jb20vOTE4ODA0MGQtNmM2Ny00YzViLWIxMTItMzZhMzA0YjY2ZGFkL3YyLjAiLCJzdWIiOiJBQUFBQUFBQUFBQUFBQUFBQUFBQUFJa3pxRlZyU2FTYUZIeTc4MmJidGFRIiwiYXVkIjoiNmNiMDQwMTgtYTNmNS00NmE3LWI5OTUtOTQwYzc4ZjVhZWYzIiwiZXhwIjoxNTM2MzYxNDExLCJpYXQiOjE1MzYyNzQ3MTEsIm5iZiI6MTUzNjI3NDcxMSwibmFtZSI6IkFiZSBMaW5jb2xuIiwicHJlZmVycmVkX3VzZXJuYW1lIjoiQWJlTGlAbWljcm9zb2Z0LmNvbSIsIm9pZCI6IjAwMDAwMDAwLTAwMDAtMDAwMC02NmYzLTMzMzJlY2E3ZWE4MSIsInRpZCI6IjMzMzgwNDBkLTZjNjctNGM1Yi1iMTEyLTM2YTMwNGI2NmRhZCIsIm5vbmNlIjoiMTIzNTIzIiwiYWlvIjoiRGYyVVZYTDFpeCFsTUNXTVNPSkJjRmF0emNHZnZGR2hqS3Y4cTVnMHg3MzJkUjVNQjVCaXN2R1FPN1lXQnlqZDhpUURMcSFlR2JJRGFreXA1bW5PcmNkcUhlWVNubHRlcFFtUnA2QUlaOGpZIn0=.1AFWW-Ck5nROwSlltm7GzZvDwUkqvhSQpm55TQsmVo9Y59cLhRXpvB8n-55HCr9Z6G_31_UbeUkoz612I2j_Sm9FFShSDDjoaLQr54CreGIJvjtmS3EkK9a7SJBbcpL1MpUtlfygow39tFjY7EVNW9plWUvRrTgVk7lYLprvfzw-CIqw3gHC-T7IK_m_xkr08INERBtaecwhTeN4chPC4W3jdmw_lIxzC48YoQ0dB1L9-ImX98Egypfrlbm0IBL5spFzL6JDZIRRJOu8vecJvj1mq-IUhGt0MacxX8jdxYLP-KUu2d9MbNKpCKJuZ7p8gwTL5B7NlUdh_dmSviPWrw).

### <a name="header-claims"></a>Üst bilgi talep

|İste | Biçimlendir | Açıklama |
|-----|--------|-------------|
|`typ` | Dize - her zaman "JWT" | JWT belirteci olduğunu gösterir.|
|`alg` | Dize | Belirteç imzalamak için kullanılan algoritmayı belirtir. Örnek: "RS256" |
|`kid` | Dize | Bu belirteç imzalamak için kullanılan ortak anahtar için parmak izi. Hem v1.0 hem de v2.0 yayılan `id_tokens`. |
|`x5t` | Dize | Aynı (kullanın ve değerini) olarak `kid`. Ancak, bu yalnızca v1.0 yayılan eski bir talep, `id_tokens` uyumluluk amacıyla. |

### <a name="payload-claims"></a>Yükü talepleri

|İste | Biçimlendir | Açıklama |
|-----|--------|-------------|
|`aud` |  Dize, bir uygulama kimliği URI'si | Amaçladığınız alıcının belirtecin tanımlar. İçinde `id_tokens`, uygulamanızın uygulama kimliği, Azure portalında uygulamanıza atanan İzleyici olduğu. Uygulamanız bu değeri doğrulamak ve değer eşleşmiyorsa belirteci reddetme. |
|`iss` |  Dize, bir STS URI | Oluşturur ve belirteci ve kullanıcı kimlik doğrulamasının yapıldığı Azure AD kiracısı döndürür güvenlik belirteci hizmeti (STS) tanımlar. Belirteç v2.0 uç noktası tarafından verildiyse, URI içinde sona erecek `/v2.0`.  Kullanıcının bir Microsoft hesabı tüketici kullanıcıdan olduğunu gösteren GUID `9188040d-6c67-4c5b-b112-36a304b66dad`. Uygulamanız varsa, uygulamaya oturum açabilirsiniz kiracılar kümesini sınırlandırmak için talep GUID kısmını kullanmanız gerekir. |
|`iat` |  int, UNIX zaman damgası | "Konumunda verilen" kimlik doğrulaması için bu belirteci gerçekleştiği gösterir.  |
|`idp`|Dize, genellikle bir STS URI | Belirtecin öznesinin kimliğini doğrulayan kimlik sağlayıcısını kaydeder. -Veren olarak aynı kiracıda değil kullanıcı hesabının konukların sürece örneği için bu değer veren talep değeri için aynıdır. Talep mevcut değilse, değeri anlamına `iss` bunun yerine kullanılabilir.  Kişisel hesapları orgnizational bağlamda (bir Azure AD kiracısına davet örneği için bir kişisel hesap) kullanılan `idp` talep 'live.com' veya Microsoft hesabı kiracısının içeren bir STS URI olabilir `9188040d-6c67-4c5b-b112-36a304b66dad`. |
|`nbf` |  int, UNIX zaman damgası | "Nbf" (önce değil) talep işlem önüne JWT değil kabul edilmesi gereken zamanı tanımlar.|
|`exp` |  int, UNIX zaman damgası | "Exp" (süre) talep veya daha sonra JWT gerekir işleme için kabul sona erme zamanı tanımlar.  Kaynak belirteci de - bu süreden önce örneğin kimlik değişikliği gerekli değildir veya belirteç iptali algılandı reddedebilir olduğunu unutmayın. |
| `c_hash`| Dize |Yalnızca bir OAuth 2.0 yetkilendirme kodu ile kimlik belirteci verildiğinde kod karma kimlik belirteçleri bulunur. Bir yetkilendirme kodu özgünlüğünü doğrulamak için kullanılabilir. Bu doğrulama gerçekleştirme hakkında daha fazla ayrıntı için bkz. [Openıd Connect belirtimi](https://openid.net/specs/openid-connect-core-1_0.html). |
|`at_hash`| Dize |Belirteç karması Kimliğinde bulunan erişim yalnızca zaman kimlik belirteci bir OAuth 2.0 erişim belirteci ile verilen belirteçler. Bir erişim belirteci özgünlüğünü doğrulamak için kullanılabilir. Bu doğrulama gerçekleştirme hakkında daha fazla ayrıntı için bkz. [Openıd Connect belirtimi](https://openid.net/specs/openid-connect-core-1_0.html). |
|`aio` | Donuk dize | Belirteci yeniden kullanım için verileri kaydetmek üzere Azure AD tarafından kullanılan bir iç talep. Yoksayılacak.|
|`preferred_username` | Dize | Kullanıcıyı temsil eden birincil kullanıcı adı. Bir e-posta adresi, telefon numarası veya belirli bir biçimdeki olmadan genel bir kullanıcı adı olabilir. Değerini değişebilir ve zaman içinde değişebilir. Değişebilir olduğundan, bu değer yetkilendirme kararları vermek için kullanılmamalıdır. `profile` Kapsamı, bu talebi için gereklidir.|
|`email` | Dize | `email` Talep bir e-posta adresine sahip Konuk hesapları için varsayılan olarak mevcut.  Uygulamanız için yönetilen kullanıcılar (Bu aynı kiracıda kaynak olarak) e-posta talebi isteyebilir kullanarak `email` [isteğe bağlı bir talep](active-directory-optional-claims.md).  V2.0 uç noktası ile uygulamanızı da isteğinde bulunabilirsiniz `email` Openıd Connect kapsam - isteğe bağlı bir talep ve kapsam talebi almak için isteği gerekmez.  E-posta talebi yalnızca kullanıcının profil bilgilerini adreslenebilir posta destekler. |
|`name` | Dize | `name` Talep belirtecinin konu tanımlayan insanlar tarafından okunabilen bir değer sağlar. Değerin benzersiz olması garanti edilmez, değişebilir ve yalnızca görüntüleme amaçları için kullanılmak üzere tasarlanmıştır. `profile` Kapsamı, bu talebi için gereklidir. |
|`nonce`| Dize | Nonce özgün dahil parametreyle eşleşen / IDP isteğine yetki. Eşleşmiyorsa, uygulamanızın belirteci reddetme. |
|`oid` | Dize, bir GUID | Microsoft kimlik sistemi, bu durumda, bir kullanıcı hesabı, bir nesne değişmez tanımlayıcısı. Bu kimliği kullanıcı uygulamalar arasında benzersiz olarak tanımlayan - aynı kullanıcı imzalama iki farklı uygulama aynı değeri alacak `oid` talep. Microsoft Graph, bu kimliği olarak döndüreceği `id` özelliği için belirtilen kullanıcı hesabı. Çünkü `oid` kullanıcılar ilişkilendirmek birden fazla uygulama sağlayan `profile` kapsamı, bu talebi için gereklidir. Tek bir kullanıcı birden fazla Kiracı varsa, kullanıcının her Kiracı farklı nesne Kimliğinde içereceğini unutmayın - kullanıcının her hesap aynı kimlik bilgileriyle oturum açtığı olsa bile farklı hesaplar kabul edilir. |
|`rh` | Donuk dize |Belirteçleri düzeltin için Azure tarafından kullanılan bir iç talep. Yoksayılacak. |
|`sub` | Dize, bir GUID | Sorumlu olduğu hakkında bir uygulamanın kullanıcı gibi bilgileri belirteci onaylar. Bu değer sabittir ve yeniden atandı yeniden veya değiştirilemez. Konu ikili bir tanımlayıcıdır - benzersiz bir uygulama belirli kimliğe Bu nedenle, tek bir kullanıcı iki farklı uygulamalara iki farklı istemci kimliklerini kullanarak oturum açtığında, bu uygulamaların konu talebi için iki farklı değerler alır. Bu olabilir veya mimari ve gizlilik gereksinimlerinize bağlı olarak gerekli değildir. |
|`tid` | Dize, bir GUID | Kullanıcı dandır Azure AD kiracısı temsil eden bir GUID. İş ve Okul hesapları için kullanıcının ait olduğu kuruluş sabit Kiracı kimliği bir GUID'dir. Kişisel hesapları için değerdir `9188040d-6c67-4c5b-b112-36a304b66dad`. `profile` Kapsamı, bu talebi için gereklidir. |
|`unique_name` | Dize | Belirtecin konusunu tanımlayan ve okunabilir bir değer sunar. Bu değer, bir kiracıda benzersiz olması garanti edilmez ve yalnızca görüntüleme amaçları için kullanılmalıdır. V1.0 yalnızca verilen `id_tokens`. |
|`uti` | Donuk dize | Belirteçleri düzeltin için Azure tarafından kullanılan bir iç talep. Yoksayılacak. |
|`ver` | Dize, 1.0 veya 2.0 | İd_token sürümünü gösterir. |

## <a name="validating-an-idtoken"></a>Bir id_token doğrulanıyor

Doğrulama bir `id_token` ilk adımı, çok benzer [bir erişim belirteci doğrulanırken](access-tokens.md#validating-tokens) - istemcinizi doğru veren belirteç geri gönderildiğini doğrulamak ve bu kurcalanmadığı. Çünkü `id_tokens` her zaman bir JWT, bu belirteçleri doğrulamak için birçok mevcut - bunlar yerine bunu yapmayı birini kullanmanızı öneririz. 

El ile belirteci doğrulamak için adımlar ayrıntılara bakın [bir erişim belirteci doğrulanırken](access-tokens.md#validating-tokens). Belirteç imza doğruladıktan sonra aşağıdaki talep (bunlar da belirteci doğrulaması kitaplığınızı yapılabilir) id_token de doğrulanmalıdır:

* Zaman damgası: `iat`, `nbf`, ve `exp` zaman damgaları tüm kalan önce veya sonra uygun olarak geçerli saati. 
* Hedef kitle: `aud` talep, uygulamanız için uygulama kimliği eşleşmelidir.
* Nonce: `nonce` talep yükünde içine geçirilen nonce parametresi eşleşmelidir / ilk istek sırasında son noktanın yetkilendirilmesi.

## <a name="next-steps"></a>Sonraki adımlar

* Hakkında bilgi edinin [Azure AD erişim belirteci](access-tokens.md)
