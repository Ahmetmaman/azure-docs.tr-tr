---
title: Azure AD federasyonu uyumluluk listesi
description: Bu sayfada, çoklu oturum açmayı uygulamak için kullanılan Microsoft olmayan kimlik sağlayıcılarını alır.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: curtand
ms.assetid: 22c8693e-8915-446d-b383-27e9587988ec
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/23/2018
ms.subservice: hybrid
ms.author: billmath
ms.openlocfilehash: 6ee772054a823ae0607672d41a17a38cd44b2fb8
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55494842"
---
# <a name="azure-ad-federation-compatibility-list"></a>Azure AD federasyonu uyumluluk listesi
Azure Active Directory çoklu oturum sağlar ve herhangi bir üçüncü taraf çözümü gerek kalmadan Office 365 ve diğer Microsoft Online Hizmetleri için karma ve yalnızca bulutta yer alan uygulamalar için uygulama erişimi güvenliği artırılmış. Office 365 gibi Microsoft Çevrimiçi Hizmetleri, çoğu Dizin Hizmetleri, kimlik doğrulama ve yetkilendirme için Azure Active Directory ile tümleştirilmiştir. Azure Active Directory çoklu oturum açma için binlerce SaaS uygulamasına da sağlar ve şirket içi web uygulamaları. Azure Active Directory bkz [uygulama Galerisi](https://azuremarketplace.microsoft.com/marketplace/apps/category/azure-active-directory-apps) desteklenen SaaS uygulamaları için. 

## <a name="idp-validation"></a>IDP doğrulama
Kuruluşunuz bir üçüncü taraf Federasyon çözümü kullanıyorsa, üçüncü taraf Federasyon çözümü Azure ile uyumlu olması şartıyla, çoklu oturum açma, Office 365 gibi Microsoft Online services ile şirket içi Active Directory kullanıcılarınız için yapılandırabilirsiniz Active Directory.  Uyumluluk ile ilgili sorularınız için lütfen kimlik sağlayıcınıza başvurun.  Microsoft, Azure AD ile uyumluluk tıklatın önceden sınanmış kimlik sağlayıcıları listesini görmek isterseniz [burada](https://www.microsoft.com/download/details.aspx?id=56843). 

>[!NOTE]
>Microsoft artık Azure Active Directory ile uyumluluk için bağımsız kimlik sağlayıcıları için doğrulama sınaması sağlar. Ürününüzü birlikte çalışabilirlik için test etmek istiyorsanız, lütfen başvurun [yönergeleri](https://www.microsoft.com/download/details.aspx?id=56843). 

## <a name="next-steps"></a>Sonraki Adımlar

- [Şirket içi dizinlerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md)
- [Azure AD Connect ve federasyon](how-to-connect-fed-whatis.md)
