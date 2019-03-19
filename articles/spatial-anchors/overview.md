---
title: Azure uzamsal yer işaretleri genel bakış | Microsoft Docs
description: Uzamsal bağlayıcılarını Azure platformlar arası karma gerçeklik deneyimlerini geliştirmenize nasıl yardımcı olduğunu öğrenin.
author: craigktreasure
manager: aliemami
services: azure-spatial-anchors
ms.author: crtreasu
ms.date: 02/24/2019
ms.topic: overview
ms.service: azure-spatial-anchors
ms.openlocfilehash: 24a35b387a8b47d44f742303ddde0a0e8fb47fe6
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2019
ms.locfileid: "57833807"
---
# <a name="azure-spatial-anchors-overview"></a>Azure uzamsal yer işaretleri genel bakış

Azure uzamsal yer işaretlerine giden Hoş Geldiniz. Azure uzamsal bağlayıcılarını mekan kullanan karma gerçeklik uygulamaları oluşturmak için temel özelliklerini geliştiricilere güçlendirir. Bu uygulamalar Microsoft HoloLens, ARKit destekleyen iOS tabanlı cihazları ve Android tabanlı cihazlar ARCore destekleyen destekleyebilir. Azure uzamsal bağlayıcılarını geliştiricilerinin alanları algılar, ilgilenilen kesin noktaları belirlemek ve desteklenen cihazlardan ilgi bu noktaları çağırmak için karma gerçeklik platformları ile çalışmasını sağlar.
İlgilendiğiniz kesin şu noktaları uzamsal bağlantı olarak adlandırılır.

![Platformlar Arası](./media/cross-platform.png)

## <a name="examples"></a>Örnekler

Uzamsal bağlayıcılarını tarafından etkin bazı örnek kullanım alanları şunlardır:

- [Birden çok kullanıcı deneyimleri](tutorials/tutorial-share-anchors-across-devices.md). Uzamsal bağlayıcılarını kolaylaştırır kişilerin aynı yerde birden çok kullanıcı karma gerçeklik uygulamaları katılmak. Örneğin, iki kişinin bir tabloda bir sanal chess Panosu yerleştirerek karma gerçeklik satranç başlayabilirsiniz. Ardından, cihazlarını tablosunu işaret ederek görüntüleyebilir ve sanal chess panosu ile birlikte etkileşim.

- [Yol-bulma](concepts/anchor-relationships-way-finding.md). Geliştiriciler, uzamsal birlikte bunların arasında ilişki oluşturma Çıpalarını da bağlanabilirsiniz. Örneğin, bir uygulama, bir kullanıcı bir görevi tamamlamak için etkileşim kurması gereken ilgilenilen iki veya daha fazla noktaları olan bir deneyim içerebilir. Bu noktaları ilgi bağlı bir şekilde oluşturulabilir. Daha sonra kullanıcının çok adımlı görev tamamlarken, uygulama için geçerli bir sonraki adımda doğru kullanıcı görevi doğrudan yakın olan yer işaretleri sorabilirsiniz.

- [Kalıcı sanal gerçek içeriği](concepts/create-locate-anchors-unity.md#create-a-cloud-spatial-anchor). Bir uygulamayı sanal bir takvim, kişiler bir telefon uygulaması veya HoloLens cihaz kullanarak görebilir bir Konferans odası nu duvara yansıtabilir, yerleştirin kullanıcı izin verebilirsiniz. Endüstriyel bir ayarda, kullanıcı desteklenen cihaz kamerasına üzerine gelerek bir makine hakkında bağlamsal bilgi alabilir.

Yönetilen hizmet ve istemci SDK'ları desteklenen cihaz platformları için Azure uzamsal bağlayıcılarını oluşur. Aşağıdaki bölümler, Azure uzamsal bağlayıcılarını kullanarak uygulamaları oluşturma ile çalışmaya başlama hakkında bilgi sağlar.

## <a name="next-steps"></a>Sonraki adımlar

Uzamsal Çıpasıyla ilk uygulamanızı oluşturun.

> [!div class="nextstepaction"]
> [Unity](unity-overview.yml)

> [!div class="nextstepaction"]
> [iOS](quickstarts/get-started-ios.md)

> [!div class="nextstepaction"]
> [Android](quickstarts/get-started-android.md)

> [!div class="nextstepaction"]
> [HoloLens](quickstarts/get-started-hololens.md)
