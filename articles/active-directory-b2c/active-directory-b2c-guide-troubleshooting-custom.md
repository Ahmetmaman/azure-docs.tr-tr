---
title: Azure Active Directory B2C'de özel ilke sorunlarını giderme | Microsoft Docs
description: Özel ilkeleri Azure Active Directory B2C ile çalışırken hataları çözmeye yaklaşımlar hakkında bilgi edinin.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 05/07/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 8bb95ae776c329e67e9f9936213a9f4c2a0c8f62
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52842603"
---
# <a name="troubleshoot-azure-ad-b2c-custom-policies-and-identity-experience-framework"></a>Azure AD B2C özel ilkeleri ve kimlik deneyimi çerçevesi sorunlarını giderme

Azure Active Directory B2C kullanıyorsanız (Azure AD B2C) özel ilkeler, ilke dil XML biçiminde kimlik deneyimi çerçevesi ayarlama zorluklar yaşayabilir.  Özel ilkeler yazmak learning yeni bir dil öğrenme gibi olabilir. Bu makalede, araçlar açıklanmaktadır ve hızlı bir şekilde yardımcı olacak ipuçları keşfedin ve sorunları çözün. 

> [!NOTE]
> Bu makalede, Azure AD B2C özel ilke yapılandırmasıyla ilgili sorunları giderme üzerinde odaklanır. Bağlı taraf uygulaması veya kendi kimlik kitaplık ele almaz.

## <a name="xml-editing"></a>XML düzenleme

Özel ilkeleri ayarlama en yaygın hata yanlış olan XML biçimli. İyi bir XML Düzenleyicisi neredeyse gereklidir. İyi bir XML Düzenleyicisi XML yerel görüntüler, içerik renk kodları, yaygın terimlerin doldurur, XML öğeleri dizini tutan ve şema ile doğrulayabilirsiniz. İki bizim sık kullanılan XML Düzenleyicisi şunlardır:

* [Visual Studio Code](https://code.visualstudio.com/)
* [Not Defteri'ni ++](https://notepad-plus-plus.org/)

XML dosyanızı karşıya yüklemeden önce XML şema doğrulama hataları tanımlar. Başlangıç paketi kök klasöründe bulunan XML şema tanımı TrustFrameworkPolicy_0.3.0.0.xsd alın. XML düzenleyicinizi belgelerinde daha fazla bilgi için Aranan *XML araçları* ve *XML doğrulama*.

XML kuralları incelenmesi yararlı bulabilirsiniz. Azure AD B2C, algıladığı hataları biçimlendirme, XML reddeder. Bazen, hatalı biçimlendirilmiş XML yanıltıcı bir hata iletileri neden olabilir.

## <a name="upload-policies-and-policy-validation"></a>İlkeleri ve ilke doğrulaması karşıya yükleyin

 XML dosyasını karşıya yükleme doğrulaması otomatiktir. Çoğu hataya karşıya yükleme başarısız olmasına neden olur. Doğrulama karşıya yüklemekte olduğunuz ilke dosyası içerir. Ayrıca, dosyaları karşıya yükleme dosyası (bağlı olan taraf ilke dosyası, dosya uzantıları ve temel dosya) başvuruyor zincirini içerir. 
 
 Sık karşılaşılan doğrulama hataları arasında şunlar yer alır.

Hata kod parçacığı: `... makes a reference to ClaimType with id "displaName" but neither the policy nor any of its base policies contain such an element`
* ClaimType değeri yanlış veya içinde şema yok.
* ClaimType değerleri dosyaları ilkesinde en az biri tanımlanmalıdır. 
    Örneğin, ` <ClaimType Id="socialIdpUserId">`
* ClaimType uzantıları dosyasında tanımlanır, ancak ayrıca temel dosya TechnicalProfile değerindeki kullanılır, temel dosyanın karşıya hatayla sonuçlanır.

Hata kod parçacığı: `...makes a reference to a ClaimsTransformation with id...`
* Hatanın nedenlerini ClaimType hata aynı olabilir.

Hata kod parçacığı: `Reason: User is currently logged as a user of 'yourtenant.onmicrosoft.com' tenant. In order to manage 'yourtenant.onmicrosoft.com', please login as a user of 'yourtenant.onmicrosoft.com' tenant`
* Tenantıd değerini onay **\<TrustFrameworkPolicy\>** ve **\<BasePolicy\>** öğeleri eşleşen hedef Azure AD B2C kiracınızı.  

## <a name="troubleshoot-the-runtime"></a>Çalışma zamanı sorunlarını giderme

* Kullanım `Run Now` ve `https://jwt.io` ilkelerinizi bağımsız olarak, web veya mobil uygulamanızı test etmek için. Bu Web sitesine bir bağlı taraf uygulaması gibi davranır. Bu içeriği, JSON Web Token (Azure AD B2C ilkeniz tarafından oluşturulan JWT) görüntüler. Kimlik deneyimi Çerçevesi ' bir test uygulaması oluşturmak için aşağıdaki değerleri kullanın:
    * Ad: TestApp
    * Web uygulaması/Web API'si: Hayır
    * Yerel istemci: Hayır

* Azure AD B2C'yi ve istemci tarayıcısı arasında ileti alışverişi izlemek için kullanımı [Fiddler](https://www.telerik.com/fiddler). Bir gösterge kullanıcı yolculuğunuza düzenleme adımlarınızı nerede başarısız olduğunu size yardımcı olabilir.

* İçinde **geliştirme modu**, kullanın **Application Insights** yolculuğunuza kimlik deneyimi çerçevesi kullanıcı etkinliğini izlemek için. İçinde **geliştirme modu**, talepler kimlik deneyimi çerçevesi ve API tabanlı Hizmetleri, kimlik sağlayıcıları gibi teknik profiller tarafından tanımlanan çeşitli talep sağlayıcıları arasında alışverişi görebilirsiniz Azure AD B2C kullanıcı dizini ve diğer hizmetleri Azure çok-faktörlü kimlik doğrulaması ister.  

## <a name="recommended-practices"></a>Önerilen uygulamalar

**Birden çok sürümünü senaryolarınızı tutun. Uygulamanızla bir projedeki gruplandırın.** Temel, uzantıları ve bağlı olan taraf dosyaları birbirine doğrudan bağlıdır. Bir grup olarak kaydedin. İlkelerinizi için yeni özellikler eklendikçe ayrı çalışma sürümleri tutun. Aşama çalışma sürümlerinde kendi dosya sistemi ile etkileşim uygulama kodu.  Uygulamalarınızın birçok farklı bağlı olan taraf ilkeleri bir kiracıdaki çağırabilir. Bunlar, Azure AD B2C ilkelerinizi bekledikleri talepleri bağımlı olabilir.

**Geliştirin ve teknik yolculuklarından bilinen kullanıcı profilleriyle test edin.** Teknik, profilleri ayarlama, test edilmiş başlangıç paketi ilkelerini kullanın. Kendi kullanıcı yolculuklarından birleştirmek önce bunları ayrı ayrı test.

**Geliştirme ve test edilmiş teknik profiline sahip kullanıcı yolculuklarından test edin.** Kullanıcı yolculuğu düzenleme adımlarını artımlı olarak değiştirin. Aşamalı olarak hedeflenen senaryolarınızı oluşturun.

## <a name="next-steps"></a>Sonraki adımlar

* Github'da indirme [active-directory-b2c-custom-policy-starterpack](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/archive/master.zip) .zip dosyası.
