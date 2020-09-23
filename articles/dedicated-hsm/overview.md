---
title: Ayrılmış HSM nedir? -Azure ayrılmış HSM | Microsoft Docs
description: Azure adanmış HSM 'nin Azure 'da şifreleme anahtar depolaması sağlayan bir Azure hizmeti olduğunu öğrenin.
services: dedicated-hsm
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.custom: mvc, seodec18
ms.date: 12/07/2018
ms.author: mbaldwin
ms.openlocfilehash: 6eb82400b2f598b4b54da4b238f4bce71d83de6b
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90970205"
---
# <a name="what-is-azure-dedicated-hsm"></a>Azure Ayrılmış HSM nedir?

Azure adanmış HSM, Azure 'da şifreleme anahtar depolaması sağlayan bir Azure hizmetidir. Adanmış HSM en katı güvenlik gereksinimlerini karşılar. FIPS 140-2 düzey 3 tarafından doğrulanan cihazlar ve HSM gerecinin tam ve dışlamalı denetimini gerektiren müşteriler için ideal bir çözümdür. 

 HSM cihazları, çeşitli Azure bölgelerinde küresel olarak dağıtılır. Bunlar, cihaz çifti olarak kolayca sağlanabilir ve yüksek kullanılabilirlik için yapılandırılabilir. Ayrıca, bölgesel düzeyde yük devretmeye karşı güvence altına almak için bölgeler arasında HSM cihazları sağlanabilir. Microsoft, Gemalto 'dan [SafeNet Luna ağ HSM 7 (model A790)](https://safenet.gemalto.com/data-encryption/hardware-security-modules-hsms/safenet-network-hsm/) gereci kullanarak adanmış HSM hizmetini sunar. Bu cihaz en yüksek performans ve şifreleme tümleştirme seçeneklerini sunar. 

Bunlar sağlandıktan sonra, HSM cihazları doğrudan bir müşterinin sanal ağına bağlanır. Ayrıca, Noktadan siteye veya siteden siteye VPN bağlantısı yapılandırdığınızda şirket içi uygulama ve yönetim araçları tarafından da erişilebilir. Müşteriler, HSM cihazlarını Gemalto 'ın destek portalından yapılandırmak ve yönetmek için yazılım ve belgeleri alırlar.

## <a name="why-use-azure-dedicated-hsm"></a>Azure ayrılmış HSM neden kullanılmalıdır?

### <a name="fips-140-2-level-3-compliance"></a>FIPS 140-2 düzey-3 uyumluluğu

Birçok kuruluşun, şifreleme anahtarı depolamanın [fıps 140-2 düzey 3](https://csrc.nist.gov/publications/detail/fips/140/2/final) gereksinimlerini karşıladığını belirten katı sektör düzenlemeleri vardır. Microsoft 'un çok kiracılı Azure Key Vault hizmeti şu anda yalnızca FIPS 140-2 düzey 2 sertifikası sağlıyor. Azure adanmış HSM, finansal hizmetler sektörünün, devlet kurumlarının ve FIPS 140-2 düzey 3 gereksinimlerini karşılaması gereken diğer kişilere gerçek bir ihtiyacı karşılar.

### <a name="single-tenant-devices"></a>Tek kiracılı cihazlar

Birçok müşterinizin, şifreleme depolama cihazının tek kiracınıza yönelik bir gereksinimi vardır. Azure ayrılmış HSM hizmeti, Microsoft 'un genel olarak dağıtılmış veri merkezlerinden birinden fiziksel bir cihaz sağlamasını sağlar. Müşteriye sağlandıktan sonra, yalnızca söz konusu müşteri cihaza erişebilir.

### <a name="full-administrative-control"></a>Tam yönetim denetimi

Birçok müşteri, yönetim amacıyla cihazlarına tam yönetim denetimi ve tek tek erişim gerektirir. Bir cihaz sağlandıktan sonra, yalnızca müşterinin cihaza yönetici veya uygulama düzeyinde erişimi vardır.

 Müşterinin cihaza ilk kez eriştiği bir yönetim denetimi yoktur, bu noktada müşterinin parolayı değiştirdiği anlamına gelir. Bu noktadan itibaren müşteri, tam yönetim denetimi ve uygulama yönetimi özelliği olan gerçek bir tek kiracıya sahiptir. Microsoft, seri bağlantı noktası bağlantısı aracılığıyla telemetri için izleyici düzeyinde erişimi (yönetici rolü değil) korur. Bu erişim sıcaklık, güç kaynağı ve fan sistem durumu gibi donanım izleyicilerini içerir. 
 
 Müşteri, gerekli olan bu izlemeyi devre dışı bırakmak için ücretsizdir. Ancak devre dışı bırakırsanız, Microsoft 'tan öngörülü sistem durumu uyarıları almaz.

### <a name="high-performance"></a>Yüksek performans

Bu hizmet için farklı nedenlerle Gemalto cihazı seçildi. Geniş kapsamlı bir şifreleme algoritması desteği, desteklenen çeşitli işletim sistemleri ve geniş API desteği sunar. Dağıtılan özel model, RSA-2048 için saniyede 10.000 işlem ile mükemmel performans sunar. Benzersiz uygulama örnekleri için kullanılabilen 10 bölümü destekler. Bu cihaz düşük gecikme süreli, yüksek kapasiteli ve yüksek performanslı bir cihazdır.

### <a name="unique-cloud-based-offering"></a>Bulut tabanlı benzersiz teklif

Microsoft, benzersiz bir müşteri kümesine yönelik belirli bir ihtiyacı fark. Bu, FIPS 140-2 düzey 3 tarafından doğrulanan özel bir HSM hizmeti sunan ve bulut tabanlı ve şirket içi uygulama tümleştirmesinin bir kapsamını sunan, yeni müşteriler sunan tek bulut sağlayıcıdır.

## <a name="is-azure-dedicated-hsm-right-for-you"></a>Azure ayrılmış HSM sizin için uygun mu?

Azure ayrılmış HSM, belirli bir büyük ölçekli kuruluş türü için benzersiz gereksinimlere yönelik özel bir hizmettir. Sonuç olarak, Azure müşterilerinin toplu 'un bu hizmet için kullanım profiline uymayacak olması beklenmektedir. Birçok, daha uygun ve uygun maliyetli bir Azure Key Vault hizmeti bulur. Gereksinimlerinize uygun olup olmadığına karar vermenize yardımcı olması için aşağıdaki ölçütleri belirledik.

### <a name="best-fit"></a>En iyi sığdırma

Azure adanmış HSM, en çok HSM cihazlarına doğrudan ve tek erişimli erişimi gerektiren "yükseltme ve-kaydırma" senaryoları için uygundur. Örnekler şunları içerir:

- Şirket içinden Azure sanal makinelerine uygulama geçirme
- AWS bulut HSM klasik hizmetini kullanan sanal makinelere (Amazon, bu hizmeti yeni müşterilere sunmadığından), Amazon AWS EC2 'tan uygulama geçirme
- Apache/Ngnx SSL boşaltması, Oracle TDE ve Azure sanal makinelerinde ADC gibi, Sarmalanan bir sarmalanmış yazılım çalıştırma 

### <a name="not-a-fit"></a>Uygun değil

Azure ayrılmış HSM aşağıdaki tür senaryoya uygun değildir: Azure adanmış HSM ile tümleştirilen, müşteri tarafından yönetilen anahtarlarla (Azure Information Protection, Azure disk şifrelemesi, Azure Data Lake Store, Azure depolama, Azure SQL veritabanı ve Office 365 için müşteri anahtarı) şifrelemeyi destekleyen Microsoft bulut hizmetleri.

### <a name="it-depends"></a>Duruma göre değişir

Azure ayrılmış HSM 'nin sizin için çalışıp çalışmadığına göre, gereksinimlerinize ve yapamayacaksınız. FIPS 140-2 düzey 3 gereksinimi bir örnektir. Bu gereksinim ortaktır ve adanmış HSM Şu anda bu uygulamayı toplantı için tek seçenektir. Bu uygulanan gereksinimleri ilgili değilse, genellikle Azure Key Vault ve adanmış HSM arasında bir seçimdir. Karar vermeden önce gereksinimlerinizi değerlendirin.

Seçenekleriniz için gereken durumlar şunlardır: 

- Müşterinin Azure sanal makinesinde çalışan yeni kod
- Azure sanal makinesinde TDE SQL Server
- Azure depolama istemci tarafı şifrelemesi
- SQL Server ve Azure SQL DB Always Encrypted

## <a name="next-steps"></a>Sonraki adımlar

Bu, son derece özelleştirilmiş bir hizmettir. Bu nedenle, fiyatlandırma, destek ve hizmet düzeyi sözleşmeleri dahil olmak üzere bu belge kümesindeki temel kavramları tam olarak anlamanız önerilir. 

[Gemalto tümleştirme kılavuzlarından](https://safenet.gemalto.com/partners/microsoft/) , HSM 'lerin mevcut bir sanal ağ ortamına sağlanması kolaylaştırılmasına yardımcı olur. Ayrıca, dağıtım mimarinizi nasıl ayarlayabileceğiniz konusunda yardım almak için nasıl yapılır kılavuzlarından de yer vardır.

* [Yüksek kullanılabilirlik](high-availability.md)
* [Fiziksel güvenlik](physical-security.md)
* [Ağ](networking.md)
* [Desteklenebilirlik](supportability.md)
* [İzleme](monitoring.md)
