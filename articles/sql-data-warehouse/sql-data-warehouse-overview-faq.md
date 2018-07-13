---
title: Azure SQL veri ambarı hakkında sık sorulan sorular | Microsoft Docs
description: Bu makalede, Azure SQL veri ambarı hakkında sık sorulan soruların müşterilere ve geliştiricilerinden kullanıma listelenir.
services: sql-data-warehouse
author: acomet
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: design
ms.date: 04/17/2018
ms.author: acomet
ms.reviewer: igorstan
ms.openlocfilehash: 7a570dfa6e0a2812a8dd3a25b8903c70fe07befc
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38971712"
---
# <a name="sql-data-warehouse-frequently-asked-questions"></a>SQL veri ambarı sık sorulan sorular

## <a name="general"></a>Genel

SORU. Hangi SQL DW veri güvenliği için sunduğu?

A. SQL DW TDE gibi veri koruma ve denetim için çeşitli çözümler sunar. Daha fazla bilgi için [güvenlik].

SORU. Hangi yasal veya iş standartları olduğunu SQL DW ile uyumlu nereden edinebilirim?

A. Ziyaret [Microsoft Uyumluluk] ürün SOC ve ISO gibi çeşitli uyumluluk teklifleri için sayfa. İlk uyumluluk başlığı seçin, ardından Azure genişletin Microsoft kapsamındaki bulut Hizmetleri bölümünde Azure olan hizmetler görmek için sayfanın sağ tarafındaki Hizmetleri uyumludur.

SORU. Power BI bağlayabilirim?

A. Evet! SQL DW ile doğrudan sorgu Powerbı destekler ancak çok sayıda kullanıcı veya gerçek zamanlı veriler için tasarlanmamıştır. Power BI'ın üretim sırasında kullanım için Azure Analysis Services veya Analysis Service Iaas üzerine Power BI'ı kullanmanızı öneririz. 

SORU. SQL veri ambarı kapasite sınırları nelerdir?

A. Bizim geçerli bkz [kapasite sınırları] sayfası. 

SORU. Neden benim ölçek/duraklatın/sürdürün kadar uzun sürüyor?

A. İşlem yönetimi işlemleri için süre çeşitli etkenlere etkileyebilir. Ortak işlemler uzun süre çalışan işlem geri alma için durumda. Bir ölçek veya duraklatma işlemi başlatıldığında, tüm gelen oturumları engellenir ve sorguları boşaltılır. Bir işlemin yeniden başlatılmadan önce sistem kararlı bir duruma ayrılabilmeniz işlemleri alınması gerekir. Büyük sayı ve daha büyük günlük boyutunu işlemler, uzun işlemi sistemi kararlı bir duruma geri durmuş.

## <a name="user-support"></a>Kullanıcı desteği

SORU. Burada gönderme bir özellik isteği sahibim?

A. Bir özellik isteği varsa, üzerinde iletin bizim [UserVoice] sayfası

SORU. Ne yapabilirim x?

A. SQL veri ambarı ile geliştirmeye yardımcı olmak için hakkında sorular sorabilirsiniz bizim [Stack Overflow] sayfası. 

SORU. Bir destek bileti nasıl gönderebilirim?

A. [Destek bileti] Azure portalı üzerinden Dosyalanan.

## <a name="sql-languagefeature-support"></a>SQL dil/özellik desteği 

SORU. Hangi veri türleri, SQL veri ambarı destekliyor mu?

A. SQL veri ambarı bkz [veri türleri].

SORU. Tablo Özellikleri destekliyorsunuz?

A. SQL veri ambarı birçok özellik desteklese de, bazı desteklenmez ve belgelenen [desteklenmeyen tablo özellikleri].

## <a name="tooling-and-administration"></a>Araçlar ve yönetim

SORU. Visual Studio veritabanı projelerini desteklemez.

A. Şu anda veritabanı projeleri Visual Studio'da SQL veri ambarı için desteklemiyoruz. Bu özelliği etkinleştirmek için oy dönüştürme yapmak isterseniz, bizim User Voice ziyaret [veritabanı projeleri özellik isteği].

SORU. SQL veri ambarı, REST API'lerini destekliyor mu?

A. Evet. SQL veritabanı ile kullanılabilecek en iyi REST işlevselliği de SQL veri ambarı ile kullanılabilir. REST belgeleri sayfaları içinde API bilgi bulabilirsiniz veya [MSDN].


## <a name="loading"></a>Yükleniyor

SORU. Hangi istemci sürücüleri destekliyorsunuz?

A. DW sürücü desteği bulunabilir [bağlantı dizeleri] sayfası

S: hangi dosya biçimlerine PolyBase ile SQL veri ambarı tarafından destekleniyor mu?

Y: Orc, RC, Parquet ve sınırlandırılmış düz metin

S: hangi PolyBase aracılığıyla SQL DW'ye bağlanabilirim? 

Y: [Azure Data Lake Store] ve [Azure depolama BLOB'ları]

S: itme hesaplama Azure depolama Blobları veya ADLS bağlanırken mi? 

C: Hayır, SQL DW PolyBase yalnızca depolama bileşenleri etkileşim kurar. 

HDI için bağlanabilirim miyim?

Y: HDI ADLS ya da WASB HDFS katman olarak kullanabilirsiniz. HDFS katmanınızdaki olarak varsa, bu verileri SQL DW'ye yükleyebilirsiniz. Ancak, itme hesaplama HDI örneği oluşturulamıyor. 

## <a name="next-steps"></a>Sonraki adımlar
Bir bütün olarak SQL veri ambarı hakkında daha fazla bilgi için bkz. bizim [genel bakış] sayfası.


<!-- Article references -->
[UserVoice]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[Bağlantı Dizeleri]: ./sql-data-warehouse-connection-strings.md
[Stack Overflow]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Destek bileti]: ./sql-data-warehouse-get-started-create-support-ticket.md
[Güvenlik]: ./sql-data-warehouse-overview-manage-security.md
[Microsoft Uyumluluk]: https://www.microsoft.com/en-us/trustcenter/compliance/complianceofferings
[Kapasite sınırları]: ./sql-data-warehouse-service-capacity-limits.md
[veri türleri]: ./sql-data-warehouse-tables-data-types.md
[Desteklenmeyen tablo özellikleri]: ./sql-data-warehouse-tables-overview.md#unsupported-table-features
[Azure Data Lake Store]: ./sql-data-warehouse-load-from-azure-data-lake-store.md
[Azure depolama BLOB'ları]: ./sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md
[Veritabanı projeleri özellik isteği]: https://feedback.azure.com/forums/307516-sql-data-warehouse/suggestions/13313247-database-project-from-visual-studio-to-support-azu
[MSDN]: https://msdn.microsoft.com/library/azure/mt163685.aspx
[Genel Bakış]: ./sql-data-warehouse-overview-faq.md
