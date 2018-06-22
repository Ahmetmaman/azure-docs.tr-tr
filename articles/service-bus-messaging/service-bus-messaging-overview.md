---
title: Azure Service Bus mesajlaşmasına genel bakış | Microsoft Belgeleri
description: Service Bus mesajlaşmasının açıklaması
services: service-bus-messaging
documentationcenter: ''
author: sethmanheim
manager: timlt
editor: ''
ms.service: service-bus-messaging
ms.topic: overview
ms.date: 05/22/2018
ms.custom: mvc
ms.author: sethm
ms.openlocfilehash: 0357602e6085b25fc6d11363113ebc962dc4d008
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34643099"
---
# <a name="what-is-azure-service-bus"></a>Azure Service Bus nedir?

Microsoft Azure Service Bus tam olarak yönetilen bir kurumsal tümleştirme ileti aracısıdır. Service Bus, en sık olarak uygulamaları ve hizmetleri birbirinden ayırmak için kullanılır ve zaman uyumsuz veri ve durum aktarımı için güvenilir ve güvenli bir platformdur. Veriler, farklı uygulamalar ve hizmetler arasında *iletiler* kullanılarak aktarılır. İletiler JSON, XML veya yalnızca metin içerebilen ikili dosya biçimindedir. 

Yaygın olarak kullanılan bazı mesajlaşma senaryoları:

* Mesajlaşma: satış veya sipariş emirleri, günlük ya da envanter hareketleri gibi iş verileri aktarma.
* Uygulamaları ayırma: uygulama ve hizmetlerin güvenilirliğini ve ölçeklenebilirliğini artırma (istemci ve hizmetin aynı anda çevrimiçi olması gerekmez).
* Konu başlıkları ve abonelikler: yayımcılar ve aboneler arasında 1:*n* ilişkileri etkinleştirme.
* İleti oturumları: ileti sıralama veya ileti erteleme gerektiren iş akışlarını uygulama.

## <a name="namespaces"></a>Ad Alanları

Ad alanı, tüm mesajlaşma bileşenlerini kapsayan bir kapsayıcıdır. Tek bir ad alanında birden fazla kuyruk ve konu bulunabilir ve ad alanları genellikle uygulama kapsayıcıları olarak görev yapar.

## <a name="queues"></a>Kuyruklar

İletiler *kuyruklara* gönderilir ve bunlardan alınır. Kuyruklar, alacak uygulama almaya ve işlemeye uygun olana kadar iletileri depolamanızı sağlar.

![Kuyruk](./media/service-bus-messaging-overview/about-service-bus-queue.png)

Kuyruklardaki iletiler geldiklerinde sıralanır ve zaman damgası alır. Kabul edildikten sonra ileti yedekli depolama alanında güvenli bir şekilde tutulur. İletiler, bunları istek üzerine teslim eden *çekme* modunda teslim edilir.

## <a name="topics"></a>Konu başlıkları

İleti göndermek ve almak için *konu başlıklarını* da kullanabilirsiniz. Bir kuyruk genellikle noktadan noktaya iletişim için kullanılır, buna karşın konu başlıkları yayımlama/abone olma senaryolarında yararlıdır.

![Konu](./media/service-bus-messaging-overview/about-service-bus-topic.png)

Konuların birden fazla ve bağımsız abonesi olabilir. Bir konu başlığının bir abonesi konu başlığına gönderilen her iletinin bir kopyasını alabilir. Abonelikler, kalıcı olarak oluşturulan ancak istenirse süresi dolabilecek veya silinebilecek adlandırılmış varlıklardır.

Bazı senaryolarda bireysel aboneliklerin bir konu başlığına gönderilen tüm iletileri almasını istemeyebilirsiniz. Böyle durumlarda, isteğe bağlı [eylemler](topic-filters.md#actions) tetikleyen, belirli iletileri filtreleyen ve ileti özelliklerini ayarlayan veya değiştiren koşullar tanımlamak için [kurallar ve filtreler](topic-filters.md) kullanabilirsiniz.

## <a name="advanced-features"></a>Gelişmiş özellikler

Service Bus ayrıca daha karmaşık mesajlaşma sorunlarını çözmenizi sağlayan gelişmiş özelliklere de sahiptir. Aşağıdaki bölümlerde bu başlıca özellikler açıklanmaktadır:

### <a name="message-sessions"></a>İleti oturumları

Service Bus'ta ilk giren ilk çıkar ilkesinin (FIFO) kullanılmasını garantilemek için oturumlar kullanın. [İleti oturumları](message-sessions.md), sınırsız sayıda birbiriyle ilgili iletinin birlikte ve düzenli olarak işlenmesini sağlar. 

### <a name="auto-forwarding"></a>Otomatik iletme

[Otomatik iletme](service-bus-auto-forwarding.md) özelliği, bir kuyruk veya aboneliği aynı ad alanında olan başka bir kuyruk veya konu başlığına zincirleme eklemenize olanak tanır. Otomatik iletme etkinleştirildiğinde, Service Bus, ilk kuyruğa veya aboneliğe (kaynak) yerleştirilen iletileri otomatik olarak kaldırıp ikinci kuyruğa veya aboneliğe (hedef) yerleştirir.

### <a name="dead-lettering"></a>Ulaşmayan posta

Service Bus, herhangi bir alıcıya teslim edilemeyen veya işlenemeyen iletileri tutmak için bir [ulaşmayan posta kuyruğunu](service-bus-dead-letter-queues.md) (DLQ) destekler. İletileri daha sonra DLQ'dan kaldırabilir ve inceleyebilirsiniz.

### <a name="scheduled-delivery"></a>Zamanlanmış teslim

Bir kuyruğa veya konu başlığına [daha sonra işlenmek](message-sequencing.md#scheduled-messages); örneğin belirli bir işi belirli bir zamanda sistem tarafından işlenmeye uygun hale gelmesi için zamanlamak üzere ileti gönderebilirsiniz.

### <a name="message-deferral"></a>İleti erteleme

Bir kuyruk veya abonelik istemcisi işlemeye hazır olduğu ancak uygulamadaki özel koşullar nedeniyle o anda işlenmesinin mümkün olmadığı bir ileti aldığında, [iletinin alınmasını (ilerideki bir zamana) erteleme](message-deferral.md) seçeneğine sahiptir. İleti sıra veya abonelikte kalır ancak bir kenara ayrılır.

### <a name="batching"></a>Toplu İşleme

[İstemci tarafında toplu iş](service-bus-performance-improvements.md#client-side-batching), bir kuyruk veya konu başlığı istemcisinin bir iletiyi göndermeyi belirli bir süre için ertelemesini sağlar. İstemci bu süre içinde başka iletiler gönderirse, iletileri tek bir toplu iş olarak gönderir. 

### <a name="transactions"></a>İşlemler

Bir [hareket](service-bus-transactions.md), iki veya daha fazla işlemi tek bir yürütme kapsamında bir araya toplar. Service Bus, bir hareketin kapsamı içindeki işlemlerin (kuyruk, konu başlığı, abonelik gibi) tek bir mesajlaşma varlığına göre gruplanmasını destekler.

### <a name="filtering-and-actions"></a>Filtreleme ve eylemler

Aboneler, bir konu başlığından hangi iletileri almak istediklerini tanımlayabilir. Bu iletiler, bir veya daha fazla [adlandırılmış abonelik kuralı](topic-filters.md) biçiminde belirtilir. Eşleşen her kural koşulu için abonelik, iletinin, her eşleşme kuralı için farklı şekilde açıklama eklenebilecek bir kopyasını üretir.

### <a name="auto-delete-on-idle"></a>Boşta beklemede otomatik silme

[Boşta beklemede otomatik silme](/dotnet/api/microsoft.servicebus.messaging.queuedescription.autodeleteonidle), bittiğinde kuyruğun otomatik olarak silindiği bir boşta bekleme süresi belirtmenizi sağlar. En düşük süre 5 dakikadır.

### <a name="duplicate-detection"></a>Yineleme algılama

İstemcinin bir gönderme işleminin sonucu konusunda kuşkulanmasına neden olacak bir hata oluşursa, [yineleme algılama](duplicate-detection.md) kuralı, gönderenin aynı iletiyi yeniden göndermesini ve kuyruk ya da konu başlığının yinelenen kopyaları atmasını sağlayarak bu kuşkuyu giderir.

### <a name="sas-rbac-and-msi"></a>SAS, RBAC ve MSI

Service Bus, [Paylaşılan Erişim İmzaları](service-bus-sas.md) (SAS), [Rol Tabanlı Erişim Denetimi](service-bus-role-based-access-control.md) (RBAC) ve [Yönetilen Hizmet Kimliği](service-bus-managed-service-identity.md) (MSI) gibi güvenlik protokollerini destekler.

### <a name="geo-disaster-recovery"></a>Coğrafi olağanüstü durum kurtarma

Azure bölgeleri veya veri merkezleri bir kesinti yaşadığında, [Coğrafi olağanüstü durum kurtarma](service-bus-geo-dr.md) veri işlemenin başka bir bölge veya veri merkezinde devam etmesini olanaklı kılar.

### <a name="security"></a>Güvenlik

Service Bus, standart [AMQP 1.0](service-bus-amqp-overview.md) ve [HTTP/REST](/rest/api/servicebus/) protokollerini destekler.

## <a name="client-libraries"></a>İstemci kitaplıkları

Service Bus, [.NET](https://github.com/Azure/azure-service-bus-dotnet/tree/master), [Java](https://github.com/Azure/azure-service-bus-java/tree/master) ve [JMS](https://github.com/Azure/azure-service-bus/tree/master/samples/Java/qpid-jms-client) istemci kitaplıklarını destekler.

## <a name="integration"></a>Tümleştirme

Service Bus, aşağıdaki Azure hizmetleriyle tam olarak tümleşiktir:

- [Event Grid](https://azure.microsoft.com/services/event-grid/) 
- [Logic Apps](https://azure.microsoft.com/services/logic-apps/) 
- [İşlevler](https://azure.microsoft.com/services/functions/) 
- [Dynamics 365](https://dynamics.microsoft.com)
- [Akış Analizi](https://azure.microsoft.com/services/stream-analytics/)
 
## <a name="next-steps"></a>Sonraki adımlar

Service Bus mesajlaşmasını kullanmaya başlamak için aşağıdaki makalelere bakın:

* [Azure mesajlaşma hizmetlerini karşılaştırma](../event-grid/compare-messaging-services.md?toc=%2fazure%2fservice-bus-messaging%2ftoc.json&bc=%2fazure%2fservice-bus-messaging%2fbreadcrumb%2ftoc.json)
* Azure Service Bus [Standart ve Premium](https://azure.microsoft.com/pricing/details/service-bus/) katmanları ve bunların fiyatlandırması hakkında daha fazla bilgi edinin
* [Azure Service Bus Premium katmanının performansı ve gecikme süresi](https://blogs.msdn.microsoft.com/servicebus/2016/07/18/premium-messaging-how-fast-is-it/)
* [.NET](service-bus-quickstart-powershell.md), [Java](service-bus-quickstart-powershell.md) ve [JMS](service-bus-quickstart-powershell.md) hızlı başlangıçlarını deneyin
