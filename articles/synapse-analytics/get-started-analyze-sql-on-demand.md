---
title: 'Öğretici: serverles SQL ile verileri çözümlemeye başlama'
description: Bu öğreticide, Spark veritabanlarında bulunan verileri kullanarak verileri SQL isteğe bağlı olarak çözümlemeyi öğreneceksiniz.
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.topic: tutorial
ms.date: 07/20/2020
ms.openlocfilehash: 8d26a03a8b61850dc17bc4efff5f8ca12dfca191
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91300233"
---
# <a name="analyze-data-with-sql-on-demand"></a>İsteğe bağlı SQL ile verileri analiz etme

Bu öğreticide, Spark veritabanlarında bulunan verileri kullanarak isteğe bağlı bir SQL havuzunu kullanarak sunucusuz SQL ile verileri çözümlemeyi öğreneceksiniz. 

## <a name="analyze-nyc-taxi-data-in-blob-storage-using-sql-on-demand-pool"></a>SQL isteğe bağlı havuzu kullanarak blob depolamada NYC TAXI verilerini çözümleme

1. **Bağlı**' ın altındaki **veri** hub 'ında Azure Blob Storage ' a sağ tıklayıp **> örnek veri kümeleri > nyc_tlc_yellow** ve **en üstteki 100 satırı seç** ' i seçin
1. Bu, aşağıdaki kodla yeni bir SQL betiği oluşturur:

    ```
    SELECT
        TOP 100 *
    FROM
        OPENROWSET(
            BULK     'https://azureopendatastorage.blob.core.windows.net/nyctlc/yellow/puYear=*/puMonth=*/*.parquet',
            FORMAT = 'parquet'
        ) AS [result];
    ```
1. **Çalıştır** 'a tıklayın

## <a name="analyze-nyc-taxi-data-in-spark-databases-using-sql-on-demand"></a>İstek üzerine SQL kullanarak Spark veritabanlarında NYC TAXI verilerini çözümleyin

Spark veritabanlarındaki tablolar otomatik olarak görünür ve isteğe bağlı olarak SQL tarafından sorgulanırlar.

1. SYNAPSE Studio 'da, **geliştirme** merkezine gidin ve yenı bir SQL betiği oluşturun.
1. **İsteğe bağlı SQL**'e **bağlanma** ayarı yapın.
1. Aşağıdaki metni betiğe yapıştırın ve betiği çalıştırın.

    ```sql
    SELECT *
    FROM nyctaxi.dbo.passengercountstats
    ```

    > [!NOTE]
    > İsteğe bağlı SQL kullanan bir sorguyu ilk kez çalıştırdığınızda, SQL istek üzerine sorguları çalıştırmak için gereken SQL kaynaklarını toplamak için yaklaşık 10 saniye sürer. Sonraki sorgular çok daha hızlı olacaktır.
  


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Spark kullanarak çözümleme](get-started-analyze-spark.md)
