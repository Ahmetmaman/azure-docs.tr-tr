---
title: 'Öğretici: Azure Active Directory ile otomatik Kullanıcı sağlaması için Elium yapılandırma | Microsoft Docs'
description: Kullanıcı hesaplarını otomatik olarak sağlamak ve devre dışı bırakmak için Azure Active Directory yapılandırma hakkında bilgi edinin.
services: active-directory
author: zchia
writer: zchia
manager: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 08/19/2019
ms.author: Zhchia
ms.openlocfilehash: e8f027ccc577df79e561fca7194c20b6cc7ef2c6
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "96005512"
---
# <a name="tutorial-configure-elium-for-automatic-user-provisioning"></a>Öğretici: otomatik Kullanıcı sağlaması için Elium yapılandırma

Bu öğreticide, Kullanıcı veya grupları Elium 'a otomatik olarak sağlamak ve devre dışı bırakmak için Elium ve Azure Active Directory (Azure AD) nasıl yapılandırılacağı gösterilmektedir.

> [!NOTE]
> Bu öğreticide, Azure AD Kullanıcı sağlama hizmeti 'nin üzerine kurulmuş bir bağlayıcı açıklanmaktadır. Bu hizmetin ne yaptığı ve nasıl çalıştığı hakkında önemli ayrıntılar ve sık sorulan sorular için bkz. [Azure Active Directory Ile SaaS uygulamalarına Kullanıcı sağlamayı ve sağlamayı kaldırmayı otomatikleştirme](../app-provisioning/user-provisioning.md).
>
> Bu bağlayıcı Şu anda önizleme aşamasındadır. Önizleme aşamasında Azure özelliklerinin genel kullanım koşulları için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide, aşağıdaki önkoşullara zaten sahip olduğunuz varsayılır:

* Bir Azure AD kiracısı
* [Elium kiracısı](https://www.elium.com/pricing/)
* Yönetici izinleriyle, bir kullanıcı hesabı

## <a name="assigning-users-to-elium"></a>Kullanıcıları Elium 'a atama

Azure AD, hangi kullanıcıların seçili uygulamalara erişim alacağını belirleyen *atamalar* adlı bir kavram kullanır. Otomatik Kullanıcı sağlama bağlamında, yalnızca Azure AD 'de bir uygulamaya atanmış olan kullanıcılar ve gruplar eşitlenir.

Otomatik Kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD 'deki hangi kullanıcıların ve grupların elim 'ye erişmesi gerektiğine karar verin. Daha sonra, [bir kullanıcı veya grubu bir kurumsal uygulamaya atama](../manage-apps/assign-user-or-group-access-portal.md)bölümündeki adımları izleyerek bu kullanıcıları ve grupları elime 'ye atayın.

## <a name="important-tips-for-assigning-users-to-elium"></a>Kullanıcıları Elium 'a atamaya yönelik önemli ipuçları 

Otomatik Kullanıcı sağlama yapılandırmasını test etmek için tek bir Azure AD kullanıcısını tam olarak atamanız önerilir. Daha sonra daha fazla Kullanıcı ve grup atanabilir.

Bir kullanıcıyı elim 'e atarken atama iletişim kutusunda geçerli, uygulamaya özgü bir rol (varsa) seçmelisiniz. **Varsayılan erişim** rolüne sahip kullanıcılar, sağlanmasından çıkarılır.

## <a name="set-up-elium-for-provisioning"></a>Sağlama için Elium ayarlama

Azure AD ile otomatik Kullanıcı sağlama için Elium 'u yapılandırmadan önce, sistemi etki alanları arası kimlik yönetimi (SCıM) sağlama için bir arada etkinleştirmeniz gerekir. Şu adımları izleyin:

1. Elim 'de oturum açın ve **profil**  >  **ayarlarıma** gidin.

    ![Elium 'da ayarlar menü öğesi](media/Elium-provisioning-tutorial/setting.png)

1. Sol alt köşede, **Gelişmiş**' ın altında **güvenlik**' i seçin.

    ![Elium 'da güvenlik bağlantısı](media/Elium-provisioning-tutorial/security.png)

1. **Kiracı URL 'si** ve **gizli belirteç** değerlerini kopyalayın. Bu değerleri daha sonra, Azure portal Elium uygulamanızın **sağlama** sekmesinde karşılık gelen alanlarda kullanacaksınız.

    ![Elium 'da kiracı URL 'SI ve gizli belirteç alanları](media/Elium-provisioning-tutorial/token.png)

## <a name="add-elium-from-the-gallery"></a>Galeriden Elium ekleyin

Azure AD ile otomatik Kullanıcı sağlaması için Elium 'u yapılandırmak için Azure AD Uygulama Galerisi 'nden yönetilen hizmet olarak yazılım (SaaS) uygulamaları listenize da Elium eklemeniz gerekir. Şu adımları izleyin:

1. [Azure Portal](https://portal.azure.com)sol gezinti panelinde **Azure Active Directory**' i seçin.

    ![Azure Active Directory menü öğesi](common/select-azuread.png)

1. **Kurumsal uygulamalar**' a gidin ve **tüm uygulamalar**' ı seçin.

     ![Azure AD kurumsal uygulamaları dikey penceresi](common/enterprise-applications.png)

1. Yeni bir uygulama eklemek için bölmenin üst kısmındaki **Yeni uygulama** ' yı seçin.

    ![Yeni uygulama bağlantısı](common/add-new-app.png)

1. Arama kutusuna **elium** yazın, sonuçlar listesinde **elium** ' ı seçin ve ardından uygulamayı eklemek için **Ekle** ' yi seçin.

    ![Galeri arama kutusu](common/search-new-app.png)

## <a name="configure-automatic-user-provisioning-to-elium"></a>Otomatik Kullanıcı sağlamayı Elium 'a yapılandırma

Bu bölümde, Azure AD sağlama hizmeti 'ni, Azure AD 'de Kullanıcı ve grup atamalarına bağlı olarak, Kullanıcı ve grupları, Kullanıcı ve grupları oluşturmak, güncelleştirmek ve devre dışı bırakmak üzere yapılandırma adımlarında size kılavuzluk eder.

> [!TIP]
> Ayrıca, [elium çoklu oturum açma öğreticisindeki](Elium-tutorial.md)yönergeleri izleyerek SECURITY ASSERTION MARKUP Language (SAML) tabanlı bir şekilde tek oturum açmayı etkinleştirmeyi de tercih edebilirsiniz. Otomatik Kullanıcı sağlamasından bağımsız olarak çoklu oturum açmayı yapılandırabilirsiniz, ancak iki özellik birbirini tamamlamada.

Azure AD 'de Elium için otomatik Kullanıcı sağlamayı yapılandırmak üzere aşağıdaki adımları izleyin:

1. [Azure Portal](https://portal.azure.com)oturum açın, **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.

    ![Azure AD kurumsal uygulamaları dikey penceresi](common/enterprise-applications.png)

1. Uygulamalar listesinde, **Elium**' u seçin.

    ![Kurumsal uygulamalar dikey penceresinde uygulamalar listesi](common/all-applications.png)

1. **Hazırlama** sekmesini seçin.

    ![Kurumsal uygulamalar dikey penceresinde sağlama sekmesi](common/provisioning.png)

1. **Hazırlama Modu**'nu **Otomatik** olarak ayarlayın.

    ![Sağlama modu için otomatik ayar](common/provisioning-automatic.png)

1. **Yönetici kimlik bilgileri** bölümünde, **kiracı URL 'si** alanına **\<tenantURL\> /Scim/v2** yazın. ( **Tenanturl** , daha önce elium Yönetici konsolundan alınan değerdir.) Ayrıca **gizli belirteç** alanına Elium **gizli belirteç** değerini yazın. Son olarak, Azure AD 'nin Elium 'a bağlanabildiğini doğrulamak için **Bağlantıyı Sına** ' yı seçin. Bağlantı başarısız olursa, Elium hesabınızın yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Yönetici kimlik bilgilerinde kiracı URL 'SI ve gizli belirteç alanları](common/provisioning-testconnection-tenanturltoken.png)

1. **Bildirim e-postası** alanına, sağlama hatası bildirimlerini alacak bir kişinin veya grubun e-posta adresini girin. Sonra, **bir hata oluştuğunda e-posta bildirimi gönder** onay kutusunu seçin.

    ![Bildirim E-postası](common/provisioning-notification-email.png)

1. **Kaydet**’e tıklayın.

1. **Eşlemeler** bölümünde **Azure Active Directory Kullanıcıları uyumum olarak eşitler**' ı seçin.

    ![Azure AD kullanıcılarını eşleme ile uyumlu hale getirmek için bağlantıyı eşitler](media/Elium-provisioning-tutorial/usermapping.png)

1. Azure AD 'den eşitlenen Kullanıcı özniteliklerini **öznitelik eşlemeleri** bölümünde elium 'a gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri Için elium 'daki Kullanıcı hesaplarını eşleştirmek için kullanılır. Değişiklikleri uygulamak için **Kaydet** ' i seçin.

    ![Azure AD ve Elium arasında öznitelik eşlemeleri](media/Elium-provisioning-tutorial/userattribute.png)

1. Kapsam filtrelerini yapılandırmak için [kapsam filtresi öğreticisindeki](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)yönergeleri izleyin.

1. Azure AD sağlama hizmetini Elium için etkinleştirmek üzere **Ayarlar** bölümünde **sağlama durumunu** **Açık** olarak değiştirin.

    ![Sağlama durumu açık olarak ayarlandı](common/provisioning-toggle-on.png)

1. **Ayarlar** bölümündeki **kapsam** açılan liste kutusunda istediğiniz değerleri seçerek, tam olarak sağlamak istediğiniz kullanıcıları ve grupları tanımlayın.

    ![Sağlama kapsamı liste kutusu](common/provisioning-scope.png)

1. Sağlamaya hazırsanız **Kaydet**' i seçin.

    ![Sağlama yapılandırması için Kaydet düğmesi](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **kapsamda** tanımlanan tüm Kullanıcı ve grupların ilk eşitlemesini başlatır. Bu ilk eşitleme işlemi daha sonra eşitlenenden daha uzun sürer. Sağlama için gereken süre hakkında daha fazla bilgi için bkz. [kullanıcıları sağlamak için ne kadar sürer?](../app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user.md#how-long-will-it-take-to-provision-users).

İlerlemeyi izlemek ve sağlama etkinliği raporunuzun bağlantılarını izlemek için **geçerli durum** bölümünü kullanın. Sağlama etkinliği raporu, Azure AD sağlama hizmeti tarafından Elium üzerinde gerçekleştirilen tüm eylemleri açıklar. Daha fazla bilgi için bkz. [Kullanıcı hazırlama durumunu denetleme](../app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user.md). Azure AD sağlama günlüklerini okumak için bkz. [Otomatik Kullanıcı hesabı sağlama hakkında raporlama](../app-provisioning/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal uygulamalar için Kullanıcı hesabı sağlamayı yönetme](../app-provisioning/configure-automatic-user-provisioning-portal.md).
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Hazırlama etkinliği günlüklerini incelemeyi ve rapor oluşturmayı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)
