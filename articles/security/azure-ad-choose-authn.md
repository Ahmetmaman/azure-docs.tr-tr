---
title: Azure AD karma kimlik çözümünüz için doğru kimlik doğrulama yöntemini seçin | Microsoft Docs
description: Bu kılavuz, şirket, CIO, CISOs, baş kimlik mimarları, Kurumsal mimarlar ve güvenlik yardımcı olur ve orta ve büyük kuruluşlar, Azure AD karma kimlik çözümü için bir kimlik doğrulama yöntemi seçme sorumlu BT karar alma mekanizmaları.
services: active-directory
keywords: ''
author: martincoetzer
ms.author: martincoetzer
ms.date: 04/12/2018
ms.topic: article
ms.service: active-directory
ms.workload: identity
ms.openlocfilehash: a05874e28c08087b6f82c3aa5a02e83d2629ffe5
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/23/2019
ms.locfileid: "56728191"
---
# <a name="choose-the-right-authentication-method-for-your-azure-active-directory-hybrid-identity-solution"></a>Azure Active Directory karma kimlik çözümünüz için doğru kimlik doğrulama yöntemini seçin 

Bu makalede, kuruluşların tam bir Azure Active Directory (Azure AD) karma kimlik çözümü makaleleri bir dizi başlar. Bu çözüm olarak ana hatları [karma kimlik dijital dönüşüm çerçevesi](https://aka.ms/aadframework). İş sonuçlarını kapsar ve hedefleri kuruluşların güçlü ve Güvenli Karma kimlik çözümü uygulamak için üzerinde odaklanabilirsiniz. 

Framework'ün ilk iş sonucu harfe dönüştüren kullanıcıların bulut uygulamalarına erişirken kimlik doğrulama işlemi güvenli hale getirmek kuruluşların gereksinimleri uğradı. İlk iş kimlik doğrulaması güvenli iş sonucu olarak, şirket içi kullanıcı adları ve parolalar'ı kullanarak bulut uygulamalarında oturum açmak için kullanıcıların yeteneğini hedeftir. Bu oturum açma işlemi ve kullanıcıların kimliklerini her şey bulutta mümkün kılar.

Doğru kimlik doğrulama yöntemi seçme uygulamalarını buluta taşımak isteyen kuruluşlar için ilk konusudur. Bu karar, aşağıdaki nedenlerden dolayı hafife almayız:

1. Buluta taşımak isteyen bir kuruluş için ilk kararıdır. 

2. Kimlik doğrulama yöntemi, bir kuruluşun bulunma bulutta kritik bir bileşenidir. Bu, tüm bulut veri ve kaynaklara erişimi denetler.

3. Bu tüm diğer gelişmiş güvenlik ve kullanıcı deneyimi özellikleri Azure AD'de altyapıdır.

4. Kimlik doğrulama yöntemini uygulandıktan sonra değiştirmek zordur.

Yeni Denetim düzlemi BT güvenlik kimliğidir. Bu nedenle yeni bulut dünya çapında bir kuruluşun erişim guard kimlik doğrulaması kullanılır. Kuruluşların, kendi güvenlik güçlendirir ve bulut uygulamalarını saldırganların güvenli tutar kimlik denetim düzlemi gerekir.

### <a name="out-of-scope"></a>Kapsam dışına
Mevcut bir şirket içi dizin ayak izine sahip olmayan kuruluşlarda, bu makalenin odak değildir. Genellikle, bu işletmelerin karma kimlik çözümü gerektirmez, yalnızca bulutta kimlikleri oluşturun. Yalnızca bulutta yer alan kimlikleri yalnızca bulutta mevcut ve karşılık gelen şirket içi kimlikleri ile ilişkili değildir.

## <a name="authentication-methods"></a>Kimlik doğrulama yöntemleri
Azure AD karma kimlik çözümü, yeni bir denetim düzlemi olduğunda, kimlik doğrulaması bulut erişimi altyapıdır. Doğru kimlik doğrulama yöntemi seçme içinde bir Azure AD karma kimlik çözümünü ayarlama önemli bir ilk karar değil. Kullanıcıların bulutta sağlayan Azure AD Connect kullanarak yapılandırılan kimlik doğrulama yöntemini uygulayın.

Bir kimlik doğrulama yöntemi seçmek için zaman, mevcut altyapı, karmaşıklığı ve maliyeti, seçtiğiniz uygulama göz önünde bulundurmanız gerekir. Bu faktörlerin her kuruluş için farklıdır ve zaman içinde değişebilir. 

>[!VIDEO https://www.youtube.com/embed/YtW2cmVqSEw]

Azure AD karma kimlik çözümleri için aşağıdaki kimlik doğrulama yöntemlerini destekler.

### <a name="cloud-authentication"></a>Bulut kimlik doğrulaması
Bu kimlik doğrulama yöntemi seçtiğinizde Azure AD kullanıcılarınızın oturum açma işlemi işler. Sorunsuz çoklu oturum açma (SSO) birlikte kullanıcılar, kimlik bilgilerini girmek zorunda kalmadan bulut uygulamalarında oturum açabilir. Bulut kimlik doğrulaması ile iki seçenekler arasından seçim yapabilirsiniz: 

**Azure AD parola karması eşitleme**. Şirket içi dizin nesnelerini Azure ad kimlik doğrulamasını etkinleştirmek için en basit yolu. Kullanıcılar, aynı kullanıcı adı ve parola şirket içinde kullandıkları kullanabileceğiniz ek altyapı dağıtmak zorunda kalmadan. Seçtiğiniz ne olursa olsun hangi kimlik doğrulama yöntemi için parola karması eşitleme gibi kimlik koruması, Azure AD premium özelliklerinden bazıları gerektirir.

> [!NOTE] 
> Hiçbir zaman parolaları düz metin olarak depolanan veya Azure AD'de bir ters çevrilebilir algoritması ile şifrelenmiş. Parola Karması eşitleme gerçek işlemi hakkında daha fazla bilgi için bkz. [Azure AD Connect eşitlemesi ile parola karması eşitlemeyi uygulama](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-password-hash-synchronization). 

**Azure AD geçişli kimlik doğrulaması**. Azure AD kimlik doğrulama hizmetleri için bir basit parola doğrulaması bir veya daha fazla şirket içi sunucular üzerinde çalışan bir yazılım aracı kullanarak sağlar. Sunucuları doğrudan şirket içi Active directory'nizle parola doğrulama bulutta gerçekleşmez sağlar, kullanıcıların doğrulayın. 

Şirket içi kullanıcı hemen uygulamak için bir güvenlik gereksinimidir şirketlerle durumları, parola ilkeleri, hesap ve oturum açma saatleri bu kimlik doğrulama yöntemini kullanabilirsiniz. Gerçek doğrudan kimlik doğrulama işlemi hakkında daha fazla bilgi için bkz. [kullanıcı oturum açma ile Azure AD geçişli kimlik doğrulaması](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-pta).

### <a name="federated-authentication"></a>Federe kimlik doğrulaması
Bu kimlik doğrulama yöntemini seçin, uygulamalı Azure AD kimlik doğrulama işlemi, şirket içi Active Directory Federasyon Hizmetleri (AD FS) gibi bir güvenilen kimlik doğrulama sistemi için kullanıcının parolasını doğrulamak için kapalı.

Kimlik doğrulama sistemi ek Gelişmiş kimlik doğrulama gereksinimleri sağlayabilir. Akıllı kart tabanlı kimlik doğrulaması veya üçüncü taraf çok faktörlü kimlik doğrulaması verilebilir. Daha fazla bilgi için [Active Directory Federasyon Hizmetleri dağıtma](https://docs.microsoft.com/windows-server/identity/ad-fs/deployment/windows-server-2012-r2-ad-fs-deployment-guide).

Aşağıdaki bölümde karar ağacı kullanarak hangi kimlik doğrulama yönteminin size uygun olduğuna karar vermenize yardımcı olur. Bulutta veya şirket dışı kimlik doğrulaması, Azure AD karma kimlik çözümü için dağıtılıp dağıtılmayacağını belirlemenize yardımcı olur.

## <a name="decision-tree"></a>Karar ağacı

![Azure AD kimlik doğrulaması karar ağacı](media/azure-ad/azure-ad-authn-image1.png)

Karar sorular hakkında ayrıntılar:

1. Azure AD oturum açma kullanıcıların parolaları doğrulamak için şirket içi bileşene bağlı olmadan başa çıkabilir.
2. Azure AD, AD FS Microsoft'un gibi bir güvenilen kimlik doğrulama sağlayıcısı için kullanıcı oturum açma devre dışı dağıtabilir.
3. Hesabın süresi gibi kullanıcı düzeyinde Active Directory güvenlik ilkeleri uygulamanız gerekiyorsa, hesabı devre dışı parolasının süresi doldu, hesap kilitlendi ve her bir kullanıcı oturum açma saatleri oturum açma, Azure AD, bazı şirket içi bileşenlerin gerektirir.
4. Yerel olarak Azure AD tarafından desteklenen oturum açma özellikleri:
   * Akıllı kartlar veya sertifikaları kullanarak oturum açın.
   * Şirket içi MFA sunucusu kullanarak oturum açın.
   * 3 taraf kimlik doğrulama çözümü kullanarak oturum açın.
   * Çok siteli şirket içi kimlik doğrulama çözümüdür.
5. Azure AD kimlik koruması, hangi oturum açma yöntemi, sağlamak için seçtiğiniz bakılmaksızın parola karması eşitlemesi gerektirir *kimlik bilgileri sızdırılan kullanıcılar* rapor. Kuruluşlar, birincil, oturum açma yöntemleri başarısız olursa ve hata olayından önce yapılandırılan parola karma eşitlemesi yük devretme gerçekleştirebilirsiniz.

>[!NOTE]
> Azure AD kimlik koruması gerektiren [Azure AD Premium P2](https://azure.microsoft.com/pricing/details/active-directory/) lisansları.

## <a name="detailed-considerations"></a>Ayrıntılı değerlendirmeler

### <a name="cloud-authentication-password-hash-synchronization"></a>Bulut kimlik doğrulaması: Parola karması eşitleme

* **Efor**. Parola Karması eşitleme, dağıtım, Bakım ve altyapı ile ilgili en az çaba gerektirir.  Bu düzeyde çaba genellikle yalnızca, kullanıcılar Office 365, SaaS uygulamaları ve diğer Azure AD tabanlı kaynakları oturum açmak için gereken kuruluşlar için de geçerlidir. Etkinleştirildiğinde, parola karması eşitleme, Azure AD Connect eşitleme işleminin bir parçası olan ve her iki dakikada bir çalışır.

* **Kullanıcı deneyimi**. Kullanıcıların oturum açma deneyimini sorunsuz çoklu oturum açma Parola Karması eşitleme ile dağıtın. Sorunsuz çoklu oturum açma, kullanıcıların oturum açtığınızda gereksiz istemleri ortadan kaldırır.

* **Gelişmiş senaryolar**. Kuruluşlar için seçerseniz, Azure AD Premium P2 ile Azure AD kimlik koruması raporlarla kimlikleri ınsights'tan kullanmak da mümkündür. Sızan kimlik bilgileri rapor buna bir örnektir. Windows iş için Hello olan diğer seçenek ise [parola karması eşitleme kullandığınızda belirli gereksinimleri](https://docs.microsoft.com/windows/access-protection/hello-for-business/hello-identity-verification). 

    Parola Karması eşitleme ile çok faktörlü kimlik doğrulaması gerektiren kuruluşlar, Azure AD ile çok faktörlü kimlik doğrulaması kullanmanız gerekir. Kuruluşlar, üçüncü taraf veya şirket içinde çok faktörlü kimlik doğrulama yöntemleri kullanamaz.

* **İş sürekliliği**. Parola Karması eşitleme bulut kimlik doğrulamasını kullanarak tüm Microsoft veri merkezleri için ölçeklenebilir bir bulut hizmeti yüksek oranda kullanılabilir. Parola Karması eşitleme uzun süreler ermez emin olmak için ikinci bir Azure AD Connect sunucusu hazırlama modunda bir bekleme yapılandırmasında dağıtın.

* **Dikkat edilecek noktalar**. Şu anda, parola karması eşitleme hemen şirket içi hesap durumları değişiklikleri zorunlu değildir. Bu durumda, bir kullanıcının bulut uygulamaları için Azure AD kullanıcı hesabı durumunu eşitlenene kadar erişimine sahiptir. Kuruluşlar, yöneticilerin şirket içi kullanıcı hesabı durumları güncelleştirmeleri toplu sonra yeni bir eşitleme döngüsü çalıştırarak bu sınırlamanın üstesinden gelmek isteyebilirsiniz. Bir örnek hesaplar devre dışı bırakılmalıdır.

> [!NOTE]
> Parolanın süresi doldu ve hesap kilitli durumları şu anda Azure AD Connect ile Azure AD'ye eşitlenen değildir. 

Başvurmak [parola karması eşitlemeyi uygulama](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-password-hash-synchronization) dağıtım adımları için.

### <a name="cloud-authentication-pass-through-authentication"></a>Bulut kimlik doğrulaması: Doğrudan Kimlik Doğrulama  

* **Efor**. Geçişli kimlik doğrulaması için bir veya daha fazla (öneririz üç) ihtiyacınız var olan sunucularda yüklü basit aracıları. Bu aracılar, şirket içi Active Directory etki alanı içeren şirket içi Hizmetleri erişimi bulunmalıdır AD etki alanı denetleyicileri. Bunlar, İnternet'e giden erişim ve etki alanı denetleyicilerinizin erişimi gerekir. Bu nedenle, bir çevre ağında aracıları dağıtmak için desteklenmiyor. 

    Geçişli kimlik doğrulaması, sınırlandırılmamış bir ağ etki alanı denetleyicilerine erişim gerektirir. Tüm ağ trafiğini şifrelenir ve kimlik doğrulama isteklerini sınırlıdır. Bu işlem hakkında daha fazla bilgi için bkz. [güvenliğe derinlemesine bakış](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-pta-security-deep-dive) geçişli kimlik doğrulaması.

* **Kullanıcı deneyimi**. Kullanıcıların oturum açma deneyimini sorunsuz çoklu oturum açma geçişli kimlik doğrulaması ile dağıtın. Sorunsuz çoklu oturum açma, kullanıcılar oturum açtıktan sonra gereksiz istemleri ortadan kaldırır.

* **Gelişmiş senaryolar**. Geçişli kimlik doğrulaması, oturum açma zaman şirket içi hesap ilkeleri uygular. Örneğin, bir şirket içi kullanıcı hesabının durumu devre dışıysa, kilitli, zaman erişim reddedildi veya [parolasının süresi doldu](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-pta-faq#what-happens-if-my-users-password-has-expired-and-they-try-to-sign-in-by-using-pass-through-authentication) veya kullanıcının ne zaman izin oturum açmak için saatleri dışında kalan. 

    Geçişli kimlik doğrulaması ile çok faktörlü kimlik doğrulaması gerektiren kuruluşlar, Azure multi-Factor Authentication (MFA) kullanmanız gerekir. Kuruluşlar, bir üçüncü taraf veya şirket içinde çok faktörlü kimlik doğrulama yöntemi olarak kullanamazsınız. Gelişmiş Özellikler, geçişli kimlik doğrulaması seçtiğiniz olup olmadığını parola karması eşitleme dağıtıldığını gerektirir. Kimlik koruması, sızan kimlik bilgileri rapor buna bir örnektir.

* **İş sürekliliği**. İki ek doğrudan kimlik doğrulama aracılarının dağıtmanızı öneririz. Azure AD Connect sunucusu ilk aracıda yanı sıra bu ek özellikler şunlardır. Bu ek dağıtım kimlik doğrulama isteklerini yüksek kullanılabilirliğini sağlar. Dağıtılan üç aracınız varsa, bakım için başka bir aracı kapalı olduğunda, bir aracı yine de başarısız olabilir. 

    Parola Karması eşitleme geçişli kimlik doğrulaması ek olarak dağıtmak için başka bir faydası yoktur. Birincil kimlik doğrulama yöntemi artık kullanılabilir olduğunda bir yedek kimlik doğrulama yöntemi olarak görev yapar.

* **Dikkat edilecek noktalar**. Aracıların şirket önemli hata nedeniyle bir kullanıcının kimlik doğrularken, geçişli kimlik doğrulaması için bir yedek kimlik doğrulama yöntemi olarak parola karması eşitleme kullanabilirsiniz. Parola Karması eşitleme için yük devretme otomatik olarak gerçekleşmez ve Azure AD Connect oturum açma yöntemini el ile geçiş yapmak için kullanmanız gerekir. 

    Geçişli kimlik doğrulaması, alternatif kimlik dahil olmak üzere diğer etmenlere desteklemek için bkz: [sık sorulan sorular](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-pta-faq).

Başvurmak [geçişli kimlik doğrulaması uygulama](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-pta) dağıtım adımları için.

### <a name="federated-authentication"></a>Federe kimlik doğrulaması

* **Efor**. Kullanıcıların kimliğini doğrulamak için bir dış güvenilen sistemde bir Federasyon kimlik doğrulama sistemini kullanır. Bazı şirketler, kendi Azure AD karma kimlik çözümü ile var olan bir Federasyon sistem yatırım getirisi yeniden kullanmak istiyorum. Bakım ve Federasyon sisteminin yönetimi, Azure AD denetimi dışında döner. Bu kuruluş için güvenli bir şekilde dağıtılır ve kimlik doğrulama yükü emin olmak için Federasyon sistem kullanarak aittir. 

* **Kullanıcı deneyimi**. Şirket dışı kimlik doğrulaması kullanıcı deneyimini uygulamasının Özellikler, topoloji ve Federasyon grubu yapılandırmasına bağlıdır. Bazı kuruluşlarda uyum ve güvenlik gereksinimlerine uyacak şekilde Federasyon grubu erişimi yapılandırmak için bu esnekliğe ihtiyaç duyarsınız. Örneğin, dahili olarak bağlı kullanıcıları ve cihazları kullanıcılar için kimlik bilgilerini sormadan otomatik olarak oturum yapılandırmak mümkündür. Bu yapılandırma, bunlar zaten cihazlarına oturum açtığınız için çalışır. Bazı gelişmiş güvenlik özellikleri gerekirse, kullanıcıların oturum açma işlemi daha zor hale.

* **Gelişmiş senaryolar**. Müşteriler Azure AD yerel olarak desteklemeyen bir kimlik doğrulaması gerekli olduğunda, bir Federasyon kimlik doğrulaması çözüm genellikle gereklidir. Yardımcı olmak için ayrıntılı bilgi [doğrudan oturum açma seçeneğini](https://blogs.msdn.microsoft.com/samueld/2017/06/13/choosing-the-right-sign-in-option-to-connect-to-azure-ad-office-365/). Ortak aşağıdaki gereksinimleri göz önünde bulundurun:

    * Akıllı kart veya sertifika gerektiren kimlik doğrulaması.
    * Şirket içi MFA sunucularda veya çok faktörlü üçüncü taraf sağlayıcılar.
    * Üçüncü taraf kimlik doğrulama çözümlerini kullanarak kimlik doğrulaması. Bkz: [Azure AD Federasyonu uyumluluk listesi](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-fed-compatibility).
    * Oturum açma gerektiren bir sAMAccountName, örneğin, etki alanı\kullanıcı adı, yerine bir kullanıcı asıl adı (UPN), örneğin, user@domain.com.

* **İş sürekliliği**. Federasyon sistemleri genellikle bir grup olarak bilinen sunucular, yük dengeli bir dizi gerektirir. Bu grupta bir iç ağa ve kimlik doğrulaması istekler için yüksek kullanılabilirlik sağlamak için çevre ağ topolojisi içinde yapılandırılır.

    Birincil kimlik doğrulama yöntemi artık kullanılabilir olduğunda, bir yedek kimlik doğrulama yöntemi olarak şirket dışı kimlik doğrulaması ile birlikte parola karması eşitleme dağıtın. Şirket içi sunucular mevcut değilken bir örnektir. Bazı büyük ölçekli kuruluşların coğrafi-DNS düşük gecikme süreli kimlik doğrulama istekleri için yapılandırılmış birden fazla Internet giriş noktalarını destekleyecek şekilde bir Federasyon çözümü gerektirir.

* **Dikkat edilecek noktalar**. Federasyon sistemleri, genellikle şirket içi altyapı daha ciddi bir yatırım gerekiyordu. Çoğu kuruluş, zaten bir şirket içi Federasyon yatırımınız varsa bu seçeneği belirleyin. Ve tek kimlik sağlayıcısı kullanmak için güçlü bir iş gereksinimi varsa. Federasyon, bulut kimlik doğrulaması çözümleri çalıştırmak ve sorun giderme için daha karmaşık kıyasla ' dir.

Azure AD'de doğrulanmış bir nonroutable etki, kullanıcı kimliği oturum açma uygulamak için ek yapılandırma gerekir. Bu gereksinim kimliği desteği alternatif bir oturum açma bilinir. Bkz: [alternatif oturum açma Kimliğini yapılandırma](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configuring-alternate-login-id) kısıtlamaları ve gereksinimleri. Federasyon ile üçüncü taraf multi-Factor authentication sağlayıcısı kullanmayı seçerseniz, cihazların Azure AD'ye katılmasına izin vermek için WS-Trust sağlayıcının desteklediği emin olun.

Başvurmak [federasyon sunucuları dağıtma](https://docs.microsoft.com/windows-server/identity/ad-fs/deployment/deploying-federation-servers) dağıtım adımları için.

> [!NOTE] 
> Desteklenen topolojiler Azure AD Connect, Azure AD karma kimlik çözümü dağıttığınızda, uygulamalıdır. Yaklaşık desteklenen ve desteklenmeyen yapılandırmalar daha fazla bilgi [Azure AD Connect için topolojiler](https://docs.microsoft.com/azure/active-directory/hybrid/plan-connect-topologies).

## <a name="architecture-diagrams"></a>Mimari diyagramları

Aşağıdaki diyagramlarda, Azure AD karma kimlik çözümü ile kullandığınız her kimlik doğrulama yöntemi için gereken üst düzey mimari bileşenler özetler. Bunlar, çözümleri arasındaki farklılıklar karşılaştırmanıza yardımcı olmak için genel bir bakış sağlar.

* Parola Karması eşitleme çözümünün Basitlik:

    ![Parola Karması eşitleme ile Azure AD karma kimlik](media/azure-ad/azure-ad-authn-image2.png)

* Doğrudan kimlik doğrulama Aracısı gereksinimleri:

    ![Geçişli kimlik doğrulaması ile Azure AD karma kimlik](media/azure-ad/azure-ad-authn-image3.png)

* Federasyon çevre ve kuruluşunuzun iç ağ için gereken bileşenler:

    ![Federe kimlik doğrulaması ile Azure AD karma kimlik](media/azure-ad/azure-ad-authn-image4.png)

## <a name="comparing-methods"></a>Karşılaştırma yöntemleri

|Önemli noktalar|Parola Karması eşitleme + sorunsuz çoklu oturum açma|Geçişli kimlik doğrulaması + sorunsuz çoklu oturum açma|AD FS ile Federasyon|
|:-----|:-----|:-----|:-----|
|Burada kimlik doğrulaması gerçekleşir?|Bulutta|Bulutta güvenli parola doğrulaması exchange şirket içi kimlik doğrulama Aracısı ile sonra|Şirket içi|
|Ne sağlama sistemin dışında şirket içi sunucu gereksinimleri şunlardır: Azure AD Connect?|None|Her ek kimlik doğrulama aracısı için bir sunucu|İki veya daha fazla AD FS sunucuları<br><br>İki veya daha fazla WAP sunucularını çevre/çevre ağındaki|
|Gereksinimlerini şirket içi Internet ve ağ sağlama sistem ötesinde nelerdir?|None|[Giden Internet erişimi](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-pta-quick-start) kimlik doğrulama aracılarının sunuculardan çalıştırma|[Gelen Internet erişimi](https://docs.microsoft.com/windows-server/identity/ad-fs/overview/ad-fs-requirements) WAP sunucularını çevre için<br><br>AD FS sunucuları için çevre WAP sunuculardan gelen ağ erişimini<br><br>Ağ Yükü Dengeleme|
|Bir SSL sertifikası gereksinimi var mı?|Hayır|Hayır|Evet|
|Sistem durumu izleme çözümü var mı?|Gerekli değil|Aracı durumu tarafından sağlanan [Azure Active Directory Yönetim Merkezi](https://docs.microsoft.com/azure/active-directory/hybrid/tshoot-connect-pass-through-authentication)|[Azure AD Connect Health](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-health-adfs)|
|Kullanıcılara çoklu oturum açma bulut kaynaklarına etki alanına katılmış cihazların şirket ağından elde ederim?|Evet ile [sorunsuz çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sso)|Evet ile [sorunsuz çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sso)|Evet|
|Hangi oturum açma türleri desteklenir?|UserPrincipalName + parola<br><br>Kullanarak Windows tümleşik kimlik [sorunsuz çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sso)<br><br>[Alternatif oturum açma kimliği](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-install-custom)|UserPrincipalName + parola<br><br>Kullanarak Windows tümleşik kimlik [sorunsuz çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sso)<br><br>[Alternatif oturum açma kimliği](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-pta-faq)|UserPrincipalName + parola<br><br>sAMAccountName + parola<br><br>Tümleşik Windows Kimlik Doğrulaması<br><br>[Sertifika ve akıllı kart kimlik doğrulaması](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configure-user-certificate-authentication)<br><br>[Alternatif oturum açma kimliği](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configuring-alternate-login-id)|
|Windows Hello için iş desteklenir mi?|[Anahtar güven modeli](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-identity-verification)<br><br>[Intune ile sertifika güven modeli](https://blogs.technet.microsoft.com/microscott/setting-up-windows-hello-for-business-with-intune/)|[Anahtar güven modeli](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-identity-verification)<br><br>[Intune ile sertifika güven modeli](https://blogs.technet.microsoft.com/microscott/setting-up-windows-hello-for-business-with-intune/)<br>*Windows Server 2016 etki alanı işlev düzeyi gerektirir*|[Anahtar güven modeli](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-identity-verification)<br><br>[Sertifika güven modeli](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-key-trust-adfs)|
|Çok faktörlü kimlik doğrulama seçenekleri nelerdir?|[Azure MFA](https://docs.microsoft.com/azure/multi-factor-authentication/)<br><br>[Özel denetimler ile koşullu erişim *](https://docs.microsoft.com/azure/active-directory/conditional-access/controls)|[Azure MFA](https://docs.microsoft.com/azure/multi-factor-authentication/)<br><br>[Özel denetimler ile koşullu erişim *](https://docs.microsoft.com/azure/active-directory/conditional-access/controls)|[Azure MFA](https://docs.microsoft.com/azure/multi-factor-authentication/)<br><br>[Azure MFA sunucusu](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfaserver-deploy)<br><br>[Üçüncü taraf MFA](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configure-additional-authentication-methods-for-ad-fs)<br><br>[Özel denetimler ile koşullu erişim *](https://docs.microsoft.com/azure/active-directory/conditional-access/controls)|
|Hangi kullanıcı hesabı durumları destekleniyor mu?|Devre dışı bırakılmış hesapları<br>(en fazla 30 dakika Gecikmeli)|Devre dışı bırakılmış hesapları<br><br>Hesap kilitli<br><br>Hesabın süresi doldu<br><br>Parolanın süresi dolsun<br><br>Oturum açma saatleri|Devre dışı bırakılmış hesapları<br><br>Hesap kilitli<br><br>Hesabın süresi doldu<br><br>Parolanın süresi dolsun<br><br>Oturum açma saatleri|
|Koşullu erişim seçenekleri nelerdir?|[Azure AD Premium ile Azure AD koşullu erişim](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)|[Azure AD Premium ile Azure AD koşullu erişim](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)|[Azure AD Premium ile Azure AD koşullu erişim](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)<br><br>[AD FS talep kuralları](https://adfshelp.microsoft.com/AadTrustClaims/ClaimsGenerator)|
|Desteklenen eski protokolleri engelliyor?|[Evet](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-conditions)|[Evet](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-conditions)|[Evet](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/access-control-policies-w2k12)|
|Logosu, resmi ve oturum açma sayfalarındaki açıklama özelleştirebilir miyim?|[Evet, Azure AD Premium ile](https://docs.microsoft.com/azure/active-directory/customize-branding)|[Evet, Azure AD Premium ile](https://docs.microsoft.com/azure/active-directory/customize-branding)|[Evet](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-federation-management#customlogo)|
|Hangi Gelişmiş senaryolar desteklenir?|[Akıllı parola kilitleme](https://docs.microsoft.com/azure/active-directory/active-directory-secure-passwords)<br><br>[Azure AD Premium P2 ile kimlik bilgileri rapor sızmasına](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-risk-events)|[Akıllı parola kilitleme](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-smart-lockout)|Çok siteli düşük gecikme süreli kimlik doğrulama sistemi<br><br>[AD FS extranet kilitleme](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configure-ad-fs-extranet-soft-lockout-protection)<br><br>[Üçüncü taraf kimlik sistemleriyle tümleşme](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-federation-compatibility)|

> [!NOTE] 
> Azure AD koşullu erişim özel denetimlerinde şu anda desteklemiyor cihaz kaydı.

## <a name="recommendations"></a>Öneriler
Kimlik sisteminizde bulut uygulamaları ve satır iş kolu uygulamaları geçirme ve bulutta kullanılabilir hale getirmek için kullanıcılarınızın erişim sağlar. Yetkili kullanıcıların üretken ve kötü aktörleri kuruluşunuzun hassas verilerini dışında tutmak için kimlik doğrulaması uygulamalara erişimi denetler.

Kullanabilir veya aşağıdaki nedenlerle seçin hangi kimlik doğrulama yöntemi için parola karması eşitlemeyi etkinleştir:

1. **Yüksek kullanılabilirlik ve olağanüstü durum kurtarma**. Doğrudan kimlik doğrulama ve Federasyon, şirket içi altyapıya kullanır. Doğrudan kimlik doğrulaması için şirket içi ayak izini sunucu donanımı içerir ve aracıları doğrudan kimlik doğrulama ağ gerektirir. Federasyon için şirket içi ayak izini daha da büyük. Çevre ağınızda Ara sunucu kimlik doğrulama isteklerine sunucuları ve iç federasyon sunucuları gerektirir. 

    Tek hata noktalarından kaçınmak için yedekli sunucuları dağıtın. Herhangi bir bileşenin başarısız olursa kimlik doğrulama istekleri her zaman verilmeyecek. Geçişli kimlik doğrulaması hem Federasyon etki alanı denetleyicilerinde de başarısız olabilir kimlik doğrulama isteklerine yanıt vermek için de kullanır. Bu bileşenlerin çok iyi durumda kalmak için bakım gerekir. Bakım planlanması ve doğru bir şekilde değil, kesintiler daha yüksektir. Microsoft Azure AD bulut kimlik hizmeti her zaman kullanılabilir ve genel olarak ölçekler için parola karması eşitleme kullanarak kesintilerinin önüne geçin.

2. **Şirket içi kesinti hayatta**.  Şirket içi kesinti siber saldırı veya olağanüstü durum nedeniyle sonuçlarını reputational marka zarar bir saldırı ile uğraşmak başlatamadı paralyzed kuruluş arasında değişen önemli olabilir. Yakın zamanda pek çok kuruluş gitmek kendi şirket içi sunucular neden hedeflenen fidye yazılımı dahil olmak üzere, kötü amaçlı yazılım saldırılarını kurbanı yoktu. Microsoft, müşterilerin bu tür saldırılar işlem yardımcı olur, kuruluşların iki kategoriye görür:

   * Parola Karması eşitleme hakkında daha önce açık olan kuruluşlar, parola karması eşitleme kullanmak için kendi kimlik doğrulama yöntemini değiştirildi. Bunlar, yalnızca birkaç saat içinde yeniden çevrimiçi. Office 365 e-posta erişimi kullanarak sorunları çözün ve diğer bulut tabanlı iş yükleri erişmek için işe yaradığını.

   * Daha önce parola karma eşitlemesini etkinleştirme fırsatınız kuruluşlar, güvenilmeyen dış tüketici e-posta sistemlerinin iletişim ve sorunları çözmek için başvurmadan gerekiyordu. Bu gibi durumlarda, bunları birkaç hafta veya yeniden çalışır olması için daha fazla işlem.

3. **Kimlik koruması**. Kullanıcıların bulutta korumak için en iyi yollarından biri, Azure AD kimlik koruması ile Azure AD Premium P2 olduğu. Microsoft Internet kullanıcı için sürekli tarar ve parola kötü aktörleri satmak ve koyu web üzerinde kullanılabilmesini listeler. Azure AD, herhangi bir kullanıcı adları ve parolalar, kuruluşunuzdaki tehlikede olursa doğrulamak için bu bilgileri kullanabilirsiniz. Bu nedenle hangi kimlik doğrulama yöntemi, Federasyon olup olmadığını, kullanın veya doğrudan kimlik doğrulama ne olursa olsun parola karması eşitlemeyi etkinleştirmek için önemlidir. Sızdırılan kimlik bilgilerine bir rapor olarak sunulur. Engelleme veya kullanıcıları, sızan parola ile oturum açmayı denediğinizde parolalarını değiştirmek için bu bilgileri kullanın.

Son olarak, aşağıdakilere göre [Gartner](https://info.microsoft.com/landingIAMGartnerreportregistration.html), Microsoft'un kimlik ve erişim yönetimi işlevleri en tam özellikli kümesi vardır. Microsoft tanıtıcıları [450 milyardan fazla kimlik doğrulama isteklerini](https://www.microsoft.com/en-us/security/intelligence-report) hemen her CİHAZDAN binlerce Office 365 gibi SaaS uygulamasına erişim sağlamak için her ay. 

## <a name="conclusion"></a>Sonuç

Bu makalede, kuruluşlar, yapılandırma ve bulut uygulamalarına erişimini desteklemek üzere dağıtma çeşitli kimlik doğrulama seçenekleri açıklanmaktadır. Çeşitli iş, güvenlik ve teknik gereksinimlerinizi karşılamak için kuruluşların parola karma eşitlemesi, geçişli kimlik doğrulaması ve Federasyon arasında seçebilirsiniz. 

Her kimlik doğrulama yöntemi göz önünde bulundurun. Çözüm ve oturum açma işleminin kullanıcı deneyimi dağıtın, iş gereksinimlerinizi adres için gereken çabayı mu? Kuruluşunuz Gelişmiş senaryoları ve iş sürekliliği özellikleri, her kimlik doğrulama yönteminin gerekip gerekmediğini değerlendirin. Son olarak, her kimlik doğrulama yöntemi ilişkin konular değerlendirin. Bunlardan herhangi birinin seçtiğiniz uygulama gelen engelliyor mu?

## <a name="next-steps"></a>Sonraki adımlar

Günümüz Dünyası, tehditleri günde 24 saat mevcut olan ve her yerden gelir. Doğru kimlik doğrulama yöntemini uygulamak ve bu güvenlik riskleri azaltmak ve kimliklerinizi koruyun.

[Başlama](https://docs.microsoft.com/azure/active-directory/get-started-azure-ad) Azure AD ile ve kuruluşunuz için doğru kimlik doğrulama çözümü dağıtın.

Geçiş hakkında düşünüyorsanız federe kimlik doğrulaması bulut, daha fazla bilgi edinin [oturum açma yöntemini değiştirme](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-user-signin#changing-the-user-sign-in-method). Planlama ve uygulama geçişi yardımcı olması için kullanın [bu proje dağıtım planlarında](https://aka.ms/deploymentplans).
