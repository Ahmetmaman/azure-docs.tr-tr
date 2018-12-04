---
title: Verilerinizi yönetmek için Azure Cosmos DB Gezgini'ni kullanma
description: Azure Cosmos DB Gezgini'ni görüntülemek ve Azure Cosmos DB'de depolanan verileri yönetmenize olanak sağlayan bir tek başına web tabanlı arabirimidir.
services: cosmos-db
author: deborahc
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/16/2018
ms.author: dech
ms.openlocfilehash: be3742bdf6bc5a22947307c44a83b1b231526afd
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52835446"
---
# <a name="use-azure-cosmos-db-explorer-to-manage-your-data"></a>Verilerinizi yönetmek için Azure Cosmos DB Gezgini'ni kullanma 

Azure Cosmos DB Gezgini'ni görüntülemek ve Azure Cosmos DB'de depolanan verileri yönetmenize olanak sağlayan bir tek başına web tabanlı arabirimidir. Azure Cosmos DB Gezgini'ni varolan eşdeğerdir **Veri Gezgini** Azure Cosmos DB hesabı oluşturduğunuzda, Azure portalında kullanılabilen sekmesi. Mevcut Veri Gezgini üzerinden Azure Cosmos DB explorer'ın önemli avantajlar şunlardır:

* Bir tam veri, sorgu çalıştırma, saklı yordamlar, Tetikleyiciler görüntülemek için ekrandan sahip ve sonuçları görüntüleyin.  

* Veritabanı hesabınız ve Azure portalı veya abonelik erişimi olmayan diğer kullanıcılara koleksiyonları için geçici veya kalıcı okuma veya okuma-yazma erişimi sağlar.  

* Sorgu sonuçları, Azure portalı veya abonelik erişimi olmayan diğer kullanıcılarla paylaşabilirsiniz.  

## <a name="access-azure-cosmos-db-explorer"></a>Azure Cosmos DB Gezgini'ni erişim

1. Oturum [Azure portalında](https://portal.azure.com/). 

2. Gelen **tüm kaynakları**bulun ve, Azure Cosmos DB hesabı, select anahtarları ve kopya Git **PRIMARY CONNECTION Strıng'i**.  

3. Git https://cosmos.azure.com/, bağlantı dizesini yapıştırın ve seçin **Connect**. Bağlantı dizesi kullanarak Azure Cosmos DB Gezgini'ni hiçbir zaman Kısıtlamasız erişebilir.  

   Diğer kullanıcıların Azure Cosmos DB hesabınız geçici erişim sağlamak istiyorsanız, okuma-yazma ve okuma erişimi URL'leri kullanarak bunu yapabilirsiniz. 

4. Açık **Veri Gezgini** dikey penceresinde **açık tam ekran**. Açılan iletişim kutusundan, iki görüntüleyebilirsiniz URL'lere – **okuma-yazma** ve **okuma**. Bu URL'leri, Azure Cosmos DB hesabınız geçici olarak diğer kullanıcılarla paylaşmanıza olanak sağlar. Hesabına erişimi süre sonu 24 saat sonra yeni bir erişim URL'si veya bağlantı dizesi kullanarak bağlanabilirsiniz. 

   **Okuma-yazma** – diğer kullanıcılarla okuma-yazma URL'si paylaştığınızda görüntüleyebilir ve veritabanları, koleksiyonlar, sorgular ve belirli ilgili hesapla ilişkili diğer kaynaklar değiştirin.

   **Okuma** - salt okunur URL'yi diğer kullanıcılarla paylaşın, veritabanları, koleksiyonlar, sorgular ve belirli ilgili hesapla ilişkili diğer kaynaklar görüntüleyebilirsiniz. Örneğin, bir sorgunun sonuçlarını Azure portal veya Azure Cosmos DB hesabınıza erişimi olmayan arkadaşlarınızla paylaşmak istiyorsanız, bunları bu URL ile sağlayabilir.

   Hesapla açıp istediğiniz erişim türünü seçin **açın**. Explorer'ı açın, sonra Azure portalında Veri Gezgini sekmesi olduğu gibi aynı deneyimidir.   

   ![Azure Cosmos DB Gezgini'ni Aç](./media/data-explorer/open-data-explorer-with-access-url.png)

## <a name="known-issues"></a>Bilinen sorunlar

Şu anda **açık tam ekran** geçici okuma-yazma paylaşım veya okuma erişimi sağlayan deneyim, Azure Cosmos DB Gremlin ve tablo API'si hesapları için henüz desteklenmiyor. Azure Cosmos DB Gezgini'ni için bağlantı dizesi geçirerek, Gremlin ve tablo API'si hesapları daha da görüntüleyebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar
Sonraki verilerinizi yönetmek için Azure Cosmos DB Gezgini ile çalışmaya başlama gerçekleştirmeyi öğrendiniz, şunları yapabilirsiniz:

* Tanımlamaya başlayın [sorguları](sql-api-sql-query-reference.md) gerçekleştirmek ve SQL söz dizimini kullanarak [sunucu tarafı programlama](programming.md) UDF'ler, saklı yordamlar'ni kullanarak tetikler. 
