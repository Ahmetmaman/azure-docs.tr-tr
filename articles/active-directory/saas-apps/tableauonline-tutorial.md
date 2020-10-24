---
title: 'Öğretici: Tableau Online ile tümleştirme Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve Tableau arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 01/31/2020
ms.author: jeedes
ms.openlocfilehash: 7841f09b113b4bdf7eacddbbfc5f054bf69b1750
ms.sourcegitcommit: 59f506857abb1ed3328fda34d37800b55159c91d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2020
ms.locfileid: "92517829"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-tableau-online"></a>Öğretici: Tableau Online ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory

Bu öğreticide, Tableau online 'ı Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. Tableau online 'ı Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de Tableau online 'a erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla çevrimiçi Tableau için otomatik olarak oturum açmalarına olanak sağlayın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

Azure AD ile SaaS uygulaması tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Tableau çevrimiçi çoklu oturum açma (SSO) etkin aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

* Tableau Online, **SP** tarafından başlatılan SSO 'yu destekler
* Tableau online 'ı yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak ayıklanmasını ve zaman korumasını koruyan oturum denetimini zorunlu kılabilirsiniz. Oturum denetimi koşullu erişimden genişletilir. [Microsoft Cloud App Security ile oturum denetimini nasıl zorlayacağınızı öğrenin](/cloud-app-security/proxy-deployment-aad)

## <a name="adding-tableau-online-from-the-gallery"></a>Galeriden çevrimiçi Tableau ekleme

Tableau online 'ın tümleştirmesini Azure AD ile yapılandırmak için, galerideki Tableau online 'ı yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

1. [Azure Portal](https://portal.azure.com) iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Tableau online** yazın.
1. Sonuçlar panelinden **Tableau online** ' ı seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma ve test etme

Bu bölümde, **Britta Simon**adlı bir test kullanıcısına göre Tableau online Ile Azure AD çoklu oturum açmayı yapılandırıp test edersiniz.
Çoklu oturum açma için, bir Azure AD kullanıcısı ve Tableau online 'daki ilgili Kullanıcı arasındaki bağlantı ilişkisinin kurulması gerekir.

Azure AD SSO 'yu Tableau Online ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını doldurun:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
    1. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
    1. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
1. **[Tableau ONLINE SSO 'Yu yapılandırma](#configure-tableau-online-sso)** -uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
    1. User 'ın Azure AD gösterimine bağlı olan Tableau online 'da B. Simon 'a sahip olmak için **[Tableau online test kullanıcısı oluşturun](#create-tableau-online-test-user)** .
1. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Bu bölümde, Azure portal Azure AD çoklu oturum açma özelliğini etkinleştirirsiniz.

Azure AD çoklu oturum açmayı Tableau Online ile yapılandırmak için aşağıdaki adımları uygulayın:

1. [Azure Portal](https://portal.azure.com/), **Tableau çevrimiçi** uygulama tümleştirmesi sayfasında, **Çoklu oturum açma**' yı seçin.

    ![Çoklu oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Çoklu oturum **açma yöntemi seç** iletişim kutusunda, çoklu oturum açmayı etkinleştirmek için **SAML/WS-Besme** modunu seçin.

    ![Çoklu oturum açma seçme modu](common/select-saml-option.png)

3. **SAML Ile tek Sign-On ayarlama** sayfasında, **temel SAML yapılandırması** Iletişim kutusunu açmak için **Düzenle** simgesine tıklayın.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. **Temel SAML yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Tableau çevrimiçi etki alanı ve URL 'Ler çoklu oturum açma bilgileri](common/sp-identifier.png)

    a. **Oturum açma URL 'si** metin kutusuna URL 'yi yazın:`https://sso.online.tableau.com/public/sp/login?alias=<entityid>`

    b. **Tanımlayıcı (VARLıK kimliği)** metın kutusuna URL yazın:`https://sso.online.tableau.com/public/sp/metadata?alias=<entityid>`

    > [!NOTE]
    > `<entityid>`Bu öğreticideki **Tableau online 'ı ayarla** bölümünde değeri alacaksınız. Varlık KIMLIĞI değeri, **Tableau online 'ı ayarla** bölümünde **Azure AD tanımlayıcı** değeri olacak.

5. **SAML Ile tek Sign-On ayarlama** sayfasında, **SAML imza sertifikası** bölümünde, **Federasyon meta veri XML** 'sini gereksiniminize göre belirtilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir** ' e tıklayın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

6. **Tableau online 'ı ayarla** bölümünde uygun URL 'leri gereksiniminize göre kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD tanımlayıcısı

    c. Oturum kapatma URL 'SI

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
    Örneğin, Brittasıon \@ contoso.com

    c. **Parolayı göster** onay kutusunu seçin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Tableau çevrimiçi erişim izni vererek Azure çoklu oturum açma özelliğini kullanmak için Britta Simon 'u etkinleştirin.

1. Azure portal **Kurumsal uygulamalar**' ı seçin, **tüm uygulamalar**' ı seçin, sonra **Tableau online**' ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Tableau online**' ı seçin.

    ![Uygulamalar listesindeki Tableau online bağlantısı](common/all-applications.png)

3. Soldaki menüde **Kullanıcılar ve gruplar**' ı seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. **Kullanıcı Ekle** düğmesine tıklayın, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. **Kullanıcılar ve gruplar** Iletişim kutusunda kullanıcılar listesinde **Britta Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

6. SAML onaylama işlemi içinde herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, listeden Kullanıcı için uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

7. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

## <a name="configure-tableau-online-sso"></a>Tableau online SSO 'yu yapılandırma

1. Farklı bir tarayıcı penceresinde, Tableau çevrimiçi uygulamanızda oturum açın. **Ayarlar** ' a ve ardından **kimlik doğrulaması**' na gidin.

    ![Ekran görüntüsü ayarlar menüsünden seçilen kimlik doğrulamasını gösterir.](./media/tableauonline-tutorial/tutorial_tableauonline_09.png)

2. SAML 'yi etkinleştirmek için **kimlik doğrulama türleri** bölümünde bölümüne gidin. **Ek bir kimlik doğrulama yöntemini etkinleştirip** **SAML** onay kutusunu işaretleyin.

    ![Ekran görüntüsü, değerleri seçebileceğiniz kimlik doğrulama türleri bölümünü gösterir.](./media/tableauonline-tutorial/tutorial_tableauonline_12.png)

3. **Meta veri dosyasını Tableau online bölümüne aktarmak** için aşağı kaydırın.  Araştır ' a tıklayın ve Azure AD 'den indirdiğiniz meta veri dosyasını içeri aktarın. Sonra **Uygula**' ya tıklayın.

   ![Ekran görüntüsü, meta veri dosyasını içeri aktarabileceğiniz bölümü gösterir.](./media/tableauonline-tutorial/tutorial_tableauonline_13.png)

4. **Onayları Eşleştir** bölümünde **e-posta adresi**, **adı**ve **Soyadı**için karşılık gelen kimlik sağlayıcısı onaylama adını ekleyin. Bu bilgileri Azure AD 'den almak için: 
  
    a. Azure portal, **Tableau çevrimiçi** uygulama tümleştirme sayfasına gidin.

    b. **Kullanıcı öznitelikleri & talepler** bölümünde düzenleme simgesine tıklayın.

   ![Ekran görüntüsü, düzenleme simgesini seçebileceğiniz Kullanıcı öznitelikleri & talepler bölümünü gösterir.](./media/tableauonline-tutorial/attributesection.png)

    c. Aşağıdaki adımları kullanarak şu özniteliklerin ad alanı değerini kopyalayın: 1., e-posta ve Soyadı:

   ![Ekran görüntüsü,, soyadı ve Emadresi özniteliklerini gösterir.](./media/tableauonline-tutorial/tutorial_tableauonline_10.png)

    d. **User.** bir değer ' e tıklayın

    e. **Ad alanı** metin kutusundan değeri kopyalayın.

    ![Ekran görüntüsü, ad alanını girebileceğiniz Kullanıcı taleplerini Yönet bölümünü gösterir.](./media/tableauonline-tutorial/attributesection2.png)

    f. E-posta için ad alanı değerlerini kopyalamak ve soyadı yukarıdaki adımları tekrarlayın.

    örneğin: Tableau Online uygulamasına geçiş yapın ve ardından **Kullanıcı öznitelikleri & talep** bölümünü aşağıdaki şekilde ayarlayın:

    * E-posta: **posta** veya **userPrincipalName**

    * Ad **: önce**

    * Soyadı: **Soyadı**

    ![Ekran görüntüsü, değerleri girebileceğiniz eşleşme öznitelikleri bölümünü gösterir.](./media/tableauonline-tutorial/tutorial_tableauonline_14.png)

### <a name="create-tableau-online-test-user"></a>Tableau online test kullanıcısı oluşturma

Bu bölümde, Tableau online 'da Britta Simon adlı bir Kullanıcı oluşturacaksınız.

1. **Tableau online**'da **Ayarlar** ' a ve ardından **kimlik doğrulama** Bölümü ' ne tıklayın. **Kullanıcıları Yönet** bölümünde aşağı kaydırın. **Kullanıcı Ekle** ' ye ve ardından **e-posta adreslerini girin ' e**tıklayın
  
    ![Ekran görüntüsü kullanıcıları Yönet bölümünü gösterir ve buradan Kullanıcı Ekle seçeneğini belirleyebilirsiniz.](./media/tableauonline-tutorial/tutorial_tableauonline_15.png)

2. **(SAML) kimlik doğrulaması için Kullanıcı Ekle**' yi seçin. **E-posta adreslerini girin** metin kutusuna Britta. simon \@ contoso.com ekleyin
  
    ![Ekran görüntüsü, bir e-posta adresi girebileceğiniz Kullanıcı Ekle sayfasını gösterir.](./media/tableauonline-tutorial/tutorial_tableauonline_11.png)

3. **Kullanıcı Ekle**' ye tıklayın.

### <a name="test-sso"></a>Test SSO 'SU

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde Tableau çevrimiçi kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız Tableau online 'da otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](../user-help/my-apps-portal-end-user-access.md).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](./tutorial-list.md)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

- [Azure Active Directory Koşullu erişim nedir?](../conditional-access/overview.md)

- [Microsoft Cloud App Security oturum denetimi nedir?](/cloud-app-security/proxy-intro-aad)