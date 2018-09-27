---
title: Azure Active Directory'de Web uygulamaları
description: Web apps nedir ve temel Protokolü akış, kayıt ve bu uygulama türü için belirteci süre sonu açıklar.
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
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: celested
ms.reviewer: saeeda, jmprieur, andret
ms.custom: aaddev
ms.openlocfilehash: 0eec1100c5246bb9f5c281971453ac60abc0339f
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2018
ms.locfileid: "47225031"
---
# <a name="web-apps"></a>Web uygulamaları

Web apps, bir kullanıcının bir web uygulaması için bir web tarayıcısında kimliğini uygulamalardır. Bu senaryoda, Azure AD'de oturum açmak için kullanıcının tarayıcısına web uygulamasının yönlendirir. Azure AD oturum açma yanıt içeren bir güvenlik belirteci kullanıcı hakkında talepler kullanıcının tarayıcısından döndürür. Bu senaryo, Openıd Connect, SAML 2.0 ve WS-Federasyon protokollerini kullanarak oturum açmayı destekler.

## <a name="diagram"></a>Diyagram

![Tarayıcıdan web uygulamasına yönelik kimlik doğrulama akışı](./media/authentication-scenarios/web_browser_to_web_api.png)

## <a name="protocol-flow"></a>Akış Protokolü

1. Bir kullanıcı oturum açmak için gereksinimleri ve uygulama ziyaret ettiğinde, bir oturum açma isteği için kimlik doğrulama uç noktası aracılığıyla Azure AD'de yönlendirilir.
1. Oturum açma sayfasında oturum açtığında kullanıcı.
1. Kimlik doğrulaması başarılı olursa, Azure AD kimlik doğrulama belirteci oluşturur ve uygulamanın yanıt URL'si Azure portalında yapılandırılmış bir oturum açma yanıtı döndürür. Bir üretim uygulaması için bu yanıt URL'si HTTPS olmalıdır. Döndürülen belirteç, Azure AD ve kullanıcı belirteci doğrulamak için uygulama tarafından gerekli olan talepleri içerir.
1. Uygulama, bir ortak imzalama anahtar ve veren bilgi kullanılabilir Azure AD için Federasyon meta veri belge kullanarak belirteci doğrular. Uygulama belirteci doğruladıktan sonra kullanıcı ile yeni bir oturum başlatır. Bu oturumun süresi sona erene kadar uygulamaya erişmek kullanıcı izin verir.

## <a name="code-samples"></a>Kod örnekleri

Web tarayıcısına web uygulaması senaryoları için kod örneklere bakın. Ve, sık sık yeni örnekler eklendikçe yeniden denetleyin.

## <a name="app-registration"></a>Uygulama kaydı

Bir web uygulamasını kaydetmek için bkz: [bir uygulamayı Azure AD'ye v1.0 uç noktası ile kaydetme](quickstart-v1-add-azure-ad-app.md).

* Tek bir Kiracı - yalnızca kuruluşunuz için bir uygulama oluşturuyorsanız, bunu şirketinizin dizininde Azure portalını kullanarak kayıtlı olması gerekir.
* Multi-kuruluşunuzun dışındaki kullanıcılar tarafından kullanılabilen bir uygulama oluşturuyorsanız tenan -, bunu şirketinizin dizinde kayıtlı olması gerekir, ancak uygulamayı kullanarak her kuruluşun dizininde da kayıtlı olması gerekir. Uygulamanızı directory'lerinde kullanılabilir hale getirme için uygulamanıza onay verme tanıyan müşterileriniz için bir kayıt işlemi dahil edebilirsiniz. Bunlar, uygulamanız için oturum açarken uygulama izinleri gösteren bir iletişim kutusu, ardından onay seçeneği ile sunulur. Gerekli izinlere bağlı olarak diğer kuruluştaki bir yöneticisinin izni vermek için gerekebilir. Kullanıcı veya yönetici onay verdiğinde, uygulama, dizinde kayıtlı.

## <a name="token-expiration"></a>Belirteç süre sonu

Azure AD tarafından verilen belirtecin süresi dolduğunda, kullanıcının oturum sona erer. Uygulamanızı bir işlem yapılmadan geçen süre üzerinde bağlı olarak kullanıcılara imzalama gibi isterseniz, bu süre içinde kısaltabilirsiniz. Oturumun süresi dolduğunda, kullanıcı yeniden oturum açmanız istenir.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer hakkında daha fazla bilgi [uygulama türleri ve senaryolar](app-types.md)
* Azure AD hakkında bilgi edinin [kimlik doğrulaması temelleri](authentication-scenarios.md)