---
title: Bir Azure Log Analytics çalışma alanını silme | Microsoft Docs
description: Çalışma alanı modelinizi yeniden yapılandırmayı veya kişisel abonelik oluşturduysanız, Log Analytics çalışma alanınızı silme öğrenin.
services: log-analytics
documentationcenter: log-analytics
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 05/07/2018
ms.author: magoedte
ms.component: ''
ms.openlocfilehash: 8502912efe9f07464c36df190221afcaa00e79d4
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2018
ms.locfileid: "52726432"
---
# <a name="delete-an-azure-log-analytics-workspace-with-the-azure-portal"></a>Azure portalı ile bir Azure Log Analytics çalışma alanını silme
Bu makalede artık gerektirebilecek bir Log Analytics çalışma alanını silmek için Azure portalını kullanmayı gösterir. 

## <a name="to-delete-a-workspace"></a>Çalışma alanı silmek için 
Bir Log Analytics çalışma alanını sildiğinizde, çalışma alanınıza bağlı tüm veriler silinir hizmetten 30 gün içinde.  Önemli veri ve hizmet işlemlerinizi olumsuz şekilde etkileyebilecek yapılandırma olabilir çünkü bir çalışma alanını sildiğinizde dikkatli istiyorsunuz. Diğer Azure Hizmetleri ve Log Analytics'te verilerini depolayan, gibi kaynakları göz önünde bulundurun:

* Application Insights
* Azure Güvenlik Merkezi
* Azure Otomasyonu
* Windows ve Linux sanal makineleri üzerinde çalışan aracıları
* Windows ve Linux bilgisayarları ortamınızda çalışan aracıları
* System Center Operations Manager
* Yönetim çözümleri 

Tüm aracıları ve çalışma alanına rapor için yapılandırılmış System Center Operations Manager yönetim grubu için yalnız bırakılmış bir durumda devam eder.  Hangi Aracıların çözümleri, stok ve diğer Azure Hizmetleri, devam etmeden önce çalışma alanı ile tümleştirilmiştir.   
 
Bir yöneticiyseniz ve çalışma alanıyla ilişkilendirilmiş birden çok kullanıcı varsa bu kullanıcılar ve çalışma alanı arasındaki ilişki kaybolur. Kullanıcılar başka çalışma alanlarıyla ilişkilendirilmişse bu diğer çalışma alanlarıyla Log Analytics'i kullanmaya devam edebilirler. Kullanıcılar başka çalışma alanlarıyla ilişkili olmamaları durumunda ancak ardından bunlar Log Analytics'i kullanmak için bir çalışma alanı oluşturmanız gerekir. 

1. [Azure portal](http://portal.azure.com) oturum açın. 
2. Azure portalının sol alt köşesinde bulunan **Diğer hizmetler**'e tıklayın. Kaynak listesinde **Log Analytics** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Log Analytics**’i seçin.
3. Log Analytics abonelikleri bölmesinde, bir çalışma alanı seçin ve ardından **Sil** Orta Bölmenin üst.<br><br> ![Çalışma alanı özellikleri bölmesinden Sil seçeneği](media/log-analytics-manage-del-workspace/log-analytics-delete-workspace.png)<br>  
4. Çalışma alanını silme işlemini onaylamak için isteyen onay iletisi penceresi görüntülendiğinde, **Evet**.<br><br> ![Çalışma alanının Silmeyi Onayla](media/log-analytics-manage-del-workspace/log-analytics-delete-workspace-confirm.png)

