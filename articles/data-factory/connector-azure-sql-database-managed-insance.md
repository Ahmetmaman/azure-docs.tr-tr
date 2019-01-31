---
title: Azure Data Factory kullanarak Azure SQL veritabanı yönetilen örneği gelen ve giden veri kopyalama | Microsoft Docs
description: Azure Data Factory kullanarak Azure SQL veritabanı yönetilen örneği gelen ve giden veri taşımayı öğreneceksiniz.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/23/2019
ms.author: jingwang
ms.openlocfilehash: 9cd2eaefb845b6ce9ca2f1cfcaf1234f8f96615c
ms.sourcegitcommit: a7331d0cc53805a7d3170c4368862cad0d4f3144
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55300345"
---
# <a name="copy-data-to-and-from-azure-sql-database-managed-instance-by-using-azure-data-factory"></a>Azure Data Factory kullanarak Azure SQL veritabanı yönetilen örneği gelen ve giden veri kopyalama

Bu makalede, kopyalama etkinliği Azure Data Factory ve Azure SQL veritabanı yönetilen örnek veri kopyalamak için nasıl kullanılacağını özetlenmektedir. Yapılar [kopyalama etkinliğine genel bakış](copy-activity-overview.md) kopyalama etkinliği genel bir bakış sunan makalesi.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Verileri Azure SQL veritabanı yönetilen örneği tüm desteklenen havuz veri deposuna kopyalayabilirsiniz. Ayrıca yönetilen örneğe herhangi bir desteklenen kaynak veri deposundan veri kopyalayabilirsiniz. Kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu Azure SQL veritabanı yönetilen örneği bağlayıcı'yı destekler:

- SQL veya Windows kimlik doğrulaması kullanarak veri kopyalama.
- Bir kaynak olarak bir SQL sorgusu veya saklı yordamı kullanarak verileri alınıyor.
- Bir havuz olarak veriler hedef tablonun veya kopyalama sırasında özel mantığı olan bir saklı yordam çağırma ekleniyor.

SQL Server [Always Encrypted](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine?view=sql-server-2017) artık desteklenmiyor. 

## <a name="prerequisites"></a>Önkoşullar

Azure SQL veritabanı yönetilen bir sanal ağda bulunan örneği verilerini kullanmak için veritabanına erişebilen bir şirket içinde barındırılan tümleştirme çalışma zamanını oluşturan ayarlayın. Daha fazla bilgi için [barındırılan tümleştirme çalışma zamanını](create-self-hosted-integration-runtime.md).

Yönetilen Örneğinize aynı sanal ağ, şirket içinde barındırılan tümleştirme çalışma zamanı sağlama, tümleştirme çalışma zamanını makinenize yönetilen Örneğinize değerinden farklı bir alt ağda olduğundan emin olun. Yönetilen Örneğinize farklı bir sanal ağ, şirket içinde barındırılan tümleştirme çalışma zamanı sağlama, bir sanal ağ eşlemesi veya sanal ağ bağlantısı için sanal ağ kullanabilirsiniz. Daha fazla bilgi için [uygulamanızı Azure SQL veritabanı yönetilen örneği bağlamak](../sql-database/sql-database-managed-instance-connect-app.md).

## <a name="get-started"></a>başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler belirli Data Factory varlıkları için Azure SQL veritabanı yönetilen örneği bağlayıcıyı tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Azure SQL veritabanı yönetilen bağlı örneği hizmeti için aşağıdaki özellikleri destekler:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır **SqlServer**. | Evet. |
| bağlantı dizesi |Bu özellik, SQL kimlik doğrulaması veya Windows kimlik doğrulaması kullanarak yönetilen örneğe bağlanmak için gereken bağlantı dizesi bilgilerini belirtir. Daha fazla bilgi için aşağıdaki örneklere bakın. Seçin **SecureString** connectionString bilgileri Data Factory'de güvenli bir şekilde depolamak için veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). |Evet. |
| Kullanıcı adı |Bu özellik, Windows kimlik doğrulamasını kullanıyorsanız kullanıcı adı belirtir. Bir örnek **domainname\\username**. |Hayır. |
| password |Bu özellik, kullanıcı adı için belirtilen kullanıcı hesabının parolasını belirtir. Seçin **SecureString** connectionString bilgileri Data Factory'de güvenli bir şekilde depolamak için veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). |Hayır. |
| connectVia | Bu [Integration runtime](concepts-integration-runtime.md) veri deposuna bağlamak için kullanılır. Şirket içinde barındırılan tümleştirme çalışma zamanının yönetilen Örneğinize aynı sanal ağda sağlayın. |Evet. |

>[!TIP]
>"Veritabanı için oturum sınırı xxx ve üst sınırına ulaşıldı." iletisiyle "UserErrorFailedToConnectToSqlServer" gibi hata kodu görebilirsiniz. Bu hata meydana gelirse, ekleme `Pooling=false` bağlantı dizesi ve yeniden deneyin.

**Örnek 1: SQL kimlik doğrulaması kullan**

```json
{
    "name": "SqlServerLinkedService",
    "properties": {
        "type": "SqlServer",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Data Source=<servername>\\<instance name if using named instance>;Initial Catalog=<databasename>;Integrated Security=False;User ID=<username>;Password=<password>;"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Örnek 2: Windows kimlik doğrulaması kullan**

```json
{
    "name": "SqlServerLinkedService",
    "properties": {
        "type": "SqlServer",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Data Source=<servername>\\<instance name if using named instance>;Initial Catalog=<databasename>;Integrated Security=True;"
            },
            "userName": "<domain\\username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Bölümleri ve veri kümeleri tanımlamak üzere kullanılmak üzere mevcut özelliklerin tam listesi için veri kümeleri makalesine bakın. Bu bölümde, Azure SQL veritabanı yönetilen örneği veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

Azure SQL veritabanı yönetilen örneği ve veri kopyalamak için dataset öğesinin type özelliği ayarlamak **SqlServerTable**. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Dataset öğesinin type özelliği ayarlanmalıdır **SqlServerTable**. | Evet. |
| tableName |Bu özellik, tablo veya Görünüm bağlı hizmetini ifade eder veritabanı örneğinde adıdır. | Kaynağı yok. Havuz için Evet. |

**Örnek**

```json
{
    "name": "SQLServerDataset",
    "properties":
    {
        "type": "SqlServerTable",
        "linkedServiceName": {
            "referenceName": "<Managed Instance linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "MyTable"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak üzere kullanılmak üzere mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md) makalesi. Bu bölümde, Azure SQL veritabanı yönetilen örneği'nın kaynak ve havuz desteklenen özelliklerin bir listesini sağlar.

### <a name="azure-sql-database-managed-instance-as-a-source"></a>Azure SQL veritabanı yönetilen örneği bir kaynak olarak

Verileri Azure SQL veritabanı yönetilen örneği kopyalamak için kopyalama etkinliği kaynak türünü ayarlayın. **SqlSource**. Kopyalama etkinliği kaynak bölümünde aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı öğesinin type özelliği ayarlanmalıdır **SqlSource**. | Evet. |
| sqlReaderQuery |Bu özellik, verileri okumak için özel bir SQL sorgusu kullanır. `select * from MyTable` bunun bir örneğidir. |Hayır. |
| sqlReaderStoredProcedureName |Bu özellik kaynak tablosundan veri okuyan saklı yordamı adıdır. Son SQL deyim bir SELECT deyimi saklı yordam içinde olmalıdır. |Hayır. |
| storedProcedureParameters |Bu parametreler için saklı yordam kullanılabilir.<br/>İzin verilen değerler, ad veya değer çiftleridir. Adları ve parametreleri büyük küçük harfleri, adları ve saklı yordam parametreleri büyük küçük harfleri eşleşmelidir. |Hayır. |

Aşağıdaki noktalara dikkat edin:

- Varsa **sqlReaderQuery** için belirtilen **SqlSource**, kopyalama etkinliği bu sorgu veri almak için yönetilen örnek kaynak karşı çalışır. Belirterek bir saklı yordam belirtebilirsiniz **sqlReaderStoredProcedureName** ve **storedProcedureParameters** saklı yordamın kullandığı parametreler varsa.
- Ya da belirtmezseniz **sqlReaderQuery** veya **sqlReaderStoredProcedureName** özelliği, bir sorgu oluşturmak için kullanılan JSON veri kümesi "yapı" bölümünde tanımlanan sütunları. Sorgu `select column1, column2 from mytable` yönetilen örneğine karşı çalışır. Veri kümesi tanımı "yapı" yoksa, tüm sütunları tablodan seçilir.

**Örnek: Bir SQL sorgusu kullanın**

```json
"activities":[
    {
        "name": "CopyFromSQLServer",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Managed Instance input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "SqlSource",
                "sqlReaderQuery": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

**Örnek: Bir saklı yordam kullanma**

```json
"activities":[
    {
        "name": "CopyFromSQLServer",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Managed Instance input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "SqlSource",
                "sqlReaderStoredProcedureName": "CopyTestSrcStoredProcedureWithParameters",
                "storedProcedureParameters": {
                    "stringData": { "value": "str3" },
                    "identifier": { "value": "$$Text.Format('{0:yyyy}', <datetime parameter>)", "type": "Int"}
                }
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

**Saklı yordam tanımında**

```sql
CREATE PROCEDURE CopyTestSrcStoredProcedureWithParameters
(
    @stringData varchar(20),
    @identifier int
)
AS
SET NOCOUNT ON;
BEGIN
    select *
    from dbo.UnitTestSrcTable
    where dbo.UnitTestSrcTable.stringData != stringData
    and dbo.UnitTestSrcTable.identifier != identifier
END
GO
```

### <a name="azure-sql-database-managed-instance-as-a-sink"></a>Azure SQL veritabanı yönetilen örneği bir havuz olarak

Azure SQL veritabanı yönetilen örneği için verileri kopyalamak için kopyalama etkinliğine de Havuz türü ayarlayın. **SqlSink**. Kopyalama etkinliği havuz bölümünde aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği havuz öğesinin type özelliği ayarlanmalıdır **SqlSink**. | Evet. |
| writeBatchSize |Bu özellik, arabellek boyutu writeBatchSize ulaştığında veri SQL tablosuna ekler.<br/>Tamsayılar için satır sayısı izin verilen değerler. |Hayır (varsayılan: 10,000). |
| writeBatchTimeout |Bu özellik toplu ekleme işlemi zaman aşımına uğramadan önce tamamlanması için bekleme süresini belirtir.<br/>Zaman aralığı için izin verilen değerler. Bir örnek verilmiştir "00: 30:00," 30 dakika olduğu. |Hayır. |
| preCopyScript |Bu özellik, yönetilen örneğe veri yazılmadan önce yürütmek kopyalama etkinliği için bir SQL sorgusu belirtir. Ayrıca, kopya çalıştırma başına yalnızca bir kez çağrılır. Önceden verileri temizlemek için bu özelliği kullanabilirsiniz. |Hayır. |
| sqlWriterStoredProcedureName |Bu ad, kaynak verileri hedef tabloya uygulanacağını tanımlayan bir saklı yordam içindir. Yordamları kendi iş mantığınızı kullanarak upsert eder veya dönüşümler gerçekleştirmenin örnekleridir. <br/><br/>Bu saklı yordam *toplu iş çağrılan*. Yalnızca bir kez çalıştırır ve kaynak verileri, örneğin, silmek veya kesmek için hiçbir şey sahip bir işlem yapmak için `preCopyScript` özelliği. |Hayır. |
| storedProcedureParameters |Bu parametreler, saklı yordam için kullanılır.<br/>İzin verilen değerler, ad veya değer çiftleridir. Adları ve parametreleri büyük küçük harfleri, adları ve saklı yordam parametreleri büyük küçük harfleri eşleşmelidir. |Hayır. |
| sqlWriterTableType |Bu özellik saklı yordamda kullanılan tablo türü adı belirtir. Kopyalama etkinliği, taşınan veri bir geçici tablo bu tablo türü ile kullanılabilir hale getirir. Saklı yordam kodu daha sonra kopyalanan verileri mevcut verilerle birleştirebilirsiniz. |Hayır. |

> [!TIP]
> Azure SQL veritabanı yönetilen örneği için verileri kopyalandığında, kopyalama etkinliği havuz tablo için verileri varsayılan olarak ekler. Upsert ya da ek iş mantığını gerçekleştirmek için SqlSink içinde saklı yordamı kullanın. Daha fazla bilgi için [SQL havuz saklı bir yordam çağırma](#invoke-a-stored-procedure-from-a-sql-sink).

**Örnek 1: Veri ekleme**

```json
"activities":[
    {
        "name": "CopyToSQLServer",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Managed Instance output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "SqlSink",
                "writeBatchSize": 100000
            }
        }
    }
]
```

**Örnek 2: Upsert için kopyalama sırasında bir saklı yordam çağırma**

Daha ayrıntılı bilgi edinin [SQL havuz saklı bir yordam çağırma](#invoke-a-stored-procedure-from-a-sql-sink).

```json
"activities":[
    {
        "name": "CopyToSQLServer",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Managed Instance output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "SqlSink",
                "sqlWriterStoredProcedureName": "CopyTestStoredProcedureWithParameters",
                "sqlWriterTableType": "CopyTestTableType",
                "storedProcedureParameters": {
                    "identifier": { "value": "1", "type": "Int" },
                    "stringData": { "value": "str1" }
                }
            }
        }
    }
]
```

## <a name="identity-columns-in-the-target-database"></a>Hedef veritabanındaki Kimlik sütunları

Aşağıdaki örnek bir hedef tablosu IDENTITY sütunu için hiç kimlik sütunu içeren bir kaynak tablo verileri kopyalar.

**Kaynak tablosu**

```sql
create table dbo.SourceTbl
(
    name varchar(100),
    age int
)
```

**Hedef Tablo**

```sql
create table dbo.TargetTbl
(
    identifier int identity(1,1),
    name varchar(100),
    age int
)
```

Hedef tablo bir kimlik sütunu olduğuna dikkat edin.

**Kaynak veri kümesi JSON tanımı**

```json
{
    "name": "SampleSource",
    "properties": {
        "type": " SqlServerTable",
        "linkedServiceName": {
            "referenceName": "TestIdentitySQL",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "SourceTbl"
        }
    }
}
```

**Hedef veri kümesi JSON tanımı**

```json
{
    "name": "SampleTarget",
    "properties": {
        "structure": [
            { "name": "name" },
            { "name": "age" }
        ],
        "type": "SqlServerTable",
        "linkedServiceName": {
            "referenceName": "TestIdentitySQL",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "TargetTbl"
        }
    }
}
```

Kaynak ve hedef tablonuz farklı şema sahip olduğuna dikkat edin. Hedef tablo bir kimlik sütunu var. Bu senaryoda, kimlik sütunu içermeyen hedef veri kümesi tanımında "yapı" özelliğini belirtin.

## <a name="invoke-a-stored-procedure-from-a-sql-sink"></a> SQL havuz saklı bir yordam çağırma

Verileri Azure SQL veritabanı yönetilen örneğine kopyalandığında bir saklı yordam yapılandırılabilir ve belirttiğiniz ek parametrelerle çağrıldı.

Yerleşik kopyalama mekanizmaları amaca hizmet yoksa, bir saklı yordamı kullanabilirsiniz. Upsert (güncelleştirme + INSERT) ya da ek işlem kaynak verilerin hedef tablodaki son ekleme önce yapılması gerektiğinde genellikle kullanılır. Ek işlem ek değerler ve birden çok tablolara ekleme bakarak sütunları, birleştirme gibi görevleri içerebilir.

Aşağıdaki örnek, yönetilen örneğinde bir tabloya bir upsert yapmak için bir saklı yordam kullanmayı gösterir. Örnek giriş verileri ve havuz "Pazarlama" tablosunda üç sütun olduğunu varsayar: Profileıd, durum ve kategorisi. Profileıd sütuna göre upsert gerçekleştirin ve yalnızca belirli bir kategori için geçerlidir.

**Çıktı veri kümesi**

```json
{
    "name": "SQLServerDataset",
    "properties":
    {
        "type": "SqlServerTable",
        "linkedServiceName": {
            "referenceName": "<Managed Instance linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "Marketing"
        }
    }
}
```

SqlSink bölümü, bir kopyalama etkinliği gibi tanımlayın:

```json
"sink": {
    "type": "SqlSink",
    "SqlWriterTableType": "MarketingType",
    "SqlWriterStoredProcedureName": "spOverwriteMarketing",
    "storedProcedureParameters": {
        "category": {
            "value": "ProductA"
        }
    }
}
```

Veritabanınızda, aynı ada sahip bir saklı yordam SqlWriterStoredProcedureName tanımlayın. Belirtilen kaynak gelen giriş verilerinin işler ve çıkış tablosuna birleştirir. Tablo türünde saklı yordam parametre adı veri kümesinde tanımlanan "TableName"değeri ile aynıdır.

```sql
CREATE PROCEDURE spOverwriteMarketing @Marketing [dbo].[MarketingType] READONLY, @category varchar(256)
AS
BEGIN
  MERGE [dbo].[Marketing] AS target
  USING @Marketing AS source
  ON (target.ProfileID = source.ProfileID and target.Category = @category)
  WHEN MATCHED THEN
      UPDATE SET State = source.State
  WHEN NOT MATCHED THEN
      INSERT (ProfileID, State, Category)
      VALUES (source.ProfileID, source.State, source.Category)
END
```

Veritabanınızda, aynı ada sahip bir tablo türü sqlWriterTableType tanımlayın. Tablo türü şemasını girişinizi tarafından döndürülen şema ile aynıdır.

```sql
CREATE TYPE [dbo].[MarketingType] AS TABLE(
    [ProfileID] [varchar](256) NOT NULL,
    [State] [varchar](256) NOT NULL，
    [Category] [varchar](256) NOT NULL，
)
```

Saklı yordam özellik yararlanır [tablo değerli parametre](https://msdn.microsoft.com/library/bb675163.aspx).

>[!NOTE]
>İçin yazarsanız **para/küçük para** veri türü bir saklı yordam çağırarak, değerleri yuvarlanır. Tablo değerli parametrelere karşılık gelen veri türünü belirtmeniz **ondalık** yerine **para/küçük para** bu sorunu gidermek için. 

## <a name="data-type-mapping-for-azure-sql-database-managed-instance"></a>Azure SQL veritabanı yönetilen örneği için veri türü eşlemesi

Azure SQL veritabanı yönetilen örneği gelen ve giden veriler kopyalandığında, aşağıdaki eşlemeler Azure SQL veritabanı yönetilen örneği veri türlerinden Azure veri fabrikası geçici veri türleri için kullanılır. Kopyalama etkinliği havuz için kaynak şema ve veri türü eşlemelerini nasıl bilgi edinmek için [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md).

| Azure SQL veritabanı yönetilen örneği veri türü | Azure veri fabrikası geçici veri türü |
|:--- |:--- |
| bigint |Int64 |
| İkili |Bayt] |
| Bit |Boole |
| Char |Dize, Char] |
| date |DateTime |
| Tarih saat |DateTime |
| datetime2 |DateTime |
| Datetimeoffset |DateTimeOffset |
| Ondalık |Ondalık |
| FILESTREAM özniteliğini (varbinary(max)) |Bayt] |
| Kayan |çift |
| image |Bayt] |
| int |Int32 |
| para |Ondalık |
| nchar |Dize, Char] |
| ntext |Dize, Char] |
| Sayısal |Ondalık |
| nvarchar |Dize, Char] |
| Gerçek |Tek |
| rowVersion |Bayt] |
| smalldatetime |DateTime |
| smallint |Int16 |
| küçük para |Ondalık |
| sql_variant |Nesne |
| metin |Dize, Char] |
| time |Zaman aralığı |
| timestamp |Bayt] |
| tinyint |Int16 |
| benzersiz tanımlayıcı |Guid |
| varbinary |Bayt] |
| varchar |Dize, Char] |
| xml |Xml |

>[!NOTE]
> Eşleme için ondalık geçiş türü veri türleri için Azure Data Factory şu anda 28 kadar duyarlık destekler. 28'den büyük duyarlılık gerektiren varsa, bir SQL sorgusu içindeki bir dizeye dönüştürmeyi düşünün.

## <a name="next-steps"></a>Sonraki adımlar
Azure Data Factory kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md##supported-data-stores-and-formats).
