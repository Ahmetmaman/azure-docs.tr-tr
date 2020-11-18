---
title: Jupi Not defterlerindeki verilere erişim-Azure Notebooks önizlemesi
description: Dosyalara, REST API 'Lerine, veritabanlarına ve farklı Azure depolama kaynaklarına bir Jupyter Not defterinden erişmeyi öğrenin.
ms.topic: how-to
ms.date: 12/04/2018
ms.custom: devx-track-python
ms.openlocfilehash: a833ff914c1ee53f024147371977ac1caa3800dc
ms.sourcegitcommit: 0a9df8ec14ab332d939b49f7b72dea217c8b3e1e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/18/2020
ms.locfileid: "94842881"
---
# <a name="access-cloud-data-in-a-notebook"></a>Not defterinde bulut verilerine erişme

[!INCLUDE [notebooks-status](../../includes/notebooks-status.md)]

Bir Jupyter not defterinde ilgi çekici iş yapmak için veri gerekir. Veriler, aslında Not defterlerinin yaşam bir kan.

Bir dosyayı doğrudan indirmek için bir not defteri içinde olan komutları kullanarak bile [bir projeye veri dosyalarını içeri aktarabilirsiniz](work-with-project-data-files.md) `curl` . Ancak REST API 'Leri, ilişkisel veritabanları ve Azure tabloları gibi bulut depolama alanı gibi dosya olmayan kaynaklardan kullanılabilen çok daha kapsamlı verilerle çalışmanız gerekir.

Bu makale, bu farklı seçenekleri kısaca özetler. Veri erişiminin en iyi şekilde görüldüğü için, Azure Notebooks örneklerimizde çalıştırılabilir kodu bulabilirsiniz [-verilerinize erişebilirsiniz](https://github.com/Microsoft/AzureNotebooks/blob/master/Samples/Access%20your%20data%20in%20Azure%20Notebooks.ipynb).

## <a name="rest-apis"></a>REST API'leri

Genel olarak, Internet 'ten kullanılabilen büyük miktarda veri, REST API 'Ler aracılığıyla dosyalar aracılığıyla değil, dosyalar aracılığıyla erişilmez. Neyse ki, bir not defteri hücresi istediğiniz kodu içerebildiğinden, istek göndermek ve JSON verileri almak için kodu kullanabilirsiniz. Daha sonra bu JSON 'ı, Pandas dataframe gibi kullanmak istediğiniz herhangi bir biçime dönüştürebilirsiniz.

REST API kullanarak verilere erişmek için, başka bir uygulamada kullandığınız bir not defteri kod hücrelerinde aynı kodu kullanın. İstekler kitaplığını kullanan genel yapı aşağıdaki gibidir:

```python
import pandas
import requests

# New York City taxi data for 2014
data_url = 'https://data.cityofnewyork.us/resource/gkne-dk5s.json'

# General data request; include other API keys and credentials as needed in the data argument
response = requests.get(data_url, data={"limit": "20"})

if response.status_code == 200:
    dataframe_rest2 = pandas.DataFrame.from_records(response.json())
    print(dataframe_rest2)
```

## <a name="azure-sql-database-and-sql-managed-instance"></a>Azure SQL veritabanı ve SQL yönetilen örneği

SQL veritabanı veya SQL yönetilen örneğindeki veritabanlarına, pyodbc veya pymssql kitaplıklarının yardımı ile erişebilirsiniz.

[Azure SQL veritabanını sorgulamak Için Python 'U kullanarak](../azure-sql/database/connect-query-python.md) , SQL veritabanı 'nda AdventureWorks verilerini içeren bir veritabanı oluşturma ve bu verilerin nasıl sorgulanalınacağını gösteren yönergeler sağlar. Bu makalenin örnek not defterinde aynı kod gösterilir.

## <a name="azure-storage"></a>Azure Depolama

Azure depolama, sahip olduğunuz veri türüne ve bu verilere nasıl erişmeniz gerektiği üzerine bağlı olarak, birden çok farklı türde ilişkisel olmayan depolama sağlar:

- Tablo Depolama: toplanan algılayıcı günlükleri, tanılama günlükleri vb. gibi tablolu veriler için düşük maliyetli, yüksek hacimli depolama alanı sağlar.
- BLOB depolama: herhangi bir veri türü için dosya benzeri depolama sağlar.

Örnek Not defteri, bloblara salt okuma erişimi sağlamak için paylaşılan erişim imzasının nasıl kullanılacağı da dahil olmak üzere hem tablolar hem de bloblarla çalışmayı gösterir.

## <a name="azure-cosmos-db"></a>Azure Cosmos DB

Azure Cosmos DB, JSON belgeleri için tam dizinli bir NoSQL deposu sağlar). Aşağıdaki makalelerde, Python 'dan Cosmos DB çalışmak için çeşitli farklı yollar sağlanmaktadır:

- [Python ile SQL API uygulaması derleme](../cosmos-db/create-sql-api-python.md)
- [MongoDB için Azure Cosmos DB API 'SI ile bir Flask uygulaması oluşturun](../cosmos-db/create-mongodb-flask.md)
- [Python ve Gremlin API kullanarak bir grafik veritabanı oluşturma](../cosmos-db/create-graph-python.md)
- [Python ve Azure Cosmos DB ile Cassandra uygulaması derleme](../cosmos-db/create-cassandra-python.md)
- [Python ve Azure Cosmos DB Tablo API'si uygulama oluşturma](../cosmos-db/table-storage-how-to-use-python.md)

Cosmos DB ile çalışırken, [Azure-cosmosdb-Table](https://pypi.org/project/azure-cosmosdb-table/) kitaplığını kullanabilirsiniz.

## <a name="other-azure-databases"></a>Diğer Azure veritabanları

Azure, kullanabileceğiniz bir dizi diğer veritabanı türü sağlar. Aşağıdaki makaleler Python 'dan bu veritabanlarına erişim için rehberlik sağlar:

- [PostgreSQL için Azure Veritabanı: Bağlanmak ve veri sorgulamak için Python'ı kullanma](../postgresql/connect-python.md)
- [Hızlı başlangıç: Python ile Azure Redis Cache kullanma](../azure-cache-for-redis/cache-python-get-started.md)
- [MySQL için Azure Veritabanı: Python'u kullanarak bağlanma ve veri sorgulama](../mysql/connect-python.md)
- [Azure Data Factory](https://azure.microsoft.com/services/data-factory/)
  - [Azure Data Factory için kopyalama Sihirbazı](https://azure.microsoft.com/updates/code-free-copy-wizard-for-azure-data-factory/)

## <a name="next-steps"></a>Sonraki adımlar

- [Nasıl yapılır: proje veri dosyalarıyla çalışma](work-with-project-data-files.md)