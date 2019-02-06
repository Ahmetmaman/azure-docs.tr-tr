---
title: Azure SQL veritabanı tek veritabanı nedir | Microsoft Docs
description: Azure SQL veritabanı tek veritabanı hakkında bilgi edinin
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 02/04/2019
ms.openlocfilehash: a2500988b174e49870f4da7087b3fa4c81f3c77a
ms.sourcegitcommit: 039263ff6271f318b471c4bf3dbc4b72659658ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/06/2019
ms.locfileid: "55754991"
---
# <a name="what-is-a-single-database-in-azure-sql-database"></a>Azure SQL veritabanı'nda tek bir veritabanı nedir

Tek veritabanı dağıtım seçeneği tek başına veritabanı kendi kaynak kümesi ile Azure SQL veritabanı'nda oluşturur ve SQL veritabanı sunucusu yönetilir. Tek bir veritabanı ile her veritabanı birbirine ve taşınabilir, her biri kendi hizmet katmanı içinde yalıtılmış [DTU tabanlı satın alma modeli](sql-database-service-tiers-dtu.md) veya [sanal çekirdek tabanlı satın alma modeli](sql-database-service-tiers-vcore.md) ve bir kesin boyutu işlem.

> [!IMPORTANT]
> Tek veritabanı, Azure SQL veritabanı için üç dağıtım seçenekleri biridir. Diğer iki olan [elastik havuzlar](sql-database-elastic-pool.md) ve [yönetilen örnek](sql-database-managed-instance.md).
> [!NOTE]
> Bir Azure SQL veritabanı'nda terimler sözlüğü için bkz: [SQL veritabanı terimler sözlüğü](sql-database-glossary-terms.md)

## <a name="dynamic-scalabilty"></a>Dinamik ölçeklenebilirliği

Düşük bir maliyetle düşük fiyat/performansından hizmet katmanda aylık küçük, tek bir veritabanı üzerinde ilk uygulamanızı oluşturabilir ve ardından [Hizmet katmanını değiştirebilirsiniz](sql-database-single-database-scale.md) el ile veya programlama yoluyla thehigher fiyat/performansından hizmeti dilediğiniz zaman çözümünüzün gereksinimlerini karşılayacak şekilde katmanı. Performansı uygulamanız veya müşterileriniz kesinti yaşamadan ayarlayabilirsiniz. Dinamik ölçeklenebilirlik, veritabanınızın hızla değişen kaynak gereksinimlerine hızlı şekilde yanıt vermesini ve yalnızca ihtiyaç duyduğunuz kaynaklara ve ihtiyaç duyduğunuz süre boyunca ödeme yapmanızı sağlar.

## <a name="single-databases-and-elastic-pools"></a>Tek veritabanları ve elastik havuzlar

Tek bir veritabanı içinde veya dışında taşınabilir bir [elastik havuz](sql-database-elastic-pool.md) kaynak paylaşımı. Tek veritabanı oluşturabilmek ve veritabanı performansını isteğe göre yükseltip düşürebilmek, özellikle kullanım biçimlerinin nispeten tahmin edilebilir olduğu durumlarda birçok işletme ve uygulama için yeterlidir. Ancak tahmin edilemeyen kullanım biçimlerine sahipseniz bu durum maliyetlerin ve iş modelinizin yönetimini zorlaştırabilir. Elastik havuzlar, bu sorunu çözmek için tasarlanmıştır. Esnek havuzların işleyiş mantığı gayet basittir. Performans kaynaklarını tek bir veritabanı yerine bir havuz ayırmak ve tek veritabanı performansı yerine havuzun toplu performansı kaynakları için ödeme yaparsınız.

## <a name="monitoring-and-alerting"></a>İzleme ve uyarı

Yerleşik kullandığınız [performansı izleme](sql-database-performance.md) ve [uyarı araçlarının yanı sıra](sql-database-insights-alerts-portal.md), performans değerlendirmeleriyle birlikte birleştirilmiş. Bu araçları kullanarak geçerli veya projeye özgü performans ihtiyaçlarınıza göre ölçek büyütme veya küçültme işlemlerinin etkisini hızlı bir şekilde değerlendirebilirsiniz. SQL Veritabanı ayrıca izlemeyi kolaylaştırmak için [ölçümler ve tanılama günlükleri oluşturabilir](sql-database-metrics-diag-logging.md).

## <a name="availability-capabilities"></a>Kullanılabilirlik özellikleri

Tek veritabanları, elastik havuzlar ve yönetilen örnekleri birçok kullanılabilirlik characterics sağlar. Bilgi için [kullanılabilirlik özelliklerini](sql-database-technical-overview.md#availability-capabilities).

## <a name="transact-sql-differences"></a>Transact-SQL farklılıkları

Uygulamaları kullanan Transact-SQL özelliklerinin çoğu hem Microsoft SQL Server hem de Azure SQL veritabanı olarak tam olarak desteklenir. Örneğin, SQL Server ve SQL veritabanı veri türleri, işleçler, dize, aritmetik, mantıksal ve imleç işlevleri gibi temel SQL bileşenleri aynı şekilde çalışır. Vardır, ancak bazı T-SQL farklılıkları (veri tanımlama dili) DDL ve DML (veri işleme dili) öğeleri T-SQL deyimlerini ve kısmen desteklenen sorgular (Bu makalenin sonraki bölümlerinde ele).
Ayrıca, bazı özellikleri ve Azure SQL veritabanı özellikleri ana veritabanında ve işletim sistemi bağımlılıklardan yalıtmak için tasarlandığından, tüm desteklenmeyen söz dizimi vardır. Bu nedenle, çoğu sunucu düzeyi etkinlik, SQL veritabanı için uygun değildir. Sunucu düzeyi seçenekleri, işletim sistemi bileşenlerini yapılandırın veya dosya sistemi yapılandırmasını belirten T-SQL deyimlerini ve seçenekler kullanılamaz. Bu gibi özellikler gerekli olduğunda, uygun bir alternatif genellikle başka bir şekilde kullanılabilir SQL veritabanı veya başka bir Azure özelliği ya da hizmeti.

Daha fazla bilgi için [Transact-SQL farklılıklarını çözümleme SQL veritabanına geçiş sırasında](sql-database-transact-sql-information.md).

## <a name="security"></a>Güvenlik

SQL veritabanı sağlayan bir dizi [yerleşik güvenlik ve Uyumluluk](sql-database-security-overview.md) uygulamanızın çeşitli güvenlik ve uyumluluk gereksinimlerine yardımcı olacak özellikler.

## <a name="next-steps"></a>Sonraki adımlar

- İle hızlı bir şekilde tek bir veritabanı ile çalışmaya başlamak için Başlat [veritabanı hızlı başlangıç guide.md tek](sql-database-single-database-quickstart-guide.md).
- Bir SQL Server veritabanını Azure'a geçirme hakkında bilgi edinmek için [Azure SQL veritabanına geçirme](sql-database-cloud-migrate.md).
- Desteklenen özellikler hakkında bilgi edinmek için bkz. [Özellikler](sql-database-features.md).
