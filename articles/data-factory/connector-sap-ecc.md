---
title: SAP ECC Azure Data Factory kullanarak verileri kopyalama | Microsoft Docs
description: Desteklenen bir havuz veri depolarına SAP ECC bir Azure Data Factory işlem hattında kopyalama etkinliği'ni kullanarak veri kopyalama hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 04/26/2018
ms.author: jingwang
ms.openlocfilehash: d6a6d9b352db61d98e85c840a3ebc5cb6a832a3f
ms.sourcegitcommit: a1cf88246e230c1888b197fdb4514aec6f1a8de2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2019
ms.locfileid: "54352470"
---
# <a name="copy-data-from-sap-ecc-using-azure-data-factory"></a>SAP ECC Azure Data Factory kullanarak veri kopyalama

Bu makalede, kopyalama etkinliği Azure Data Factory'de veri SAP ECC (SAP Kurumsal merkezi bileşeni) kopyalamak için nasıl kullanılacağını özetlenmektedir. Yapılar [kopyalama etkinliği'ne genel bakış](copy-activity-overview.md) kopyalama etkinliği genel bir bakış sunan makalesi.

## <a name="supported-capabilities"></a>Desteklenen özellikler

SAP ECC tüm desteklenen havuz veri deposuna veri kopyalayabilirsiniz. Kaynakları/havuz kopyalama etkinliği tarafından desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu SAP ECC bağlayıcı'yı destekler:

- SAP ECC SAP NetWeaver sürüm 7.0 ve üzeri veri kopyalama. 
- Tüm SAP ECC OData hizmetlerini (ör SAP tablo/görünümler, BAPI, veri ayıklayıcıları, vb.) tarafından sunulan nesneleri veya OData göreli bağdaştırıcıları aracılığıyla olarak alınan SAP PI gönderilen veri/IDoc veri kopyalama.
- Temel kimlik doğrulaması kullanarak veri kopyalama.

## <a name="prerequisites"></a>Önkoşullar

Genel olarak, SAP ECC varlıkları SAP ağ geçidi ile OData Hizmetleri yoluyla kullanıma sunar. SAP ECC bu bağlayıcıyı kullanmak için gerekir:

- **SAP ağ geçidi ayarlama**. SAP NetWeaver sürüm 7.4 yüksek olan sunucuları için SAP ağ geçidi zaten yüklü. Karışık ağ geçidi veya ağ geçidi yüklemenize gerek Aksi takdirde OData Hizmetleri aracılığıyla SAP ECC verilerin ifşa önce hub. SAP ağ geçidini ayarlama hakkında bilgi edinin [Yükleme Kılavuzu](https://help.sap.com/saphelp_gateway20sp12/helpdata/en/c3/424a2657aa4cf58df949578a56ba80/frameset.htm).

- **Etkinleştirme ve SAP OData hizmeti yapılandırma**. Saniyeler içinde TCODE SICF ile OData Hizmetleri etkinleştirebilirsiniz. Ayrıca, hangi nesneleri sağlamak gereksinimlerini yapılandırabilirsiniz. Bir örneği aşağıdadır [adım adım rehberlik](https://blogs.sap.com/2012/10/26/step-by-step-guide-to-build-an-odata-service-based-on-rfcs-part-1/).

## <a name="getting-started"></a>Başlarken

.NET SDK'sı, Python SDK'sı, Azure PowerShell, REST API'si veya Azure Resource Manager şablonu kullanarak kopyalama etkinlikli bir işlem hattı oluşturabilirsiniz. Bkz: [kopyalama etkinliği Öğreticisi](quickstart-create-data-factory-dot-net.md) kopyalama etkinliği ile işlem hattı oluşturmak adım adım yönergeler için.

Aşağıdaki bölümler, Data Factory varlıklarını belirli SAP ECC bağlayıcıya tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

SAP ECC bağlı hizmeti için aşağıdaki özellikleri destekler:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **SapEcc** | Evet |
| url | SAP ECC OData hizmeti URL'si. | Evet |
| kullanıcı adı | SAP ECC için bağlanmak için kullanılan kullanıcı adı. | Hayır |
| password | SAP ECC için bağlanmak için kullanılan düz metin parolası. | Hayır |
| connectVia | [Integration Runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deponuz genel olarak erişilebilir değilse), şirket içinde barındırılan tümleştirme çalışma zamanı veya Azure Integration Runtime kullanabilirsiniz. Belirtilmezse, varsayılan Azure Integration Runtime kullanır. |Hayır |

**Örnek:**

```json
{
    "name": "SapECCLinkedService",
    "properties": {
        "type": "SapEcc",
        "typeProperties": {
            "url": "<SAP ECC OData url e.g. http://eccsvrname:8000/sap/opu/odata/sap/zgw100_dd02l_so_srv/>",
            "username": "<username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        }
    },
    "connectVia": {
        "referenceName": "<name of Integration Runtime>",
        "type": "IntegrationRuntimeReference"
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölümde, SAP ECC veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

SAP ECC verileri kopyalamak için dataset öğesinin type özelliği ayarlamak **SapEccResource**. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gereklidir |
|:--- |:--- |:--- |
| yol | SAP ECC OData varlık yolu. | Evet |

**Örnek**

```json
{
    "name": "SapEccDataset",
    "properties": {
        "type": "SapEccResource",
        "typeProperties": {
            "path": "<entity path e.g. dd04tentitySet>"
        },
        "linkedServiceName": {
            "referenceName": "<SAP ECC linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md) makalesi. Bu bölümde, SAP ECC kaynak tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="sap-ecc-as-source"></a>SAP ECC kaynağı olarak

SAP ECC verileri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **SapEccSource**. Kopyalama etkinliği aşağıdaki özellikler desteklenir **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı öğesinin type özelliği ayarlanmalıdır: **SapEccSource** | Evet |
| sorgu | Verilere filtre uygulamak için OData sorgu seçenekleri. Örnek: "$select adı, açıklama ve $top = 10 =".<br/><br/>SAP ECC bağlayıcı birleşik URL'den veri kopyalar: (bağlı hizmette belirtilen url) / (veri kümesinde belirtilen yol)? (sorgu) kopyalama etkinliği kaynağı belirtildi. Başvurmak [OData URL'si bileşenleri](http://www.odata.org/documentation/odata-version-3-0/url-conventions/). | Hayır |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromSAPECC",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<SAP ECC input dataset name>",
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
                "type": "SapEccSource",
                "query": "$top=10"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="data-type-mapping-for-sap-ecc"></a>SAP ECC için veri türü eşlemesi

SAP ECC veri kopyalama işlemi sırasında aşağıdaki eşlemeler OData veri türlerinden Azure veri fabrikası geçici veri türleri için SAP ECC veriler için kullanılır. Bkz: [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md) eşlemelerini nasıl yapar? kopyalama etkinliği kaynak şema ve veri türü için havuz hakkında bilgi edinmek için.

| OData veri türü | Veri Fabrikası geçici veri türü |
|:--- |:--- |:--- |
| Edm.Binary | Dize |
| Edm.Boolean | bool |
| Edm.Byte | Dize |
| Edm.DateTime | DateTime |
| Edm.Decimal | Onluk |
| Edm.Double | çift |
| Edm.Single | Tek |
| Edm.Guid | Dize |
| Edm.Int16 | Int16 |
| Edm.Int32 | Int32 |
| Edm.Int64 | Int64 |
| Edm.SByte | Int16 |
| Edm.String | Dize |
| Edm.Time | Zaman aralığı |
| Edm.DateTimeOffset | DateTimeOffset |

> [!NOTE]
> Karmaşık veri türleri artık desteklenmez.

## <a name="next-steps"></a>Sonraki adımlar
Azure Data Factory kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).
