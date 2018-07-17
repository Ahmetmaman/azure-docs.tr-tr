---
title: Azure Blob Depolama olaylarına tepki | Microsoft Docs
description: Blob depolama olaylarına abone olmak için Azure Event Grid’i kullanın.
services: storage,event-grid
keywords: ''
author: cbrooksmsft
ms.author: cbrooks
ms.date: 01/30/2018
ms.topic: article
ms.service: storage
ms.openlocfilehash: 68d722338562d21d59dd720250a62b8603c8af43
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39076549"
---
# <a name="reacting-to-blob-storage-events"></a>BLOB Depolama olaylarına tepki verme

Azure Depolama olaylarını uygulamaların oluşturulmasını ve modern sunucusuz mimarileri kullanarak blobları silme işlemi için tepki verin. Bunu karmaşık kod veya pahalı ve verimsiz yoklama Hizmetleri gerek kalmadan yapar.  Bunun yerine, olayların gönderilmesini [Azure Event Grid](https://azure.microsoft.com/services/event-grid/) gibi abonelere [Azure işlevleri](https://azure.microsoft.com/services/functions/), [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/), ve hatta kendi özel http dinleyicisi ve yalnızca kullandığınız kadarı için ödeme yaparsınız. 

Ortak Blob Depolama olayı senaryolar resim veya video işleme, arama dizini oluşturma veya tüm dosya odaklı iş akışı içerir.  Zaman uyumsuz dosya yüklemeleri olayları için harika bir uygun olan.  Seyrek görülen değişiklikler, ancak senaryonuza anında yanıt verme hızını gerektirir, olay tabanlı mimari özellikle etkili olabilir.

Depolama olaylarının kullanılabilirliği Event Grid'in [kullanılabilirliğine](../../event-grid/overview.md) bağlıdır ve Event Grid gibi diğer bölgelerde kullanıma sunulacaktır. Bir göz atın [rota Blob Depolama olaylarını bir özel web uç noktası - CLI](storage-blob-event-quickstart.md) veya [rota Blob Depolama olaylarını bir özel web uç noktası - PowerShell](storage-blob-event-quickstart-powershell.md) hızlı bir örnek için. 

![Event Grid modeli](./media/storage-blob-event-overview/event-grid-functional-model.png)

## <a name="blob-storage-accounts"></a>Blob Storage hesapları
BLOB Depolama olaylarını kullanılabilir [Blob Depolama hesapları](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#blob-storage-accounts) ve [genel amaçlı v2 depolama hesaplarının](../common/storage-account-options.md#general-purpose-v2-accounts). **Genel Amaçlı v2 (GPv2)** hesapları Bloblar, Dosyalar, Kuyruklar ve Tablolar dahil olmak üzere depolama hizmetlerinin tamamına yönelik tüm özellikleri destekleyen depolama hesaplarıdır. **Blob depolama hesabı**, yapılandırılmamış verilerinizi bloblar (nesneler) olarak Azure Storage’da depolamanıza yönelik özel depolama hesabıdır. Blob Storage hesapları, genel amaçlı depolama hesaplarınıza benzer ve blok blobları ve ilave blobları için %100 API tutarlığı dahil günümüzde kullandığınız tüm harika dayanıklılık, kullanılabilirlik, ölçeklenebilirlik ve performans özelliklerini paylaşır. Yalnızca blok veya engelleme blobunun gerektiği uygulamalar için Blob Storage hesaplarının kullanılmasını öneririz. 

## <a name="available-blob-storage-events"></a>Blob Depolama olayları kullanılabilir
Event grid'i kullanır [olay abonelikleri](../../event-grid/concepts.md#event-subscriptions) abonelere olay iletileri yönlendirmek için.  BLOB Depolama olay abonelikleri, iki tür olay şunları içerebilir:  

> |Olay Adı|Açıklama|
> |----------|-----------|
> |`Microsoft.Storage.BlobCreated`|Bir blob oluşturulduğunda veya aracılığıyla yerini harekete `PutBlob`, `PutBlockList`, veya `CopyBlob` işlemleri|
> |`Microsoft.Storage.BlobDeleted`|Bir blob aracılığıyla silindiğinde tetiklenen bir `DeleteBlob` işlemi|

## <a name="event-schema"></a>Olay Şeması
BLOB Depolama olaylarını verilerinizdeki değişiklikleri yanıtlamak için gereken tüm bilgileri içerir.  EventType özelliği "Microsoft.Storage" ile başladığından bir Blob Depolama olayı belirleyebilirsiniz. Event Grid olay özelliklerinin kullanımı hakkında ek bilgi belirtilmiştir [Event Grid olay şeması](../../event-grid/event-schema.md).  

> |Özellik|Tür|Açıklama|
> |-------------------|------------------------|-----------------------------------------------------------------------|
> |konu başlığı|dize|Olay yayar depolama hesabının Tam Azure Resource Manager kimliği.|
> |Konu|dize|Depolama hesapları, hizmetler ve kapsayıcılar için Azure RBAC açıklamak için kullandığımız aynı Genişletilmiş Azure Resource Manager biçimini kullanarak konusu olayın nesne göreli bir kaynak yolu.  Bu biçim bir servis talebi koruyarak blob adı içerir.|
> |eventTime|dize|Olayın oluşturulduğu, ISO 8601 biçiminde tarih/saat|
> |olay türü|dize|"Microsoft.Storage.BlobCreated" veya "Microsoft.Storage.BlobDeleted"|
> |Kimlik|dize|Bu benzersiz tanımlayıcı olay|
> |dataVersion|dize|Veri nesnesinin şema sürümü.|
> |metadataVersion|dize|Üst düzey özellikler şema sürümü.|
> |veri|object|Blob depolama özel olay verilerini toplama|
> |data.contentType|dize|Content-Type üst bilgisinde blobundan döndürülürdü blob'u, içerik türü|
> |data.contentLength|number|Content-Length üst bilgisinde blobundan döndürülürdü gibi bayt sayısını temsil eden tamsayı olduğu gibi blob boyutu.  BlobCreated olaylı, ancak BlobDeleted gönderilir.|
> |Data.URL|dize|Olayın konusu nesnenin URL'si|
> |data.eTag|dize|Bu olay harekete nesne etag'i.  BlobDeleted olay için kullanılamaz.|
> |Data.api|dize|Bu olay harekete API işlemi adı. BlobCreated olaylar için bu değer "PutBlob", "PutBlockList" veya "CopyBlob" olur. BlobDeleted olaylar için bu değer "DeleteBlob" olur. Bu değerler Azure depolama tanılama günlüklerinde mevcut olan aynı API adlardır. Bkz: [işlemler ve durum iletileri günlüğe](https://docs.microsoft.com/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages).|
> |Data.Sequencer|dize|Herhangi bir belirli blob adı için olayların mantıksal sırasını temsil eden bir donuk bir dize değeri.  Kullanıcılar, standart dize karşılaştırması, aynı blob adı iki olayların göreli sırasını anlamak için kullanabilirsiniz.|
> |data.requestId|dize|Depolama API işlemi için istek hizmet tarafından oluşturulan kimliği. Azure tanılama günlüklerinde "istek kimliği üstbilgisi" alanını kullanarak kaydeder ve başlatmasını API çağrısı 'x-ms-isteği-id' üst bilgisinde döndürülen depolama ilişkilendirmek için kullanılabilir. Bkz: [günlük biçimi](https://docs.microsoft.com/rest/api/storageservices/storage-analytics-log-format).|
> |data.clientRequestId|dize|Depolama API işlemi için sağlanan istemci istek kimliği. "X-ms-istemci-request-id" üst bilgisini kullanarak istemci isteklerinde sağlanan ve günlüklerinde "client-request-id" alanını kullanarak Azure depolama tanılama günlüklerine ilişkilendirmek için kullanılır. Bkz: [günlük biçimi](https://docs.microsoft.com/rest/api/storageservices/storage-analytics-log-format). |
> |data.storageDiagnostics|object|Bazen Azure depolama hizmeti tarafından bulunan Tanılama verileri. Mevcut olduğunda olay tüketicileri tarafından yoksayılacak.|
|data.blobType|dize|Blob türü. Geçerli değerler "BlockBlob" veya "PageBlob" olmalı.| 

BlobCreated olayın bir örnek aşağıda verilmiştir:
```json
[{
  "topic": "/subscriptions/319a9601-1ec0-0000-aebc-8fe82724c81e/resourceGroups/testrg/providers/Microsoft.Storage/storageAccounts/myaccount",
  "subject": "/blobServices/default/containers/testcontainer/blobs/file1.txt",
  "eventType": "Microsoft.Storage.BlobCreated",
  "eventTime": "2017-08-16T01:57:26.005121Z",
  "id": "602a88ef-0001-00e6-1233-1646070610ea",
  "data": {
    "api": "PutBlockList",
    "clientRequestId": "799304a4-bbc5-45b6-9849-ec2c66be800a",
    "requestId": "602a88ef-0001-00e6-1233-164607000000",
    "eTag": "0x8D4E44A24ABE7F1",
    "contentType": "text/plain",
    "contentLength": 447,
    "blobType": "BlockBlob",
    "url": "https://myaccount.blob.core.windows.net/testcontainer/file1.txt",
    "sequencer": "00000000000000EB000000000000C65A",
  },
  "dataVersion": "",
  "metadataVersion": "1"
}]

```

Daha fazla bilgi için [Blob Depolama olayları şema](../../event-grid/event-schema-blob-storage.md).

## <a name="filtering-events"></a>Olayları filtreleme
BLOB olay abonelikleri olayın türüne ve kapsayıcı adı ve blob adı, oluşturulduğu veya silindiği nesne tarafından göre filtrelenebilir.  Filtre uygulanabilir olay abonelikleri ya da sırasında [oluşturma](/cli/azure/eventgrid/event-subscription?view=azure-cli-latest#az_eventgrid_event_subscription_create) olay aboneliğinin veya [sonraki bir zamanda](/cli/azure/eventgrid/event-subscription?view=azure-cli-latest#az_eventgrid_event_subscription_update). Konu filtreleri temel Event Grid çalışma "ile başlayan" ve "böylece abone ile eşleşen bir konu ile olayları teslim ile eşleşir, ends". 

Blob Depolama olaylarını konusu biçimini kullanır:

```
/blobServices/default/containers/<containername>/blobs/<blobname>
```

Bir depolama hesabı için tüm olaylara eşleştirmek için boş konu filtreleri bırakabilirsiniz.

BLOB kapsayıcıları bir ön eki paylaşımı kümesi içinde oluşturulan olaylardan eşleştirmek için kullanmak bir `subjectBeginsWith` gibi Filtrele:

```
/blobServices/default/containers/containerprefix
```

Belirli bir kapsayıcıda oluşturulan BLOB'ları olaylardan eşleştirmek için kullanmak bir `subjectBeginsWith` gibi Filtrele:

```
/blobServices/default/containers/containername/
```

BLOB'ları belirli kapsayıcısına bir blob adı ön eki paylaşımı oluşturulan olaylardan eşleştirmek için kullanmak bir `subjectBeginsWith` gibi Filtrele:

```
/blobServices/default/containers/containername/blobs/blobprefix
```

BLOB'ları belirli kapsayıcısına bir blob soneki paylaşımı oluşturulan olaylardan eşleştirmek için kullanmak bir `subjectEndsWith` Filtresi ".log" veya ".jpg" gibi. Daha fazla bilgi için [Event Grid kavramları](../../event-grid/concepts.md#event-subscriptions).

## <a name="practices-for-consuming-events"></a>Olayları kullanan uygulamalar
Blob Depolama olayları işleyen uygulamaları birkaç önerilen uygulamaları izlemelisiniz:
> [!div class="checklist"]
> * Birden çok abonelik aynı olay işleyicisi için rota olayları yapılandırılması gibi belirli bir kaynaktan gelen olayları olduğu varsayılır değil ancak beklediğiniz depolama hesabından geldiğinden emin olmak için iletisinin konu denetlemek için önemlidir.
> * Benzer şekilde, eventType tek bir işlem için hazır ve aldığınız tüm olayları beklediğiniz türleri olacağını varsaymayın olup olmadığını denetleyin.
> * Bazı gecikmeden sonra ve iletileri sıralamaya gelen nesnelerle ilgili bilgilerinizi hala güncel olup olmadığını anlamak için etag alanları kullanın.  Ayrıca, herhangi bir nesne üzerinde olayların sırası anlamak için sıralayıcı'yı alanlarını kullanın.
> * Ne tür işlem bir bloba izin anlamak için blobType alanı kullanın ve hangi istemci kitaplığı, türleri bloba erişmek için kullanmanız gerekir. Geçerli değerler ya da `BlockBlob` veya `PageBlob`. 
> * Url alanla kullanmak `CloudBlockBlob` ve `CloudAppendBlob` bloba erişmek için oluşturucu.
> * Alanları anlamadığınız yoksayın. Bu yöntem, dayanıklı, gelecekte eklenebilir yeni özellikler kalmasına yardımcı olur.


## <a name="next-steps"></a>Sonraki adımlar

Event Grid hakkında daha fazla bilgi edinmek ve Blob Depolama olaylarını deneyin:

- [Event Grid Hakkında](../../event-grid/overview.md)
- [BLOB Depolama olaylarını bir özel web uç noktasına yönlendirme](storage-blob-event-quickstart.md)
