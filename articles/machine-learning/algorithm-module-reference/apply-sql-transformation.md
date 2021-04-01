---
title: SQL Dönüşümü Uygulama
titleSuffix: Azure Machine Learning
description: Verileri dönüştürmek üzere giriş veri kümelerinde bir SQLite sorgusu çalıştırmak için Azure Machine Learning SQL dönüştürme modülünü nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 11/12/2020
ms.openlocfilehash: c66fbe59fd5b2660d02bfca285f78666d64569fe
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2021
ms.locfileid: "94555609"
---
# <a name="apply-sql-transformation"></a>SQL Dönüşümü Uygulama

Bu makalede bir Azure Machine Learning Designer modülü açıklanmaktadır.

SQL dönüştürme modülünü Uygula ' yı kullanarak şunları yapabilirsiniz:
  
-   Sonuçlar için tablo oluşturma ve veri kümelerini taşınabilir bir veritabanına kaydetme.  
  
-   Veri türlerinde özel dönüşümler gerçekleştirin veya toplamalar oluşturun.  
  
-   Verileri filtrelemek veya değiştirmek ve sorgu sonuçlarını veri tablosu olarak döndürmek için SQL sorgu deyimlerini yürütün.  

> [!IMPORTANT]
> Bu modülde kullanılan SQL altyapısı **SQLite**' dur. SQLite sözdizimi hakkında daha fazla bilgi için bkz. [SQLite tarafından anlaşıldığı gibi SQL](https://www.sqlite.org/index.html).
> Bu modül, verileri bellek DB 'de bulunan SQLite 'a çarpacaktır, bu nedenle modül yürütmesi çok daha fazla bellek gerektirir ve bir `Out of memory` hatayla karşılaşabilir. Bilgisayarınızda yeterli sayıda RAM bulunduğundan emin olun.

## <a name="how-to-configure-apply-sql-transformation"></a>SQL dönüşümünü Uygula ' yı yapılandırma  

Modülün giriş olarak üç veri kümesi olabilir. Her giriş bağlantı noktasına bağlı veri kümelerine başvurduğunuzda, ve adlarını kullanmanız gerekir `t1` `t2` `t3` . Tablo numarası, giriş bağlantı noktasının dizinini gösterir.  

Aşağıdaki örnek kod, iki tablonun nasıl birleştirileceğini gösterir. T1 ve T2, **SQL dönüşümünü Uygula**'nın sol ve orta giriş bağlantı noktalarına bağlı iki veri kümeleridir:

```sql
SELECT t1.*
    , t3.Average_Rating
FROM t1 join
    (SELECT placeID
        , AVG(rating) AS Average_Rating
    FROM t2
    GROUP BY placeID
    ) as t3
on t1.placeID = t3.placeID
```
  
Kalan parametre, SQLite sözdizimini kullanan bir SQL sorgusudur. **SQL betiği** metin kutusuna birden çok satır yazarken her bir ifadeyi sonlandırmak için noktalı virgül kullanın. Aksi takdirde, satır sonları boşluklara dönüştürülür.  

Bu modül, SQLite sözdiziminin tüm standart deyimlerini destekler. Desteklenmeyen deyimlerin bir listesi için [Teknik notlar](#technical-notes) bölümüne bakın.

##  <a name="technical-notes"></a>Teknik notlar  

Bu bölümde, sık sorulan soruların uygulama ayrıntıları, ipuçları ve yanıtları yer almaktadır.

-   Bağlantı noktası 1 üzerinde her zaman bir giriş gereklidir.  
  
-   Boşluk veya diğer özel karakterler içeren sütun tanımlayıcıları için, `SELECT` veya yan tümcelerinde sütuna başvuru yaparken her zaman köşeli ayraç veya çift tırnak işareti içinde sütun tanımlayıcısını çevreler `WHERE` .  
  
### <a name="unsupported-statements"></a>Desteklenmeyen deyimler  

SQLite, ANSI SQL standardının çoğunu desteklese de, ticari ilişkisel veritabanı sistemleri tarafından desteklenen birçok özellik içermez. Daha fazla bilgi için bkz. [SQLite tarafından anlaşıldığı gibi SQL](http://www.sqlite.org/lang.html). Ayrıca, SQL deyimleri oluştururken aşağıdaki kısıtlamalara dikkat edin:  
  
- SQLite, en ilişkisel veritabanı sistemlerinde olduğu gibi bir sütuna bir tür atamak yerine, değerler için dinamik yazma kullanır. Kesin olarak yazılmış ve örtük tür dönüştürmeye izin veren.  
  
- `LEFT OUTER JOIN` uygulandı, ancak değil `RIGHT OUTER JOIN` `FULL OUTER JOIN` .  

- `RENAME TABLE`Komutuyla ve deyimlerini kullanabilirsiniz `ADD COLUMN` `ALTER TABLE` , ancak, ve dahil diğer yan tümceler desteklenmez `DROP COLUMN` `ALTER COLUMN` `ADD CONSTRAINT` .  
  
- SQLite içinde bir görünüm oluşturabilirsiniz, ancak bundan sonra görünümler salt okunurdur. `DELETE` `INSERT` Bir görünümde, veya ifadesini yürütemezsiniz `UPDATE` . Ancak, bir görünümde,, veya bir deneyin üzerinde başlatılan bir tetikleyici oluşturabilir `DELETE` `INSERT` `UPDATE` ve Tetikleyicinin gövdesinde başka işlemler gerçekleştirebilirsiniz.  
  

Resmi SQLite sitesinde sağlanan desteklenmeyen işlevlerin listesine ek olarak, aşağıdaki wiki desteklenmeyen diğer özelliklerin bir listesini sağlar: [SQLite-desteklenmeyen SQL](http://www2.sqlite.org/cvstrac/wiki?p=UnsupportedSql)  
    
## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül kümesine](module-reference.md) bakın. 
