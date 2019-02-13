---
title: Bir uygulama için oturum açarken beklenmedik bir onay istemi | Microsoft Docs
description: Bir kullanıcı girmek istemediğiniz Azure AD ile tümleşik bir uygulama için bir onay istemi gördüğünde sorunlarını giderme
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: celested
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: 178a5e4e2a6ed520a6c58ce07a30684b2d1cbc09
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2019
ms.locfileid: "56217301"
---
# <a name="unexpected-consent-prompt-when-signing-in-to-an-application"></a>Bir uygulama için oturum açarken beklenmedik bir onay istemi

Azure Active Directory ile tümleştirme, birçok uygulama çeşitli kaynaklarıyla çalıştırmak için ilgili izinleri gerektirir. Bu kaynakları Azure Active Directory ile bunlara erişmek için izinleri de zaman tümleşik Azure AD'ye onay çerçevesine kullanarak istenir. 

Bu genellikle tek seferlik bir işlem olduğu uygulamanın ilk kullanılışında gösterilen bir onay istemi olduğu sonuçlanır. 

## <a name="scenarios-in-which-users-see-consent-prompts"></a>Hangi kullanıcıların gördüğü senaryoları onay istemleri

Ek istemleri, çeşitli senaryolarda beklenebilir:

* Uygulama için gereken izinler kümesini değişti.

* İlk olarak uygulamaya onay veren kullanıcıya yönetici değildi ve artık farklı (yönetici olmayan) kullanıcı uygulamayı ilk kez kullanıyor.

* Yönetici ilk uygulamaya onay veren kullanıcıya ait olduğu, ancak adına tüm kuruluş izin vermedi.

* Uygulamayı kullanarak [artımlı ve dinamik onay](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-compare#incremental-and-dynamic-consent) onay başlangıçta verildikten sonra ilave izin istemek için. Bu, genellikle isteğe bağlı ek uygulama özelliklerini temel işlevselliği için gerekli olanlar dışında izinleri gerektiğinde kullanılır.

* Başlangıçta verilen sonra onay iptal edildi.

* Geliştirici uygulamayı her kullanıldığında bir onay istemi gerektirecek şekilde yapılandırılmış (Not: Bu en iyi yöntem değildir).

## <a name="next-steps"></a>Sonraki adımlar

-   [Uygulamalar, izinler ve onay Azure Active Directory'de (v1.0 uç noktası)](https://docs.microsoft.com/azure/active-directory/active-directory-apps-permissions-consent)

-   [Kapsamlar, izinler ve onay Azure Active Directory'de (v2.0 uç noktası)](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)


