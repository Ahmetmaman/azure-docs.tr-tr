---
title: Otomatik sağlama DPS - Windows ile Azure IOT Edge cihazı | Microsoft Docs
description: Otomatik cihaz, cihaz sağlama hizmeti ile Azure IOT Edge için sağlama test etmek için Windows makinenizde bir simülasyon cihazı kullanın
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/27/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 46970d5628df3b46ec88df998a328928f60e15b4
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39090240"
---
# <a name="create-and-provision-a-simulated-tpm-edge-device-on-windows"></a>Windows üzerinde sanal bir TPM Edge cihazı oluşturma ve sağlama

Azure IOT Edge cihazları otomatik-sağlanabilir kullanarak [cihaz sağlama hizmeti](../iot-dps/index.yml) edge etkin olmayan cihazlar'olduğu gibi. Otomatik sağlama işlemine bilmiyorsanız gözden [otomatik sağlama kavramlarını](../iot-dps/concepts-auto-provisioning.md) devam etmeden önce. 

Bu makalede, test etmek otomatik sağlama aşağıdaki adımlarla sanal bir kenar cihazda gösterilmektedir: 

* Bir örnek, IOT Hub cihaz sağlama hizmeti (DPS) oluşturun.
* Donanım güvenlik için Windows makinenizde ile bir sanal Güvenilir Platform Modülü (TPM) bir simülasyon cihazı oluşturun.
* Cihazı için bireysel bir kayıt oluşturun.
* IOT Edge çalışma zamanını yükleyin ve cihazı IOT hub'a bağlayın.

## <a name="prerequisites"></a>Önkoşullar

* Bir Windows geliştirme makinesi. Bu makalede, Windows 10 kullanır. 
* Etkin bir IOT Hub. 

## <a name="set-up-the-iot-hub-device-provisioning-service"></a>IOT Hub cihazı sağlama hizmetini ayarlama

Azure'da yeni bir IOT Hub cihazı sağlama hizmeti örneğini oluşturun ve IOT hub'ınıza bağlayın. ' Ndaki yönergeleri takip edebilirsiniz [IOT hub'ı DPS ' ayarlamak](../iot-dps/quick-setup-auto-provision.md).

Cihaz sağlama hizmeti çalışıyor sonra değerini kopyalayın **kimlik kapsamı** genel bakış sayfasında. IOT Edge çalışma zamanı yapılandırdığınızda bu değeri kullanın. 

## <a name="simulate-a-tpm-device"></a>TPM cihazının benzetimini yapma

Windows geliştirme makinenizde sanal bir TPM cihazı oluşturma. Alma **kayıt kimliği** ve **onay anahtarını** , cihazınız için ve DPS'de bir bireysel kayıt girişi oluşturma için bunları kullanın. 

DPS'de bir kayıt oluşturduğunuzda, bildirme fırsatına sahip bir **ilk cihaz İkizi durumu**. Cihaz ikizinde bölge, ortam, konuma veya cihaz türü gibi çözümünüzdeki gereken herhangi bir ölçümü tarafından cihazları için etiketler ayarlayabilirsiniz. Bu etiketleri oluşturmak için kullanılan [otomatik dağıtımlar](how-to-deploy-monitor.md). 

Sanal cihazı oluşturmak için kullanmak istediğiniz SDK dil seçin ve bireysel kayıt oluşturana kadar adımları izleyin. 

Bireysel kayıt oluşturduğunuzda **etkinleştirme** bu sanal makine olduğunu bildirmek için bir **IOT Edge cihazı**.

Sanal cihazı ve bireysel kayıt kılavuzları: 
* [C](../iot-dps/quick-create-simulated-device.md)
* [Java](../iot-dps/quick-create-simulated-device-tpm-java.md)
* [C#](../iot-dps/quick-create-simulated-device-tpm-csharp.md)
* [Node.js](../iot-dps/quick-create-simulated-device-tpm-node.md)
* [Python](../iot-dps/quick-create-simulated-device-tpm-python.md)

Bireysel kayıt oluşturduktan sonra değerini kaydedin **kayıt kimliği**. IOT Edge çalışma zamanı yapılandırdığınızda bu değeri kullanın. 

## <a name="install-the-iot-edge-runtime"></a>IOT Edge çalışma zamanını yükleme

IoT Edge çalışma zamanı tüm IoT Edge cihazlarına dağıtılır. Bileşenleri kapsayıcılarında çalıştırmak ve kod ucuna çalıştırabilmeniz için cihaza ek kapsayıcıları dağıtma olanak sağlar. Windows çalıştıran cihazlarda ya da Windows kapsayıcıları ya da Linux kapsayıcıları'ı kullanmayı da tercih edebilirsiniz. Kullanmak istediğiniz kapsayıcıları türünü seçin ve adımları izleyin. Otomatik değil el ile sağlama için IOT Edge çalışma zamanı yapılandırdığınızdan emin olun. 

Sanal TPM önceki bölümden çalıştıran cihazın IOT Edge çalışma zamanı yüklemek için yönergeleri izleyin. 

DPS'niz bilmeniz **kimlik kapsamı** ve cihaz **kayıt kimliği** Bu makaleler başlamadan önce. 

* [Windows kapsayıcıları](how-to-install-iot-edge-windows-with-windows.md)
* [Linux kapsayıcıları](how-to-install-iot-edge-windows-with-linux.md)

## <a name="create-a-tpm-environment-variable"></a>TPM ortam değişkeni oluşturma

Sanal cihazınız çalıştıran makinede değişiklik **iotedge** bir ortam değişkenini ayarlamak için hizmet kayıt defteri.

1. Gelen **Başlat** menüsünde, açık **regedit**. 
2. Gidin **Computer\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\iotedge**. 
3. Seçin **Düzenle** > **yeni** > **çok dizeli değer**. 
4. Bir ad girin **ortam**. 
5. Yeni değişken çift tıklayın ve değer verisini ayarlayın **IOTEDGE_USE_TPM_DEVICE = ON**. 
6. Değişikliklerinizi kaydetmek için **Tamam**’a tıklayın. 

## <a name="restart-the-iot-edge-runtime"></a>IOT Edge çalışma zamanı yeniden başlatın

Bu cihaz üzerinde yaptığınız tüm yapılandırma değişiklikleri alır, böylece IOT Edge çalışma zamanı yeniden başlatın. 

```powershell
Stop-Service iotedge -NoWait
sleep 5
Start-Service iotedge
```

## <a name="verify-successful-installation"></a>Yüklemenin başarılı olduğunu doğrulamak

Çalışma zamanı başarıyla başlatıldı, IOT Hub'ına gidin ve yeni Cihazınızı otomatik olarak sağlandı ve IOT Edge modüllerini çalıştırmak hazırdır. 

IoT Edge hizmetinin durumunu kontrol edin.

```powershell
Get-Service iotedge
```

Son 5 dakika Hizmeti günlüklerini inceleyin.

```powershell
# Displays logs from last 5 min, newest at the bottom.

Get-WinEvent -ea SilentlyContinue `
  -FilterHashtable @{ProviderName= "iotedged";
    LogName = "application"; StartTime = [datetime]::Now.AddMinutes(-5)} |
  select TimeCreated, Message |
  sort-object @{Expression="TimeCreated";Descending=$false}
```

Çalışan modülleri listeleyin.

```powershell
iotedge list
```

## <a name="next-steps"></a>Sonraki adımlar

Cihaz sağlama hizmeti kayıt işlemi, yeni cihaz sağlama gibi cihaz kimliği ve cihaz ikizi etiketleri aynı anda belirlemenizi sağlar. Bu değerleri ayrı ayrı cihazlar ya da otomatik cihaz Yönetimi'ni kullanarak cihaz grupları hedeflemek için kullanabilirsiniz. Bilgi edinmek için nasıl [dağıtma ve izleme IOT Edge modülleri, ölçeklendirme Azure portalını kullanarak](how-to-deploy-monitor.md) veya [Azure CLI kullanarak](how-to-deploy-monitor-cli.md)