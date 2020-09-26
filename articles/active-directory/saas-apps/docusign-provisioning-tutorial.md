---
title: "Öğretici: Azure Active Directory ile otomatik Kullanıcı sağlaması için DocuSign 'ı yapılandırma | Microsoft Docs"
description: Azure Active Directory ve DocuSign arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: article
ms.date: 01/26/2018
ms.author: jeedes
ms.openlocfilehash: d56f9890396d0381d24676964dabc57e2020ec28
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91317438"
---
# <a name="tutorial-configure-docusign-for-automatic-user-provisioning"></a>Öğretici: otomatik Kullanıcı sağlaması için DocuSign 'ı yapılandırma

Bu öğreticinin amacı, Azure AD 'den DocuSign 'a Kullanıcı hesaplarını otomatik olarak sağlamak ve devre dışı bırakmak için DocuSign ve Azure AD 'de gerçekleştirmeniz gereken adımları gösterir.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide anlatılan senaryoda aşağıdakilere sahip olduğunuz kabul edilmiştir:

*   Azure Active Directory kiracısı.
*   Bir DocuSign çoklu oturum açma etkin aboneliği.
*   Ekip Yöneticisi izinleri ile DocuSign içindeki bir kullanıcı hesabı.

## <a name="assigning-users-to-docusign"></a>Kullanıcıları DocuSign 'a atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişim alacağını belirleyebilmek için "atamalar" adlı bir kavram kullanır. Otomatik Kullanıcı hesabı sağlama bağlamında, yalnızca Azure AD 'de bir uygulamaya "atanmış" olan kullanıcılar ve gruplar eşitlenir.

Sağlama hizmetini yapılandırmadan ve etkinleştirmeden önce, Azure AD 'deki hangi kullanıcıların ve/veya grupların DocuSign uygulamanıza erişmesi gereken kullanıcıları temsil ettiğini belirlemeniz gerekir. Karar verdikten sonra buradaki yönergeleri izleyerek bu kullanıcıları DocuSign uygulamanıza atayabilirsiniz:

[Kurumsal uygulamaya Kullanıcı veya Grup atama](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-docusign"></a>DocuSign 'e Kullanıcı atamaya yönelik önemli ipuçları

*   Sağlama yapılandırmasını test etmek için, DocuSign 'a tek bir Azure AD kullanıcısının atanması önerilir. Daha sonra ek kullanıcılar atanabilir.

*   Bir kullanıcıyı DocuSign 'a atarken geçerli bir kullanıcı rolü seçmeniz gerekir. "Varsayılan erişim" rolü sağlama için çalışmaz.

> [!NOTE]
> Azure AD, Docusign uygulamasıyla grup sağlamayı desteklemez, yalnızca kullanıcılar sağlanabilir.

## <a name="enable-user-provisioning"></a>Kullanıcı sağlamayı etkinleştir

Bu bölümde, Azure AD 'nizi DocuSign 'ın Kullanıcı hesabı sağlama API 'sine bağlama ve sağlama hizmeti 'ni, Azure AD 'de Kullanıcı ve grup atamasını temel alan DocuSign 'da atanan kullanıcı hesaplarını oluşturmak, güncelleştirmek ve devre dışı bırakmak için nasıl yapılandıracağınız konusunda kılavuzluk eder.

> [!Tip]
> Ayrıca, [Azure Portal](https://portal.azure.com)' de sağlanan yönergeleri Izleyerek, DocuSign için SAML tabanlı çoklu oturum açmayı da tercih edebilirsiniz. Çoklu oturum açma özelliği otomatik sağlanmadan bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini karmaşıdirebilirler.

### <a name="to-configure-user-account-provisioning"></a>Kullanıcı hesabı sağlamayı yapılandırmak için:

Bu bölümün amacı, Kullanıcı hesaplarının Active Directory Kullanıcı tarafından bir DocuSign 'a nasıl etkinleştirileceğini özetler.

1. [Azure portal](https://portal.azure.com) **Azure Active Directory > Enterprise Apps > tüm uygulamalar** bölümüne gidin.

1. Çoklu oturum açma için zaten DocuSign yapılandırdıysanız, arama alanını kullanarak DocuSign örneğinizi arayın. Aksi takdirde, **Ekle** ' yi seçin ve uygulama galerisinde **Docusign** için arama yapın. Arama sonuçlarından DocuSign ' ı seçin ve uygulama listenize ekleyin.

1. DocuSign örneğinizi seçin, sonra **sağlama** sekmesini seçin.

1. **Hazırlama Modu**'nu **Otomatik** olarak ayarlayın. 

    ![Azure portal 'de DocuSign için sağlama sekmesinin ekran görüntüsü. Sağlama modu otomatik ve yönetici kullanıcı adına ayarlanır, parola ve test bağlantısı vurgulanır.](./media/docusign-provisioning-tutorial/provisioning.png)

1. **Yönetici kimlik bilgileri** bölümünde aşağıdaki yapılandırma ayarlarını sağlayın:
   
    a. **Yönetici Kullanıcı adı** metin kutusuna Docusign.com atanmış **Sistem Yöneticisi** profiline sahip bir Docusign hesap adı yazın.
   
    b. **Yönetici parolası** metin kutusuna bu hesabın parolasını yazın.

> [!NOTE]
> Hem SSO hem de Kullanıcı sağlama kurulumu ise, sağlama için kullanılan yetkilendirme kimlik bilgilerinin hem SSO hem de Kullanıcı adı/parola ile çalışacak şekilde yapılandırılması gerekir.

1. Azure portal, Azure AD 'nin DocuSign uygulamanıza bağlanabildiğinden emin olmak için **Bağlantıyı Sına** ' ya tıklayın.

1. **Bildirim e-postası** alanına, sağlama hatası bildirimleri alması gereken kişinin veya grubun e-posta adresini girin ve onay kutusunu işaretleyin.

1. Kaydet ' e tıklayın **.**

1. Eşlemeler bölümünde **Azure Active Directory Kullanıcıları DocuSign Ile eşitler** ' ı seçin.

1. **Öznitelik eşlemeleri** bölümünde, Azure AD 'Den DocuSign 'a eşitlenen Kullanıcı özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri Için DocuSign içindeki kullanıcı hesaplarıyla eşleştirmek için kullanılır. Değişiklikleri uygulamak için Kaydet düğmesini seçin.

1. DocuSign için Azure AD sağlama hizmetini etkinleştirmek üzere ayarlar bölümünde **sağlama durumunu** **Açık** olarak değiştirin

1. Kaydet ' e tıklayın **.**

Kullanıcılar ve Gruplar bölümünde DocuSign 'a atanan tüm kullanıcıların ilk eşitlemesini başlatır. İlk eşitlemenin daha sonra, hizmetin çalıştığı sürece yaklaşık 40 dakikada bir oluşan sonraki eşitlemeler yerine gerçekleştirilmesi daha uzun sürer. İlerleme durumunu izlemek için **eşitleme ayrıntıları** bölümünü kullanabilir ve kaynak hazırlama uygulamanızın sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklayan etkinlik günlüklerinin sağlanması için bağlantıları izleyebilirsiniz.

Azure AD sağlama günlüklerinin nasıl okunduğu hakkında daha fazla bilgi için bkz. [Otomatik Kullanıcı hesabı sağlamayı raporlama](../app-provisioning/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı hazırlamayı yönetme](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)
* [Çoklu oturum açmayı yapılandırma](docusign-tutorial.md)
