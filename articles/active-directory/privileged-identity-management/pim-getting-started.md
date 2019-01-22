---
title: PIM - Azure'ı kullanmaya başlayın | Microsoft Docs
description: Etkinleştirme ve Azure portalında Azure AD Privileged Identity Management (PIM) kullanmaya başlama hakkında bilgi edinin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: pim
ms.topic: conceptual
ms.workload: identity
ms.date: 11/09/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 7d97f68c04656c1f8e227069bb3d33aba13d4b53
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2019
ms.locfileid: "54435082"
---
# <a name="start-using-pim"></a>PIM kullanmaya başlama

Azure Active Directory (Azure AD) Privileged Identity Management (PIM) ile yönetebilir, denetleyebilir ve kuruluşunuzda erişim izleyin. Azure kaynaklarına, Azure AD’ye ve Office 365 ya da Microsoft Intune gibi diğer Microsoft çevrimiçi hizmetlerine erişim de bu kapsama dahildir.

Bu makalede, PIM ile çalışmaya başlamak nasıl etkinleştirileceği açıklanır.

## <a name="prerequisites"></a>Önkoşullar

PIM'i kullanmak için aşağıdaki lisanslardan birine sahip olmalıdır:

- Azure AD Premium P2
- Enterprise Mobility + Security (EMS) E5

Daha fazla bilgi için [lisans PIM kullanmak için gereksinimler](subscription-requirements.md).

## <a name="first-person-to-use-pim"></a>İlk kişinin PIM'i kullanma

PIM'i kullanmak dizininizde ilk kişi siz, otomatik olarak size atanır [Güvenlik Yöneticisi](../users-groups-roles/directory-assign-admin-roles.md#security-administrator) ve [ayrıcalıklı Rol Yöneticisi](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator) dizindeki roller. Kullanıcıların Azure AD dizini rol atamaları, yalnızca ayrıcalıklı rol yöneticileri tarafından yönetilebilir. Ayrıca, çalıştırmayı da seçebilirsiniz [Güvenlik Sihirbazı](pim-security-wizard.md) , ilk Keşif ve atama deneyiminizde size yol gösterir.

## <a name="enable-pim"></a>PIM'i etkinleştirin

PIM dizininizde kullanmaya başlamak için öncelikle PIM etkinleştirmeniz gerekir.

1. Oturum [Azure portalında](https://portal.azure.com/) dizininizin genel Yöneticisi olarak.

    Bir kurumsal hesap ile bir genel yönetici olması gerekir (örneğin, @yourdomain.com), bir Microsoft hesabı (örneğin, @outlook.com) için bir dizin PIM etkinleştirmek için.

1. Tıklayın **tüm hizmetleri** ve bulma **Azure AD Privileged Identity Management** hizmeti.

    ![Azure AD Privileged Identity Management içindeki tüm hizmetleri](./media/pim-getting-started/pim-all-services-find.png)

1. PIM Hızlı Başlangıç'ı açmak için tıklayın.

1. Listesinde **PIM onayı**.

    ![PIM'i onayla](./media/pim-getting-started/consent-pim.png)

1. Tıklayın **Kimliğimi doğrula** Azure MFA ile kimliğinizi doğrulayın. Bir hesap seçin istenir.

    ![Bir hesap seçin](./media/pim-getting-started/pick-account.png)

1. Daha fazla bilgi için doğrulama gerekliyse sürecinde destekli. Daha fazla bilgi için [iki aşamalı doğrulama konusunda Yardım Al](https://go.microsoft.com/fwlink/p/?LinkId=708614).

    ![Daha fazla bilgi gerekli](./media/pim-getting-started/more-information-required.png)

    Örneğin, telefon doğrulama sağlamanız istenebilir.

    ![Ek güvenlik doğrulaması](./media/pim-getting-started/additional-security-verification.png)

1. Doğrulama işlemi tamamladıktan sonra tıklayın **onay** düğmesi.

1. Görüntülenen iletisinde tıklayın **Evet** PIM hizmetine onay vermek için.

    ![PIM ileti onayı](./media/pim-getting-started/consent-pim-message.png)

## <a name="sign-up-pim-for-azure-ad-roles"></a>Azure AD rolleri için PIM oturum

PIM dizininiz için etkinleştirdikten sonra Azure AD rolleri yönetmek için PIM oturum açmanız gerekir.

1. Açık **Azure AD Privileged Identity Management**.

1. Tıklayın **Azure AD rolleri**.

    ![Azure AD rolleri için PIM oturum](./media/pim-getting-started/sign-up-pim-azure-ad-roles.png)

1. Tıklayın **kaydolun**.

1. Görüntülenen iletisinde tıklayın **Evet** PIM, Azure AD rolleri yönetmek için oturum açmak için.

    ![PIM ' için Azure AD rolleri ileti oturum](./media/pim-getting-started/sign-up-pim-message.png)

    Kayıt tamamlandığında, Azure AD seçenekleri etkinleştirilecektir. Portalı yenilemeniz gerekebilir.

    PIM ile korumak için Azure kaynaklarını seçin ve bulma hakkında daha fazla bilgi için bkz: [PIM'de yönetmek için keşfetmek Azure kaynaklarını](pim-resource-roles-discover-resources.md).

## <a name="navigate-to-your-tasks"></a>Görevlerinize gitme

PIM kurulumundan sonra kimlik yönetimi görevlerini gerçekleştirebilir.

![PIM için üst düzey görevler - ekran görüntüsü](./media/pim-getting-started/pim-quickstart-tasks.png)

| Görev + yönetme | Açıklama |
| --- | --- |
| **Rollerim**  | Size atanan uygun ve etkin rollerin bir listesini görüntüler. Burada atanan uygun rolleri etkinleştirebilirsiniz. |
| **İsteklerim** | Uygun rol atamalarını etkinleştirmek için Bekleyen isteklerinizi görüntüler. |
| **Uygulama erişimi** | Olası gecikmelerini azaltmak ve hemen etkinleştirildikten sonra rol kullanmanıza olanak sağlar. |
| **İstekleri onaylama** | Uygun rolleri dizininizdeki kullanıcılar tarafından etkinleştirme isteklerini listesini görüntüler onaylanacak atanmış. |
| **Erişim gözden geçirin** | Erişimi kendiniz veya başka bir kullanıcı için inceliyor olmanızdan bağımsız tamamlamak için atanan beklemedeki erişim gözden geçirmelerini listeler. |
| **Azure AD rolleri** | Bir Pano ve Azure AD dizini rol atamalarını yönetmek ayrıcalıklı rol yöneticileri ayarlarını görüntüler. Bu pano, ayrıcalıklı rol yöneticisi olmayan kullanıcılar için devre dışıdır. Bu kullanıcılar Görünümüm adlı özel bir panoya erişebilir. Görünümüm panosu, tüm kiracı değil yalnızca panoya erişen kullanıcı hakkında bilgileri görüntüler. |
| **Azure kaynakları** | Bir Pano ve Azure kaynak rol atamalarını yönetmek ayrıcalıklı rol yöneticileri ayarlarını görüntüler. Bu pano, ayrıcalıklı rol yöneticisi olmayan kullanıcılar için devre dışıdır. Bu kullanıcılar Görünümüm adlı özel bir panoya erişebilir. Görünümüm panosu, tüm kiracı değil yalnızca panoya erişen kullanıcı hakkında bilgileri görüntüler. |

## <a name="add-a-pim-tile-to-the-dashboard"></a>PIM kutucuğu panoya ekleme

PIM açma daha kolay hale getirmek için PIM kutucuğu, Azure portalı panonuza eklemeniz gerekir.

1. [Azure Portal](https://portal.azure.com/) oturum açın.

1. Tıklayın **tüm hizmetleri** ve bulma **Azure AD Privileged Identity Management** hizmeti.

    ![Azure AD Privileged Identity Management içindeki tüm hizmetleri](./media/pim-getting-started/pim-all-services-find.png)

1. PIM Hızlı Başlangıç'ı açmak için tıklayın.

1. Denetleme **dikey pencereyi panoya Sabitle** PIM hızlı başlangıç dikey pencereyi panoya sabitleyin.

    ![Dikey pencereyi panoya sabitle](./media/pim-getting-started/pim-quickstart-pin-to-dashboard.png)

    Azure panosunda, böyle bir kutucuk görürsünüz:

    ![PIM hızlı kutucuğu](./media/pim-getting-started/pim-quickstart-dashboard-tile.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD dizin rollerini PIM atayın](pim-how-to-add-role-to-user.md)
- [PIM'de yönetmek için Azure kaynaklarını keşfedin](pim-resource-roles-discover-resources.md)
