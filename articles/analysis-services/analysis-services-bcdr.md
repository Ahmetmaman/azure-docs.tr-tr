---
title: Yüksek kullanılabilirlik Azure Analysis Services | Microsoft Docs
description: Bu makalede, Azure Analysis Services hizmet kesintisi sırasında yüksek kullanılabilirlik sağlar.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 03/30/2020
ms.author: owend
ms.reviewer: minewiskan
ms.custom: references_regions
ms.openlocfilehash: abfcc38601887cd14509ac436e0344b681e3577e
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "86506764"
---
# <a name="analysis-services-high-availability"></a>Analysis Services ile yüksek kullanılabilirlik

Bu makalede Azure Analysis Services sunucularının en yüksek kullanılabilirliği açıklanır. 

## <a name="assuring-high-availability-during-a-service-disruption"></a>Hizmet kesintisi sırasında yüksek kullanılabilirlik

Nadir bir Azure veri merkezinde kesinti olabilir. Bir kesinti oluştuğunda, en son birkaç dakika veya saat için en son olabilecek iş kesintilerine neden olabilir. Yüksek kullanılabilirlik çoğu zaman sunucu yedekliliği ile elde edilir. Azure Analysis Services, bir veya daha fazla bölgede ek, ikincil sunucular oluşturarak yedeklilik elde edebilirsiniz. Gereksiz sunucular oluştururken, bu sunuculardaki verilerin ve meta verilerin güvence altına almak için, çevrimdışı duruma geçmiş bir bölgedeki sunucu ile eşitlenmiş durumda olabilir:

* Diğer bölgelerdeki gereksiz sunuculara modeller dağıtın. Bu yöntem, hem birincil sunucunuzda hem de yedekli sunucularınızdaki verilerin hem paralel hem de aynı şekilde işlenmesini gerektirir, bu da tüm sunucular eşitlenir.

* Birincil sunucunuzdaki veritabanlarını [yedekleyin](analysis-services-backup.md) ve gereksiz sunuculara geri yükleyin. Örneğin, gecelik yedeklemeleri Azure depolama 'ya otomatikleştirebilir ve diğer bölgelerdeki diğer yedekli sunuculara geri yükleyebilirsiniz. 

Her iki durumda da, birincil sunucunuz bir kesinti yaşıyorsa, raporlama istemcilerindeki bağlantı dizelerini farklı bir bölgesel veri merkezinde sunucuya bağlanacak şekilde değiştirmeniz gerekir. Bu değişiklik, son çare ve yalnızca çok zararlı bir bölgesel veri merkezi kesintisi oluşursa göz önünde bulundurulmalıdır. Tüm istemcilerde bağlantıları güncelleştirebilmeniz için birincil sunucunuzu barındıran bir veri merkezi kesintisi daha olasıdır. 

Raporlama istemcilerinde bağlantı dizelerini değiştirmek zorunda kalmamak için, birincil sunucunuz için bir sunucu [diğer adı](analysis-services-server-alias.md) oluşturabilirsiniz. Birincil sunucu kapalıysa diğer adı başka bir bölgedeki gereksiz sunucuya işaret etmek üzere değiştirebilirsiniz. Birincil sunucuda bir uç nokta sistem durumu denetimi kodlayarak sunucu adı ' nı otomatik hale getirebilirsiniz. Sistem durumu denetimi başarısız olursa, aynı uç nokta başka bir bölgedeki gereksiz sunucuya doğrudan yönlendirebilir. 

## <a name="related-information"></a>İlgili bilgiler

[Yedekleme ve geri yükleme](analysis-services-backup.md)   
[Azure Analysis Services Yönet](analysis-services-manage.md)   
[Diğer ad sunucu adları](analysis-services-server-alias.md) 

