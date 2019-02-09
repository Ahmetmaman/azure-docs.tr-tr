---
title: Uzaktan erişim için şirket içi uygulamalar - Azure Active Directory Uygulama proxy'si | Microsoft Docx
description: Azure Active Directory Uygulama proxy'si, şirket içi web uygulamalarına güvenli uzaktan erişim sağlar. Bir çoklu oturum açma sonra Azure ad, kullanıcılar hem buluttaki hem de şirket içi uygulamaları dış URL veya bir iç uygulama portal erişebilir. Örneğin, uygulama ara sunucusu uzaktan erişim ve çoklu oturum açma Uzak Masaüstü, SharePoint, Teams, Tableau, Qlik ve iş kolu (LOB) uygulamaları için sağlayabilir.
services: active-directory
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 01/07/2019
ms.author: celested
ms.reviewer: japere
ms.openlocfilehash: b96ffcc7f131e54a0816875250cd10cd42c9af52
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55962001"
---
# <a name="remote-access-to-on-premises-applications-through-azure-active-directorys-application-proxy"></a>Azure Active Directory Uygulama proxy'si aracılığıyla şirket içi uygulamalara uzaktan erişim 

Azure Active Directory Uygulama proxy'si, şirket içi web uygulamalarına güvenli uzaktan erişim sağlar. Bir çoklu oturum açma sonra Azure ad, kullanıcılar hem buluttaki hem de şirket içi uygulamaları dış URL veya bir iç uygulama portal erişebilir. Örneğin, uygulama ara sunucusu uzaktan erişim ve çoklu oturum açma Uzak Masaüstü, SharePoint, Teams, Tableau, Qlik ve iş kolu (LOB) uygulamaları için sağlayabilir.

Azure AD uygulama ara sunucusu aşağıdaki gibidir:

- **Kullanımı kolaydır**. Kullanıcılar, şirket içi uygulamalarınızı, O365 ve Azure AD ile tümleştirilmiş diğer SaaS uygulamalarına eriştikleri aynı şekilde erişebilir. Değiştirmeniz veya uygulamalarınızı uygulaması Ara sunucusu ile çalışacak şekilde güncelleştirmeniz gerekmez. 

- **Güvenli**. Şirket içi uygulamaları, Azure'nın yetkilendirme denetimlerinden ve güvenlik analizinden kullanabilir. Örneğin, şirket içi uygulamalara koşullu erişim ve iki aşamalı doğrulama kullanabilirsiniz. Uygulama Ara sunucusu, güvenlik duvarı üzerinden gelen bağlantı açmanız gerekmez.
 
- **Uygun maliyetli**. Şirket içi çözümlerde genellikle ayarlayıp (DMZ'ler) arındırılmış bölge, kenar sunucu veya diğer karmaşık altyapı kurmanız gerekir. Uygulama proxy'si, kullanımı kolay hale getiren bulutta çalışır. Uygulama proxy'si kullanmak için ağ altyapısını değiştirmek veya ek cihazları, şirket içi ortamınızda yüklemek gerekmez.

## <a name="what-is-application-proxy"></a>Uygulama Ara sunucusu nedir?
Uygulama proxy'si, kullanıcıların şirket içi web uygulamalarına uzak istemciden erişmesine olanak sağlayan Azure ad için kullanılan bir özelliktir. Uygulama proxy'si, bulutta çalışan uygulama proxy'si hizmeti hem bir şirket içi sunucu üzerinde çalışan uygulama Proxy Bağlayıcısı içerir. Azure AD uygulama proxy'si hizmeti ve birlikte güvenli bir şekilde oturum açma kullanıcı belirteci Azure AD'den web uygulamasına geçirmek için uygulama ara sunucusu Bağlayıcısı iş.

Uygulama proxy'si ile çalışır:

* Web kullanan uygulamaları [tümleşik Windows kimlik doğrulaması](application-proxy-configure-single-sign-on-with-kcd.md) kimlik doğrulaması  
* Web form tabanlı kullanan uygulamalar veya [üst bilgi tabanlı](application-proxy-configure-single-sign-on-with-ping-access.md) erişim  
* Web için zengin uygulamalar farklı cihazlarda kullanıma sunmak istiyorsanız API'leri  
* Arkasında barındırılan uygulamalarını bir [Uzak Masaüstü Ağ Geçidi](application-proxy-integrate-with-remote-desktop-services.md)  
* Active Directory Authentication Library (ADAL) ile tümleşik olan zengin istemci uygulamaları

Uygulama Ara sunucusu, çoklu oturum açmayı destekler. Desteklenen yöntemler hakkında daha fazla bilgi için bkz. [tek bir oturum açma yöntemini seçme](what-is-single-sign-on.md#choosing-a-single-sign-on-method).

## <a name="how-application-proxy-works"></a>Uygulama proxy'si nasıl çalışır?

Aşağıdaki diyagramda gösterildiği Azure AD ve uygulama proxy'si, şirket içi uygulamalar için çoklu oturum açma sağlamak için birlikte çalışır.

![AzureAD uygulama proxy'si diyagramı](./media/application-proxy/azureappproxxy.png)

1. Kullanıcı, bir uç noktası aracılığıyla uygulama eriştiğini sonra kullanıcının Azure AD oturum açma sayfasına yönlendirilir. 
2. Bir başarılı oturum açma işleminden sonra Azure AD kullanıcının istemci cihaza bir belirteç gönderir.
3. İstemci belirteci güvenlik asıl adı (SPN) ve kullanıcı asıl adı (UPN) belirteçten alır. uygulama ara Sunucusu hizmetine gönderir. Uygulama proxy'si, daha sonra uygulama Proxy Bağlayıcısı için isteği gönderir.
4. Çoklu oturum açma yapılandırdıysanız, bağlayıcı kullanıcı adına gerekli tüm ek kimlik doğrulaması gerçekleştirir.
5. Bağlayıcıyı şirket içi uygulamaya isteği gönderir.  
6. Yanıt, kullanıcıya bağlayıcı ve uygulama proxy'si hizmeti aracılığıyla gönderilir.

| Bileşen | Açıklama |
| --------- | ----------- |
| Uç Nokta  | Uç nokta bir URL olduğundan veya bir [son kullanıcı portalı](end-user-experiences.md). Kullanıcılar, uygulamalar, Ağ üzerindeyken dışında bir dış URL erişerek ulaşabilirsiniz. Ağınızdaki kullanıcılar, bir URL veya bir son kullanıcı portalı uygulamaya erişebilir. Kullanıcılar bu uç noktaları birine gittiğinizde, Azure AD'de kimlik doğrulaması yapmak ve şirket içi uygulama Bağlayıcısı üzerinden yönlendirilir.|
| Azure AD | Azure AD, bulutta depolanan Kiracı dizinini kullanarak kimlik doğrulaması gerçekleştirir. |
| Uygulama proxy'si hizmeti | Bu uygulama proxy'si hizmeti Azure AD parçası olarak bulutta çalışır. Kullanıcı oturum açma belirteci için uygulama ara sunucusu Bağlayıcısı geçirir. Uygulama proxy'si, istekte erişilebilir üst bilgileri iletir ve kendi protokol başına üstbilgileri için istemci IP adresini ayarlar. Gelen istek için proxy bu başlığı zaten varsa, istemci IP adresi üstbilgisinin değerini virgülle ayrılmış liste sonuna eklenir.|
| Uygulama Ara sunucusu Bağlayıcısı | Bağlayıcı ağınızdaki bir Windows Server üzerinde çalışan basit bir aracıdır. Bağlayıcıyı şirket içi uygulama ile bulutta uygulama proxy'si hizmeti arasındaki iletişimi yönetir. Yalnızca bağlayıcı giden bağlantılar kullanır, gelen bağlantı noktalarının açık veya her şeyi bir DMZ'ye yerleştirmeniz gerekmez. Bağlayıcılar, durum bilgisiz olduğundan ve gerektiği şekilde buluttan bilgi isteme. Nasıl gibi bağlayıcılar hakkında daha fazla bilgi için Yük Dengeleme ve kimlik doğrulaması için bkz [anlamak Azure AD uygulama ara sunucusu bağlayıcıları](application-proxy-connectors.md).|
| Active Directory (AD) | Active Directory etki alanı hesapları için kimlik doğrulaması gerçekleştirmek üzere şirket içinde çalıştırır. Bağlayıcı, çoklu oturum açma yapılandırıldığında, gereken ek kimlik doğrulaması gerçekleştirmek için AD ile iletişim kurar.
| Şirket içi uygulama | Son olarak, kullanıcının şirket içi uygulamaya erişebilir. 

## <a name="next-steps"></a>Sonraki adımlar
Uygulama proxy'sini kullanmaya başlamak için bkz: [Öğreticisi: Uygulama proxy'si aracılığıyla uzaktan erişim için şirket içi uygulama ekleme](application-proxy-add-on-premises-application.md). 

En son haberler ve güncelleştirmeler için bkz. [uygulama ara sunucusu bloguna](https://blogs.technet.com/b/applicationproxyblog/)


