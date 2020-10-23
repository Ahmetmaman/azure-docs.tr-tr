---
title: 'Öğretici: ENVI MMSıS ile tümleştirme Azure Active Directory Microsoft Docs'
description: Azure Active Directory ile ENVI MMNE arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 02/06/2019
ms.author: jeedes
ms.openlocfilehash: 6ccf755a73cafa4b855f602aa18246d710e5e1ff
ms.sourcegitcommit: 9b8425300745ffe8d9b7fbe3c04199550d30e003
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2020
ms.locfileid: "92454020"
---
# <a name="tutorial-azure-active-directory-integration-with-envi-mmis"></a>Öğretici: ENVI MMSıS ile tümleştirme Azure Active Directory

Bu öğreticide, Envi MMSıS 'i Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz.
Envi MMSıS 'yi Azure AD ile tümleştirmek aşağıdaki avantajları sağlar:

* ENVI MMSıS 'e erişimi olan Azure AD 'de denetim yapabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla DSıS MMIN (çoklu oturum açma) için otomatik olarak oturum açmasını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz-Azure portal.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek istiyorsanız, bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini ENVI MMWITH ile yapılandırmak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Bir Azure AD ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü edinebilirsiniz
* ENVI MMSıS çoklu oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

* ENVI MMSıS, **SP** ve **IDP** tarafından başlatılan SSO 'yu destekler

## <a name="adding-envi-mmis-from-the-gallery"></a>Galeriden ENVI MMSıS ekleme

Envi MMSıS 'in tümleştirmesini Azure AD 'ye göre yapılandırmak için, Galeriden Envi MMSıS ' i yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden Envi MMSıS eklemek için aşağıdaki adımları uygulayın:**

1. **[Azure Portal](https://portal.azure.com)** sol gezinti panelinde **Azure Active Directory** simgesine tıklayın.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar** seçeneğini belirleyin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için, iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesine tıklayın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **ENVı mmsıs**yazın, sonuç panelinden **ENVı mmsıs** ' i seçin ve sonra uygulamayı eklemek için **Ekle** düğmesine tıklayın.

     ![ENVI MMSıS sonuçlar listesinde](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma ve test etme

Bu bölümde, Azure AD çoklu oturum açmayı, Envi **Simon**adlı bir test kullanıcısına göre yapılandırma ve test edersiniz.
Çoklu oturum açma 'nın çalışması için, bir Azure AD kullanıcısı ile ilgili Kullanıcı arasındaki bir bağlantı ilişkisinin oluşturulması gerekir.

Azure AD çoklu oturum açma 'yı Envi MMWITH ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını gerçekleştirmeniz gerekir:

1. **[Azure AD çoklu oturum açma özelliğini yapılandırarak](#configure-azure-ad-single-sign-on)** kullanıcılarınızın bu özelliği kullanmasına olanak sağlayın.
2. Bir uygulama tarafında tek Sign-On ayarlarını yapılandırmak için **[ENVI MMSıS 'In çoklu oturum açma yapılandırması](#configure-envi-mmis-single-sign-on)** .
3. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -Britta Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
4. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanarak Britta Simon 'u etkinleştirin.
5. **[ENVı mmsıs test kullanıcısı oluşturma](#create-envi-mmis-test-user)** -kullanıcının Azure AD gösterimine bağlı olan Envi mmon 'Da Britta Simon 'ın bir karşılığı olmalıdır.
6. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[Çoklu oturum açmayı sınayın](#test-single-sign-on)** .

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure portal Azure AD çoklu oturum açma özelliğini etkinleştirirsiniz.

Azure AD çoklu oturum açmayı ENVI MMWITH ile yapılandırmak için aşağıdaki adımları uygulayın:

1. [Azure Portal](https://portal.azure.com/), **Envi mmsıs** uygulama tümleştirmesi sayfasında, **Çoklu oturum açma**' yı seçin.

    ![Çoklu oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Çoklu oturum **açma yöntemi seç** iletişim kutusunda, çoklu oturum açmayı etkinleştirmek için **SAML/WS-Besme** modunu seçin.

    ![Çoklu oturum açma seçme modu](common/select-saml-option.png)

3. **SAML Ile tek Sign-On ayarla** sayfasında, **temel SAML yapılandırması** Iletişim kutusunu açmak için **Düzenle** simgesine tıklayın.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. **Temel SAML yapılandırması** bölümünde, uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız aşağıdaki adımları uygulayın:

    !["Tanımlayıcı", "Yanıtla U R L" ve "Kaydet" düğmesinin vurgulandığı "temel S A M L yapılandırmasını" gösteren ekran görüntüsü.](common/idp-intiated.png)

    a. **Tanımlayıcı** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://www.<CUSTOMER DOMAIN>.com/Account`

    b. **Yanıt URL 'si** metin kutusuna aşağıdaki kalıbı kullanarak bir URL yazın:`https://www.<CUSTOMER DOMAIN>.com/Account/Acs`

5. Uygulamayı **SP** tarafından başlatılan modda yapılandırmak Istiyorsanız **ek URL 'ler ayarla** ' ya tıklayın ve aşağıdaki adımı gerçekleştirin:

    ![Envi MMSıS etki alanı ve URL 'Ler çoklu oturum açma bilgileri](common/metadata-upload-additional-signon.png)

    **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://www.<CUSTOMER DOMAIN>.com/Account`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek tanımlayıcı, yanıt URL 'SI ve oturum açma URL 'SI ile güncelleştirin. Bu değerleri almak için [ENVI Mmsıs istemci destek ekibine](mailto:support@ioscorp.com) başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

6. **SAML Ile tek Sign-On ayarla** sayfasında, **SAML imza sertifikası** bölümünde, **Federasyon meta veri XML** 'sini gereksiniminize göre belirtilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir** ' e tıklayın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

7. **En yeni** URL 'leri ayarlama bölümünde, gereksiniminize göre uygun URL 'leri kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD tanımlayıcısı

    c. Oturum kapatma URL 'SI

### <a name="configure-envi-mmis-single-sign-on"></a>ENVI MMSıS tek Sign-On yapılandırma

1. Farklı bir Web tarayıcısı penceresinde, ENVI MMSıS sitenizde yönetici olarak oturum açın.

2. **Etki alanım** sekmesine tıklayın.

    !["Etki alanım" seçiliyken "Kullanıcı" menüsünü gösteren ekran görüntüsü.](./media/envimmis-tutorial/configure1.png)

3. **Düzenle**’ye tıklayın.

    !["Düzenle" düğmesinin seçili olduğunu gösteren ekran görüntüsü.](./media/envimmis-tutorial/configure2.png)

4. **Uzaktan kimlik doğrulaması kullan** onay kutusunu seçin ve ardından **kimlik doğrulama türü** açılan listesinden **http yeniden yönlendirme** ' yi seçin.

    !["Uzaktan kimlik doğrulaması kullan" işaretli ve "H T T P yeniden yönlendirme" seçiliyken "Ayrıntılar" sekmesini gösteren ekran görüntüsü.](./media/envimmis-tutorial/configure3.png)

5. **Kaynaklar** sekmesini seçin ve ardından **meta verileri karşıya yükle**' ye tıklayın.

    !["Meta verileri karşıya yükle" eylemi seçiliyken "kaynaklar" sekmesini gösteren ekran görüntüsü.](./media/envimmis-tutorial/configure4.png)

6. **Meta verileri karşıya yükle** açılır penceresinde aşağıdaki adımları uygulayın:

    !["Dosya" seçeneği işaretli ve "Dosya Seç" simgesi ve "Tamam" düğmesinin vurgulandığı "meta verileri karşıya yükle" açılan ekran görüntüsü.](./media/envimmis-tutorial/configure5.png)

    a. Açılan **listeden** **Dosya** seçeneğini belirleyin.

    b. **Dosya Seç simgesini**seçerek Azure Portal indirilen meta veri dosyasını karşıya yükleyin.

    c. **Tamam**’a tıklayın.

7. İndirilen meta veri dosyasını karşıya yükledikten sonra, alanlar otomatik olarak doldurulur. **Güncelleştir** 'e tıklayın

    ![Tek Sign-On Kaydet düğmesini yapılandırma](./media/envimmis-tutorial/configure6.png)

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

Bu bölümde, Envi MMSıS 'e erişim vererek Azure çoklu oturum açma özelliğini kullanmak için Britta Simon 'u etkinleştirin.

1. Azure portal **Kurumsal uygulamalar**' ı seçin, **tüm uygulamalar**' ı seçin ve ardından **Envi mmsıs**' yi seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde, **Envi mmsıs**' i seçin.

    ![Uygulamalar listesinde Envi MMSıS bağlantısı](common/all-applications.png)

3. Soldaki menüde **Kullanıcılar ve gruplar**' ı seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. **Kullanıcı Ekle** düğmesine tıklayın, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. **Kullanıcılar ve gruplar** Iletişim kutusunda kullanıcılar listesinden **Britta Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

6. SAML onaylama işlemi içinde herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, Kullanıcı için listeden uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

7. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

### <a name="create-envi-mmis-test-user"></a>ENVI MMSıS test kullanıcısı oluştur

Azure AD kullanıcılarının, ENVI MMO 'da oturum açmasını sağlamak için, Envi MMSıS 'e sağlanması gerekir. Envi MMSıS durumunda, sağlama el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. ENVI MMSıS şirket sitenizde yönetici olarak oturum açın.

2. **Kullanıcı listesi** sekmesine tıklayın.

    !["Kullanıcı listesi" seçiliyken "Kullanıcı" menüsünü gösteren ekran görüntüsü.](./media/envimmis-tutorial/user1.png)

3. **Kullanıcı Ekle** düğmesine tıklayın.

    !["Kullanıcı Ekle" düğmesinin seçili olduğu "kullanıcılar" bölümünü gösteren ekran görüntüsü.](./media/envimmis-tutorial/user2.png)

4. **Kullanıcı Ekle** bölümünde aşağıdaki adımları uygulayın:

    ![Çalışan Ekle](./media/envimmis-tutorial/user3.png)

    a. **Kullanıcı adı** metin kutusuna **brittasıon \@ contoso.com**gibi Britta Simon hesabının kullanıcı adını yazın.
    
    b. **Ilk ad** metin kutusuna, **Britta**gibi brittasıın adını yazın.

    c. **Soyadı** metin kutusunda, **Simon**gibi brittasıon adının soyadını yazın.

    d. Metin kutusunun **başlığında** kullanıcının başlığını girin.
    
    e. **E-posta adresi** metin kutusuna **brittasıon \@ contoso.com**gibi Britta Simon hesabının e-posta adresini yazın.

    f. **SSO Kullanıcı adı** metin kutusuna **brittasıon \@ contoso.com**gibi Britta Simon hesabının kullanıcı adını yazın.

    örneğin: **Kaydet**’e tıklayın.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde Envi MMSıS kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız Envi MMSıS ' de otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](../user-help/my-apps-portal-end-user-access.md).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](./tutorial-list.md)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

- [Azure Active Directory Koşullu erişim nedir?](../conditional-access/overview.md)