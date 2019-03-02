---
title: Data Factory kullanarak Azure Data Lake depolama Gen1 gelen veya veri kopyalama | Microsoft Docs
description: Data Factory kullanarak desteklenen havuz depolarına, Data Lake Store veya Azure Data Lake Store için desteklenen kaynak veri depolarından veri kopyalama hakkında bilgi edinin.
services: data-factory
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 02/22/2019
ms.author: jingwang
ms.openlocfilehash: 9976abfbf3a338b46e2acf05e6231a3a85ff6dd3
ms.sourcegitcommit: c712cb5c80bed4b5801be214788770b66bf7a009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57217470"
---
# <a name="copy-data-to-or-from-azure-data-lake-storage-gen1-by-using-azure-data-factory"></a>Azure Data Factory kullanarak Azure Data Lake depolama Gen1 gelen veya veri kopyalama
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1](v1/data-factory-azure-datalake-connector.md)
> * [Geçerli sürüm](connector-azure-data-lake-store.md)

Bu makalede, kopyalama etkinliği Azure Data Factory ve Azure Data Lake depolama Gen1 (daha önce Azure Data Lake Store da bilinir) veri kopyalamak için nasıl kullanılacağını özetlenmektedir. Yapılar [kopyalama etkinliğine genel bakış](copy-activity-overview.md).

## <a name="supported-capabilities"></a>Desteklenen özellikler

Azure Data Lake Store için herhangi bir desteklenen kaynak veri deposundan veri kopyalama ya da Azure Data Lake Store ' tüm desteklenen havuz veri deposuna veri kopyalamak. Bkz: [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu Azure Data Lake Store bağlayıcı'yı destekler:

- Kimlik doğrulama aşağıdaki yöntemlerden birini kullanarak dosyaları kopyalanıyor: **hizmet sorumlusu** veya **kimliklerini Azure kaynakları için yönetilen**.
- Dosyaları olarak kopyalama-olan veya ayrıştırma ile dosya oluşturma [desteklenen dosya biçimleri ve codec sıkıştırma](supported-file-formats-and-compression-codecs.md).

> [!IMPORTANT]
> Şirket içinde barındırılan tümleştirme çalışma zamanını kullanarak verileri kopyalarsanız, giden trafiğe izin vermek için Kurumsal güvenlik duvarı yapılandırma `<ADLS account name>.azuredatalakestore.net` ve `login.microsoftonline.com/<tenant>/oauth2/token` bağlantı noktası 443 üzerinden. İkincisi Azure güvenlik belirteci, tümleştirme çalışma zamanı erişim belirteci almak için iletişim kurması için gereken hizmetidir.

## <a name="get-started"></a>başlarken

> [!TIP]
> Azure Data Lake Store Bağlayıcısı'nı kullanarak bir kılavuz için bkz. [Azure Data Lake Store ile veri yükleme](load-azure-data-lake-store.md).

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler belirli Data Factory varlıkları Azure Data Lake Store için tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Azure Data Lake Store bağlı hizmeti için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | `type` Özelliği ayarlanmalıdır **birlikte AzureDataLakeStore**. | Evet |
| dataLakeStoreUri | Azure Data Lake Store hesabı hakkında bilgi. Bu bilgiler aşağıdaki biçimlerden birini alır: `https://[accountname].azuredatalakestore.net/webhdfs/v1` veya `adl://[accountname].azuredatalakestore.net/`. | Evet |
| subscriptionId | Data Lake Store hesabına ait olduğu Azure abonelik kimliği. | Havuz için gerekli |
| resourceGroupName | Data Lake Store hesabına ait olduğu Azure kaynak grubu adı. | Havuz için gerekli |
| connectVia | [Integration runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. Integration runtime (veri deponuz özel bir ağda yer alıyorsa) şirket içinde barındırılan veya Azure tümleştirme çalışma zamanı kullanabilirsiniz. Bu özellik belirtilmezse, varsayılan Azure tümleştirme çalışma zamanı kullanır. |Hayır |

### <a name="use-service-principal-authentication"></a>Hizmet sorumlusu kimlik doğrulaması kullanma

Hizmet sorumlusu kimlik doğrulaması kullanmak için bir uygulama varlığı Azure Active Directory'ye kaydetmeniz ve Data Lake Store erişimi verin. Ayrıntılı adımlar için bkz. [hizmetten hizmete kimlik doğrulaması](../data-lake-store/data-lake-store-authenticate-using-active-directory.md). Bağlı hizmetini tanımlamak için kullandığınız şu değerleri not edin:

- Uygulama Kimliği
- Uygulama anahtarı
- Kiracı Kimliği

>[!IMPORTANT]
> Hizmet sorumlusu uygun Data Lake Store içinde izni olduğundan emin olun:
>- **Kaynak olarak**: İçinde **Veri Gezgini** > **erişim**, en az izni **okuma + yürütme** izni listeler ve klasör ve alt klasörlerde dosyaları kopyalayın. Ya da size verebilir **okuma** tek bir dosyayı kopyalama izni. Eklemek seçebileceğiniz **bu klasör ve tüm alt öğeleri** için özyinelemeli olarak ekleyin **erişim izni ve varsayılan izin girdisi**. Hesap düzeyinde erişim denetimi (IAM) gereksinimi yoktur.
>- **Havuz olarak**: İçinde **Veri Gezgini** > **erişim**, en az izni **yazma + yürütme** klasörde alt öğeler oluşturmak için izni. Eklemek seçebileceğiniz **bu klasör ve tüm alt öğeleri** için özyinelemeli olarak ekleyin **erişim izni ve varsayılan izin girdisi**. Kopyalamak için Azure tümleştirme çalışma zamanı kullanıyorsanız (hem kaynak hem de bulutta) verme IAM içinde en az **okuyucu** Data Factory'ye Data Lake Store için bölge algılamak izin vermek için rol. Bu IAM rol açıkça önlemek istediğiniz [Azure tümleştirme çalışma zamanı oluşturma](create-azure-integration-runtime.md#create-azure-ir) Data Lake Store konumu ile. Bunları, aşağıdaki örnek olarak Data Lake Store bağlı hizmetinde ilişkilendirin.

>[!NOTE]
>Listeye klasör, kökten başlayarak izni için izin verilen hizmet sorumlusunun ayarlamalısınız **"Yürütme" iznine sahip kök düzeyinde**. Kullandığınızda, bu durum geçerlidir:
>- **Kopyalama aracı veri** Yazar kopyalama işlem hattını için.
>- **Data Factory kullanıcı arabirimini** bağlantı ve geliştirme sırasında klasörleri gezinme test etmek için.
>Kök düzeyinde izin verme üzerinde kaygısı varsa, bağlantıyı test et ve Giriş yolu el ile yazma sırasında atlayabilirsiniz. Hizmet sorumlusu kopyalanacak uygun ile dosyalarını izni sürece kopyalama etkinliği çalışmaya devam eder.

Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| servicePrincipalId | Uygulamanın istemci kimliği belirtin. | Evet |
| serviceprincipalkey değerleri | Uygulama anahtarını belirtin. Bu alan olarak işaretlemek bir `SecureString` Data Factory'de güvenle depolamak için veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). | Evet |
| kiracı | Kiracı bilgileri (etki alanı adı veya Kiracı kimliği), uygulamanızın bulunduğu altında belirtin. Azure portalının sağ üst köşedeki fare getirerek geri alabilirsiniz. | Evet |

**Örnek:**

```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": {
                "type": "SecureString",
                "value": "<service principal key>"
            },
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
            "subscriptionId": "<subscription of ADLS>",
            "resourceGroupName": "<resource group of ADLS>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="managed-identity"></a> Azure kaynakları ile kimlik doğrulaması için yönetilen kimlikleri kullanmak

Veri Fabrikası ile ilişkilendirilebilen bir [yönetilen Azure kaynakları için kimliği](data-factory-service-identity.md), bu belirli veri fabrikası temsil eder. Data Lake Store kimlik doğrulaması, kendi hizmet sorumlusunu kullanmaya benzer şekilde, bu yönetilen kimlik doğrudan kullanabilirsiniz. Belirlenen Bu fabrika, verilere erişmek ve bunları kopyalamak için veya Data Lake Store sağlar.

Azure kaynakları ile kimlik doğrulaması için yönetilen kimlikleri kullanmak için:

1. [Data factory yönetilen kimlik bilgilerini alma](data-factory-service-identity.md#retrieve-managed-identity) "hizmet kimliği uygulama fabrikanızı birlikte oluşturulan kimliği" değerini kopyalayarak.
2. Yönetilen kimlik için Data Lake Store, bu notlar aşağıdaki hizmet sorumlusu için yaptığınız şekilde erişim.

>[!IMPORTANT]
> Data Lake Store data factory yönetilen kimlik uygun izin verme emin olun:
>- **Kaynak olarak**: İçinde **Veri Gezgini** > **erişim**, en az izni **okuma + yürütme** izni listeler ve klasör ve alt klasörlerde dosyaları kopyalayın. Ya da size verebilir **okuma** tek bir dosyayı kopyalama izni. Eklemek seçebileceğiniz **bu klasör ve tüm alt öğeleri** için özyinelemeli olarak ekleyin **erişim izni ve varsayılan izin girdisi**. Hesap düzeyinde erişim denetimi (IAM) gereksinimi yoktur.
>- **Havuz olarak**: İçinde **Veri Gezgini** > **erişim**, en az izni **yazma + yürütme** klasörde alt öğeler oluşturmak için izni. Eklemek seçebileceğiniz **bu klasör ve tüm alt öğeleri** için özyinelemeli olarak ekleyin **erişim izni ve varsayılan izin girdisi**. Kopyalamak için Azure tümleştirme çalışma zamanı kullanıyorsanız (hem kaynak hem de bulutta) verme IAM içinde en az **okuyucu** Data Factory'ye Data Lake Store için bölge algılamak izin vermek için rol. Bu IAM rol açıkça önlemek istediğiniz [Azure tümleştirme çalışma zamanı oluşturma](create-azure-integration-runtime.md#create-azure-ir) Data Lake Store konumu ile. Bunları, aşağıdaki örnek olarak Data Lake Store bağlı hizmetinde ilişkilendirin.

>[!NOTE]
>Listeye klasör, kökten başlayarak izni için izin verilen yönetilen kimlik ayarlamalısınız **"Yürütme" iznine sahip kök düzeyinde**. Kullandığınızda, bu durum geçerlidir:
>- **Kopyalama aracı veri** Yazar kopyalama işlem hattını için.
>- **Data Factory kullanıcı arabirimini** bağlantı ve geliştirme sırasında klasörleri gezinme test etmek için.
>Kök düzeyinde izin verme üzerinde kaygısı varsa, bağlantıyı test et ve Giriş yolu el ile yazma sırasında atlayabilirsiniz. Yönetilen kimlik kopyalanacak uygun ile dosyalarını izni sürece kopyalama etkinliği çalışmaya devam eder.

Azure Data Factory'de bağlı hizmet Data Lake Store genel bilgilerin yanı sıra herhangi bir özelliği belirtmeniz gerekmez.

**Örnek:**

```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "subscriptionId": "<subscription of ADLS>",
            "resourceGroupName": "<resource group of ADLS>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Azure Data Lake Store ve veri kopyalamak için ayarlanmış `type` veri kümesine özelliği **AzureDataLakeStoreFile**. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Dataset öğesinin type özelliği ayarlanmalıdır: **AzureDataLakeStoreFile** |Evet |
| folderPath | Data Lake Store içinde bir klasörün yolu. Belirtilmezse, kök dizinine işaret eder. <br/><br/>Joker karakter filtresi desteklenir, joker karakterlere izin şunlardır: `*` (sıfır veya daha fazla karakter ile eşleşir) ve `?` (eşleşen sıfır ya da tek bir karakter); kullanmak `^` joker karakter veya içinde bu kaçış karakteri, gerçek bir klasör adı varsa, kaçış için. <br/><br/>Örnekler: rootfolder/alt/daha fazla örneklere bakın [klasör ve dosya filtreleme örnekler](#folder-and-file-filter-examples). |Hayır |
| fileName | **Adı veya joker karakter filtresi** belirtilen "folderPath" altında dosyaları için. Bu özellik için bir değer belirtmezseniz, klasördeki tüm dosyaları için veri kümesini işaret eder. <br/><br/>Filtre için joker karakterlere izin verilir: `*` (sıfır veya daha fazla karakter ile eşleşir) ve `?` (eşleşen sıfır ya da tek bir karakter).<br/>-Örnek 1: `"fileName": "*.csv"`<br/>-Örnek 2: `"fileName": "???20180427.txt"`<br/>Kullanım `^` joker karakter veya içinde bu kaçış karakteri, gerçek dosya adı varsa, kaçış için.<br/><br/>Dosya adı değil belirtildiği zaman için bir çıktı veri kümesi ve **preserveHierarchy** belirtilmediyse etkinliği havuz kopyalama etkinliği, dosya adı şu deseni ile otomatik olarak oluşturur: "*Veri. [etkinlik çalıştırma kimliği GUID]. [GUID, FlattenHierarchy]. [biçim] yapılandırılmışsa. [yapılandırdıysanız sıkıştırma]* ". Örneğin, "Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.gz". Tablo adı yerine sorgu kullanarak tablo kaynağından kopyalarsanız, adı desendir "*[tablo adı]. [ Biçim]. [yapılandırdıysanız sıkıştırma]* ". Örneğin, "MyTable.csv". |Hayır |
| modifiedDatetimeStart | Dosya Filtresi özniteliğine dayanarak: Son değiştirme. Kendi son değiştirilme zamanı zaman aralığı içinde olduğunda dosyaları seçilir `modifiedDatetimeStart` ve `modifiedDatetimeEnd`. Zaman biçimi UTC saat diliminde uygulanan "2018-12-01T05:00:00Z". <br/><br/> Özellikler, hiçbir dosya öznitelik filtresi, veri kümesine uygulanacak anlamına NULL olabilir.  Zaman `modifiedDatetimeStart` datetime değerine sahip ancak `modifiedDatetimeEnd` NULL olduğu için daha büyük olan son değiştirilen özniteliği dosyaları geldiğini veya tarih saat değeri ile eşit seçilir.  Zaman `modifiedDatetimeEnd` datetime değerine sahip ancak `modifiedDatetimeStart` NULL ise, son değiştirilen özniteliği, tarih saat değeri seçilir daha az dosya anlamına gelir.| Hayır |
| modifiedDatetimeEnd | Dosya Filtresi özniteliğine dayanarak: Son değiştirme. Kendi son değiştirilme zamanı zaman aralığı içinde olduğunda dosyaları seçilir `modifiedDatetimeStart` ve `modifiedDatetimeEnd`. Zaman biçimi UTC saat diliminde uygulanan "2018-12-01T05:00:00Z". <br/><br/> Özellikler, hiçbir dosya öznitelik filtresi, veri kümesine uygulanacak anlamına NULL olabilir.  Zaman `modifiedDatetimeStart` datetime değerine sahip ancak `modifiedDatetimeEnd` NULL olduğu için daha büyük olan son değiştirilen özniteliği dosyaları geldiğini veya tarih saat değeri ile eşit seçilir.  Zaman `modifiedDatetimeEnd` datetime değerine sahip ancak `modifiedDatetimeStart` NULL ise, son değiştirilen özniteliği, tarih saat değeri seçilir daha az dosya anlamına gelir.| Hayır |
| biçim | İsterseniz **olarak dosya kopyalama-olan** dosya tabanlı depoları arasında (ikili kopya), her iki girdi ve çıktı veri kümesi tanımları biçimi bölümünde atlayın.<br/><br/>Ayrıştırma veya belirli bir biçime sahip dosyaları oluşturmak istiyorsanız, aşağıdaki dosya biçimi türleri desteklenir: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Ayarlama **türü** özelliği şu değerlerden biri olarak biçimine altında. Daha fazla bilgi için [metin biçimi](supported-file-formats-and-compression-codecs.md#text-format), [Json biçimine](supported-file-formats-and-compression-codecs.md#json-format), [Avro biçimi](supported-file-formats-and-compression-codecs.md#avro-format), [Orc biçimi](supported-file-formats-and-compression-codecs.md#orc-format), ve [Parquetbiçimi](supported-file-formats-and-compression-codecs.md#parquet-format) bölümler. |Hayır (yalnızca ikili kopya senaryosu için) |
| Sıkıştırma | Veri sıkıştırma düzeyi ve türünü belirtin. Daha fazla bilgi için [desteklenen dosya biçimleri ve codec sıkıştırma](supported-file-formats-and-compression-codecs.md#compression-support).<br/>Desteklenen türler şunlardır: **GZip**, **Deflate**, **Bzıp2**, ve **ZipDeflate**.<br/>Desteklenen düzeyleri şunlardır: **En iyi** ve **hızlı**. |Hayır |


>[!TIP]
>Bir klasör altındaki tüm dosyaları kopyalamak için belirtin **folderPath** yalnızca.<br>Belirli bir ada sahip tek bir dosya kopyalamak için belirtin **folderPath** klasör bölümüyle ve **fileName** dosya adına sahip.<br>Bir alt klasörü altında bulunan dosyaları kopyalamak için belirtin **folderPath** klasör bölümüyle ve **fileName** joker karakter Filtresi ile. 

**Örnek:**

```json
{
    "name": "ADLSDataset",
    "properties": {
        "type": "AzureDataLakeStoreFile",
        "linkedServiceName":{
            "referenceName": "<ADLS linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "folderPath": "datalake/myfolder/",
            "modifiedDatetimeStart": "2018-12-01T05:00:00Z",
            "modifiedDatetimeEnd": "2018-12-01T06:00:00Z",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ",",
                "rowDelimiter": "\n"
            },
            "compression": {
                "type": "GZip",
                "level": "Optimal"
            }
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md). Bu bölümde, Azure Data Lake Store kaynak ve havuz desteklenen özelliklerin bir listesini sağlar.

### <a name="azure-data-lake-store-as-source"></a>Kaynak olarak Azure Data Lake Store

Data Lake Store ' veri kopyalamak için kopyalama etkinliği kaynak türünü ayarlayın. **kümesinin kullanılması gerekir**. Kopyalama etkinliği aşağıdaki özellikler desteklenir **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | `type` Kopyalama etkinliği kaynak özelliği ayarlanmalıdır: **Kümesinin kullanılması gerekir**. |Evet |
| özyinelemeli | Belirtilen klasörün alt klasörleri ya da yalnızca veri yinelemeli olarak okunur olup olmadığını belirtir. Dikkat edin `recursive` true olarak ve havuz boş bir klasörü bir dosya tabanlı depolama veya alt değil ya da kopyalanır havuz oluşturulur. İzin verilen değerler: **true** (varsayılan) ve **false**. | Hayır |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromADLS",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<ADLS input dataset name>",
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
                "type": "AzureDataLakeStoreSource",
                "recursive": true
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="azure-data-lake-store-as-sink"></a>Havuz olarak Azure Data Lake Store

Data Lake Store için veri kopyalamak için kopyalama etkinliği Havuz türü ayarlayın. **AzureDataLakeStoreSink**. Aşağıdaki özellikler desteklenir **havuz** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | `type` Kopyalama etkinliği havuz özelliği ayarlanmalıdır: **AzureDataLakeStoreSink**. |Evet |
| copyBehavior | Kaynak dosyaları bir dosya tabanlı veri deposundan olduğunda kopyalama davranışını tanımlar.<br/><br/>İzin verilen değerler şunlardır:<br/><b>-(Varsayılan) PreserveHierarchy</b>: hedef klasördeki ise dosya hiyerarşisini korur. Kaynak dosyanın kaynak klasöre göreli yol hedef dosya hedef klasöre göreli yol aynıdır.<br/><b>-FlattenHierarchy</b>: kaynak klasördeki tüm dosyaları ilk hedef klasörün içinde düzeyindedir. Hedef dosyalar otomatik olarak oluşturulan adlarına sahip. <br/><b>-MergeFiles</b>: tüm dosyaları kaynak klasörden bir dosya birleştirir. Dosya/blob adı belirtilmezse, birleştirilmiş dosya adı belirtilen adıdır. Aksi takdirde, dosya adı otomatik olarak üretilir. | Hayır |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyToADLS",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<ADLS output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "AzureDataLakeStoreSink",
                "copyBehavior": "PreserveHierarchy"
            }
        }
    }
]
```

### <a name="folder-and-file-filter-examples"></a>Klasör ve dosya filtresi örnekleri

Bu bölümde, sonuçta elde edilen davranışını klasör yolu ve dosya adı joker filtrelerle açıklanmaktadır.

| folderPath | fileName | özyinelemeli | Kaynak klasör yapısını ve filtre sonucunu (dosyalar **kalın** alınır)|
|:--- |:--- |:--- |:--- |
| `Folder*` | (boş, varsayılan kullanın) | false | Klasörüdür<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.JSON<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.csv<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| `Folder*` | (boş, varsayılan kullanın) | true | Klasörüdür<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File4.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5.csv**<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| `Folder*` | `*.csv` | false | Klasörüdür<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.JSON<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.csv<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| `Folder*` | `*.csv` | true | Klasörüdür<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.JSON<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5.csv**<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |

### <a name="examples-of-behavior-of-the-copy-operation"></a>Kopyalama işlemi davranışını örnekleri

Bu bölümde farklı birleşimlerini kopyalama işlemi elde edilen davranışını açıklar `recursive` ve `copyBehavior` değerleri.

| özyinelemeli | copyBehavior | Kaynak klasör yapısı | Sonuçta elde edilen hedef |
|:--- |:--- |:--- |:--- |
| true |preserveHierarchy | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Hedef kaynak olarak aynı yapıya sahip Klasör1 oluşturulur:<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5. |
| true |flattenHierarchy | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Hedef Klasör1 aşağıdaki yapısı ile oluşturulur: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de otomatik olarak oluşturulan adı<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya2 için otomatik olarak oluşturulan ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya3 için otomatik olarak oluşturulan ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;File4 için otomatik olarak oluşturulan ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;File5 için otomatik olarak oluşturulan ad |
| true |mergeFiles | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Hedef Klasör1 aşağıdaki yapısı ile oluşturulur: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de dosya2 + dosya3 + File4 + 5 dosya içeriği, bir otomatik olarak oluşturulan dosya adında bir dosya halinde birleştirilir. |
| false |preserveHierarchy | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Hedef Klasör1 aşağıdaki yapısı ile oluşturulur:<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/><br/>Subfolder1 dosya3 File4 ve File5 ile değil teslim alındı. |
| false |flattenHierarchy | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Hedef Klasör1 aşağıdaki yapısı ile oluşturulur:<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de otomatik olarak oluşturulan adı<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya2 için otomatik olarak oluşturulan ad<br/><br/>Subfolder1 dosya3 File4 ve File5 ile değil teslim alındı. |
| false |mergeFiles | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Hedef Klasör1 aşağıdaki yapısı ile oluşturulur:<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de + dosya2 içeriği otomatik olarak oluşturulan dosya adında bir dosya halinde birleştirilir. Fıle1'de otomatik olarak oluşturulan adı<br/><br/>Subfolder1 dosya3 File4 ve File5 ile değil teslim alındı. |

## <a name="next-steps"></a>Sonraki adımlar
Azure veri fabrikasında kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md##supported-data-stores-and-formats).
