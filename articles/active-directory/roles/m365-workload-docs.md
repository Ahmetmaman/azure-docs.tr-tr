---
title: Microsoft 365 hizmetleri arasında yönetici rolü belgeleri-Azure AD | Microsoft Docs
description: Azure Active Directory Microsoft 365 hizmetleri için yönetici rollerine içerik ve API başvuruları bulma
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: roles
ms.topic: reference
ms.date: 11/05/2020
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8178f0753cd6a93caac2f9dece62f21e1c1b311d
ms.sourcegitcommit: 0d171fe7fc0893dcc5f6202e73038a91be58da03
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2020
ms.locfileid: "93378899"
---
# <a name="administrator-roles-for-microsoft-365-services"></a>Microsoft 365 hizmetleri için yönetici rolleri

Microsoft 365 tüm ürünler Azure AD 'de Yönetim rolleriyle yönetilebilir. Bazı ürünler Ayrıca bu ürüne özgü ek roller de sağlar. Her ürün tarafından desteklenen roller hakkında daha fazla bilgi için aşağıdaki tabloya bakın. [Azure Active Directory ' de rol temsili planlamada](concept-delegation.md), yetkilendirme sorunlarının Genel tartışmaları bulunabilir.

## <a name="where-to-find-content"></a>İçeriğin nerede bulunacağı

Microsoft 365 hizmeti | Rol içeriği | API içeriği
---------------------- | ------------------ | -----------------
Office 365 ve Microsoft 365 iş planlarındaki yönetici rolleri | [Yönetici rollerini Microsoft 365](/office365/admin/add-users/about-admin-roles?view=o365-worldwide) | Kullanılamaz
Azure Active Directory (Azure AD) ve Azure AD Kimlik Koruması| [Azure AD yönetici rolleri](permissions-reference.md) | [Graph API](/graph/api/overview?view=graph-rest-1.0)<br>[Rol atamalarını getir](/graph/api/directoryrole-list?view=graph-rest-1.0)
Exchange Online| [Exchange rol tabanlı erişim denetimi](/exchange/understanding-role-based-access-control-exchange-2013-help) |  [Exchange için PowerShell](/powershell/module/exchange/role-based-access-control/add-managementroleentry?view=exchange-ps)<br>[Rol atamalarını getir](/powershell/module/exchange/role-based-access-control/get-rolegroup?view=exchange-ps)
SharePoint Online | [Azure AD yönetici rolleri](permissions-reference.md)<br>Ayrıca [Microsoft 365 SharePoint yönetici rolü hakkında](/sharepoint/sharepoint-admin-role) | [Graph API](/graph/api/overview?view=graph-rest-1.0)<br>[Rol atamalarını getir](/graph/api/directoryrole-list?view=graph-rest-1.0)
Takımlar/Skype Kurumsal | [Azure AD yönetici rolleri](permissions-reference.md) | [Graph API](/graph/api/overview?view=graph-rest-1.0)<br>[Rol atamalarını getir](/graph/api/directoryrole-list?view=graph-rest-1.0)
Güvenlik & Uyumluluk Merkezi (Office 365 Gelişmiş tehdit koruması, Exchange Online koruması, Information Protection) | [Office 365 yönetici rolleri](/office365/SecurityCompliance/permissions-in-the-security-and-compliance-center) | [Exchange PowerShell](/powershell/module/exchange/role-based-access-control/add-managementroleentry?view=exchange-ps)<br>[Rol atamalarını getir](/powershell/module/exchange/role-based-access-control/get-rolegroup?view=exchange-ps)
Güvenli puan | [Azure AD yönetici rolleri](permissions-reference.md) | [Graph API](/graph/api/overview?view=graph-rest-1.0)<br>[Rol atamalarını getir](/graph/api/directoryrole-list?view=graph-rest-1.0)
Uyumluluk Yöneticisi | [Uyumluluk Yöneticisi rolleri](/office365/securitycompliance/meet-data-protection-and-regulatory-reqs-using-microsoft-cloud#permissions-and-role-based-access-control) | Kullanılamaz
Azure Information Protection | [Azure AD yönetici rolleri](permissions-reference.md) | [Graph API](/graph/api/overview?view=graph-rest-1.0)<br>[Rol atamalarını getir](/graph/api/directoryrole-list?view=graph-rest-1.0)
Microsoft Cloud App Security | [Rol tabanlı erişim denetimi](/cloud-app-security/manage-admins) | [API başvurusu](/cloud-app-security/api-tokens) 
Azure Gelişmiş Tehdit Koruması | [Azure ATP rol grupları](/azure-advanced-threat-protection/atp-role-groups) | Kullanılamaz
Windows Defender Gelişmiş Tehdit Koruması | [Windows Defender ATP rol tabanlı erişim denetimi](/windows/security/threat-protection/windows-defender-atp/rbac-windows-defender-advanced-threat-protection) | Kullanılamaz
Privileged Identity Management | [Azure AD yönetici rolleri](permissions-reference.md) | [Graph API](/graph/api/overview?view=graph-rest-1.0)<br>[Rol atamalarını getir](/graph/api/directoryrole-list?view=graph-rest-1.0)
Intune | [Intune rol tabanlı erişim denetimi](/intune/role-based-access-control) | [Graph API](/graph/api/resources/intune-rbac-conceptual?view=graph-rest-beta)<br>[Rol atamalarını getir](/graph/api/intune-rbac-roledefinition-list?view=graph-rest-beta)
Yönetilen masaüstü | [Azure AD yönetici rolleri](permissions-reference.md) | [Graph API](/graph/api/overview?view=graph-rest-1.0)<br>[Rol atamalarını getir](/graph/api/directoryrole-list?view=graph-rest-1.0)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure AD yönetici rolleri atama veya kaldırma](manage-roles-portal.md)
* [Azure AD yönetici rolleri başvurusu](permissions-reference.md)