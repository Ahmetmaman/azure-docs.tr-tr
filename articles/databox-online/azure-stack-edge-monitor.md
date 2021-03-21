---
title: Azure Stack Edge Pro cihazınızı izleme | Microsoft Docs
description: Azure Stack Edge Pro 'Yu izlemek için Azure portal ve yerel Web Kullanıcı arabiriminin nasıl kullanılacağını açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 03/04/2021
ms.author: alkohli
ms.openlocfilehash: aae64cad3603725a4062d5afb42df974bbf8ac40
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102438804"
---
# <a name="monitor-your-azure-stack-edge-pro"></a>Azure Stack Edge Pro 'Yu izleyin

[!INCLUDE [applies-to-GPU-and-pro-r-mini-r-and-fpga-skus](../../includes/azure-stack-edge-applies-to-gpu-pro-r-mini-r-fpga-sku.md)]

Bu makalede Azure Stack Edge Pro 'Nun nasıl izleneceği açıklanır. Cihazınızı izlemek için Azure portal veya yerel Web Kullanıcı arabirimi ' ni kullanabilirsiniz. Cihaz olaylarını görüntülemek, uyarıları yapılandırmak ve yönetmek ve ölçümleri görüntülemek için Azure portal kullanın. Çeşitli cihaz bileşenlerinin donanım durumunu görüntülemek için fiziksel cihazınızda yerel Web Kullanıcı arabirimini kullanın.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
>
> * Cihaz olaylarını ve ilgili uyarıları görüntüleme
> * Cihaz bileşenlerinin donanım durumunu görüntüleme
> * Cihazınız için kapasite ve işlem ölçümlerini görüntüleme

## <a name="view-device-events"></a>Cihaz olaylarını görüntüle

[!INCLUDE [Supported OS for clients connected to device](../../includes/data-box-edge-gateway-view-device-events.md)]

## <a name="view-hardware-status"></a>Donanım durumunu görüntüleme

Cihaz bileşenlerinizin donanım durumunu görüntülemek için yerel web kullanıcı arabiriminde aşağıdaki adımları izleyin.

1. Cihazınızın yerel web kullanıcı arabirimine bağlanın.
2. **Bakım > donanım durumu**' na gidin. Çeşitli cihaz bileşenlerinin sistem durumunu görüntüleyebilirsiniz.

    ![Donanım durumunu görüntüleme](media/azure-stack-edge-monitor/view-hardware-status.png)

## <a name="view-metrics"></a>Ölçümleri görüntüle

[!INCLUDE [Supported OS for clients connected to device](../../includes/data-box-edge-gateway-view-metrics.md)]

### <a name="metrics-on-your-device"></a>Cihazınızdaki ölçümler

Bu bölümde, cihazınızdaki izleme ölçümleri açıklanmaktadır. Ölçümler şu şekilde olabilir:

* Kapasite ölçümleri. Kapasite ölçümleri, cihazın kapasitesiyle ilgilidir.

* İşlem ölçümleri. İşlem ölçümleri, Azure depolama 'ya yönelik okuma ve yazma işlemleriyle ilgilidir.

* Edge işlem ölçümleri. Edge işlem ölçümleri, cihazınızdaki Edge işlem kullanımı ile ilgilidir.

Ölçümlerin tam listesi aşağıdaki tabloda gösterilmiştir:

|Kapasite ölçümleri                     |Description  |
|-------------------------------------|-------------|
|**Kullanılabilir kapasite**               | Cihaza yazılabilen verilerin boyutunu ifade eder. Diğer bir deyişle, bu ölçüm cihazda kullanılabilir hale getirilebilir kapasitedir. <br></br>Hem cihaz hem de bulutta bir kopyası olan dosyaların yerel kopyasını silerek cihaz kapasitesini serbest bırakabilirsiniz.        |
|**Toplam kapasite**                   | , Yerel önbelleğin toplam boyutu olarak da adlandırılan, verileri yazmak için aygıttaki toplam bayt sayısını ifade eder. <br></br> Artık bir veri diski ekleyerek var olan bir sanal cihazın kapasitesini artırabilirsiniz. VM için hiper yönetici yönetimi aracılığıyla bir veri diski ekleyin ve ardından sanal makineyi yeniden başlatın. Ağ Geçidi cihazının yerel depolama havuzu, yeni eklenen veri diskine uyum sağlayacak şekilde genişletilir. <br></br>Daha fazla bilgi için, [Hyper-V sanal makinesi için sabit sürücü ekleme](https://www.youtube.com/watch?v=EWdqUw9tTe4)bölümüne gidin. |

|İşlem ölçümleri              | Description         |
|-------------------------------------|---------|
|**Karşıya yüklenen bulut baytları (cihaz)**    | Cihazınızdaki tüm paylaşımlar genelinde karşıya yüklenen tüm baytların toplamı        |
|**Karşıya yüklenen bulut baytları (paylaşma)**     | Her bir paylaşılan bayt karşıya yüklendi. Bu ölçüm şu olabilir: <br></br> Ort, (paylaşım başına karşıya yüklenen baytların toplamı/paylaşım sayısı),  <br></br>En fazla, bir paylaşımdan karşıya yüklenen en fazla bayt sayısıdır <br></br>En az, bir paylaşımdan karşıya yüklenen bayt sayısı alt sınırı      |
|**Bulut indirme verimlilik (paylaşma)**| Her bir paylaşıma indirilen bayt. Bu ölçüm şu olabilir: <br></br> Ortalama, (okuma veya bir paylaşıma/paylaşım sayısına indirilen tüm baytların toplamı) <br></br> En fazla, bir paylaşımdan indirilen en fazla bayt sayısıdır<br></br> ve bir paylaşımdan indirilen en az bayt sayısı olan dk  |
|**Bulut okuma performansı**            | Cihazınızdaki tüm paylaşımlar genelinde buluttan okunan tüm baytların toplamı     |
|**Bulut karşıya yükleme performansı**          | Cihazınızdaki tüm paylaşımlar genelinde buluta yazılan tüm baytların toplamı     |
|**Bulut karşıya yükleme üretilen işi (paylaşma)**  | Bir paylaşımdan buluta yazılan tüm baytların toplamı, paylaşım başına ortalama, en fazla ve en az      |
|**Okuma performansı (ağ)**           | Buluttan okunan tüm baytlar için sistem ağı aktarım hızını içerir. Bu görünüm, paylaşımlarla sınırlı olmayan verileri içerebilir. <br></br>Bölmek, bağlı olmayan veya etkin bağdaştırıcılar dahil olmak üzere cihazdaki tüm ağ bağdaştırıcılarının trafiğini gösterir.      |
|**Yazma işleme (ağ)**       | Buluta yazılan tüm baytlar için sistem ağı aktarım hızını içerir. Bu görünüm, paylaşımlarla sınırlı olmayan verileri içerebilir. <br></br>Bölmek, bağlı olmayan veya etkin bağdaştırıcılar dahil olmak üzere cihazdaki tüm ağ bağdaştırıcılarının trafiğini gösterir.          |

| Edge işlem ölçümleri              | Description         |
|-------------------------------------|---------|
|**Edge işlem-bellek kullanımı**      |           |
|**Edge hesaplama-CPU yüzdesi**    |         |

## <a name="next-steps"></a>Sonraki adımlar

[Bant genişliğini yönetmeyi](azure-stack-edge-manage-bandwidth-schedules.md) öğrenin.
[Cihaz olay uyarı bildirimlerini yönetmeyi](azure-stack-edge-gpu-manage-device-event-alert-notifications.md)öğrenin.
