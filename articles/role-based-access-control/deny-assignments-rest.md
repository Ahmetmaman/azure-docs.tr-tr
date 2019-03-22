---
title: REST API - Azure kullanarak Azure kaynakları için atamaları izin verilmeyenler listesi | Microsoft Docs
description: Liste öğrenin kullanıcıları, grupları ve uygulamaları, Azure kaynaklarını ve REST API için rol tabanlı erişim denetimi (RBAC) kullanarak atamalarını reddet.
services: active-directory
documentationcenter: na
author: rolyon
manager: mtillman
editor: ''
ms.assetid: ''
ms.service: role-based-access-control
ms.workload: multiple
ms.tgt_pltfrm: rest-api
ms.devlang: na
ms.topic: conceptual
ms.date: 03/13/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 59bcf2b33d203ae216b4965b963a727a6b34ae72
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/18/2019
ms.locfileid: "57998416"
---
# <a name="list-deny-assignments-for-azure-resources-using-the-rest-api"></a>REST API kullanarak Azure kaynakları için atamaları izin verilmeyenler listesi

[Atamalar Reddet](deny-assignments.md) bir rol ataması bunları erişim verse bile kullanıcıların belirli bir Azure kaynak eylemler gerçekleştirme. Bu makalede listesine REST API'SİNİN nasıl kullanılacağı atamaları reddet.

> [!NOTE]
> Şu anda kendi ekleyebilirsiniz tek yolu reddetme atamaları olan Azure şemaları kullanarak. Daha fazla bilgi için [yeni kaynaklar ile Azure Blueprint kaynak kilitleri korumak](../governance/blueprints/tutorials/protect-new-resources.md).

## <a name="prerequisites"></a>Önkoşullar

Bir reddetme atama hakkında bilgi edinmek için şunlara sahip olmalısınız:

- `Microsoft.Authorization/denyAssignments/read` çoğu dahil izni [Azure kaynakları için yerleşik roller](built-in-roles.md).

## <a name="list-a-single-deny-assignment"></a>İzin verilmeyenler listesi tek bir atama

1. Aşağıdaki isteği başlatın:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/denyAssignments/{deny-assignment-id}?api-version=2018-07-01-preview
    ```

1. URI içinde değiştirin *{kapsamı}* Reddet atamalarını listelemek istediğiniz kapsama sahip.

    | Kapsam | Type |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Abonelik |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Kaynak grubu |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Kaynak |

1. Değiştirin *{Reddet-atama-kimliği}* almak istediğiniz Reddet atama tanımlayıcısına sahip.

## <a name="list-multiple-deny-assignments"></a>Birden çok listesinde atamaları Reddet

1. Aşağıdaki isteklerini biriyle başlayın:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/denyAssignments?api-version=2018-07-01-preview
    ```

    İsteğe bağlı parametrelerle:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/denyAssignments?api-version=2018-07-01-preview&$filter={filter}
    ```

1. URI içinde değiştirin *{kapsamı}* Reddet atamalarını listelemek istediğiniz kapsama sahip.

    | Kapsam | Type |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Abonelik |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Kaynak grubu |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Kaynak |

1. Değiştirin *{filter}* reddetme atama listesini filtrelemek için uygulamak istediğiniz koşulu.

    | Filtre | Açıklama |
    | --- | --- |
    | (filtre yok) | Tüm atamaları en üstünde ve aşağıda belirtilen kapsam reddet. |
    | `$filter=atScope()` | Atamalar ve üzeri yalnızca belirtilen kapsam için izin verilmeyenler listesi. Reddetme atamaları subscopes olarak içermez. |
    | `$filter=denyAssignmentName%20eq%20'{deny-assignment-name}'` | Belirtilen ada sahip atamaları izin verilmeyenler listesi. |

## <a name="list-deny-assignments-at-the-root-scope-"></a>Atamalarını (/) kök kapsamda izin verilmeyenler listesi

1. Açıklandığı gibi erişimini yükseltme [için Azure Active Directory'de genel yönetici erişimini yükseltme](elevate-access-global-admin.md).

1. Aşağıdaki isteği kullanın:

    ```http
    GET https://management.azure.com/providers/Microsoft.Authorization/denyAssignments?api-version=2018-07-01-preview&$filter={filter}
    ```

1. Değiştirin *{filter}* reddetme atama listesini filtrelemek için uygulamak istediğiniz koşulu. Bir filtre gereklidir.

    | Filtre | Açıklama |
    | --- | --- |
    | `$filter=atScope()` | Atamalar yalnızca kök kapsamı için izin verilmeyenler listesi. Reddetme atamaları subscopes olarak içermez. |
    | `$filter=denyAssignmentName%20eq%20'{deny-assignment-name}'` | Belirtilen ada sahip atamaları izin verilmeyenler listesi. |

1. Yükseltilmiş erişim kaldırın.

## <a name="next-steps"></a>Sonraki adımlar

- [Anlamak Azure kaynakları için atamaları Reddet](deny-assignments.md)
- [Azure Active Directory'de Genel Yönetici erişimini yükseltme](elevate-access-global-admin.md)
- [Azure REST API Başvurusu](/rest/api/azure/)
