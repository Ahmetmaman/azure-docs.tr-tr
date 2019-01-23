---
title: Ekleme veya güncelleştirme kullanıcı profili bilgileri - Azure Active Directory | Microsoft Docs
description: Azure Active Directory'de bir resim ve iş ayrıntılar dahil olmak üzere, bir kullanıcının profilini bilgi ekleme hakkında yönergeler.
services: active-directory
author: eross-msft
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: conceptual
ms.date: 09/05/2018
ms.author: lizross
ms.reviewer: jeffsta
ms.openlocfilehash: df0f4fac471192c7e8745c07423857ea3c3d906b
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2019
ms.locfileid: "54450856"
---
# <a name="add-or-update-a-users-profile-information-using-azure-active-directory"></a>Ekleme veya Azure Active Directory'yi kullanarak kullanıcının profil bilgilerini güncelleştirme
Kullanıcı profili bilgilerini, profil resminizi dahil olmak üzere, işe özgü bilgileri ve Azure Active Directory (Azure AD) kullanarak bazı ayarları ekleyin. Yeni kullanıcı ekleme hakkında daha fazla bilgi için bkz. [ekleyin veya Azure Active Directory'de kullanıcı silme](add-users-azure-active-directory.md).

## <a name="add-or-change-profile-information"></a>Profil bilgileri ekleme veya değiştirme
Gördüğünüz gibi yok daha fazla bilgi bir kullanıcının profilinde daha kullanıcının oluşturma sırasında ekleyebilirsiniz. Bu ek bilgiler isteğe bağlıdır ve kuruluşunuz tarafından gereken şekilde eklenebilir.

## <a name="to-add-or-change-profile-information"></a>Ekleme veya profil bilgilerini değiştirme
1. Oturum [Azure portalında](https://portal.azure.com/) bir genel yönetici veya kullanıcı dizinin Yöneticisi olarak.

2. Seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından bir kullanıcı seçin. Örneğin, _Alain Charon_.

    **Alain Charon - profili** sayfası görüntülenir.

    ![Kullanıcının profil sayfasını düzenlenebilir bilgiler dahil olmak üzere,](media/active-directory-users-profile-azure-portal/user-profile-all-blade.png)

3. Seçin **Düzenle** isteğe bağlı olarak ekleyin ya da her bir kullanılabilir bölümler dahil bilgileri güncelleştirin.

    ![Kullanıcının profil sayfasını, düzenlenebilir alanları gösterme](media/active-directory-users-profile-azure-portal/user-profile-edit.png)

    - **Profil resmi.** Kullanıcı hesabı için bir küçük resim görüntüsünü seçin. Bu resim, Azure Active Directory ve myapps.microsoft.com sayfası gibi kullanıcının kişisel sayfalarında görünür.

    - **Kimlik.** Evli Soyadı veya değiştirilen kullanıcı adı gibi tüm hesabıyla ilgili bilgileri ekleyin. 

    - **İş bilgisi.** Kullanıcının iş unvanı, bölüme veya Yöneticisi gibi tüm iş ile ilgili bilgileri ekleyin.

    - **Ayarlar.** Kullanıcının Azure Active Directory kiracısı ile oturum olup olmadığını belirleyin. Ayrıca, kullanıcının genel konum belirtebilirsiniz.

    - **İletişim bilgileri.** Kullanıcı için tüm ilgili kişi bilgilerini ekleyin. Örneğin, bir posta adresi veya bir cep telefonu numarası.

    - **Kimlik doğrulaması iletişim bilgileri.** Bir kullanıcı için etkin bir telefon numarası ve e-posta adresi olduğundan emin olmak için bu bilgileri doğrulayın. Bu bilgiler, kullanıcının gerçekten oturum açma sırasında kullanıcı olduğundan emin olmak için Azure Active Directory tarafından kullanılır. Kimlik doğrulaması iletişim bilgileri, yalnızca bir genel yönetici tarafından güncelleştirilebilir.

4. **Kaydet**’i seçin.

    Kullanıcı için yaptığınız tüm değişiklikler kaydedilir.

    >[!Note]
    >Windows Server Active Directory, kimlik, iletişim bilgileri veya Windows Server Active Directory, yetki kaynağı olan kullanıcılar için iş bilgileri güncelleştirmek için kullanmanız gerekir. Güncelleştirme tamamlandıktan sonra değişiklikleri görürsünüz önce tamamlanması için bir sonraki eşitleme döngüsü beklemeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar
Kullanıcılarınızın profilleri güncelleştirdikten sonra aşağıdaki temel işlemleri gerçekleştirebilirsiniz:

- [Ekleme veya kullanıcıları Sil](add-users-azure-active-directory.md)

- [Kullanıcılara rol atama](active-directory-users-assign-role-azure-portal.md)

- [Temel bir grup oluşturma ve üye ekleme](active-directory-groups-create-azure-portal.md)

Veya temsilci atayarak, ilkeleri kullanarak ve kullanıcı hesapları paylaşma gibi diğer kullanıcı yönetim görevlerini gerçekleştirebilirsiniz. Diğer kullanılabilir eylemler hakkında daha fazla bilgi için bkz: [Azure Active Directory kullanıcı yönetimi belgeleri](../users-groups-roles/index.yml).
