---
title: "Öğretici: MongoDB için API için Azure Cosmos DB'nin MongoDB çevrimdışı geçirmek için Azure veritabanı geçiş hizmeti kullanın | Microsoft Docs"
description: MongoDB şirket içinden Azure Cosmos DB API için çevrimdışı MongoDB için Azure veritabanı geçiş hizmeti kullanarak geçirmeyi öğrenin.
services: dms
author: pochiraju
ms.author: rajpo
manager: craigg
ms.reviewer: douglasl
ms.service: dms
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: article
ms.date: 12/11/2018
ms.openlocfilehash: 5fd3200ab787a26b11feb121b5db125e4a79365c
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2019
ms.locfileid: "56960395"
---
# <a name="tutorial-migrate-mongodb-to-azure-cosmos-dbs-api-for-mongodb-offline-using-dms"></a>Öğretici: MongoDB için Azure Cosmos DB API için MongoDB geçişi DMS kullanarak çevrimdışı
MongoDB için API Azure Cosmos DB'nin MongoDB örneğini Bulut veya bir şirket içi veritabanlarının çevrimdışı (tek seferlik) geçiş gerçekleştirmek için Azure veritabanı geçiş hizmetini kullanabilirsiniz.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> * Azure Veritabanı Geçiş Hizmeti örneği oluşturma.
> * Azure Veritabanı Geçiş Hizmeti'ni kullanarak geçiş projesi oluşturma.
> * Geçişi çalıştırma.
> * Geçişi izleme.

Bu öğreticide, Azure Cosmos DB API'si için bir Azure sanal Makinesi'nde MongoDB için Azure veritabanı geçiş hizmeti kullanarak barındırılan bir MongoDB kümesinde geçirin. Önceden kurulmuş bir MongoDB kaynağına sahip değilseniz, bkz [yükleyin ve azure'da Windows sanal makinesi üzerinde MongoDB yapılandırma](https://docs.microsoft.com/azure/virtual-machines/windows/install-mongodb).

## <a name="prerequisites"></a>Önkoşullar
Bu öğreticiyi tamamlamak için aşağıdakileri yapmanız gerekir:
- [MongoDB hesabı için bir Azure Cosmos DB'nin API'si oluşturma](https://ms.portal.azure.com/#create/Microsoft.DocumentDB).
- Azure Resource Manager dağıtım modelini kullanarak Azure Veritabanı Geçiş Hizmeti için [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) veya [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways) kullanarak şirket içi kaynak sunucularınıza siteden siteye bağlantı sağlayan bir sanal ağ oluşturun.
- Azure sanal ağ (VNET) ağ güvenlik grubu kurallarınızı aşağıdaki iletişim bağlantı noktalarını engelleme emin olun: 443, 53, 9354, 12000 yanı sıra 445. Azure VNET NSG trafiğini filtreleme hakkında ayrıntılı bilgi için [Ağ güvenlik grupları ile ağ trafiğini filtreleme](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) makalesine bakın.
- Varsayılan olarak TCP bağlantı noktası olan 27017 kaynak MongoDB sunucusuna erişmek Azure veritabanı geçiş hizmeti, Windows Güvenlik Duvarı'nı açın.
- Kaynak veritabanlarınızın önünde bir güvenlik duvarı cihazı kullanıyorsanız, Azure Veritabanı Geçiş Hizmeti'nin geçiş amacıyla kaynak veritabanlarına erişmesi için güvenlik duvarı kuralları eklemeniz gerekebilir.

## <a name="register-the-microsoftdatamigration-resource-provider"></a>Microsoft.DataMigration kaynak sağlayıcısını kaydetme
1. Azure portal'da oturum açın, **Tüm hizmetler** seçeneğini belirleyin ve ardından **Abonelikler**'i seçin.

   ![Portal aboneliklerini gösterme](media/tutorial-mongodb-to-cosmosdb/portal-select-subscription1.png)
       
2. Azure Veritabanı Geçiş Hizmeti örneğini oluşturmak istediğiniz aboneliği seçin ve sonra **Kaynak sağlayıcıları**’nı seçin.
 
    ![Kaynak sağlayıcılarını gösterme](media/tutorial-mongodb-to-cosmosdb/portal-select-resource-provider.png)
    
3.  "migration" araması yapın ve **Microsoft.DataMigration** öğesinin sağ tarafındaki **Kaydet**'i seçin.
 
    ![Kaynak sağlayıcısını kaydetme](media/tutorial-mongodb-to-cosmosdb/portal-register-resource-provider.png)    

## <a name="create-an-instance"></a>Örnek oluşturma
1.  Azure portalda +**Kaynak oluştur**'u seçin, Azure Veritabanı Geçiş Hizmeti araması yapın ve açılan listeden **Azure Veritabanı Geçiş Hizmeti**'ni seçin.

    ![Azure Market](media/tutorial-mongodb-to-cosmosdb/portal-marketplace.png)

2.  **Azure Veritabanı Geçiş Hizmeti** ekranında **Oluştur**'u seçin.
 
    ![Azure Veritabanı Geçiş Hizmeti örneğini oluşturma](media/tutorial-mongodb-to-cosmosdb/dms-create1.png)
  
3.  **Geçiş Hizmeti oluşturun** ekranında hizmet için bir ad belirtin, aboneliği ve yeni ya da var olan bir kaynak grubunu seçin.

4. Azure Veritabanı Geçiş Hizmeti örneğini oluşturmak istediğiniz konumu seçin. 

5. Var olan bir sanal ağı (VNET) seçin veya yeni bir tane oluşturun.

    Sanal ağ, Azure veritabanı geçiş Hizmeti'nin kaynak MongoDB örneği ve ' % s'hedef Azure Cosmos DB hesabı için erişim sağlar.

    Azure portalda sanal ağ oluşturma hakkında daha fazla bilgi için [Azure portalı kullanarak sanal ağ oluşturma](https://aka.ms/DMSVnet) makalesine bakın.

6. Fiyatlandırma katmanını seçin.

    Maliyetler ve fiyatlandırma katmanları hakkında daha fazla bilgi için [fiyatlandırma sayfasına](https://aka.ms/dms-pricing) bakın.

    Öneriler blog gönderisinde doğru Azure veritabanı geçiş hizmeti katmanını seçme yardıma ihtiyacınız olursa başvurmak [burada](https://go.microsoft.com/fwlink/?linkid=861067).  

     ![Azure Veritabanı Geçiş Hizmeti örneği ayarlarını yapılandırma](media/tutorial-mongodb-to-cosmosdb/dms-settings2.png)

7.  Hizmeti oluşturmak için **Oluştur**’u seçin.

## <a name="create-a-migration-project"></a>Geçiş projesi oluşturma
Hizmet oluşturulduktan sonra Azure portaldan bulun, açın ve yeni bir geçiş projesi oluşturun.

1. Azure portalda **Tüm hizmetler**'i seçin, Azure Veritabanı Geçiş Hizmeti araması yapın ve **Azure Veritabanı Geçiş Hizmeti**'ni seçin.
 
      ![Azure Veritabanı Geçiş Hizmeti’nin tüm örneklerini bulma](media/tutorial-mongodb-to-cosmosdb/dms-search.png)

2. **Azure Veritabanı Geçiş Hizmeti** ekranında oluşturduğunuz Azure Veritabanı Geçiş Hizmeti örneğinin adını arayın ve sonuçlardan bu örneği seçin.

3. +**Yeni Geçiş Projesi**'ni seçin.

4. Üzerinde **yeni geçiş projesi** projesi için bir ad belirtin, ekran **kaynak sunucu türü** metin kutusunda **MongoDB**, **hedef sunucu türü**  metin kutusunda **CosmosDB (MongoDB API'SİYLE)** ve ardından **etkinlik türünü seçin**seçin **çevrimdışı veri geçişi**. 

    ![Veritabanı geçiş hizmeti projesi oluşturma](media/tutorial-mongodb-to-cosmosdb/dms-create-project.png)

5.  Projeyi oluşturmak ve geçiş etkinliğini çalıştırmak için **Etkinlik oluştur ve çalıştır**'ı seçin.

## <a name="specify-source-details"></a>Kaynak ayrıntılarını belirtme
1. Üzerinde **kaynak ayrıntıları** ekranında, kaynak MongoDB sunucu bağlantı ayrıntılarını belirtin.
    
   Ayrıca, bağlantı dizesi modu kullanın ve geçirmek istediğiniz koleksiyon verisi yazılan bir blob depolama dosya kapsayıcısı için bir konum sağlayın.

   > [!NOTE]
   > Azure veritabanı geçiş hizmeti, Azure Cosmos DB'nin MongoDB koleksiyonları API'sine bson belgeleri veya json belgelerini de geçirebilirsiniz.
    
   DNS ad çözümlemenin mümkün olmadığı durumlarda IP Adresini de kullanabilirsiniz.

   ![Kaynak ayrıntılarını belirtme](media/tutorial-mongodb-to-cosmosdb/dms-specify-source.png)

2. **Kaydet**’i seçin.

## <a name="specify-target-details"></a>Hedef ayrıntılarını belirtme
1. Üzerinde **geçiş hedef ayrıntıları** ekranında, önceden sağlanmış Azure Cosmos DB'nin API geçip MongoDB verilerini geçiş yaptığınız MongoDB hesabı için Azure Cosmos DB hesabı, hedef bağlantı ayrıntılarını belirtin.

    ![Hedef ayrıntılarını belirtme](media/tutorial-mongodb-to-cosmosdb/dms-specify-target.png)

2. **Kaydet**’i seçin.

## <a name="map-to-target-databases"></a>Hedef veritabanlarıyla eşleyin
1. Üzerinde **hedef veritabanlarına eşleme** ekranında, kaynak ve hedef veritabanı geçiş eşleyin.

    Hedef veritabanı, kaynak veritabanıyla aynı veritabanı adına sahipse Azure Veritabanı Geçiş Hizmeti varsayılan olarak hedef veritabanını seçer.

    Dize **Oluştur** gösteren Azure veritabanı geçiş hizmeti hedef veritabanında bulunamadı ve hizmet veritabanı oluşturacaktır veritabanı adının yanında görünür.

    Bu noktada geçiş işleminde, şunları yapabilirsiniz [sağlama aktarım hızı](https://docs.microsoft.com/azure/cosmos-db/set-throughput). Cosmos DB'de aktarım hızı ve veritabanı düzeyinde veya tek tek her koleksiyon için sağlayabilirsiniz. Aktarım hızı ölçülür [istek birimi](https://docs.microsoft.com/azure/cosmos-db/request-units) (RU). Daha fazla bilgi edinin [Azure Cosmos DB fiyatlandırma](https://azure.microsoft.com/pricing/details/cosmos-db/).

    ![Hedef veritabanlarıyla eşleyin](media/tutorial-mongodb-to-cosmosdb/dms-map-target-databases.png)

2. **Kaydet**’i seçin.
3. Üzerinde **koleksiyon ayarını** ekran, liste koleksiyonları genişletin ve ardından geçirilecek koleksiyonları listesini gözden geçirin.

    Azure veritabanı geçiş hizmeti otomatik Azure Cosmos DB hesabı hedefte mevcut olmayan kaynak MongoDB örneğinde mevcut tüm koleksiyonlar seçer unutmayın. Zaten veri içeren koleksiyonlar yeniden geçirmek istiyorsanız, açıkça bu dikey penceredeki koleksiyonları'ı seçmeniz gerekir.

    Kullanılacak koleksiyonların istediğiniz RU miktarı belirtebilirsiniz. Azure veritabanı geçiş hizmeti, koleksiyon boyutunu temel alan akıllı varsayılanlar önerir.

    Yararlanmak için bir parça anahtarı belirtebilirsiniz [Azure Cosmos DB'de bölümleme](https://docs.microsoft.com/azure/cosmos-db/partitioning-overview) en iyi ölçeklendirilebilirlik için. Gözden geçirmeyi unutmayın [parça/bölüm anahtarının seçilmesi için en iyi yöntemler](https://docs.microsoft.com/azure/cosmos-db/partitioning-overview#choose-partitionkey).

    ![Koleksiyonları tabloları seçin](media/tutorial-mongodb-to-cosmosdb/dms-collection-setting.png)

4. **Kaydet**’i seçin.

5. **Geçiş özeti** ekranının **Etkinlik adı** metin kutusunda geçiş etkinliği için bir ad belirtin.

    ![Geçiş özeti](media/tutorial-mongodb-to-cosmosdb/dms-migration-summary.png)

## <a name="run-the-migration"></a>Geçişi çalıştırma
- **Geçişi çalıştır**'ı seçin.

    Geçiş etkinlik penceresi görüntülenir ve **durumu** etkinliğini olan **başlatılmadı**.

    ![Etkinlik durumu](media/tutorial-mongodb-to-cosmosdb/dms-activity-status.png)

## <a name="monitor-the-migration"></a>Geçişi izleme
- Geçiş etkinliği ekranında **Yenile**'yi seçerek geçişin **Durum** bilgisi **Tamamlandı** olana kadar gösterilen verileri güncelleştirebilirsiniz.

   > [!NOTE]
   > Veritabanı ve koleksiyon düzeyi geçiş ölçümleri ayrıntılarını almak için etkinlik seçebilirsiniz.

    ![Etkinlik durumunu tamamlandı](media/tutorial-mongodb-to-cosmosdb/dms-activity-completed.png)

## <a name="verify-data-in-cosmos-db"></a>Verileri Cosmos DB'de doğrula

- Geçiş tamamlandıktan sonra tüm koleksiyonları başarıyla geçirildiğini doğrulamak için Azure Cosmos DB hesabınızı kontrol edebilirsiniz.

    ![Etkinlik durumunu tamamlandı](media/tutorial-mongodb-to-cosmosdb/dms-cosmosdb-data-explorer.png)

## <a name="additional-resources"></a>Ek kaynaklar

 * [Cosmos DB hizmet bilgileri](https://azure.microsoft.com/services/cosmos-db/)

## <a name="next-steps"></a>Sonraki adımlar
- Microsoft ek senaryolar için geçiş kılavuzunu gözden [veritabanı Geçiş Kılavuzu](https://datamigration.microsoft.com/).
