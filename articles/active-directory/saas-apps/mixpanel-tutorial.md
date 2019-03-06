---
title: "Öğretici: Azure Active Directory Tümleştirmesi ile Mixpanel'a | Microsoft Docs"
description: Azure Active Directory ve Mixpanel'a arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: a2df26ef-d441-44ac-a9f3-b37bf9709bcb
ms.service: Azure-Active-Directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/28/2019
ms.author: jeedes
ms.openlocfilehash: 6e8f9f571a255fb9c210791848aa86196a106175
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57441789"
---
# <a name="tutorial-azure-active-directory-integration-with-mixpanel"></a>Öğretici: Mixpanel'a ile Azure Active Directory Tümleştirme

Bu öğreticide, Mixpanel'a Azure Active Directory (Azure AD) ile tümleştirmeyi öğrenin.
Azure AD ile Mixpanel'a tümleştirme ile aşağıdaki avantajları sağlar:

* Mixpanel'a erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak Mixpanel'a gönderdiğiniz (çoklu oturum açma) ile Azure AD hesaplarına oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Mixpanel'a yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Abonelik Mixpanel'a çoklu oturum açma etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Mixpanel'a destekler **SP** tarafından başlatılan

## <a name="adding-mixpanel-from-the-gallery"></a>Galeriden Mixpanel'a ekleme

Azure AD'de Mixpanel'a tümleştirmesini yapılandırmak için Mixpanel'a Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Mixpanel'a eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Mixpanel'a**seçin **Mixpanel'a** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde Mixpanel'a](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Mixpanel'a adlı bir test kullanıcı tabanlı test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısının Mixpanel'a ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Mixpanel'a ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Mixpanel'a çoklu oturum açmayı yapılandırma](#configure-mixpanel-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Mixpanel'a test kullanıcısı oluşturma](#create-mixpanel-test-user)**  - kullanıcı Azure AD gösterimini bağlı Mixpanel'a Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma ile Mixpanel'a yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Mixpanel'a** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Mixpanel'a etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-signonurl.png)

    İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın:  `https://mixpanel.com/login/`

    > [!NOTE]
    > Lütfen adresindeki kayıt [ https://mixpanel.com/register/ ](https://mixpanel.com/register/) , oturum açma kimlik bilgileri ve iletişim kurmak için [Mixpanel'a Destek ekibine](mailto:support@mixpanel.com) kiracınızın SSO ayarlarını etkinleştirmek için. Ayrıca, üzerinde oturum URL değeri gerekirse Mixpanel'a destek ekibinizden alabilirsiniz. 

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. Üzerinde **Mixpanel'a kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-mixpanel-single-sign-on"></a>Mixpanel'a tek oturum açmayı yapılandırın

1. Farklı bir tarayıcı penceresinde, Mixpanel'a uygulamanıza yönetici olarak oturum.

2. Sayfanın altındaki üzerinde bırakmaya tıklayın **dişli** sol üst köşedeki bir simge. 
   
    ![Mixpanel'a çoklu oturum açma](./media/mixpanel-tutorial/tutorial_mixpanel_06.png) 

3. Tıklayın **erişim güvenlik** sekmesine ve ardından **ayarlarını değiştir**.
   
    ![Mixpanel'a ayarları](./media/mixpanel-tutorial/tutorial_mixpanel_08.png) 

4. Üzerinde **sertifikanızı değiştirmek** iletişim sayfasına tıklayın **dosya** indirilen sertifikanızı karşıya yükleyin ve ardından **sonraki**.
   
    ![Mixpanel'a ayarları](./media/mixpanel-tutorial/tutorial_mixpanel_09.png) 

5.  Kimlik doğrulama URL'si metin kutusuna **kimlik doğrulaması URL'nizi değiştirmek** iletişim sayfasında, değerini yapıştırın **oturum açma URL'si** Azure portaldan kopyaladığınız ve ardından **sonraki**.
   
    ![Mixpanel'a ayarları](./media/mixpanel-tutorial/tutorial_mixpanel_10.png) 

6. **Bitti**’ye tıklayın.

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

Bu bölümde, Mixpanel'a gönderdiğiniz erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Mixpanel'a**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Mixpanel'a**.

    ![Uygulamalar listesini Mixpanel'a bağlantıdaki](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-mixpanel-test-user"></a>Mixpanel'a test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon Mixpanel'da adlı bir kullanıcı oluşturmaktır. 

1. Mixpanel'a şirketinizin sitesi için bir yönetici olarak oturum açın.

2. Sayfanın sonuna üzerinde çok az açmak için sol üst köşedeki dişli düğmesine tıklayın **ayarları** penceresi.

3. Tıklayın **takım** sekmesi.

4. İçinde **takım üyesi** metin Azure'da Britta'nın e-posta adresini yazın.
   
    ![Mixpanel'a ayarları](./media/mixpanel-tutorial/tutorial_mixpanel_11.png) 

5. Tıklayın **davet**. 

> [!Note]
> Kullanıcı profili ayarlamak için bir e-posta alırsınız.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Mixpanel'a kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama Mixpanel'a için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [ SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

