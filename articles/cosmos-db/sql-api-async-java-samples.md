---
title: "Azure Cosmos DB: SQL API'si için Zaman Uyumsuz Java örnekleri | Microsoft Docs"
description: CRUD işlemleri gibi Azure Cosmos DB SQL API'sinin kullanıldığı yaygın görevler için GitHub'da Zaman Uyumsuz Java örnekleri bulabilirsiniz.
services: cosmos-db
author: SnehaGunda
manager: kfile
documentationcenter: java
ms.service: cosmos-db
ms.workload: data-services
ms.devlang: java
ms.topic: sample
ms.date: 06/18/2018
ms.author: sngun
ms.openlocfilehash: 9e2deb2a3cbd66db7d7abe03bb5ea8e1574f817b
ms.sourcegitcommit: ebf2f2fab4441c3065559201faf8b0a81d575743
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2018
ms.locfileid: "52161998"
---
# <a name="azure-cosmos-db-async-java-examples-for-the-sql-api"></a>Azure Cosmos DB: SQL API'si için Zaman Uyumsuz Java örnekleri

> [!div class="op_single_selector"]
> * [.NET Örnekleri](sql-api-dotnet-samples.md)
> * [Java Örnekleri](sql-api-java-samples.md)
> * [Async Java Örnekleri](sql-api-async-java-samples.md)
> * [Node.js Örnekleri](sql-api-nodejs-samples.md)
> * [Python Örnekleri](sql-api-python-samples.md)
> * [Azure Kod Örneği Galerisi](https://azure.microsoft.com/resources/samples/?sort=0&service=cosmos-db)
> 
> 

Azure Cosmos DB kaynaklarında CRUD işlemlerini ve diğer yaygın işlemleri gerçekleştiren en son örnek uygulamalar, [azure-cosmosdb-java](https://github.com/Azure/azure-cosmosdb-java) GitHub deposuna eklenmiştir. Bu makalede aşağıdakiler sunulmaktadır:

* Örnek Zaman Uyumsuz Java proje dosyalarının her birindeki görevlere bağlantılar. 
* İlgili API başvurusu içeriğine bağlantılar.

**Önkoşullar**

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]
  
- [Visual Studio abone avantajlarını etkinleştirebilirsiniz](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio): Visual Studio aboneliğiniz, her ay size ücretli Azure hizmetleri için kullanabileceğiniz krediler verir.

[!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

Bu örnek uygulamayı çalıştırmanız için aşağıdakiler gereklidir:

* Java Development Kit 8
* Microsoft Azure Cosmos DB Java SDK’sı

Projenizde kullanmak üzere en son Microsoft Azure Cosmos DB Java SDK'sı ikili dosyalarını almak için isteğe bağlı olarak Maven de kullanabilirsiniz. [Buradan](https://mvnrepository.com/artifact/com.microsoft.azure/azure-cosmosdb) en son sürümü seçebilirsiniz. Maven tüm gerekli bağımlılıkları otomatik olarak ekler. Aksi takdirde, pom.xml dosyasında listelenen bağımlılıkları doğrudan indirebilir ve derleme yolunuza ekleyebilirsiniz.

```bash
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-cosmosdb</artifactId>
    <version>${LATEST}</version>
</dependency>
```

**Örnek uygulamaları çalıştırma**

Örnek depoyu kopyalama:
```bash
$ git clone https://github.com/Azure/azure-cosmosdb-java.git

$ cd azure-cosmosdb-java
```

Örnekleri Eclipse kullanarak çalıştırabileceğiniz gibi, Maven kullanıp komut satırından da çalıştırabilirsiniz.

Eclipse'ten çalıştırmak için:
* Eclipse'te ana üst proje pom.xml dosyasını yükleyin; bunun azure-cosmosdb-examples dosyasını otomatik olarak yüklemesi gerekir.
* Örnekleri çalıştırmak için geçerli bir Azure Cosmos DB Uç Noktası gerekir. Uç noktalar `src/test/java/com/microsoft/azure/cosmosdb/rx/examples/TestConfigurations.java` konumundan okunur.
* Uç nokta kimlik bilgilerinizi Eclipse JUnit Run Config'de VM Arguments olarak geçirebilir veya TestConfigurations.java dosyasına koyabilirsiniz.
    ```bash
    -DACCOUNT_HOST=<Fill your Azure Cosmos DB account name> -DACCOUNT_KEY=<Fill your Azure Cosmos DB primary key>
    ```
* Şimdi örnekleri Eclipse'de JUnit testleri olarak çalıştırabilirsiniz.

Komut satırından çalıştırmak için:
* Örnekleri çalıştırmanın diğer yolu Maven kullanmaktır:
* Maven'i çalıştırın ve Azure Cosmos DB Uç Nokta kimlik bilgilerinizi geçirin:
    ```bash
    mvn test -DACCOUNT_HOST=<Fill your Azure Cosmos DB account name> -DACCOUNT_KEY=<Fill your Azure Cosmos DB primary key>
    ```

   > [!NOTE]
   > Örnekler birbirinden bağımsızdır; kendi kendine ayarlanır ve sonra kendini temizler. Örneklerde [DocumentClient.createCollection](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx._async_document_client.createcollection)'a birden çok çağrı yapılır. Bu her yapıldığında, oluşturulan koleksiyonun performans katmanına karşılık aboneliğiniz 1 saatlik kullanım için faturalandırılır. 
   > 
   > 

## <a name="database-examples"></a>Veritabanı örnekleri
[DatabaseCrudAsyncAPITest](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DatabaseCRUDAsyncAPITest.java) dosyası aşağıdaki görevlerin nasıl gerçekleştirileceğini gösterir:

| Görev | API başvurusu |
| --- | --- |
| [Veritabanı oluşturma](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DatabaseCRUDAsyncAPITest.java#L115-L132) | [AsyncDocumentClient.createDatabase](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx._async_document_client.createdatabase) |
| [Zaten var olan bir veritabanı oluşturulamadı](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DatabaseCRUDAsyncAPITest.java#L189-L204) | |
| [Veritabanını oluşturma ve okuma](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DatabaseCRUDAsyncAPITest.java#L228-L249) | [AsyncDocumentClient.readDatabase](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx._async_document_client.readdatabase) |
| [Veritabanı oluşturma ve silme](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DatabaseCRUDAsyncAPITest.java#L255-L276) | [AsyncDocumentClient.deleteDatabase](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx._async_document_client.deletedatabase) |
| [Veritabanını oluşturma ve sorgulama](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DatabaseCRUDAsyncAPITest.java#L282-L312) | [AsyncDocumentClient.queryDatabases](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx._async_document_client.querydatabases) |

## <a name="collection-examples"></a>Koleksiyon örnekleri
[CollectionCrudAsyncAPITest](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/CollectionCRUDAsyncAPITest.java) dosyası aşağıdaki görevlerin nasıl gerçekleştirileceğini gösterir:

| Görev | API başvurusu |
| --- | --- |
| [Tek bir bölüm koleksiyonu oluşturma](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/CollectionCRUDAsyncAPITest.java#L134-L153) | [AsyncDocumentClient.createCollection](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx._async_document_client.createcollection) |
| [Özel çok bölümlü bir koleksiyon oluşturma](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/CollectionCRUDAsyncAPITest.java#L164-L184) | [DocumentCollection](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb._document_collection)<br>[PartitionKeyDefinition](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb._partition_key_definition)<br>[RequestOptions](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb._request_options) |
| [Zaten var olan bir koleksiyon oluşturulamadı](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/CollectionCRUDAsyncAPITest.java#L241-L256) | |
| [Koleksiyon oluşturma ve okuma](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/CollectionCRUDAsyncAPITest.java#L281-L304) | [AsyncDocumentClient.readCollection](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx._async_document_client.readcollection) |
| [Koleksiyon oluşturma ve silme](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/CollectionCRUDAsyncAPITest.java#L310-L333) | [AsyncDocumentClient.deleteCollection](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx._async_document_client.deletecollection) |
| [Koleksiyon oluşturma ve sorgulama](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/CollectionCRUDAsyncAPITest.java#L339-L372) | [AsyncDocumentClient.queryCollection](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx._async_document_client.querycollections) |

## <a name="document-examples"></a>Belge örnekleri
[DocumentCrudAsyncAPITest](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DocumentCRUDAsyncAPITest.java) dosyası aşağıdaki görevlerin nasıl gerçekleştirileceğini gösterir:

| Görev | API başvurusu |
| --- | --- |
| [Belge oluşturma](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DocumentCRUDAsyncAPITest.java#L137-L156) | [AsyncDocumentClient.createDocument](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx._async_document_client.createdocument) |
| [Programlanabilir belge tanımıyla belge oluşturma](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DocumentCRUDAsyncAPITest.java#L213-L242) | [Belge](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb._document)<br>[Resource.setId](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb._resource.setid) |
| [Belge oluşturma ve toplam RU maliyetini bulma](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DocumentCRUDAsyncAPITest.java#L248-L280) | [ResourceResponse.getRequestCharge](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb._resource_response.getrequestcharge) |
| [Zaten var olan bir belge oluşturulamadı](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DocumentCRUDAsyncAPITest.java#L316-L336) | |
| [Belge oluşturma ve değiştirme](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DocumentCRUDAsyncAPITest.java#L342-L365) | [AsyncDocumentClient.replaceDocument](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx._async_document_client.replacedocument) |
| [Belge oluşturma ve upsert etme](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DocumentCRUDAsyncAPITest.java#L371-L393) | [AsyncDocumentClient.upsertDocument](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx._async_document_client.upsertdocument) |
| [Belge oluşturma ve silme](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DocumentCRUDAsyncAPITest.java#L399-L431) | [AsyncDocumentClient.deleteDocument](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx._async_document_client.deletedocument) |
| [Belge oluşturma ve okuma](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DocumentCRUDAsyncAPITest.java#L437-L458) | [AsyncDocumentClient.readDocument](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx._async_document_client.readdocument) |

## <a name="indexing-examples"></a>Dizin oluşturma örnekleri
[CollectionCrudAsyncAPITest](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/CollectionCRUDAsyncAPITest.java) dosyası aşağıdaki görevlerin nasıl gerçekleştirileceğini gösterir:

| Görev | API başvurusu |
| --- | --- |
| [Dizin oluşturma ve dizin oluşturma ilkesini ayarlama](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/CollectionCRUDAsyncAPITest.java#L394-L410) | [Dizin](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb._index)<br>[IndexingPolicy](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb._indexing_policy) |

Dizin oluşturma hakkında daha fazla bilgi için bkz. [Azure Cosmos DB dizin oluşturma ilkeleri](indexing-policies.md).

## <a name="query-examples"></a>Sorgu örnekleri
[DocumentQuerySamples](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DocumentQueryAsyncAPITest.java) dosyasında aşağıdaki görevlerin nasıl gerçekleştirileceği gösterilir:

| Görev | API başvurusu |
| --- | --- |
| [Basit bir belge sorgusu gerçekleştirme](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DocumentQueryAsyncAPITest.java#L154-L190) | [AsyncDocumentClient.queryDocuments](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx._async_document_client.querydocuments) |
| [Basit bir belge sorgusu gerçekleştirme ve Toplam RU maliyetini öğrenme](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DocumentQueryAsyncAPITest.java#L249-L268) | [FeedResponse.getRequestCharge](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb._feed_response.getrequestcharge) |
| [Basit bir belge sorgusu gerçekleştirme, bir sayfa okuma ve döndürülen gözlemlenebilir öğe aboneliğinden çıkma](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DocumentQueryAsyncAPITest.java#L274-L312) | |
| [Basit bir belge sorgusu gerçekleştirme ve sonuçları filtreleme](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DocumentQueryAsyncAPITest.java#L318-L368) | |
| [Sırama ölçütü çapraz bölümlü belge sorgusu yapma](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DocumentQueryAsyncAPITest.java#L410-L457) | [FeedOptions.setEnableCrossPartitionQuery](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb._feed_options.setenablecrosspartitionquery) |

Sorgu yazma hakkında daha fazla bilgi için bkz. [Azure Cosmos DB içinde SQL sorgusu](how-to-sql-query.md).

## <a name="offer-examples"></a>Teklif örnekleri
[OfferCRUDAsyncAPITest](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/OfferCRUDAsyncAPITest.java) dosyası aşağıdaki görevlerin nasıl gerçekleştirileceğini gösterir:

| Görev | API başvurusu |
| --- | --- |
| [Koleksiyon oluşturma ve aktarım hızını ayarlama](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/OfferCRUDAsyncAPITest.java#L106-L113) | [AsyncDocumentClient.createCollection](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx._async_document_client.createcollection)<br>[RequestOptions.setOfferThroughput](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb._request_options.setofferthroughput) |
| [İlişkili teklifi bulmak için koleksiyonu okuma](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/OfferCRUDAsyncAPITest.java#L118-L130) | [Offer.getContent](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._offer.getContent)<br>[DocumentClient.queryOffers](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx._async_document_client.queryoffers) |
| [Bir koleksiyonun teklifini değiştirerek aktarım hızını güncelleştirme](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/OfferCRUDAsyncAPITest.java#L101-L153) | [AsyncDocumentClient.replaceOffer](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx._async_document_client.replaceoffer) |

## <a name="stored-procedure-examples"></a>Saklı yordam örnekleri
[StoredProcedureAsyncAPITest](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/StoredProcedureAsyncAPITest.java) dosyasında aşağıdaki görevlerin nasıl gerçekleştirileceği gösterilir:

| Görev | API başvurusu |
| --- | --- |
| [Saklı yordam oluşturma ve çalıştırma](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/StoredProcedureAsyncAPITest.java#L108-L155) | [StoredProcedure](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb._stored_procedure)<br>[DocumentClient.createStoredProcedure](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx._async_document_client.createstoredprocedure)<br>[AsyncDocumentClient.executeStoredProcedure](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx._async_document_client.executestoredprocedure) |
| [Saklı yordamı bağımsız değişkenlerle oluşturma ve çalıştırma](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/StoredProcedureAsyncAPITest.java#L161-L195) | |
| [Saklı yordamı bir nesne bağımsız değişkeniyle oluşturma ve çalıştırma](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/StoredProcedureAsyncAPITest.java#L201-L241) | |

## <a name="unique-key"></a>Benzersiz Anahtar
[UniqueIndexAsyncAPITest](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/UniqueIndexAsyncAPITest.java) dosyası aşağıdaki görevlerin nasıl gerçekleştirileceğini gösterir:

| Görev | API başvurusu |
| --- | --- |
| [Benzersiz bir anahtarla bir koleksiyon oluşturma](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/UniqueIndexAsyncAPITest.java#L65-L97) | [UniqueKey](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb._unique_key)<br>[UniqueKeyPolicy](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb._unique_key_policy)<br>[DocumentCollection.setUniqueKeyPolicy](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb._document_collection.setuniquekeypolicy) |
