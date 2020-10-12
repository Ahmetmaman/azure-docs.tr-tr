---
title: Azure Data Factory kullanarak Phoenix 'ten veri kopyalama
description: Azure Data Factory işlem hattındaki kopyalama etkinliğini kullanarak, Phoenix 'ten desteklenen havuz veri depolarına veri kopyalamayı öğrenin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 09/04/2019
ms.author: jingwang
ms.openlocfilehash: d236b9f8800b644a0aa51597d01df1c1442475ac
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "81416766"
---
# <a name="copy-data-from-phoenix-using-azure-data-factory"></a>Azure Data Factory kullanarak Phoenix 'ten veri kopyalama 
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu makalede, verileri Phoenix 'ten kopyalamak için Azure Data Factory kopyalama etkinliğinin nasıl kullanılacağı özetlenmektedir. Kopyalama etkinliğine genel bir bakış sunan [kopyalama etkinliğine genel bakış](copy-activity-overview.md) makalesinde oluşturulur.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Bu Phoenix Bağlayıcısı aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/havuz matrisi](copy-activity-overview.md) ile [kopyalama etkinliği](copy-activity-overview.md)
- [Arama etkinliği](control-flow-lookup-activity.md)

Phoenix 'ten desteklenen herhangi bir havuz veri deposuna veri kopyalayabilirsiniz. Kopyalama etkinliği tarafından kaynak/havuz olarak desteklenen veri depolarının listesi için [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablosuna bakın.

Azure Data Factory, bağlantıyı etkinleştirmek için yerleşik bir sürücü sağlar, bu nedenle bu bağlayıcıyı kullanarak herhangi bir sürücüyü el ile yüklemeniz gerekmez.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [data-factory-v2-integration-runtime-requirements](../../includes/data-factory-v2-integration-runtime-requirements.md)]

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, Phoenix bağlayıcısına özgü Data Factory varlıkları tanımlamak için kullanılan özelliklerle ilgili ayrıntıları sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmet özellikleri

Phoenix bağlantılı hizmeti için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| tür | Type özelliği: **Phoenix** olarak ayarlanmalıdır | Evet |
| konak | Phoenix sunucusunun IP adresi veya ana bilgisayar adı. (yani, 192.168.222.160)  | Evet |
| port | Phoenix sunucusunun istemci bağlantılarını dinlemek için kullandığı TCP bağlantı noktası. Varsayılan değer 8765 ' dir. Azure Hdınsights 'e bağlanıyorsanız, bağlantı noktasını 443 olarak belirtin. | Hayır |
| httpPath | Phoenix sunucusuna karşılık gelen kısmi URL. (yani,/Gateway/Sandbox/Phoenix/Version). `/hbasephoenix0`Hdınsights kümesi kullanıp kullanın belirtin.  | Hayır |
| authenticationType | Phoenix sunucusuna bağlanmak için kullanılan kimlik doğrulama mekanizması. <br/>İzin verilen değerler: **Anonymous**, **userNameAndPassWord**, **WindowsAzureHDInsightService** | Evet |
| username | Phoenix sunucusuna bağlanmak için kullanılan Kullanıcı adı.  | Hayır |
| password | Kullanıcı adına karşılık gelen parola. Data Factory güvenli bir şekilde depolamak için bu alanı SecureString olarak işaretleyin veya [Azure Key Vault depolanan bir gizli dizi başvurusu](store-credentials-in-key-vault.md)yapın. | Hayır |
| enableSsl | Sunucu bağlantılarının TLS kullanılarak şifrelenip şifrelenmediğini belirtir. Varsayılan değer false'tur.  | Hayır |
| trustedCertPath | TLS üzerinden bağlanılırken sunucuyu doğrulamak için güvenilir CA sertifikaları içeren. ped dosyasının tam yolu. Bu özellik yalnızca, şirket içinde barındırılan IR 'de TLS kullanılırken ayarlanabilir. Varsayılan değer, IR ile birlikte yüklenen CAcert. Pez dosyasıdır.  | Hayır |
| useSystemTrustStore | Sistem güven deposundan veya belirtilen ped dosyasından bir CA sertifikası kullanılıp kullanılmayacağını belirtir. Varsayılan değer false'tur.  | Hayır |
| Allowwhostnamecnuyuşmazlığını | TLS üzerinden bağlanılırken, CA tarafından verilen bir TLS/SSL sertifika adının, sunucunun ana bilgisayar adıyla eşleşmesi gerekip gerekmediğini belirtir. Varsayılan değer false'tur.  | Hayır |
| allowSelfSignedServerCert | Sunucudan kendinden imzalı sertifikalara izin verilip verilmeyeceğini belirtir. Varsayılan değer false'tur.  | Hayır |
| connectVia | Veri deposuna bağlanmak için kullanılacak [Integration Runtime](concepts-integration-runtime.md) . [Önkoşullar](#prerequisites) bölümünden daha fazla bilgi edinin. Belirtilmemişse, varsayılan Azure Integration Runtime kullanır. |Hayır |

>[!NOTE]
>Kümeniz, HDInsight gibi yapışkan oturumu desteklemiyorsa, http yolu ayarının sonunda doğrudan düğüm dizini ekleyin, örneğin `/hbasephoenix0` yerine belirtin `/hbasephoenix` .

**Örnek:**

```json
{
    "name": "PhoenixLinkedService",
    "properties": {
        "type": "Phoenix",
        "typeProperties": {
            "host" : "<cluster>.azurehdinsight.net",
            "port" : "443",
            "httpPath" : "/hbasephoenix0",
            "authenticationType" : "WindowsAzureHDInsightService",
            "username" : "<username>",
            "password": {
                 "type": "SecureString",
                 "value": "<password>"
            }
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Veri kümelerini tanımlamaya yönelik bölümlerin ve özelliklerin tam listesi için bkz. [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölüm, Phoenix veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

Phoenix 'ten veri kopyalamak için, veri kümesinin Type özelliğini **PhoenixObject**olarak ayarlayın. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| tür | DataSet 'in Type özelliği: **PhoenixObject** olarak ayarlanmalıdır | Evet |
| schema | Şemanın adı. |Hayır (etkinlik kaynağı içinde "sorgu" belirtilmişse)  |
| tablo | Tablonun adı. |Hayır (etkinlik kaynağı içinde "sorgu" belirtilmişse)  |
| tableName | Şemanın bulunduğu tablonun adı. Bu özellik geriye dönük uyumluluk için desteklenir. `schema` `table` Yeni iş yükü için ve kullanın. | Hayır (etkinlik kaynağı içinde "sorgu" belirtilmişse) |

**Örnek**

```json
{
    "name": "PhoenixDataset",
    "properties": {
        "type": "PhoenixObject",
        "typeProperties": {},
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<Phoenix linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Etkinlikleri tanımlamaya yönelik bölümlerin ve özelliklerin tam listesi için bkz. işlem [hatları](concepts-pipelines-activities.md) makalesi. Bu bölüm, Phoenix kaynağı tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="phoenix-as-source"></a>Kaynak olarak Phoenix

Phoenix 'ten veri kopyalamak için kopyalama etkinliğindeki kaynak türünü **PhoenixSource**olarak ayarlayın. Aşağıdaki özellikler, etkinlik **kaynağını** kopyalama bölümünde desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| tür | Kopyalama etkinliği kaynağının Type özelliği: **PhoenixSource** olarak ayarlanmalıdır | Evet |
| sorgu | Verileri okumak için özel SQL sorgusunu kullanın. Örneğin: `"SELECT * FROM MyTable"`. | Hayır (veri kümesinde "tableName" belirtilmişse) |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromPhoenix",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Phoenix input dataset name>",
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
                "type": "PhoenixSource",
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
