---
title: Azure Active Directory kiracısı oluşturma | Microsoft Docs
description: Uygulamaları kaydetmek ve derlemek için kullanmak üzere Azure AD kiracısı oluşturmayı öğrenin.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 1f4b24eb-ab4d-4baa-a717-2a0e5b8d27cd
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 09/24/2018
ms.author: celested
ms.reviewer: dadobali
ms.custom: aaddev
ms.openlocfilehash: 731b68e3f7dbb46f2fa51a18cb5b3da6b4626fa6
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46963943"
---
# <a name="quickstart-set-up-a-dev-environment"></a>Hızlı başlangıç: Geliştirme ortamını ayarlama

Microsoft kimlik platformu geliştiricilerin çok çeşitli özel Microsoft 365 ortamlarını ve kimliklerini hedefleyen uygulamalar derlemesine olanak tanır. Microsoft kimlik platformunu kullanmaya başlamak için, Azure AD kiracısı olarak da adlandırılan ve uygulamaları kaydedip yönetebilen, Microsoft 365 verilerine erişimi olan ve özel Koşullu Erişim ve kiracı kısıtlamaları dağıtan bir ortama erişeceksiniz. 

Kiracı, bir kuruluşun gösterimidir. Kuruluş veya uygulama geliştirici Microsoft'la bir ilişki oluşturduğunda, örneğin Azure'a, Microsoft Intune'a veya Microsoft 365'e kaydolduğunda kuruluşun veya uygulama geliştiricinin aldığı özel bir Azure AD örneğidir. 

Her Azure AD kiracısı ayrıdır ve diğer Azure AD kiracılarından ayrılmıştır. Kendi iş ve okul kimlikleri, tüketici kimlikleri (bir Azure AD B2C kiracısıysa) ve uygulama kayıtları gösterimi vardır. Kiracınızın içinde yapılan bir uygulama kaydı, yalnızca kiracınızın veya tüm kiracıların içinden hesapların kimlik doğrulaması yapmasına izin verebilir. 

## <a name="determining-environment-type"></a>Ortam türünü saptama

İki tür ortam oluşturabilirsiniz. Hangisine ihtiyacınız olduğuna karar verirken yalnızca uygulamanızın kimliklerini doğrulayacağı kullanıcı türlerine bakılır.

* İş ve okul hesapları (Azure AD hesapları) veya Microsoft hesapları (outlook.com ve live.com gibi)
* Sosyal ve yerel hesaplar (Azure AD B2C)

Bu hızlı başlangıç, derlemek istediğiniz uygulamanın türüne bağlı olarak iki senaryoya bölünmüştür. Bir kimlik türünü hedefleme konusunda daha fazla yardıma ihtiyacınız varsa, [Microsoft kimlik platformu hakkında](about-microsoft-identity-platform.md) konusuna bakın

## <a name="work-and-school-accounts-or-personal-microsoft-accounts"></a>İş ve okul hesapları veya kişisel Microsoft hesapları

### <a name="use-an-existing-tenant"></a>Mevcut kiracıyı kullanma

Birçok geliştiricinin, Azure AD kiracılarına bağlı hizmetler veya abonelikler (örneğin, Microsoft 365 veya Azure abonelikleri) aracılığıyla zaten kiracıları vardır.

1. Kiracıyı denetlemek için, uygulamanızı yönetirken kullanmak istediğiniz hesapla [Azure portalında](https://portal.azure.com) oturum açın.
1. Sağ üst köşeye bakın. Bir kiracınız varsa, otomatik olarak bu kiracıda oturum açar ve kiracı adını doğrudan hesap adınızın altında görebilirsiniz.
   * Adınızı, e-postanızı, dizininizi/kiracı kimliğinizi (GUID) ve etki alanınızı görmek için, Azure portalının sağ üst kısmında bulunan hesap adınızın üzerine gelin.
   * Hesabınız birden çok kiracıyla ilişkiliyse, kiracılar arasında geçiş yapabileceğiniz bir menüyü açmak için hesap adınızı seçebilirsiniz. Her kiracının kendi kiracı kimliği vardır.

> [!TIP]
> Kiracı kimliğini bulmanız gerekiyorsa şunları yapabilirsiniz:
* Dizin / kiracı kimliği değerini almak için hesap adınızın üzerine gelin, veya
* Azure portalında **Azure Active Directory > Özellikler > Dizin Kimliği**'ni seçin

Hesabınızla ilişkilendirilmiş bir kiracınız yoksa, hesap adınızın altında bir GUID görürsünüz ve sonraki bölümde verilen adımları izleyene kadar uygulamaları kaydetme gibi eylemler gerçekleştiremezsiniz.

### <a name="create-a-new-azure-ad-tenant"></a>Yeni Azure AD kiracısı oluşturma

Henüz bir Azure AD kiracınız yoksa veya geliştirme için yeni kiracı oluşturmak istiyorsanız, [dizin oluşturma deneyimi](https://portal.azure.com/#create/Microsoft.AzureActiveDirectory) yönergelerini izleyin. Yeni kiracı oluşturmak için aşağıdaki bilgileri sağlamanız gerekecektir:

- **Kuruluş adı**
- **İlk etki alanı** - Bu, *.onmicrosoft.com'un bir parçası olacaktır. Etki alanını daha sonra özelleştirebilirsiniz. 
- **Ülke veya bölge**

## <a name="social-and-local-accounts"></a>Sosyal ve yerel hesaplar

Sosyal ve yerel hesapların oturumunu açan bir uygulama derlemeye başlamak için, bir Azure AD B2C kiracısı oluşturmanız gerekir. Başlamak için, [Azure AD B2C kiracısı oluşturma](../../active-directory-b2c/tutorial-create-tenant.md) yönergelerini izleyin. 

## <a name="next-steps"></a>Sonraki adımlar

* Bir kodlama hızlı başlangıcını deneyin ve kullanıcıların kimliklerini doğrulamaya başlayın. 
* Daha ayrıntılı kod örnekleri için, belgelerin **Öğreticiler** bölümünü gözden geçirin.
* Uygulamanızı bulutta mı dağıtmak istiyorsunuz? [Azure'a kapsayıcı dağıtma](https://docs.microsoft.com/azure/index#pivot=products&panel=containers) konusunu gözden geçirin. 
