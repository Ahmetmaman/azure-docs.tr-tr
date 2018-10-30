---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/26/2018
ms.author: tamram
ms.openlocfilehash: 042aedf1a043cd89d74ff099554642d38a3c7dd3
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2018
ms.locfileid: "50165881"
---
## <a name="what-is-table-storage"></a>Tablo depolama nedir
Azure Tablo depolama, büyük miktarlarda yapısal veriyi depolar. Bu hizmet, Azure bulutunun içinden ve dışından gelen kimliği doğrulanmış çağrıları kabul eden bir NoSQL olmayan veri deposudur. Azure tabloları, yapılandırılmış ve ilişkisel olmayan verilerin depolanması için idealdir. Tablo depolamanın yaygın kullanımları şunlardır:

* Web ölçekli uygulamalara hizmet verebilen yapılandırılmış verilerin TB depolaması
* Karmaşık katılımların, yabancı anahtarların veya depolanan yordamların gerekmediği ve hızlı erişim için normal olmayabilen veri kümelerinin depolanması
* Kümelenmiş dizin kullanarak hızlı veri sorgulaması
* WCF Veri Hizmeti .NET Kitaplıklarıyla OData protokolü ve LINQ sorgularını kullanarak verilere erişilmesi

Yapılandırılmış ve ilişkisel olmayan verilerin büyük kümelerini depolamak ve sorgulamak için Tablo depolamayı kullanabilirsiniz; tablolara talep arttıkça da ölçekleneceklerdir.

## <a name="table-storage-concepts"></a>Tablo depolama kavramları
Tablo depolamada şu bileşenler bulunur:

![Tablo depolama bileşen diyagramı][Table1]

* **URL biçimi:** Azure Tablo Depolama hesapları şu biçimi kullanır: `http://<storage account>.table.core.windows.net/<table>`

  Azure Cosmos DB Tablo API’si hesapları şu biçimi kullanır: `http://<storage account>.table.cosmosdb.azure.com/<table>`  

  Bu adres biçimini OData protokolüyle birlikte kullanarak doğrudan Azure tablolarını işaret edebilirsiniz. Daha fazla bilgi için bkz. [OData.org][OData.org].
* **Hesaplar** Tüm Azure Depolama erişimi bir depolama hesabı üzerinden yapılır. Depolama hesabı kapasitesi hakkında ayrıntılı bilgi için bkz. [Azure Storage Ölçeklenebilirlik ve Performans Hedefleri](../articles/storage/common/storage-scalability-targets.md). 

    Tüm Azure Cosmos DB erişimi bir Tablo API’si hesabı üzerinden yapılır. Tablo API’si hesabı oluşturmaya ilişkin ayrıntılar için bkz. [Tablo API’si hesabı oluşturma](../articles/cosmos-db/create-table-dotnet.md#create-a-database-account).
* **Tablo**: Tablo, bir varlıklar koleksiyonudur. Tablolar varlıklardaki şemayı zorlamaz; bu da, tek tabloda farklı özellik kümelerine sahip varlıklar olabileceği anlamına gelir.  
* **Varlık**: Varlık bir dizi özellikler kümesidir, veritabanı satırına benzer. Azure Depolama’daki bir varlığın boyutu en fazla 1 MB olabilir. Azure Cosmos DB’deki bir varlığın boyutu en fazla 2 MB olabilir.
* **Özellikler**: Özellik bir ad-değer çiftidir. Her varlıkta verileri depolayacak en çok 252 özellik olabilir. Her varlıkta ayrıca, bölüm anahtarını, satır anahtarını ve zaman damgasını belirten üç sistem özelliği bulunur. Aynı bölüm anahtarına sahip varlıklar daha hızlı sorgulanabilir ve atomik işlemlere eklenir/güncelleştirilir. Varlığın satır anahtarı bölüm içinde kendine ait benzersiz tanımlayıcıdır.

Tabloları ve özellikleri adlandırma hakkında daha fazla bilgi için bkz. [Tablo Hizmeti Veri Modelini Anlama](/rest/api/storageservices/Understanding-the-Table-Service-Data-Model).

[Table1]: ./media/storage-table-concepts-include/table1.png
[OData.org]: http://www.odata.org/
