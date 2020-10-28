---
title: SaaS uygulamalarına erişimi yönetmek için kiracı kısıtlamalarını kullanma-Azure AD
description: Hangi kullanıcıların Azure AD kiracısına bağlı olarak uygulamalara erişebileceğini yönetmek için kiracı kısıtlamalarını kullanma.
services: active-directory
author: kenwith
manager: celestedg
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 10/26/2020
ms.author: kenwith
ms.reviewer: hpsin
ms.collection: M365-identity-device-management
ms.openlocfilehash: ce96eb5e91ccc4cb9f69711f9e6fd8fd59ce65bc
ms.sourcegitcommit: 4cb89d880be26a2a4531fedcc59317471fe729cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2020
ms.locfileid: "92669941"
---
# <a name="use-tenant-restrictions-to-manage-access-to-saas-cloud-applications"></a>SaaS bulut uygulamalarına erişimi yönetmek için kiracı kısıtlamalarını kullanın

Güvenliği vurguladığınız büyük kuruluşlar, Microsoft 365 gibi bulut hizmetlerine geçmek ister, ancak kullanıcıların yalnızca onaylanan kaynaklara erişip erişebileceklerini bilmeleri gerekir. Geleneksel olarak, şirketler erişimi yönetmek istediklerinde etki alanı adlarını veya IP adreslerini kısıtlar. Bu yaklaşım, hizmet olarak yazılım (veya SaaS) uygulamalarının ortak bir bulutta barındırıldığı, [Outlook.Office.com](https://outlook.office.com/) ve [login.microsoftonline.com](https://login.microsoftonline.com/)gibi paylaşılan etki alanı adlarında çalıştırılan bir dünyada başarısız olur. Bu adreslerin engellenmesi, kullanıcıların Web üzerinde Outlook 'U yalnızca onaylanan kimlikler ve kaynaklarla kısıtlamak yerine tamamen web üzerinde erişimini engeller.

Bu zorluk Azure Active Directory (Azure AD) çözümü, kiracı kısıtlamaları adlı bir özelliktir. Kuruluşlar, kiracı kısıtlamaları ile, uygulamalar çoklu oturum açma için kullanılan Azure AD kiracısına göre SaaS bulut uygulamalarına erişimi denetleyebilir. Örneğin, kuruluşunuzun Microsoft 365 uygulamalarına erişime izin vermek isteyebilirsiniz, bu da aynı uygulamalardaki diğer kuruluşların örneklerine erişimi önler.  

Kiracı kısıtlamalarıyla kuruluşlar, kullanıcılarının erişmelerine izin verilen kiracılar listesini belirtebilir. Azure AD bundan sonra yalnızca izin verilen kiracılar için erişim izni verir.

Bu makale Microsoft 365 için kiracı kısıtlamalarına odaklanır, ancak bu özellik, Azure AD ile çoklu oturum açma için modern kimlik doğrulama protokolleri kullanan tüm SaaS bulut uygulamaları ile birlikte çalışmalıdır. Microsoft 365 tarafından kullanılan kiracıdan farklı bir Azure AD kiracısıyla SaaS uygulamaları kullanıyorsanız, gerekli tüm kiracılara izin olduğundan emin olun. SaaS bulut uygulamaları hakkında daha fazla bilgi için bkz. [market Active Directory](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.AzureActiveDirectory).

## <a name="how-it-works"></a>Nasıl çalışır?

Genel çözüm aşağıdaki bileşenleri içerir:

1. **Azure AD** : varsa `Restrict-Access-To-Tenants: <permitted tenant list>` , Azure AD izin verilen kiracılar için yalnızca güvenlik belirteçleri verir.

2. **Şirket içi proxy sunucu altyapısı** : Bu altyapı, aktarım katmanı GÜVENLIĞI (TLS) denetimi yapabilen bir ara sunucu aygıtıdır. Azure AD 'ye yönelik trafiğe izin verilen kiracılar listesini içeren üstbilgiyi eklemek için proxy 'yi yapılandırmanız gerekir.

3. **İstemci yazılımı** : kiracı kısıtlamalarını desteklemek için, istemci yazılımlarının belirteçleri doğrudan Azure AD 'den istemesi gerekir, bu sayede proxy altyapısının trafiği ele geçirebilir. Tarayıcı tabanlı Microsoft 365 uygulamalar, modern kimlik doğrulaması kullanan Office istemcileri gibi (OAuth 2,0 gibi), şu anda kiracı kısıtlamalarını desteklemektedir.

4. **Modern kimlik doğrulaması** : bulut Hizmetleri, kiracı kısıtlamalarını kullanmak ve izin verilmeyen tüm kiracılara erişimi engellemek için modern kimlik doğrulaması kullanmalıdır. Microsoft 365 bulut hizmetlerini, varsayılan olarak modern kimlik doğrulama protokollerini kullanacak şekilde yapılandırmanız gerekir. Modern kimlik doğrulaması için Microsoft 365 destek hakkında en son bilgiler için, [Güncelleştirilmiş Office 365 modern kimlik doğrulamasını](https://www.microsoft.com/en-us/microsoft-365/blog/2015/03/23/office-2013-modern-authentication-public-preview-announced/)okuyun.

Aşağıdaki diyagramda, üst düzey trafik akışı gösterilmektedir. Kiracı kısıtlamaları, Microsoft 365 bulut hizmetlerine değil, yalnızca Azure AD trafiği için TLS incelemesi gerektirir. Azure AD kimlik doğrulaması için trafik hacmi, Exchange Online ve SharePoint Online gibi SaaS uygulamalarına Trafik biriminden çok daha düşük olduğundan bu ayrım önemlidir.

![Kiracı kısıtlamaları trafik akışı-diyagram](./media/tenant-restrictions/traffic-flow.png)

## <a name="set-up-tenant-restrictions"></a>Kiracı kısıtlamalarını ayarlama

Kiracı kısıtlamalarını kullanmaya başlamak için iki adım vardır. İlk olarak, istemcilerinizin doğru adreslere bağlanabildiğinizden emin olun. İkincisi, proxy altyapınızı yapılandırın.

### <a name="urls-and-ip-addresses"></a>URL 'Ler ve IP adresleri

Kiracı kısıtlamalarını kullanmak için, istemcilerinizin kimlik doğrulaması için aşağıdaki Azure AD URL 'Lerine bağlanabilmesi gerekir: [login.microsoftonline.com](https://login.microsoftonline.com/), [login.Microsoft.com](https://login.microsoft.com/)ve [login.Windows.net](https://login.windows.net/). Ayrıca, Office 365 ' e erişmek için istemcileriniz Ayrıca [office 365 URL 'lerinde ve IP adresi aralıklarında](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2)tanımlı tam etki alanı adlarına (FQDN), URL 'LERE ve IP adreslerine bağlanabilmelidir. 

### <a name="proxy-configuration-and-requirements"></a>Proxy yapılandırması ve gereksinimleri

Proxy altyapınız aracılığıyla kiracı kısıtlamalarını etkinleştirmek için aşağıdaki yapılandırma gereklidir. Bu kılavuz geneldir, bu nedenle belirli uygulama adımları için proxy satıcınızın belgelerine başvurmalısınız.

#### <a name="prerequisites"></a>Ön koşullar

- Ara sunucu, FQDN/URL 'Leri kullanarak TLS yakasını, HTTP üst bilgisi eklemeyi ve filtre hedeflerini gerçekleştirebilmelidir.

- İstemciler, TLS iletişimleri için proxy tarafından sunulan sertifika zincirine güvenmelidir. Örneğin, bir iç [ortak anahtar altyapısı (PKI)](/windows/desktop/seccertenroll/public-key-infrastructure) sertifikaları kullanılırsa, iç veren kök sertifika yetkilisi sertifikası güvenilir olmalıdır.

- Bu özellik Microsoft 365 aboneliklerine dahildir, ancak diğer SaaS uygulamalarına erişimi denetlemek için kiracı kısıtlamalarını kullanmak istiyorsanız, Azure AD Premium 1 lisans gerekir.

#### <a name="configuration"></a>Yapılandırma

Login.microsoftonline.com, login.microsoft.com ve login.windows.net için gelen her istek için iki HTTP üst bilgisi ekleyin: *Restrict-Access-to-kiracılar* ve *Restrict-Access-Context* .

> [!NOTE]
> SSL yakalanması ve üst bilgi ekleme 'yi yapılandırırken trafiğin https://device.login.microsoftonline.com dışlandığından emin olun. Bu URL, cihaz kimlik doğrulaması için kullanılır ve TLS kesme işlemini gerçekleştirerek, cihaz kaydı ve cihaz tabanlı koşullu erişim ile ilgili sorunlara neden olabilecek Istemci sertifikası kimlik doğrulaması ile karışabilir.



Üst bilgiler aşağıdaki öğeleri içermelidir:

- *Kısıtla-erişim-kiracılar* için, \<permitted tenant list\> kullanıcıların erişmesine izin vermek istediğiniz kiracılar için virgülle ayrılmış bir liste olan değerini kullanın. Kiracıya kayıtlı tüm etki alanı, bu listedeki kiracıyı ve Dizin KIMLIĞININ kendisini belirlemek için kullanılabilir. Bir kiracıyı açıklamanın üç yolu hakkında bir örnek için contoso, Fabrikam ve Microsoft 'a izin veren ad/değer çifti şöyle görünür: `Restrict-Access-To-Tenants: contoso.com,fabrikam.onmicrosoft.com,72f988bf-86f1-41af-91ab-2d7cd011db47`

- *Restrict-Access-Context* için, kiracı kısıtlamalarını hangi kiracının ayarlamasında olduğunu bildirmek üzere tek BIR dizin kimliği değeri kullanın. Örneğin, contoso 'yu kiracı kısıtlamaları ilkesini belirten kiracı olarak bildirmek için ad/değer çifti şöyle görünür: `Restrict-Access-Context: 456ff232-35l2-5h23-b3b3-3236w0826f3d` .  Bu nokta içinde kendi dizin KIMLIĞINIZI kullanmanız **gerekir** .

> [!TIP]
> Dizin KIMLIĞINIZI [Azure Active Directory portalında](https://aad.portal.azure.com/)bulabilirsiniz. Yönetici olarak oturum açın, **Azure Active Directory** ' yi seçin ve ardından **Özellikler** ' i seçin. 
>
> Bir dizin KIMLIĞI veya etki alanı adının aynı kiracıya başvurmasını doğrulamak için, bu URL 'nin yerine bu KIMLIĞI veya etki alanını kullanın <tenant> : `https://login.microsoftonline.com/<tenant>/v2.0/.well-known/openid-configuration` .  Etki alanı ve KIMLIĞI aynı olan sonuçlar aynıysa, aynı kiracıya başvururlar. 

Kullanıcıların, kendi HTTP üst bilgisini onaylanmamış kiracılar ile eklemesini engellemek için, gelen istekte zaten varsa, proxy 'nin *Restrict-to-to-to----kiracılar* üst bilgisini değiştirmesini gerekir.

İstemciler, login.microsoftonline.com, login.microsoft.com ve login.windows.net için tüm istekler için proxy 'yi kullanmaya zorlanmalıdır. Örneğin, istemcileri ara sunucuyu kullanmak üzere yönlendirmek için PAC dosyaları kullanılıyorsa, son kullanıcılar PAC dosyalarını düzenleyemez veya devre dışı bırakamamalıdır.

> [!NOTE]
> Ara sunucu yapılandırmanızda *. login.microsoftonline.com altına alt etki alanları eklemeyin. Bunun yapılması, device.login.microsoftonline.com içerir ve cihaz kaydı ve cihaz tabanlı koşullu erişim senaryolarında kullanılan Istemci sertifikası kimlik doğrulamasını kesintiye uğratabilecek. Ara sunucuyu TLS kesme ve İnceleme ve üst bilgi ekleme ile device.login.microsoftonline.com dışında bırakacak şekilde yapılandırın.

## <a name="the-user-experience"></a>Kullanıcı deneyimi

Bu bölümde hem son kullanıcılar hem de Yöneticiler için deneyim açıklanmaktadır.

### <a name="end-user-experience"></a>Son kullanıcı deneyimi

Örnek Kullanıcı contoso ağı üzerinde bulunur, ancak Outlook Online gibi paylaşılan SaaS uygulamasının fabrikam örneğine erişmeye çalışıyor. Fabrikam, contoso örneği için izin verilmeyen bir kiracı ise, Kullanıcı, BT departmanınız tarafından onaylanmamış bir kuruluşa ait olan bir kaynağa erişmeye çalıştığınız bir erişim reddi iletisi görür.

### <a name="admin-experience"></a>Yönetici deneyimi

Şirket proxy altyapısında kiracı kısıtlamaları yapılandırması yapıldığında, Yöneticiler Azure portal doğrudan kiracı kısıtlamaları raporlarına erişebilir. Raporları görüntülemek için:

1. [Azure Active Directory portalında](https://aad.portal.azure.com/)oturum açın. **Azure Active Directory Yönetim Merkezi** panosu görüntülenir.

2. Sol bölmede **Azure Active Directory** ’yi seçin. Azure Active Directory genel bakış sayfası görüntülenir.

3. Genel Bakış sayfasında, **kiracı kısıtlamaları** ' nı seçin.

Kısıtlanmış erişim bağlamı kiracısı olarak belirtilen kiracının Yöneticisi, kullanılan kimlik ve hedef dizin KIMLIĞI de dahil olmak üzere kiracı kısıtlama ilkesi nedeniyle engellenen oturum açma işlemlerini görmek için bu raporu kullanabilir. Kısıtlamanın kiracı ayarı, oturum açma işlemi için Kullanıcı kiracısı veya kaynak kiracısı ise, oturum açma işlemleri dahil edilir.

> [!NOTE]
> Bu rapor, kısıtlı erişim bağlamı kiracısı dışında bir kiracıda olan bir Kullanıcı oturum açtığında, hedef dizin KIMLIĞI gibi sınırlı bilgiler içerebilir. Bu durumda, ad ve Kullanıcı asıl adı gibi kullanıcı tarafından tanımlanabilir bilgiler, diğer kiracılardaki ("") Kullanıcı verilerini korumak için maskelenir 00000000-0000-0000-0000-00000000@domain.com 

Azure portal diğer raporlar gibi, raporunuzun kapsamını belirtmek için filtreler kullanabilirsiniz. Belirli bir zaman aralığı, Kullanıcı, uygulama, istemci veya durum üzerinde filtre uygulayabilirsiniz. **Sütunlar** düğmesini seçerseniz, verileri aşağıdaki alanların herhangi bir birleşimiyle görüntülemeyi seçebilirsiniz:

- **Kullanıcı**
- **Uygulama**
- **Durum**
- **Date**
- **Tarih (UTC)** (UTC Eşgüdümlü Evrensel Saat)
- **MFA auth yöntemi** (çok faktörlü kimlik doğrulama yöntemi)
- **MFA auth ayrıntısı** (çok faktörlü kimlik doğrulama ayrıntısı)
- **MFA sonucu**
- **IP adresi**
- **İstemcilerinin**
- **Kullanıcı adı**
- **Konum**
- **Hedef kiracı KIMLIĞI**

## <a name="microsoft-365-support"></a>Microsoft 365 desteği

Microsoft 365 uygulamaların kiracı kısıtlamalarını tam olarak desteklemesi için iki ölçütü karşılaması gerekir:

1. Kullanılan istemci modern kimlik doğrulamasını destekler.
2. Modern kimlik doğrulaması, bulut hizmeti için varsayılan kimlik doğrulama protokolü olarak etkinleştirilir.

Office istemcilerinin Şu anda modern kimlik doğrulamasını desteklediği en son bilgiler için bkz. [güncelleştirilmiş office 365 modern kimlik doğrulaması](https://www.microsoft.com/en-us/microsoft-365/blog/2015/03/23/office-2013-modern-authentication-public-preview-announced/) . Bu sayfada Ayrıca belirli Exchange Online ve Skype Kurumsal Çevrimiçi kiracılarında modern kimlik doğrulamayı etkinleştirmeye yönelik yönergelerin bağlantıları yer alır. SharePoint Online, varsayılan olarak modern kimlik doğrulamaya zaten izin tanıyor.

Microsoft 365 tarayıcı tabanlı uygulamalar (Office portalı, Yammer, SharePoint siteleri, Web 'de Outlook ve daha fazlası) Şu anda kiracı kısıtlamalarını desteklemektedir. Kalın istemciler (Outlook, Skype Kurumsal, Word, Excel, PowerPoint ve daha fazlası) yalnızca modern kimlik doğrulaması kullanırken kiracı kısıtlamalarını zorlayabilir.  

Modern kimlik doğrulamasını destekleyen Outlook ve Skype Kurumsal istemcileri, modern kimlik doğrulamasının etkinleştirilmediği, kiracı kısıtlamalarını etkin bir şekilde atlayarak eski protokollerde hala eski protokolleri kullanabiliyor olabilir. Kiracı kısıtlamaları, kimlik doğrulaması sırasında login.microsoftonline.com, login.microsoft.com veya login.windows.net ile iletişim kurduklarında eski protokolleri kullanan uygulamaları engelleyebilirler.

Müşteriler Windows üzerinde Outlook için, son kullanıcıların profillerine onaylanmamış posta hesapları eklemesini engelleyen kısıtlamalar uygulamayı seçebilirler. Örneğin, [varsayılan olmayan Exchange hesaplarını eklemeyi engelle](https://gpsearch.azurewebsites.net/default.aspx?ref=1) Grup İlkesi ayarını inceleyin.

## <a name="testing"></a>Test Etme

Kiracı kısıtlamalarını tüm kuruluşunuz için uygulamadan önce denemek istiyorsanız, iki seçeneğiniz vardır: Fiddler gibi bir araç kullanan ana bilgisayar tabanlı bir yaklaşım veya bir ara sunucu ayarları sunumu.

### <a name="fiddler-for-a-host-based-approach"></a>Ana bilgisayar tabanlı bir yaklaşım için Fiddler

Fiddler, http üst bilgilerini ekleme de dahil olmak üzere HTTP/HTTPS trafiğini yakalamak ve değiştirmek için kullanılabilen, ücretsiz bir Web hata ayıklama proxy 'si. Fiddler 'i kiracı kısıtlamalarını test etmek üzere yapılandırmak için aşağıdaki adımları uygulayın:

1. [Fiddler 'ı indirip yükleyin](https://www.telerik.com/fiddler).

2. Fiddler 'i, [Fiddler 'in yardım belgeleri](https://docs.telerik.com/fiddler/Configure-Fiddler/Tasks/DecryptHTTPS)başına https trafiğinin şifresini çözmek için yapılandırın.

3. Fiddler 'i, özel kurallar kullanarak *Restrict-to-kiracılar* ve *Restrict-Access-Context* üstbilgilerini ekleyecek şekilde yapılandırın:

   1. Fiddler Web hata ayıklayıcı aracında, **kurallar** menüsünü seçin ve **kuralları Özelleştir...** seçeneğini belirleyin. CustomRules dosyasını açmak için.

   2. İşlevin başına aşağıdaki satırları ekleyin `OnBeforeRequest` . \<tenant domain\>Kiracınızla birlikte kaydedilmiş bir etki alanıyla değiştirin (örneğin, `contoso.onmicrosoft.com` ). \<directory ID\>Kiracınızın Azure AD GUID tanımlayıcısı ile değiştirin.

      ```JScript.NET
      if (
          oSession.HostnameIs("login.microsoftonline.com") ||
          oSession.HostnameIs("login.microsoft.com") ||
          oSession.HostnameIs("login.windows.net")
      )
      {
          oSession.oRequest["Restrict-Access-To-Tenants"] = "<tenant domain>";
          oSession.oRequest["Restrict-Access-Context"] = "<directory ID>";
      }
      ```

      Birden çok kiracıya izin vermeniz gerekiyorsa, kiracı adlarını ayırmak için virgül kullanın. Örneğin:

      `oSession.oRequest["Restrict-Access-To-Tenants"] = "contoso.onmicrosoft.com,fabrikam.onmicrosoft.com";`

4. CustomRules dosyasını kaydedin ve kapatın.

Fiddler 'ı yapılandırdıktan sonra, **Dosya** menüsüne gidip **trafiği yakala** ' yı seçerek trafiği yakalayabilirsiniz.

### <a name="staged-rollout-of-proxy-settings"></a>Proxy ayarlarının aşamalı dağıtımı

Ara sunucu altyapınızın özelliklerine bağlı olarak, ayarların dağıtımını kullanıcılarınıza sunma olanağı sağlayabilirsiniz. Aşağıda, dikkat edilmesi gereken birkaç üst düzey seçenek verilmiştir:

1. Test kullanıcılarını test proxy altyapısına işaret etmek için PAC dosyalarını kullanın, normal kullanıcılar üretim proxy altyapısını kullanmaya devam eder.
2. Bazı proxy sunucular, grupları kullanarak farklı konfigürasyonları destekleyebilir.

Belirli Ayrıntılar için proxy sunucusu belgelerinize bakın.

## <a name="next-steps"></a>Sonraki adımlar

- [Güncelleştirilmiş Office 365 modern kimlik doğrulaması](https://www.microsoft.com/en-us/microsoft-365/blog/2015/03/23/office-2013-modern-authentication-public-preview-announced/) hakkında bilgi edinin
- [Office 365 URL 'lerini ve IP adresi aralıklarını](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2) gözden geçirin
