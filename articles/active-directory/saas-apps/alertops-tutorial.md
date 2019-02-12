---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile AlertOps | Microsoft Docs'
description: Azure Active Directory ve AlertOps arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 3db13ed4-35c2-4b1e-bed8-9b5977061f93
ms.service: Azure-Active-Directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/08/2019
ms.author: jeedes
ms.openlocfilehash: 7ea7ac7e9c41dd11815da85d4a791eb8c93f7bec
ms.sourcegitcommit: 39397603c8534d3d0623ae4efbeca153df8ed791
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2019
ms.locfileid: "56100389"
---
# <a name="tutorial-azure-active-directory-integration-with-alertops"></a>Öğretici: AlertOps ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile AlertOps tümleştirme konusunda bilgi edinin.
Azure AD ile AlertOps tümleştirme ile aşağıdaki avantajları sağlar:

* AlertOps erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak (çoklu oturum açma) AlertOps için kendi Azure AD hesapları ile oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile AlertOps yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* AlertOps tek oturum açma etkin abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* AlertOps destekler **SP ve IDP** tarafından başlatılan

## <a name="adding-alertops-from-the-gallery"></a>Galeriden AlertOps ekleme

Azure AD'de AlertOps tümleştirmesini yapılandırmak için AlertOps Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden AlertOps eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **AlertOps**seçin **AlertOps** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde AlertOps](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma AlertOps adlı bir test kullanıcı tabanlı test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısının AlertOps ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma AlertOps ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[AlertOps çoklu oturum açmayı yapılandırma](#configure-alertops-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[AlertOps test kullanıcısı oluşturma](#create-alertops-test-user)**  - kullanıcı Azure AD gösterimini bağlı AlertOps Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma ile AlertOps yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **AlertOps** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** uygulamada yapılandırmak isterseniz, bölümü **IDP** başlatılan modu, aşağıdaki adımları gerçekleştirin:

    ![AlertOps etki alanı ve URL'ler tek oturum açma bilgileri](common/idp-intiated.png)

    a. İçinde **tanımlayıcı** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<SUBDOMAIN>.alertops.com`

    b. İçinde **yanıt URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<SUBDOMAIN>.alertops.com/login.aspx`

5. Tıklayın **ek URL'lerini ayarlayın** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![AlertOps etki alanı ve URL'ler tek oturum açma bilgileri](common/metadata-upload-additional-signon.png)

    İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın:  `https://<SUBDOMAIN>.alertops.com/login.aspx`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. İlgili kişi [AlertOps istemci Destek ekibine](mailto:support@alertops.com) bu değerleri almak için. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

6. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

7. Üzerinde **AlertOps kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure Ad tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-alertops-single-sign-on"></a>AlertOps tek oturum açmayı yapılandırın

1. Bir farklı bir tarayıcı penceresinde AlertOps şirketinizin sitesi için yönetici olarak oturum.

2. Tıklayarak **hesap ayarları** sol gezinti panelinde.

    ![AlertOps yapılandırma](./media/alertops-tutorial/configure1.png)

3. Üzerinde **Abonelik ayarları** seçin sayfasında **SSO** ve aşağıdaki adımları gerçekleştirin:

    ![AlertOps yapılandırma](./media/alertops-tutorial/configure2.png)

    a. Seçin **kullanım tek Sign-On(SSO)** onay kutusu.

    b. Seçin **Azure Active Directory** olarak bir **SSO sağlayıcısı** açılır listeden.

    c. İçinde **veren URL'si** metin kutusu, tanımlayıcı olarak kullanılan değeri kullanımı **temel SAML yapılandırma** bölümünde Azure portalında.

    d. İçinde **SAML uç nokta URL'si** metin kutusu, yapıştırma **oturum açma URL'si** Azure portaldan kopyaladığınız değeri.

    e. İçinde **SLO uç nokta URL'si** metin kutusu, yapıştırma **oturum açma URL'si** Azure portaldan kopyaladığınız değeri.

    f. Seçin **SHA256** olarak bir **SAML imza algoritması** açılır listeden.

    g. İndirilen Certificate(Base64) dosyanızı Not Defteri'nde açın. Bu içeriği, panoya kopyalayın ve X.509 sertifikası metin kutusuna yapıştırın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. İçinde **adı** alana **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü **brittasimon@yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için AlertOps erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **AlertOps**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **AlertOps**.

    ![Uygulamalar listesinde AlertOps bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-alertops-test-user"></a>AlertOps test kullanıcısı oluşturma

1. Bir farklı bir tarayıcı penceresinde AlertOps şirketinizin sitesi için yönetici olarak oturum.

2. Tıklayarak **kullanıcılar** sol gezinti panelinde.

    ![AlertOps yapılandırma](./media/alertops-tutorial/user1.png)

3. Seçin **kullanıcı ekleme**.

    ![AlertOps yapılandırma](./media/alertops-tutorial/user2.png)

4. Üzerinde **Kullanıcı Ekle** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![AlertOps yapılandırma](./media/alertops-tutorial/user3.png)

    a. İçinde **oturum açma kullanıcı adı** metin gibi kullanıcının kullanıcı adını girin **Brittasimon**.

    b. İçinde **resmi bir e-posta** metin gibi kullanıcı e-posta adresini girin **Brittasimon@contoso.com**.

    c. İçinde **ad** metin gibi kullanıcı adını girin **Britta**.

    d. İçinde **Soyadı** metin gibi kullanıcı adını girin **Simon**.

    e. Seçin **türü** kuruluşunuz göre açılır listeden bir değer.

    f. Seçin **rol** kullanıcının kuruluşunuzun göre açılır listeden.

    g. **Add (Ekle)** seçeneğini belirleyin.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli AlertOps kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama AlertOps için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [ SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
