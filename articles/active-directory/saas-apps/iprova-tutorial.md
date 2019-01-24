---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile iProva | Microsoft Docs'
description: Azure Active Directory ve iProva arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 1eaeef9b-4479-4a9f-b1b2-bc13b857c75c
ms.service: Azure-Active-Directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/24/2018
ms.author: jeedes
ms.openlocfilehash: ba3b0e06630665082b62e070dac64e8bc572f6dc
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54811715"
---
# <a name="tutorial-azure-active-directory-integration-with-iprova"></a>Öğretici: İProva ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile iProva tümleştirme konusunda bilgi edinin.
Azure AD ile iProva tümleştirme ile aşağıdaki avantajları sağlar:

* İProva erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak (çoklu oturum açma) iProva için kendi Azure AD hesapları ile oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile iProva yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Abonelik iProva çoklu oturum açma etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* iProva destekler **SP** tarafından başlatılan

## <a name="adding-iprova-from-the-gallery"></a>Galeriden iProva ekleme

Azure AD'de iProva tümleştirmesini yapılandırmak için iProva Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden iProva eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **iProva**seçin **iProva** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![sonuç listesinde iProva](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma iProva adlı bir test kullanıcı tabanlı test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısının iProva ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma iProva ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[İProva yapılandırma bilgilerini almak](#retrieve-configuration-information-from-iprova)**  - sonraki adımlar için bir hazırlık olarak.
2. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
3. **[Çoklu oturum açma iProva yapılandırma](#configure-iprova-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
4. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
5. **[İProva test kullanıcısı oluşturma](#create-iprova-test-user)**  - Britta Simon kullanıcı Azure AD gösterimini bağlı iProva içinde bir karşılığı vardır.
6. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
7. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="retrieve-configuration-information-from-iprova"></a>İProva yapılandırma bilgilerini alma

Bu bölümde, iProva bazı gerekli bilgileri alır.
Azure AD çoklu oturum açmayı yapılandırmak için bu bilgiler gerekir

1. Bir web tarayıcısı açın ve gidin **SAML2 bilgi sayfası** iProva aşağıdaki URL düzeni kullanarak:

    | | |
    |-|-|
    | `https://SUBDOMAIN.iprova.nl/saml2info`|
    | `https://SUBDOMAIN.iprova.be/saml2info`|
    | | |

     ![İProva SAML2 bilgileri sayfasını görüntüle](media/iprova-tutorial/iprova-saml2-info.png)

2. Başka bir tarayıcı sekmesinde sonraki adımlar ile devam ederken tarayıcı sekmesini açık bırakın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma ile iProva yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **iProva** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    a. Dolgu **tanımlayıcı** etiketi görüntülenir; değeri ile alan **Entityıd** üzerinde **iProva SAML2 bilgi sayfası** (hala diğer tarayıcı sekmesinde açık).

    b. Dolgu **yanıt URL'si** etiketi görüntülenir; değeri ile alan **yanıt URL'si** üzerinde **iProva SAML2 bilgi sayfası** (hala diğer tarayıcı sekmesinde açık).

    c. Dolgu **oturum açma URL'si** etiketi görüntülenir; değeri ile alan **oturum açma URL'si** üzerinde **iProva SAML2 bilgi sayfası** (hala diğer tarayıcı sekmesinde açık).

    ![iProva etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier-reply.png)

5. iProva uygulama belirli bir biçimde SAML onaylamalarını bekler. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu öznitelikleri değerlerini yönetebilirsiniz **kullanıcı öznitelikleri** uygulama tümleştirme sayfasında bölümü. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için düğmeyi **kullanıcı öznitelikleri** iletişim.

    ![image](common/edit-attribute.png)

6. İçinde **kullanıcı taleplerini** bölümünde **kullanıcı öznitelikleri** iletişim kutusunda, SAML belirteci özniteliği yukarıdaki görüntüde gösterilen şekilde yapılandırın ve aşağıdaki adımları gerçekleştirin:

    | Ad | Kaynak özniteliği| Ad alanı |
    | ---------------| -------- | -----|
    | `samaccountname` | `user.onpremisessamaccountname`| `http://schemas.xmlsoap.org/ws/2005/05/identity/claims`|
    | | |

    a. Tıklayın **Ekle yeni talep** açmak için **yönetmek, kullanıcı talepleri** iletişim.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.

    c. Gelen **Namespace** listesinde, ilgili satır için gösterilen ad alanı değeri yazın.

    d. Kaynağı olarak **özniteliği**.

    e. Gelen **kaynak özniteliği** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    f. Tıklayın **Tamam**

    g. **Kaydet**’e tıklayın.

7. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlayın** sayfasında **SAML imzalama sertifikası** bölümünde, kopyalamak için Kopyala düğmesine **uygulama Federasyon meta verileri URL'sini** ve üzerinde kaydedin, bilgisayar.

    ![Sertifika indirme bağlantısı](common/copy-metadataurl.png)

### <a name="configure-iprova-single-sign-on"></a>Çoklu oturum açma iProva yapılandırın

1. Oturum iProva kullanarak **yönetici** hesabı.

2. Açık **Git** menüsü.

3. Tıklayarak **Uygulama Yönetimi**.

4. Tıklayarak **genel** içinde **sistem ayarlarını** paneli.

5. Tıklayarak **Düzenle**.

6. Ekranı aşağı kaydırarak **erişim denetimi**.

    ![iProva erişim denetimi ayarları](media/iprova-tutorial/iprova-accesscontrol.png)

7. Ayar Bul **otomatik olarak oturum açan kullanıcılar, ağ hesaplarıyla**ve şekilde değiştirin **Evet, kimlik doğrulama ile SAML**. Ek seçenekler görünür.

8. Tıklayın **Kurulum** düğmesi.

9. Tıklayın **sonraki** düğmesi.

10. iProva artık Federasyon verileri bir URL'den indirin veya bir dosyadan yüklemek isteyip istemediğinizi sorar. Seçin **URL'den** seçeneği.

    ![Azure AD meta verileri indirme](media/iprova-tutorial/iprova-download-metadata.png)

11. Şimdi son adımda kaydettiğiniz meta veri URL'sini yapıştırın **yapılandırma Azure AD çoklu oturum açma** bölüm.

12. Azure AD'den meta verileri indirmek için ok şeklinde düğmesine tıklayın.

13. Yükleme tamamlandığında, onay iletisi **geçerli federasyon veri dosyasını indirdiğiniz** görünür.

14. Tıklayın **sonraki** düğmesi.

15. Skip **Test oturum açma** şimdilik seçeneğini ve tıklayın **sonraki** düğmesi.

16. Açılan menüdeki adlı **kullanılacak talep**seçin **windowsaccountname**.

17. **Son** düğmesine tıklayın.

18. Şimdi Geri **genel ayarlarını Düzenle** ekran. **Aşağı kaydırarak** sayfa seçeneğine tıklayıp altındaki **Tamam** yapılandırmanızı kaydetmek için düğme.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. İçinde **adı** alan adı gibi girin **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü **yourname@yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma iProva erişim vererek kullanmak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **iProva**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **iProva**.

    ![Uygulamalar listesinde iProva bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-iprova-test-user"></a>İProva test kullanıcısı oluşturma

1. Oturum iProva kullanarak **yönetici** hesabı.

2. Açık **Git** menüsü.

3. Tıklayarak **Uygulama Yönetimi**.

4. Tıklayarak **kullanıcılar** içinde **kullanıcılar ve kullanıcı grupları** paneli.

5. **Ekle** düğmesine tıklayın.

6. İçinde **kullanıcıadı** alan türü **brittasimon@yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

7. İçinde **tam adı** gibi tam ad alanı girin **BrittaSimon**.

8. Seçin **parolasız (kullanım çoklu oturum açma)** seçeneği.

9. İçinde **e-posta adresi** alan türü **yourname@yourcompanydomain.extension** Örneğin, BrittaSimon@contoso.com

10. Sayfanın sonuna kaydırın ve tıklayın **son** düğmesi.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli iProva kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama iProva için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [ SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [iProva - olduğu saml2 tabanlı çoklu oturum açmayı yapılandırma](https://webshare.iprova.nl/0wqwm45yn09f5poh/Document.aspx)