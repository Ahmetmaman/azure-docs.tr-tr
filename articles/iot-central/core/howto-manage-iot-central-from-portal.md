---
title: Azure portal IoT Central Yönet | Microsoft Docs
description: Bu makalede, Azure portal IoT Central uygulamalarınızın nasıl oluşturulacağı ve yönetileceği açıklanmaktadır.
services: iot-central
ms.service: iot-central
author: dominicbetts
ms.author: dobett
ms.date: 02/11/2020
ms.topic: how-to
manager: philmea
ms.openlocfilehash: 2af97206db00d683ab409710bc71a3b5048bf6ae
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "104658474"
---
# <a name="manage-iot-central-from-the-azure-portal"></a>Azure portal IoT Central yönetme

[!INCLUDE [iot-central-selector-manage](../../../includes/iot-central-selector-manage.md)]

[Azure IoT Central uygulama Yöneticisi](https://aka.ms/iotcentral) web sitesinde IoT Central uygulamaları oluşturup yönetmek yerine uygulamalarınızı yönetmek için [Azure Portal](https://portal.azure.com) kullanabilirsiniz.

## <a name="create-iot-central-applications"></a>IoT Central uygulamaları oluşturma

[!INCLUDE [Warning About Access Required](../../../includes/iot-central-warning-contribitorrequireaccess.md)]


Bir uygulama oluşturmak için [Azure Portal](https://ms.portal.azure.com) gidin ve **kaynak oluştur**' u seçin.

**Market çubuğunda ara** çubuğuna *IoT Central* yazın:

![Yönetim Portalı: arama](media/howto-manage-iot-central-from-portal/image0a1.png)

Arama sonuçlarında **IoT Central uygulama** kutucuğunu seçin:

![Yönetim Portalı: arama sonuçları](media/howto-manage-iot-central-from-portal/image0b1.png)

Şimdi **Oluştur**' u seçin.

![Yönetim Portalı: IoT Central kaynağı](media/howto-manage-iot-central-from-portal/image0c1.png)

Formdaki tüm alanları doldurur. Bu form, [Azure IoT Central uygulama Yöneticisi](https://aka.ms/iotcentral) Web sitesinde uygulama oluşturmak için doldurduğunuz forma benzer. Daha fazla bilgi için [IoT Central uygulaması oluşturma](quick-deploy-iot-central.md) hızlı başlangıcı bölümüne bakın.

![IoT Central form oluştur](media/howto-manage-iot-central-from-portal/image6a.png)

**Konum** , uygulamanızı oluşturmak istediğiniz [Coğrafya](https://azure.microsoft.com/global-infrastructure/geographies/) ' dır. Genellikle en iyi performansı elde etmek için cihazlarınıza fiziksel olarak en yakın konumu seçmeniz gerekir. Azure IoT Central Şu anda **Avustralya**, **Asya Pasifik**, **Avrupa**, **Birleşik Devletler**, **Birleşik Krallık** ve **Japonya** coğrafi graflarını kullanabilir. Bir konum seçtikten sonra, uygulamanızı daha sonra farklı bir konuma taşıyamazsınız.

Tüm alanları doldurduktan sonra **Oluştur**' u seçin.

## <a name="manage-existing-iot-central-applications"></a>Mevcut IoT Central uygulamalarını yönetme

Zaten bir Azure IoT Central uygulamanız varsa, bunu silebilir veya Azure portal farklı bir aboneliğe veya kaynak grubuna taşıyabilirsiniz.

> [!NOTE]
> *Ücretsiz* plan kullanılarak oluşturulan uygulamalar, Azure abonelikleri gerektirmez ve bu nedenle, bunları Azure Portal Azure aboneliğinizdeki listede bulamayamayacağız. IoT Central portalından yalnızca ücretsiz uygulamaları görebilir ve yönetebilirsiniz.

Başlamak için portalda **tüm kaynaklar** ' ı seçin. **Gizli türleri göster** ' i seçin ve bunu bulmak için **ada göre filtrele** ' de uygulamanızın adını yazmaya başlayın. Ardından, yönetmek istediğiniz IoT Central uygulamayı seçin.

Uygulamaya gitmek için **IoT Central uygulama URL 'sini** seçin:

!["IoT Central uygulama URL 'SI" vurgulanmış "genel bakış" sayfasını gösteren ekran görüntüsü.](media/howto-manage-iot-central-from-portal/image3.png)

Uygulamayı farklı bir kaynak grubuna taşımak için kaynak grubunun yanındaki **Değiştir** ' i seçin. **Kaynakları taşı** sayfasında, bu uygulamayı taşımak istediğiniz kaynak grubunu seçin:

!["Genel bakış" sayfasını "kaynak grubu (değişiklik)" vurgulanmış şekilde gösteren ekran görüntüsü.](media/howto-manage-iot-central-from-portal/image4a.png)

Uygulamayı farklı bir aboneliğe taşımak için, aboneliğin yanındaki  **Değiştir** ' i seçin. **Kaynakları taşı** sayfasında, bu uygulamayı taşımak istediğiniz aboneliği seçin:

![Yönetim Portalı: kaynak yönetimi](media/howto-manage-iot-central-from-portal/image5a.png)

## <a name="next-steps"></a>Sonraki adımlar

Azure IoT Central uygulamalarını Azure portal nasıl yönetebileceğinizi öğrendiğinize göre, önerilen sonraki adım aşağıda verilmiştir:

> [!div class="nextstepaction"]
> [Uygulamanızı yönetme](howto-administer.md)