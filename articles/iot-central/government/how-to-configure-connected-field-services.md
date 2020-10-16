---
title: Azure IoT Central uygulamanızı Dynamics 365 alan hizmetleriyle bağlama | Microsoft Docs
description: Azure IoT Central ve Dynamics 365 alan hizmeti ile uçtan uca bir çözüm oluşturmayı öğrenin
author: miriambrus
ms.author: miriamb
ms.date: 10/23/2019
ms.topic: how-to
ms.service: iot-central
services: iot-central
ms.openlocfilehash: 2b3e006f717ed1c66c7db29dbd70c226d2d75ea8
ms.sourcegitcommit: 7dacbf3b9ae0652931762bd5c8192a1a3989e701
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2020
ms.locfileid: "92127225"
---
# <a name="build-end-to-end-solution-with-azure-iot-central-and-dynamics-365-field-service"></a>Azure IoT Central ve Dynamics 365 alan hizmeti ile uçtan uca çözüm oluşturma 



Bir Oluşturucu olarak, Azure IoT Central uygulamanızın diğer iş sistemleriyle tümleştirilmesine olanak sağlayabilirsiniz. 


Örneğin, bağlı bir çöp yönetimi çözümünde çöp koleksiyonları kamyonları gönderimi iyileştirebilirsiniz. İyileştirme, bağlı çöp kutularının IoT algılayıcı verilerine göre yapılabilir. [IoT Central bağlı çöp yönetimi uygulamanızda](./tutorial-connected-waste-management.md) , kuralları ve eylemleri yapılandırabilir ve bu ayarı Dynamics alan hizmeti 'nde uyarı oluşturmayı tetikleyebilirsiniz. Bu senaryo, uygulamalar ve hizmetler genelinde iş akışlarını otomatikleştirmek için IoT Central doğrudan yapılandırılacak olan Microsoft Flow kullanılarak gerçekleştirilir. Ayrıca, alan hizmetindeki hizmet etkinliklerine bağlı olarak, bilgiler Azure IoT Central 'ye gönderilebilir. 

## <a name="how-to-connect-your-azure-iot-central-application-with-dynamics-365-field-services"></a>Azure IoT Central uygulamanızı Dynamics 365 alan hizmetleriyle bağlama 

Aşağıdaki tümleştirme işlemlerine, saf bir yapılandırma deneyimine göre kolayca uygulanabilirler:
* Azure IoT Central, Tanılama için bağlı alan hizmeti (IoT uyarısı olarak) ile cihaz bozuklukları hakkında bilgi gönderebilir.
* Bağlı alan hizmeti, cihaz bozuklularından tetiklenen servis talepleri veya iş emirleri oluşturabilir.
* Bağlı alan hizmeti, kapalı kalma süresinin oluşmasını önlemeye yönelik teknisyenleri İnceleme için zamanlayabilir.
* Azure IoT Central cihaz panosu, ilgili hizmet ve zamanlama bilgileriyle güncelleştirilir.


## <a name="next-steps"></a>Sonraki adımlar
* [IoT Central kamu şablonları](./overview-iot-central-government.md) hakkında daha fazla bilgi edinin
* [IoT Central](../core/overview-iot-central.md) hakkında daha fazla bilgi edinin
* [Dynamics 365 alan Hizmetleri](/dynamics365/field-service/cfs-iot-overview) hakkında daha fazla bilgi edinin