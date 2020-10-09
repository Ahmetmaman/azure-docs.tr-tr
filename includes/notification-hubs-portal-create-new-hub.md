---
title: dosya dahil etme
description: dosya dahil etme
services: notification-hubs
author: jwargo
ms.service: notification-hubs
ms.topic: include
ms.date: 01/17/2019
ms.author: jowargo
ms.custom: include file
ms.openlocfilehash: 5afcc8e4524a0e8353766ba239d5ab9161b29d86
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "67509134"
---
1. [Azure Portal](https://portal.azure.com)’ında oturum açın.

1. Sol taraftaki menüden **tüm hizmetler** ' i seçin ve ardından **Mobil** bölümünde **Notification Hubs** ' yi seçin. Hizmet adının yanındaki yıldız simgesini seçerek hizmeti Sol menüdeki **Sık Kullanılanlar** bölümüne ekleyin. **Sık kullanılanlara** **Notification Hubs** ekledikten sonra, soldaki menüden seçin.

      ![Azure portal Notification Hubs seçin](./media/notification-hubs-portal-create-new-hub/all-services-select-notification-hubs.png)

1. **Notification Hubs** sayfasında, araç çubuğunda **Ekle** ' yi seçin.

      ![Notification Hubs-araç çubuğu düğmesi Ekle](./media/notification-hubs-portal-create-new-hub/add-toolbar-button.png)

1. **Bildirim Hub** 'ı sayfasında, aşağıdaki adımları uygulayın:

    1. **Bildirim Hub 'ına**bir ad girin.  

    1. **Yeni ad alanı oluştur**' a bir ad girin. Bir ad alanı bir veya daha fazla hub içerir.

    1. **Konum** açılan liste kutusundan bir değer seçin. Bu değer, hub 'ı oluşturmak istediğiniz konumu belirtir.

    1. **Kaynak grubunda**var olan bir kaynak grubunu seçin veya yeni bir kaynak grubu için bir ad oluşturun.

    1. **Oluştur**’u seçin.

        ![Azure portalı - Bildirim hub'ı özelliklerini ayarlama](./media/notification-hubs-portal-create-new-hub/notification-hubs-azure-portal-settings.png)

1. **Bildirimleri** (zil simgesi) seçin ve ardından **Kaynağa Git**' i seçin. Ayrıca **Notification Hubs** sayfasında listeyi yenileyebilir ve hub 'ınızı seçebilirsiniz.

      ![Azure portalı - bildirimler -> Kaynağa git](./media/notification-hubs-portal-create-new-hub/go-to-notification-hub.png)

1. Listeden **Erişim İlkeleri**'ni seçin. İki bağlantı dizesinin sizin için kullanılabilir olduğunu unutmayın. Anında iletme bildirimlerini işlemek için onlara daha sonra ihtiyacınız olacak.

      >[!IMPORTANT]
      >Uygulamanızda **Defaultfullsharedaccesssignature** *ilkesini kullanmayın.* Bu, yalnızca arka uçta kullanılmak üzere tasarlanmıştır.
      >

      ![Azure portalı - Bildirim hub'ı bağlantı dizeleri](./media/notification-hubs-portal-create-new-hub/notification-hubs-connection-strings-portal.png)
