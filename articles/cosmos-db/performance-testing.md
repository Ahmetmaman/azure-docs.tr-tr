---
title: Azure Cosmos DB ölçek ve performans testi | Microsoft Docs
description: Ölçek ve performans testi Azure Cosmos DB ile yapmayı öğrenin
keywords: Performans testi
services: cosmos-db
author: SnehaGunda
manager: kfile
editor: ''
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 08/29/2017
ms.author: sngun
ms.openlocfilehash: 09ed72c73acf16f944c3b1101aff5ea04acb624d
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/26/2018
ms.locfileid: "52308180"
---
# <a name="performance-and-scale-testing-with-azure-cosmos-db"></a>Performans ve ölçek testi ile Azure Cosmos DB

Performans ve ölçek testi, uygulama geliştirme önemli bir adım olduğunu. Birçok uygulama için veritabanı katmanı genel performans ve ölçeklenebilirlik üzerinde önemli bir etkisi yoktur. Bu nedenle performans testi, kritik bir bileşen gereklidir. [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) esnek ölçek ve öngörülebilir performans için özel olarak tasarlanmış. Bu özellikler, yüksek performanslı veritabanı katmanı gerek duyan uygulamalar için mükemmel bir uyum sağlar. 

Bu makalede, Azure Cosmos DB iş yükleri için performansı test paketlerini uygulama geliştiricilerine yönelik bir başvurudur. Ayrıca, yüksek performanslı uygulama senaryoları için Azure Cosmos DB değerlendirmek için de kullanılabilir. Veritabanını öncelikle yalıtılmış performans testi üzerinde odaklanmaktadır, ancak üretim uygulamaları için en iyi uygulamalar da içerir.

Bu makaleyi okuduktan sonra aşağıdaki soruları yanıtlamak mümkün olacaktır: 

* Azure Cosmos DB performans testi için örnek .NET istemci uygulaması nerede bulabilirim? 
* İstemci uygulamamı nasıl Azure Cosmos DB ile yüksek aktarım hızı düzeylerine ulaşabilirsiniz?

Kod ile çalışmaya başlamak için içinden projeyi karşıdan [Azure Cosmos DB performans örnek test](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark). 

> [!NOTE]
> Bu uygulama amacı, az sayıda istemci makine ile Azure Cosmos DB'den en iyi performansı elde etme göstermektir. Örnek amacı, yoğun işleme kapasitesi (Bu sınırlar ölçeklendirebilirsiniz) Azure Cosmos DB, elde yok etmektir.
> 
> 

Azure Cosmos DB performansını artırmak, istemci tarafı yapılandırma seçenekleri arıyorsanız bkz [Azure Cosmos DB performans ipuçları](performance-tips.md).

## <a name="run-the-performance-testing-application"></a>Performans uygulama testi Çalıştır
Başlamak için hızlı derlemek ve bir .NET örneği çalıştırmak için aşağıdaki adımlarda anlatıldığı gibi yoludur. Ayrıca, kaynak kodu gözden geçirin ve kendi istemci uygulamalar üzerinde benzer yapılandırmaları uygular.

**1. adım:** içinden projeyi karşıdan [Azure Cosmos DB performans örnek test](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark), veya GitHub depo çatalı oluşturma.

**2. adım:** EndpointUrl ve AuthorizationKey, CollectionThroughput ve DocumentTemplate (App.config dosyasında isteğe bağlı) ayarlarını değiştirin.

> [!NOTE]
> Yüksek aktarım hızı koleksiyonlarla sağlamadan önce başvurmak [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/cosmos-db/) koleksiyon başına maliyetlerini tahmin etmek için. Azure Cosmos DB, depolama ve aktarım hızını bağımsız olarak bir saatlik olarak düzenler. Silerek veya test edildikten sonra Azure Cosmos DB koleksiyonları verimini azaltmayı maliyetlerinden tasarruf edebilirler.
> 
> 

**3. adım:** derleyin ve konsol uygulamasını komut satırından çalıştırın. Şuna benzer bir çıktı görmeniz gerekir:

    C:\Users\cosmosdb\Desktop\Benchmark>DocumentDBBenchmark.exe
    Summary:
    ---------------------------------------------------------------------
    Endpoint: https://arramacquerymetrics.documents.azure.com:443/
    Collection : db.data at 100000 request units per second
    Document Template*: Player.json
    Degree of parallelism*: -1
    ---------------------------------------------------------------------

    DocumentDBBenchmark starting...
    Found collection data with 100000 RU/s
    Starting Inserts with 100 tasks
    Inserted 4503 docs @ 4491 writes/s, 47070 RU/s (122B max monthly 1KB reads)
    Inserted 17910 docs @ 8862 writes/s, 92878 RU/s (241B max monthly 1KB reads)
    Inserted 32339 docs @ 10531 writes/s, 110366 RU/s (286B max monthly 1KB reads)
    Inserted 47848 docs @ 11675 writes/s, 122357 RU/s (317B max monthly 1KB reads)
    Inserted 58857 docs @ 11545 writes/s, 120992 RU/s (314B max monthly 1KB reads)
    Inserted 69547 docs @ 11378 writes/s, 119237 RU/s (309B max monthly 1KB reads)
    Inserted 80687 docs @ 11345 writes/s, 118896 RU/s (308B max monthly 1KB reads)
    Inserted 91455 docs @ 11272 writes/s, 118131 RU/s (306B max monthly 1KB reads)
    Inserted 102129 docs @ 11208 writes/s, 117461 RU/s (304B max monthly 1KB reads)
    Inserted 112444 docs @ 11120 writes/s, 116538 RU/s (302B max monthly 1KB reads)
    Inserted 122927 docs @ 11063 writes/s, 115936 RU/s (301B max monthly 1KB reads)
    Inserted 133157 docs @ 10993 writes/s, 115208 RU/s (299B max monthly 1KB reads)
    Inserted 144078 docs @ 10988 writes/s, 115159 RU/s (298B max monthly 1KB reads)
    Inserted 155415 docs @ 11013 writes/s, 115415 RU/s (299B max monthly 1KB reads)
    Inserted 166126 docs @ 10992 writes/s, 115198 RU/s (299B max monthly 1KB reads)
    Inserted 173051 docs @ 10739 writes/s, 112544 RU/s (292B max monthly 1KB reads)
    Inserted 180169 docs @ 10527 writes/s, 110324 RU/s (286B max monthly 1KB reads)
    Inserted 192469 docs @ 10616 writes/s, 111256 RU/s (288B max monthly 1KB reads)
    Inserted 199107 docs @ 10406 writes/s, 109054 RU/s (283B max monthly 1KB reads)
    Inserted 200000 docs @ 9930 writes/s, 104065 RU/s (270B max monthly 1KB reads)

    Summary:
    ---------------------------------------------------------------------
    Inserted 200000 docs @ 9928 writes/s, 104063 RU/s (270B max monthly 1KB reads)
    ---------------------------------------------------------------------
    DocumentDBBenchmark completed successfully.
    Press any key to exit...


**(Gerekirse) 4. adım:** rapor işleme (RU/s) aracından aynı olmalıdır veya koleksiyon için sağlanan aktarım hızı veya koleksiyonları kümesi daha yüksek. Yüklü değilse, DegreeOfParallelism küçük artışlarla artırma sınırı ulaşmanıza yardımcı olabilir. İstemci uygulamanızı akışından plateaus, ek istemci makinelerinde birden fazla uygulamayı başlatın. Bu adımla ilgili yardıma ihtiyacınız varsa, e-posta askcosmosdb@microsoft.com veya bir destek bileti dosya [Azure portalında](https://portal.azure.com).

Uygulamayı oluşturduktan sonra farklı deneyebilirsiniz [dizinleme ilkeleri](index-policy.md) ve [tutarlılık düzeyleri](consistency-levels.md) aktarım hızı ve gecikme süresini üzerindeki etkilerini anlamak için. Ayrıca, kaynak kodu gözden geçirin ve kendi test paketleri veya üretim uygulamaları için benzer yapılandırmaları uygulayın.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, nasıl performans ve ölçek bir .NET konsol uygulaması kullanarak Azure Cosmos DB ile testi gerçekleştirebilirsiniz incelemiştik. Daha fazla bilgi için aşağıdaki makalelere bakın:

* [Azure Cosmos DB performans örnek test etme](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark)
* [Azure Cosmos DB performansını artırmak için istemci yapılandırma seçenekleri](performance-tips.md)
* [Sunucu tarafı Azure Cosmos DB'de bölümleme](partition-data.md)


