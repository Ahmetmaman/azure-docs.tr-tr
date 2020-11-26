---
title: 'Öğretici: Cloud akademik My-SSO ile SSO tümleştirmesi Azure Active Directory'
description: Bu öğreticide, Azure Active Directory ile bulut akademik My-SSO arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğreneceksiniz.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 07/16/2020
ms.author: jeedes
ms.openlocfilehash: 822e28402d0b7829b835ad03a3b3cf7d05c3d343
ms.sourcegitcommit: d22a86a1329be8fd1913ce4d1bfbd2a125b2bcae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/26/2020
ms.locfileid: "96181013"
---
# <a name="tutorial-azure-active-directory-single-sign-on-integration-with-cloud-academy---sso"></a>Öğretici: bulut akademik My-SSO ile çoklu oturum açma tümleştirmesi Azure Active Directory

Bu öğreticide Cloud akademik My-SSO 'yu Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. Cloud akademik My-SSO 'yu Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Cloud akademik My-SSO ile kimlerin erişebileceğini denetlemek için Azure AD 'yi kullanın.
* Kullanıcılarınızın Azure AD hesaplarıyla bulut akademik My-SSO ' da otomatik olarak oturum açmalarına olanak sağlayın.
* Hesaplarınızı tek bir merkezi konumda yönetin: Azure portal.

Azure AD ile SaaS uygulaması tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Çoklu oturum açma (SSO) özellikli bir bulut akademik My-SSO aboneliğim.

## <a name="tutorial-description"></a>Öğretici açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edeceksiniz.

Cloud akademik My-SSO, SP tarafından başlatılan SSO 'yu destekler.

Cloud akademik My-SSO 'yu yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak ayıklanmasını ve zaman korumasını koruyan oturum denetimini zorunlu kılabilirsiniz. Oturum denetimleri koşullu erişimden genişletilir. [Microsoft Cloud App Security ile oturum denetimini nasıl zorlayacağınızı öğrenin](/cloud-app-security/proxy-deployment-any-app).

## <a name="add-cloud-academy---sso-from-the-gallery"></a>Galeriden bulut akademik My-SSO 'SU Ekle

Cloud akademik My-SSO ile Azure AD arasındaki tümleştirmeyi yapılandırmak için, Galeriden bulut akademik My-SSO ' u yönetilen SaaS uygulamaları listenize eklemeniz gerekir:

1. [Azure Portal](https://portal.azure.com) bir iş veya okul hesabıyla ya da kişisel bir Microsoft hesabı oturum açın.
1. Sol bölmede **Azure Active Directory**’yi seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Bir uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Cloud AKADEMIK My-SSO** yazın.
1. Sonuçlar panelinde **Cloud akademik My-SSO** ' yı seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.


## <a name="configure-and-test-azure-ad-sso-for-cloud-academy---sso"></a>Cloud akademik My-SSO için Azure AD SSO 'yu yapılandırma ve test etme

Azure AD SSO 'yu, **B. Simon** adlı bir test kullanıcısı kullanarak Cloud akademik My-SSO ile yapılandırıp test edersiniz. SSO 'nun çalışması için, bir Azure AD kullanıcısı ve Cloud akademik My-SSO ' daki ilgili Kullanıcı arasında bir bağlantı ilişkisi oluşturmanız gerekir.

Azure AD SSO 'yu Cloud akademik My-SSO ile yapılandırmak ve test etmek için şu üst düzey adımları tamamlayacaksınız:

1. Kullanıcılarınızın özelliğini kullanmasını sağlamak için **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** .
    1. Azure AD çoklu oturum açma sınamasını test etmek için **[bir Azure AD test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** .
    1. Kullanıcının Azure AD çoklu oturum açma özelliğini kullanmasını sağlamak için **[Test kullanıcısına erişim Izni verin](#grant-access-to-the-test-user)** .
1. Uygulama tarafında **[bulut akademik My-SSO için çoklu oturum açmayı yapılandırın](#configure-single-sign-on-for-cloud-academy)** .
    1. Kullanıcının Azure AD gösterimine karşılık gelen bir **[bulut akademik My-SSO test kullanıcısı oluşturun](#create-a-cloud-academy-test-user)** .
1. Yapılandırmanın çalıştığını doğrulamak için **[test SSO 'su](#test-sso)** .

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için şu adımları izleyin:

1. [Azure Portal](https://portal.azure.com/), **Cloud AKADEMIK My-SSO** uygulama tümleştirmesi sayfasında, **Yönet** bölümünde **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML Ile tek Sign-On ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** kalem düğmesini seçin:

   ![Temel SAML yapılandırmasını düzenlemenin kalem düğmesini gösteren ekran görüntüsü.](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölümünde, **oturum açma URL 'si** kutusuna girin `https://cloudacademy.com/login/enterprise/` .

1. **SAML Ile tek Sign-On ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, **uygulama Federasyon meta verileri URL 'sini** kopyalamak için Kopyala düğmesini seçin. URL 'YI kaydedin.

    ![Uygulama Federasyon meta veri URL 'SI için Kopyala düğmesini gösteren ekran görüntüsü.](common/copy-metadataurl.png)

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, Azure portal B. Simon adlı bir test kullanıcısı oluşturacaksınız.

1. Azure portal sol bölmesinde **Azure Active Directory**' ı seçin. **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.
1. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.
1. **Kullanıcı** özellikleri ' nde şu adımları uygulayın:
   1. **Ad** kutusuna **B. Simon** girin.  
   1. **Kullanıcı adı** kutusuna \<username> @ \<companydomain> .. yazın. \<extension> Örneğin, `B.Simon@contoso.com`.
   1. **Parolayı göster**' i seçin ve ardından **parola** kutusunda görüntülenen değeri yazın.
   1. **Oluştur**’u seçin.

### <a name="grant-access-to-the-test-user"></a>Test kullanıcısına erişim izni verme

Bu bölümde, bu kullanıcıya bulut akademik mi-SSO 'SU erişimi vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde, **Cloud akademik My-SSO**' yı seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünde, **Kullanıcılar ve gruplar**' ı seçin:

   ![Kullanıcılar ve gruplar seçeneğini gösteren ekran görüntüsü.](common/users-groups-blade.png)

1. **Kullanıcı Ekle**' yi seçin ve sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin:

    ![Kullanıcı Ekle düğmesini gösteren ekran görüntüsü.](common/add-assign-user.png)

1. **Kullanıcılar ve gruplar** iletişim kutusunda, **Kullanıcılar** listesinde **B. Simon** öğesini seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. SAML onaylama işlemi içinde herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, Kullanıcı için listeden uygun rolü seçin. Ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. **Atama Ekle** Iletişim kutusunda **ata**' yı seçin.

## <a name="configure-single-sign-on-for-cloud-academy"></a>Bulut akademik My için çoklu oturum açmayı yapılandırma

1. Farklı bir tarayıcı penceresinde, bulut akademik My-SSO şirket sitesinde yönetici olarak oturum açın.

1. Şirketinizin adını seçin ve ardından açılan menüdeki **ayarlar & tümleştirmeler** ' i seçin:

    ![Ayarlar & tümleştirmeler seçeneğini gösteren ekran görüntüsü.](./media/cloud-academy-sso-tutorial/config-1.PNG)

1. **Ayarlar & tümleştirmeler** sayfasında, **tümleştirmeler** sekmesinde **SSO** kartını seçin:

    ![Tümleştirme sekmesindeki SSO kartını gösteren ekran görüntüsü.](./media/cloud-academy-sso-tutorial/config-2.PNG)

1. Bu sayfada aşağıdaki adımları uygulayın:

    ![Inegrations > SSO sayfasını gösteren ekran görüntüsü.](./media/cloud-academy-sso-tutorial/config-3.PNG)

    a. **VARLıK kimliği URL 'si** kutusuna, Azure Portal KOPYALADıĞıNıZ varlık kimliği değerini girin.

    b. **SSO URL 'si** kutusunda, Azure Portal kopyaladığınız oturum açma URL 'si değerini yapıştırın.

    c. İndirilen Base64 sertifikasını Not defteri 'ndeki Azure portal açın. İçeriğini **sertifika** kutusuna yapıştırın.

    d. **Ad kimliği biçimi** kutusunda varsayılan değeri tutun: `urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified` .

1. **Kaydet**’i seçin.

    > [!NOTE]
    > Cloud akademik My-SSO yapılandırma hakkında daha fazla bilgi için bkz. [Çoklu oturum açmayı ayarlama](https://support.cloudacademy.com/hc/articles/360043908452-Setting-Up-Single-Sign-On).

### <a name="create-a-cloud-academy-test-user"></a>Bir bulut akademik test kullanıcısı oluşturma

1. Cloud akademik My-SSO ' da oturum açın.

1. Şirketinizin adını seçin ve açılan menüden **Üyeler** ' i seçin:

    ![Üyeler seçeneğini gösteren ekran görüntüsü.](./media/cloud-academy-sso-tutorial/create-user.PNG)

1. **Üyeleri davet et** ' i ve sonra **tek bir üyeyi davet et**' i seçin

    ![Tek bir üyeyi davet et seçeneğini gösteren ekran görüntüsü.](./media/cloud-academy-sso-tutorial/create-user-1.PNG)

1. Gerekli alanlara değerleri girip **davet et**' i seçin:

    ![Üye davet etme iletişim kutusunu gösteren ekran görüntüsü.](./media/cloud-academy-sso-tutorial/create-user-2.PNG)

## <a name="test-sso"></a>Test SSO 'SU 

Artık erişim paneli 'ni kullanarak Azure AD SSO yapılandırmanızı test edersiniz.

Access panel 'de Cloud akademik My-SSO kutucuğunu seçtiğinizde, SSO 'yu ayarladığınız Cloud akademik My-SSO örneğinde otomatik olarak oturum açmış olmanız gerekir. Daha fazla bilgi için bkz. [erişim paneli 'Ne giriş](../user-help/my-apps-portal-end-user-access.md).

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler](./tutorial-list.md)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

- [Azure Active Directory Koşullu erişim nedir?](../conditional-access/overview.md)

- [Azure AD ile Cloud akademik My-SSO 'yu deneyin](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security oturum denetimi nedir?](/cloud-app-security/proxy-intro-aad)

- [Gelişmiş görünürlük ve denetimleri kullanarak bulut akademik My-SSO koruması](/cloud-app-security/proxy-intro-aad)