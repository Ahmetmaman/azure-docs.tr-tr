---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile barındırılan Grafit | Microsoft Docs'
description: Azure Active Directory ve barındırılan Grafit arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: a1ac4d7f-d079-4f3c-b6da-0f520d427ceb
ms.service: Azure-Active-Directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/15/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: c44b89b66c1908c00e075606d6b0201bd3ea6af6
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/26/2019
ms.locfileid: "56872956"
---
# <a name="tutorial-azure-active-directory-integration-with-hosted-graphite"></a>Öğretici: Barındırılan Grafit ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile barındırılan Grafit tümleştirme konusunda bilgi edinin.
Azure AD ile barındırılan Grafit tümleştirme ile aşağıdaki avantajları sağlar:

* Barındırılan Grafit erişimi, Azure AD'de kontrol edebilirsiniz.
* Azure AD hesaplarına otomatik olarak (çoklu oturum açma) barındırılan Grafit için oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile barındırılan Grafit yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Barındırılan Grafit tek oturum açma etkin aboneliği

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Grafit destekler barındırılan **SP ve IDP** tarafından başlatılan
* Grafit destekler barındırılan **zamanında** kullanıcı sağlama

## <a name="adding-hosted-graphite-from-the-gallery"></a>Galeriden barındırılan Grafit ekleme

Azure AD'de barındırılan Grafit tümleştirmesini yapılandırmak için barındırılan Grafit Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden barındırılan Grafit eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **barındırılan Grafit**seçin **barındırılan Grafit** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde barındırılan Grafit](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma adlı bir test kullanıcı tabanlı Grafit barındırılan test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ile barındırılan Grafit ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma barındırılan Grafit ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Barındırılan Grafit çoklu oturum açmayı yapılandırma](#configure-hosted-graphite-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Barındırılan Grafit test kullanıcısı oluşturma](#create-hosted-graphite-test-user)**  - kullanıcı Azure AD gösterimini bağlı barındırılan Grafit Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma ile barındırılan Grafit yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **barındırılan Grafit** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** uygulamada yapılandırmak isterseniz, bölümü **IDP** başlatılan modu, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açma bilgileri barındırılan Grafit etki alanı ve URL'ler](common/idp-intiated.png)

    a. İçinde **tanımlayıcı** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://www.hostedgraphite.com/metadata/<user id>`

    b. İçinde **yanıt URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://www.hostedgraphite.com/complete/saml/<user id>`

5. Tıklayın **ek URL'lerini ayarlayın** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![Çoklu oturum açma bilgileri barındırılan Grafit etki alanı ve URL'ler](common/metadata-upload-additional-signon.png)

    İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın:  `https://www.hostedgraphite.com/login/saml/<user id>/`

    > [!NOTE]
    > Bunlar gerçek değerleri olmadığına dikkat edin. Bu değerler gerçek tanımlayıcısı, yanıt URL'si ve oturum temel URL ile güncelleştirmeniz gerekiyor. Bu değerleri almak için erişim gidebilirsiniz SAML Kurulumu uygulama tarafından veya kişi -> [barındırılan Grafit Destek ekibine](mailto:help@hostedgraphite.com).

6. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

7. Üzerinde **barındırılan Grafit kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure Ad tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-hosted-graphite-single-sign-on"></a>Barındırılan Grafit çoklu oturum açmayı yapılandırın

1. Barındırılan Grafit kiracınıza yönetici olarak oturum.

2. Git **SAML Kurulumu sayfasına** Kenar çubuğunda (**erişim -> SAML Kurulumu**).

    ![Çoklu oturum açma üzerinde uygulama tarafı yapılandırma](./media/hostedgraphite-tutorial/tutorial_hostedgraphite_000.png)

3. Onayı bu URL'leri aynı işlemi yapılandırmanızı **temel SAML yapılandırma** Azure portal'ın bölümü.

    ![Çoklu oturum açma üzerinde uygulama tarafı yapılandırma](./media/hostedgraphite-tutorial/tutorial_hostedgraphite_001.png)

4. İçinde **varlık veya sertifikayı verenin kimliği** ve **SSO oturum açma URL'si** metin kutuları, değerini yapıştırın **Azure Ad tanımlayıcısı** ve **oturum açma URL'si** kopyaladığınız Azure portalından.

    ![Çoklu oturum açma üzerinde uygulama tarafı yapılandırma](./media/hostedgraphite-tutorial/tutorial_hostedgraphite_002.png)

5. Seçin **salt okunur** olarak **varsayılan kullanıcı rolü**.

    ![Çoklu oturum açma üzerinde uygulama tarafı yapılandırma](./media/hostedgraphite-tutorial/tutorial_hostedgraphite_004.png)

6. Base-64 kodlanmış sertifikanızı Azure portalından indirdiğiniz Not Defteri'nde açın, içeriğini, panoya kopyalayın ve ardından ona yapıştırın **X.509 sertifikası** metin.

    ![Çoklu oturum açma üzerinde uygulama tarafı yapılandırma](./media/hostedgraphite-tutorial/tutorial_hostedgraphite_005.png)

7. Tıklayın **Kaydet** düğmesi.

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

Bu bölümde, Azure çoklu oturum açma kullanmak için barındırılan Grafit erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **barındırılan Grafit**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **barındırılan Grafit**.

    ![Uygulamalar listesini barındırılan Grafit bağlantıdaki](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-hosted-graphite-test-user"></a>Barındırılan Grafit test kullanıcısı oluşturma

Bu bölümde, Britta Simon adlı bir kullanıcı barındırılan Grafit oluşturulur. Barındırılan Grafit just-ın-time kullanıcı hazırlama, varsayılan olarak etkin olduğu destekler. Bu bölümde, hiçbir eylem öğesini yoktur. Bir kullanıcı barındırılan Grafit içinde zaten mevcut değilse yeni bir kimlik doğrulamasından sonra oluşturulur.

> [!NOTE]
> Bir kullanıcı el ile oluşturmanız gerekiyorsa, barındırılan Grafit destek ekibi ile iletişime geçmeniz <mailto:help@hostedgraphite.com>.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli barındırılan Grafit kutucuğa tıkladığınızda, size otomatik olarak barındırılan SSO'yu ayarlama Grafit için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

