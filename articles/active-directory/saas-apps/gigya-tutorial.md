---
title: 'Öğretici: Gigya ile tümleştirme Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve Gigya arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 02/18/2019
ms.author: jeedes
ms.openlocfilehash: 849b54e6d96b8f6f33f2a0925b9a1b9c3eebd56b
ms.sourcegitcommit: 9b8425300745ffe8d9b7fbe3c04199550d30e003
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2020
ms.locfileid: "92450127"
---
# <a name="tutorial-azure-active-directory-integration-with-gigya"></a>Öğretici: Gigya ile tümleştirme Azure Active Directory

Bu öğreticide, Gigya Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz.
Gigya 'ın Azure AD ile tümleştirilmesi aşağıdaki avantajları sağlar:

* Azure AD 'de Gigya 'ya erişimi olan bir denetim yapabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla Gigya (çoklu oturum açma) için otomatik olarak oturum açmasını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz-Azure portal.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek istiyorsanız, bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Gigya ile yapılandırmak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Bir Azure AD ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü edinebilirsiniz
* Gigya çoklu oturum açma etkin abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

* Gigya **SP** tarafından başlatılan SSO 'yu destekler

## <a name="adding-gigya-from-the-gallery"></a>Galeriden Gigya ekleme

Gigya 'ın Azure AD 'ye tümleştirmesini yapılandırmak için, Galeriden Gigya 'yı yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden Gigya eklemek için aşağıdaki adımları uygulayın:**

1. **[Azure Portal](https://portal.azure.com)** sol gezinti panelinde **Azure Active Directory** simgesine tıklayın.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar** seçeneğini belirleyin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için, iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesine tıklayın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Gigya**yazın, sonuç panelinden **Gigya** ve ardından **Ekle** düğmesine tıklayarak uygulamayı ekleyin.

     ![Sonuç listesinde Gigya](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma ve test etme

Bu bölümde, **Britta Simon**adlı bir test kullanıcısına göre Gigya Ile Azure AD çoklu oturum açmayı yapılandırıp test edersiniz.
Çoklu oturum açma için, bir Azure AD kullanıcısı ve Gigya 'da ilgili Kullanıcı arasındaki bağlantı ilişkisinin oluşturulması gerekir.

Azure AD çoklu oturum açma 'yı Gigya ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını gerçekleştirmeniz gerekir:

1. **[Azure AD çoklu oturum açma özelliğini yapılandırarak](#configure-azure-ad-single-sign-on)** kullanıcılarınızın bu özelliği kullanmasına olanak sağlayın.
2. Uygulama tarafında tek Sign-On ayarlarını yapılandırmak için **[Gigya Single oturum açmayı yapılandırın](#configure-gigya-single-sign-on)** .
3. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -Britta Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
4. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanarak Britta Simon 'u etkinleştirin.
5. **[Gigya test kullanıcısı oluşturun](#create-gigya-test-user)** . Gigya 'Da kullanıcının Azure AD gösterimine bağlanmış Britta Simon 'un bir karşılığı vardır.
6. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[Çoklu oturum açmayı sınayın](#test-single-sign-on)** .

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure portal Azure AD çoklu oturum açma özelliğini etkinleştirirsiniz.

Gigya ile Azure AD çoklu oturum açmayı yapılandırmak için aşağıdaki adımları uygulayın:

1. [Azure Portal](https://portal.azure.com/), **Gigya** uygulama tümleştirmesi sayfasında, **Çoklu oturum açma**' yı seçin.

    ![Çoklu oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Çoklu oturum **açma yöntemi seç** iletişim kutusunda, çoklu oturum açmayı etkinleştirmek için **SAML/WS-Besme** modunu seçin.

    ![Çoklu oturum açma seçme modu](common/select-saml-option.png)

3. **SAML Ile tek Sign-On ayarlama** sayfasında, **temel SAML yapılandırması** Iletişim kutusunu açmak için **Düzenle** simgesine tıklayın.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. **Temel SAML yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Gigya etki alanı ve URL 'Ler çoklu oturum açma bilgileri](common/sp-identifier.png)

    a. **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`http://<companyname>.gigya.com`

    b. **Tanımlayıcı (VARLıK kimliği)** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://fidm.gigya.com/saml/v2.0/<companyname>`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri, gerçek oturum açma URL 'SI ve tanımlayıcısı ile güncelleştirin. Bu değerleri almak için [Gigya istemci destek ekibine](https://developers.gigya.com/display/GD/Opening+A+Support+Incident) başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

5. **SAML Ile tek Sign-On ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, **sertifika (base64)** ' i gereksiniminize göre ve bilgisayarınıza kaydetmek için **İndir** ' e tıklayın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. **Gigya ayarla** bölümünde uygun URL 'leri gereksiniminize göre kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD tanımlayıcısı

    c. Oturum kapatma URL 'SI

### <a name="configure-gigya-single-sign-on"></a>Gigya Single Sign-On yapılandırma

1. Farklı bir Web tarayıcısı penceresinde, Gigya şirket sitenizde yönetici olarak oturum açın.

2. **Ayarlar \> SAML oturum açma**bölümüne gidin ve **Ekle** düğmesine tıklayın.
   
    ![SAML oturum açma](./media/gigya-tutorial/ic789532.png "SAML oturum açma")

3. **SAML oturum açma** bölümünde aşağıdaki adımları uygulayın:
   
    ![SAML yapılandırması](./media/gigya-tutorial/ic789533.png "SAML yapılandırması")
   
    a. **Ad** metin kutusuna yapılandırmanız için bir ad yazın.
   
    b. **Veren** metin kutusunda, Azure portalından kopyaladığınız **Azure AD tanımlayıcısının** değerini yapıştırın. 
   
    c. **Tek Sign-On hizmet URL 'si** metin kutusunda, Azure portalından kopyaladığınız **oturum açma URL 'si** değerini yapıştırın.
   
    d. **Ad kimliği biçimi** metin kutusunda, Azure portalından kopyaladığınız **ad tanımlayıcı biçiminin** değerini yapıştırın.
   
    e. Azure portal 'den indirilen Not defteri 'nde Base-64 kodlu sertifikanızı açın, içeriğini panonuza kopyalayın ve **X. 509.440 sertifika** metin kutusuna yapıştırın.
   
    f. **Ayarları Kaydet**' e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Azure portal Britta Simon adlı bir test kullanıcısı oluşturmaktır.

1. Azure portal, sol bölmedeki **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.

    !["Kullanıcılar ve gruplar" ve "tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.

    ![Yeni Kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı Özellikleri ' nde aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanına **Brittasıon**girin.
  
    b. **Kullanıcı adı** alanına ** \@ bricompansıon yourcompanydomain. Extension** yazın  
    Örneğin, BrittaSimon@contoso.com

    c. **Parolayı göster** onay kutusunu seçin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Gigya 'ye erişim izni vererek Azure çoklu oturum açma özelliğini kullanmak için Britta Simon 'u etkinleştirirsiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin, **tüm uygulamalar**' ı seçin ve ardından **Gigya**' yi seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Gigya**öğesini seçin.

    ![Uygulamalar listesindeki Gigya bağlantısı](common/all-applications.png)

3. Soldaki menüde **Kullanıcılar ve gruplar**' ı seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. **Kullanıcı Ekle** düğmesine tıklayın, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. **Kullanıcılar ve gruplar** Iletişim kutusunda kullanıcılar listesinde **Britta Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

6. SAML onaylama işlemi içinde herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, listeden Kullanıcı için uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

7. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

### <a name="create-gigya-test-user"></a>Gigya test kullanıcısı oluşturma

Azure AD kullanıcılarının Gigya 'da oturum açmasını sağlamak için, Gigya 'a sağlanması gerekir. Gigya durumunda, sağlama el ile gerçekleştirilen bir görevdir.

### <a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:

1. **Gigya** şirket sitenizde yönetici olarak oturum açın.

2. **Yönetici \> Kullanıcıları Yönet**' e gidin ve ardından **kullanıcıları davet et**' i tıklatın.
   
    ![Kullanıcıları yönetme](./media/gigya-tutorial/ic789535.png "Kullanıcıları Yönet")

3. Kullanıcıları davet et iletişim kutusunda aşağıdaki adımları gerçekleştirin:
   
    ![Kullanıcıları davet et](./media/gigya-tutorial/ic789536.png "Kullanıcıları davet et")
   
    a. **E-posta** metin kutusuna, sağlamak istediğiniz geçerli bir Azure Active Directory hesabının e-posta diğer adını yazın.
    
    b. **Kullanıcıyı davet et**' e tıklayın.
      
    > [!NOTE]
    > Azure Active Directory hesap sahibi, hesabı etkin olmadan önce onaylamaya yönelik bir bağlantı içeren bir e-posta alır.
    > 

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde Gigya kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız Gigya 'da otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](../user-help/my-apps-portal-end-user-access.md).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](./tutorial-list.md)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

- [Azure Active Directory Koşullu erişim nedir?](../conditional-access/overview.md)