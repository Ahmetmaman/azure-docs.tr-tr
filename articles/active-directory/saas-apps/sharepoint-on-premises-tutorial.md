---
title: 'Öğretici: SharePoint şirket içi ile Azure Active Directory tümleştirme | Microsoft Docs'
description: Şirket içi Azure Active Directory ve SharePoint arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 09/10/2020
ms.author: jeedes
ms.openlocfilehash: a693b22c609829f3bf6e76637eac5793d73703e6
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "96862318"
---
# <a name="tutorial-azure-active-directory-single-sign-on-integration-with-sharepoint-on-premises"></a>Öğretici: SharePoint şirket içi ile çoklu oturum açma tümleştirmesi Azure Active Directory

Bu öğreticide, SharePoint şirket içi Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. SharePoint şirket içi Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de şirket içi SharePoint 'e kimlerin erişimi olduğunu denetleyin.
* Kullanıcılarınızın Azure AD hesaplarıyla şirket içi SharePoint 'te otomatik olarak oturum açmalarına olanak sağlayın.
* Azure portal hesaplarınızı yönetin.

## <a name="prerequisites"></a>Önkoşullar

Şirket içi SharePoint ile Azure AD tümleştirmesini yapılandırmak için şu öğelere ihtiyacınız vardır:

* Bir Azure AD aboneliği. Azure AD ortamınız yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* SharePoint 2013 grubu veya daha yeni bir sürüm.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açma 'yı (SSO) bir test ortamında yapılandırıp test edersiniz. Azure AD kullanıcıları, şirket içi SharePoint 'nize erişebilir.

## <a name="create-enterprise-applications-in-the-azure-portal"></a>Azure portal kurumsal uygulamalar oluşturun

SharePoint 'in şirket içi tümleştirmesini Azure AD ile yapılandırmak için, Galeriden SharePoint Şirket içinden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

Galeriden SharePoint şirket içi eklemek için:

1. Azure portal en soldaki bölmede **Azure Active Directory**' i seçin.

   > [!NOTE]
   > Öğe kullanılamıyorsa, en sol bölmenin üst kısmındaki **tüm hizmetler** bağlantısı aracılığıyla da açabilirsiniz. Aşağıdaki genel bakışta, **Azure Active Directory** bağlantısı **kimlik** bölümünde bulunur. Filtre kutusunu kullanarak da arama yapabilirsiniz.

1. **Kurumsal uygulamalar**' a gidin ve **tüm uygulamalar**' ı seçin.

1. Yeni bir uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** ' yı seçin.

1. Arama kutusuna **SharePoint şirket içi** yazın. Sonuç bölmesinden **SharePoint şirket içi** seçeneğini belirleyin.

    <kbd>![Sonuç listesinde SharePoint şirket içi](./media/sharepoint-on-premises-tutorial/search-new-app.png)</kbd>

1. SharePoint şirket içi örneğiniz için bir ad belirtin ve uygulamayı eklemek için **Ekle** ' yi seçin.

1. Yeni kurumsal uygulamada **Özellikler**' i seçin ve **gerekli Kullanıcı atamasının** değerini denetleyin.

   <kbd>![Kullanıcı Ataması gerekli mi? den](./media/sharepoint-on-premises-tutorial/user-assignment-required.png)</kbd>

   Bu senaryoda değer **Hayır** olarak ayarlanır.

## <a name="configure-and-test-azure-ad"></a>Azure AD 'yi yapılandırma ve test etme

Bu bölümde, Azure AD SSO 'yu şirket içi SharePoint ile yapılandırırsınız. SSO 'nun çalışması için, bir Azure AD kullanıcısı ve şirket içi SharePoint 'te ilgili Kullanıcı arasında bir bağlantı ilişkisi kurarsınız.

Azure AD SSO 'yu şirket içi SharePoint ile yapılandırmak ve test etmek için şu yapı taşlarını doldurun:

- Kullanıcılarınızın bu özelliği kullanmasını sağlamak için [Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso) .
- Uygulama tarafında SSO ayarlarını yapılandırmak için [SharePoint şirket içi 'ı yapılandırın](#configure-sharepoint-on-premises) .
- SSO için Azure AD 'de yeni bir kullanıcı oluşturmak üzere [Azure Portal Azure AD test kullanıcısı oluşturun](#create-an-azure-ad-test-user-in-the-azure-portal) .
- SSO için Azure AD 'de yeni bir güvenlik grubu oluşturmak üzere [Azure Portal Azure AD güvenlik grubu oluşturun](#create-an-azure-ad-security-group-in-the-azure-portal) .
- Azure AD kullanıcısına izin vermek için [SharePoint 'te şirket Içi Azure AD hesabına Izin verin](#grant-permissions-to-an-azure-ad-account-in-sharepoint-on-premises) .
- Azure AD grubuna izinler vermek için [SharePoint 'te şirket Içi Azure AD grubuna Izin verin](#grant-permissions-to-an-azure-ad-group-in-sharepoint-on-premises) .
- Şirket içi SharePoint için Azure AD 'de Konuk hesabına izin vermek üzere [Azure Portal şirket Içi SharePoint 'e bir Konuk hesabı erişimi verin](#grant-access-to-a-guest-account-to-sharepoint-on-premises-in-the-azure-portal) .
- Birden çok Web uygulaması için [güvenilen kimlik sağlayıcısını](#configure-the-trusted-identity-provider-for-multiple-web-applications) , birden çok Web uygulaması için aynı güvenilir kimlik sağlayıcısını kullanacak şekilde yapılandırın.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Bu bölümde, Azure portal Azure AD SSO 'yu etkinleştirirsiniz.

Azure AD SSO 'yu SharePoint şirket içi ile yapılandırmak için:

1. Azure Portal   >  **Kurumsal uygulamalar** Azure Active Directory ' i seçin. Önceden oluşturulan kurumsal uygulama adını seçin ve **Çoklu oturum açma**' yı seçin.

1. **Çoklu oturum açma yöntemi seç** iletişim kutusunda, SSO 'yu etkinleştirmek için **SAML** modunu seçin.
 
1. **SAML Ile tek Sign-On ayarlama** sayfasında, **temel SAML yapılandırması** Iletişim kutusunu açmak için **Düzenle** simgesini seçin.

1. **Temel SAML yapılandırması** bölümünde şu adımları izleyin:

    ![SharePoint şirket içi etki alanı ve URL SSO bilgileri](./media/sharepoint-on-premises-tutorial/sp-identifier-reply.png)

    1. **Tanımlayıcı** kutusunda, bu stili kullanarak bir URL girin: `urn:<sharepointFarmName>:<federationName>` .

    1. **Yanıt URL 'si** kutusuna şu stili kullanarak bir URL girin: `https://<YourSharePointSiteURL>/_trust/` .

    1. **Oturum açma URL 'si** kutusunda, bu stili kullanarak bir URL girin: `https://<YourSharePointSiteURL>/` .
    1. **Kaydet**’i seçin.

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek oturum açma URL 'SI, tanımlayıcı ve yanıt URL 'siyle güncelleştirin.

1. **SAML Ile tek Sign-On ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, gereksinimlerinize göre verilen seçenekten **sertifikayı** indirmek için **İndir** ' i seçin ve bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/sharepoint-on-premises-tutorial/certificatebase64.png)

1. SharePoint Şirket **Içi ayarla** bölümünde, gereksiniminize göre uygun URL 'leri kopyalayın:
    
    - **Oturum açma URL 'SI**
    
        Oturum açma URL 'sini kopyalayın ve **/SAML2** ' i bitiş noktasında **/wsbestekine** benzer şekilde değiştirin https://login.microsoftonline.com/2c4f1a9f-be5f-10ee-327d-a95dac567e4f/wsfed . (Bu URL doğru değil.)

    - **Azure AD tanımlayıcısı**
    - **Oturum kapatma URL 'SI**

    > [!NOTE]
    > Bu URL, SharePoint 'te olduğu gibi kullanılamaz. **/SAML2** öğesini **/wsbesle** değiştirmelisiniz. SharePoint şirket içi uygulaması, bir SAML 1,1 belirteci kullanır, bu nedenle Azure AD, SharePoint sunucusundan bir WS Bestalebi bekliyor. Kimlik doğrulamasından sonra SAML 1,1 belirtecini yayınlar.

### <a name="configure-sharepoint-on-premises"></a>SharePoint 'i şirket içinde yapılandırma

1. SharePoint Server 2016 ' de yeni bir güvenilen kimlik sağlayıcısı oluşturun.

    SharePoint sunucusunda oturum açın ve SharePoint Yönetim Kabuğu 'nu açın. Değerleri girin:
    - **$Realm** , Azure Portal SharePoint şirket içi etki alanı ve URL 'ler bölümünün tanımlayıcı değeridir.
    - **$wsfedurl** , SSO hizmeti URL 'sidir.
    - **$FilePath** , sertifika dosyasını Azure Portal indirdiğiniz dosya yoludur.

    Yeni bir güvenilen kimlik sağlayıcısı yapılandırmak için aşağıdaki komutları çalıştırın.

    > [!TIP]
    > PowerShell 'i kullanmaya yeni başladıysanız veya PowerShell 'in nasıl çalıştığı hakkında daha fazla bilgi edinmek istiyorsanız bkz. [SharePoint PowerShell](/powershell/sharepoint/overview).


    ```
    $realm = "urn:sharepoint:sps201x"
    $wsfedurl="https://login.microsoftonline.com/2c4f1a9f-be5f-10ee-327d-a95dac567e4f/wsfed"
    $filepath="C:\temp\SharePoint 2019 OnPrem.cer"
    $cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($filepath)
    New-SPTrustedRootAuthority -Name "AzureAD" -Certificate $cert
    $map1 = New-SPClaimTypeMapping -IncomingClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name" -IncomingClaimTypeDisplayName "name" -LocalClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn"
    $map2 = New-SPClaimTypeMapping -IncomingClaimType "http://schemas.microsoft.com/ws/2008/06/identity/claims/role" -IncomingClaimTypeDisplayName "Role" -SameAsIncoming
    $ap = New-SPTrustedIdentityTokenIssuer -Name "AzureAD" -Description "Azure AD SharePoint server 201x" -realm $realm -ImportTrustCertificate $cert -ClaimsMappings $map1,$map2 -SignInUrl $wsfedurl -IdentifierClaim $map1.InputClaimType
    ```
1. Uygulamanız için güvenilen kimlik sağlayıcısını etkinleştirin.

    1. **Merkezi Yönetim**'de **Web uygulamasını Yönet** ' e gidin ve Azure AD ile güvenli hale getirmek istediğiniz Web uygulamasını seçin.

    1. Şeritte, **kimlik doğrulama sağlayıcıları** ' nı seçin ve kullanmak istediğiniz bölgeyi seçin.

    1. **Güvenilen kimlik sağlayıcısı**' nı seçin ve ardından *azuread* adlı yeni kaydettiğiniz sağlayıcıyı tanımla ' yı seçin.

    1. **Tamam**’ı seçin.

    ![Kimlik doğrulama sağlayıcınızı yapılandırma](./media/sharepoint-on-premises-tutorial/config-auth-provider.png)

### <a name="create-an-azure-ad-test-user-in-the-azure-portal"></a>Azure portal bir Azure AD test kullanıcısı oluşturun

Bu bölümün amacı, Azure portal bir test kullanıcısı oluşturmaktır.

1. Azure portal en soldaki bölmede **Azure Active Directory**' i seçin. **Yönet** bölmesinde, **Kullanıcılar**' ı seçin.

1. Ekranın üst kısmındaki **tüm kullanıcılar**  >  **Yeni Kullanıcı** ' yı seçin.

1. **Kullanıcı oluştur**' u seçin ve Kullanıcı Özellikleri ' nde bu adımları izleyin. Azure AD 'de kiracı sonekini veya doğrulanmış herhangi bir etki alanını kullanarak Kullanıcı oluşturabilirsiniz. 

    1. **Ad** kutusuna kullanıcı adını girin. **Testuser**'yi kullandık.
  
    1. **Kullanıcı adı** kutusuna `TestUser@yourcompanydomain.extension` girin. Bu örnekte gösterilmektedir `TestUser@contoso.com` .

       ![Kullanıcı iletişim kutusu](./media/sharepoint-on-premises-tutorial/user-properties.png)

    1. **Parolayı göster** onay kutusunu seçin ve ardından **parola** kutusunda görünen değeri yazın.

    1. **Oluştur**’u seçin.

    1. Artık siteyi ile paylaşabilir TestUser@contoso.com ve bu kullanıcıya erişime izin verebilirsiniz.

### <a name="create-an-azure-ad-security-group-in-the-azure-portal"></a>Azure portal bir Azure AD güvenlik grubu oluşturun

1. **Azure Active Directory**  >  **grupları**' nı seçin.

1. **Yeni Grup**' u seçin.

1. **Grup türü**, **Grup adı**, **Grup açıklaması** ve **üyelik türü** kutularını girin. Üyeler ' i seçmek için okları seçin ve ardından gruba eklemek istediğiniz üyeleri arayın veya seçin. Seçili üyeleri eklemek için **Seç** ' i seçin ve ardından **Oluştur**' u seçin.

![Azure AD güvenlik grubu oluşturma](./media/sharepoint-on-premises-tutorial/new-group.png)

### <a name="grant-permissions-to-an-azure-ad-account-in-sharepoint-on-premises"></a>Şirket içi SharePoint 'te bir Azure AD hesabına izin verme

SharePoint şirket içi SharePoint 'te bir Azure AD kullanıcısına erişim vermek için site koleksiyonunu paylaşma veya Azure AD kullanıcısını site koleksiyonunun gruplarından birine ekleme. Kullanıcılar artık Azure AD 'den kimlik kullanarak SharePoint 201x 'te oturum açabilirler, ancak kullanıcı deneyimine yönelik geliştirme için hala fırsatlar vardır. Örneğin, bir kullanıcının aranması, kişiler seçicisindeki birden çok arama sonucu sunar. Talep eşlemesinde oluşturulan her talep türü için bir arama sonucu vardır. Kişi seçiciyi kullanarak bir kullanıcı seçmek için, Kullanıcı adını tam olarak girmeniz ve **ad** talep sonucunu seçmeniz gerekir.

![Talep arama sonuçları](./media/sharepoint-on-premises-tutorial/claims-search-results.png)

Aradığınız değerlerde bir doğrulama yoktur; bu, yanlış bir talep türünü yanlışlıkla seçme hatalarına veya kullanıcılara yol açabilir. Bu durum, kullanıcıların kaynaklara başarıyla erişmesini engelleyebilir.

Bu senaryoyu kişiler seçiciyle düzeltmek için, [Azurecp](https://yvand.github.io/AzureCP/) adlı açık kaynaklı bir çözüm SharePoint 2013, 2016 ve 2019 için özel bir talep sağlayıcısı sağlar. Kullanıcıların hangi kullanıcılara girip doğrulama gerçekleştireceğini çözümlemek için Microsoft Graph API 'sini kullanır. Daha fazla bilgi için bkz. [Azurecp](https://yvand.github.io/AzureCP/).

  > [!NOTE]
  > AzureCP olmadan, Azure AD grubunun KIMLIĞINI ekleyerek gruplar ekleyebilirsiniz, ancak bu yöntem Kullanıcı dostu ve güvenilir değildir. Şöyle görünür:
  > 
  >![KIMLIĞE göre SharePoint grubuna bir Azure AD grubu ekleme](./media/sharepoint-on-premises-tutorial/adding-group-by-id.png)
  
### <a name="grant-permissions-to-an-azure-ad-group-in-sharepoint-on-premises"></a>SharePoint 'te şirket içi Azure AD grubuna izin verme

Şirket içi SharePoint 'e Azure AD güvenlik grupları atamak için SharePoint Server için özel bir talep sağlayıcısı kullanılması gerekir. Bu örnek AzureCP kullanır.

 > [!NOTE]
 > AzureCP, Microsoft ürünü değildir ve Microsoft Desteği tarafından desteklenmez. AzureCP 'yi şirket içi SharePoint grubunda indirmek, yüklemek ve yapılandırmak için bkz. [azurecp](https://yvand.github.io/AzureCP/) Web sitesi. 

1. AzureCP 'yi SharePoint şirket içi grubunda veya alternatif bir özel talep sağlayıcı çözümünde yapılandırın. AzureCP 'yi yapılandırmak için bu [azurecp](https://yvand.github.io/AzureCP/Register-App-In-AAD.html) Web sitesine bakın.

1. Azure Portal   >  **Kurumsal uygulamalar** Azure Active Directory ' i seçin. Önceden oluşturulan kurumsal uygulama adını seçin ve **Çoklu oturum açma**' yı seçin.

1. **SAML Ile tek Sign-On ayarlama** sayfasında, **Kullanıcı öznitelikleri & talepler** bölümünü düzenleyin.

1. **Grup talebi ekle**' yi seçin.

1. Kullanıcı ile ilişkili olan hangi grupların talep içinde döndürüleceğini seçin. Bu durumda, **tüm gruplar**' ı seçin. **Kaynak özniteliği** bölümünde **Grup Kimliği** ' ni seçin ve **Kaydet**' i seçin.

SharePoint şirket içinde Azure AD güvenlik grubuna erişim izni vermek için site koleksiyonunu paylaşma veya Azure AD güvenlik grubunu site koleksiyonunun gruplarından birine ekleme.

1. **SharePoint site koleksiyonuna** gidin. Site koleksiyonu için **site ayarları** ' nın altında, **kişiler ve gruplar**' ı seçin. 

1. SharePoint grubunu seçin ve ardından **Yeni**  >  **kullanıcıları bu gruba ekle**' yi seçin. Grubunuzun adını yazdığınızda, kişiler Seçicisi Azure AD güvenlik grubunu görüntüler.

    ![SharePoint grubuna bir Azure AD grubu ekleme](./media/sharepoint-on-premises-tutorial/permission-azure-ad-group.png)

### <a name="grant-access-to-a-guest-account-to-sharepoint-on-premises-in-the-azure-portal"></a>Azure portal şirket içi SharePoint 'e bir Konuk hesabı erişimi verme

UPN şimdi değiştirildiğinden, SharePoint sitenize bir Konuk hesabına erişimi tutarlı bir şekilde verebilirsiniz. Örneğin, Kullanıcı `jdoe@outlook.com` olarak temsil edilir `jdoe_outlook.com#ext#@TENANT.onmicrosoft.com` . Sitenizi dış kullanıcılarla paylaşmak için, Azure portal **Kullanıcı özniteliklerine & talepler** bölümüne bazı değişiklikler eklemeniz gerekir.

1. Azure Portal   >  **Kurumsal uygulamalar** Azure Active Directory ' i seçin. Önceden oluşturulan kurumsal uygulama adını seçin ve **Çoklu oturum açma**' yı seçin.

1. **SAML Ile tek Sign-On ayarlama** sayfasında, **Kullanıcı öznitelikleri & talepler** bölümünü düzenleyin.

1. **Gerekli talep** bölgesinde **benzersiz kullanıcı tanımlayıcısı (ad kimliği)** öğesini seçin.

1. **Kaynak özniteliği** özelliğini **User. localuserprincipalname** değeri olarak değiştirin ve **Kaydet**' i seçin.

    ![Kullanıcı öznitelikleri & talepler ilk kaynak özniteliği](./media/sharepoint-on-premises-tutorial/manage-claim.png)

1. Şeriti kullanarak **SAML tabanlı oturum açma**'ya geri dönün. Artık **Kullanıcı öznitelikleri & talepler** bölümü şöyle görünür: 

    ![Kullanıcı öznitelikleri & talepler son](./media/sharepoint-on-premises-tutorial/user-attributes-claims-final.png)

    > [!NOTE]
    > Bu kurulumda soyadı ve verilen ad gerekli değildir.

1. Azure portal, en soldaki bölmede **Azure Active Directory** ' i seçin ve ardından **Kullanıcılar**' ı seçin.

1. **Yeni Konuk Kullanıcı**' yı seçin.

1. **Kullanıcıyı davet et** seçeneğini belirleyin. Kullanıcı özelliklerini doldurup **davet et**' i seçin.

1. Artık siteyi ile paylaşabilir MyGuestAccount@outlook.com ve bu kullanıcıya erişime izin verebilirsiniz.

    ![Bir siteyi Konuk hesapla paylaşma](./media/sharepoint-on-premises-tutorial/sharing-guest-account.png)

### <a name="configure-the-trusted-identity-provider-for-multiple-web-applications"></a>Birden çok Web uygulaması için güvenilen kimlik sağlayıcısını yapılandırma

Yapılandırma tek bir Web uygulaması için çalışıyor, ancak birden çok Web uygulaması için aynı güvenilir kimlik sağlayıcısını kullanmayı düşünüyorsanız ek yapılandırma gerekir. Örneğin, URL 'YI kullanmak için bir Web uygulaması genişleteceğini `https://sales.contoso.com` ve artık kullanıcıların kimliğini doğrulamak istediğinizi varsayalım `https://marketing.contoso.com` . Bunu yapmak için, kimlik sağlayıcısını WReply parametresini kabul etmek üzere güncelleştirin ve bir yanıt URL 'SI eklemek için Azure AD 'de uygulama kaydını güncelleştirin.

1. Azure Portal   >  **Kurumsal uygulamalar** Azure Active Directory ' i seçin. Önceden oluşturulan kurumsal uygulama adını seçin ve **Çoklu oturum açma**' yı seçin.

1. **SAML Ile tek Sign-On ayarlama** sayfasında, **temel SAML yapılandırmasını** düzenleyin.

    ![Temel SAML yapılandırması](./media/sharepoint-on-premises-tutorial/add-reply-url.png)

1. **Yanıt URL 'si (onaylama tüketici hizmeti URL 'si)** için ek Web uygulamalarının URL 'sini ekleyin ve **Kaydet**' i seçin.

    ![Temel SAML yapılandırmasını düzenleme](./media/sharepoint-on-premises-tutorial/reply-url-for-web-application.png)

1. SharePoint sunucusunda, SharePoint 201x Yönetim Kabuğu 'nu açın ve aşağıdaki komutları çalıştırın. Daha önce kullandığınız güvenilen kimlik belirteci verenin adını kullanın.
    ```
    $t = Get-SPTrustedIdentityTokenIssuer "AzureAD"
    $t.UseWReplyParameter=$true
    $t.Update()
    ```
1. **Merkezi Yönetim**'de Web uygulamasına gidin ve mevcut güvenilen kimlik sağlayıcısını etkinleştirin.

İç kullanıcılarınız için SharePoint şirket içi örneğine erişim vermek istediğiniz başka senaryolar olabilir. Bu senaryo için, şirket içi kullanıcılarınızı Azure AD ile eşitlemeye izin vermek üzere Microsoft Azure Active Directory Connect dağıtmanız gerekir. Bu kurulum, başka bir makalede ele alınmıştır.

## <a name="next-steps"></a>Sonraki Adımlar

SharePoint 'i şirket içinde yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak ayıklanmasını ve zaman korumasını koruyan oturum denetimini zorunlu kılabilirsiniz. Oturum denetimi koşullu erişimden genişletiliyor. [Microsoft Cloud App Security ile oturum denetimini nasıl zorlayacağınızı öğrenin](/cloud-app-security/proxy-deployment-aad)
