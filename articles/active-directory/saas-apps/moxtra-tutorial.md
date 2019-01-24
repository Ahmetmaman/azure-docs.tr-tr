---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Moxtra | Microsoft Docs'
description: Azure Active Directory ve Moxtra arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 2aed2d4b-1dcd-4839-8fed-9419d107c61c
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: jeedes
ms.openlocfilehash: ee8931f1f9121f3e645b2f94eece919ae6b19075
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54808859"
---
# <a name="tutorial-azure-active-directory-integration-with-moxtra"></a>Öğretici: Moxtra ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Moxtra tümleştirme konusunda bilgi edinin.

Azure AD ile Moxtra tümleştirme ile aşağıdaki avantajları sağlar:

- Moxtra erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için Moxtra (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Moxtra yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik Moxtra çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Moxtra ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-moxtra-from-the-gallery"></a>Galeriden Moxtra ekleme
Azure AD'de Moxtra tümleştirmesini yapılandırmak için Moxtra Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Moxtra eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **Moxtra**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/moxtra-tutorial/tutorial_moxtra_search.png)

1. Sonuçlar panelinde seçin **Moxtra**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/moxtra-tutorial/tutorial_moxtra_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Moxtra sınayın.

Tek iş için oturum açma için Azure AD ne Moxtra karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Moxtra ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Moxtra içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Moxtra ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Moxtra test kullanıcısı oluşturma](#creating-a-moxtra-test-user)**  - kullanıcı Azure AD gösterimini bağlı Moxtra Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Moxtra uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile Moxtra yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Moxtra** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/moxtra-tutorial/tutorial_moxtra_samlbase.png)

1. Üzerinde **Moxtra etki alanı ve URL'ler** bölümünde, aşağıdaki adımı uygulayın:

    ![Çoklu oturum açmayı yapılandırın](./media/moxtra-tutorial/tutorial_moxtra_url.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL: `https://www.moxtra.com/service/#login`

1. Moxtra uygulama belirli bir biçimde SAML onaylamalarını bekler. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu öznitelikleri değerlerini yönetebilirsiniz "**kullanıcı öznitelikleri**" uygulama tümleştirme sayfasında bölümü. Aşağıdaki ekran görüntüsünde, bu yapılandırma için bir örnek gösterilmektedir. 

    ![Çoklu oturum açmayı yapılandırın](./media/moxtra-tutorial/tutorial_moxtra_attributes.png)
    
1. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda, SAML belirteci özniteliği resimde gösterildiği gibi yapılandırın ve aşağıdaki adımları gerçekleştirin:
    
    | Öznitelik Adı | Öznitelik Değeri |
    | ------------------- | -------------------- |    
    | firstName | User.givenName |
    | Soyadı | User.surname |
    | idpid    | < SAML varlık kimliği > 

    > [!Note]
    > Değerini **idpid** özniteliği gerçek değil. Gelen gerçek değer elde edebileceği **hızlı başvuru** bölümüne **Moxtra yapılandırma**.
    
    a. Tıklayın **eklemek agentconfigutil** açmak için **öznitelik Ekle** iletişim.

    ![Çoklu oturum açmayı yapılandırın](./media/moxtra-tutorial/tutorial_attribute_04.png)

    b. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.

    ![Çoklu oturum açmayı yapılandırın](./media/moxtra-tutorial/tutorial_attribute_05.png)

    c. Gelen **değer** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    d. **Tamam**’a tıklayın.
    
1. Üzerinde **SAML imzalama sertifikası** bölümünde **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/moxtra-tutorial/tutorial_moxtra_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/moxtra-tutorial/tutorial_general_400.png)

1. Üzerinde **Moxtra yapılandırma** bölümünde **yapılandırma Moxtra** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/moxtra-tutorial/tutorial_moxtra_configure.png) 

1. Başka bir tarayıcı penceresinde Moxtra şirketinizin sitesi için bir yönetici olarak oturum açın.

1. Sol taraftaki araç çubuğunda tıklatın **Yönetici Konsolu > SAML çoklu oturum açma**ve ardından **yeni**.
   
    ![Çoklu oturum açmayı yapılandırın](./media/moxtra-tutorial/tutorial_moxtra_06.png) 

1. Üzerinde **SAML** sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![Çoklu oturum açmayı yapılandırın](./media/moxtra-tutorial/tutorial_moxtra_08.png)   
 
    a. İçinde **adı** metin yapılandırmanız için bir ad yazın (örneğin: *SAML*). 
  
    b. İçinde **IDP varlık kimliği** metin değerini yapıştırın **SAML varlık kimliği** , Azure Portalı'ndan kopyaladığınız. 
 
    c. İçinde **oturum açma URL'si** metin değerini yapıştırın **SAML çoklu oturum açma hizmeti URL'si** , Azure Portalı'ndan kopyaladığınız. 
 
    d. İçinde **AuthnContextClassRef** metin kutusuna **urn: OASIS: adları: tc: SAML:2.0:ac:classes:Password**. 
 
    e. İçinde **Nameıd biçimi** metin kutusuna **urn: OASIS: adları: tc: SAML:1.1:nameid-biçimi: emailAddress**. 
 
    f. Not Defteri'nde, Azure portalından indirilen açık sertifika içeriği kopyalayın ve ardından yapıştırın **sertifika** metin.    
 
    g. SAML e-posta etki alanınıza SAML e-posta etki alanı metin kutusuna yazın.    
  
    >[!NOTE]
    >Etki alanını doğrulamak için gereken adımları görmek için tıklayın "**miyim**" altında.

    h. Tıklayın **güncelleştirme**.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/moxtra-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/moxtra-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/moxtra-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/moxtra-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-moxtra-test-user"></a>Moxtra test kullanıcısı oluşturma

Bu bölümün amacı Moxtra Britta Simon adlı bir kullanıcı oluşturmaktır.

**Britta Simon Moxtra içinde adlı bir kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Moxtra şirketinizin sitesi için bir yönetici olarak oturum açın.

1. Sol taraftaki araç çubuğunda tıklatın **Yönetici Konsolu > Kullanıcı Yönetimi**, ardından **Kullanıcı Ekle**.
   
    ![Çoklu oturum açmayı yapılandırın](./media/moxtra-tutorial/tutorial_moxtra_10.png) 

1. Üzerinde **Kullanıcı Ekle** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
  
    a. İçinde **ad** metin kutusuna **Britta**.
  
    b. İçinde **Soyadı** metin kutusuna **Simon**.
  
    c. İçinde **e-posta** metin kutusuna Britta'nın e-posta adresi ile aynı Azure portalında.
  
    d. İçinde **bölme** metin kutusuna **geliştirme**.
  
    e. İçinde **departmanı** metin kutusuna **BT**.
  
    f. Seçin **yönetici**.
  
    g. **Ekle**'ye tıklayın.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için Moxtra erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon Moxtra için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Moxtra**.

    ![Çoklu oturum açmayı yapılandırın](./media/moxtra-tutorial/tutorial_moxtra_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Moxtra kutucuğa tıkladığınızda, otomatik olarak Moxtra uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/moxtra-tutorial/tutorial_general_01.png
[2]: ./media/moxtra-tutorial/tutorial_general_02.png
[3]: ./media/moxtra-tutorial/tutorial_general_03.png
[4]: ./media/moxtra-tutorial/tutorial_general_04.png

[100]: ./media/moxtra-tutorial/tutorial_general_100.png

[200]: ./media/moxtra-tutorial/tutorial_general_200.png
[201]: ./media/moxtra-tutorial/tutorial_general_201.png
[202]: ./media/moxtra-tutorial/tutorial_general_202.png
[203]: ./media/moxtra-tutorial/tutorial_general_203.png

