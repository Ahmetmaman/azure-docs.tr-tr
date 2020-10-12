---
title: Azure reddetme atamalarını anlama-Azure RBAC
description: Azure rol tabanlı erişim denetimi (Azure RBAC) üzerinde Azure reddetme atamaları hakkında bilgi edinin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: ''
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/26/2020
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: ''
ms.openlocfilehash: a5f17f009caa9306631debf511f2c890f8f2a450
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "82733784"
---
# <a name="understand-azure-deny-assignments"></a>Azure reddetme atamalarını anlama

Bir rol atamasına benzer şekilde, *reddetme atama* , erişimi reddetmek amacıyla belirli bir kapsamdaki bir Kullanıcı, Grup veya hizmet sorumlusu için bir reddetme eylemleri kümesi iliştirir. Bir rol ataması erişime izin verirse, atamaları Reddet, kullanıcıların belirli Azure Kaynak eylemlerini gerçekleştirmesini engeller.

Bu makalede, reddetme atamalarının nasıl tanımlandığı açıklanır.

## <a name="how-deny-assignments-are-created"></a>Reddetme atamaları nasıl oluşturulur

Reddetme atamaları, kaynakları korumak için Azure tarafından oluşturulur ve yönetilir. Azure şemaları ve Azure yönetilen uygulamalar, sistem tarafından yönetilen kaynakları korumak için reddetme atamalarını kullanır. Azure şemaları ve Azure yönetilen uygulamalar, atamaları reddetme için tek yoldur. Kendi reddetme atamalarınızı doğrudan oluşturamazsınız. Planların kaynakları kilitlemek için atamaları reddetme kullanımı hakkında daha fazla bilgi için bkz. [Azure 'da kaynak kilitlemeyi anlama](../governance/blueprints/concepts/resource-locking.md).

> [!NOTE]
> Kendi reddetme atamalarınızı doğrudan oluşturamazsınız.

## <a name="compare-role-assignments-and-deny-assignments"></a>Rol atamalarını karşılaştırma ve atamaları reddetme

Atamaları Reddet, benzer bir kalıbı rol atamaları olarak izler, ancak bazı farklılıklar da vardır.

| Özellik | Rol ataması | Atamayı Reddet |
| --- | --- | --- |
| Erişim verme | :heavy_check_mark: |  |
| Erişimi engelleme |  | :heavy_check_mark: |
| Doğrudan oluşturulabilir | :heavy_check_mark: |  |
| Bir kapsamda Uygula | :heavy_check_mark: | :heavy_check_mark: |
| Sorumluları hariç tut |  | :heavy_check_mark: |
| Alt kapsamlar devralınmasını engelle |  | :heavy_check_mark: |
| [Klasik abonelik Yöneticisi](rbac-and-directory-admin-roles.md) atamalarına Uygula |  | :heavy_check_mark: |

## <a name="deny-assignment-properties"></a>Atama özelliklerini Reddet

 Reddetme atama aşağıdaki özelliklere sahiptir:

> [!div class="mx-tableFixed"]
> | Özellik | Gerekli | Tür | Açıklama |
> | --- | --- | --- | --- |
> | `DenyAssignmentName` | Evet | Dize | Reddetme atamasının görünen adı. Adlar, belirli bir kapsam için benzersiz olmalıdır. |
> | `Description` | Hayır | Dize | Reddetme atamasının açıklaması. |
> | `Permissions.Actions` | En az bir eylem veya bir veri eylemi | String [] | Reddetme atamasının erişimi engellediği yönetim işlemlerini belirten dizeler dizisi. |
> | `Permissions.NotActions` | Hayır | String [] | Reddetme atamasından dışlanacak yönetim işlemlerini belirten dizeler dizisi. |
> | `Permissions.DataActions` | En az bir eylem veya bir veri eylemi | String [] | Reddetme atamasının erişimi engellediği veri işlemlerini belirten dizeler dizisi. |
> | `Permissions.NotDataActions` | Hayır | String [] | Reddetme atamasından çıkarılacak veri işlemlerini belirten dizeler dizisi. |
> | `Scope` | Hayır | Dize | Reddetme atamasının geçerli olduğu kapsamı belirten bir dize. |
> | `DoNotApplyToChildScopes` | Hayır | Boole | Reddetme atamasının alt kapsamlar için geçerli olup olmadığını belirtir. Varsayılan değer false 'dur. |
> | `Principals[i].Id` | Evet | String [] | Reddetme atamasının uygulandığı Azure AD Principal nesne kimliklerinin (Kullanıcı, Grup, hizmet sorumlusu veya yönetilen kimlik) bir dizisi. `00000000-0000-0000-0000-000000000000`Tüm sorumluları temsil etmek için boş BIR GUID olarak ayarlayın. |
> | `Principals[i].Type` | Hayır | String [] | Sorumlular tarafından temsil edilen nesne türleri dizisi [i]. ID. `SystemDefined` tüm sorumluları temsil edecek şekilde ayarlanır. |
> | `ExcludePrincipals[i].Id` | Hayır | String [] | Reddetme atamasının uygulanmadığından, Azure AD sorumlusu nesne kimliklerinin (Kullanıcı, Grup, hizmet sorumlusu veya yönetilen kimlik) bir dizisi. |
> | `ExcludePrincipals[i].Type` | Hayır | String [] | Excludesorumlularını [i]. ID tarafından temsil edilen nesne türleri dizisi. |
> | `IsSystemProtected` | Hayır | Boole | Bu reddetme atamasının Azure tarafından oluşturulup oluşturulmayacağını veya silinemeyeceğini belirtir. Şu anda tüm reddetme atamaları sistem korumalıdır. |

## <a name="the-all-principals-principal"></a>Tüm asıl adlar

Reddedilen atamaları desteklemek için, *Tüm sorumlular* adlı sistem tanımlı bir sorumlu tanıtılmıştır. Bu sorumlu, bir Azure AD dizinindeki tüm kullanıcıları, grupları, hizmet sorumlularını ve yönetilen kimlikleri temsil eder. Asıl KIMLIK sıfır bir GUID ise `00000000-0000-0000-0000-000000000000` ve asıl tür ise `SystemDefined` , sorumlu tüm sorumluları temsil eder. Azure PowerShell çıktısında, tüm sorumlular aşağıdaki gibi görünür:

```azurepowershell
Principals              : {
                          DisplayName:  All Principals
                          ObjectType:   SystemDefined
                          ObjectId:     00000000-0000-0000-0000-000000000000
                          }
```

Tüm sorumlular, `ExcludePrincipals` bazı kullanıcılar hariç tüm sorumluları reddedecek şekilde birleştirilebilir. Tüm sorumlular aşağıdaki kısıtlamalara sahiptir:

- Yalnızca içinde kullanılabilir ve ' `Principals` de kullanılamaz `ExcludePrincipals` .
- `Principals[i].Type` olarak ayarlanmalıdır `SystemDefined` .

## <a name="next-steps"></a>Sonraki adımlar

* [Öğretici: Azure şemaları kaynak kilitleri ile yeni kaynakları koruma](../governance/blueprints/tutorials/protect-new-resources.md)
* [Azure portal kullanarak Azure reddetme atamalarını listeleyin](deny-assignments-portal.md)
