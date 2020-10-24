---
title: 'Öğretici: TOPdesk-public ile tümleştirme Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve TOPdesk-public arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 05/02/2019
ms.author: jeedes
ms.openlocfilehash: 3688dc2db67c0c1343a74be4652cb52466ef762e
ms.sourcegitcommit: 59f506857abb1ed3328fda34d37800b55159c91d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2020
ms.locfileid: "92520447"
---
# <a name="tutorial-azure-active-directory-integration-with-topdesk---public"></a>Öğretici: Topmasa-genel ile tümleştirme Azure Active Directory

Bu öğreticide, Azure Active Directory (Azure AD) ile TOPdesk-public tümleştirme hakkında bilgi edineceksiniz.
Azure AD ile TOPdesk-Public ' i tümleştirme aşağıdaki avantajları sağlar:

* Azure AD 'de Topmasa-herkese erişimi olan ' i denetleyebilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla otomatik olarak Topmasa-genel (çoklu oturum açma) için oturum açmasını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz-Azure portal.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek istiyorsanız, bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesini TOPdesk-public ile yapılandırmak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Bir Azure AD ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü edinebilirsiniz
* Topmasa-genel çoklu oturum açma etkin aboneliği

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

* Topmasa-genel **SP** tarafından başlatılan SSO 'yu destekler

## <a name="adding-topdesk---public-from-the-gallery"></a>Galeriden TOPdesk-public ekleme

Topmasa-genel ' i Azure AD ile tümleştirmeyi yapılandırmak için, Galeriden Topmasa-Public ' i yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden TOPdesk-public eklemek için aşağıdaki adımları uygulayın:**

1. **[Azure Portal](https://portal.azure.com)** sol gezinti panelinde **Azure Active Directory** simgesine tıklayın.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar** seçeneğini belirleyin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için, iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesine tıklayın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Topmasa-public**yazın, sonuç panelinden **Topmasa-genel** ' i seçin, sonra da uygulamayı eklemek için düğme **Ekle** ' ye tıklayın.

     ![Sonuç listesinde Topmasa-genel](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma ve test etme

Bu bölümde, Azure AD çoklu oturum açmayı, **Britta Simon**adlı bir test kullanıcısına göre TopDesk-public ile yapılandırıp test edersiniz.
Çoklu oturum açma için, bir Azure AD kullanıcısı ile ilgili Kullanıcı ile Topmasa-genel arasındaki bağlantı ilişkisinin kurulması gerekir.

Azure AD çoklu oturum açmayı, TOPdesk-public ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını gerçekleştirmeniz gerekir:

1. **[Azure AD çoklu oturum açma özelliğini yapılandırarak](#configure-azure-ad-single-sign-on)** kullanıcılarınızın bu özelliği kullanmasına olanak sağlayın.
2. Uygulama tarafında tek Sign-On ayarlarını yapılandırmak için **[Topmasa-genel çoklu oturum açmayı yapılandırın](#configure-topdesk---public-single-sign-on)** .
3. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -Britta Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
4. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanarak Britta Simon 'u etkinleştirin.
5. Topmasa **[-genel test kullanıcısı oluşturma](#create-topdesk---public-test-user)** -kullanıcının Azure AD gösterimine bağlı olan Topmasa-Genel bölümünde Britta Simon 'a sahip olmak için.
6. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[Çoklu oturum açmayı sınayın](#test-single-sign-on)** .

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure portal Azure AD çoklu oturum açma özelliğini etkinleştirirsiniz.

Azure AD çoklu oturum açmayı TOPdesk-public ile yapılandırmak için aşağıdaki adımları uygulayın:

1. [Azure Portal](https://portal.azure.com/), **Topmasa-genel** uygulama tümleştirmesi sayfasında, **Çoklu oturum açma**' yı seçin.

    ![Çoklu oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Çoklu oturum **açma yöntemi seç** iletişim kutusunda, çoklu oturum açmayı etkinleştirmek için **SAML/WS-Besme** modunu seçin.

    ![Çoklu oturum açma seçme modu](common/select-saml-option.png)

3. **SAML Ile tek Sign-On ayarlama** sayfasında, **temel SAML yapılandırması** Iletişim kutusunu açmak için **Düzenle** simgesine tıklayın.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4.  **Temel SAML yapılandırması** bölümünde, **hizmet sağlayıcısı meta verileri dosyanız**varsa, aşağıdaki adımları uygulayın:

    >[!NOTE]
    >**Hizmet sağlayıcı meta veri dosyasını** , Öğreticinin ilerleyen kısımlarında açıklanan, **Topmasa genel çoklu oturum açma** bölümünde bulabilirsiniz.

    a. **Meta veri dosyasını karşıya yükle**' ye tıklayın.
    
    ![Meta veri dosyasını karşıya yükle](common/upload-metadata.png)

    b. Meta veri dosyasını seçmek için **klasör logosu** ' na tıklayın ve **karşıya yükle**' ye tıklayın.

    ![meta veri dosyası seçin](common/browse-upload-metadata.png)

    c. Meta veri dosyası başarıyla karşıya yüklendikten sonra, **tanımlayıcı** ve **yanıt URL** değerleri temel SAML yapılandırması bölümünde otomatik olarak doldurulur.

    ![Topmasa-genel etki alanı ve URL 'Ler çoklu oturum açma bilgileri](common/sp-identifier-reply.png)

    d. **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<companyname>.topdesk.net`

    e. **Tanımlayıcı URL** metin kutusunda, TopDesk yapılandırmasından alabileceğiniz TopDesk meta veri URL 'sini girin. Bu, aşağıdaki kalıbı kullanmalıdır: `https://<companyname>.topdesk.net/saml-metadata/<identifier>`
    
    f. **Yanıt URL** metin kutusuna aşağıdaki kalıbı kullanarak bir URL yazın:`https://<companyname>.topdesk.net/tas/public/login/verify`
    
    > [!NOTE] 
    > **Tanımlayıcı** ve **yanıt URL 'si** değerleri otomatik olarak doldurulmazsa, bunları el ile girmeniz gerekir. Tanımlayıcı için yukarıda belirtilen kalıbı izleyin ve öğreticide daha sonra açıklanan **Topmasa genel çoklu oturum açma** bölümünden yanıt URL 'si değerini alın. **Oturum açma URL 'si** değeri gerçek değil, bu değeri gerçek Sign-On URL 'siyle güncelleştirmeniz gerekiyor. Değeri almak için [Topmasa-genel istemci desteği ekibine](https://help.topdesk.com/saas/enterprise/user/) başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

5. **SAML Ile tek Sign-On ayarlama** sayfasında, **SAML imza sertifikası** bölümünde, **Federasyon meta veri XML** 'sini gereksiniminize göre belirtilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir** ' e tıklayın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

6. **TOPdesk-public ayarlama** bölümünde uygun URL 'leri gereksiniminize göre kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD tanımlayıcısı

    c. Oturum kapatma URL 'SI

### <a name="configure-topdesk---public-single-sign-on"></a>TOPdesk-genel tek Sign-On yapılandırma

1. **Topoloji-ortak** şirket sitenizde yönetici olarak oturum açın.

2. **TopDesk** menüsünde **Ayarlar**' a tıklayın.
   
    ![Ayarlar](./media/topdesk-public-tutorial/ic790598.png "Ayarlar")

3. **Oturum açma ayarları**' na tıklayın.
   
    ![Oturum açma ayarları](./media/topdesk-public-tutorial/ic790599.png "Oturum açma ayarları")

4. **Oturum açma ayarları** menüsünü genişletin ve ardından **genel**' e tıklayın.
   
    ![Genel](./media/topdesk-public-tutorial/ic790600.png "Genel")

5. **SAML oturum açma** Yapılandırması bölümünün **genel** bölümünde aşağıdaki adımları uygulayın:
   
    ![Teknik ayarlar](./media/topdesk-public-tutorial/ic790601.png "Teknik ayarlar")
   
    a. Ortak meta veri dosyasını indirmek için **İndir** ' e tıklayın ve ardından bilgisayarınıza yerel olarak kaydedin.
   
    b. İndirilen meta veri dosyasını açın ve ardından **Assertionconsumerservice** düğümünü bulun.

    ![AssertionConsumerService](./media/topdesk-public-tutorial/ic790619.png "AssertionConsumerService")
   
    c. **Assertionconsumerservice** değerini kopyalayın, bu DEĞERI **temel SAML YAPıLANDıRMASı** bölümündeki **yanıt URL** metin kutusuna yapıştırın.      
   
6. Bir sertifika dosyası oluşturmak için aşağıdaki adımları gerçekleştirin:
    
    ![Sertifika](./media/topdesk-public-tutorial/ic790606.png "Sertifika")
    
    a. İndirilen meta veri dosyasını Azure portal açın.
    
    b. **Xsi: Type türünde** **Besleyicisi: Applicationservicetype**olan **RoleDescriptor** düğümünü genişletin.
    
    c. **X509Certificate** düğümünün değerini kopyalayın.
    
    d. Kopyalanmış **X509Certificate** değerini bilgisayarınıza yerel olarak bir dosyaya kaydedin.

7. **Genel** bölümünde, **Ekle**' ye tıklayın.
    
    ![SAML oturum açma](./media/topdesk-public-tutorial/ic790625.png "SAML oturum açma")

8. **SAML yapılandırma Yardımcısı** iletişim kutusunda, aşağıdaki adımları uygulayın:
    
    ![SAML yapılandırma Yardımcısı](./media/topdesk-public-tutorial/ic790608.png "SAML yapılandırma Yardımcısı")
    
    a. İndirilen meta veri dosyanızı Azure portal karşıya yüklemek için, **Federasyon meta verileri**altında, **Araştır**' a tıklayın.

    b. Sertifika dosyanızı karşıya yüklemek için, **sertifika (RSA)** altında, **Araştır**' a tıklayın.

    c. TOPdesk destek ekibinden aldığınız logo dosyasını karşıya yüklemek için, **logo simgesi**altında, **Araştır**' a tıklayın.

    d. **Kullanıcı adı öznitelik** metin kutusuna yazın `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress` .

    e. **Görünen ad** metin kutusuna yapılandırmanız için bir ad yazın.

    f. **Kaydet**’e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Azure portal Britta Simon adlı bir test kullanıcısı oluşturmaktır.

1. Azure portal, sol bölmedeki **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.

    !["Kullanıcılar ve gruplar" ve "tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.

    ![Yeni Kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı Özellikleri ' nde aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanına **Brittasıon**girin.
  
    b. **Kullanıcı adı** alanına yazın brittasimon@yourcompanydomain.extension . Örneğin, BrittaSimon@contoso.com

    c. **Parolayı göster** onay kutusunu seçin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Topmasa-herkese erişimi vererek Azure çoklu oturum açma özelliğini kullanmak için Britta Simon 'u etkinleştirin.

1. Azure portal **Kurumsal uygulamalar**' ı seçin, **tüm uygulamalar**' ı seçin ve ardından **topmasa-genel**' i seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde, **Topmasa-genel**' i seçin.

    ![Uygulamalar listesindeki Topmasa-genel bağlantısı](common/all-applications.png)

3. Soldaki menüde **Kullanıcılar ve gruplar**' ı seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. **Kullanıcı Ekle** düğmesine tıklayın, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. **Kullanıcılar ve gruplar** Iletişim kutusunda kullanıcılar listesinde **Britta Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

6. SAML onaylama işlemi içinde herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, listeden Kullanıcı için uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

7. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

### <a name="create-topdesk---public-test-user"></a>Topmasa-ortak test kullanıcısı oluşturma

Azure AD kullanıcılarının Topmasa-genel ' te oturum açmasını sağlamak için, bu kullanıcıların Topmasa-genel ' e sağlanması gerekir. Topmasa-genel durumunda, sağlama el ile gerçekleştirilen bir görevdir.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Kullanıcı sağlamayı yapılandırmak için aşağıdaki adımları uygulayın:

1. **Topoloji-ortak** şirket sitenizde yönetici olarak oturum açın.

2. Üstteki menüde **TopDesk \> Yeni \> destek dosyaları \> kişisi**' ne tıklayın.
   
    ![Kişi](./media/topdesk-public-tutorial/ic790628.png "Kişi")

3. Yeni kişi iletişim kutusunda aşağıdaki adımları gerçekleştirin:
   
    ![Yeni kişi](./media/topdesk-public-tutorial/ic790629.png "Yeni kişi")
   
    a. Genel sekmesine tıklayın.

    b. **Soyadı** metin kutusunda, Simon gibi kullanıcının soyadı yazın
 
    c. Hesap için bir **site** seçin.
 
    d. **Kaydet**’e tıklayın.

> [!NOTE]
> Azure AD Kullanıcı hesapları sağlamak için, TOPdesk-public tarafından sunulan diğer tüm Topmasa-genel kullanıcı hesabı oluşturma araçlarını veya API 'Leri kullanabilirsiniz.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde Topmasa-genel kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız Topmasa-genel ' e otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](../user-help/my-apps-portal-end-user-access.md).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](./tutorial-list.md)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

- [Azure Active Directory Koşullu erişim nedir?](../conditional-access/overview.md)