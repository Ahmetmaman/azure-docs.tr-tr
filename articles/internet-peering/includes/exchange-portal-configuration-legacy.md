---
title: include dosyası
titleSuffix: Azure
description: include dosyası
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: include
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: b8869e93a7156b24d61ac555c95b9ca7f850ae34
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "81678532"
---
1. **Eşleme oluştur** sayfasında, **yapılandırma** sekmesinde, kutulara aşağıda gösterildiği gibi kutuyu girin.

    > [!div class="mx-imgBorder"]
    > ![Eşleme sayfası değişim eşleme türü oluştur](../media/setup-exchange-conf-tab.png)

    * **Eşleme türü** için **Exchange**' i seçin.
    * **SKU 'Yu** **temel ücretsiz** olarak seçin.
    * Eşlemeyi bir Azure kaynağına dönüştürmek istediğiniz **Metro** konumunu seçin. Azure kaynağına dönüştürülmemiş seçili **Metro** konumunda Microsoft ile eşleme bağlantılarınız varsa, bu bağlantılar gösterilen şekilde **eşleme bağlantıları** bölümünde listelenecektir. Artık bu eşleme bağlantılarını bir Azure kaynağına dönüştürebilirsiniz.

        > [!div class="mx-imgBorder"]
        > ![Eşleme bağlantıları listesi](../media/setup-exchange-legacy-conf-tab.png)

        > [!NOTE]
        > Eski eşleme bağlantılarının ayarlarını değiştiremezsiniz. Seçili **Metro** konumunda Microsoft ile ek eşleme bağlantıları eklemek Istiyorsanız **Yeni oluştur**' u seçin. Daha fazla bilgi için bkz. [portalı kullanarak bir Exchange eşlemesi oluşturma veya değiştirme](../howto-exchange-portal.md).
        >

1. **Gözden geçir ve oluştur**’u seçin. Portalın girdiğiniz bilgilerin temel doğrulamasını çalıştırdığına dikkat edin. Üstteki bir şerit *son doğrulama çalıştıran iletiyi görüntüler...*.

    > [!div class="mx-imgBorder"]
    > ![Eşleme doğrulama sekmesi](../media/setup-direct-review-tab-validation.png)

1. *Doğrulama başarılı* olduktan sonra, bilgilerinizi doğrulayın. **Oluştur** seçeneğini belirleyerek isteği gönderebilirsiniz. İsteğinizi değiştirmeniz gerekiyorsa, **önceki** seçeneğini belirleyip adımları yineleyin.

    > [!div class="mx-imgBorder"]
    > ![Eşleme gönderimi](../media/setup-exchange-review-tab-submit.png)

1. İsteği gönderdikten sonra, dağıtımın bitmesini bekleyin. Dağıtım başarısız olursa, [Microsoft eşleme](mailto:peering@microsoft.com)ile iletişime geçin. Başarılı bir dağıtım gösterildiği gibi görüntülenir.

    > [!div class="mx-imgBorder"]
    > ![Eşleme başarısı](../media/setup-direct-success.png)
