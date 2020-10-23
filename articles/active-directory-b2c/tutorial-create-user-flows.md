---
title: Öğretici-Kullanıcı akışları oluşturma-Azure Active Directory B2C
description: Azure Active Directory B2C ' de uygulamalarınız için kaydolma, oturum açma ve Kullanıcı profili düzenlemesini etkinleştirmek üzere Azure portal Kullanıcı akışları oluşturmayı öğrenmek için bu öğreticiyi izleyin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: tutorial
ms.date: 07/30/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: af0fafe98c3ca48ecced46cef6fb5fe8876c7f50
ms.sourcegitcommit: 8d8deb9a406165de5050522681b782fb2917762d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/20/2020
ms.locfileid: "92216003"
---
# <a name="tutorial-create-user-flows-in-azure-active-directory-b2c"></a>Öğretici: Azure Active Directory B2C Kullanıcı akışları oluşturma

Uygulamalarınızda, kullanıcıların kaydolmalarına, oturum açmalarına veya profillerini yönetmesine olanak tanıyan [Kullanıcı akışlarına](user-flow-overview.md) sahip olabilirsiniz. Azure Active Directory B2C (Azure AD B2C) kiracınızda farklı türlerde birden çok Kullanıcı akışı oluşturabilir ve bunları gerektiği şekilde uygulamalarınızda kullanabilirsiniz. Kullanıcı akışları, uygulamalar arasında yeniden kullanılabilir.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Kaydolma ve oturum açma Kullanıcı akışı oluşturma
> * Profil düzenlemesi Kullanıcı akışı oluşturma
> * Parola sıfırlama Kullanıcı akışı oluşturma

Bu öğreticide, Azure portal kullanarak önerilen bazı Kullanıcı akışlarını nasıl oluşturacağınız gösterilmektedir. Uygulamanızda bir kaynak sahibi parola kimlik bilgileri (ROPC) akışının nasıl ayarlanacağı hakkında bilgi arıyorsanız, bkz. [Azure AD B2C kaynak sahibi parola kimlik bilgileri akışını yapılandırma](configure-ropc.md).

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

> [!IMPORTANT]
> Kullanıcı akış sürümlerine başvurduğumuz şekilde değiştirdik. Daha önce v1 (üretime Ready) sürümleri ve V 1.1 ve v2 (Önizleme) sürümlerini sunuyoruz. Artık Kullanıcı akışlarını **Önerilen** (yeni nesil Önizleme) ve **Standart** (genel kullanıma açık) sürümler halinde birleştiriyoruz. Tüm V 1.1 ve v2 eski Preview Kullanıcı akışları **, 1 ağustos 2021 '** e kadar kullanımdan kalkmaya yönelik bir yoldur. Ayrıntılar için bkz. [Azure AD B2C Içindeki Kullanıcı akışı sürümleri](user-flow-versions.md).

## <a name="prerequisites"></a>Önkoşullar

Oluşturmak istediğiniz kullanıcı akışlarının parçası olan [uygulamalarınızı kaydedin](tutorial-register-applications.md) .

## <a name="create-a-sign-up-and-sign-in-user-flow"></a>Kaydolma ve oturum açma Kullanıcı akışı oluşturma

Kaydolma ve oturum açma Kullanıcı akışı, hem kayıt hem de oturum açma deneyimlerini tek bir yapılandırmayla işler. Uygulamanızın kullanıcıları, bağlama göre doğru yolun altına alınır.

1. [Azure portalında](https://portal.azure.com) oturum açın.
1. Portal araç çubuğunda **Dizin + abonelik** simgesini seçin ve ardından Azure AD B2C kiracınızı içeren dizini seçin.

    ![B2C kiracısı, dizin ve abonelik bölmesi, Azure portal](./media/tutorial-create-user-flows/directory-subscription-pane.png)

1. Azure portal, araması yapın ve **Azure AD B2C**seçin.
1. **İlkeler**altında **Kullanıcı akışları**' nı seçin ve ardından **Yeni Kullanıcı akışı**' nı seçin.

    ![Yeni Kullanıcı akışı düğmesi vurgulanmış şekilde portalda Kullanıcı akışları sayfası](./media/tutorial-create-user-flows/signup-signin-user-flow.png)

1. **Kullanıcı akışı oluştur** sayfasında, **oturum aç ve kullanıcı akışını oturum aç** ' ı seçin.

    ![Kaydolma ve oturum açma akışı vurgulanmış bir Kullanıcı akış sayfası seçin](./media/tutorial-create-user-flows/select-user-flow-type.png)

1. **Sürüm seçin**altında **Önerilen**' i seçin ve ardından **Oluştur**' u seçin. (Kullanıcı akışı sürümleri hakkında[daha fazla bilgi edinin](user-flow-versions.md) .)

    ![Azure portal ' de vurgulanan özelliklerle Kullanıcı akışı oluşturma sayfası](./media/tutorial-create-user-flows/select-version.png)

1. Kullanıcı akışı için bir **ad** girin. Örneğin, *signupsignin1*.
1. **Kimlik sağlayıcıları**Için **e-posta kaydı**' nı seçin.
1. **Kullanıcı öznitelikleri ve talepler**için, kayıt sırasında kullanıcıdan toplamak ve göndermek istediğiniz talepleri ve öznitelikleri seçin. Örneğin, **daha fazla göster**' i seçin ve ardından **ülke/bölge**, **görünen ad**ve **posta kodu**için öznitelikler ve talepler ' i seçin. **Tamam**'a tıklayın.

    ![Üç talep seçili olan öznitelikler ve talepler seçim sayfası](./media/tutorial-create-user-flows/signup-signin-attributes.png)

1. Kullanıcı akışını eklemek için **Oluştur** ' a tıklayın. *B2C_1* bir ön eki otomatik olarak ada eklenir.

### <a name="test-the-user-flow"></a>Kullanıcı akışını test etme

1. Genel Bakış sayfasını açmak için oluşturduğunuz kullanıcı akışını seçin ve ardından **Kullanıcı akışını Çalıştır**' ı seçin.
1. **Uygulama**için, daha önce kaydettiğiniz *WebApp1* adlı Web uygulamasını seçin. **Yanıt URL 'si** gösterilmesi gerekir `https://jwt.ms` .
1. **Kullanıcı akışını Çalıştır**' a tıklayın ve ardından **Şimdi kaydolun**' ı seçin.

    ![Kullanıcı akışını Çalıştır düğmesi vurgulanmış şekilde portalda Kullanıcı akış sayfasını Çalıştır](./media/tutorial-create-user-flows/signup-signin-run-now.PNG)

1. Geçerli bir e-posta adresi girin, **doğrulama kodu gönder**' e tıklayın, aldığınız doğrulama kodunu girin ve **kodu doğrula**' yı seçin.
1. Yeni bir parola girin ve parolayı onaylayın.
1. Ülkenizi ve bölgenizi seçin, görüntülenmesini istediğiniz adı girin, bir posta kodu girin ve ardından **Oluştur**' a tıklayın. Belirteç öğesine döner `https://jwt.ms` ve size gösterilmesi gerekir.
1. Artık kullanıcı akışını yeniden çalıştırabilirsiniz ve oluşturduğunuz hesapla oturum açabilmelisiniz. Döndürülen belirteç, ülke/bölge, ad ve posta kodu ' nu seçtiğiniz talepleri içerir.

> [!NOTE]
> "Kullanıcı akışını Çalıştır" deneyimi şu anda yetkilendirme kodu akışını kullanan SPA yanıt URL 'SI türüyle uyumlu değil. "Kullanıcı akışını Çalıştır" deneyimini bu tür uygulamalarla kullanmak için, "Web" türünde bir yanıt URL 'SI kaydedin ve [burada](tutorial-register-spa.md)açıklandığı gibi örtük akışı etkinleştirin.

## <a name="create-a-profile-editing-user-flow"></a>Profil düzenlemesi Kullanıcı akışı oluşturma

Kullanıcıların uygulamanızdaki profilini düzenlemesini etkinleştirmek istiyorsanız, bir profil düzenleme Kullanıcı akışı kullanın.

1. Azure AD B2C kiracının Genel Bakış sayfasının menüsünde **Kullanıcı akışları**' nı seçin ve ardından **Yeni Kullanıcı akışı**' nı seçin.
1. **Kullanıcı akışı oluştur** sayfasında **profil düzenlemesi** Kullanıcı akışı ' nı seçin. 
1. **Sürüm seçin**altında **Önerilen**' i seçin ve ardından **Oluştur**' u seçin.
1. Kullanıcı akışı için bir **ad** girin. Örneğin, *profileediting1*.
1. **Kimlik sağlayıcıları**Için **yerel hesap oturumu açma**' yı seçin.
2. **Kullanıcı öznitelikleri**için, müşterinin profilinde düzenleyebilmesini istediğiniz öznitelikleri seçin. Örneğin, **daha fazla göster**' i seçin ve **görünen ad** ve **iş unvanı**için her iki özniteliği ve talebi seçin. **Tamam**'a tıklayın.
3. Kullanıcı akışını eklemek için **Oluştur** ' a tıklayın. *B2C_1* bir ön eki otomatik olarak ada eklenir.

### <a name="test-the-user-flow"></a>Kullanıcı akışını test etme

1. Genel Bakış sayfasını açmak için oluşturduğunuz kullanıcı akışını seçin ve ardından **Kullanıcı akışını Çalıştır**' ı seçin.
1. **Uygulama**için, daha önce kaydettiğiniz *WebApp1* adlı Web uygulamasını seçin. **Yanıt URL 'si** gösterilmesi gerekir `https://jwt.ms` .
1. **Kullanıcı akışını Çalıştır**' a tıklayın ve daha önce oluşturduğunuz hesapla oturum açın.
1. Artık Kullanıcı için görünen adı ve iş başlığını değiştirme fırsatına sahipsiniz. **Devam**’a tıklayın. Belirteç öğesine döner `https://jwt.ms` ve size gösterilmesi gerekir.

## <a name="create-a-password-reset-user-flow"></a>Parola sıfırlama Kullanıcı akışı oluşturma

Uygulamanızın kullanıcılarının parolasını sıfırlamasına olanak tanımak için, parola sıfırlama kullanıcı akışını kullanırsınız.

1. Azure AD B2C kiracı genel bakış menüsünde **Kullanıcı akışları**' nı seçin ve ardından **Yeni Kullanıcı akışı**' nı seçin.
1. **Kullanıcı akışı oluştur** sayfasında, **parola sıfırlama** Kullanıcı akışını seçin. 
1. **Sürüm seçin**altında **Önerilen**' i seçin ve ardından **Oluştur**' u seçin.
1. Kullanıcı akışı için bir **ad** girin. Örneğin, *passwordreset1*.
1. **Kimlik sağlayıcıları**için **e-posta adresini kullanarak parolayı sıfırlamayı**etkinleştirin.
2. Uygulama talepleri bölümünde **daha fazla göster** ' e tıklayın ve uygulamanıza geri gönderilen yetkilendirme belirteçlerinde döndürülmesini istediğiniz talepleri seçin. Örneğin, **Kullanıcının Nesne Kimliği**’ni seçin.
3. **Tamam**'a tıklayın.
4. Kullanıcı akışını eklemek için **Oluştur** ' a tıklayın. *B2C_1* bir ön eki otomatik olarak ada eklenir.

### <a name="test-the-user-flow"></a>Kullanıcı akışını test etme

1. Genel Bakış sayfasını açmak için oluşturduğunuz kullanıcı akışını seçin ve ardından **Kullanıcı akışını Çalıştır**' ı seçin.
1. **Uygulama**için, daha önce kaydettiğiniz *WebApp1* adlı Web uygulamasını seçin. **Yanıt URL 'si** gösterilmesi gerekir `https://jwt.ms` .
1. **Kullanıcı akışını Çalıştır**' a tıklayın, daha önce oluşturduğunuz hesabın e-posta adresini doğrulayın ve **devam**' ı seçin.
1. Artık Kullanıcı parolasını değiştirme fırsatına sahipsiniz. Parolayı değiştirin ve **devam**' ı seçin. Belirteç öğesine döner `https://jwt.ms` ve size gösterilmesi gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, şu şekilde nasıl yapılacağını öğrendiniz:

> [!div class="checklist"]
> * Kaydolma ve oturum açma Kullanıcı akışı oluşturma
> * Profil düzenlemesi Kullanıcı akışı oluşturma
> * Parola sıfırlama Kullanıcı akışı oluşturma

Daha sonra, Azure AD, Amazon, Facebook, GitHub, LinkedIn, Microsoft veya Twitter gibi sağlayıcılardan Kullanıcı oturum açmasını etkinleştirmek için uygulamalarınıza kimlik sağlayıcıları ekleme hakkında bilgi edinin.

> [!div class="nextstepaction"]
> [Uygulamalarınıza kimlik sağlayıcıları ekleyin >](tutorial-add-identity-providers.md)
