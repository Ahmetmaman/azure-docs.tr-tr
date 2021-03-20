---
title: Bir veritabanını içeri ve dışarı aktarma işlemi uzun zaman alır
description: Azure SQL veritabanı ve Azure SQL yönetilen örnek Içeri/dışarı aktarma hizmeti bir veritabanını içeri veya dışarı aktarmak uzun zaman alıyor
ms.custom: seo-lt-2019, sqldbrb=1
services: sql-database
ms.service: sql-db-mi
ms.subservice: data-movement
ms.topic: troubleshooting
author: v-miegge
ms.author: ramakoni
ms.reviewer: ''
ms.date: 09/27/2019
ms.openlocfilehash: e69bba858ccf62f1b3a3b45b08771ddba71f11cf
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "92671393"
---
# <a name="azure-sql-database-and-managed-instance-importexport-service-takes-a-long-time-to-import-or-export-a-database"></a>Azure SQL veritabanı ve yönetilen örnek Içeri/dışarı aktarma hizmeti bir veritabanını içeri veya dışarı aktarmak uzun zaman alıyor

[!INCLUDE[appliesto-sqldb-sqlmi](../includes/appliesto-sqldb-sqlmi.md)]

Içeri/dışarı aktarma hizmetini kullandığınızda, işlem beklenenden uzun sürebilir. Bu makalede, bu gecikme ve alternatif geçici çözüm yöntemlerinin olası nedenleri açıklanmaktadır.

## <a name="azure-sql-database-importexport-service"></a>Azure SQL veritabanı Içeri/dışarı aktarma hizmeti

Azure SQL veritabanı Içeri/dışarı aktarma hizmeti, her Azure veri merkezinde çalışan REST tabanlı bir Web hizmetidir. Veritabanınızı Azure portal taşımak için [veritabanını Içeri aktar](database-import.md#using-azure-portal) veya [dışarı aktar](./database-import.md#using-azure-portal) seçeneğini kullandığınızda bu hizmet çağrılır. Hizmet, Azure SQL veritabanı ile Azure Blob depolama arasında içeri aktarmalar ve dışarı aktarmalar gerçekleştirmek için ücretsiz istek sıraya alma ve işlem hizmetleri sağlar.

İçeri ve dışarı aktarma işlemleri geleneksel bir fiziksel veritabanı yedeklemesini temsil etmez ancak bunun yerine özel bir BACPAC biçimi kullanan veritabanının mantıksal bir yedeklemesi değildir. BACPAC biçimi Microsoft SQL Server, Azure SQL veritabanı ve Azure SQL yönetilen örneği sürümleri arasında değişebilen bir fiziksel biçim kullanmanızı önlemenize olanak sağlar.

## <a name="what-causes-delays-in-the-process"></a>İşlemde gecikmelere neden oluyor?

Azure SQL veritabanı Içeri/dışarı aktarma hizmeti, içeri ve dışarı aktarma işlemlerini işlemek için bölge başına sınırlı sayıda işlem sanal makinesi (VM) sağlar. İşlem VM 'Leri bölge başına barındırılır ve içeri aktarma veya dışarı aktarma, bölgeler arası bant genişliği gecikmelerini ve ücretlerinden kaçınılmasını sağlar. Aynı bölgede aynı anda çok fazla istek yapılırsa, işlemleri işlemek için önemli gecikmeler meydana gelebilir. İstekleri tamamlamaya yönelik gereken süre birkaç saniyeden fazla saate kadar farklılık gösterebilir.

> [!NOTE]
> Bir istek dört gün içinde işlenmezse, hizmet isteği otomatik olarak iptal eder.

## <a name="recommended-solutions"></a>Önerilen çözümler

Veritabanınızın dışarı aktarmaları yalnızca yanlışlıkla veri silme işleminden kurtarmak için kullanılırsa, tüm Azure SQL veritabanı sürümleri sistem tarafından oluşturulan yedeklerden Self Servis geri yükleme özelliği sağlar. Ancak diğer nedenlerle bu dışarı aktarmalar varsa ve sürekli olarak daha hızlı veya daha öngörülebilir içeri/dışarı aktarma performansına ihtiyacınız varsa, aşağıdaki seçenekleri göz önünde bulundurun:

* [SQLPackage yardımcı programını kullanarak BIR BACPAC dosyasına dışarı aktarın](./database-export.md#sqlpackage-utility).
* [SQL Server Management Studio (SSMS) kullanarak BACPAC dosyasına dışarı aktarın](./database-export.md#sql-server-management-studio-ssms).
* Microsoft SQL Server Data-Tier uygulama çerçevesi (DacFx) API 'sini kullanarak doğrudan kodunuzda BACPAC içeri aktarma veya dışarı aktarma işlemini çalıştırın. Daha fazla bilgi için bkz.
  * [Veri katmanı uygulamasını dışarı aktarma](/sql/relational-databases/data-tier-applications/export-a-data-tier-application)
  * [Microsoft. SqlServer. Dac ad alanı](/dotnet/api/microsoft.sqlserver.dac)
  * [DACFx 'i indir](https://www.microsoft.com/download/details.aspx?id=55713)

## <a name="things-to-consider-when-you-export-or-import-a-database"></a>Bir veritabanını dışarı veya içeri aktarırken göz önünde bulundurmanız gerekenler

* Bu makalede ele alınan tüm yöntemler veritabanı Işlem birimi (DTU) kotasını kullanır ve bu da Azure SQL veritabanı hizmeti tarafından azaltma işlemine neden olur. [Veritabanı IÇIN DTU istatistiklerini Azure Portal görüntüleyebilirsiniz](./monitor-tune-overview.md#azure-sql-database-and-azure-sql-managed-instance-resource-monitoring). Veritabanı kaynak sınırlarına ulaştıysa, daha fazla kaynak eklemek için [hizmet katmanını yükseltin](./scale-resources.md) .
* İdeal olarak, veritabanı ile aynı bölgedeki bir VM 'den istemci uygulamaları (SqlPackage yardımcı programı veya özel DAC uygulamanız gibi) çalıştırmalısınız. Aksi takdirde, ağ gecikme süresiyle ilgili performans sorunlarıyla karşılaşabilirsiniz.
* Kümelenmiş dizinler olmadan büyük tabloları dışarı aktarmak çok yavaş olabilir veya hatta hataya neden olabilir. Bu davranış, tablo bölünemediği ve paralel olarak verilemeyeceği için oluşur. Bunun yerine, tek bir işlemde dışa aktarılmalıdır ve özellikle büyük tablolar için, dışa aktarma sırasında yavaş performans ve olası hata oluşmasına neden olur.


## <a name="related-documents"></a>İlgili belgeler

[Bir veritabanını dışarı aktarırken dikkat edilecek noktalar](./database-export.md#considerations)