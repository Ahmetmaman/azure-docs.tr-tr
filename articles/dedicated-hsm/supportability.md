---
title: Desteklenebilirlik - Azure ayrılmış HSM | Microsoft Docs
description: Destek seçenekleri ve farklı senaryolarda Azure ayrılmış HSM'ye sorumluluk alanları
services: dedicated-hsm
author: barclayn
manager: mbaldwin
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.custom: seodec18
ms.date: 12/07/2018
ms.author: barclayn
ms.openlocfilehash: 2ed6a79b8736a1d3b472e31ce643c0d1ee085bbb
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53085918"
---
# <a name="azure-dedicated-hsm-supportability"></a>Azure ayrılmış HSM Desteklenebilirliği

Azure ayrılmış HSM hizmetini tek müşteri kullanmak için fiziksel bir cihaz ile tam yönetimsel denetim ve yönetim sorumluluk sağlar. Kullanılabilir cihaz bir [Gemalto SafeNet Luna 7 HSM modeli A790](https://safenet.gemalto.com/data-encryption/hardware-security-modules-hsms/safenet-network-hsm/). Microsoft bir kez fiziksel seri bağlantı noktası ek izleme rolü olarak ötesinde bir müşteri tarafından sağlanan yönetim erişimi olacaktır.  Microsoft, erişimi olmadan, hiçbir devam eden yazılım düzeyi bakım ya da sistem yönetim sorumlulukları olabilir. Sonuç olarak, müşteriler için tipik çalışma etkinliklerinin sorumludur.
Müşteriler, HSM'ler kullanın ve Gemalto ile destek veya Yardım danışmanlık tabanlı iş uygulamaları için tam olarak sorumludur. İşletimsel sağlığı müşteri sahipliğini kapsamını nedeniyle, Microsoft bu hizmeti için yüksek kullanılabilirlik garantisi herhangi bir türden sunmak için mümkün değildir. Bu, müşteri sorumluluk uygulamalarını sağlamak için yüksek kullanılabilirlik elde etmek için doğru şekilde yapılandırıldığından olur. Microsoft, izleyin ve cihaz sistem durumu ve ağ bağlantısı sağlamak.

## <a name="gemalto-support"></a>Gemalto desteği

Ayrılmış HSM hizmetini kullanan müşteriler, bir destek olmalıdır Gemalto ile yerinde sözleşme. Kendi destek sözleşmesi kapsamında, müşterileri, rehberlik ve Destek Hizmetleri doğrudan Gemalto alır. Gemalto destek almak için bir mekanizma aracılığıyla olan kendi [müşteri desteği portalı](https://supportportal.gemalto.com/csm/).
Gemalto HSM (örneğin, istemci erişim yazılım ve SDK'ları) kullanmak için gerekli tüm yazılım bileşenlerini sağlar. Bunlar ayrıca yapılandırma desteği ve tasarım, geliştirme ve SafeNet Luna 7 HSM kullanarak uygulamaların dağıtımını danışmanlık hizmetleri sunar.

### <a name="software-components"></a>Yazılım bileşenleri

HSM cihazlarına yapılandırmada çeşitli yazılım bileşenlerini kullanılır:

* İstemci yazılımı
* SDK
* Araçlar

### <a name="guidance"></a>Rehber

Gemalto yapar aracılığıyla kullanılabilir yönetim ve yapılandırma yönergeleri [müşteri desteği portalı](https://supportportal.gemalto.com/csm/). Geçerli müşteri kimliği kullanarak oturum açmış sonra bu belgeleri indirmek için mevcuttur. Gemalto de bir dizi farklı senaryolar ve yazılım ile müşterilerine yardımcı olmak için tümleştirme kılavuz sağlar. Daha fazla bilgi için [Gemalto iş ortağının sitesini Microsoft](https://safenet.gemalto.com/partners/microsoft/).

### <a name="support"></a>Destek

Herhangi bir yazılım düzeyi sorunu veya ayrılmış HSM hizmetini bir parçası olarak HSM kullanma ile ilgili soru ele Gemalto için doğrudan desteklemez. Yukarıda listelenen tüm yazılım bileşenlerinin yanı sıra, sağlama sonrası olan herhangi bir özel HSM yapılandırma Gemalto tarafından ele alınacaktır. Daha fazla bilgi için [Gemalto müşteri desteği portalı](https://supportportal.gemalto.com/csm/).

### <a name="consulting-services"></a>Danışmanlık hizmetleri

Tüm tasarım, geliştirme ve dağıtımı HSM kullanan özel uygulamalar, Gemalto hesap temsilcinizle iletişime geçin.

## <a name="microsoft-support"></a>Microsoft Desteği

Microsoft, fiziksel HSM sağlamaktan sorumlu cihazları erişilebilir olduğundan ve özel için işletimsel bir durumda tek bir müşteriye kullanın. Müşteriler, yönetim ve cihaz yönetim sorumludur. Microsoft sorumlulukları şunlardır:

* Cihazın güce sahip olduğundan emin ve soğutma yapma
* HSM (örneğin, onarım senaryoları) bir işletimsel durumunu koruma
* Cihaz, ağ üzerinden erişilebilir.

Microsoft bildirilen sorunları aşağıdaki gibi:

* Bileşeni hataları
* Tam cihaz arızası
* Ağ erişim sorunları
* Sağlama ve sağlamayı kaldırma sorunları.

Microsoft, temel sistem durumu telemetri sağlayan bir izleme rolü (diğer bir deyişle, yönetici olmayan rol) aracılığıyla cihaza fiziksel seri bağlantı noktası erişebilir.  Bu, Microsoft bu erişimi kısıtlamak müşterinin seçtiği sürece müşteri için sorunları proaktif bildirim sağlamaya olanak tanır. 

### <a name="provisioning-and-decommissioning"></a>Sağlama ve kullanımdan kaldırma

Ayrılmış HSM hizmeti için onaylanmış bir kaydı bir müşteri sahip olduktan sonra HSM kaynakları (şu anda aracılığıyla PowerShell veya komut satırı arabirimi ve Azure portalını değil) oluşturmak mümkün olacaktır. Kaynak fiziksel bir cihaz belirli bir bölgede bir müşterinin önceden tanımlı sanal ağa (VNet) eşleşen bir ayırma işleminden geçer. Görünür bir VNet üzerinde sonra müşteri cihazınıza erişim ve daha ayrıntılı gereksinimleri uyarınca yapılandırın. Müşteriler, ayrılmış Hsm'lerin Gemalto istemci yazılımları ve araçları kullanarak erişebilirsiniz. Kaynak oluşturma işlemi, Microsoft tarafından desteklenir. Özel yapılandırma işlemek ve ötesine Gemalto tarafından desteklenir. (bkz. Yukarıdaki destek Gemalto). HSM kullanan bir müşteri sona erdiğinde, sıfırlama (zeroized ya gerekir) hiçbir veri sürekliliğini sağlamak için. Cihaz sıfırlama işlemi, tüm özel yapılandırma ve verileri kaldırır. Microsoft, cihaz kaldırır ve havuza pristine durumda döndürür. Cihaz kanıt önceki müşteri etkinliği yoktur havuza geri döndürüldüğünde anlamına gelir. 

### <a name="hardware-issues"></a>Donanım sorunları

HSM cihazını yedekli ve değiştirilebilen güç kaynakları ve fanı birimleri var. Fan birimini kaldırma işlemi, cihaz üzerinde çalıştırıldığında kaldırdıysanız kurcalamaya olay neden olur. Bir bileşeni hatası oluştuğunda, Microsoft, en az kesinti ve müşterilerin hizmeti kullanılabilirlik düşük riskle neden olan bir yolla bileşen düzeyinde sorunu gidermek için en uygun işlemi kullanır.
Cihazın herhangi daha ciddi bir hata ücretsiz havuzundan yeni bir tane değiştirilmekte olan cihazın neden olur. Müşteri yalnızca yeni cihaz eşitleme ve tam çalışır duruma döndürmek için de mevcut HA çifti içerir. Başarısız Aygıt kaldırıldı ve veri merkezinde sitesinde shredded cihazları pul verilerini olacaktır. Kasa için Gemalto geri dönüştürme için döndürülür.


### <a name="networking-issues"></a>Ağ sorunları

Müşteriler için HSM cihazını ağ erişim sorunlarla karşılaşırsanız, Microsoft Destek'e başvurmanız gerekir. Ağ erişim için basit bir test HSM cihaza bağlanmak için SSH kullanmaktır. Bu başarısız olursa, Microsoft desteğine başvurun.

## <a name="service-level-expectations-for-support"></a>Hizmet düzeyi beklentileri desteği

Microsoft destek hizmet düzeyleri için başvurmak [Azure destek planı](https://azure.microsoft.com/support/plans/).
Gemalto destek hizmet düzeyleri için başvurmak [Gemalto destek Essentials](https://azure.microsoft.com/support/plans/).

## <a name="next-steps"></a>Sonraki adımlar

Yüksek kullanılabilirlik ve güvenlik gibi temel kavramların cihaz sağlama ve uygulama tasarım veya dağıtımınızın önce iyi anlaşıldığından önerilir.

* [Dağıtım mimarisi](deployment-architecture.md)
* [Yüksek kullanılabilirlik](high-availability.md)
* [Fiziksel güvenlik](physical-security.md)
* [Ağ](networking.md)
* [İzleme](monitoring.md)