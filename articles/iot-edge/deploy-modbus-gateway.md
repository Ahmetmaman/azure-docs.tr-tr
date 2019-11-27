---
title: Ağ geçidi - Azure IOT Edge modbus protokollerini çevirme | Microsoft Docs
description: IoT Edge ağ geçidi cihazı oluşturarak Modbus TCP kullanan cihazların Azure IoT Hub ile iletişim kurmasına imkan sağlama
author: kgremban
manager: philmea
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 06/28/2019
ms.author: kgremban
ms.openlocfilehash: 649c7f620b83464d1bb56cf4b8191b0747105f01
ms.sourcegitcommit: 12d902e78d6617f7e78c062bd9d47564b5ff2208
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/24/2019
ms.locfileid: "74457204"
---
# <a name="connect-modbus-tcp-devices-through-an-iot-edge-device-gateway"></a>Bir IOT Edge ağ geçidi cihazı aracılığıyla Modbus TCP cihazlarını bağlama

Modbus TCP veya RTU protokollerini kullanan IoT cihazlarını bir Azure IoT hub’ına bağlamak istiyorsanız ağ geçidi olarak bir IoT Edge cihazı kullanın. Ağ geçidi cihazı Modbus cihazlarınızdan verileri okur, sonra desteklenen bir protokolü kullanarak bu verileri buluta iletir.

![Modbus Cihazları IoT Edge Ağ Geçidi üzerinden IoT Hub bağlanır](./media/deploy-modbus-gateway/diagram.png)

Bu makalede, bir Modbus modülü için kendi kapsayıcı görüntünüzü oluşturma (dilerseniz önceden oluşturulmuş bir örneği de kullanabilirsiniz) ve bu görüntüyü ağ geçidi olarak kullanacağınız IoT Edge cihazına dağıtma işlemleri açıklanmaktadır.

Bu makalede Modbus TCP protokolünü kullandığınız varsayılır. Modülün Modbus RTU 'yi destekleyecek şekilde nasıl yapılandırılacağı hakkında daha fazla bilgi için bkz. GitHub 'da [Azure IoT Edge Modbus modülü](https://github.com/Azure/iot-edge-modbus) projesi.

## <a name="prerequisites"></a>Önkoşullar
* Bir Azure IoT Edge cihazı. Bir adım ayarlama hakkında yönergeler için bkz. Windows veya [Linux](quickstart-linux.md) [üzerinde Azure IoT Edge dağıtma](quickstart.md) .
* IoT Edge cihazı için birincil anahtar bağlantı dizesi.
* Modbus TCP’yi destekleyen fiziksel veya sanal bir cihaz.

## <a name="prepare-a-modbus-container"></a>Modbus kapsayıcısı hazırlama

Modbus ağ geçidinin işlevselliğini test etmek istiyorsanız Microsoft tarafından sağlanan örnek modülü kullanabilirsiniz. Modüle Azure Marketi, [Modbus](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft_iot.edge-modbus?tab=Overview)veya görüntü urı 'si **MCR.Microsoft.com/azureiotedge/Modbus:1.0**ile erişebilirsiniz.

Kendi modülünüzü oluşturmak ve ortamınız için özelleştirmek istiyorsanız, GitHub 'da açık kaynaklı [Azure IoT Edge Modbus modülü](https://github.com/Azure/iot-edge-modbus) projesi vardır. Kendi kapsayıcı görüntünüzü oluşturmak için bu projedeki yönergeleri izleyin. Bir kapsayıcı görüntüsü oluşturmak için, [Visual Studio 'da C# modül geliştirme](how-to-visual-studio-develop-csharp-module.md) veya [Visual Studio Code modüller geliştirme](how-to-vs-code-develop-module.md)bölümüne bakın. Bu makaleler, yeni modüller oluşturma ve kapsayıcı görüntülerini bir kayıt defterine yayımlama hakkında yönergeler sağlar.

## <a name="try-the-solution"></a>Çözümü deneyin

Bu bölümde Microsoft 'un örnek Modbus modülünün IoT Edge cihazınıza dağıtımı gösterilmektedir.

1. [Azure portalında](https://portal.azure.com/) IoT hub'ınıza gidin.

2. **IoT Edge** gidin ve IoT Edge cihazınıza tıklayın.

3. **Modül ayarla**’yı seçin.

4. Modbus modülünü ekleyin:

   1. **Ekle** ' ye tıklayın ve **IoT Edge modülü**' nü seçin.

   2. **Ad** alanına "modbus" yazın.

   3. **Resim** alanına örnek kapsayıcının resim URI’sini girin: `mcr.microsoft.com/azureiotedge/modbus:1.0`.

   4. Modül ikizinin istenen özelliklerini güncelleştirmek için **Etkinleştir** kutusunu işaretleyin.

   5. Aşağıdaki JSON’u metin kutusuna kopyalayın. **SlaveConnection** değerini Modbus cihazınızın IPv4 adresi olarak değiştirin.

      ```JSON
      {
        "properties.desired":{
          "PublishInterval":"2000",
          "SlaveConfigs":{
            "Slave01":{
              "SlaveConnection":"<IPV4 address>","HwId":"PowerMeter-0a:01:01:01:01:01",
              "Operations":{
                "Op01":{
                  "PollingInterval": "1000",
                  "UnitId":"1",
                  "StartAddress":"40001",
                  "Count":"2",
                  "DisplayName":"Voltage"
                }
              }
            }
          }
        }
      }
      ```

   6. **Kaydet**’i seçin.

5. **Modül Ekle** adımına dönüp **İleri**’yi seçin.

7. **Yolları Belirtin** adımında, aşağıdaki JSON’u metin kutusuna kopyalayın. Bu yol, Modbus modülü tarafından toplanan tüm iletileri IoT Hub’a gönderir. Bu rotada, **Modbusoutput** , modübus modülünün verileri çıkarmak için kullandığı uç noktadır ve **$upstream** IoT Edge hub 'ına IoT Hub ileti göndermesini söyleyen özel bir hedefdir.

   ```JSON
   {
     "routes": {
       "modbusToIoTHub":"FROM /messages/modules/modbus/outputs/modbusOutput INTO $upstream"
     }
   }
   ```

8. **İleri**’yi seçin.

9. **Dağıtımı Gözden Geçirin** adımında **Gönder**'i seçin.

10. Cihaz ayrıntıları sayfasına dönüp **Yenile**’yi seçin. IoT Edge çalışma zamanı ile birlikte çalışan yeni **Modbus** modülünü görmeniz gerekir.

## <a name="view-data"></a>Verileri görüntüleme
Modbus modülü üzerinden gelen verileri görüntüleyin:
```cmd/sh
iotedge logs modbus
```

Ayrıca, cihazın gönderdiği Telemetriyi, [Visual Studio Code Için azure IoT Hub araç seti uzantısını](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) (eskiden Azure IoT araç seti uzantısı) kullanarak da görüntüleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- IoT Edge cihazların ağ geçitleri olarak nasıl davranabileceği hakkında daha fazla bilgi edinmek için bkz. [saydam ağ geçidi olarak davranan bir IoT Edge cihaz oluşturma](./how-to-create-transparent-gateway.md).
- IoT Edge modüllerinin nasıl çalıştığı hakkında daha fazla bilgi için bkz. [Azure IoT Edge modüllerini anlama](iot-edge-modules.md).
