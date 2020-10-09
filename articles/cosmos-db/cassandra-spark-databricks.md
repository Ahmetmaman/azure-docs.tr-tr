---
title: Azure Databricks Azure Cosmos DB Cassandra API erişim
description: Bu makalede Azure Databricks Azure Cosmos DB Cassandra API ile nasıl çalışılacağı ele alınmaktadır.
author: kanshiG
ms.author: govindk
ms.reviewer: sngun
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: how-to
ms.date: 09/24/2018
ms.openlocfilehash: 30bd3187973de204f27a3be3862351550d6a56f6
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "85260483"
---
# <a name="access-azure-cosmos-db-cassandra-api-data-from-azure-databricks"></a>Azure Databricks Azure Cosmos DB Cassandra API verilere erişin

Bu makalede, [Azure Databricks](https://docs.microsoft.com/azure/azure-databricks/what-is-azure-databricks)Spark 'tan Azure Cosmos DB Cassandra API nasıl çalıştığı açıklanır.

## <a name="prerequisites"></a>Önkoşullar

* [Azure Cosmos DB Cassandra API hesabı sağlama](create-cassandra-dotnet.md#create-a-database-account)

* [Azure Cosmos DB bağlanma temellerini inceleyin Cassandra API](cassandra-spark-generic.md)

* [Azure Databricks kümesi sağlama](../azure-databricks/quickstart-create-databricks-workspace-portal.md)

* [Cassandra API ile çalışmak için kod örneklerini gözden geçirin](cassandra-spark-generic.md#next-steps)

* [İsterseniz doğrulama için csqlsh kullanın](cassandra-spark-generic.md#connecting-to-azure-cosmos-db-cassandra-api-from-spark)

* **Cassandra Bağlayıcısı için Cassandra API örneği yapılandırması:**

  Cassandra API Bağlayıcısı, Cassandra bağlantı ayrıntılarının Spark bağlamının bir parçası olarak başlatılmasını gerektirir. Bir Databricks Not defteri başlattığınızda Spark bağlamı zaten başlatılmış ve durdurulması ve yeniden başlatılması önerilmez. Bir çözüm, küme Spark yapılandırmasındaki Cassandra API örneği yapılandırmasını bir küme düzeyinde eklemektir. Bu, küme başına tek seferlik bir etkinliktir. Spark yapılandırmasına, boşlukla ayrılmış bir anahtar değer çifti olarak aşağıdaki kodu ekleyin:
 
  ```scala
  spark.cassandra.connection.host YOUR_COSMOSDB_ACCOUNT_NAME.cassandra.cosmosdb.azure.com
  spark.cassandra.connection.port 10350
  spark.cassandra.connection.ssl.enabled true
  spark.cassandra.auth.username YOUR_COSMOSDB_ACCOUNT_NAME
  spark.cassandra.auth.password YOUR_COSMOSDB_KEY
  ```

## <a name="add-the-required-dependencies"></a>Gerekli bağımlılıkları ekleme

* **Cassandra Spark Bağlayıcısı:** -Azure Cosmos DB Cassandra API Spark ile bütünleştirmek Için, Cassandra Bağlayıcısı Azure Databricks kümesine eklenmelidir. Kümeyi eklemek için:

  * Spark sürümü olan Databricks çalışma zamanı sürümünü gözden geçirin. Ardından, Cassandra Spark Bağlayıcısı ile uyumlu olan [Maven koordinatlarını](https://mvnrepository.com/artifact/com.datastax.spark/spark-cassandra-connector) bulun ve kümeye ekleyin. Bağlayıcı kitaplığını kümeye eklemek için ["Maven paketi veya Spark paketini karşıya yükleme"](https://docs.databricks.com/user-guide/libraries.html) makalesini inceleyin. Örneğin, "Databricks Runtime Version 4,3", "Spark 2.3.1" ve "Scala 2,11" için Maven koordinatı `spark-cassandra-connector_2.11-2.3.1`

* **Cassandra API özel kitaplığı Azure Cosmos DB:** -yeniden deneme Ilkesini Cassandra Spark bağlayıcısından Azure Cosmos DB Cassandra API kadar yapılandırmak için özel bir bağlantı fabrikası gerekir. `com.microsoft.azure.cosmosdb:azure-cosmos-cassandra-spark-helper:1.0.0`Kitaplığı kümeye eklemek için [Maven koordinatlarını](https://search.maven.org/artifact/com.microsoft.azure.cosmosdb/azure-cosmos-cassandra-spark-helper/1.0.0/jar) ekleyin.

## <a name="sample-notebooks"></a>Örnek not defterleri

Azure Databricks [örnek Not defterlerinin](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-api-spark-notebooks-databricks/tree/master/notebooks/scala) listesi, GitHub deposunda indirileceği için kullanılabilir. Bu örnekler, Spark 'tan Azure Cosmos DB Cassandra API bağlanmayı ve veriler üzerinde farklı CRUD işlemleri yapmayı içerir. Ayrıca [, tüm not defterlerini](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-api-spark-notebooks-databricks/tree/master/dbc) Databricks kümesi çalışma alanınıza aktarabilir ve çalıştırabilirsiniz. 

## <a name="accessing-azure-cosmos-db-cassandra-api-from-spark-scala-programs"></a>Spark Scala programlarından Azure Cosmos DB Cassandra API erişme

Azure Databricks üzerinde otomatikleştirilmiş işlemler olarak çalıştırılacak Spark programları [Spark-gönder](https://spark.apache.org/docs/latest/submitting-applications.html)kullanılarak kümeye gönderilir ve Azure Databricks işleri aracılığıyla çalıştırılmak üzere zamanlanır.

Aşağıda, Azure Cosmos DB Cassandra API etkileşimde bulunmak için Spark Scala programları oluşturmaya başlamanıza yardımcı olacak bağlantılar yer alır.
* [Spark Scala programından Azure Cosmos DB Cassandra API bağlama](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-api-spark-connector-sample/blob/master/src/main/scala/com/microsoft/azure/cosmosdb/cassandra/SampleCosmosDBApp.scala)
* [Spark Scala programını Azure Databricks üzerinde otomatik iş olarak çalıştırma](https://docs.azuredatabricks.net/user-guide/jobs.html)
* [Cassandra API ile çalışmak için kod örneklerinin tüm listesi](cassandra-spark-generic.md#next-steps)

## <a name="next-steps"></a>Sonraki adımlar

Java uygulaması kullanarak [Cassandra API hesabı, veritabanı ve tablo oluşturmaya](create-cassandra-api-account-java.md) başlayın.
