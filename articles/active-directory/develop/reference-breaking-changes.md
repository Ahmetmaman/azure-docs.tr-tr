---
title: Azure Active Directory bozucu değişiklikler başvurusu | Microsoft Docs
description: Uygulamanızı etkileyebilecek Azure AD'ye protokolleri için yapılan değişiklikler hakkında bilgi edinin.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 68517c83-1279-4cc7-a7c1-c7ccc3dbe146
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/02/2018
ms.author: celested
ms.reviewer: hirsin
ms.custom: aaddev
ms.openlocfilehash: 3da99506d50ec12140c188cd86cde2556be4a308
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2019
ms.locfileid: "55099150"
---
# <a name="whats-new-for-authentication"></a>Kimlik doğrulaması için yenilikler nelerdir? 

>Bu sayfaya güncelleştirmeler hakkında bildirim alın. Eklemeniz yeterlidir [bu URL'yi](https://docs.microsoft.com/api/search/rss?search=%22whats%20new%20for%20authentication%22&locale=en-us) Okuyucu için RSS akışı.

Kimlik doğrulama sistemi değiştirir ve güvenlik ve uyumluluk standartları geliştirmek için sürekli olarak özellikler ekler. İle en son gelişmeleri güncel kalmak için bu makalede, aşağıdaki ayrıntılar hakkında bilgi sağlar:

- En yeni özellikler
- Bilinen sorunlar
- Protokol değişiklikleri
- Kullanım dışı işlev

> [!TIP] 
> Bu sayfayı düzenli olarak güncelleştirilen, sık sık ziyaret edin. Aksi belirtilmediği sürece, bu değişiklikler yalnızca yeni kayıtlı uygulamalar için yerinde yerleştirilir.  

## <a name="upcoming-changes"></a>Yaklaşan değişiklikleri

Hiçbiri şu anda zamanlanmış. 

## <a name="october-2018"></a>Ekim 2018

### <a name="authorization-codes-can-no-longer-be-reused"></a>Yetkilendirme kodları yeniden artık kullanılabilir

**Geçerlilik tarihi**: 15 Kasım 2018

**Etkilenen uç noktaları**: V1.0 hem v2.0

**Etkilenen Protokolü**: [Kod akışı](v2-oauth2-auth-code-flow.md)

15 Kasım 2018'de başlayarak, Azure AD uygulamaları için daha önce kullanılan kimlik doğrulama kodlarını kabul durdurur. Bu güvenlik değişiklik v1 ve v2 Uç noktalara zorlanmasını sağlar ve Azure AD OAuth belirtimi ayarlarına uygun olarak çıkarmak yardımcı olur.

Uygulamanız için birden fazla kaynak belirteçlerini almak için yetkilendirme kodları yeniden kullanır, bir yenileme belirteci almak için kodu kullanın ve ardından diğer kaynaklar için ek belirteçlerini almak için yenileme belirtecini kullanmak öneririz. Yetkilendirme kodları yalnızca bir kez kullanılabilir, ancak yenileme belirteçleri birden fazla kaynak arasında birden çok kez kullanılabilir. Kimlik doğrulaması kodu OAuth kod akışı sırasında yeniden başlatmayı deneyen herhangi yeni bir uygulama bir invalid_grant hata alırsınız.

Yenileme belirteçleri hakkında daha fazla bilgi için bkz: [erişim belirteçlerini yenileme](v1-protocols-oauth-code.md#refreshing-the-access-tokens).  ADAL veya MSAL kullanıyorsanız bu sizin için kitaplığı tarafından işlenir; 'AcquireTokenSilentAsync' ile 'AcquireTokenByAuthorizationCodeAsync' in ikinci örneğini değiştirin. 

## <a name="may-2018"></a>Mayıs 2018

### <a name="id-tokens-cannot-be-used-for-the-obo-flow"></a>OBO akışı kimlik belirteçlerini kullanılamaz

**Tarih**: 1 Mayıs 2018

**Etkilenen uç noktaları**: V1.0 hem v2.0

**Etkilenen protokolleri**: Örtük akış ve [OBO akış](v1-oauth2-on-behalf-of-flow.md)

1 Mayıs 2018'den sonra id_tokens bir OBO akışında onay olarak yeni uygulamalar için kullanılamaz. Erişim belirteçleri, bunun yerine, API'leri, hatta bir istemci ve orta katman aynı uygulamanın arasında güvenli hale getirmek için de kullanılmalıdır. 1 Mayıs 2018'den iş ve bir erişim belirteci için id_tokens alışverişi yapmak devam etmeden önce kayıtlı uygulamalar; Ancak, bu deseni en iyi uygulama olarak kabul edilmez.

Bu değişikliği geçici olarak çözmek için aşağıdakileri yapabilirsiniz:

1. Bir Web API uygulamanız için bir veya daha fazla kapsamı ile oluşturun. Bu açık giriş noktası, daha ayrıntılı denetim ve güvenlik sağlar.
1. Uygulama bildiriminde içinde [Azure portalında](https://portal.azure.com) veya [uygulama kayıt portalı](https://apps.dev.microsoft.com), örtük akış aracılığıyla erişim belirteçlerini vermek için uygulama izin verildiğinden emin olun. Bu aracılığıyla denetlenir `oauth2AllowImplicitFlow` anahtarı.
1. İstemci uygulamanız aracılığıyla bir id_token istediğinde `response_type=id_token`, ayrıca bir erişim belirteci isteği (`response_type=token`) Web API'si, yukarıda oluşturduğunuz için. Bu nedenle, v2.0 uç noktası kullanırken `scope` parametresi benzer şekilde görünmelidir `api://GUID/SCOPE`. V1.0 uç noktasında `resource` parametresi URI Web API'sini uygulama olmalıdır.
1. Orta katman id_token yerine bu erişim belirtecini geçirin.  
