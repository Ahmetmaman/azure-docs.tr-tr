---
title: Yükseltme Bing yazım denetimi API'si v5 için v7
titlesuffix: Azure Cognitive Services
description: Sürüm 7 kullanılacak güncelleştirmeye gerek duyduğunuz uygulamanızın parçalarını tanıtır.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-spell-check
ms.topic: conceptual
ms.date: 02/20/2019
ms.author: scottwhi
ms.openlocfilehash: b19681fd1ad70985bccd375931f5d5801f153aa8
ms.sourcegitcommit: 24906eb0a6621dfa470cb052a800c4d4fae02787
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2019
ms.locfileid: "56886358"
---
# <a name="spell-check-api-upgrade-guide"></a>Yazım denetimi API'si Yükseltme Kılavuzu

Bu Yükseltme Kılavuzu, sürüm 5 ve Bing yazım denetimi API'si 7 sürümü arasındaki değişiklikleri tanımlar. Sürüm 7 kullanılacak güncelleştirmeye gerek duyduğunuz uygulamanızın parçalarını tanımlamanıza yardımcı olması için bu kılavuzu kullanın.

## <a name="breaking-changes"></a>Yeni değişiklikler

### <a name="endpoints"></a>Uç Noktalar

- Uç noktanın sürüm numarası için v7 v5 değiştirildi. Örneğin, `https://api.cognitive.microsoft.com/bing/v7.0/spellcheck`.

### <a name="error-response-objects-and-error-codes"></a>Hata yanıtı nesneleri ve hata kodları

- Tüm başarısız istekler şimdi içermelidir bir `ErrorResponse` yanıt gövdesinde bir nesne.

- Aşağıdaki alanları eklenen `Error` nesne.  
  - `subCode`&mdash;Hata kodu ayrık demetlerin içine mümkünse bölümleri
  - `moreDetails`&mdash;İçinde açıklanan hata hakkında ek bilgi `message` alan
   

- V5 hata kodları ile aşağıdaki olası yerine `code` ve `subCode` değerleri.  
  
|Kod|Alt|Açıklama
|-|-|-
|ServerError|UnexpectedError<br/>ResourceError<br/>Uygulanmadı|Alt koşullardan herhangi biri gerçekleştiğinde Bing ServerError döndürür. HTTP durum kodunu 500 ise yanıt bu hataları içeriyor.
|InvalidRequest|ParameterMissing<br/>ParameterInvalidValue<br/>HttpNotAllowed<br/>Engellendi|Her isteğin herhangi bir bölümü geçerli değil Bing InvalidRequest döndürür. Örneğin, bir gerekli parametre eksik veya bir parametre değeri geçerli değil.<br/><br/>Hata ParameterMissing veya ParameterInvalidValue ise, HTTP durum kodu 400 ' dir.<br/><br/>Hata HttpNotAllowed ise, HTTP durumu 410 kod.
|RateLimitExceeded||/ Saniye (QPS) sorguları veya sorgu başına aylık (QPM) kota aştığında Bing RateLimitExceeded döndürür.<br/><br/>Bing QPM aşılırsa QPS ve 403 aşıldı HTTP durum kodu 429 döndürür.
|InvalidAuthorization|AuthorizationMissing<br/>AuthorizationRedundancy|Bing çağıran doğrulandığında Bing InvalidAuthorization döndürür. Örneğin, `Ocp-Apim-Subscription-Key` üstbilgisi eksik veya abonelik anahtarı geçerli değil.<br/><br/>Birden fazla kimlik doğrulama yöntemi belirtmek, yedeklilik meydana gelir.<br/><br/>Hata InvalidAuthorization ise, HTTP durum kodunu 401 ' dir.
|InsufficientAuthorization|AuthorizationDisabled<br/>AuthorizationExpired|Çağıran kaynağa erişmek için izinlere sahip olmadığında Bing InsufficientAuthorization döndürür. Bu abonelik anahtarını devre dışı bırakıldı veya süresi ortaya çıkabilir. <br/><br/>Hata InsufficientAuthorization, HTTP durum kodu 403 ise.

- Aşağıdaki yeni kodları için önceki hata kodlarını eşler. Bir bağımlılık v5 hata kodlarıyla ilgili zamandaki, kodunuzu buna göre güncelleştirin.  
  
|Sürüm 5 kodu|Sürüm 7 code.subCode
|-|-
|RequestParameterMissing|InvalidRequest.ParameterMissing
RequestParameterInvalidValue|InvalidRequest.ParameterInvalidValue
ResourceAccessDenied|InsufficientAuthorization
ExceededVolume|RateLimitExceeded
ExceededQpsLimit|RateLimitExceeded
Devre dışı|InsufficientAuthorization.AuthorizationDisabled
UnexpectedError|ServerError.UnexpectedError
DataSourceErrors|ServerError.ResourceError
AuthorizationMissing|InvalidAuthorization.AuthorizationMissing
HttpNotAllowed|InvalidRequest.HttpNotAllowed
UserAgentMissing|InvalidRequest.ParameterMissing
Uygulanmadı|ServerError.NotImplemented
InvalidAuthorization|InvalidAuthorization
InvalidAuthorizationMethod|InvalidAuthorization
MultipleAuthorizationMethod|InvalidAuthorization.AuthorizationRedundancy
ExpiredAuthorizationToken|InsufficientAuthorization.AuthorizationExpired
InsufficientScope|InsufficientAuthorization
Engellendi|InvalidRequest.Blocked

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Kullanım ve görüntüleme gereksinimleri](./UseAndDisplayRequirements.md)
