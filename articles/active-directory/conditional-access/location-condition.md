---
title: Konum koşulları Azure Active Directory koşullu erişim nedir? | Microsoft Docs
description: Konum koşulu, bir kullanıcının ağ konumuna dayalı bulut uygulamalarınıza erişimi denetlemek için kullanmayı öğrenin.
services: active-directory
keywords: uygulamalara koşullu erişim, Azure AD ile koşullu erişim, şirket kaynaklarına güvenli erişim, koşullu erişim ilkeleri
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.subservice: conditional-access
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/01/2019
ms.author: markvi
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: e405b592e75ca8b9fd811c7f891baafa19e528da
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2019
ms.locfileid: "57241196"
---
# <a name="what-is-the-location-condition-in-azure-active-directory-conditional-access"></a>Konum koşulu Azure Active Directory koşullu erişim nedir? 

İle [Azure Active Directory (Azure AD) koşullu erişim](../active-directory-conditional-access-azure-portal.md), nasıl yetkili kullanıcılar denetleyebilir, bulut uygulamalarınızı erişebilirsiniz. Koşullu erişim ilkesinin konum koşulu kullanıcılarınızın ağ konumlarına erişim denetimleri ayarlarını bağlamasına olanak tanır.

Bu makalede konum koşulu yapılandırmak gereken bilgileri sağlar. 

## <a name="locations"></a>Konumlar

Azure AD çoklu oturum açma cihazları ve uygulamaları etkinleştirir ve hizmetlere her yerden genel internet'te. Konum koşulu ile bir kullanıcının ağ konumuna dayalı bulut uygulamalarınıza erişimi denetleyebilirsiniz. Konum koşulu için yaygın kullanım örnekleri şunlardır:

- Şirket ağı devre dışı olduklarında hizmet erişen kullanıcılar için çok faktörlü kimlik doğrulaması gerektiren.

- Bir hizmetin belirli ülke veya bölgelerden erişen kullanıcılar için erişimi engelliyor.

Güvenilen IP'ler bir ağ konumu ya da temsil ettiğini belirtilen konum veya çok faktörlü kimlik doğrulaması için bir konum bir etikettir.


## <a name="named-locations"></a>Adlandırılan yerler 

Adlandırılmış konumlar ile mantıksal gruplandırmaları olan IP adres aralıkları, ülke ve bölge oluşturabilirsiniz. 

Adlandırılmış konumlarınıza erişebileceğiniz **Yönet** koşullu erişim bölümü.

![Konumlar](./media/location-condition/02.png)

 


Adlandırılmış bir konuma aşağıdaki bileşenlere sahiptir:

![Konumlar](./media/location-condition/42.png)

- **Ad** -adlandırılmış bir konuma görünen adı.

- **IP aralıklarını** -bir veya daha fazla IPv4 adres aralıklarını CIDR biçiminde. Bir IPv6 adres aralığı belirtilmesi desteklenmiyor.

- **Güvenilen konum olarak işaretle** -güvenilen bir konum belirtmek adlandırılmış bir konum için ayarlayabileceğiniz bir bayrak. Genellikle, güvenilen konumları BT departmanınız tarafından denetlenen ağ alanlardır. Koşullu erişim yanı sıra güvenilen adlandırılmış konumlar ayrıca Azure kimlik koruması ve Azure AD güvenlik raporları tarafından azaltmak için kullanılan [hatalı pozitif sonuçları](../reports-monitoring/concept-risk-events.md#impossible-travel-to-atypical-locations-1).

- **Ülkeler/bölgeler** -bu seçenek, bir veya daha fazla ülke veya bölge adlandırılmış bir konuma tanımlamak için seçmenize olanak sağlar. 

- **Bilinmeyen alanları dahil et** -bazı IP adreslerini belirli bir ülkeye eşlenmedi. Bu seçenek, bu IP adresleri adlandırılmış bir konumda dahil edilip edilmeyeceğini seçmenize olanak tanır. Belirtilen konum kullanarak ilke bilinmeyen konumlara uygulanmasını gerektiren bu ayarı kullanın.

Adlandırılmış konumlar yapılandırabileceğiniz sayısı, Azure AD'de ilgili nesne boyutu tarafından sınırlanır. Aşağıdakilerden birini yapılandırabilirsiniz:

- Bir adlı 1200 IP aralıklarına sahip konum.

- Her birine atanan bir IP aralığı 90 adlı konumlarıyla en fazla.




## <a name="trusted-ips"></a>Güvenilen IP'ler

Ayrıca, kuruluşunuzun yerel intranet temsil eden IP adresi aralıklarını yapılandırabilirsiniz [multi-Factor authentication hizmeti ayarlarını](https://account.activedirectory.windowsazure.com/usermanagement/mfasettings.aspx). Bu özellik, 50 adede kadar IP adresi aralıklarını yapılandırmanızı sağlar. IP adresi aralıklarını CIDR biçimindedir. Daha fazla bilgi için [güvenilen IP'ler](../authentication/howto-mfa-mfasettings.md#trusted-ips).  

Yapılandırılan güvendiyseniz, bunlar olarak görünmesini **MFA güvenilen IP'ler** konum koşulu için konumları listesinde.   

### <a name="skipping-multi-factor-authentication"></a>Çok faktörlü kimlik doğrulaması atlanıyor

Çok faktörlü kimlik doğrulaması Hizmeti Ayarları sayfasında, Kurumsal intranet kullanıcıları seçerek belirleyebilirsiniz **Atla intranetimde bulunan Federasyon kullanıcılardan gelen istekleri için multi-Factor authentication**. Bu ayar gösteren şirket içi AD FS tarafından verilen, talep ağ verilecek güvenilen ve kurumsal ağ üzerinde olarak kullanıcıyı tanımlamak için kullanılır. Daha fazla bilgi için [koşullu erişim kullanarak güvenilen IP'ler özelliği etkinleştirmek](../authentication/howto-mfa-mfasettings.md#enable-the-trusted-ips-feature-by-using-conditional-access).

Bu seçenek denetledikten sonra adlandırılmış konumu dahil olmak üzere **MFA güvenilen IP'ler** tüm ilkeler için bu seçili uygulanır.

Oturum süreleri uzun ömürlü, mobil ve Masaüstü uygulamalar için koşullu erişim düzenli aralıklarla yeniden değerlendirilir. Varsayılan bir kez bir saattir. İç şirket ağına talep yalnızca ilk kimlik doğrulaması yapıldığı sırada verilen, güvenilen IP aralıkları listesini Azure AD'ye sahip olmayabilir. Bu durumda, kullanıcı hala şirket ağında olup olmadığını belirlemek daha zordur:

1. Kullanıcının IP adresi güvenilen IP aralıkları birinde olup olmadığını denetleyin.

2. İlk üç sekizlisinin aynı kullanıcının IP adresi, IP adresinin ilk kimlik doğrulamasının ilk 3 sekizlik tabanda eşleşip eşleşmediğini kontrol edin. Bu ne zaman olduğundan IP adresinin ilk kimlik doğrulaması ile karşılaştırılır iç kurumsal ağa talep ilk verildiği ve kullanıcı konumu doğrulandı.

Her iki adım başarısız olursa, bir kullanıcı artık bir güvenilen IP olarak değerlendirilir.



## <a name="location-condition-configuration"></a>Konum koşulu yapılandırma

Konum koşulu yapılandırdığınızda ayırt etmek için seçeneğiniz vardır:

- Herhangi bir konum 
- Tüm güvenilen konumlar
- Seçili konumlar

![Konumlar](./media/location-condition/01.png)

### <a name="any-location"></a>Herhangi bir konum

Varsayılan olarak, seçerek **herhangi bir konuma** herhangi bir Internet adresi yani tüm IP adresleri için uygulanacak bir ilke neden olur. Bu ayar adlandırılmış konumu olarak yapılandırdığınız IP adreslerini sınırlı değildir. Seçtiğinizde, **herhangi bir konuma**, belirli konumlara bir ilkeden yine de hariç tutabilirsiniz. Örneğin, şirket ağı dışında tüm konumlara kapsamını ayarlamak için güvenilen konumları hariç tüm konumlara bir ilke uygulayabilirsiniz.

### <a name="all-trusted-locations"></a>Tüm güvenilen konumlar

Bu seçenek için geçerlidir:

- Güvenilen konum olarak işaretlenmiş tüm konumlar
- **MFA güvenilen IP'ler** (yapılandırılmışsa)


### <a name="selected-locations"></a>Seçili konumlar

Bu seçenek ile bir veya daha fazla adlandırılmış konumlar seçebilirsiniz. Bu ayar, uygulamak ile bir ilke için bir kullanıcı seçili konumlardan birini bağlanması gerekir. Tıkladığınızda **seçin** adlandırılmış ağlar listesi gösteren adlandırılmış ağ seçim denetim açar. Ağ konumu işaretli listede de gösterilir olarak güvenilir. Belirtilen konum adlı **MFA güvenilen IP'ler** çok faktörlü kimlik doğrulaması hizmeti ayar sayfasında yapılandırılabilir IP ayarları eklemek için kullanılır.

## <a name="what-you-should-know"></a>Bilmeniz gerekenler

### <a name="when-is-a-location-evaluated"></a>Ne zaman bir konum olarak kabul edilir?

Koşullu erişim ilkeleri değerlendirilir olduğunda: 

- Bir kullanıcı ilk kez bir web uygulaması, mobil veya masaüstü uygulamasına oturum açtığında. 

- Modern kimlik doğrulaması kullanan bir mobil veya masaüstü uygulaması, yeni bir erişim belirteci almak için bir yenileme belirteci kullanır. Varsayılan olarak bu kez bir saattir. 

Mobil için başka bir deyişle ve Masaüstü uygulamaları, modern kimlik doğrulaması kullanan bir saat içinde ağ konumunu değiştirme konumunda bir değişiklik algılanır. Modern kimlik doğrulaması kullanmayan mobil ve Masaüstü uygulamalar için her bir belirteç isteğinde ilke uygulanır. İstek sıklığını uygulama göre farklılık gösterebilir. Benzer şekilde, web uygulamaları için ilke ilk oturum açma işleminde uygulanır ve konumundaki web uygulaması oturumunun ömrü boyunca geçerlidir. Uygulamalar arasında oturum süreleri farklılıkları nedeniyle, ilke değerlendirmesi arasındaki süreyi de değişir. Uygulamanın yeni bir oturum açma belirteci, istekleri her zaman ilke uygulanır.


Varsayılan olarak, Azure AD üzerinden saatlik olarak bir belirteç verir. Bir saat içinde Kurumsal ağa taşıdıktan sonra modern kimlik doğrulaması kullanan uygulamalar için ilke uygulanmaz.


### <a name="user-ip-address"></a>Kullanıcının IP adresi

İlkesi değerlendirmesi içine kullanılan IP adresini, kullanıcının genel IP adresidir. İçin özel bir ağdaki cihazları, bu kullanıcının cihazına intranet üzerindeki istemci IP'si değil, bu ağın genel internet'e bağlanmak için kullandığı adrestir. Cihazınız yalnızca bir IPv6 adresi varsa, konum koşulu yapılandırılması desteklenmiyor.

### <a name="bulk-uploading-and-downloading-of-named-locations"></a>Toplu karşıya yükleme ve adlandırılmış konumları indirme

Oluşturma veya güncelleştirme adlandırılmış konumlar, toplu güncelleştirmeler için yükleme veya kullanabilirsiniz IP aralıklarını içeren bir CSV dosyalarını indirme. Karşıya yükleme IP aralıkları listesinde bu dosya ile değiştirir. Dosyanın her satırı CIDR biçiminde bir IP adresi aralığı içerir. 


### <a name="cloud-proxies-and-vpns"></a>Bulut Ara sunucuları ve VPN 

Bulutta barındırılan proxy ya da VPN çözümü kullandığınızda, bir ilke değerlendirmesi proxy IP adresi olduğu IP adresini Azure AD kullanır. Bir IP adresi faking bir yöntem sunmak şekilde güvenilir bir kaynaktan geldiği doğrulama olduğundan kullanıcının genel IP adresini içeren X-Forwarded-For (XFF) üst bilgisi kullanılmaz. 

Bir yerde, bir etki alanına katılmış cihaz kullanılabilir zorunlu tutmak için kullanılan bir ilke veya içeriden bir bulut proxy olduğunda talebi AD fs'den.



### <a name="api-support-and-powershell"></a>API desteği ve PowerShell 

API ve PowerShell henüz desteklenmiyor veya koşullu erişim ilkeleri adlandırılmış konumlar için.





## <a name="next-steps"></a>Sonraki adımlar

- Koşullu erişim ilkesi yapılandırmak için bkz. nasıl bilmek istiyorsanız [Azure Active Directory koşullu erişimiyle belirli uygulamalar için MFA gerektiren](app-based-mfa.md).

- Ortamınızda koşullu erişim ilkelerini yapılandırmaya hazırsanız bkz. [Azure Active Directory'de koşullu erişim için en iyi yöntemler](best-practices.md). 
