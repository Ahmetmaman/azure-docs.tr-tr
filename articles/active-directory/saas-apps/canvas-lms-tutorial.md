---
title: 'Öğretici: Tuval ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve tuval arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: bfed291c-a33e-410d-b919-5b965a631d45
ms.service: Azure-Active-Directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/02/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: ecc3ad9fcf1bb1aee9392f0dfcf40807b0edf508
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2019
ms.locfileid: "56183778"
---
# <a name="tutorial-azure-active-directory-integration-with-canvas"></a>Öğretici: Tuval ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile tuval tümleştirme konusunda bilgi edinin.
Tuval Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* Tuval erişimi, Azure AD'de kontrol edebilirsiniz.
* Kullanıcılarınızın otomatik olarak tuvale (çoklu oturum açma) ile Azure AD hesaplarına açan olmasını etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Tuval ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Tuval çoklu oturum açma abonelik etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Tuval destekler **SP** tarafından başlatılan

## <a name="adding-canvas-from-the-gallery"></a>Galeriden tuval ekleme

Azure AD'de tuvalinin tümleştirmesini yapılandırmak için tuval Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden tuval eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **tuval**seçin **tuval** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde tuvali](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma tuval adlı bir test kullanıcı tabanlı test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ile ilgili kullanıcının tuval arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma tuval ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Tuval çoklu oturum açmayı yapılandırma](#configure-canvas-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Tuval test kullanıcısı oluşturma](#create-canvas-test-user)**  - kullanıcı Azure AD gösterimini bağlı tuvalinde Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma tuval ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **tuval** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Tuval etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier.png)

    a. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<tenant-name>.instructure.com`

    b. İçinde **tanımlayıcı (varlık kimliği)** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<tenant-name>.instructure.com/saml2`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL ve tanımlayıcıdır ile güncelleştirin. İlgili kişi [tuval istemci Destek ekibine](https://community.canvaslms.com/community/help) bu değerleri almak için. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

5. İçinde **SAML imzalama sertifikası** bölümünde **Düzenle** açmak için düğmeyi **SAML imzalama sertifikası** iletişim.

    ![SAML imzalama sertifikası Düzenle](common/edit-certificate.png)

6. İçinde **SAML imzalama sertifikası** bölümünde, kopya **parmak İZİ** ve bilgisayarınıza kaydedin.

    ![Parmak izi değerini kopyalayın](common/copy-thumbprint.png)

7. Üzerinde **tuval kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure Ad tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-canvas-single-sign-on"></a>Tuval çoklu oturum açmayı yapılandırın

1. Farklı bir web tarayıcı penceresinde bir tuval şirketinizin sitesi için bir yönetici olarak oturum açın.

2. Git **kursları \> yönetilen hesapları \> Microsoft**.

    ![Tuval](./media/canvas-lms-tutorial/ic775990.png "tuvali")

3. Sol taraftaki gezinti bölmesinde seçin **kimlik doğrulaması**ve ardından **yeni SAML yapılandırma Ekle**.

    ![Kimlik doğrulaması](./media/canvas-lms-tutorial/ic775991.png "kimlik doğrulaması")

4. Geçerli tümleştirme sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Geçerli tümleştirme](./media/canvas-lms-tutorial/ic775992.png "geçerli tümleştirme")

    a. İçinde **IDP varlık kimliği** metin değerini yapıştırın **Azure Ad tanımlayıcısı** , Azure Portalı'ndan kopyaladığınız.

    b. İçinde **oturum üzerinde URL'sini** metin değerini yapıştırın **oturum açma URL'si** , Azure Portalı'ndan kopyaladığınız.

    c. İçinde **oturum kapatma URL'sini** metin değerini yapıştırın **oturum kapatma URL'si** , Azure Portalı'ndan kopyaladığınız.

    d. İçinde **Değiştir parola bağlantısını** metin değerini yapıştırın **parola URL'yi Değiştir** , Azure Portalı'ndan kopyaladığınız.

    e. İçinde **sertifika parmak izi** metin kutusu, yapıştırma **parmak izi** Azure Portalı'ndan kopyaladığınız sertifika değeri.

    f. Gelen **oturum açma özniteliği** listesinden **Nameıd**.

    g. Gelen **tanımlayıcı biçimi** listesinden **emailAddress**.

    h. Tıklayın **kimlik doğrulama ayarlarını kaydetme**.

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

Bu bölümde, Azure çoklu oturum açma tuvale erişim vererek kullanmak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **tuval**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **tuval**.

    ![Uygulamalar listesini tuval bağlantıdaki](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-canvas-test-user"></a>Tuval test kullanıcısı oluşturma

Tuvale oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunlar tuvale sağlanması gerekir. Söz konusu olduğunda, kullanıcı sağlamayı elle bir görevin tuvaldir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Oturum açın, **tuval** Kiracı.

2. Git **kursları \> yönetilen hesapları \> Microsoft**.

   ![Tuval](./media/canvas-lms-tutorial/ic775990.png "tuvali")

3. **Kullanıcılar**’a tıklayın.

   ![Kullanıcılar](./media/canvas-lms-tutorial/ic775995.png "kullanıcılar")

4. Tıklayın **yeni kullanıcı ekleme**.

   ![Kullanıcılar](./media/canvas-lms-tutorial/ic775996.png "kullanıcılar")

5. Yeni kullanıcı iletişim Sayfa Ekle üzerinde aşağıdaki adımları gerçekleştirin:

   ![Kullanıcı ekleme](./media/canvas-lms-tutorial/ic775997.png "kullanıcı ekleme")

   a. İçinde **tam adı** metin gibi kullanıcı adını girin **BrittaSimon**.

   b. İçinde **e-posta** metin gibi kullanıcının e-posta girin **brittasimon@contoso.com**.

   c. İçinde **oturum açma** metin kutusu, kullanıcının Azure AD e-posta adresi gibi girin **brittasimon@contoso.com**.

   d. Seçin **bu hesap oluşturma hakkında kullanıcıya e-posta**.

   e. Tıklayın **kullanıcı ekleme**.

> [!NOTE]
> Herhangi diğer tuval kullanıcı hesabı oluşturma araçları kullanabilir veya API'leri için AAD kullanıcı hesapları sağlamak tuval tarafından sağlanan.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli tuval kutucuğa tıkladığınızda, size otomatik olarak tuvale SSO'yu ayarlama oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [ SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

