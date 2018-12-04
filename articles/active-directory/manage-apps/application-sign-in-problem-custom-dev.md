---
title: Özel olarak geliştirilmiş bir uygulamada oturum açarken sorun | Microsoft Docs
description: Bir uygulamaya oturum açabilir olmaması için neden olabilecek yaygın rrors Azure AD ile geliştirilmiştir.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: barbkess
ms.reviewer: asteen
ms.openlocfilehash: 2dade35b05a07b649282ae00bb6fee354adcd195
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52845508"
---
# <a name="problems-signing-in-to-an-custom-developed-application"></a>Özel olarak geliştirilmiş bir uygulamada oturum açarken sorun

Bir uygulamada oturum açması mümkün olmaması için neden olabilecek çeşitli hatalar var. Bu sorun büyük neden değinilmektedir uygulamaları yanlış.

## <a name="errors-related-to--misconfigured-apps"></a>Yanlış yapılandırılmış uygulamalarıyla ilgili hataları

* Uygulamanızda sahip olduğunuz yapılandırmaları portalında eşleştiğini doğrulayın. Özellikle, istemci/uygulama kimliği, yanıt URL'leri, istemci gizli dizileri/anahtarları ve uygulama kimliği URI'si karşılaştırın.

* Kod içinde yapılandırılan izinlerle erişim isteyen kaynak karşılaştırma **gerekli kaynakları** yapılandırdığınız kaynaklar yalnızca istek emin olmak için sekmesinde.

* Bkz: [Azure AD StackOverflow](https://stackoverflow.com/questions/tagged/azure-active-directory) benzer hatalar veya sorunlar için.

## <a name="next-steps"></a>Sonraki adımlar

[Azure AD Geliştirici Kılavuzu](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide)<br>

[Onay ve Azure ad uygulamaları tümleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications>)<br>

[Uygulama onay ve Azure AD v2.0 için kullanarak yakınsanmış](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)<br>

[Azure AD StackOverflow](https://stackoverflow.com/questions/tagged/azure-active-directory>)
