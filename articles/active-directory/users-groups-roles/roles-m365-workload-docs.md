---
title: Microsoft 365 iş yükleri - Azure AD Yönetici rolü İçeriklerinden | Microsoft Docs
description: Microsoft 365 iş yükleri, Azure Active Directory'de yönetici rolleri için içerik ve API başvuruları Bul
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 01/23/2019
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.openlocfilehash: f44f7167cff316b15124448678c4cae90b384ed9
ms.sourcegitcommit: b4755b3262c5b7d546e598c0a034a7c0d1e261ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54890087"
---
# <a name="administrator-roles-for-microsoft-365-workloads"></a>Microsoft 365 iş yükleri için yönetici rolleri

Bu makalede, Microsoft 365 iş yükleri rol içeriği ve özellik alanı API içerik için bağlantılarla birlikte bir listesini sağlar. Temsilci seçme sorunlarını genel tartışmalar bulunabilir [rol temsilcisi seçme içinde Azure Active Directory planlama](roles-concept-delegation.md).

## <a name="where-to-find-content"></a>İçerik nerede bulacağını

Microsoft 365 iş yükü | Obsah role | API içeriği
---------------------- | ------------------ | -----------------
Office 365 ve Microsoft 365 iş planları'te yönetici rolleri | [Office 365 yönetici rolleri](https://docs.microsoft.com/office365/admin/add-users/about-admin-roles?view=o365-worldwide) | Kullanılamaz
Azure Active Directory (Azure AD) ve Azure AD kimlik koruması| [Azure AD yönetim rolleri](directory-assign-admin-roles.md) | [Graph API'si](https://docs.microsoft.com/graph/api/overview?view=graph-rest-1.0)<br>[Rol atamaları getirilemedi](https://docs.microsoft.com/graph/api/directoryrole-list?view=graph-rest-1.0)
Exchange Online| [Exchange rol tabanlı erişim denetimi](https://docs.microsoft.com/exchange/understanding-role-based-access-control-exchange-2013-help) |  [Exchange için PowerShell](https://docs.microsoft.com/powershell/module/exchange/role-based-access-control/add-managementroleentry?view=exchange-ps)<br>[Rol atamaları getirilemedi](https://docs.microsoft.com/powershell/module/exchange/role-based-access-control/get-rolegroup?view=exchange-ps)
SharePoint Online | [Azure AD yönetim rolleri](directory-assign-admin-roles.md)<br>Ayrıca [Office 365'te hakkında SharePoint Yöneticisi rolü](https://docs.microsoft.com/sharepoint/sharepoint-admin-role) | [Graph API'si](https://docs.microsoft.com/graph/api/overview?view=graph-rest-1.0)<br>[Rol atamaları getirilemedi](https://docs.microsoft.com/graph/api/directoryrole-list?view=graph-rest-1.0)
Takımlar/Skype Kurumsal | [Azure AD yönetim rolleri](directory-assign-admin-roles.md) | [Graph API'si](https://docs.microsoft.com/graph/api/overview?view=graph-rest-1.0)<br>[Rol atamaları getirilemedi](https://docs.microsoft.com/graph/api/directoryrole-list?view=graph-rest-1.0)
Güvenlik ve uyumluluk Merkezi'nde (Office 365 Gelişmiş tehdit koruması, Exchange Online Protection, Information Protection) | [Office 365 yönetici rolleri](https://docs.microsoft.com/office365/SecurityCompliance/permissions-in-the-security-and-compliance-center) | [Exchange PowerShell](https://docs.microsoft.com/powershell/module/exchange/role-based-access-control/add-managementroleentry?view=exchange-ps)<br>[Rol atamaları getirilemedi](https://docs.microsoft.com/powershell/module/exchange/role-based-access-control/get-rolegroup?view=exchange-ps)
Güvenlik Puanı | [Azure AD yönetim rolleri](directory-assign-admin-roles.md) | [Graph API'si](https://docs.microsoft.com/graph/api/overview?view=graph-rest-1.0)<br>[Rol atamaları getirilemedi](https://docs.microsoft.com/graph/api/directoryrole-list?view=graph-rest-1.0)
Uyumluluk Yöneticisi | [Uyumluluk yöneticisi rolleri](https://docs.microsoft.com/office365/securitycompliance/meet-data-protection-and-regulatory-reqs-using-microsoft-cloud#permissions-and-role-based-access-control) | Kullanılamaz
Azure Information Protection | [Azure AD yönetim rolleri](directory-assign-admin-roles.md) | [Graph API'si](https://docs.microsoft.com/graph/api/overview?view=graph-rest-1.0)<br>[Rol atamaları getirilemedi](https://docs.microsoft.com/graph/api/directoryrole-list?view=graph-rest-1.0)
Microsoft Cloud App Security | [Rol tabanlı erişim denetimi](https://docs.microsoft.com/cloud-app-security/manage-admins) | [API başvurusu](https://docs.microsoft.com/cloud-app-security/api-tokens) 
Azure Gelişmiş Tehdit Koruması | [Azure ATP rol grupları](https://docs.microsoft.com/azure-advanced-threat-protection/atp-role-groups) | Kullanılamaz
Windows Defender Advanced Threat Protection | [Windows Defender ATP rol tabanlı erişim denetimi](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-atp/rbac-windows-defender-advanced-threat-protection) | Kullanılamaz
Privileged Identity Management | [Azure AD yönetim rolleri](directory-assign-admin-roles.md) | [Graph API'si](https://docs.microsoft.com/graph/api/overview?view=graph-rest-1.0)<br>[Rol atamaları getirilemedi](https://docs.microsoft.com/graph/api/directoryrole-list?view=graph-rest-1.0)
Intune | [Intune rol tabanlı erişim denetimi](https://docs.microsoft.com/intune/role-based-access-control) | [Graph API'si](https://docs.microsoft.com/graph/api/resources/intune-rbac-conceptual?view=graph-rest-beta)<br>[Rol atamaları getirilemedi](https://docs.microsoft.com/graph/api/intune-rbac-roledefinition-list?view=graph-rest-beta)
Yönetilen Masaüstü | [Azure AD yönetim rolleri](directory-assign-admin-roles.md) | [Graph API'si](https://docs.microsoft.com/graph/api/overview?view=graph-rest-1.0)<br>[Rol atamaları getirilemedi](https://docs.microsoft.com/graph/api/directoryrole-list?view=graph-rest-1.0)

## <a name="next-steps"></a>Sonraki adımlar

* [Atama veya Azure AD yönetici rollerini kaldırın](directory-manage-roles-portal.md)
* [Azure AD yönetici rollerini başvurusu](directory-assign-admin-roles.md)
