---
title: include dosyası
description: include dosyası
services: cosmos-db
author: deborahc
ms.service: cosmos-db
ms.topic: include
ms.date: 11/24/2018
ms.author: dech
ms.custom: include file
ms.openlocfilehash: 6169821d0e81f0af108dccbfa761f5512e352628
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52853120"
---
1. Yeni bir tarayıcı penceresinde [Azure portalında](https://portal.azure.com/) oturum açın.
2. **Kaynak oluştur** > **Veritabanları** > **Azure Cosmos DB** seçeneğine tıklayın.
   
   ![Azure portalındaki Veritabanları bölmesi](./media/cosmos-db-create-dbaccount/create-nosql-db-databases-json-tutorial-1.png)

3. İçinde **Azure Cosmos DB hesabı oluştur** sayfasında, yeni Azure Cosmos DB hesabının ayarlarını girin. 
 
    Ayar|Değer|Açıklama
    ---|---|---
    Abonelik|*Aboneliğiniz*|Bu Azure Cosmos DB hesabı için kullanmak istediğiniz Azure aboneliğini seçin. 
    Kaynak Grubu|Yeni oluştur<br><br>*Benzersiz bir ad girin*|**Yeni oluştur**’u seçin ve ardından hesabınız için yeni bir kaynak grubu adı girin. Kolaylık olması için hesap adınız aynı adı kullanabilirsiniz. 
    Hesap Adı|*Benzersiz bir ad girin*|Azure Cosmos DB hesabınızı tanımlayan benzersiz bir ad girin. Girdiğiniz kimliğe *documents.azure.com* eklenerek URI'niz oluşturulacağından benzersiz bir kimlik kullanın.<br><br>Kimlik yalnızca küçük harf, sayı ve kısa çizgi (-) karakterini içerebilir ve 3 - 50 karakterden oluşmalıdır.
    API|Çekirdek (SQL)|API, oluşturulacak hesap türünü belirler. Azure Cosmos DB beş API sunar: SQL (belge veritabanı), Gremlin (grafik veritabanı), MongoDB (belge veritabanı), Tablo API'si ve Cassandra API. Her API şu anda ayrı bir hesap oluşturmanızı gerektirir. <br><br>Seçin **çekirdek (SQL)** bu makalede, bir belge veritabanı oluşturur ve SQL söz dizimini kullanarak sorgulamak için. <br><br>[SQL API’si hakkında daha fazla bilgi edinme](../articles/cosmos-db/documentdb-introduction.md)|
    Konum|*Kullanıcılarınıza en yakın bölgeyi seçin*|Azure Cosmos DB hesabınızın barındırılacağı coğrafi konumu seçin. Verilere en hızlı erişimi sağlamak için kullanıcılarınıza en yakın olan konumu kullanın.
    Coğrafi yedekliliği etkinleştir| Boş bırakın | Bu, ikinci (eşleştirilmiş) bir bölgede veritabanınızın çoğaltılmış bir sürümünü oluşturur. Bunu boş bırakın.  
    Çok bölgeli Yazar| Boş bırakın | Bu, her iki okuma ve yazma bölgesi, veritabanı bölgeleri sağlar. Bunu boş bırakın.  

    Ardından **Gözden geçir + oluştur**’a tıklayın. Atlayabilirsiniz **ağ** ve **etiketleri** bölümü. 

    ![Azure Cosmos DB için yeni hesap sayfası](./media/cosmos-db-create-dbaccount/azure-cosmos-db-create-new-account-preview.png)

    Özet bilgilerini gözden geçirin ve tıklayın **Oluştur**. 

    ![Hesap doğrulama Özet sayfası](./media/cosmos-db-create-dbaccount/azure-cosmos-db-create-new-account-summary-preview.png)

4. Hesabın oluşturulması birkaç dakika sürer. Görüntülemek portalın **dağıtımınız tamamlandıktan** tıklayın ve ileti **kaynağa Git**.     

    ![Hesap başarıyla oluşturuldu sayfası](./media/cosmos-db-create-dbaccount/azure-cosmos-db-create-new-account-complete-preview.png)

5. Portalı artık görüntülenecek **Tebrikler! Azure Cosmos DB hesabınız oluşturuldu** sayfasını görüntülemesini bekleyin.

    ![Azure portalındaki Bildirimler bölmesi](./media/cosmos-db-create-dbaccount/azure-cosmos-db-account-created.png)

