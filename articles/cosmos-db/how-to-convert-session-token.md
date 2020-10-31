---
title: .NET SDK-Azure Cosmos DB oturum belirteci biçimlerini dönüştürme
description: Farklı .NET SDK sürümleri arasında uyumluluk sağlamak için oturum belirteci biçimlerini dönüştürmeyi öğrenin
author: vinhms
ms.service: cosmos-db
ms.topic: how-to
ms.date: 04/30/2020
ms.author: vitrinh
ms.custom: devx-track-csharp
ms.openlocfilehash: ece181d06c7d3dbd00ba2f1262a3887ad966d088
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93101672"
---
# <a name="convert-session-token-formats-in-net-sdk"></a>.NET SDK 'da oturum belirteci biçimlerini dönüştürme
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

Bu makalede, SDK sürümleri arasında uyumluluk sağlamak için farklı oturum belirteci biçimleri arasında dönüştürme işleminin nasıl yapılacağı açıklanır.

> [!NOTE]
> Varsayılan olarak SDK, oturum belirtecini otomatik olarak izler ve en son oturum belirtecini kullanır.  Daha fazla bilgi için lütfen [oturum belirteçlerini](how-to-manage-consistency.md#utilize-session-tokens)kullanın ' ı ziyaret edin. Bu makaledeki yönergeler yalnızca aşağıdaki koşullara göre geçerlidir:
> * Azure Cosmos DB hesabınız oturum tutarlılığını kullanır.
> * Oturum belirteçlerini yönetmiyorsanız, el ile.
> * Aynı anda SDK 'nın birden çok sürümünü kullanıyorsunuz.

## <a name="session-token-formats"></a>Oturum belirteci biçimleri

İki oturum belirteci biçimi vardır: **basit** ve **vektör** .  Bu iki biçim de değiştirilemez, bu nedenle, farklı sürümlerle istemci uygulamasına geçiş yaparken biçimin dönüştürülmesi gerekir.
- **Basit** oturum belirteci BIÇIMI .NET SDK v1 tarafından kullanılır (Microsoft.Azure.DocumentDB-sürüm 1. x)
- **Vektör** oturum belirteci BIÇIMI .NET SDK V2 (Microsoft.Azure.DocumentDB-sürüm 2. x) tarafından kullanılır

### <a name="simple-session-token"></a>Basit oturum belirteci

Basit bir oturum belirteci şu biçimdedir: `{pkrangeid}:{globalLSN}`

### <a name="vector-session-token"></a>Vektör oturum belirteci

Vektör oturumu belirteci aşağıdaki biçimdedir: `{pkrangeid}:{Version}#{GlobalLSN}#{RegionId1}={LocalLsn1}#{RegionId2}={LocalLsn2}....#{RegionIdN}={LocalLsnN}`

## <a name="convert-to-simple-session-token"></a>Basit oturum belirtecine Dönüştür

.NET SDK v1 kullanarak bir oturum belirtecini istemciye geçirmek için **basit** bir oturum belirteci biçimi kullanın.  Örneğin, dönüştürmek için aşağıdaki örnek kodu kullanın.

```csharp
private static readonly char[] SegmentSeparator = (new[] { '#' });
private static readonly char[] PkRangeSeparator = (new[] { ':' });

// sessionTokenToConvert = session token from previous response
string[] items = sessionTokenToConvert.Split(PkRangeSeparator, StringSplitOptions.RemoveEmptyEntries);
string[] sessionTokenSegments = items[1].Split(SessionTokenHelpers.SegmentSeparator, StringSplitOptions.RemoveEmptyEntries);

string sessionTokenInSimpleFormat;

if (sessionTokenSegments.Length == 1)
{
    // returning the same token since it already has the correct format
    sessionTokenInSimpleFormat = sessionTokenToConvert;
}
else
{
    long version = 0;
    long globalLSN = 0;

    if (!long.TryParse(sessionTokenSegments[0], out version)
        || !long.TryParse(sessionTokenSegments[1], out globalLSN))
    {
        throw new ArgumentException("Invalid session token format", sessionTokenToConvert);
    }

    sessionTokenInSimpleFormat = string.Format("{0}:{1}", items[0], globalLSN);
}
```

## <a name="convert-to-vector-session-token"></a>Vektör oturum belirtecine Dönüştür

.NET SDK v2 kullanarak bir oturum belirtecini istemciye geçirmek için **vektör** oturum belirteci biçimini kullanın.  Örneğin, dönüştürmek için aşağıdaki örnek kodu kullanın.

```csharp

private static readonly char[] SegmentSeparator = (new[] { '#' });
private static readonly char[] PkRangeSeparator = (new[] { ':' });

// sessionTokenToConvert = session token from previous response
string[] items = sessionTokenToConvert.Split(PkRangeSeparator, StringSplitOptions.RemoveEmptyEntries);
string[] sessionTokenSegments = items[1].Split(SegmentSeparator, StringSplitOptions.RemoveEmptyEntries);

string sessionTokenInVectorFormat;

if (sessionTokenSegments.Length == 1)
{
    long globalLSN = 0;
    if (long.TryParse(sessionTokenSegments[0], out globalLSN))
    {
        sessionTokenInVectorFormat = string.Format("{0}:-2#{1}", items[0], globalLSN);
    }
    else
    {
        throw new ArgumentException("Invalid session token format", sessionTokenToConvert);
    }
}
else
{
    // returning the same token since it already has the correct format
    sessionTokenInVectorFormat = sessionTokenToConvert;
}
```

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makaleleri okuyun:

* [Azure Cosmos DB tutarlılığı yönetmek için oturum belirteçlerini kullanın](how-to-manage-consistency.md#utilize-session-tokens)
* [Azure Cosmos DB doğru tutarlılık düzeyini seçin](./consistency-levels.md)
* [Azure Cosmos DB tutarlılık, kullanılabilirlik ve performans avantajları](./consistency-levels.md)
* [Çeşitli tutarlılık düzeyleri için kullanılabilirlik ve performans avantajları](./consistency-levels.md)