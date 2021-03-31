---
title: "Öğretici: Azure Active Directory ile otomatik Kullanıcı sağlama için Jive 'yi yapılandırma | Microsoft Docs"
description: Azure AD 'den Jive 'ye Kullanıcı hesaplarını otomatik olarak sağlamak ve devre dışı bırakmak için Jive ve Azure AD 'de gerçekleştirmeniz gereken adımları öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 01/26/2018
ms.author: jeedes
ms.openlocfilehash: ebee5d986007e07d497056620f0cfc437b2da4d1
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "94356408"
---
# <a name="tutorial-configure-jive-for-automatic-user-provisioning"></a>Öğretici: otomatik Kullanıcı sağlama için Jive 'yi yapılandırma

Bu öğreticinin amacı, Azure AD 'den Jive 'ye Kullanıcı hesaplarını otomatik olarak sağlamak ve devre dışı bırakmak için Jive ve Azure AD 'de gerçekleştirmeniz gereken adımları gösterir.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide anlatılan senaryoda aşağıdakilere sahip olduğunuz kabul edilmiştir:

*   Azure Active Directory kiracısı.
*   Bir Jive çoklu oturum açma etkin aboneliği.
*   Jive 'deki bir kullanıcı hesabı ekip Yöneticisi izinleri ile.

## <a name="assigning-users-to-jive"></a>Kullanıcıları Jive 'ye atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişim alacağını belirleyebilmek için "atamalar" adlı bir kavram kullanır. Otomatik Kullanıcı hesabı sağlama bağlamında, yalnızca Azure AD 'de bir uygulamaya "atanmış" olan kullanıcılar ve gruplar eşitlenir.

Sağlama hizmetini yapılandırmadan ve etkinleştirmeden önce, Azure AD 'deki hangi kullanıcıların ve/veya grupların Jive uygulamanıza erişmesi gereken kullanıcıları temsil ettiğini belirlemeniz gerekir. Karar verdikten sonra buradaki yönergeleri izleyerek bu kullanıcıları Jive uygulamanıza atayabilirsiniz:

[Kurumsal uygulamaya Kullanıcı veya Grup atama](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-jive"></a>Kullanıcıları Jive 'ye atamaya yönelik önemli ipuçları

*   Sağlama yapılandırmasını test etmek için tek bir Azure AD kullanıcısının Jive 'ye atanması önerilir. Ek kullanıcılar ve/veya grupları daha sonra atanabilir.

*   Bir kullanıcıyı Jive 'ye atarken geçerli bir kullanıcı rolü seçmeniz gerekir. "Varsayılan erişim" rolü sağlama için çalışmaz.

## <a name="enable-user-provisioning"></a>Kullanıcı sağlamayı etkinleştir

Bu bölümde, Azure AD 'nizi Jive 'nin Kullanıcı hesabı sağlama API 'sine bağlama ve sağlama hizmeti 'ni, Azure AD 'de Kullanıcı ve grup atamasını temel alan Jive 'de atanan kullanıcı hesaplarını oluşturmak, güncelleştirmek ve devre dışı bırakmak için yapılandırma işlemi kılavuzluk eder.

> [!TIP]
> Ayrıca, [Azure Portal](https://portal.azure.com)sağlanan yönergeleri izleyerek, cive için SAML tabanlı tek Sign-On de seçebilirsiniz. Çoklu oturum açma özelliği otomatik sağlanmadan bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini karmaşıdirebilirler.

### <a name="to-configure-user-account-provisioning"></a>Kullanıcı hesabı sağlamayı yapılandırmak için:

Bu bölümün amacı, Kullanıcı hesaplarının Active Directory Kullanıcı tarafından sağlanması için Jive 'nin nasıl etkinleştirileceğini özetler.
Bu yordamın bir parçası olarak, Jive.com ' den istemeniz gereken bir Kullanıcı güvenlik belirteci sağlamanız gerekir.

1. [Azure portal](https://portal.azure.com) **Azure Active Directory > Enterprise Apps > tüm uygulamalar** bölümüne gidin.

1. Çoklu oturum açma için Jive 'yi zaten yapılandırdıysanız arama alanını kullanarak Jive örneğinizi arayın. Aksi takdirde, **Ekle** ' yi seçin ve uygulama galerisinde **Jive** 'yi arayın. Arama sonuçlarından Jive ' yi seçin ve uygulama listenize ekleyin.

1. Jive örneğinizi seçin, sonra **sağlama** sekmesini seçin.

1. **Hazırlama Modu**'nu **Otomatik** olarak ayarlayın. 

    ![Ekran görüntüsü, sağlama modu otomatik ve ayarlayabileceğiniz diğer değerlere ayarlanmış olan Jive sağlama sayfasını gösterir.](./media/jive-provisioning-tutorial/provisioning.png)

1. **Yönetici kimlik bilgileri** bölümünde aşağıdaki yapılandırma ayarlarını sağlayın:
   
    a. **Jive Yönetici Kullanıcı adı** metin kutusuna Jive.com atanmış **Sistem Yöneticisi** profiline sahip bir Jive hesap adı yazın.
   
    b. **Jive yönetici parolası** metin kutusuna bu hesabın parolasını yazın.
   
    c. **Jive kiracı URL 'si** metin kutusuna Jive KIRACı URL 'sini yazın.
      
      > [!NOTE]
      > Jive kiracı URL 'SI, kuruluşunuz tarafından Jive 'da oturum açmak için kullanılan URL 'dir.  
      > Genellikle, URL şu biçimdedir: **www. \<organization\> . jive.com**.          

1. Azure portal, Azure AD 'nin Jive uygulamanıza bağlanabildiğinden emin olmak için **Bağlantıyı Sına** ' ya tıklayın.

1. **Bildirim e-postası** alanında sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve aşağıdaki onay kutusunu işaretleyin.

1. Kaydet ' e tıklayın **.**

1. Eşlemeler bölümünde **Azure Active Directory Kullanıcıları Jive olarak eşitler** ' ı seçin.

1. **Öznitelik eşlemeleri** bölümünde, Azure AD 'Den Jive 'ye eşitlenen Kullanıcı özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, Jive güncelleştirme işlemlerinde Kullanıcı hesaplarını eşleştirmek için kullanılır. Değişiklikleri uygulamak için Kaydet düğmesini seçin.

1. Jive için Azure AD sağlama hizmetini etkinleştirmek üzere ayarlar bölümünde **sağlama durumunu** **Açık** olarak değiştirin

1. Kaydet ' e tıklayın **.**

Kullanıcılar ve Gruplar bölümünde Jive 'ye atanan tüm Kullanıcı ve/veya grupların ilk eşitlemesini başlatır. İlk eşitlemenin daha sonra, hizmetin çalıştığı sürece yaklaşık 40 dakikada bir oluşan sonraki eşitlemeler yerine gerçekleştirilmesi daha uzun sürer. İlerleme durumunu izlemek için **eşitleme ayrıntıları** bölümünü kullanabilir ve Jive uygulamanızdaki sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklayan etkinlik günlüklerinin sağlanması için bağlantıları izleyebilirsiniz.

Azure AD sağlama günlüklerinin nasıl okunduğu hakkında daha fazla bilgi için bkz. [Otomatik Kullanıcı hesabı sağlamayı raporlama](../app-provisioning/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı hazırlamayı yönetme](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)
* [Çoklu oturum açmayı yapılandırma](jive-tutorial.md)