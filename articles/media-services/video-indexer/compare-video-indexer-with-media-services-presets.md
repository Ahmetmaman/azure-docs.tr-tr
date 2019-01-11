---
title: Video Indexer'ı ve Azure Media Services v3 hazır karşılaştırması | Microsoft Docs
description: Bu konuda, Video Indexer'ı ve Azure Media Services v3 hazır karşılaştırır.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/03/2018
ms.author: juliako
ms.openlocfilehash: 968614d183fe6857336ea92791ba4f1d279a5016
ms.sourcegitcommit: d4f728095cf52b109b3117be9059809c12b69e32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2019
ms.locfileid: "54198108"
---
# <a name="compare-azure-media-services-v3-presets-and-video-indexer"></a>Azure Media Services v3 hazır ve Video Indexer'ı karşılaştırma 

Bu makalede yeteneklerini karşılaştırır **Video Indexer API** ve **Media Services v3 API'ler**. 

Şu anda, sunduğu özellikler arasında bir çakışma var. [Video Indexer v2 API'leri](https://api-portal.videoindexer.ai/) ve [Media Services v3 API'ler](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/mediaservices/resource-manager/Microsoft.Media/stable/2018-07-01/Encoding.json). Aşağıdaki tabloda benzerlikleri ve farkları anlamak için geçerli bir kılavuz sağlar. 

## <a name="compare"></a>Karşılaştır

|Özellik|Video Indexer API |Video Çözümleyicisi ve ses Çözümleyicisi hazır<br/>Media Services v3 API'lerindeki|
|---|---|---|
|Medya öngörüleri|[Gelişmiş](video-indexer-output-json-v2.md) |[Temel Konular](../latest/intelligence-concept.md)|
|Deneyimleri|Desteklenen özelliklerin tam listesine bakın: <br/> [Genel Bakış](video-indexer-overview.md)|Video içgörüleri yalnızca döndürür|
|Faturalandırma|[Media Services fiyatlandırma](https://azure.microsoft.com/pricing/details/media-services/#analytics)|[Media Services fiyatlandırma](https://azure.microsoft.com/pricing/details/media-services/#analytics)|
|Uyumluluk|[Azure uyumluluk](https://aka.ms/AzureCompliance)|Media Services, birçok sertifikaları ile uyumludur. Kullanıma [Azure uyumluluk Offerings.pdf](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942/file/178110/23/Microsoft%20Azure%20Compliance%20Offerings.pdf) ve ilgilendiğiniz bir sertifika ile uyumlu olmadığını görmek "Media Services" için arama yapın.|
|Ücretsiz Deneme|Doğu ABD|Kullanılamaz|
|Kullanılabilirlik |Batı ABD, Doğu Asya, Kuzey Avrupa|Bkz: [Azure durumu](https://azure.microsoft.com/global-infrastructure/services/?products=media-services).|

## <a name="next-steps"></a>Sonraki adımlar

[Video Indexer’a genel bakış](video-indexer-overview.md)

[Media Services v3 genel bakış](../latest/media-services-overview.md)
