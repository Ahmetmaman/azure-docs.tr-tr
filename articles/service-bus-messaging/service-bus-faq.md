---
title: Azure Service Bus sık sorulan sorular (SSS) | Microsoft Docs
description: Bu makalede Azure Service Bus hakkında sık sorulan soruların (SSS) bazılarına yanıtlar verilmektedir.
ms.topic: article
ms.date: 09/16/2020
ms.openlocfilehash: acd741101928f5a2dfd72eab1598af6e4556a3d1
ms.sourcegitcommit: 1bf144dc5d7c496c4abeb95fc2f473cfa0bbed43
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/24/2020
ms.locfileid: "96022157"
---
# <a name="azure-service-bus---frequently-asked-questions-faq"></a>Azure Service Bus-sık sorulan sorular (SSS)

Bu makalede Microsoft Azure Service Bus hakkında sık sorulan bazı sorular açıklanmaktadır. Ayrıca, genel Azure fiyatlandırması ve destek bilgileri için [Azure desteği SSS](https://azure.microsoft.com/support/faq/) ' ni de ziyaret edebilirsiniz.


## <a name="general-questions-about-azure-service-bus"></a>Azure Service Bus ilgili genel sorular
### <a name="what-is-azure-service-bus"></a>Azure Service Bus nedir?
[Azure Service Bus](service-bus-messaging-overview.md) , ayrılmış sistemler arasında veri göndermenizi sağlayan zaman uyumsuz mesajlaşma bulutu platformudur. Microsoft bu özelliği bir hizmet olarak sunar. Bu, bunu kullanmak için kendi donanımınızı barındırmanıza gerek kalmaz anlamına gelir.

### <a name="what-is-a-service-bus-namespace"></a>Service Bus ad alanı nedir?
[Ad alanı](service-bus-create-namespace-portal.md) , uygulamanızdaki Service Bus kaynakları adresleme için bir kapsam kapsayıcısı sağlar. Service Bus kullanmak için ad alanı oluşturma gereklidir ve Başlarken ilk adımlardan biridir.

### <a name="what-is-an-azure-service-bus-queue"></a>Azure Service Bus kuyruğu nedir?
[Service Bus kuyruğu](service-bus-queues-topics-subscriptions.md) , iletilerin depolandığı bir varlıktır. Kuyruklar, birden çok uygulamanız veya birbirleriyle iletişim kurması gereken dağıtılmış bir uygulamanın birden çok bölümü olduğunda faydalıdır. Sıra, birden çok ürüne (ileti) alınmış ve ardından bu konumdan gönderildiği bir dağıtım merkeziyle benzerdir.

### <a name="what-are-azure-service-bus-topics-and-subscriptions"></a>Azure Service Bus konuları ve abonelikleri nelerdir?
Bir konu bir sıra olarak görselleştirilir ve birden çok abonelik kullanıldığında, daha zengin bir mesajlaşma modeli olur; esas olarak bire çok iletişim aracı. Bu yayımla/abone ol modeli (veya *pub/Sub*), birden fazla uygulama tarafından alınan bir konuya bir ileti gönderen bir uygulamayı, bu iletinin birden çok uygulama tarafından alınmasını sağlar.

### <a name="what-is-a-partitioned-entity"></a>Bölümlenmiş varlık nedir?
Geleneksel bir kuyruk veya konu, tek bir ileti Aracısı tarafından işlenir ve tek bir mesajlaşma deposunda depolanır. Yalnızca temel ve standart mesajlaşma katmanlarında desteklenir, [bölümlenmiş bir kuyruk veya konu](service-bus-partitioning.md) , birden çok ileti aracılarıyla işlenir ve birden çok mesajlaşma mağazasında saklanır. Bu özellik, bölümlenmiş bir sıranın veya konunun genel üretilen işinin artık tek bir ileti aracısının veya mesajlaşma deposunun performansıyla sınırlı olmadığı anlamına gelir. Ayrıca, bir mesajlaşma deposunun geçici kesilmesi bölümlenmiş bir kuyruğu veya konuyu işlemez.

Bölümlenmiş varlıklar kullanılırken sıralama değildir. Bir bölümün kullanılamaz olması durumunda diğer bölümlerden ileti almaya ve almaya devam edebilirsiniz.

 Bölümlenmiş varlıklar artık [Premium SKU 'sunda](service-bus-premium-messaging.md)desteklenmez. 

### <a name="where-does-azure-service-bus-store-customer-data"></a><a name="in-region-data-residency"></a>Azure Service Bus müşteri verilerini nerede depolar?
Azure Service Bus müşteri verilerini depolar. Bu veriler tek bir bölgede Service Bus tarafından otomatik olarak depolanır. bu nedenle bu hizmet, [Güven Merkezi](https://azuredatacentermap.azurewebsites.net/)'nde belirtilenler de dahil olmak üzere bölge verileri 'nin gereksinimlerini otomatik olarak karşılar.

### <a name="what-ports-do-i-need-to-open-on-the-firewall"></a>Güvenlik duvarında hangi bağlantı noktalarını açmalıyım? 
Aşağıdaki protokolleri, ileti göndermek ve almak için Azure Service Bus ile birlikte kullanabilirsiniz:

- Gelişmiş İleti Sıraya Alma Protokolü 1,0 (AMQP)
- TLS ile Köprü Metni Aktarım Protokolü 1,1 (HTTPS)

Azure Service Bus ile iletişim kurmak için bu protokolleri kullanmak üzere açmanız gereken giden TCP bağlantı noktaları için aşağıdaki tabloya bakın:

| Protokol | Bağlantı noktası | Ayrıntılar | 
| -------- | ----- | ------- | 
| AMQP | 5671 | TLS ile AMQP. Bkz. [AMQP protokol Kılavuzu](service-bus-amqp-protocol-guide.md) | 
| HTTPS | 443 | Bu bağlantı noktası HTTP/REST API ve AMQP-Over-WebSockets için kullanılır |

HTTPS bağlantı noktası, istemci SDK 'Ları tarafından gerçekleştirilen çeşitli yönetim işlemleri ve Azure Active Directory (kullanıldığında) ' dan belirteçleri alma HTTPS üzerinden çalıştırıldığında, genellikle giden iletişim için bağlantı noktası 5671 üzerinden de kullanılır. 

Resmi Azure SDK 'Ları genellikle Service Bus ileti göndermek ve almak için AMQP protokolünü kullanır. 

[!INCLUDE [service-bus-websockets-options](../../includes/service-bus-websockets-options.md)]

.NET Framework için eski WindowsAzure. ServiceBus paketi, "NetMessaging" olarak da adlandırılan eski "Service Bus mesajlaşma protokolünü" (SBMP) kullanma seçeneğine sahiptir. Bu protokol 9350-9354 TCP bağlantı noktalarını kullanır. Bu paketin varsayılan modu, bu bağlantı noktalarının iletişim için kullanılabilir olup olmadığını otomatik olarak algılamadır ve bu durum söz konusu değilse 443 bağlantı noktası üzerinden TLS ile WebSockets ' ye geçiş yapar. Bu ayarı geçersiz kılabilir ve `Https` uygulama için global olarak uygulanan ayarda [connectivitymode](/dotnet/api/microsoft.servicebus.connectivitymode?view=azure-dotnet) ayarını yaparak bu modu zorlayabilirsiniz [`ServiceBusEnvironment.SystemConnectivity`](/dotnet/api/microsoft.servicebus.servicebusenvironment.systemconnectivity?view=azure-dotnet) .

### <a name="what-ip-addresses-do-i-need-to-add-to-allow-list"></a>İzin verilenler listesine hangi IP adreslerini eklemem gerekir?
Bağlantılarınızın izin verilenler listesine eklenecek doğru IP adreslerini bulmak için şu adımları izleyin:

1. Komut isteminden aşağıdaki komutu çalıştırın: 

    ```
    nslookup <YourNamespaceName>.servicebus.windows.net
    ```
2. ' De döndürülen IP adresini aklınızda edin `Non-authoritative answer` . 

Ad alanınız için **bölge yedekliliği** kullanırsanız, birkaç ek adım yapmanız gerekir: 

1. İlk olarak, ad alanında Nslookup ' ı çalıştırırsınız.

    ```
    nslookup <yournamespace>.servicebus.windows.net
    ```
2. **Yetkili olmayan yanıt** bölümündeki adı aşağıdaki biçimlerden birinde olan bir yere göz önünde edin: 

    ```
    <name>-s1.cloudapp.net
    <name>-s2.cloudapp.net
    <name>-s3.cloudapp.net
    ```
3. Her biri için S1, S2 ve S3 sonekleri ile her biri için Nslookup ' ı çalıştırarak üç kullanılabilirlik alanında çalışan üç örnek için IP adreslerini alın. 

    > [!NOTE]
    > Komut tarafından döndürülen IP adresi `nslookup` statik BIR IP adresi değil. Ancak, temeldeki dağıtım silinene veya farklı bir kümeye taşınana kadar sabit kalır.

### <a name="where-can-i-find-the-ip-address-of-the-client-sendingreceiving-messages-tofrom-a-namespace"></a>Bir ad alanına/bir ad alanından gönderilen/alınan iletilerin IP adresini nerede bulabilirim? 
Ad alanınızı/onlardan ileti gönderen veya alan istemcilerin IP adreslerini günlüğe almadık. Yalnızca izin verilen kullanıcıların veya uygulamaların ad alanına erişebildiğinden emin olmak için mevcut tüm istemcilerin kimlik doğrulaması yapamaması ve [Azure rol tabanlı erişim denetimi (Azure RBAC)](authenticate-application.md#azure-built-in-roles-for-azure-service-bus)ayarlarını gözden geçirebilmesi için anahtarları yeniden oluşturun. 

**Premium** ad alanı kullanıyorsanız, ad alanına erişimi sınırlandırmak için [IP filtrelemesi](service-bus-ip-filtering.md), [sanal ağ hizmeti uç noktaları](service-bus-service-endpoints.md)ve [Özel uç noktalar](private-link-service.md) kullanın. 

## <a name="best-practices"></a>En iyi uygulamalar
### <a name="what-are-some-azure-service-bus-best-practices"></a>Azure Service Bus en iyi uygulamalar nelerdir?
[Service Bus kullanarak performans iyileştirmeleri Için en iyi uygulamalar][Best practices for performance improvements using Service Bus] bölümüne bakın – bu makalede ileti alışverişi sırasında performansın nasıl iyileştirileceği açıklanır.

### <a name="what-should-i-know-before-creating-entities"></a>Varlık oluşturmadan önce neleri bilmem gerekir?
Bir kuyruğun ve konunun aşağıdaki özellikleri sabittir. Bu özellikleri yeni bir değiştirme varlığı oluşturmadan değiştirilemediği için, varlıklarınızı sağladığınızda Bu sınırlamayı göz önünde bulundurun.

* Bölümleme
* Oturumlar
* Yineleme algılama
* Hızlı varlık

## <a name="pricing"></a>Fiyatlandırma
Bu bölümde Service Bus Fiyatlandırma yapısı hakkında sık sorulan bazı sorular yanıtlanmaktadır.

[Service Bus fiyatlandırma ve faturalandırma](https://azure.microsoft.com/pricing/details/service-bus/) makalesinde, Service Bus faturalama ölçümleri açıklanmaktadır. Service Bus fiyatlandırma seçenekleri hakkında belirli bilgiler için bkz. [Service Bus fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/service-bus/).

Genel Azure fiyatlandırma bilgileri için [Azure desteği SSS](https://azure.microsoft.com/support/faq/) ' ni de ziyaret edebilirsiniz. 

### <a name="how-do-you-charge-for-service-bus"></a>Service Bus için ücret ödersiniz?
Service Bus fiyatlandırması hakkında tüm bilgiler için bkz. [Service Bus fiyatlandırma ayrıntıları][Pricing overview]. Belirtilen fiyatlara ek olarak, uygulamanızın sağlandığı veri merkezinin dışında çıkış için ilişkili veri aktarımları için ücretlendirilirsiniz.

### <a name="what-usage-of-service-bus-is-subject-to-data-transfer-what-isnt"></a>Hangi Service Bus kullanımı veri aktarımına tabidir? Ne değildir?
Belirli bir Azure bölgesindeki tüm veri aktarımlarında, tüm gelen veri aktarımlarında da ücret alınmaz. Bölge dışında veri aktarımı, [burada](https://azure.microsoft.com/pricing/details/bandwidth/)bulunan çıkış ücretlerine tabidir.

### <a name="does-service-bus-charge-for-storage"></a>Service Bus depolama için ücretlendirilir mi?
Hayır. Service Bus depolama için ücret alınmaz. Ancak, kuyruk/Konu başına kalıcı olabilecek maksimum veri miktarını sınırlayan bir kota vardır. Sonraki SSS bölümüne bakın.

### <a name="i-have-a-service-bus-standard-namespace-why-do-i-see-charges-under-resource-group-system"></a>Service Bus standart bir ad alanı var. ' $System ' kaynak grubu altında ücretleri neden görüyorum?
Azure Service Bus son zamanlarda faturalandırma bileşenleri yükseltildi. Bu değişiklik nedeniyle, Service Bus standart bir ad alanınız varsa, ' $system ' kaynak grubu altında '/Subscriptions/<azure_subscription_id>/resourceGroups/$system/providers/Microsoft.ServiceBus/namespaces/$system ' kaynağı için satır öğeleri görebilirsiniz.

Bu ücretler, Azure aboneliği başına Service Bus standart ad alanı sağlayan temel ücreti temsil eder. 

Bu ücretlerin yeni değil, diğer bir deyişle önceki faturalandırma modelinde var olduğunu unutmayın. Tek değişiklik, artık ' $system ' altında listelenmektedir. Yeni faturalandırma sisteminde, belirli bir kaynağa bağlı olmayan, ' $system ' kaynak KIMLIĞI altında, abonelik düzeyi ücretleri gruplandıran kısıtlamalar nedeniyle bu işlem yapılır.

## <a name="quotas"></a>Kotalar

Service Bus limitlerin ve kotaların listesi için bkz. [Service Bus kotalara genel bakış][Quotas overview].

### <a name="how-to-handle-messages-of-size--1-mb"></a>> 1 MB boyutundaki iletiler nasıl ele alınacağını?
Service Bus mesajlaşma hizmetleri (kuyruklar ve konular/abonelikler), uygulamanın 256 KB 'ye (Standart katmana) veya 1 MB 'a (Premium katman) kadar mesaj göndermesini sağlar. 1 MB 'den büyük iletilerle uğraşıyorsanız, [Bu blog gönderisine](https://www.serverless360.com/blog/deal-with-large-service-bus-messages-using-claim-check-pattern)göre açıklanan talep denetimi modelini kullanın.

## <a name="troubleshooting"></a>Sorun giderme
### <a name="why-am-i-not-able-to-create-a-namespace-after-deleting-it-from-another-subscription"></a>Neden bir ad alanını başka bir abonelikten sildikten sonra oluşturamadım? 
Bir aboneliğden bir ad alanını sildiğinizde, başka bir abonelikte aynı adla yeniden oluşturmadan önce 4 saat bekleyin. Aksi takdirde, aşağıdaki hata iletisini alabilirsiniz: `Namespace already exists` . 

### <a name="what-are-some-of-the-exceptions-generated-by-azure-service-bus-apis-and-their-suggested-actions"></a>Azure Service Bus API 'Leri ve önerilen eylemleri tarafından oluşturulan özel durumlar nelerdir?
Olası Service Bus özel durumların listesi için bkz. [özel durumlara genel bakış][Exceptions overview].

### <a name="what-is-a-shared-access-signature-and-which-languages-support-generating-a-signature"></a>Paylaşılan erişim Imzası nedir ve hangi dillerin imza oluşturulmasını destekler?
Paylaşılan erişim Imzaları, SHA-256 güvenli karmaları veya URI 'Leri temel alan bir kimlik doğrulama mekanizmasıdır. Node.js, PHP, Java, Python ve C# ' de kendi imzalarınızı oluşturma hakkında daha fazla bilgi için bkz. [paylaşılan erişim imzaları][Shared Access Signatures] makalesi.

## <a name="subscription-and-namespace-management"></a>Abonelik ve ad alanı yönetimi
### <a name="how-do-i-migrate-a-namespace-to-another-azure-subscription"></a>Bir ad alanını başka bir Azure aboneliğine geçirmek Nasıl yaparım? mı?

[Azure Portal](https://portal.azure.com) veya PowerShell komutlarını kullanarak bir Azure aboneliğinden diğerine bir ad alanı taşıyabilirsiniz. İşlemi yürütmek için ad alanı zaten etkin olmalıdır. Komutları yürüten kullanıcının hem kaynak hem de hedef aboneliklerde yönetici olması gerekir.

#### <a name="portal"></a>Portal

Service Bus ad alanlarını başka bir aboneliğe geçirmek üzere Azure portal kullanmak için [buradaki](../azure-resource-manager/management/move-resource-group-and-subscription.md#use-the-portal)yönergeleri izleyin. 

#### <a name="powershell"></a>PowerShell

Aşağıdaki PowerShell komutları dizisi bir ad alanını bir Azure aboneliğinden diğerine taşılardır. Bu işlemi yürütmek için, ad alanı zaten etkin olmalıdır ve PowerShell komutlarını çalıştıran kullanıcının hem kaynak hem de hedef aboneliklerde yönetici olması gerekir.

```powershell
# Create a new resource group in target subscription
Select-AzSubscription -SubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff'
New-AzResourceGroup -Name 'targetRG' -Location 'East US'

# Move namespace from source subscription to target subscription
Select-AzSubscription -SubscriptionId 'aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa'
$res = Find-AzResource -ResourceNameContains mynamespace -ResourceType 'Microsoft.ServiceBus/namespaces'
Move-AzResource -DestinationResourceGroupName 'targetRG' -DestinationSubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff' -ResourceId $res.ResourceId
```

## <a name="next-steps"></a>Sonraki adımlar
Service Bus hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [Azure Service Bus Premium 'A giriş (blog gönderisi)](https://azure.microsoft.com/blog/introducing-azure-service-bus-premium-messaging/)
* [Azure Service Bus Premium 'A giriş (Channel9)](https://channel9.msdn.com/Blogs/Subscribe/Introducing-Azure-Service-Bus-Premium-Messaging)
* [Service Bus genel bakış](service-bus-messaging-overview.md)
* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)

[Best practices for performance improvements using Service Bus]: service-bus-performance-improvements.md
[Best practices for insulating applications against Service Bus outages and disasters]: service-bus-outages-disasters.md
[Pricing overview]: https://azure.microsoft.com/pricing/details/service-bus/
[Quotas overview]: service-bus-quotas.md
[Exceptions overview]: service-bus-messaging-exceptions.md
[Shared Access Signatures]: service-bus-sas.md
