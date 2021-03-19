---
title: Grup üyeleri ekleme veya kaldırma-Azure Active Directory | Microsoft Docs
description: Azure Active Directory kullanarak bir gruba üye ekleme veya kaldırma hakkında yönergeler.
services: active-directory
author: ajburnle
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: how-to
ms.date: 08/23/2018
ms.author: ajburnle
ms.custom: it-pro, seodec18
ms.reviewer: krbain
ms.collection: M365-identity-device-management
ms.openlocfilehash: af5a85bad1e7b2a6bf645084d6b78f77e6c0d8b2
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "92371843"
---
# <a name="add-or-remove-group-members-using-azure-active-directory"></a>Azure Active Directory kullanarak Grup üyelerini ekleme veya kaldırma
Azure Active Directory kullanarak Grup üyelerini ekleme ve kaldırma işlemine devam edebilirsiniz.

## <a name="to-add-group-members"></a>Grup üyeleri eklemek için

1. Dizin için bir Genel yönetici hesabı kullanarak [Azure portalda](https://portal.azure.com) oturum açın.

2. **Azure Active Directory**' yi seçin ve ardından **gruplar**' ı seçin.

3. **Gruplar-tüm gruplar** sayfasında, üyeyi eklemek istediğiniz grubu arayıp seçin. Bu durumda, önceden oluşturulmuş grubumuzu ( **MDM ilkesi-Batı**) kullanın.

    ![Groups-All grupları sayfası, Grup adı vurgulanmış](media/active-directory-groups-members-azure-portal/group-all-groups-screen.png)

4. **MDM ilkesi - Batı Genel Bakışı** sayfasında, **Yönet** alanından **Üyeler** seçeneğini belirleyin.

    ![MDM ilkesi-Batı genel bakış sayfası, Üyeler seçeneği vurgulanmış](media/active-directory-groups-members-azure-portal/group-overview-blade.png)

5. **Üye Ekle**' yi seçin ve sonra gruba eklemek istediğiniz üyelerin her birini arayıp seçin ve ardından **Seç**' i seçin.

    Üyelerin başarıyla eklendiğini belirten bir ileti alırsınız.

    ![Gösterilen üye için aranan ile üye ekleme sayfası](media/active-directory-groups-members-azure-portal/update-members.png)

6. Gruba eklenen tüm üye adlarını görmek için ekranı yenileyin.

## <a name="to-remove-group-members"></a>Grup üyelerini kaldırmak için

1. **Gruplar-tüm gruplar** sayfasında, üyeyi kaldırmak istediğiniz grubu bulun ve seçin. **MDM ilkesi-Batı** kullanacağız.

2. **Yönet** alanından **Üyeler** ' i seçin, kaldırılacak üyenin adını arayıp seçin ve ardından **Kaldır**' ı seçin.

    ![Üye bilgileri sayfası, Kaldır seçeneği ile](media/active-directory-groups-members-azure-portal/remove-members-from-group.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Grupları ve üyeleri görüntüleme](active-directory-groups-view-azure-portal.md)

- [Grup ayarlarınızı düzenleme](active-directory-groups-settings-azure-portal.md)

- [Grupları kullanarak kaynaklara erişimi yönetme](active-directory-manage-groups.md)

- [Bir gruptaki kullanıcılar için dinamik kuralları yönetme](../enterprise-users/groups-create-rule.md)

- [Azure Active Directory bir Azure aboneliği ilişkilendirin veya ekleyin](active-directory-how-subscriptions-associated-directory.md)
