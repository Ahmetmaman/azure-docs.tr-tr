---
title: dosya dahil etme
description: dosya dahil etme
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 11/04/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 6f910dbe91ed8e1cb65eefa6dfc48c72a689bf25
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "73491595"
---
1. Oluşturduğunuz sanal WAN 'ı bulun. Sanal WAN sayfasında, **bağlantı** bölümünde, **hub**' ı seçin.
2. Hub 'Lar sayfasında **+ yeni hub** ' ı seçerek **sanal hub oluştur** sayfasını açın.
3. **Sanal hub** sayfası **temel bilgileri** Oluştur sekmesinde aşağıdaki alanları doldurun:

   ![Temel Bilgiler](./media/virtual-wan-tutorial-er-hub-include/hub1.png "Temel Bilgiler")

    **Proje ayrıntıları**

   * Bölge (daha önce konum olarak adlandırılır)
   * Name
   * Hub özel adres alanı. En küçük adres alanı bir hub oluşturmak için/24 ' tir. Bu,/25 ile/32 arasında herhangi bir aralığın oluşturulması sırasında bir hata üretecektir.
4. **ExpressRoute sekmesini**seçin.

5. **ExpressRoute** sekmesinde aşağıdaki alanları doldurun:

   ![ExpressRoute](./media/virtual-wan-tutorial-er-hub-include/hub2.png "ExpressRoute")

   * **ExpressRoute** ağ geçidi oluşturmak için **Evet** ' i seçin.
   * Açılan menüden **ağ geçidi ölçek birimleri** değerini seçin.
6. Doğrulamak için **gözden geçir + oluştur** ' u seçin.
7. Hub 'ı oluşturmak için **Oluştur** ' u seçin. 30 dakika **sonra hub 'ı bir sayfada görüntülemek** için **yenileyin** . Kaynağa gitmek için **Kaynağa Git** ' i seçin.