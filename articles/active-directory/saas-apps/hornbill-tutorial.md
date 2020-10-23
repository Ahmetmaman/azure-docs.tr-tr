---
title: 'Öğretici: Hornbill ile tümleştirme Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve Hornbill arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 02/15/2019
ms.author: jeedes
ms.openlocfilehash: 4131f1d0ef36eb49641ac184cd3f72fb719ea90e
ms.sourcegitcommit: 9b8425300745ffe8d9b7fbe3c04199550d30e003
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2020
ms.locfileid: "92443451"
---
# <a name="tutorial-azure-active-directory-integration-with-hornbill"></a>Öğretici: Hornbill ile tümleştirme Azure Active Directory

Bu öğreticide, Hornbill 'i Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz.
Hornbill 'ı Azure AD ile tümleştirmek aşağıdaki avantajları sağlar:

* Azure AD 'de Hornfaturaya erişimi olan denetimi yapabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla Hornbill (çoklu oturum açma) ile otomatik olarak oturum açmasını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz-Azure portal.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek istiyorsanız, bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Hornbill ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Bir Azure AD aboneliği. Bir Azure AD ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü edinebilirsiniz
* Hornbill çoklu oturum açma etkin abonelik

> [!NOTE]
> Bu tümleştirme Ayrıca Azure AD ABD kamu bulut ortamından kullanılabilir. Bu uygulamayı Azure AD ABD kamu bulutu uygulama galerisinde bulabilir ve bunu ortak buluttan yaptığınız şekilde yapılandırabilirsiniz.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

* Hornbill **SP** tarafından başlatılan SSO 'yu destekler
* Hornbill **, tam zamanında** Kullanıcı sağlamayı destekliyor

## <a name="adding-hornbill-from-the-gallery"></a>Galeriden Hornbill ekleme

Hornbill 'in tümleştirmesini Azure AD 'ye göre yapılandırmak için Galeri 'den yönetilen SaaS uygulamaları listenize Hornbill eklemeniz gerekir.

**Galeriden Hornbill eklemek için aşağıdaki adımları uygulayın:**

1. **[Azure Portal](https://portal.azure.com)** sol gezinti panelinde **Azure Active Directory** simgesine tıklayın.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar** seçeneğini belirleyin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için, iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesine tıklayın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Hornbill**yazın, sonuç panelinden **Hornbill** ' i seçin ve ardından **Ekle** düğmesine tıklayarak uygulamayı ekleyin.

     ![Sonuçlar listesinde Hornbill](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma ve test etme

Bu bölümde, **Britta Simon**adlı bir test kullanıcısına göre Hornbill Ile Azure AD çoklu oturum açmayı yapılandırıp test edersiniz.
Çoklu oturum açma için, bir Azure AD kullanıcısı ve Hornbill içindeki ilgili Kullanıcı arasındaki bağlantı ilişkisinin oluşturulması gerekir.

Azure AD çoklu oturum açma 'yı Hornbill ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını gerçekleştirmeniz gerekir:

1. **[Azure AD çoklu oturum açma özelliğini yapılandırarak](#configure-azure-ad-single-sign-on)** kullanıcılarınızın bu özelliği kullanmasına olanak sağlayın.
2. **[Hornfaturala çoklu oturum açmayı yapılandırma](#configure-hornbill-single-sign-on)** -uygulama tarafında tek Sign-On ayarlarını yapılandırmak için.
3. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -Britta Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
4. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanarak Britta Simon 'u etkinleştirin.
5. Hornbill **[test kullanıcısı oluşturun](#create-hornbill-test-user)** . Bu, kullanıcının Azure AD gösterimine bağlı olan Hortta Simon 'ta Britta Simon 'a sahip olacak.
6. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[Çoklu oturum açmayı sınayın](#test-single-sign-on)** .

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure portal Azure AD çoklu oturum açma özelliğini etkinleştirirsiniz.

Azure AD çoklu oturum açmayı Hornbill ile birlikte yapılandırmak için aşağıdaki adımları uygulayın:

1. [Azure Portal](https://portal.azure.com/), **Hornbill** uygulama tümleştirmesi sayfasında, **Çoklu oturum açma**' yı seçin.

    ![Çoklu oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Çoklu oturum **açma yöntemi seç** iletişim kutusunda, çoklu oturum açmayı etkinleştirmek için **SAML/WS-Besme** modunu seçin.

    ![Çoklu oturum açma seçme modu](common/select-saml-option.png)

3. **SAML Ile tek Sign-On ayarla** sayfasında, **temel SAML yapılandırması** Iletişim kutusunu açmak için **Düzenle** simgesine tıklayın.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. **Temel SAML yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Hornbill etki alanı ve URL 'Ler çoklu oturum açma bilgileri](common/sp-identifier.png)

    a. **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<SUBDOMAIN>.hornbill.com/<INSTANCE_NAME>/`

    b. **Tanımlayıcı (VARLıK kimliği)** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<SUBDOMAIN>.hornbill.com/<INSTANCE_NAME>/lib/saml/auth/simplesaml/module.php/saml/sp/metadata.php/saml`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri, gerçek oturum açma URL 'SI ve tanımlayıcısı ile güncelleştirin. Bu değerleri almak için [Hornbill Client destek ekibine](https://www.hornbill.com/support/?request/) başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

5. **SAML Ile tek Sign-On ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, **uygulama Federasyon meta verileri URL 'sini** kopyalamak ve bilgisayarınıza kaydetmek için Kopyala düğmesine tıklayın.

    ![Sertifika indirme bağlantısı](common/copy-metadataurl.png)

### <a name="configure-hornbill-single-sign-on"></a>Hornbill Single Sign-On yapılandırma

1. Farklı bir Web tarayıcısı penceresinde, bir güvenlik yöneticisi olarak Hornfaturaya oturum açın.

2. Giriş sayfasında, **sistem**' e tıklayın.

    ![Hornbill sistemi](./media/hornbill-tutorial/tutorial_hornbill_system.png)

3. **Güvenlik**' e gidin.

    ![Hornbill güvenliği](./media/hornbill-tutorial/tutorial_hornbill_security.png)

4. **SSO profilleri**' ne tıklayın.

    ![Hornbill Single](./media/hornbill-tutorial/tutorial_hornbill_sso.png)

5. Sayfanın sağ tarafında, **Logo ekle**' ye tıklayın.

    ![Hornbill ekleme](./media/hornbill-tutorial/tutorial_hornbill_addlogo.png)

6. **Profil ayrıntıları** çubuğunda, **SAML meta logosunu içeri aktar**' a tıklayın.

    ![Hornbill logosu](./media/hornbill-tutorial/tutorial_hornbill_logo.png)

7. **URL** metin kutusundaki açılır sayfada, Azure Portal kopyaladığınız **uygulama Federasyon meta veri URL 'Sini**yapıştırın ve **işle**' ye tıklayın.

    ![Hornbill işlemi](./media/hornbill-tutorial/tutorial_hornbill_process.png)

8. İşlemler ' i tıkladıktan sonra, **profil ayrıntıları** bölümünde otomatik olarak otomatik olarak doldurulur.

    ![Hornbill Sayfa1](./media/hornbill-tutorial/tutorial_hornbill_ssopage.png)

    ![Hornbill Page2](./media/hornbill-tutorial/tutorial_hornbill_ssopage1.png)

    ![Hornbill PAGE3](./media/hornbill-tutorial/tutorial_hornbill_ssopage2.png)

9. **Değişiklikleri Kaydet**’e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Azure portal Britta Simon adlı bir test kullanıcısı oluşturmaktır.

1. Azure portal, sol bölmedeki **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.

    !["Kullanıcılar ve gruplar" ve "tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.

    ![Yeni Kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı Özellikleri ' nde aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanına **Brittasıon**yazın.
  
    b. **Kullanıcı adı** alanına **brittasıon \@ yourcompanydomain. Extension** yazın  
    Örneğin, BrittaSimon@contoso.com

    c. **Parolayı göster** onay kutusunu seçin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Hornfaturaya erişim vererek Azure çoklu oturum açma özelliğini kullanmak için Britta Simon 'u etkinleştirin.

1. Azure portal **Kurumsal uygulamalar**' ı seçin, **tüm uygulamalar**' ı seçin ve ardından **hornfaturala**' yı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Hornbill**' ı seçin.

    ![Uygulamalar listesindeki Hornbill bağlantısı](common/all-applications.png)

3. Soldaki menüde **Kullanıcılar ve gruplar**' ı seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. **Kullanıcı Ekle** düğmesine tıklayın, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. **Kullanıcılar ve gruplar** Iletişim kutusunda kullanıcılar listesinden **Britta Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

6. SAML onaylama işlemi içinde herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, listeden Kullanıcı için uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

7. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

### <a name="create-hornbill-test-user"></a>Hornbill test kullanıcısı oluştur

Bu bölümde, Hortta Simon adlı bir Kullanıcı Hornbill ' de oluşturulur. Hornbill, varsayılan olarak etkinleştirilen tam zamanında Kullanıcı sağlamayı destekler. Bu bölümde sizin için herhangi bir eylem öğesi yok. Hornbill 'de bir kullanıcı zaten mevcut değilse, kimlik doğrulamasından sonra yeni bir tane oluşturulur.

> [!Note]
> El ile bir kullanıcı oluşturmanız gerekiyorsa, [Hornbill Client destek ekibine](https://www.hornbill.com/support/?request/)başvurun.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde Hornbill kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız Hornfaturanızda otomatik olarak oturum açmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](../user-help/my-apps-portal-end-user-access.md).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](./tutorial-list.md)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

- [Azure Active Directory Koşullu erişim nedir?](../conditional-access/overview.md)