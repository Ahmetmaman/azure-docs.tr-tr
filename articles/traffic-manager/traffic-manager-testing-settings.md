---
title: Azure Traffic Manager ayarlarını doğrulama | Microsoft Docs
description: Bu makalede, Traffic Manager ayarlarınızı doğrulamanıza yardımcı olur
services: traffic-manager
documentationcenter: ''
author: kumudd
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/16/2017
ms.author: kumud
ms.openlocfilehash: ce6467f2cdb47dd7cc61897831019a34e174478f
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2018
ms.locfileid: "50140460"
---
# <a name="verify-traffic-manager-settings"></a>Traffic Manager ayarlarını doğrulama

Traffic Manager ayarlarınızı test etme için birden çok istemciyi çeşitli yerlerde, testlerinizi çalıştırmak gerekir. Ardından, uç noktaları Traffic Manager profilinizin teker teker aşağı getirin.

* Hızlı bir şekilde (örneğin, 30 saniye) değişiklikleri yaymak için DNS TTL değeri düşük ayarlayın.
* Azure bulut hizmetlerinizi ve Web siteleri, sınadığınız profilindeki IP adreslerini bildirin.
* Bir DNS adı bir IP adresine çözümlemek ve bu adresi görüntüler olanak tanıyan Araçlar kullanın.

DNS adları profilinizde bitiş IP adreslerini çözümlemeye olduğunu görmek için iade ederler. Adlar Traffic Manager profilinde tanımlanan bir trafik yönlendirme yöntemini ile tutarlı şekilde çözümlenmesi gerekir. Araçları gibi kullanabileceğiniz **nslookup** veya **dig** DNS adlarını çözümlemek için.

Aşağıdaki örnekler Traffic Manager profilinizin test etmenize yardımcı olur.

### <a name="check-traffic-manager-profile-using-nslookup-and-ipconfig-in-windows"></a>Traffic Manager profili içinde Windows nslookup ve ipconfig kullanarak denetleme

1. Bir komut veya Windows PowerShell istemini yönetici olarak açın.
2. Tür `ipconfig /flushdns` DNS çözümleyicisi önbelleğini temizlemek için.
3. `nslookup <your Traffic Manager domain name>` yazın. Örneğin, aşağıdaki komutu bir etki alanı adı ön eki denetler *myapp.contoso*

        nslookup myapp.contoso.trafficmanager.net

    Tipik bir sonuç, aşağıdaki bilgileri gösterir:

    + Bu Traffic Manager etki alanı adını çözümlemek için erişilen DNS sunucusunun IP adresi ve DNS adı.
    + Traffic Manager etki alanı adını Traffic Manager etki alanına çözümler yapılacağı IP adresi "nslookup" sonra komut satırında yazdınız. İkinci IP adresini kontrol etmek için önemli bir sertifikadır. Bu, bulut Hizmetleri veya Web sitesi test ettiğiniz Traffic Manager profili biri için bir genel sanal IP (VIP) adresi eşleşmelidir.

## <a name="how-to-test-the-failover-traffic-routing-method"></a>Yük devretme trafik yönlendirme yöntemini test etme

1. Tüm uç noktalar ayarlama bırakın.
2. Tek bir istemci kullanarak DNS çözümlemesi nslookup veya benzer bir yardımcı programını kullanarak, şirketinizin etki alanı adı isteği.
3. Çözümlenen IP adresini birincil uç noktaya eşleştiğinden emin olun.
4. Birincil uç noktanızı taşıyın veya Traffic Manager, uygulama kapalı olduğunu düşündüğü izleme dosyasını kaldırın.
5. DNS zaman yaşam için (TTL) Traffic Manager profili ve iki dakika bekleyin. Örneğin, DNS TTL 300 saniyedir (5 dakika) ise, yedi dakika beklemeniz gerekir.
6. Nslookup ile DNS istemci önbelleği ve istek DNS çözümlemenizin temizler. Windows içinde ipconfig/flushdns komutu, DNS önbelleğini boşaltabilirsiniz.
7. Çözümlenen IP adresini ikincil uç noktanız eşleştiğinden emin olun.
8. Her uç noktası sırayla getirme işlemi yineleyin. DNS listesinde sonraki uç noktasının IP adresini döndürdüğünden emin olun. Tüm uç noktalar kapalı olduğunda, birincil uç noktasının IP adresini yeniden edinmeleri gerekir.

## <a name="how-to-test-the-weighted-traffic-routing-method"></a>Ağırlıklı trafik yönlendirme yöntemini test etme

1. Tüm uç noktalar ayarlama bırakın.
2. Tek bir istemci kullanarak DNS çözümlemesi nslookup veya benzer bir yardımcı programını kullanarak, şirketinizin etki alanı adı isteği.
3. Çözümlenen IP adresini, uç noktalardan biri eşleştiğinden emin olun.
4. DNS istemci önbelleğinizi temizlemek ve her uç nokta için 2 ve 3. adımları tekrarlayın. Farklı IP adreslerini her uç noktalarınız için döndürülen görmeniz gerekir.

## <a name="how-to-test-the-performance-traffic-routing-method"></a>Performans trafiği yönlendirme yöntemini test etme

Performans trafiği yönlendirme metodunu etkili bir şekilde test etmek için dünyanın farklı bölümlerinde yer alan istemciler olması gerekir. İstemciler, hizmetlerinizi test etmek için kullanılan farklı Azure bölgelerinde oluşturabilirsiniz. Global bir ağda varsa, uzaktan dünyanın diğer bölgelerindeki istemciler için oturum açın ve buradan testlerinizi çalıştırın.

Alternatif olarak, ücretsiz web tabanlı DNS araması ve vardır dig Hizmetleri kullanılabilir. Bu araçlardan bazıları, dünyanın her yerindeki çeşitli konumlardan DNS ad çözümlemesi denetleme olanağı sağlar. Örnekler üzerinde "DNS araması" araması yapın. Üçüncü taraf hizmetleri gibi Gomez veya açılış profillerinizi trafiği beklendiği gibi dağıtıyorsanız onaylamak için kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

* [Traffic Manager trafik yönlendirme yöntemleri hakkında](traffic-manager-routing-methods.md)
* [Traffic Manager için performans konuları](traffic-manager-performance-considerations.md)
* [Düzeyi düşürülmüş Traffic Manager durumu için sorun giderme](traffic-manager-troubleshooting-degraded.md)
