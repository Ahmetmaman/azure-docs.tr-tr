---
title: Azure AD uygulama ara sunucusu ile uygulamalara çoklu oturum açma | Microsoft Docs
description: Oturum açma için Azure portalında Azure AD uygulama ara sunucusu ile yayımlanan şirket içi uygulamalarınızı tek dönüştürün.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 11/12/2018
ms.author: barbkess
ms.reviewer: japere
ms.custom: it-pro
ms.openlocfilehash: 8cdb5b89bf43d3265f3f9c73d29708f11e17b21c
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2018
ms.locfileid: "51624038"
---
# <a name="password-vaulting-for-single-sign-on-with-application-proxy"></a>Uygulama proxy'si ile çoklu oturum açma için vaulting parola

Azure Active Directory Uygulama proxy'si uzak çalışanlar güvenli bir şekilde, çok erişebilmesi için şirket içi uygulamalar yayımlayarak üretkenliğini artırmaya yardımcı olur. Azure portalında, aynı zamanda çoklu oturum açma (SSO) bu uygulamalara ayarlayabilirsiniz. Kullanıcılarınız yalnızca Azure AD ile kimlik doğrulaması yapmanız ve yeniden oturum açmak zorunda kalmadan Kurumsal uygulamanızı erişebilir.

Uygulama proxy'si birkaç destekler [çoklu oturum açma modları](what-is-single-sign-on.md#choosing-a-single-sign-on-method). Parola tabanlı oturum açma kimlik doğrulaması için bir kullanıcı adı/parola birleşimini kullanan uygulamalar için tasarlanmıştır. Uygulamanız için parola tabanlı oturum açmayı yapılandırırken şirket içi uygulamaya bir kez oturum açmak, kullanıcılarınızın sahip. Bundan sonra Azure Active Directory oturum açma bilgilerini depolar ve kullanıcılarınızın, uzaktan erişim otomatik olarak uygulamaya sağlar. 

Zaten varsa yayımlanan ve uygulamanızı uygulama ara sunucusu ile test. Aksi takdirde, adımları [Azure AD uygulama ara sunucusu kullanarak uygulama yayımlama](application-proxy-publish-azure-portal.md) tekrar buraya gelin. 

## <a name="set-up-password-vaulting-for-your-application"></a>Uygulamanız için vaulting parola ayarlama

1. [Azure Portal](https://portal.azure.com)’da yönetici olarak oturum açın.
2. Seçin **Azure Active Directory** > **kurumsal uygulamalar** > **tüm uygulamaları**.
3. SSO ile ayarlamak istediğiniz uygulamayı listeden seçin.  
4. Seçin **çoklu oturum açma**.

   ![Çoklu oturum açma seçin](./media/application-proxy-configure-single-sign-on-password-vaulting/select-sso.png)

5. SSO modu için **parola tabanlı oturum açma**.
6. Oturum açma URL'si için burada kullanıcıların kullanıcı adı ve uygulamanızı şirket ağı dışından oturum açmak için parola girin, sayfası URL'sini girin. Bu, uygulama proxy'si aracılığıyla uygulama yayımladığınızda, oluşturduğunuz dış URL olabilir. 

   ![Parola tabanlı oturum açma'yı seçin ve URL'nizi girin](./media/application-proxy-configure-single-sign-on-password-vaulting/password-sso.png)

7. **Kaydet**’i seçin.

<!-- Need to repro?
7. The page should tell you that a sign-in form was successfully detected at the provided URL. If it doesn't, select **Configure [your app name] Password Single Sign-on Settings** and choose **Manually detect sign-in fields**. Follow the instructions to point out where the sign-in credentials go. 
-->

## <a name="test-your-app"></a>Uygulamanızı test etme

Uygulamanız için uzaktan erişim için yapılandırılmış bir dış URL gidin. Bu uygulama (veya erişimle ayarladığınız bir test hesabının kimlik bilgilerini) için kimlik bilgilerinizle oturum. Başarıyla oturum açtıktan sonra uygulamayı bırakın ve kimlik bilgilerinizi yeniden girmeye gerek kalmadan geri dönün. 

## <a name="next-steps"></a>Sonraki adımlar

- Diğer yollarını uygulamak için okuma [çoklu oturum açma](what-is-single-sign-on.md)
- Hakkında bilgi edinin [uygulamaları Azure AD uygulama proxy'si ile uzaktan erişim için güvenlik konuları](application-proxy-security.md)