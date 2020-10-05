---
title: Cihaz benzetimi çözümünü deneme ve çalıştırma-Azure | Microsoft Docs
description: Bu hızlı başlangıçta, Azure IoT Cihaz Benzetimini dağıtır ve bir benzetim çalıştırırsınız
author: troyhopwood
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: quickstart
ms.custom: mvc
ms.date: 03/08/2019
ms.author: troyhop
ms.openlocfilehash: 68e0c3c89698bafbab65ca786231e0364900213a
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2020
ms.locfileid: "65467871"
---
# <a name="quickstart-deploy-and-run-an-iot-device-simulation-in-azure"></a>Hızlı Başlangıç: Azure’da IoT cihaz benzetimini dağıtma ve çalıştırma

Bu hızlı başlangıçta, IoT çözümünüzü test etmek için Azure IoT Cihaz Benzetimini dağıtma işlemi gösterilir. Çözüm hızlandırıcısını dağıttıktan sonra çalışmaya başlamak için bir örnek benzetim çalıştırırsınız.

Bu hızlı başlangıcı tamamlamak etkin bir Azure aboneliğinizin olması gerekir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="deploy-device-simulation"></a>Cihaz Benzetimini Dağıtma

Cihaz Benzetimini Azure aboneliğinize dağıttığınızda ayarlamanız gereken yapılandırma seçenekleri vardır.

Azure hesabınızın kimlik bilgilerini kullanarak [azureiotsolutions.com](https://www.azureiotsolutions.com/Accelerators) adresinden oturum açın.

**Cihaz Benzetimi** kutucuğuna tıklayın:

![Cihaz Benzetimi seçme](./media/quickstart-device-simulation-deploy/devicesimulation.png)

Cihaz Benzetimi açıklama sayfasındaki **Şimdi Deneyin**’e tıklayın:

![Şimdi Deneyin öğesine tıklayın](./media/quickstart-device-simulation-deploy/devicesimulationpdp1.png)

**Cihaz Benzetimi çözümü oluşturun** sayfasına benzersiz bir **Çözüm adı** girin.

Çözüm hızlandırıcısını dağıtırken kullanmak istediğiniz **Subscription** (Abonelik) ve **Region** (Bölge) seçimini yapın. Genelde size en yakın bölgeyi seçmeniz gerekir. Abonelikte [genel yönetici veya kullanıcı](iot-accelerators-permissions.md) olmanız gerekir.

Cihaz Benzetimi çözümünüzle kullanabileceğiniz bir IoT hub’ı dağıtmak için kutuyu işaretleyin. Benzetiminizin kullandığı IoT hub’ını istediğiniz zaman değiştirebilirsiniz.

Çözümünüzü sağlamaya başlamak için **Oluştur** ' a tıklayın. Bu işlemin çalışması en az beş dakika sürer:

![Cihaz Benzetimi çözümü ayrıntıları](./media/quickstart-device-simulation-deploy/createform.png)

## <a name="sign-in-to-the-solution"></a>Çözümde oturum açma

Hazırlama işlemi tamamlandığında **Başlat**’a tıklayarak kendi Cihaz Benzetimi örneğinizde oturum açabilirsiniz:

![Cihaz Benzetimini açma](./media/quickstart-device-simulation-deploy/choosenew.png)

İzin isteğini kabul etmek için **Kabul et** öğesine tıklayın. Cihaz Benzetimi çözümü panosu tarayıcınızda görüntülenir.

İlk açıldığında **Başlarken** kılavuzunun bulunduğu Cihaz Benzetimi panosunu görürsünüz. Örnek benzetimi açmak için ilk kutucuğa tıklayın. **Başlarken** kılavuzunu kapatırsanız **Örnek Basit Benzetim**’i kutucuğuna tıklayarak panodan açabilirsiniz:

![Çözüm panosu](./media/quickstart-device-simulation-deploy/gettingstarted1.png)

## <a name="sample-simulation"></a>Örnek Benzetim

Örnek benzetim olduğu için düzenlenemez. Benzetim aşağıdaki ayarlarla yapılandırılır:

| Ayar             | Değer                       |
| ------------------- | --------------------------- |
| Hedef IoT Hub'ı      | Önceden sağlanan IoT Hub’ı kullan |
| Cihaz modeli        | Tır                       |
| Cihaz sayısı   | 10                          |
| Telemetri sıklığı | 10 saniye                  |
| Benzetim süresi | Süresiz olarak çalıştır            |

![Benzetim yapılandırması](./media/quickstart-device-simulation-deploy/samplesimulation1.png)

## <a name="run-the-simulation"></a>Benzetimi çalıştırma

**Benzetimi başlat**’a tıklayın. Benzetim, yapılandırıldığı gibi süresiz olarak çalıştırılır. Benzetimi istediğiniz zaman **Benzetimi durdur**’a tıklayarak durdurabilirsiniz. Benzetim, geçerli çalışmadaki istatistikleri gösterir.

![Benzetim çalıştırması](./media/quickstart-device-simulation-deploy/runningsimulation.png)

Cihaz Benzetimi örneğinden aynı anda yalnızca bir benzetim çalıştırabilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Daha fazla incelemeyi planlıyorsanız, Cihaz Benzetimini dağıtımda bırakın.

Cihaz Benzetimine artık ihtiyacınız kalmadıysa [Sağlanan çözümler](https://www.azureiotsolutions.com/Accelerators#dashboard) sayfasında kutucuğuna ve ardından **Çözümü Sil**’e tıklayarak bunu silin:

![Çözümü sil](media/quickstart-device-simulation-deploy/deletesolution.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta Cihaz Benzetimini dağıttınız ve örnek IoT cihazı benzetimini çalıştırdınız.

> [!div class="nextstepaction"]
> [Bir veya birden fazla cihaz türüyle benzetim oluşturma](iot-accelerators-device-simulation-create-simulation.md)