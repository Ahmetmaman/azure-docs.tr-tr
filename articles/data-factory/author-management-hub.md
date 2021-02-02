---
title: Yönetim merkezi
description: Azure Data Factory yönetim hub 'ında bağlantılarınızı, kaynak denetimi yapılandırmanızı ve küresel yazma özelliklerinizi yönetin
services: data-factory
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
author: dcstwh
ms.author: weetok
manager: anandsub
ms.date: 02/01/2021
ms.openlocfilehash: c3366b7ba0eb0b49d4d5b89481b7bed843e52c8e
ms.sourcegitcommit: eb546f78c31dfa65937b3a1be134fb5f153447d6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/02/2021
ms.locfileid: "99429031"
---
# <a name="management-hub-in-azure-data-factory"></a>Azure Data Factory 'de Yönetim Merkezi

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Azure Data Factory UX içindeki *Yönet* sekmesi tarafından erişilen Yönetim Merkezi, veri fabrikanızın genel yönetim eylemlerini barındıran bir portalıdır. Burada, veri depoları ve dış hesaplar, kaynak denetimi yapılandırması ve tetikleyici ayarları bağlantılarını yönetebilirsiniz.

## <a name="manage-connections"></a>Bağlantıları yönetme

### <a name="linked-services"></a>Bağlı hizmetler

Bağlı hizmetler, dış veri depolarına ve işlem ortamlarına bağlanmak üzere Azure Data Factory için bağlantı bilgilerini tanımlar. Daha fazla bilgi için bkz. [bağlı hizmetler kavramları](concepts-linked-services.md). Bağlı hizmet oluşturma, düzenlemenin ve silmenin yönetimi hub 'ında yapılır.

![Bağlı hizmetleri yönetme](media/author-management-hub/management-hub-linked-services.png)

### <a name="integration-runtimes"></a>Tümleştirme çalışma zamanları

Tümleştirme çalışma zamanı, farklı ağ ortamlarında veri tümleştirme özellikleri sağlamak için Azure Data Factory tarafından kullanılan bir işlem altyapısıdır. Daha fazla bilgi için [Integration Runtime kavramları](concepts-integration-runtime.md)hakkında bilgi edinin. Yönetim merkezinde, tümleştirme çalışma zamanlarını oluşturabilir, silebilir ve izleyebilirsiniz.

![Tümleştirme çalışma zamanlarını yönetme](media/author-management-hub/management-hub-integration-runtime.png)

## <a name="manage-source-control"></a>Kaynak denetimini Yönet

### <a name="git-configuration"></a>Git yapılandırması

Git yapılandırma ayarları altındaki git ile ilgili tüm bilgileri yönetim hub 'ında görüntüleyebilir/düzenleyebilirsiniz. 

Son yayımlanan kayıt bilgileri de listelenir ve ortamlar genelinde en son yayımlanan/dağıtılan kesin yürütmeyi anlamanıza yardımcı olabilir. Üretimde sık düzeltmeler yapılırken da yararlı olabilir.

Daha fazla bilgi için [Azure Data Factory içindeki kaynak denetimi](source-control.md)hakkında bilgi edinin.

![Git deposunu yönetme](media/author-management-hub/management-hub-git.png)

### <a name="parameterization-template"></a>Parametreleştirme şablonu

İşbirliği dalından yayımlarken oluşturulan Kaynak Yöneticisi şablonu parametrelerini geçersiz kılmak için özel bir parametre dosyası oluşturabilir veya düzenleyebilirsiniz. Daha fazla bilgi için [Kaynak Yöneticisi şablonunda özel parametreleri nasıl kullanacağınızı](continuous-integration-deployment.md#use-custom-parameters-with-the-resource-manager-template)öğrenin. Parameterleştirme şablonu yalnızca bir git deposunda çalışırken kullanılabilir. Dosyadaki *arm-template-parameters-definition.js* çalışma dalında yoksa, varsayılan şablonu düzenlediğinizde bunu oluşturacaktır.

![Özel parametreleri Yönet](media/author-management-hub/management-hub-custom-parameters.png)

## <a name="manage-authoring"></a>Yazma yönetimi

### <a name="triggers"></a>Tetikleyiciler

Tetikleyiciler, bir işlem hattı çalıştırmasının ne zaman ekleneceğini tespit eder. Şu anda Tetikleyiciler bir duvar saati zamanlaması üzerinde olabilir, düzenli aralıklarla çalışabilir veya bir olaya bağlı olabilir. Daha fazla bilgi için [tetikleyici yürütme](concepts-pipeline-execution-triggers.md#trigger-execution)hakkında bilgi edinin. Yönetim merkezinde, bir tetikleyicinin geçerli durumunu oluşturabilir, düzenleyebilir, silebilir veya görüntüleyebilirsiniz.

![Tetikleyicinin geçerli durumunun oluşturulacağı, düzenleneceği, silineceği veya görüntüleneceği yerleri gösteren ekran görüntüsü.](media/author-management-hub/management-hub-triggers.png)

### <a name="global-parameters"></a>Genel parametreler

Genel parametreler, herhangi bir ifadede bir işlem hattı tarafından tüketilen bir veri fabrikası genelinde sabitler. Daha fazla bilgi için [genel parametreler](author-global-parameters.md)hakkında bilgi edinin.

![Genel parametreler oluştur](media/author-global-parameters/create-global-parameter-3.png)

## <a name="next-steps"></a>Sonraki adımlar

ADF 'nize [bir git deposu yapılandırma](source-control.md) hakkında bilgi edinin


