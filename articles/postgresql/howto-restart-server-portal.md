---
title: Azure portalını kullanarak PostgreSQL sunucusu için Azure veritabanını yeniden başlatın
description: Bu makalede, Azure portalını kullanarak PostgreSQL için Azure veritabanı nasıl yeniden açıklanır.
author: ajlam
ms.author: andrela
ms.service: postgresql
ms.topic: conceptual
ms.date: 3/18/2019
ms.openlocfilehash: 83947571ed835c53c6cf8da6e73deb8dceabdd62
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2019
ms.locfileid: "58168784"
---
# <a name="restart-azure-database-for-postgresql-server-using-the-azure-portal"></a>Azure portalını kullanarak PostgreSQL sunucusu için Azure veritabanını yeniden başlatın
Bu konuda, PostgreSQL sunucusu için Azure veritabanı nasıl yeniden açıklanmaktadır. Sunucu işlemi gerçekleştirirken, kısa bir kesintiye neden sunucunuzun bakım nedeniyle yeniden başlatmanız gerekebilir.

Sunucunun yeniden başlatılması, Hizmet meşgul olduğunda engellenir. Örneğin, hizmet sanal çekirdekler ölçeklendirme gibi daha önce istenen bir işlemin işliyor olabilir.
 
Yeniden başlatma tamamlamak için gereken süreyi PostgreSQL kurtarma işlemi bağlıdır. Yeniden başlatma süresini azaltmak için yeniden başlatma öncesinde sunucusunda gerçekleşen etkinliği miktarını en aza öneririz.

## <a name="prerequisites"></a>Önkoşullar
Bu nasıl yapılır kılavuzunda tamamlanması gerekir:
- Bir [PostgreSQL sunucusu ve veritabanı için Azure veritabanı](quickstart-create-server-database-portal.md)

## <a name="perform-server-restart"></a>Sunucunun yeniden başlatılmasını gerçekleştirme

Aşağıdaki adımlar, PostgreSQL sunucuyu yeniden başlatın:

1. İçinde [Azure portalında](https://portal.azure.com/), PostgreSQL için Azure veritabanı sunucunuza seçin.

2. Sunucunun araç **genel bakış** sayfasında **yeniden**.

   ![PostgreSQL - genel bakış - yeniden Başlat düğmesi için Azure veritabanı](./media/howto-restart-server-portal/2-server.png)

3. Tıklayın **Evet** sunucuyu yeniden başlatmadan onaylamak için.

   ![-PostgreSQL için Azure veritabanı, yeniden başlatma işlemini onaylayın](./media/howto-restart-server-portal/3-restart-confirm.png)

4. Sunucu durumu "Yeniden için" değiştiğini gözlemleyin.

   ![PostgreSQL - yeniden başlatma durumu için Azure veritabanı](./media/howto-restart-server-portal/4-restarting-status.png)

5. Sunucunun yeniden başlatılması başarılı olduğunu onaylayın.

   ![-Yeniden başlatma başarılı olan PostgreSQL için Azure veritabanı](./media/howto-restart-server-portal/5-restart-success.png)

## <a name="next-steps"></a>Sonraki adımlar

Hakkında bilgi edinin [nasıl PostgreSQL için Azure veritabanı'nda parametrelerini ayarla](howto-configure-server-parameters-using-portal.md)