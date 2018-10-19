---
title: Azure Analysis Services Power BI ile bağlanma | Microsoft Docs
description: Power BI'ı kullanarak bir Azure Analysis Services sunucusuna bağlanmayı öğreneceksiniz.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 10/18/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 707bc41a2a66782d9540d95606c41685908e9848
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2018
ms.locfileid: "49429735"
---
# <a name="connect-with-power-bi"></a>Power BI ile bağlanma

Bir sunucu Azure'da oluşturduğunuz ve tablosal bir model dağıttıktan sonra kuruluşunuzdaki kullanıcıların bağlanıp verileri araştırmaya başladıktan hazır olursunuz. 

> [!TIP]
> En son sürümünü kullandığınızdan emin olun [Power BI Desktop](https://powerbi.microsoft.com/desktop/).
> 
> 
  
## <a name="connect-in-power-bi-desktop"></a>Power BI Desktop'ta bağlanma

1. Power BI Desktop'ta, **Veri Al** > **Azure** > **Azure Analysis Services veritabanı**'na tıklayın.

2. İçinde **sunucu**, sunucu adını girin. Tam URL eklediğinizden emin olun; Örneğin, asazure://westcentralus.asazure.windows.net/advworks.

3. İçinde **veritabanı**, tablosal model veritabanı veya perspektif kopyalayıp buraya yapıştırın, bağlanmak istediğiniz adını biliyorsanız. Aksi takdirde, bu alanı boş bırakın ve daha sonra bir veritabanı veya perspektif seçin.

4. Bir bağlantı seçeneği seçin ve sonra basın **Connect**. 

    Her ikisi de **Canlı Bağlan** ve **alma** seçenek desteklenmez. Ancak, bazı sınırlamalar içeri aktarma moduna sahip olmadığından Canlı bağlantılar kullanmanızı öneririz; en önemlisi, içeri aktarma sırasında sunucu performansı etkilenebilir. Ayrıca, Power BI hizmetinde yenilenmesi ise **Power BI'dan erişime** ayar, yalnızca seçerken geçerlidir **Canlı Bağlan**.

5. İstenirse, oturum açma kimlik bilgilerinizi girin. 

6. İçinde **Gezgin**, sunucuyu genişletin ve ardından modeli veya perspektif bağlanın ve ardından istediğiniz seçin **Connect**. Bir model veya perspektif bu görünümün tüm nesneleri göstermek için tıklayın.

    Model Power BI Desktop'ta rapor görünümü'nde boş bir raporla açılır. Alanlar listesindeki tüm gizli olmayan bir model nesneleri görüntüler. Bağlantı durumu sağ alt köşede gösterilir.

## <a name="connect-in-power-bi-service"></a>Power BI (hizmet) bağlanma

1. Sunucunuzda, modele bir canlı bağlantı içeren bir Power BI Desktop dosyası oluşturun.
2. İçinde [Power BI](https://powerbi.microsoft.com), tıklayın **Veri Al** > **dosyaları**, bulun ve .pbix dosyanızı seçin.



## <a name="see-also"></a>Ayrıca bkz.
[Azure Analysis Services'a bağlanın](analysis-services-connect.md)   
[İstemci kitaplıkları](analysis-services-data-providers.md)

