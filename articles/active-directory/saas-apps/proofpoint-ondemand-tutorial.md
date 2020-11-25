---
title: 'Öğretici: Isteğe bağlı olarak yazım Azure Active Directory tümleştirme | Microsoft Docs'
description: Isteğe bağlı Azure Active Directory ve düzeltme noktası arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 12/31/2018
ms.author: jeedes
ms.openlocfilehash: 673856800ccb05a2b4b52511777a9a24769bcd6f
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "95993610"
---
# <a name="tutorial-azure-active-directory-integration-with-proofpoint-on-demand"></a>Öğretici: Isteğe bağlı olarak, denetleme noktası ile tümleştirme Azure Active Directory

Bu öğreticide, Azure Active Directory (Azure AD) ile Isteğe bağlı olarak, redaksyonun nasıl tümleştirileceğini öğreneceksiniz.
Azure AD ile Istek üzerine Redaknme noktasını tümleştirmek aşağıdaki avantajları sağlar:

* Azure AD 'de, Isteğe bağlı olarak yazım denetimi erişimine sahip bir denetim yapabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla Istek üzerine (çoklu oturum açma) otomatik olarak oturum açmasını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz-Azure portal.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek istiyorsanız, bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesini Isteğe bağlı bir Istek üzerine yapılandırma için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Bir Azure AD ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü edinebilirsiniz
* Isteğe bağlı çoklu oturum açma aboneliğindeki denetleme noktası

> [!NOTE]
> MFA veya passwordless kimlik doğrulamasını Azure AD ile kullanıyorsanız, SAML Isteğindeki AuthnContext değerini kapatın. Aksi takdirde, Azure AD, AuthnContext uyuşmazlığından sonra hatayı oluşturur ve belirteci uygulamaya geri göndermez.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

* Istek üzerine denetleme noktası **SP** tarafından başlatılan SSO 'yu destekler

## <a name="adding-proofpoint-on-demand-from-the-gallery"></a>Galeriden Istek üzerine Redakkpoint ekleme

Isteğe bağlı olarak, Azure AD 'ye Istek üzerine redaksyonun tümleştirmesini yapılandırmak için, galerinizden yönetilen SaaS uygulamaları listenize Isteğe bağlı olarak Redakz noktası eklemeniz gerekir.

**Galeriden Istek üzerine Redakz noktası eklemek için aşağıdaki adımları uygulayın:**

1. **[Azure Portal](https://portal.azure.com)** sol gezinti panelinde **Azure Active Directory** simgesine tıklayın.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar** seçeneğini belirleyin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için, iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesine tıklayın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **isteğe bağlı yazım noktası** yazın, sonuç panelinden Istek üzerine **Yazım noktası** ' nı seçin ve sonra uygulamayı eklemek için düğme **Ekle** ' ye tıklayın.

     ![Sonuçlar listesinde Isteğe bağlı bir denetleme noktası](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma ve test etme

Bu bölümde, **Britta Simon** adlı bir test kullanıcısına göre isteğe bağlı olarak, Azure AD çoklu oturum açmayı yapılandırıp test edersiniz.
Çoklu oturum açma için, bir Azure AD kullanıcısı ve Isteğe bağlı yazım noktasında ilgili Kullanıcı arasındaki bağlantı ilişkisinin kurulması gerekir.

Azure AD 'nin Isteğe bağlı olarak Redaklala çoklu oturum açmayı yapılandırmak ve test etmek için aşağıdaki yapı taşlarını gerçekleştirmeniz gerekir:

1. **[Azure AD çoklu oturum açma özelliğini yapılandırarak](#configure-azure-ad-single-sign-on)** kullanıcılarınızın bu özelliği kullanmasına olanak sağlayın.
2. , Uygulama tarafında tek Sign-On ayarlarını yapılandırmak için, **[isteğe bağlı çoklu oturum açma üzerinde Redakpoint 'ı yapılandırın](#configure-proofpoint-on-demand-single-sign-on)** .
3. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -Britta Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
4. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanarak Britta Simon 'u etkinleştirin.
5. Kullanıcının Azure AD gösterimine bağlı Isteğe bağlı olarak, yazım noktasında bir Britta Simon 'a sahip olmak için, Isteğe bağlı test kullanıcısına yönelik bir istek üzerine **[denetleme noktası oluşturun](#create-proofpoint-on-demand-test-user)** .
6. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[Çoklu oturum açmayı sınayın](#test-single-sign-on)** .

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure portal Azure AD çoklu oturum açma özelliğini etkinleştirirsiniz.

Azure AD 'nin Isteğe bağlı olarak, yazım noktasında çoklu oturum açmayı yapılandırmak için aşağıdaki adımları uygulayın:

1. [Azure Portal](https://portal.azure.com/), **istek üzerine yazım** tabanlı uygulama tümleştirmesi sayfasında, **Çoklu oturum açma**' yı seçin.

    ![Çoklu oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Çoklu oturum **açma yöntemi seç** iletişim kutusunda, çoklu oturum açmayı etkinleştirmek için **SAML/WS-Besme** modunu seçin.

    ![Çoklu oturum açma seçme modu](common/select-saml-option.png)

3. **SAML Ile tek Sign-On ayarlama** sayfasında, **temel SAML yapılandırması** Iletişim kutusunu açmak için **Düzenle** simgesine tıklayın.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. **Temel SAML yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Istek üzerine yazım tabanlı etki alanı ve URL 'Ler çoklu oturum açma bilgileri](common/sp-identifier-reply.png)

    a. **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<hostname>.pphosted.com/ppssamlsp_hostname`

    b. **Tanımlayıcı** kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<hostname>.pphosted.com/ppssamlsp`

    c. **Yanıt URL 'si** metin kutusuna aşağıdaki kalıbı kullanarak bir URL yazın:`https://<hostname>.pphosted.com:portnumber/v1/samlauth/samlconsumer`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Sign-On URL 'SI, tanımlayıcı ve yanıt URL 'siyle güncelleştirin. Bu değerleri almak için [Isteğe bağlı, istek üzerine istemci desteği ekibine](https://www.proofpoint.com/us/support-services) başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

5. **SAML Ile tek Sign-On ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, **sertifika (base64)** ' i gereksiniminize göre ve bilgisayarınıza kaydetmek için **İndir** ' e tıklayın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. **İsteğe bağlı yazım noktası ayarlama** bölümünde uygun URL 'leri gereksiniminize göre kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD tanımlayıcısı

    c. Oturum kapatma URL 'SI

### <a name="configure-proofpoint-on-demand-single-sign-on"></a>Isteğe bağlı tek Sign-On için bir Istek üzerine yazım yapılandırma

**İsteğe bağlı yazım noktasında** çoklu oturum açmayı yapılandırmak için, indirilen **sertifikayı (Base64)** ve Azure Portal ' den uygun kopyalanmış URL 'leri [isteğe bağlı destek ekibine](https://www.proofpoint.com/us/support-services)göndermeniz gerekir. Bu ayar, SAML SSO bağlantısının her iki tarafında da düzgün bir şekilde ayarlanmasını sağlamak üzere ayarlanmıştır.

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

    d. **Oluştur**'a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Isteğe bağlı olarak yazım tabanlı erişim sağlayarak Azure çoklu oturum açma özelliğini kullanmak için Britta Simon 'u etkinleştirin.

1. Azure portal **Kurumsal uygulamalar**' ı seçin, **tüm uygulamalar**' ı seçin ve **Isteğe bağlı olarak redakþ noktası**' nı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde, **isteğe bağlı olarak Redakta noktası**' nı seçin.

    ![Uygulamalar listesindeki Istek üzerine yazım denetleme bağlantısı](common/all-applications.png)

3. Soldaki menüde **Kullanıcılar ve gruplar**' ı seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. **Kullanıcı Ekle** düğmesine tıklayın, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. **Kullanıcılar ve gruplar** Iletişim kutusunda kullanıcılar listesinde **Britta Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

6. SAML onaylama işlemi içinde herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, listeden Kullanıcı için uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

7. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

### <a name="create-proofpoint-on-demand-test-user"></a>Isteğe bağlı test kullanıcısına yönelik yazım noktası oluştur

Bu bölümde, Istek üzerine yazım noktasında Britta Simon adlı bir Kullanıcı oluşturacaksınız. Istek üzerine yazım noktasında Kullanıcı eklemek için, isteğe bağlı, [istek üzerine istemci destek ekibi](https://www.proofpoint.com/us/support-services) ile çalışın.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde Isteğe bağlı yazım noktası kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız Istek üzerine otomatik olarak bir denetleme noktasında oturum açmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](../user-help/my-apps-portal-end-user-access.md).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](./tutorial-list.md)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

- [Azure Active Directory Koşullu erişim nedir?](../conditional-access/overview.md)