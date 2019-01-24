---
title: 'Azure Active Directory etki alanı Hizmetleri: SharePoint kullanıcı profili hizmeti desteğini etkinleştirme | Microsoft Docs'
description: SharePoint Server için profil eşitleme desteklemek için Azure Active Directory Domain Services yönetilen etki alanlarını yapılandırma
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: daveba
editor: curtand
ms.assetid: 938a5fbc-2dd1-4759-bcce-628a6e19ab9d
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/22/2018
ms.author: ergreenl
ms.openlocfilehash: bb831c2b3cb9aee5146a667841a97771ff0ff85a
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54843540"
---
# <a name="configure-a-managed-domain-to-support-profile-synchronization-for-sharepoint-server"></a>SharePoint Server için profil eşitleme desteklemek için yönetilen bir etki alanı yapılandırma
SharePoint Server, kullanıcı profili eşitleme için kullanılan kullanıcı profili hizmeti içerir. Kullanıcı profili hizmeti'kurmak için uygun izinlere bir Active Directory etki alanına sahip olmanız gerekir. Daha fazla bilgi için [Active Directory Domain Services için SharePoint Server 2013'te profil eşitleme izinleri](https://technet.microsoft.com/library/hh296982.aspx).

Bu makalede, SharePoint Server kullanıcı profili eşitleme hizmeti dağıtmak için kullanılan Azure AD Domain Services yönetilen etki alanlarını nasıl yapılandırabileceğiniz açıklanmaktadır.

[!INCLUDE [active-directory-ds-prerequisites.md](../../includes/active-directory-ds-prerequisites.md)]

## <a name="the-aad-dc-service-accounts-group"></a>'AAD DC hizmeti hesapları' grubuna
Bir güvenlik grubu adı '**AAD DC hizmet hesapları**', yönetilen etki alanınızda 'Kullanıcılar' kuruluş birimi içerisinde kullanılabilir. Bu grupta gördüğünüz **Active Directory Kullanıcıları ve Bilgisayarları** MMC ek bileşeninde, yönetilen etki alanınızda.

![AAD DC hizmet hesapları güvenlik grubu](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts.png)

Bu güvenlik grubunun üyesi bir temsilci aşağıdaki ayrıcalıklara:
- Kök DSE yönetilen etki alanının 'Dizin değişikliklerini çoğaltma' ayrıcalığı.
- Yapılandırma adlandırma bağlamında 'Dizin değişikliklerini çoğaltma' ayrıcalığı (cn = yapılandırma kapsayıcısı), yönetilen etki alanı.

Bu güvenlik grubunu ayrıca yerleşik grubunun bir üyesi olan **Pre-Windows 2000 Compatible Access**.

![AAD DC hizmet hesapları güvenlik grubu](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts-properties.png)


## <a name="enable-your-managed-domain-to-support-sharepoint-server-user-profile-sync"></a>SharePoint Server kullanıcı profili eşitleme desteklemek yönetilen etki alanınıza etkinleştir
SharePoint kullanıcı profili eşitleme için kullanılan hizmet hesabı ekleyebilirsiniz **AAD DC hizmet hesapları** grubu. Sonuç olarak, eşitleme hesabı dizine değişiklikleri çoğaltmak için yeterli ayrıcalıkları alır. Bu yapılandırma adımı düzgün çalışması SharePoint Server kullanıcı profili eşitleme sağlar.

![AAD DC hizmet hesapları - üyeleri Ekle](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts-add-member.png)

![AAD DC hizmet hesapları - üyeleri Ekle](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts-add-member2.png)

## <a name="related-content"></a>İlgili İçerik
* [Teknik başvuru - SharePoint Server 2013'te profil eşitleme izinleri verme Active Directory etki alanı Hizmetleri](https://technet.microsoft.com/library/hh296982.aspx)
