---
title: Azure İzleyici (Önizleme) Azure Event Hubs ölçümlerde | Microsoft Docs
description: Azure izleme, olay hub'ları izlemek için kullanın
services: event-hubs
documentationcenter: .NET
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/16/2018
ms.author: shvija
ms.openlocfilehash: c59cb8277bee13a83d0bf17c26deef1b8fc8d3e6
ms.sourcegitcommit: 8899e76afb51f0d507c4f786f28eb46ada060b8d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/16/2018
ms.locfileid: "51822852"
---
# <a name="azure-event-hubs-metrics-in-azure-monitor-preview"></a>Azure İzleyici (Önizleme), Azure Event Hubs ölçümleri

Olay hub'ları ölçümleri Event Hubs kaynakları Azure aboneliğinizde durumunu sağlar. Ölçüm verileri zengin ile event hubs'ınız değil yalnızca ad alanı düzeyinde, aynı zamanda varlık düzeyinde genel durumunu değerlendirebilirsiniz. Bu istatistikler, olay hub'larınız durumunu izlemek için yardımcı önemli olabilir. Ölçümler, Azure desteğine başvurun gerek kalmadan kök neden sorunlarını da yardımcı olabilir.

Azure İzleyici, çeşitli Azure Hizmetleri genelinde izleme için birleştirilmiş bir kullanıcı arabirimi sağlar. Daha fazla bilgi için [Microsoft Azure'da izleme](../monitoring-and-diagnostics/monitoring-overview.md) ve [almak Azure İzleyici ölçümleri .NET ile](https://github.com/Azure-Samples/monitor-dotnet-metrics-api) GitHub üzerinde örnek.

## <a name="access-metrics"></a>Erişim ölçümleri

Azure İzleyici ölçümlerine erişim birden çok yol sağlar. Ya da erişim ölçümleri ile yapabilecekleriniz [Azure portalında](https://portal.azure.com), veya Azure İzleyici API'leri (REST ve .NET) ve Operations Management Suite ve Event Hubs gibi analiz çözümleri kullanın. Daha fazla bilgi için [İzleme verilerine Azure İzleyicisi tarafından toplanan](../azure-monitor/platform/data-collection.md).

Ölçümler, varsayılan olarak etkindir ve en son 30 Günün verilerini erişebilir. Uzun bir süre saklamak istiyorsanız ölçüm verileri bir Azure depolama hesabına arşivleyebilir. Bu yapılandırılan [tanılama ayarları](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#diagnostic-settings) Azure İzleyici'de.

## <a name="access-metrics-in-the-portal"></a>Portalda erişim ölçümlerini

Zaman içinde ölçümleri izleyebilirsiniz [Azure portalında](https://portal.azure.com). Aşağıdaki örnek, başarılı istekleri ve hesap düzeyinde gelen istekleri görüntülemek gösterilmektedir:

![][1]

Ad alanı aracılığıyla doğrudan ölçümleri de erişebilirsiniz. Bunu yapmak için ad alanını seçin ve ardından **ölçümleri (Peview)**. Olay hub'ı kapsamına filtrelenmiş ölçümleri görüntülemek için olay hub'ı seçin ve ardından **ölçümler (Önizleme)**.

Ölçümleri boyutlarını desteklemek için aşağıdaki örnekte gösterildiği gibi istenen boyut değerine sahip filtre gerekir:

![][2]

## <a name="billing"></a>Faturalandırma

Ölçümleri kullanarak Azure İzleyici'de önizleme şu an ücretsizdir. Ölçüm verilerini alma, ek çözümleri kullanırsanız, ancak bu çözümler tarafından faturalandırılırsınız. Örneğin, ölçüm verileri bir Azure depolama hesabına arşivleme, Azure Depolama tarafından faturalandırılır. Gelişmiş analiz için ölçüm verileri Log analytics'e akış sahipse Azure tarafından ayrıca faturalandırılır.

Aşağıdaki ölçümler size sistem hizmetinizin genel bakış sunar. 

> [!NOTE]
> Farklı bir adla geçildiği size çeşitli ölçümleri kullanımdan. Bu, başvurularını güncelleştirmek gerektirebilir. "Kullanım dışı" anahtar sözcüğüyle işaretli ölçümleri, bundan sonra desteklenmeyecek.

Azure İzleyici, tüm ölçüm değerleri dakikada gönderilir. Zaman ayrıntı düzeyi ölçüm değerleri sunulduğu zaman aralığını tanımlar. Tüm Event Hubs ölçümler için desteklenen zaman aralığı 1 dakikadır.

## <a name="request-metrics"></a>İstek ölçümleri

Veri ve yönetim işlemleri istek sayısını sayar.

| Ölçüm Adı | Açıklama |
| ------------------- | ----------------- |
| Gelen istekler (Önizleme) | Belirtilen bir süredeki Azure Event Hubs hizmetine yapılan isteklerin sayısı. <br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName |
| Başarılı istekler (Önizleme)   | Azure Event Hubs hizmeti için belirtilen bir süredeki iletilen başarılı istek sayısı. <br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName |
| Sunucu hataları (Önizleme) | Azure Event Hubs hizmetinde bir hata nedeniyle belirtilen bir süredeki işlenmedi istek sayısı. <br/><br/>Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName |
|Kullanıcı hataları (Önizleme)|Kullanıcı hataları nedeniyle, belirtilen bir süredeki işlenmedi istek sayısı.<br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName|
|Kota aşıldı hataları (Önizleme)|Kullanılabilir kota isteklerinin sayısı aşıldı. Bkz: [bu makalede](event-hubs-quotas.md) Event Hubs kotaları hakkında daha fazla bilgi.<br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName|

## <a name="throughput-metrics"></a>Verimlilik metriklerini

| Ölçüm Adı | Açıklama |
| ------------------- | ----------------- |
|Daraltılmış istekler (Önizleme)|Aktarım hızı birimi kullanım aşıldığından bulunduğu için kısıtlanan istek sayısı.<br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName|

## <a name="message-metrics"></a>İleti ölçümleri

| Ölçüm Adı | Açıklama |
| ------------------- | ----------------- |
|Gelen iletiler (Önizleme)|Olay veya olay hub'ları için belirtilen bir süredeki gönderilen iletilerin sayısı.<br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName|
|Giden iletiler (Önizleme)|Olayları veya ileti sayısını belirtilen bir süredeki Event Hubs'dan alınan.<br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName|
|Gelen bayt (Önizleme)|Azure Event Hubs hizmeti için belirtilen bir süredeki gönderilen bayt sayısı.<br/><br/> Birim: bayt <br/> Toplama türü: toplam <br/> Boyut: EntityName|
|Giden bayt (Önizleme)|Bayt sayısı, belirtilen bir süredeki Azure Event Hubs hizmetinden alınır.<br/><br/> Birim: bayt <br/> Toplama türü: toplam <br/> Boyut: EntityName|

## <a name="connection-metrics"></a>Bağlantı ölçümü

| Ölçüm Adı | Açıklama |
| ------------------- | ----------------- |
|ActiveConnections (Önizleme)|Bir varlığın yanı sıra bir ad alanı etkin bağlantı sayısı.<br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName|
|Bağlantılar açık (Önizleme)|Açık bağlantıları sayısı.<br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName|
|Bağlantı kapalı (Önizleme)|Kapalı bağlantılarının sayısı.<br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName|

## <a name="event-hubs-capture-metrics"></a>Event Hubs yakalama ölçümleri

Yakalama özelliği, event hubs için etkinleştirdiğinizde, Event Hubs yakalama ölçümleri izleyebilirsiniz. Aşağıdaki ölçümler etkin yakalama ile izleyebilirsiniz açıklanmaktadır.

| Ölçüm Adı | Açıklama |
| ------------------- | ----------------- |
|Kapsam (Önizleme) yakalayın|Seçtiğiniz hedefe Yakalanacak henüz olan bayt sayısı.<br/><br/> Birim: bayt <br/> Toplama türü: toplam <br/> Boyut: EntityName|
|Yakalanan iletiler (Önizleme)|Seçtiğiniz hedefe, belirtilen bir süredeki yakalanır olay sayısı.<br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName|
|Yakalanan Baytlar (Önizleme)|Seçilen hedef için belirtilen bir süredeki yakalanan bayt sayısı.<br/><br/> Birim: bayt <br/> Toplama türü: toplam <br/> Boyut: EntityName|

## <a name="metrics-dimensions"></a>Ölçümleri boyutları

Azure Event Hubs, Azure İzleyicisi'nde ölçümler için aşağıdaki boyutlarını destekler. Boyutları için ölçümlerinizi eklemek isteğe bağlıdır. Ölçümleri, boyutları eklemezseniz, ad alanı düzeyinde belirtilir. 

| Ölçüm Adı | Açıklama |
| ------------------- | ----------------- |
|EntityName| Event Hubs ad alanı altında olay hub'ı varlıkları destekler.|

## <a name="next-steps"></a>Sonraki adımlar

* Bkz: [Azure izlemeye genel bakış](../monitoring-and-diagnostics/monitoring-overview.md).
* [.NET ile Azure İzleyici ölçümleri alma](https://github.com/Azure-Samples/monitor-dotnet-metrics-api) GitHub üzerinde örnek. 

Event Hubs hakkında daha fazla bilgi için şu bağlantıları ziyaret edin:

* [Event Hubs öğreticisi](event-hubs-dotnet-standard-getstarted-send.md) ile çalışmaya başlayın
* [Event Hubs ile ilgili SSS](event-hubs-faq.md)
* [Event Hubs kullanan örnek uygulamalar](https://github.com/Azure/azure-event-hubs/tree/master/samples)

[1]: ./media/event-hubs-metrics-azure-monitor/event-hubs-monitor1.png
[2]: ./media/event-hubs-metrics-azure-monitor/event-hubs-monitor2.png



