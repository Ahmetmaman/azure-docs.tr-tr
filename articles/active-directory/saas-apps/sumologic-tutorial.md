---
title: 'Öğretici: SumoLogic ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve SumoLogic arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 01/03/2020
ms.author: jeedes
ms.openlocfilehash: 2dcc52688cabebaa6eb813e3240150ea8774e716
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "92521907"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-sumologic"></a>Öğretici: SumoLogic ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory

Bu öğreticide, SumoLogic 'i Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. SumoLogic 'i Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de SumoLogic 'e erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla SumoLogic otomatik olarak oturum açmalarına olanak tanıyın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

Azure AD ile SaaS uygulaması tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* SumoLogic çoklu oturum açma (SSO) etkin abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz.

* SumoLogic, **IDP** tarafından başlatılan SSO 'yu destekler

## <a name="adding-sumologic-from-the-gallery"></a>Galeriden SumoLogic ekleme

SumoLogic tümleştirmesini Azure AD 'ye göre yapılandırmak için, Galeriden SumoLogic yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

1. [Azure Portal](https://portal.azure.com) iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **SumoLogic** yazın.
1. Sonuçlar panelinden **SumoLogic** ' i seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-sumologic"></a>SumoLogic için Azure AD çoklu oturum açmayı yapılandırma ve test etme

**B. Simon** adlı bir test kullanıcısı kullanarak Azure AD SSO 'yu SumoLogic ile yapılandırın ve test edin. SSO 'nun çalışması için, SumoLogic içinde bir Azure AD kullanıcısı ve ilgili Kullanıcı arasında bir bağlantı ilişkisi oluşturmanız gerekir.

Azure AD SSO 'yu SumoLogic ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını doldurun:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
    * Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
    * Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
1. Uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için **[SUMOLOGIC SSO 'Yu yapılandırın](#configure-sumologic-sso)** .
    * Kullanıcının Azure AD gösterimine bağlı olan SumoLogic 'de B. Simon 'ya karşılık gelen bir **[SumoLogic test kullanıcısı oluşturun](#create-sumologic-test-user)** .
1. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. [Azure Portal](https://portal.azure.com/), **SumoLogic** uygulama tümleştirmesi sayfasında **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** için Düzenle/kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, aşağıdaki alanlar için değerleri girin:

    a. **Tanımlayıcı** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:

    - `https://service.sumologic.com`
    - `https://<tenantname>.us2.sumologic.com`
    - `https://<tenantname>.us4.sumologic.com`
    - `https://<tenantname>.eu.sumologic.com`
    - `https://<tenantname>.jp.sumologic.com`
    - `https://<tenantname>.de.sumologic.com`
    - `https://<tenantname>.ca.sumologic.com`

    b. **Yanıt URL 'si** metin kutusuna aşağıdaki kalıbı kullanarak bir URL yazın:

    - `https://service.sumologic.com/sumo/saml/consume/<tenantname>`
    - `https://service.us2.sumologic.com/sumo/saml/consume/<tenantname>`
    - `https://service.us4.sumologic.com/sumo/saml/consume/<tenantname>`
    - `https://service.eu.sumologic.com/sumo/saml/consume/<tenantname>`
    - `https://service.jp.sumologic.com/sumo/saml/consume/<tenantname>`
    - `https://service.de.sumologic.com/sumo/saml/consume/<tenantname>`
    - `https://service.ca.sumologic.com/sumo/saml/consume/<tenantname>`
    - `https://service.au.sumologic.com/sumo/saml/consume/<tenantname>`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek tanımlayıcı ve yanıt URL 'siyle güncelleştirin. Bu değerleri almak için [SumoLogic istemci destek ekibine](https://www.sumologic.com/contact-us/) başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

1. SumoLogic uygulaması, SAML belirteci öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML onayları bekler. Aşağıdaki ekran görüntüsünde varsayılan özniteliklerin listesi gösterilmektedir.

    ![image](common/default-attributes.png)

1. SumoLogic uygulaması, yukarıdakine ek olarak, aşağıda gösterilen SAML yanıtına daha fazla öznitelik geçirilmesini bekler. Bu öznitelikler de önceden doldurulur, ancak gereksinimlerinize göre bunları gözden geçirebilirsiniz.

    |  Name | Kaynak özniteliği |
    | ---------------| --------------- |
    | FirstName | Kullanıcı. |
    | LastName | User. soyadı |
    | Roller | Kullanıcı. atandroles |

    > [!NOTE]
    > Azure AD 'de **rolün** nasıl yapılandırılacağını öğrenmek için lütfen [buraya](../develop/active-directory-enterprise-app-role-management.md) tıklayın.

1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, **SAML Imzalama sertifikası** bölümünde **sertifika bulun (base64)** ve sertifikayı indirip bilgisayarınıza kaydetmek için **İndir** ' i seçin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. **SumoLogic ayarla** bölümünde, gereksiniminize göre uygun URL 'leri kopyalayın.

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

Bu bölümde, SumoLogic 'e erişim vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde **SumoLogic**' yi seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. SAML assertion 'da herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, Kullanıcı için listeden uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

## <a name="configure-sumologic-sso"></a>SumoLogic SSO 'yu yapılandırma

1. Farklı bir Web tarayıcısı penceresinde, SumoLogic şirket sitenizde yönetici olarak oturum açın.

1. **\> Güvenliği Yönet**' e gidin.

    ![Yönetme](./media/sumologic-tutorial/ic778556.png "Yönetme")

1. **SAML**' ye tıklayın.

    ![Genel güvenlik ayarları](./media/sumologic-tutorial/ic778557.png "Genel güvenlik ayarları")

1. **Yapılandırma seçin veya yeni** bir liste oluşturun LISTESINDEN **Azure AD**' ı seçin ve ardından **Yapılandır**' a tıklayın.

    ![Ekran görüntüsü, Azure A 'nın seçebileceğiniz SAML 2,0 ' i gösterir.](./media/sumologic-tutorial/ic778558.png "SAML 2,0 yapılandırma")

1. **SAML 2,0 yapılandırma** iletişim kutusunda aşağıdaki adımları gerçekleştirin:

    ![Ekran görüntüsünde, açıklanan değerleri girebileceğiniz SAML 2,0 yapılandırma iletişim kutusu gösterilir.](./media/sumologic-tutorial/ic778559.png "SAML 2,0 yapılandırma")

    a. **Yapılandırma adı** metin kutusuna **Azure AD** yazın.

    b. **Hata ayıklama modu**' nu seçin.

    c. **Veren** metin kutusunda, Azure Portal KOPYALADıĞıNıZ **Azure AD tanımlayıcısının** değerini yapıştırın.

    d. **AuthN istek URL 'si** metin kutusunda, Azure Portal kopyaladığınız **oturum açma URL 'si** değerini yapıştırın.

    e. Base-64 kodlu sertifikanızı Not defteri 'nde açın, bu içeriği panonuza kopyalayın ve ardından tüm sertifikayı **X. 509.440 sertifikası** metin kutusuna yapıştırın.

    f. **E-posta özniteliği** olarak, **SAML Subject kullan**' ı seçin.  

    örneğin: **SP tarafından başlatılan oturum açma yapılandırmasını** seçin.

    h. **Oturum açma yolu** metin kutusuna **Azure** yazın ve **Kaydet**' e tıklayın.

### <a name="create-sumologic-test-user"></a>SumoLogic test kullanıcısı oluştur

Azure AD kullanıcılarının SumoLogic 'de oturum açmasını sağlamak için, SumoLogic ' a sağlanması gerekir. SumoLogic durumunda sağlama, el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. **SumoLogic** kiracınızda oturum açın.

1. **\> Kullanıcıları Yönet**' e gidin.

    ![Ekran görüntüsü Yönet menüsünden seçilen kullanıcıları gösterir.](./media/sumologic-tutorial/ic778561.png "Kullanıcılar")

1. **Ekle**'ye tıklayın.

    ![Ekran görüntüsü kullanıcılar için Ekle düğmesini gösterir.](./media/sumologic-tutorial/ic778562.png "Kullanıcılar")

1. **Yeni Kullanıcı** iletişim kutusunda aşağıdaki adımları gerçekleştirin:

    ![Yeni Kullanıcı](./media/sumologic-tutorial/ic778563.png "Yeni Kullanıcı")

    a. Sağlamak istediğiniz Azure AD hesabının **ad**, **Soyadı** ve **e-posta** kutularına bilgilerini yazın.
  
    b. Bir rol seçin.
  
    c. **Durum** olarak **etkin**' i seçin.
  
    d. **Kaydet**’e tıklayın.

> [!NOTE]
> Azure AD Kullanıcı hesaplarını sağlamak için SumoLogic tarafından sunulan diğer tüm SumoLogic Kullanıcı hesabı oluşturma araçlarını veya API 'Leri kullanabilirsiniz.

## <a name="test-sso"></a>Test SSO 'SU

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde SumoLogic kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız SumoLogic için otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](../user-help/my-apps-portal-end-user-access.md).

## <a name="additional-resources"></a>Ek kaynaklar

- [ SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi ](./tutorial-list.md)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir? ](../manage-apps/what-is-single-sign-on.md)

- [Azure Active Directory'de koşullu erişim nedir?](../conditional-access/overview.md)

- [Azure AD ile SumoLogic deneyin](https://aad.portal.azure.com/)