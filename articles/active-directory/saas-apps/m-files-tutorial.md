---
title: 'Öğretici: a-Files ile tümleştirme Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve d dosyaları arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 02/19/2019
ms.author: jeedes
ms.openlocfilehash: 552e5c2411889ee7f07265eb1cc35d98aed4d7c1
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "99574187"
---
# <a name="tutorial-azure-active-directory-integration-with-m-files"></a>Öğretici: d-dosyalarla Azure Active Directory tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile e-dosyaları tümleştirmeyi öğreneceksiniz.
P dosyalarını Azure AD ile tümleştirmek aşağıdaki avantajları sağlar:

* Azure AD 'de, e-dosyalara erişimi olan ' i denetleyebilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla, e-dosyalarda (çoklu oturum açma) otomatik olarak oturum açmasını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz-Azure portal.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek istiyorsanız, bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesini e-dosyalarla birlikte yapılandırmak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Bir Azure AD ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü edinebilirsiniz
* A-dosyalar çoklu oturum açma etkin abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

* D-dosyalar **SP** tarafından başlatılan SSO 'yu destekler

## <a name="adding-m-files-from-the-gallery"></a>Galeriden z dosyaları ekleme

E-dosyaların Azure AD ile tümleştirilmesini yapılandırmak için Galeriden, yönetilen SaaS uygulamaları listenize e-dosyaları eklemeniz gerekir.

**Galeriden z dosyaları eklemek için aşağıdaki adımları uygulayın:**

1. **[Azure Portal](https://portal.azure.com)** sol gezinti panelinde **Azure Active Directory** simgesine tıklayın.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar** seçeneğini belirleyin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için, iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesine tıklayın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna, **a-Files** yazın, sonuç panelinden **d-Files** ' ı seçin, sonra da uygulamayı eklemek için düğme **Ekle** ' ye tıklayın.

     ![D-sonuçlar listesindeki dosyalar](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma ve test etme

Bu bölümde, Azure AD çoklu oturum açmayı, **Britta Simon** adlı bir test kullanıcısına göre, a-Files ile yapılandırıp test edersiniz.
Çoklu oturum açma 'nın çalışması için, bir Azure AD kullanıcısı ve a-Files içindeki ilgili Kullanıcı arasındaki bağlantı ilişkisinin kurulması gerekir.

Azure AD çoklu oturum açma 'yı e-dosyalarla yapılandırmak ve test etmek için aşağıdaki yapı taşlarını gerçekleştirmeniz gerekir:

1. **[Azure AD çoklu oturum açma özelliğini yapılandırarak](#configure-azure-ad-single-sign-on)** kullanıcılarınızın bu özelliği kullanmasına olanak sağlayın.
2. **[A-Files çoklu oturum açmayı yapılandırma](#configure-m-files-single-sign-on)** -uygulama tarafında tek Sign-On ayarlarını yapılandırmak için.
3. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -Britta Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
4. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanarak Britta Simon 'u etkinleştirin.
5. E- **[Files test kullanıcısı oluşturun](#create-m-files-test-user)** -kullanıcının Azure AD gösterimine bağlı olan, d-dosyalarında Britta Simon 'a sahip olmak için.
6. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[Çoklu oturum açmayı sınayın](#test-single-sign-on)** .

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure portal Azure AD çoklu oturum açma özelliğini etkinleştirirsiniz.

Azure AD çoklu oturum açmayı, e-dosyalarla birlikte yapılandırmak için aşağıdaki adımları uygulayın:

1. [Azure Portal](https://portal.azure.com/), **a-Files** uygulama tümleştirmesi sayfasında, **Çoklu oturum açma**' yı seçin.

    ![Çoklu oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Çoklu oturum **açma yöntemi seç** iletişim kutusunda, çoklu oturum açmayı etkinleştirmek için **SAML/WS-Besme** modunu seçin.

    ![Çoklu oturum açma seçme modu](common/select-saml-option.png)

3. **SAML Ile tek Sign-On ayarlama** sayfasında, **temel SAML yapılandırması** Iletişim kutusunu açmak için **Düzenle** simgesine tıklayın.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. **Temel SAML yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![A-dosya etki alanı ve URL 'Ler çoklu oturum açma bilgileri](common/sp-identifier.png)

    a. **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<tenantname>.cloudvault.m-files.com/authentication/MFiles.AuthenticationProviders.Core/sso`

    b. **Tanımlayıcı (VARLıK kimliği)** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<tenantname>.cloudvault.m-files.com`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri, gerçek oturum açma URL 'SI ve tanımlayıcısı ile güncelleştirin. Bu değerleri almak için, [e-Files istemci destek ekibine](mailto:support@m-files.com) başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

5. **SAML Ile tek Sign-On ayarlama** sayfasında, **SAML imza sertifikası** bölümünde, **Federasyon meta veri XML** 'sini gereksiniminize göre belirtilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir** ' e tıklayın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

6. **K-Files ayarla** bölümünde, uygun URL 'leri gereksiniminize göre kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD tanımlayıcısı

    c. Oturum kapatma URL 'SI

### <a name="configure-m-files-single-sign-on"></a>Tek tek dosyaları yapılandırın Sign-On

1. Uygulamanız için yapılandırılmış SSO 'yu almak için, [e-Files destek ekibine](mailto:support@m-files.com) başvurun ve Indirilen meta verileri sağlayın.
   
    >[!NOTE]
    >D-dosya masaüstü uygulamanız için SSO 'yu yapılandırmak istiyorsanız sonraki adımları izleyin. Yalnızca e-Files Web sürümü için SSO yapılandırmak istiyorsanız ek adım gerekmez.  

1. Azure AD ile SSO 'yu etkinleştirmek üzere, e-dosya masaüstü uygulamasını yapılandırmak için sonraki adımları izleyin. E-dosyaları indirmek için, ı [-Files indirme](https://www.m-files.com/customers/product-downloads/download-update-links/) sayfasına gidin.

1. **D-Files masaüstü ayarları** penceresini açın. Ardından **Ekle**' ye tıklayın.
   
    ![Ekran görüntüsü, Ekle ' yi seçebileceğiniz, a-Files masaüstü ayarlarını gösterir.](./media/m-files-tutorial/tutorial_m_files_10.png)

1. **Belge Kasası bağlantısı özellikleri** penceresinde, aşağıdaki adımları uygulayın:
   
    ![Ekran görüntüsü, açıklanan değerleri girebileceğiniz belge Kasası bağlantı özelliklerini gösterir.](./media/m-files-tutorial/tutorial_m_files_11.png)  

    Sunucu bölümünün altında, aşağıdaki gibi değerleri:  

    a. **Ad** için yazın `<tenant-name>.cloudvault.m-files.com` . 
 
    b. **Bağlantı noktası numarası** için **4466** yazın. 

    c. **Protokol** için **https**' yi seçin. 

    d. **Kimlik doğrulama** alanında, **belirli Windows kullanıcısı**' nı seçin. Ardından, sizden bir imzalama sayfası istenir. Azure AD kimlik bilgilerinizi ekleyin. 

    e. **Sunucu üzerindeki kasa** için, sunucuda ilgili kasayı seçin.
 
    f. **Tamam**'a tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Azure portal Britta Simon adlı bir test kullanıcısı oluşturmaktır.

1. Azure portal, sol bölmedeki **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.

    !["Kullanıcılar ve gruplar" ve "tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.

    ![Yeni Kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı Özellikleri ' nde aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanına **Brittasıon** girin.
  
    b. **Kullanıcı adı** alanına **\@ bricompansıon yourcompanydomain. Extension** yazın  
    Örneğin, BrittaSimon@contoso.com

    c. **Parolayı göster** onay kutusunu seçin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, e-dosyalara erişim vererek Azure çoklu oturum açma özelliğini kullanmak için Britta Simon özelliğini etkinleştirirsiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin, **tüm uygulamalar**' ı seçin ve ardından, **e-dosyalar**' ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde, ı **-Files**' ı seçin.

    ![Uygulamalar listesindeki a-Files bağlantısı](common/all-applications.png)

3. Soldaki menüde **Kullanıcılar ve gruplar**' ı seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. **Kullanıcı Ekle** düğmesine tıklayın, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. **Kullanıcılar ve gruplar** Iletişim kutusunda kullanıcılar listesinde **Britta Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

6. SAML onaylama işlemi içinde herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, listeden Kullanıcı için uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

7. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

### <a name="create-m-files-test-user"></a>A-Files test kullanıcısı oluştur

Bu bölümün amacı, a-dosyalarında Britta Simon adlı bir Kullanıcı oluşturmaktır. E-dosyalarda kullanıcıları eklemek için,  [e-Files destek](mailto:support@m-files.com) ekibiyle çalışın.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde, n-Files kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız ı dosyalarında otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](../user-help/my-apps-portal-end-user-access.md).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](./tutorial-list.md)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

- [Azure Active Directory Koşullu erişim nedir?](../conditional-access/overview.md)