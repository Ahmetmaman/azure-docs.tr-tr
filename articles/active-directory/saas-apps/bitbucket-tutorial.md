---
title: 'Öğretici: GmbH çözünürlüğün Bitbucket için SAML SSO ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Çoklu oturum açma SAML SSO için Bitbucket ile Azure Active Directory arasında GmbH çözünürlüğün yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: fc947df1-f24e-43ae-9a34-518293583d69
ms.service: Azure-Active-Directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/27/2018
ms.author: jeedes
ms.openlocfilehash: 14811ef9da1a50ba3b0ec0363cede1988d386e78
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54818158"
---
# <a name="tutorial-azure-active-directory-integration-with-saml-sso-for-bitbucket-by-resolution-gmbh"></a>Öğretici: GmbH çözünürlüğün Bitbucket için SAML SSO ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile GmbH çözünürlüğün Bitbucket için SAML SSO tümleştirme konusunda bilgi edinin.
Bitbucket için SAML SSO çözünürlüğün GmbH Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* SAML SSO Bitbucket için GmbH çözünürlüğün erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak Bitbucket için SAML SSO (çoklu oturum açma) GmbH çözünürlüğüyle kendi Azure AD hesapları ile oturum, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi GmbH çözünürlüğüyle Bitbucket için SAML SSO ile yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Abonelik Bitbucket çözünürlüğün GmbH çoklu oturum açma için SAML SSO etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* SAML SSO için Bitbucket GmbH destekler çözünürlüğün **SP ve IDP** tarafından başlatılan
* SAML SSO için Bitbucket GmbH destekler çözünürlüğün **zamanında** kullanıcı sağlama


## <a name="adding-saml-sso-for-bitbucket-by-resolution-gmbh-from-the-gallery"></a>Bitbucket için SAML SSO GmbH çözünürlüğüyle galeri ekleme

Bitbucket için SAML SSO, Azure AD'de tümleştirmesini GmbH çözünürlüğün yapılandırmak için Bitbucket için SAML SSO GmbH çözünürlüğüyle Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Bitbucket için SAML SSO GmbH çözünürlüğüyle Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **GmbH çözünürlüğün Bitbucket için SAML SSO**seçin **GmbH çözünürlüğün Bitbucket için SAML SSO** sonucu panelinden ardından **Ekle** eklemek için Ekle düğmesine uygulama.

     ![Sonuç listesinde GmbH çözünürlüğün Bitbucket için SAML SSO](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Bitbucket GmbH çözünürlüğün adlı bir test kullanıcı tabanlı için Azure AD çoklu oturum açma SAML SSO ile test etme **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ve SAML SSO Bitbucket için ilgili kullanıcı çözünürlüğün arasında bir bağlantı ilişki GmbH kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma SAML SSO için Bitbucket ile GmbH çözünürlüğün sınamak için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bitbucket için SAML SSO çözünürlüğüyle GmbH çoklu oturum açmayı yapılandırma](#configure-saml-sso-for-bitbucket-by-resolution-gmbh-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[SAML SSO için çözüm GmbH test kullanıcısı tarafından Bitbucket oluşturma](#create-saml-sso-for-bitbucket-by-resolution-gmbh-test-user)**  - kullanıcı Azure AD gösterimini bağlı GmbH çözünürlüğün Bitbucket için SAML SSO içinde bir karşılığı Britta simon'un sağlamak için.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma SAML SSO için Bitbucket ile GmbH çözünürlüğüyle yapılandırmak için aşağıdaki adımları uygulayın:

1. İçinde [Azure portalında](https://portal.azure.com/), **GmbH çözünürlüğün Bitbucket için SAML SSO** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** başlatılan modu:

    ![Oturum açma bilgileri tek bir SAML SSO için Bitbucket çözünürlüğün GmbH etki alanı ve URL'ler](common/idp-intiated.png)

    a. İçinde **tanımlayıcı** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<server-base-url>/plugins/servlet/samlsso`

    b. İçinde **yanıt URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<server-base-url>/plugins/servlet/samlsso`

    c. Tıklayın **ek URL'lerini ayarlayın** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![Oturum açma bilgileri tek bir SAML SSO için Bitbucket çözünürlüğün GmbH etki alanı ve URL'ler](common/metadata-upload-additional-signon.png)

    İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın:  `https://<server-base-url>/plugins/servlet/samlsso`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. İlgili kişi [SAML SSO GmbH istemci çözünürlüğün Bitbucket için Destek ekibine](https://marketplace.atlassian.com/apps/1217045/saml-single-sign-on-sso-bitbucket?hosting=server&tab=support) bu değerleri almak için. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **Federasyon meta veri XML**  bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

### <a name="configure-saml-sso-for-bitbucket-by-resolution-gmbh-single-sign-on"></a>Bitbucket için SAML SSO çözünürlüğüyle GmbH çoklu oturum açmayı yapılandırın

1. SAML SSO için çözüm GmbH şirket site tarafından Bitbucket için yönetici olarak oturum.

2. Ana araç çubuğunun sağ tarafında tıklayın **ayarları**.

3. HESAPLAR bölümüne gidin, tıklayarak **SAML SingleSignOn** menü çubuğu üzerinde.

    ![Samlsingle](./media/bitbucket-tutorial/tutorial_bitbucket_samlsingle.png)

4. Üzerinde **SAML SIngleSignOn eklentisi yapılandırma sayfası**, tıklayın **IDP ekleme**. 

    ![IDP Ekle](./media/bitbucket-tutorial/tutorial_bitbucket_addidp.png)

5. Üzerinde **SAML kimlik sağlayıcınızı seçin** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Kimlik sağlayıcısı](./media/bitbucket-tutorial/tutorial_bitbucket_identityprovider.png)

    a. Seçin **IDP türü** olarak **AZURE AD'ye**.

    b. İçinde **adı** metin kutusuna bir ad yazın.

    c. İçinde **açıklama** metin kutusuna bir açıklama yazın.

    d. **İleri**’ye tıklayın.

6. Üzerinde **kimlik sağlayıcı yapılandırma sayfası**, tıklayın **sonraki**.

    ![Kimlik yapılandırma](./media/bitbucket-tutorial/tutorial_bitbucket_identityconfig.png)

7.  Üzerinde **SAML IDP meta verileri içeri aktarma** sayfasında, **yük dosyası** yüklenecek **meta veri XML** Azure portalından indirdiğiniz dosyası.

    ![İdpmetadata](./media/bitbucket-tutorial/tutorial_bitbucket_idpmetadata.png)
    
8. **İleri**’ye tıklayın.

9. **Ayarları kaydet**’e tıklayın.

    ![Kaydetme](./media/bitbucket-tutorial/tutorial_bitbucket_save.png)

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

Bu bölümde, Azure çoklu oturum açma GmbH çözünürlüğüyle Bitbucket için SAML SSO için erişim vererek kullanmak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **GmbH çözünürlüğün Bitbucket için SAML SSO**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde yazın ve **GmbH çözünürlüğün Bitbucket için SAML SSO**.

    ![SAML SSO uygulamaları listesinde çözümleme GmbH bağlantısıyla Bitbucket için](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-saml-sso-for-bitbucket-by-resolution-gmbh-test-user"></a>SAML SSO çözümleme GmbH test kullanıcısı tarafından Bitbucket için oluşturma

Bu bölümün amacı, Britta Simon SAML SSO için Bitbucket GmbH çözünürlüğüyle adlı bir kullanıcı oluşturmaktır. SAML SSO GmbH çözünürlüğün Bitbucket, just-ın-time sağlamayı destekler ve ayrıca kullanıcılar el ile oluşturulabilir başvurun [SAML SSO GmbH istemci çözünürlüğün Bitbucket için Destek ekibine](https://marketplace.atlassian.com/plugins/com.resolution.atlasplugins.samlsso-bitbucket/server/support) ihtiyacınıza göre.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Çözüm GmbH kutucuk erişim Paneli'nde tarafından SAML SSO için Bitbucket'ye tıkladığınızda, otomatik olarak Bitbucket için SAML SSO için SSO'yu ayarlama çözümleme GmbH tarafından oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [ SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

