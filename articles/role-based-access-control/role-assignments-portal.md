---
title: Azure portal-Azure RBAC kullanarak Azure rol atamaları ekleme veya kaldırma
description: Azure portal ve Azure rol tabanlı erişim denetimi (Azure RBAC) kullanarak kullanıcılar, gruplar, hizmet sorumluları veya yönetilen kimlikler için Azure kaynaklarına nasıl erişim sağlayacağınızı öğrenin.
services: active-directory
author: rolyon
manager: mtillman
ms.service: role-based-access-control
ms.topic: how-to
ms.workload: identity
ms.date: 09/30/2020
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 7c58641f0039982f05be14d0f24ba89c62273d4b
ms.sourcegitcommit: f6f928180504444470af713c32e7df667c17ac20
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/07/2021
ms.locfileid: "97964312"
---
# <a name="add-or-remove-azure-role-assignments-using-the-azure-portal"></a>Azure portalını kullanarak Azure rol ataması ekleme veya kaldırma

[!INCLUDE [Azure RBAC definition grant access](../../includes/role-based-access-control/definition-grant.md)] Bu makalede, Azure portal kullanarak rollerin nasıl atanacağı açıklanır.

Azure Active Directory ' de yönetici rolleri atamanız gerekiyorsa, bkz. [Azure Active Directory yönetici rollerini görüntüleme ve atama](../active-directory/roles/manage-roles-portal.md).

## <a name="prerequisites"></a>Ön koşullar

Rol atamaları eklemek veya kaldırmak için şunları yapmanız gerekir:

- `Microsoft.Authorization/roleAssignments/write`ve `Microsoft.Authorization/roleAssignments/delete` [Kullanıcı erişimi Yöneticisi](built-in-roles.md#user-access-administrator) veya [sahibi](built-in-roles.md#owner) gibi izinler

## <a name="access-control-iam"></a>Erişim denetimi (IAM)

**Erişim denetimi (IAM)** , genellikle Azure kaynaklarına erişim izni vermek üzere rol atamak için kullandığınız sayfasıdır. Kimlik ve erişim yönetimi olarak da bilinir ve Azure portal çeşitli konumlarda görüntülenir. Aşağıda, abonelik için erişim denetimi (ıAM) sayfasının bir örneği gösterilmektedir.

![Abonelik için erişim denetimi (ıAM) sayfası](./media/role-assignments-portal/access-control-subscription.png)

Erişim denetimi (ıAM) sayfası ile en etkili olması için, rol atamak üzere bu adımları izlemeye yardımcı olur.

1. Kimlerin erişime ihtiyacı olduğunu belirleme. Bir Kullanıcı, Grup, hizmet sorumlusu veya yönetilen kimliğe bir rol atayabilirsiniz.

1. Uygun rolü bulun. İzinler, rollerle birlikte gruplandırılır. Çeşitli [Azure yerleşik rollerinin](built-in-roles.md) listesinden seçim yapabilir veya kendi özel rollerinizi kullanabilirsiniz.

1. Gerekli kapsamı belirler. Azure dört kapsam düzeyi sağlar: [Yönetim grubu](../governance/management-groups/overview.md), abonelik, [kaynak grubu](../azure-resource-manager/management/overview.md#resource-groups)ve kaynak. Kapsam hakkında daha fazla bilgi için bkz. [kapsamı anlama](scope-overview.md).

1. Rol atamak için aşağıdaki bölümlerden birindeki adımları gerçekleştirin.

## <a name="add-a-role-assignment"></a>Rol ataması ekleyin

Azure RBAC 'de, bir Azure kaynağına erişim izni vermek için bir rol ataması eklersiniz. Rol atamak için aşağıdaki adımları izleyin.

1. Azure portal, **tüm hizmetler** ' e tıklayın ve ardından erişim vermek istediğiniz kapsamı seçin. Örneğin, **Yönetim grupları**, **abonelikler**, **kaynak grupları** veya bir kaynak seçebilirsiniz.

1. Bu kapsam için özel kaynağa tıklayın.

1. **Erişim denetimi (IAM)** öğesine tıklayın.

1. Bu kapsamdaki rol atamalarını görüntülemek için **rol atamaları** sekmesine tıklayın.

    ![Erişim denetimi (ıAM) ve rol atamaları sekmesi](./media/role-assignments-portal/role-assignments.png)

1.   >  **Rol Ekle ataması** Ekle ' ye tıklayın.

   Rol atama izniniz yoksa rol ataması Ekle seçeneği devre dışı bırakılır.

   ![Rol atama menüsü Ekle](./media/shared/add-role-assignment-menu.png)

    Rol ataması ekle bölmesi açılır.

   ![Rol atama bölmesi Ekle](./media/role-assignments-portal/add-role-assignment.png)

1. **Rol** açılan listesinde **Sanal Makine Katılımcısı** gibi bir rol seçin.

1. **Seç** listesinde, bir Kullanıcı, Grup, hizmet sorumlusu veya yönetilen kimlik seçin. Listede güvenlik sorumlusunu görmüyorsanız **Seç** kutusuna giriş yaparak dizinde görünen ad, e-posta adresi ve nesne tanımlayıcısı arayabilirsiniz.

1. Rolü atamak için **Kaydet**’e tıklayın.

   Birkaç dakika sonra, güvenlik sorumlusu seçili kapsamda role atanır.

    ![Kaydedilmiş rol ataması ekleme](./media/role-assignments-portal/add-role-assignment-save.png)

## <a name="assign-a-user-as-an-administrator-of-a-subscription"></a>Kullanıcıyı aboneliğin yöneticisi olarak atama

Bir kullanıcıyı bir Azure aboneliğinin Yöneticisi yapmak için, abonelik kapsamında bu rolü [sahip](built-in-roles.md#owner) rolüne atayın. Sahip rolü, kullanıcıya, başkalarına erişim verme izni dahil olmak üzere abonelikteki tüm kaynaklara tam erişim sağlar. Bu adımlar diğer rol atamasıyla aynıdır.

1. Azure portal, **tüm hizmetler** ve ardından **abonelikler**' e tıklayın.

1. Erişim izni vermek istediğiniz aboneliğe tıklayın.

1. **Erişim denetimi (IAM)** öğesine tıklayın.

1. Bu aboneliğin rol atamalarını görüntülemek için **rol atamaları** sekmesine tıklayın.

    ![Erişim denetimi (ıAM) ve rol atamaları sekmesi](./media/role-assignments-portal/role-assignments.png)

1.   >  **Rol Ekle ataması** Ekle ' ye tıklayın.

   Rol atama izniniz yoksa rol ataması Ekle seçeneği devre dışı bırakılır.

   ![Abonelik için rol atama menüsü ekleme](./media/shared/add-role-assignment-menu.png)

    Rol ataması ekle bölmesi açılır.

   ![Abonelik için rol atama bölmesi ekleme](./media/role-assignments-portal/add-role-assignment.png)

1. **Rol** açılan listesinde **Sahip** rolünü seçin.

1. **Seç** listesinde bir kullanıcı seçin. Listede kullanıcıyı görmüyorsanız **Seç** kutusuna giriş yaparak dizinde görünen ad ve e-posta adresi arayabilirsiniz.

1. Rolü atamak için **Kaydet**’e tıklayın.

   Birkaç dakika sonra kullanıcıya abonelik kapsamında Sahip rolü atanmış olur.

## <a name="add-a-role-assignment-for-a-managed-identity-preview"></a>Yönetilen kimlik (Önizleme) için rol ataması ekleme

Bu makalenin önceki kısımlarında açıklandığı gibi, **Access Control (IAM)** sayfasını kullanarak yönetilen bir kimlik için rol atamaları ekleyebilirsiniz. Erişim denetimi (ıAM) sayfasını kullandığınızda, kapsamla başlar ve ardından yönetilen kimliği ve rolü seçersiniz. Bu bölümde, yönetilen bir kimlik için rol atamaları eklemenin alternatif bir yolu açıklanmaktadır. Bu adımları kullanarak, yönetilen kimlikle başlayıp kapsamı ve rolü seçersiniz.

> [!IMPORTANT]
> Bu alternatif adımları kullanan yönetilen bir kimlik için rol ataması eklemek Şu anda önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

### <a name="system-assigned-managed-identity"></a>Sistem tarafından atanan yönetilen kimlik

Yönetilen kimlikle başlayarak, sistem tarafından atanan yönetilen kimliğe bir rol atamak için bu adımları izleyin.

1. Azure portal, sistem tarafından atanan bir yönetilen kimlik açın.

1. Sol taraftaki menüden **kimlik**' e tıklayın.

    ![Sistem tarafından atanan yönetilen kimlik](./media/shared/identity-system-assigned.png)

1. **İzinler** altında **Azure rol atamaları**' na tıklayın.

    Seçilen sistem tarafından atanan yönetilen kimliğe roller zaten atanmışsa, rol atamalarının listesini görürsünüz. Bu liste, okuma izninizin olduğu tüm rol atamalarını içerir.

    ![Sistem tarafından atanan yönetilen kimlik için rol atamaları](./media/shared/role-assignments-system-assigned.png)

1. Aboneliği değiştirmek için **abonelik** listesine tıklayın.

1. **Rol ataması Ekle (Önizleme)** seçeneğine tıklayın.

1. Rol atamasının **abonelik**, **kaynak grubu** veya kaynak gibi uyguladığı kaynak kümesini seçmek için açılan listeleri kullanın.

    Seçili kapsam için rol ataması yazma izinleriniz yoksa, bir satır içi ileti görüntülenir. 

1. **Rol** açılan listesinde **Sanal Makine Katılımcısı** gibi bir rol seçin.

   ![Sistem tarafından atanan yönetilen kimlik için rol atama bölmesi ekleme](./media/role-assignments-portal/add-role-assignment-with-scope.png)

1. Rolü atamak için **Kaydet**’e tıklayın.

   Birkaç dakika sonra, yönetilen kimliğe seçili kapsamda rol atanır.

### <a name="user-assigned-managed-identity"></a>Kullanıcı tarafından atanan yönetilen kimlik

Yönetilen kimlikle başlayarak Kullanıcı tarafından atanan yönetilen kimliğe bir rol atamak için bu adımları izleyin.

1. Azure portal, Kullanıcı tarafından atanan bir yönetilen kimlik açın.

1. Sol taraftaki menüden **Azure rol atamaları**' na tıklayın.

    Roller zaten seçili kullanıcı tarafından atanan yönetilen kimliğe atanmışsa, rol atamalarının listesini görürsünüz. Bu liste, okuma izninizin olduğu tüm rol atamalarını içerir.

    ![Kullanıcı tarafından atanan yönetilen kimlik için rol atamaları](./media/shared/role-assignments-user-assigned.png)

1. Aboneliği değiştirmek için **abonelik** listesine tıklayın.

1. **Rol ataması Ekle (Önizleme)** seçeneğine tıklayın.

1. Rol atamasının **abonelik**, **kaynak grubu** veya kaynak gibi uyguladığı kaynak kümesini seçmek için açılan listeleri kullanın.

    Seçili kapsam için rol ataması yazma izinleriniz yoksa, bir satır içi ileti görüntülenir. 

1. **Rol** açılan listesinde **Sanal Makine Katılımcısı** gibi bir rol seçin.

   ![Kullanıcı tarafından atanan yönetilen kimlik için rol atama bölmesi ekleme](./media/role-assignments-portal/add-role-assignment-with-scope.png)

1. Rolü atamak için **Kaydet**’e tıklayın.

   Birkaç dakika sonra, yönetilen kimliğe seçili kapsamda rol atanır.

## <a name="remove-a-role-assignment"></a>Rol atamasını kaldırma

Azure RBAC 'de, bir Azure kaynağından erişimi kaldırmak için bir rol atamasını kaldırırsınız. Rol atamasını kaldırmak için aşağıdaki adımları izleyin.

1. Erişim **denetimi 'ni (IAM)** , erişimi kaldırmak istediğiniz yönetim grubu, abonelik, kaynak grubu veya kaynak gibi bir kapsamda açın.

1. Bu aboneliğin tüm rol atamalarını görüntülemek için **Rol atamaları** sekmesine tıklayın.

1. Rol ataması listesinde kaldırmak istediğiniz rol atamasına sahip güvenlik sorumlusunun yanına onay işareti ekleyin.

   ![Kaldırılacak rol ataması seçildi](./media/role-assignments-portal/remove-role-assignment-select.png)

1. **Kaldır**’a tıklayın.

   ![Rol atamasını kaldırma iletisi](./media/role-assignments-portal/remove-role-assignment.png)

1. Görüntülenen rol atamasını Kaldır iletisinde **Evet**' e tıklayın.

    Devralınan rol atamalarının kaldırılamadığını belirten bir ileti görürseniz, bir alt kapsamda rol atamasını kaldırmaya çalışıyorsunuz. Rolün atandığı kapsamdaki erişim denetimini (ıAM) açmanız ve yeniden denemeniz gerekir. Doğru kapsamda erişim denetimini (ıAM) açmak için hızlı bir yol, **kapsam** sütununa bakmanız ve **(devralınmış)** yanındaki bağlantıya tıklamanız.

   ![Devralınan rol atamaları için rol atama iletisini kaldır](./media/role-assignments-portal/remove-role-assignment-inherited.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure portal kullanarak Azure rol atamalarını listeleyin](role-assignments-list-portal.md)
- [Öğretici: Azure portal kullanarak Azure kaynaklarına Kullanıcı erişimi verme](quickstart-assign-role-user-portal.md)
- [Azure RBAC sorunlarını giderme](troubleshooting.md)
- [Kaynaklarınızı Azure yönetim gruplarıyla düzenleme](../governance/management-groups/overview.md)
