---
title: Azure IoT Central'da cihazlarınızı izleme | Microsoft Docs
description: Bir operatör olarak, Azure IoT Central uygulamanızı cihazlarınızı izlemek için kullanın.
author: dominicbetts
ms.author: dobett
ms.date: 10/12/2018
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: philmea
ms.openlocfilehash: 941fbffaa7a0129de30d6fdf14d3d0559429309f
ms.sourcegitcommit: d4f728095cf52b109b3117be9059809c12b69e32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2019
ms.locfileid: "54200250"
---
# <a name="tutorial-use-azure-iot-central-to-monitor-your-devices"></a>Öğretici: Azure IoT Central kullanarak cihazlarınızı izleme

Bu öğreticide, bir operatör olarak cihazlarınızı izlemek ve ayarlarınızı değiştirmek için Microsoft Azure IoT Central uygulamasını nasıl kullanacağınız gösterilmektedir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bildirim alma
> * Sorun araştırma
> * Sorunu düzeltme

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce, oluşturucunun Azure IoT Central uygulamasını oluşturmaya yönelik üç oluşturucu öğreticisini tamamlaması gerekir:

* [Yeni bir cihaz türü belirleme](tutorial-define-device-type.md)
* [Cihazınız için kurallar ve eylemler yapılandırma](tutorial-configure-rules.md)
* [Operatörün görünümlerini özelleştirme](tutorial-customize-operator.md)

## <a name="receive-a-notification"></a>Bildirim alma

Azure IoT Central, e-posta iletileri halinde cihazlara ilişkin bildirimler gönderir. Oluşturucu, bağlı bir klima cihazındaki sıcaklık belirli bir eşiği aştığında bildirim gönderecek şekilde bir kural eklemiştir. Oluşturucunun bildirim almayı seçtiği hesaba gönderilen e-postaları kontrol edin.

[Cihazınız için kurallar ve eylemler yapılandırma](tutorial-configure-rules.md) öğreticisinin sonunda aldığınız e-posta iletisini açın. E-postada **Cihazınızı açmak için buraya tıklayın**’ı seçin:

![Uygulama Oluşturucu kuralları](media/tutorial-monitor-devices/email.png)

Önceki öğreticilerde oluşturduğunuz **Bağlı Klima-1** sanal cihazının **Cihaz** sayfası tarayıcıda açılır:

![Bildirim e-posta iletisini tetikleyen cihaz](media/tutorial-monitor-devices/sourcedevice.png)

## <a name="investigate-an-issue"></a>Sorun araştırma

Operatör olarak, bir cihaza ilişkin bilgileri **Ölçümler**, **Ayarlar**, **Özellikler**, **Kurallar** ve **Pano** sayfalarında görüntüleyebilirsiniz. Oluşturucu, **Pano**’yu bağlı bir klima cihazına ilişkin önemli bilgileri gösterecek şekilde özelleştirmiştir.

Cihaza ilişkin bilgileri görmek için **Pano** görünümünü seçin.

![Cihaz panosu](media/tutorial-monitor-devices/initial_screen.png)

Panodaki grafik, cihaz sıcaklığının bir çizimini gösterir. Cihazın geçerli hedef sıcaklığını da **Ayarlanmış hedef sıcaklık** kutucuğunda görebilirsiniz. Hedef sıcaklığın çok yüksek olmasına karar verdiniz.

## <a name="remediate-an-issue"></a>Sorunu düzeltme

Cihazın hedef sıcaklığını değiştirmek için **Ayarlar** sayfasını kullanın:

1. **Ayarlar**'ı seçin. **Sıcaklığı Ayarla**’yı 75 olarak değiştirin. Yeni hedef sıcaklığı cihaza göndermek için **Güncelleştir**’i seçin. Cihaz ayarların değiştiğini kabul ettiğinde, ayar değerinin durumu **eşitlendi** olarak değişir:

    ![Ayarları güncelleştirme](media/tutorial-monitor-devices/change_settings.png)

2. **Pano**’yu seçin ve yeni ayar değerini doğrulayın:

    ![Güncelleştirilmiş cihaz panosu](media/tutorial-monitor-devices/new_settings.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="nextstepaction"]
> * Bildirim alma
> * Sorun araştırma
> * Sorunu düzeltme

Artık cihazınızı nasıl izleyeceğinizi bildiğinize göre [Cihaz ekleme](tutorial-add-device.md) adımını uygulamanız önerilir.