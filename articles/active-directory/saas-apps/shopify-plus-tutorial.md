---
title: 'Öğretici: Shopify Plus ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve Shopify Plus arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 02/11/2021
ms.author: jeedes
ms.openlocfilehash: 65f4963f23d97ca2e3af34febb0d5dbea652fc12
ms.sourcegitcommit: a67b972d655a5a2d5e909faa2ea0911912f6a828
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2021
ms.locfileid: "101646996"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-shopify-plus"></a>Öğretici: Shopify Plus ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory

Bu öğreticide, Shopify Plus 'ı Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. Shopify Plus 'ı Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de Shopify Plus erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Shopify Plus 'ta otomatik olarak oturum açmalarına olanak sağlayın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Shopify ve çoklu oturum açma (SSO) etkin aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz.

* Shopify Plus **, SP ve ıDP** tarafından başlatılan SSO 'yu destekler.

## <a name="add-shopify-plus-from-the-gallery"></a>Galeriden Shopify Plus ekleyin

Shopify Plus 'ın Azure AD Ile tümleştirilmesini yapılandırmak için, Galeriden Shopify Plus 'yi yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure portal iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Shopify Plus** yazın.
1. Sonuçlar panelinden **Shopify Plus** ' ı seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-sso-for-shopify-plus"></a>Shopify Plus için Azure AD SSO 'yu yapılandırma ve test etme

**B. Simon** adlı bir test kullanıcısı kullanarak Azure AD SSO 'Yu Shopify Plus ile yapılandırın ve test edin. SSO 'nun çalışması için, Shopify Plus içindeki bir Azure AD kullanıcısı ve ilgili Kullanıcı arasında bir bağlantı ilişkisi oluşturmanız gerekir.

Azure AD SSO 'yu Shopify Plus ile yapılandırmak ve test etmek için aşağıdaki adımları gerçekleştirin:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
    1. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
    1. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
1. Uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için **[Shopify Plus SSO 'Yu yapılandırın](#configure-shopify-plus-sso)** .
    1. **[Shopify ve test kullanıcısı oluşturun](#create-shopify-plus-test-user)** ve bu, kullanıcının Azure AD gösterimine bağlı olan Shopify Plus 'ta B. Simon 'ın bir karşılığı olacak şekilde.
1. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. Azure portal, **Shopify Plus** uygulama tümleştirmesi sayfasında **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölümünde, **IDP** tarafından başlatılan modda uygulamayı yapılandırmak istiyorsanız aşağıdaki alanlar için değerleri girin:

    **Yanıt URL 'si** metin kutusuna aşağıdaki kalıbı kullanarak bir URL yazın:`https://accounts.shopify.com/saml/consume/organization/<ORGANIZATION_ID>`

1. Uygulamayı **SP** tarafından başlatılan modda yapılandırmak Istiyorsanız **ek URL 'ler ayarla** ' ya tıklayın ve aşağıdaki adımı gerçekleştirin:

    **Oturum açma URL** 'si metin kutusuna URL 'yi yazın:`https://shopify.plus/login`

    > [!NOTE]
    > Yanıt URL 'SI değeri gerçek değil. Değeri gerçek yanıt URL 'siyle güncelleştirin. Değeri almak için [Shopify Plus istemci desteği ekibine](mailto:plus-user-management@shopify.com) başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

1. Shopify Plus uygulaması, SAML belirteci öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML onayları bekler. Aşağıdaki ekran görüntüsünde varsayılan özniteliklerin listesi gösterilmektedir.

    ![image](common/default-attributes.png)

1. Shopify Plus uygulamasının yanı sıra, aşağıda gösterilen SAML yanıtına daha fazla öznitelik geçirilmesini bekler. Bu öznitelikler de önceden doldurulur, ancak gereksinimlerinize göre bunları gözden geçirebilirsiniz.

    | Name | Kaynak özniteliği|
    | ---- | --------------- |
    | e-posta | Kullanıcı. Mail |

1. **Ad kimlik** biçimini **kalıcı** olarak değiştirin. **Benzersiz kullanıcı tanımlayıcısı (ad kimliği)** seçeneğini belirleyin ve ardından **ad tanımlayıcı** biçimini seçin. Bu seçenek için **persistent** ' i seçin. Yaptığınız değişiklikleri kaydedin.
1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, Kopyala düğmesini seçerek **App Federasyon meta verileri URL 'sini** kopyalayın ve bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](common/copy-metadataurl.png)

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

Bu bölümde, Shopify Plus 'a erişim vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde **Shopify Plus**' yi seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.
1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.
1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. Kullanıcılara bir rolün atanmasını bekliyorsanız, **Rol Seç** açılır listesinden bunu seçebilirsiniz. Bu uygulama için ayarlanmış bir rol yoksa, "varsayılan erişim" rolü seçili olduğunu görürsünüz.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

## <a name="configure-shopify-plus-sso"></a>Shopify Plus SSO 'yu yapılandırma

Tüm adımları görüntülemek için, [Shopify 'un SAML tümleştirmelerini ayarlama hakkında](https://help.shopify.com/en/manual/shopify-plus/saml)bölümüne bakın.

**Shopify Plus** tarafında çoklu oturum açmayı yapılandırmak için Azure Active Directory 'Den **uygulama Federasyon meta veri URL 'sini** kopyalayın. Ardından, [Kuruluş Yöneticisi](https://shopify.plus) ' nde oturum açın ve **Kullanıcılar**  >  **güvenlik**' e gidin. **Yapılandırma ayarla**' yı seçin ve ardından **kimlik sağlayıcısı meta veri URL** 'Si bölümüne uygulamanızın Federasyon meta veri URL 'sini yapıştırın. Bu adımı gerçekleştirmek için **Ekle** ' yi seçin.

### <a name="create-shopify-plus-test-user"></a>Shopify Plus test kullanıcısı oluşturma

Bu bölümde, Shopify Plus içinde B. Simon adlı bir Kullanıcı oluşturacaksınız. **Kullanıcılar** bölümüne dönün ve e-postalarını ve izinlerini girerek bir kullanıcı ekleyin. Çoklu oturum açma kullanılmadan önce kullanıcıların oluşturulması ve etkinleştirilmesi gerekir.

### <a name="enforce-saml-authentication"></a>SAML kimlik doğrulamasını zorla

> [!NOTE]
> Büyük ölçüde uygulamadan önce bireysel kullanıcıları kullanarak tümleştirmeyi test etmenizi öneririz.

Bireysel kullanıcılar:
1. Azure AD tarafından yönetilen bir e-posta etki alanı ile Shopify ve Shopify Plus ' de doğrulanan tek bir kullanıcının sayfasına gidin.
1. SAML kimlik doğrulaması bölümünde **Düzenle**' yi seçin, **gerekli**' ı seçin ve ardından **Kaydet**' i seçin.
1. Bu kullanıcının IDP tarafından başlatılan ve SP tarafından başlatılan akışlar aracılığıyla başarıyla oturum açmasını test edebilirsiniz.

Bir e-posta etki alanı altındaki tüm kullanıcılar için:
1. **Güvenlik** sayfasına dönün.
1. SAML kimlik doğrulaması ayarınız için **gerekli** ' yi seçin. Bu, Shopify Plus genelindeki e-posta etki alanına sahip tüm kullanıcılar için SAML 'yi zorlar.
1. **Kaydet**’i seçin.

> [!IMPORTANT]
> Bir e-posta etki alanı altındaki tüm kullanıcılar için SAML 'nin etkinleştirilmesi, bu uygulamayı kullanan tüm kullanıcıları etkiler. Kullanıcılar, normal oturum açma sayfasını kullanarak oturum açamaz. Yalnızca Azure Active Directory aracılığıyla uygulamaya erişebilecekler. Shopify, kullanıcıların normal kullanıcı adını ve parolasını kullanarak oturum açmasını sağlayan bir yedekleme oturum açma URL 'SI sağlamaz. Gerekirse SAML 'yi kapatmak için Shopify desteğine başvurabilirsiniz.

## <a name="test-sso"></a>Test SSO 'SU 

Bu bölümde, Azure AD çoklu oturum açma yapılandırmanızı aşağıdaki seçeneklerle test edersiniz. 

#### <a name="sp-initiated"></a>SP başlatıldı:

* Azure portal içinde **Bu uygulamayı test et** ' e tıklayın. Bu, oturum açma akışını başlatabileceğiniz Shopify ve oturum açma URL 'sine yönlendirir.  

* Doğrudan Shopify Plus oturum açma URL 'sine gidin ve oturum açma akışını buradan başlatın.

#### <a name="idp-initiated"></a>IDP başlatıldı:

* Azure portal **Bu uygulamayı test et** ' e tıklayın ve SSO 'Yu ayarladığınız Shopify Plus ' de otomatik olarak oturum açmış olmanız gerekir. 

Uygulamayı dilediğiniz modda test etmek için Microsoft My Apps ' i de kullanabilirsiniz. Uygulamamda Shopify Plus kutucuğuna tıkladığınızda, SP modunda yapılandırıldıysa, oturum açma akışını başlatmak için uygulama oturum açma sayfasına yönlendirilirsiniz ve ıDP modunda yapılandırıldıysa, SSO 'yu ayarladığınız Shopify Plus 'da otomatik olarak oturum açmanız gerekir. Uygulamalarım hakkında daha fazla bilgi için bkz. [uygulamalarıma giriş](../user-help/my-apps-portal-end-user-access.md).

## <a name="next-steps"></a>Sonraki adımlar

Shopify 'ı yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak ayıklanmasını ve zaman korumasını koruyan oturum denetimini zorunlu kılabilirsiniz. Oturum denetimi koşullu erişimden genişletiliyor. [Microsoft Cloud App Security ile oturum denetimini nasıl zorlayacağınızı öğrenin](/cloud-app-security/proxy-deployment-any-app).