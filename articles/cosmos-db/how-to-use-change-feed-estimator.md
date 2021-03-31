---
title: Değişiklik akışı tahmini Azure Cosmos DB kullanın
description: Değişiklik akışı işlemcinizin ilerlemesini çözümlemek için değişiklik akışı tahmin aracı 'ı nasıl kullanacağınızı öğrenin
author: ealsur
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: how-to
ms.date: 08/15/2019
ms.author: maquaran
ms.custom: devx-track-csharp
ms.openlocfilehash: a44557d15f437317c2b5fa659ab8d4ca3c208edf
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "93339852"
---
# <a name="use-the-change-feed-estimator"></a>Değişiklik akışı tahmin aracı 'ı kullanın
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

Bu makalede değişiklik akışını okurken [değişiklik akışı işlemci](./change-feed-processor.md) örneklerinizin ilerlemesini nasıl izleyebileceğinizi açıklar.

## <a name="why-is-monitoring-progress-important"></a>İzleme ilerleme durumu neden önemlidir?

Değişiklik akışı işlemcisi, [değişiklik akışınızda](./change-feed.md) ileri doğru hareket eden bir işaretçi görevi görür ve değişiklikleri bir temsilci uygulamasına gönderir. 

Değişiklik akışı işlemcisi dağıtımı, CPU, bellek, ağ gibi kullanılabilir kaynaklara bağlı olarak belirli bir hızda değişiklik işleyebilir.

Bu oran, değişikliklerinizin Azure Cosmos kapsayıcısında gerçekleştiği orandan daha yavaşsa, işlemcinizin arkasında gecikme olacak şekilde bekleecektir.

Bu senaryonun belirlenmesi, değişiklik akışı işlemci dağıtımınızı ölçeklendirmemiz gerektiğini anlamanıza yardımcı olur.

## <a name="implement-the-change-feed-estimator"></a>Değişiklik akışı tahmin aracı 'ı uygulama

[Değişiklik akışı işlemcisi](./change-feed-processor.md)gibi değişiklik akışı tahmini, bir gönderme modeli olarak da kullanılır. Tahmin Aracı, son işlenen öğe (kiralamalar kapsayıcısının durumu tarafından tanımlanır) ve kapsayıcıda en son değişiklik arasındaki farkı ölçecek ve bu değeri bir temsilciye göndermeyecektir. Ölçümün alındığı zaman aralığı, varsayılan değer olan 5 saniyeye de özelleştirilebilir.

Örnek olarak, değişiklik akışı işlemciniz aşağıdaki gibi tanımlanır:

[!code-csharp[Main](~/samples-cosmosdb-dotnet-v3/Microsoft.Azure.Cosmos.Samples/Usage/ChangeFeed/Program.cs?name=StartProcessorEstimator)]

İşlemcinin şu şekilde kullanılması için bir tahmin aracı başlatmanın doğru yolu `GetChangeFeedEstimatorBuilder` :

[!code-csharp[Main](~/samples-cosmosdb-dotnet-v3/Microsoft.Azure.Cosmos.Samples/Usage/ChangeFeed/Program.cs?name=StartEstimator)]

Hem işlemcinin hem de tahmin aracı aynı `leaseContainer` adı ve aynı adı paylaşır.

Diğer iki parametre, işlemci tarafından **okunmayı bekleyen kaç değişiklik** olduğunu ve bu ölçümün alınmasını istediğiniz zaman aralığını temsil eden bir sayı alacak olan temsilcidir.

Tahmini alan bir temsilci örneği şunlardır:

[!code-csharp[Main](~/samples-cosmosdb-dotnet-v3/Microsoft.Azure.Cosmos.Samples/Usage/ChangeFeed/Program.cs?name=EstimationDelegate)]

Bu tahmini, izleme çözümünüze gönderebilir ve zaman içinde ilerleme durumunun nasıl davrandığınızı anlamak için kullanabilirsiniz.

> [!NOTE]
> Değişiklik akışı tahmin aracı 'ın, değişiklik akışı işlemcinizin bir parçası olarak dağıtılması veya aynı projenin bir parçası olması gerekmez. Bağımsız olabilir ve tamamen farklı bir örnekte çalıştırılabilir. Yalnızca aynı adı ve kira yapılandırmasını kullanması gerekir.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Cosmos DB SDK](sql-api-sdk-dotnet.md)
* [GitHub 'da kullanım örnekleri](https://github.com/Azure/azure-cosmos-dotnet-v3/tree/master/Microsoft.Azure.Cosmos.Samples/Usage/ChangeFeed)
* [GitHub 'da ek örnekler](https://github.com/Azure-Samples/cosmos-dotnet-change-feed-processor)

## <a name="next-steps"></a>Sonraki adımlar

Şimdi aşağıdaki makalelerde akış işlemcisini Değiştir hakkında daha fazla bilgi edinebilirsiniz:

* [Değişiklik akışı işlemcisine genel bakış](change-feed-processor.md)
* [Değişiklik akışı işlemcisi başlangıç zamanı](./change-feed-processor.md#starting-time)