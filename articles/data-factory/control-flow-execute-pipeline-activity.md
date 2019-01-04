---
title: Azure Data Factory'de işlem hattı etkinliğini yürütmek | Microsoft Docs
description: İşlem hattı yürütme etkinliği bir Data Factory işlem hattı başka bir Data Factory işlem hattından çağırmak için nasıl kullanabileceğinizi öğrenin.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: shlo
ms.openlocfilehash: f36d9eed11685d1bb35a46a97eb58fe870970075
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2019
ms.locfileid: "54019444"
---
# <a name="execute-pipeline-activity-in-azure-data-factory"></a>Azure Data Factory'de işlem hattı Etkinlik yürütme
İşlem hattı yürütme etkinliği bir Data Factory işlem hattının başka bir işlem hattını çağırmasını sağlar.

## <a name="syntax"></a>Sözdizimi

```json
{
    "name": "MyPipeline",
    "properties": {
        "activities": [
            {
                "name": "ExecutePipelineActivity",
                "type": "ExecutePipeline",
                "typeProperties": {
                    "parameters": {                        
                        "mySourceDatasetFolderPath": {
                            "value": "@pipeline().parameters.mySourceDatasetFolderPath",
                            "type": "Expression"
                        }
                    },
                    "pipeline": {
                        "referenceName": "<InvokedPipelineName>",
                        "type": "PipelineReference"
                    },
                    "waitOnCompletion": true
                 }
            }
        ],
        "parameters": [
            {
                "mySourceDatasetFolderPath": {
                    "type": "String"
                }
            }
        ]
    }
}
```

## <a name="type-properties"></a>Tür özellikleri
Özellik | Açıklama | İzin verilen değerler | Gereklidir
-------- | ----------- | -------------- | --------
ad | İşlem hattı yürütme etkinliğinin adı. | Dize | Evet
type | Ayarlamanız gerekir: **ExecutePipeline**. | Dize | Evet
ardışık düzen | Bu işlem hattını çağıran bağımlı işlem hattının işlem hattı başvuru. Bir işlem hattı başvuru nesnesi iki özelliğe sahiptir: **başvuru adını** ve **türü**. Başvuru adını Özellik Başvurusu işlem hattının adını belirtir. PipelineReference için type özelliği ayarlanmalıdır. | PipelineReference | Evet
parametreler | Çağrılan işlem hattına geçirilen parametreleri | Parametre adları ve bağımsız değişken değerleri eşleyen bir JSON nesnesi | Hayır
waitOnCompletion | Etkinlik yürütme tamamlanması için bağımlı bir işlem hattı yürütme beklemediğini tanımlar. | Varsayılan değer false’tur. | Boole | Hayır

## <a name="sample"></a>Örnek
Bu senaryo, iki işlem hattı sahiptir:

- **Ana işlem hattı** -çağrılan işlem hattını çağıran bir işlem hattı yürütme etkinliği bu işlem hattı içerir. Ana işlem hattı iki parametre alır: `masterSourceBlobContainer`, `masterSinkBlobContainer`.
- **Çağrılan işlem hattı** -bir Azure Blob kaynağından için havuz Azure Blob veri kopyalayan bir kopyalama etkinliği bu işlem hattı içerir. Çağrılan işlem hattı iki parametre alır: `sourceBlobContainer`, `sinkBlobContainer`.

### <a name="master-pipeline-definition"></a>Ana işlem hattı

```json
{
  "name": "masterPipeline",
  "properties": {
    "activities": [
      {
        "type": "ExecutePipeline",
        "typeProperties": {
          "pipeline": {
            "referenceName": "invokedPipeline",
            "type": "PipelineReference"
          },
          "parameters": {
            "sourceBlobContainer": {
              "value": "@pipeline().parameters.masterSourceBlobContainer",
              "type": "Expression"
            },
            "sinkBlobContainer": {
              "value": "@pipeline().parameters.masterSinkBlobContainer",
              "type": "Expression"
            }
          },
          "waitOnCompletion": true
        },
        "name": "MyExecutePipelineActivity"
      }
    ],
    "parameters": {
      "masterSourceBlobContainer": {
        "type": "String"
      },
      "masterSinkBlobContainer": {
        "type": "String"
      }
    }
  }
}

```

### <a name="invoked-pipeline-definition"></a>Çağrılan işlem hattı

```json
{
  "name": "invokedPipeline",
  "properties": {
    "activities": [
      {
        "type": "Copy",
        "typeProperties": {
          "source": {
            "type": "BlobSource"
          },
          "sink": {
            "type": "BlobSink"
          }
        },
        "name": "CopyBlobtoBlob",
        "inputs": [
          {
            "referenceName": "SourceBlobDataset",
            "type": "DatasetReference"
          }
        ],
        "outputs": [
          {
            "referenceName": "sinkBlobDataset",
            "type": "DatasetReference"
          }
        ]
      }
    ],
    "parameters": {
      "sourceBlobContainer": {
        "type": "String"
      },
      "sinkBlobContainer": {
        "type": "String"
      }
    }
  }
}

```

**Bağlı hizmet**

```json
{
    "name": "BlobStorageLinkedService",
    "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": {
        "value": "DefaultEndpointsProtocol=https;AccountName=*****",
        "type": "SecureString"
      }
    }
  }
}
```

**Kaynak veri kümesi**
```json
{
    "name": "SourceBlobDataset",
    "properties": {
    "type": "AzureBlob",
    "typeProperties": {
      "folderPath": {
        "value": "@pipeline().parameters.sourceBlobContainer",
        "type": "Expression"
      },
      "fileName": "salesforce.txt"
    },
    "linkedServiceName": {
      "referenceName": "BlobStorageLinkedService",
      "type": "LinkedServiceReference"
    }
  }
}
```

**Havuz veri kümesi**
```json
{
    "name": "sinkBlobDataset",
    "properties": {
    "type": "AzureBlob",
    "typeProperties": {
      "folderPath": {
        "value": "@pipeline().parameters.sinkBlobContainer",
        "type": "Expression"
      }
    },
    "linkedServiceName": {
      "referenceName": "BlobStorageLinkedService",
      "type": "LinkedServiceReference"
    }
  }
}
```

### <a name="running-the-pipeline"></a>İşlem hattını çalıştırma

Bu örnekte ana işlem hattını çalıştırmak için aşağıdaki değerleri masterSourceBlobContainer ve masterSinkBlobContainer parametrelerini geçirilir: 

```json
{
  "masterSourceBlobContainer": "executetest",
  "masterSinkBlobContainer": "executesink"
}
```

Aşağıdaki örnekte gösterildiği gibi ana işlem hattı çağrılan işlem hattı için bu değerleri iletir: 

```json
{
    "type": "ExecutePipeline",
    "typeProperties": {
      "pipeline": {
        "referenceName": "invokedPipeline",
        "type": "PipelineReference"
      },
      "parameters": {
        "sourceBlobContainer": {
          "value": "@pipeline().parameters.masterSourceBlobContainer",
          "type": "Expression"
        },
        "sinkBlobContainer": {
          "value": "@pipeline().parameters.masterSinkBlobContainer",
          "type": "Expression"
        }
      },

      ....
}

```
## <a name="next-steps"></a>Sonraki adımlar
Data Factory tarafından desteklenen diğer denetim akışı etkinlikleri bakın: 

- [Her etkinlik için](control-flow-for-each-activity.md)
- [Meta Veri Alma Etkinliği](control-flow-get-metadata-activity.md)
- [Arama Etkinliği](control-flow-lookup-activity.md)
- [Web etkinliği](control-flow-web-activity.md)
