---
title: Açık hub aracılığıyla SAP Business Warehouse 'tan veri kopyalama
description: Azure Data Factory bir işlem hattındaki kopyalama etkinliğini kullanarak SAP Business Warehouse 'tan (siyah beyaz) açık Merkez aracılığıyla desteklenen havuz veri depolarına veri kopyalamayı öğrenin.
services: data-factory
documentationcenter: ''
ms.author: jingwang
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 06/12/2020
ms.openlocfilehash: 930c7e7881a00cd0cb1f4abc6b219c0fbdeebac5
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "87533419"
---
# <a name="copy-data-from-sap-business-warehouse-via-open-hub-using-azure-data-factory"></a>Azure Data Factory kullanarak SAP Business Warehouse 'tan açık hub aracılığıyla veri kopyalama
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu makalede, Açık hub aracılığıyla SAP Business Warehouse 'tan (bant genişliği) veri kopyalamak için Azure Data Factory kopyalama etkinliğinin nasıl kullanılacağı özetlenmektedir. Kopyalama etkinliğine genel bir bakış sunan [kopyalama etkinliğine genel bakış](copy-activity-overview.md) makalesinde oluşturulur.

>[!TIP]
>ADF 'nin SAP veri tümleştirme senaryosunda genel desteğini öğrenmek için, her SAP Bağlayıcısı, karşılaştırma ve kılavuza ayrıntılı giriş ile [Azure Data Factory Teknik İnceleme kullanarak SAP veri tümleştirme](https://github.com/Azure/Azure-DataFactory/blob/master/whitepaper/SAP%20Data%20Integration%20using%20Azure%20Data%20Factory.pdf) konusuna bakın.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Bu SAP Iş ambarı, açık Merkez bağlayıcı aracılığıyla aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/havuz matrisi](copy-activity-overview.md) ile [kopyalama etkinliği](copy-activity-overview.md)
- [Arama etkinliği](control-flow-lookup-activity.md)

SAP Business Warehouse 'tan verileri, desteklenen herhangi bir havuz veri deposuna açık hub aracılığıyla kopyalayabilirsiniz. Kopyalama etkinliği tarafından kaynak/havuz olarak desteklenen veri depolarının listesi için [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablosuna bakın.

Özellikle, bu SAP Business Warehouse açık hub Bağlayıcısı şunları destekler:

- SAP Business Warehouse **sürüm 7,01 veya üzeri (2015 yıldan sonra yayınlanan son sap desteği paket yığınında)**. SAP BW4/HANA bu bağlayıcı tarafından desteklenmiyor.
- ' Nin altında açık hub hedefi yerel tablosu aracılığıyla veri kopyalama, DSO, InfoCube, MultiProvider, DataSource vb. olabilir.
- Temel kimlik doğrulaması kullanarak verileri kopyalama.
- SAP uygulama sunucusuna veya SAP ileti sunucusuna bağlanma.
- RFC aracılığıyla veri alma.

## <a name="sap-bw-open-hub-integration"></a>SAP BW açık Hub tümleştirmesi 

[SAP BW açık hub hizmeti](https://wiki.scn.sap.com/wiki/display/BI/Overview+of+Open+Hub+Service) , verileri SAP BW ayıklamaya yönelik etkili bir yoldur. Aşağıdaki diyagramda, müşterilerin SAP sisteminde sahip olduğu tipik akışlardan biri gösterilmektedir. Bu durumda, verilerin SAP ECC-> PSA-> DSO-> küpünden akışı vardır.

SAP BW açık hub hedefi (OHD), SAP verilerinin geçirilme hedefini tanımlar. SAP Veri Aktarımı Process (DTP) tarafından desteklenen tüm nesneler açık Hub veri kaynakları (örneğin, DSO, InfoCube, DataSource vb.) olarak kullanılabilir. Açık hub hedefi türü-geçirilen verilerin depolandığı yer-veritabanı tabloları (yerel veya uzak) ve düz dosyalar olabilir. Bu SAP BW Open hub Connector, OHD yerel tablosundan sıyah verileri kopyalamayı destekler. Başka türler kullanıyorsanız, diğer bağlayıcıları kullanarak veritabanına veya dosya sistemine doğrudan bağlanabilirsiniz.

![Açık hub SAP BW](./media/connector-sap-business-warehouse-open-hub/sap-bw-open-hub.png)

## <a name="delta-extraction-flow"></a>Değişim ayıklama akışı

ADF SAP BW Open hub Bağlayıcısı iki isteğe bağlı özellik sunar: `excludeLastRequest` ve `baseRequestId` Açık hub 'dan Delta yükünü işlemek için kullanılabilir. 

- **Excludelastrequestıd**: son isteğin kayıtlarının dışlanıp dışlanmayacağı. True varsayılan değerdir. 
- **baseRequestId**: Delta yükleme isteğinin kimliği. Ayarlandıktan sonra yalnızca RequestId ile bu özelliğin değerinden büyük olan veriler alınır. 

Genel olarak, SAP bilgi sağlayıcılarından Azure Data Factory (ADF) olarak ayıklama 2 adımdan oluşur: 

1. **SAP BW veri aktarımı işlemi (DTP)** Bu adım, verileri bir SAP BW ınfoprovider 'tan SAP BW açık hub tablosuna kopyalar 

1. **ADF veri kopyalama** Bu adımda, Açık hub tablosu ADF Bağlayıcısı tarafından okundu 

![Değişim ayıklama akışı](media/connector-sap-business-warehouse-open-hub/delta-extraction-flow.png)

İlk adımda bir DTP yürütülür. Her yürütme yeni bir SAP istek KIMLIĞI oluşturur. İstek KIMLIĞI açık hub tablosunda depolanır ve sonra, Delta tanımlamak için ADF Bağlayıcısı tarafından kullanılır. İki adım zaman uyumsuz olarak çalışır: DTP SAP tarafından tetiklenir ve ADF veri kopyalama, ADF aracılığıyla tetiklenir. 

Varsayılan olarak, ADF açık hub tablosundan en son Delta değerini okumuyor ("son isteği hariç tut" seçeneği doğrudur). Burada, ADF 'deki veriler açık hub tablosundaki veriler ile %100 güncel değildir (son Delta eksik). Bu yordam, dönüş sırasında zaman uyumsuz ayıklamanın neden olduğu hiçbir satırın kaybolmamasını sağlar. DTP hala aynı tabloya yazarken ADF, Açık hub tablosunu okurken bile sorunsuz bir şekilde çalışıyor. 

Genellikle, en fazla kopyalanmış istek KIMLIĞINI bir hazırlama veri deposunda (Yukarıdaki diyagramda Azure Blob gibi) ADF tarafından son çalıştırmada depoladığınız bir şekilde depoluyordu. Bu nedenle, sonraki çalıştırmada aynı istek ADF tarafından ikinci kez okunmaz. Bu arada, verilerin açık hub tablosundan otomatik olarak silinmediğini not edin.

Doğru Delta işleme için, aynı Açık hub tablosunda farklı DTPs 'lerden istek kimliklerinin bulunmasına izin verilmez. Bu nedenle, her bir açık hub hedefi (OHD) için birden fazla DTP oluşturmanız gerekir. Aynı bilgi sağlayıcısından tam ve Delta ayıklama gereksinimi olduğunda, aynı ınfoprovider için iki Dirend oluşturmalısınız. 

## <a name="prerequisites"></a>Önkoşullar

Bu SAP Business Warehouse açık hub bağlayıcısını kullanmak için şunları yapmanız gerekir:

- 3,13 veya üzeri bir sürümü olan şirket içinde barındırılan Integration Runtime ayarlayın. Ayrıntılar için bkz. [Şirket içinde barındırılan Integration Runtime](create-self-hosted-integration-runtime.md) makalesi.

- SAP web sitesinden **64 bitlik [sap .net Connector 3,0](https://support.sap.com/en/product/connectors/msnet.html) ** ' i indirin ve şirket içinde barındırılan IR makinesine yükleyin. Yükleme sırasında, isteğe bağlı kurulum adımları penceresinde, aşağıdaki görüntüde gösterildiği gibi **GAC 'ye derlemeleri yükleme** seçeneğini seçtiğinizden emin olun. 

    ![SAP .NET bağlayıcısını yükler](./media/connector-sap-business-warehouse-open-hub/install-sap-dotnet-connector.png)

- Data Factory bant genişliği Bağlayıcısı 'nda kullanılan SAP kullanıcısının aşağıdaki izinlere sahip olması gerekir: 

    - RFC ve SAP BW için yetkilendirme. 
    - "S_SDSAUTH" yetkilendirme nesnesinin "yürütme" etkinliğinin izinleri.

- "Teknik anahtar" seçeneği işaretli SAP Open hub hedef türünü **veritabanı tablosu** olarak oluşturun.  Ayrıca, gerekli olmasa da tablo silme, tablodaki verileri işaretsiz olarak bırakmak için de önerilir. , Açık Merkez hedefi tablosuna seçtiğiniz kaynak nesneden (küp gibi) verileri aktarmak için DTP 'den yararlanın (doğrudan var olan işlem zinciriyle yürütün veya tümleştirin).

## <a name="getting-started"></a>Başlarken

> [!TIP]
>
> SAP BW açık hub bağlayıcısını kullanmaya yönelik bir anlatım için, bkz. [Azure Data Factory kullanarak SAP Business Warehouse 'tan (bant genişliği) veri yükleme](load-sap-bw-data.md).

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümlerde, SAP Business Warehouse açık hub Bağlayıcısı ' na özgü Data Factory varlıkları tanımlamak için kullanılan özellikler hakkında ayrıntılı bilgi sağlanmaktadır.

## <a name="linked-service-properties"></a>Bağlı hizmet özellikleri

SAP Business Warehouse açık hub bağlı hizmeti için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| tür | Type özelliği: **Sapopenhub** olarak ayarlanmalıdır | Evet |
| sunucu | SAP BW örneğinin bulunduğu sunucunun adı. | Evet |
| systemNumber | SAP BW sisteminin sistem numarası.<br/>İzin verilen değer: dize olarak temsil edilen iki basamaklı ondalık sayı. | Evet |
| messageServer | SAP ileti sunucusunun ana bilgisayar adı.<br/>Bir SAP ileti sunucusuna bağlanmak için kullanın. | Hayır |
| messageServerService | İleti sunucusunun hizmet adı veya bağlantı noktası numarası.<br/>Bir SAP ileti sunucusuna bağlanmak için kullanın. | Hayır |
| SystemId | Tablonun bulunduğu SAP sisteminin KIMLIĞI.<br/>Bir SAP ileti sunucusuna bağlanmak için kullanın. | Hayır |
| logonGroup | SAP sistemi için oturum açma grubu.<br/>Bir SAP ileti sunucusuna bağlanmak için kullanın. | Hayır |
| clientId | SAP W sistemindeki istemcinin istemci KIMLIĞI.<br/>İzin verilen değer: dize olarak temsil edilen üç basamaklı ondalık sayı. | Evet |
| language | SAP sisteminin kullandığı dil. | Hayır (varsayılan değer **en**)|
| userName | SAP sunucusuna erişimi olan kullanıcının adı. | Evet |
| password | Kullanıcının parolası. Data Factory güvenli bir şekilde depolamak için bu alanı SecureString olarak işaretleyin veya [Azure Key Vault depolanan bir gizli dizi başvurusu](store-credentials-in-key-vault.md)yapın. | Evet |
| connectVia | Veri deposuna bağlanmak için kullanılacak [Integration Runtime](concepts-integration-runtime.md) . [Önkoşul](#prerequisites)bölümünde belirtildiği gibi, kendinden konak Integration Runtime gereklidir. |Evet |

**Örnek:**

```json
{
    "name": "SapBwOpenHubLinkedService",
    "properties": {
        "type": "SapOpenHub",
        "typeProperties": {
            "server": "<server name>",
            "systemNumber": "<system number>",
            "clientId": "<client id>",
            "userName": "<SAP user>",
            "password": {
                "type": "SecureString",
                "value": "<Password for SAP user>"
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

Veri kümelerini tanımlamaya yönelik bölümlerin ve özelliklerin tam listesi için bkz. [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölüm SAP BW açık Hub veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

Ve SAP BW açık hub 'a veri kopyalamak için, veri kümesinin Type özelliğini **Sapopenhubtable**olarak ayarlayın. Aşağıdaki özellikler desteklenir.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| tür | Type özelliği **Sapopenhubtable**olarak ayarlanmalıdır.  | Evet |
| openHubDestinationName | Verilerin kopyalanacağı açık hub hedefinin adı. | Evet |

`excludeLastRequest`Veri kümesinde ve ' i ayarlıyorsanız, `baseRequestId` hala olduğu gibi desteklenir, ancak etkinlik kaynağı ' nda yeni modeli kullanmanız önerilir.

**Örnek:**

```json
{
    "name": "SAPBWOpenHubDataset",
    "properties": {
        "type": "SapOpenHubTable",
        "typeProperties": {
            "openHubDestinationName": "<open hub destination name>"
        },
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<SAP BW Open Hub linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Etkinlikleri tanımlamaya yönelik bölümlerin ve özelliklerin tam listesi için bkz. işlem [hatları](concepts-pipelines-activities.md) makalesi. Bu bölüm SAP BW açık Merkez kaynağı tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="sap-bw-open-hub-as-source"></a>Açık hub 'ı kaynak olarak SAP BW

SAP BW açık hub 'dan veri kopyalamak için, etkinlik **kaynağını** kopyalama bölümünde aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| tür | Kopyalama etkinliği kaynağının **Type** özelliği **Sapopenhubsource**olarak ayarlanmalıdır. | Evet |
| excludeLastRequest | Son isteğin kayıtlarının dışlanıp dışlanmayacağı. | Hayır (varsayılan değer **doğru**) |
| baseRequestId | Delta yükleme isteğinin Kımlığı. Ayarlandıktan sonra yalnızca RequestId ile bu özelliğin değerinden **büyük** olan veriler alınır.  | Hayır |

>[!TIP]
>Açık hub tablonuz yalnızca tek bir istek KIMLIĞI tarafından oluşturulan verileri içeriyorsa, her zaman tam yükleme yapın ve tablodaki mevcut verilerin üzerine yazar veya test için yalnızca DTP 'yi bir kez çalıştırırsanız, verileri dışarı kopyalamak için "excludeLastRequest" seçeneğinin işaretini kaldırmanız gerektiğini unutmayın.

Veri yüklemeyi hızlandırmak için [`parallelCopies`](copy-activity-performance-features.md#parallel-copy) kopyalama etkinliğini, SAP BW açık hub 'dan paralel olarak yüklemek için ayarlayabilirsiniz. Örneğin, `parallelCopies` dört olarak ayarlarsanız Data Factory eşzamanlı olarak dört RFC çağrısını yürütür ve her RFC çağrısı, DTP Istek kimliği ve paket kimliği tarafından bölümlenen SAP BW açık hub tablosundan verilerin bir kısmını alır. Bu, benzersiz DTP istek KIMLIĞI + paket KIMLIĞI sayısının değerinden büyük olduğunda geçerlidir `parallelCopies` . Dosya tabanlı veri deposuna veri kopyalarken, bir klasöre birden çok dosya (yalnızca klasör adını belirt) olarak yazmak da daha da iyidir. Bu durumda, performans tek bir dosyaya yazılmasından daha iyidir.

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromSAPBWOpenHub",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<SAP BW Open Hub input dataset name>",
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
                "type": "SapOpenHubSource",
                "excludeLastRequest": true
            },
            "sink": {
                "type": "<sink type>"
            },
            "parallelCopies": 4
        }
    }
]
```

## <a name="data-type-mapping-for-sap-bw-open-hub"></a>SAP BW açık Hub için veri türü eşleme

SAP BW açık hub 'dan veri kopyalarken aşağıdaki eşlemeler, SAP BW veri türlerinden Azure Data Factory geçici veri türlerine kadar kullanılır. Kopyalama etkinliğinin kaynak şemayı ve veri türünü havuza nasıl eşlediğini öğrenmek için bkz. [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md) .

| SAP ABAP türü | Veri Fabrikası geçici veri türü |
|:--- |:--- |
| C (dize) | Dize |
| I (tamsayı) | Int32 |
| F (float) | Çift |
| D (Tarih) | Dize |
| T (Time) | Dize |
| P (BCD paketlenmiş, para birimi, ondalık, miktar) | Ondalık |
| N (NUMC) | Dize |
| X (Ikili ve ham) | Dize |

## <a name="lookup-activity-properties"></a>Arama etkinliği özellikleri

Özelliklerle ilgili ayrıntıları öğrenmek için [arama etkinliğini](control-flow-lookup-activity.md)denetleyin.

## <a name="troubleshooting-tips"></a>Sorun giderme ipuçları

**Belirtiler:** HANA 'da SAP BW çalıştırıyorsanız ve yalnızca ADF kopyalama etkinliği (1.000.000 satırları) kullanılarak verilerin yalnızca bir alt kümesini gözlemlerseniz, olası neden DTP 'niz içinde "SAP HANA yürütme" seçeneğini etkinleştirmenizi ve bu durumda ADF 'nin yalnızca ilk veri toplu işini alabilmesini sağlar.

**Çözüm:** DTP 'de "SAP HANA yürütme" seçeneğini devre dışı bırakın, verileri yeniden işleyin, sonra kopyalama etkinliğini yeniden yürütmeyi deneyin.

## <a name="next-steps"></a>Sonraki adımlar
Azure Data Factory içindeki kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).
