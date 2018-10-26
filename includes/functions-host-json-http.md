---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: 5abefd55aa06f53bc3b437b29a2582b3e2239a65
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50133009"
---
```json
{
    "http": {
        "routePrefix": "api",
        "maxOutstandingRequests": 200,
        "maxConcurrentRequests": 100,
        "dynamicThrottlesEnabled": true
    }
}
```

|Özellik  |Varsayılan | Açıklama |
|---------|---------|---------| 
|routeprefix öğesi|api|Tüm yollar için geçerli bir rota öneki. Varsayılan ön ekini kaldırmak için boş bir dize kullanın. |
|maxOutstandingRequests|200 *|Belirli bir zamanda tutulan bekleyen istek sayısı. Bu sınır, kuyruğa alınır, ancak yürütme devam eden yürütmeler birinde yanı sıra başlamamış olan istekleri içerir. "Çok meşgul" bir 429 yanıtı bu sınırın üzerindeki tüm gelen istekler reddedilir. Zamana bağlı bir yeniden deneme stratejileri kullanmak istemiyorsunuz arayanlara izin verir ve ayrıca en fazla istek gecikme denetlemenize yardımcı olur. Bu, yalnızca betik konak yürütme yolu içinde oluşan queuing denetler. ASP.NET istek kuyruğu gibi diğer kuyrukları, efekt ve bu ayar etkilenmez görünmeye devam edecektir. * Varsayılan 1.x sürümü için sınırsızdır. Varsayılan sürüm 2.x bir tüketim planında 200'dür. Varsayılan sürüm için ayrılmış bir planda 2.x sınırsızdır.|
|maxConcurrentRequests|100 *|En fazla paralel olarak yürütülen http işlev sayısı. Bu, kaynak kullanımını yönetmenize yardımcı olabilir denetimi eşzamanlılık için sağlar. Örneğin, sistem kaynakları (cpu/bellek/yuvaları) çok fazla eşzamanlılık çok fazla olduğunda sorunlarına neden olur, kullanan bir http işleve sahip olabilir. Veya hizmet bir üçüncü tarafa Giden istekleri oluşturan bir işlev olabilir ve bu çağrıları oranı sınırlı olması gerekir. Bu gibi durumlarda, burada bir kısıtlama uygulama yardımcı olabilir. * Varsayılan 1.x sürümü için sınırsızdır. Varsayılan sürüm 2.x bir tüketim planında 100'dür. Varsayılan sürüm için ayrılmış bir planda 2.x sınırsızdır.|
|dynamicThrottlesEnabled|TRUE *|Etkinleştirildiğinde, istek işleme ardışık düzenli aralıklarla sistem performansını denetlemek için bağlantı/iş parçacığı/işlemler/bellek/cpu/vb. gibi sayaçları ve bu sayaçlar, herhangi bir yerleşik yüksek eşiği (% 80), üzerinde olması durumunda istekleri bu ayarı neden olacaktır Sayaç normal düzeylerine geri dönene kadar "Çok meşgul" bir 429 yanıtı reddetti. * Varsayılan 1.x sürümü için false. Varsayılan tüketim planında 2.x sürümü için geçerlidir. Varsayılan sürüm için ayrılmış bir planda 2.x false'tur.|
