---
title: IoT Edge üzerinde Azure Stream Analytics
description: Azure Stream Analytics Edge işleri oluşturun ve Azure IoT Edge çalıştıran cihazlara dağıtın.
ms.service: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.topic: how-to
ms.date: 10/29/2020
ms.custom: seodec18
ms.openlocfilehash: cba81b8415f0f9cf7253e674e90ae09718b94d54
ms.sourcegitcommit: 857859267e0820d0c555f5438dc415fc861d9a6b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93130485"
---
# <a name="azure-stream-analytics-on-iot-edge"></a>IoT Edge üzerinde Azure Stream Analytics
 
IoT Edge üzerinde Azure Stream Analytics (ASA), geliştiricilerin neredeyse gerçek zamanlı analitik zekayı IoT cihazlarına daha yakın bir biçimde dağıtmasına ve böylece cihaz tarafından üretilen verilerin tüm değerini ortaya çıkarabilmesine olanak tanır. Azure Stream Analytics düşük gecikme süresi, dayanıklılık, bant genişliğinin verimli kullanımı ve uyumluluk için tasarlanmıştır. Şimdi kuruluşlar bulutta yapılan endüstriyel işlemlerin ve tamamlayıcı Büyük Veri analizinin yakınında denetim mantığı dağıtımı yapabilir.  

IoT Edge üzerinde Azure Stream Analytics, [Azure IoT Edge](https://azure.microsoft.com/campaigns/iot-edge/) Framework içinde çalışır. İş ASA içinde oluşturulduktan sonra, IoT Hub kullanarak dağıtabilir ve yönetebilirsiniz.

## <a name="scenarios"></a>Senaryolar
![IoT Edge üst düzey diyagramı](media/stream-analytics-edge/ASAedge-highlevel-diagram.png)

* **Düşük gecikmeli komut ve denetim** : Örneğin, üretim güvenlik sistemleri, işlemsel verilere Ultra düşük gecikme süresiyle yanıt vermelidir. IoT Edge on ile, algılayıcı verilerini neredeyse gerçek zamanlı olarak çözümleyebilir ve bir makineyi durdurmayı veya uyarıları tetikleme bozukluklarını saptadığınızda komutları verebilirsiniz.
*   **Buluta sınırlı bağlantı** : uzaktan araştırma Ekipmanı, bağlı Vessels veya daha fazla detaya gitme gibi görev açısından kritik sistemler, bulut bağlantısı aralıklı olarak olsa bile verileri analiz etmeniz ve bunlara yanıt vermelidirler. ASA ile, akış mantığınızı ağ bağlantısından bağımsız olarak çalışır ve daha fazla işlem veya depolama için buluta ne gönderdiklerinizi seçebilirsiniz.
* **Sınırlı bant genişliği** : jet motorları veya bağlı otomobiller tarafından oluşturulan veri hacmi, verilerin buluta gönderilmeden önce filtrelenme veya önceden işlenmesi gereken büyüklükte olabilir. ASA kullanarak, buluta gönderilmesi gereken verileri filtreleyebilir veya toplayabilirsiniz.
* **Uyumluluk** : yasal uyumluluk, bazı verilerin buluta gönderilmeden önce yerel olarak anonimleştirmesini veya toplanmasını gerektirebilir.

## <a name="edge-jobs-in-azure-stream-analytics"></a>Azure Stream Analytics Edge işleri
### <a name="what-is-an-edge-job"></a>"Edge" işi nedir?

ASA Edge işleri [Azure IoT Edge cihazlara](../iot-edge/about-iot-edge.md)dağıtılan kapsayıcılar üzerinde çalışır. Bunlar iki bölümden oluşur:
1.  İş tanımından sorumlu bir bulut bölümü: kullanıcılar, bulutta giriş, çıkış, sorgu ve diğer ayarları (sıra dışı olaylar vb.) tanımlar.
2.  IoT cihazlarınızda çalışan bir modül. ASA altyapısını içerir ve buluttan iş tanımını alır. 

ASA, cihazlara Edge işleri dağıtmak için IoT Hub kullanır. IoT Edge dağıtımı hakkında daha fazla bilgiye [buradan görünebilirler](../iot-edge/module-deployment-monitoring.md).

![Azure Stream Analytics Edge işi](media/stream-analytics-edge/stream-analytics-edge-job.png)


### <a name="installation-instructions"></a>Yükleme yönergeleri
Üst düzey adımlar aşağıdaki tabloda açıklanmıştır. Daha fazla ayrıntı aşağıdaki bölümlerde verilmiştir.

| Adım | Notlar |
| --- | --- |
| **Depolama kapsayıcısı oluşturma** | Depolama kapsayıcıları, iş tanımınızı, IoT cihazlarınızın erişebileceği yerlerde kaydetmek için kullanılır. <br>  Var olan herhangi bir depolama kapsayıcısını yeniden kullanabilirsiniz. |
| **ASA Edge işi oluşturma** | Yeni bir iş oluşturun, bir **barındırma ortamı** olarak **Edge** ' i seçin. <br> Bu işler buluttan oluşturulur/yönetilir ve kendi IoT Edge cihazlarınızda çalışır. |
| **IoT Edge ortamınızı cihazınızda ayarlayın** | [Windows](../iot-edge/quickstart.md) veya [Linux](../iot-edge/quickstart-linux.md)için yönergeler.|
| **IoT Edge cihazlarınızın ASA dağıtımını yapın** | ASA iş tanımı daha önce oluşturulan depolama kapsayıcısına aktarılmalıdır. |

İlk ASA işinizi IoT Edge dağıtmak için [Bu adım adım öğreticiyi](../iot-edge/tutorial-deploy-stream-analytics.md) izleyebilirsiniz. Aşağıdaki videoda, bir IoT Edge cihazında Stream Analytics işi çalıştırma işlemini anlamanıza yardımcı olması gerekir:  


> [!VIDEO https://channel9.msdn.com/Events/Connect/2017/T157/player]

#### <a name="create-a-storage-container"></a>Depolama kapsayıcısı oluşturma
ASA derlenen sorguyu ve iş yapılandırmasını dışarı aktarmak için bir depolama kapsayıcısı gereklidir. ASA Docker görüntüsünü özel Sorgunuzla yapılandırmak için kullanılır. 
1. Azure portal bir depolama hesabı oluşturmak için [Bu yönergeleri](../storage/common/storage-account-create.md) izleyin. Bu hesabı ASA ile kullanmak için tüm varsayılan seçenekleri koruyabilirsiniz.
2. Yeni oluşturulan depolama hesabında bir BLOB depolama kapsayıcısı oluşturun:
    1. **Bloblar** ' a ve ardından **+ kapsayıcı** ' ya tıklayın. 
    2. Bir ad girin ve kapsayıcıyı **özel** olarak tutun.

#### <a name="create-an-asa-edge-job"></a>ASA Edge işi oluşturma
> [!Note]
> Bu öğretici, Azure portal kullanarak ASA iş oluşturmaya odaklanır. Ayrıca, [Visual Studio eklentisini kullanarak asa Edge işi oluşturabilirsiniz](./stream-analytics-tools-for-visual-studio-edge-jobs.md)

1. Azure portal yeni bir "Stream Analytics işi" oluşturun. [Burada yeni BIR asa işi oluşturmak Için doğrudan bağlantı](https://ms.portal.azure.com/#create/Microsoft.StreamAnalyticsJob).

2. Oluşturma ekranında, bir **kenara** **barındırma ortamı** olarak seçin (aşağıdaki resme bakın)

   ![Edge üzerinde Stream Analytics iş oluşturma](media/stream-analytics-edge/create-asa-edge-job.png)
3. İş tanımı
    1. **Giriş akışı (ler) i tanımlayın** . İşiniz için bir veya birkaç giriş akışı tanımlayın.
    2. Başvuru verilerini tanımlayın (isteğe bağlı).
    3. **Çıkış akışı (ler) i tanımlayın** . İşiniz için bir veya birkaç çıkış akışı tanımlayın. 
    4. **Sorgu tanımlayın** . Satır içi düzenleyiciyi kullanarak, bulutta ASA sorgusunu tanımlayın. Derleyici, ASA Edge için etkinleştirilmiş sözdizimini otomatik olarak denetler. Ayrıca, örnek verileri karşıya yükleyerek sorgunuzu test edebilirsiniz. 

4. **IoT Edge ayarları** menüsünde depolama kapsayıcısı bilgilerini ayarlayın.

5. İsteğe bağlı ayarları ayarla
    1. **Olay sıralaması** . Portalda sıra dışı ilkesini yapılandırabilirsiniz. Belgeler [burada](/stream-analytics-query/time-skew-policies-azure-stream-analytics)bulunabilir.
    2. **Yerel ayar** . İnternalization biçimini ayarlayın.



> [!Note]
> Bir dağıtım oluşturulduğunda, ASA iş tanımını bir depolama kapsayıcısına dışarı aktarır. Bu iş tanımı, bir dağıtım süresince aynı kalır. Sonuç olarak, Edge üzerinde çalışan bir işi güncelleştirmek istiyorsanız, işi ASA içinde düzenlemeniz ve ardından IoT Hub yeni bir dağıtım oluşturmanız gerekir.


#### <a name="set-up-your-iot-edge-environment-on-your-devices"></a>Cihazınızda IoT Edge ortamınızı ayarlama
Edge işleri, Azure IoT Edge çalıştıran cihazlara dağıtılabilir.
Bunun için aşağıdaki adımları izlemeniz gerekir:
- IoT Hub 'ı oluşturun.
- Edge cihazlarınıza Docker ve IoT Edge Runtime 'ı yükler.
- Cihazlarınızı IoT Hub cihaz olarak ayarlayın **IoT Edge** .

Bu adımlar, [Windows](../iot-edge/quickstart.md) veya [Linux](../iot-edge/quickstart-linux.md)için IoT Edge belgelerinde açıklanmıştır.  


####  <a name="deployment-asa-on-your-iot-edge-devices"></a>IoT Edge cihazınızdan dağıtım ASA
##### <a name="add-asa-to-your-deployment"></a>Dağıtımınıza ASA ekleyin
- Azure portal, IoT Hub açın, **IoT Edge** ' e gidin ve bu dağıtım için hedeflemek istediğiniz cihaza tıklayın.
- **Modül ayarla** ' yı seçin, ardından **+ Ekle** ' yi seçin ve **Azure Stream Analytics modülünü** seçin.
- Oluşturduğunuz aboneliği ve ASA Edge işini seçin. Kaydet’e tıklayın.
![Dağıtımınıza ASA modülü ekleme](media/stream-analytics-edge/add-stream-analytics-module.png)


> [!Note]
> Bu adım sırasında, ASA depolama kapsayıcısında "EdgeJobs" adlı bir klasör oluşturur (zaten mevcut değilse). Her dağıtım için, "EdgeJobs" klasöründe yeni bir alt klasör oluşturulur.
> İşinizi IoT Edge cihazlara dağıttığınızda, ASA iş tanımı dosyası için bir paylaşılan erişim imzası (SAS) oluşturur. SAS anahtarı cihaz ikizi kullanarak IoT Edge cihazlara güvenli bir şekilde iletilir. Bu anahtarın süre sonu, oluşturma gününden üç yıldır. Bir IoT Edge işini güncelleştirdiğinizde SAS değişir, ancak görüntü sürümü değişmeyecektir. **Güncelleştirme** yaptıktan sonra dağıtım iş akışını izleyin ve cihazda bir güncelleştirme bildirimi kaydedilir.


IoT Edge dağıtımları hakkında daha fazla bilgi için [Bu sayfaya](../iot-edge/module-deployment-monitoring.md)bakın.


##### <a name="configure-routes"></a>Rotaları yapılandırma
IoT Edge modüller arasında ve modüller ile IoT Hub arasında bildirimli olarak ileti yönlendirmek için bir yol sağlar. Tam sözdizimi [burada](../iot-edge/module-composition.md)açıklanmıştır.
ASA işinde oluşturulan girişlerin ve çıktıların adları, yönlendirme için uç nokta olarak kullanılabilir.  

###### <a name="example"></a>Örnek

```json
{
    "routes": {
        "sensorToAsa":   "FROM /messages/modules/tempSensor/* INTO BrokeredEndpoint(\"/modules/ASA/inputs/temperature\")",
        "alertsToCloud": "FROM /messages/modules/ASA/* INTO $upstream",
        "alertsToReset": "FROM /messages/modules/ASA/* INTO BrokeredEndpoint(\"/modules/tempSensor/inputs/control\")"
    }
}

```
Bu örnekte, aşağıdaki resimde açıklanan senaryoya yönelik yollar gösterilmektedir. " **Sıcaklık** " adlı ve " **Uyarı** " adlı bir çıktı içeren " **asa** " adlı bir kenar işi içerir.
![İleti yönlendirme diyagramı örneği](media/stream-analytics-edge/edge-message-routing-example.png)

Bu örnek aşağıdaki yolları tanımlar:
- **Tempsensörü** tarafından gönderilen her Ileti, **asa** **adlı girişe,**
- **Asa** modülünün tüm çıktıları bu cihaza bağlı IoT Hub gönderilir ($upstream),
- **Asa** modülünün tüm çıkışları, **tempsensörü** **Denetim** uç noktasına gönderilir.


## <a name="technical-information"></a>Teknik bilgiler
### <a name="current-limitations-for-iot-edge-jobs-compared-to-cloud-jobs"></a>Bulut işleriyle karşılaştırılan IoT Edge işleri için geçerli sınırlamalar
Amaç IoT Edge işleri ve bulut işleri arasında eşlik sahibi olmaktır. Çoğu SQL sorgu dili özelliği desteklenir, hem bulutta hem de IoT Edge aynı mantığı çalıştırmaya olanak tanır.
Ancak şu özellikler Edge işleri için henüz desteklenmemektedir:
* JavaScript 'te Kullanıcı tanımlı işlevler (UDF). UDF, IoT Edge işleri (Önizleme) [Için C#](./stream-analytics-edge-csharp-udf.md) dilinde kullanılabilir.
* Kullanıcı tanımlı toplamalar (UDA).
* Azure ML işlevleri.
* Tek bir adımda 14 ' ten fazla toplama kullanma.
* Giriş/çıkış için AVRO biçimi. Şu anda yalnızca CSV ve JSON desteklenir.
* Aşağıdaki SQL işleçleri:
    * BÖLÜM ÖLÇÜTÜ
    * GetMetadataPropertyValue
* Geç varış ilkesi

### <a name="runtime-and-hardware-requirements"></a>Çalışma zamanı ve donanım gereksinimleri
IoT Edge ' de ASA çalıştırmak için [Azure IoT Edge](https://azure.microsoft.com/campaigns/iot-edge/)çalıştırabileceği cihazlara ihtiyacınız vardır. 

ASA ve Azure IoT Edge birden çok konak işletim sisteminde (Windows, Linux) çalışan taşınabilir bir çözüm sağlamak için **Docker** kapsayıcıları kullanın.

IoT Edge on, hem x86-64 hem de ARM (Gelişmiş RıSC makineleri) mimarilerinde çalışan Windows ve Linux görüntüleri olarak kullanılabilir hale getirilir. 


### <a name="input-and-output"></a>Girdi ve çıktı
#### <a name="input-and-output-streams"></a>Giriş ve çıkış akışları
ASA Edge işleri, IoT Edge cihazlarda çalışan diğer modüllerden giriş ve çıkış alabilirler. Ve belirli modüllerden bağlanmak için, dağıtım zamanında yönlendirme yapılandırmasını ayarlayabilirsiniz. Daha fazla bilgi [IoT Edge modül oluşturma belgelerinde](../iot-edge/module-composition.md)açıklanmıştır.

Hem giriş hem de çıkış için CSV ve JSON biçimleri desteklenir.

ASA işte oluşturduğunuz her giriş ve çıkış akışı için, dağıtılan modülünüzün karşılık gelen bir uç nokta oluşturulur. Bu uç noktalar, dağıtımınızın rotalarında kullanılabilir.

Burada, desteklenen akış girişi ve akış çıkış türleri yalnızca Edge hub 'ıdır. Başvuru girişi başvuru dosya türünü destekler. Diğer çıkışlara, bir bulut işi aşağı akış kullanılarak ulaşılabilir. Örneğin, Edge 'de barındırılan bir Stream Analytics işi uç hub 'ına çıktı göndererek, daha sonra çıktıyı IoT Hub gönderebilirler. IoT Hub ve çıktısından Power BI ya da başka bir çıkış türüne göre giriş ile ikinci bir bulut barındırılan Azure Stream Analytics işini kullanabilirsiniz.



##### <a name="reference-data"></a>Başvuru verileri
Başvuru verileri (arama tablosu olarak da bilinir), statik veya yavaş değişen, sınırlı bir veri kümesidir. Arama gerçekleştirmek veya veri akışla ilişkilendirmek için kullanılır. Azure Stream Analytics işinizdeki başvuru verilerini kullanmak için, genellikle sorgunuzda bir [başvuru VERISI katılımı](/stream-analytics-query/reference-data-join-azure-stream-analytics) kullanacaksınız. Daha fazla bilgi için [Stream Analytics aramalar için başvuru verilerini kullanma](stream-analytics-use-reference-data.md)konusuna bakın.

Yalnızca yerel başvuru verileri desteklenir. IoT Edge cihaza bir iş dağıtıldığında, Kullanıcı tanımlı dosya yolundan başvuru verilerini yükler.

Edge 'de başvuru verileriyle bir iş oluşturmak için:

1. İşiniz için yeni bir giriş oluşturun.

2. **Kaynak türü** olarak **başvuru verileri** ' ni seçin.

3. Cihaza bir başvuru veri dosyası hazırlayın. Bir Windows kapsayıcısı için, başvuru veri dosyasını yerel sürücüye yerleştirin ve yerel sürücüyü Docker kapsayıcısı ile paylaşabilirsiniz. Bir Linux kapsayıcısı için, bir Docker birimi oluşturun ve veri dosyasını birime doldurun.

4. Dosya yolunu ayarlayın. Windows konak işletim sistemi ve Windows kapsayıcısı için mutlak yolu kullanın: `E:\<PathToFile>\v1.csv` . Bir Windows ana bilgisayarı işletim sistemi ve Linux kapsayıcısı veya Linux IŞLETIM sistemi ve Linux kapsayıcısı için, şu birimdeki yolu kullanın: `<VolumeName>/file1.txt` .

![IoT Edge Azure Stream Analytics iş için yeni başvuru verileri girişi](./media/stream-analytics-edge/Reference-Data-New-Input.png)

IoT Edge güncelleştirmedeki başvuru verileri bir dağıtım tarafından tetiklenir. Tetiklendikten sonra ASA modülü, çalışan işi durdurmadan güncelleştirilmiş verileri seçer.

Başvuru verilerini güncelleştirmenin iki yolu vardır:
* Azure portal, ASA işinizdeki başvuru veri yolunu güncelleştirin.
* IoT Edge dağıtımını güncelleştirin.

## <a name="license-and-third-party-notices"></a>Lisans ve üçüncü taraf bildirimleri
* [IoT Edge üzerinde Azure Stream Analytics lisansı](https://go.microsoft.com/fwlink/?linkid=862827). 
* [IoT Edge üzerinde Azure Stream Analytics Için üçüncü taraf bildirimi](https://go.microsoft.com/fwlink/?linkid=862828).

## <a name="azure-stream-analytics-module-image-information"></a>Azure Stream Analytics modülü görüntü bilgileri 

Bu sürüm bilgileri 2019-06-27 tarihinde son güncelleştirilme tarihi:

- Görüntü: `mcr.microsoft.com/azure-stream-analytics/azureiotedge:1.0.9-linux-amd64`
   - temel görüntü: mcr.microsoft.com/dotnet/core/runtime:2.1.13-alpine
   - platformunun
      - Mimari: AMD64
      - işletim sistemi: Linux
 
- Görüntü: `mcr.microsoft.com/azure-stream-analytics/azureiotedge:1.0.9-linux-arm32v7`
   - temel görüntü: mcr.microsoft.com/dotnet/core/runtime:2.1.13-bionic-arm32v7
   - platformunun
      - Mimari: ARM
      - işletim sistemi: Linux
 
- Görüntü: `mcr.microsoft.com/azure-stream-analytics/azureiotedge:1.0.9-linux-arm64`
   - temel görüntü: mcr.microsoft.com/dotnet/core/runtime:3.0-bionic-arm64v8
   - platformunun
      - Mimari: arm64
      - işletim sistemi: Linux
      
      
## <a name="get-help"></a>Yardım alın
Daha fazla yardım için, [Azure Stream Analytics Için Microsoft Q&soru sayfasını](/answers/topics/azure-stream-analytics.html)deneyin.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure IoT Edge hakkında daha fazla bilgi](../iot-edge/about-iot-edge.md)
* [IoT Edge öğreticide ASA](../iot-edge/tutorial-deploy-stream-analytics.md)
* [Visual Studio araçlarını kullanarak Stream Analytics Edge işleri geliştirme](./stream-analytics-tools-for-visual-studio-edge-jobs.md)
* [API 'Leri kullanarak Stream Analytics için CI/CD uygulayın](stream-analytics-cicd-api.md)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.query.language.reference]: /stream-analytics-query/stream-analytics-query-language-reference
[stream.analytics.rest.api.reference]: /rest/api/streamanalytics/