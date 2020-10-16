---
title: "Öğretici: Azure Active Directory ile otomatik Kullanıcı sağlaması için Templamy 'ı yapılandırma | Microsoft Docs"
description: Kullanıcı hesaplarını Templamy 'a otomatik olarak sağlamak ve sağlamak için Azure Active Directory yapılandırmayı öğrenin.
services: active-directory
author: zchia
writer: zchia
manager: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: article
ms.date: 07/26/2019
ms.author: zhchia
ms.openlocfilehash: 98eae8b63b9eaaa8cff800bade091f4b5d36d005
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91255722"
---
# <a name="tutorial-configure-templafy-for-automatic-user-provisioning"></a>Öğretici: otomatik Kullanıcı sağlaması için Templamy 'ı yapılandırma

Bu öğreticinin amacı, Azure AD 'yi, Kullanıcı ve/veya grupları Templamy 'a otomatik olarak sağlamak ve devre dışı bırakmak üzere yapılandırmak için Templamy ve Azure Active Directory (Azure AD) içinde gerçekleştirilecek adımları göstermektir.

> [!NOTE]
> Bu öğreticide, Azure AD Kullanıcı sağlama hizmeti ' nin üzerine oluşturulmuş bir bağlayıcı açıklanmaktadır. Hizmetin işlevleri ve çalışma şekli hakkında daha fazla bilgi edinmek ve sık sorulan soruları incelemek için bkz. [Azure Active Directory ile SaaS uygulamalarına kullanıcı hazırlama ve kaldırma işlemlerini otomatik hale getirme](../app-provisioning/user-provisioning.md).
>
> Bu bağlayıcı Şu anda genel önizleme aşamasındadır. Önizleme özellikleri için genel Microsoft Azure kullanım koşulları hakkında daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki önkoşulların zaten olduğunu varsayar:

* Azure AD kiracısı.
* [Bir Templamy kiracısı](https://www.templafy.com/pricing/).
* Yönetici izinleriyle Templamy 'daki bir kullanıcı hesabı.

## <a name="assigning-users-to-templafy"></a>Kullanıcıları Templamy 'a atama

Azure Active Directory seçili uygulamalara hangi kullanıcıların erişimi alacağını belirleyen *atama* adı verilen bir kavram kullanır. Otomatik Kullanıcı sağlama bağlamında, yalnızca Azure AD 'de bir uygulamaya atanmış olan kullanıcılar ve/veya gruplar eşitlenir.

Otomatik Kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD 'deki hangi kullanıcıların ve/veya grupların Templamy 'a erişmesi gerektiğine karar vermeniz gerekir. Karar verdikten sonra buradaki yönergeleri izleyerek bu kullanıcıları ve/veya grupları Templamy öğesine atayabilirsiniz:
* [Kurumsal uygulamaya Kullanıcı veya Grup atama](../manage-apps/assign-user-or-group-access-portal.md)

## <a name="important-tips-for-assigning-users-to-templafy"></a>Kullanıcıları Templamy 'a atamaya yönelik önemli ipuçları

* Otomatik Kullanıcı sağlama yapılandırmasını test etmek için Templamy 'e tek bir Azure AD kullanıcısı atanması önerilir. Ek kullanıcılar ve/veya grupları daha sonra atanabilir.

* Bir kullanıcıyı Templamy 'a atarken atama iletişim kutusunda uygulamaya özgü geçerli herhangi bir rolü (varsa) seçmeniz gerekir. **Varsayılan erişim** rolüne sahip kullanıcılar, sağlanmasından çıkarılır.

## <a name="setup-templafy-for-provisioning"></a>Sağlama için Templamy kurulumu

Azure AD ile otomatik Kullanıcı sağlaması için Templamy 'ı yapılandırmadan önce, Templamy üzerinde SCıM sağlamasını etkinleştirmeniz gerekir.

1. Templamy yönetici konsolunda oturum açın. **Yönetim**' e tıklayın.

    ![Templamy Yönetici Konsolu](media/templafy-provisioning-tutorial/image00.png)

2. **Kimlik doğrulama yöntemi**' ne tıklayın.

    ![Kimlik doğrulama yöntemi seçeneği olarak adlandırılan Templamy yönetim bölümünün ekran görüntüsü.](media/templafy-provisioning-tutorial/image01.png)

3. **SCIM API anahtarı** değerini kopyalayın. Bu değer, Azure portal Templamy uygulamanızın sağlama sekmesindeki **gizli belirteç** alanına girilir.

    ![S C M A P ı anahtar ekran görüntüsü.](media/templafy-provisioning-tutorial/image02.png)

## <a name="add-templafy-from-the-gallery"></a>Galeriden Templamy ekleme

Azure AD ile otomatik Kullanıcı sağlaması için Templamy 'ı yapılandırmak için, Azure AD Uygulama Galerisi 'nden yönetilen SaaS uygulamaları listenize Templamy öğesini eklemeniz gerekir.

**Azure AD Uygulama Galerisi 'nden Templamy eklemek için aşağıdaki adımları uygulayın:**

1. **[Azure Portal](https://portal.azure.com)** sol gezinti panelinde **Azure Active Directory**' i seçin.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. **Kurumsal uygulamalar**' a gidin ve **tüm uygulamalar**' ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için bölmenin üst kısmındaki **Yeni uygulama** düğmesini seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **templamy**yazın, sonuçlar panelinde **templamy** ' ı seçin ve sonra uygulamayı eklemek için **Ekle** düğmesine tıklayın.

    ![Sonuçlar listesinde templamy](common/search-new-app.png)

## <a name="configuring-automatic-user-provisioning-to-templafy"></a>Templamy otomatik Kullanıcı sağlamasını yapılandırma 

Bu bölümde, Azure AD sağlama hizmeti 'ni, Azure AD 'de Kullanıcı ve/veya grup atamalarını temel alan Templamy 'te kullanıcıları ve/veya grupları oluşturmak, güncelleştirmek ve devre dışı bırakmak üzere yapılandırma adımlarında size kılavuzluk eder.

> [!TIP]
> Templamy [Çoklu oturum açma öğreticisinde](templafy-tutorial.md)belirtilen talimatları Izleyerek templamy için SAML tabanlı çoklu oturum açmayı etkinleştirmeyi de tercih edebilirsiniz. Çoklu oturum açma, otomatik Kullanıcı sağlamasından bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini karmaşıdirebilirler.

### <a name="to-configure-automatic-user-provisioning-for-templafy-in-azure-ad"></a>Azure AD 'de Templamy otomatik Kullanıcı sağlamasını yapılandırmak için:

1. [Azure Portal](https://portal.azure.com) oturum açın. **Kurumsal Uygulamalar**'ı ve ardından **Tüm uygulamalar**'ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Templamy**' ı seçin.

    ![Uygulamalar listesindeki Templamy bağlantısı](common/all-applications.png)

3. **Hazırlama** sekmesini seçin.

    ![Sağlama seçeneğinin kullanıma aldığı yönetim seçeneklerinin ekran görüntüsü.](common/provisioning.png)

4. **Hazırlama Modu**'nu **Otomatik** olarak ayarlayın.

    ![Otomatik seçeneği olarak adlandırılan sağlama modu açılan listesinin ekran görüntüsü.](common/provisioning-automatic.png)

5. **Yönetici kimlik bilgileri** bölümünün altında, `https://scim.templafy.com/scim` **kiracı URL 'sini**girin. **Gizli belirteçte**daha önce alınan **SCIM API anahtarı** değerini girin. Azure AD 'nin Templamy 'a bağlanabildiğinden emin olmak için **Bağlantıyı Sına** ' ya tıklayın. Bağlantı başarısız olursa, Templamy hesabınızın yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Kiracı URL 'SI + belirteç](common/provisioning-testconnection-tenanturltoken.png)

6. **Bildirim e-postası** alanına, sağlama hatası bildirimlerini alması gereken bir kişinin veya grubun e-posta adresini girin ve hata oluştuğunda onay kutusu- **e-posta bildirimi gönder**' i işaretleyin.

    ![Bildirim E-postası](common/provisioning-notification-email.png)

7. **Kaydet**’e tıklayın.

8. **Eşlemeler** bölümünde **Azure Active Directory Kullanıcıları Şablonlarım**' a eşitler ' ı seçin.

    ![Templamy Kullanıcı eşlemeleri](media/templafy-provisioning-tutorial/usermapping.png)

9. **Öznitelik eşleme** bölümünde Azure AD 'Den templamy 'a eşitlenen Kullanıcı özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri Için templamy Kullanıcı hesaplarını eşleştirmek için kullanılır. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.

    ![Templamy Kullanıcı öznitelikleri](media/templafy-provisioning-tutorial/userattribute.png)

10. **Eşlemeler** bölümünde, **Azure Active Directory grupları Şablonlarım**' a eşitler ' ı seçin.

    ![Templamy grup eşlemeleri](media/templafy-provisioning-tutorial/groupmapping.png)

11. **Öznitelik eşleme** bölümünde Azure AD 'Den templamy 'a eşitlenen grup özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri Için templamy grupları ile eşleştirmek için kullanılır. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.

    ![Templamy grup öznitelikleri](media/templafy-provisioning-tutorial/groupattribute.png)

12. Kapsam belirleme filtrelerini yapılandırmak için [Kapsam belirleme filtresi öğreticisi](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md) ile sunulan yönergeleri izleyin.

13. Templamy Azure AD sağlama hizmetini etkinleştirmek için, **Ayarlar** bölümünde **sağlama durumunu** **Açık** olarak değiştirin.

    ![Hazırlama Durumu Açık](common/provisioning-toggle-on.png)

14. **Ayarlar** bölümünde **kapsam** Içinde Istenen değerleri seçerek templamy için sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Hazırlama Kapsamı](common/provisioning-scope.png)

15. Hazırlama işlemini başlatmak için **Kaydet**'e tıklayın.

    ![Hazırlama Yapılandırmasını Kaydetme](common/provisioning-configuration-save.png)

    Bu işlem, **Ayarlar** bölümünde **kapsam** içinde tanımlanan tüm kullanıcılar ve/veya grupların ilk eşitlemesini başlatır. İlk eşitlemenin daha sonra, Azure AD sağlama hizmeti çalıştığı sürece yaklaşık 40 dakikada bir oluşan sonraki eşitlemeler yerine gerçekleştirilmesi daha uzun sürer. İlerlemeyi izlemek için **eşitleme ayrıntıları** bölümünü kullanabilir ve Azure AD sağlama hizmeti tarafından templamy üzerinde gerçekleştirilen tüm eylemleri açıklayan, sağlama etkinlik raporuna ilişkin bağlantıları izleyebilirsiniz.

    Azure AD sağlama günlüklerinin nasıl okunduğu hakkında daha fazla bilgi için bkz. [Otomatik Kullanıcı hesabı sağlamayı raporlama](../app-provisioning/check-status-user-account-provisioning.md)
    
## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı hazırlamayı yönetme](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Hazırlama etkinliği günlüklerini incelemeyi ve rapor oluşturmayı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)
