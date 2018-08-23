---
title: Azure Data Factory kullanarak verileri Dynamics CRM veya Dynamics 365 (Common Data Service) kaynak ve hedef | Microsoft Docs
description: Microsoft Dynamics CRM uygulamasından veri kopyalama hakkında bilgi edinmek veya Microsoft Dynamics 365 (Common Data desteklenen Service), havuz veri deposu veya Dynamics CRM veya Dynamics 365, kaynak veri depolarını bir data factory işlem hattında kopyalama etkinliği'ni kullanarak desteklenir.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/02/2018
ms.author: jingwang
ms.openlocfilehash: e4ebddc35b402d7a8997d899ce97577e93a27b84
ms.sourcegitcommit: fab878ff9aaf4efb3eaff6b7656184b0bafba13b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2018
ms.locfileid: "42444869"
---
# <a name="copy-data-from-and-to-dynamics-365-common-data-service-or-dynamics-crm-by-using-azure-data-factory"></a>Azure Data Factory kullanarak veri kopyalama kaynak ve hedef (Common Data Service) Dynamics 365 veya Dynamics CRM

Bu makalede, kopyalama etkinliği Azure Data Factory'de gelen ve Microsoft Dynamics 365 veya Microsoft Dynamics CRM veri kopyalamak için nasıl kullanılacağını özetlenmektedir. Yapılar [kopyalama etkinliğine genel bakış](copy-activity-overview.md) kopyalama etkinliği genel bir bakış sunan makalesi.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Tüm desteklenen havuz veri deposuna (Common Data Service) Dynamics 365 veya Dynamics CRM veri kopyalayabilirsiniz. Tüm desteklenen kaynak veri deposundan (Common Data Service) Dynamics 365 veya Dynamics CRM veri kopyalayabilirsiniz. Kopyalama etkinliği tarafından kaynak ve havuz desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Bu Dynamics bağlayıcı aşağıdaki Dynamics sürümleri ve kimlik doğrulama türlerini destekler. (IFD için internet'e yönelik dağıtım içindir.)

| Dynamics sürümleri | Kimlik doğrulama türleri | Bağlı hizmet örnekleri |
|:--- |:--- |:--- |
| Dynamics 365 çevrimiçi <br> Dynamics CRM Online | Office365 | [Çevrimiçi Dynamics + Office365 kimlik doğrulaması](#dynamics-365-and-dynamics-crm-online) |
| Dynamics 365 ile şirket içi IFD <br> Dynamics CRM 2016 ile şirket içi IFD <br> Dynamics CRM 2015 ile şirket içi IFD | IFD | [Dynamics şirket içi IFD + IFD kimlik doğrulama](#dynamics-365-and-dynamics-crm-on-premises-with-ifd) |

Dynamics 365 için özellikle, aşağıdaki uygulama türlerini destekler:

- Dynamics 365 for Sales
- Dynamics 365 for Customer Service
- Dynamics 365 for Customer Service
- Dynamics 365 for Project Service Automation
- Pazarlama için Dynamics 365

Örneğin Operations ve Finans, beceri diğer uygulama türleri, vb. desteklenmez.

## <a name="get-started"></a>başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, Data Factory varlıklarını belirli Dynamics tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Aşağıdaki özellikler Dynamics bağlı hizmeti için desteklenir.

### <a name="dynamics-365-and-dynamics-crm-online"></a>Dynamics 365 ve Dynamics CRM Online

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır **Dynamics**. | Evet |
| deploymentType | Dynamics örnek dağıtım türü. Olmalıdır **"Çevrimiçi"** çevrimiçi Dynamics için. | Evet |
| serviceUri | Örneğin, Dynamics hizmet URL'sini örnek `https://adfdynamics.crm.dynamics.com`. | Evet |
| authenticationType | Bir Dynamics sunucusuna bağlanmak için kimlik doğrulaması türü. Belirtin **"Office365"** çevrimiçi Dynamics için. | Evet |
| kullanıcı adı | Dynamics bağlanmak için kullanıcı adı belirtin. | Evet |
| password | Kullanıcı adı için belirtilen kullanıcı hesabı için parola belirtin. Data Factory'de güvenle depolamak için bir SecureString olarak bu alanı işaretleyin veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). | Evet |
| connectVia | [Integration runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. Belirtilmezse, varsayılan Azure Integration Runtime kullanır. | Kaynak bağlı Hayır kaynağı için Evet havuz için hizmet bir tümleştirme çalışma zamanı yok |

>[!IMPORTANT]
>Dynamics veri kopyalama, kopyalama yürütmek için varsayılan Azure Integration Runtime kullanılamaz. Kaynağınıza bağlı diğer bir deyişle, hizmet bir belirtilen bir tümleştirme çalışma zamanı açıkça yok [Azure tümleştirme çalışma zamanı oluşturma](create-azure-integration-runtime.md#create-azure-ir) Dynamics örneğinizin yakın bir konum. Aşağıdaki örnekte olduğu gibi Dynamics bağlantılı hizmetteki ilişkilendirin.

>[!NOTE]
>Dynamics CRM/365 Online örneğinizi tanımlamak için isteğe bağlı kuruluş "adı" özelliği kullanmak için kullanılan Dynamics Bağlayıcısı. Çalışmaya devam eder, ancak bunun yerine bulma örneği için daha iyi performans elde etmek için yeni "serviceUri" özelliği belirtmek için önerilir.

**Örnek: Çevrimiçi Dynamics Office365 kimlik doğrulaması kullanma**

```json
{
    "name": "DynamicsLinkedService",
    "properties": {
        "type": "Dynamics",
        "description": "Dynamics online linked service using Office365 authentication",
        "typeProperties": {
            "deploymentType": "Online",
            "serviceUri": "https://adfdynamics.crm.dynamics.com",
            "authenticationType": "Office365",
            "username": "test@contoso.onmicrosoft.com",
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

### <a name="dynamics-365-and-dynamics-crm-on-premises-with-ifd"></a>Dynamics 365 ve Dynamics CRM şirket içi ile IFD

*Dynamics Çevrimiçi karşılaştırma ek özellikler, "ana bilgisayar adı" ve "bağlantı noktası" içindedir.*

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır **Dynamics**. | Evet |
| deploymentType | Dynamics örnek dağıtım türü. Olmalıdır **"OnPremisesWithIfd"** Dynamics ile şirket içi IFD için.| Evet |
| ana bilgisayar adı | Şirket içi Dynamics sunucusunun konak adı. | Evet |
| port | Şirket içi Dynamics sunucusunun bağlantı noktası. | Hayır, varsayılan 443'tür |
| Kuruluş adı | Dynamics örneğinin kuruluş adı. | Evet |
| authenticationType | Dynamics sunucusuna bağlanmak için kimlik doğrulaması türü. Belirtin **"Ifd"** Dynamics ile şirket içi IFD için. | Evet |
| kullanıcı adı | Dynamics bağlanmak için kullanıcı adı belirtin. | Evet |
| password | Kullanıcı adı için belirtilen kullanıcı hesabı için parola belirtin. Bu alan ADF içinde güvenli bir şekilde depolayın veya Azure anahtar Kasası'nda parolayı depolamak için bir SecureString olarak işaretlemek seçin ve veri kopyalama gerçekleştirirken buradan çekme - daha fazla bilgi için kopyalama etkinliği izin [anahtar Kasası'nda kimlik bilgileri Store](store-credentials-in-key-vault.md). | Evet |
| connectVia | [Integration runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. Belirtilmezse, varsayılan Azure Integration Runtime kullanır. | Kaynak, havuz için Evet Hayır |

>[!IMPORTANT]
>Dynamics açıkça veri kopyalamak için [Azure tümleştirme çalışma zamanı oluşturma](create-azure-integration-runtime.md#create-azure-ir) Dynamics örneğinizin yakın konum. Aşağıdaki örnekte olduğu gibi bağlantılı hizmetteki ilişkilendirin.

**Örnek: Dynamics ile şirket içi IFD IFD kimlik doğrulaması kullanma**

```json
{
    "name": "DynamicsLinkedService",
    "properties": {
        "type": "Dynamics",
        "description": "Dynamics on-premises with IFD linked service using IFD authentication",
        "typeProperties": {
            "deploymentType": "OnPremisesWithIFD",
            "hostName": "contosodynamicsserver.contoso.com",
            "port": 443,
            "organizationName": "admsDynamicsTest",
            "authenticationType": "Ifd",
            "username": "test@contoso.onmicrosoft.com",
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

Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölümde, Dynamics veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

İlk ve son Dynamics veri kopyalamak için dataset öğesinin type özelliği ayarlamak **DynamicsEntity**. Aşağıdaki özellikler desteklenir.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Dataset öğesinin type özelliği ayarlanmalıdır **DynamicsEntity**. |Evet |
| EntityName | Alınacak varlığın mantıksal adı. | Evet, havuz için (etkinlik kaynağı "sorgu" belirtilmişse) kaynak için Hayır |

> [!IMPORTANT]
>- Dynamics verileri kopyaladığınızda, "yapı" bölümünde Dynamics kümesinde gereklidir. Sütun adı ve veri türü üzerinde kopyalamak istediğiniz Dynamics verileri tanımlar. Daha fazla bilgi için bkz. [Dataset yapısını](concepts-datasets-linked-services.md#dataset-structure) ve [Dynamics için veri türü eşlemesi](#data-type-mapping-for-dynamics).
>- Dynamics verileri kopyaladığınızda, "yapı" bölümünde Dynamics kümesinde isteğe bağlıdır. Hangi sütunların kopyalayın kaynak veri şema tarafından belirlenir. Kaynağınız bir üst bilgi içermeyen bir CSV dosyası ise giriş veri kümesi "yapı" sütun adı ve veri türü ile belirtin. Bunlar sırayla CSV dosyasındaki tek tek alanları eşleyin.

**Örnek:**

```json
{
    "name": "DynamicsDataset",
    "properties": {
        "type": "DynamicsEntity",
        "structure": [
            {
                "name": "accountid",
                "type": "Guid"
            },
            {
                "name": "name",
                "type": "String"
            },
            {
                "name": "marketingonly",
                "type": "Boolean"
            },
            {
                "name": "modifiedon",
                "type": "Datetime"
            }
        ],
        "typeProperties": {
            "entityName": "account"
        },
        "linkedServiceName": {
            "referenceName": "<Dynamics linked service name>",
            "type": "linkedservicereference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md) makalesi. Bu bölümde, kaynak ve havuz Dynamics türleri tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="dynamics-as-a-source-type"></a>Dynamics kaynak türü

Dynamics veri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **DynamicsSource**. Kopyalama etkinliği aşağıdaki özellikler desteklenir **kaynak** bölümü.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı öğesinin type özelliği ayarlanmalıdır **DynamicsSource**. | Evet |
| sorgu | FetchXML olan Dynamics'te kullanılan bir özel sorgu dili (çevrimiçi ve şirket içi). Aşağıdaki örneğe bakın. Daha fazla bilgi için bkz. [derleme FeachXML sorgularla](https://msdn.microsoft.com/library/gg328332.aspx). | Yok (veri kümesinde "entityName" belirtilmişse) |

>[!NOTE]
>FetchXML sorgusu içinde yapılandırdığınız sütun projeksiyonu, içermese bile PK sütun her zaman kopyalanacak.

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromDynamics",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Dynamics input dataset>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "DynamicsSource",
                "query": "<FetchXML Query>"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="sample-fetchxml-query"></a>FetchXML sorgusu örneği

```xml
<fetch>
  <entity name="account">
    <attribute name="accountid" />
    <attribute name="name" />
    <attribute name="marketingonly" />
    <attribute name="modifiedon" />
    <order attribute="modifiedon" descending="false" />
    <filter type="and">
      <condition attribute ="modifiedon" operator="between">
        <value>2017-03-10 18:40:00z</value>
        <value>2017-03-12 20:40:00z</value>
      </condition>
    </filter>
  </entity>
</fetch>
```

### <a name="dynamics-as-a-sink-type"></a>Bir havuz türü olarak Dynamics

Dynamics veri kopyalamak için kopyalama etkinliğine de Havuz türü ayarlayın. **DynamicsSink**. Kopyalama etkinliği aşağıdaki özellikler desteklenir **havuz** bölümü.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği havuz öğesinin type özelliği ayarlanmalıdır **DynamicsSink**. | Evet |
| WriteBehavior | İşlemi yazma davranışını.<br/>Değer izin verilen **"Upsert"**. | Evet |
| writeBatchSize | Her toplu Dynamics yazılan veriler satır sayısı. | Hayır (varsayılan: 10) |
| ignoreNullValues | (Anahtar alanı dışında) giriş verilerinden null değerler yazma işlemi sırasında yok sayılacak belirtir.<br/>İzin verilen değerler **true** ve **false**.<br>- **True**: upsert/güncelleştirme işlemi yaptığınızda hedef nesnedeki verileri değiştirmeden bırakın. Bir ekleme işlemi yaptığınızda, tanımlanan varsayılan bir değer ekleyin.<br/>- **False**: upsert/güncelleştirme işlemi yaptığınızda verileri hedef nesne NULL olarak güncelleştirin. Bir ekleme işlemi yaptığınızda, bir NULL değer ekleyin. | Hayır (varsayılan değer: false) |

>[!NOTE]
>Havuz varsayılan değerini "**writeBatchSize**"ve kopyalama etkinliği"**[parallelCopies](copy-activity-performance.md#parallel-copy)**" Dynamics havuz için her iki 10 olan. Bu nedenle, 100 kayıtları Dynamics eşzamanlı olarak gönderilir.

Çevrimiçi Dynamics 365 için bir sınır yoktur [kuruluş 2 eş zamanlı batch çağrılık](https://msdn.microsoft.com/en-us/library/jj863631.aspx#Run-time%20limitations). Bu sınır aşılırsa, ilk isteği hiç olmadığı kadar yürütülmeden önce bir "Sunucu meşgul" hatası oluşturulur. "WriteBatchSize" 10'a eşit veya bu tür eş zamanlı çağrılar azaltma kaçının.

En iyi birleşimi "**writeBatchSize**"ve"**parallelCopies**" şemanın varlığınızın sayısı sütun, satır boyutu, iş akışları/plugins/iş akışı etkinlikleri ölçekledikçe sayısı örn bağlıdır. Bu çağrı, vb. için. Varsayılan ayar 10 writeBatchSize * 10 parallelCopies olan öneri göre Dynamics özelleştirebilirsiniz ancak en iyi performansı olmayabilir için işe yarar Dynamics hizmeti. Kopyalama etkinliği ayarlarında birleşimi ayarlayarak performans ayarlayabilirsiniz.

**Örnek:**

```json
"activities":[
    {
        "name": "CopyToDynamics",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Dynamics output dataset>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "DynamicsSink",
                "writeBehavior": "Upsert",
                "writeBatchSize": 10,
                "ignoreNullValues": true
            }
        }
    }
]
```

## <a name="data-type-mapping-for-dynamics"></a>Eşleme Dynamics için veri türü

Dynamics verileri kopyaladığınızda, aşağıdaki eşlemeler Dynamics veri türlerinden veri fabrikası geçici veri türleri için kullanılır. Kopyalama etkinliği havuz için kaynak şema ve veri türü eşlemelerini nasıl bilgi edinmek için [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md).

Karşılık gelen Data Factory veri türü, kaynak Dynamics veri türü eşlemesi aşağıdaki tabloyu kullanarak temel bir dataset yapısını yapılandırın.

| Dynamics veri türü | Veri Fabrikası geçici veri türü | Kaynak olarak desteklenen | Havuz olarak desteklenen |
|:--- |:--- |:--- |:--- |
| AttributeTypeCode.BigInt | Uzun | ✓ | ✓ |
| AttributeTypeCode.Boolean | Boole | ✓ | ✓ |
| AttributeType.Customer | Guid | ✓ | | 
| AttributeType.DateTime | Tarih saat | ✓ | ✓ |
| AttributeType.Decimal | Ondalık | ✓ | ✓ |
| AttributeType.Double | çift | ✓ | ✓ |
| AttributeType.EntityName | Dize | ✓ | ✓ |
| AttributeType.Integer | Int32 | ✓ | ✓ |
| AttributeType.Lookup | Guid | ✓ | ✓ (ile ilişkili tek hedef) |
| AttributeType.ManagedProperty | Boole | ✓ | |
| AttributeType.Memo | Dize | ✓ | ✓ |
| AttributeType.Money | Ondalık | ✓ | ✓ |
| AttributeType.Owner | Guid | ✓ | |
| AttributeType.Picklist | Int32 | ✓ | ✓ |
| AttributeType.Uniqueidentifier | Guid | ✓ | ✓ |
| AttributeType.String | Dize | ✓ | ✓ |
| AttributeType.State | Int32 | ✓ | ✓ |
| AttributeType.Status | Int32 | ✓ | ✓ |


> [!NOTE]
> AttributeType.CalendarRules ve AttributeType.PartyList Dynamics veri türleri desteklenmiyor.

## <a name="next-steps"></a>Sonraki adımlar
Veri fabrikasında kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).
