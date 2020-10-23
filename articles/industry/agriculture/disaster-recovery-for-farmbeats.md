---
title: Farmtts için olağanüstü durum kurtarma
description: Bu makalede, veri kurtarmanın verilerinizi kaybetmekten nasıl koruduğu açıklanmaktadır.
author: uhabiba04
ms.topic: article
ms.date: 04/13/2020
ms.author: v-umha
ms.openlocfilehash: bdd59d078da1df021b7e23ec858ba7305f72cfa1
ms.sourcegitcommit: 419c8c8061c0ff6dc12c66ad6eda1b266d2f40bd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2020
ms.locfileid: "92164219"
---
# <a name="disaster-recovery-for-farmbeats"></a>Farmtts için olağanüstü durum kurtarma

Veri kurtarma, Azure bölgesini daraltma gibi bir olaydaki verilerinizi kaybetmenize karşı korur. Böyle bir olayda, yük devretmeyi başlatabilir ve Farmtts dağıtımınızda depolanan verileri kurtarabilirsiniz.

Veri kurtarma, Azure Farmtempts 'de varsayılan bir özellik değildir. Bu özelliği, verileri bir Azure eşlenmiş bölgesinde depolamak üzere Farmtts tarafından kullanılan gerekli Azure kaynaklarını yapılandırarak el ile yapılandırabilirsiniz. Kurtarmayı etkinleştirmek için etkin – Pasif yaklaşım kullanın.

Aşağıdaki bölümlerde Azure Farmtts 'de veri kurtarmayı nasıl yapılandırabileceğiniz hakkında bilgi sağlanmaktadır:

- [Veri artıklığını etkinleştir](#enable-data-redundancy)
- [Hizmeti çevrimiçi yedekten geri yükleme](#restore-service-from-online-backup)


## <a name="enable-data-redundancy"></a>Veri artıklığını etkinleştir

Farm, verileri **Azure depolama**, **Cosmos DB** ve **Time Series Insights**olmak üzere üç Azure birinci taraf hizmeti halinde depolar. Bu hizmetler için eşleştirilmiş bir Azure bölgesine veri yedekliliği etkinleştirmek üzere aşağıdaki adımları kullanın:

1.  **Azure depolama** -bu yönergeleri Izleyerek, Farmtts dağıtımınızdaki her bir depolama hesabı için veri yedekliliği etkinleştirin.
2.  **Azure Cosmos DB** -bu kılavuzu, Farmtts dağıtımınızın Cosmos DB hesabı için veri yedekliliği etkinleştirmek üzere izleyin.
3.  **Azure Time Series Insights (TSİ)** -TSI Şu anda veri artıklığı sunmaz. Time Series Insights verileri kurtarmak için sensör/Hava durumu iş ortağınıza gidin ve verileri Farmto dağıtımına yeniden gönderin.

## <a name="restore-service-from-online-backup"></a>Hizmeti çevrimiçi yedekten geri yükleme

Yük devretmeyi başlatabilir ve depolanan verileri kurtarabilirsiniz, bu, yukarıda belirtilen verilerin her biri, Farmtts dağıtımınız için depolar. Azure depolama ve Cosmos DB yönelik verileri kurtardıktan sonra, Azure eşlenmiş bölgesinde başka bir Farmtts dağıtımı oluşturun ve ardından yeni dağıtımı, aşağıdaki adımları kullanarak geri yüklenen veri depolarından (yani Azure depolama ve Cosmos DB) veri kullanacak şekilde yapılandırın:

1. [Cosmos DB’yi yapılandırma](#configure-cosmos-db)
2. [Depolama hesabını yapılandırma](#configure-storage-account)


### <a name="configure-cosmos-db"></a>Cosmos DB’yi yapılandırma

Geri yüklenen Cosmos DB erişim anahtarını kopyalayın ve yeni Farmrets veri hub 'ını Key Vault güncelleştirin.


  ![Erişim anahtarının kopyasının nereden alınacağını vurgulayan bir ekran görüntüsü.](./media/disaster-recovery-for-farmbeats/key-vault-secrets.png)

> [!NOTE]
> Geri yüklenen Cosmos DB URL 'sini kopyalayın ve yeni Farmrets Datahub App Service yapılandırmasında güncelleştirin. Artık yeni Farmtts dağıtımında Cosmos DB hesabını silebilirsiniz.

  ![Geri yüklenen Cosmos DB URL 'sinin nereye kopyalanacağını gösteren ekran görüntüsü.](./media/disaster-recovery-for-farmbeats/configuration.png)

### <a name="configure-storage-account"></a>Depolama hesabını yapılandırma

Geri yüklenen depolama hesabının erişim anahtarını kopyalayın ve yeni Farmrets veri hub 'ında Key Vault güncelleştirin.

![Geri yüklenen depolama hesabının erişim anahtarının nereye kopyalanacağını gösteren ekran görüntüsü.](./media/disaster-recovery-for-farmbeats/key-vault-7-secrets.png)

>[!NOTE]
> Yeni Farmtts Batch VM yapılandırma dosyasında depolama hesabı adını güncelleştirdiğinizden emin olun.

![Olağanüstü Durum Kurtarma](./media/disaster-recovery-for-farmbeats/batch-prep-files.png)

Benzer şekilde, Hızlandırıcı depolama hesabınız için veri kurtarmayı etkinleştirdiyseniz, yeni Farmtts örneğinde Hızlandırıcı depolama hesabı erişim anahtarı ve adını güncelleştirmek için adım 2 ' yi izleyin.
