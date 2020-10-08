---
title: 'Öğretici: yardım Scout ile tümleştirme Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve yardım Scout arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 10/24/2019
ms.author: jeedes
ms.openlocfilehash: 1c6faf2bb1811f7b27a49f8029c833499273d8a1
ms.sourcegitcommit: d2222681e14700bdd65baef97de223fa91c22c55
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/07/2020
ms.locfileid: "91817117"
---
# <a name="tutorial-azure-active-directory-integration-with-help-scout"></a>Öğretici: yardım Scout ile Azure Active Directory tümleştirme

Bu öğreticide, yardım Scout 'ı Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz.
Yardım scout with Azure AD ile tümleştirmek aşağıdaki avantajları sağlar:

* Yardım scout 'a erişimi olan Azure AD 'de denetim yapabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla Scout (çoklu oturum açma) yardımcı olması için otomatik olarak oturum açmasını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz-Azure portal.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek istiyorsanız, bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesini yardım Scout ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Yardım scout çoklu oturum açma etkin abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

* Yardım scout **, SP ve ıDP** tarafından başlatılan SSO 'yu destekler
* Yardım scout **, tam zamanında** Kullanıcı sağlamayı destekler

## <a name="adding-help-scout-from-the-gallery"></a>Galeri 'den yardım Scout ekleme

Yardım scout 'ın Azure AD 'ye tümleştirilmesini yapılandırmak için Galeriden yardım Scout 'ı yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. [Azure Portal](https://portal.azure.com) iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Yardım scout** yazın.
1. Sonuçlar panelinden **Yardım scout** ' ı seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma ve test etme

Bu bölümde, **B. Simon**adlı bir test kullanıcısına dayalı yardım Scout Ile Azure AD çoklu oturum açmayı yapılandırıp test edersiniz.
Çoklu oturum açma için, bir Azure AD kullanıcısı ve yardım Scout içindeki ilgili Kullanıcı arasındaki bağlantı ilişkisinin oluşturulması gerekir.

Azure AD çoklu oturum açmayı, yardım Scout ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını gerçekleştirmeniz gerekir:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
    * Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
    * Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
1. Uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için **[Yardım scout SSO 'Yu yapılandırın](#configure-help-scout-sso)** .
    * Kullanıcının Azure AD gösterimine bağlı olan yardım Scout 'da B. Simon 'a sahip olmak için **[Yardım scout test kullanıcısı oluşturun](#create-help-scout-test-user)** .
1. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Bu bölümde, Azure portal Azure AD çoklu oturum açma özelliğini etkinleştirirsiniz.

Azure AD çoklu oturum açmayı yardım Scout ile birlikte yapılandırmak için aşağıdaki adımları uygulayın:

1. [Azure Portal](https://portal.azure.com/), **Yardım scout** uygulama tümleştirmesi sayfasında, **Çoklu oturum açma**' yı seçin.

    ![Çoklu oturum açma bağlantısını yapılandırma](common/select-sso.png)

1. Çoklu oturum **açma yöntemi seç** iletişim kutusunda, çoklu oturum açmayı etkinleştirmek için **SAML/WS-Besme** modunu seçin.

    ![Çoklu oturum açma seçme modu](common/select-saml-option.png)

1. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **temel SAML yapılandırması** Iletişim kutusunu açmak için **Düzenle** simgesine tıklayın.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölümünde, uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız aşağıdaki adımları uygulayın:

    ![Ekran görüntüsü; tanımlayıcı girebileceğiniz, yanıt U R L ve Kaydet ' i seçebileceğiniz temel SAML yapılandırmasını gösterir.](common/idp-intiated.png)

    a. **Tanımlayıcı** , yardım Scout 'Dan başlayan **hedef kitle URI 'Si (HIZMET sağlayıcısı varlık kimliği)**`urn:`

    b. **Yanıt URL** 'Si, yardım Scout 'dan başlayan **geri gönderme URL 'Si (onaylama işlemi tüketici hizmeti URL 'si)** ile başlar `https://` 

    > [!NOTE]
    > Bu URL 'Lerdeki değerler yalnızca tanıtım amaçlıdır. Bu değerleri gerçek yanıt URL 'SI ve tanımlayıcısından güncelleştirmeniz gerekir. Bu değerleri, Öğreticinin ilerleyen kısımlarında açıklanan kimlik doğrulama bölümü altındaki **Çoklu oturum açma** sekmesinden alırsınız.

1. Uygulamayı **SP** tarafından başlatılan modda yapılandırmak Istiyorsanız **ek URL 'ler ayarla** ' ya tıklayın ve aşağıdaki adımı gerçekleştirin:

    ![Ekran görüntüsü, U R L 'ye bir Işaret girebileceğiniz ek U R 'Leri ayarlamayı gösterir.](common/metadata-upload-additional-signon.png)

    **Oturum açma URL 'si** metin kutusuna bir URL yazın:`https://secure.helpscout.net/members/login/`

1. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, **sertifika (base64)** ' i gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir** ' e tıklayın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. **Yardım scout ayarla** bölümünde uygun URL 'leri gereksiniminize göre kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD tanımlayıcısı

    c. Oturum kapatma URL 'SI

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, B. Simon adlı Azure portal bir test kullanıcısı oluşturmaktır.

1. Azure portal, sol bölmedeki **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.

    !["Kullanıcılar ve gruplar" ve "tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.

    ![Yeni Kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı Özellikleri ' nde aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanına **B. Simon**girin.
  
    b. **Kullanıcı adı** alanına **B. Simon \@ yourcompanydomain. Extension** yazın  
    Örneğin, B.Simon@contoso.com

    c. **Parolayı göster** onay kutusunu seçin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, yardım Scout 'a erişim vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'ı etkinleştirin.

1. Azure portal **Kurumsal uygulamalar**' ı seçin, **tüm uygulamalar**' ı seçin ve ardından **Yardım scout**' u seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Yardım scout**' ı seçin.

    ![Uygulamalar listesindeki yardım Scout bağlantısı](common/all-applications.png)

3. Soldaki menüde **Kullanıcılar ve gruplar**' ı seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. **Kullanıcı Ekle** düğmesine tıklayın, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, kullanıcılar listesinde **B. Simon** öğesini seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

6. SAML onaylama işlemi içinde herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, listeden Kullanıcı için uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

7. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

## <a name="configure-help-scout-sso"></a>Yardım scout SSO 'yu yapılandırma

1. Yardım scout içindeki yapılandırmayı otomatikleştirmek için, **uzantıyı yüklemeniz**' ne tıklayarak **uygulamalarım güvenli oturum açma tarayıcı uzantısını** yüklemeniz gerekir.

    ![Uygulamalarım uzantısı](common/install-myappssecure-extension.png)

1. Tarayıcıya Uzantı eklendikten sonra, **Kurulum Yardım scout** ' a tıklayarak sizi yardım Scout uygulamasına yönlendirirsiniz. Buradan yardım Scout 'da oturum açmak için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı, uygulamayı sizin için otomatik olarak yapılandırır ve 3-7 adımlarını otomatikleştirecektir.

    ![Kurulum yapılandırması](common/setup-sso.png)

1. Yardım scout 'i el ile kurmak istiyorsanız, yeni bir Web tarayıcısı penceresi açın ve yardım Scout şirket sitenizde yönetici olarak oturum açın ve aşağıdaki adımları gerçekleştirin:

1. Üst menüden **Yönet** ' e tıklayın ve ardından açılan menüden **Şirket** ' i seçin.

    ![Ekran görüntüsü, şirket seçiliyken Yönet menüsünü gösterir.](./media/helpscout-tutorial/settings1.png)

1. Sol gezinti bölmesinden **kimlik doğrulaması** ' nı seçin.

    ![Ekran görüntüsü seçilen kimlik doğrulamasını gösterir.](./media/helpscout-tutorial/settings2.png)

1. Bu, sizi SAML ayarları bölümüne götürür ve aşağıdaki adımları gerçekleştirir:

    ![Ekran görüntüsü, belirtilen bilgileri girebileceğiniz çoklu oturum açma sekmesini gösterir.](./media/helpscout-tutorial/settings3.png)

    a. **Geri gönderme URL 'si (onaylama tüketici hizmeti URL 'si)** değerini kopyalayın ve değeri Azure Portal **temel SAML yapılandırması** bölümündeki **yanıt URL 'si** metin kutusuna yapıştırın.

    b. **Hedef kitle URI 'si (hizmet sağlayıcısı VARLıK kimliği)** değerini kopyalayın ve değeri Azure Portal **temel SAML yapılandırması** bölümündeki **tanımlayıcı** metin kutusuna yapıştırın.

1. **SAML etkinleştir** ' i açın ve aşağıdaki adımları gerçekleştirin:

    ![Ekran görüntüsü, SAML 'Yi etkinleştirdiğiniz ve diğer bilgileri ekleyebileceğiniz çoklu oturum açma sekmesini gösterir.](./media/helpscout-tutorial/settings4.png)

    a. **Çoklu oturum açma URL 'si** metin kutusunda, Azure Portal kopyaladığınız **oturum açma URL 'si**değerini yapıştırın.

    b. Azure portal 'ten indirilen **sertifikayı (base64)** karşıya yüklemek Için **sertifikayı karşıya yükle** ' ye tıklayın.

    c. E-posta etki alanları metin kutusunda, kuruluşunuzun e-posta etki alanlarını (s) e.x. girin `contoso.com` . **Email Domains** Birden çok etki alanını virgülle ayırarak ayırabilirsiniz. [Yardım scout oturum açma sayfasında](https://secure.helpscout.net/members/login/) belirli bir etki alanını giren bir yardım Scout kullanıcısı veya Yöneticisi, kimlik bilgileriyle kimlik doğrulaması yapmak Için kimlik sağlayıcısına yönlendirilir.

    d. Son olarak, kullanıcıların bu yöntemden yalnızca bir yardım almak için oturum açmasını istiyorsanız **SAML oturum açmayı zorla** ' ya geçiş yapabilirsiniz. Hala, yardım Scout kimlik bilgileriyle oturum açmak için bu seçeneği bırakmak istiyorsanız, devre dışı bırakabilirsiniz. Bu etkin olsa bile, hesap sahibi, hesap parolasıyla Scout 'a yardımcı olmak için her zaman oturum açabilir.

    e. **Kaydet**’e tıklayın.

### <a name="create-help-scout-test-user"></a>Yardım scout test kullanıcısı oluşturma

Bu bölümde, yardım Scout 'da B. Simon adlı bir Kullanıcı oluşturulur. Yardım scout, varsayılan olarak etkinleştirilen tam zamanında Kullanıcı sağlamayı destekler. Bu bölümde sizin için herhangi bir eylem öğesi yok. Yardım scout 'ta bir kullanıcı zaten mevcut değilse, kimlik doğrulamasından sonra yeni bir tane oluşturulur.

### <a name="test-sso"></a>Test SSO 'SU

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde yardım Scout kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız yardım Scout 'da otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory Koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile yardım Scout 'ı deneyin](https://aad.portal.azure.com/)