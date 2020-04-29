---
title: Özel simülasyon cihazı oluşturma - Azure | Microsoft Docs
description: Bu öğreticide, simülasyonlarda kullanmak üzere özel bir simülasyon cihazı oluşturmak için Cihaz Simülasyonu'nu kullanacaksınız.
author: troyhopwood
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: tutorial
ms.custom: mvc
ms.date: 10/25/2018
ms.author: troyhop
ms.openlocfilehash: 302b863e7ad7d6df286adf53342356f279ab92d2
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "61450582"
---
# <a name="tutorial-create-a-custom-simulated-device"></a>Öğretici: Özel simülasyon cihazı oluşturma

Bu öğreticide, simülasyonlarda kullanmak üzere özel bir simülasyon cihazı oluşturmak için Cihaz Simülasyonu'nu kullanacaksınız. Cihaz Simülasyonu ile çalışmaya başlamak için, eklenen örnek simülasyon cihazlarından birini kullanabilirsiniz. Ayrıca, bu makalede açıklandığı gibi özel simülasyon cihazları da oluşturabilirsiniz. Diğer özelleştirme seçenekleri için bkz. [Gelişmiş bir cihaz modeli oluşturma](iot-accelerators-device-simulation-advanced-device.md).

Bu öğreticide şunları yaptınız:

>[!div class="checklist"]
> * Simülasyon cihazı modellerinizin listesini görüntüleme
> * Özel simülasyon cihazı oluşturma
> * Cihaz modelini kopyalama
> * Cihaz modelini silme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi izlemek için Azure aboneliğinizde Cihaz Simülasyonu'nun dağıtılmış örneğine sahip olmanız gerekir.

Cihaz Simülasyonu'nu henüz dağıtmadıysanız, [Azure'da IoT cihaz simülasyonunu dağıtma ve çalıştırma](quickstart-device-simulation-deploy.md) hızlı başlangıç kılavuzunu tamamlamalısınız.

## <a name="open-device-simulation"></a>Cihaz Benzetimini açma

Tarayıcınızda Cihaz Simülasyonu'nu çalıştırmak için, önce [Microsoft Azure IoT Çözüm Hızlandırıcıları](https://www.azureiotsolutions.com)'na gidin.

Azure aboneliği kimlik bilgilerinizi kullanarak oturum açmanız istenebilir.

Ardından [Azure'da IoT cihaz simülasyonunu dağıtma ve çalıştırma](quickstart-device-simulation-deploy.md) hızlı başlangıcında dağıttığınız Cihaz Simülasyonu'nun kutucuğunda **Başlat**'a tıklayın.

## <a name="view-your-device-models"></a>Cihaz modellerinizi görüntüleme

Menü çubuğunda **Cihaz modelleri**'ni seçin. **Cihaz modelleri** sayfasında, bu Cihaz Simülasyonu örneğindeki tüm kullanılabilir cihaz modelleri listelenir:

![Cihaz modelleri](media/iot-accelerators-device-simulation-create-custom-device/devicemodelnav.png)

## <a name="create-a-device-model"></a>Cihaz modeli oluşturma

Sayfanın sağ üst köşesindeki **+ Cihaz Modeli Ekle**'ye tıklayın:

![Cihaz modeli ekleme](media/iot-accelerators-device-simulation-create-custom-device/devicemodels.png)

Bu öğreticide, hem sıcaklık hem de nem verilerini gönderen bir buzdolabı simülasyonu oluşturursunuz.

Formu aşağıdaki bilgilerle doldurun:

| Ayar             | Değer                                                |
| ------------------- | ---------------------------------------------------- |
| Cihaz modeli adı   | Buzdolabı                                         |
| Model açıklaması   | Sıcaklık ve nem algılayıcıları olan bir buzdolabı |
| Sürüm             | 1.0                                                  |

> [!NOTE]
> Cihaz modeli adı benzersiz olmalıdır.

Aşağıdaki değerlerle sıcaklık ve nem veri noktaları eklemek için **+ Veri noktası ekle**'ye tıklayın:

| Veri Noktası          | Davranış        | Minimum Değer | Maksimum Değer | Birim |
| ------------------- | --------------- | --------- | --------- | ---- |
| Sıcaklık         | Rasgele          | -50       | 100       | F    |
| Nem oranı            | Rasgele          | 0         | 100       | %    |

Cihaz modelini kaydetmek için **Kaydet**’e tıklayın.

![Cihaz modelini oluşturma](media/iot-accelerators-device-simulation-create-custom-device/adddevicemodel.png)

Artık buzdolabınız cihaz modelleri listesine eklenir. Başka bir sayfaya gidip buzdolabınızı görmek için **Sonraki**'ne tıklamanız gerekebilir.

## <a name="clone-a-device-model"></a>Cihaz modelini kopyalama

Cihaz modelini kopyalamanız, mevcut cihaz modelinin bir kopyasını oluşturmanızı sağlar. Ardından belirli gereksinimlerinizi karşılamak amacıyla kopyayı düzenleyebilirsiniz. Benzer cihaz modelleri oluşturmanız gerektiğinde, kopyalama zaman kazandırabilir.

Cihaz modelini kopyalamak için, modelin yanındaki kutuyu işaretleyin ve ardından eylem çubuğunda **Kopyala**'ya tıklayın:

![Cihaz modelini silme](media/iot-accelerators-device-simulation-create-custom-device/clonedevice.png)

## <a name="delete-a-device-model"></a>Cihaz modelini silme

İstediğiniz özel cihaz modelini silebilirsiniz. Cihaz modelini silmek için, modelin yanındaki kutuyu işaretleyin ve ardından eylem çubuğunda **Sil**'e tıklayın:

![Cihaz modelini silme](media/iot-accelerators-device-simulation-create-custom-device/deletedevice.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, özel cihaz modellerini oluşturmayı, kopyalamayı ve silmeyi öğrendiniz. Cihaz modelleri hakkında daha fazla bilgi edinmek için aşağıdaki nasıl yapılır makalesine bakın:

> [!div class="nextstepaction"]
> [Gelişmiş bir cihaz modeli oluşturma](iot-accelerators-device-simulation-advanced-device.md)