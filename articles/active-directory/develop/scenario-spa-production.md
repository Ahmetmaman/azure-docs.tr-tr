---
title: Tek sayfalı uygulamayı üretime taşıma-Microsoft Identity platform | Mavisi
description: Tek sayfalı bir uygulama oluşturmayı öğrenin (üretime geçin)
services: active-directory
author: navyasric
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 05/07/2019
ms.author: nacanuma
ms.custom: aaddev
ms.openlocfilehash: 21ba0193c3f1e19ffc74452aaceee34759c7e606
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "88949023"
---
# <a name="single-page-application-move-to-production"></a>Tek sayfalı uygulama: üretime taşı

Artık, Web API 'Lerini çağırmak için bir belirteç edinmeyi öğrenmiş olduğunuza göre, üretime nasıl geçeceğinizi öğrenin.

## <a name="improve-your-app"></a>Uygulamanızı geliştirme

Uygulama üretim ortamınızı hazırlamak için [günlük kaydını etkinleştirin](msal-logging.md) .

## <a name="test-your-integration"></a>Tümleştirmenizi test etme

[Microsoft Identity platform tümleştirme denetim listesini](identity-platform-integration-checklist.md)izleyerek tümleştirmenizi test edin.

## <a name="deploy-your-app"></a>Uygulamanızı dağıtın

SPA ve Web API projelerinizi, sırasıyla Azure Storage ve Azure Uygulama Hizmetleri ile dağıtmayı öğrenmek için bir [dağıtım örneğine](https://github.com/Azure-Samples/ms-identity-javascript-angular-spa-aspnet-webapi-multitenant/tree/master/Chapter3) göz atın. 

## <a name="next-steps"></a>Sonraki adımlar

Kullanıcı oturumu açma ve **MSAL.js**kullanarak **Microsoft Graph API** 'sini çağırmak için bir erişim belirteci alma kodunu açıklayan hızlı başlangıç örneğinin derinlemesine bakış:

> [!div class="nextstepaction"]
> [JavaScript SPA öğreticisi](./tutorial-v2-javascript-spa.md)

**MSAL.js**kullanarak kendi arka uç Web API 'niz (ASP.NET Core) için belirteçlerin nasıl alınacağını gösteren örnek:

> [!div class="nextstepaction"]
> [ASP.NET arka ucu olan SPA](https://github.com/Azure-Samples/ms-identity-javascript-angular-spa-aspnetcore-webapi)

**Passport-Azure-AD**kullanarak arka uç Web API 'niz (Node.js) için erişim belirteçlerinin nasıl doğrulandığını gösteren örnek.

> [!div class="nextstepaction"]
> [Node.js Web API 'SI (Azure AD)](https://github.com/Azure-Samples/active-directory-javascript-nodejs-webapi-v2)

**Azure Active Directory B2C** (Azure AD B2C) ile kaydedilmiş bir uygulamada kullanıcıları oturum açmak için **MSAL.js** nasıl kullanacağınızı gösteren örnek:

> [!div class="nextstepaction"]
> [Azure AD B2C ile SPA](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp)

**Azure Active Directory B2C** (Azure AD B2C) ile kaydedilen uygulamalar için erişim belirteçlerini doğrulamak üzere **Passport-Azure-AD** ' nin nasıl kullanılacağını gösteren örnek

> [!div class="nextstepaction"]
> [Node.js Web API 'SI (Azure AD B2C)](https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi)
