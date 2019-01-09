---
title: Azure dijital İkizlerini, UDF'ler hata ayıklama | Microsoft Docs
description: Azure dijital İkizlerini, UDF'ler hata ayıklama hakkında kılavuz.
author: stefanmsft
manager: deshner
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 12/27/2018
ms.author: stefanmsft
ms.custom: seodec18
ms.openlocfilehash: ebeed6d2a52937a6e80dfe28574ad854643fa7f2
ms.sourcegitcommit: 818d3e89821d101406c3fe68e0e6efa8907072e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2019
ms.locfileid: "54119241"
---
# <a name="how-to-debug-user-defined-functions-in-azure-digital-twins"></a>Azure dijital İkizlerini kullanıcı tanımlı işlevlerde hata ayıklama

Bu makalede, tanılamak ve kullanıcı tanımlı işlevleri hata ayıklama nasıl özetlenir. Ardından, bunları hata ayıklama sırasında bulunamadı en yaygın senaryolardan bazıları tanımlar.

>[!TIP]
> Okuma [izleme ve günlüğe kaydetme yapılandırma](./how-to-configure-monitoring.md) etkinlik günlükleri, tanılama günlükleri ve Azure İzleyicisi'ni kullanarak Azure dijital İkizlerini araçlarındaki hata ayıklamayı kurma hakkında daha fazla bilgi edinmek için.

## <a name="debug-issues"></a>Sorunlarında hata ayıklama

Azure dijital İkizlerini örneğinizin içinde gerçekleşen tüm sorunlarının nasıl tanılandığını bilerek sorunu, sorunu ve çözüm nedenini etkili bir şekilde tanımlamak için kolaylık sağlar.

### <a name="enable-log-analytics-for-your-instance"></a>Örneğiniz için log analytics etkinleştir

Günlükleri ve ölçümleri Azure dijital İkizlerini örneğinizin Azure İzleyicisi'nde görüntülenir. Bu belge, oluşturduğunuz varsayar bir [Azure Log Analytics](../azure-monitor/log-query/log-query-overview.md) çalışma alanını kullanarak [Azure portalı](../azure-monitor/learn/quick-create-workspace.md)temellidir [Azure CLI](../azure-monitor/learn/quick-create-workspace-cli.md), aracılığıyla veya [ PowerShell](../azure-monitor/learn/quick-create-workspace-posh.md).

> [!NOTE]
> 5 dakikalık bir gecikmeyle olayları ilk kez Azure Log Analytics'e gönderirken karşılaşabilirsiniz.

İzleme ve günlüğe kaydetme dijital İkizlerini Azure kaynakları için yapılandırmak için okuma [izleme ve günlüğe kaydetme yapılandırma](./how-to-configure-monitoring.md).

Makaleyi okuyun [toplamak ve Azure kaynaklarınızdan günlük verilerini kullanma](../azure-monitor/platform/diagnostic-logs-overview.md) Azure portalı, Azure CLI veya PowerShell aracılığıyla Azure dijital çiftleri olarak tanılama günlüğü ayarlarını yapılandırmak için.

>[!IMPORTANT]
> Tüm günlük kategorileri, Ölçümler ve Azure Log Analytics çalışma alanınızı seçtiğinizden emin olun.

### <a name="trace-sensor-telemetry"></a>Telemetri algılayıcı izleme

Telemetri algılayıcı izleme, tanılama ayarları Azure dijital İkizlerini Örneğiniz için etkinleştirildiğini doğrulayın. Ardından, istenen tüm günlük kategorileri seçildiğinden emin olun. Son olarak, Azure Log Analytics için istenen günlükleri gönderildiğini doğrulayın.

Algılayıcı telemetri iletileriyle ilgili günlüklerinin için eşleştirilecek gönderilen olay verileri üzerinde bir bağıntı kimliği belirtebilirsiniz. Bunu yapmak için ayarlanmış `x-ms-client-request-id` özelliğini bir GUID.

Telemetri gönderdikten sonra sorgulamak için günlükleri kullanarak Azure Log Analytics'i açın bağıntı kimliği:

```Kusto
AzureDiagnostics
| where CorrelationId == 'YOUR_CORRELATION_IDENTIFIER'
```

| Sorgu değeri | Şununla değiştir |
| --- | --- |
| YOUR_CORRELATION_IDENTIFIER | Olay verileri üzerinde belirtilen bağıntı kimliği |

Kullanıcı tanımlı işlev için günlüğe yazmayı etkinleştirirseniz, bu günlükleri kategori Azure Log Analytics örneğinizle görünür `UserDefinedFunction`. Bunları almak için Azure Log Analytics'te şu sorgu koşulunu girin:

```Kusto
AzureDiagnostics
| where Category == 'UserDefinedFunction'
```

Güçlü sorgu işlemleri hakkında daha fazla bilgi için okuma [sorguları ile çalışmaya başlama](../azure-monitor/log-query/get-started-queries.md).

## <a name="identify-common-issues"></a>Sık karşılaşılan sorunları tanımlayın

Tanılama hem ortak sorunları belirlemenize, çözüme giderirken önemlidir. Kullanıcı tanımlı işlevleri geliştirirken yaygın olarak karşılaşılan birkaç sorun aşağıdaki alt kısımlarda özetlenmiştir.

[!INCLUDE [Digital Twins Management API](../../includes/digital-twins-management-api.md)]

### <a name="check-if-a-role-assignment-was-created"></a>Bir rol ataması oluşturulduğundan emin olun

Yönetim API'si içinde oluşturulan bir rol ataması olmadan kullanıcı tanımlı işlev bildirimleri, meta verileri alınırken gönderme gibi eylemleri gerçekleştirmek için erişime sahip değil ve ayarı topolojisi içindeki değerleri hesaplanan.

Bir rol ataması, yönetim API'si aracılığıyla kullanıcı tanımlı işleviniz için mevcut olup olmadığını denetleyin:

```plaintext
GET YOUR_MANAGEMENT_API_URL/roleassignments?path=/&traverse=Down&objectId=YOUR_USER_DEFINED_FUNCTION_ID
```

| Parametre değeri | Şununla değiştir |
| --- | --- |
| YOUR_USER_DEFINED_FUNCTION_ID | Rol atamalarını almak için kullanıcı tanımlı işlev kimliği|

Bilgi [kullanıcı tanımlı işleviniz için bir rol ataması oluşturma](./how-to-user-defined-functions.md), hiçbir rol ataması yok.

### <a name="check-if-the-matcher-works-for-a-sensors-telemetry"></a>Eşleştiricisi için telemetri algılayıcı'nın çalışıp çalışmadığını denetleyin

Azure dijital İkizlerini örneklerinizin yönetim API'sine aşağıdaki çağrısı ile verilen Eşleştiricisi için verilen algılayıcı geçerliyse belirlemek kullanabilirsiniz.

```plaintext
GET YOUR_MANAGEMENT_API_URL/matchers/YOUR_MATCHER_IDENTIFIER/evaluate/YOUR_SENSOR_IDENTIFIER?enableLogging=true
```

| Parametre | Şununla değiştir |
| --- | --- |
| *YOUR_MATCHER_IDENTIFIER* | Değerlendirmek istediğiniz Eşleştiricisi kimliği |
| *YOUR_SENSOR_IDENTIFIER* | Değerlendirmek istediğiniz algılayıcı kimliği |

Yanıt:

```JavaScript
{
    "success": true,
    "logs": [
        "$.dataType: \"Motion\" Equals \"Motion\" => True"
    ]
}
```

### <a name="check-what-a-sensor-triggers"></a>Neyin algılayıcı tetikleyeceğini denetleyin

Azure dijital İkizlerini yönetim API'leri karşı aşağıdaki çağrısı ile verilen algılayıcının gelen telemetriyi tarafından tetiklenen olan kullanıcı tanımlı işlevleri tanımlayıcıların belirleyebilirsiniz:

```plaintext
GET YOUR_MANAGEMENT_API_URL/sensors/YOUR_SENSOR_IDENTIFIER/matchers?includes=UserDefinedFunctions
```

| Parametre | Şununla değiştir |
| --- | --- |
| *YOUR_SENSOR_IDENTIFIER* | Telemetri göndermek için algılayıcı kimliği |

Yanıt:

```JavaScript
[
    {
        "id": "48a64768-797e-4832-86dd-de625f5f3fd9",
        "name": "MotionMatcher",
        "spaceId": "2117b3e1-b6ce-42c1-9b97-0158bef59eb7",
        "conditions": [
            {
                "id": "024a067a-414f-415b-8424-7df61392541e",
                "target": "Sensor",
                "path": "$.dataType",
                "value": "\"Motion\"",
                "comparison": "Equals"
            }
        ],
        "userDefinedFunctions": [
            {
                "id": "373d03c5-d567-4e24-a7dc-f993460120fc",
                "spaceId": "2117b3e1-b6ce-42c1-9b97-0158bef59eb7",
                "name": "Motion User-Defined Function",
                "disabled": false
            }
        ]
    }
]
```

### <a name="issue-with-receiving-notifications"></a>Bildirim alma ile sorun

Tetiklenmiş kullanıcı tanımlı işlevini bildirimleri almadığınızı, topoloji nesne türü parametreniz kullanılan tanımlayıcı türünü eşleştiğini onaylayın.

**Yanlış** örneği:

```JavaScript
var customNotification = {
    Message: 'Custom notification that will not work'
};

sendNotification(telemetry.SensorId, "Space", JSON.stringify(customNotification));
```

Bu senaryo ortaya topolojisi nesne türü belirtilen sırada bir algılayıcı için kullanılan tanımlayıcı başvurduğundan `Space`.

**Doğru** örneği:

```JavaScript
var customNotification = {
    Message: 'Custom notification that will work'
};

sendNotification(telemetry.SensorId, "Sensor", JSON.stringify(customNotification));
```

Bu sorunu konusunda en kolay yolu kullanmaktır `Notify` meta veri nesnesi üzerinde yöntemi.

Örnek:

```JavaScript
function process(telemetry, executionContext) {
    var sensorMetadata = getSensorMetadata(telemetry.SensorId);

    var customNotification = {
        Message: 'Custom notification'
    };

    // Short-hand for above methods where object type is known from metadata.
    sensorMetadata.Notify(JSON.stringify(customNotification));
}
```

## <a name="common-diagnostic-exceptions"></a>Ortak tanılama özel durumları

Tanılama ayarlarını etkinleştirme, bu sık karşılaşılan özel durumlar karşılaşabilirsiniz:

1. **Azaltma**: kullanıcı tanımlı işlevinizi sınırları ana hatlarıyla belirtilen yürütme hızını aşıyorsa [hizmet sınırları](./concepts-service-limits.md) makalesi, bu kısıtlanır. Azaltma sınırları süresi dolana kadar başka işlem başarıyla yürütüldü.

1. **Veri bulunamadı**:, kullanıcı tanımlı işlev, işlem başarısız yok meta verilerine erişim dener.

1. **Yetkili**:, kullanıcı tanımlı işlev yoksa, bir rol ataması ayarlayın veya topolojisinden belirli meta verilerine erişmek için yeterli izinlere sahip değil, işlem başarısız.

## <a name="next-steps"></a>Sonraki adımlar

- Nasıl etkinleştireceğinizi öğrenin [izleme ve günlükleri](../azure-monitor/platform/activity-logs-overview.md) Azure dijital İkizlerini içinde.
