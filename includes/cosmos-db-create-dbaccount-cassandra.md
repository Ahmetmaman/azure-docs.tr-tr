---
title: dosya dahil etme
description: dosya dahil etme
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.topic: include
ms.date: 01/22/2020
ms.author: sngun
ms.custom: include file
ms.openlocfilehash: 8f7a69b81430d964d1aade26ed179354171e4164
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2020
ms.locfileid: "77134717"
---
1. Yeni bir tarayıcı penceresinde [Azure portalında](https://portal.azure.com/) oturum açın.

2. Sol taraftaki menüde **kaynak oluştur**' u seçin.
   
   ![Azure portalında kaynak oluşturma](./media/cosmos-db-create-dbaccount-cassandra/create-nosql-db-databases-json-tutorial-0.png)
   
3. **Yeni** sayfada **veritabanları**  >  **Azure Cosmos DB**' nı seçin.
   
   ![Azure portalındaki Veritabanları bölmesi](./media/cosmos-db-create-dbaccount-cassandra/create-nosql-db-databases-json-tutorial-1.png)
   
3. **Azure Cosmos DB hesabı oluştur** sayfasında, yeni Azure Cosmos DB hesabının ayarlarını girin. 
 
    Ayar|Değer|Açıklama
    ---|---|---
    Abonelik|Aboneliğiniz|Bu Azure Cosmos DB hesabı için kullanmak istediğiniz Azure aboneliğini seçin. 
    Kaynak Grubu|Yeni oluştur<br><br>Daha sonra hesap adı ile aynı adı girin|**Yeni oluştur**’u seçin. Ardından, hesabınız için yeni bir kaynak grubu adı girin. Kolaylık olması için, Azure Cosmos hesap adınızla aynı adı kullanın. 
    Hesap Adı|Benzersiz bir ad girin|Azure Cosmos DB hesabınızı tanımlayan benzersiz bir ad girin. Hesap URI 'niz, benzersiz hesap adınızın *Cassandra.Cosmos.Azure.com* eklenir.<br><br>Hesap adı yalnızca küçük harf, sayı ve kısa çizgi (-) kullanabilir ve 3 ila 31 karakter uzunluğunda olmalıdır.
    API|Cassandra|API, oluşturulacak hesap türünü belirler. Azure Cosmos DB beş API sağlar: belge veritabanları için çekirdek (SQL), grafik veritabanları için Gremlin, belge veritabanları için MongoDB, Azure tablosu ve Cassandra. Her API için ayrı bir hesap oluşturmanız gerekir. <br><br>**Cassandra**' ı seçin, bu hızlı başlangıçta Cassandra API ile birlikte çalışacak bir tablo oluşturuyorsunuz. <br><br>[Cassandra API hakkında daha fazla bilgi edinin](../articles/cosmos-db/cassandra-introduction.md).|
    Konum|Kullanıcılarınıza en yakın bölgeyi seçin|Azure Cosmos DB hesabınızın barındırılacağı coğrafi konumu seçin. Verilere en hızlı erişimi sağlamak için kullanıcılarınıza en yakın olan konumu kullanın.

    **Gözden geçir + oluştur**' u seçin. **Ağ** ve **Etiketler** bölümünü atlayabilirsiniz. 

    ![Azure Cosmos DB için yeni hesap sayfası](./media/cosmos-db-create-dbaccount-cassandra/azure-cosmos-db-create-new-account.png)

4. Hesabın oluşturulması birkaç dakika sürer. Portalın Tebrikler sayfasını görüntülemesini bekleyin **! Azure Cosmos DB hesabınız oluşturuldu**.

