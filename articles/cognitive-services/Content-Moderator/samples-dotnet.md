---
title: Kod örnekleri - Content Moderator, .NET
description: Content Moderator SDK'sı aracılığıyla .NET uygulamaları kullanın.
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: sample
ms.date: 01/10/2019
ms.author: sajagtap
ms.openlocfilehash: 43aa1ca624bce78fa113c34fa19da9ccfea69bbc
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55222108"
---
# <a name="content-moderator-net-sdk-samples"></a>Content Moderator .NET SDK örnekleri

Aşağıdaki listede .NET için Azure Content Moderator SDK'sı kullanılarak derlenen kod örneklerine bağlantılar bulunmaktadır.

- **Yardımcı kitaplık**: [Content Moderator istemci kullanım için diğer örnekleri oluşturma](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ModeratorHelper/Clients.cs). Bkz: [hızlı başlangıç](content-moderator-helper-quickstart-dotnet.md).

## <a name="moderation"></a>Denetleme

- **Görüntü denetimi**: [Yetişkinlere yönelik ve müstehcen içeriğin denetimi, metin ve yüz için bir görüntü değerlendirmek](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageModeration/Program.cs). Bkz: [hızlı başlangıç](image-moderation-quickstart-dotnet.md).
- **Özel görüntüleri**: [Özel görüntü listeleriyle orta](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageListManagement/Program.cs). Bkz: [hızlı başlangıç](image-lists-quickstart-dotnet.md).

> [!NOTE]
> Liste sayısı üst sınırı, her biri **10.000 görüntüyü aşmamak** kaydıyla **5 görüntü listesidir**.
>

- **Metin denetimi**: [Ekran küfür ve kişisel bilgileri (PII) metin](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/TextModeration/Program.cs). Bkz: [hızlı başlangıç](text-moderation-quickstart-dotnet.md).
- **Özel terimleri**: [Özel terim listeleri ile orta](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/TermListManagement/Program.cs). Bkz: [hızlı başlangıç](term-lists-quickstart-dotnet.md).

> [!NOTE]
> En çok **5 terim listeniz** olabilir ve her listedeki **terimlerin sayısı 10.000'i aşmamalıdır**.
>

- **Video denetimi**: [Video yetişkinlere yönelik ve müstehcen içeriğin denetimi için tarama ve sonuçları elde](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/VideoModeration/Program.cs). Bkz: [hızlı başlangıç](video-moderation-api.md).

## <a name="review"></a>Gözden geçirme

- **Görüntü işleri**: [Tarayan ve gözden geçirmeler oluşturan bir denetimi işi başlatmak](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageJobs/Program.cs). Bkz: [hızlı başlangıç](moderation-jobs-quickstart-dotnet.md).
- **Görüntü incelemeleri**: [İncelemeleri İnsan-de--döngü oluşturma](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageReviews/Program.cs). Bkz: [hızlı başlangıç](moderation-reviews-quickstart-dotnet.md).
- **Görüntü incelemeleri**: [Görüntü incelemeleri İnsan içinde--döngüsü için oluşturma](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/VideoReviews/Program.cs). Bkz: [hızlı başlangıç](video-reviews-quickstart-dotnet.md)
- **Video deşifre metni inceler**: [Video deşifre metni incelemeleri İnsan-de--döngü oluşturma](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/VideoTranscriptReviews/Program.cs) bkz [hızlı başlangıç](video-reviews-quickstart-dotnet.md)

[GitHub'daki Content Moderator .NET örnekleri](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) arasında tüm .NET örneklerine bakın.
