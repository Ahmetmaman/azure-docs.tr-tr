---
title: Azure IoT Central cihaz köprüsü oluşturma | Microsoft Docs
description: Diğer IoT bulutlarını (Sigfox, parçacık, şeyler ağı vb.) IoT Central uygulamanıza bağlamak için IoT Central cihaz Köprüsü oluşturun.
services: iot-central
ms.service: iot-central
author: viv-liu
ms.author: viviali
ms.date: 07/09/2019
ms.topic: how-to
manager: peterpr
ms.custom: device-developer
ms.openlocfilehash: fc8ea41e804344735cfa2400d5d763622d8811c8
ms.sourcegitcommit: 10d00006fec1f4b69289ce18fdd0452c3458eca5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2020
ms.locfileid: "95026259"
---
# <a name="build-the-iot-central-device-bridge-to-connect-other-iot-clouds-to-iot-central"></a>Diğer IoT bulutlarını IoT Central için bağlamak üzere IoT Central cihaz Köprüsü oluşturun

*Bu konu, Yöneticiler için geçerlidir.*

IoT Central cihaz Köprüsü, Sigfox, parçacık, nesnelerin ağını ve diğer bulutlarınızı IoT Central uygulamanıza bağlayan açık kaynaklı bir çözümdür. Sigfox 'un düşük güç düzeyi alan ağına bağlı varlık izleme cihazları mı yoksa parçacık cihaz bulutu üzerinde hava kalitesi izleme cihazlarını mı kullandığınızı ya da TTN 'de SOIL nemi izleme cihazlarını kullanarak, IoT Central cihaz köprüsünü kullanarak IoT Central gücünden doğrudan yararlanabilirsiniz. Cihaz Köprüsü, cihazlarınızın diğer bulutlara gönderdiği verileri IoT Central uygulamanıza ileterek, diğer IoT bulutlarını IoT Central bağlar. IoT Central uygulamanızda kurallar oluşturabilir ve bu verilerde analiz çalıştırabilir, Power otomatikleştirin ve Azure Logic Apps 'te iş akışları oluşturabilir, bu verileri dışarı aktarabilir ve çok daha fazlasını yapabilirsiniz. GitHub 'dan [IoT Central cihaz Köprüsü](https://aka.ms/iotcentralgithubdevicebridge) al

## <a name="what-is-it-and-how-does-it-work"></a>Nedir ve nasıl çalışır?
IoT Central cihaz Köprüsü, GitHub 'daki açık kaynaklı bir çözümdür. Azure aboneliğinize birkaç Azure kaynağı olan özel bir Azure Resource Manager şablonu dağıtan bir "Azure 'a dağıt" düğmesine sahip olmaya başlamaya hazırız. Kaynaklar şunları içerir:
-    Azure Işlevi uygulaması
-    Azure Depolama Hesabı
-    Tüketim Planı
-    Azure Key Vault

İşlev uygulaması, cihaz Köprüsü 'nün kritik parçasıdır. Diğer IoT platformlarından veya basit bir Web kancası tümleştirmesi aracılığıyla herhangi bir özel platformda HTTP POST istekleri alır. Sigfox, parçacık ve TTN bulutlarının nasıl bağlanacağını gösteren örnekler sunuyoruz. Bu çözümü, platformunuz, işlev uygulamanıza HTTP POST istekleri gönderebiliyorsanız özel IoT bulutunuza bağlanmak üzere kolayca genişletebilirsiniz.
Işlev uygulaması, verileri IoT Central tarafından kabul edilen bir biçime dönüştürür ve bu dosyayı DPS API 'Leri aracılığıyla iletir.

![Azure işlevleri ekran görüntüsü](media/howto-build-iotc-device-bridge/azfunctions.png)

IoT Central uygulamanız, iletilen iletideki cihazı cihaz KIMLIĞINE göre tanırsa, bu cihaz için yeni bir ölçüm görünür. Cihaz KIMLIĞI IoT Central uygulamanız tarafından hiç görülmüyorsa, işlev uygulamanız bu cihaz KIMLIĞIYLE yeni bir cihaz kaydetmeye çalışır ve IoT Central uygulamanızda "Ilişkilendirilmemiş cihaz" olarak görünür. 

## <a name="how-do-i-set-it-up"></a>Nasıl yaparım? ayarla mı?
Yönergeler, GitHub deposundaki BENIOKU dosyasında ayrıntılı olarak listelenmiştir. 

## <a name="pricing"></a>Fiyatlandırma
Azure kaynakları Azure aboneliğinizde barınacaktır. [Benioku dosyasında](https://aka.ms/iotcentralgithubdevicebridge)fiyatlandırma hakkında daha fazla bilgi edinebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Artık IoT Central cihaz köprüsü oluşturmayı öğrendiğinize göre, önerilen sonraki adım aşağıda verilmiştir:

> [!div class="nextstepaction"]
> [Cihazlarınızı yönetme](howto-manage-devices.md)
