---
title: Tutarlılık düzeyleri ve Azure Cosmos DB API’leri
description: Azure Cosmos DB ve Apache Cassandra, MongoDB 'de farklı API 'Ler arasındaki tutarlılık düzeyi eşlemesini anlama
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 08/6/2020
ms.reviewer: sngun
ms.openlocfilehash: bb8a413f2e2a3aa4a8facd533d822312bb61fa0e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91613569"
---
# <a name="consistency-levels-and-azure-cosmos-db-apis"></a>Tutarlılık düzeyleri ve Azure Cosmos DB API’leri

Azure Cosmos DB popüler veritabanları için kablolu protokol ile uyumlu API 'Ler için yerel destek sağlar. Bunlar MongoDB, Apache Cassandra, Gremlin ve Azure Tablo depolama 'yı içerir. Bu veritabanları, tutarlılık düzeyleri için tam olarak tanımlanmış tutarlılık modelleri veya SLA ile desteklenen garantiler sunmaz. Genellikle, Azure Cosmos DB tarafından sunulan beş tutarlılık modelinin yalnızca bir alt kümesini sağlar.

SQL API, Gremlin API ve Tablo API'si kullanıldığında, Azure Cosmos hesabında yapılandırılan varsayılan tutarlılık düzeyi kullanılır. 

MongoDB için Cassandra API veya Azure Cosmos DB API 'sini kullanırken, uygulamalar, sırasıyla Apache Cassandra ve MongoDB tarafından sunulan bir tutarlılık düzeyi kümesini, hatta daha güçlü tutarlılık ve dayanıklılık garantisi elde eder. Bu belgede Apache Cassandra ve MongoDB tutarlılık düzeyleri için karşılık gelen Azure Cosmos DB tutarlılığı düzeyleri gösterilmektedir.

> [!NOTE]
> Azure Cosmos DB için varsayılan tutarlılık modeli oturumdur. Oturum, Cassandra ya da MongoDB tarafından yerel olarak desteklenmeyen istemci merkezli bir tutarlılık modelidir. Hangi tutarlılık modelinin seçeyli hakkında daha fazla bilgi için [Azure Cosmos DB ' deki tutarlılık düzeyleri](consistency-levels.md) ' ne bakın

## <a name="mapping-between-apache-cassandra-and-azure-cosmos-db-consistency-levels"></a><a id="cassandra-mapping"></a>Apache Cassandra ve Azure Cosmos DB tutarlılık düzeyleri arasında eşleme

Azure Cosmos DB aksine Apache Cassandra kesin olarak tanımlanmış tutarlılık garantisi sağlamaz.  Bunun yerine Apache Cassandra, yüksek kullanılabilirlik, tutarlılık ve gecikme süresi avantajları sağlamak için bir yazma tutarlılığı düzeyi ve bir okuma tutarlılığı düzeyi sağlar. Azure Cosmos DB Cassandra API kullanırken: 

* Apache Cassandra 'ın yazma tutarlılığı düzeyi, Azure Cosmos hesabınızda yapılandırılmış varsayılan tutarlılık düzeyiyle eşleştirilir. Bir yazma işlemi (CL) tutarlılığı, istek başına temelinde değiştirilemez.

* Azure Cosmos DB, Cassandra istemci sürücüsüyle belirtilen okuma tutarlılığı düzeyini, bir okuma isteği üzerinde dinamik olarak yapılandırılmış Azure Cosmos DB tutarlılık düzeylerinden birine dinamik olarak eşler. 

Aşağıdaki tabloda, Cassandra API kullanılırken yerel Cassandra tutarlılık düzeylerinin Azure Cosmos DB tutarlılık düzeylerine nasıl eşlendiği gösterilmektedir:  

:::image type="content" source="./media/consistency-levels-across-apis/consistency-model-mapping-cassandra.png" alt-text="Cassandra tutarlılık modeli eşleme" lightbox="./media/consistency-levels-across-apis/consistency-model-mapping-cassandra.png" :::

## <a name="mapping-between-mongodb-and-azure-cosmos-db-consistency-levels"></a><a id="mongo-mapping"></a>MongoDB ve Azure Cosmos DB tutarlılık düzeyleri arasında eşleme

Azure Cosmos DB aksine, yerel MongoDB kesin olarak tanımlanmış tutarlılık garantisi sağlamaz. Bunun yerine, yerel MongoDB kullanıcıların, istenen tutarlılık düzeyini elde etmek için, okuma işlemlerini birincil veya ikincil çoğaltmalara yönlendirecek bir yazma sorunu, bir okuma sorunu ve isMaster yönergesi sağlar.

MongoDB için Azure Cosmos DB API 'SI kullanılırken MongoDB sürücüsü, yazma bölgenizi birincil çoğaltma olarak değerlendirir ve diğer tüm bölgeler okuma çoğaltmasıdır. Azure Cosmos hesabınızla ilişkilendirilen bölgeyi birincil çoğaltma olarak seçebilirsiniz. 

Azure Cosmos DB, MongoDB için API 'sini kullanırken:

* Yazma sorunu, Azure Cosmos hesabınızda yapılandırılmış varsayılan tutarlılık düzeyiyle eşleştirilir.

* Azure Cosmos DB, MongoDB istemci sürücüsüyle belirtilen okuma kaygısını, bir okuma isteği üzerinde dinamik olarak yapılandırılmış Azure Cosmos DB tutarlılık düzeylerinden birine dinamik olarak eşler.  

* Bölgeyi ilk yazılabilir bölge olarak yaparak Azure Cosmos hesabınızla ilişkili belirli bir bölgeye "birincil" olarak ek açıklama ekleyebilirsiniz. 

Aşağıdaki tabloda, MongoDB için Azure Cosmos DB API 'SI kullanılırken yerel MongoDB yazma/okuma sorunlarının Azure Cosmos tutarlılık düzeylerine nasıl eşlendiği gösterilmektedir:

:::image type="content" source="./media/consistency-levels-across-apis/consistency-model-mapping-mongodb.png" alt-text="Cassandra tutarlılık modeli eşleme" lightbox= "./media/consistency-levels-across-apis/consistency-model-mapping-mongodb.png":::

## <a name="next-steps"></a>Sonraki adımlar

Açık kaynaklı API 'lerle Azure Cosmos DB API 'Ler arasındaki tutarlılık düzeyleri ve uyumluluk hakkında daha fazla bilgi edinin. Aşağıdaki makalelere bakın:

* [Çeşitli tutarlılık düzeyleri için kullanılabilirlik ve performans avantajları](consistency-levels-tradeoffs.md)
* [MongoDB için Azure Cosmos DB API 'sinin desteklediği MongoDB özellikleri](mongodb-feature-support.md)
* [Azure Cosmos DB tarafından desteklenen Apache Cassandra özellikleri Cassandra API](cassandra-support.md)
