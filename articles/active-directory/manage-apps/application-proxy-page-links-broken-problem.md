---
title: Sayfadaki bağlantıları için bir uygulama proxy'si uygulaması çalışmıyor | Microsoft Docs
description: Azure AD ile tümleştirilmiş uygulama Proxy uygulamaları bozuk bağlantıları ile ilgili sorunları giderme
services: active-directory
documentationcenter: ''
author: barbkess
manager: daveba
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/10/2018
ms.author: barbkess
ms.reviewer: asteen
ms.openlocfilehash: 65e903ee1ee8111d3d3b064d6018f49b2e96af47
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54478017"
---
# <a name="links-on-the-page-dont-work-for-an-application-proxy-application"></a>Bir uygulama proxy'si uygulaması için çalışma sayfasındaki bağlantılar çalışmıyor

Bu makalede, Azure Active Directory Uygulama proxy'si uygulaması bağlantıları düzgün çalışmaz neden gidermenize yardımcı olur.

## <a name="overview"></a>Genel Bakış 
Uygulama proxy'si uygulaması yayımladıktan sonra çalışan uygulamada varsayılan olarak yalnızca hedeflere yayımlanan kök URL'si içinde yer alan bağlantıları bağlantılardır. Uygulamaların içindeki bağlantılar çalışmayan, uygulama için dahili URL'yi büyük olasılıkla uygulamadaki bağlantıların tüm hedefler dahil değildir.

**Bu neden gerçekleşir?** Bir uygulamada bir bağlantıya tıkladığınızda, uygulama ara sunucusu URL'si aynı uygulama içinde ya da bir iç URL veya harici olarak kullanılabilir bir URL olarak çözmeye çalışır. Bağlantı noktalarını aynı uygulama içinde bir iç URL için değil ya da bu kadar demeti ait ve bulunamadı bir hataya neden.

## <a name="ways-you-can-resolve-broken-links"></a>Bozuk bağlantılar, çözme yollarına

Bu sorunu çözmek için üç yolu vardır. Aşağıdaki seçenek artan karmaşıklık listelenir.

1.  Uygulama için tüm bağlantıları içeren bir kök İç URL olduğundan emin olun. Bu, aynı uygulama içinde yayımlanan içerik olarak çözülmesi tüm bağlantılara izin verir.

    İç URL değiştirme, ancak kullanıcıların giriş sayfası değiştirmek istemiyorsanız, giriş sayfası URL'si, önceden yayımlanmış dahili URL'yi değiştirin. Bu, "Azure Active Directory" - giderek yapılabilir&gt; uygulama kayıtları -&gt; - uygulamayı seçin&gt; özellikleri. Bu özellikler sekmesinde, "Giriş sayfasına istenen giriş sayfası olacak şekilde ayarlayabildiğiniz URL", alan görürsünüz.

2.  Uygulamalarınızı tam etki alanı adlarını (FQDN) kullanıyorsanız, [özel etki alanları](application-proxy-configure-custom-domain.md) uygulamalarınızı yayımlamak için. Bu özellik hem dahili olarak kullanılan ve dışarıdan aynı URL'yi sağlar.

    Bu seçenek, iç URL uygulamaya içindeki bağlantılar da harici olarak tanınır olduğundan uygulamanız bağlantıları uygulama proxy'si aracılığıyla harici olarak erişilebilen olmasını sağlar. Yayımlanan bir uygulamaya ait tüm bağlantıları yine de gerekir. Ancak, bu seçenekle bağlantıları aynı uygulamaya ait gerekmez ve birden çok uygulamalara ait olabilir.

3.  Bu seçeneklerin ikisi de uygulanabilir, satır içi bağlantı çeviri etkinleştirmek için birden çok seçenek vardır. Bu seçeneklerin, Intune Managed Browser, uygulamalarım uzantısı veya uygulama bağlantısını çeviri ayarı kullanarak içerir. Tüm bu seçenekler ve bunları etkinleştirme hakkında daha fazla bilgi için bkz: [yeniden yönlendirme, Azure AD uygulama ara sunucusu ile yayımlanan uygulamalar için sabit kodlanmış bağlantı](application-proxy-configure-hard-coded-link-translation.md).

## <a name="next-steps"></a>Sonraki adımlar
[Mevcut şirket içi proxy sunucuları ile çalışma](application-proxy-configure-connectors-with-proxy-servers.md)

