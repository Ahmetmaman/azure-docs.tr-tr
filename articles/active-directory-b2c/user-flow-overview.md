---
title: Azure Active Directory B2C Kullanıcı akışları | Microsoft Docs
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C Genişletilebilir ilke çerçevesi ve çeşitli Kullanıcı akışları oluşturma hakkında daha fazla bilgi edinin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 07/30/2020
ms.custom: project-no-code
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 06253b571fd71623501c27fd5b0d9d4013727fc2
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "94840208"
---
# <a name="user-flows-in-azure-active-directory-b2c"></a>Azure Active Directory B2C'de kullanıcı akışları

Uygulamalarınızın en yaygın kimlik görevlerini ayarlamanıza yardımcı olmak için Azure AD B2C portalı **kullanıcı akışları** olarak adlandırılan önceden tanımlanmış, yapılandırılabilir ilkeler içerir. Bir Kullanıcı akışı, kullanıcıların oturum açma, kaydolma, profil düzenleme veya bir parolayı sıfırlama gibi şeyler yaparken uygulamanızla nasıl etkileşime gireceğini belirlemenizi sağlar. Kullanıcı akışları ile aşağıdaki özellikleri kontrol edebilirsiniz:

- Facebook veya yerel hesaplar gibi sosyal hesaplar gibi, oturum açma için kullanılan hesap türleri
- Tüketiciden toplanacak öznitelikler (örneğin, ilk ad, posta kodu ve ayakkabı boyutu)
- Azure AD Multi-Factor Authentication
- Kullanıcı arabiriminin özelleştirilmesi
- Uygulamanın bir belirteçte talepler olarak aldığı bilgiler

Kiracınızda farklı türlerde birçok kullanıcı akışı oluşturabilir ve bunları gerektiği şekilde uygulamalarınızda kullanabilirsiniz. Kullanıcı akışları, uygulamalar arasında yeniden kullanılabilir. Bu esneklik, kodunuzda en az değişiklik yapmadan kimlik deneyimlerini tanımlamanızı ve değiştirmenizi sağlar. Uygulamanız, bir Kullanıcı akış parametresi içeren standart bir HTTP kimlik doğrulama isteği kullanarak bir Kullanıcı akışı tetikler. Özelleştirilmiş bir [belirteç](tokens-overview.md) yanıt olarak alınır.

Aşağıdaki örneklerde, kullanılacak kullanıcı akışını belirten "p" sorgu dizesi parametresi gösterilmektedir:

```
https://contosob2c.b2clogin.com/contosob2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e      // Your registered Application ID
&redirect_uri=https%3A%2F%2Flocalhost%3A44321%2F    // Your registered Reply URL, url encoded
&response_mode=form_post                            // 'query', 'form_post' or 'fragment'
&response_type=id_token
&scope=openid
&nonce=dummy
&state=12345                                        // Any value provided by your application
&p=b2c_1_siup                                       // Your sign-up user flow
```

```
https://contosob2c.b2clogin.com/contosob2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e      // Your registered Application ID
&redirect_uri=https%3A%2F%2Flocalhost%3A44321%2F    // Your registered Reply URL, url encoded
&response_mode=form_post                            // 'query', 'form_post' or 'fragment'
&response_type=id_token
&scope=openid
&nonce=dummy
&state=12345                                        // Any value provided by your application
&p=b2c_1_siin                                       // Your sign-in user flow
```

## <a name="user-flow-versions"></a>Kullanıcı akışı sürümleri

Azure AD B2C çeşitli Kullanıcı akışı türlerini içerir:

- Kaydolma ve oturum açma deneyimlerinin her ikisi de tek bir yapılandırmayla kaydolun **ve oturum açın** . Kullanıcılar, bağlama göre doğru yolun altına alınır. Ayrıca, ayrı **kaydolma** veya **oturum açma** Kullanıcı akışıdır. Ancak, genellikle Birleşik oturum açma ve oturum açma kullanıcı akışını öneririz.
- **Profil düzenleme** -kullanıcıların profil bilgilerini düzenlemesine olanak sağlar.
- **Parola sıfırlama** -kullanıcıların parolalarını sıfırlayıp sıfırlayamayacağını ve bunların nasıl sıfırlanamayacağını yapılandırmanıza olanak sağlar.

Çoğu Kullanıcı akış türü hem **Önerilen** bir sürüme hem de **Standart** sürüme sahiptir. Ayrıntılar için bkz. [Kullanıcı akış sürümleri](user-flow-versions.md).

> [!IMPORTANT]
> Daha önce Azure AD B2C Kullanıcı akışları ile çalıştıysanız, Kullanıcı akışı sürümlerine başvurduğumuz şekilde değiştirildiğini fark edeceksiniz. Daha önce V1 (üretime hazır) sürümleriyle V1.1 ve V2 (önizleme) sürümlerini sunduk. Artık Kullanıcı akışlarını iki sürüme birleştiriyoruz:
>
>- **Önerilen** Kullanıcı akışları, Kullanıcı akışlarının yeni önizleme sürümlerindedir. Kapsamlı olarak test edilmiştir ve eski **v2** ve **v 1.1** sürümlerinin tüm özelliklerini birleştirirler. İleri giderek, önerilen yeni Kullanıcı akışları korunur ve güncelleştirilir. Bu yeni önerilen Kullanıcı akışlarına taşıdıktan sonra, serbest bırakılanlar gibi yeni özelliklere erişebilirsiniz.
>- Daha önce **v1** olarak bilinen **Standart** Kullanıcı akışları, genel olarak kullanılabilir, üretime hazır Kullanıcı akışıdır. Kullanıcı akışlarınız görev açısından kritiktir ve yüksek oranda kararlı sürümlere bağlıysa, standart Kullanıcı akışlarını kullanmaya devam edebilirsiniz ve bu sürümlerin korunmayacağını ve güncelleştirilmediğini da sürdürmez.
>
>Eski tüm önizleme Kullanıcı akışları (V 1.1 ve v2) **1 ağustos 2021 '** e kadar kullanımdan kalkmaya yönelik bir yoldur. Mümkün olan yerlerde, en son özellikler ve güncelleştirmelerden her zaman faydalanabilir olması [için, **Önerilen** Yeni Kullanıcı akışlarına](user-flow-versions.md#how-to-switch-to-a-new-recommended-user-flow) en kısa sürede geçmeniz önerilir.

## <a name="linking-user-flows"></a>Kullanıcı akışlarını bağlama

Yerel hesaplarla **kaydolma veya oturum açma** Kullanıcı akışı, deneyimin ilk sayfasında **parolayı unuttum?** bağlantısını içerir. Bu bağlantıya tıkladığınızda parola sıfırlama Kullanıcı akışı otomatik olarak tetiklenemez.

Bunun yerine, hata kodu `AADB2C90118` uygulamanıza döndürülür. Uygulamanızın, parolayı sıfırlayan belirli bir kullanıcı akışını çalıştırarak bu hata kodunu işlemesi gerekir. Bir örneği görmek için, Kullanıcı akışlarının bağlantısını gösteren [basit bir ASP.net örneğine](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-DotNet-SUSI) göz atın.

## <a name="email-address-storage"></a>E-posta adresi depolaması

Bir Kullanıcı akışının parçası olarak bir e-posta adresi gerekebilir. Kullanıcı bir sosyal kimlik sağlayıcısı ile kimlik doğrulaması gerçekleştiriyorsa, e-posta adresi, **Otherpostalarını** özelliğinde depolanır. Yerel bir hesap Kullanıcı adını temel alıyorsa, e-posta adresi güçlü bir kimlik doğrulama ayrıntısı özelliğinde depolanır. Yerel bir hesap bir e-posta adresini temel alıyorsa, e-posta adresi **Signınnames** özelliğinde depolanır.

Bu durumda e-posta adresinin doğrulanması garanti edilmez. Bir kiracı yöneticisi, yerel hesaplar için temel ilkelerde e-posta doğrulamayı devre dışı bırakabilir. E-posta adresi doğrulama etkin olsa bile, bir sosyal kimlik sağlayıcısından geliyorsa ve değiştirilmedikleri durumlarda adresler doğrulanmaz.

Microsoft Graph API 'SI aracılığıyla yalnızca **Otherpostalarını** ve **signınnames** özellikleri sunulur. Güçlü kimlik doğrulama ayrıntısı özelliğindeki e-posta adresi kullanılamıyor.

## <a name="next-steps"></a>Sonraki adımlar

Önerilen Kullanıcı akışlarını oluşturmak için [öğretici: Kullanıcı akışı oluşturma](tutorial-create-user-flows.md)' daki yönergeleri izleyin.
