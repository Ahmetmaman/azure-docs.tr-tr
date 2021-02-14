---
title: Salesforce pazarlama bulutlarından veri kopyalama
description: Azure Data Factory bir işlem hattındaki kopyalama etkinliğini kullanarak Salesforce pazarlama bulutlarından desteklenen havuz veri depolarına veri kopyalamayı öğrenin.
ms.author: jingwang
author: linda33wj
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 07/17/2020
ms.openlocfilehash: 161b81b196a1e178c7244845b25594440e6d6e1e
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2021
ms.locfileid: "100369756"
---
# <a name="copy-data-from-salesforce-marketing-cloud-using-azure-data-factory"></a>Azure Data Factory kullanarak Salesforce pazarlama bulutlarından veri kopyalama

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu makalede, Salesforce pazarlama bulutundaki verileri kopyalamak için Azure Data Factory kopyalama etkinliğinin nasıl kullanılacağı özetlenmektedir. Kopyalama etkinliğine genel bir bakış sunan [kopyalama etkinliğine genel bakış](copy-activity-overview.md) makalesinde oluşturulur.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Bu Salesforce Marketing Cloud Connector, aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/havuz matrisi](copy-activity-overview.md) ile [kopyalama etkinliği](copy-activity-overview.md)
- [Arama etkinliği](control-flow-lookup-activity.md)

Salesforce pazarlama bulutundaki verileri desteklenen herhangi bir havuz veri deposuna kopyalayabilirsiniz. Kopyalama etkinliği tarafından kaynak/havuz olarak desteklenen veri depolarının listesi için [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablosuna bakın.

Salesforce Marketing Cloud Connector, OAuth 2 kimlik doğrulamasını destekler ve hem eski hem de geliştirilmiş paket türlerini destekler. Bağlayıcı [Salesforce Marketing Cloud REST API](https://developer.salesforce.com/docs/atlas.en-us.mc-apis.meta/mc-apis/index-api.htm)üzerine kurulmuştur.

>[!NOTE]
>Bu bağlayıcı özel nesneler veya özel veri uzantıları almayı desteklemiyor.

## <a name="getting-started"></a>Kullanmaya başlama

.NET SDK, Python SDK, Azure PowerShell, REST API veya Azure Resource Manager şablonu kullanarak kopyalama etkinliği ile bir işlem hattı oluşturabilirsiniz. Kopyalama etkinliğine sahip bir işlem hattı oluşturmak için adım adım yönergeler için bkz. [kopyalama etkinliği öğreticisi](quickstart-create-data-factory-dot-net.md) .

Aşağıdaki bölümler Salesforce pazarlama bulut bağlayıcısına özgü Data Factory varlıkları tanımlamak için kullanılan özellikler hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmet özellikleri

Aşağıdaki özellikler, Salesforce pazarlama bulutu bağlı hizmeti için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| tür | Type özelliği şu şekilde ayarlanmalıdır: **Salesforcepazarbulutu** | Yes |
| connectionProperties | Salesforce pazarlama bulutuna bağlanmayı tanımlayan bir özellik grubu. | Yes |
| ***Altında `connectionProperties` :*** | | |
| authenticationType | Kullanılacak kimlik doğrulama yöntemini belirtir. İzin verilen değerler `Enhanced sts OAuth 2.0` veya `OAuth_2.0` .<br><br>Salesforce pazarlama bulutu eski paketi yalnızca `OAuth_2.0` , Gelişmiş paket gereksinimdeyken desteklenir `Enhanced sts OAuth 2.0` . <br>1 Ağustos 2019 ' den itibaren Salesforce pazarlama bulutu eski paketleri oluşturma özelliğini kaldırdı. Tüm yeni paketler gelişmiş paketlerdir. | Yes |
| konak | Gelişmiş paket için ana bilgisayar, "Mc" harflerinden başlayan bir 28 karakterlik dize tarafından temsil edilen alt [etki alanı](https://developer.salesforce.com/docs/atlas.en-us.mc-apis.meta/mc-apis/your-subdomain-tenant-specific-endpoints.htm) olmalıdır, örneğin `mc563885gzs27c5t9-63k636ttgm` . <br>Eski paket için, belirtin `www.exacttargetapis.com` . | Yes |
| clientId | Salesforce pazarlama bulut uygulamasıyla ilişkili istemci KIMLIĞI.  | Yes |
| clientSecret | Salesforce pazarlama bulut uygulamasıyla ilişkili istemci gizli dizisi. Bu alanı bir SecureString olarak güvenli bir şekilde depolamak için bir SecureString olarak işaretlemeyi veya gizli dizi Azure Key Vault ve veri kopyalama işlemini gerçekleştirirken ADF kopyalama etkinliği çekimine izin vermek için, [Key Vault mağaza kimlik bilgilerinden](store-credentials-in-key-vault.md)daha fazla bilgi edinin. | Yes |
| useEncryptedEndpoints | Veri kaynağı uç noktalarının HTTPS kullanılarak şifrelenip şifrelenmediğini belirtir. Varsayılan değer true şeklindedir.  | No |
| Usehostdoğrulaması | Sunucu sertifikasında, TLS üzerinden bağlanırken sunucunun ana bilgisayar adıyla eşleşecek şekilde, ana bilgisayar adının istenip istenmeyeceğini belirtir. Varsayılan değer true şeklindedir.  | No |
| Usepeerdoğrulaması | TLS üzerinden bağlanılırken sunucu kimliğinin doğrulanıp doğrulanmayacağını belirtir. Varsayılan değer true şeklindedir.  | No |

**Örnek: Gelişmiş STS için genişletilmiş STS OAuth 2 kimlik doğrulamasını kullanma** 

```json
{
    "name": "SalesforceMarketingCloudLinkedService",
    "properties": {
        "type": "SalesforceMarketingCloud",
        "typeProperties": {
            "connectionProperties": {
                "host": "<subdomain e.g. mc563885gzs27c5t9-63k636ttgm>",
                "authenticationType": "Enhanced sts OAuth 2.0",
                "clientId": "<clientId>",
                "clientSecret": {
                     "type": "SecureString",
                     "value": "<clientSecret>"
                },
                "useEncryptedEndpoints": true,
                "useHostVerification": true,
                "usePeerVerification": true
            }
        }
    }
}

```

**Örnek: eski paket için OAuth 2 kimlik doğrulamasını kullanma** 

```json
{
    "name": "SalesforceMarketingCloudLinkedService",
    "properties": {
        "type": "SalesforceMarketingCloud",
        "typeProperties": {
            "connectionProperties": {
                "host": "www.exacttargetapis.com",
                "authenticationType": "OAuth_2.0",
                "clientId": "<clientId>",
                "clientSecret": {
                     "type": "SecureString",
                     "value": "<clientSecret>"
                },
                "useEncryptedEndpoints": true,
                "useHostVerification": true,
                "usePeerVerification": true
            }
        }
    }
}

```

Aşağıdaki yük ile Salesforce pazarlama bulutu bağlı hizmetini kullanıyorsanız, bu, hala olduğu gibi desteklenirken, Gelişmiş paket desteği ekleyen yeni bir iletme işlemi kullanmanız önerilir.

```json
{
    "name": "SalesforceMarketingCloudLinkedService",
    "properties": {
        "type": "SalesforceMarketingCloud",
        "typeProperties": {
            "clientId": "<clientId>",
            "clientSecret": {
                 "type": "SecureString",
                 "value": "<clientSecret>"
            },
            "useEncryptedEndpoints": true,
            "useHostVerification": true,
            "usePeerVerification": true
        }
    }
}

```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Veri kümelerini tanımlamaya yönelik bölümlerin ve özelliklerin tam listesi için bkz. [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölüm, Salesforce pazarlama bulut veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

Salesforce pazarlama bulutundaki verileri kopyalamak için veri kümesinin Type özelliğini **Salesforcemarketing Cloudobject** olarak ayarlayın. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| tür | Veri kümesinin Type özelliği şu şekilde ayarlanmalıdır: **Salesforcepazarbir dobject** | Yes |
| tableName | Tablonun adı. | Hayır (etkinlik kaynağı içinde "sorgu" belirtilmişse) |

**Örnek**

```json
{
    "name": "SalesforceMarketingCloudDataset",
    "properties": {
        "type": "SalesforceMarketingCloudObject",
        "typeProperties": {},
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<SalesforceMarketingCloud linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Etkinlikleri tanımlamaya yönelik bölümlerin ve özelliklerin tam listesi için bkz. işlem [hatları](concepts-pipelines-activities.md) makalesi. Bu bölüm, Salesforce pazarlama bulut kaynağı tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="salesforce-marketing-cloud-as-source"></a>Kaynak olarak Salesforce pazarlama bulutu

Salesforce pazarlama bulutundaki verileri kopyalamak için kopyalama etkinliğindeki kaynak türünü **Salesforcemarketing CloudSource** olarak ayarlayın. Aşağıdaki özellikler, etkinlik **kaynağını** kopyalama bölümünde desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| tür | Kopyalama etkinliği kaynağının Type özelliği şu şekilde ayarlanmalıdır: **Salesforcepazarlamakaynağı** | Yes |
| sorgu | Verileri okumak için özel SQL sorgusunu kullanın. Örneğin: `"SELECT * FROM MyTable"`. | Hayır (veri kümesinde "tableName" belirtilmişse) |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromSalesforceMarketingCloud",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<SalesforceMarketingCloud input dataset name>",
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
                "type": "SalesforceMarketingCloudSource",
                "query": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="lookup-activity-properties"></a>Arama etkinliği özellikleri

Özelliklerle ilgili ayrıntıları öğrenmek için [arama etkinliğini](control-flow-lookup-activity.md)denetleyin.

## <a name="next-steps"></a>Sonraki adımlar
Azure Data Factory içindeki kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).
