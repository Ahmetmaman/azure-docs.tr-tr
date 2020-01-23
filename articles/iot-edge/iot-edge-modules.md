---
title: Modüller mantıksal cihazlarınızda - Azure IOT Edge çalışma şeklini öğrenin | Microsoft Docs
description: Azure IOT Edge modüllerini dağıtılabilir ve böylece IOT Edge üzerinde iş mantığını cihazlara çalıştırabilirsiniz yönetilmesine mantığı kapsayıcılı birimlerin
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 03/21/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 1c625e628f53d156ad56a1c69df1c23aec9120ac
ms.sourcegitcommit: 87781a4207c25c4831421c7309c03fce5fb5793f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2020
ms.locfileid: "76548722"
---
# <a name="understand-azure-iot-edge-modules"></a>Azure IoT Edge modüllerini anlama

Azure IOT Edge dağıtma ve yönetmenize olanak sağlar biçiminde edge üzerinde iş mantığını *modülleri*. Azure IOT Edge modülleri, IOT Edge tarafından yönetilen bir hesaplama birimi en küçük olan ve Azure hizmetlerinin (örneğin, Azure Stream Analytics) veya kendi çözümünüze özgü kodla içerebilir. Modüllerin nasıl geliştirildiğini, dağıtıldığını ve yapıldığını anlamak için, bir modülün dört kavramsal öğesini göz önünde bulundurun:

* A **modülü görüntüsü** bir modül tanımlar ve yazılımı içeren bir paket.
* A **modül örneğinin** belirli bir IOT Edge cihazında modülü görüntüsü çalıştıran hesaplama birimidir. Modül örneğinin IOT Edge çalışma zamanı tarafından başlatılır.
* A **modülü kimlik** her modülü örneğine ilişkili olan IOT Hub'ındaki depolanan bilgileri (güvenlik kimlik bilgileri dahil) bir parçasıdır.
* A **modül ikizi** meta veriler, yapılandırmalar ve koşullar dahil olmak üzere bir modül örneğinin için durum bilgilerini içeren IOT Hub'ında depolanan bir JSON belgesidir.

## <a name="module-images-and-instances"></a>Modül görüntüleri ve örnekleri

IOT Edge modülü görüntüler, yönetim, güvenlik ve IOT Edge çalışma zamanı iletişim özelliklerini yararlanarak uygulamalar içerir. Kendi modül görüntüleri geliştirin veya bir Azure Stream Analytics gibi desteklenen bir Azure hizmet verin.
Bulutta görüntü var ve bunlar güncelleştirilebilir, değiştirilmiş ve farklı çözümler dağıtılan. Örneğin, bir insansız hava aracı ile denetlemek için görüntü işleme kullanan bir modül daha ayrı bir görüntü olarak üretim satırı çıkışı tahmin etmek için makine öğrenimini kullanıyor bir modül bulunmaktadır.

Bir modül görüntüsü bir cihaza dağıtılan ve IOT Edge çalışma zamanı tarafından başlatılan her zaman bu modülü yeni bir örneğini oluşturulur. Dünyanın farklı kısımlarındaki iki cihaz aynı modül görüntüsünü kullanabilir. Ancak, modül cihazda başlatıldığında her cihazın kendi modül örneği olur.

![Diyagram - bulutta modül görüntüleri, cihazlarda modülü örnekleri](./media/iot-edge-modules/image_instance.png)

Uygulamada, modülleri görüntü deposundaki kapsayıcı görüntüleri olarak mevcut ve cihazlarda modülü örnekleri kapsayıcılardır.

<!--
As use cases for Azure IoT Edge grow, new types of module images and instances will be created. For example, resource constrained devices cannot run containers so may require module images that exist as dynamic link libraries and instances that are executables. 
-->

## <a name="module-identities"></a>Modül kimlikleri

IoT Edge çalışma zamanı tarafından yeni bir modül örneği oluşturulduğunda, buna karşılık gelen bir modül kimliği alır. Modül kimliği IoT Hub depolanır ve söz konusu modül örneği için tüm yerel ve bulut iletişimleri için adresleme ve güvenlik kapsamı olarak kullanılır.

Cihazın kimliğini bağımlı bir modül örneği ile ilişkili kimlik hangi örneğinin çalıştığından ve çözümünüzde bu modül için yazılan adı. Örneğin, eğer `insight` ve bir Azure Stream Analytics kullanan modül dağıtma, adlı bir cihazda `Hannover01`, IOT Edge çalışma zamanı adlı karşılık gelen bir modül kimliği oluşturur `/devices/Hannover01/modules/insight`.

Açıkça görülebileceği gibi senaryolarda, birden çok kez aynı cihazda, bir modül görüntüsünü dağıtmak ihtiyacınız olduğunda farklı adlar aynı görüntü birden çok kez dağıtabilirsiniz.

![Diyagram - modülü kimlikleri benzersiz cihazları içinde ve cihazlar arasında](./media/iot-edge-modules/identity.png)

## <a name="module-twins"></a>Modül ikizlerini

Her bir modül örneğinin modülü örneği yapılandırmak için kullanabileceğiniz karşılık gelen bir modül ikizi de vardır. Örnek ve ikizi birbiriyle modül kimliği ile ilişkilendirilir.

Modül ikizi modülü bilgilerine ve yapılandırma özelliklerini depolayan bir JSON belgesidir. Bu kavramı parallels [cihaz ikizi](../iot-hub/iot-hub-devguide-device-twins.md) kavramı IOT Hub'ından. İkizi modülünün yapısı bir Device ikizi ile aynıdır. İkizlerini her iki tür ile etkileşim kurmak için kullanılan API'ler de aynı olur. İkisi arasındaki tek fark, istemci SDK'sı örneği oluşturmak için kullanılan kimliktir.

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

Azure IoT Edge modüller, en az bir kez IoT Hub eşitlemeden sonra sonsuza kadar çevrimdışı olarak çalışabilir. IoT Edge cihazlar, bu çevrimdışı özelliği diğer IoT cihazlarına da genişletebilir. Daha fazla bilgi için [anlayın cihazları, modülleri ve alt cihazlar bu çevrimdışı özellikleri IOT Edge için Genişletilmiş](offline-capabilities.md).

## <a name="next-steps"></a>Sonraki adımlar

* [IOT Edge modülleri geliştirmek için Araçlar ve gereksinimleri anlama](module-development.md)
* [Azure IOT Edge çalışma zamanı ve mimarisini anlama](iot-edge-runtime.md)
