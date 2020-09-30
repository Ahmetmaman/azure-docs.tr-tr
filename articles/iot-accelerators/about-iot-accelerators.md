---
title: IoT çözüm hızlandırıcılarına giriş-Azure | Microsoft Docs
description: Azure IoT çözüm hızlandırıcıları hakkında bilgi edinin. IoT çözüm hızlandırıcıları, IoT çözümlerini dağıtmak için kullanılan tam kapsamlı, uçtan uca ve dağıtıma hazır sistemlerdir.
author: dominicbetts
ms.author: dobett
ms.date: 03/09/2019
ms.topic: overview
ms.custom: mvc
ms.service: iot-accelerators
services: iot-accelerators
manager: timlt
ms.openlocfilehash: a8314fed43c8282c536fc6e95e6c3aa877408b09
ms.sourcegitcommit: f5580dd1d1799de15646e195f0120b9f9255617b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/29/2020
ms.locfileid: "91542504"
---
# <a name="what-are-azure-iot-solution-accelerators"></a>Azure IoT çözüm hızlandırıcıları nedir?

Bulut tabanlı bir IoT çözümü genellikle cihaz bağlantısı, veri işleme ve analiz ve sunum işlemlerini yönetmek için özel kod ve bulut hizmetleri kullanır.

IoT çözüm hızlandırıcıları sık kullanılan IoT senaryolarını uygulayan tam kapsamlı ve dağıtıma hazır IoT çözümleridir. Senaryolar uzaktan izleme, bağlı fabrika, tahmine dayalı bakım ve cihaz benzetimini içerir. Bir çözüm hızlandırıcısını dağıttığınızda gerekli tüm bulut tabanlı hizmetler ve gerekli uygulama kodları dağıtılır.

Çözüm hızlandırıcıları, kendi IoT çözümlerinizi oluşturmak için başlangıç noktalarıdır. Tüm çözüm hızlandırıcılarının kaynak kodu açık kaynaktır ve GitHub üzerinde mevcuttur. Çözüm hızlandırıcılarını indirmeniz ve gereksinimlerinize göre özelleştirmeniz önerilir.

Sıfırdan özel bir IoT çözümü derlemeden önce çözüm hızlandırıcılarını eğitim aracı olarak kullanabilirsiniz. Çözüm hızlandırıcıları, bulut tabanlı IoT çözümleri için uygulayabileceğiniz kanıtlanmış yöntemler sunar.

Her çözüm hızlandırıcısındaki uygulama kodunda çözüm hızlandırıcısını yönetmenizi sağlayan bir web uygulaması bulunur.

## <a name="supported-iot-scenarios"></a>Desteklenen IoT senaryoları

Şu anda dağıtıma hazır dört çözüm hızlandırıcısı vardır:

### <a name="remote-monitoring"></a>Uzaktan İzleme

Uzak cihazlardan telemetri toplamak ve bunları denetlemek için [Uzaktan izleme çözümü hızlandırıcıyı](iot-accelerators-remote-monitoring-sample-walkthrough.md) kullanın. Cihazlara örnek olarak müşterilerinizin tesislerindeki soğutma sistemleri veya uzak pompa istasyonlarındaki valfler verilebilir.

Uzaktan izleme panosunu kullanarak bağlı cihazlarınızdan gelen telemetri verilerini görüntüleyebilir, yeni cihazlar sağlayabilir veya bağlı cihazlarınızdaki üretici yazılımını yükseltebilirsiniz:

[![Uzaktan izleme çözüm panosu](./media/about-iot-accelerators/rm-dashboard-inline.png)](./media/about-iot-accelerators/rm-dashboard-expanded.png#lightbox)

### <a name="connected-factory"></a>Bağlı Fabrika

[OPC Birleşik mimari](https://opcfoundation.org/about/opc-technologies/opc-ua/) arabirimiyle endüstriyel varlıklardan telemetri toplamak ve bunları denetlemek Için [bağlı fabrika çözümü hızlandırıcıyı](iot-accelerators-connected-factory-features.md) kullanın. Endüstriyel varlıklar arasında bir fabrikanın üretim hattındaki montaj ve test istasyonları olabilir.

Bağlı fabrika panonuzu kullanarak endüstriyel cihazlarınızı izleyebilir ve yönetebilirsiniz:

:::image type="content" source="./media/about-iot-accelerators/cf-dashboard-inline.png" alt-text="Bağlı fabrika çözümü panosunu gösteren ekran görüntüsü." lightbox="./media/about-iot-accelerators/cf-dashboard-expanded.png":::

### <a name="predictive-maintenance"></a>Tahmine Dayalı Bakım

Bir uzak cihazın başarısız olması beklendiğini tahmin etmek için tahmine [dayalı bakım çözümü hızlandırıcıyı](iot-accelerators-predictive-walkthrough.md) kullanın, böylece cihaz başarısız olmadan önce bakım gerçekleştirebilirsiniz. Bu çözüm hızlandırıcısı, cihazların telemetri verilerini kullanarak arıza tahmini gerçekleştirmek için makine öğrenimi algoritmalarını kullanır. Örnek cihazlar uçak motorları veya asansörler olabilir.

Tahmine dayalı bakım panosunu kullanarak tahmine dayalı bakım analizlerini görüntüleyebilirsiniz:

:::image type="content" source="./media/about-iot-accelerators/pm-dashboard-inline.png" alt-text="Bağlı fabrika çözümü panosunu gösteren ekran görüntüsü." lightbox="./media/about-iot-accelerators/pm-dashboard-expanded.png":::

### <a name="device-simulation"></a>Cihaz Benzetimi

Gerçekçi telemetri üreten sanal cihazları çalıştırmak için [cihaz benzetimi çözüm hızlandırıcıyı](iot-accelerators-device-simulation-overview.md) kullanın. Bu çözüm hızlandırıcısını kullanarak diğer çözüm hızlandırıcılarının davranışını veya kendi IoT çözümlerinizi test edebilirsiniz.

Cihaz benzetimi web uygulamasını kullanarak benzetimlerinizi yapılandırabilir ve çalıştırabilirsiniz:

[![Bağlı fabrika çözümü panosu](./media/about-iot-accelerators/ds-dashboard-inline.png)](./media/about-iot-accelerators/ds-dashboard-expanded.png#lightbox)

## <a name="design-principles"></a>Tasarım ilkeleri

Tüm çözüm hızlandırıcıları aynı tasarım ilkelerini ve hedeflerini takip etmektedir. Bu bileşenler şu şekilde tasarlanmıştır:

* **Ölçeklenebilir**: Milyonlarca cihaz bağlamanıza ve yönetmenize izin verir.
* **Genişletilebilir**: Çözümleri ihtiyaçlarınıza göre özelleştirmenizi sağlar.
* **Anlaşılır**: Çözümlerin nasıl çalıştığını ve nasıl uygulandığını kavramanızı sağlar.
* **Modüler**: Hizmetleri alternatifleriyle değiştirmeniz izin verir.
* **Güvenli**: Azure güvenliğini yerleşik bağlantı ve cihaz güvenliği özellikleriyle birleştirir.

## <a name="architectures-and-languages"></a>Mimariler ve diller

Özgün çözüm hızlandırıcıları .NET ile model-görünüm-denetleyici (MVC) mimarisi kullanılarak yazılmıştır. Microsoft, çözüm hızlandırıcıları yeni bir mikro hizmet mimarisiyle güncelleştirmektedir. Aşağıdaki tabloda, çözüm hızlandırıcılarının geçerli durumu ve GitHub deposu bağlantıları gösterilmektedir:

| Çözüm hızlandırıcısı   | Mimari  | Diller     |
| ---------------------- | ------------- | ------------- |
| Uzaktan İzleme      | Mikro hizmetler | [Java](https://github.com/Azure/azure-iot-pcs-remote-monitoring-java) ve [.NET](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet) |
| Tahmine Dayalı Bakım | MVC           | [.NET](https://github.com/Azure/azure-iot-predictive-maintenance)          |
| Bağlı Fabrika      | MVC           | [.NET](https://github.com/Azure/azure-iot-connected-factory)          |
| Cihaz Benzetimi      | Mikro hizmetler | [.NET](https://github.com/Azure/device-simulation-dotnet)          |

Mikro hizmet mimarisi hakkında daha fazla bilgi edinmek için bkz. [Azure IoT başvuru mimarisine Giriş](https://docs.microsoft.com/azure/architecture/reference-architectures/iot/).

## <a name="deployment-options"></a>Dağıtım seçenekleri

Çözüm hızlandırıcılarını [Microsoft Azure IoT Çözüm Hızlandırıcıları](https://www.azureiotsolutions.com/Accelerators#) sitesinden veya komut satırını kullanarak dağıtabilirsiniz.

Uzaktan İzleme çözümü hızlandırıcısını aşağıdaki yapılandırmalarla dağıtabilirsiniz:

* **Standart:** Bir üretim dağıtımı geliştirmek için genişletilmiş altyapı dağıtımı. Azure Container Service mikro hizmetleri birkaç Azure sanal makinesine dağıtır. Kubernetes mikro hizmetleri tek tek barındıran Docker kapsayıcılarını düzenler.
* **Temel:** Tanıtım için veya bir dağıtımı test etmek için daha düşük maliyetli sürüm. Tüm mikro hizmetler tek bir Azure sanal makinesine dağıtılır.
* **Yerel:** Test ve geliştirme için yerel makineye dağıtma. Bu yaklaşımda mikro hizmetler yerel bir Docker kapsayıcısına dağıtılır ve buluttaki IoT Hub, Azure Cosmos DB ve Azure depolama hizmetlerine bağlanır.

Çözüm Hızlandırıcısı çalıştırmanın maliyeti, [temel alınan Azure hizmetlerini çalıştırmanın Birleşik maliyetlidir](https://azure.microsoft.com/pricing). Kullanılan Azure hizmetlerinin ayrıntılarını dağıtım seçeneklerinizi belirlerken görebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

IoT çözüm hızlandırıcılarından birini denemek için hızlı başlangıçları inceleyin:

* [Uzaktan izleme çözümü deneyin](quickstart-remote-monitoring-deploy.md)
* [Bağlı fabrika çözümünü deneyin](quickstart-connected-factory-deploy.md)
* [Tahmine dayalı bakım çözümünü deneyin](quickstart-predictive-maintenance-deploy.md)
* [Cihaz benzetimi çözümünü deneyin](quickstart-device-simulation-deploy.md)
