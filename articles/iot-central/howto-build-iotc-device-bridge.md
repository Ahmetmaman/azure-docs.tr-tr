---
title: Azure IOT Central cihaz köprüsü oluşturun | Microsoft Docs
description: IOT Central, IOT Central uygulamasına (Sigfox, Parçacık, şeyler ağ vb.) diğer IOT bulutlara bağlanmak için cihaz köprü oluşturun.
services: iot-central
ms.service: iot-central
author: viv-liu
ms.author: viviali
ms.date: 12/4/2018
ms.topic: conceptual
manager: peterpr
ms.openlocfilehash: 9c774a463264a3df859ac097dce4aa21df1c1dd8
ms.sourcegitcommit: efcd039e5e3de3149c9de7296c57566e0f88b106
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2018
ms.locfileid: "53163370"
---
# <a name="build-the-iot-central-device-bridge-to-connect-other-iot-clouds-to-iot-central"></a>IOT Central, IOT Central için diğer IOT bulutlara bağlanmak için cihaz köprüsü oluşturun

*Bu konu, Yöneticiler için geçerlidir.*

IOT Central cihaz köprüsü, Sigfox, Parçacık, şeyler ağ ve diğer bulutlarda IOT Central uygulamanıza bağlanan bir açık kaynak çözümüdür. Varlık Sigfox'ın düşük güç geniş alan ağına bağlı cihazları izleme kullandığınız ya da parçacık cihaz bulut cihazlarda izleme veya TTN cihazlarda izleme Toprak nem kullanarak uzaktan kalite kullanarak doğrudan yapabilecekleriniz IOT gücünden yararlanın IOT Central cihaz Köprüsü'nü kullanarak orta. Cihaz köprüsü, diğer bulut IOT Central uygulamanıza cihazlarınıza gönderin veri ileterek, IOT Central ile diğer IOT bulut bağlanır. IOT Central uygulamanızda derleme kuralları ve bu veriler üzerinde analiz çalıştırın, Microsoft Flow ve Azure Logic apps'te iş akışları oluşturun, yapabilirsiniz verileri ve daha fazlasını dışarı aktarın. Alma [IOT Central cihaz köprüsü](https://aka.ms/iotcentralgithubdevicebridge) github'dan

## <a name="what-is-it-and-how-does-it-work"></a>Nedir ve nasıl çalışır?
IOT Central cihaz köprüsü, github'da açık kaynaklı bir çözümdür. Çeşitli Azure kaynakları ile özel bir Azure Resource Manager şablonu Azure aboneliğinize dağıtan bir "Azure'a dağıtın" düğmesi ile kullanıma hazır. Kaynakları şunlardır:
-   Azure işlev uygulaması
-   Azure Depolama Hesabı
-   Tüketim Planı
-   Azure Key Vault işlev uygulaması, cihaz köprüsü kritik parçasıdır. Diğer IOT platformları veya basit bir Web kancası tümleştirmesi aracılığıyla özel hiçbir platforma HTTP POST isteği alır. Sigfox ve parçacık TTN bulutlara bağlanma gösteren örnekler sağladık. Platformunuza işlev uygulamanız için HTTP POST istekleri gönderebilir, özel IOT bulutuna bağlanmak için bu çözümü kolayca genişletebilirsiniz.
İşlev uygulaması verileri IOT Central tarafından kabul edilen bir biçime dönüştürür ve boyunca DPS API'leri iletir.

![Azure işlevleri ekran görüntüsü](media/howto-build-iotc-device-bridge/azfunctions.png)

IOT Central uygulamanızı yönlendirilmiş ileti cihaz tarafından cihaz Kimliğini tanıyorsa bu cihaz için yeni bir ölçüm görünür. Cihaz kimliği, IOT Central uygulamanız tarafından hiçbir zaman görüldü, işlev uygulamanızı yeni bir cihaz, cihaz kimliği ile kaydolmayı deneyecek ve IOT Central uygulamanızda "ilişkilendirilmemiş aygıt" olarak görünür. 

## <a name="how-do-i-set-it-up"></a>Nasıl, ayarladığım?
Ayrıntılı GitHub deposunda ve benioku dosyasındaki yönergeleri listelenir. 

## <a name="pricing"></a>Fiyatlandırma
Azure aboneliğinizdeki Azure kaynaklarını barındırılacak. İçinde fiyatlandırması hakkında daha fazla bilgi [Benioku dosyası](https://aka.ms/iotcentralgithubdevicebridge).

## <a name="next-steps"></a>Sonraki adımlar
IOT Central cihaz köprü oluşturulacağını öğrendiniz, önerilen sonraki adım aşağıda verilmiştir:

> [!div class="nextstepaction"]
> [Cihazlarınızı yönetme](howto-manage-devices.md)
