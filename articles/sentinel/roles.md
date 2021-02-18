---
title: Azure Sentinel izinleri | Microsoft Docs
description: Bu makalede, Azure Sentinel 'in kullanıcılara izin atamak için Azure rol tabanlı erişim denetimi nasıl kullandığı ve her bir rol için izin verilen eylemleri tanımladığı açıklanır.
services: sentinel
cloud: na
documentationcenter: na
author: yelevin
manager: rkarlin
ms.assetid: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/28/2020
ms.author: yelevin
ms.openlocfilehash: f9b50c831b435a6189a9b9e4d79a934a2661b033
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100570401"
---
# <a name="permissions-in-azure-sentinel"></a>Azure Sentinel'de izinler

Azure Sentinel, Azure 'daki kullanıcılara, gruplara ve hizmetlere atanabilecek [yerleşik roller](../role-based-access-control/built-in-roles.md) sağlamak için [Azure rol tabanlı erişim denetimi 'NI (Azure RBAC)](../role-based-access-control/role-assignments-portal.md) kullanır.

Azure Sentinel 'e uygun erişim sağlamak için güvenlik işlemleri ekibiniz dahilinde roller oluşturup atamak için Azure RBAC kullanın. Farklı roller, Azure Sentinel 'in kullanıcılarının görebileceği ve yapabilecekleri ayrıntılı denetim sağlar. Azure rolleri, Azure Sentinel çalışma alanına doğrudan atanabilir (aşağıdaki nota bakın) veya çalışma alanının ait olduğu bir abonelikte veya kaynak grubunda Azure Sentinel 'in devraldığı bir abonelik veya kaynak grubu olabilir.

## <a name="roles-for-working-in-azure-sentinel"></a>Azure Sentinel 'de çalışmak için roller

### <a name="azure-sentinel-specific-roles"></a>Azure Sentinel'e özgü roller

Üç adanmış yerleşik Azure Sentinel rolü vardır.

**Tüm Azure Sentinel yerleşik rolleri, Azure Sentinel çalışma alanınızdaki verilere yönelik okuma erişimi verir.**

- [Azure Sentinel okuyucusu](../role-based-access-control/built-in-roles.md#azure-sentinel-reader) verileri, olayları, çalışma kitaplarını ve diğer Azure Sentinel kaynaklarını görüntüleyebilir.

- [Azure Sentinel Yanıtlayıcı](../role-based-access-control/built-in-roles.md#azure-sentinel-responder) , yukarıdaki, olayları yönetme (ata, kapat vb.)

- [Azure Sentinel katılımcısı](../role-based-access-control/built-in-roles.md#azure-sentinel-contributor) , yukarıdaki ' a ek olarak, çalışma kitapları, analiz kuralları ve diğer Azure Sentinel kaynakları oluşturabilir ve düzenleyebilir.

> [!NOTE]
>
> - En iyi sonuçlar için, bu rollerin Azure Sentinel çalışma alanını içeren **kaynak grubunda** atanması gerekir. Bu şekilde, bu kaynaklar aynı kaynak grubuna yerleştirilmesi gerektiği için, roller Azure Sentinel 'i desteklemek için dağıtılan tüm kaynaklara uygulanır.
>
> - Diğer bir seçenek de rolleri doğrudan Azure Sentinel **çalışma alanına** atamanız olur. Bunu yaparsanız, bu çalışma alanındaki Securityınsights **çözüm kaynağında** aynı rolleri de atamanız gerekir. Bunları diğer kaynaklara da atamanız gerekebilir ve kaynaklarda rol atamalarını sürekli olarak yönetmeniz gerekir.

### <a name="additional-roles-and-permissions"></a>Ek roller ve izinler

Belirli iş gereksinimlerine sahip kullanıcılara görevlerini gerçekleştirmek için ek roller veya belirli izinler atanması gerekebilir.

- Tehditlere yönelik yanıtları otomatik hale getirmek için PlayBook 'lar ile çalışma

    Azure Sentinel otomatik tehdit yanıtı için **PlayBook 'ları** kullanır. Playbooks **Azure Logic Apps** kurulmuştur ve ayrı bir Azure kaynağıdır. Güvenlik işlemleri takımınızın belirli üyelerine, güvenlik düzenleme, otomasyon ve yanıt (SOAR) işlemleri için Logic Apps kullanma yeteneği atamak isteyebilirsiniz. PlayBook 'ları kullanmaya açık izin atamak için [Logic App katılımcısı](../role-based-access-control/built-in-roles.md#logic-app-contributor) rolünü veya [mantıksal uygulama işletmeni](../role-based-access-control/built-in-roles.md#logic-app-operator) rolünü kullanabilirsiniz.

- Veri kaynaklarını Azure Sentinel 'e bağlama

    Bir kullanıcının **veri bağlayıcıları** eklemesi Için, Azure Sentinel çalışma alanında Kullanıcı yazma izinlerini atamanız gerekir. Ayrıca, ilgili bağlayıcı sayfasında listelendiği gibi her bağlayıcı için gerekli ek izinleri göz önünde bulabilirsiniz.

- Olayları atayan Konuk kullanıcılar

    Bir Konuk kullanıcının olayları atayabilmesi gerekiyorsa, Azure Sentinel Yanıtlayıcı rolüne ek olarak, kullanıcının da [Dizin okuyucu](../active-directory/roles/permissions-reference.md#directory-readers)rolüne atanması gerekir. Bu rolün bir Azure rolü, ancak bir **Azure Active Directory** rolü *olmadığı* ve normal (konuk olmayan) kullanıcıların bu rolü varsayılan olarak atandığını unutmayın. 

Yan yana karşılaştırma için [aşağıdaki tabloya](#roles-and-allowed-actions)bakın.

### <a name="other-roles-you-might-see-assigned"></a>Atanmış olarak görebileceğiniz diğer roller

Azure Sentinel 'e özgü Azure rolleri atarken, diğer amaçlar için kullanıcılara atanmış olabilecek diğer Azure ve Log Analytics Azure rolleri arasında gelebiliriz. Bu rollerin Azure Sentinel çalışma alanınıza ve diğer kaynaklara erişim de dahil olmak üzere daha geniş bir izin kümesi verildiğini bilmeniz gerekir:

- **Azure rolleri:** [sahip](../role-based-access-control/built-in-roles.md#owner), [katkıda bulunan](../role-based-access-control/built-in-roles.md#contributor)ve [okuyucu](../role-based-access-control/built-in-roles.md#reader). Azure rolleri, Log Analytics çalışma alanları ve Azure Sentinel kaynakları dahil olmak üzere tüm Azure kaynaklarınız genelinde erişim izni verir.

- **Log Analytics roller:** [katkıda bulunan](../role-based-access-control/built-in-roles.md#log-analytics-contributor) ve [Log Analytics okuyucu](../role-based-access-control/built-in-roles.md#log-analytics-reader)Log Analytics. Log Analytics roller Log Analytics çalışma alanlarınızın erişimine izin verir. 

Örneğin, Azure Sentinel **Reader** rolünü atayan, ancak Azure **Sentinel katkıda** bulunan rolüne sahip olmayan bir Kullanıcı, Azure düzeyinde **katkıda** bulunan rolü atanmışsa Azure Sentinel 'de öğeleri düzenleyebilecektir. Bu nedenle, yalnızca Azure Sentinel 'de bir kullanıcıya izin vermek istiyorsanız, bu kullanıcının önceki izinlerini dikkatle kaldırmanız gerekir ve bu, başka bir kaynağa gereken erişimi bozmayın.

## <a name="roles-and-allowed-actions"></a>Roller ve izin verilen eylemler

Aşağıdaki tabloda Azure Sentinel'deki roller ve izin verilen eylemler özetlenmiştir. 

| Rol | Playbook'ları oluşturma ve çalıştırma| Çalışma kitaplarını, analiz kurallarını ve diğer Azure Sentinel kaynaklarını oluşturma ve düzenleme | Olayları yönetme (Kapat, ata, vb.) | Verileri, olayları, çalışma kitaplarını ve diğer Azure Sentinel kaynaklarını görüntüleyin |
|---|---|---|---|---|
| Azure Sentinel Okuyucusu | -- | -- | -- | &#10003; |
| Azure Sentinel Yanıtlayıcısı | -- | -- | &#10003; | &#10003; |
| Azure Sentinel Katkıda Bulunanı | -- | &#10003; | &#10003; | &#10003; |
| Azure Sentinel katkıda bulunan + Logic App Katılımcısı | &#10003; | &#10003; | &#10003; | &#10003; |

## <a name="custom-roles-and-advanced-azure-rbac"></a>Özel roller ve gelişmiş Azure RBAC

- Ya da yerine, Azure yerleşik rollerini kullanarak Azure Sentinel için Azure özel rolleri oluşturabilirsiniz. Azure Sentinel için Azure özel rolleri, [Azure Sentinel 'e](../role-based-access-control/resource-provider-operations.md#microsoftsecurityinsights) ve [Azure Log Analytics kaynaklarına](../role-based-access-control/resource-provider-operations.md#microsoftoperationalinsights)özel izinler temelinde diğer [Azure özel rollerini](../role-based-access-control/custom-roles-rest.md#create-a-custom-role)oluşturduğunuz şekilde oluşturulur.

- Azure Sentinel çalışma alanınızdaki veriler genelinde gelişmiş Log Analytics Azure rol tabanlı erişim denetimini kullanabilirsiniz. Bu hem veri türü temelindeki Azure RBAC'yi hem de kaynak odaklı Azure RBAC'yi içerir. Log Analytics rolleriyle ilgili daha fazla bilgi için bkz. [Azure İzleyici’de günlük verilerini ve çalışma alanlarını yönetme](../azure-monitor/logs/manage-access.md#manage-access-using-workspace-permissions).

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, Azure Sentinel kullanıcıları için rollerle çalışmayı ve her rolün kullanıcılara ne yaptığını öğrendiniz.

* [Azure Sentinel blogu](https://aka.ms/azuresentinelblog). Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulun.
