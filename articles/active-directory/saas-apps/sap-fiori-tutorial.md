---
title: 'Öğretici: SAP Fiori ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve SAP Fiori arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 12/28/2020
ms.author: jeedes
ms.openlocfilehash: 547d96a9591b99318a74977106e99511c9c80507
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "101687116"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-sap-fiori"></a>Öğretici: SAP Fiori ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory

Bu öğreticide SAP Fiori 'ı Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. SAP Fiori 'i Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de SAP Fiori 'e erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla SAP Fiori 'ta otomatik olarak oturum açmalarına olanak sağlayın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.


## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* SAP Fiori çoklu oturum açma (SSO) etkin aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz.

* SAP Fiori, **SP** tarafından başlatılan SSO 'yu destekler

> [!NOTE]
> SAP Fiori tarafından başlatılan iFrame kimlik doğrulaması için, bir mini kimlik doğrulaması için SAML Authbir parametresinin **ıpassive** parametresini kullanmanızı öneririz. **Ipassive** parametresi hakkında daha fazla bilgi IÇIN [Azure AD SAML çoklu oturum açma](../develop/single-sign-on-saml-protocol.md) bilgilerine bakın.

## <a name="adding-sap-fiori-from-the-gallery"></a>Galeriden SAP Fiori ekleme

SAP Fiori 'ın Azure AD 'ye tümleştirilmesini yapılandırmak için, Gallery 'den yönetilen SaaS uygulamaları listenize SAP Fiori eklemeniz gerekir.

1. Azure portal iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **SAP Fiori** yazın.
1. Sonuçlar panelinden **SAP Fiori** ' ı seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-sso-for-sap-fiori"></a>SAP Fiori için Azure AD SSO 'yu yapılandırma ve test etme

**B. Simon** adlı bir test KULLANıCıSı kullanarak SAP Fiori Ile Azure AD SSO 'yu yapılandırın ve test edin. SSO 'nun çalışması için, SAP Fiori 'deki bir Azure AD kullanıcısı ve ilgili Kullanıcı arasında bir bağlantı ilişkisi oluşturmanız gerekir.

Azure AD SSO 'yu SAP Fiori ile yapılandırmak ve test etmek için aşağıdaki adımları gerçekleştirin:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
    1. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
    1. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
1. **[SAP FIORI SSO 'Yu yapılandırma](#configure-sap-fiori-sso)** -uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
    1. SAP **[Fiori test kullanıcısı oluşturun](#create-sap-fiori-test-user)** -bu, kullanıcının Azure AD gösterimine bağlı olan SAP Fiori 'de B. Simon 'a karşılık gelen bir.
1. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. Yeni bir Web tarayıcısı penceresi açın ve SAP Fiori şirket sitenizde yönetici olarak oturum açın.

1. **Http** ve **https** hizmetlerinin etkin olduğundan ve Ilgili bağlantı noktalarının **smicm** işlem koduna atandığından emin olun.

1. SAP System **T01** Için SAP Business Client 'da oturum açın; çoklu oturum açma gereklidir. Ardından, HTTP güvenlik oturumu yönetimini etkinleştirin.

    1. İşlem kodu **SICF_SESSIONS** git. Geçerli değerlere sahip tüm ilgili profil parametreleri gösteriliyor. Aşağıdaki örneğe benzer şekilde görünür:

        ```
        login/create_sso2_ticket = 2
        login/accept_sso2_ticket = 1
        login/ticketcache_entries_max = 1000
        login/ticketcache_off = 0  login/ticket_only_by_https = 0 
        icf/set_HTTPonly_flag_on_cookies = 3
        icf/user_recheck = 0  http/security_session_timeout = 1800
        http/security_context_cache_size = 2500
        rdisp/plugin_auto_logout = 1800
        rdisp/autothtime = 60
        ```

        >[!NOTE]
        > Parametreleri kuruluşunuzun gereksinimlerine göre ayarlayın. Yukarıdaki parametreler yalnızca örnek olarak verilmiştir.

    1. Gerekirse, SAP sisteminin örnek (varsayılan) profilinde parametreleri ayarlayın ve SAP sistemini yeniden başlatın.

    1. Bir HTTP güvenlik oturumunu etkinleştirmek için ilgili istemciye çift tıklayın.

        ![SAP 'deki Ilgili profil parametrelerinin geçerli değerleri sayfası](./media/sapfiori-tutorial/tutorial-sapnetweaver-profileparameter.png)

    1. Aşağıdaki SıCF hizmetlerini etkinleştirin:

        ```
        /sap/public/bc/sec/saml2
        /sap/public/bc/sec/cdc_ext_service
        /sap/bc/webdynpro/sap/saml2
        /sap/bc/webdynpro/sap/sec_diag_tool (This is only to enable / disable trace)
        ```

1. SAP System [**T01/122.368**] iş istemcisinde Transaction Code **SAML2** sayfasına gidin. Yapılandırma Kullanıcı arabirimi yeni bir tarayıcı penceresinde açılır. Bu örnekte, SAP System 122 için Iş Istemcisini kullanırız.

    ![SAP Fiori Iş Istemcisi oturum açma sayfası](./media/sapfiori-tutorial/tutorial-sapnetweaver-sapbusinessclient.png)

1. Kullanıcı adınızı ve parolanızı girip **oturum aç**' ı seçin.

    ![SAP 'de ABAP System T01/122.368 sayfasının SAML 2,0 yapılandırması](./media/sapfiori-tutorial/tutorial-sapnetweaver-userpwd.png)

1. **Sağlayıcı adı** kutusunda **T01122** değerini **http: \/ /t01122** ile değiştirin ve ardından **Kaydet**' i seçin.

    > [!NOTE]
    > Varsayılan olarak, sağlayıcı adı biçimindedir \<sid> \<client> . Azure AD,://biçiminde ad bekliyor \<protocol> \<name> . \: // \<sid> \<client> Azure AD 'de birden çok SAP Fiori ABAP altyapısını yapılandırabilmek için sağlayıcı adını https olarak tutmanızı öneririz.

    ![SAP 'de ABAP System T01/122.368 sayfasının SAML 2,0 yapılandırmasındaki güncelleştirilmiş sağlayıcı adı](./media/sapfiori-tutorial/tutorial-sapnetweaver-providername.png)

1. **Yerel sağlayıcı sekmesi**  >  **meta verileri**' ni seçin.

1. **SAML 2,0 meta verileri** iletişim kutusunda, oluşturulan meta veri xml dosyasını indirin ve bilgisayarınıza kaydedin.

    ![SAP SAML 2,0 meta verileri iletişim kutusunda meta verileri Indir bağlantısı](./media/sapfiori-tutorial/tutorial-sapnetweaver-generatesp.png)

1. Azure portal, **SAP Fiori** uygulama tümleştirmesi sayfasında, **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölümünde, **hizmet sağlayıcısı meta verileri dosyanız** varsa, aşağıdaki adımları uygulayın:

    a. **Meta veri dosyasını karşıya yükle**' ye tıklayın.

    ![Meta veri dosyasını karşıya yükle](common/upload-metadata.png)

    b. Meta veri dosyasını seçmek için **klasör logosu** ' na tıklayın ve **karşıya yükle**' ye tıklayın.

    ![meta veri dosyası seçin](common/browse-upload-metadata.png)

    c. Meta veri dosyası başarıyla karşıya yüklendiğinde, **tanımlayıcı** ve **yanıt URL 'SI** değerleri **temel SAML yapılandırması** bölmesine otomatik olarak doldurulur. **Oturum açma URL 'si** kutusunda, aşağıdaki düzene sahıp bir URL girin: `https://<your company instance of SAP Fiori>` .

    > [!NOTE]
    > Bazı müşteriler yanlış yapılandırılmış **yanıt URL** değerleriyle ilgili hataları raporlar. Bu hatayı görürseniz, örneğiniz için doğru yanıt URL 'sini ayarlamak için aşağıdaki PowerShell betiğini kullanabilirsiniz:
    >
    > ```
    > Set-AzureADServicePrincipal -ObjectId $ServicePrincipalObjectId -ReplyUrls "<Your Correct Reply URL(s)>"
    > ``` 
    > 
    > `ServicePrincipal`Komut dosyasını çalıştırmadan önce nesne kimliğini kendiniz ayarlayabilir veya buraya geçirebilirsiniz.

1. SAP Fiori uygulaması, SAML onayları 'nin belirli bir biçimde olmasını bekler. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu öznitelik değerlerini yönetmek için, **SAML Ile tek Sign-On ayarla** bölmesinde **Düzenle**' yi seçin.

    ![Kullanıcı öznitelikleri bölmesi](common/edit-attribute.png)

1. **Kullanıcı öznitelikleri & talepler** BÖLMESINDE, SAML belirteci özniteliklerini önceki görüntüde gösterildiği gibi yapılandırın. Ardından, aşağıdaki adımları izleyin:

    1. **Kullanıcı taleplerini Yönet** bölmesini açmak için **Düzenle** ' yi seçin.

    1. **Dönüştürme** listesinde **Extractmailprefix ()** öğesini seçin.

    1. **Parametre 1** listesinde **User. UserPrincipalName**' i seçin.

    1. **Kaydet**’i seçin.

       ![Kullanıcı taleplerini Yönet bölmesi](./media/sapfiori-tutorial/nameidattribute.png)

       ![Kullanıcı taleplerini Yönet bölmesindeki dönüştürme bölümü](./media/sapfiori-tutorial/nameidattribute1.png)
    
1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, **Federasyon meta verileri XML** 'i bulun ve sertifikayı indirip bilgisayarınıza kaydetmek için **İndir** ' i seçin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

1. **SAP Fiori ayarla** bölümünde, gereksiniminize göre uygun URL 'leri kopyalayın.

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

Bu bölümde, SAP Fiori 'e erişim vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde **SAP Fiori**' ı seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.
1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.
1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. Kullanıcılara bir rolün atanmasını bekliyorsanız, **Rol Seç** açılır listesinden bunu seçebilirsiniz. Bu uygulama için ayarlanmış bir rol yoksa, "varsayılan erişim" rolü seçili olduğunu görürsünüz.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

## <a name="configure-sap-fiori-sso"></a>SAP Fiori SSO 'yu yapılandırma

1. SAP sisteminde oturum açın ve işlem kodu **SAML2**' e gidin. SAML yapılandırma sayfası ile yeni bir tarayıcı penceresi açılır.

1. Güvenilir bir kimlik sağlayıcısı (Azure AD) için uç noktaları yapılandırmak üzere, **güvenilir sağlayıcılar** sekmesini seçin.

    ![SAP 'deki güvenilir sağlayıcılar sekmesi](./media/sapfiori-tutorial/tutorial-sapnetweaver-samlconfig.png)

1. **Ekle**' yi seçin ve bağlam menüsünden **meta veri dosyasını karşıya yükle** ' yi seçin.

    ![SAP 'de meta veri dosyası ekleme ve karşıya yükleme seçenekleri](./media/sapfiori-tutorial/tutorial-sapnetweaver-uploadmetadata.png)

1. Azure portal indirdiğiniz meta veri dosyasını karşıya yükleyin. **İleri**’yi seçin.

    ![SAP 'ye yüklenecek meta veri dosyasını seçin](./media/sapfiori-tutorial/tutorial-sapnetweaver-metadatafile.png)

1. Sonraki sayfada, **diğer** ad kutusuna diğer adı girin. Örneğin, **aadsts**. **İleri**’yi seçin.

    ![SAP 'deki diğer ad kutusu](./media/sapfiori-tutorial/tutorial-sapnetweaver-aliasname.png)

1. **Özet algoritması** kutusundaki değerin **SHA-256** olduğundan emin olun. **İleri**’yi seçin.

    ![SAP 'de Özet algoritması değerini doğrulama](./media/sapfiori-tutorial/tutorial-sapnetweaver-identityprovider.png)

1. **Tek Sign-On uç noktaları** altında **http post**' ı seçin ve ardından **İleri**' yi seçin.

    ![SAP 'de tek Sign-On uç noktası seçenekleri](./media/sapfiori-tutorial/tutorial-sapnetweaver-httpredirect.png)

1. **Çoklu oturum kapatma uç noktaları** altında **http yeniden yönlendirme**' yi seçin ve **İleri**' yi seçin.

    ![SAP 'de çoklu oturum kapatma uç noktaları seçenekleri](./media/sapfiori-tutorial/tutorial-sapnetweaver-httpredirect1.png)

1. **Yapıt uç noktaları** altında devam etmek için **İleri** ' yi seçin.

    ![SAP 'de yapıt uç noktaları seçenekleri](./media/sapfiori-tutorial/tutorial-sapnetweaver-artifactendpoint.png)

1. **Kimlik doğrulama gereksinimleri** altında **son**' u seçin.

    ![SAP 'de kimlik doğrulama gereksinimleri seçenekleri ve son seçeneği](./media/sapfiori-tutorial/tutorial-sapnetweaver-authentication.png)

1. **Güvenilen sağlayıcı**  >  **kimliği Federasyonu** (sayfanın alt kısmında) seçeneğini belirleyin. **Düzenle**'yi seçin.

    ![SAP 'deki güvenilen sağlayıcı ve Kimlik Federasyonu sekmeleri](./media/sapfiori-tutorial/tutorial-sapnetweaver-trustedprovider.png)

1. **Add (Ekle)** seçeneğini belirleyin.

    ![Kimlik Federasyonu sekmesindeki Ekle seçeneği](./media/sapfiori-tutorial/tutorial-sapnetweaver-addidentityprovider.png)

1. **Desteklenen NameID biçimleri** Iletişim kutusunda **belirtilmemiş**' i seçin. **Tamam**’ı seçin.

    ![Desteklenen NameID biçimleri iletişim kutusu ve SAP 'de seçenekler](./media/sapfiori-tutorial/tutorial-sapnetweaver-nameid.png)

    **Kullanıcı kimliği kaynağı** ve **Kullanıcı kimliği eşleme modu** değerleri, SAP kullanıcısı ve Azure AD talebi arasındaki bağlantıyı belirlenir.  

    **Senaryo 1**: SAP KULLANıCıSıNıN Azure AD kullanıcı eşlemesi

    1. SAP 'de, **NameID biçimi "belirtilmemiş"** bölümünde Ayrıntılar:

        ![A P 'deki ' NameID biçimi "belirtilmemiş" ' iletişim kutusunun ' ayrıntılarını gösteren ekran görüntüsü.](./media/sapfiori-tutorial/nameiddetails.png)

    1. Azure portal, **Kullanıcı öznitelikleri & talepler** altında, Azure AD 'den gerekli talepleri aklınızda edin.

        !["Kullanıcı öznitelikleri & talepleri" iletişim kutusunu gösteren ekran görüntüsü.](./media/sapfiori-tutorial/claimsaad1.png)

    **2. senaryo**: SU01 ' de yapılandırılan e-posta adresine bağlı olarak SAP Kullanıcı kimliğini seçin. Bu durumda, SSO gerektiren her kullanıcı için e-posta KIMLIĞI SU01 ' de yapılandırılmalıdır.

    1.  SAP 'de, **NameID biçimi "belirtilmemiş"** bölümünde Ayrıntılar:

        ![SAP 'de NameID biçimi "belirtilmemiş" iletişim kutusunun ayrıntıları](./media/sapfiori-tutorial/tutorial-sapnetweaver-nameiddetails1.png)

    1. Azure portal, **Kullanıcı öznitelikleri & talepler** altında, Azure AD 'den gerekli talepleri aklınızda edin.

       ![Azure portal Kullanıcı öznitelikleri & talepleri iletişim kutusu](./media/sapfiori-tutorial/claimsaad2.png)

1. **Kaydet**' i seçin ve ardından kimlik sağlayıcısını etkinleştirmek için **Etkinleştir** ' i seçin.

    ![SAP 'de kaydetme ve etkinleştirme seçenekleri](./media/sapfiori-tutorial/configuration1.png)

1. İstendiğinde **Tamam ' ı** seçin.

    ![SAP 'de SAML 2,0 yapılandırma iletişim kutusunda Tamam seçeneği](./media/sapfiori-tutorial/configuration2.png)

### <a name="create-sap-fiori-test-user"></a>SAP Fiori test kullanıcısı oluşturma

Bu bölümde SAP Fiori 'da Britta Simon adlı bir Kullanıcı oluşturacaksınız. SAP Fiori platformunda Kullanıcı eklemek için şirket içi SAP uzmanlarınız veya kuruluşunuzun SAP iş ortağı ile çalışın.

## <a name="test-sso"></a>Test SSO 'SU

1. SAP Fiori ' de kimlik sağlayıcısı Azure AD etkinleştirildikten sonra, çoklu oturum açmayı test etmek için aşağıdaki URL 'Lerden birine erişmeyi deneyin (Kullanıcı adı ve parola istenmez):

    * https: \/ / \<sapurl\> /SAP/BC/BSP/SAP/it00/default.htm
    * https: \/ / \<sapurl\> /SAP/BC/BSP/SAP/it00/default.htm

    > [!NOTE]
    > *Sapurl 'yi* gerçek SAP ana bilgisayar adıyla değiştirin.

1. Test URL 'SI sizi SAP 'deki aşağıdaki test uygulaması sayfasına götürebilmelidir. Sayfa açılırsa Azure AD çoklu oturum açma başarıyla ayarlanır.

    ![SAP 'deki standart test uygulaması sayfası](./media/sapfiori-tutorial/testingsso.png)

1. Kullanıcı adı ve parola istenirse, sorunu tanılamaya yardımcı olması için izlemeyi etkinleştirin. İzleme için şu URL 'yi kullanın: https: \/ / \<sapurl\> /SAP/BC/WebDynpro/SAP/sec_diag_tool? SAP-Client = 122.368&SAP-Language = en #.

## <a name="next-steps"></a>Sonraki adımlar

SAP Fiori 'ı yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak ayıklanmasını ve zaman korumasını koruyan oturum denetimini zorunlu kılabilirsiniz. Oturum denetimi koşullu erişimden genişletiliyor. [Microsoft Cloud App Security ile oturum denetimini nasıl zorlayacağınızı öğrenin](/cloud-app-security/proxy-deployment-any-app).