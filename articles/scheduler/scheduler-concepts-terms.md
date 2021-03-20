---
title: Kavramlar, terimler ve varlıklar
description: İşler ve iş koleksiyonları dahil Azure Scheduler kavramları, terminolojisi ve varlık hiyerarşisi hakkında bilgi edinin
services: scheduler
ms.service: scheduler
ms.suite: infrastructure-services
author: derek1ee
ms.author: deli
ms.reviewer: klam, estfan
ms.topic: conceptual
ms.date: 08/18/2016
ms.openlocfilehash: 899c64e818896cde18e955d6abd82594734c4b57
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "92368171"
---
# <a name="concepts-terminology-and-entities-in-azure-scheduler"></a>Azure Scheduler kavramları, terminolojisi ve varlıkları

> [!IMPORTANT]
> [Azure Logic Apps](../logic-apps/logic-apps-overview.md) , [devre dışı bırakılmakta](../scheduler/migrate-from-scheduler-to-logic-apps.md#retire-date)olan Azure Scheduler 'ı değiştiriyor. Zamanlayıcı 'da ayarladığınız işlerle çalışmaya devam etmek için lütfen en kısa sürede [Azure Logic Apps geçirin](../scheduler/migrate-from-scheduler-to-logic-apps.md) . 
>
> Zamanlayıcı artık Azure portal kullanılamıyor, ancak iş ve iş koleksiyonlarınızı yönetebilmeniz için [REST API](/rest/api/scheduler) ve [Azure Scheduler PowerShell cmdlet 'leri](scheduler-powershell-reference.md) Şu anda kullanılabilir durumda kalır.

## <a name="entity-hierarchy"></a>Varlık hiyerarşisi

Azure Scheduler REST API'si şu ana varlıkları veya kaynakları kullanıma sunar ve kullanır:

| Varlık | Description |
|--------|-------------|
| **İş** | Yürütmek üzere basit veya karmaşık stratejilere sahip tek bir yinelenen eylemi tanımlar. Eylemler HTTP, Depolama kuyruğu, Service Bus kuyruğu ya da Service Bus konusu isteklerini içerebilir. | 
| **İş koleksiyonu** | Bir grup işi içerir ve koleksiyondaki işler tarafından paylaşılan ayarlar, kotalar ve kısıtlamaları tutar. Azure aboneliği sahibi olarak iş koleksiyonları oluşturabilir ve işleri kullanım veya uygulama sınırlarına göre gruplandırabilirsiniz. Bir iş koleksiyonu şu özniteliklere sahiptir: <p>- Tek bir bölge için kısıtlanmıştır. <br>- Koleksiyondaki tüm işlerin kullanımını kısıtlamak için kota uygulamanızı sağlar. <br>- Kotalar MaxJobs ve MaxRecurrence değerlerini içerir. | 
| **İş geçmişi** | Durum ve yanıt ayrıntıları gibi iş yürütme ayrıntılarını açıklar. |
||| 

## <a name="entity-management"></a>Varlık yönetimi

En geniş anlamıyla Scheduler REST API'si, varlıkların yönetilmesi için bu işlemleri kullanıma sunar.

### <a name="job-management"></a>İş yönetimi

İş oluşturma ve düzenleme işlemlerini destekler. Açık oluşturma olmaması için tüm işlerin var olan bir iş koleksiyonuna ait olması gerekir. Daha fazla bilgi için bkz. [Scheduler REST API - İşler](/rest/api/scheduler/jobs). Bu işlemler için URI adresi aşağıda verilmiştir:

```
https://management.azure.com/subscriptions/{subscriptionID}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}/jobs/{jobName}
```

### <a name="job-collection-management"></a>İş koleksiyonu yönetimi

Kotalar ve paylaşılan ayarlarla eşleştirilen işlerin ve iş koleksiyonlarının oluşturulmasını ve düzenlenmesini destekler. Örneğin kotalar, maksimum iş sayısını ve en küçük yineleme aralığını belirtir. Daha fazla bilgi için bkz. [Scheduler REST API - İş Koleksiyonları](/rest/api/scheduler/jobcollections). Bu işlemler için URI adresi aşağıda verilmiştir:

```
https://management.azure.com/subscriptions/{subscriptionID}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}
```

### <a name="job-history-management"></a>İş geçmişi yönetimi

Geçen iş süresi ve iş yürütme sonuçları gibi 60 günlük iş yürütme geçmişinin alınması için GET işleminin kullanılmasını destekler. Durum ve duruma göre filtreleme için sorgu dizesi parametresi desteği sunar. Daha fazla bilgi için bkz. [Scheduler REST API - İşler - İş Geçmişini Listeleme](/rest/api/scheduler/jobs/listjobhistory). Bu işlemin URI adresi aşağıdadır:

```
https://management.azure.com/subscriptions/{subscriptionID}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}/jobs/{jobName}/history
```

## <a name="job-types"></a>İş türleri

Azure Scheduler, birden fazla iş türünü destekler: 

* Mevcut bir hizmet veya iş yükü için uç nokta olduğunda, için TLS 'yi destekleyen HTTPS işleri dahil olmak üzere HTTP işleri
* Depolama kuyruklarına ileti gönderme gibi Depolama kuyruklarını kullanan iş yükleri için Depolama kuyruğu işleri
* Service Bus kuyruklarını kullanan iş yükleri için Service Bus kuyruğu işleri
* Service Bus konularını kullanan iş yükleri için Service Bus konusu işleri

## <a name="job-definition"></a>İş tanımı

Bir Scheduler işi genel olarak şu temel bölümlerden oluşur:

* İş zamanlayıcı başlatıldığında çalıştırılan eylem
* İsteğe bağlı: İşin çalıştırma süresi
* İsteğe bağlı: İşin ne zaman ve ne sıklıkta tekrarlanacağı
* İsteğe bağlı: Birincil eylem başarısız olursa çalıştırılacak hata eylemi

İş aynı zamanda işin bir sonraki zamanlanmış çalışma zamanı gibi sistem tarafından sağlanan verileri de içerir. İşin kod tanımı, şu öğelere sahip olan bir JavaScript Nesne Gösterimi (JSON) biçimli nesnedir:

| Öğe | Gerekli | Açıklama | 
|---------|----------|-------------| 
| [**startTime**](#start-time) | No | [ISO 8601 biçiminde](https://en.wikipedia.org/wiki/ISO_8601) saat dilimi farkına sahip olan işin başlangıç zamanı | 
| [**ön**](#action) | Yes | **errorAction** nesnesi de içerebilecek birincil eylem ayrıntıları | 
| [**errorAction**](#error-action) | No | Birincil eylemin başarısız olması durumunda çalışan ikinci eylemin ayrıntıları |
| [**yinelemeyi**](#recurrence) | No | Yinelenen bir işin sıklık ve aralık gibi ayrıntıları | 
| [**retryPolicy**](#retry-policy) | No | Bir eylemin yeniden deneme sıklığını belirten ayrıntılar | 
| [**durumunda**](#state) | Yes | İşin geçerli durumunun ayrıntıları |
| [**durumlarına**](#status) | Yes | Hizmet tarafından denetlenen geçerli iş durumu ayrıntıları |
||||

Bu örnekte bir HTTP eyleminin kapsamlı iş tanımı gösterilmiştir ve öğeler sonraki bölümlerde ayrıntılı bir şekilde açıklanacaktır: 

```json
"properties": {
   "startTime": "2012-08-04T00:00Z",
   "action": {
      "type": "Http",
      "request": {
         "uri": "http://contoso.com/some-method", 
         "method": "PUT",          
         "body": "Posting from a timer",
         "headers": {
            "Content-Type": "application/json"
         },
         "retryPolicy": { 
             "retryType": "None" 
         },
      },
      "errorAction": {
         "type": "Http",
         "request": {
            "uri": "http://contoso.com/notifyError",
            "method": "POST"
         }
      }
   },
   "recurrence": {
      "frequency": "Week",
      "interval": 1,
      "schedule": {
         "weekDays": ["Monday", "Wednesday", "Friday"],
         "hours": [10, 22]
      },
      "count": 10,
      "endTime": "2012-11-04"
   },
   "state": "Disabled",
   "status": {
      "lastExecutionTime": "2007-03-01T13:00:00Z",
      "nextExecutionTime": "2007-03-01T14:00:00Z ",
      "executionCount": 3,
      "failureCount": 0,
      "faultedCount": 0
   }
}
```

<a name="start-time"></a>

## <a name="starttime"></a>startTime

**startTime** nesnesinde başlangıç zamanını ve saat dilimi farkını [ISO 8601 biçiminde](https://en.wikipedia.org/wiki/ISO_8601) belirtebilirsiniz.

<a name="action"></a>

## <a name="action"></a>eylem

Scheduler işiniz belirtilen zamanlamayı kullanarak birincil **eylemi** çalıştırır. Scheduler HTTP, Depolama kuyruğu, Service Bus kuyruğu ve Service Bus konusu eylemlerini destekler. Birincil **action** nesnesi başarısız olursa Scheduler hatayı işleyen ikincil [**errorAction**](#erroraction) nesnesini çalıştırabilir. **action** nesnesi şu öğeleri tanımlar:

* Eylemin hizmet türü
* Eylemin ayrıntıları
* Alternatif bir **errorAction**

Yukarıdaki örnekte bir HTTP eylemi açıklanmıştır. Depolama kuyruğu eylemi örneği aşağıda verilmiştir:

```json
"action": {
   "type": "storageQueue",
   "queueMessage": {
      "storageAccount": "myStorageAccount",  
      "queueName": "myqueue",                
      "sasToken": "TOKEN",                   
      "message": "My message body"
    }
}
```

Service Bus kuyruğu eylemi örneği aşağıda verilmiştir:

```json
"action": {
   "type": "serviceBusQueue",
   "serviceBusQueueMessage": {
      "queueName": "q1",  
      "namespace": "mySBNamespace",
      "transportType": "netMessaging", // Either netMessaging or AMQP
      "authentication": {  
         "sasKeyName": "QPolicy",
         "type": "sharedAccessKey"
      },
      "message": "Some message",  
      "brokeredMessageProperties": {},
      "customMessageProperties": {
         "appname": "FromScheduler"
      }
   }
},
```

Service Bus konusu eylemi örneği aşağıda verilmiştir:

```json
"action": {
   "type": "serviceBusTopic",
   "serviceBusTopicMessage": {
      "topicPath": "t1",  
      "namespace": "mySBNamespace",
      "transportType": "netMessaging", // Either netMessaging or AMQP
      "authentication": {
         "sasKeyName": "QPolicy",
         "type": "sharedAccessKey"
      },
      "message": "Some message",
      "brokeredMessageProperties": {},
      "customMessageProperties": {
         "appname": "FromScheduler"
      }
   }
},
```

Paylaşılan Erişim İmzası (SAS) belirteçleri hakkında daha fazla bilgi için bkz. [Paylaşılan Erişim İmzasıyla Yetkilendirme](../storage/common/storage-sas-overview.md).

<a name="error-action"></a>

## <a name="erroraction"></a>errorAction

İşinizin birincil **action** nesnesi başarısız olursa Scheduler hatayı işleyen bir **errorAction** nesnesini çalıştırabilir. Birincil **action** nesnesinde bir **errorAction** nesnesi belirterek Scheduler uygulamasının uç nokta hatasını işlemesini veya kullanıcıya bildirim göndermesini sağlayabilirsiniz. 

Örneğin birincil uç noktada olağanüstü durum oluşursa **errorAction** kullanarak ikincil uç noktayı çağırabilir veya hata işleme uç noktasını bilgilendirebilirsiniz. 

Birincil **eylemde** olduğu gibi, hata eylemi diğer eylemler temelinde basit ya da birleşik mantıklı olabilir. 

<a name="recurrence"></a>

## <a name="recurrence"></a>yineleme

İşin JSON tanımında **recurrence** nesnesi varsa o iş yinelenir, örneğin:

```json
"recurrence": {
   "frequency": "Week",
   "interval": 1,
   "schedule": {
      "hours": [10, 22],
      "minutes": [0, 30],
      "weekDays": ["Monday", "Wednesday", "Friday"]
   },
   "count": 10,
   "endTime": "2012-11-04"
},
```

| Özellik | Gerekli | Değer | Açıklama | 
|----------|----------|-------|-------------| 
| **frequency** | **recurrence** kullanıldığında evet | "Minute", "Hour", "Day", "Week", "Month", "Year" | Yinelemeler arası zaman birimi | 
| **aralığında** | No | 1-1000 arası, ikisi de dahil | **frequency** nesnesine göre yinelemeler arasındaki zaman birimi sayısını belirleyen pozitif tamsayı | 
| **çizelgesini** | No | Değişir | Daha karmaşık ve gelişmiş zamanlamaların ayrıntıları. Bkz. **hours**, **minutes**, **weekDays**, **months** ve **monthDays** | 
| **saatlerinin** | No | 1-24 arası | İşin çalıştırılacağı saati belirten dizi | 
| **dakika** | No | 0-59 | İşin çalıştırılacağı dakikayı belirten dizi | 
| **aylar** | No | 1-12 arası | İşin çalıştırılacağı ayı belirten dizi | 
| **monthDays** | No | Değişir | İşin çalıştırılacağı ayın gününü belirten dizi | 
| **weekDays** | No | "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday" | İşin çalıştırılacağı haftanın gününü belirten dizi | 
| **count** | No | <*seçim*> | Yineleme sayısı. Varsayılan değer sonsuzdur. **count** ve **endTime** nesnelerini birlikte kullanamazsınız ancak ilk tamamlanan kural uygulanır. | 
| **endTime** | No | <*seçim*> | Yinelemenin durdurulacağı tarih ve saat. Varsayılan değer sonsuzdur. **count** ve **endTime** nesnelerini birlikte kullanamazsınız ancak ilk tamamlanan kural uygulanır. | 
||||

Bu öğeler hakkında daha fazla bilgi için bkz. [Karmaşık zamanlamalar ve gelişmiş yinelemeler oluşturma](../scheduler/scheduler-advanced-complexity.md).

<a name="retry-policy"></a>

## <a name="retrypolicy"></a>retryPolicy

Bir Scheduler işinin başarısız olması durumunda Scheduler uygulamasının eylemi yeniden deneme kararını ve şeklini belirleyen bir yenileme ilkesi ayarlayabilirsiniz. Scheduler varsayılan olarak işi 30 saniyelik aralıklarla dört kez daha dener. Bu ilkeyi daha katı veya gevşek haline getirebilirsiniz. Örneğin şu ilke eylemi günde iki kez dener:

```json
"retryPolicy": { 
   "retryType": "Fixed",
   "retryInterval": "PT1D",
   "retryCount": 2
},
```

| Özellik | Gerekli | Değer | Açıklama | 
|----------|----------|-------|-------------| 
| **retryType** | Yes | **Fixed**, **None** | Bir yenileme ilkesi belirtip (**fixed**) belirtmediğinizi (**none**) belirler. | 
| **retryInterval** | No | PT30S | Yeniden deneme girişimleri arasındaki aralığı ve sıklığı [ISO 8601 biçiminde](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations) belirtir. Minimum değer 15 saniye, maksimum değer ise 18 aydır. | 
| **retryCount** | No | 4 | Yeniden deneme girişimlerinin sayısını belirtir. Maksimum değer 20'dir. | 
||||

Daha fazla bilgi için bkz. [Yüksek kullanılabilirlik ve güvenilirlik](../scheduler/scheduler-high-availability-reliability.md).

<a name="status"></a>

## <a name="state"></a>state

Bir işin durumu **Etkin**, **Devre Dışı**, **Tamamlandı** veya **Hatalı** olacaktır, örneğin: 

`"state": "Disabled"`

İşleri **Etkin** veya **Devre Dışı** durumuna almak için işlerde PUT veya PATCH işlemlerini kullanabilirsiniz.
Ancak bir işin durumu **Tamamlandı** veya **Arızalı** şeklindeyse durumu güncelleştiremezsiniz ancak iş üzerinde DELETE işlemini gerçekleştirebilirsiniz. Scheduler, tamamlandı ve arızalı durumundaki işleri 60 gün sonra siler. 

<a name="status"></a>

## <a name="status"></a>durum

Bir iş başlatıldıktan sonra Scheduler, yalnızca Scheduler tarafından denetlenen **status** nesnesiyle iş durumu hakkında bilgi döndürür. Ancak **status** nesnesini **job** nesnesinin içinde bulabilirsiniz. İş durumunda bulunan bilgiler şunlardır:

* Varsa bir önceki yürütme işleminin zamanı
* Devam eden işler için bir sonraki zamanlanmış yürütmenin zamanı
* İş yürütme sayısı
* Varsa başarısız işlem sayısı
* Varsa hatalı işlem sayısı

Örnek:

```json
"status": {
   "lastExecutionTime": "2007-03-01T13:00:00Z",
   "nextExecutionTime": "2007-03-01T14:00:00Z ",
   "executionCount": 3,
   "failureCount": 0,
   "faultedCount": 0
}
```

## <a name="next-steps"></a>Sonraki adımlar

* [Karmaşık zamanlamalar ve gelişmiş yinelemeler oluşturma](scheduler-advanced-complexity.md)
* [Azure Scheduler REST API başvurusu](/rest/api/scheduler)
* [Azure Scheduler PowerShell cmdlet’leri başvurusu](scheduler-powershell-reference.md)
* [Sınırlar, kotalar, varsayılan değerler ve hata kodları](scheduler-limits-defaults-errors.md)