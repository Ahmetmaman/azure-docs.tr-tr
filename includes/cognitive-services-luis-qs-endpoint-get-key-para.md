---
title: include dosyası
description: include dosyası
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: luis
ms.topic: include
ms.custom: include file
ms.date: 08/16/2018
ms.author: diberry
ms.openlocfilehash: fcf3bf29e2cdf89bdc93c7ebac313e5d9a9c18f0
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47044041"
---
Tahmin uç noktasına erişim bir uç noktası anahtarı ile sağlanır. Bu hızlı başlangıçta LUIS hesabınızla ilişkili ücretsiz başlangıç anahtarını kullanın. 
 
1. LUIS hesabınızı kullanarak oturum açın. 

    [![Dil Anlama (LUIS) uygulama listesinin ekran görüntüsü](media/cognitive-services-luis/app-list.png "Dil Anlama (LUIS) uygulama listesinin ekran görüntüsü")](media/cognitive-services-luis/app-list.png)

2. Sağ üst köşedeki menüden adınızı, ardından **Ayarlar**'ı seçin.

    ![LUIS kullanıcı ayarları menüsüne erişime](media/cognitive-services-luis/get-user-settings-in-luis.png)

3. **Yazarlık anahtarı**'nın değerini kopyalayın. Bunun hızlı başlangıçta ileride kullanacaksınız. 

    [![Dil Anlama (LUIS) kullanıcı ayarlarının ekran görüntüsü](media/cognitive-services-luis/get-user-authoring-key.png "Dil Anlama (LUIS) kullanıcı ayarlarının ekran görüntüsü")](media/cognitive-services-luis/get-user-authoring-key.png)

    Yazarlık anahtarı tüm LUIS uygulamalarınız için ücretsiz olarak yazarlık API'sine sınırsız istek ve tahmin uç noktası API'sine ayda 1000'e kadar sorgu göndermenize izin verir. <!--Once the prediction endpoint quota from the authoring key is used for the month, you need to create a **Language Understanding** key from the Azure portal. The key created in the portal is known as the endpoint key. The endpoint key is used _only_ for prediction endpoint queries.-->