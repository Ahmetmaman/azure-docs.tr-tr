---
title: Azure Active Directory koşullu erişim erişim denetimleri nelerdir? | Microsoft Docs
description: Erişimi Azure Active Directory koşullu erişim işlerinde nasıl denetimleri hakkında bilgi edinin.
services: active-directory
keywords: uygulamalara koşullu erişim, Azure AD ile koşullu erişim, şirket kaynaklarına güvenli erişim, koşullu erişim ilkeleri
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.component: conditional-access
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/18/2018
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 065eee57b78ea10d5e8079de3a938dc00dd60ee2
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2018
ms.locfileid: "49406781"
---
# <a name="what-are-access-controls-in-azure-active-directory-conditional-access"></a>Azure Active Directory koşullu erişim erişim denetimleri nelerdir? 

İle [Azure Active Directory (Azure AD) koşullu erişim](../active-directory-conditional-access-azure-portal.md), bulut uygulamalarınızı nasıl yetkili kullanıcılara erişimi denetleyebilirsiniz. Bir koşullu erişim ilkesinde ("Bu durumda") ilkeniz tetikleme nedeni için ("bunu") yanıtı tanımlayın. 

![Denetim](./media/controls/10.png)


Koşullu erişim bağlamında 

- "**Bu durumda**" olarak adlandırılır **koşulları**

- "**Bunu yapmak**" olarak adlandırılır **erişim denetimleri**


Bir koşul deyimi, denetimleri ile koşullu erişim ilkesi temsil eder.

![Denetim](./media/controls/61.png)

Kişi tarafından yerine getirilmesi gereken bir gereksinim ya da sistem oturum açarken her denetimidir ve hangi kullanıcının bir kısıtlama oturum açtıktan sonra yapabilirsiniz. 

Denetimlerin iki tür vardır: 

- **İzin verme denetimleri** - kapı erişimi

- **Oturum denetimleri** - oturum içindeki erişimi kısıtlamak için

Bu konuda, Azure AD koşullu erişim kullanılabilen çeşitli denetimleri açıklanmaktadır. 

## <a name="grant-controls"></a>İzin verme denetimleri

Verme denetimleri sayesinde, erişimi tamamen engelleme veya istenen denetimleri seçerek ek gereksinimler ile erişim izni. Birden fazla denetim için gerektirebilir:

- Tüm seçilen yerine getirilmesi için denetimleri (*ve*) 
- Seçili tek getirilmesi için denetimi (*veya*)

![Denetim](./media/controls/17.png)



### <a name="multi-factor-authentication"></a>Multi-factor authentication

Bu denetim, belirtilen bulut uygulamasına erişmek için çok faktörlü kimlik doğrulama isteyecek şekilde kullanabilirsiniz. Bu denetimi aşağıdaki multi-Factor Authentication sağlayıcılarını destekler: 

- Azure Multi-Factor Authentication 

- Active Directory Federasyon Hizmetleri (AD FS) ile birlikte bir şirket içi multi-Factor authentication sağlayıcısı.
 
Multi-Factor authentication kullanarak, geçerli kullanıcının birincil kimlik bilgilerini erişim elde etmiştir yetkisiz bir kullanıcı tarafından erişilen kaynaklar korunmasına yardımcı olur.



### <a name="compliant-device"></a>Uyumlu cihaz

Cihaz tabanlı koşullu erişim ilkelerini yapılandırabilirsiniz. Cihaz tabanlı koşullu erişim ilkesinin amacı yalnızca yapılandırılmış kaynakların erişim sağlamaktır [yönetilen cihazlar](require-managed-devices.md). Uyumlu cihaz gerektiren bir seçenektir tanımlamak için sahip olduğunuz bir yönetilen cihaz. Bu seçenek belirlenirse, koşullu erişim ilkenizi olan cihazları ile yapılan erişimi için erişim girişimlerini verir [kayıtlı](../devices/overview.md) için Azure Active Directory ve bu işaretlenmiş veya (herhangi bir cihaz için işletim sistemi) Intune tarafından uyumlu, Windows 10 cihazları için üçüncü taraf MDM Sistemi. Windows 10 dışında cihaz işletim sistemi türleri için üçüncü taraf MDM sistemleri desteklenmez.

Daha fazla bilgi için [Azure Active Directory cihaz tabanlı koşullu erişim ilkeleri ayarlama](require-managed-devices.md).

### <a name="hybrid-azure-ad-joined-device"></a>Hibrit Azure AD'ye katılmış

Karma Azure AD'ye katılmış cihaz tabanlı koşullu erişim ilkelerini yapılandırmak için sahip olduğunuz başka bir seçenek olan gerek. Bu gereksinim, Windows Masaüstü, dizüstü bilgisayarlar ve bir şirket içi Active Directory'ye katılmış Kurumsal tabletler ifade eder. Bu seçenek belirlenirse, koşullu erişim ilkenizi, şirket içi Active Directory'niz ve Azure Active Directory'nize katılmış cihazları yapılan erişimi için erişim girişimlerini verir.  

Daha fazla bilgi için [Azure Active Directory cihaz tabanlı koşullu erişim ilkeleri ayarlama](require-managed-devices.md).





### <a name="approved-client-app"></a>Onaylı istemci uygulaması

Çalışanlarınız hem kişisel mobil cihazları ve çalışma görevlerini kullandığından, durumda bile nerede bunlar sizin tarafınızdan yönetilmeyen cihazları kullanarak erişilen şirket verilerini koruma özelliğine sahip olmak isteyebilirsiniz.
Kullanabileceğiniz [Intune uygulama koruma ilkeleri](https://docs.microsoft.com/intune/app-protection-policy) tüm mobil cihaz Yönetimi (MDM) çözümünden bağımsız Şirketinizin verilerini korumaya yardımcı olmak için.


Onaylı istemci uygulamaları ile bulut uygulamalarınızı destekleyecek şekilde erişmeyi denediği bir istemci uygulaması gerektirebilir [Intune uygulama koruma ilkeleri](https://docs.microsoft.com/intune/app-protection-policy). Örneğin, erişimi Exchange Online için Outlook uygulamasında kısıtlayabilirsiniz. Onaylı istemci uygulamalarını gerektiren bir koşullu erişim ilkesi de denir [uygulama tabanlı koşullu erişim ilkesi](app-based-conditional-access.md). Desteklenen onaylı istemci uygulamalarının listesi için bkz. [onaylı istemci uygulaması gereksinimi](technical-reference.md#approved-client-app-requirement).


### <a name="terms-of-use"></a>Kullanım Koşulları

Bir kullanıcı bir kaynağa erişim izni verilmeden önce kullanım koşullarını kabul kiracınızdaki gerektirebilir. Yönetici olarak, yapılandırma ve kullanım koşullarını PDF'yi karşıya yükleyerek özelleştirin. Bir kullanıcı kalırsa bu denetimi bir uygulamaya erişim kapsamını yalnızca kullanım koşullarını kabul verilir. 


### <a name="custom-controls-preview"></a>Özel denetimler (önizleme) 

Kullanıcılarınızın daha fazla Azure Active Directory dışında gereksinimlerini karşılamak için uyumlu bir hizmete yönlendirin koşullu erişimde özel denetimler oluşturabilirsiniz. Bu, belirli harici çok faktörlü kimlik doğrulaması ve doğrulama sağlayıcıları koşullu erişim kurallarını zorunlu tutmak için ya da kendi özel hizmet oluşturmak için kullanmayı sağlar. Bu denetimi gerçekleştirmek için bir kullanıcının tarayıcı dış hizmete yönlendirilir, gerekli herhangi bir kimlik doğrulaması veya doğrulama etkinliklerini gerçekleştirir ve ardından Azure Active Directory'ye yönlendirilir. Kullanıcı başarıyla kimlik doğrulaması veya doğrulanan kullanıcı koşullu erişim akışı devam eder. 

## <a name="custom-controls"></a>Özel denetimler

Özel denetimler, Azure Active Directory Premium P1 edition özellikleridir. Özel denetimleri kullanarak, kullanıcılarınızın daha fazla Azure Active Directory dışında gereksinimlerini karşılamak için uyumlu bir hizmete yönlendirilir. Bu denetimi gerçekleştirmek için bir kullanıcının tarayıcı dış hizmete yönlendirilir, gerekli herhangi bir kimlik doğrulaması veya doğrulama etkinliklerini gerçekleştirir ve ardından Azure Active Directory'ye yönlendirilir. Azure Active Directory yanıt doğrular ve kullanıcı başarıyla kimlik doğrulaması veya doğrulanan kullanıcı koşullu erişim akışı devam eder.

Bu denetimler belirli dış veya özel hizmetler koşullu erişim denetimleri olarak kullanılmasına izin verin ve genellikle koşullu erişim özelliklerini genişleten.

Şu anda uyumlu bir hizmet teklifi sağlayıcıları içerir:

- [Duo güvenlik](https://duo.com/docs/azure-ca)
- [Entrust Datacard](https://www.entrustdatacard.com/products/authentication/intellitrust)
- [Ping Identity](https://documentation.pingidentity.com/pingid/pingidAdminGuide/index.shtml#pid_c_AzureADIntegration.html)
- RSA
- [Trusona](https://www.trusona.com/docs/azure-ad-integration-guide)

Bu hizmetler hakkında daha fazla bilgi için sağlayıcı doğrudan başvurun.

### <a name="creating-custom-controls"></a>Özel denetimler oluşturma

Özel bir denetim oluşturmak için kullanmak istediğiniz sağlayıcıyı başvurmalısınız. Her bir Microsoft dışı sağlayıcısı, kendi işlem ve kaydolun, abone olmanızı veya aksi halde hizmetin bir parçası haline gelir ve koşullu erişim ile tümleştirmek istediğiniz belirtmek için gereksinimleri vardır. Bu noktada, sağlayıcı, JSON biçiminde bir veri bloğunu sağlayacak. Bu veri sağlayıcısı ve koşullu erişim, kiracınız için birlikte çalışmasına olanak sağlar, yeni denetimi oluşturur ve koşullu erişimi tanımlar, kullanıcılarınızın doğrulama sağlayıcısı ile başarıyla gerçekleştirdiniz varsa söyleyebilirsiniz.

JSON verilerini kopyalayın ve ardından ilgili metin kutusuna yapıştırın. Açıkça hale getirildiği değişiklik anlamadan JSON herhangi bir değişiklik yapmayın. Herhangi bir değişiklik yapmadan, sağlayıcısı ile Microsoft arasındaki bağlantıyı kesin ve potansiyel olarak siz ve kullanıcılarınız hesaplarınızı dışında kilitleyin.

Özel bir denetim oluşturmak için seçeneği **Yönet** bölümünü **koşullu erişim** sayfası.

![Denetim](./media/controls/82.png)

Tıklayarak **yeni özel denetim**, bir metin kutusu denetiminizin JSON verilerini içeren bir dikey pencere açılır.  


![Denetim](./media/controls/81.png)


### <a name="deleting-custom-controls"></a>Özel denetim siliniyor

Özel denetim silmek için önce tüm koşullu erişim ilkesinde kullanılmadığından emin olmalısınız. Bir kez tamamlayın:

1. Özel denetimleri listesine Git

2. Tıklayın...  

3. **Sil**’i seçin.

### <a name="editing-custom-controls"></a>Özel denetimleri düzenleme

Özel denetim düzenlemek için geçerli denetim silin ve güncelleştirilmiş bilgileri ile yeni bir denetim oluşturmak gerekir.




## <a name="session-controls"></a>Oturum denetimleri

Oturum denetimleri, bulut uygulaması içinde sınırlı deneyim sağlar. Oturum denetimleri, bulut uygulamaları tarafından zorunlu tutulmaz ve oturumla ilgili uygulamayı Azure AD'ye tarafından sağlanan ek bilgiler dayanır.

![Denetim](./media/controls/31.png)

### <a name="use-app-enforced-restrictions"></a>Uygulama tarafından zorlanan kısıtlamaları kullan

Bu denetim, cihaz bilgilerini bulut uygulamasına geçirmek Azure AD zorunlu tutmak için kullanabilirsiniz. Bu, kullanıcı bir cihaz uyumlu veya etki alanına katılmış CİHAZDAN geliyorsa bilmeniz bulut uygulaması yardımcı olur. Bu denetimidir şu anda yalnızca bulut uygulaması olarak SharePoint ile desteklenir. SharePoint cihaz bilgilerini kullanıcılara cihaz durumuna bağlı olarak sınırlı veya tam bir deneyim sağlamak için kullanır.
SharePoint ile sınırlı erişim isteme hakkında daha fazla bilgi için bkz: [yönetilmeyen cihazların erişimini denetleme](https://aka.ms/spolimitedaccessdocs).



## <a name="next-steps"></a>Sonraki adımlar

- Koşullu erişim ilkesi yapılandırmak için bkz. nasıl bilmek istiyorsanız [Azure Active Directory koşullu erişimiyle belirli uygulamalar için MFA gerektiren](app-based-mfa.md).

- Ortamınızda koşullu erişim ilkelerini yapılandırmaya hazırsanız bkz. [Azure Active Directory'de koşullu erişim için en iyi yöntemler](best-practices.md). 
