---
title: Hizmet sorumlusu yönetilen bir kimlik, Azure portalında görüntüleme
description: Hizmet sorumlusu yönetilen bir kimlik, Azure portalında görüntülemek için adım adım yönergeler.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/29/2018
ms.author: priyamo
ms.openlocfilehash: 7d4d37ebb224a8420dd0201be1bc7a8e37d244bf
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55195265"
---
# <a name="view-the-service-principal-of-a-managed-identity-in-the-azure-portal"></a>Hizmet sorumlusu yönetilen bir kimlik, Azure portalında görüntüleme

Azure kaynakları için yönetilen kimlikleri Azure Active Directory'de otomatik olarak yönetilen bir kimlikle Azure hizmetleri sağlar. Bu kimlik, Azure AD kimlik doğrulaması, kimlik bilgilerini kodunuzda zorunda kalmadan destekleyen herhangi bir hizmeti kimlik doğrulaması için kullanabilirsiniz. 

Bu makalede, hizmet sorumlusu, Azure portalını kullanarak bir yönetilen kimlik görüntüleme hakkında bilgi edinin.

## <a name="prerequisites"></a>Önkoşullar

- Azure kaynakları için yönetilen kimliklerle bilmiyorsanız kullanıma [genel bakış bölümünde](overview.md).
- Azure hesabınız yoksa, [ücretsiz bir hesap için kaydolun](https://azure.microsoft.com/free/).
- Etkinleştirme [bir sanal makinede sistem tarafından atanan kimlik](/azure/active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm#system-assigned-managed-identity) veya [uygulama](/azure/app-service/overview-managed-identity#adding-a-system-assigned-identity).

## <a name="view-the-service-principal"></a>Hizmet sorumlusu görüntüleyin

Bu yordam, hizmet sorumlusu, bir VM ile sistem tarafından atanan kimliği etkinleştirildi görüntülemek gösterilmektedir (aynı adımlar, bir uygulama için geçerlidir).

1. Tıklayın **Azure Active Directory** ve ardından **kurumsal uygulamalar**.
2. Altında **uygulama türü**, seçin **tüm uygulamaları**.
3. Arama filtre kutusuna adını yazın VM veya yönetilen kimliği uygulama etkin veya sunulan listeden seçin.

   ![Yönetilen kimlik hizmet sorumlusu portalında görüntüleme](./media/how-to-view-managed-identity-service-principal-portal/view-managed-identity-service-principal-portal.png)

## <a name="next-steps"></a>Sonraki adımlar

[Azure kaynakları için yönetilen kimlikler](/azure/active-directory/managed-identities-azure-resources/overview)

