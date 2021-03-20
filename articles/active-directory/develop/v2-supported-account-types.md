---
title: Desteklenen hesap türleri | Mavisi
titleSuffix: Microsoft identity platform
description: Uygulamalarda izleyiciler ve desteklenen hesap türleri hakkında kavramsal belgeler
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 07/14/2020
ms.author: jmprieur
ms.reviewer: saeeda
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: d6c184e2983a072dec4b3021a1b58a61cd206dba
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "98755993"
---
# <a name="supported-account-types"></a>Desteklenen hesap türleri

Bu makalede, Microsoft Identity platform uygulamalarında hangi hesap türlerinin (bazen *hedef kitleleri* olarak adlandırılır) desteklendiği açıklanmaktadır.

<!-- This section can be in an include for many of the scenarios (SPA, web app signing-in users, protecting a web API, Desktop (depending on the flows), Mobile -->

## <a name="account-types-in-the-public-cloud"></a>Genel buluttaki hesap türleri

Microsoft Azure genel bulutta, çoğu uygulama türü kullanıcılara herhangi bir kitle ile oturum açabilir:

- İş kolu (LOB) uygulaması yazıyorsanız, kullanıcıların kendi kuruluşunuzda oturum açmasını sağlayabilirsiniz. Böyle bir uygulama bazen *tek kiracılı* olarak adlandırılır.
- ISV iseniz, kullanıcılar oturumunu açan bir uygulama yazabilirsiniz:

  - Herhangi bir kuruluşta. Böyle bir uygulamaya *çok kiracılı* bir Web uygulaması denir. Bazen kullanıcılara iş veya okul hesaplarıyla oturum açabileceksiniz.
  - İş veya okul veya kişisel Microsoft hesaplarıyla.
  - Yalnızca kişisel Microsoft hesaplarıyla.
    
- Bir işletmeden tüketici uygulaması yazıyorsanız, Azure Active Directory B2C (Azure AD B2C) kullanarak kullanıcıların sosyal kimliklerini kullanarak da oturum açabilirsiniz.

## <a name="account-type-support-in-authentication-flows"></a>Kimlik doğrulama akışlarında hesap türü desteği

Bazı hesap türleri belirli kimlik doğrulama akışlarıyla kullanılamaz. Örneğin, Masaüstü, UWP veya Daemon uygulamalarında:

- Daemon uygulamaları yalnızca Azure AD kuruluşları ile kullanılabilir. Microsoft kişisel hesaplarını işlemek için Daemon uygulamalarını kullanmayı denemek mantıklı değildir. Yönetici onayı hiçbir şekilde verilmeyecektir.
- Tümleşik Windows kimlik doğrulaması akışını yalnızca iş veya okul hesaplarıyla (kuruluşunuzda veya herhangi bir kuruluşta) kullanabilirsiniz. Tümleşik Windows kimlik doğrulaması etki alanı hesaplarıyla birlikte çalışarak, makinelerin etki alanına katılmış veya Azure AD 'ye katılmış olmasını gerektirir. Bu akış kişisel Microsoft hesapları için anlamlı değildir.
- [Kaynak sahibi parola kimlik bilgileri verme](./v2-oauth-ropc.md) (Kullanıcı adı/parola) kişisel Microsoft hesaplarıyla kullanılamaz. Kişisel Microsoft hesapları, kullanıcının her oturum açma oturumunda kişisel kaynaklara erişmesini gerektirir. Bu davranış, etkileşimli olmayan akışlarla uyumlu değildir.

## <a name="account-types-in-national-clouds"></a>Ulusal bulutlarda hesap türleri

Uygulamalar, kullanıcıların [Ulusal bulutlarda](authentication-national-cloud.md)da oturum açabilir. Ancak, Microsoft kişisel hesapları bu bulutlarda desteklenmez. Bu nedenle, desteklenen hesap türlerinin bu bulutlara, kuruluşunuzda (tek bir kiracıya) veya herhangi bir kuruluşa (çok kiracılı uygulamalar) karşı azaltıldı.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Active Directory 'de kiracı](./single-and-multi-tenant-apps.md)hakkında daha fazla bilgi edinin.
- [Ulusal bulutlar](./authentication-national-cloud.md)hakkında daha fazla bilgi edinin.
