---
title: Azure Service Bus yinelenen ileti algılama | Microsoft Docs
description: Service Bus yinelenen iletileri algılama
services: service-bus-messaging
documentationcenter: ''
author: axisc
manager: timlt
editor: spelluru
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2019
ms.author: aschhab
ms.openlocfilehash: c419ee1eec9e451cad835d8b4a56818101dc853a
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54856222"
---
# <a name="duplicate-detection"></a>Yineleme algılama

Uygulamanın şu nedenle başarısız olursa bir ileti hemen sonra önemli bir hata gönderir ve yeniden uygulama örneğini deneyebileceğinizi düşündüğü önceki ileti teslim oluşmadı olduğunu, sonraki gönderme aynı iletinin sistemde iki kez görünmesine neden olur.

Ayrıca biraz daha önce gerçekleşmesi için istemci veya ağ düzeyinde hata mümkündür ve bildirim ile kuyruğuna başarıyla tamamlanması gönderilen ileti için istemciye döndürülen. Bu senaryoda, istemci gönderme işleminin sonucu hakkında şüpheli bırakır.

Yinelenen algılama aynı iletiyi gönderen Gönder'i etkinleştirerek bu durumlar dışında şüpheli alır ve kuyruğun veya konunun tüm yinelenen kopyaları atar.

Yineleme algılamayı etkinleştirmek yardımcı olan uygulama tarafından denetlenen izlemek *MessageID* bir kuyruk veya konuda içinde belirtilen zaman penceresi boyunca gönderilen tüm iletilerin. İle herhangi bir yeni ileti gönderildiğinde *MessageID* , oturum zaman penceresi boyunca, iletiyi kabul olarak bildirildi (gönderme işlemi başarılı), ancak yeni gönderilen ileti anında yoksayıldı ve bırakıldı. İletinin dışında hiçbir diğer bölümlerini *MessageID* olarak kabul edilir.

Yalnızca bu bağlamanın verdiğinden tanımlayıcısının uygulama denetimi gerekli *MessageID* , bunu tahmin edilebilir bir biçimde reconstructed bir hata oluştuğunda bir iş işlemi bağlam.

İçinde birden çok ileti gönderilir bazı uygulama içeriği, işleme sırasında bir iş işlemi *MessageID* bileşik bir satınalma siparişi numarası gibi uygulama düzeyinde bağlam tanımlayıcısının olabilir ve ileti, örneğin, konu **12345.2017/ödeme**.

*MessageID* bazı GUID her zaman olabilir, ancak iş süreci tanımlayıcısına sabitleme, yinelenen algılama özelliği etkili bir şekilde yararlanmak için istenen tahmin edilebilir yinelenebilirliği verir.

## <a name="enable-duplicate-detection"></a>Yinelenen algılamayı etkinleştir

Portalda özelliği ile varlık oluşturma sırasında açık **yinelenen algılamayı etkinleştirme** onay kutusu, varsayılan olarak kapalıdır. Yeni konular oluşturmak için bu ayarı eşdeğerdir.

![][1]

> [!IMPORTANT]
> Etkinleştir/yinelenen algılama kuyruk oluşturulduktan sonra devre dışı bırakamaz. Yalnızca kuyruk oluşturma sırasında bunu yapabilirsiniz. 

Bayrağı ile programlı olarak ayarladığınız [QueueDescription.requiresDuplicateDetection](/dotnet/api/microsoft.servicebus.messaging.queuedescription.requiresduplicatedetection#Microsoft_ServiceBus_Messaging_QueueDescription_RequiresDuplicateDetection) tam Framework .NET API özelliği. Azure Resource Manager API'si ile ile değerin ayarlanması [queueProperties.requiresDuplicateDetection](/azure/templates/microsoft.servicebus/namespaces/queues#property-values) özelliği.

Yineleme algılama süresi geçmiş varsayılan olarak 30 saniyeye kuyruklar ve konularla yedi günde bir en yüksek değeri. Azure portalında kuyruk ve konu Özellikler penceresinde bu ayarı değiştirebilirsiniz.

![][2]

Kullanarak bu sırada iletisi kimlikleri korunur, yineleme algılama penceresi boyutunu programlı olarak yapılandırabilirsiniz [QueueDescription.DuplicateDetectionHistoryTimeWindow](/dotnet/api/microsoft.servicebus.messaging.queuedescription.duplicatedetectionhistorytimewindow#Microsoft_ServiceBus_Messaging_QueueDescription_DuplicateDetectionHistoryTimeWindow) tam .NET Framework API özelliği . Azure Resource Manager API'si ile ile değerin ayarlanması [queueProperties.duplicateDetectionHistoryTimeWindow](/azure/templates/microsoft.servicebus/namespaces/queues#property-values) özelliği.

Yeni gönderilen ileti tanımlayıcısı karşı eşlenmesi gereken tüm kayıtlı iletisi kimlikleri bu yana etkinleştirme yinelenen algılama ve pencere boyutunu doğrudan kuyruk (ve konu) aktarım hızını etkiler.

Daha az iletisi kimlikleri korunur ve eşleşmesi gerekir ve üretilen iş penceresi küçük anlamına gelir tutarak daha az etkilenmiş. Yinelenen algılama gerektiren yüksek aktarım hızı varlıklar için olabildiğince küçük pencereyi tutmalısınız.

## <a name="next-steps"></a>Sonraki adımlar

Service Bus mesajlaşması hakkında daha fazla bilgi edinmek için aşağıdaki konulara bakın:

* [Service Bus kuyrukları, konu başlıkları ve abonelikleri](service-bus-queues-topics-subscriptions.md)
* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus konu başlıklarını ve aboneliklerini kullanma](service-bus-dotnet-how-to-use-topics-subscriptions.md)

[1]: ./media/duplicate-detection/create-queue.png
[2]: ./media/duplicate-detection/queue-prop.png
