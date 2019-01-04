---
title: / SQL Server'dan Azure Data Factory kullanarak verileri kopyalama | Microsoft Docs
description: Veri gönderip buralardan şirket içi SQL Server veritabanı veya Azure Data Factory kullanarak Azure VM taşıma hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 11/08/2018
ms.author: jingwang
ms.openlocfilehash: 776b1eb71b4f15c3376644de92205a4eeb77e4b2
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2019
ms.locfileid: "54020532"
---
# <a name="copy-data-to-and-from-sql-server-using-azure-data-factory"></a>İçin ve SQL Server'dan Azure Data Factory kullanarak veri kopyalama
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1](v1/data-factory-sqlserver-connector.md)
> * [Geçerli sürüm](connector-sql-server.md)

Bu makalede, kopyalama etkinliği Azure Data Factory'de gelen ve bir SQL Server veritabanına veri kopyalamak için nasıl kullanılacağını özetlenmektedir. Yapılar [kopyalama etkinliği'ne genel bakış](copy-activity-overview.md) kopyalama etkinliği genel bir bakış sunan makalesi.

## <a name="supported-capabilities"></a>Desteklenen özellikler

/ İçin SQL Server veritabanı veri tüm desteklenen havuz veri deposuna kopyalamak ya da SQL Server veritabanı için herhangi bir desteklenen kaynak veri deposundan veri kopyalama. Kaynakları/havuz kopyalama etkinliği tarafından desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu SQL Server connector'ı destekler:

- SQL Server 2016, 2014, 2012, 2008 R2, 2008 ve 2005 sürüm
- Kullanarak verileri kopyalama **SQL** veya **Windows** kimlik doğrulaması.
- SQL sorgusu veya saklı yordamı kullanarak veri kaynağı olarak alınıyor.
- Havuz olarak veriler hedef tablonun veya kopyalama sırasında özel mantığı olan bir saklı yordam çağırma ekleniyor.

## <a name="prerequisites"></a>Önkoşullar

Genel olarak erişilebilir olmayan SQL Server veritabanından veri kopyalama kullanmak için şirket içinde barındırılan tümleştirme çalışma zamanını oluşturan gerekir. Bkz: [şirket içinde barındırılan tümleştirme çalışma zamanı](create-self-hosted-integration-runtime.md) makale Ayrıntılar için. Tümleştirme çalışma zamanı yerleşik bir SQL Server veritabanı sürücüsünü sağlar, bu nedenle herhangi bir sürücü / için SQL Server veritabanına veri kopyalama işlemi sırasında el ile yüklemeniz gerekmez.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, Data Factory varlıklarını belirli SQL Server veritabanı bağlayıcıya tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Aşağıdaki özellikler, SQL Server bağlı hizmeti için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **SqlServer** | Evet |
| bağlantı dizesi |SQL kimlik doğrulaması veya Windows kimlik doğrulaması kullanarak SQL Server veritabanına bağlanmak üzere gereken bağlantı dizesi bilgilerini belirtin. Aşağıdaki örneğe bakın. Data Factory'de güvenle depolamak için bir SecureString olarak bu alanı işaretleyin veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). |Evet |
| Kullanıcı adı |Windows kimlik doğrulamasını kullanıyorsanız kullanıcı adı belirtin. Örnek: **domainname\\username**. |Hayır |
| password |Kullanıcı adı için belirtilen kullanıcı hesabı için parola belirtin. Data Factory'de güvenle depolamak için bir SecureString olarak bu alanı işaretleyin veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). |Hayır |
| connectVia | [Integration Runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deponuz genel olarak erişilebilir değilse), şirket içinde barındırılan tümleştirme çalışma zamanı veya Azure Integration Runtime kullanabilirsiniz. Belirtilmezse, varsayılan Azure Integration Runtime kullanır. |Hayır |

>[!TIP]
>Hata olarak "UserErrorFailedToConnectToSqlServer" hata koduyla isabet ve gibi ileti "veritabanı için oturum sınırı xxx ve üst sınırına ulaşıldı.", ekleme `Pooling=false` bağlantı dizesi ve yeniden deneyin.

**Örnek 1: SQL kimlik doğrulaması kullanma**

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

**Örnek 2: Windows kimlik doğrulaması kullanma**

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

Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için veri kümeleri makalesine bakın. Bu bölümde, SQL Server veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

/ SQL Server veritabanına veri kopyalamak için dataset öğesinin type özelliği ayarlamak **SqlServerTable**. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Dataset öğesinin type özelliği ayarlanmalıdır: **SqlServerTable** | Evet |
| tableName |Tablo veya Görünüm bağlı hizmeti SQL Server veritabanı örneğinde başvurduğu adı. | Kaynak, havuz için Evet Hayır |

**Örnek:**

```json
{
    "name": "SQLServerDataset",
    "properties":
    {
        "type": "SqlServerTable",
        "linkedServiceName": {
            "referenceName": "<SQL Server linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "MyTable"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md) makalesi. Bu bölümde, SQL Server kaynak ve havuz desteklenen özelliklerin bir listesini sağlar.

### <a name="sql-server-as-source"></a>Kaynak SQL Server

SQL Server'dan veri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **SqlSource**. Kopyalama etkinliği aşağıdaki özellikler desteklenir **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı öğesinin type özelliği ayarlanmalıdır: **SqlSource** | Evet |
| sqlReaderQuery |Verileri okumak için özel bir SQL sorgusu kullanın. Örnek: `select * from MyTable`. |Hayır |
| sqlReaderStoredProcedureName |Kaynak tablo verilerini okuyan saklı yordamın adı. Son SQL deyim bir SELECT deyimi saklı yordam içinde olmalıdır. |Hayır |
| storedProcedureParameters |Saklı yordamın parametreleri.<br/>İzin verilen değerler: ad/değer çiftleri. Adları ve parametreleri büyük küçük harfleri, adları ve saklı yordam parametreleri büyük küçük harfleri eşleşmelidir. |Hayır |

**Dikkat edilecek noktalar:**

- Varsa **sqlReaderQuery** belirtilen SqlSource için kopyalama etkinliği, verileri almak için SQL Server Kaynak karşı bu sorgu çalıştırır. Alternatif olarak, bir saklı yordam belirterek belirtebileceğiniz **sqlReaderStoredProcedureName** ve **storedProcedureParameters** (saklı yordamın parametreleri sürerse).
- "SqlReaderQuery" veya "sqlReaderStoredProcedureName" belirtmezseniz, sütunları veri kümesi JSON "yapı" bölümünde tanımlanan bir sorgu oluşturmak için kullanılır (`select column1, column2 from mytable`) SQL sunucuda çalıştırılacak. Veri kümesi tanımı "yapı" yoksa, tüm sütunları tablodan seçilir.

**Örnek: SQL sorgu kullanma**

```json
"activities":[
    {
        "name": "CopyFromSQLServer",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<SQL Server input dataset name>",
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

**Örnek: saklı yordam kullanma**

```json
"activities":[
    {
        "name": "CopyFromSQLServer",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<SQL Server input dataset name>",
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

**Saklı yordam tanımı:**

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

### <a name="sql-server-as-sink"></a>Havuz olarak SQL Server

SQL Server veri kopyalamak için kopyalama etkinliğine de Havuz türü ayarlayın. **SqlSink**. Kopyalama etkinliği aşağıdaki özellikler desteklenir **havuz** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği havuz öğesinin type özelliği ayarlanmalıdır: **SqlSink** | Evet |
| writeBatchSize |Arabellek boyutu writeBatchSize ulaştığında veri SQL tablosuna ekler.<br/>İzin verilen değerler: tamsayı (satır sayısı). |Hayır (varsayılan: 10000) |
| writeBatchTimeout |Toplu ekleme işlemi zaman aşımına uğramadan önce tamamlanması için bir süre bekleyin.<br/>İzin verilen değerler: TimeSpan değeri. Örnek: "00: 30:00" (30 dakika). |Hayır |
| preCopyScript |SQL Server'a veri yazılmadan önce yürütmek kopyalama etkinliği için bir SQL sorgusunu belirtin. Bu yalnızca bir kez çalıştır kopyalama çağrılır. Önceden yüklenmiş ve verileri temizlemek için bu özelliği kullanabilirsiniz. |Hayır |
| sqlWriterStoredProcedureName |Kaynak verileri hedef tabloya örn upsert eder ya da kendi iş mantığınızı kullanarak dönüşüm nasıl uygulanacağını tanımlayan saklı yordamın adı. <br/><br/>Bu saklı yordamı olacaktır Not **toplu iş çağrılan**. Yalnızca bir kez çalışır ve hiçbir kaynak verileri ile/delete örn truncate yapmak için kullanma işlemi yapmak istiyorsanız `preCopyScript` özelliği. |Hayır |
| storedProcedureParameters |Saklı yordamın parametreleri.<br/>İzin verilen değerler: ad/değer çiftleri. Adları ve parametreleri büyük küçük harfleri, adları ve saklı yordam parametreleri büyük küçük harfleri eşleşmelidir. |Hayır |
| sqlWriterTableType |Saklı yordam, kullanılacak bir tablo türü adı belirtin. Kopyalama etkinliği, taşınan veri bir geçici tablo bu tablo türü ile kullanılabilir hale getirir. Saklı yordam kodu daha sonra mevcut verilerle kopyalanan verileri birleştirebilirsiniz. |Hayır |

> [!TIP]
> SQL Server için veri kopyalama, kopyalama etkinliği havuz tabloya verileri varsayılan olarak ekler. UPSERT ya da ek iş mantığını gerçekleştirmek için SqlSink içinde saklı yordamı kullanın. Daha ayrıntılı bilgi edinin [SQL havuz için saklı yordam çağırma](#invoking-stored-procedure-for-sql-sink).

**Örnek 1: veri ekleme**

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
                "referenceName": "<SQL Server output dataset name>",
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

**Örnek 2: upsert için kopyalama sırasında bir saklı yordam çağırma**

Daha ayrıntılı bilgi edinin [SQL havuz için saklı yordam çağırma](#invoking-stored-procedure-for-sql-sink).

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
                "referenceName": "<SQL Server output dataset name>",
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

Bu bölümde, bir kimlik sütunu hedef tabloyla kimlik sütunu ile bir kaynak tablosundan veri kopyalayan bir örnek sağlar.

**Kaynak Tablo:**

```sql
create table dbo.SourceTbl
(
       name varchar(100),
       age int
)
```

**Hedef Tablo:**

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

Kaynak ve hedef tablonuz olarak farklı bir şemaya sahip olduğuna dikkat edin (hedef ek bir sütun kimliği içeriyor). Bu senaryoda, belirtmeniz gerekir. **yapısı** kimlik sütunu içermeyen hedef veri kümesi tanımında özelliği.

## <a name="invoking-stored-procedure-for-sql-sink"></a> SQL havuz saklı yordam çağırma

SQL Server veritabanına veri kopyalama işlemi sırasında bir kullanıcı saklı yordam yapılandırılabilir ve ek parametre tarafından çağrılıyor belirtti.

Yerleşik kopyalama mekanizmaları amacına hizmet etmediğini bir saklı yordam kullanılabilir. Upsert (Ekle + güncelleştirme) ya da ek işleme (ek değerler, birden çok tablo, vb. içine Arama sütunları, birleştirme) kaynak verilerin hedef tablodaki son ekleme önce yapılması gerektiğinde genellikle kullanılır.

Aşağıdaki örnek, bir saklı yordamı SQL Server veritabanındaki bir tabloya bir upsert yapmak için nasıl kullanılacağını gösterir. Girdi verilerini ve "Pazarlama" havuz tablo üç sütun vardır: Profileıd, durum ve kategorisi. Upsert "Profileıd" sütunu temel alarak gerçekleştirin ve yalnızca belirli bir kategori için geçerlidir.

**Çıktı veri kümesi**

```json
{
    "name": "SQLServerDataset",
    "properties":
    {
        "type": "SqlServerTable",
        "linkedServiceName": {
            "referenceName": "<SQL Server linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "Marketing"
        }
    }
}
```

SqlSink bölümü gibi kopyalama etkinliği tanımlayın.

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

Veritabanınızda, aynı ada sahip bir saklı yordam SqlWriterStoredProcedureName tanımlayın. Bu, çıkış tabloya belirtilen kaynak ve birleştirme gelen giriş verilerinin işler. Tablo türünde saklı yordam parametre adı veri kümesinde tanımlanan "tableName" ile aynı olması gerekir.

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

Veritabanınızda, aynı ada sahip bir tablo türü sqlWriterTableType tanımlayın. Tablo türü şemasını girişinizi tarafından döndürülen şema olarak aynı olması gerektiğini unutmayın.

```sql
CREATE TYPE [dbo].[MarketingType] AS TABLE(
    [ProfileID] [varchar](256) NOT NULL,
    [State] [varchar](256) NOT NULL，
    [Category] [varchar](256) NOT NULL，
)
```

Saklı yordam özellik yararlanır [Table-Valued parametreleri](https://msdn.microsoft.com/library/bb675163.aspx).

>[!NOTE]
>Para/küçük para veri türüne çağrılıyor saklı yordam tarafından yazarsanız, değerleri yuvarlatılmış. Karşılık gelen veri türünü TVP azaltmak için para/küçük para yerine ondalık olarak belirtin. 

## <a name="data-type-mapping-for-sql-server"></a>SQL server için eşleme veri türü

/ İçin SQL Server verilerini kopyalarken şu eşlemeler, SQL Server veri türleri arasından Azure veri fabrikası geçici veri türleri için kullanılır. Bkz: [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md) eşlemelerini nasıl yapar? kopyalama etkinliği kaynak şema ve veri türü için havuz hakkında bilgi edinmek için.

| SQL Server veri türü | Veri Fabrikası geçici veri türü |
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
| sql_variant |Nesne * |
| metin |Dize, Char] |
| time |Zaman aralığı |
| timestamp |Bayt] |
| tinyint |Int16 |
| benzersiz tanımlayıcı |Guid |
| varbinary |Bayt] |
| varchar |Dize, Char] |
| xml |Xml |

## <a name="troubleshooting-connection-issues"></a>Bağlantı sorunlarını giderme

1. Uzak bağlantıları kabul etmek için SQL Server'ınızı yapılandırın. Başlatma **SQL Server Management Studio**, sağ **sunucu**, tıklatıp **özellikleri**. Seçin **bağlantıları** denetimi ve liste **sunucunun uzak bağlantılara izin vermek**.

    ![Uzak bağlantıları etkinleştir](media/copy-data-to-from-sql-server/AllowRemoteConnections.png)

    Bkz: [uzaktan erişim sunucu yapılandırma seçeneğini yapılandırma](https://msdn.microsoft.com/library/ms191464.aspx) ayrıntılı adımlar için.

2. Başlatma **SQL Server Yapılandırma Yöneticisi**. Genişletin **SQL Server Ağ Yapılandırması** ve seçin, örneğin **MSSQLSERVER protokolleri**. Sağ bölmede protokolleri görmeniz gerekir. Sağ tıklayarak TCP/IP'yi etkinleştirin **TCP/IP'yi** tıklayıp **etkinleştirme**.

    ![TCP/IP'yi etkinleştirin](./media/copy-data-to-from-sql-server/EnableTCPProptocol.png)

    Bkz: [etkinleştirmek veya sunucu ağ protokolünü devre dışı](https://msdn.microsoft.com/library/ms191294.aspx) Ayrıntılar ve TCP/IP protokolünü etkinleştirme alternatif yolu.

3. Aynı pencerede çift **TCP/IP'yi** başlatmak için **TCP/IP özelliklerini** penceresi.
4. Geçiş **IP adresleri** sekmesi. Görmek için aşağı kaydırın **IPAll** bölümü. Not ** TCP bağlantı noktası ** (varsayılan değer **1433**).
5. Oluşturma bir **Windows Güvenlik Duvarı Kuralı** Bu bağlantı noktası üzerinden gelen trafiğe izin vermek için makinede.  
6. **Bağlantıyı doğrulama**: Tam adı kullanılarak SQL Server'a bağlanmak için farklı bir makinede SQL Server Management Studio'yu kullanın. Örneğin: `"<machine>.<domain>.corp.<company>.com,1433"`.


## <a name="next-steps"></a>Sonraki adımlar
Azure Data Factory kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md##supported-data-stores-and-formats).
