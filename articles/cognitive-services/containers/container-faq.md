---
title: Bilişsel hizmetler kapsayıcıları sık sorulan sorular (SSS)
titleSuffix: Azure Cognitive Services
description: Sık sorulan sorular ve yanıtlar.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: conceptual
ms.date: 08/31/2020
ms.author: aahi
ms.openlocfilehash: 3d35a1f6913d0b657956489d0e57836a05f9eb1d
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "90900046"
---
# <a name="azure-cognitive-services-containers-frequently-asked-questions-faq"></a>Azure bilişsel hizmetler kapsayıcıları sık sorulan sorular (SSS)

## <a name="general-questions"></a>Genel sorular

**S: kullanılabilir nedir?**

Y **:** Azure bilişsel hizmetler kapsayıcıları, geliştiricilerin Azure 'da kullanılabilen akıllı API 'Leri kullanmasına izin verir, ancak kapsayıcıların [avantajlarından](../cognitive-services-container-support.md#features-and-benefits) yararlanabilir. Bazı kapsayıcılar, bir uygulamanın erişimini gerektirebilecek bir geçişli önizleme olarak kullanılabilir. Diğer kapsayıcılar, geçişli olmayan önizleme olarak genel kullanıma sunulmuştur veya genel kullanıma sunulmuştur. Kapsayıcıların tam listesini ve bunların kullanılabilirliğini Azure bilişsel [Hizmetler makalesinde kapsayıcı desteği](../cognitive-services-container-support.md#container-availability-in-azure-cognitive-services) ' nde bulabilirsiniz. Kapsayıcıları [Docker Hub 'ında](https://hub.docker.com/_/microsoft-azure-cognitive-services)da görüntüleyebilirsiniz.

**S: bilişsel hizmetler bulutu ve kapsayıcılar arasında herhangi bir fark var mı?**

Y **:** Bilişsel hizmetler kapsayıcıları bilişsel hizmetler bulutunun bir alternatifidir. Kapsayıcılar, karşılık gelen bulut hizmetleriyle aynı özellikleri sunar. Müşteriler, kapsayıcıları şirket içinde veya Azure 'da dağıtabilir. Temel AI teknolojisi, fiyatlandırma katmanları, API anahtarları ve API imzası, kapsayıcı ve ilgili bulut hizmetleri arasında aynıdır. Burada, bulut hizmeti eşdeğerini üzerinde kapsayıcılar seçmeye yönelik [Özellikler ve avantajlar](../cognitive-services-container-support.md#features-and-benefits) verilmiştir.

**S: Nasıl yaparım? erişim ve geçişli bir önizleme kapsayıcısı kullanıyor musunuz?**

Y **:** Daha önce, geçitli önizleme kapsayıcıları `containerpreview.azurecr.io` depoda barındırılıyor. 22 2020 ' den itibaren bu kapsayıcılar Microsoft Container Registry barındırılır ve indirme, Docker Login komutunu kullanmanızı gerektirmez. Azure kaynağınız onaylanan Azure abonelik KIMLIĞIYLE oluşturulduysa geçişli bir önizleme kapsayıcısını çalıştırabileceksiniz. Azure aboneliğiniz [istek formunu](https://aka.ms/csgate)tamamladıktan sonra onaylanmamışsa kapsayıcıyı çalıştıramazsınız.


**S: tüm bilişsel hizmetler için kapsayıcılar kullanılabilir ve Beklendiğimiz bir sonraki kapsayıcı kümesi ne olur?**

Y **:** Daha fazla bilişsel Hizmetleri kapsayıcı teklifleri olarak kullanılabilir hale getirmek istiyoruz. Yeni kapsayıcı yayınlarına ve diğer bilişsel hizmetler bildirilerinde güncelleştirmeler almak için yerel Microsoft hesabı yöneticinize başvurun.

**S: bilişsel hizmetler kapsayıcıları için Service-Level Sözleşmesi (SLA) ne olacak?**

Y **:** Bilişsel hizmetler kapsayıcılarının SLA 'sı yoktur.

Kaynakların bilişsel hizmetler kapsayıcı konfigürasyonları müşteriler tarafından denetlenir, bu nedenle Microsoft genel kullanılabilirlik (GA) için SLA sunmaz. Müşteriler şirket içinde kapsayıcı dağıtmak için ücretsizdir, bu nedenle konak ortamlarını tanımlar.

> [!IMPORTANT]
> Bilişsel hizmetler Service-Level anlaşmaları hakkında daha fazla bilgi edinmek için [SLA sayfamızı ziyaret edin](https://azure.microsoft.com/support/legal/sla/cognitive-services/v1_1/).

**S: Bu kapsayıcılar bağımsız bulutlarda kullanılabilir mi?**

Y **:** Herkes "sogeign Cloud" terimiyle tanıdık değildir, bu nedenle tanımıyla başlayalım:

> "Sogeign Cloud", [Azure Kamu](../../azure-government/documentation-government-welcome.md), [Azure Almanya](../../germany/germany-welcome.md)ve [Azure Çin 21Vianet](https://docs.microsoft.com/azure/china/overview-operations) bulutlarından oluşur.

Ne yazık ki *bilişsel* hizmetler kapsayıcıları, sogeign bulutlarında yerel olarak desteklenmez. Kapsayıcılar bu bulutlarda çalıştırılabilir, ancak genel buluttan çekilir ve kullanım verilerinin genel uç noktaya gönderilmesi gerekir.

### <a name="versioning"></a>Sürüm Oluşturma

**S: kapsayıcılar en son sürüme nasıl güncelleştirilir?**

Y **:** Müşteriler, dağıttıkları kapsayıcıları ne zaman güncelleştirebilecekleri seçebilirler. Kapsayıcılar, en son sürümü göstermek için gibi standart [Docker etiketleriyle](https://docs.docker.com/engine/reference/commandline/tag/) işaretlenir `latest` . Müşterilerin, bir görüntü güncelleştirildikten sonra bildirim alma hakkında ayrıntılı bilgi edinmek için [Web kancalarını Azure Container Registry](../../container-registry/container-registry-webhook.md) kullanıma sundukları son sürümünü çekmelerini öneririz.
 
**S: hangi sürümler desteklenecek?**

Y **:** Kapsayıcının geçerli ve son ana sürümü desteklenecek. Ancak, müşterilerin en son teknolojiyi almak için güncel kalmasını öneririz.
 
**S: güncelleştirmeler nasıl sürümlenmiş?**

Y **:** Ana sürüm değişiklikleri, API imzasında bir yeni değişiklik olduğunu gösterir. Bunun, genellikle karşılık gelen bilişsel hizmet bulutu sunumundaki ana sürüm değişiklikleriyle ilgili olduğunu tahmin ederiz. İkincil sürüm değişiklikleri, hata düzeltmelerini, model güncelleştirmelerini veya API imzasında bir değişiklik yapmayan yeni özellikleri gösterir.

## <a name="technical-questions"></a>Teknik sorular

**S: bilişsel hizmetler kapsayıcılarını IoT cihazlarında nasıl çalıştırmalıyım?**

Y **:** Güvenilir bir internet bağlantınız yoksa veya bant genişliği maliyetlerine kaydetmek isteyip istemediğiniz. Ya da düşük gecikme süreli gereksinimleriniz varsa veya sitede çözümlenmesi gereken hassas verilerle uğraşıyorsanız, bilişsel [Hizmetler kapsayıcıları ile Azure IoT Edge](https://azure.microsoft.com/blog/running-cognitive-services-on-iot-edge/) , bulut ile tutarlılık sağlar.

**S: Bu kapsayıcılar OpenShift ile uyumlu mı?** 

OpenShift ile kapsayıcıları test etmedik, ancak bilişsel hizmetler kapsayıcıları Docker görüntülerini destekleyen herhangi bir platformda çalışmalıdır. OpenShift kullanıyorsanız, kapsayıcıları olarak çalıştırmayı öneririz `root-user` .

**S: ürün geri bildirimi ve özellik önerileri sağlamak Nasıl yaparım? misiniz?**

Y **:** Müşterilerin sorunları herkese açık bir şekilde duymaları ve olası sorunların çakıştığı yerde aynısını yapmış olan diğerlerinin oyunu [oylamalarını](https://cognitive.uservoice.com/) sağlar. Kullanıcı sesli aracı hem ürün geri bildirimi hem de özellik önerileri için kullanılabilir.

**S: bilişsel hizmetler kapsayıcıları tarafından hangi durum iletileri ve hatalar döndürülür?**

Y **:** Durum iletilerinin ve hataların listesi için aşağıdaki tabloya bakın.

|Durum  | Açıklama  |
|---------|---------|
| `Valid` | API anahtarınız geçerli, hiçbir eylem gerekmiyor. |
| `Invalid` |   API anahtarınız geçersiz. Kapsayıcıyı çalıştırmak için geçerli bir API anahtarı sağlamanız gerekir. Azure portal Azure bilişsel hizmetler kaynağınız için **anahtarlar ve uç nokta** bölümünde API anahtarınızı ve hizmet bölgenizi bulun. |
| `Mismatch` | Farklı bir bilişsel hizmetler kaynağı türü için bir API anahtarı veya uç nokta sağladınız. Azure bilişsel hizmetler kaynağınız için **anahtarlar ve uç nokta** bölümünde API anahtarınızı ve hizmet bölgenizi bulun. |
| `CouldNotConnect` | Kapsayıcı, faturalama uç noktasına bağlanamadı. Daha `Retry-After` fazla istek yapmadan önce değeri denetleyip bu sürenin bitmesini bekleyin. |
| `OutOfQuota` | API anahtarı kotayı tükendi. Fiyatlandırma katmanınızı yükseltebilir ya da ek kotanın kullanılabilir hale gelmesini bekleyebilirsiniz. Azure portal Azure bilişsel hizmet kaynağınızın **fiyatlandırma katmanı** bölümünde yer alarak katmanınızı bulun. |
| `BillingEndpointBusy` | Faturalama uç noktası şu anda meşgul. Daha `Retry-After` fazla istek yapmadan önce değeri denetleyip bu sürenin bitmesini bekleyin. |
| `ContainerUseUnauthorized` | Verilen API anahtarı bu kapsayıcı ile kullanım için yetkili değil. Muhtemelen kapılı bir kapsayıcı kullanıyorsunuz, bu nedenle [çevrimiçi bir istek](https://aka.ms/csgate)göndererek Azure abonelik kimliğinizin onaylandığından emin olun. |
| `Unknown` | Sunucu şu anda faturalandırma isteklerini işleyemiyor. |


**S: destek için kimler iletişim kuracağım?**

Y **:** Müşteri desteği kanalları bilişsel hizmetler bulut teklifiyle aynıdır. Tüm bilişsel hizmetler kapsayıcıları, bize ve topluluk destek müşterilerine yardımcı olacak günlüğe kaydetme özelliklerini içerir. Ek destek için aşağıdaki seçeneklere bakın.

### <a name="customer-support-plan"></a>Müşteri desteği planı

Müşteriler destek için kimin iletişim kuradığını görmek için [Azure Destek planına](https://azure.microsoft.com/support/plans/) başvurmalıdır.

### <a name="azure-knowledge-center"></a>Azure bilgi merkezi

Müşteri, soruları yanıtlamak ve sorunları gidermek için [Azure bilgi merkezi](https://azure.microsoft.com/resources/knowledge-center/) 'ni keşfetmeye ücretsizdir.

### <a name="stack-overflow"></a>Stack Overflow

> [Stack Overflow](https://en.wikipedia.org/wiki/Stack_Overflow) , profesyonel ve meridik programcıları için bir soru ve yanıt sitesidir.

Gereksinimlerinize göre uygun olabilecek sorular ve yanıtlar için aşağıdaki etiketleri araştırın.

* [Azure Bilişsel Hizmetler](https://stackoverflow.com/questions/tagged/azure-cognitive-services)
* [Microsoft bilişsel](https://stackoverflow.com/questions/tagged/microsoft-cognitive)

**S: Faturalandırma nasıl çalışır?**

Y **:** Müşteriler, bilişsel hizmetler bulutuna benzer şekilde tüketimine göre ücretlendirilir. Kapsayıcıların Azure 'a ölçüm verileri gönderecek şekilde yapılandırılması gerekir ve işlemler buna göre faturalandırılır. Barındırılan ve şirket içi hizmetler genelinde kullanılan kaynaklar, her iki kullanımlar için de olmak üzere katmanlı fiyatlandırmayla tek bir kotaya eklenir. Daha fazla ayrıntı için ilgili teklifin fiyatlandırma sayfasına bakın.

* [Anomali Algılayıcısı][ad-containers-billing]
* [Görüntü İşleme][cv-containers-billing]
* [Yüz Tanıma][fa-containers-billing]
* [Form Tanıma][fr-containers-billing]
* [Language Understanding (LUIS)][lu-containers-billing]
* [Konuşma Hizmeti API’si][sp-containers-billing]
* [Metin Analizi][ta-containers-billing]

> [!IMPORTANT]
> Bilişsel hizmetler kapsayıcıları, ölçüm için Azure 'a bağlı kalmadan çalıştırılmak üzere lisanslanmaz. Müşterilerin, ödeme bilgilerini her zaman ölçüm hizmetiyle iletişimine olanak tanımak için kapsayıcıların etkinleştirilmesi gerekir. Bilişsel hizmetler kapsayıcıları, müşteri verilerini Microsoft 'a göndermez.
 
**S: kapsayıcılar için geçerli destek garantisi nedir?**

Y **:** Önizlemeler için garanti yoktur. Microsoft 'un kurumsal yazılım için standart garantisi, kapsayıcılar genel kullanım (GA) olarak duyurulduğu zaman geçerlidir.
 
**S: internet bağlantısı kesildiğinde bilişsel hizmetler kapsayıcılarına ne olur?**

Y **:** Bilişsel hizmetler kapsayıcıları, ölçüm için Azure 'a bağlı kalmadan çalıştırılmak üzere *lisanslanmaz* . Müşterilerin, ölçüm hizmeti ile her zaman iletişim kurması için kapsayıcıları etkinleştirmeleri gerekir.

**S: kapsayıcı, Azure 'a bağlı olmadan ne kadar süreyle çalışabilir?**

Y **:** Bilişsel hizmetler kapsayıcıları, ölçüm için Azure 'a bağlı kalmadan çalıştırılmak üzere *lisanslanmaz* . Müşterilerin, ölçüm hizmeti ile her zaman iletişim kurması için kapsayıcıları etkinleştirmeleri gerekir.
 
**S: Bu kapsayıcıları çalıştırmak için geçerli donanım ne gerekir?**

Y **:** Bilişsel hizmetler kapsayıcıları, x64 Linux Docker kapsayıcılarını destekleyen uyumlu bir Linux düğümü, VM ve Edge cihazını çalıştıran x64 tabanlı kapsayıcılardır. Bunların hepsi CPU işlemcileri gerektirir. Her kapsayıcı teklifi için en düşük ve önerilen yapılandırmalara aşağıda ulaşılabilir:

* [Anomali Algılayıcısı][ad-containers-recommendations]
* [Görüntü İşleme][cv-containers-recommendations]
* [Yüz Tanıma][fa-containers-recommendations]
* [Form Tanıma][fr-containers-recommendations]
* [Language Understanding (LUIS)][lu-containers-recommendations]
* [Konuşma Hizmeti API’si][sp-containers-recommendations]
* [Metin Analizi][ta-containers-recommendations]
 
**S: Bu kapsayıcılar Şu anda Windows üzerinde destekleniyor mu?**

Y **:** Bilişsel hizmetler kapsayıcıları Linux kapsayıcılarıdır, ancak Windows üzerinde Linux kapsayıcıları için bazı destek vardır. Windows üzerindeki Linux kapsayıcıları hakkında daha fazla bilgi için bkz. [Docker belgeleri](https://blog.docker.com/2017/09/preview-linux-containers-on-windows/).
 
**S: kapsayıcıları keşfet Nasıl yaparım??**

Y **:** Bilişsel hizmetler kapsayıcıları Azure portal, Docker Hub ve Azure Container Registry gibi çeşitli konumlarda mevcuttur. En son kapsayıcı konumları için, [kapsayıcı depoları ve görüntüleri](../cognitive-services-container-support.md#container-repositories-and-images)öğesine bakın.

**S: bilişsel hizmetler kapsayıcıları AWS ve Google teklifleriyle nasıl karşılaştırılmaktadır?**

Y **:** Microsoft, müşteriler bir bulut hizmeti kullanıyor olsa da, önceden eğitilen AI modellerini işlem başına basit faturalandırmayla taşımaya yönelik ilk bulut sağlayıcıdır. Microsoft Hibrit bulutun müşterilere daha fazla seçenek sunabiliyor.

**S: kapsayıcılar hangi uyumluluk sertifikalarına sahip?**

Y **:** Bilişsel hizmetler kapsayıcılarının uyumluluk sertifikaları yok

**S: bilişsel hizmet kapsayıcıları hangi bölgelerde kullanılabilir?**

Y **:** Kapsayıcılar herhangi bir bölgedeki herhangi bir yerde çalıştırılabilir, ancak bir anahtara gerek duyar ve ölçüm için Azure 'a geri çağrı yapılır. Bulut hizmeti için desteklenen tüm bölgeler, kapsayıcılar ölçüm çağrısı için desteklenir.

[!INCLUDE [Containers next steps](includes/containers-next-steps.md)]

[ad-containers]: ../anomaly-Detector/anomaly-detector-container-howto.md
[cv-containers]: ../computer-vision/computer-vision-how-to-install-containers.md
[fa-containers]: ../face/face-how-to-install-containers.md
[fr-containers]: ../form-recognizer/form-recognizer-container-howto.md
[lu-containers]: ../luis/luis-container-howto.md
[sp-containers]: ../speech-service/speech-container-howto.md
[ta-containers]: ../text-analytics/how-tos/text-analytics-how-to-install-containers.md

[ad-containers-billing]: ../anomaly-Detector/anomaly-detector-container-howto.md#billing
[cv-containers-billing]: ../computer-vision/computer-vision-how-to-install-containers.md#billing
[fa-containers-billing]: ../face/face-how-to-install-containers.md#billing
[fr-containers-billing]: ../form-recognizer/form-recognizer-container-howto.md#billing
[lu-containers-billing]: ../luis/luis-container-howto.md#billing
[sp-containers-billing]: ../speech-service/speech-container-howto.md#billing
[ta-containers-billing]: ../text-analytics/how-tos/text-analytics-how-to-install-containers.md#billing

[ad-containers-recommendations]: ../anomaly-Detector/anomaly-detector-container-howto.md#container-requirements-and-recommendations
[cv-containers-recommendations]: ../computer-vision/computer-vision-how-to-install-containers.md#container-requirements-and-recommendations
[fa-containers-recommendations]: ../face/face-how-to-install-containers.md#container-requirements-and-recommendations
[fr-containers-recommendations]: ../form-recognizer/form-recognizer-container-howto.md#container-requirements-and-recommendations
[lu-containers-recommendations]: ../luis/luis-container-howto.md#container-requirements-and-recommendations
[sp-containers-recommendations]: ../speech-service/speech-container-howto.md#container-requirements-and-recommendations
[ta-containers-recommendations]: ../text-analytics/how-tos/text-analytics-how-to-install-containers.md#container-requirements-and-recommendations
