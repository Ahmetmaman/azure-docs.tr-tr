---
title: Azure Izleyici ile Azure kaynağını izleme
description: Azure Izleyici 'de Azure kaynağı için verileri nasıl toplayacağınızı ve analiz edeceğinizi öğrenin.
ms. subservice: logs
ms.topic: quickstart
author: bwren
ms.author: bwren
ms.date: 12/15/2019
ms.openlocfilehash: 00cec2ca11b77f6cb9d6c0f3c429931bdcc6f6e4
ms.sourcegitcommit: d22a86a1329be8fd1913ce4d1bfbd2a125b2bcae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/26/2020
ms.locfileid: "96186142"
---
# <a name="quickstart-monitor-an-azure-resource-with-azure-monitor"></a>Hızlı başlangıç: Azure Izleyici ile Azure kaynağını Izleme
[Azure izleyici](../overview.md) , verileri oluşturuldukları anda Azure kaynaklarından toplamayı başlatır. Bu hızlı başlangıçta, bir kaynak için otomatik olarak toplanan veriler ve belirli bir kaynak için Azure portal nasıl görüntüleneceği hakkında kısa bir yol sunulmaktadır. Daha sonra, ek verileri toplamak için yapılandırma ekleyebilirsiniz ve aboneliğinizdeki tüm kaynaklar için toplanan verilere erişmek üzere aynı araçları kullanmak için Azure Izleyici menüsüne gidebilirsiniz.

Azure kaynaklarından toplanan izleme verilerinin daha ayrıntılı açıklamaları için bkz. Azure [izleyici Ile Azure kaynaklarını izleme](../insights/monitor-azure-resource.md).


## <a name="sign-in-to-azure-portal"></a>Azure portalda oturum açın

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın. 


## <a name="overview-page"></a>Genel Bakış sayfası
Birçok hizmet, **genel bakış** sayfasına, işlem için hızlı bir bakış olarak izleme verileri içerir. Bu, genellikle Azure Izleyici ölçümlerinde depolanan platform ölçümlerinin bir alt kümesini temel alır.

1. Aboneliğinizde bir Azure kaynağı bulun.
2. **Genel bakış** sayfasına gidin ve performans verilerinin görüntülendiğini aklınızda olun. Bu veriler, Azure Izleyici tarafından sağlanacaktır. Aşağıdaki örnek, bir Azure depolama hesabının **genel bakış** sayfasıdır ve birden çok ölçüm görüntülendiğini görebilirsiniz.

    ![Genel Bakış sayfası](media/quick-monitor-azure-resource/overview.png)

3. Aşağıda açıklanan Ölçüm Gezgini 'nde verileri açmak için grafiklerin herhangi birine tıklayabilirsiniz.

## <a name="view-the-activity-log"></a>Etkinlik günlüğünü görüntüleme
Etkinlik günlüğü, abonelikteki her bir Azure kaynağı üzerinde işlemler hakkında öngörüler sağlar. Bu, bir kaynağın oluşturulma veya değiştirilme tarihi, bir iş başlatıldığında veya belirli bir işlem gerçekleştiğinde olduğu gibi bilgileri içerir.

1. Kaynağınızın menüsünün üst kısmında **etkinlik günlüğü**' nü seçin.
2. Geçerli filtre, kaynağınız ile ilgili olaylara ayarlanır. Herhangi bir olay görmüyorsanız zaman kapsamını artırmak için **TimeSpan** 'i değiştirmeyi deneyin.

    ![Kaynak etkinlik günlüğü ' nde, varsayılan filtre ölçütü kullanılarak oluşturulan kaynak için işlem listesini gösteren Azure portal ekran görüntüsü.](media/quick-monitor-azure-resource/activity-log-resource.png)

4. Aboneliğinizdeki diğer kaynaklardan gelen olayları görmek isterseniz, filtredeki ölçütleri değiştirin ya da filtre özelliklerini kaldırın.

    ![Değiştirilen filtre ölçütü kullanılarak oluşturulan kaynak için işlem listesini gösteren Azure portal bir kaynak etkinlik günlüğünün ekran görüntüsü.](media/quick-monitor-azure-resource/activity-log-all.png)



## <a name="view-metrics"></a>Ölçümleri görüntüle
Ölçümler, belirli bir zamanda kaynağınızın bazı yönlerini tanımlayan sayısal değerlerdir. Azure Izleyici, tüm Azure kaynaklarından bir dakikalık aralıklarla platform ölçümlerini otomatik olarak toplar. Bu ölçümleri, Ölçüm Gezgini 'ni kullanarak görüntüleyebilirsiniz.

1. Kaynağınızın menüsünün **izleme** bölümü altında **ölçümler**' i seçin. Bu, Ölçüm Gezgini 'ni kaynak olarak ayarlanan kapsamla açar.
2. Grafiğe ölçüm eklemek için **ölçüm Ekle** ' ye tıklayın.
   
   ![Bir kaynağın ölçümlerini gösteren Azure portal Ölçüm Gezgini ekran görüntüsü. Ölçüm açılan menüsü yeni ölçümler eklemek için açıktır.](media/quick-monitor-azure-resource/metrics-explorer-01.png)
   
4. Açılan listeden bir **ölçüm** ve ardından bir **toplama** seçin. Bu, toplanan değerlerin her zaman aralığında nasıl örnekleneceğini tanımlar.

    ![Bir kaynağın ölçümlerini gösteren Azure portal Ölçüm Gezgini ekran görüntüsü. Toplama açılan kutusu açıktır ve AVG seçilidir.](media/quick-monitor-azure-resource/metrics-explorer-02.png)

5. Grafiğe ek ölçüm ve toplama birleşimleri eklemek için **ölçüm Ekle** ' ye tıklayın.

    ![Bir kaynağın ölçümlerini gösteren Azure portal Ölçüm Gezgini ekran görüntüsü.](media/quick-monitor-azure-resource/metrics-explorer-03.png)



## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçta, Azure Izleyici tarafından otomatik olarak toplanan bir Azure kaynağı için etkinlik günlüğü ve ölçümleri görüntülenir. Etkinlik günlüğünü, [günlük sorguları](../log-query/log-query-overview.md)kullanılarak çözümlenebilecekleri bir Log Analytics çalışma alanında nasıl toplayacağınızı gösteren bir sonraki hızlı başlangıca geçin.

> [!div class="nextstepaction"]
> [Log Analytics çalışma alanına Azure etkinlik günlüğü gönder](./quick-collect-activity-log-portal.md)