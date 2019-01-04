---
title: Oluşturma ve MySQL için Azure veritabanı'nda salt okunur çoğaltmalar yönetme
description: Bu makalede, ayarlama ve portalını kullanarak MySQL için Azure veritabanı'nda salt okunur çoğaltmalar yönetmek açıklar.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 8e622a11c489618cf66e9cdddf369309e7188645
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/17/2018
ms.locfileid: "53548026"
---
# <a name="how-to-create-and-manage-read-replicas-in-azure-database-for-mysql-using-the-azure-portal"></a>Nasıl oluşturmak ve yönetmek, Azure portalını kullanarak MySQL için Azure veritabanı çoğaltmalarını okuyun

Bu makalede, oluşturmak ve yönetmek için aynı Azure bölgesindeki Yöneticisi olarak Azure portalını kullanarak MySQL hizmeti için Azure veritabanı salt okunur çoğaltmalar öğreneceksiniz. Bu özellik şu anda genel Önizleme aşamasındadır.

## <a name="prerequisites"></a>Önkoşullar

- Bir [MySQL sunucusu için Azure veritabanı](quickstart-create-mysql-server-database-using-azure-portal.md) ana sunucu olarak kullanılır.

> [!IMPORTANT]
> Salt okunur çoğaltma özelliği yalnızca için Azure veritabanı genel amaçlı veya bellek için iyileştirilmiş fiyatlandırma katmanları MySQL sunucuları için kullanılabilir. Bu fiyatlandırma katmanlarından birini ana sunucusu olduğundan emin olun.

## <a name="create-a-read-replica"></a>Salt okunur bir çoğaltma oluşturma

Salt okunur çoğaltma sunucusu, aşağıdaki adımları kullanarak oluşturulabilir:

1. [Azure portal](https://portal.azure.com/) oturum açın.

2. Bir şablonu kullanmak istediğiniz bir MySQL sunucusu için mevcut Azure veritabanını seçin. Bu eylem açar **genel bakış** sayfası.

3. Seçin **çoğaltma** menüsünden altında **ayarları**.

4. Seçin **çoğaltma ekleme**.

   ![MySQL - çoğaltma için Azure veritabanı ](./media/howto-read-replica-portal/add-replica.png)

5. Çoğaltma sunucusu için bir ad girin ve tıklayın **Tamam** oluşturma işlemini onaylamak için.

   ![Azure veritabanı - MySQL için çoğaltma oluşturma ](./media/howto-read-replica-portal/create-replica.png)

> [!NOTE]
> Okuma çoğaltmaları aynı sunucu yapılandırma yöneticisi olarak oluşturulur. Çoğaltma sunucusu yapılandırması, oluşturulduktan sonra değiştirilebilir. Çoğaltma sunucusunun yapılandırmasını çoğaltma ana ayak olduğundan emin olmak için ana daha eşit veya daha fazla değerlerinde tutulması gereken önerilir.

Çoğaltma sunucusu oluşturulduktan sonra bunu görüntülenebilir **çoğaltma** dikey penceresi.

   ![MySQL - liste çoğaltmalar için Azure veritabanı ](./media/howto-read-replica-portal/list-replica.png)

## <a name="stop-replication-to-a-replica-server"></a>Bir çoğaltma sunucusu için çoğaltma durdurma

> [!IMPORTANT]
> Bir sunucuya çoğaltma durdurma işlemi geri alınamaz. Bir ana ve çoğaltma arasında çoğaltmayı durdurdu sonra geri alınamaz. Çoğaltma sunucusu bir tek başına sunucu olur ve artık hem okuma hem de yazma işlemleri destekler. Bu sunucu bir yinelemeye yeniden yapılamıyor.

Bir ana ve Azure portalından bir çoğaltma sunucusu arasında çoğaltmayı durdurmak için aşağıdaki adımları kullanın:

1. Azure portalında, ana Azure veritabanınızı MySQL sunucusuna seçin. 

2. Seçin **çoğaltma** menüsünden altında **ayarları**.

3. Çoğaltma sunucusu için çoğaltma durdurma istediğiniz seçin.

   ![MySQL - durdurma çoğaltma sunucu için Azure veritabanı ](./media/howto-read-replica-portal/stop-replication-select.png)

4. Seçin **çoğaltmayı durdurma**.

   ![MySQL - durdurma çoğaltma için Azure veritabanı ](./media/howto-read-replica-portal/stop-replication.png)

5. Çoğaltma tıklayarak durdurmak istediğinizi onaylamak **Tamam**.

   ![-MySQL için Azure veritabanı çoğaltmasını doğrulayın ](./media/howto-read-replica-portal/stop-replication-confirm.png)

## <a name="delete-a-replica-server"></a>Çoğaltma sunucusunu Sil

Azure portalından bir salt okunur çoğaltma sunucusunu silmek için aşağıdaki adımları kullanın:

1. Azure portalında, ana Azure veritabanınızı MySQL sunucusuna seçin.

2. Seçin **çoğaltma** menüsünden altında **ayarları**.

3. Silmek istediğiniz çoğaltma sunucusunu seçin.

   ![MySQL - Delete çoğaltma sunucu için Azure veritabanı ](./media/howto-read-replica-portal/delete-replica-select.png)

4. Seçin **çoğaltmayı Sil**

   ![MySQL - çoğaltmayı Sil'için Azure veritabanı ](./media/howto-read-replica-portal/delete-replica.png)

5. Adını tıklatın ve çoğaltma, **Sil** çoğaltmayı silme işlemini onaylamak için.  

   ![Çoğaltmayı Sil - MySQL için Azure veritabanı onaylayın ](./media/howto-read-replica-portal/delete-replica-confirm.png)

## <a name="delete-a-master-server"></a>Bir ana sunucu silme

> [!IMPORTANT]
> Ana sunucu silme tüm çoğaltma sunucuları için çoğaltma durdurulur ve ana sunucusunu siler. Artık hem okuma hem de yazma işlemleri destekleyen tek başına sunucular çoğaltma sunucusu olur.

Azure portalından bir ana sunucu silmek için aşağıdaki adımları kullanın:

1. Azure portalında, ana Azure veritabanınızı MySQL sunucusuna seçin.

2. Gelen **genel bakış**seçin **Sil**.

   ![MySQL - Delete ana için Azure veritabanı ](./media/howto-read-replica-portal/delete-master-overview.png)

3. Ana sunucunun adını yazın ve tıklayın **Sil** ana sunucu silme işlemini onaylamak için.  

   ![MySQL - Delete ana için Azure veritabanı ](./media/howto-read-replica-portal/delete-master-confirm.png)

## <a name="monitor-replication"></a>İzleyici çoğaltma

1. İçinde [Azure portalında](https://portal.azure.com/), izlemek istediğiniz MySQL sunucusu için Azure veritabanı çoğaltmayı seçin.

2. Altında **izleme** select yan bölümünü **ölçümleri**:

3. Seçin **çoğaltma bekleme süresini saniye cinsinden** ölçümlerin aşağı açılan listeden. 

   ![Çoğaltma gecikmesi seçin ](./media/howto-read-replica-portal/monitor-select-replication-lag.png)

4. Görüntülemek istediğiniz zaman aralığını seçin. Aşağıdaki resimde, 30 dakikalık bir zaman aralığı seçer.

   ![Zaman aralığı seçin ](./media/howto-read-replica-portal/monitor-replication-lag-time-range.png)

5. Seçili zaman aralığı için çoğaltma gecikmesi görüntüleyin. Aşağıdaki görüntüde, son 30 dakika görüntüler.

   ![Zaman aralığı seçin ](./media/howto-read-replica-portal/monitor-replication-lag-time-range-thirty-mins.png)

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinin [çoğaltmaları okuyun](concepts-read-replicas.md)