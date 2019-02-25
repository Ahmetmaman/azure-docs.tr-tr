---
title: "Öğretici: Tanılama ve etkinlik günlüğü verileri Azure veri Gezgini'nde bir kod satırı olmadan alma"
description: Bu öğreticide, verileri Azure Veri Gezgini veri olmadan tek satırlık bir kod ve sorgu alma konusunda bilgi edinin.
services: data-explorer
author: orspod
ms.author: v-orspod
ms.reviewer: jasonh
ms.service: data-explorer
ms.topic: tutorial
ms.date: 2/5/2019
ms.openlocfilehash: a678722666146fdf22e88680ab414b09d2a7ffaa
ms.sourcegitcommit: e88188bc015525d5bead239ed562067d3fae9822
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/24/2019
ms.locfileid: "56749945"
---
# <a name="tutorial-ingest-data-in-azure-data-explorer-without-one-line-of-code"></a>Öğretici: Azure veri Gezgini'nde verileri tek satırlık bir kod olmadan alma

Bu öğreticide kod yazmaya gerek kalmadan, etkinlik günlükleri için bir Azure Veri Gezgini kümesi ve tanılama verilerini alma öğretir. Bu basit alma yöntemi ile veri analizi için Azure Veri Gezgini sorgulama hızlı bir şekilde başlayabilirsiniz.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir Azure Veri Gezgini veritabanında tablolar ve alım eşlemesi oluşturun.
> * Alınan verileri bir güncelleştirme İlkesi kullanarak biçimlendirin.
> * Oluşturma bir [olay hub'ı](/azure/event-hubs/event-hubs-about) ve Azure veri Gezgini'ne bağlanın.
> * Veri bir olay hub'ından Stream [Azure İzleyici tanılama günlükleri](/azure/azure-monitor/platform/diagnostic-logs-overview) ve [Azure İzleyici etkinlik günlüklerini](/azure/azure-monitor/platform/activity-logs-overview).
> * Azure Veri Gezgini'ni kullanarak alınan verileri sorgulayın.

> [!NOTE]
> Tüm kaynaklar aynı Azure konumuna veya bölgede oluşturun. Bu, Azure İzleyici tanılama günlükleri için bir zorunluluktur.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.
* [Bir Azure Veri Gezgini kümesi ile veritabanı](create-cluster-database-portal.md). Bu öğreticide, veritabanı adı. *AzureMonitoring*.

## <a name="azure-monitor-data-provider-diagnostic-and-activity-logs"></a>Azure İzleyici, veri sağlayıcısı: tanılama ve etkinlik günlükleri

Görüntüleyebilir ve Azure İzleyici tanılama ve etkinlik günlükleri tarafından sağlanan verileri anlayın. Bu veri şemalarını temel alan bir alma işlem hattı oluşturacağız.

### <a name="diagnostic-logs-example"></a>Tanılama günlüklerini örnek

Azure tanılama günlükleri, hizmetin çalışması hakkında veriler sağlayan bir Azure hizmeti tarafından yayılan ölçümleridir. Veriler ile 1 dakikalık bir zaman dilimi toplanır. Tanılama Günlüğü'ndeki her olay, bir kayıt içerir. Sorgu süresini bir Azure Veri Gezgini ölçüm olay şeması örneği aşağıdadır:

```json
{
    "count": 14,
    "total": 0,
    "minimum": 0,
    "maximum": 0,
    "average": 0,
    "resourceId": "/SUBSCRIPTIONS/F3101802-8C4F-4E6E-819C-A3B5794D33DD/RESOURCEGROUPS/KEDAMARI/PROVIDERS/MICROSOFT.KUSTO/CLUSTERS/KEREN",
    "time": "2018-12-20T17:00:00.0000000Z",
    "metricName": "QueryDuration",
    "timeGrain": "PT1M"
}
```

### <a name="activity-logs-example"></a>Etkinlik günlükleri örneği

Azure etkinlik günlüklerini kayıtların bir koleksiyonunu içeren abonelik düzeyinde günlüklerdir. Günlükleri, aboneliğinizdeki kaynaklar üzerinde gerçekleştirilen işlemler hakkında bilgi sağlar. Tanılama günlükleri farklı olarak, her bir etkinlik günlüğü olayında bir kayıt dizisi vardır. Öğreticide daha sonra bu kayıtları dizisi bölmek gerekir. Erişim denetimi için bir etkinlik günlüğü olayının bir örnek aşağıda verilmiştir:

```json
{
    "records": [
    {
        "time": "2018-12-26T16:23:06.1090193Z",
        "resourceId": "/SUBSCRIPTIONS/F80EB51C-C534-4F0B-80AB-AEBC290C1C19/RESOURCEGROUPS/CLEANUPSERVICE/PROVIDERS/MICROSOFT.WEB/SITES/CLNB5F73B70-DCA2-47C2-BB24-77B1A2CAAB4D/PROVIDERS/MICROSOFT.AUTHORIZATION",
        "operationName": "MICROSOFT.AUTHORIZATION/CHECKACCESS/ACTION",
        "category": "Action",
        "resultType": "Start",
        "resultSignature": "Started.",
        "durationMs": 0,
        "callerIpAddress": "13.66.225.188",
        "correlationId": "0de9f4bc-4adc-4209-a774-1b4f4ae573ed",
        "identity": {
            "authorization": {
                ...
            },
            "claims": {
                ...
            }
        },
        "level": "Information",
        "location": "global",
        "properties": {
            ...
        }
    },
    {
        "time": "2018-12-26T16:23:06.3040244Z",
        "resourceId": "/SUBSCRIPTIONS/F80EB51C-C534-4F0B-80AB-AEBC290C1C19/RESOURCEGROUPS/CLEANUPSERVICE/PROVIDERS/MICROSOFT.WEB/SITES/CLNB5F73B70-DCA2-47C2-BB24-77B1A2CAAB4D/PROVIDERS/MICROSOFT.AUTHORIZATION",
        "operationName": "MICROSOFT.AUTHORIZATION/CHECKACCESS/ACTION",
        "category": "Action",
        "resultType": "Success",
        "resultSignature": "Succeeded.OK",
        "durationMs": 194,
        "callerIpAddress": "13.66.225.188",
        "correlationId": "0de9f4bc-4adc-4209-a774-1b4f4ae573ed",
        "identity": {
            "authorization": {
                ...
            },
            "claims": {
                ...
            }
        },
        "level": "Information",
        "location": "global",
        "properties": {
            "statusCode": "OK",
            "serviceRequestId": "87acdebc-945f-4c0c-b931-03050e085626"
        }
    }]
}
```

## <a name="set-up-an-ingestion-pipeline-in-azure-data-explorer"></a>Azure veri Gezgini'nde bir alma işlem hattı ayarlama

Bir Azure Veri Gezgini işlem hattı ayarlama içeren birkaç adım gibi [tablo oluşturma ve veri alımı](/azure/data-explorer/ingest-sample-data#ingest-data). Düzenleme, eşleyin ve verileri güncelleştirin.

### <a name="connect-to-the-azure-data-explorer-web-ui"></a>Azure Veri Gezgini Web kullanıcı Arabirimine bağlanma

Azure veri Gezgini'nde *AzureMonitoring* veritabanı **sorgu** Azure Veri Gezgini Web kullanıcı arabirimini açın.

![Sorgu sayfası](media/ingest-data-no-code/query-database.png)

### <a name="create-the-target-tables"></a>Hedef Tablo oluşturma

Azure Veri Gezgini veritabanında hedef tablolar oluşturmak için Azure Veri Gezgini Web kullanıcı arabirimini kullanın.

#### <a name="the-diagnostic-logs-table"></a>Tanılama günlükleri tablo

1. İçinde *AzureMonitoring* adlı bir tablo oluşturma, veritabanı *DiagnosticLogsRecords* tanılama günlüğü kayıtlarını depolamak için. Aşağıdaki `.create table` denetim komutu:

    ```kusto
    .create table DiagnosticLogsRecords (Timestamp:datetime, ResourceId:string, MetricName:string, Count:int, Total:double, Minimum:double, Maximum:double, Average:double, TimeGrain:string)
    ```

1. Seçin **çalıştırma** tablo oluşturun.

    ![Sorgu çalıştırma](media/ingest-data-no-code/run-query.png)

#### <a name="the-activity-logs-tables"></a>Tabloları etkinlik günlükleri

Etkinlik günlükleri yapısını tablo olmadığından, verileri işlemek ve her olay için bir veya daha fazla kayıt genişletin gerekir. Ham verileri, adlı bir ara tablo alınan *ActivityLogsRawRecords*. O anda veri yönetilebilir ve genişletilmiş. Genişletilmiş veriler daha sonra içine alınan *ActivityLogsRecords* güncelleştirme İlkesi kullanarak tablo. Başka bir deyişle, etkinlik günlüklerini almak için iki ayrı tablolar oluşturmak gerekir.

1. Adlı bir tablo oluşturun *ActivityLogsRecords* içinde *AzureMonitoring* etkinlik günlük kayıtları almak için veritabanı. Tablo oluşturmak için aşağıdaki Azure Veri Gezgini sorguyu çalıştırın:

    ```kusto
    .create table ActivityLogsRecords (Timestamp:datetime, ResourceId:string, OperationName:string, Category:string, ResultType:string, ResultSignature:string, DurationMs:int, IdentityAuthorization:dynamic, IdentityClaims:dynamic, Location:string, Level:string)
    ```

1. Adlı ara veri tablosu oluşturma *ActivityLogsRawRecords* içinde *AzureMonitoring* veritabanı veri işleme için:

    ```kusto
    .create table ActivityLogsRawRecords (Records:dynamic)
    ```

<!--
     ```kusto
     .alter-merge table ActivityLogsRawRecords policy retention softdelete = 0d
    <[Retention](/azure/kusto/management/retention-policy) for an intermediate data table is set at zero retention policy.
-->

### <a name="create-table-mappings"></a>Tablo eşlemeleri oluşturma

 Veri biçimi olduğundan `json`, veri eşlemesi gereklidir. `json` Eşleme her json yolu bir tablo sütunu adıyla eşleşir.

#### <a name="table-mapping-for-diagnostic-logs"></a>Tanılama günlükleri için Tablo eşleme

Tanılama günlükleri veri tablosuna eşlemek için aşağıdaki sorguyu kullanın:

```kusto
.create table DiagnosticLogsRecords ingestion json mapping 'DiagnosticLogsRecordsMapping' '[{"column":"Timestamp","path":"$.time"},{"column":"ResourceId","path":"$.resourceId"},{"column":"MetricName","path":"$.metricName"},{"column":"Count","path":"$.count"},{"column":"Total","path":"$.total"},{"column":"Minimum","path":"$.minimum"},{"column":"Maximum","path":"$.maximum"},{"column":"Average","path":"$.average"},{"column":"TimeGrain","path":"$.timeGrain"}]'
```

#### <a name="table-mapping-for-activity-logs"></a>Etkinlik günlükleri için Tablo eşleme

Etkinlik günlükleri veri tablosuna eşlemek için aşağıdaki sorguyu kullanın:

```kusto
.create table ActivityLogsRawRecords ingestion json mapping 'ActivityLogsRawRecordsMapping' '[{"column":"Records","path":"$.records"}]'
```

### <a name="create-the-update-policy-for-activity-logs-data"></a>Etkinlik günlükleri veriler için güncelleştirme ilkesi oluştur

1. Oluşturma bir [işlevi](/azure/kusto/management/functions) , genişleyen sahip kayıtların koleksiyonudur ve böylece koleksiyondaki her değer ayrı bir satır alır. Kullanım [ `mvexpand` ](/azure/kusto/query/mvexpandoperator) işleci:

    ```kusto
    .create function ActivityLogRecordsExpand() {
        ActivityLogsRawRecords
        | mvexpand events = Records
        | project
            Timestamp = todatetime(events["time"]),
            ResourceId = tostring(events["resourceId"]),
            OperationName = tostring(events["operationName"]),
            Category = tostring(events["category"]),
            ResultType = tostring(events["resultType"]),
            ResultSignature = tostring(events["resultSignature"]),
            DurationMs = toint(events["durationMs"]),
            IdentityAuthorization = events.identity.authorization,
            IdentityClaims = events.identity.claims,
            Location = tostring(events["location"]),
            Level = tostring(events["level"])
    }
    ```

2. Ekleme [güncelleştirme ilkesi](/azure/kusto/concepts/updatepolicy) hedef tablosu için. Bu ilke otomatik olarak yeni alınan tüm veriler üzerinde sorgu çalıştıracaksınız *ActivityLogsRawRecords* ara veri tablosu ve sonuçları içine alma *ActivityLogsRecords* tablosu:

    ```kusto
    .alter table ActivityLogsRecords policy update @'[{"Source": "ActivityLogsRawRecords", "Query": "ActivityLogRecordsExpand()", "IsEnabled": "True"}]'
    ```

## <a name="create-an-azure-event-hubs-namespace"></a>Bir Azure Event Hubs ad alanı oluşturma

Azure tanılama günlükleri bir depolama hesabına veya olay hub'ına verme ölçümlerini etkinleştirin. Bu öğreticide, bir olay hub'ı aracılığıyla ölçümleri biz rota. Aşağıdaki adımlarda bir Event Hubs ad alanı ve tanılama günlükleri için bir olay hub'ı oluşturacaksınız. Azure İzleyici, olay hub'ı oluşturacaktır *insights-operational-logs* etkinlik günlükleri için.

1. Azure portalında bir Azure Resource Manager şablonu kullanarak bir olay hub'ı oluşturun. Bu makaledeki adımları izlemeden için sağ **azure'a Dağıt** düğmesini ve ardından **yeni pencerede aç**. **Azure'a Dağıt** düğmesi Azure portalına yönlendirilirsiniz.

    [![Azure düğmeye dağıtma](media/ingest-data-no-code/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-event-hubs-create-event-hub-and-consumer-group%2Fazuredeploy.json)

1. Bir Event Hubs ad alanı ve tanılama günlükleri için bir olay hub'ı oluşturun.

    ![Olay hub'ı oluşturma](media/ingest-data-no-code/event-hub.png)

1. Formu aşağıdaki bilgilerle doldurun. Aşağıdaki tabloda listelenmeyen tüm ayarlar için varsayılan değerleri kullanın.

    **Ayar** | **Önerilen değer** | **Açıklama**
    |---|---|---|
    | **Abonelik** | *Aboneliğiniz* | Olay hub'ınız için kullanmak istediğiniz Azure aboneliğini seçin.|
    | **Kaynak grubu** | *test-resource-group* | Yeni bir kaynak grubu oluşturun. |
    | **Konum** | İhtiyaçlarınıza en uygun bölgeyi seçin. | Event Hubs ad alanı, diğer kaynaklar ile aynı konumda oluşturun.
    | **Namespace adı** | *AzureMonitoringData* | Ad alanınızı tanımlayan benzersiz bir ad seçin.
    | **Olay hub'ı adı** | *DiagnosticLogsData* | Olay hub'ı benzersiz bir kapsayıcı kapsamı sunan ad alanında bulunur. |
    | **Tüketici grubu adı** | *adxpipeline* | Bir tüketici grubu adı oluşturun. Tüketici grupları birden fazla tüketici uygulamasının ayrı olay akışı görünümüne sahip olmasını sağlar. |
    | | |

## <a name="connect-azure-monitor-logs-to-your-event-hub"></a>Azure İzleyici günlükleri Olay hub'ınıza bağlanın

Şimdi, tanılama günlükleri ve, etkinlik günlükleri Olay hub'ına bağlamak gerekir.

### <a name="connect-diagnostic-logs-to-your-event-hub"></a>Tanılama günlükleri Olay hub'ınıza bağlanın

Ölçümleri dışarı aktarılacağı bir kaynak seçin. Çeşitli kaynak türleri, Event Hubs ad alanı, Azure Key Vault, Azure IOT Hub ve Azure Veri Gezgini kümeleri dahil olmak üzere tanılama günlükleri dışarı aktarılması. Bu öğreticide, bizim kaynağı olarak bir Azure Veri Gezgini kümesi kullanacağız.

1. Kusto kümeniz, Azure portalında seçin.
1. Seçin **tanılama ayarları**ve ardından **tanılamayı Aç** bağlantı. 

    ![Tanılama ayarları](media/ingest-data-no-code/diagnostic-settings.png)

1. **Tanılama ayarları** bölmesi açılır. Aşağıdaki adımları uygulayın:
    1. Tanılama günlük verilerini adı verin *ADXExportedData*.
    1. Altında **ÖLÇÜM**seçin **AllMetrics** onay kutusunu (isteğe bağlı).
    1. Seçin **Stream olay hub'ına** onay kutusu.
    1. Seçin **yapılandırma**.

    ![Tanılama ayarları bölmesi](media/ingest-data-no-code/diagnostic-settings-window.png)

1. İçinde **Select olay hub'ı** bölmesinde, oluşturduğunuz olay hub'ına tanılama günlüklerinin verileri dışarı aktarma yapılandırın:
    1. İçinde **seçin, olay hub'ı ad alanı** listesinde, seçin *AzureMonitoringData*.
    1. İçinde **Select olay hub'ı adı** listesinde, seçin *diagnosticlogsdata*.
    1. İçinde **Select olay hub'ı ilke adı** listesinde, seçin **RootManagerSharedAccessKey**.
    1. **Tamam**’ı seçin.

1. **Kaydet**’i seçin. Olay hub'ı ad alanı adı ve ilke adı penceresinde görünür.

    ![Tanılama ayarları kaydedin](media/ingest-data-no-code/save-diagnostic-settings.png)

### <a name="connect-activity-logs-to-your-event-hub"></a>Etkinlik günlükleri Olay hub'ınıza bağlanın

1. Azure portalının sol taraftaki menüde seçin **etkinlik günlüğü**.
1. **Etkinlik günlüğü** penceresi açılır. Seçin **dışarı aktarma, olay Hub'ına**.

    ![Etkinlik Günlüğü penceresi](media/ingest-data-no-code/activity-log.png)

1. **Etkinlik günlüğünü dışarı aktar** penceresi açılır:
 
    ![Dışarı aktarma etkinlik günlüğü penceresi](media/ingest-data-no-code/export-activity-log.png)

1. İçinde **Etkinlik günlüğünü dışarı aktar** penceresinde aşağıdaki adımları uygulayın:
      1. Aboneliğinizi seçin.
      1. İçinde **bölgeleri** listesinde **Tümünü Seç**.
      1. Seçin **dışarı aktarma, olay hub'ına** onay kutusu.
      1. Seçin **bir service bus ad alanı seçin** açmak için **Select olay hub'ı** bölmesi.
      1. İçinde **Select olay hub'ı** bölmesinde, aboneliğinizi seçin.
      1. İçinde **seçin, olay hub'ı ad alanı** listesinde, seçin *AzureMonitoringData*.
      1. İçinde **Select olay hub'ı ilke adı** listesinde, varsayılan olay hub'ı ilke adı seçin.
      1. **Tamam**’ı seçin.
      1. Pencerenin sol üst köşesinde bulunan seçin **Kaydet**.
   Bir olay hub'ı adı ile *insights-operational-logs* oluşturulur.

### <a name="see-data-flowing-to-your-event-hubs"></a>Verilerin, event hubs'a aktığını

1. Bağlantı tanımlanana kadar birkaç dakika bekleyin ve olay hub'ına Etkinlik günlüğünü dışarı aktarma işlemi tamamlandı. Oluşturduğunuz olay hub'ları görmek için Event Hubs ad alanınıza gidin.

    ![Oluşturulan olay hub'ları](media/ingest-data-no-code/event-hubs-created.png)

1. Olay hub'ınıza akan verileri görürsünüz:

    ![Olay hub'ın verileri](media/ingest-data-no-code/event-hubs-data.png)

## <a name="connect-an-event-hub-to-azure-data-explorer"></a>Bir olay hub'ı Azure veri Gezgini'ne bağlanma

Şimdi, tanılama günlükleri ve etkinlik günlükleri için veri bağlantıları oluşturmanız gerekir.

### <a name="create-the-data-connection-for-diagnostic-logs"></a>Tanılama günlüklerine yönelik veri bağlantısı oluşturma

1. Azure Veri Gezgini kümenizde adlı *kustodocs*seçin **veritabanları** soldaki menüde.
1. İçinde **veritabanları** penceresinde, seçin, *AzureMonitoring* veritabanı.
1. Sol menüde **veri alımı**.
1. İçinde **veri alımı** penceresinde tıklayın **+ veri bağlantısı ekleme**.
1. İçinde **veri bağlantısı** penceresinde aşağıdaki bilgileri girin:

    ![Olay hub'ı veri bağlantısı](media/ingest-data-no-code/event-hub-data-connection.png)

    Veri kaynağı:

    **Ayar** | **Önerilen değer** | **Alan açıklaması**
    |---|---|---|
    | **Veri bağlantısı adı** | *DiagnosticsLogsConnection* | Azure Veri Gezgini'nde oluşturmak istediğiniz bağlantının adı.|
    | **Olay hub'ı ad alanı** | *AzureMonitoringData* | Önceden seçtiğiniz ve ad alanınızı tanımlayan ad. |
    | **Olay hub'ı** | *diagnosticlogsdata* | Oluşturduğunuz olay hub'ı. |
    | **Tüketici grubu** | *adxpipeline* | Oluşturduğunuz olay hub'ında tanımlanan tüketici grubu. |
    | | |

    Hedef Tablo:

    İki yönlendirme seçeneği vardır: *statik* ve *dinamik*. Bu öğreticide tablo adı, veri biçimi ve eşleme belirttiğiniz (varsayılan), yönlendirme statik kullanacaksınız. **Verilerimde yönlendirme bilgileri var** seçeneğini işaretlemeyin.

     **Ayar** | **Önerilen değer** | **Alan açıklaması**
    |---|---|---|
    | **Tablo** | *DiagnosticLogsRecords* | Oluşturduğunuz tabloyu *AzureMonitoring* veritabanı. |
    | **Veri biçimi** | *JSON* | Tabloda kullanılan biçim. |
    | **Sütun eşleme** | *DiagnosticLogsRecordsMapping* | Oluşturduğunuz eşleme *AzureMonitoring* sütun adları ve veri türleri için gelen JSON verilerini eşleştiren veritabanı *DiagnosticLogsRecords* tablo.|
    | | |

1. **Oluştur**’u seçin.  

### <a name="create-the-data-connection-for-activity-logs"></a>Etkinlik günlükleri için veri bağlantısı oluşturma

Adımları yineleyin [tanılama günlüklerine yönelik veri bağlantısı oluşturma](#diagnostic-logs-data-connection) , etkinlik günlükleri için veri bağlantısı oluşturmak için bölüm.

1. Aşağıdaki ayarları kullanın **veri bağlantısı** penceresi:

    Veri kaynağı:

    **Ayar** | **Önerilen değer** | **Alan açıklaması**
    |---|---|---|
    | **Veri bağlantısı adı** | *ActivityLogsConnection* | Azure Veri Gezgini'nde oluşturmak istediğiniz bağlantının adı.|
    | **Olay hub'ı ad alanı** | *AzureMonitoringData* | Önceden seçtiğiniz ve ad alanınızı tanımlayan ad. |
    | **Olay hub'ı** | *insights-operational-logs* | Oluşturduğunuz olay hub'ı. |
    | **Tüketici grubu** | *$Default* | Varsayılan bir tüketici grubu. Gerekirse, farklı bir tüketici grubu oluşturabilirsiniz. |
    | | |

    Hedef Tablo:

    İki yönlendirme seçeneği vardır: *statik* ve *dinamik*. Bu öğreticide tablo adı, veri biçimi ve eşleme belirttiğiniz (varsayılan), yönlendirme statik kullanacaksınız. **Verilerimde yönlendirme bilgileri var** seçeneğini işaretlemeyin.

     **Ayar** | **Önerilen değer** | **Alan açıklaması**
    |---|---|---|
    | **Tablo** | *ActivityLogsRawRecords* | Oluşturduğunuz tabloyu *AzureMonitoring* veritabanı. |
    | **Veri biçimi** | *JSON* | Tabloda kullanılan biçim. |
    | **Sütun eşleme** | *ActivityLogsRawRecordsMapping* | Oluşturduğunuz eşleme *AzureMonitoring* sütun adları ve veri türleri için gelen JSON verilerini eşleştiren veritabanı *ActivityLogsRawRecords* tablo.|
    | | |

1. **Oluştur**’u seçin.  

## <a name="query-the-new-tables"></a>Yeni Tablo sorgulama

Artık bir işlem hattı ile veri akışının sahipsiniz. Küme aracılığıyla alımı varsayılan olarak 5 dakika sürer, bu nedenle veri sorgulamak başlamadan önce birkaç dakika için akışı izin.

### <a name="an-example-of-querying-the-diagnostic-logs-table"></a>Tanılama günlükleri tablo sorgulanmasına bir örnek

Aşağıdaki sorguyu sorgu süresi tanılama günlüğü kayıtlarını Azure veri Gezgini'nde verileri analiz eder:

```kusto
DiagnosticLogsRecords
| where Timestamp > ago(15m) and MetricName == 'QueryDuration'
| summarize avg(Average)
```

Sorgu sonuçları:

|   |   |
| --- | --- |
|   |  avg_Average |
|   | 00:06.156 |
| | |

### <a name="an-example-of-querying-the-activity-logs-table"></a>Etkinlik günlükleri tablo sorgulanmasına bir örnek

Aşağıdaki sorgu, etkinlik günlüğü kayıtlarını Azure veri Gezgini'nde verileri analiz eder:

```kusto
ActivityLogsRecords
| where OperationName == 'MICROSOFT.EVENTHUB/NAMESPACES/AUTHORIZATIONRULES/LISTKEYS/ACTION'
| where ResultType == 'Success'
| summarize avg(DurationMs)
```

Sorgu sonuçları:
|   |   |
| --- | --- |
|   |  AVG(DurationMs) |
|   | 768.333 |
| | |

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makalede kullanarak Azure veri Gezgini'nde ayıklanan verilerin çok daha fazla sorguları yazma öğrenin:

> [!div class="nextstepaction"]
> [Azure Veri Gezgini için sorgu yazma](write-queries.md)
