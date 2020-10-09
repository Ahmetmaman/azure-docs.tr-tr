---
title: Tek bir sayfa-Azure SYNAPSE Analytics (çalışma alanları Önizleme)
description: Azure SYNAPSE Analytics aracılığıyla başvuru kılavuzu yürüyen Kullanıcı
services: synapse-analytics
author: saveenr
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: overview
ms.date: 04/15/2020
ms.author: saveenr
ms.reviewer: jrasnick
ms.openlocfilehash: 774e503bec3f1f8c4cc5b85bb599230a3397f811
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2020
ms.locfileid: "91858447"
---
# <a name="azure-synapse-analytics-cheat-sheet"></a>Azure SYNAPSE Analytics, yemek sayfası

[!INCLUDE [preview](includes/note-preview.md)]

Azure SYNAPSE Analytics, hizmet ve önemli komutların temel kavramları boyunca size kılavuzluk eder. Bu makale, hem yeni öğrenenlere hem de temel Azure SYNAPSE konularının vurgulamaları isteyen kişilere yardımcı olur.

## <a name="basics"></a>Temel Bilgiler

**SYNAPSE çalışma alanı** , Azure 'da bulut tabanlı kurumsal analizler gerçekleştirmek için güvenli kılınabilir bir işbirliği sınırıdır. Çalışma alanı belirli bir bölgeye dağıtılır ve ilişkili bir ADLS 2. hesabına ve dosya sistemine sahiptir (geçici verileri depolamak için). Çalışma alanı bir kaynak grubu altında.

Bir çalışma alanı SQL ve Apache Spark ile analiz gerçekleştirmenize olanak tanır. SQL ve Spark Analytics için kullanılabilen kaynaklar SQL ve Spark **havuzlarında**düzenlenir. 

## <a name="synapse-sql"></a>Synapse SQL
**SYNAPSE SQL** , SYNAPSE çalışma alanında T-SQL tabanlı analizler yapabilme olanağıdır. SYNAPSE SQL 'in iki tüketim modeli vardır: adanmış ve sunucusuz.  Adanmış model için adanmış **SQL havuzları**kullanın. Bir çalışma alanı bu havuzlardan herhangi bir sayıda olabilir. Sunucusuz modeli kullanmak için, "SQL isteğe bağlı" adlı sunucusuz SQL havuzunu kullanın. Her çalışma alanı bu havuzlardan birine sahiptir.

## <a name="apache-spark-for-synapse"></a>SYNAPSE için Apache Spark
Spark Analytics 'i kullanmak için SYNAPSE çalışma alanınızda **Spark havuzları** oluşturun ve kullanın.

## <a name="sql-terminology"></a>SQL terminolojisi
| Süre                         | Tanım      |
|:---                                 |:---                 |
| **SQL Isteği**  |   Sorgu gibi işlem, SQL havuzu veya isteğe bağlı SQL üzerinden çalışır. |

## <a name="spark-terminology"></a>Spark terminolojisi
| Süre                         | Tanım      |
|:---                                 |:---                 |
|**SYNAPSE için Apache Spark** | Spark havuzunda kullanılan Spark çalışma zamanı. Desteklenen geçerli sürüm, Python 3.6.1, Scala 2.11.12, Apache Spark 0,5 ve Delta Lake 0,3 için .NET desteğiyle Spark 2,4.  | 
| **Apache Spark havuzu**  | karşılık gelen veritabanları ile 0--N Spark tarafından sağlanan kaynaklar bir çalışma alanında dağıtılabilir. Spark havuzu otomatik duraklatılabilir, devam ettirilebilir ve ölçeklendirilebilir.  |
| **Spark uygulaması**  |   Bir sürücü işlemi ve bir yürütücü işlemleri kümesinden oluşur. Spark uygulaması Spark havuzunda çalışır.            |
| **Spark oturumu**  |   Spark uygulamasının Birleşik giriş noktası. Spark 'ın çeşitli işlevleri ve daha az sayıda yapı ile etkileşime geçmek için bir yol sağlar. Bir not defteri çalıştırmak için bir oturumun oluşturulması gerekir. Bir oturum, belirli bir boyuttaki belirli sayıda yürüticiler üzerinde çalışacak şekilde yapılandırılabilir. Bir not defteri oturumunun varsayılan yapılandırması 2 orta ölçekli yürütmeçiler üzerinde çalıştırılır. |
|**Veri tümleştirme**| Çeşitli kaynaklar arasında veri alma ve çalışma alanı içinde veya çalışma alanı dışında çalışan etkinlikleri düzenleme özelliği sağlar.| 
|**Artifacts**| Bir kullanıcının veri kaynaklarını yönetmesi, geliştirilmesi, düzenlemeleri ve görselleştirmeleri için gereken tüm nesneleri kapsülleyen kavram.|
|**Not defteri**| Scala, PySpark, C# ve mini SQL destekleyen etkileşimli ve reaktif veri bilimi ve mühendislik arabirimi. |
|**Spark iş tanımı**|Kodu ve bağımlılıklarını içeren derleme jar ile Spark işi göndermek için arabirim.|
|**Veri Akışı**|  Büyük veri dönüştürmesi yapmak için hiçbir kodlamaya gerek olmadan tam bir görsel deneyimi sağlar. Tüm iyileştirme ve yürütme işlemleri sunucusuz bir biçimde işlenir. |
|**SQL betiği**| Bir dosyaya kaydedilmiş SQL komutları kümesi. Bir SQL betiği bir veya daha fazla SQL deyimi içerebilir. SQL havuzu veya istek üzerine SQL istekleri aracılığıyla SQL istekleri çalıştırmak için kullanılabilir.|
|**İşlem Hattı**| Bir görevi birlikte gerçekleştiren etkinliklerin mantıksal gruplandırması.|
|**Etkinlik**| Verilerin kopyalanması, bir not defteri veya bir SQL betiği çalıştırması gibi verilerde gerçekleştirilecek eylemleri tanımlar.|
|**Tetikleyici**| Bir işlem hattı yürütür. El ile veya otomatik olarak çalıştırılabilir (zamanlama, pencere veya olay tabanlı).|
|**Bağlı hizmet**| Dış kaynaklara bağlanmak için çalışma alanı için gereken bağlantı bilgilerini tanımlayan bağlantı dizeleri.|
|**Veri kümesi**|  Bir etkinlikte giriş ve çıkış olarak kullanılacak verileri işaret eden veya başvuruda bulunan verilerin adlandırılmış görünümü. Bu, bağlı bir hizmete aittir.|

## <a name="next-steps"></a>Sonraki adımlar

- [Çalışma alanı oluşturma](quickstart-create-workspace.md)
- [Synapse Studio’yu kullanma](quickstart-synapse-studio.md)
- [SQL havuzu oluşturma](quickstart-create-sql-pool-portal.md)
- [Apache Spark havuzu oluşturma](quickstart-create-apache-spark-pool-portal.md)
- [İsteğe bağlı SQL kullanma](quickstart-sql-on-demand.md)

