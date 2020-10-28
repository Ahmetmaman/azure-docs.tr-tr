---
title: 'Öğretici: SAML 1,1 belirteci etkin LOB uygulamasıyla Azure Active Directory tümleştirme | Microsoft Docs'
description: Azure Active Directory ve SAML 1,1 belirteci etkinleştirilmiş LOB uygulaması arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 12/24/2018
ms.author: jeedes
ms.openlocfilehash: 0b15d560e2678772cefdf3d87c047013b24ed467
ms.sourcegitcommit: 4cb89d880be26a2a4531fedcc59317471fe729cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2020
ms.locfileid: "92675479"
---
# <a name="tutorial-azure-active-directory-integration-with-saml-11-token-enabled-lob-app"></a>Öğretici: SAML 1,1 belirteci etkin LOB uygulamasıyla Azure Active Directory tümleştirme

Bu öğreticide, SAML 1,1 belirteci etkinleştirilmiş LOB uygulamasını Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz.
SAML 1,1 belirteci etkinleştirilmiş LOB uygulamasını Azure AD ile tümleştirmek aşağıdaki avantajları sağlar:

* Azure AD 'de SAML 1,1 belirteci etkinleştirilmiş LOB uygulamasına erişimi olan bir denetim yapabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla SAML 1,1 belirteci etkinleştirilmiş LOB uygulaması (çoklu oturum açma) ile otomatik olarak oturum açmasını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz-Azure portal.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek istiyorsanız, bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Ön koşullar

SAML 1,1 belirteci etkinleştirilmiş LOB uygulamasıyla Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Bir Azure AD ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü edinebilirsiniz
* SAML 1,1 belirteci etkin LOB uygulaması çoklu oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

* SAML 1,1 belirteci etkin LOB uygulaması **SP** tarafından başlatılan SSO 'yu destekler

## <a name="adding-saml-11-token-enabled-lob-app-from-the-gallery"></a>Galeriden SAML 1,1 belirteci etkin LOB uygulaması ekleniyor

SAML 1,1 belirteci etkinleştirilmiş LOB uygulamasının tümleştirmesini Azure AD 'ye göre yapılandırmak için, Galeriden SAML 1,1 belirteci etkin LOB uygulaması 'nı galerisinden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden SAML 1,1 belirteci etkin LOB uygulaması eklemek için aşağıdaki adımları uygulayın:**

1. **[Azure Portal](https://portal.azure.com)** sol gezinti panelinde **Azure Active Directory** simgesine tıklayın.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar** seçeneğini belirleyin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için, iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesine tıklayın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **saml 1,1 belirteci ETKIN lob uygulaması** yazın, sonuç panelinden **SAML 1,1 belirteci etkin lob uygulaması** ' nı seçin, sonra da uygulamayı eklemek için düğme **Ekle** ' ye tıklayın.

     ![SAML 1,1 belirteci sonuç listesinde LOB uygulaması etkinleştirildi](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma ve test etme

Bu bölümde, Azure AD çoklu oturum açmayı, **Britta Simon** adlı bir test KULLANıCıSıNA göre SAML 1,1 BELIRTECI etkinleştirilmiş lob uygulamasıyla yapılandırıp test edersiniz.
Çoklu oturum açma için, bir Azure AD kullanıcısı ve SAML 1,1 belirteci etkinleştirilmiş LOB uygulaması içindeki ilgili Kullanıcı arasındaki bağlantı ilişkisi kurulmalıdır.

SAML 1,1 belirteci etkinleştirilmiş LOB uygulamasıyla Azure AD çoklu oturum açmayı yapılandırmak ve test etmek için aşağıdaki yapı taşlarını gerçekleştirmeniz gerekir:

1. **[Azure AD çoklu oturum açma özelliğini yapılandırarak](#configure-azure-ad-single-sign-on)** kullanıcılarınızın bu özelliği kullanmasına olanak sağlayın.
2. **[SAML 1,1 belirtecini yapılandırma ETKIN lob uygulaması çoklu oturum açma](#configure-saml-11-token-enabled-lob-app-single-sign-on)** -uygulama tarafında tek Sign-On ayarlarını yapılandırmak için.
3. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -Britta Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
4. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanarak Britta Simon 'u etkinleştirin.
5. SAML **[1,1 belirteci ETKIN lob uygulaması test kullanıcısı oluşturma](#create-saml-11-token-enabled-lob-app-test-user)** -kullanıcının Azure AD gösterimine bağlı olan SAML 1,1 BELIRTECI etkinleştirilmiş lob uygulamasında Britta Simon 'un bir karşılığı olacak şekilde.
6. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[Çoklu oturum açmayı sınayın](#test-single-sign-on)** .

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure portal Azure AD çoklu oturum açma özelliğini etkinleştirirsiniz.

Azure AD çoklu oturum açmayı SAML 1,1 belirteci etkinleştirilmiş LOB uygulamasıyla yapılandırmak için aşağıdaki adımları uygulayın:

1. [Azure Portal](https://portal.azure.com/), **SAML 1,1 belirteci etkinleştirilmiş lob** uygulaması uygulama tümleştirmesi sayfasında, **Çoklu oturum açma** ' yı seçin.

    ![Çoklu oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Çoklu oturum **açma yöntemi seç** iletişim kutusunda, çoklu oturum açmayı etkinleştirmek için **SAML/WS-Besme** modunu seçin.

    ![Çoklu oturum açma seçme modu](common/select-saml-option.png)

3. **SAML Ile tek Sign-On ayarlama** sayfasında, **temel SAML yapılandırması** Iletişim kutusunu açmak için **Düzenle** simgesine tıklayın.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. **Temel SAML yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![SAML 1,1 belirteci etkin LOB uygulaması etki alanı ve URL 'Ler çoklu oturum açma bilgileri](common/sp-identifier.png)

    a. **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://your-app-url`

    b. **Tanımlayıcı (VARLıK kimliği)** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://your-app-url`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri, gerçek oturum açma URL 'SI ve tanımlayıcısı ile güncelleştirin. Bu değerleri almak için SAML 1,1 belirteci etkinleştirilmiş LOB uygulama Istemci desteği ekibine başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

4. **SAML Ile tek Sign-On ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, **sertifika (base64)** ' i gereksiniminize göre ve bilgisayarınıza kaydetmek için **İndir** ' e tıklayın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. **SAML 1,1 belirteci ETKINLEŞTIRILMIŞ lob uygulamasını ayarlama** bölümünde uygun URL 'leri gereksiniminize göre kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD tanımlayıcısı

    c. Oturum kapatma URL 'SI

### <a name="configure-saml-11-token-enabled-lob-app-single-sign-on"></a>SAML 1,1 belirteci etkin LOB uygulaması tek Sign-On yapılandırma

**Saml 1,1 belirteci ETKINLEŞTIRILMIŞ lob uygulaması** tarafında çoklu oturum açmayı yapılandırmak için, indirilen **sertifikayı (Base64)** ve Azure Portal ' den uygun kopyalanmış URL 'leri SAML 1,1 belirteci etkinleştirilmiş LOB uygulama desteği ekibine göndermeniz gerekir. Bu ayar, SAML SSO bağlantısının her iki tarafında da düzgün bir şekilde ayarlanmasını sağlamak üzere ayarlanmıştır.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Azure portal Britta Simon adlı bir test kullanıcısı oluşturmaktır.

1. Azure portal, sol bölmedeki **Azure Active Directory** ' i seçin, **Kullanıcılar** ' ı seçin ve ardından **tüm kullanıcılar** ' ı seçin.

    !["Kullanıcılar ve gruplar" ve "tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.

    ![Yeni Kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı Özellikleri ' nde aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanına **Brittasıon** girin.
  
    b. **Kullanıcı adı** alanına **\@ bricompansıon yourcompanydomain. Extension** yazın  
    Örneğin, BrittaSimon@contoso.com

    c. **Parolayı göster** onay kutusunu seçin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur** 'a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, SAML 1,1 belirteci etkinleştirilmiş LOB uygulamasına erişim izni vererek Azure çoklu oturum açma özelliğini kullanmak için Britta Simon özelliğini etkinleştirirsiniz.

1. Azure portal **Kurumsal uygulamalar** ' ı seçin, **tüm uygulamalar** ' ı seçin, sonra **SAML 1,1 belirteci etkin lob uygulaması** ' nı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde, yazın ve **SAML 1,1 belirteci ETKIN lob uygulaması** ' nı seçin.

    ![Uygulamalar listesinde SAML 1,1 belirteci etkin LOB uygulaması bağlantısı](common/all-applications.png)

3. Soldaki menüde **Kullanıcılar ve gruplar** ' ı seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. **Kullanıcı Ekle** düğmesine tıklayın, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. **Kullanıcılar ve gruplar** Iletişim kutusunda kullanıcılar listesinde **Britta Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

6. SAML onaylama işlemi içinde herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, listeden Kullanıcı için uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

7. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

### <a name="create-saml-11-token-enabled-lob-app-test-user"></a>SAML 1,1 belirteci etkin LOB uygulaması test kullanıcısı oluşturma

Bu bölümde SAML 1,1 belirteci etkinleştirilmiş LOB uygulaması bölümünde Britta Simon adlı bir Kullanıcı oluşturacaksınız. SAML 1,1 belirteci etkinleştirilmiş LOB uygulama platformunda kullanıcıları eklemek için SAML 1,1 belirteci etkinleştirilmiş LOB uygulama destek ekibi ile çalışın. Çoklu oturum açma kullanılmadan önce kullanıcıların oluşturulması ve etkinleştirilmesi gerekir.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde SAML 1,1 belirteci etkin LOB uygulaması kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız SAML 1,1 belirteci etkinleştirilmiş LOB uygulamasında otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](../user-help/my-apps-portal-end-user-access.md).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](./tutorial-list.md)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

- [Azure Active Directory Koşullu erişim nedir?](../conditional-access/overview.md)