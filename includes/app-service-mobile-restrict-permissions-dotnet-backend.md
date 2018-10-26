---
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.author: crdun
ms.openlocfilehash: b609a708a987194398c53bdf83f0d6e1f281808d
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50134236"
---
Varsayılan olarak, Mobile Apps arka uç API'leri anonim olarak çağrılabilir. Ardından, yalnızca kimliği doğrulanmış kullanıcıların erişimini kısıtlamak gerekir.  

* **Node.js geri bitiş (Azure portalı)** :  

    Mobile Apps ayarlarınızda tıklayın **kolay tablolar** ve tablonuzu seçme. Tıklayın **izinleri değiştirme**seçin **kimlik doğrulamalı erişim yalnızca** tüm izinleri ve ardından **Kaydet**.
* **.NET arka ucu (C#)**:  

    Sunucu projesinde gidin **denetleyicileri** > **TodoItemController.cs**. Ekleme `[Authorize]` özniteliğini **TodoItemController** sınıfına aşağıdaki gibi. Yalnızca belirli metotlar için erişimi kısıtlamak için yalnızca sınıf yerine bu yöntemleri için bu öznitelik uygulayabilirsiniz. Sunucu projesini yeniden yayımlayın.

        [Authorize]
        public class TodoItemController : TableController<TodoItem>

* **Node.js arka ucu (aracılığıyla Node.js kodu)** :  

    Tablo erişmesi için kimlik doğrulaması için Node.js sunucusu betiğe aşağıdaki satırı ekleyin:

        table.access = 'authenticated';

    Daha fazla ayrıntı için [nasıl yapılır: tablolara erişim için kimlik doğrulaması gerektiren](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#howto-tables-auth). Hızlı Başlangıç kod projesi, sitesinden öğrenmek için bkz [nasıl yapılır: Git kullanarak Node.js arka ucu hızlı başlangıç kod projesi indirme](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart).
