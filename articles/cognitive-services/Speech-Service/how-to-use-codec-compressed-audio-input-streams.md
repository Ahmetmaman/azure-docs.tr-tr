---
title: Konuşma SDK-konuşma hizmeti ile sıkıştırılmış ses codec bileşeni
titleSuffix: Azure Cognitive Services
description: Konuşma SDK 'Sı ile konuşma hizmetine sıkıştırılmış ses akışını nasıl sağlayacağınızı öğrenin. Linux için C++, C# ve Java, Android 'de Java ve amaç-C için kullanılabilir.
services: cognitive-services
author: amitkumarshukla
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/30/2020
ms.author: amishu
ms.custom: devx-track-csharp
zone_pivot_groups: programming-languages-set-twenty-two
ms.openlocfilehash: 4585538e552e73f8f7a4b7b105153a9d26eeb4c4
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "88934114"
---
# <a name="use-codec-compressed-audio-input-with-the-speech-sdk"></a>Konuşma SDK 'Sı ile codec ile sıkıştırılmış ses girişi kullanma

Konuşma hizmeti SDK 'Sı **sıkıştırılmış ses girişi akış** API 'si, veya kullanarak konuşma hizmetine sıkıştırılmış ses akışını sağlamanın bir yolunu sağlar `PullStream` `PushStream` .

Windows üzerinde C#, C++, Java ve Python için akış sıkıştırılmış giriş sesi şu anda desteklenmektedir (UWP uygulamaları desteklenmez) ve Linux (Ubuntu 16,04, Ubuntu 18,04, de, 9, RHEL 7/8, CentOS 7/8). Android 'de Java için de desteklenir.
* RHEL 8 ve CentOS 8 için konuşma SDK sürümü 1.10.0 veya üzeri gereklidir
* Windows için konuşma SDK sürümü 1.11.0 veya üzeri gereklidir.

[!INCLUDE [supported-audio-formats](includes/supported-audio-formats.md)]

## <a name="prerequisites"></a>Ön koşullar

::: zone pivot="programming-language-csharp"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/csharp/prerequisites.md)]
::: zone-end

::: zone pivot="programming-language-cpp"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/cpp/prerequisites.md)]
::: zone-end

::: zone pivot="programming-language-java"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/java/prerequisites.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/python/prerequisites.md)]
::: zone-end

## <a name="example-code-using-codec-compressed-audio-input"></a>Codec ile sıkıştırılmış ses girişini kullanan örnek kod

::: zone pivot="programming-language-csharp"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/csharp/examples.md)]
::: zone-end

::: zone pivot="programming-language-cpp"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/cpp/examples.md)]
::: zone-end

::: zone pivot="programming-language-java"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/java/examples.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/python/examples.md)]
::: zone-end

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Konuşmayı tanımayı öğrenin](quickstarts/speech-to-text-from-microphone.md)
