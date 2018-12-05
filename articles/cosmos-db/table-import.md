---
title: Azure Cosmos DB Tablo API'si ile kullanmak üzere veri içe aktarma
description: Azure Cosmos DB Tablo API'si ile kullanmak üzere veri içe aktarma hakkında bilgi edinin.
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.component: cosmosdb-table
ms.topic: tutorial
ms.date: 11/28/2017
ms.author: sngun
ms.openlocfilehash: d277df3fa98564ebec92e548fa070e92cf76f200
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52874048"
---
# <a name="migrate-your-data-to-azure-cosmos-db-table-api-account"></a>Verilerinizi Azure Cosmos DB Tablo API'si hesabına geçirme

Bu öğretici, Azure Cosmos DB [Tablo API’si](table-introduction.md) ile kullanmak üzere veri içeri aktarma hakkında yönergeler sağlar. Azure Tablo depolama alanında depolanan verileriniz varsa verilerinizi Azure Cosmos DB Tablo API'sine aktarmak için Veri Taşıma Aracı'nı veya AzCopy'yi kullanabilirsiniz. Bir Azure Cosmos DB Tablo API'si (önizleme) hesabında depolanan verileriniz varsa, verilerinizi taşımak için Veri Taşıma Aracı'nı kullanmanız gerekir. 

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Veri Taşıma Aracı ile veri içeri aktarma
> * AzCopy ile veri içeri aktarma
> * Tablo API’sinden (önizleme) Tablo API’sine geçiş 

## <a name="prerequisites"></a>Önkoşullar

* **Aktarım hızını artırma:** Veri geçişinizin süresi, tek bir kapsayıcı veya bir kapsayıcı kümesi için ayarladığınız aktarım hızı miktarına bağlıdır. Büyük veri geçişleri için aktarım hızını artırdığınızdan emin olun. Geçişi tamamladıktan sonra maliyet tasarrufu sağlamak için aktarım hızını azaltın. Azure portalda aktarım hızını artırma hakkında daha fazla bilgi için bkz. Azure Cosmos DB’de performans düzeyleri ve fiyatlandırma katmanları.

* **Azure Cosmos DB kaynaklarını oluşturma:** Veri geçişini başlatmadan önce Azure portaldan tüm tablolarınızı oluşturun. Veritabanı düzeyinde aktarım hızına sahip olan bir Azure Cosmos DB hesabına geçiş yapıyorsanız Azure Cosmos DB tablolarını oluştururken bölüm anahtarı girdiğinizden emin olun.

## <a name="data-migration-tool"></a>Veri Taşıma aracı

Komut satırı Azure Cosmos DB Veri Taşıma Aracı (dt.exe), var olan Azure Table depolama verilerinizi bir Tablo API GA hesabına içeri aktarmak veya verileri bir Tablo API (önizleme) hesabından bir Tablo API GA hesabına taşımak için kullanılabilir. Diğer kaynaklar şu anda desteklenmemektedir. Kullanıcı arabirimi temelli Veri Taşıma Aracı (dtui.exe), Tablo API hesapları için şu anda desteklenmemektedir. 

Tablo verilerinin geçişini gerçekleştirmek için aşağıdaki görevleri tamamlayın:

1. Geçiş aracını [GitHub](https://github.com/azure/azure-documentdb-datamigrationtool)'dan indirin.
2. `dt.exe` aracını, senaryonuzun komut satırı bağımsız değişkenlerini kullanarak çalıştırın. `dt.exe` aşağıdaki biçimde bir komut alır:

   ```bash
    dt.exe [/<option>:<value>] /s:<source-name> [/s.<source-option>:<value>] /t:<target-name> [/t.<target-option>:<value>] 
```

Komutun seçenekleri şunlardır:

    /ErrorLog: Optional. Name of the CSV file to redirect data transfer failures
    /OverwriteErrorLog: Optional. Overwrite error log file
    /ProgressUpdateInterval: Optional, default is 00:00:01. Time interval to refresh on-screen data transfer progress
    /ErrorDetails: Optional, default is None. Specifies that detailed error information should be displayed for the following errors: None, Critical, All

### <a name="command-line-source-settings"></a>Komut satırı kaynak ayarları

Azure Tablo Depolama veya Tablo API önizlemesini geçişin kaynağı olarak tanımlarken aşağıdaki kaynak seçeneklerini kullanın.

    /s:AzureTable: Reads data from Azure Table storage
    /s.ConnectionString: Connection string for the table endpoint. This can be retrieved from the Azure portal
    /s.LocationMode: Optional, default is PrimaryOnly. Specifies which location mode to use when connecting to Azure Table storage: PrimaryOnly, PrimaryThenSecondary, SecondaryOnly, SecondaryThenPrimary
    /s.Table: Name of the Azure Table
    /s.InternalFields: Set to All for table migration as RowKey and PartitionKey are required for import.
    /s.Filter: Optional. Filter string to apply
    /s.Projection: Optional. List of columns to select

Azure tablo depolamasından içeri aktarırken kaynak bağlantı dizesini almak için Azure Portal’ı açın ve **Depolama hesapları** > **Hesap** > **Erişim anahtarları**'na tıklayın ve **Bağlantı dizesini** kopyalamak için kopyalama düğmesini kullanın.

![HBase kaynağı seçeneklerinin ekran görüntüsü](./media/table-import/storage-table-access-key.png)

Bir Azure Cosmos DB Tablo API (önizleme) hesabından içeri aktarma sırasında kaynak bağlantı dizesini almak için, Azure portalı açın, **Azure Cosmos DB** > **Hesap** > **Bağlantı Dizesi**'ni tıklatın ve **Bağlantı Dizesi**'ni kopyalamak için kopyalama düğmesini kullanın.

![HBase kaynağı seçeneklerinin ekran görüntüsü](./media/table-import/cosmos-connection-string.png)

[Azure Tablo Depolama komutu örneği](#azure-table-storage)

[Azure Cosmos DB Tablo API’si (önizleme) komutu örneği](#table-api-preview)

### <a name="command-line-target-settings"></a>Komut satırı hedefi ayarları

Azure Cosmos DB Tablo API'sini geçiş hedefi olarak tanımlarken aşağıdaki hedef seçeneklerini kullanın.

    /t:TableAPIBulk: Uploads data into Azure CosmosDB Table in batches
    /t.ConnectionString: Connection string for the table endpoint
    /t.TableName: Specifies the name of the table to write to
    /t.Overwrite: Optional, default is false. Specifies if existing values should be overwritten
    /t.MaxInputBufferSize: Optional, default is 1GB. Approximate estimate of input bytes to buffer before flushing data to sink
    /t.Throughput: Optional, service defaults if not specified. Specifies throughput to configure for table
    /t.MaxBatchSize: Optional, default is 2MB. Specify the batch size in bytes

<a id="azure-table-storage"></a>
### <a name="sample-command-source-is-azure-table-storage"></a>Örnek komut: Kaynak Azure Table depolaması

Azure Tablo depolamasından Tablo API'sine içeri aktarmayı gösteren bir komut satırı örneği:

```
dt /s:AzureTable /s.ConnectionString:DefaultEndpointsProtocol=https;AccountName=<Azure Table storage account name>;AccountKey=<Account Key>;EndpointSuffix=core.windows.net /s.Table:<Table name> /t:TableAPIBulk /t.ConnectionString:DefaultEndpointsProtocol=https;AccountName=<Azure Cosmos DB account name>;AccountKey=<Azure Cosmos DB account key>;TableEndpoint=https://<Account name>.table.cosmosdb.azure.com:443 /t.TableName:<Table name> /t.Overwrite
```
<a id="table-api-preview"></a>
### <a name="sample-command-source-is-azure-cosmos-db-table-api-preview"></a>Örnek komut: Kaynak Azure Cosmos DB Tablo API (önizleme)

Tablo API önizleme sürümünden Table API GA'ya içeri aktarma için bir komut satırı örneği:

```
dt /s:AzureTable /s.ConnectionString:DefaultEndpointsProtocol=https;AccountName=<Table API preview account name>;AccountKey=<Table API preview account key>;TableEndpoint=https://<Account Name>.documents.azure.com; /s.Table:<Table name> /t:TableAPIBulk /t.ConnectionString:DefaultEndpointsProtocol=https;AccountName=<Azure Cosmos DB account name>;AccountKey=<Azure Cosmos DB account key>;TableEndpoint=https://<Account name>.table.cosmosdb.azure.com:443 /t.TableName:<Table name> /t.Overwrite
```

## <a name="migrate-data-by-using-azcopy"></a>AzCopy'yi kullanarak veri geçirme

Azure Tablo depolamasından Azure Cosmos DB Tablo API'sine veri geçirmek için AzCopy komut satırı yardımcı programı da kullanılabilir. AzCopy'yi kullanmak için önce verilerinizi [Tablo deposundan veri dışarı aktarma](../storage/common/storage-use-azcopy.md#export-data-from-table-storage)'da açıklandığı gibi dışarı aktarır, sonra bu verileri [Azure Cosmos DB Tablo API](../storage/common/storage-use-azcopy.md#import-data-into-table-storage)'de açıklandığı gibi Azure Cosmos DB'ye içeri aktarırsınız.

Azure Cosmos DB'ye içeri aktarmayı gerçekleştirirken aşağıdaki örneğe bakın. /Dest değerinin core değil cosmosdb kullandığını unutmayın.

Örnek içeri aktarma komutu:

```
AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.table.cosmosdb.windows.net/mytable1/ /DestKey:key /Manifest:"myaccount_mytable_20140103T112020.manifest" /EntityOperation:InsertOrReplace
```

## <a name="migrate-from-table-api-preview-to-table-api"></a>Tablo API’sinden (önizleme) Tablo API’sine geçiş

> [!WARNING]
> Genel olarak kullanılabilen tabloların avantajlarından hemen yararlanmak istiyorsanız, lütfen var olan önizleme tablolarınızı bu bölümde belirtildiği şekilde geçirin. Aksi halde mevcut önizleme müşterileri için önümüzdeki haftalarda otomatik taşımalar gerçekleştireceğiz, ancak otomatik taşınan önizleme tablolarında yeni oluşturulan tablolarda olmayan bazı kısıtlamalar olacak.
> 

Tablo API genel kullanıma (GA) sunulmuştur. Tabloların önizleme ve GA sürümleri arasında gerek bulutta çalışan kodda, gerek istemcide çalışan kodda farklar vardır. Bu nedenle bir önizleme SDK istemcisini bir GA Tablo API hesabıyla karıştırmayı veya bunun aksini denemek önerilmez. Mevcut tablolarını kullanmaya devam etmek isteyen ancak bir üretim ortamında önizleme GA ortamına geçmesi gereken Tablo API önizleme müşterileri otomatik geçişi beklemelidir. Otomatik geçişi beklerseniz, taşınan tablolardaki kısıtlamalar size bildirilir. Geçişten sonra mevcut hesabınızda kısıtlamaları olmayan yeni tablolar oluşturabilirsiniz (yalnızca geçirilen tabloların kısıtlamaları olacaktır).

Tablo API önizleme sürümünden genel olarak kullanılabilen Tablo API'sine geçiş için:

1. Yeni bir Azure Cosmos DB hesabı oluşturun ve API türünü [Veritabanı hesabı oluşturma](create-table-dotnet.md#create-a-database-account)'da açıklandığı gibi Azure Tablosu olarak ayarlayın.

2. İstemcileri [Tablo API SDK'larının](table-sdk-dotnet.md) GA sürümünü kullanacak şekilde değiştirin.

3. Veri Taşıma Aracı'nı kullanarak istemci veritabanını önizleme tablolarından GA tablolarına geçirin. Veri taşıma aracının bu amaçla kullanım yönergeleri [Veri Taşıma Aracı](#data-migration-tool)'nda açıklanmaktadır. 

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Veri Taşıma Aracı ile veri içeri aktarma
> * AzCopy ile veril içeri aktarma
> * Tablo API’sinden (önizleme) Tablo API’sine geçiş

Şimdi sonraki öğreticiye devam edebilir ve Azure Cosmos DB Tablo API’sini kullanarak veri sorgulamayı öğrenebilirsiniz. 

> [!div class="nextstepaction"]
>[Veriler nasıl sorgulanır?](../cosmos-db/tutorial-query-table.md)
