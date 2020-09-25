---
title: Azure Maps API kullanım ölçümlerini görüntüleme | Microsoft Azure haritaları
description: Toplam istek sayısı, toplam hata ve kullanılabilirlik gibi Azure Maps API kullanım ölçümlerini görüntülemeyi öğrenin. Bkz. verileri filtreleme ve sonuçları bölme.
author: anastasia-ms
ms.author: v-stharr
ms.date: 08/06/2018
ms.topic: how-to
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 3f5dfcafe1c7b08a2c8320f807bf809107197d40
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91335288"
---
# <a name="view-azure-maps-api-usage-metrics"></a>Azure Haritalar API'si kullanım ölçümlerini görüntüleme

Bu makalede, Azure haritalar hesabınız için [Azure Portal](https://portal.azure.com)API kullanım ölçümlerini nasıl görüntüleyebileceğiniz gösterilmektedir. Ölçümler, özelleştirilebilir bir süre boyunca uygun bir grafik biçiminde gösterilir.

## <a name="view-metric-snapshot"></a>Ölçüm anlık görüntüsünü görüntüle

Haritalar hesabınızın **genel bakış** sayfasında bazı ortak ölçümleri görebilirsiniz. Bu, şu anda *toplam istek*, *Toplam hata*ve seçilebilir bir süre boyunca *kullanılabilirliği* gösteriyor.

![Azure Maps kullanım ölçümlerine genel bakış](media/how-to-view-api-usage/portal-overview.png)

Belirli analize yönelik bu grafikleri özelleştirmeniz gerekiyorsa sonraki bölüme geçin.

## <a name="view-detailed-metrics"></a>Ayrıntılı ölçümleri görüntüleme

1. [Portalda](https://portal.azure.com)Azure aboneliğinizde oturum açın.

2. Sol taraftaki **tüm kaynaklar** menü öğesine tıklayın ve *Azure haritalar hesabınıza*gidin.

3. Haritalar hesabınız açıkken, sol taraftaki **ölçümler** menüsüne tıklayın.

4. **Ölçümler** bölmesinde, aşağıdaki seçeneklerden birini seçin:

   1. **Kullanılabilirlik** -bir süre içinde API kullanılabilirliği *ortalamasını* gösterir.
   2. **Kullanım** -hesabınız Için kullanım *sayısını* gösterir.

      ![Azure Haritalar kullanım ölçümleri bölmesi](media/how-to-view-api-usage/portal-metrics.png)

5. Ardından, **son 24 saat (otomatik)** öğesine tıklayarak *zaman aralığını* seçebilirsiniz. Varsayılan olarak, zaman aralığı 24 saat olarak ayarlanır. ' İ tıkladıktan sonra, tüm seçilebilir zaman aralıklarını görürsünüz. *Zaman parçalı yapısını* seçebilir ve aynı açılan kutuda saati *Yerel* veya *GMT* olarak göstermeyi seçebilirsiniz. **Uygula**’ya tıklayın.

    ![Azure haritalar ölçümleri zaman aralığı](media/how-to-view-api-usage/time-range.png)

6. Ölçümünüzün eklendikten sonra, Bu ölçüyle ilgili özelliklerden **filtre ekleyebilirsiniz** . Ardından, grafiğe yansıtılmasını istediğiniz özelliğin değerini seçin.

    ![Azure Haritalar kullanım ölçümleri filtresi](media/how-to-view-api-usage/filter.png)

7. Ayrıca, seçtiğiniz ölçüm özelliğine göre ölçümünüzün **bölünmesini de uygulayabilirsiniz** . Grafiğin, bu özelliğin her değeri için birden çok grafiğe bölünmesine izin verir. Aşağıdaki resimde, her grafiğin rengi grafiğin altında gösterilen özellik değerine karşılık gelir.

    ![Azure Haritalar kullanım ölçümleri bölme](media/how-to-view-api-usage/splitting.png)

8. Ayrıca, en üstteki **ölçüm Ekle** düğmesine tıklayarak da aynı grafikteki birden fazla ölçümü gözlemleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Kullanım için kullanımı izlemek istediğiniz Azure Maps API 'Leri hakkında daha fazla bilgi edinin:
> [!div class="nextstepaction"] 
> [Azure Haritalar Web SDK 'Sı nasıl yapılır](how-to-use-map-control.md)

> [!div class="nextstepaction"] 
> [Azure Haritalar Android SDK nasıl yapılır?](how-to-use-android-map-control-library.md)

> [!div class="nextstepaction"]
> [Azure haritalar REST API belgeleri](https://docs.microsoft.com/rest/api/maps)
