---
title: Azure AD 'de koşullu erişimle Azure yönetimine erişimi yönetme
description: Azure yönetimine erişimi yönetmek için Azure AD 'de koşullu erişim kullanma hakkında bilgi edinin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: skwan
ms.assetid: 0adc8b11-884e-476c-8c43-84f9bf12a34b
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/15/2019
ms.author: rolyon
ms.reviewer: skwan
ms.openlocfilehash: 2a52635dbaa7a76034f3a535b099320a901e8c07
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "83758784"
---
# <a name="manage-access-to-azure-management-with-conditional-access"></a>Koşullu erişimle Azure yönetimine erişimi yönetme

> [!CAUTION]
> Azure yönetimine erişimi yönetmek üzere bir ilke ayarlamadan önce Koşullu erişimin nasıl çalıştığını anladığınızdan emin olun. Portala kendi erişiminizi engelleyebilen koşullar oluşturduğunuzdan emin olun.

Azure Active Directory (Azure AD) koşullu erişim, belirttiğiniz belirli koşullara göre bulut uygulamalarına erişimi denetler. Erişime izin vermek için ilkedeki gereksinimlerin karşılanıp karşılanmadığını temel alarak erişime izin veren veya erişimi engelleyen koşullu erişim ilkeleri oluşturabilirsiniz. 

Genellikle, bulut uygulamalarınıza erişimi denetlemek için koşullu erişim kullanın. Ayrıca, belirli koşullara göre (oturum açma riski, konum veya cihaz gibi) Azure yönetimine erişimi denetlemek ve Multi-Factor Authentication gibi gereksinimleri zorlamak için ilkeler ayarlayabilirsiniz.

Azure yönetimine yönelik bir ilke oluşturmak için, ilkenin uygulanacağı uygulamayı seçerken **bulut uygulamaları** altında **Microsoft Azure Yönetim** ' i seçersiniz.

![Azure yönetimi için Koşullu Erişim](./media/conditional-access-azure-management/conditional-access-azure-mgmt.png)

Oluşturduğunuz ilke, aşağıdakiler de dahil olmak üzere tüm Azure Yönetim uç noktaları için geçerlidir:

- Azure portal
- Azure Resource Manager sağlayıcı
- Klasik hizmet yönetimi API 'Leri
- Azure PowerShell
- Visual Studio abonelikleri Yönetici portalı
- Azure DevOps
- Azure Data Factory portalı

İlkenin Azure Resource Manager API 'yi çağıran Azure PowerShell için geçerli olduğunu unutmayın. Microsoft Graph çağıran [Azure AD PowerShell](/powershell/azure/active-directory/install-adv2)için de geçerlidir.

Microsoft Azure yönetimi için koşullu erişimi etkinleştirmek üzere örnek bir ilke ayarlama hakkında daha fazla bilgi için bkz. [koşullu erişim: Azure IÇIN MFA gerektirme](../active-directory/conditional-access/howto-conditional-access-policy-azure-management.md).
