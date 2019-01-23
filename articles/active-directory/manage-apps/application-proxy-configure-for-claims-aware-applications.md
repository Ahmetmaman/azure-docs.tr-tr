---
title: Talep kullanan uygulamalar - Azure AD uygulama ara sunucusu | Microsoft Docs
description: Kullanıcılarınız tarafından güvenli uzaktan erişim için ADFS talep kabul şirket içi ASP.NET uygulamaları yayımlamak nasıl.
services: active-directory
documentationcenter: ''
author: barbkess
manager: daveba
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/08/2018
ms.author: barbkess
ms.reviewer: japere
ms.openlocfilehash: 6313bc8f3245966d22e904f96a90674b27d396f7
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54472945"
---
# <a name="working-with-claims-aware-apps-in-application-proxy"></a>Uygulama proxy'sinde talep kullanan uygulamalar ile çalışma
[Talep kullanan uygulamalar](https://msdn.microsoft.com/library/windows/desktop/bb736227.aspx) yeniden yönlendirmesi için güvenlik belirteci hizmeti (STS) gerçekleştirin. STS, bir belirteç lisanslarınıza kullanıcıdan kimlik bilgilerini ister ve ardından kullanıcıyı uygulamaya yönlendirir. Bu yeniden yönlendirme ile çalışmak uygulama proxy'sini etkinleştirmek için birkaç yolu vardır. Talep kullanan uygulamalar için dağıtımınızı yapılandırmak için bu makaleyi kullanın. 

## <a name="prerequisites"></a>Önkoşullar
Talep kullanan uygulamaya yönlendirilir STS şirket içi ağınızın dışında kullanılabilir olduğundan emin olun. STS kullanılabilir bir ara sunucu aracılığıyla kullanıma sunmak veya dış bağlantılara izin vermeden yapabilirsiniz. 

## <a name="publish-your-application"></a>Uygulamanızı yayımlama

1. Açıklanan yönergelere göre uygulamanızı yayımlayın [uygulama ara sunucusu ile uygulama yayımlama](application-proxy-add-on-premises-application.md).
2. Uygulama seçin ve portal sayfasına gidebilirsiniz **çoklu oturum açma**.
3. Seçerseniz, **Azure Active Directory** olarak, **ön kimlik doğrulama yöntemi**seçin **Azure AD çoklu oturum açma devre dışı** olarak, **iç Kimlik doğrulama yöntemi**. Seçerseniz, **geçiş** olarak, **ön kimlik doğrulama yöntemi**, hiçbir şey değiştirmeniz gerekmez.

## <a name="configure-adfs"></a>AD FS yapılandırma

İki yoldan biriyle talep kullanan uygulamalar için ADFS yapılandırabilirsiniz. İlk özel etki alanları kullanmaktır. WS-Federasyon ile saniyedir. 

### <a name="option-1-custom-domains"></a>1. seçenek: Özel etki alanları

Tüm uygulamalarınız için iç URL tam olarak nitelikli etki alanı adlarını (FQDN) sonra yapılandırabileceğiniz [özel etki alanları](application-proxy-configure-custom-domain.md) uygulamalarınız için. İç URL ile aynıdır, dış URL'leri oluşturmak için özel etki alanları kullanın. Ardından, dış URL'leri iç URL'nizde eşleştiğinde, kullanıcılarınızın şirket içi veya uzak olup STS yeniden yönlendirmeleri çalışır. 

### <a name="option-2-ws-federation"></a>2. seçenek: WS-Federation

1. AD FS Yönetimi'ni açın.
2. Git **bağlı olan taraf güvenleri**uygulaması Ara sunucusu ile yayımlama, uygulamayı sağ tıklatın ve seçin **özellikleri**.  

   ![Uygulama adı - ekran görüntüsü bağlı olan taraf Güvenleri'sağ tıklayın](./media/application-proxy-configure-for-claims-aware-applications/appproxyrelyingpartytrust.png)  

3. Üzerinde **uç noktaları** sekmesindeki **uç noktası türü**seçin **WS-Federation**.
4. Altında **güvenilen URL**, uygulama proxy'si girdiğiniz URL'yi girin **dış URL** tıklatıp **Tamam**.  

   ![Uç nokta - ekleme güvenilen URL değeri - ekran görüntüsü ayarlama](./media/application-proxy-configure-for-claims-aware-applications/appproxyendpointtrustedurl.png)  

## <a name="next-steps"></a>Sonraki adımlar
* [Çoklu oturum açmayı etkinleştirme](configure-single-sign-on-portal.md) talep kullanan olmayan uygulamalar için
* [Yerel istemci uygulama proxy uygulamaları ile etkileşim kurmak etkinleştirin](application-proxy-configure-native-client-application.md)


