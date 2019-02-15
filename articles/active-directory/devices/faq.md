---
title: Azure Active Directory cihaz yönetimi hakkında SSS | Microsoft Docs
description: Azure Active Directory cihaz yönetimi hakkında SSS.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
ms.assetid: cdc25576-37f2-4afb-a786-f59ba4c284c2
ms.service: active-directory
ms.subservice: devices
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/14/2019
ms.author: markvi
ms.reviewer: jairoc
ms.collection: M365-identity-device-management
ms.openlocfilehash: 31e380379b5237f6b1a72b3427eb857f64d55c2e
ms.sourcegitcommit: f715dcc29873aeae40110a1803294a122dfb4c6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2019
ms.locfileid: "56269068"
---
# <a name="azure-active-directory-device-management-faq"></a>Azure Active Directory cihaz yönetimi hakkında SSS

**S: Ben kısa bir süre önce cihazın kayıtlı. Azure portalındaki kullanıcı Bilgilerim altında cihazın neden göremiyorum? Ya da cihaz sahibinin katılmış cihazların hibrit Azure Active Directory (Azure AD) için yok olarak neden işaretlenmiş?**

**Y:** Hibrit Azure AD'ye katılmış olan Windows 10 cihazları görünmüyor altında **kullanıcı cihazları**.
Kullanım **tüm cihazlar** Azure portalında görünümü. Bir PowerShell de kullanabilirsiniz [Get-MsolDevice](https://docs.microsoft.com/powershell/module/msonline/get-msoldevice?view=azureadps-1.0) cmdlet'i.

Yalnızca aşağıdaki cihazlar altında listelenen **kullanıcı cihazları**:

- Hibrit Azure AD'ye olmayan tüm kişisel cihazların katıldı. 
- Tüm Windows 10 veya Windows Server 2016 cihazları.
- Tüm Windows olmayan cihazlar. 

--- 

**S: İstemci cihaz kayıt durumu nedir olduğunu nasıl öğrenebilirim?**

**Y:** Azure portalında Git **tüm cihazlar**. Cihaz için cihaz kimliğini kullanarak arama yapın. Birleştirme türü sütunu altındaki değerini denetleyin. Bazı durumlarda, cihaz sıfırlama görüntüsü yeniden oluşturulabildiği veya. Bu nedenle de cihazın aygıt kayıt durumunu denetlemek için önemlidir:

- Windows 10 ve Windows Server 2016 veya üzeri cihazlar için çalıştırma `dsregcmd.exe /status`.
- Alt düzey işletim sistemi sürümleri için çalıştırma `%programFiles%\Microsoft Workplace Join\autoworkplace.exe`.

---

**S: Azure portalındaki kullanıcı bilgileri altında cihaz kaydı görüyorum. Ve durum kayıtlı bir cihazda bakın. Doğru koşullu erişim kullanmanın ayarladığım?**

**Y:** Tarafından gösterilen cihaz birleşim durumu **DeviceID**gerekir Azure AD'de durumuyla eşleşen ve koşullu erişim için herhangi bir değerlendirme ölçütleri karşılayan. Daha fazla bilgi için [gerektiren yönetilen cihazlar için koşullu erişim ile bulut uygulaması erişimi](../conditional-access/require-managed-devices.md).

---

**S: Ben Azure portalında veya Windows PowerShell'i kullanarak Cihazınızı silindi. Ancak hala kayıtlı cihazdaki yerel durumu belirtiyor.**

**Y:** Bu işlem, tasarım gereğidir. Cihaz, bulutta kaynaklara erişimi yok. 

Yeniden kaydetmek isterseniz, cihazda el ile bir eylem gerçekleştirmeniz gerekir. 

Windows 10 ve Windows Server 2016'de, şirket içi Active Directory etki alanına katılan birleşim durumu temizlemek için aşağıdaki adımları uygulayın:

1.  Komut istemini yönetici olarak açın.

2.  `dsregcmd.exe /debug /leave` yazın.

3.  Oturumu kapatın ve yeniden Azure AD ile cihaz kaydeden zamanlanmış tetikleyici için oturum açın. 

Şirket içi Active Directory etki alanına katılan alt düzey Windows işletim sistemi sürümleri için aşağıdaki adımları uygulayın:

1.  Komut istemini yönetici olarak açın.
2.  `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /l"` yazın.
3.  `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /j"` yazın.

---

**S: Yinelenen cihaz girişlerini Azure portalında neden görüyorum?**

**Y:**

-   Yinelenen çalışır ayrılma ve aynı cihaza katılabilir, Windows 10 ve Windows Server 2016 için yinelenen girdiler neden olabilir. 

-   Kullanan her bir Windows kullanıcı **eklemek iş veya Okul hesabı** cihaz aynı ada sahip yeni bir cihaz kaydı oluşturur.

-   Şirket içi Azure Directory etki alanına katılmış olan alt düzey Windows işletim sistemi sürümleri için otomatik kayıt, cihaza oturum açtığı her etki alanı kullanıcısı için aynı cihaz adı ile yeni bir cihaz kaydı oluşturur. 

-   Cihaz aynı ada sahip başka bir kayıt olarak silinebilen, yeniden ve aynı adla yeniden katılınması bir Azure AD alanına katılmış makine gösterilir.

---

**S: Neden bir kullanıcı hala kaynakları ı Azure portalından devre dışı bir CİHAZDAN erişebilir miyim?**

**Y:** Bir iptal etme uygulanacak bir saat sürer.

>[!NOTE] 
>Kayıtlı cihazlar için kullanıcıların kaynaklara erişemez emin olmak için cihaz silme öneririz. Daha fazla bilgi için [cihaz kaydı nedir?](https://docs.microsoft.com/intune/deploy-use/enroll-devices-in-microsoft-intune). 

---

## <a name="azure-ad-join-faq"></a>Azure AD katılımı ile ilgili SSS

**S: Nasıl ben bir Azure AD'ye katılmış yerel olarak cihazda ayrılma?**

**Y:** 
- İçin hibrit Azure AD'ye katılan cihazlar otomatik kaydı Kapat emin olun. Ardından zamanlanmış görev cihazı yeniden kaydedin değil. Ardından, bir yönetici olarak bir komut istemi açın ve girin `dsregcmd.exe /debug /leave`. Veya toplu olarak ayrılma için birkaç cihaz arasında bir komut dosyası olarak şu komutu çalıştırın.

- Saf Azure AD'ye katılan cihazlar çevrimdışı yerel yönetici hesabına sahip veya bir oluşturma emin olun. Tüm Azure AD kullanıcı kimlik bilgileriyle oturum açamazsınız. Ardından, Git **ayarları** > **hesapları** > **işe veya okula erişim**. Hesabınızı seçip **Bağlantıyı Kes**. Komut istemlerini izleyin ve istendiğinde yerel yönetici kimlik bilgilerini girin. Ayrılma işlemi tamamlamak için cihazı yeniden başlatın.

---

**S: Kullanıcılar Azure AD'de devre dışı veya silinmiş bir Azure AD'ye katılmış cihazların oturum açarak?**

**Y:** Evet. Windows, önbelleğe alınan kullanıcı adı ve hatta ağ bağlantısı hızlıca erişmenizi önceden açmış kullanıcılar izin veren parola özelliği vardır. 

Cihazın silindi veya Azure AD'de devre dışı olduğunda Windows cihaza bilinmiyor. Bu nedenle önceden açmış kullanıcılar, Masaüstü önbelleğe alınan kullanıcı adı ve parola ile erişmek devam edin. Ancak cihaz silinmiş veya devre dışı olarak Kullanıcılar Cihaz tabanlı koşullu erişim tarafından korunan herhangi bir kaynağa erişemez. 

Daha önce oturum istemediğiniz Kullanıcılar Cihaz erişemez. Önbelleğe alınan kullanıcı adı ve parola için bunları etkin yoktur. 

---

**S: Devre dışı bırakılan veya silinen kullanıcılar Azure AD'ye katılmış cihazlar için oturum açabilir?**

**Y:** Evet, ancak yalnızca sınırlı bir süreliğine. Bir kullanıcı silindi veya Azure AD'de devre dışı olduğunda Windows cihaza hemen bilinir. Bu nedenle önceden açmış kullanıcılar, Masaüstü önbelleğe alınan kullanıcı adı ve parola ile erişebilirsiniz. 

Genellikle, cihaz dört saatten daha kısa bir süre içinde kullanıcı durumunu farkındadır. Ardından Windows Masaüstü için bu kullanıcıların erişimini engeller. Kullanıcı silindi veya Azure AD'de devre dışı olarak tüm belirteçleri iptal edilir. Bu nedenle bunlar herhangi bir kaynağa erişemez. 

Daha önce oturum yaramadı silinmiş veya devre dışı bırakılmış kullanıcılar, bir cihaz erişemez. Önbelleğe alınan kullanıcı adı ve parola için bunları etkin yoktur. 

---

**S: Neden kullanıcılarımın UPN değiştirdikten sonra sorunları Azure AD'ye katılmış cihazlarda gerekiyor?**

**Y:** Şu anda, UPN değişiklikler Azure AD'ye katılmış cihazlarda tam olarak desteklenmemektedir. Bu nedenle Azure AD ile kimlik doğrulamasını UPN değişikliklerini sonra başarısız olur. Sonuç olarak, kullanıcılar SSO ve kullanıcıların cihazlarında koşullu erişim verir. Şu anda, kullanıcıların bu sorunu çözmek için yeni UPN kullanarak "Kullanıcı diğer" kutucuğunda Windows için oturum açmanız gerekir. Şu anda bu sorunu gidermeye çalışıyoruz. Ancak, kullanıcıların oturum Windows iş için Hello imzalama bu sorunla karşılaşmaya değil. 

---

**S: Kullanıcılar cihazları Azure AD'ye katılmış yazıcıları arama yapamazsınız. Bu cihazlardan yazdırma nasıl etkinleştirebilirim?**

**Y:** Katılmış cihazların Azure AD'ye yazıcılar dağıtmak için bkz: [ön kimlik doğrulaması ile Windows Server karma bulut yazdırma dağıtma](https://docs.microsoft.com/windows-server/administration/hybrid-cloud-print/hybrid-cloud-print-deploy). Hibrit bulut yazdırma dağıtmak için bir şirket içi Windows Server ihtiyacınız vardır. Bulut tabanlı yazdırma hizmeti şu anda kullanılamıyor. 

---

**S: Nasıl bir uzak Azure AD'ye bağlanabilirim katılmış?**

**Y:** Bkz: [Azure Active Directory'ye katılmış uzak bir Bilgisayara Bağlan](https://docs.microsoft.com/windows/client-management/connect-to-remote-aadj-pc).

---

**S: Kullanıcılarım neden görüyorum *oraya buradan ulaşamazsınız*?**

**Y:** Belirli cihaz durumu gerektirecek şekilde belirli bir koşullu erişim kuralları yapılandırabilir? Cihaz ölçütleri karşılamıyorsa, kullanıcıların engellenir ve bu iletiyi görürler. Koşullu erişim ilkesi kurallarını değerlendirin. Cihaz oluşmaması için ölçütleri karşıladığından emin olun.

---

**S: Neden bazı kullanıcılarımın Azure çok faktörlü kimlik doğrulama istemleri Azure AD'ye katılmış cihazlarda elde etmezsiniz?**

**Y:** Bir kullanıcı katılın veya multi-Factor Authentication'ı kullanarak bir cihazı Azure AD'ye kaydetme. Daha sonra cihaz söz konusu kullanıcı için güvenilir bir ikinci faktör olur. Aynı kullanıcı oturum açtığında cihaza ve uygulamaya erişen her Azure AD cihaz ikinci bir faktör olarak dikkate alır. Bu ek çok faktörlü kimlik doğrulama istemleri olmadan uygulamalara sorunsuz bir şekilde erişmek bu kullanıcı sağlar. 

Bu davranışı:

- Geçerli Azure AD'ye katılmış ve Azure AD cihazları - kayıtlı ancak cihazlar için hibrit Azure AD'ye katılmamış.

- Bu cihaza açan diğer bir kullanıcı için geçerli değildir. Bu cihaz erişim kadar tüm diğer kullanıcılar, çok faktörlü kimlik doğrulaması sınaması alın. Ardından bunlar çok faktörlü kimlik doğrulaması gerektiren uygulamalarda erişebilirsiniz.

---

**S: Neden bir *kullanıcı adı veya parola yanlış* miyim şimdi katıldı Azure AD'ye ileti bir cihaz için?**

**Y:** Bu senaryo için yaygın nedenler aşağıda belirtilmiştir:

- Kullanıcı kimlik bilgilerinizi artık geçerli değil.

- Bilgisayarınızı Azure Active Directory ile iletişim kuramıyor. Tüm ağ bağlantısı sorunlarını denetleyin.

- Federasyon oturum açma etkin ve erişilebilir olan WS-Trust uç noktaları desteklemek için Federasyon sunucunuz gerektirir. 

- Geçişli kimlik doğrulaması etkin. Bu nedenle parolanızı geçici oturum açtığınızda değiştirilmesi gerekir.

---

**S: Neden görüyorum *hata... bir hata oluştu!* Azure AD'ye denediğimde iletişim katılın Bilgisayarımda?**

**Y:** Intune ile Azure Active Directory kayıt ayarladığınızda bu hata oluşur. Azure AD'ye katılmasını sağlamaya çalışan kullanıcının doğru Intune lisansı atanmış olduğundan emin olun. Daha fazla bilgi için [Windows cihazları için kaydı ayarlama](https://docs.microsoft.com/intune/windows-enroll).  

---

**S: Neden hata bilgileri almadım olsa da Azure AD'ye my girişimi bir bilgisayar başarısız katılmak?**

**Y:** Olası bir nedeni, cihaza yerleşik yerel yönetici hesabını kullanarak oturum açtığını ' dir. Kurulumu tamamlamak için Azure Active Directory join kullanmadan önce farklı bir yerel hesap oluşturun. 

---

**Soru: bizim Windows 10 cihazlarda mevcut kuruluş P2P erişim MS sertifikalar nelerdir?**

**Y:** Kuruluş P2P erişim MS sertifikalarının her ikisi de Azure AD, Azure AD'ye katılmış ve hibrit Azure AD'ye katılmış cihazlar. Bu sertifikalar, Uzak Masaüstü senaryoları için aynı kiracıda cihazları arasında güven etkinleştirmek için kullanılır. Cihaza bir sertifikanın verildiği ve başka bir kullanıcıya verilir. Cihaz sertifika varsa `Local Computer\Personal\Certificates` ve bir gün boyunca geçerlidir. (Yeni bir sertifika vererek) bu sertifikanın yenilenmesi cihaz Azure AD'ye hala etkin değilse. Kullanıcı sertifikası varsa `Current User\Personal\Certificates` ve bu sertifika aynı zamanda bir gün boyunca geçerlidir, ancak bir kullanıcı başka bir Azure AD alanına katılmış cihaz bir Uzak Masaüstü oturumu çalıştığında üzerine verilir. Bitiş tarihinde yenilenmez. Hem bu sertifikaların mevcut MS Kuruluş P2P erişim sertifikası kullanarak verilen `Local Computer\AAD Token Issuer\Certificates`. Bu sertifika, cihaz kaydı sırasında Azure AD tarafından verilir. 

---

**Q:Why bizim Windows 10 cihazlarda MS-Kuruluş-P2P-erişim tarafından verilen birden fazla süresi dolmuş sertifikaları görüyor musunuz? Bunları nasıl silebilir miyim?**

**Y:** Windows 10 sürüm 1709 ve daha düşük burada MS Kuruluş P2P erişim süresi dolmuş sertifikaları mevcut bilgisayar deposunda şifreleme sorunları nedeniyle devam tanımlanan bir sorun oluştu. Süresi dolmuş sertifikaları çok sayıda işleyemiyor tüm VPN istemcileri (örneğin Cisco AnyConnect) kullanıyorsanız, kullanıcılarınızın ağ bağlantısına sahip bir sorunla karşılaşırsanız. Bu tür süresi dolmuş kuruluş P2P erişim MS sertifikaları otomatik olarak silmek için Windows 10, 1803 sürümde bu sorunu düzeltildi. Windows 10, 1803 cihazlarınızı güncelleştirerek bu sorunu çözebilir. Güncelleştirilecek bulamıyorsanız, olumsuz bir etkisi olmadan bu sertifikaları silebilirsiniz.  

---


## <a name="hybrid-azure-ad-join-faq"></a>Hibrit Azure AD katılımı ile ilgili SSS

**S: Sorun giderme nereden bulabilirim hibrit Azure AD'ye katılma hatalarını tanılamak için bilgi?**

**Y:** Sorun giderme bilgileri için şu makalelere bakın:

- [Hibrit Azure Active Directory sorun giderme alanına katılmış Windows 10 ve Windows Server 2016 cihazları](troubleshoot-hybrid-join-windows-current.md)

- [Alt düzey cihazları katılmış karma Azure Active Directory sorun giderme](troubleshoot-hybrid-join-windows-legacy.md)
 
**S: Yinelenen bir Azure AD neden görüyorum my Windows 10 hibrit Azure AD'ye kayıtlı kaydı alanına katılmış cihaz Azure AD'ye cihaz listesinde?**

**Y:** Kullanıcılarınızın etki alanına katılmış bir cihazda uygulamalar için kendi hesapları eklediğinizde, bunlar ile istenebilir **hesabı eklemek için Windows?** Olacaklardır **Evet** satırına cihazı Azure AD'ye kaydeder. Güven türü kayıtlı Azure AD işaretlenir. Hibrit Azure AD'ye katılma kuruluşunuzda etkinleştirdikten sonra cihazın Azure AD'ye katılmış karma de alır. Aynı cihaz için bundan sonra iki cihaz durumu gösterilir. 

Hibrit Azure AD'ye katılma Azure AD'ye kayıtlı durumu daha önceliklidir. Bu nedenle Cihazınızı herhangi bir kimlik doğrulama ve koşullu erişim değerlendirmesinin için Azure AD'ye katılmış karma olarak kabul edilir. Azure AD Portalı'ndan kayıtlı Azure AD cihaz kaydı güvenli bir şekilde silebilirsiniz. Öğrenme [önlemek veya Windows 10 makinesi üzerinde bu ikili durum Temizleme](https://docs.microsoft.com/azure/active-directory/devices/hybrid-azuread-join-plan#review-things-you-should-know). 


---

**S: Neden kullanıcılarımın UPN değiştirdikten sonra sorunları Windows 10 hibrit Azure AD'ye katılmış cihazlarda gerekiyor?**

**Y:** Şu anda UPN değişiklikler ile hibrit Azure AD'ye katılmış cihazları tam olarak desteklenmemektedir. Kullanıcıların cihazda oturum açın ve kullanıcıların şirket içi uygulamalara karşın, bir UPN değiştirdikten sonra Azure AD ile kimlik doğrulaması başarısız olur. Sonuç olarak, kullanıcılar SSO ve kullanıcıların cihazlarında koşullu erişim verir. Şu anda, cihaz ("dsregcmd /leave" yükseltilmiş ayrıcalıklarla çalıştır) Azure AD'den ayrılma gerekir ve yeniden katılabilir (otomatik olarak gerçekleşir) sorunu gidermek için. Şu anda bu sorunu gidermeye çalışıyoruz. Ancak, kullanıcıların oturum Windows iş için Hello imzalama bu sorunla karşılaşmaya değil. 

---

**S: Windows 10 hibrit Azure AD'ye katılmış cihazları görebilmesi için bulut kaynaklarına erişim elde etmek için etki alanı denetleyicisi gerektiriyor mu?**

**Y:** Hayır. Onra Windows 10 hibrit Azure AD'ye katılımı tamamlandıktan ve kullanıcı en az bir kez oturum açtıktan, cihaz görebilmesi için bulut kaynaklarına erişmek için etki alanı denetleyicisi gerektirmez. Windows 10 alabilirsiniz çoklu oturum açma Azure AD uygulamalarına her yerden bir parola değiştirildiğinde dışında bir internet bağlantısı ile. Bir parola kurumsal ağ dışından (örneğin, Azure AD SSPR kullanarak) olarak değiştirilirse, kullanıcının, cihaza yeni parola ile oturum açabilir gelmeden önce görebilmesi için etki alanı denetleyicisi olması gerekir. Aksi takdirde, yalnızca Azure AD tarafından geçersiz kılındı ve çoklu oturum açma üzerinde engeller, eski bir parola ile oturum açabilirsiniz. Bununla birlikte, Windows Hello iş için kullanırken bu sorun oluşmaz. Bir parolayı değiştirdikten sonra bunlar görebilmesi için kendi etki alanı denetleyicisi olmasa bile tek almak için oturum açın. Windows iş için Hello devam eden kullanıcıların Azure AD uygulamaları için oturum. 

---


## <a name="azure-ad-register-faq"></a>Azure AD kaydı SSS

**S: Android veya iOS KCG cihazları kaydedebilir miyim?**

**Y:** Evet, ancak yalnızca Azure cihaz Kayıt Hizmeti'ni ve karma müşteriler için. Şirket cihaz kaydı hizmeti Active Directory Federasyon Hizmetleri'nde (AD FS) ile desteklenmiyor.

**S: Bir macOS cihazı nasıl kaydedebilir miyim?**

**Y:** Aşağıdaki adımları uygulayın:

1.  [Uyumluluk ilkesi oluşturma](https://docs.microsoft.com/intune/compliance-policy-create-mac-os)
2.  [MacOS cihazlar için koşullu erişim ilkesi tanımlama](../active-directory-conditional-access-azure-portal.md) 

**Notlar:**

- Koşullu erişim ilkenizi dahil kullanıcıların bir [macOS için Office sürümü desteklenen](../conditional-access/technical-reference.md#client-apps-condition) kaynaklara erişmek için. 

- İlk erişim denemede sırasında kullanıcılarınızın şirket Portalı'nı kullanarak cihazını kaydetmesi istenir.

