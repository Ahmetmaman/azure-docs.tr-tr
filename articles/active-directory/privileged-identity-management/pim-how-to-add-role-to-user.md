---
title: PıM-Azure Active Directory 'de Azure AD rolleri atama | Microsoft Docs
description: Azure AD Privileged Identity Management (PıM) içinde Azure AD rolleri atamayı öğrenin.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: how-to
ms.workload: identity
ms.subservice: pim
ms.date: 02/16/2021
ms.author: curtand
ms.collection: M365-identity-device-management
ms.openlocfilehash: fd4374067fe0070c379a76ef5f59bb6aef5b29fc
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102123114"
---
# <a name="assign-azure-ad-roles-in-privileged-identity-management"></a>Privileged Identity Management Azure AD rolleri atama

Azure Active Directory (Azure AD) ile genel yönetici **kalıcı** Azure AD yönetici rolü atamaları yapabilir. Bu rol atamaları [Azure Portal](../roles/permissions-reference.md) veya [PowerShell komutları](/powershell/module/azuread#directory_roles)kullanılarak oluşturulabilir.

Azure AD Privileged Identity Management (PıM) hizmeti, ayrıcalıklı rol yöneticilerinin kalıcı yönetici rolü atamaları yapmasına de olanak tanır. Ayrıca, ayrıcalıklı rol yöneticileri, kullanıcıların Azure AD yönetici rollerine **uygun** olmasını sağlayabilir. Uygun bir yönetici, bu rolü gerektiğinde etkinleştirebilir ve sonra izinlerinin süresi bittiğinde sona erer.

## <a name="determine-your-version-of-pim"></a>PıM sürümünüzü belirleme

2019 Kasım 'Dan başlayarak Privileged Identity Management Azure AD rolleri bölümü, Azure Kaynak rolleri deneyimleriyle eşleşen yeni bir sürüme güncelleştiriliyor. Bu, ek özellikleri [ve var olan API üzerinde yapılan değişiklikleri](azure-ad-roles-features.md#api-changes)de oluşturur. Yeni sürüm kullanıma sunulurken, bu makalede izlediğiniz yordamlar Şu anda sahip olduğunuz Privileged Identity Management sürümüne bağlıdır. Hangi Privileged Identity Management sürümünü istediğinizi öğrenmek için bu bölümdeki adımları izleyin. Privileged Identity Management Sürümünüzü öğrendikten sonra bu makaledeki sürümle eşleşen yordamları seçebilirsiniz.

1. [Ayrıcalıklı rol yöneticisi](../roles/permissions-reference.md#privileged-role-administrator) rolünde olan bir kullanıcıyla [Azure Portal](https://portal.azure.com/) oturum açın.
1. **Azure AD Privileged Identity Management** açın. Genel Bakış sayfasının üst kısmında yer alan bir başlık varsa, bu makalenin **Yeni sürüm** sekmesinde yer alan yönergeleri izleyin. Aksi takdirde, **önceki sürüm** sekmesindeki yönergeleri izleyin.

  [![Azure AD > Privileged Identity Management seçin.](media/pim-how-to-add-role-to-user/pim-new-version.png)](media/pim-how-to-add-role-to-user/pim-new-version.png#lightbox)

# <a name="new-version"></a>[Yeni sürüm](#tab/new)

## <a name="assign-a-role"></a>Rol atama

Bir kullanıcıyı Azure AD yöneticisi rolüne uygun hale getirmek için bu adımları izleyin.

1. [Ayrıcalıklı rol yöneticisi](../roles/permissions-reference.md#privileged-role-administrator) rolünün üyesi olan bir kullanıcıyla [Azure Portal](https://portal.azure.com/) oturum açın.

    Privileged Identity Management yönetmek için başka bir yöneticiye erişim verme hakkında daha fazla bilgi için bkz. [Privileged Identity Management yönetmek için diğer yöneticilere erişim verme](pim-how-to-give-access-to-pim.md).

1. **Azure AD Privileged Identity Management** açın.

1. **Azure AD rolleri**' ni seçin.

1. Azure AD izinlerinin rollerinin listesini görmek için **Roller** ' i seçin.

    !["Atama Ekle" eylemi seçiliyken "Roller" sayfasının ekran görüntüsü.](./media/pim-how-to-add-role-to-user/roles-list.png)

1. **Atama Ekle sayfasını** **açmak için atamaları Ekle '** yi seçin.

1. **Rol Seç sayfasını açmak** Için **Rol Seç** ' i seçin.

    ![Yeni atama bölmesi](./media/pim-how-to-add-role-to-user/select-role.png)

1. Atamak istediğiniz bir rol seçin, role atamak istediğiniz üyeyi seçin ve ardından **İleri**' yi seçin.

1. **Üyelik ayarları** bölmesindeki **atama türü** listesinde **uygun** veya **etkin**' i seçin.

    - **Uygun** atamalar, rolü kullanmak için bir eylem gerçekleştirmek üzere rolün üyesini gerektirir. Eylemler, bir Multi-Factor Authentication (MFA) denetimi gerçekleştirmeye, iş gerekçesinin sağlanmasından veya belirlenen onaylayanlardan onay isteğinde bulunabilir.

    - **Etkin** atamalar, üyenin rolü kullanmak için herhangi bir eylem gerçekleştirmesini gerektirmez. Etkin olarak atanan üyelerin her zaman role atanan ayrıcalıkları vardır.

1. Belirli bir atama süresi belirtmek için bir başlangıç ve bitiş tarih ve saat kutuları ekleyin. Tamamlandığında, yeni rol atamasını oluşturmak için **ata** ' yı seçin.

    ![Üyelik ayarları-Tarih ve saat](./media/pim-how-to-add-role-to-user/start-and-end-dates.png)

1. Rol atandıktan sonra, bir atama durumu bildirimi görüntülenir.

    ![Yeni atama-bildirim](./media/pim-how-to-add-role-to-user/assignment-notification.png)

## <a name="assign-a-role-with-restricted-scope"></a>Kısıtlanmış kapsama sahip bir rol atama

Belirli roller için, verilen izinlerin kapsamı, tek bir yönetici birimi, hizmet sorumlusu veya uygulamayla kısıtlanabilir. Bu yordam bir yönetim birimi kapsamına sahip bir rol atarken bir örnektir. Yönetim birimi aracılığıyla kapsamı destekleyen rollerin listesi için bkz. [bir yönetim birimine kapsamlı roller atama](../roles/admin-units-assign-roles.md). Bu özellik şu anda Azure AD kuruluşları için kullanıma alınıyor.

1. [Azure Active Directory Yönetim merkezinde](https://aad.portal.azure.com) ayrıcalıklı rol yöneticisi izinleriyle oturum açın.

1. **Azure Active Directory**  >  **Roller ve yöneticiler '** i seçin.

1. **Kullanıcı Yöneticisi**' ni seçin.

    ![Portalda bir rol açtığınızda atama Ekle komutu kullanılabilir](./media/pim-how-to-add-role-to-user/add-assignment.png)

1. **Atama Ekle**' yi seçin.

    ![Bir rol kapsamı destekliyorsa, bir kapsam seçebilirsiniz](./media/pim-how-to-add-role-to-user/add-scope.png)

1. **Atama Ekle** sayfasında şunları yapabilirsiniz:

   - Role atanacak bir kullanıcı veya grup seçin
   - Rol kapsamını seçin (Bu durumda yönetim birimleri)
   - Kapsam için bir yönetim birimi seçin

Yönetim birimleri oluşturma hakkında daha fazla bilgi için bkz. [yönetim birimleri ekleme ve kaldırma](../roles/admin-units-manage.md).

## <a name="update-or-remove-an-existing-role-assignment"></a>Var olan bir rol atamasını güncelleştirme veya kaldırma

Varolan bir rol atamasını güncelleştirmek veya kaldırmak için bu adımları izleyin. **Yalnızca Azure AD P2 lisanslı müşteriler**: bir gruba hem Azure AD hem de PRIVILEGED IDENTITY Management (PIM) aracılığıyla bir rol için etkin olarak atamayın. Ayrıntılı bir açıklama için bkz. [bilinen sorunlar](../roles/groups-concept.md#known-issues).

1. **Azure AD Privileged Identity Management** açın.

1. **Azure AD rolleri**' ni seçin.

1. Azure AD rollerinin listesini görmek için **Roller** ' i seçin.

1. Güncelleştirmek veya kaldırmak istediğiniz rolü seçin.

1. **Uygun roller** veya **etkin roller** sekmelerinde rol atamasını bulun.

    ![Rol atamasını güncelleştirme veya kaldırma](./media/pim-how-to-add-role-to-user/remove-update-assignments.png)

1. Rol atamasını güncelleştirmek veya kaldırmak için **Güncelleştir** ' i veya **Kaldır** ' ı seçin.

# <a name="previous-version"></a>[Önceki sürüm](#tab/previous)

## <a name="make-a-user-eligible-for-a-role"></a>Bir kullanıcıyı bir rol için uygun hale getirme

Bir kullanıcıyı Azure AD yöneticisi rolüne uygun hale getirmek için bu adımları izleyin.

1. **Rolleri** veya **üyeleri** seçin.

    ![Azure AD rollerini açma](./media/pim-how-to-add-role-to-user/pim-directory-roles.png)

1. **Yönetilen üyeleri Ekle**' yi açmak Için **üye Ekle** ' yi seçin.

1. **Rol Seç**' i seçin, yönetmek istediğiniz rolü seçin ve ardından **Seç**' i seçin.

    ![Rol seçin](./media/pim-how-to-add-role-to-user/pim-select-a-role.png)

1. **Üyeleri Seç**' i seçin, role atamak istediğiniz kullanıcıları seçin ve ardından **Seç**' i seçin.

    ![Atanacak bir kullanıcı veya grup seçin](./media/pim-how-to-add-role-to-user/pim-select-members.png)

1. **Yönetilen Üyeler Ekle**' de, kullanıcıyı role eklemek için **Tamam** ' ı seçin.

1. Rol listesinde, üye listesini görmek için yeni atadığınız rolü seçin.

     Rol atandığında, seçtiğiniz Kullanıcı, rol için **uygun** olan Üyeler listesinde görünür.

    ![Kullanıcı bir rol için uygun](./media/pim-how-to-add-role-to-user/pim-directory-role-eligible.png)

1. Artık Kullanıcı role uygun olduğuna göre, [Privileged Identity Management ' de Azure AD rollerimi etkinleştirme](pim-how-to-activate-role.md)' deki yönergelere göre etkinleştirebileceklerini bilmesini sağlar.

    Uygun yöneticilerin etkinleştirme sırasında Azure AD Multi-Factor Authentication kaydolması istenir. Bir Kullanıcı MFA için kaydoya da bir Microsoft hesabı (gibi @outlook.com ) kullanıyorsa, bunların tüm rollerinde kalıcı hale getirmeniz gerekir.

## <a name="make-a-role-assignment-permanent"></a>Rol atamasını kalıcı hale getirme

Varsayılan olarak, yeni kullanıcılar yalnızca bir Azure AD yönetici rolü için *uygundur* . Rol atamasını kalıcı hale getirmek istiyorsanız bu adımları izleyin.

1. **Azure AD Privileged Identity Management** açın.

1. **Azure AD rolleri**' ni seçin.

1. **Üyeler**’i seçin.

    ![Üye listesi](./media/pim-how-to-add-role-to-user/pim-directory-role-list-members.png)

1. Kalıcı hale getirmek istediğiniz **uygun** bir rol seçin.

1. **Diğer** ' i seçin ve ardından **izin oluştur**' u seçin.

    ![Rol atamasını kalıcı yap](./media/pim-how-to-add-role-to-user/pim-make-perm.png)

    Rol artık **kalıcı** olarak listelendi.

    ![Kalıcı değişiklik içeren üyelerin listesi](./media/pim-how-to-add-role-to-user/pim-directory-role-list-members-permanent.png)

## <a name="remove-a-user-from-a-role"></a>Bir rolden Kullanıcı kaldırma

Kullanıcıları rol atamalarından kaldırabilirsiniz, ancak kalıcı bir genel yönetici olan en az bir kullanıcı olduğundan emin olabilirsiniz. Hangi kullanıcıların rol atamalarına ihtiyacı olduğundan emin değilseniz, [rol için bir erişim incelemesi başlatabilirsiniz](pim-how-to-start-security-review.md).

Belirli bir kullanıcıyı Azure AD yönetici rolünden kaldırmak için aşağıdaki adımları izleyin.

1. **Azure AD Privileged Identity Management** açın.

1. **Azure AD rolleri**' ni seçin.

1. **Üyeler**’i seçin.

    ![Üye listesi](./media/pim-how-to-add-role-to-user/pim-directory-role-list-members.png)

1. Kaldırmak istediğiniz bir rol ataması seçin.

1. **Diğer** ' i seçin ve ardından **Kaldır**' ı seçin.

    ![Rol kaldırma](./media/pim-how-to-add-role-to-user/pim-remove-role.png)

1. Onaylamanızı isteyen iletide **Evet**' i seçin.

    ![Kaldırmayı Onayla](./media/pim-how-to-add-role-to-user/pim-remove-role-confirm.png)

    Rol ataması kaldırılır.

## <a name="authorization-error-when-assigning-roles"></a>Rol atarken yetkilendirme hatası

Yakın zamanda bir abonelik için Privileged Identity Management etkinleştirdiyseniz ve bir kullanıcıyı Azure AD yöneticisi rolüne uygun hale getirmek istediğinizde bir yetkilendirme hatası alırsanız, bunun nedeni MS-PıM hizmet sorumlusunun uygun izinlere sahip olmaması olabilir. MS-PıM hizmet sorumlusu, diğer kullanıcılara roller atamak için [Kullanıcı erişimi Yöneticisi](../../role-based-access-control/built-in-roles.md#user-access-administrator) rolüne sahip olmalıdır. MS-PıM ' y i Kullanıcı erişimi Yöneticisi rolüne atanıncaya kadar beklemek yerine el ile atayabilirsiniz.

Kullanıcı erişimi yönetici rolünü bir abonelik için MS-PıM hizmet sorumlusuna atamak için aşağıdaki adımları izleyin.

1. Azure portal genel yönetici olarak oturum açın.

1. **Tüm hizmetler** ' i ve ardından **abonelikler**' i seçin.

1. Aboneliğinizi seçin.

1. **Erişim denetimi (IAM)** öğesini seçin.

1. Abonelik kapsamındaki rol atamalarının geçerli listesini görmek için **rol atamaları** ' nı seçin.

   ![Abonelik için erişim denetimi (ıAM) dikey penceresi](./media/pim-how-to-add-role-to-user/ms-pim-access-control.png)

1. **MS-PIM** hizmet sorumlusuna **Kullanıcı erişimi yönetici** rolü atanıp atanmadığını denetleyin.

1. Aksi takdirde, rol **ataması Ekle ' yi seçerek** rol ataması **Ekle** bölmesini açın.

1. **Rol** açılan listesinde **Kullanıcı erişimi yönetici** rolü ' nü seçin.

1. **Seç** listesinde, **MS-PIM** hizmet sorumlusu ' nı bulup seçin.

   ![Rol atama bölmesi ekleme-MS-PıM hizmet sorumlusu için izin ekleme](./media/pim-how-to-add-role-to-user/ms-pim-add-permissions.png)

1. Rolü atamak için **Kaydet** ' i seçin.

   Birkaç dakika sonra MS-PıM hizmet sorumlusu, abonelik kapsamında Kullanıcı erişimi Yöneticisi rolüne atanır.

   ![MS-PıM hizmet sorumlusu için Kullanıcı erişimi yönetici rolü atamasını gösteren erişim denetimi sayfası](./media/pim-how-to-add-role-to-user/ms-pim-user-access-administrator.png)

 ---

## <a name="next-steps"></a>Sonraki adımlar

- [Privileged Identity Management 'de Azure AD yönetici rolü ayarlarını yapılandırma](pim-how-to-change-default-settings.md)
- [Privileged Identity Management Azure Kaynak rolleri atama](pim-resource-roles-assign-roles.md)