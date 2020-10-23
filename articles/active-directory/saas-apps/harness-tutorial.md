---
title: 'Öğretici: Azure Active Directory çoklu oturum açma (SSO) Tümleştirmesi | Microsoft Docs'
description: Azure Active Directory ve bandı arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 09/02/2019
ms.author: jeedes
ms.openlocfilehash: d6a6c8b49582b34c2603e0ddf78b76736f97c183
ms.sourcegitcommit: 9b8425300745ffe8d9b7fbe3c04199550d30e003
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2020
ms.locfileid: "92445622"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-harness"></a>Öğretici: bandı ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory

Bu öğreticide, Azure Active Directory (Azure AD) ile bir bandı tümleştirmeyi öğreneceksiniz. Bandı Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de, bir bandı erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla otomatik olarak oturum açmalarına olanak sağlar.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

Azure AD ile SaaS uygulaması tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Çoklu oturum açma (SSO) aboneliği etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz.

* Bandı **SP ve ıDP** tarafından başlatılan SSO 'yu destekler

## <a name="adding-harness-from-the-gallery"></a>Galeriden bir bandı ekleme

Azure AD 'de bulunan bir bandı tümleştirmeyi yapılandırmak için, Galeriden, yönetilen SaaS uygulamaları listenize bir bandı eklemeniz gerekir.

1. [Azure Portal](https://portal.azure.com) iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **bir tür yazın** .
1. Sonuçlar panelinden bir **bandı** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-harness"></a>Azure AD çoklu oturum açmayı yapılandırma ve test etme

**B. Simon**adlı bir test kullanıcısı kullanarak Azure AD SSO 'yu, ile birlikte yapılandırın ve test edin. SSO 'nun çalışması için, bir Azure AD kullanıcısı ve bu kullanıcı ile ilgili Kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO 'yu ana ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını doldurun:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
    1. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
    1. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
1. Uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için, bir uygulama **[SSO 'Yu yapılandırın](#configure-harness-sso)** .
    1. Kullanıcı için Azure AD gösterimine bağlı olan, bu arada bir B. Simon 'a sahip olmak için, bir **[test kullanıcısı oluşturun](#create-harness-test-user)** .
1. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. [Azure Portal](https://portal.azure.com/), **Ara uygulama tümleştirmesi** sayfasında, **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** için Düzenle/kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölümünde, **IDP** tarafından başlatılan modda uygulamayı yapılandırmak istiyorsanız aşağıdaki alanlar için değerleri girin:

    **Yanıt URL 'si** metin kutusuna aşağıdaki kalıbı kullanarak bir URL yazın:`https://app.harness.io/gateway/api/users/saml-login?accountId=<harness_account_id>`

1. Uygulamayı **SP** tarafından başlatılan modda yapılandırmak Istiyorsanız **ek URL 'ler ayarla** ' ya tıklayın ve aşağıdaki adımı gerçekleştirin:

    **Oturum açma URL 'si** metin kutusuna bir URL yazın:`https://app.harness.io/`

    > [!NOTE]
    > Yanıt URL 'SI değeri gerçek değil. Gerçek yanıt URL 'sini, Öğreticinin ilerleyen kısımlarında **açıklanan, Get** Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, **Federasyon meta verileri XML** 'i bulun ve sertifikayı indirip bilgisayarınıza kaydetmek için **İndir** ' i seçin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

1. **Bandı ayarla** bölümünde, gereksiniminize göre uygun URL 'leri kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, B. Simon adlı Azure portal bir test kullanıcısı oluşturacaksınız.

1. Azure portal sol bölmeden **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.
1. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.
1. **Kullanıcı** özellikleri ' nde şu adımları izleyin:
   1. **Ad** alanına `B.Simon` girin.  
   1. **Kullanıcı adı** alanına, girin username@companydomain.extension . Örneğin, `B.Simon@contoso.com`.
   1. **Parolayı göster** onay kutusunu seçin ve ardından **parola** kutusunda görüntülenen değeri yazın.
   1. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, B. Simon 'u, imzalanacak erişim vererek Azure çoklu oturum açma özelliğini kullanacak şekilde etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde, **bandı**' ni seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. SAML assertion 'da herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, Kullanıcı için listeden uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

## <a name="configure-harness-sso"></a>Bir i 'yi yapılandırma bandı

1. Bu yapılandırmayı, bandı içinde otomatik hale getirmek için, **uzantıyı yüklemek**üzere **uygulamalarımı güvenli oturum açma tarayıcı uzantısı** ' nı yüklemeniz gerekir.

    ![Uygulamalarım uzantısı](common/install-myappssecure-extension.png)

2. Tarayıcıya uzantı ekledikten sonra **Kurulum bandı** ' ne tıklayın. Buradan, oturum açmak için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı, uygulamayı sizin için otomatik olarak yapılandırır ve 3-6 adımlarını otomatikleştirecektir.

    ![Kurulum yapılandırması](common/setup-sso.png)

3. Bandı el ile ayarlamak isterseniz, yeni bir Web tarayıcısı penceresi açın ve bir yönetici olarak, bir yönetici olarak oturum açın ve aşağıdaki adımları gerçekleştirin:

4. Sayfanın sağ üst kısmında, **sürekli güvenlik**  >  **erişimi yönetimi**  >  **kimlik doğrulama ayarları**' na tıklayın.

    !["Erişim yönetimi" ve "kimlik doğrulama ayarları" seçiliyken "sürekli güvenlik" menüsünü gösteren ekran görüntüsü.](./media/harness-tutorial/configure01.png)

5. **SSO sağlayıcıları** bölümünde, **+ SSO sağlayıcıları Ekle**  >  **SAML**' ye tıklayın.

    !["+ S o sağlayıcıları-S A M L}" seçiliyken "S S O sağlayıcılarını" gösteren ekran görüntüsü.](./media/harness-tutorial/configure03.png)

6. **SAML sağlayıcısı** açılır penceresinde aşağıdaki adımları uygulayın:

    !["U R l" ve "görünen ad" alanları vurgulanmış ve "Dosya Seç" ve "Gönder" düğmelerinin seçildiği ekran görüntüsü.](./media/harness-tutorial/configure02.png)

    a. **ÖĞESINI SSO sağlayıcınızda kopyalayın, lütfen SAML tabanlı oturum açmayı etkinleştirin, ardından AŞAĞıDAKI URL örneğini girin** ve Azure Portal **temel SAML YAPıLANDıRMASı** bölümünde yanıt URL metin kutusuna yapıştırın.

    b. **Görünen ad** metin kutusuna görünen adınızı yazın.

    c. Azure AD 'den indirdiğiniz Federasyon meta veri XML dosyasını karşıya yüklemek için **Dosya Seç** ' e tıklayın.

    d. **Gönder**' e tıklayın.

### <a name="create-harness-test-user"></a>Test kullanıcısı oluştur

Azure AD kullanıcılarının, ana oturum açmasını sağlamak için, bu kullanıcıların, ana ' a sağlanması gerekir. Bu durumda, sağlama el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Yönetici olarak oturum açın.

1. Sayfanın sağ üst kısmında, **sürekli güvenlik**  >  **erişimi yönetimi**  >  **kullanıcıları**' na tıklayın.

    !["Erişim yönetimi" ve "kullanıcılar" seçiliyken "sürekli güvenlik" menüsünü gösteren ekran görüntüsü.](./media/harness-tutorial/configure04.png)

1. Sayfanın sağ tarafında, **+ Kullanıcı Ekle**' ye tıklayın.

    !["+ Kullanıcı Ekle" eylemi seçiliyken "kullanıcılar" sayfasını gösteren ekran görüntüsü.](./media/harness-tutorial/configure05.png)

1. **Kullanıcı Ekle** açılır penceresinde aşağıdaki adımları uygulayın:

    ![Bandı yapılandırma](./media/harness-tutorial/configure06.png)

    a. **E-posta adresi (es)** metin kutusuna kullanıcının e-postasını girin `B.simon@contoso.com` .

    b. **Kullanıcı gruplarınızı**seçin.

    c. **Gönder**' e tıklayın.

## <a name="test-sso"></a>Test SSO 'SU 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde bir ara Kutucuğa tıkladığınızda, SSO 'yu ayarladığınız bir şekilde otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](../user-help/my-apps-portal-end-user-access.md).

## <a name="additional-resources"></a>Ek kaynaklar

- [ SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi ](./tutorial-list.md)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir? ](../manage-apps/what-is-single-sign-on.md)

- [Azure Active Directory'de koşullu erişim nedir?](../conditional-access/overview.md)

- [Azure AD ile bir bandı deneyin](https://aad.portal.azure.com/)