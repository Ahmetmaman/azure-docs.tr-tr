---
title: Windows sanal masaüstü 'nde temsilci erişimi-Azure
description: Örnekler de dahil olmak üzere Windows sanal masaüstü dağıtımında yönetim özellikleri atama.
author: Heidilohr
ms.topic: conceptual
ms.date: 04/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: f2aa2c74704cf89c082d2837b39e82902efa0a62
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "88010064"
---
# <a name="delegated-access-in-windows-virtual-desktop"></a>Windows Sanal Masaüstü'ne temsilci erişimi

>[!IMPORTANT]
>Bu içerik Azure Resource Manager Windows sanal masaüstü nesneleri ile Windows sanal masaüstü için geçerlidir. Azure Resource Manager nesneleri olmadan Windows sanal masaüstü (klasik) kullanıyorsanız, [Bu makaleye](./virtual-desktop-fall-2019/delegated-access-virtual-desktop-2019.md)bakın.

Windows sanal masaüstü, belirli bir kullanıcının bir rol atayarak izin verilen erişim miktarını tanımlamanızı sağlayan bir temsilci erişim modeli sağlar. Rol atamasının üç bileşeni vardır: güvenlik sorumlusu, rol tanımı ve kapsam. Windows sanal masaüstü temsilcisi erişim modeli, Azure RBAC modelini temel alır. Belirli rol atamaları ve bileşenleri hakkında daha fazla bilgi edinmek için bkz. [Azure rol tabanlı erişim denetimine genel bakış](../role-based-access-control/built-in-roles.md).

Windows sanal masaüstü temsilcisi erişimi, rol atamasının her bir öğesi için aşağıdaki değerleri destekler:

* Güvenlik sorumlusu
    * Kullanıcılar
    * Kullanıcı grupları
    * Hizmet sorumluları
* Rol tanımı
    * Yerleşik roller
    * Özel roller
* Kapsam
    * Konak havuzları
    * Uygulama grupları
    * Çalışma Alanları

## <a name="powershell-cmdlets-for-role-assignments"></a>Rol atamaları için PowerShell cmdlet 'leri

Başlamadan önce, henüz yapmadıysanız Windows sanal masaüstü PowerShell modülünü ayarlamak için [PowerShell modülünü ayarlama](powershell-module.md) bölümündeki yönergeleri izlediğinizden emin olun.

Windows sanal masaüstü, uygulama gruplarını kullanıcılara veya Kullanıcı gruplarına yayımlarken Azure rol tabanlı erişim denetimi (Azure RBAC) kullanır. Masaüstü Sanallaştırma Kullanıcı rolü kullanıcı veya kullanıcı grubuna atanır ve kapsam uygulama grubudur. Bu rol, kullanıcıya uygulama grubunda özel veri erişimi sağlar.

Bir uygulama grubuna Azure Active Directory kullanıcıları eklemek için aşağıdaki cmdlet 'i çalıştırın:

```powershell
New-AzRoleAssignment -SignInName <userupn> -RoleDefinitionName "Desktop Virtualization User" -ResourceName <appgroupname> -ResourceGroupName <resourcegroupname> -ResourceType 'Microsoft.DesktopVirtualization/applicationGroups'
```

Azure Active Directory kullanıcı grubunu bir uygulama grubuna eklemek için aşağıdaki cmdlet 'i çalıştırın:

```powershell
New-AzRoleAssignment -ObjectId <usergroupobjectid> -RoleDefinitionName "Desktop Virtualization User" -ResourceName <appgroupname> -ResourceGroupName <resourcegroupname> -ResourceType 'Microsoft.DesktopVirtualization/applicationGroups'
```

## <a name="next-steps"></a>Sonraki adımlar

Her rolün kullanabileceği PowerShell cmdlet 'lerinin daha kapsamlı bir listesi için bkz. [PowerShell Başvurusu](/powershell/windows-virtual-desktop/overview).

Azure RBAC 'de desteklenen rollerin tam listesi için bkz. [Azure yerleşik rolleri](../role-based-access-control/built-in-roles.md).

Windows sanal masaüstü ortamı ayarlama yönergeleri için bkz. [Windows sanal masaüstü ortamı](environment-setup.md).
