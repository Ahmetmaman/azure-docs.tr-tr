---
title: Bekleyen verilerin şifrelenmesi Content Moderator
titleSuffix: Azure Cognitive Services
description: Bekleyen verilerin şifrelenmesi Content Moderator.
author: erindormier
manager: venkyv
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 03/13/2020
ms.author: egeaney
ms.openlocfilehash: 1401108a594e30790e842ec379724603f11d493f
ms.sourcegitcommit: d76108b476259fe3f5f20a91ed2c237c1577df14
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2020
ms.locfileid: "92913713"
---
# <a name="content-moderator-encryption-of-data-at-rest"></a>Bekleyen verilerin şifrelenmesi Content Moderator

Content Moderator, bulutta kalıcı olduğunda verilerinizi otomatik olarak şifreler, böylece kurumsal güvenlik ve uyumluluk hedeflerinizi karşılamanıza yardımcı olur.

[!INCLUDE [cognitive-services-about-encryption](../includes/cognitive-services-about-encryption.md)]

> [!IMPORTANT]
> Müşteri tarafından yönetilen anahtarlar yalnızca E0 fiyatlandırma katmanında kullanılabilir. Müşteri tarafından yönetilen anahtarları kullanma olanağı istemek için [Content Moderator Customer-Managed anahtar Isteği formunu](https://aka.ms/cogsvc-cmk)doldurun ve gönderebilirsiniz. İsteğinizin durumunu öğrenmek yaklaşık 3-5 iş günü sürer. Talebe bağlı olarak, bir kuyruğa yerleştirilmiş ve alan kullanılabilir olduğunda onaylanmış olabilir. Content Moderator hizmeti ile CMK kullanmaya onaylandıktan sonra, yeni bir Content Moderator kaynak oluşturmanız ve fiyatlandırma katmanı olarak E0 ' ı seçmeniz gerekir. E0 fiyatlandırma katmanı ile Content Moderator kaynağınız oluşturulduktan sonra, yönetilen kimliğinizi ayarlamak için Azure Key Vault kullanabilirsiniz.

[!INCLUDE [cognitive-services-cmk](../includes/cognitive-services-cmk-regions.md)]

[!INCLUDE [cognitive-services-cmk](../includes/configure-customer-managed-keys.md)]

## <a name="enable-data-encryption-for-your-content-moderator-team"></a>Content Moderator ekibiniz için veri şifrelemeyi etkinleştirme

Content Moderator gözden geçirme ekibiniz için veri şifrelemeyi etkinleştirmek için bkz. [hızlı başlangıç: Web üzerinde Try Content moderator](quick-start.md#create-a-review-team).  

> [!NOTE]
> Content Moderator E0 fiyatlandırma katmanıyla bir _kaynak kimliği_ sağlamanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

* CMK 'yı destekleyen hizmetlerin tam listesi için bkz. bilişsel [Hizmetler Için müşteri tarafından yönetilen anahtarlar](../encryption/cognitive-services-encryption-keys-portal.md)
* [Azure Key Vault nedir](../../key-vault/general/overview.md)?
* [Bilişsel hizmetler Customer-Managed anahtar Isteği formu](https://aka.ms/cogsvc-cmk)