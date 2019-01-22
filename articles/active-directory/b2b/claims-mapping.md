---
title: B2B işbirliği kullanıcısı talep eşlemesi, Azure Active Directory'de | Microsoft Docs
description: Kullanıcı Azure Active Directory (Azure AD) B2B kullanıcıları için SAML belirtecinde verilen talepleri özelleştirme.
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: conceptual
ms.date: 04/06/2018
ms.author: mimart
author: msmimart
manager: daveba
ms.reviewer: sasubram
ms.openlocfilehash: 7a06afd7e6e295510f393177d9828fe6dd7a3c0e
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2019
ms.locfileid: "54428912"
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

