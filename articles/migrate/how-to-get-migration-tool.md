---
title: Geçiş makineleri değerlendirme sonrasında Azure geçişi ile | Microsoft Docs
description: Geçiş için öneriler alın açıklar Azure geçişi hizmeti ile bir değerlendirme çalıştırdıktan sonra makineleri.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 09/17/2018
ms.author: raynew
ms.openlocfilehash: 0b02ae4b75426b379ad7c124f5ddeb053c142ce6
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/17/2018
ms.locfileid: "45730303"
---
# <a name="migrate-machines-after-assessment"></a>Değerlendirmenin ardından makineleri geçirme


[Azure geçişi](migrate-overview.md) şirket içi makineleri, Azure'a geçiş için uygun ve makine Azure'da çalıştırmak için boyutlandırma ve maliyet tahminleri sağlar olup olmadığını denetlemek için değerlendirir. Şu anda Azure geçişi yalnızca makineler geçiş için değerlendirir. Geçiş, diğer Azure Hizmetleri kullanılarak gerçekleştirilir.

Bu makalede, geçiş değerlendirmesi çalıştırdıktan sonra geçiş aracı için öneriler almak açıklar.

## <a name="migration-tool-suggestion"></a>Geçiş Aracı önerisi

Geçiş Araçları ile ilgili öneriler almak için şirket içi ortamın bir derin bulma yapmanız gerekir. Derin bulma şirket içi makinelere aracılar yükleyerek gerçekleştirilir.  

1. Bir Azure geçişi projesi oluşturun, şirket içi makineleri keşfetmek ve geçiş değerlendirmesi oluşturun. [Daha fazla bilgi edinin](tutorial-assessment-vmware.md).
2. İndirin ve önerilen geçiş yöntemi görmek istediğiniz her bir şirket içi makinede Azure geçişi aracılarını yükleyin. [Bu yordamı izlemeden](how-to-create-group-machine-dependencies.md#prepare-for-dependency-visualization) aracıları yüklemek için.
2. Lift-and-shift ile taşıma geçiş için uygun olan şirket içi makinelerinizi belirleyin. Bunlar, VM'ler üzerinde çalışan uygulamalar herhangi bir değişiklik gerektirmez ve olarak geçirilebilir.
3. Lift-and-shift ile taşıma geçiş için Azure Site Recovery kullanmanızı öneririz. [Daha fazla bilgi edinin](../site-recovery/tutorial-migrate-on-premises-to-azure.md). Alternatif olarak, azure'a geçiş destekleyen üçüncü taraf araçları kullanabilirsiniz.
4. Diğer bir deyişle, lift-and-shift ile taşıma geçiş için uygun olmayan şirket içi makineleriniz varsa, VM'nin tamamını yerine belirli bir uygulama geçirmek istiyorsanız diğer geçiş araçları kullanabilirsiniz. Örneğin, öneririz [Azure veritabanı geçiş hizmeti](https://azure.microsoft.com/campaigns/database-migration/) böyle bir SQL Server, MySQL veya Oracle azure'a geçirmek istiyorsanız şirket içi veritabanları.


## <a name="review-suggested-migration-methods"></a>Önerilen geçiş yöntemleri gözden geçirin

1. Önerilen geçiş yöntemi sağlayabilmek için önce bir Azure geçişi projesi oluşturun, şirket içi makineleri keşfetmek ve geçiş değerlendirmesi gerekir. [Daha fazla bilgi edinin](tutorial-assessment-vmware.md).
2. Değerlendirme oluşturulduktan sonra projede görüntüleme > **genel bakış** > **Pano**. Tıklayın **hazır olma durumu değerlendirmesi**

    ![Hazır olma durumu değerlendirmesi](./media/tutorial-assessment-vmware/assessment-report.png)  

3. İçinde **önerilen araç**, geçiş için kullanabileceğiniz araçlar önerilerinizi gözden geçirin.

    ![Önerilen araç](./media/tutorial-assessment-vmware/assessment-suitability.png)




## <a name="next-steps"></a>Sonraki adımlar

Değerlendirmelerin nasıl hesaplandığı hakkında [daha fazla bilgi](concepts-assessment-calculation.md) edinin.
