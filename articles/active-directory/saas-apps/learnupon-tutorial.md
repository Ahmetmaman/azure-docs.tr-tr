---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile LearnUpon | Microsoft Docs'
description: Azure Active Directory ve LearnUpon arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: b11c6315-c79d-4f34-9610-bd17070ab7c7
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 70d4e507087e645c9bfd41e7ef6b90098079ab1d
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2019
ms.locfileid: "56202443"
---
# <a name="tutorial-azure-active-directory-integration-with-learnupon"></a>Öğretici: LearnUpon ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile LearnUpon tümleştirme konusunda bilgi edinin.

Azure AD ile LearnUpon tümleştirme ile aşağıdaki avantajları sağlar:

- LearnUpon erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için LearnUpon (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile LearnUpon yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik LearnUpon çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden LearnUpon ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-learnupon-from-the-gallery"></a>Galeriden LearnUpon ekleme
Azure AD'de LearnUpon tümleştirmesini yapılandırmak için LearnUpon Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden LearnUpon eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **LearnUpon**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/learnupon-tutorial/tutorial_learnupon_search.png)

1. Sonuçlar panelinde seçin **LearnUpon**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/learnupon-tutorial/tutorial_learnupon_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı LearnUpon sınayın.

Tek iş için oturum açma için Azure AD ne LearnUpon karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının LearnUpon ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

LearnUpon içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma LearnUpon ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[LearnUpon test kullanıcısı oluşturma](#creating-a-learnupon-test-user)**  - kullanıcı Azure AD gösterimini bağlı LearnUpon Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve LearnUpon uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile LearnUpon yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **LearnUpon** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/learnupon-tutorial/tutorial_learnupon_samlbase.png)

1. Üzerinde **LearnUpon etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/learnupon-tutorial/tutorial_learnupon_url.png)

    İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<companyname>.learnupon.com/saml/consumer`

    > [!NOTE] 
    > Bu gerçek değer olmadığını unutmayın. Bu değer ile gerçek yanıt URL'sini güncelleştirmeniz gerekiyor. Bu kişi değerini almak için [LearnUpon Destek ekibine](https://www.learnupon.com/features/support/).



1. Üzerinde **SAML imzalama sertifikası** bölümünde, bulun **parmak izi** -LearnUpon SAML ayarlarınızı eklenir.

    ![Çoklu oturum açmayı yapılandırın](./media/learnupon-tutorial/tutorial_learnupon_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/learnupon-tutorial/tutorial_general_400.png)

1. Üzerinde **LearnUpon yapılandırma** bölümünde **yapılandırma LearnUpon** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si, SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/learnupon-tutorial/tutorial_learnupon_configure.png) 

1. Başka bir tarayıcı örneğinde ve oturum açma LearnUpon bir yönetici hesabıyla açın. 

1. Tıklayın **ayarları** sekmesi.
   
    ![Çoklu oturum açmayı yapılandırın](./media/learnupon-tutorial/tutorial_learnupon_06.png)

1. Tıklayın **çoklu oturum açma - SAML**ve ardından **genel ayarlar** SAML ayarlarını yapılandırmak için.
   
    ![Çoklu oturum açmayı yapılandırın](./media/learnupon-tutorial/tutorial_learnupon_07.png) 

1. İçinde **genel ayarlar** bölümünde, aşağıdaki adımları gerçekleştirin:
   
    ![Çoklu oturum açmayı yapılandırın](./media/learnupon-tutorial/tutorial_learnupon_08.png)  
  
    a. **Etkin**’i seçin.

    b. Seçin **sürüm** olarak **2.0**.

    c. Seçin **Atla koşullar** olarak **Hayır**.

    d. İçinde **SAML belirteci sonrası parametre adı** metin doğrulandı ve kimlik doğrulaması - örneğin için SAML onayı içeriyor SAML tüketici URL'sine istek post parametresinin adı belirtilen yukarıda kutusuna **SAMLResponse** .

    e. İçinde **ad tanımlayıcı biçimi** metin kutusuna, SAML onayı kullanıcılar tanımlayıcı (e-posta adresi) nerede belirten değeri bulunduğu - örneğin **urn: OASIS: adları: tc: SAML:1.1:nameid-biçimi: emailAddress**.
  
    f. İçinde **sağlayıcı konumu belirlemek** metin, Azure portalı oturum açma ekranından, karşıya yüklenen simgesine tıklarsanız burada kullanıcıların yönlendirildiği belirten bir değer yazın.
  
    g. İçinde **oturum kapatma URL'si** metin kutusu, yapıştırma **oturum kapatma URL'si** hangi Azure portaldan kopyaladığınız.
    
    h. Tıklayın **parmak yazdırır yönetme**ve ardından indirilen sertifikanızın parmak izi karşıya yükleyin.

1. Tıklayın **kullanıcı ayarları**ve ardından aşağıdaki adımları gerçekleştirin:
   
     ![Çoklu oturum açmayı yapılandırın](./media/learnupon-tutorial/tutorial_learnupon_11.png)  
 
    a. İçinde **ad tanımlayıcı biçimi** metin kutusuna, SAML onaylama işlemi burada kullanıcılar firstname de bize bildiren değerin bulunduğu - örneğin: **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.
  
    b. İçinde **son ad tanımlayıcı biçimi** metin kutusuna, SAML onaylama işlemi burada kullanıcılar lastname, bize bildiren değerin bulunduğu - örneğin: **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/learnupon-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/learnupon-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/learnupon-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/learnupon-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-learnupon-test-user"></a>LearnUpon test kullanıcısı oluşturma

Bu bölümün amacı LearnUpon Britta Simon adlı bir kullanıcı oluşturmaktır. LearnUpon tam zamanında sağlama, varsayılan olarak etkin olan destekler.

Bu bölümde, hiçbir eylem öğesini yoktur. Yeni bir kullanıcı henüz mevcut değilse LearnUpon erişme denemesi sırasında oluşturulur. Azure AD çoklu oturum açmayı yapılandırma.

>[!NOTE]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa, iletişime geçmeniz [LearnUpon Destek ekibine](https://www.learnupon.com/features/support/). 

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için LearnUpon erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon LearnUpon için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **LearnUpon**.

    ![Çoklu oturum açmayı yapılandırın](./media/learnupon-tutorial/tutorial_learnupon_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde LearnUpon kutucuğa tıkladığınızda, otomatik olarak LearnUpon uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/learnupon-tutorial/tutorial_general_01.png
[2]: ./media/learnupon-tutorial/tutorial_general_02.png
[3]: ./media/learnupon-tutorial/tutorial_general_03.png
[4]: ./media/learnupon-tutorial/tutorial_general_04.png

[100]: ./media/learnupon-tutorial/tutorial_general_100.png

[200]: ./media/learnupon-tutorial/tutorial_general_200.png
[201]: ./media/learnupon-tutorial/tutorial_general_201.png
[202]: ./media/learnupon-tutorial/tutorial_general_202.png
[203]: ./media/learnupon-tutorial/tutorial_general_203.png

