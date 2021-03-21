---
title: 'Öğretici: Signagelive ile tümleştirme Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve Signagelive arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 1/11/2019
ms.author: jeedes
ms.openlocfilehash: 78324cfa58a8ac015b085052bdec7e3793befc1b
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "96348458"
---
# <a name="tutorial-azure-active-directory-integration-with-signagelive"></a>Öğretici: Signagelive ile tümleştirme Azure Active Directory

Bu öğreticide, Signagelive 'i Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz.
Signagelive Azure AD ile tümleştirmek aşağıdaki avantajları sağlar:

* Signagelive 'e erişimi olan Azure AD 'de denetim yapabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla Signagelive 'de (çoklu oturum açma) otomatik olarak oturum açmasını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz: Azure portal.

Azure AD ile SaaS uygulaması tümleştirmesi hakkında daha fazla bilgi için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md). Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesini Signagelive ile yapılandırmak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Bir Azure AD ortamınız yoksa, bir [aylık deneme sürümü](https://azure.microsoft.com/pricing/free-trial/)alabilirsiniz.
* Signagelive çoklu oturum açma özellikli bir abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

* Signagelive, SP tarafından başlatılan SSO 'yu destekler.

## <a name="add-signagelive-from-the-gallery"></a>Galeriden Signagelive ekleme

Signagelive tümleştirmesini Azure AD 'ye göre yapılandırmak için, önce galerideki Signagelive öğesini yönetilen SaaS uygulamaları listenize ekleyin.

Galeriden Signagelive eklemek için aşağıdaki adımları uygulayın:

1. [Azure Portal](https://portal.azure.com)sol bölmedeki **Azure Active Directory** simgesini seçin.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. **Kurumsal uygulamalar**' a gidin ve ardından **tüm uygulamalar** seçeneğini belirleyin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için iletişim kutusunun üst kısmındaki  **Yeni uygulama** düğmesini seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Signagelive** girin. 

     ![Sonuç listesinde Signagelive](common/search-new-app.png)

5. Sonuçlar bölmesinde **Signagelive** ' ı seçin ve sonra uygulamayı eklemek için **Ekle** düğmesini seçin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma ve test etme

Bu bölümde, Azure AD çoklu oturum açmayı, **Britta Simon** adlı bir test kullanıcısına göre Signagelive ile yapılandırıp test edersiniz.
Çoklu oturum açma için, Signagelive içinde bir Azure AD kullanıcısı ve ilgili Kullanıcı arasında bir bağlantı kurmanız gerekir.

Azure AD çoklu oturum açmayı Signagelive ile yapılandırmak ve test etmek için önce aşağıdaki yapı taşlarını doldurun:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için [Azure AD çoklu oturum açmayı yapılandırın](#configure-azure-ad-single-sign-on) .
2. Uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için [Signagelive çoklu oturum açmayı yapılandırın](#configure-signagelive-single-sign-on) .
3. Britta Simon ile Azure AD çoklu oturum açma sınamasını test etmek için [bir Azure AD test kullanıcısı oluşturun](#create-an-azure-ad-test-user) .
4. Azure AD 'de çoklu oturum açma özelliğini kullanarak Britta Simon 'u etkinleştirmek için [Azure AD test kullanıcısını atayın](#assign-the-azure-ad-test-user) .
5. Kullanıcının Azure AD gösterimine bağlı olan Signagelive 'de Britta Simon 'a sahip olacak [bir Signagelive test kullanıcısı oluşturun](#create-a-signagelive-test-user) .
6. Yapılandırmanın çalıştığını doğrulamak için [Çoklu oturum açmayı test](#test-single-sign-on) edin.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure portal Azure AD çoklu oturum açma özelliğini etkinleştirirsiniz.

Azure AD çoklu oturum açmayı Signagelive ile yapılandırmak için aşağıdaki adımları uygulayın:

1. [Azure Portal](https://portal.azure.com/), **Signagelive** uygulama tümleştirmesi sayfasında, **Çoklu oturum açma**' yı seçin.

    ![Çoklu oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Çoklu oturum **açma yöntemi seç** iletişim kutusunda, çoklu oturum açmayı etkinleştirmek için **SAML** ' yi seçin.

    ![Çoklu oturum açma seçme modu](common/select-saml-option.png)

3. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, **temel SAML yapılandırması** Iletişim kutusunu açmak için **Düzenle** ' yi seçin.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. **Temel SAML yapılandırması** bölümünde aşağıdaki adımları uygulayın:

    ![Signagelive etki alanı ve URL 'Ler çoklu oturum açma bilgileri](common/sp-signonurl.png)

    **Oturum açma URL 'si** kutusunda, aşağıdaki kalıbı kullanan bir URL girin:`https://login.signagelive.com/sso/<ORGANIZATIONALUNITNAME>`

    > [!NOTE]
    > Değer gerçek değil. Değeri, gerçek oturum açma URL 'SI ile güncelleştirin. Değeri almak için [Signagelive istemci destek ekibine](mailto:support@signagelive.com)başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

5. **SAML Ile tekli Sign-On ayarlama** sayfasında, ihtiyaç verdiğiniz seçenekten **sertifikayı (ham)** Indirmek Için, **SAML imzalama sertifikası** bölümünde **İndir** ' i seçin. Ardından bu dosyayı bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](common/certificateraw.png)

6. **Signagelive ayarla** bölümünde, IHTIYACıNıZ olan URL 'leri kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD tanımlayıcısı

    c. Oturum kapatma URL 'SI

### <a name="configure-signagelive-single-sign-on"></a>Signagelive çoklu oturum açmayı yapılandırma

Signagelive tarafında çoklu oturum açmayı yapılandırmak için, indirilen **sertifikayı (ham)** ve Azure Portal URL 'leri [Signagelive destek ekibine](mailto:support@signagelive.com)gönderin. SAML SSO bağlantısının her iki tarafında da düzgün şekilde ayarlandığından emin olurlar.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Azure portal Britta Simon adlı bir test kullanıcısı oluşturmaktır.

1. Azure portal, sol bölmedeki **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.

    !["Kullanıcılar ve gruplar" ve "tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.

    ![Yeni Kullanıcı düğmesi](common/new-user.png)

3. **Kullanıcı** iletişim kutusunda aşağıdaki adımları uygulayın.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanına **Brittasıon** yazın.
  
    b. **Kullanıcı adı** alanına " brittasimon@yourcompanydomain.extension " yazın. Örneğin, bu durumda " BrittaSimon@contoso.com " girebilirsiniz.

    c. **Parolayı göster** onay kutusunu seçin ve ardından parola kutusunda görüntülenen değeri unutmayın.

    d. **Oluştur**’u seçin.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Signagelive 'e erişim vererek Azure çoklu oturum açma özelliğini kullanmak için Britta Simon özelliğini etkinleştirin.

1. Azure portal **Kurumsal uygulamalar**' ı seçin, **tüm uygulamalar**' ı seçin ve ardından **Signagelive**' ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Signagelive**' yi seçin.

    ![Uygulamalar listesindeki Signagelive bağlantısı](common/all-applications.png)

3. Soldaki menüde **Kullanıcılar ve gruplar**' ı seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. **Kullanıcı Ekle** düğmesini seçin. Sonra **atama Ekle** Iletişim kutusunda **Kullanıcılar ve gruplar**' ı seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. **Kullanıcılar ve gruplar** iletişim kutusunda, **Kullanıcılar** listesinde **Britta Simon**' u seçin. Ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

6. SAML onaylama 'da bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, Kullanıcı için listeden uygun rolü seçin. Sonra, ekranın alt kısmındaki **Seç** düğmesine tıklayın.

7. **Atama Ekle** Iletişim kutusunda **ata** düğmesini seçin.

### <a name="create-a-signagelive-test-user"></a>Signagelive test kullanıcısı oluşturma

Bu bölümde, Signagelive içinde Britta Simon adlı bir Kullanıcı oluşturacaksınız. Kullanıcıları Signagelive platformuna eklemek için [Signagelive destek](mailto:support@signagelive.com) ekibiyle çalışın. Çoklu oturum açma kullanmadan önce kullanıcıları oluşturmanız ve etkinleştirmeniz gerekir.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, Azure AD çoklu oturum açma yapılandırmanızı Uygulamaps portalını kullanarak test edersiniz.

Uygulamaps portalındaki **Signagelive** kutucuğunu seçtiğinizde, otomatik olarak oturum açmış olmanız gerekir. Uygulamaps portalı hakkında daha fazla bilgi için, bkz. [Uygulamaps portalı nedir?](../user-help/my-apps-portal-end-user-access.md).

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](./tutorial-list.md)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

- [Azure Active Directory Koşullu erişim nedir?](../conditional-access/overview.md)