---
title: Azure Haritalar ile erişilebilir bir uygulama yapma | Microsoft Docs
description: Azure haritalar kullanan erişilebilir bir uygulama oluşturma
services: azure-maps
keywords: ''
author: chgennar
ms.author: chgennar
ms.date: 09/17/2018
ms.topic: article
ms.service: azure-maps
documentationcenter: ''
manager: timlt
ms.devlang: na
ms.openlocfilehash: 59e4226d7abb1d2692c531f146685feb203ab423
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46129446"
---
# <a name="building-an-accessible-application"></a>Erişilebilir bir uygulama oluşturma

Bu makalede, ekran okuyucu tarafından kullanılabilecek bir harita uygulaması oluşturma işlemini göstermektedir.

## <a name="understand-the-code"></a>Kodu anlama

<iframe height='500' scrolling='no' title='Erişilebilir bir uygulama yapın' src='//codepen.io/azuremaps/embed/ZoVyZQ/?height=504&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/ZoVyZQ/'>erişilebilir bir uygulama</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Harita erişilebilirlik özellikleri ile önceden oluşturulmuş gelir. Kullanıcılar klavye kullanarak haritada gidebilirsiniz. Ekran Okuyucu çalışıyorsa, eşleme durumunu yapılan değişikliklerin kullanıcıya bildirir.
Örneğin, haritayı veya yatay olarak kaydırıldığında uzaklaştırılacağını kullanıcılar Haritası değişikliklerini bildirilir. Temel harita üzerinde yerleştirilen ek bilgiler, ekran okuyucusu kullanıcıları için karşılık gelen metin tabanlı bilgiler olması gerekir.

Kullanarak [açılan](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest) bunu yapmanın bir yoludur. Yukarıdaki arama örnekte açılır penceresi ile metin tabanlı bilgiler harita üzerinde yerleştirilen her PIN eşlemesine eklenir. Kullanarak [açılan'ın](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest) [ekleme](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#attach) açılan görsel olarak haritada görüntülemeden ekran okuyucu tarafından görülebilmesi açılan yöntemi sağlar.

## <a name="next-steps"></a>Sonraki adımlar

Açılan pencere sınıfı ve bu makalede kullanılan yöntemlerinden hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Açılan menüsü](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest)

Daha fazla kod örneklerini gözden denetleyin:

> [!div class="nextstepaction"]
> [Kod örnek sayfası](http://aka.ms/AzureMapsSamples)
