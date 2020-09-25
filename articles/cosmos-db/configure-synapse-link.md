---
title: Azure Cosmos DB için Azure SYNAPSE bağlantısı 'nı yapılandırma ve kullanma (Önizleme)
description: Azure Cosmos DB hesapları için SYNAPSE bağlantısının nasıl etkinleştirileceğini, analitik deponun etkinleştirildiği bir kapsayıcı oluşturmayı, Azure Cosmos veritabanını SYNAPSE çalışma alanına bağlamayı ve sorguları çalıştırmayı öğrenin.
author: Rodrigossz
ms.service: cosmos-db
ms.topic: how-to
ms.date: 08/31/2020
ms.author: rosouz
ms.custom: references_regions
ms.openlocfilehash: 559c596ed9b7412b277fbfc1cf30cad15e852cef
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91253165"
---
# <a name="configure-and-use-azure-synapse-link-for-azure-cosmos-db-preview"></a>Azure Cosmos DB için Azure SYNAPSE bağlantısı 'nı yapılandırma ve kullanma (Önizleme)

Azure Cosmos DB için SYNAPSE bağlantısı, Azure Cosmos DB ' de işletimsel veriler üzerinde neredeyse gerçek zamanlı analizler çalıştırmanıza olanak sağlayan, bulutta yerel bir karma işlem ve analitik işleme (HTAP) özelliğidir. SYNAPSE link, Azure Cosmos DB ile Azure SYNAPSE Analytics arasında sıkı sorunsuz bir tümleştirme oluşturur.

> [!IMPORTANT]
> Azure SYNAPSE bağlantısı kullanmak için, desteklenen bölgelerden birinde Azure SYNAPSE Analytics çalışma alanı & Azure Cosmos DB hesabınızı sağladığınızdan emin olun. Azure SYNAPSE bağlantısı şu anda şu Azure bölgelerinde sunulmaktadır: ABD Orta Batı, Doğu ABD, Batı ABD2, Kuzey Avrupa, Batı Avrupa, Orta Güney ABD, Güneydoğu Asya, Avustralya Doğu, Doğu U2, UK Güney.

Azure SYNAPSE link, Azure Cosmos DB SQL API kapsayıcıları veya Mongo DB koleksiyonları için Azure Cosmos DB API 'SI için kullanılabilir. Azure Cosmos DB için Azure SYNAPSE bağlantısı ile analitik sorgular çalıştırmak için aşağıdaki adımları kullanın:

* [Azure Cosmos DB hesaplarınız için SYNAPSE bağlantısını etkinleştirin](#enable-synapse-link)
* [Bir analitik depo Azure Cosmos DB kapsayıcısı oluşturma](#create-analytical-ttl)
* [Azure Cosmos DB veritabanınızı bir Synapse çalışma alanına bağlama](#connect-to-cosmos-database)
* [SYNAPSE Spark kullanarak analitik depoyu sorgulama](#query-analytical-store-spark)
* [SYNAPSE SQL sunucusuz kullanarak analitik depoyu sorgulama](#query-analytical-store-sql-on-demand)
* [Power BI verileri analiz etmek ve görselleştirmek için SYNAPSE SQL sunucusuz kullanın](#analyze-with-powerbi)

## <a name="enable-azure-synapse-link-for-azure-cosmos-db-accounts"></a><a id="enable-synapse-link"></a>Azure Cosmos DB hesapları için Azure SYNAPSE bağlantısını etkinleştirme

### <a name="azure-portal"></a>Azure portal

1. [Azure portal](https://portal.azure.com/) oturum açın.

1. [Yeni bir Azure hesabı oluşturun](create-sql-api-dotnet.md#create-account)veya mevcut bir Azure Cosmos DB hesabı seçin.

1. Azure Cosmos DB hesabınıza gidin ve **Özellikler** bölmesini açın.

1. Özellikler listesinden **SYNAPSE bağlantısı** ' nı seçin.

   :::image type="content" source="./media/configure-synapse-link/find-synapse-link-feature.png" alt-text="SYNAPSE link Preview özelliğini bulma":::

1. Bundan sonra hesabınızda SYNAPSE bağlantısını etkinleştirmenizi ister. **Etkinleştir**’i seçin. Bu işlemin tamamlanması 1 ila 5 dakika sürebilir.

   :::image type="content" source="./media/configure-synapse-link/enable-synapse-link-feature.png" alt-text="SYNAPSE Link özelliğini etkinleştir":::

1. Hesabınız artık SYNAPSE bağlantısını kullanacak şekilde etkinleştirilmiştir. Ardından, işletimsel verilerinizi işlemsel depodan analitik depoya çoğaltmaya otomatik olarak başlamak için bkz. analitik depo etkin kapsayıcıları oluşturma.

> [!NOTE]
> SYNAPSE bağlantısının etkinleştirilmesi, analitik depoyu otomatik olarak yapmaz. Cosmos DB hesapta SYNAPSE bağlantısını etkinleştirdikten sonra, işlem verilerinizi analitik depoya Çoğaltmaya başlamak için, onları oluşturduğunuz sırada kapsayıcı üzerinde analitik depoyu etkinleştirin. 

## <a name="create-an-azure-cosmos-container-with-analytical-store"></a><a id="create-analytical-ttl"></a> Analitik depo ile Azure Cosmos kapsayıcısı oluşturma

Kapsayıcıyı oluştururken Azure Cosmos kapsayıcısında analitik depoyu açabilirsiniz. Azure portal kullanabilir veya `analyticalTTL` Azure Cosmos DB SDK 'ları kullanarak kapsayıcı oluşturma sırasında özelliğini yapılandırabilirsiniz.

> [!NOTE]
> Şu anda analitik depoyu **Yeni** kapsayıcılar için (hem yeni hem de mevcut hesaplarda) etkinleştirebilirsiniz. [Azure Cosmos DB geçiş araçları](cosmosdb-migrationchoices.md) 'nı kullanarak, mevcut kapsayıcılarınızdaki verileri yeni kapsayıcılara geçirebilirsiniz.

### <a name="azure-portal"></a>Azure portal

1. [Azure Portal](https://portal.azure.com/) veya [Azure Cosmos Gezgini](https://cosmos.azure.com/)' nde oturum açın.

1. Azure Cosmos DB hesabınıza gidin ve **Veri Gezgini** sekmesini açın.

1. **Yeni kapsayıcı** ' yı seçin ve veritabanınız, Kapsayıcınız, Bölüm anahtarınız ve verimlilik ayrıntılarınız için bir ad girin. **Analitik depo** seçeneğini açın. Analitik depoyu etkinleştirdikten sonra, `AnalyicalTTL` özelliği-1 varsayılan değerine ayarlanmış bir kapsayıcı oluşturur (sonsuz saklama). Kayıtların tüm geçmiş sürümlerini koruyan bu analitik depo.

   :::image type="content" source="./media/configure-synapse-link/create-container-analytical-store.png" alt-text="Azure Cosmos kapsayıcısı için analitik depoyu aç":::

1. Bu hesapta daha önce SYNAPSE bağlantısı etkinleştirilmemişse, analitik depo özellikli bir kapsayıcı oluşturmak için önkoşul olduğundan bunu yapmanız istenir. İstenirse, **SYNAPSE bağlantısını etkinleştir**' i seçin. Bu işlemin tamamlanması 1 ila 5 dakika sürebilir.

1. Azure Cosmos kapsayıcısını etkin bir analitik mağaza oluşturmak için **Tamam**' ı seçin.

1. Kapsayıcı oluşturulduktan sonra, **Ayarlar**' a tıklayıp Veri Gezgini ' deki belgeler ' e tıklayarak analitik deponun etkinleştirildiğini doğrulayın ve **analitik depo süresinin canlı** olup olmadığını denetleyin.

### <a name="net-sdk"></a>.NET SDK

Aşağıdaki kod, .NET SDK kullanarak analitik depolarla bir kapsayıcı oluşturur. Analitik TTL özelliğini gerekli değere ayarlayın. İzin verilen değerler listesi için bkz. [ANALITIK TTL desteklenen değerler](analytical-store-introduction.md#analytical-ttl) makalesi:

```csharp
// Create a container with a partition key, and analytical TTL configured to  -1 (infinite retention)
string containerId = “myContainerName”;
int analyticalTtlInSec = -1;
ContainerProperties cpInput = new ContainerProperties()
            {
Id = containerId,
PartitionKeyPath = "/id",
AnalyticalStorageTimeToLiveInSeconds = analyticalTtlInSec,
};
 await this. cosmosClient.GetDatabase("myDatabase").CreateContainerAsync(cpInput);
```

### <a name="java-v4-sdk"></a>Java v4 SDK 'Sı

Aşağıdaki kod, Java v4 SDK 'sını kullanarak analitik depolarla bir kapsayıcı oluşturur. `AnalyticalStoreTimeToLiveInSeconds`Özelliği gerekli değere ayarlayın:

```java
// Create a container with a partition key and  analytical TTL configured to  -1 (infinite retention) 
CosmosContainerProperties containerProperties = new CosmosContainerProperties("myContainer", "/myPartitionKey");

containerProperties.setAnalyticalStoreTimeToLiveInSeconds(-1);

container = database.createContainerIfNotExists(containerProperties, 400).block().getContainer();
```

### <a name="python-v4-sdk"></a>Python v4 SDK 'Sı

Python 2,7 ve Azure Cosmos DB SDK 4.1.0, gereken en düşük sürümlerdir ve SDK yalnızca SQL API ile uyumludur.

İlk adım, [Azure Cosmos DB Python SDK 'sının](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cosmos/azure-cosmos)en az sürüm 4.1.0 kullandığınızdan emin olmak için gereklidir:

```python
import azure.cosmos as cosmos

print (cosmos.__version__)
```
Sonraki adım, Azure Cosmos DB Python SDK 'sını kullanarak analitik depolarla bir kapsayıcı oluşturur:

```python
# Azure Cosmos DB Python SDK, for SQL API only.
# Creating an analytical store enabled container.

import azure.cosmos.cosmos_client as cosmos_client
import azure.cosmos.exceptions as exceptions
from azure.cosmos.partition_key import PartitionKey

HOST = 'your-cosmos-db-account-URI'
KEY = 'your-cosmos-db-account-key'
DATABASE = 'your-cosmos-db-database-name'
CONTAINER = 'your-cosmos-db-container-name'

client = cosmos_client.CosmosClient(HOST,  KEY )
# setup database for this sample. 
# If doesn't exist, creates a new one with the name informed above.
try:
    db = client.create_database(DATABASE)

except exceptions.CosmosResourceExistsError:
    db = client.get_database_client(DATABASE)

# Creating the container with analytical store enabled, using the name informed above.
# If a container with the same name exists, an error is returned.
#
# The 3 options for the analytical_storage_ttl parameter are:
# 1) 0 or Null or not informed (Not enabled).
# 2) -1 (The data will be stored in analytical store infinitely).
# 3) Any other number is the actual ttl, in seconds.

try:
    container = db.create_container(
        id=CONTAINER,
        partition_key=PartitionKey(path='/id', kind='Hash'),analytical_storage_ttl=-1
    )
    properties = container.read()
    print('Container with id \'{0}\' created'.format(container.id))
    print('Partition Key - \'{0}\''.format(properties['partitionKey']))

except exceptions.CosmosResourceExistsError:
    print('A container with already exists')
```

### <a name="update-the-analytical-store-time-to-live"></a><a id="update-analytical-ttl"></a> Analitik depo zamanını canlı olarak güncelleştirme

Analiz deposunu belirli bir TTL değeriyle etkinleştirdikten sonra ilerleyen zamanlarda farklı bir geçerli değer kullanarak güncelleştirebilirsiniz. Değeri güncelleştirmek için Azure portalını veya SDK'ları kullanabilirsiniz. Çeşitli analitik TTL yapılandırma seçenekleri hakkında daha fazla bilgi için bkz. [ANALITIK TTL desteklenen değerler](analytical-store-introduction.md#analytical-ttl) makalesi.

#### <a name="azure-portal"></a>Azure portal

Azure portal aracılığıyla analitik mağaza etkin bir kapsayıcı oluşturduysanız, varsayılan bir analitik TTL-1 ' i içerir. Bu değeri güncelleştirmek için aşağıdaki adımları kullanın:

1. [Azure Portal](https://portal.azure.com/) veya [Azure Cosmos Gezgini](https://cosmos.azure.com/)' nde oturum açın.

1. Azure Cosmos DB hesabınıza gidin ve **Veri Gezgini** sekmesini açın.

1. Analitik deponun etkinleştirildiği mevcut bir kapsayıcıyı seçin. Genişletin ve aşağıdaki değerleri değiştirin:

  * **Ölçek & ayarları** penceresini açın.
  * Bul **ayarı** altında, * * gerçek zamanlı analitik depolama süresi * *.
  * **Açık (varsayılan)** **seçeneğini belirleyin veya SEÇIN ve bir** TTL değeri ayarlayın
  * Değişiklikleri kaydetmek için **Kaydet**’e tıklayın.

#### <a name="net-sdk"></a>.NET SDK

Aşağıdaki kod, .NET SDK kullanarak analitik depo için TTL 'nin nasıl güncelleştirilmesini göstermektedir:

```csharp
// Get the container, update AnalyticalStorageTimeToLiveInSeconds 
ContainerResponse containerResponse = await client.GetContainer("database", "container").ReadContainerAsync();
// Update analytical store TTL
containerResponse.Resource. AnalyticalStorageTimeToLiveInSeconds = 60 * 60 * 24 * 180  // Expire analytical store data in 6 months;
await client.GetContainer("database", "container").ReplaceContainerAsync(containerResponse.Resource);
```

#### <a name="java-v4-sdk"></a>Java v4 SDK 'Sı

Aşağıdaki kod, Java v4 SDK 'sını kullanarak analitik depo için TTL 'nin nasıl güncelleştirilmesini göstermektedir:

```java
CosmosContainerProperties containerProperties = new CosmosContainerProperties("myContainer", "/myPartitionKey");

// Update analytical store TTL to expire analytical store data in 6 months;
containerProperties.setAnalyticalStoreTimeToLiveInSeconds (60 * 60 * 24 * 180 );  
 
// Update container settings
container.replace(containerProperties).block();
```

## <a name="connect-to-a-synapse-workspace"></a><a id="connect-to-cosmos-database"></a> Bir Synapse çalışma alanına bağlanma

Azure SYNAPSE link ile Azure SYNAPSE Analytics Studio 'dan bir Azure Cosmos DB veritabanına nasıl eriştiğine ilişkin Azure [SYNAPSE Connect](../synapse-analytics/synapse-link/how-to-connect-synapse-link-cosmos-db.md) 'teki yönergeleri kullanın.

## <a name="query-analytical-store-using-apache-spark-for-azure-synapse-analytics"></a><a id="query-analytical-store-spark"></a> Azure SYNAPSE Analytics için Apache Spark kullanarak analitik depoyu sorgulama

SYNAPSE Spark ile sorgulama hakkında [analitik depo makalesindeki sorgu Azure Cosmos DB sorgulama](../synapse-analytics/synapse-link/how-to-query-analytical-store-spark.md) bölümündeki yönergeleri kullanın. Bu makalede, SYNAPSE hareketlerinden analitik depoyla nasıl etkileşim kurabileceğine ilişkin bazı örnekler verilmektedir. Bir kapsayıcıya sağ tıkladığınızda bu hareketler görünür. Hareketlerle hızlı bir şekilde kod oluşturabilir ve bunu gereksinimlerinize ince ayar edebilirsiniz. Tek bir tıklama ile verileri bulmak için de mükemmeldir.

## <a name="query-the-analytical-store-using-synapse-sql-serverless"></a><a id="query-analytical-store-sql-on-demand"></a> SYNAPSE SQL sunucusuz kullanarak analitik depoyu sorgulama

SYNAPSE SQL sunucusuz (daha önce **isteğe bağlı SQL**olarak adlandırılan bir önizleme özelliği), Azure SYNAPSE bağlantısı ile etkinleştirilen Azure Cosmos DB kapsayıcılarınızdaki verileri sorgulamanızı ve çözümlemenize olanak tanır. İşlemsel iş yüklerinizin performansını etkilemeden verileri neredeyse gerçek zamanlı olarak analiz edebilirsiniz. Analitik depodan veri sorgulama ve T-SQL arabirimi aracılığıyla çok çeşitli bı ve geçici sorgulama araçlarıyla tümleşik bağlantı sunan tanıdık bir T-SQL söz dizimi sunar. Daha fazla bilgi için bkz. [SYNAPSE SQL sunucusuz makalesini kullanarak analitik depoyu sorgulama](../synapse-analytics/sql/query-cosmos-db-analytical-store.md) .

> [!NOTE]
> SYNAPSE SQL sunucusuz ile Azure Cosmos DB analitik deposunun kullanılması şu anda geçitli önizleme aşamasındadır. Erişim istemek için [Azure Cosmos DB ekibine](mailto:cosmosdbsynapselink@microsoft.com)ulaşın.

## <a name="use-synapse-sql-serverless-to-analyze-and-visualize-data-in-power-bi"></a><a id="analyze-with-powerbi"></a>Power BI verileri analiz etmek ve görselleştirmek için SYNAPSE SQL sunucusuz kullanın

Azure Cosmos DB için bir Synapse SQL sunucusuz veritabanı ve görünümleri SYNAPSE bağlantısı üzerinden oluşturabilirsiniz. Daha sonra, Azure Cosmos kapsayıcılarını sorgulayabilir ve ardından bu görünümler üzerinde Power BI olan ve bu sorguyu yansıtan bir model oluşturabilirsiniz. Daha fazla bilgi edinmek için bkz. [SYNAPSE link ile Azure Cosmos DB verilerini çözümlemek Için SYNAPSE SQL sunucusuz](synapse-link-power-bi.md) kullanma makalesine bakın.

## <a name="azure-resource-manager-template"></a>Azure Resource Manager şablonu

[Azure Resource Manager şablonu](manage-sql-with-resource-manager.md#azure-cosmos-account-with-analytical-store) , SQL API 'si için etkin bir Synapse bağlantısı Azure Cosmos DB hesabı oluşturur. Bu şablon, analitik TTL etkin olarak yapılandırılmış bir kapsayıcı içeren bir bölgede bir çekirdek (SQL) API hesabı oluşturur ve el ile veya otomatik ölçeklendirme üretimi kullanma seçeneği sağlar. Bu şablonu dağıtmak için Benioku sayfasında **Azure 'A dağıt** ' a tıklayın.

## <a name="getting-started-with-azure-synpase-link---samples"></a><a id="cosmosdb-synapse-link-samples"></a> Azure Synpao bağlantısı ile çalışmaya başlama-örnekler

[GitHub](https://aka.ms/cosmosdb-synapselink-samples)'Da Azure SYNAPSE bağlantısı ile çalışmaya başlama örneklerini bulabilirsiniz. Bu, IoT ve perakende senaryolarıyla uçtan uca çözümler sergileme. MongoDB için Azure Cosmos DB API 'sine karşılık gelen örnekleri [MongoDB](https://github.com/Azure-Samples/Synapse/tree/master/Notebooks/PySpark/Synapse%20Link%20for%20Cosmos%20DB%20samples/MongoDB) klasörü altındaki aynı depoda da bulabilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki belgelere bakın:

* [Azure Cosmos DB için Azure SYNAPSE bağlantısı.](synapse-link.md)

* [Azure Cosmos DB analiz deposuna genel bakış.](analytical-store-introduction.md)

* [Azure Cosmos DB için SYNAPSE bağlantısı hakkında sık sorulan sorular.](synapse-link-frequently-asked-questions.md)

* [Azure SYNAPSE Analytics 'te Apache Spark](../synapse-analytics/spark/apache-spark-concepts.md).

* [Azure SYNAPSE Analytics 'Te SQL sunucusuz çalışma zamanı desteği](../synapse-analytics/sql/on-demand-workspace-overview.md).
