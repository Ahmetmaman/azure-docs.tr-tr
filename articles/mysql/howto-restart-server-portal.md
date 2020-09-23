---
title: Sunucuyu yeniden Başlat-Azure portal-MySQL için Azure veritabanı
description: Bu makalede, Azure portal kullanarak MySQL için Azure veritabanı sunucusunu nasıl yeniden başlatabileceğinizi açıklar.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: how-to
ms.date: 3/18/2020
ms.openlocfilehash: d885cc64eeebd4873ad5993b39b48845d1365c23
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90902753"
---
# <a name="restart-azure-database-for-mysql-server-using-azure-portal"></a>Azure portal kullanarak MySQL için Azure veritabanı sunucusunu yeniden başlatma
Bu konuda, MySQL sunucusu için Azure veritabanı 'nı nasıl yeniden başlatabileceğinizi açıklanmaktadır. Sunucu işlemi gerçekleştirdiğinden kısa süreli bir kesinti oluşmasına neden olan bakım nedenleriyle sunucunuzu yeniden başlatmanız gerekebilir.

Hizmet meşgulse sunucu yeniden başlatması engellenir. Örneğin, hizmet, sanal çekirdekleri ölçeklendirirken daha önce istenen bir işlemi işliyor olabilir.

Yeniden başlatmayı tamamlaması için gereken süre MySQL kurtarma işlemine bağlıdır. Yeniden başlatma süresini azaltmak için, yeniden başlatmadan önce sunucuda gerçekleşen etkinlik miktarını en aza indirmenizi öneririz.

## <a name="prerequisites"></a>Önkoşullar
Bu nasıl yapılır kılavuzunu tamamlayabilmeniz için şunlar gerekir:
- [MySQL Için Azure veritabanı sunucusu](quickstart-create-mysql-server-database-using-azure-portal.md)

## <a name="perform-server-restart"></a>Sunucu yeniden başlatması gerçekleştir

Aşağıdaki adımlar MySQL sunucusunu yeniden başlatır:

1. Azure portal, MySQL için Azure veritabanı sunucunuzu seçin.

2. Sunucunun **genel bakış** sayfasının araç çubuğunda **Yeniden Başlat**' a tıklayın.

   :::image type="content" source="./media/howto-restart-server-portal/2-server.png" alt-text="MySQL için Azure veritabanı-genel bakış-yeniden Başlat düğmesi":::

3. Sunucuyu yeniden başlatmayı onaylamak için **Evet** ' e tıklayın.

   :::image type="content" source="./media/howto-restart-server-portal/3-restart-confirm.png" alt-text="MySQL için Azure veritabanı-yeniden başlatma Onayla":::

4. Sunucu durumunun "yeniden başlatılıyor" olarak değiştiğini gözlemleyin.

   :::image type="content" source="./media/howto-restart-server-portal/4-restarting-status.png" alt-text="MySQL için Azure veritabanı-yeniden başlatma durumu":::

5. Sunucu yeniden başlatma işleminin başarılı olduğunu onaylayın.

   :::image type="content" source="./media/howto-restart-server-portal/5-restart-success.png" alt-text="MySQL için Azure veritabanı-yeniden başlatma başarılı":::

## <a name="next-steps"></a>Sonraki adımlar

[Hızlı başlangıç: Azure portal kullanarak MySQL için Azure veritabanı sunucusu oluşturma](./quickstart-create-mysql-server-database-using-azure-portal.md)
