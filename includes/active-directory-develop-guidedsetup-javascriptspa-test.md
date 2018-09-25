---
title: include dosyası
description: include dosyası
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: mtillman
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/17/2018
ms.author: nacanuma
ms.custom: include file
ms.openlocfilehash: b2950720ae7038f435a8e2dfb24333afc49984b1
ms.sourcegitcommit: 715813af8cde40407bd3332dd922a918de46a91a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47060665"
---
## <a name="test-your-code"></a>Kodunuzu test etme

### <a name="test-with-node"></a>Düğüm ile test
Visual Studio kullanmıyorsanız, web sunucunuza başlatıldığından emin olun.
1. Sunucusunu konumunu temel alarak bir TCP bağlantı noktasını dinlemek üzere yapılandırmak, **index.html** dosya. Düğüm için uygulama klasöründeki bir komut satırı isteminde aşağıdaki komutları çalıştırarak bağlantı noktasını dinlemek için web sunucusunu başlatın:

    ```bash
    npm install
    node server.js
    ```
1. Tarayıcıyı açın ve http:// yazın<span></span>localhost:30662 veya http://<span></span>localhost: {port} burada **bağlantı noktası** , web sunucusunun dinleme yaptığı bağlantı noktası. İndex.html dosyanızın içeriğini görmeniz gerekir ve **oturum** düğmesi.

<p/><!-- -->

### <a name="test-with-visual-studio"></a>Visual Studio ile test
Visual Studio kullanıyorsanız, tuşuna basın ve proje çözüm seçtiğinizden emin olun **F5** projeyi çalıştırın. Http:// tarayıcı açılır<span></span>localhost: {port} konumu göreceksiniz **oturum** düğmesi.


## <a name="test-your-application"></a>Uygulamanızı test edin

Tarayıcı, bir index.html dosyası yüklendikten sonra tıklayın **oturum**. Microsoft Azure Active Directory (Azure AD) v2.0 uç noktası ile oturum açmanız istenir:

![JavaScript SPA'ya hesabınızda oturum açın](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptspascreenshot1.png)


### <a name="provide-consent-for-application-access"></a>Uygulama erişimi için rıza sağlamanın

Uygulamanıza oturum ilk kez profilinizi erişmek ve oturum açmak için uygulama izin vermek için onay vermeniz istenir:

![Uygulama erişimi için izninizi sağlayın](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptspaconsent.png)

### <a name="view-application-results"></a>Uygulama sonuçlarını görüntüle
Sonra oturum açma işlemlerini sayfasında gösterilen Microsoft Graph API yanıtında döndürülen kullanıcı profili bilgilerinize görmeniz gerekir.

![Microsoft Graph API çağrısından beklenen sonuçları](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptsparesults.png)

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Kapsamlar ve temsilci izinleri hakkında daha fazla bilgi

Microsoft Graph API'sini gerektirir `user.read` kapsamı, bir kullanıcının profilini okuma için. Bu kapsam kayıt portalı üzerinde kayıtlı her uygulamada varsayılan olarak otomatik olarak eklenir. Diğer Microsoft Graph API'leri yanı sıra özel API'ler, arka uç sunucusu için ek kapsamlarla gerektirebilir. Örneğin, Microsoft Graph API'sini gerektirir `Calendars.Read` kullanıcının takvimleri listelemek için kapsam.

Bir uygulama bağlamında kullanıcının takvimler erişmek için ekleme `Calendars.Read` izin uygulama kayıt bilgileri için temsilci. Ardından, ekleme `Calendars.Read` için kapsam `acquireTokenSilent` çağırın.

>[!NOTE]
>Kapsamların sayısı arttıkça, kullanıcı için ek bir onayları istenebilir.

Arka uç API (önerilmez) bir kapsam gerektirmiyorsa kullanabileceğiniz `clientId` belirteçlerini almak için çağrıları kapsamda olarak.

<!--end-collapse-->

[!INCLUDE [Help and support](./active-directory-develop-help-support-include.md)]
