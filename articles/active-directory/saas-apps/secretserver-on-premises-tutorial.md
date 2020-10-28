---
title: 'Öğretici: Secret Server (On-Premises) ile tümleştirme Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve Secret Server (On-Premises) arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 08/07/2019
ms.author: jeedes
ms.openlocfilehash: 9540922619864b0c239a22422bab5f6bf178e5fe
ms.sourcegitcommit: 4cb89d880be26a2a4531fedcc59317471fe729cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2020
ms.locfileid: "92678410"
---
# <a name="tutorial-integrate-secret-server-on-premises-with-azure-active-directory"></a>Öğretici: Secret Server (On-Premises) Azure Active Directory ile tümleştirin

Bu öğreticide, Secret Server (On-Premises) Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. Secret Server (On-Premises) Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de Secret Server (On-Premises) erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Secret Server (On-Premises) için otomatik olarak oturum açmalarına olanak sağlayın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

Azure AD ile SaaS uygulaması tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Çoklu oturum açma (SSO) özellikli abonelik Secret Server (On-Premises).

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz.

* Secret Server (On-Premises) **SP ve ıDP** tarafından başlatılan SSO 'yu destekler

## <a name="adding-secret-server-on-premises-from-the-gallery"></a>Galeriden Secret Server (On-Premises) ekleme

Secret Server (On-Premises) tümleştirmesini Azure AD ile yapılandırmak için, Galeriden Secret Server (On-Premises) yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. [Azure Portal](https://portal.azure.com) iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar** ' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama** ' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Secret Server (on-premises)** yazın.
1. Sonuçlar panelinden **Secret Server (on-premises)** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.


## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma ve test etme

**B. Simon** adlı bir test kullanıcısı kullanarak Azure AD SSO 'yu Secret Server (on-premises) ile yapılandırın ve test edin. SSO 'nun çalışması için, Secret Server (On-Premises) bir Azure AD kullanıcısı ve ilgili Kullanıcı arasında bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO 'yu Secret Server (On-Premises) ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını doldurun:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
2. Uygulama tarafında tek Sign-On ayarlarını yapılandırmak için **[Secret Server (on-PREMISES) SSO 'Yu yapılandırın](#configure-secret-server-on-premises-sso)** .
3. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
4. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
5. Kullanıcının Azure AD gösterimine bağlı olan Secret Server (On-Premises) bir B. Simon 'ya sahip olmak için **[Secret Server (on-premises) test kullanıcısı oluşturun](#create-secret-server-on-premises-test-user)** .
6. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. [Azure Portal](https://portal.azure.com/), **Secret Server (on-premises)** uygulama tümleştirmesi sayfasında, **Yönet** bölümünü bulun ve **Çoklu oturum açma** ' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML** ' yi seçin.
1. **SAML Ile tek Sign-On ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** için Düzenle/kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölümünde, **IDP** tarafından başlatılan modda uygulamayı yapılandırmak istiyorsanız aşağıdaki alanlar için değerleri girin:

    a. **Tanımlayıcı** metin kutusuna kullanıcı tarafından seçilen değeri bir örnek olarak girin:`https://secretserveronpremises.azure`

    b. **Yanıt URL 'si** metin kutusuna aşağıdaki kalıbı kullanarak bir URL yazın:`https://<SecretServerURL>/SAML/AssertionConsumerService.aspx`

    > [!NOTE]
    > Yukarıda gösterilen varlık KIMLIĞI yalnızca bir örnektir ve Azure AD 'de gizli sunucu örneğinizi tanımlayan benzersiz bir değer seçebilirsiniz. Bu varlık KIMLIĞINI [Secret Server (on-premises) istemci desteği ekibine](https://thycotic.force.com/support/s/) göndermeniz gerekir ve bunları tarafında yapılandıracaksınız. Daha fazla ayrıntı için lütfen [Bu makaleyi](https://thycotic.force.com/support/s/article/Configuring-SAML-in-Secret-Server)okuyun.

1. Uygulamayı **SP** tarafından başlatılan modda yapılandırmak Istiyorsanız **ek URL 'ler ayarla** ' ya tıklayın ve aşağıdaki adımı gerçekleştirin:

    **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<SecretServerURL>/login.aspx`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek yanıt URL 'si ve Sign-On URL 'siyle güncelleştirin. Bu değerleri almak için [Secret Server (on-premises) istemci destek ekibine](https://thycotic.force.com/support/s/) başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

1. **SAML Ile tekli Sign-On ayarlama** sayfasında, **SAML Imzalama sertifikası** bölümünde **sertifika bulun (base64)** ve sertifikayı indirip bilgisayarınıza kaydetmek için **İndir** ' i seçin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. **SAML Ile tek Sign-On ayarlama** sayfasında, **SAML imzalama sertifikası** Iletişim kutusunu açmak için **Düzenle** simgesine tıklayın.

    ![İmzalama seçenekleri](./media/secretserver-on-premises-tutorial/edit-saml-signon.png)

1. **Imza seçeneğini** belirtin **SAML yanıtı ve onaylama olarak imzala** .

    ![İmzalama seçenekleri](./media/secretserver-on-premises-tutorial/signing-option.png)

1. **Secret Server (on-premises) ayarla** bölümünde, gereksiniminize göre uygun URL 'leri kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

### <a name="configure-secret-server-on-premises-sso"></a>Secret Server (On-Premises) SSO 'yu yapılandırma

**Secret Server (on-premises)** tarafında çoklu oturum açmayı yapılandırmak için, indirilen **sertifikayı (base64)** ve uygun kopyalanmış URL 'leri Azure Portal [Secret Server (on-premises) destek ekibine](https://thycotic.force.com/support/s/)göndermeniz gerekir. Bu ayar, SAML SSO bağlantısının her iki tarafında da düzgün bir şekilde ayarlanmasını sağlamak üzere ayarlanmıştır.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, B. Simon adlı Azure portal bir test kullanıcısı oluşturacaksınız.

1. Azure portal sol bölmeden **Azure Active Directory** ' i seçin, **Kullanıcılar** ' ı seçin ve ardından **tüm kullanıcılar** ' ı seçin.
1. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.
1. **Kullanıcı** özellikleri ' nde şu adımları izleyin:
   1. **Ad** alanına `B.Simon` girin.  
   1. **Kullanıcı adı** alanına, girin username@companydomain.extension . Örneğin, `B.Simon@contoso.com`.
   1. **Parolayı göster** onay kutusunu seçin ve ardından **parola** kutusunda görüntülenen değeri yazın.
   1. **Oluştur** 'a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Secret Server (On-Premises) erişim vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar** ' ı seçin ve ardından **tüm uygulamalar** ' ı seçin.
1. Uygulamalar listesinde **Secret Server (on-premises)** ' yi seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar** ' ı seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle** ' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. SAML assertion 'da herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, Kullanıcı için listeden uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

### <a name="create-secret-server-on-premises-test-user"></a>Secret Server (On-Premises) test kullanıcısı oluştur

Bu bölümde, Secret Server (On-Premises) 'da Britta Simon adlı bir Kullanıcı oluşturacaksınız. Secret Server (On-Premises) platformunda kullanıcıları eklemek için [Secret Server (on-premises) destek ekibi](https://thycotic.force.com/support/s/) ile çalışın. Çoklu oturum açma kullanılmadan önce kullanıcıların oluşturulması ve etkinleştirilmesi gerekir.

### <a name="test-sso"></a>Test SSO 'SU

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde Secret Server (On-Premises) kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız Secret Server (On-Premises) otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](../user-help/my-apps-portal-end-user-access.md).

## <a name="additional-resources"></a>Ek kaynaklar

- [ SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi ](./tutorial-list.md)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir? ](../manage-apps/what-is-single-sign-on.md)

- [Azure Active Directory'de koşullu erişim nedir?](../conditional-access/overview.md)