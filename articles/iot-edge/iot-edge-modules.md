---
title: Azure IOT Edge modüllerini anlama | Microsoft Docs
description: Azure IOT Edge modülleri ve nasıl yapılandırılacağı hakkında bilgi edinin
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 09/21/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 5d80b6438569e74ee254d27e0061443a87efc6ce
ms.sourcegitcommit: 42405ab963df3101ee2a9b26e54240ffa689f140
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/28/2018
ms.locfileid: "47423400"
---
# <a name="understand-azure-iot-edge-modules"></a>Azure IOT Edge modüllerini anlama

Azure IOT Edge dağıtma ve yönetmenize olanak sağlar biçiminde edge üzerinde iş mantığını *modülleri*. Azure IOT Edge modülleri, IOT Edge tarafından yönetilen bir hesaplama birimi en küçük olan ve Azure hizmetlerinin (örneğin, Azure Stream Analytics) veya kendi çözümünüze özgü kodla içerebilir. Anlamak için nasıl modülleri geliştirilen, dağıtılan ve tutulan yapmak modül ve kavramsal dört adede kadar aklınıza yardımcı olur:

* A **modülü görüntüsü** bir modül tanımlar ve yazılımı içeren bir paket.
* A **modül örneğinin** belirli bir IOT Edge cihazında modülü görüntüsü çalıştıran hesaplama birimidir. Modül örneğinin IOT Edge çalışma zamanı tarafından başlatılır.
* A **modülü kimlik** her modülü örneğine ilişkili olan IOT Hub'ındaki depolanan bilgileri (güvenlik kimlik bilgileri dahil) bir parçasıdır.
* A **modül ikizi** meta veriler, yapılandırmalar ve koşullar dahil olmak üzere bir modül örneğinin için durum bilgilerini içeren IOT Hub'ında depolanan bir JSON belgesidir. 

## <a name="module-images-and-instances"></a>Modül görüntüleri ve örnekleri

IOT Edge modülü görüntüler, yönetim, güvenlik ve IOT Edge çalışma zamanı iletişim özelliklerini yararlanarak uygulamalar içerir. Kendi modül görüntüleri geliştirin veya bir Azure Stream Analytics gibi desteklenen bir Azure hizmet verin.
Bulutta görüntü var ve bunlar güncelleştirilebilir, değiştirilmiş ve farklı çözümler dağıtılan. Örneğin, bir insansız hava aracı ile denetlemek için görüntü işleme kullanan bir modül daha ayrı bir görüntü olarak üretim satırı çıkışı tahmin etmek için makine öğrenimini kullanıyor bir modül bulunmaktadır. 

Bir modül görüntüsü bir cihaza dağıtılan ve IOT Edge çalışma zamanı tarafından başlatılan her zaman bu modülü yeni bir örneğini oluşturulur. İki cihazı dünyanın farklı bölümleri aynı modülü görüntüsü kullanabilirsiniz; Ancak, modül cihazda başlatıldığında her kendi modül örneğinin sahip olacaktır. 

![Modül görüntüleri - bulutta cihazlarda modülü örnekleri][1]

Uygulamada, modülleri görüntü deposundaki kapsayıcı görüntüleri olarak mevcut ve cihazlarda modülü örnekleri kapsayıcılardır. 

<!--
As use cases for Azure IoT Edge grow, new types of module images and instances will be created. For example, resource constrained devices cannot run containers so may require module images that exist as dynamic link libraries and instances that are executables. 
-->

## <a name="module-identities"></a>Modül kimlikleri

IOT Edge çalışma zamanı tarafından yeni bir modül örneği oluşturulduğunda, karşılık gelen bir modül kimliği ile ilişkili bir örneğidir. Modül kimliği IOT Hub'ında depolanan ve adresleme ve güvenlik kapsamı, tüm yerel ve bulut iletişimleri için belirli bir modül örneğinin olarak kullanılır.
Cihazın kimliğini bağımlı bir modül örneği ile ilişkili kimlik hangi örneğinin çalıştığından ve çözümünüzde bu modül için yazılan adı. Örneğin, eğer `insight` ve bir Azure Stream Analytics kullanan modül dağıtma, adlı bir cihazda `Hannover01`, IOT Edge çalışma zamanı adlı karşılık gelen bir modül kimliği oluşturur `/devices/Hannover01/modules/insight`.

Açıkça görülebileceği gibi senaryolarda, birden çok kez aynı cihazda, bir modül görüntüsünü dağıtmak ihtiyacınız olduğunda farklı adlar aynı görüntü birden çok kez dağıtabilirsiniz.

![Modül kimlikleri benzersiz][2]

## <a name="module-twins"></a>Modül ikizlerini

Her bir modül örneğinin modülü örneği yapılandırmak için kullanabileceğiniz karşılık gelen bir modül ikizi de vardır. Örnek ve ikizi birbiriyle modül kimliği ile ilişkilendirilir. 

Modül ikizi modülü bilgilerine ve yapılandırma özelliklerini depolayan bir JSON belgesidir. Bu kavramı parallels [cihaz ikizi] [ lnk-device-twin] kavramı IOT Hub'ından. Modül ikizi yapısını tam olarak bir cihaz çifti ile aynıdır. İkizlerini her iki tür ile etkileşim kurmak için kullanılan API'ler de aynı olur. İkisi arasındaki tek fark, istemci SDK'sı örneği oluşturmak için kullanılan kimliktir. 

```csharp
// Create a ModuleClient object. This ModuleClient will act on behalf of a 
// module since it is created with a module’s connection string instead 
// of a device connection string. 
ModuleClient client = new ModuleClient.CreateFromEnvironmentAsync(settings); 
await client.OpenAsync(); 
 
// Get the module twin 
Twin twin = await client.GetTwinAsync(); 
```

## <a name="offline-capabilities"></a>Çevrimdışı özellikler

Azure IOT Edge, IOT Edge cihazlarınıza çevrimdışı işlemleri destekler. Şimdilik bu özellikleri sınırlıdır. 

IOT Edge modülleri, aşağıdaki gereksinimlerin karşılandığından sürece uzun süreler çevrimdışı olabilir: 

* **İleti yaşam süresi (TTL) süresinin geçmemiş**. İki saat ileti TTL'si için varsayılan değer olan ancak Store içinde değiştirilen daha yüksek veya düşük ve IOT Edge yapılandırmasında hub ayarları iletebilir. 
* **Modüller, IOT Edge hub'ı ile çevrimdışı durumdayken yeniden kimlik doğrulamaya zorlayabilir gerekmez**. Modüller yalnızca IOT hub'ı etkin bir bağlantı olan Edge hub'ları ile kimlik doğrulaması yapabilir. Modüller için herhangi bir nedenle yeniden başlatıldığında yeniden kimlik doğrulamanız gerekir. Kimliklerini SAS belirteci süresi dolduktan sonra modülleri hala Edge hub'a iletileri gönderebilir. Bağlantı geri döndüğünde, Edge hub'ı yeni bir belirteç modülün istekleri ve IOT hub'ı ile doğrular. Başarılı olursa, Edge hub'ı saklanan, modül iletileri modülün belirtecin süresi sona erdi olsa bile gönderilen iletiler gönderir. 
* **Hata iletileri gönderilen modülü bağlantı çıktığında çevrimdışı hala çalıştığından**. Edge hub'ı, IOT Hub'ına bağlandıklarında (önceki bir tarihte dolduysa) modülü iletileri iletebilir önce yeni bir modül belirteci doğrulamak gerekir. Modül yeni bir belirteç sağlamak kullanılabilir durumda değilse, Edge hub'ı üzerinde depolanan iletilerinize modülün davranamaz. 
* **Edge hub'ı iletileri depolamak için disk alanı olan**. Varsayılan olarak, iletiler Edge hub'ı kapsayıcının dosya sistemi içinde saklanır. Bunun yerine iletileri depolamak için bir bağlı birimini belirtmek için bir yapılandırma seçeneği yoktur. Her iki durumda da var. ertelenmiş teslim IOT hub'ına iletileri depolamak kullanılabilir alan olması gerekir.  

Ek çevrimdışı özellikleri, genel önizlemede kullanılabilir. Daha fazla bilgi için [anlayın cihazları, modülleri ve alt cihazlar bu çevrimdışı özellikleri IOT Edge için Genişletilmiş](offline-capabilities.md).

## <a name="next-steps"></a>Sonraki adımlar
 - [IOT Edge modülleri geliştirmek için Araçlar ve gereksinimleri anlama][lnk-mod-dev]
 - [Azure IOT Edge çalışma zamanı ve mimarisini anlama][lnk-runtime]

<!-- Images -->
[1]: ./media/iot-edge-modules/image_instance.png
[2]: ./media/iot-edge-modules/identity.png

<!-- Links -->
[lnk-device-identity]: ../iot-hub/iot-hub-devguide-identity-registry.md
[lnk-device-twin]: ../iot-hub/iot-hub-devguide-device-twins.md
[lnk-runtime]: iot-edge-runtime.md
[lnk-mod-dev]: module-development.md
