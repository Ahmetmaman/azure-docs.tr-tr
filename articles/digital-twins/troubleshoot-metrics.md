---
title: Azure Izleyici ile ölçümleri görüntüleme
titleSuffix: Azure Digital Twins
description: Bkz. Azure Izleyici 'de Azure dijital TWINS ölçümlerini görüntüleme.
author: baanders
ms.author: baanders
ms.date: 8/4/2020
ms.topic: troubleshooting
ms.service: digital-twins
ms.openlocfilehash: db4383526874c98f717c5f166b296d814724184a
ms.sourcegitcommit: 2e72661f4853cd42bb4f0b2ded4271b22dc10a52
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/14/2020
ms.locfileid: "92044644"
---
# <a name="troubleshooting-azure-digital-twins-metrics"></a>Azure dijital TWINS sorunlarını giderme: ölçümler

Bu makalede açıklanan ölçümler, Azure aboneliğinizdeki Azure dijital TWINS kaynaklarının durumu hakkında bilgi verir. Azure dijital TWINS ölçümleri, Azure dijital TWINS hizmetinin genel sistem durumunu ve ona bağlı kaynakları değerlendirmenize yardımcı olur. Bu kullanıcıya yönelik istatistikler, Azure dijital Tağınızda neler olduğunu görmenizi ve Azure desteği ile iletişim kurmaya gerek kalmadan sorunlar için kök neden analizi gerçekleştirmeye yardımcı olur.

Ölçümler varsayılan olarak etkindir. [Azure Portal](https://portal.azure.com)Azure dijital TWINS ölçümlerini görüntüleyebilirsiniz.

## <a name="how-to-view-azure-digital-twins-metrics"></a>Azure dijital TWINS ölçümlerini görüntüleme

1. Azure dijital TWINS örneği oluşturun. [*Nasıl yapılır: bir örneği ve kimlik doğrulamasını ayarlama*](how-to-set-up-instance-portal.md)konusunda bir Azure dijital TWINS örneği ayarlama hakkında yönergeler bulabilirsiniz.

2. [Azure Portal](https://portal.azure.com) Azure dijital TWINS örneğinizi bulun (adını Portal arama çubuğuna yazarak bu sayfayı açabilirsiniz). 

    Örneğin menüsünden **ölçümler**' i seçin.
   
    :::image type="content" source="media/troubleshoot-metrics/azure-digital-twins-metrics.png" alt-text="Azure dijital TWINS için ölçüm sayfasını gösteren ekran görüntüsü":::

    Bu sayfa, Azure dijital TWINS örneğiniz için ölçümleri görüntüler. Ayrıca, listeden görmek istediklerinizi seçerek ölçümlerinizin özel görünümlerini de oluşturabilirsiniz.
    
3. Menüdeki **Tanılama ayarları** ' nı ve ardından **Tanılama ayarı Ekle**' yi seçerek ölçüm verilerinizi bir Event Hubs uç noktasına veya bir Azure depolama hesabına gönderebilirsiniz.

    :::image type="content" source="media/troubleshoot-diagnostics/diagnostic-settings.png" alt-text="Azure dijital TWINS için ölçüm sayfasını gösteren ekran görüntüsü":::

    Bu işlem hakkında daha fazla bilgi için bkz. [*sorun giderme: tanılamayı ayarlama*](troubleshoot-diagnostics.md).

4. Menüden **Uyarılar** ' ı ve sonra **+ Yeni uyarı kuralı**' nı seçerek ölçüm verilerinize yönelik uyarıları ayarlamayı tercih edebilirsiniz.
    :::image type="content" source="media/troubleshoot-alerts/alerts-pre.png" alt-text="Azure dijital TWINS için ölçüm sayfasını gösteren ekran görüntüsü":::

    Bu işlem hakkında daha fazla bilgi için bkz. [*sorun giderme: uyarıları ayarlama*](troubleshoot-alerts.md).

## <a name="azure-digital-twins-metrics-and-how-to-use-them"></a>Azure dijital TWINS ölçümleri ve bunların nasıl kullanılacağı

Azure dijital TWINS, örneğinizin sistem durumuna ve ilişkili kaynaklarına ilişkin bir genel bakış sunmak için çeşitli ölçümler sunar. Ayrıca, örneğinizin durumunun büyük bir resmini boyamak için birden fazla ölçüden bilgi birleştirebilirsiniz. 

Aşağıdaki tablolarda, her bir Azure dijital TWINS örneği tarafından izlenen ölçümler ve her ölçümün örneğinizin genel durumuyla nasıl ilişkili olduğu açıklanır.

#### <a name="api-request-metrics"></a>API isteği ölçümleri

API istekleriyle yapılacak ölçümler:

| Ölçüm | Ölçüm görünen adı | Birim | Toplama türü| Açıklama | Boyutlar |
| --- | --- | --- | --- | --- | --- |
| ApiRequests | API Istekleri (Önizleme) | Sayı | Toplam | Dijital TWINS okuma, yazma, silme ve sorgu işlemleri için yapılan API Isteklerinin sayısı. |  Yetkilendirmesi <br>Çalışmasını <br>Protocol <br>Durum kodu, <br>Durum kodu sınıfı, <br>Durum metni |
| ApiRequestsFailureRate | API Isteği başarısızlık oranı (Önizleme) | Yüzde | Ortalama | Dijital TWINS okuma, yazma, silme ve sorgu işlemleri için bir iç hata (500) yanıt kodu veren örneğiniz için hizmetin aldığı API isteklerinin yüzdesi. | Yetkilendirmesi <br>Çalışmasını <br>Protocol <br>Durum kodu, <br>Durum kodu sınıfı, <br>Durum metni
| Apirequestslatlik | API Istekleri gecikmesi (Önizleme) | Mayacak | Ortalama | API istekleri için yanıt süresi. Bu, hizmet, dijital TWINS okuma, yazma, silme ve sorgu işlemleri için başarılı/başarısız sonucu gönderene kadar Azure Digital TWINS tarafından isteğin alındığı zamana başvurur. | Yetkilendirmesi <br>Çalışmasını <br>Protokol |

#### <a name="billing-metrics"></a>Faturalandırma ölçümleri

Faturalandırma ile yapılacak ölçümler:

>[!NOTE]
> Önizleme süresince **faturalandırma sıfır maliyetlidir**. Bu ölçümler seçilebilir listede görüntülenmeye devam ederken, önizleme sırasında uygulanmaz ve hizmet Önizleme dışına çıkana kadar sıfır olarak kalır.

| Ölçüm | Ölçüm görünen adı | Birim | Toplama türü| Açıklama | Boyutlar |
| --- | --- | --- | --- | --- | --- |
| BillingApiOperations | Faturalama API 'SI Işlemleri (Önizleme) | Sayı | Toplam | Azure Digital TWINS hizmetinde yapılan tüm API isteklerinin sayısı için faturalandırma ölçümü. | Ölçüm kimliği |
| Billingiletide Işlendi | Işlenen faturalama Iletileri (Önizleme) | Sayı | Toplam | Azure dijital TWINS 'den dış uç noktalara gönderilen ileti sayısı için faturalandırma ölçümü.<br><br>Faturalama amacıyla tek bir ileti olarak kabul edilebilmesi için yükün 1 KB 'den büyük olmaması gerekir. Bundan büyük yük, 1 KB 'lık artışlarla ek iletiler olarak sayılır (Bu nedenle 1 ile 2 KB arasında bir ileti 2 ileti olarak sayılır, 2 ile 3 KB arasında bir ileti 3 ileti olur ve bu şekilde devam eder).<br>Bu kısıtlama, yanıtlar için de geçerlidir. bu nedenle yanıt gövdesinde 1,5 KB döndüren bir çağrı, örneğin, 2 işlem olarak faturalandırılacaktır. | Ölçüm kimliği |
| BillingQueryUnits | Faturalama sorgu birimleri (Önizleme) | Sayı | Toplam | Sorgu birimlerinin sayısı, sorgu yürütmek için tüketilen, hizmet kaynağı kullanımının dahili olarak hesaplanan bir ölçümüdür. Sorgu birimlerini ölçmek için bir yardımcı API de mevcuttur: [Querychargehelper sınıfı](/dotnet/api/azure.digitaltwins.core.querychargehelper?preserve-view=true&view=azure-dotnet-preview) | Ölçüm kimliği |

#### <a name="ingress-metrics"></a>Giriş ölçümleri

Veri girişi ile ilgili ölçümler:

| Ölçüm | Ölçüm görünen adı | Birim | Toplama türü| Açıklama | Boyutlar |
| --- | --- | --- | --- | --- | --- |
| Inressevents | Giriş olayları (Önizleme) | Sayı | Toplam | Azure dijital TWINS 'e gelen telemetri olaylarının sayısı. | Sonuç |
| Inresyettsfailurerate | Giriş olayları hata oranı (Önizleme) | Yüzde | Ortalama | Hizmetin bir iç hata (500) yanıt kodu döndürdüğü gelen telemetri olaylarının yüzdesi. | Sonuç |
| Ingresseventsla | Giriş olayları gecikmesi (Önizleme) | Mayacak | Ortalama | Bir olayın Azure dijital TWINS tarafından yumurtada ayrılmaya hazırlandığında, hizmetin bir başarı/başarısızlık sonucu gönderdiği zaman. | Sonuç |

#### <a name="routing-metrics"></a>Yönlendirme ölçümleri

Yönlendirme ile yapmak zorunda olan ölçümler:

| Ölçüm | Ölçüm görünen adı | Birim | Toplama türü| Açıklama | Boyutlar |
| --- | --- | --- | --- | --- | --- |
| Iletime yönlendirildi | Yönlendirilen iletiler (Önizleme) | Sayı | Toplam | Olay Hub 'ı, Service Bus veya Event Grid gibi bir uç nokta Azure hizmetine yönlendirilen ileti sayısı. | Uç nokta türü, <br>Sonuç |
| RoutingFailureRate | Yönlendirme hatası oranı (Önizleme) | Yüzde | Ortalama | Azure dijital TWINS 'den, Olay Hub 'ı, Service Bus veya Event Grid gibi bir uç nokta Azure hizmetine yönlendirildikleri için hataya neden olan olayların yüzdesi. | Uç nokta türü, <br>Sonuç |
| RoutingLatency | Yönlendirme gecikmesi (Önizleme) | Mayacak | Ortalama | Azure dijital TWINS 'den, Olay Hub 'ı, Service Bus veya Event Grid gibi bir uç nokta Azure hizmetine gönderildiğinde bir olay arasında geçen süre. | Uç nokta türü, <br>Sonuç |

## <a name="dimensions"></a>Boyutlar

Boyutlar ölçümler hakkında daha fazla ayrıntı belirlemenize yardımcı olur. Yönlendirme ölçümlerinden bazıları uç nokta başına bilgi sağlar. Aşağıdaki tabloda bu boyutlar için olası değerler listelenmiştir.

| Boyut | Değerler |
| --- | --- |
| Kimlik Doğrulaması | OAuth |
| İşlem (API Istekleri için) | Microsoft. DigitalTwins/digitaltwıns/DELETE, <br>Microsoft. DigitalTwins/digitaltwıns/Write, <br>Microsoft. DigitalTwins/digitaltwıns/Read, <br>Microsoft. DigitalTwins/eventroutes/Read, <br>Microsoft. DigitalTwins/eventrotalar/Write, <br>Microsoft. DigitalTwins/eventrotalar/DELETE, <br>Microsoft. DigitalTwins/modeller/okuma, <br>Microsoft. DigitalTwins/modeller/Write, <br>Microsoft. DigitalTwins/modeller/silme, <br>Microsoft. DigitalTwins/sorgu/eylem |
| Uç nokta türü | Event Grid <br>Olay Hub 'ı, <br>Service Bus |
| Protokol | HTTPS |
| Sonuç | Başarılı <br>Hata |
| Durum Kodu | 200, 404, 500 vb. |
| Durum kodu sınıfı | 2xx, 4xx, 5xx vb. |
| Durum metni | İç sunucu hatası, bulunamadı, vb. |

## <a name="next-steps"></a>Sonraki adımlar

Azure dijital TWINS için kaydedilmiş ölçümleri yönetme hakkında daha fazla bilgi için bkz. [*sorun giderme: tanılamayı ayarlama*](troubleshoot-diagnostics.md).