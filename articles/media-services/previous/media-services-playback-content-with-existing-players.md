---
title: İçeriğinizi kayıttan yürütmek için mevcut oyuncuları kullanın-Azure | Microsoft Docs
description: Bu makalede, içeriğinizi kayıttan yürütmek için kullanabileceğiniz mevcut oynatıcılar listelenmektedir.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.assetid: 7e9fcf89-0fb6-4fa4-96cb-666320684d69
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/10/2021
ms.author: inhenkel
ms.openlocfilehash: ce773adace1baf6db8f1b747643ef0ffdc56a97c
ms.sourcegitcommit: 225e4b45844e845bc41d5c043587a61e6b6ce5ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/11/2021
ms.locfileid: "103008304"
---
# <a name="playing-your-content-with-existing-players"></a>İçeriğinizi mevcut oynatıcılarda oynatma

[!INCLUDE [media services api v2 logo](./includes/v2-hr.md)]

Azure Media Services, Kesintisiz Akış, HTTP Canlı Akışı ve MPEG-Dash gibi birçok popüler akış biçimini destekler. Bu konu, akışlarınızı test etmek için kullanabileceğiniz mevcut oyunculara işaret eder.

## <a name="the-azure-portal-media-services-content-player"></a>Azure portal Media Services içerik yürütücüsü

**Azure** Portal, videonuzu test etmek için kullanabileceğiniz bir içerik oynatıcı sağlar.

İstediğiniz videoya tıklayın ( [yayımlanmış](media-services-portal-publish.md)olduğundan emin olun) ve portalın altındaki **oynat** düğmesine tıklayın.

Bazı dikkate alınması gereken noktalar vardır:

* **MEDYA HİZMETLERİ İÇERİK OYNATICISI** varsayılan akış uç noktasından oynatır. Varsayılan dışı bir akış uç noktasından oynatmak istiyorsanız başka bir oynatıcı kullanın. Örneğin, [Azure Media Player](https://aka.ms/azuremediaplayer).

### <a name="azure-media-player"></a>Azure Media Player

İçeriğinizi (şifresiz veya korumalı) aşağıdaki biçimlerden birinde kayıttan yürütmek için [Azure Media Player](https://aka.ms/azuremediaplayer) kullanın:

* Kesintisiz Akış
* MPEG DASH
* HLS
* İlerleyen MP4

### <a name="flash-player"></a>Flash oynatıcı

#### <a name="playready-with-token"></a>Belirteç ile PlayReady

[http://reference.dashif.org/dash.js/nightly/samples/dash-if-reference-player/index.html](http://reference.dashif.org/dash.js/nightly/samples/dash-if-reference-player/index.html)

### <a name="dash-players"></a>DASH çalarlar

[kesik çizgi oynatıcı](http://players.akamai.com/players/dashjs)

[https://dashif.org](https://dashif.org)

### <a name="other"></a>Diğer

HLS URL 'Leri test etmek için aşağıdakileri de kullanabilirsiniz:

* İOS cihazında **Safari** veya
* Windows üzerinde **3ivx HLS oynatıcı** .

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geribildirim gönderme

[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]
