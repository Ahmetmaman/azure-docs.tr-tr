---
title: Azure işlevleri çalışma zamanına genel bakış | Microsoft Docs
description: Azure işlevleri çalışma zamanı Önizlemesi'ne genel bakış
services: functions
documentationcenter: ''
author: apwestgarth
manager: stefsch
editor: ''
ms.assetid: ''
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 11/28/2017
ms.author: anwestg
ms.openlocfilehash: 2be04c7037dd26755300cf8b7794678a6a958278
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38488469"
---
# <a name="azure-functions-runtime-overview-preview"></a>Azure işlevleri çalışma zamanına genel bakış (Önizleme)

Azure işlevleri çalışma zamanı'nı (Önizleme) modeli şirket içi programlama avantajlarından kolaylık ve esneklik Azure işlevleri'nin yararlanmak yeni bir yol sağlar. Azure işlevleri olarak aynı açık kaynak kökleri üzerine inşa edilen Azure işlevleri çalışma zamanı dağıtılan bir bulut hizmeti olarak neredeyse aynı geliştirme deneyimi sağlamak için şirket içinde olur.

![Azure işlevleri çalışma zamanı Önizleme portalı][1]

Azure işlevleri çalışma zamanı, Azure işlevleri, buluta göndermeden önce deneyimi bir yol sağlar. Geçiş yaptığınızda bu yolla, derleme kod varlıklarına sonra sizinle buluta alınabilir.  Çalışma zamanı, sizin için yeni seçenekler gibi toplu işlemleri gece boyunca çalışacak şekilde şirket içi bilgisayarlarınızı yedek işlem gücünden yararlanarak'kurmak da açılır. Koşullu olarak başka sistemlere, hem şirket içi ve bulut veri göndermek için kuruluşunuzdaki cihazları da kullanabilirsiniz.

Azure işlevleri çalışma zamanı iki parça oluşur:

* Azure işlevleri çalışma zamanı yönetim rolü
* Azure işlevleri çalışma zamanı çalışan rolü

## <a name="azure-functions-management-role"></a>Azure işlevleri yönetim rolü

Azure işlevleri yönetim rolü işlevleri şirket içi yönetimi için bir konak sağlar. Bu rol, aşağıdaki görevleri gerçekleştirir:

* Azure işlevleri Yönetim Portalı'nın aynı içinde gördüğünüz olan, barındırma [Azure portalında](https://portal.azure.com). Portal Azure Portalı'nda olduğu gibi aynı şekilde işlevlerinizi geliştirmenize olanak tanıyan tutarlı bir deneyim sunar.
* İşlevleri arasında birden çok işlevleri çalışanları dağıtıyor.
* Yükleme ve yayımlama profilini içeri aktarma, işlevleri doğrudan Microsoft Visual Studio'dan yayımlayabilirsiniz, böylece bir yayımlama uç noktası sağlama.

## <a name="azure-functions-worker-role"></a>Azure işlevleri çalışan rolü

Azure işlevleri çalışan rolleri, Windows kapsayıcıları içinde dağıtılır ve olan işlev kodunuzu nereye yürütür.  Kuruluşunuzda birden fazla çalışan rollerini de dağıtabilirsiniz ve bu seçeneği, müşterilerin yapabilirsiniz anahtar bir şekilde, yedek işlem gücünü kullanın.  Bir yedek işlem birçok kuruluşta mevcut olduğu bir örneği olan makineler sürekli açık ancak büyük sürelerle kullanılmıyor.

## <a name="minimum-requirements"></a>En düşük gereksinimleri

Azure işlevleri çalışma zamanı ile çalışmaya başlamak için bir SQL Server örneğine erişimi olan Windows Server 2016 veya Windows 10 Creators Update ile bir makine olmalıdır.

## <a name="next-steps"></a>Sonraki Adımlar

Yükleme [Azure işlevleri çalışma zamanı Önizleme](https://aka.ms/azafrdoc)

<!--Image references-->
[1]: ./media/functions-runtime-overview/AzureFunctionsRuntime_Portal.png
