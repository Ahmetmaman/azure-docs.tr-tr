---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile moconavi | Microsoft Docs'
description: Azure Active Directory ve moconavi arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: e1916224-e1c2-426f-b233-0a2518fa41db
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/30/2018
ms.author: jeedes
ms.openlocfilehash: 3009cb42ac477b18d45ab5968d6f5793ce1cd36c
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55165906"
---
# <a name="tutorial-azure-active-directory-integration-with-moconavi"></a>Öğretici: Moconavi ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile moconavi tümleştirme konusunda bilgi edinin.

Azure AD ile moconavi tümleştirme ile aşağıdaki avantajları sağlar:

- Moconavi erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan için moconavi (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile moconavi yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik moconavi çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin.
Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden moconavi ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-moconavi-from-the-gallery"></a>Galeriden moconavi ekleme
Azure AD'de moconavi tümleştirmesini yapılandırmak için moconavi Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden moconavi eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **moconavi**seçin **moconavi** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![sonuç listesinde moconavi](./media/moconavi-tutorial/tutorial_moconavi_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı moconavi sınayın.

Tek iş için oturum açma için Azure AD ne moconavi karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının moconavi ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma moconavi ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Moconavi test kullanıcısı oluşturma](#create-a-moconavi-test-user)**  - Britta Simon kullanıcı Azure AD gösterimini bağlı moconavi içinde bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve moconavi uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile moconavi yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **moconavi** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma iletişim kutusu](./media/moconavi-tutorial/tutorial_moconavi_samlbase.png)

3. Üzerinde **moconavi etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![moconavi etki alanı ve URL'ler tek oturum açma bilgileri](./media/moconavi-tutorial/tutorial_moconavi_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<yourserverurl>/moconavi-saml2/saml/login`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<yourserverurl>/moconavi-saml2`

    C. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<yourserverurl>/moconavi-saml2/saml/SSO`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si, tanımlayıcı ve yanıt URL'si ile güncelleştirin. İlgili kişi [moconavi istemci Destek ekibine](mailto:support@recomot.co.jp) bu değerleri almak için.

4. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/moconavi-tutorial/tutorial_moconavi_certificate.png)

5. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/moconavi-tutorial/tutorial_general_400.png)

6. Çoklu oturum açmayı yapılandırma **moconavi** tarafı, indirilen göndermek için ihtiyacınız **meta veri XML** için [moconavi Destek ekibine](mailto:support@recomot.co.jp). Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/moconavi-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/moconavi-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/moconavi-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/moconavi-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.

### <a name="create-a-moconavi-test-user"></a>Moconavi test kullanıcısı oluşturma

Bu bölümde, Britta Simon moconavi içinde adlı bir kullanıcı oluşturun. Çalışmak [moconavi Destek ekibine](mailto:support@recomot.co.jp) moconavi platform kullanıcıları eklemek için. Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce etkinleştirildi.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma moconavi erişim vererek kullanmak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200]

**Britta Simon moconavi için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201]

2. Uygulamalar listesinde **moconavi**.

    ![Uygulamalar listesinde moconavi bağlantı](./media/moconavi-tutorial/tutorial_moconavi_app.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

1. Moconavi Microsoft Mağazası'ndan yükleyin.

2. Moconavi başlatın.

3. Tıklayın **Bağlan ayarı** düğmesi.

    ![Çoklu oturum açma testi](./media/moconavi-tutorial/testing1.png)

4. Girin `https://mcs-admin.moconavi.biz/gateway` içine **URL Bağlan** metin kutusuna ve ardından **Bitti** düğmesi.

    ![Çoklu oturum açma testi](./media/moconavi-tutorial/testing2.png)

5. Aşağıdaki ekran görüntüsünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açma testi](./media/moconavi-tutorial/testing3.png)

    a. Girin **giriş kimlik doğrulama anahtarı**:`azureAD` içine **giriş kimlik doğrulama anahtarı** metin.

    b. Girin **giriş kullanıcı kimliği**: `your ad account` içine **giriş kullanıcı kimliği** metin.

    c. Tıklayın **oturum açma**.

6. Azure AD parolanızı giriş **parola** metin kutusuna ve ardından **oturum açma** düğmesi.

    ![Çoklu oturum açma testi](./media/moconavi-tutorial/testing4.png)

7. Menü görüntülendiğinde, azure AD kimlik doğrulaması başarılı olur.

    ![Çoklu oturum açma testi](./media/moconavi-tutorial/testing5.png)

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/moconavi-tutorial/tutorial_general_01.png
[2]: ./media/moconavi-tutorial/tutorial_general_02.png
[3]: ./media/moconavi-tutorial/tutorial_general_03.png
[4]: ./media/moconavi-tutorial/tutorial_general_04.png

[100]: ./media/moconavi-tutorial/tutorial_general_100.png

[200]: ./media/moconavi-tutorial/tutorial_general_200.png
[201]: ./media/moconavi-tutorial/tutorial_general_201.png
[202]: ./media/moconavi-tutorial/tutorial_general_202.png
[203]: ./media/moconavi-tutorial/tutorial_general_203.png

