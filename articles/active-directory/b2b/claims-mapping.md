---
title: Azure Active Directory B2B işbirliği kullanıcısı talep eşlemesi - | Microsoft Docs
description: Kullanıcı Azure Active Directory (Azure AD) B2B kullanıcıları için SAML belirtecinde verilen talepleri özelleştirme.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 04/06/2018
ms.author: mimart
author: msmimart
manager: daveba
ms.reviewer: sasubram
ms.collection: M365-identity-device-management
ms.openlocfilehash: 445d6c8b527b56f63a1156a39ac07393b4ffe115
ms.sourcegitcommit: 8ca6cbe08fa1ea3e5cdcd46c217cfdf17f7ca5a7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/22/2019
ms.locfileid: "56668556"
---
# <a name="b2b-collaboration-user-claims-mapping-in-azure-active-directory"></a>Eşleme Azure Active Directory B2B işbirliği kullanıcı talepleri

B2B işbirliği kullanıcıları için SAML belirtecinde verilen talepleri özelleştirme azure Active Directory (Azure AD) destekler. Bir kullanıcı uygulama kimliğini doğruladığında, Azure AD benzersiz olarak tanımlayan bir kullanıcı hakkında bilgileri (veya talep) içeren uygulamaya bir SAML belirteci verir. Varsayılan olarak, bu kullanıcının kullanıcı adı, e-posta adresi, ad ve Soyadı içerir.

İçinde [Azure portalında](https://portal.azure.com), görüntüleyebilir veya uygulamaya SAML belirtecindeki gönderilen talepleri düzenleyin. Ayarlara erişmek için seçin **Azure Active Directory** > **kurumsal uygulamalar** > çoklu oturum açma için yapılandırılmış uygulama > **çoklu oturum açma** . SAML belirteci ayarlarında bkz **kullanıcı öznitelikleri** bölümü.

![SAML belirteci öznitelikleri kullanıcı Arabiriminde gösterir](media/claims-mapping/view-claims-in-saml-token.png)

SAML belirtecinde verilen talepleri düzenlemeniz gerekebilir neden iki olası nedeni vardır:

1. Farklı uygulama gerektiriyorsa URI'ler talep veya değerleri.

2. Uygulamanın Azure AD'de depolanan kullanıcı asıl adı (UPN) dışında bir şey olacak şekilde NameIdentifier talebini gerektirir.

Ekleme ve talep düzenleme hakkında daha fazla bilgi için bkz. [Azure Active Directory'de kurumsal uygulamalar için SAML belirtecinde verilen talepleri özelleştirme](../develop/active-directory-saml-claims-customization.md).

B2B işbirliği için Nameıd ve UPN kiracılar arası eşleme kullanıcıları, güvenlik nedenleriyle engellenir.

## <a name="next-steps"></a>Sonraki adımlar

- B2B işbirliği kullanıcı özellikleri hakkında daha fazla bilgi için bkz. [bir Azure Active Directory B2B işbirliği kullanıcısı özelliklerini](user-properties.md).
- B2B işbirliği kullanıcıları için kullanıcı belirteçlerinin hakkında daha fazla bilgi için bkz. [Azure AD B2B işbirliği içinde çalışan kullanıcı belirteçleri anlamak](user-token.md).

