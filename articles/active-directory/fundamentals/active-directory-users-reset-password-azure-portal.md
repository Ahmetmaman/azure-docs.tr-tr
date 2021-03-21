---
title: Kullanıcının parolasını sıfırlama-Azure Active Directory | Microsoft Docs
description: Azure Active Directory kullanarak bir kullanıcının parolasını sıfırlama yönergeleri.
services: active-directory
author: ajburnle
manager: daveba
ms.assetid: fad5624b-2f13-4abc-b3d4-b347903a8f16
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: how-to
ms.date: 09/05/2018
ms.author: ajburnle
ms.reviewer: jeffsta
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8809f8c168e7095f05587c7a572e08287637dc5a
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102034600"
---
# <a name="reset-a-users-password-using-azure-active-directory"></a>Azure Active Directory kullanarak kullanıcının parolasını sıfırlama

Yönetici olarak, parola unutursa Kullanıcı bir cihazdan kilitlenirse veya Kullanıcı hiç parola almamışsa bir kullanıcının parolasını sıfırlayabilirsiniz.

>[!Note]
>Azure AD kiracınız bir kullanıcının giriş dizini değilse, parolasını sıfırlayamayacağız. Yani, kullanıcılarınız kuruluşunuzda başka bir kuruluştan, Microsoft hesabı veya Google hesabından bir hesap kullanarak oturum açıyorsanız, parolasını sıfırlayamayacağız.<br><br>Kullanıcı Windows Server Active Directory bir yetkili kaynağına sahipse, parolayı yalnızca parola geri yazma özelliğini etkinleştirdiyseniz sıfırlayabilirsiniz.<br><br>Kullanıcının dış Azure AD olarak bir yetkili kaynağı varsa, parolayı sıfırlayamayacağız. Yalnızca dış Azure AD 'deki Kullanıcı veya yönetici parolayı sıfırlayabilir.

>[!Note]
>Yönetici değilseniz ve bunun yerine kendi iş veya okul parolanızı sıfırlama hakkında yönergeler arıyorsanız, bkz. [iş veya okul parolanızı sıfırlama](../user-help/active-directory-passwords-update-your-own-password.md).

## <a name="to-reset-a-password"></a>Parolayı sıfırlamak için

1. [Azure Portal](https://portal.azure.com/) Kullanıcı Yöneticisi veya parola Yöneticisi olarak oturum açın. Kullanılabilir roller hakkında daha fazla bilgi için bkz. [Azure AD yerleşik rolleri](../roles/permissions-reference.md)

2. **Azure Active Directory** seçin, **Kullanıcılar**' ı seçin, sıfırlanması gereken kullanıcıyı arayıp seçin ve **Parolayı Sıfırla**' yı seçin.

    **Alain Charon profili** sayfası, **Parolayı Sıfırla** seçeneğiyle birlikte görüntülenir.

    ![Parolayı Sıfırla seçeneği vurgulanmış şekilde kullanıcının profil sayfası](media/active-directory-users-reset-password-azure-portal/user-profile-reset-password-link.png)

3. **Parolayı Sıfırla** sayfasında, **Parolayı Sıfırla**' yı seçin.

    > [!Note]
    > Azure Active Directory kullanırken, Kullanıcı için geçici bir parola otomatik olarak oluşturulur. Şirket içi Active Directory kullanırken, kullanıcının parolasını oluşturursunuz.

4. Parolayı kopyalayın ve kullanıcıya verin. Kullanıcının bir sonraki oturum açma işlemi sırasında parolayı değiştirmesi gerekecektir.

    >[!Note]
    >Geçici parolanın süresi hiçbir zaman dolmaz. Kullanıcı bir sonraki sefer oturum açtığında, geçici parolanın oluşturulmasından bu yana geçen süre ne olursa olsun parola çalışmaya devam edecektir.

> [!IMPORTANT]
> Bir yönetici kullanıcının parolasını sıfırlayamaz ve Azure AD Connect sunucusundaki uygulama olay günlüklerinde, HR = 80231367 aşağıdaki hata kodu görülür. kullanıcının özniteliklerini Active Directory inceleyin.  **AdminCount** özniteliği 1 olarak ayarlandıysa, bu, yöneticinin kullanıcının parolasını sıfırlamasını engeller.  Yönetici Kullanıcı parolasını sıfırlayabilmesi için **adminCount** özniteliğinin 0 olarak ayarlanması gerekir.


## <a name="next-steps"></a>Sonraki adımlar

Kullanıcı parolasını sıfırladıktan sonra, aşağıdaki temel işlemleri gerçekleştirebilirsiniz:

- [Kullanıcı ekleme veya silme](add-users-azure-active-directory.md)

- [Kullanıcılara rol atama](active-directory-users-assign-role-azure-portal.md)

- [Profil bilgilerini ekleme veya değiştirme](active-directory-users-profile-azure-portal.md)

- [Temel bir grup oluşturma ve üye ekleme](active-directory-groups-create-azure-portal.md)

Ya da temsilciler atama, ilkeleri kullanma ve Kullanıcı hesaplarını paylaşma gibi daha karmaşık kullanıcı senaryoları da gerçekleştirebilirsiniz. Diğer kullanılabilir eylemler hakkında daha fazla bilgi için bkz. [Kullanıcı yönetimi belgelerini Azure Active Directory](../enterprise-users/index.yml).
