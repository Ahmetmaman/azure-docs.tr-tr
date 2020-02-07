---
title: Dağıtım mimarisi - Azure ayrılmış HSM | Microsoft Docs
description: Azure ayrılmış HSM uygulama mimarisinin bir parçası olarak kullanırken, temel tasarım konuları
services: dedicated-hsm
author: msmbaldwin
manager: rkarlin
ms.custom: mvc, seodec18
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/05/2020
ms.author: mbaldwin
ms.openlocfilehash: 89e3bf95a6b048e5e97cfb151ef9302b70eac1c9
ms.sourcegitcommit: 57669c5ae1abdb6bac3b1e816ea822e3dbf5b3e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/06/2020
ms.locfileid: "77048561"
---
# <a name="azure-dedicated-hsm-deployment-architecture"></a>Azure Ayrılmış HSM dağıtım mimarisi

Azure ayrılmış HSM azure'da şifreleme anahtar depolama alanı sağlar. Bu, katı güvenlik gereksinimleri karşılar. Müşteriler, Azure ayrılmış HSM kullanarak yararlanacaktır bunlar:

* FIPS 140-2 Düzey 3 sertifika karşılamalıdır
* HSM özel erişime sahip olmasını gerektirir
* tüm cihazları tam denetime sahip olmalıdır

HSM'ler, Microsoft veri merkezleri arasında dağıtılan ve yüksek oranda kullanılabilir bir çözümün temel olarak bir cihaz çifti olarak kolayca sağlanabilir. Bir olağanüstü durum dayanıklı çözümü için bölgede de dağıtılabilir. Şu anda kullanılabilir, ayrılmış HSM ile bölgeleri şunlardır:

* Doğu ABD
* Doğu ABD 2
* Batı ABD
* Orta Güney ABD
* Güneydoğu Asya
* Doğu Asya
* Hindistan Orta
* Hindistan Güney
* Japonya Doğu
* Japonya Batı
* Kuzey Avrupa
* Batı Avrupa
* UK Güney
* UK Batı
* Kanada Orta
* Kanada Doğu
* Avustralya Doğu
* Avustralya Güneydoğu

Bu bölgeler her iki bağımsız veri merkezine veya en az iki bağımsız kullanılabilirlik alanları dağıtılmış HSM raflar sahiptir. Güney Doğu Asya üç kullanılabilirlik ve Doğu ABD 2 iki sahiptir. Avrupa, Asya ve ayrılmış HSM hizmeti sunan ABD toplam sekiz bölgede yoktur. Azure bölgeleri hakkında daha fazla bilgi için bkz. resmi [Azure bölgeleri bilgileri](https://azure.microsoft.com/global-infrastructure/regions/).
Ayrılmış HSM tabanlı herhangi bir çözüm için bazı tasarım etkenleri konum/gecikme, yüksek kullanılabilirliği olan ve diğer dağıtılmış uygulamalar için destek.

## <a name="device-location"></a>Cihaz konumu

En iyi HSM cihaz şifreleme işlemleri gerçekleştiren uygulamalar için en yakın olmasa konumdur. Bölge gecikme süresi, tek basamaklı milisaniye olması beklenir. Bölgeler arası gecikme bundan 5 ila 10 kat daha yüksek olabilir.

## <a name="high-availability"></a>Yüksek kullanılabilirlik

Yüksek kullanılabilirlik elde etmek için bir müşteri yüksek kullanılabilirlik çiftinde Gemalto yazılımını kullanarak yapılandırılan iki HSM cihazlarına bir bölgede kullanmanız gerekir. Bu dağıtım türü, tek bir cihaz anahtar işlemleri işlemesini önleyen bir sorunla karşılaşırsa anahtarları kullanılabilirliğini sağlar. Ayrıca önemli ölçüde risk güç kaynağı değiştirme gibi arıza/tamir bakımın gerçekleştirildiği sırada azaltır. Bölgesel düzeyinde hata herhangi bir türden için hesap bir tasarım önemlidir. Deprem kasırgalar veya sel gibi doğal afetler olduğunda bölgesel düzeyindeki hatalar oluşabilir. HSM cihazlarına başka bir bölgede sağlayarak bu tür olaylar azaltılması. Başka bir bölgede dağıtılan cihazları birlikte Gemalto yazılım yapılandırma ile eşleştirilmiş. En az bir dağıtım için yüksek oranda kullanılabilir ve olağanüstü durum yani dayanıklı çözümüdür dört HSM cihazlar arasında iki bölgeleri. Yerel yedeklilik ve bölgeler arası yedeklilik HSM cihaz dağıtımları gecikme süresi, kapasite desteklemek için veya diğer uygulamaya özgü gereksinimlerini karşılamak üzere daha eklemek için temel olarak kullanılabilir.

## <a name="distributed-application-support"></a>Dağıtılmış uygulama desteği

Ayrılmış HSM cihazlar genellikle desteklemek üzere anahtar deposu ve anahtar alma işlemleri gerçekleştirmek için gereken uygulamalar olarak dağıtılır. Ayrılmış HSM cihazlarına bağımsız uygulama desteği için 10 bölüme sahip. Cihaz konumu hizmetini kullanmak için gereken tüm uygulamalar bütüncül bir görünümü temel almalıdır.

## <a name="next-steps"></a>Sonraki adımlar

Dağıtım mimarisi belirlendikten sonra o mimaride uygulamak için çoğu yapılandırma etkinlikleri Gemalto tarafından sağlanır. Bu cihaz yapılandırması ve bunun yanı sıra uygulama içerir tümleştirme senaryoları. Daha fazla bilgi için, [Gemalto müşteri destek](https://supportportal.gemalto.com/csm/) portalını kullanın ve yönetim ve yapılandırma kılavuzlarını indirin. Microsoft iş ortağı site tümleştirme kılavuzlarını çeşitli sahiptir.
Yüksek kullanılabilirlik ve güvenlik gibi hizmete tüm temel kavramlarını gibi iyi cihaz sağlama veya uygulama tasarım ve dağıtım önce anlaşıldığından önerilir.
Daha fazla kavramı düzey konular:

* [Yüksek Kullanılabilirlik](high-availability.md)
* [Fiziksel güvenlik](physical-security.md)
* [Ağ](networking.md)
* [Desteklenebilirlik](supportability.md)
* [İzleme](monitoring.md)
