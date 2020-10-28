---
title: 'Öğretici: çözümleme GmbH ile Confluence için SAML SSO ile tümleştirme Azure Active Directory | Microsoft Docs'
description: Çözümleme GmbH ile Confluence için Azure Active Directory ve SAML SSO arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
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
ms.openlocfilehash: c8f85c6dd42f1f4505474e03e378c0fe48d70005
ms.sourcegitcommit: 4064234b1b4be79c411ef677569f29ae73e78731
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/28/2020
ms.locfileid: "92896506"
---
# <a name="tutorial-azure-active-directory-integration-with-saml-sso-for-confluence-by-resolution-gmbh"></a>Öğretici: çözümleme GmbH ile Confluence için SAML SSO ile tümleştirme Azure Active Directory

Bu öğreticide, Azure Active Directory (Azure AD) ile çözümleme GmbH tarafından Confluence için SAML SSO 'SU tümleştirmeyi öğreneceksiniz.
Azure AD ile çözümleme GmbH tarafından Confluence için SAML SSO 'yu tümleştirme, aşağıdaki avantajları sağlar:

* Çözümleme GmbH tarafından Confluence için SAML SSO 'ya erişimi olan Azure AD 'de denetim yapabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla çözünürlüklü çözüm GmbH (çoklu oturum açma) ile uygun SAML SSO 'ya otomatik olarak oturum açmasını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz-Azure portal.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek istiyorsanız, bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Çözümleme GmbH tarafından Confluence için SAML SSO ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Bir Azure AD aboneliği. Bir Azure AD ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü edinebilirsiniz
* Çözüm GmbH çoklu oturum açma özellikli aboneliğe göre Confluence için SAML SSO 'SU

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

* Çözümleme GmbH tarafından Confluence için SAML SSO, **SP** ve **IDP** tarafından başlatılan SSO 'yu destekler

## <a name="adding-saml-sso-for-confluence-by-resolution-gmbh-from-the-gallery"></a>Galeriden çözüm GmbH tarafından Confluence için SAML SSO 'SU ekleniyor

SAML SSO 'yu çözüm GmbH tarafından Azure AD 'ye tümleştirme için yapılandırmak üzere, Galeri 'den yönetilen SaaS uygulamaları listenize çözüm GmbH tarafından Confluence için SAML SSO 'SU eklemeniz gerekir.

**Galerinin Resolution GmbH tarafından Confluence için SAML SSO 'SU eklemek için aşağıdaki adımları uygulayın:**

1. **[Azure Portal](https://portal.azure.com)** sol gezinti panelinde **Azure Active Directory** simgesine tıklayın.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar** seçeneğini belirleyin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için, iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesine tıklayın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **çözüm GmbH tarafından Confluence Için SAML SSO** yazın, sonuç panelinden **çözüm GmbH için SAML SSO** 'yu seçin ve sonra uygulamayı eklemek için **Ekle** düğmesine tıklayın.

     ![Sonuç listesinde çözüm GmbH tarafından Confluence için SAML SSO 'SU](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma ve test etme

Bu bölümde, **Britta Simon** adlı bir test kullanıcısına bağlı olarak çözüm GmbH Ile Azure AD çoklu oturum açma 'yı çözümleme için yapılandırın ve test edin.
Çoklu oturum açma için, bir Azure AD kullanıcısı ve çözümleme GmbH ile ilgili SAML SSO 'SU ile ilgili Kullanıcı arasındaki bağlantı ilişkisinin kurulması gerekir.

Çözümleme GmbH tarafından Confluence için SAML SSO 'SU ile Azure AD çoklu oturum açmayı yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma özelliğini yapılandırarak](#configure-azure-ad-single-sign-on)** kullanıcılarınızın bu özelliği kullanmasına olanak sağlayın.
2. Uygulama tarafında tek Sign-On ayarlarını yapılandırmak için, **[Çözümleme GmbH Ile tekli oturum açma IÇIN SAML SSO 'Yu yapılandırın](#configure-saml-sso-for-confluence-by-resolution-gmbh-single-sign-on)** .
3. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -Britta Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
4. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanarak Britta Simon 'u etkinleştirin.
5. **[Çözümleme GmbH test kullanıcısı tarafından Confluence için SAML SSO 'Su oluşturun](#create-saml-sso-for-confluence-by-resolution-gmbh-test-user)** ; bu, kullanıcının Azure AD gösterimine bağlı olan çözüm GmbH tarafından Confluence için SAML SSO 'su Ile Ilgili Britta Simon 'a sahip olmalıdır.
6. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[Çoklu oturum açmayı sınayın](#test-single-sign-on)** .

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure portal Azure AD çoklu oturum açma özelliğini etkinleştirirsiniz.

Çözümleme GmbH tarafından Confluence için SAML SSO 'SU ile Azure AD çoklu oturum açmayı yapılandırmak için aşağıdaki adımları uygulayın:

1. [Azure Portal](https://portal.azure.com/), **Çözümleme GmbH için SAML SSO** uygulama tümleştirmesi sayfasında, **Çoklu oturum açma** ' yı seçin.

    ![Çoklu oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Çoklu oturum **açma yöntemi seç** iletişim kutusunda, çoklu oturum açmayı etkinleştirmek için **SAML/WS-Besme** modunu seçin.

    ![Çoklu oturum açma seçme modu](common/select-saml-option.png)

3. **SAML Ile tek Sign-On ayarlama** sayfasında, **temel SAML yapılandırması** Iletişim kutusunu açmak için **Düzenle** simgesine tıklayın.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. **Temel SAML yapılandırması** bölümünde, uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin:

    !["Tanımlayıcı" ve "Yanıt U R L" metin kutuları vurgulanmış ve "Kaydet" eylemi seçili olan "temel S A M L yapılandırmasını" gösteren ekran görüntüsü.](common/idp-intiated.png)

    a. **Tanımlayıcı** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<server-base-url>/plugins/servlet/samlsso`

    b. **Yanıt URL 'si** metin kutusuna aşağıdaki kalıbı kullanarak bir URL yazın:`https://<server-base-url>/plugins/servlet/samlsso`

    c. Uygulamayı SP tarafından başlatılan modda yapılandırmak istiyorsanız **ek URL 'Ler ayarla** ' ya tıklayın ve aşağıdaki adımı gerçekleştirin:

    ![Çözümleme GmbH etki alanı ve URL 'Ler çoklu oturum açma bilgileri tarafından Confluence için SAML SSO 'SU](common/metadata-upload-additional-signon.png)

    **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<server-base-url>/plugins/servlet/samlsso`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek tanımlayıcı, yanıt URL 'SI ve oturum açma URL 'SI ile güncelleştirin. Bu değerleri almak için [Çözümleme GmbH istemci desteği ekibine yönelik SAML SSO 'su ile](https://www.resolution.de/go/support) iletişim kurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

4. **SAML Ile tek Sign-On ayarlama** sayfasında, **SAML imza sertifikası** bölümünde, **Federasyon meta veri XML** 'sini gereksiniminize göre belirtilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir** ' e tıklayın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

### <a name="configure-saml-sso-for-confluence-by-resolution-gmbh-single-sign-on"></a>Çözümleme GmbH tek Sign-On için SAML SSO yapılandırma

1. Farklı bir Web tarayıcısı penceresinde, yönetici olarak **çözüm GmbH Yönetici portalı Ile Confluence Için SAML SSO** 'ınızla oturum açın.

2. Dişli üzerine gelin ve **eklentilere** tıklayın.
    
    ![Seçili "COG" simgesini ve açılan listeden "Eklentiler" i gösteren ekran görüntüsü.](./media/samlssoconfluence-tutorial/addon1.png)

3. Yönetici erişimi sayfasına yönlendirilirsiniz. Parolayı girin ve **Onayla** düğmesine tıklayın.

    !["Onayla" düğmesinin seçili olduğu "yönetici erişimi" sayfasını gösteren ekran görüntüsü.](./media/samlssoconfluence-tutorial/addon2.png)

4. **Atlasme marketi** sekmesinde **yeni eklentiler bul** ' a tıklayın. 

    !["Yeni eklentiler bul" seçiliyken "Attlassian Market" sekmesini gösteren ekran görüntüsü.](./media/samlssoconfluence-tutorial/addon.png)

5. **Confluence Için SAML çoklu oturum açma (SSO)** araması yapın ve yeni SAML eklentisini yüklemek için **Install** düğmesine tıklayın.

    ![Arama kutusunda ve "Install" düğmesinin seçili olduğu "yeni eklentiler bul" sayfasını "Confluence için bir M L Single Sign on (S S O) ile gösteren ekran görüntüsü.](./media/samlssoconfluence-tutorial/addon7.png)

6. Eklenti yüklemesi başlar. **Kapat** ’a tıklayın.

    !["Yükleme" iletişim kutusunu gösteren ekran görüntüsü.](./media/samlssoconfluence-tutorial/addon8.png)

    !["Yüklü ve hazırlanmaya başlamaya!" gösteren ekran görüntüsü "Kapat" eylemi seçili iletişim kutusu.](./media/samlssoconfluence-tutorial/addon9.png)

7.  **Yönet** 'e tıklayın.

    !["Yönet" düğmesi seçili olarak "Confluence için M L tek oturum açma (S O) uygulama sayfası" gösteren ekran görüntüsü.](./media/samlssoconfluence-tutorial/addon10.png)
    
8. Yeni eklentiyi yapılandırmak için **Yapılandır** ' a tıklayın.

    !["Yapılandır" düğmesinin seçili olduğu "Yönet" sayfasını gösteren ekran görüntüsü.](./media/samlssoconfluence-tutorial/addon11.png)

9. Bu yeni eklenti Ayrıca, **kullanıcılar & güvenlik** sekmesi altında bulunabilir.

    !["Kullanıcılar & güvenliği" sekmesini "S A M L SingleSignOn" seçiliyken gösteren ekran görüntüsü.](./media/samlssoconfluence-tutorial/addon3.png)
    
10. **SAML SingleSignOn eklenti yapılandırması** sayfasında, kimlik sağlayıcısının ayarlarını yapılandırmak Için **Yeni IDP Ekle** düğmesine tıklayın.

    !["Yeni ı d P ekle" düğmesi seçiliyken "S A M L SingleSignOn eklenti yapılandırması" sayfasını gösteren ekran görüntüsü.](./media/samlssoconfluence-tutorial/addon4.png)

11. **SAML kimlik sağlayıcınızı seçin** sayfasında aşağıdaki adımları uygulayın:

    !["I d P türü", "ad" ve "Açıklama" metin kutularından oluşan "S A M L-u kimlik sağlayıcısını seçin" sayfasını gösteren ekran görüntüsü.](./media/samlssoconfluence-tutorial/addon5a.png)
 
    a. **Azure AD** 'Yi IDP türü olarak ayarlayın.
    
    b. Kimlik sağlayıcısının **adını** ekleyin (ör. Azure AD).
    
    c. Kimlik sağlayıcısının **açıklamasını** ekleyin (ör. Azure AD).
    
    d. **İleri** ’ye tıklayın.
    
12. **Kimlik sağlayıcısı yapılandırma** sayfasında **İleri** düğmesine tıklayın.

    !["Ileri" düğmesi seçili "kimlik sağlayıcısı yapılandırması" sayfasını gösteren ekran görüntüsü.](./media/samlssoconfluence-tutorial/addon5b.png)

13. **SAML IDP meta verilerini Içeri aktar** sayfasında, aşağıdaki adımları uygulayın:

    !["Içeri aktarma", "dosya yükleme" ve "Ileri" düğmelerinin seçili olduğu "a M L L L P meta verilerini Içeri aktarma" sayfasını gösteren ekran görüntüsü.](./media/samlssoconfluence-tutorial/addon5c.png)

    a. **Dosya Yükle** düğmesine tıklayın ve 5. adımda Indirdiğiniz meta veri xml dosyasını seçin.

    b. **Al** düğmesine tıklayın.
    
    c. İçeri aktarma başarılı olana kadar kısa bir süre bekleyin.
    
    d. **İleri** düğmesine tıklayın.
    
14. **Kullanıcı kimliği özniteliği ve dönüştürme** sayfasında, **İleri** düğmesine tıklayın.

    !["Ileri" düğmesi seçili "Kullanıcı KIMLIĞI özniteliği ve dönüştürme" sayfasını gösteren ekran görüntüsü.](./media/samlssoconfluence-tutorial/addon5d.png)
    
15. **Kullanıcı oluşturma ve güncelleştirme** sayfasında, Ayarları Kaydet ' in **yanındaki & kaydet** ' e tıklayın.   
    
    !["Kullanıcı oluşturma ve güncelleştirme" sayfasını "& sonrakini Kaydet" düğmesinin seçili olduğunu gösteren ekran görüntüsü.](./media/samlssoconfluence-tutorial/addon6a.png)
    
16. **Ayarlarınızı test** etme sayfasında, şimdi için Kullanıcı testini atlamak üzere **testi atla & el ile yapılandır** ' ı tıklatın. Bu, sonraki bölümde gerçekleştirilecek ve Azure portal bazı ayarlar gerektirir. 
    
    !["Test & el ile yapılandırmayı atla" düğmesinin seçili olduğu "ayarlarınızı test etme" sayfasını gösteren ekran görüntüsü.](./media/samlssoconfluence-tutorial/addon6b.png)
    
17. Görüntülenen iletişim kutusunda okuma **, test anlamına geliyor...** , **Tamam** ' a tıklayın.
    
    ![Tek Sign-On yapılandırma](./media/samlssoconfluence-tutorial/addon6c.png)

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

Bu bölümde, çözüm GmbH tarafından Confluence için SAML SSO 'ya erişim vererek Azure çoklu oturum açma özelliğini kullanmak için Britta Simon 'u etkinleştirin.

1. Azure portal **Kurumsal uygulamalar** ' ı seçin, **tüm uygulamalar** ' ı seçin ve ardından **çözüm GmbH tarafından Confluence için SAML SSO** ' yı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde, **Çözümleme GmbH Ile Confluence Için SAML SSO** 'yu yazın ve seçin.

    ![Uygulamalar listesinde çözüm GmbH bağlantısı tarafından Confluence için SAML SSO 'SU](common/all-applications.png)

3. Soldaki menüde **Kullanıcılar ve gruplar** ' ı seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. **Kullanıcı Ekle** düğmesine tıklayın, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. **Kullanıcılar ve gruplar** Iletişim kutusunda kullanıcılar listesinde **Britta Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

6. SAML onaylama işlemi içinde herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, listeden Kullanıcı için uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

7. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

### <a name="create-saml-sso-for-confluence-by-resolution-gmbh-test-user"></a>Çözüm GmbH test kullanıcısı tarafından Confluence için SAML SSO oluşturma

Azure AD kullanıcılarının çözüm GmbH tarafından Confluence için SAML SSO 'da oturum açmasını sağlamak için, çözüm GmbH tarafından Confluence için SAML SSO 'ya sağlanması gerekir.  
Çözüm GmbH tarafından Confluence için SAML SSO 'da, sağlama el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Çözümleme GmbH Şirket sitesini yönetici olarak çözümlemek için SAML SSO 'sitenizde oturum açın.

2. Dişli 'ye gelin ve **Kullanıcı yönetimine** tıklayın.

    !["COG" simgesinin seçili olduğunu gösteren ekran görüntüsü ve menüden "Kullanıcı Yönetimi" seçilidir.](./media/samlssoconfluence-tutorial/user1.png) 

3. Kullanıcılar bölümünde, **Kullanıcı Ekle** sekmesini tıklatın. **"Kullanıcı Ekle"** iletişim sayfasında, aşağıdaki adımları uygulayın:

    ![Çalışan Ekle](./media/samlssoconfluence-tutorial/user2.png) 

    a. Kullanıcı **adı** metin kutusuna, Britta Simon gibi kullanıcının e-postasını yazın.

    b. **Tam ad** metin kutusuna, Britta Simon gibi kullanıcının tam adını yazın.

    c. **E-posta** metin kutusuna, gibi kullanıcının e-posta adresini yazın Brittasimon@contoso.com .

    d. **Parola** metin kutusuna Britta Simon parolasını yazın.

    e. Parolayı **Onayla** ' ya tıklayarak parolayı yeniden girin.
    
    f. **Ekle** düğmesine tıklayın.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde çözümlenme çözümü için SAML SSO GmbH kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız çözüm GmbH tarafından Confluence için SAML SSO 'ya otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](../user-help/my-apps-portal-end-user-access.md).

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](./tutorial-list.md)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

- [Azure Active Directory Koşullu erişim nedir?](../conditional-access/overview.md)