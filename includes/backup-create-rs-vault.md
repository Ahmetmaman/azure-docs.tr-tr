---
title: include dosyası
description: include dosyası
services: backup
author: markgalioto
ms.service: backup
ms.topic: include
ms.date: 5/14/2018
ms.author: markgal
ms.custom: include file
ms.openlocfilehash: 0ff9bf829e2dbe1bca078360ccded94bad63d9a6
ms.sourcegitcommit: 156364c3363f651509a17d1d61cf8480aaf72d1a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39249470"
---
## <a name="create-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası oluşturma
Kurtarma Hizmetleri kasası, yedeklemeleri ve zaman içinde oluşturulan kurtarma noktalarını depolayan bir varlıktır. Kurtarma Hizmetleri kasası Ayrıca, korunan sanal makinelerle ilişkili yedekleme ilkeleri içerir.

Kurtarma Hizmetleri kasası oluşturmak için:

1. Oturum açmak için aboneliğinizde [Azure portalında](https://portal.azure.com/).

2. Sol menüden **tüm hizmetleri**.

    ![Tüm hizmetleri seçin](./media/backup-create-rs-vault/click-all-services.png)

3. İçinde **tüm hizmetleri** iletişim kutusuna **kurtarma Hizmetleri**. Kaynakların listesini girişinizi göre filtreler. Kaynak listesinde seçin **kurtarma Hizmetleri kasaları**.

    ![Girin ve kurtarma Hizmetleri kasaları seçin](./media/backup-create-rs-vault/all-services.png)

    Abonelikte kurtarma Hizmetleri kasalarının listesi görünür.
    
4. Üzerinde **kurtarma Hizmetleri kasaları** panoyu seçin **Ekle**.

    ![Bir kurtarma Hizmetleri kasası Ekle](./media/backup-create-rs-vault/add-button-create-vault.png)

    **Kurtarma Hizmetleri kasası** iletişim kutusu açılır. İçin değerler sağlayın **adı**, **abonelik**, **kaynak grubu**, ve **konumu**.

    ![Kurtarma Hizmetleri kasasını yapılandırma](./media/backup-create-rs-vault/create-new-vault-dialog.png)

   - **Ad**: kasayı tanımlamak için bir kolay ad girin. Adın Azure aboneliği için benzersiz olmalıdır. En az iki, ancak 50'den fazla karakter içeren bir ad belirtin. Adı bir harf ile başlamalı ve yalnızca harf, rakam ve kısa çizgi oluşur.
   - **Abonelik**: kullanılacak aboneliği seçin. Yalnızca bir abonelik üyesi değilseniz, bu adı görürsünüz. Hangi aboneliğin emin değilseniz varsayılan (önerilen) aboneliği kullanın. Çalışmanızı birden çok seçenek eksikse veya Okul hesabı birden fazla Azure aboneliği ile ilişkili olduğunda.
   - **Kaynak grubu**: mevcut bir kaynak grubunu kullanın veya yeni bir tane oluşturun. Aboneliğinizdeki kullanılabilir kaynak grupları listesini görmek için seçin **var olanı kullan**ve sonra aşağı açılan liste kutusundan bir kaynak seçin. Yeni bir kaynak grubu oluşturmak için Seç **Yeni Oluştur** ve adını girin. Kaynak grupları hakkında eksiksiz bilgiler için bkz. [Azure Resource Manager'a genel bakış](../articles/azure-resource-manager/resource-group-overview.md).
   - **Konum**: kasa için coğrafi bölgeyi seçin. Sanal makineleri, kasa korumak için bir kasa oluşturmak için **gerekir** sanal makinelerle aynı bölgede olmalıdır.

      > [!IMPORTANT]
      > Sanal makinenizin konumunu emin değilseniz, iletişim kutusunu kapatın. Portaldaki sanal makineler listesine gidin. Birçok bölgede sanal makineniz varsa her bölgede bir kurtarma Hizmetleri kasası oluşturun. Kasa için başka bir konuma oluşturmadan önce kasayı ilk konumda oluşturun. Yedekleme verilerini depolamak için depolama hesaplarının belirtilmesi gerek yoktur. Kurtarma Hizmetleri kasası ve Azure Backup hizmeti, otomatik olarak işler.
      >
      >

5. Kurtarma Hizmetleri kasası oluşturmak hazır olduğunuzda **Oluştur**.

    ![Kurtarma Hizmetleri kasası oluşturma](./media/backup-create-rs-vault/click-create-button.png)

    Bu, Kurtarma Hizmetleri kasası oluşturmak için biraz zaman alabilir. Durum bildirimlerini izleyin **bildirimleri** portalının sağ üst köşesindeki alan. Kasanız oluşturulduktan sonra kurtarma Hizmetleri kasaları listesinde görünür. Kasanız görmüyorsanız seçin **Yenile**.

     ![Yedekleme kasalarının listesi Yenile](./media/backup-create-rs-vault/refresh-button.png)
