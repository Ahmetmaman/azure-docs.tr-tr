---
title: Yönetim birimleri ekleme ve kaldırma-Azure Active Directory | Microsoft Docs
description: Azure Active Directory içindeki rol izinlerinin kapsamını kısıtlamak için yönetim birimlerini kullanın.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.topic: how-to
ms.subservice: users-groups-roles
ms.workload: identity
ms.date: 09/22/2020
ms.author: curtand
ms.reviewer: anandy
ms.custom: oldportal;it-pro;
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7b47ea3cc55be26521dfa6e2b3230b477f82f442
ms.sourcegitcommit: 28c5fdc3828316f45f7c20fc4de4b2c05a1c5548
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/22/2020
ms.locfileid: "92377905"
---
# <a name="manage-administrative-units-in-azure-active-directory"></a>Azure Active Directory Yönetim birimlerini yönetme

Azure Active Directory (Azure AD) içinde daha ayrıntılı yönetim denetimi için, bir veya daha fazla yönetim birimiyle (AU) sınırlı bir kapsama sahip bir Azure AD rolüne kullanıcı atayabilirsiniz.

## <a name="get-started"></a>başlarken

1. [Grafik Gezgini](https://aka.ms/ge)aracılığıyla aşağıdaki yönergelerden sorguları çalıştırmak için aşağıdakileri yapın:

    a. Azure portal Azure AD 'ye gidin. Uygulamalar listesinde, **Graph Explorer**' ı seçin ve ardından **Graph Explorer 'A yönetici izni ver**' i seçin.

    !["Yönetici onayı verme" bağlantısını gösteren ekran görüntüsü](./media/admin-units-manage/select-graph-explorer.png)


1. Azure AD PowerShell 'in önizleme sürümünü kullanın.

## <a name="add-an-administrative-unit"></a>Yönetim birimi ekleme

### <a name="use-the-azure-portal"></a>Azure portalını kullanma

1. Azure portal Azure AD ' a gidin ve ardından sol bölmedeki **yönetim birimleri**' ni seçin.

    ![Azure AD 'de Yönetim unitslink ekran görüntüsü](./media/admin-units-manage/nav-to-admin-units.png)

1. **Ekle** ' yi seçin ve ardından yönetim biriminin adını girin. İsteğe bağlı olarak, yönetim biriminin bir açıklamasını ekleyin.

    ![Yönetim biriminin adını girmek için Ekle düğmesinin ve metin kutusunun ekran görüntüsü](./media/admin-units-manage/add-new-admin-unit.png)

1. Yönetim birimini sonlandırmak için **Ekle** ' yi seçin.

### <a name="use-powershell"></a>PowerShell kullanma

Aşağıdaki komutları çalıştırmayı denemeden önce Azure AD PowerShell 'i (Önizleme) yükleyebilirsiniz:

```powershell
Connect-AzureAD
New-AzureADMSAdministrativeUnit -Description "West Coast region" -DisplayName "West Coast"
```

Gerektiğinde, tırnak işaretleri içine alınmış değerleri değiştirebilirsiniz.

### <a name="use-microsoft-graph"></a>Microsoft Graph kullan

```http
Http Request
POST /administrativeUnits
Request body
{
  "displayName": "North America Operations",
  "description": "North America Operations administration"
}
```

## <a name="remove-an-administrative-unit"></a>Yönetim birimini kaldırma

Azure AD 'de, artık yönetici rolleri için bir kapsam birimi olarak ihtiyacınız olmayan bir yönetim birimini kaldırabilirsiniz.

### <a name="use-the-azure-portal"></a>Azure portalını kullanma

1. Azure Portal **Azure AD**  >  **yönetim birimleri**' ne gidin. 
1. Silinecek yönetim birimini seçin ve **Sil**' i seçin. 
1. Yönetim birimini silmek istediğinizi onaylamak için **Evet**' i seçin. Yönetim birimi silinir.

![Yönetim birimi silme düğmesi ve onay penceresinin ekran görüntüsü](./media/admin-units-manage/select-admin-unit-to-delete.png)

### <a name="use-powershell"></a>PowerShell kullanma

```powershell
$delau = Get-AzureADMSAdministrativeUnit -Filter "displayname eq 'DeleteMe Admin Unit'"
Remove-AzureADMSAdministrativeUnit -ObjectId $delau.ObjectId
```

Belirli bir ortam için gereken şekilde, tırnak işaretleri içine alınmış değerleri değiştirebilirsiniz.

### <a name="use-the-graph-api"></a>Graph API kullanın

```http
HTTP request
DELETE /administrativeUnits/{Admin id}
Request body
{}
```

## <a name="next-steps"></a>Sonraki adımlar

* [Yönetici birimindeki kullanıcıları yönetme](admin-units-add-manage-users.md)
* [Yönetim birimindeki grupları yönetme](admin-units-add-manage-groups.md)
