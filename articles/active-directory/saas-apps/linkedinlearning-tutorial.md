---
title: 'Öğretici Azure Active Directory: LinkedIn Learning ile çoklu oturum açma (SSO) Tümleştirmesi | Microsoft Docs'
description: Azure Active Directory ve LinkedIn öğrenimi arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
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
ms.openlocfilehash: fde0bcdb70384e791d7337d212d7353b6ea168bf
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2020
ms.locfileid: "91856420"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-linkedin-learning"></a>Öğretici: LinkedIn Learning ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory

Bu öğreticide, LinkedIn öğrenimini Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. LinkedIn öğrenimini Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de LinkedIn öğrenimine erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesapları ile LinkedIn öğrenimine otomatik olarak kaydolmalarına imkan tanıyın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

Azure AD ile SaaS uygulaması tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* LinkedIn Learning çoklu oturum açma (SSO) etkin aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz.

* LinkedIn Learning **, SP ve ıDP** tarafından başlatılan SSO 'yu destekler
* LinkedIn Learning **, tam zamanında** Kullanıcı sağlamayı destekler
* LinkedIn öğrenimini yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak ayıklanmasını ve bu verileri korumayı koruyan oturum denetimini zorunlu kılabilirsiniz. Oturum denetimi koşullu erişimden genişletilir. [Microsoft Cloud App Security ile oturum denetimini nasıl zorlayacağınızı öğrenin](https://docs.microsoft.com/cloud-app-security/proxy-deployment-aad)

## <a name="adding-linkedin-learning-from-the-gallery"></a>Galeriden LinkedIn öğrenme ekleme

LinkedIn Learning 'in Azure AD ile tümleştirilmesini yapılandırmak için, galerinizden yönetilen SaaS uygulamaları listenize LinkedIn öğrenimini eklemeniz gerekir.

1. [Azure Portal](https://portal.azure.com) iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **LinkedIn Learning** yazın.
1. Sonuçlar panelinden **LinkedIn Learning** ' i seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.


## <a name="configure-and-test-azure-ad-single-sign-on-for-linkedin-learning"></a>LinkedIn Learning için Azure AD çoklu oturum açmayı yapılandırma ve test etme

**B. Simon**adlı bir test kullanıcısı kullanarak LinkedIn Ile Azure AD SSO 'yu yapılandırın ve test edin. SSO 'nun çalışması için, LinkedIn Learning 'de bir Azure AD kullanıcısı ve ilgili Kullanıcı arasında bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO 'yu LinkedIn öğrenimi ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını doldurun:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
    1. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
    1. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
1. **[LinkedIn LEARNING SSO 'Yu yapılandırma](#configure-linkedin-learning-sso)** -uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
    1. LinkedIn Learning 'de kullanıcının Azure AD gösterimine bağlanmış bir B. Simon 'ya sahip olmak için **[LinkedIn öğrenme testi kullanıcısı oluşturun](#create-linkedin-learning-test-user)** .
1. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. [Azure Portal](https://portal.azure.com/), **LinkedIn öğrenme** uygulaması tümleştirmesi sayfasında **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** için Düzenle/kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölümünde, **IDP** tarafından başlatılan modda uygulamayı yapılandırmak istiyorsanız aşağıdaki alanlar için değerleri girin:

     a. **Tanımlayıcı** metin kutusunda, LinkedIn portalından KOPYALANMıŞ **varlık kimliğini** girin. 

    b. **Yanıt URL 'si** metin kutusuna, LinkedIn portalından kopyalanmış **TÜKETICI hizmeti (ACS) URL 'sini** girin.

    c. Uygulamayı **SP tarafından başlatılan** modda yapılandırmak istiyorsanız, oturum açma URL 'Nizi BELIRTEBILECEĞINIZ **temel SAML yapılandırması** bölümünde **ek URL 'ler ayarla** seçeneğine tıklayın. Oturum açma URL 'nizi oluşturmak için, **onaylama tüketici hizmeti (ACS) URL 'sini** kopyalayın ve/SAML/değerini/login/ile değiştirin. Bu işlem yapıldıktan sonra oturum açma URL 'SI aşağıdaki düzende olmalıdır:

    `https://www.linkedin.com/checkpoint/enterprise/login/<AccountId>?application=learning&applicationInstanceId=<InstanceId>`

    ![LinkedIn Learning etki alanı ve URL 'Ler çoklu oturum açma bilgileri](common/metadata-upload-additional-signon.png)

    > [!NOTE]
    > Bu değerler gerçek değer değildir. Bu değerleri, öğreticinin daha sonra **LinkedIn öğrenme SSO 'Yu yapılandırma** bölümünde açıklanan gerçek tanımlayıcı ve yanıt URL 'si ile güncelleşceksiniz.

1. LinkedIn öğrenme uygulaması, SAML belirteci öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML onayları bekler. Aşağıdaki ekran görüntüsünde, **NameIdentifier** 'ın **User. UserPrincipalName**ile eşlendiği varsayılan özniteliklerin listesi gösterilmektedir. LinkedIn Learning uygulaması, **NameIdentifier** 'ın **User. Mail**ile eşlenmesini bekler, bu nedenle, **Düzenle** simgesine tıklayarak ve öznitelik eşlemesini değiştirerek öznitelik eşlemesini düzenlemeniz gerekir.

    ![image](common/edit-attribute.png)

1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, **Federasyon meta verileri XML** 'i bulun ve sertifikayı indirip bilgisayarınıza kaydetmek için **İndir** ' i seçin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

1. **LinkedIn öğrenimini ayarla** bölümünde, gereksiniminize göre uygun URL 'leri kopyalayın.

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

Bu bölümde, LinkedIn Learning 'e erişim izni vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde **LinkedIn Learning**' i seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. SAML assertion 'da herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, Kullanıcı için listeden uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

## <a name="configure-linkedin-learning-sso"></a>LinkedIn Learning SSO 'yu yapılandırma

1. Farklı bir Web tarayıcısı penceresinde, LinkedIn öğrenme kiracınızda yönetici olarak oturum açın.

2. **Hesap Merkezi**'nde **Ayarlar**' ın altında **Genel ayarlar** ' a tıklayın. Ayrıca, açılır listeden **öğrenme-varsayılan** öğesini seçin.

    ![Ekran görüntüsü varsayılan seçebileceğiniz genel ayarları gösterir.](./media/linkedinlearning-tutorial/tutorial_linkedin_admin_01.png)

3. Form ve **varlık kimliği** ve **onaylama TÜKETICI hizmeti (ACS) URL** **'sinden ayrı alanları yüklemek ve kopyalamak için buraya tıklayın veya tıklayın** ve Azure Portal içindeki **temel SAML yapılandırması** bölümüne yapıştırın.

    ![Ekran görüntüsü, açıklanan değerleri girebileceğiniz tek Sign-On gösterir.](./media/linkedinlearning-tutorial/tutorial_linkedin_admin_03.png)

4. **LinkedIn yönetici ayarları** bölümüne gidin. **XML dosyasını karşıya yükle** seçeneğine tıklayarak Azure Portal indirdiğiniz XML dosyasını karşıya yükleyin.

    ![Ekran görüntüsü, bir X M L dosyasını karşıya yükleyebileceğiniz LinkedIn Service Provider S S O ayarlarını yapılandırmayı gösterir.](./media/linkedinlearning-tutorial/tutorial_linkedin_metadata_03.png)

5. SSO 'yu **etkinleştirmek için tıklayın** . SSO durum, bağlı **değil** olarak değişir **Connected**

    ![Ekran görüntüsü, S S ile kullanıcıların kimlik doğrulamasını etkinleştirebileceğiniz tek bir Sign-On gösterir.](./media/linkedinlearning-tutorial/tutorial_linkedin_admin_05.png)

### <a name="create-linkedin-learning-test-user"></a>LinkedIn öğrenme testi kullanıcısı oluşturma

LinkedIn öğrenme uygulaması, tam zamanında Kullanıcı sağlamayı ve kimlik doğrulama kullanıcılarının uygulamada otomatik olarak oluşturulmasını destekler. LinkedIn öğrenme portalındaki yönetici ayarları sayfasında, anahtarı ters çevir, otomatik olarak etkin bir şekilde sağlamak üzere **Lisansları atar** ve bu da kullanıcıya bir lisans atar.

   ![Azure AD test kullanıcısı oluşturma](./media/linkedinlearning-tutorial/LinkedinUserprovswitch.png)

## <a name="test-sso"></a>Test SSO 'SU 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde LinkedIn öğrenme kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız LinkedIn öğrenimi için otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [ SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile LinkedIn öğrenimini deneyin](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)