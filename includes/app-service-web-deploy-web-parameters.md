---
author: cephalin
ms.service: app-service-web
ms.topic: include
ms.date: 11/03/2016
ms.author: cephalin
ms.openlocfilehash: 5bde217601d27129e044b64d90184727ea717950
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50134063"
---
Azure Resource Manager sayesinde, şablon dağıtıldığında belirtmek istediğiniz değerlerin parametrelerini siz tanımlarsınız. Şablon parametreleri olarak adlandırılan bir tüm parametre değerlerini içeren bölüm içerir.
Dağıtmakta olduğunuz projeye veya dağıtım yaptığınız ortama bağlı olarak değişir bu değerleri için bir parametre tanımlamanız gerekir. Her zaman aynı kalır değerler için parametre tanımlamayın. Her parametre değeri, dağıtılan kaynakları tanımlamak için şablonda kullanılır. 

Parametreleri tanımlarken kullanmak **allowedValues** alan, bir kullanıcı değerleri belirtmek için dağıtım sırasında sağlayabilir. Kullanım **defaultValue** dağıtımı sırasında herhangi bir değer sağlanmazsa parametreye bir değer atamak için alan.

Biz şablondaki her bir parametre açıklanmıştır.

### <a name="sitename"></a>Site adı
Oluşturmak istediğiniz web uygulamasının adı.

    "siteName":{
      "type":"string"
    }

### <a name="hostingplanname"></a>hostingPlanName
Web uygulamasını barındırmak için kullanılacak App Service planı adı.

    "hostingPlanName":{
      "type":"string"
    }

### <a name="sku"></a>SKU
Barındırma planı için bir fiyatlandırma katmanı.

    "sku": {
      "type": "string",
      "allowedValues": [
        "F1",
        "D1",
        "B1",
        "B2",
        "B3",
        "S1",
        "S2",
        "S3",
        "P1",
        "P2",
        "P3",
        "P4"
      ],
      "defaultValue": "S1",
      "metadata": {
        "description": "The pricing tier for the hosting plan."
      }
    }

Şablon, bu parametre için izin verilen değerleri tanımlar ve hiçbir değer belirtilmemişse, varsayılan değer (S1) atar.

### <a name="workersize"></a>workerSize
Örnek boyutu (küçük, Orta veya büyük) barındırma planı.

    "workerSize":{
      "type":"string",
      "allowedValues":[
        "0",
        "1",
        "2"
      ],
      "defaultValue":"0"
    }

Şablon (0, 1 veya 2) Bu parametre için izin verilen değerleri tanımlar ve hiçbir değer belirtilmemişse, varsayılan değer (0) atar. Değerler için küçük, Orta ve büyük karşılık gelir.

