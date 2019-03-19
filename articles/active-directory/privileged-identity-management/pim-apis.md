---
title: Microsoft Graph API'leri için PIM (Önizleme) | Microsoft Docs
description: Azure Active Directory Privileged Identity Management (PIM için) (Önizleme) Microsoft Graph API'lerini kullanma hakkında bilgi sağlar.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.subservice: pim
ms.topic: overview
ms.date: 11/13/2018
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: c5d853579335653987321fded154eb9fb0f8dbdb
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/18/2019
ms.locfileid: "58000293"
---
# <a name="microsoft-graph-apis-for-pim-preview"></a>Microsoft Graph API'leri için PIM (Önizleme)

Azure AD Privileged Identity Management (Azure portalını kullanarak PIM içinde) gerçekleştirebileceğiniz görevlerin çoğunu için ayrıca kullanarak gerçekleştirebileceğiniz [Microsoft Graph API'lerini](https://developer.microsoft.com/graph/docs/concepts/overview). Bu makalede, Microsoft Graph API'leri için PIM kullanırken bazı önemli kavramlar açıklanmaktadır. Microsoft Graph API'leri hakkında daha fazla ayrıntı için kullanıma [Azure AD Privileged Identity Management API Başvurusu](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/privilegedidentitymanagement_root).

> [!IMPORTANT]
> /Beta sürümünü Microsoft Graph API'leri Önizleme aşamasındadır ve değiştirilebilir. Bu API'leri üretim uygulamalarında kullanılması desteklenmiyor.

## <a name="required-permissions"></a>Gerekli izinler

PIM için Microsoft Graph API'leri çağırmak için olmalıdır **bir veya daha fazla** aşağıdaki izinleri:

- `Directory.AccessAsUser.All`
- `Directory.Read.All`
- `Directory.ReadWrite.All`
- `PrivilegedAccess.ReadWrite.AzureAD`

### <a name="set-permissions"></a>İzin ayarla

PIM için Microsoft Graph API'leri çağırmak uygulamalar için gerekli izinlere sahip olmalıdır. Gerekli izinleri belirlemek için en kolay yolu kullanmaktır [Azure AD'ye onay çerçevesine](../develop/consent-framework.md).

### <a name="set-permissions-in-graph-explorer"></a>Graph Explorer'da izinleri ayarlama

Aramalarınız test etmek için Graph Gezgini kullanıyorsanız Aracı'nda izinleri belirtebilir.

1. Oturum [Graph Gezgini](https://developer.microsoft.com/graph/graph-explorer) genel Yöneticisi olarak.

1. Tıklayın **değiştirme izinleri**.

    ![Graph Gezgini - izinleri değiştirme](./media/pim-apis/graph-explorer.png)

1. Dahil etmek istediğiniz izinleri yanına onay işaretleri koyacağız ekleyin. `PrivilegedAccess.ReadWrite.AzureAD` henüz Graph Explorer'da kullanılamıyor.

    ![Graph Gezgini - izinleri değiştirme](./media/pim-apis/graph-explorer-modify-permissions.png)

1. Tıklayın **değiştirme izinlerini** izin değişiklikleri uygulamak için.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD Privileged Identity Management API Başvurusu](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/privilegedidentitymanagement_root)
