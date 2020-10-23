---
title: "Öğretici: çözüm GmbH göre Bitbucket için SAML SSO 'SU ile tümleştirme Azure Active Directory | Microsoft Docs"
description: GmbH ile Bitbucket için SAML SSO ve Azure Active Directory arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 12/27/2018
ms.author: jeedes
ms.openlocfilehash: ffd93016a2cbde3aa2a5844cfdb72e4c0cc8b623
ms.sourcegitcommit: 9b8425300745ffe8d9b7fbe3c04199550d30e003
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2020
ms.locfileid: "92457121"
---
# <a name="tutorial-azure-active-directory-integration-with-saml-sso-for-bitbucket-by-resolution-gmbh"></a>Öğretici: çözüm GmbH göre Bitbucket için SAML SSO ile tümleştirme Azure Active Directory

Bu öğreticide, Azure Active Directory (Azure AD) ile çözümleme GmbH ile Bitbucket için SAML SSO 'SU tümleştirmeyi öğreneceksiniz.
Azure AD ile, GmbH tarafından sağlanan Bitbucket için SAML SSO 'SU ile tümleştirme, aşağıdaki avantajları sağlar:

* Azure AD 'de, GmbH göre Bitbucket için SAML SSO 'ya erişimi olan bir denetim yapabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla çözünürlüklü GmbH (çoklu oturum açma) ile Bitbucket için SAML SSO 'ya otomatik olarak oturum açmasını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz-Azure portal.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek istiyorsanız, bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Ön koşullar

GmbH tarafından Bitbucket için SAML SSO 'SU ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Bir Azure AD aboneliği. Bir Azure AD ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü edinebilirsiniz
* Çözünürlükte GmbH çoklu oturum açma özellikli aboneliğe göre Bitbucket için SAML SSO 'SU

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

* Resolution GmbH tarafından Bitbucket için SAML SSO **, SP ve ıDP** tarafından başlatılan SSO 'yu destekler
* Resolution GmbH tarafından desteklenen Bitbucket için SAML SSO **, tam zamanında** Kullanıcı sağlamayı destekler


## <a name="adding-saml-sso-for-bitbucket-by-resolution-gmbh-from-the-gallery"></a>Galeriden çözüm GmbH göre Bitbucket için SAML SSO 'SU ekleniyor

GmbH ile Bitbucket için SAML SSO 'yu Azure AD 'ye tümleştirme özelliğini yapılandırmak için, Galeriden, yönetim SaaS uygulamaları listenize çözünürlüklü Bitbucket için SAML SSO 'SU ' nu çözüm GmbH ile eklemeniz gerekir.

**Galeriden Resolution GmbH tarafından Bitbucket için SAML SSO 'SU eklemek için aşağıdaki adımları uygulayın:**

1. **[Azure Portal](https://portal.azure.com)** sol gezinti panelinde **Azure Active Directory** simgesine tıklayın.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar** seçeneğini belirleyin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için, iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesine tıklayın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna, **GmbH göre Bitbucket Için SAML SSO**yazın; sonuç panelinde, **çözünürlükte GmbH ile Bitbucket için SAML SSO** 'yu seçin ve sonra uygulamayı eklemek için **Ekle** düğmesine tıklayın.

     ![Sonuç listesinde çözüm GmbH göre Bitbucket için SAML SSO 'SU](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma ve test etme

Bu bölümde, **Britta Simon**adlı bir test kullanıcısına bağlı olarak, GmbH tarafından verilen Bitbucket için SAML SSO 'Su Ile Azure AD çoklu oturum açmayı yapılandırıp test edersiniz.
Çoklu oturum açma 'nın çalışması için, bir Azure AD kullanıcısı ve GmbH ile Bitbucket için SAML SSO 'daki ilgili Kullanıcı arasındaki bağlantı ilişkisinin kurulması gerekir.

GmbH tarafından Bitbucket için SAML SSO 'SU ile Azure AD çoklu oturum açmayı yapılandırmak ve test etmek için aşağıdaki yapı taşlarını gerçekleştirmeniz gerekir:

1. **[Azure AD çoklu oturum açma özelliğini yapılandırarak](#configure-azure-ad-single-sign-on)** kullanıcılarınızın bu özelliği kullanmasına olanak sağlayın.
2. Uygulama tarafında tek Sign-On ayarlarını yapılandırmak için, tek bir **[oturum açma GmbH göre Bitbucket IÇIN SAML SSO 'Yu yapılandırın](#configure-saml-sso-for-bitbucket-by-resolution-gmbh-single-sign-on)** .
3. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -Britta Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
4. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanarak Britta Simon 'u etkinleştirin.
5. User 'ın Azure AD gösterimine bağlı olan, Resolution GmbH 'e göre Bitbucket için SAML SSO 'SU ' nde bir Britta Simon 'ın bir karşılığı olması için, **[GmbH test kullanıcısına göre Bitbucket IÇIN SAML SSO 'Su oluşturun](#create-saml-sso-for-bitbucket-by-resolution-gmbh-test-user)** .
6. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[Çoklu oturum açmayı sınayın](#test-single-sign-on)** .

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure portal Azure AD çoklu oturum açma özelliğini etkinleştirirsiniz.

Azure AD çoklu oturum açmayı, bit demeti için SAML SSO 'yu çözüm GmbH göre yapılandırmak için aşağıdaki adımları uygulayın:

1. [Azure Portal](https://portal.azure.com/), **GmbH ile Bitbucket için SAML SSO** uygulama tümleştirmesi sayfasında, **Çoklu oturum açma**' yı seçin.

    ![Çoklu oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Çoklu oturum **açma yöntemi seç** iletişim kutusunda, çoklu oturum açmayı etkinleştirmek için **SAML/WS-Besme** modunu seçin.

    ![Çoklu oturum açma seçme modu](common/select-saml-option.png)

3. **SAML Ile tek Sign-On ayarlama** sayfasında, **temel SAML yapılandırması** Iletişim kutusunu açmak için **Düzenle** simgesine tıklayın.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. **Temel SAML yapılandırması** bölümünde, uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin:

    ![Ekran görüntüsü; tanımlayıcı girebileceğiniz, yanıt U R L ve Kaydet ' i seçebileceğiniz temel SAML yapılandırmasını gösterir.](common/idp-intiated.png)

    a. **Tanımlayıcı** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<server-base-url>/plugins/servlet/samlsso`

    b. **Yanıt URL 'si** metin kutusuna aşağıdaki kalıbı kullanarak bir URL yazın:`https://<server-base-url>/plugins/servlet/samlsso`

    c. Uygulamayı **SP** tarafından başlatılan modda yapılandırmak Istiyorsanız **ek URL 'ler ayarla** ' ya tıklayın ve aşağıdaki adımı gerçekleştirin:

    ![Ekran görüntüsü, U R L 'ye bir Işaret girebileceğiniz ek U R 'Leri ayarlamayı gösterir.](common/metadata-upload-additional-signon.png)

    **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<server-base-url>/plugins/servlet/samlsso`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek tanımlayıcı, yanıt URL 'SI ve oturum açma URL 'SI ile güncelleştirin. Bu değerleri almak için, [Resolution GmbH istemci desteği ekibine göre Bitbucket Için SAML SSO 'su ile](https://marketplace.atlassian.com/apps/1217045/saml-single-sign-on-sso-bitbucket?hosting=server&tab=support) iletişim kurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

5. **SAML Ile tek Sign-On ayarlama** sayfasında, **SAML imza sertifikası** bölümünde, **Federasyon meta veri XML** 'sini gereksiniminize göre belirtilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir** ' e tıklayın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

### <a name="configure-saml-sso-for-bitbucket-by-resolution-gmbh-single-sign-on"></a>GmbH tek Sign-On tarafından Bitbucket için SAML SSO yapılandırma

1. Yönetici olarak çözüm GmbH Şirket sitesine göre Bitbucket için SAML SSO 'ünüz üzerinde oturum açın.

2. Ana araç çubuğunun sağ tarafında, **Ayarlar**' a tıklayın.

3. HESAPLAR bölümüne gidin ve menü çubuğunda **SAML SingleSignOn** öğesine tıklayın.

    ![Samlsingle](./media/bitbucket-tutorial/tutorial_bitbucket_samlsingle.png)

4. **SAML SIngleSignOn eklenti yapılandırması sayfasında** **IDP Ekle**' ye tıklayın. 

    ![IDP ekleme](./media/bitbucket-tutorial/tutorial_bitbucket_addidp.png)

5. **SAML kimlik sağlayıcınızı seçin** sayfasında aşağıdaki adımları uygulayın:

    ![Kimlik sağlayıcısı](./media/bitbucket-tutorial/tutorial_bitbucket_identityprovider.png)

    a. **IDP türünü** **Azure AD**olarak seçin.

    b. **Ad** metin kutusuna adı yazın.

    c. **Açıklama** metin kutusuna açıklamayı yazın.

    d. **İleri**’ye tıklayın.

6. **Kimlik sağlayıcısı yapılandırma sayfasında** **İleri**' ye tıklayın.

    ![Kimlik yapılandırması](./media/bitbucket-tutorial/tutorial_bitbucket_identityconfig.png)

7.  Azure portal 'den indirdiğiniz **meta VERI XML** dosyasını karşıya yüklemek Için **SAML IDP meta verilerini Içeri aktar** sayfasında **Dosya Yükle** ' ye tıklayın.

    ![Idpmetadata](./media/bitbucket-tutorial/tutorial_bitbucket_idpmetadata.png)
    
8. **İleri**’ye tıklayın.

9. **Ayarları kaydet**’e tıklayın.

    ![Kaydet](./media/bitbucket-tutorial/tutorial_bitbucket_save.png)

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

Bu bölümde, çözünürlükte GmbH tarafından Bitbucket için SAML SSO 'SU erişimi vererek Azure çoklu oturum açma özelliğini kullanarak Britta Simon 'u etkinleştirin.

1. Azure portal **Kurumsal uygulamalar**' ı seçin, **tüm uygulamalar**' ı seçin ve ardından **çözüm GmbH göre Bitbucket için SAML SSO**' yı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde, **çözüm GmbH göre Bitbucket Için SAML SSO**'yu yazın ve seçin.

    ![Uygulamalar listesinde, çözünürlükte GmbH bağlantısı tarafından Bitbucket için SAML SSO 'SU](common/all-applications.png)

3. Soldaki menüde **Kullanıcılar ve gruplar**' ı seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. **Kullanıcı Ekle** düğmesine tıklayın, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. **Kullanıcılar ve gruplar** Iletişim kutusunda kullanıcılar listesinde **Britta Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

6. SAML onaylama işlemi içinde herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, listeden Kullanıcı için uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

7. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

### <a name="create-saml-sso-for-bitbucket-by-resolution-gmbh-test-user"></a>Resolution GmbH test kullanıcısına göre Bitbucket için SAML SSO oluşturma

Bu bölümün amacı, GmbH tarafından verilen Bitbucket için SAML SSO 'da Britta Simon adlı bir Kullanıcı oluşturmaktır. Resolution GmbH tarafından desteklenen Bitbucket için SAML SSO, tam zamanında sağlamayı destekler ve ayrıca kullanıcılar el ile oluşturulabilir, gereksiniminize göre, BT [GmbH istemci desteği ekibine göre Bitbucket Için SAML SSO 'su](https://marketplace.atlassian.com/plugins/com.resolution.atlasplugins.samlsso-bitbucket/server/support) ile iletişim kurun.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde,-Resolution GmbH for the Bitbucket için SAML SSO 'su ' ne tıkladığınızda, SSO 'yu ayarladığınız çözünürlükte GmbH göre Bitbucket için SAML SSO 'SU için otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](../user-help/my-apps-portal-end-user-access.md).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](./tutorial-list.md)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

- [Azure Active Directory Koşullu erişim nedir?](../conditional-access/overview.md)