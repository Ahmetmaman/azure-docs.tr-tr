---
title: Desteklenen sürümler-MySQL için Azure veritabanı
description: MySQL Server 'ın hangi sürümlerini MySQL için Azure veritabanı hizmetinde destekleyeceğinizi öğrenin.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 6/3/2020
ms.openlocfilehash: 0c4836ad48426bc5f2c8c18b888f0c296a90f222
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2020
ms.locfileid: "93417699"
---
# <a name="supported-azure-database-for-mysql-server-versions"></a>MySQL sunucu sürümleri için desteklenen Azure veritabanı

MySQL için Azure veritabanı, InnoDB altyapısı kullanılarak [MySQL Community Edition](https://www.mysql.com/products/community/)'dan geliştirilmiştir.

MySQL, X. Y. Z adlandırma şemasını kullanır. X ana sürümdür, Y ise küçük sürümdür ve Z hata çözme sürümüdür. Düzen hakkında daha fazla bilgi için [MySQL belgelerine](https://dev.mysql.com/doc/refman/5.7/en/which-version.html)bakın.


> [!NOTE]
> Hizmette, bağlantıları sunucu örneklerine yeniden yönlendirmek için bir ağ geçidi kullanılır. Bağlantı kurulduktan sonra, MySQL istemcisi MySQL sunucu örneğinizde çalışan gerçek sürümü değil ağ geçidinde belirlenen MySQL sürümünü görüntüler. MySQL sunucu örneğinizin sürümünü öğrenmek için MySQL komut isteminde `SELECT VERSION();` komutunu kullanın.

MySQL için Azure veritabanı şu anda aşağıdaki sürümleri desteklemektedir:

## <a name="mysql-version-56"></a>MySQL sürüm 5,6

Hata çözme sürümü: 5.6.47

Bu sürümdeki geliştirmeler ve düzeltmeler hakkında daha fazla bilgi edinmek için MySQL [sürüm notlarına](https://dev.mysql.com/doc/relnotes/mysql/5.6/en/news-5-6-47.html) bakın.

## <a name="mysql-version-57"></a>MySQL sürüm 5,7

Hata çözme sürümü: 5.7.29

Bu sürümdeki geliştirmeler ve düzeltmeler hakkında daha fazla bilgi edinmek için MySQL [sürüm notlarına](https://dev.mysql.com/doc/relnotes/mysql/5.7/en/news-5-7-29.html) bakın.

## <a name="mysql-version-80"></a>MySQL sürüm 8,0

Hata çözme sürümü: 8.0.15

Bu sürümdeki geliştirmeler ve düzeltmeler hakkında daha fazla bilgi edinmek için MySQL [sürüm notlarına](https://dev.mysql.com/doc/relnotes/mysql/8.0/en/news-8-0-15.html) bakın.

## <a name="managing-updates-and-upgrades"></a>Güncelleştirmeleri ve yükseltmeleri yönetme
Hizmet, hata düzeltme sürümü güncelleştirmeleri için düzeltme eki uygulamayı otomatik olarak yönetir. Örneğin, 5.7.20 yükle to 5.7.21.  

Şu anda alt ve ana sürüm yükseltmeleri desteklenmez. Örneğin MySQL 5.6'dan MySQL 5.7'ye yükseltme desteklenmez. 5.6'dan 5.7'ye yükseltmek isterseniz [döküm alın ve bunu yeni altyapı sürümüyle oluşturulmuş bir sunucuya geri yükleyin](./concepts-migrate-dump-restore.md).

## <a name="next-steps"></a>Sonraki adımlar

- MySQL için Azure veritabanı sürüm oluşturma ilkesi hakkında daha fazla bilgi için [Bu belgeye](concepts-version-policy.md)bakın.
- **Hizmet katmanınıza** dayalı belirli kaynak kotaları ve sınırlamalar hakkında bilgi için bkz. [hizmet katmanları](./concepts-pricing-tiers.md)
