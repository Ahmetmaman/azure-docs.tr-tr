---
title: Azure AD uygulama ara sunucusu ile Uzak Masaüstü yayımlama | Microsoft Docs
description: Azure AD uygulama ara sunucusu bağlayıcıları ile ilgili temel bilgileri kapsar.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: barbkess
ms.custom: it-pro
ms.reviewer: harshja
ms.openlocfilehash: 61ac0d823322b919952b7ea426c447e070a09fc1
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2018
ms.locfileid: "39363205"
---
# <a name="publish-remote-desktop-with-azure-ad-application-proxy"></a>Azure AD uygulama ara sunucusu ile Uzak Masaüstü yayımlama

Uzak Masaüstü hizmetini ve Azure AD uygulama proxy'si, şirket ağına uzakta olan çalışanlarına verimliliğini artırmak için birlikte çalışır. 

Bu makale için hedef kitle aşağıdaki gibidir:
- Geçerli uygulama ara sunucusu Uzak Masaüstü Hizmetleri üzerinden şirket içi uygulamalar yayımlayarak kendi son kullanıcıları için daha fazla uygulama sunmak isteyen müşteriler.
- İsteyen geçerli Uzak Masaüstü Hizmetleri müşterileri, Azure AD uygulama proxy'si kullanarak bunların dağıtım saldırı yüzeyini azaltın. Bu senaryo için RDS'yi sınırlı sayıda iki aşamalı doğrulama ve koşullu erişim denetimleri sağlar.

## <a name="how-application-proxy-fits-in-the-standard-rds-deployment"></a>Uygulama proxy'si standart RDS dağıtımda nasıl uyduğunu

Standart bir RDS dağıtımının, Windows Server üzerinde çalışan çeşitli Uzak Masaüstü rol hizmetlerini kapsar. Bakarak [Uzak Masaüstü Hizmetleri mimarisi](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/desktop-hosting-logical-architecture), birden çok dağıtım seçenekleri vardır. Diğer RDS dağıtım seçenekleri aksine [Azure AD uygulama ara sunucusu ile bir RDS dağıtımının](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/desktop-hosting-logical-architecture) (Aşağıdaki diyagramda gösterilmiştir) bağlayıcı Hizmeti'ni çalıştıran sunucunun kalıcı giden bağlantı vardır. Diğer dağıtımlar, açık, load balancer üzerinden gelen bağlantılar bırakın.

![Uygulama proxy'si RDS VM ve genel internet yapmalarını sağlar.](./media/application-proxy-integrate-with-remote-desktop-services/rds-with-app-proxy.png)

Bir RDS dağıtımında, RD Web rolü ve RD Ağ Geçidi rolü Internet'e yönelik makineler üzerinde çalıştırın. Bu uç noktaları, aşağıdaki nedenlerden dolayı ortaya çıkar:
- RD Web kullanıcı oturum açmak ve çeşitli şirket içi uygulamalara ve masaüstlerine erişebilmek görüntülemek için genel bir uç nokta sağlar. Bir kaynak seçtikten sonra işletim sisteminde yerel uygulama kullanarak bir RDP bağlantısı oluşturulur.
- Bir kullanıcı ile RDP bağlantısı başlatır sonra RD Ağ Geçidi devreye. RD Ağ geçidi, internet üzerinden gelen şifrelenmiş RDP trafiği işler ve kullanıcının bağlandığı şirket içi sunucusuna çevirir. Bu senaryoda, RD Ağ Geçidi alma trafiği Azure AD uygulama proxy'si gelir.

>[!TIP]
>RDS önce dağıtılan yapmadıysanız veya başlamadan önce daha fazla bilgiye ihtiyacınız varsa, bilgi nasıl [sorunsuz bir şekilde Azure Resource Manager ve Azure Market ile RDS dağıtma](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/rds-in-azure).

## <a name="requirements"></a>Gereksinimler

- Uygulama proxy'si web istemcisi desteklemediğinden, Uzak Masaüstü web istemcisi dışındaki bir istemci kullanın.

- RD Web ve RD Ağ Geçidi uç noktaları, aynı makinede ve bir ortak kök bulunmalıdır. Böylece iki uygulama arasındaki çoklu oturum açma deneyimini olabilir RD Web ve RD Ağ geçidi, uygulama ara sunucusu ile tek bir uygulama olarak yayımlanır.

- Zaten olmalıdır [RDS dağıtılan](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/rds-in-azure), ve [uygulama Proxy etkin](application-proxy-enable.md).

- Bu senaryo, son kullanıcılarınızın RD Web sayfası aracılığıyla bağlanan Windows 7 veya Windows 10 Masaüstü cihazlarda Internet Explorer aracılığıyla Git varsayar. Diğer işletim sistemleri için destek gerekiyorsa, bkz: [desteklemek için diğer istemci yapılandırmaları](#support-for-other-client-configurations).

- RD Web yayımlarken, aynı iç ve dış FQDN kullanmanız önerilir. Ardından iç ve dış FQDN'leri farklıysa istek üst bilgisi geçersiz bağlantıları alma istemcinin önlemek için çeviri devre dışı bırakmalısınız. 

- Internet Explorer, RDS ActiveX eklentiyi etkinleştirin.

## <a name="deploy-the-joint-rds-and-application-proxy-scenario"></a>Birleşik RDS ve uygulama proxy'si senaryo dağıtma

RDS ve ortamınız için Azure AD uygulama proxy'si ayarladıktan sonra iki çözümü birleştirmek için adımları izleyin. Bu adımları iki web'e yönelik RDS uç (RD Web ve RD Ağ Geçidi) yayımlama ve ardından uygulama proxy'si aracılığıyla Git, RDS üzerinde trafiği yönlendiren yol.

### <a name="publish-the-rd-host-endpoint"></a>RD konak uç nokta yayımlama

1. [Yeni bir uygulama proxy'si uygulaması yayımlama](application-proxy-publish-azure-portal.md) aşağıdaki değerlerle:
   - İç URL: https://\<rdhost\>.com / Burada \<rdhost\> RD Web ve RD Ağ Geçidi paylaşan ortak kökü.
   - Dış URL: Bu alan otomatik olarak uygulama adına göre doldurulur, ancak bunu değiştirebilirsiniz. RDS'yi eriştiklerinde, kullanıcılarınızın bu URL'ye geçer
   - Ön kimlik doğrulama yöntemi: Azure Active Directory
   - URL üst bilgileri çevir: Hayır
2. Kullanıcılar, yayımlanmış RD uygulamaya atayın. Tüm bunlar çok RDS, erişimi olduğundan emin olun.
3. Çoklu oturum açma yöntemi uygulamanın bırakın **Azure AD çoklu oturum açma devre dışı**. Kullanıcılarınızın Azure AD için bir kez de RD Web kimlik doğrulaması, ancak çoklu oturum açma RD ağ geçidine sahip istenir.
4. Git **Azure Active Directory** > **uygulama kayıtları** > *uygulamanızı* > **ayarları**.
5. Seçin **özellikleri** ve güncelleştirme **giriş sayfası URL'si** RD Web uç noktanıza işaret edecek şekilde alan (https:// gibi\<rdhost\>.com/RDWeb).

### <a name="direct-rds-traffic-to-application-proxy"></a>Uygulama proxy'si doğrudan RDS trafiği

Bir RDS dağıtımının yönetici olarak bağlanın ve dağıtım için RD Ağ Geçidi sunucu adını değiştirebilirsiniz. Bu yapılandırma, bağlantılar Azure AD uygulama proxy'si hizmeti aracılığıyla Git sağlar.

1. Çalıştıran RD Bağlantı Aracısı rol RDS sunucuya bağlanın.
2. Başlatma **Sunucu Yöneticisi**.
3. Seçin **Uzak Masaüstü Hizmetleri** sol bölmesinden.
4. **Genel Bakış**’ı seçin.
5. Dağıtıma genel bakış bölümünde, aşağı açılan menüyü seçip **dağıtım özelliklerini düzenleme**.
6. RD Ağ Geçidi sekmede değiştirme **sunucu adı** uygulama proxy'sinde RD konak uç noktası için ayarladığınız dış URL alanı.
7. Değişiklik **oturum açma yöntemi** alanı **parola kimlik doğrulaması**.

  ![Dağıtım özellikleri RDS ekranda](./media/application-proxy-integrate-with-remote-desktop-services/rds-deployment-properties.png)

8. Her koleksiyon için bu komutu çalıştırın. Değiştirin *\<yourcollectionname\>* ve *\<proxyfrontendurl\>* kendi bilgilerinizle. Bu komut, RD Web ve RD Ağ Geçidi arasında çoklu oturum açmayı etkinleştirir ve performansı en iyi duruma getirir:

   ```
   Set-RDSessionCollectionConfiguration -CollectionName "<yourcollectionname>" -CustomRdpProperty "pre-authentication server address:s:<proxyfrontendurl>`nrequire pre-authentication:i:1"
   ```

   **Örneğin:**
   ```
   Set-RDSessionCollectionConfiguration -CollectionName "QuickSessionCollection" -CustomRdpProperty "pre-authentication server address:s:https://remotedesktoptest-aadapdemo.msappproxy.net/`nrequire pre-authentication:i:1"
   ```

9. Bu koleksiyon için RDWeb indirilecek RDP dosya içeriğini görüntülemek yanı sıra özel RDP özellikleri değişikliği doğrulamak için aşağıdaki komutu çalıştırın:
    ```
    (get-wmiobject -Namespace root\cimv2\terminalservices -Class Win32_RDCentralPublishedRemoteDesktop).RDPFileContents
    ```

Uzak Masaüstü yapılandırdığınıza göre Azure AD uygulama proxy'si RDS'yi internet'e yönelik bileşeni olarak gerçekleştirdiği RD Web ve RD Ağ Geçidi makinelerinizde diğer ortak internet'e yönelik uç noktalar kaldırabilirsiniz.

## <a name="test-the-scenario"></a>Test senaryosu

Windows 7 veya 10 bilgisayarda Internet Explorer ile senaryoyu test edin.

1. Dış URL ayarladığınız gidin veya uygulamanızda bulabilirsiniz [MyApps paneli](https://myapps.microsoft.com).
2. Azure Active Directory kimlik doğrulaması istenir. Uygulamaya atanmış bir hesap kullanın.
3. RD Web kimlik doğrulaması istenir.
4. RDS kimlik Doğrulamanızın başarılı olduktan sonra Masaüstü veya istediğiniz uygulamayı seçin ve çalışmaya başlayın.

## <a name="support-for-other-client-configurations"></a>Diğer istemci yapılandırmaları için destek

Bu makalede açıklanan yapılandırma Windows 7 veya 10, Internet Explorer ve RDS ActiveX eklenti sahip kullanıcılara yöneliktir. Ancak, gerekirse, diğer işletim sistemleri veya tarayıcılar destekleyebilir. Kullandığınız kimlik doğrulama yöntemi farktır.

| Kimlik doğrulama Yöntemi | Desteklenen istemci yapılandırması |
| --------------------- | ------------------------------ |
| Kimlik doğrulaması öncesi    | Windows 7/10 kullanarak Internet Explorer + RDS ActiveX eklentisi |
| Geçiş | Tüm diğer Microsoft Uzak Masaüstü uygulamasını destekleyen işletim sistemi |

Ön kimlik doğrulaması akışı geçiş akışı değerinden daha fazla güvenlik avantaj sunar. Ön kimlik doğrulama ile şirket içi kaynaklarınız için çoklu oturum açma, koşullu erişim ve iki aşamalı doğrulama gibi Azure AD kimlik doğrulama özellikleri kullanabilirsiniz. Ayrıca, yalnızca trafik ulaştığında, ağ kimliği doğrulanmış emin olun.

Geçişli kimlik doğrulamasını kullanmak için bu makalede listelenen adımları yalnızca iki değişiklik vardır:
1. İçinde [RD konak uç nokta yayımlama](#publish-the-rd-host-endpoint) 1. adımda, ön kimlik doğrulama yöntemi kümesine **geçiş**.
2. İçinde [uygulama proxy'si doğrudan RDS trafiği](#direct-rds-traffic-to-application-proxy), tamamen 8. adımına atlayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[Azure AD uygulama ara sunucusu ile SharePoint uzaktan erişimi etkinleştirme](application-proxy-integrate-with-sharepoint-server.md)  
[Azure AD uygulama proxy'si kullanarak uygulamalara uzaktan erişim için güvenlik konuları](application-proxy-security.md)
