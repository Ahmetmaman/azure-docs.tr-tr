---
title: 'Öğretici: Azure Active Directory ile otomatik Kullanıcı sağlaması için Velpıc yapılandırma | Microsoft Docs'
description: Kullanıcı hesaplarını Velpıc 'e otomatik olarak sağlamak ve yeniden sağlamak üzere Azure Active Directory nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: zhchia
writer: zhchia
manager: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 03/27/2019
ms.author: zhchia
ms.openlocfilehash: cdd4fb96a42d154ccd8b508950283978ddf58ef4
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "94354912"
---
# <a name="tutorial-configuring-velpic-for-automatic-user-provisioning"></a>Öğretici: otomatik Kullanıcı sağlaması için Velpıc yapılandırma

Bu öğreticinin amacı, Azure AD 'den Velpıc 'e Kullanıcı hesaplarını otomatik olarak sağlamak ve devre dışı bırakmak için Velpıc ve Azure AD 'de gerçekleştirmeniz gereken adımları gösteriyoruz.

> [!NOTE]
> Bu öğreticide, Azure AD Kullanıcı sağlama hizmeti ' nin üzerine oluşturulmuş bir bağlayıcı açıklanmaktadır. Hizmetin işlevleri ve çalışma şekli hakkında daha fazla bilgi edinmek ve sık sorulan soruları incelemek için bkz. [Azure Active Directory ile SaaS uygulamalarına kullanıcı hazırlama ve kaldırma işlemlerini otomatik hale getirme](../app-provisioning/user-provisioning.md).

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide anlatılan senaryoda aşağıdakilere sahip olduğunuz kabul edilmiştir:

* Azure Active Directory kiracısı
* [Kurumsal plan](https://www.velpic.com/pricing.html) veya daha iyi etkinleştirilmiş bir velpıc kiracısı
* Yönetici izinleriyle Velpıc 'de bir kullanıcı hesabı

## <a name="assigning-users-to-velpic"></a>Kullanıcıları Velpıc 'e atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişim alacağını belirleyebilmek için "atamalar" adlı bir kavram kullanır. Otomatik Kullanıcı hesabı sağlama bağlamında, yalnızca Azure AD 'de bir uygulamaya "atanmış" olan kullanıcılar ve gruplar eşitlenir. 

Sağlama hizmetini yapılandırmadan ve etkinleştirmeden önce, Azure AD 'deki hangi kullanıcı ve/veya grupların Velpıc uygulamanıza erişmesi gereken kullanıcıları temsil ettiğini belirlemeniz gerekir. Karar verdikten sonra buradaki yönergeleri izleyerek bu kullanıcıları Velpıc uygulamanıza atayabilirsiniz:

[Kurumsal uygulamaya Kullanıcı veya Grup atama](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-velpic"></a>Velpıc 'e Kullanıcı atamaya yönelik önemli ipuçları

* Sağlama yapılandırmasını test etmek için Velpıc 'e tek bir Azure AD kullanıcısının atanması önerilir. Ek kullanıcılar ve/veya grupları daha sonra atanabilir.

* Bir kullanıcıyı Velpıc 'e atarken, atama iletişim kutusunda **Kullanıcı** rolünü veya geçerli uygulamaya özgü bir rolü (varsa) seçmeniz gerekir. **Varsayılan erişim** rolünün sağlama için çalışmadığına ve bu kullanıcıların atlanacağını unutmayın.

## <a name="configuring-user-provisioning-to-velpic"></a>Velpıc 'e Kullanıcı hazırlama yapılandırma

Bu bölümde, Azure AD 'nizi Velpıc 'nin Kullanıcı hesabı sağlama API 'sine bağlama ve sağlama hizmeti 'ni, Azure AD 'de Kullanıcı ve grup atamasını temel alan Velpıc 'de atanan kullanıcı hesaplarını oluşturmak, güncelleştirmek ve devre dışı bırakmak için yapılandırma işlemi kılavuzluk eder.

> [!TIP]
> Ayrıca, [Azure Portal](https://portal.azure.com)sağlanan yönergeleri Izleyerek VELPıC için SAML tabanlı tek Sign-On de seçebilirsiniz. Çoklu oturum açma özelliği otomatik sağlanmadan bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini karmaşıdirebilirler.

### <a name="to-configure-automatic-user-account-provisioning-to-velpic-in-azure-ad"></a>Azure AD 'de Velpıc 'e otomatik Kullanıcı hesabı sağlamayı yapılandırmak için:

1. [Azure portal](https://portal.azure.com) **Azure Active Directory > Enterprise Apps > tüm uygulamalar** bölümüne gidin.

2. Zaten çoklu oturum açma için Velpıc yapılandırdıysanız arama alanını kullanarak Velpıc örneğinizi arayın. Aksi takdirde, **Ekle** ' yi seçin ve uygulama galerisinde **velpic** için arama yapın. Arama sonuçlarından Velpic ' ı seçin ve uygulama listenize ekleyin.

3. Velpic örneğinizi seçin, sonra **sağlama** sekmesini seçin.

4. **Hazırlama Modu**'nu **Otomatik** olarak ayarlayın.

    ![Velpıc sağlama](./media/velpic-provisioning-tutorial/Velpic1.png)

5. **Yönetici kimlik bilgileri** bölümünün altında, bir Velpıc 'Nin **gizli belirteç&kiracı URL 'sini** girin. (Bu değerleri, Velpıc hesabınız altında bulabilirsiniz: **Yönet**  >  **Tümleştirme**  >  **Eklenti**  >  **SCIM**)

    ![Yetkilendirme değerleri](./media/velpic-provisioning-tutorial/Velpic2.png)

6. Azure portal, Azure AD 'nin Velpıc uygulamanıza bağlanabildiğinden emin olmak için **Bağlantıyı Sına** ' ya tıklayın. Bağlantı başarısız olursa, Velpıc hesabınızın yönetici izinlerine sahip olduğundan emin olun ve 5. adımı yeniden deneyin.

7. **Bildirim e-postası** alanında sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve aşağıdaki onay kutusunu işaretleyin.

8. **Kaydet**’e tıklayın.

9. Eşlemeler bölümünde **Azure Active Directory Kullanıcıları Velpıc olarak eşitler**' ı seçin.

10. **Öznitelik eşlemeleri** bölümünde, Azure AD 'Den velpıc 'e eşitlenecek Kullanıcı özniteliklerini gözden geçirin. Güncelleştirme işlemleri için, **eşleşen** özellikler olarak seçilen özniteliklerin, velpıc içindeki kullanıcı hesaplarıyla eşleştirmek için kullanılacağını unutmayın. Değişiklikleri uygulamak için Kaydet düğmesini seçin.

11. Velpıc için Azure AD sağlama hizmetini etkinleştirmek üzere **Ayarlar** bölümünde **sağlama durumunu** **Açık** olarak değiştirin

12. **Kaydet**’e tıklayın.

Bu, kullanıcılar ve Gruplar bölümünde Velpıc 'e atanan tüm Kullanıcı ve/veya grupların ilk eşitlemesini başlatır. İlk eşitlemenin daha sonra, hizmetin çalıştığı sürece yaklaşık 40 dakikada bir gerçekleşen sonraki eşitlemeler yerine daha uzun süreceğini unutmayın. İlerleme durumunu izlemek ve sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklayan etkinlik raporlarını sağlamak için bağlantıları izlemek üzere **eşitleme ayrıntıları** bölümünü kullanabilirsiniz.

Azure AD sağlama günlüklerinin nasıl okunduğu hakkında daha fazla bilgi için bkz. [Otomatik Kullanıcı hesabı sağlamayı raporlama](../app-provisioning/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı hazırlamayı yönetme](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Hazırlama etkinliği günlüklerini incelemeyi ve rapor oluşturmayı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)