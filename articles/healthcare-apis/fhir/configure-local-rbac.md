---
title: FHıR için Azure API 'SI için yerel rol tabanlı erişim denetimi 'ni (yerel RBAC) yapılandırma
description: Bu makalede, FHıR için Azure API 'sinin veri düzlemi için dış bir Azure AD kiracısı kullanacak şekilde nasıl yapılandırılacağı açıklanır.
author: matjazl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: reference
ms.date: 03/15/2020
ms.author: matjazl
ms.openlocfilehash: 096e4e3ecbcedaec674e074a2baccbb336e03c94
ms.sourcegitcommit: 225e4b45844e845bc41d5c043587a61e6b6ce5ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/11/2021
ms.locfileid: "103020518"
---
# <a name="configure-local-rbac-for-fhir"></a>FHıR için yerel RBAC 'yi yapılandırma 

Bu makalede, FHıR için Azure API 'sinin, veri düzlemi erişimini yönetmek için dış, ikincil Azure Active Directory kiracı kullanacak şekilde nasıl yapılandırılacağı açıklanmaktadır. Bu modu yalnızca aboneliğinizle ilişkili Azure Active Directory kiracı kullanmanız mümkün değilse kullanın.

> [!NOTE]
> FHıR hizmeti veri düzlemi, aboneliğinizle ilişkili birincil Azure Active Directory kiracınızı kullanacak şekilde yapılandırıldıysa, [veri düzlemi rolleri atamak Için Azure RBAC kullanın](configure-azure-rbac.md).

## <a name="add-service-principal"></a>Hizmet sorumlusu ekleme

Yerel RBAC, FHıR sunucunuza bir dış Azure Active Directory kiracı kullanmanıza olanak sağlar. Yerel RBAC sisteminin bu Kiracıdaki grup üyeliklerini denetlemesine izin vermek için, FHıR için Azure API 'sinin kiracıda bir hizmet sorumlusu olmalıdır. Bu hizmet sorumlusu, FHıR için Azure API 'SI dağıtmış olan aboneliklerde bulunan kiracılarda otomatik olarak oluşturulur, ancak kiracınızın kendisine bağlı bir aboneliği yoksa, bir kiracı yöneticisinin aşağıdaki komutlardan biriyle bu hizmet sorumlusunu oluşturması gerekir:

`Az`PowerShell modülünü kullanma:

```azurepowershell-interactive
New-AzADServicePrincipal -ApplicationId 3274406e-4e0a-4852-ba4f-d7226630abb7
```

isterseniz PowerShell modülünü de kullanabilirsiniz `AzureAd` :

```azurepowershell-interactive
New-AzureADServicePrincipal -AppId 3274406e-4e0a-4852-ba4f-d7226630abb7
```

isterseniz Azure CLı 'yi de kullanabilirsiniz:

```azurecli-interactive
az ad sp create --id 3274406e-4e0a-4852-ba4f-d7226630abb7
```

## <a name="configure-local-rbac"></a>Yerel RBAC 'yi yapılandırma

FHıR için Azure API 'sini, **kimlik doğrulama** dikey penceresinde dış veya ikincil Azure Active Directory kiracı kullanacak şekilde yapılandırabilirsiniz:

![Yerel RBAC atamaları](media/rbac/local-rbac-guids.png).

Yetkili kutusuna geçerli bir Azure Active Directory kiracı girin. Kiracı doğrulandıktan sonra **Izin verilen nesne kimlikleri** kutusu etkinleştirilmelidir ve kimlik nesnesi kimliklerinin bir listesini girebilirsiniz. Bu kimlikler, öğesinin kimlik nesne kimlikleri olabilir:

* Azure Active Directory Kullanıcı.
* Bir Azure Active Directory hizmet sorumlusu.
* Bir Azure Active Directory güvenlik grubu.

Daha fazla ayrıntı için [kimlik nesne kimliklerini bulma](find-identity-object-ids.md) makalesini okuyabilirsiniz.

Gerekli nesne kimliklerini girdikten sonra **Kaydet** ' e tıklayın ve atanan kullanıcılar, hizmet sorumluları veya grupları kullanarak veri düzlemine erişmeyi denemeden önce değişikliklerin kaydedilmesini bekleyin.

## <a name="caching-behavior"></a>Önbelleğe alma davranışı

FHıR için Azure API 'SI, kararları 5 dakikaya kadar önbelleğe alacak. İzin verilen nesne kimlikleri listesine ekleyerek FHıR sunucusuna bir kullanıcı erişimi verirseniz veya onları listeden kaldırırsanız, bu, en fazla beş dakika sonra yayarak yayma izinleridir.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir dış (ikincil) Azure Active Directory kiracısı kullanarak FHıR veri düzlemi erişimi atamayı öğrendiniz. Daha sonra FHıR için Azure API 'SI için ek ayarlar hakkında bilgi edinin:
 
>[!div class="nextstepaction"]
>[Ek ayarlar FHıR için Azure API](azure-api-for-fhir-additional-settings.md)
