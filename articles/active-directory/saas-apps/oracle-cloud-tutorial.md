---
title: 'Öğretici: Oracle Cloud Infrastructure konsolu ile tümleştirme Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve Oracle bulut altyapısı konsolu arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 01/16/2020
ms.author: jeedes
ms.openlocfilehash: ed697b54b868319d715fdbb88dcae8f8968852d2
ms.sourcegitcommit: 59f506857abb1ed3328fda34d37800b55159c91d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2020
ms.locfileid: "92515670"
---
# <a name="tutorial-integrate-oracle-cloud-infrastructure-console-with-azure-active-directory"></a>Öğretici: Oracle bulut altyapısı konsolunu Azure Active Directory ile tümleştirme

Bu öğreticide, Oracle bulut altyapısı konsolunu Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. Oracle bulut altyapısı konsolunu Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de Oracle bulut altyapısı konsoluna erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Oracle bulut altyapısı konsoluna otomatik olarak oturum açmalarına olanak sağlayın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

Azure AD ile SaaS uygulaması tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Oracle Cloud Infrastructure konsolu çoklu oturum açma (SSO) aboneliği etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz.

* Oracle bulut altyapısı Konsolu **SP** tarafından başlatılan SSO 'yu destekler.
* Oracle bulut altyapısı konsolunu yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin boyutunu gerçek zamanlı olarak koruyan oturum denetimlerini zorunlu kılabilirsiniz. Oturum denetimleri koşullu erişimden genişletilir. [Microsoft Cloud App Security ile oturum denetimini nasıl zorlayacağınızı öğrenin](/cloud-app-security/proxy-deployment-aad)

## <a name="adding-oracle-cloud-infrastructure-console-from-the-gallery"></a>Galeriden Oracle bulut altyapısı konsolunu ekleme

Oracle bulut altyapısı konsolunun tümleştirmesini Azure AD ile yapılandırmak için, Galeriden Oracle bulut altyapısı konsolunu yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. [Azure Portal](https://portal.azure.com) iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Oracle Cloud Infrastructure konsolunu** yazın.
1. Sonuçlar panelinden **Oracle bulut altyapısı konsolunu** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma ve test etme

**B. Simon**adlı bir test kullanıcısı kullanarak Oracle bulut altyapısı konsolu Ile Azure AD SSO 'yu yapılandırın ve test edin. SSO 'nun çalışması için, Oracle bulut altyapısı konsolundaki bir Azure AD kullanıcısı ve ilgili Kullanıcı arasında bir bağlantı ilişkisi oluşturmanız gerekir.

Azure AD SSO 'yu Oracle bulut altyapısı konsolu ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını doldurun:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** .
    1. B. Simon ile Azure AD çoklu oturum açma sınamasını test etmek için **[bir Azure AD test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** .
    1. Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek üzere **[Azure AD test kullanıcısını atayın](#assign-the-azure-ad-test-user)** .
1. Uygulama tarafında SSO ayarlarını yapılandırmak için **[Oracle bulut altyapı konsolu 'Nu yapılandırın](#configure-oracle-cloud-infrastructure-console)** .
    1. Oracle bulut altyapısı konsolunda, kullanıcının Azure AD gösterimine bağlı olan B. Simon 'un bir karşılığı olacak şekilde **[Oracle Cloud Infrastructure konsol test kullanıcısı oluşturun](#create-oracle-cloud-infrastructure-console-test-user)** .
1. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[test SSO 'su](#test-sso)** .

### <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. [Azure Portal](https://portal.azure.com/), **Oracle Cloud Infrastructure konsolu** uygulama tümleştirmesi sayfasında, **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML Ile tek Sign-On ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** için Düzenle/kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **Temel SAML yapılandırması** sayfasında, aşağıdaki alanlar için değerleri girin:

   > [!NOTE]
   > Hizmet sağlayıcı meta veri dosyasını, öğreticinin **Oracle bulut altyapı konsolu çoklu oturum açma bölümünde yapılandırma** bölümünden alacaksınız.
    
   1. **Meta veri dosyasını karşıya yükle**' ye tıklayın.

   1. Meta veri dosyasını seçmek için **klasör logosu** ' na tıklayın ve **karşıya yükle**' ye tıklayın.

   1. Meta veri dosyası başarıyla karşıya yüklendikten sonra **tanımlayıcı** ve **yanıt URL** değerleri, **temel SAML yapılandırması** bölüm metin kutusunda otomatik olarak doldurulur.
    
      > [!NOTE]
      > **Tanımlayıcı** ve **yanıt URL 'si** değerleri otomatik olarak alamazsanız, değerleri gereksinimlerinize göre el ile girin.

      **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://console.<REGIONNAME>.oraclecloud.com/`

      > [!NOTE]
      > Değer gerçek değil. Değeri gerçek Sign-On URL 'siyle güncelleştirin. Değeri almak için [Oracle Cloud Infrastructure Konsolu istemci destek ekibine](https://www.oracle.com/support/advanced-customer-support/products/cloud.html) başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

1. **SAML Ile tekli Sign-On ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, **Federasyon meta verileri XML** 'i bulun ve sertifikayı indirip bilgisayarınıza kaydetmek için **İndir** ' i seçin.

   ![Sertifika indirme bağlantısı](common/metadataxml.png)

1. Oracle Cloud Infrastructure konsol uygulaması, SAML belirteci öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML onayları bekler. Aşağıdaki ekran görüntüsünde varsayılan özniteliklerin listesi gösterilmektedir. Kullanıcı öznitelikleri iletişim kutusunu açmak için **Düzenle** simgesine tıklayın.

   ![Sağ üst köşedeki "Düzenle" simgesiyle "Kullanıcı öznitelikleri" iletişim kutusunu gösteren ekran görüntüsü.](common/edit-attribute.png)

1. Daha fazlasına ek olarak, Oracle bulut altyapısı konsol uygulaması, SAML yanıtına daha fazla öznitelik geçirilmesini bekler. **Grup talepleri (Önizleme)** Iletişim kutusundaki **Kullanıcı öznitelikleri & talepler** bölümünde aşağıdaki adımları uygulayın:

   1. **Ad tanımlayıcı değeri**' nin yanındaki **kaleme** tıklayın.

   1. **Ad tanımlayıcı biçimini seçin**olarak **persistent** ' i seçin.
 
   1. **Kaydet**’e tıklayın.

      !["Ad tanımlayıcı değeri" ve "talepte döndürülen gruplar" vurgulanmış "düzenleme" simgeleri ile "Kullanıcı öznitelikleri & talepler" bölümünü gösteren ekran görüntüsü.](./media/oracle-cloud-tutorial/config07.png)
    
      !["Persistant" ve "Kaydet" düğmesinin seçili olduğu "Kullanıcı taleplerini yönetme" iletişim kutusunu gösteren ekran görüntüsü.](./media/oracle-cloud-tutorial/config11.png)

   1. **Talepte döndürülen gruplar ' ın**yanındaki **kaleme** tıklayın.

   1. Radyo listesinden **güvenlik grupları** ' nı seçin.

   1. **Grup Kimliği**'Nin **kaynak özniteliğini** seçin.

   1. **Grup talebinin adını özelleştirmeyi**denetleyin.

   1. **Ad** metin kutusuna **GroupName**yazın.

   1. **Ad alanı (isteğe bağlı)** metin kutusuna yazın `https://auth.oraclecloud.com/saml/claims` .

   1. **Kaydet**’e tıklayın.

      ![image](./media/oracle-cloud-tutorial/config08.png)

1. **Oracle bulut altyapısı konsolunu ayarla** bölümünde, gereksiniminize göre uygun URL 'leri kopyalayın.

   ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)



### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, B. Simon adlı Azure portal bir test kullanıcısı oluşturacaksınız.

1. Azure portal sol bölmeden **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.
1. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.
1. **Kullanıcı** özellikleri ' nde şu adımları izleyin:
   1. **Ad** alanına `B. Simon` girin.  
   1. **Kullanıcı adı** alanına, girin username@companydomain.extension . Örneğin, `B. Simon@contoso.com`.
   1. **Parolayı göster** onay kutusunu seçin ve ardından **parola** kutusunda görüntülenen değeri yazın.
   1. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Oracle bulut altyapısı konsoluna erişim vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde **Oracle Cloud Infrastructure konsolu**' nu seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

   ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. SAML assertion 'da herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, Kullanıcı için listeden uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

## <a name="configure-oracle-cloud-infrastructure-console"></a>Oracle bulut altyapısı konsolunu yapılandırma

1. Farklı bir Web tarayıcısı penceresinde, yönetici olarak Oracle bulut altyapısı konsolunda oturum açın.

1. Menünün sol tarafına tıkladıktan sonra **kimlik** ' e tıklayın ve ardından **Federasyon**' a gidin.

   ![Sol üst taraftaki menüden "Identity" ve "Federation" seçiliyken "Oracle Cloud Infrastructure konsolunu" gösteren ekran görüntüsü.](./media/oracle-cloud-tutorial/config01.png)

1. **Bu belgeyi indir** bağlantısını tıklatıp Azure Portal **Service Provider metadata file** **temel SAML yapılandırması** bölümüne yükleyin ve ardından **kimlik sağlayıcısı ekle**' ye tıklayın.

   ![Sol taraftaki menüden "Federasyon" ' u ve "kimlik sağlayıcısı ekle" düğmesini ve "Bu belgeyi Indir" eylemi seçili olan ekran görüntüsü.](./media/oracle-cloud-tutorial/config02.png)

1. **Kimlik sağlayıcı ekle** açılır penceresinde aşağıdaki adımları uygulayın:

   !["Kimlik sağlayıcısı ekle" açılır penceresini gösteren ekran görüntüsü.](./media/oracle-cloud-tutorial/config03.png)

   1. **Ad** metin kutusuna adınızı girin.

   1. **Açıklama** metin kutusuna açıklamanızı girin.

   1. **MICROSOFT ACTIVE DIRECTORY Federasyon Hizmeti (ADFS) veya SAML 2,0 uyumlu KIMLIK sağlayıcısını** **tür**olarak seçin.

   1. Azure portal 'ten indirdiğiniz Federasyon meta veri XML 'sini karşıya yüklemek için, **Araştır** ' a tıklayın.

   1. **Devam** ' a tıklayın ve **kimlik sağlayıcısını Düzenle** bölümünde aşağıdaki adımları uygulayın:

      !["Kimlik sağlayıcısı grubu", "O C I Grubu", "eşleme Ekle" ve "Gönder" düğmesinin vurgulandığı "kimlik sağlayıcısını düzenleme" bölümünü gösteren ekran görüntüsü.](./media/oracle-cloud-tutorial/config09.png)

   1. **KIMLIK sağlayıcısı grubu** özel grup olarak seçilmelidir. Grup KIMLIĞI, Azure Active Directory grubun GUID 'SI olmalıdır. Grubun, **OCı grubu** alanında karşılık gelen grupla eşlenmesi gerekir.

   1. Azure portal ve kuruluşunuzun ihtiyaç duyduğu kuruluma göre birden çok grubu eşleyebilirsiniz. İhtiyaç duyduğunuz sayıda grup eklemek için **+ eşleme Ekle** ' ye tıklayın.

   1. **Gönder**' e tıklayın.
   
### <a name="create-oracle-cloud-infrastructure-console-test-user"></a>Oracle bulut altyapısı konsol test kullanıcısı oluşturma

 Oracle bulut altyapısı konsolu, varsayılan olarak tam zamanında sağlamayı destekler. Bu bölümde sizin için herhangi bir eylem öğesi yok. Erişim denemesi sırasında yeni bir Kullanıcı oluşturulmaz ve ayrıca Kullanıcı oluşturma gereksinimi yoktur.

### <a name="test-sso"></a>Test SSO 'SU

Erişim panelinde Oracle bulut altyapısı konsol kutucuğunu seçtiğinizde, Oracle bulut altyapısı Konsolu oturum açma sayfasına yönlendirilirsiniz. Açılan menüden **KIMLIK sağlayıcısını** seçin ve oturum açmak için aşağıda gösterildiği gibi **devam** ' a tıklayın. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](../user-help/my-apps-portal-end-user-access.md).

![Yapılandırma](./media/oracle-cloud-tutorial/config10.png)

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](./tutorial-list.md)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

- [Azure Active Directory Koşullu erişim nedir?](../conditional-access/overview.md)

- [Gelişmiş görünürlük ve denetimlerle Oracle bulut altyapısı konsolunu koruma](/cloud-app-security/proxy-intro-aad)