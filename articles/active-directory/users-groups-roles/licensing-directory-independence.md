---
title: Birden çok özellikleri etkileşim - Azure Active Directory Kiracı | Microsoft Docs
description: Azure Active Kiracı kiracılarınız kiracılarınız tamamen bağımsız bir kaynak olarak anlayarak yönetme
services: active-tenant
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: article
ms.workload: identity
ms.subservice: users-groups-roles
ms.date: 01/31/2019
ms.author: curtand
ms.custom: it-pro
ms.reviewer: sumitp
ms.openlocfilehash: 6dcc02d8d4c1f0629d69460be28a70c470704d75
ms.sourcegitcommit: fea5a47f2fee25f35612ddd583e955c3e8430a95
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55511386"
---
# <a name="understand-how-multiple-azure-active-directory-tenants-interact"></a>Birden çok Azure Active Directory kiracıları etkileşim anlama

Azure Active Directory'de (Azure AD), her Kiracı tamamen bağımsız bir kaynaktır: Eş yönettiğiniz diğer kiracılardan mantıksal olarak bağımsızdır. Kiracılar arasında üst-alt ilişkisi yoktur. Dizinler kiracılar arasındaki bu bağımsızlığa kaynak bağımsızlığı, yönetim bağımsızlığı ve eşitleme bağımsızlığı dahildir.

## <a name="resource-independence"></a>Kaynak bağımsızlığı
* Oluşturursanız veya bir kiracının kaynak silin, dış kullanıcılar kısmen bu durumun başka bir kiracıdaki herhangi bir kaynak üzerinde herhangi bir etkisi yoktur. 
* Bir etki alanı ile bir kiracı kullanıyorsanız, başka hiçbir kiracıyla kullanılamaz.

## <a name="administrative-independence"></a>Yönetim bağımsızlığı
Kiracı 'Contoso' yönetici olmayan bir kullanıcı bir de test kiracılığınız 'Test' sonra oluşturursa:

* Varsayılan olarak, bir kiracı oluşturan kullanıcı bu yeni kiracıda bir dış kullanıcı olarak eklenmiş ve bu kiracısında genel Yönetici rolüne atanır.
* 'Test' Yöneticisi özel olarak yetki vermediği sürece 'Contoso' Kiracı yöneticilerinin 'Test' kiracısı için doğrudan yönetim ayrıcalıkları var. Ancak, bunlar 'Test' oluşturan bir kullanıcı hesabı denetimi "Contoso" yöneticileri, Kiracı 'Test' erişimi denetleyebilirsiniz
* Size eklemek/bir kiracıda bir kullanıcı için bir yönetici rolü Kaldır, Değiştir, kullanıcının başka bir kiracıya sahip yönetici rolleri etkilemez.

## <a name="synchronization-independence"></a>Eşitleme bağımsızlığı
Her Azure AD kiracısını bağımsız olarak veya tek bir örneğinden verilerini almak için yapılandırabilirsiniz:

* Verileri tek bir AD ormanıyla eşitlemek için Azure AD Connect aracı.
* Bağlayıcı için Forefront Identity bir veya daha fazla şirket içi ormanları ve/veya Azure olmayan AD veri kaynakları ile veri eşitlemek için Manager, Azure Active Kiracı.

## <a name="add-an-azure-ad-tenant"></a>Azure AD kiracısı ekleme
Azure portalında Azure AD kiracısı eklemek için oturum açın [Azure portalında](https://portal.azure.com) bir Azure AD genel yöneticisi olan bir hesapla, sol, seçin ve **yeni**.

> [!NOTE]
> Diğer Azure kaynaklarının aksine, kiracılarınızın bir Azure aboneliğinin alt kaynakları değildir. Azure aboneliğinizin süresi doldu veya iptal edilirse, Azure PowerShell, Azure Graph API'sini veya Office 365 Yönetim merkezini kullanarak Kiracı verilerinize erişmeye devam edebilirsiniz. Ayrıca [başka bir abonelik kiracısıyla ilişkilendirmek](../fundamentals/active-directory-how-subscriptions-associated-directory.md).
>

## <a name="next-steps"></a>Sonraki adımlar
Azure AD lisans sorunları ve en iyi uygulamalar kapsamlı bir bakış için bkz [ne olduğu Azure Active Kiracı lisans?](../fundamentals/active-directory-licensing-whatis-azure-portal.md).
