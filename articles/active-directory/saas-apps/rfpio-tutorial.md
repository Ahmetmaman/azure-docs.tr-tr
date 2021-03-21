---
title: 'Öğretici: RFıO ile tümleştirme Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve RFıO arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 04/14/2019
ms.author: jeedes
ms.openlocfilehash: c4e838afa867a7fb1e7fa8f582bc8879c24056a9
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "92506199"
---
# <a name="tutorial-azure-active-directory-integration-with-rfpio"></a>Öğretici: RFıO ile tümleştirme Azure Active Directory

Bu öğreticide, RFıO 'ı Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz.
RFıO 'ı Azure AD ile tümleştirmek aşağıdaki avantajları sağlar:

* Azure AD 'de RFıO erişimi olan bir denetim yapabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla RFıO (çoklu oturum açma) ile otomatik olarak oturum açmasını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz-Azure portal.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek istiyorsanız, bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesini RFıO ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Bir Azure AD aboneliği. Bir Azure AD ortamınız yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) alabilirsiniz
* RFıO çoklu oturum açma etkin aboneliği

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

* RFıO **, SP ve ıDP** tarafından başlatılan SSO 'yu destekler

## <a name="adding-rfpio-from-the-gallery"></a>Galeriden RFıO ekleme

RFıO 'ın Azure AD ile tümleştirilmesini yapılandırmak için, Galeriden RFıO 'ı yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden RFıO eklemek için aşağıdaki adımları uygulayın:**

1. **[Azure Portal](https://portal.azure.com)** sol gezinti panelinde **Azure Active Directory** simgesine tıklayın.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar** seçeneğini belirleyin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için, iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesine tıklayın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **rfıo** yazın, sonuç panelinden **rfıo** ' yı seçin ve ardından **Ekle** düğmesine tıklayarak uygulamayı ekleyin.

    ![Sonuçlar listesinde RFıO](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma ve test etme

Bu bölümde, Azure AD çoklu oturum açmayı, **Britta Simon** adlı bir test kullanıcısına göre rfıo ile yapılandırıp test edersiniz.
Çoklu oturum açma için, bir Azure AD kullanıcısı ve RFıO 'daki ilgili Kullanıcı arasındaki bağlantı ilişkisinin oluşturulması gerekir.

Azure AD çoklu oturum açma 'yı RFıO ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını gerçekleştirmeniz gerekir:

1. **[Azure AD çoklu oturum açma özelliğini yapılandırarak](#configure-azure-ad-single-sign-on)** kullanıcılarınızın bu özelliği kullanmasına olanak sağlayın.
2. Uygulama tarafında tek Sign-On ayarlarını yapılandırmak için **[RFıO çoklu oturum açmayı yapılandırın](#configure-rfpio-single-sign-on)** .
3. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -Britta Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
4. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanarak Britta Simon 'u etkinleştirin.
5. Kullanıcı Azure AD gösterimi ile bağlantılı olan RFıO 'da Britta Simon 'ın bir karşılığı olacak şekilde **[rfıo test kullanıcısı oluşturun](#create-rfpio-test-user)** .
6. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[Çoklu oturum açmayı sınayın](#test-single-sign-on)** .

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure portal Azure AD çoklu oturum açma özelliğini etkinleştirirsiniz.

Azure AD çoklu oturum açmayı RFıO ile yapılandırmak için aşağıdaki adımları uygulayın:

1. [Azure Portal](https://portal.azure.com/), **rfıo** uygulama tümleştirmesi sayfasında, **Çoklu oturum açma**' yı seçin.

    ![Çoklu oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Çoklu oturum **açma yöntemi seç** iletişim kutusunda, çoklu oturum açmayı etkinleştirmek için **SAML/WS-Besme** modunu seçin.

    ![Çoklu oturum açma seçme modu](common/select-saml-option.png)

3. **SAML Ile tek Sign-On ayarlama** sayfasında, **temel SAML yapılandırması** Iletişim kutusunu açmak için **Düzenle** simgesine tıklayın.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. **Temel SAML yapılandırması** bölümünde, uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız aşağıdaki adımı uygulayın:

    ![Ekran görüntüsü, bir tanımlayıcı girebileceğiniz temel SAML yapılandırmasını gösterir.](common/idp-identifier.png)

    a. **Tanımlayıcı** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://www.rfpio.com`

    b. **Ek URL 'Ler ayarla**' ya tıklayın.

    c. **Geçiş durumu** metin kutusuna bir dize değeri girin. Bu değeri almak için [Rfıo destek ekibine](https://www.rfpio.com/contact/) başvurun.

    ![Ekran görüntüsünde ek U R ls gösterilmektedir.](common/idp-preintegrated-relay.png)

5. Uygulamayı **SP** tarafından başlatılan modda yapılandırmak Istiyorsanız **ek URL 'ler ayarla** ' ya tıklayın ve aşağıdaki adımı gerçekleştirin:

    ![image](common/both-preintegrated-signon.png)

    **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://www.app.rfpio.com`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek tanımlayıcı ve oturum açma URL 'SI ile güncelleştirin. Bu değerleri almak için [Rfıo istemci destek ekibine](https://www.rfpio.com/contact/) başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

6. **SAML Ile tek Sign-On ayarlama** sayfasında, **SAML imza sertifikası** bölümünde, **Federasyon meta veri XML** 'sini gereksiniminize göre belirtilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir** ' e tıklayın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

7. **RFıO ayarla** bölümünde uygun URL 'leri gereksiniminize göre kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD tanımlayıcısı

    c. Oturum kapatma URL 'SI

### <a name="configure-rfpio-single-sign-on"></a>RFıO tek Sign-On yapılandırma

1. Farklı bir Web tarayıcısı penceresinde, **Rfıo** Web sitesinde yönetici olarak oturum açın.

1. Sol alt köşedeki aşağı açılan listeye tıklayın.

    ![Ekran görüntüsü bölmenin alt kısmındaki aşağı oku gösterir.](./media/rfpio-tutorial/app1.png)

1. **Kuruluş ayarları**' na tıklayın. 

    ![Ekran görüntüsü kuruluş ayarlarının seçili olduğunu gösterir.](./media/rfpio-tutorial/app2.png)

1. **Tümleştirme & Özellikler**' e tıklayın.

    ![Ekran görüntüsünde, ayarlardan seçilen özellikler ve tümleştirme gösterilmektedir.](./media/rfpio-tutorial/app4.png)

1. **SAML SSO yapılandırması** ' nda **Düzenle**' ye tıklayın.

    ![Ekran görüntüsü, Düzenle düğmesi olarak adlandırılan SAML S S O yapılandırmasını gösterir.](./media/rfpio-tutorial/app3.png)

1. Bu bölümde aşağıdaki eylemleri gerçekleştirin:

    ![CEkran görüntüsü SAML 'nin etkinleştirildiği SAML S S O yapılandırmasını gösterir.](./media/rfpio-tutorial/app5.png)
    
    a. **Indirilen meta VERI XML** içeriğini kopyalayın ve **kimlik yapılandırma** alanına yapıştırın.

    > [!NOTE]
    >İndirilen **Federasyon meta VERILERI XML** içeriğini kopyalamak için **Notepad + +** veya uygun **XML Düzenleyicisi** kullanın.

    b. **Doğrula**' ya tıklayın.

    c. **Doğrula**' ya tıkladıktan sonra **SAML 'yi (etkin)** açık olarak çevirin.

    d. **Gönder**' e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Azure portal Britta Simon adlı bir test kullanıcısı oluşturmaktır.

1. Azure portal, sol bölmedeki **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.

    !["Kullanıcılar ve gruplar" ve "tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.

    ![Yeni Kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı Özellikleri ' nde aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanına **Brittasıon** girin.
  
    b. **Kullanıcı adı** alanına yazın `brittasimon@yourcompanydomain.extension` . Örneğin, BrittaSimon@contoso.com

    c. **Parolayı göster** onay kutusunu seçin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, RFıO 'a erişim vererek Azure çoklu oturum açma özelliğini kullanmak için Britta Simon özelliğini etkinleştirirsiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin, **tüm uygulamalar**' ı seçin ve ardından **rfıo**' yi seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Rfıo**' yı seçin.

    ![Uygulamalar listesindeki RFıO bağlantısı](common/all-applications.png)

3. Soldaki menüde **Kullanıcılar ve gruplar**' ı seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. **Kullanıcı Ekle** düğmesine tıklayın, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. **Kullanıcılar ve gruplar** Iletişim kutusunda kullanıcılar listesinde **Britta Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

6. SAML onaylama işlemi içinde herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, listeden Kullanıcı için uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

7. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

### <a name="create-rfpio-test-user"></a>RFıO test kullanıcısı oluşturma

1. RFıO şirket sitenizde yönetici olarak oturum açın.

1. Sol alt köşedeki aşağı açılan listeye tıklayın.

    ![Ekran görüntüsü bölmenin alt kısmındaki aşağı oku gösterir.](./media/rfpio-tutorial/app1.png)

1. **Kuruluş ayarları**' na tıklayın. 

    ![Ekran görüntüsü kuruluş ayarlarının seçili olduğunu gösterir.](./media/rfpio-tutorial/app2.png)

1. **Takım üyeleri**' ne tıklayın.

    ![Ekran görüntüsü, ayarlardan seçilen takım üyelerini gösterir.](./media/rfpio-tutorial/app6.png)

1. **Üye Ekle**' ye tıklayın.

    ![Ekran görüntüsü üye Ekle düğmesini gösterir.](./media/rfpio-tutorial/app7.png)

1. **Yeni Üyeler Ekle** bölümünde. Aşağıdaki eylemleri gerçekleştirin:

    ![Ekran görüntüsü, açıklanan değerleri girebileceğiniz yeni üye Ekle ' nin gösterir.](./media/rfpio-tutorial/app8.png)

    a. **Her satıra bir e-posta girin** alanına e-posta **adresini** girin.

    b. Lütfen gereksinimlerinize göre **rol** seçin.

    c. **Üye Ekle**' ye tıklayın.

    > [!NOTE]
    > Azure Active Directory hesap sahibi bir e-posta alır ve etkin hale gelmeden önce hesaplarını doğrulamak için bir bağlantıyı izler.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde RFıO kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız RFıO 'da otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](../user-help/my-apps-portal-end-user-access.md).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](./tutorial-list.md)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

- [Azure Active Directory Koşullu erişim nedir?](../conditional-access/overview.md)