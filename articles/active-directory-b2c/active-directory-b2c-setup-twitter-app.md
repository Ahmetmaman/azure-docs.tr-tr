---
title: Azure Active Directory B2C kullanarak bir Twitter hesabıyla kaydolma ve oturum açma ayarlama | Microsoft Docs
description: Azure Active Directory B2C kullanarak uygulamalarınızı Twitter hesaplarında sahip müşteriler için kaydolma ve oturum açma sağlar.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: f0f0b8e0cbb5fbab81a07a28a9d4a2c264be6545
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2018
ms.locfileid: "52719870"
---
# <a name="set-up-sign-up-and-sign-in-with-a-twitter-account-using-azure-active-directory-b2c"></a>Azure Active Directory B2C kullanarak bir Twitter hesabıyla kaydolma ve oturum açma ayarlama

## <a name="create-an-application"></a>Uygulama oluşturma

Bir Azure AD B2C kimlik sağlayıcısı olarak twitter'ı kullanmak için bir Twitter uygulaması oluşturmanız gerekir. Twitter hesabıyla yoksa adresinden edinebilirsiniz [ https://twitter.com/signup ](https://twitter.com/signup).

1. Oturum [Twitter geliştiriciler](https://developer.twitter.com/en/apps) Twitter hesabınızın kimlik bilgileriyle Web sitesi.
2. Seçin **uygulama oluşturma**.
3. Girin bir **uygulama adı** ve **uygulama açıklaması**.
4. İçinde **Web sitesi URL'si**, girin `https://your-tenant.b2clogin.com`. Değiştirin `your-tenant` kiracınızın ada sahip. Örneğin, https://contosob2c.b2clogin.com.
5. İçin **geri çağırma URL'si**, girin `https://your-tenant.b2clogin.com/your-tenant.onmicrosoft.com/your-user-flow-Id/oauth1/authresp`. Değiştirin `your-tenant` Kiracı adınızın adıyla ve `your-user-flow-Id` kullanıcı akışınızı tanıtıcısı ile. Örneğin, `b2c_1A_signup_signin_twitter`. Tüm harfleri büyük harflerle Azure AD B2C ile Kiracı tanımlansa bile Kiracı adınızın girerken kullanmanız gerekir.
6. Sayfanın altındaki okuyun ve koşulları kabul edin ve ardından **Oluştur**.
7. Üzerinde **uygulama ayrıntıları** sayfasında, **Düzenle > Ayrıntıları Düzenle**, kutusunu işaretlemeniz **Twitter etkinleştirme oturum**ve ardından **Kaydet**.
8. Seçin **anahtarları ve belirteçleri** ve kayıt **tüketici API anahtarı** ve **tüketici API gizli anahtarı** daha sonra kullanılacak değerler.

## <a name="configure-twitter-as-an-identity-provider-in-your-tenant"></a>Kiracınızdaki bir kimlik sağlayıcısı olarak twitter'ı yapılandırma

1. [Azure portalda](https://portal.azure.com/) Azure AD B2C kiracınızın genel yöneticisi olarak oturum açın.
2. Azure AD B2C kiracınızı tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve kiracınız içeren dizine seçme.
3. Azure portalın sol üst köşesinde **Tüm hizmetler**’i seçin ve **Azure AD B2C**’yi arayıp seçin.
4. Seçin **kimlik sağlayıcıları**ve ardından **Ekle**.
5. Sağlayan bir **adı**. Örneğin, *Twitter*.
6. Seçin **kimlik sağlayıcısı türü**seçin **Twitter**, tıklatıp **Tamam**.
7. Seçin **bu kimlik sağlayıcısını ayarlama** ve API anahtarını girin **istemci kimliği** ve API gizli anahtarınızı **gizli**.
8. Tıklayın **Tamam**ve ardından **Oluştur** Twitter yapılandırmanızı kaydetmek için.