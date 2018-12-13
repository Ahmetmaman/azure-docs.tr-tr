---
title: Azure Search .NET Yönetim SDK sürüm 2 - Azure Search için yükseltme
description: Azure Search .NET Yönetim SDK sürüm 2 için önceki sürümlerinden yükseltin. Nelerin yeni olduğunu öğrenin ve hangi kodda değişiklik yapmanız gerekmez.
author: brjohnstmsft
manager: jlembicz
ms.author: brjohnst
services: search
ms.service: search
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 01/15/2018
ms.custom: seodec2018
ms.openlocfilehash: 369e5283d7350729e0d8cc44f94f1f2e71b133c3
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53316885"
---
# <a name="upgrading-to-the-azure-search-net-management-sdk-version-2"></a>Azure Search .NET Yönetim SDK sürüm 2 için yükseltme
Sürüm 1.0.2 kullanıyorsanız, eski veya [Azure Search .NET Yönetim SDK'sı](https://aka.ms/search-mgmt-sdk), bu makalede, uygulamanızı sürüm 2 kullanacak şekilde yükseltin yardımcı olur.

Azure Search .NET Yönetim SDK'sı sürüm 2, bazı değişiklikler daha önceki sürümlerin içerir. Kodunuzu değiştirmek, yalnızca en az çaba istemeniz gerekir böylece bunlar çoğunlukla küçük. Bkz: [yükseltme adımları](#UpgradeSteps) yeni SDK sürümü kullanmak kodunuzu değiştirmek konusunda yönergeler için.

<a name="WhatsNew"></a>

## <a name="whats-new-in-version-2"></a>Sürüm 2 yenilikler nelerdir?
Azure Search .NET Yönetim SDK'sı sürüm 2 özellikle 2015-08-19 olarak önceki SDK sürümleri, Azure arama yönetimi REST API'si aynı genel kullanıma sunulan sürümünü hedefleyen. SDK SDK'ın kullanılabilirliği iyileştirmek için kesinlikle istemci tarafı değişiklikleri değişikliklerdir. Bu değişiklikler şunları içerir:

* `Services.CreateOrUpdate` ve kendi zaman uyumsuz sürümleri artık otomatik olarak sağlama yoklaması `SearchService` ve sağlama hizmeti tamamlanana kadar döndürmüyor. Bu tür yoklama kodu kendiniz yazmak zorunda kalmaktan kurtarır.
* El ile sağlama hizmeti yoklama hala istiyorsanız yeni kullanabilirsiniz `Services.BeginCreateOrUpdate` yöntemi veya bir zaman uyumsuz sürümlerini.
* Yeni yöntemler `Services.Update` ve SDK, zaman uyumsuz sürümlere eklenmiştir. Bu yöntemler, artımlı bir hizmeti güncelleştirme desteklemek için HTTP PATCH kullanın. Örneğin, artık bir hizmet geçirerek ölçeklendirebileceğiniz bir `SearchService` yalnızca istenen içeren örneği bu yöntemlere yapılan `partitionCount` ve `replicaCount` özellikleri. Eski yöntem çağırma `Services.Get`, döndürülen değiştirme `SearchService`ve aktarması `Services.CreateOrUpdate` hala desteklenmektedir, ancak artık gerekli değildir. 

<a name="UpgradeSteps"></a>

## <a name="steps-to-upgrade"></a>Yükseltme adımları
İlk olarak, için NuGet başvuru güncelleştirme `Microsoft.Azure.Management.Search` NuGet Paket Yöneticisi konsolu kullanılarak veya ile proje başvurularınızın sağ tıklayıp "Yönetme NuGet paketleri..." Visual Studio'da.

NuGet yeni paketler ve bağımlılıkları İndirildikten sonra projenizi yeniden derleyin. Kodunuzu nasıl yapılandırıldığını bağlı olarak başarıyla yeniden oluşturmak. Bu durumda, başlamaya hazırsınız!

Yapı başarısız olursa, bazı değiştirildi (örneğin, amacı doğrultusunda, birim testi), SDK'sı arabirimleri uyguladık nedeni olabilir. Bu sorunu çözmek için yeni yöntemler gibi uygulama gerekecektir `BeginCreateOrUpdateWithHttpMessagesAsync`.

Derleme hataları düzelttik sonra isterseniz yeni işlevsellikten yararlanmak için uygulamanızı değişiklik yapabilirsiniz. SDK'sı yeni özellikleri ayrıntılı olarak [sürüm 2 yenilikler](#WhatsNew).

## <a name="conclusion"></a>Sonuç
Bildirimleriniz üzerindeki SDK bizim için değerli. Sorunlarla karşılaşırsanız, bize yardım isteyin çekinmeyin [Azure Search MSDN Forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=azuresearch). Bir hata bulursanız, sorunu bildirin [Azure .NET SDK'sı GitHub deposu](https://github.com/Azure/azure-sdk-for-net/issues). "[Azure Search]", bir sorun başlığı önek emin olun.

Azure Search kullandığınız için teşekkür ederiz!
