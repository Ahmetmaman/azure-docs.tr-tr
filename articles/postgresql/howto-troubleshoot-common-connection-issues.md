---
title: PostgreSQL için Azure veritabanı bağlantı sorunlarını giderme | Microsoft Docs
description: PostgreSQL için Azure veritabanı bağlantı sorunlarını giderme hakkında bilgi edinin.
keywords: postgresql bağlantı, bağlantı dizesi, bağlantı sorunları, geçici bir hata oluştu, bağlantı hatası
services: postgresql
author: jan-eng
ms.author: janeng
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 11/09/2018
ms.openlocfilehash: 67383db4bd1d57d194e10de2dc1964532b3619a4
ms.sourcegitcommit: efcd039e5e3de3149c9de7296c57566e0f88b106
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2018
ms.locfileid: "53160820"
---
# <a name="troubleshoot-connection-issues-to-azure-database-for-postgresql"></a>PostgreSQL için Azure veritabanı bağlantı sorunlarını giderme

Çeşitli gibi çeşitli işlemler tarafından bağlantı sorunlara neden olabilir:

* Güvenlik duvarı ayarları
* Bağlantı zaman aşımı
* Hatalı oturum açma bilgileri
* Bazı kaynaklar PostgreSQL için Azure veritabanı üzerinde üst sınırına ulaştınız
* Hizmet, altyapı ile ilgili sorunlar
* Hizmet üzerinde gerçekleştirilen bakım
* İşlem ayırma sunucusunun sanal çekirdekler veya farklı hizmet katmanına taşıyarak sayısında ölçeklendirme tarafından değiştirildi

Genellikle, PostgreSQL için Azure veritabanı bağlantısı sorunları şu şekilde sınıflandırılabilir:

* Geçici hatalar (kısa süreli veya aralıklı)
* Kalıcı veya geçici olmayan hatalar (düzenli olarak yinele hatalar)

## <a name="troubleshoot-transient-errors"></a>Geçici hataları giderme

Bakım, sistem donanım veya yazılım bir hatayla karşılaştığında veya sanal çekirdekler veya hizmet katmanı sunucunuzun geçici hatalar oluşur. Hizmet PostgreSQL için Azure veritabanı, yerleşik yüksek kullanılabilirliğe sahip ve otomatik olarak bu tür sorunları azaltmak için tasarlanmıştır. Ancak, uygulamanızın en fazla süresi genellikle 60 saniyeden kısa bir süre için sunucu bağlantısını kaybeder. Bazı olaylar bazen azaltmak, büyük bir işlem bir uzun süre çalışan kurtarma sebep olduğunda gibi daha uzun sürebilir.

### <a name="steps-to-resolve-transient-connectivity-issues"></a>Geçici bağlantı sorunlarını giderme adımları

1. Denetleme [Microsoft Azure hizmet Panosu](https://azure.microsoft.com/status) , hataları rapor edilen uygulama tarafından süre boyunca gerçekleşen bilinen kesintiler için.
2. PostgreSQL için Azure veritabanı geçici hatalar beklediğiniz ve uygulama gibi bir bulut hizmetine bağlanan uygulamaları, uygulama hataları olarak kullanıcılara kavrayış yerine bu hataları işlemek için mantığı yeniden deneyin. Gözden geçirme [geçici bağlantı hatalarının için PostgreSQL için Azure veritabanı işleme](concepts-connectivity.md) en iyi yöntemler ve geçici hataları işlemek için tasarım yönergeleri.
3. Bir sunucu, kaynak sınırları yaklaştığında, hataları görünüyor geçici bir bağlantı sorunu. Bkz: [PostgreSQL için Azure veritabanı'nda sınırlamaları](concepts-limits.md).
4. Bağlantı sorunları devam ya da uygulamanızın karşılaştığı hata süresi 60 saniye aşarsa veya belirli bir günde birden çok defa geçmelerine hata görürseniz Azure destek isteği seçerek dosya **alma desteği**üzerinde [Azure Destek](https://azure.microsoft.com/support/options) site.

## <a name="troubleshoot-persistent-errors"></a>Kalıcı hatalarını giderme

PostgreSQL için Azure veritabanı'na bağlanmak uygulamayı kalıcı olarak başarısız olursa, genellikle aşağıdakilerden biri ile ilgili bir sorun gösterir:

* Sunucu güvenlik duvarı yapılandırması: PostgreSQL sunucusu güvenlik duvarı için Azure veritabanı, proxy sunucuları ve ağ geçitleri dahil istemcinizden bağlantılarına izin verecek şekilde yapılandırıldığından emin olun.
* İstemci güvenlik duvarı yapılandırması: İstemci üzerindeki güvenlik duvarı, veritabanı sunucunuza bağlantılar izin vermeniz gerekir. IP adresleri ve bağlantı noktaları için olamaz sunucunun PostgreSQL gibi bazı güvenlik duvarları uygulama adları hem de izin verilmelidir.
* Kullanıcı hatası: Bağlantı dizesi veya eksik bir sunucu adı gibi bağlantı parametrelerini yazdığınızı *@servername* kullanıcı adı soneki.

### <a name="steps-to-resolve-persistent-connectivity-issues"></a>Kalıcı bağlantı sorunlarını giderme adımları

1. Ayarlanan [güvenlik duvarı kuralları](howto-manage-firewall-using-portal.md) istemci IP adreslerine izin verecek şekilde. Geçici yalnızca test amaçlıdır için başlangıç IP adresi ve bitiş IP adresi 255.255.255.255 kullanarak 0.0.0.0 kullanarak güvenlik duvarı kuralı ayarlama. Bu, tüm IP adreslerinin sunucuya açar. Bu, bağlantı sorunu giderip, bu kuralı kaldırmak ve uygun şekilde sınırlı IP adresi veya adres aralığı için bir güvenlik duvarı kuralı oluşturun.
2. İstemci internet arasındaki tüm güvenlik duvarlarının üzerinde 3306 numaralı bağlantı noktasını giden bağlantılar için açık olduğundan emin olun.
3. Bağlantı dizenizi ve diğer bağlantı ayarlarını doğrulayın.
4. Hizmet durumu Panosu denetleyin. Bölgesel bir kesinti olduğunu düşünüyorsanız, bkz. [PostgreSQL için Azure veritabanı ile iş sürekliliğine genel bakış](concepts-business-continuity.md) için yeni bir bölgeye kurtarmak için adımlar.

## <a name="next-steps"></a>Sonraki adımlar

* [Geçici bağlantı hatalarının için PostgreSQL için Azure veritabanı işleme](concepts-connectivity.md)
