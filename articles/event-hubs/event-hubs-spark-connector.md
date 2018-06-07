---
title: Apache Spark Azure Event Hubs ile tümleştirme | Microsoft Docs
description: Event Hubs ile yapılandırılmış akışını etkinleştirmek için Apache Spark ile tümleştirme
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: ''
ms.service: event-hubs
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/21/2018
ms.author: sethm
ms.openlocfilehash: 9f1cf75fdea1dd7f5842c2efdaeca663d611065c
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34626930"
---
# <a name="integrating-apache-spark-with-azure-event-hubs"></a>Apache Spark Azure Event Hubs ile tümleştirme

Azure Event Hubs ile sorunsuz şekilde tümleşir [Apache Spark](https://spark.apache.org/) yapı etkinleştirmek için akış uygulamaları dağıtılmış. Bu tümleştirmeyi destekler [Spark Core](http://spark.apache.org/docs/latest/rdd-programming-guide.html), [Spark akış](http://spark.apache.org/docs/latest/streaming-programming-guide.html), ve [yapılandırılmış akış](https://spark.apache.org/docs/latest/structured-streaming-programming-guide.html). Olay hub'ları bağlayıcı Apache Spark için kullanılabilir [GitHub](https://github.com/Azure/azure-event-hubs-spark). Bu kitaplığı da Maven projelerden kullanmak için kullanılabilir [Maven merkezi bir depoya](http://search.maven.org/#artifactdetails%7Ccom.microsoft.azure%7Cazure-eventhubs-spark_2.11%7C2.1.6%7C).

Bu makalede sürekli bir uygulama oluşturmak nasıl [Azure Databricks](https://azure.microsoft.com/services/databricks/). Bu makalede Azure Databricks kullanırken, Spark kümeleri de ile kullanılabilen [Hdınsight](../hdinsight/spark/apache-spark-overview.md).

Bu makaledeki örnek iki Scala not defterlerini kullanır: biri olayları bir event hub ve olayları yeniden göndermek için başka bir akış.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Biri, yoksa [ücretsiz bir hesap oluşturma](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).
* Bir olay hub'ları örneği. Biri, yoksa [oluşturmak](event-hubs-create.md).
* Bir [Azure Databricks](https://azure.microsoft.com/services/databricks/) örneği. Biri, yoksa [oluşturmak](../azure-databricks/quickstart-create-databricks-workspace-portal.md).
* [Maven koordinatları kullanarak bir kitaplığı oluşturun](https://docs.databricks.com/user-guide/libraries.html#upload-a-maven-package-or-spark-package): `com.microsoft.azure:azure‐eventhubs‐spark_2.11:2.3.1`.

Aşağıdaki kodu kullanarak olay hub'ınız olaylarından akış:

```scala
import org.apache.spark.eventhubs._

// To connect to an event hub, EntityPath is required as part of the connection string.
// Here, we assume that the connection string from the Azure portal does not have the EntityPath part.
val connectionString = ConnectionStringBuilder("{EVENT HUB CONNECTION STRING FROM AZURE PORTAL}")
  .setEventHubName("{EVENT HUB NAME}")
  .build 
val ehConf = EventHubsConf(connectionString)
  .setStartingPosition(EventPosition.fromEndOfStream)

// Create a stream that reads data from the specified Event Hub.
val reader = spark.readStream
  .format("eventhubs")
  .options(ehConf.toMap)
  .load()
val eventhubs = reader.select($"body" cast "string")

eventhubs.writeStream
  .format("console")
  .outputMode("append")
  .start()
  .awaitTermination()
```
Aşağıdaki kod, Spark batch API'leri ile olay hub'ınıza olayları gönderir. Ayrıca, olay hub'ına olayları göndermek için akış için sorgu yazabilirsiniz:

```scala
import org.apache.spark.eventhubs._
import org.apache.spark.sql.functions._

// To connect to an event hub, EntityPath is required as part of the connection string.
// Here, we assume that the connection string from the Azure portal does not have the EntityPath part.
val connectionString = ConnectionStringBuilder("{EVENT HUB CONNECTION STRING FROM AZURE PORTAL}")
  .setEventHubName("{EVENT HUB NAME}")
  .build
val ehConf = EventHubsConf(connectionString)

// Create random strings as the body of the message.
val bodyColumn = concat(lit("random nunmber: "), rand()).as("body")

// Write 200 rows to the specified event hub.
val df = spark.range(200).select(bodyColumn)
df.write
  .format("eventhubs")
  .options(ehConf.toMap)
  .save() 
```

## <a name="next-steps"></a>Sonraki adımlar

Olay hub'ları bağlayıcı için Apache Spark kullanarak ölçeklenebilir, hataya dayanıklı bir akış ayarlamak nasıl biliyorsunuz. Bu bağlantıları takip ederek olay hub'ları yapılandırılmış akış ve Spark akış ile kullanma hakkında daha fazla bilgi edinin:

* [Yapılandırılmış akış + Azure olay hub'ları tümleştirme Kılavuzu](https://github.com/Azure/azure-event-hubs-spark/blob/master/docs/structured-streaming-eventhubs-integration.md)
* [Spark akış + olay hub'ları tümleştirme Kılavuzu](https://github.com/Azure/azure-event-hubs-spark/blob/master/docs/spark-streaming-eventhubs-integration.md)
