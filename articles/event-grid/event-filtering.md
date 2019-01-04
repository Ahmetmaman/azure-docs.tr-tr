---
title: Azure olay ızgarası için filtreleme olay
description: Azure Event Grid aboneliği oluştururken olayları filtrelemek açıklar.
services: event-grid
author: tfitzmac
ms.service: event-grid
ms.topic: conceptual
ms.date: 12/21/2018
ms.author: tomfitz
ms.openlocfilehash: 77225c4d659755ec6de1a14bf67bd0a62659fb6a
ms.sourcegitcommit: 7862449050a220133e5316f0030a259b1c6e3004
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/22/2018
ms.locfileid: "53753872"
---
# <a name="understand-event-filtering-for-event-grid-subscriptions"></a>Olay filtreleme için Event Grid abonelikleri anlama

Bu makalede, hangi olayların uç noktanıza gönderilen filtresi için farklı yollar açıklanmaktadır. Bir olay aboneliği oluştururken, filtreleme için üç seçeneğiniz vardır:

* Olay türleri
* Konu ile başlayan veya biten
* Gelişmiş alanları ve işleçler

## <a name="event-type-filtering"></a>Olay türü filtreleme

Varsayılan olarak, tüm [olay türleri](event-schema.md) için olay kaynağı uç noktasına gönderilir. Yalnızca belirli olay türleri uç noktanıza göndermek karar verebilirsiniz. Örneğin, kaynaklarınıza güncelleştirme bildirimlerini, ancak silme gibi diğer işlemler için bildirim yok. Bu durumda, filtre `Microsoft.Resources.ResourceWriteSuccess` olay türü. Bir dizi olay türleriyle sağlayın veya belirtin `All` tüm etkinlik türleri için olay kaynağı alınamıyor.

Olay türüne göre filtrelemek için JSON söz dizimi şu şekildedir:

```json
"filter": {
  "includedEventTypes": [
    "Microsoft.Resources.ResourceWriteFailure",
    "Microsoft.Resources.ResourceWriteSuccess"
  ]
}
```

## <a name="subject-filtering"></a>Konu filtresi

Basit konuya göre filtreleme için konu için bir başlangıç veya bitiş değeri belirtin. Örneğin, konu ile sona erer belirtebilirsiniz `.txt` yalnızca depolama hesabı için bir metin dosyası karşıya yükleme için ilgili olayları almak için. Ya da konu filtreleyebilirsiniz ile başlayan `/blobServices/default/containers/testcontainer` bu kapsayıcı, ancak diğer kapsayıcı olmayan depolama hesabındaki tüm olayları almak için.

Olayları özel konular yayımlarken konularla ilgili olay ilgilenen olup olmadığını bilmek aboneleri için kolaylaştıran olaylarınızı oluşturun. Aboneleri filtre ve rota olayları konu özelliğini kullanın. Aboneler tarafından yol kesimleri süzebilirsiniz olayın gerçekleştiği için yol eklemeyi düşünün. Aboneler, sayısı azalacağından veya geniş çapta olayları filtrelemek yol sağlar. Üç segment yolu gibi sağlarsanız `/A/B/C` Bu konu, abonelerin ilk segmente göre filtreleyebilirsiniz `/A` çok sayıda olayları almak için. Bu olaylar gibi konular ile aboneleri `/A/B/C` veya `/A/D/E`. Diğer aboneler tarafından filtreleyebilirsiniz `/A/B` daha dar bir etkinlik kümesi alınamıyor.

Olay türüne göre filtrelemek için JSON söz dizimi şu şekildedir:

```json
"filter": {
  "subjectBeginsWith": "/blobServices/default/containers/mycontainer/log",
  "subjectEndsWith": ".jpg"
}

```

## <a name="advanced-filtering"></a>Gelişmiş filtreleme

Veri alanlardaki değerlere göre filtrelemek ve karşılaştırma işleci belirtmek için Gelişmiş filtreleme seçeneğini kullanın. İçinde belirttiğiniz Gelişmiş filtreleme:

* işleç türü - karşılaştırma türü.
* anahtarı - alanda filtreleme için kullanmakta olduğunuz olay verileri. Bir sayı, Boole veya dize olabilir.
* değeri veya değerleri - değer veya anahtar Karşılaştırılacak değerler.

Gelişmiş filtreler kullanmak için JSON söz dizimi şu şekildedir:

```json
"filter": {
  "advancedFilters": [
    {
      "operatorType": "NumberGreaterThanOrEquals",
      "key": "Data.Key1",
      "value": 5
    },
    {
      "operatorType": "StringContains",
      "key": "Subject",
      "values": ["container1", "container2"]
    }
  ]
}
```

### <a name="operator"></a>İşleç

Sayılar için kullanılabilir işleçler şunlardır:

* NumberGreaterThan
* NumberGreaterThanOrEquals
* NumberLessThan
* NumberLessThanOrEquals
* NumberIn
* NumberNotIn

Boole değerlerini için kullanılabilir işleç şöyledir: BoolEquals

Dizeler için kullanılabilir işleçler şunlardır:

* StringContains
* StringBeginsWith
* StringEndsWith
* StringIn
* StringNotIn

Tüm dize karşılaştırmaları çalışması insensitve ' dir.

### <a name="key"></a>Anahtar

Event Grid şema olayları için anahtar için aşağıdaki değerleri kullanın:

* Kimlik
* Konu
* Özne
* EventType
* dataVersion
* Olay verilerini (gibi Data.key1)

Bulut olayları şemasında olayları için anahtar için aşağıdaki değerleri kullanın:

* EventID
* Kaynak
* EventType
* eventTypeVersion
* Olay verilerini (gibi Data.key1)

Özel giriş şeması için olay veri alanlarını (gibi Data.key1) kullanın.

### <a name="values"></a>Değerler

Değerler olabilir:

* number
* dize
* boole
* array

### <a name="limitations"></a>Sınırlamalar

Gelişmiş filtreleme aşağıdaki sınırlamalara sahiptir:

* Event grid aboneliği başına beş Gelişmiş Filtreler
* dize değeri başına 512 karakter
* Beş değerleri **içinde** ve **değil** işleçleri
* Anahtarı yalnızca bir düzey iç içe geçme (gibi data.key1) olabilir
* Özel olay şemaları yalnızca üst düzey alanlarda filtrelenebilir

Birden fazla filtreye aynı anahtar kullanılır.

## <a name="next-steps"></a>Sonraki adımlar

* PowerShell ve Azure CLI ile olayları filtreleme hakkında bilgi edinmek için [olayları Event Grid için filtre](how-to-filter-events.md).
* Event Grid ile hızla çalışmaya başlamak için bkz: [Azure Event Grid ile özel olaylar oluşturma ve yönlendirme](custom-event-quickstart.md).
