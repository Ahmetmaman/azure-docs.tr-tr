---
title: Azure IOT Hub kotaları anlama ve azaltma | Microsoft Docs
description: Geliştirici Kılavuzu - IOT hub'ı ve beklenen azaltma davranış için geçerli kotalar açıklaması.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: dobett
ms.openlocfilehash: c9004e776488006d563fd4de791cade69736a5b8
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/06/2018
ms.locfileid: "44024378"
---
# <a name="reference---iot-hub-quotas-and-throttling"></a>Başvuru - IOT Hub kotaları ve azaltma

## <a name="quotas-and-throttling"></a>Kotalar ve azaltma
Her Azure aboneliği, en fazla 50 IOT hub ve en fazla 1 ücretsiz hub sahip olabilir.

Her IOT hub'ı, belirli sayıda birimleri belirli bir katman içinde sağlanır. Katman ve birim sayısı en fazla günlük kota gönderebileceğiniz iletilerinin belirleyin. İleti boyutu 0,5 KB'lık ücretsiz katmanı hub için ve diğer katmanlar için 4 KB Günlük kotayı hesaplamak için kullanılır. Daha fazla bilgi için [Azure IOT Hub fiyatlandırması][lnk-pricing].

Katman da üzerindeki tüm işlemler IOT hub'ı uygulayan azaltma sınırları belirler.

## <a name="operation-throttles"></a>İşlem kısıtlamalar
İşlem kısıtlamalar dakikalık aralıklar uygulanır ve kötüye kullanımı önlemek için kullanılmaya oranı sınırlamalardır. IOT hub'ı mümkün olduğunda hataları döndürüyor önlemek çalışır, ancak çok uzun süredir kısıtlama ihlal edilirse, özel durumları döndüren başlatır.

Bir IOT hub'ındaki sağlanan birim sayısını artırarak, herhangi bir zamanda, kota veya azaltma sınırları artırabilirsiniz.

Aşağıdaki tabloda zorlanan kısıtlamalar gösterilmektedir. Değerleri tek tek bir hub'ına bakın.

| Kısıtlama | Ücretsiz, S1 ve B1 | B2 ve S2 | B3 ve S3 | 
| -------- | ------- | ------- | ------- |
| Kimlik kayıt defteri işlemleri (oluşturma, Al, Listele, güncelleştirme ve silme) | 1.67/sec/Unit (100/dk/birim) | 1.67/sec/Unit (100/dk/birim) | 83.33/sec/Unit (5000/dk/birim) |
| Yeni cihaz bağlantılarını (oranı için bu sınır geçerlidir _yeni bağlantıları_ kurulan bağlantılar toplam sayısını değil) | Daha yüksek 100/sn veya 12/sn/birim <br/> Örneğin, iki adet S1 birimi 2 olan\*12 = 24 yeni bağlantıları/sn, ancak sahip en az 100 yeni bağlantıları/sn, birimleri. Dokuz adet S1 birimi ile kullandığınız 108 yeni bağlantıları/sn (9\*12), birimleri arasında. | 120 yeni bağlantıları/sn/birim | 6000 yeni bağlantıları/sn/birim |
| Cihazdan buluta gönderim | Daha yüksek 100/sn veya 12/sn/birim <br/> Örneğin, iki adet S1 birimi 2 olan\*12 = 24/sn, ancak en az 100/sn, birimleri arasında. Dokuz adet S1 birimi ile kullandığınız 108/sn (9\*12), birimleri arasında. | 120/sn/birim | 6000/sn/birim |
| Bulut-cihaz gönderen<sup>1</sup> | 1.67/sec/Unit (100/dk/birim) | 1.67/sec/Unit (100/dk/birim) | 83.33/sec/Unit (5000/dk/birim) |
| Bulut-cihaz alır<sup>1</sup> <br/> (yalnızca cihaz HTTPS kullandığında)| 16.67/sec/Unit (1000/dk/birim) | 16.67/sec/Unit (1000/dk/birim) | 833.33/sec/Unit (50000/dk/birim) |
| Karşıya dosya yükleme | 1.67 dosya karşıya yükleme bildirimi/sn/birim (100/dk/birim) | 1.67 dosya karşıya yükleme bildirimi/sn/birim (100/dk/birim) | 83.33 dosya karşıya yükleme bildirimi/sn/birim (5000/dk/birim) |
| Doğrudan yöntemler<sup>1</sup> | 160KB/sn/birim<sup>2</sup> | 480KB/sn/birim<sup>2</sup> | 24MB/sn/birim<sup>2</sup> | 
| İkiz (cihaz ve modül) okuma<sup>1</sup> | 10/sn | Daha yüksek 10/sn veya 1/sn/birim | 50/sn/birim |
| İkiz Güncelleştirmesi (cihaz ve modül)<sup>1</sup> | 10/sn | Daha yüksek 10/sn veya 1/sn/birim | 50/sn/birim |
| İşleri oluşturma, güncelleştirme, listeleme, silme işlemleri | 1.67/sec/Unit (100/dk/birim) | 1.67/sec/Unit (100/dk/birim) | 83.33/sec/Unit (5000/dk/birim) |
| İşleri ikiz güncelleştirmesi, işlemleri doğrudan yöntem çağırma | 10/sn | Daha yüksek 10/sn veya 1/sn/birim | 50/sn/birim |
| İçeri/dışarı aktarma işlemlerini işler toplu | 1 etkin iş hub'ı başına | 1 etkin iş hub'ı başına | 1 etkin iş hub'ı başına |
| Yapılandırmalar ve edge dağıtımlarını<sup>1</sup> <br/> (oluşturma, güncelleştirme, listeleme, silme) | 0.33/sec/Unit (20/dk/birim) | 0.33/sec/Unit (20/dk/birim) | 0.33/sec/Unit (20/dk/birim) |


<sup>1</sup>bu özellik, IOT Hub'ın temel katmanda kullanılabilir değil. Daha fazla bilgi için [doğru IOT hub'a seçme](iot-hub-scaling.md). <br/><sup>2</sup>8 KB'lık olan ölçüm boyutunu azaltma.

*Cihaz bağlantılarını* kısıtlama oranı, yeni cihaz bağlantılarını kurulabileceği ile IOT hub'ı yönetir. *Cihaz bağlantılarını* azaltma, en fazla eşzamanlı olarak bağlanan cihaz sayısını belirleyen değil. IOT hub için sağlanan birim sayısını azaltma bağlıdır.

Örneğin, tek bir S1 birimi satın alırsanız, saniye başına 100 bağlantılarının bir kısıtlama alın. Bu nedenle, 100.000 cihaz bağlanmak için en az 1000 saniye (yaklaşık 16 dakika) alır. Ancak, kimlik kayıt defterinde kayıtlı cihazları olan sayıda eşzamanlı olarak bağlanan cihazlara sahip olabilir.

IOT Hub'ın ayrıntılı incelemesi için azaltma davranışı, blog gönderisine bakın [IOT Hub'ın azaltma ve][lnk-throttle-blog].

> [!IMPORTANT]
> Kimlik kayıt defteri işlemleri, cihaz yönetimi ve sağlama senaryolarında çalışma zamanı kullanmak için tasarlanmıştır. Okuma veya çok sayıda cihaz kimliklerini güncelleştirme aracılığıyla desteklenir [içeri ve dışarı aktarma işleri][lnk-importexport].
> 
> 

## <a name="other-limits"></a>Diğer sınırlamaları

IOT hub'ı diğer kullanım sınırlamaları uygular:

| İşlem | Sınır |
| --------- | ----- |
| Karşıya dosya yükleme URI'ler | 10000 SAS URI'si için bir depolama hesabı aynı anda çıkarılabilir. <br/> Tek seferde 10 SAS URI’si/cihaz çıkarılabilir. |
| İşleri<sup>1</sup> | İş Geçmişi yukarı 30 güne kadar tutulur <br/> En fazla eşzamanlı iş olan yalnızca 1 (ücretsiz) ve S1, 5 (S2 için), 10 (S3 için). |
| Ek uç noktalar | SKU Ücretli hubs 10 ek uç noktalar olabilir. Ücretsiz SKU hub'ları, bir ek uç nokta olabilir. |
| İleti yönlendirme kuralları | SKU Ücretli hubs 100 yönlendirme kuralları olabilir. Ücretsiz SKU hubs beş yönlendirme kuralları olabilir. |
| CİHAZDAN buluta ileti gönderme | En büyük ileti boyutu 256 KB |
| Bulut-cihaz Mesajlaşma<sup>1</sup> | En büyük ileti boyutu 64 KB. Bekleyen iletileri teslim için en fazla 50'dir. |
| Doğrudan yöntem<sup>1</sup> | En fazla bir doğrudan yöntem yükünün boyutu 128 KB ' dir. |
| Yapılandırmalar | hub başına 20 yapılandırmalar. |
| Edge dağıtımları | hub başına 20 dağıtımları. Dağıtım başına 20 modüller. |
| Çiftleri | 8 KB'lık ikizi bölümü (etiketler, istenen özellikler, bildirilen özellikleri) başına en büyük boyut: |

<sup>1</sup>bu özellik, IOT Hub'ın temel katmanda kullanılabilir değil. Daha fazla bilgi için [doğru IOT hub'a seçme](iot-hub-scaling.md).

> [!NOTE]
> Şu anda en fazla cihazları tek bir IOT hub'ına bağlanabilir 500.000 sayısıdır. Bu sınırı artırmak istiyorsanız, ilgili kişi [Microsoft Support](https://azure.microsoft.com/support/options/).

## <a name="latency"></a>Gecikme süresi
Tüm işlemler için düşük gecikme süresi sağlamak IOT hub'ı içindedir. Ancak, ağ koşulları ve öngörülemeyen diğer faktörler nedeniyle, bir en yüksek gecikme süresi garanti edemez. Çözümünüzü tasarlarken, aşağıdakileri yapmalısınız:

* En yüksek gecikme süresi hakkında varsayımlar her IOT Hub işleminin yapmaktan kaçının.
* IOT hub'ınıza cihazlarınıza en yakın Azure bölgesine sağlayın.
* Azure IOT Edge cihazı veya bir ağ geçidi cihazı yakın gecikmeye duyarlı işlemleri gerçekleştirmek için kullanmayı düşünün.

Birden çok IOT Hub birimi, daha önce açıklandığı gibi azaltma etkiler, ancak herhangi bir ek gecikme avantajları veya garanti sağlamaz.
İşlem gecikme süresi içinde beklenmeyen artışları görürseniz, başvurun [Microsoft Support](https://azure.microsoft.com/support/options/).

## <a name="next-steps"></a>Sonraki adımlar
Bu IOT Hub Geliştirici Kılavuzu'nda olan diğer başvuru konularını içerir:

* [IOT Hub uç noktaları][lnk-devguide-endpoints]
* [Cihaz ikizleri, işler ve ileti yönlendirme için IOT Hub sorgu dili][lnk-devguide-query]
* [IOT hub'ı MQTT desteği][lnk-devguide-mqtt]

[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub
[lnk-throttle-blog]: https://azure.microsoft.com/blog/iot-hub-throttling-and-you/
[lnk-importexport]: iot-hub-devguide-identity-registry.md#import-and-export-device-identities

[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-devguide-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
