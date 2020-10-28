---
title: Özel ilkeler kullanarak ADFS 'yi SAML kimlik sağlayıcısı olarak ekleme
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C 'de SAML protokolünü ve özel ilkeleri kullanarak ADFS 2016 'yi ayarlama
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 02/27/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 60bc4623416eeb491d073dba9517ac13861a3e9e
ms.sourcegitcommit: fb3c846de147cc2e3515cd8219d8c84790e3a442
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2020
ms.locfileid: "92633459"
---
# <a name="add-adfs-as-a-saml-identity-provider-using-custom-policies-in-azure-active-directory-b2c"></a>Azure Active Directory B2C içindeki özel ilkeleri kullanarak ADFS 'yi SAML kimlik sağlayıcısı olarak ekleyin

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makalede, Azure Active Directory B2C (Azure AD B2C) içinde [özel ilkeler](custom-policy-overview.md) kullanarak bir ADFS Kullanıcı hesabı için oturum açma 'nın nasıl etkinleştirileceği gösterilmektedir. Bir [SAML kimlik sağlayıcısı teknik profilini](saml-identity-provider-technical-profile.md) özel ilkeye ekleyerek oturum açmayı etkinleştirebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

- [Azure Active Directory B2C özel ilkeleri kullanmaya başlama](custom-policy-get-started.md)bölümündeki adımları uygulayın.
- Özel anahtarı olan bir Certificate. pfx dosyasına erişiminiz olduğundan emin olun. Kendi imzalı sertifikanızı oluşturup Azure AD B2C yükleyebilirsiniz. Azure AD B2C, SAML kimlik sağlayıcınıza gönderilen SAML isteğini imzalamak için bu sertifikayı kullanır. Sertifika oluşturma hakkında daha fazla bilgi için bkz. [imza sertifikası oluşturma](identity-provider-salesforce-custom.md#generate-a-signing-certificate).
- Azure 'un. pfx dosya parolasını kabul edebilmesi için parola, AES256-SHA256 aksine Windows sertifika deposu dışarı aktarma yardımcı programı 'ndaki TripleDES-SHA1 seçeneğiyle şifrelenmelidir.

## <a name="create-a-policy-key"></a>İlke anahtarı oluşturma

Sertifikanızı Azure AD B2C kiracınızda depolamanız gerekir.

1. [Azure portalında](https://portal.azure.com/) oturum açın.
2. Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun. Üstteki menüden **Dizin + abonelik** filtresini seçin ve kiracınızı içeren dizini seçin.
3. Azure portal sol üst köşesindeki **tüm hizmetler** ' i seçin ve ardından **Azure AD B2C** ' i arayıp seçin.
4. Genel Bakış sayfasında **kimlik deneyimi çerçevesi** ' ni seçin.
5. **Ilke anahtarlarını** seçin ve ardından **Ekle** ' yi seçin.
6. **Seçenekler** için öğesini seçin `Upload` .
7. İlke anahtarı için bir **ad** girin. Örneğin, `SamlCert`. Ön ek, `B2C_1A_` anahtarınızın adına otomatik olarak eklenir.
8. Özel anahtarla Certificate. pfx dosyanıza gidin ve bu dosyayı seçin.
9. **Oluştur** 'a tıklayın.

## <a name="add-a-claims-provider"></a>Talep sağlayıcısı ekleme

Kullanıcıların bir ADFS hesabı kullanarak oturum açmasını istiyorsanız, hesabı Azure AD B2C bir uç nokta aracılığıyla iletişim kurabildiği bir talep sağlayıcı olarak tanımlamanız gerekir. Uç noktası, belirli bir kullanıcının kimliği doğrulandığını doğrulamak için Azure AD B2C tarafından kullanılan bir talep kümesi sağlar.

Bir ADFS hesabını, ilkenizin uzantı dosyasındaki **Claimsproviders** öğesine ekleyerek talep sağlayıcı olarak tanımlayabilirsiniz. Daha fazla bilgi için bkz. [SAML kimlik sağlayıcısı teknik profili tanımlama](saml-identity-provider-technical-profile.md).

1. *TrustFrameworkExtensions.xml* açın.
1. **Claimsproviders** öğesini bulun. Yoksa, kök öğenin altına ekleyin.
1. Yeni bir **ClaimsProvider** 'ı aşağıdaki şekilde ekleyin:

    ```xml
    <ClaimsProvider>
      <Domain>contoso.com</Domain>
      <DisplayName>Contoso ADFS</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="Contoso-SAML2">
          <DisplayName>Contoso ADFS</DisplayName>
          <Description>Login with your ADFS account</Description>
          <Protocol Name="SAML2"/>
          <Metadata>
            <Item Key="WantsEncryptedAssertions">false</Item>
            <Item Key="PartnerEntity">https://your-ADFS-domain/federationmetadata/2007-06/federationmetadata.xml</Item>
            <Item Key="XmlSignatureAlgorithm">Sha256</Item>
          </Metadata>
          <CryptographicKeys>
            <Key Id="SamlMessageSigning" StorageReferenceId="B2C_1A_SamlCert"/>
          </CryptographicKeys>
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="issuerUserId" PartnerClaimType="userPrincipalName" />
            <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name"/>
            <OutputClaim ClaimTypeReferenceId="surname" PartnerClaimType="family_name"/>
            <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="email"/>
            <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name"/>
            <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="contoso.com" />
            <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication"/>
          </OutputClaims>
          <OutputClaimsTransformations>
            <OutputClaimsTransformation ReferenceId="CreateRandomUPNUserName"/>
            <OutputClaimsTransformation ReferenceId="CreateUserPrincipalName"/>
            <OutputClaimsTransformation ReferenceId="CreateAlternativeSecurityId"/>
            <OutputClaimsTransformation ReferenceId="CreateSubjectClaimFromAlternativeSecurityId"/>
          </OutputClaimsTransformations>
          <UseTechnicalProfileForSessionManagement ReferenceId="SM-Saml-idp"/>
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
    ```

1. Öğesini `your-ADFS-domain` ADFS etki alanınız adıyla değiştirin ve **IdentityProvider** çıkış talebinin değerini DNS ile değiştirin (etki alanınızı gösteren rastgele değer).

1. Bölümünü bulun `<ClaimsProviders>` ve AŞAĞıDAKI XML kod parçacığını ekleyin. İlkeniz zaten `SM-Saml-idp` Teknik profili içeriyorsa, sonraki adıma atlayın. Daha fazla bilgi için bkz. [Çoklu oturum açma oturumu yönetimi](custom-policy-reference-sso.md).

    ```xml
    <ClaimsProvider>
      <DisplayName>Session Management</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="SM-Saml-idp">
          <DisplayName>Session Management Provider</DisplayName>
          <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.SamlSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
          <Metadata>
            <Item Key="IncludeSessionIndex">false</Item>
            <Item Key="RegisterServiceProviders">false</Item>
          </Metadata>
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
    ```

1. Dosyayı kaydedin.

### <a name="upload-the-extension-file-for-verification"></a>Uzantı dosyasını doğrulama için karşıya yükle

Şimdi, ilkenizi Azure AD B2C ADFS hesabıyla nasıl iletişim kuracağını bilmesini sağlayacak şekilde yapılandırdınız. Şu ana kadar herhangi bir sorun olmadığını doğrulamak için, ilkenizin uzantı dosyasını karşıya yüklemeyi deneyin.

1. Azure AD B2C kiracınızdaki **özel ilkeler** sayfasında, **ilkeyi karşıya yükle** ' yi seçin.
2. Varsa **Ilkenin üzerine yazmayı** etkinleştirin ve sonra *TrustFrameworkExtensions.xml* dosyasına gidip seçin.
3. **Karşıya Yükle** 'ye tıklayın.

> [!NOTE]
> Visual Studio Code B2C uzantısı "Socialidpuserıd" kullanır. ADFS için de sosyal bir ilke gereklidir.
>

## <a name="register-the-claims-provider"></a>Talep sağlayıcısını Kaydet

Bu noktada, kimlik sağlayıcısı ayarlanmıştır, ancak kaydolma veya oturum açma ekranlarından hiçbirinde kullanılamaz. Kullanılabilir hale getirmek için, var olan bir şablon Kullanıcı yolculuğunun bir yinelemesini oluşturun ve ardından bunu, ADFS kimlik sağlayıcısı da olacak şekilde değiştirin.

1. *TrustFrameworkBase.xml* dosyasını başlangıç paketinden açın.
2. Dahil olan **Userelde ney** öğesinin tüm içeriğini bulup kopyalayın `Id="SignUpOrSignIn"` .
3. *TrustFrameworkExtensions.xml* açın ve **User, neys** öğesini bulun. Öğe yoksa, bir tane ekleyin.
4. **User, neys** öğesinin bir alt öğesi olarak kopyaladığınız **User, ney** öğesinin tüm içeriğini yapıştırın.
5. Kullanıcı yolculuğunun KIMLIĞINI yeniden adlandırın. Örneğin, `SignUpSignInADFS`.

### <a name="display-the-button"></a>Düğmeyi görüntüleme

**Claimsproviderselection** öğesi, kaydolma veya oturum açma ekranındaki bir kimlik sağlayıcısı düğmesine benzer. Bir ADFS hesabı için bir **Claimsproviderselection** öğesi eklerseniz, bir Kullanıcı sayfada yer alıyorsa yeni bir düğme görüntülenir.

1. Oluşturduğunuz Kullanıcı yolculuğuna dahil olan **Orchestrationstep** öğesini bulun `Order="1"` .
2. **Claimsproviderseçimleri** altında aşağıdaki öğeyi ekleyin. **Targetclaimsexchangeıd** değerini uygun bir değere ayarlayın, örneğin `ContosoExchange` :

    ```xml
    <ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
    ```

### <a name="link-the-button-to-an-action"></a>Düğmeyi bir eyleme bağlama

Artık bir düğmeye sahip olduğunuza göre, bunu bir eyleme bağlamanız gerekir. Bu durumda, bir belirteci almak için bir ADFS hesabıyla iletişim kurması Azure AD B2C.

1. Kullanıcı yolculuğu ' nda yer alan **Orchestrationstep** ' i bulun `Order="2"` .
2. **Targetclaimsexchangeıd** IÇIN kullandığınız kimlik için aynı değeri kullandığınızdan emin olmak Için aşağıdaki **claimsexchange** öğesini ekleyin:

    ```xml
    <ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="Contoso-SAML2" />
    ```

    **TechnicalProfileReferenceId** değerini daha önce oluşturduğunuz teknık profilin kimliğiyle güncelleştirin. Örneğin, `Contoso-SAML2`.

3. *TrustFrameworkExtensions.xml* dosyasını kaydedin ve doğrulama için yeniden yükleyin.


## <a name="configure-an-adfs-relying-party-trust"></a>ADFS bağlı olan taraf güveni yapılandırma

ADFS 'yi Azure AD B2C bir kimlik sağlayıcısı olarak kullanmak için, Azure AD B2C SAML meta verileriyle bir ADFS bağlı olan taraf güveni oluşturmanız gerekir. Aşağıdaki örnekte, bir Azure AD B2C teknik profilinin SAML meta verilerine yönelik bir URL adresi gösterilmektedir:

```
https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/your-policy/samlp/metadata?idptp=your-technical-profile
```

Aşağıdaki değerleri değiştirin:

- Kiracı **adınızla kiracınız** , Your-Tenant.onmicrosoft.com gibi.
- ilke adınızla **ilkenize** . Örneğin, B2C_1A_signup_signin_adfs.
- SAML kimlik sağlayıcısı teknik profilinizin adı ile **Teknik profiliniz** . Örneğin, contoso-SAML2.

Bir tarayıcı açın ve URL 'ye gidin. Doğru URL 'YI yazdığınızdan ve XML meta veri dosyasına erişiminiz olduğundan emin olun. ADFS Yönetimi ek bileşenini kullanarak yeni bir bağlı olan taraf güveni eklemek ve ayarları el ile yapılandırmak için, bir federasyon sunucusu üzerinde aşağıdaki yordamı gerçekleştirin. Bu yordamı gerçekleştirmek için gereken en düşük düzeyde **Yöneticiler** veya yerel bilgisayar üzerinde üyelik gereklidir.

1. Sunucu Yöneticisi ' de **Araçlar** ' ı seçin ve ardından **ADFS yönetimi** ' ni seçin.
2. **Bağlı olan taraf güveni Ekle** ' yi seçin.
3. **Hoş geldiniz** sayfasında **talep duyarlı** ' i seçin ve ardından **Başlat** ' a tıklayın.
4. **Veri kaynağı seç** sayfasında, **bağlı olan taraf hakkında verileri içeri aktar ' ı seçin veya yerel bir ağ üzerinde** , Azure AD B2C meta veri URL 'Nizi girin ve ardından **İleri** ' ye tıklayın.
5. **Görünen ad belirtin** sayfasında, bir **görünen ad** girin, **Notlar** bölümüne bu bağlı olan taraf güveni Için bir açıklama girin ve ardından **İleri** ' ye tıklayın.
6. **Access Control Ilkesi seçin** sayfasında, bir ilke seçin ve ardından **İleri** ' ye tıklayın.
7. **Güven eklemeye hazırlanma** sayfasında, ayarları gözden geçirin ve ardından bağlı olan taraf güven bilgilerinizi kaydetmek için **İleri** ' ye tıklayın.
8. **Son** sayfasında, **Kapat** ' a tıklayın, bu eylem **talep kurallarını Düzenle** iletişim kutusunu otomatik olarak görüntüler.
9. **Kural Ekle** ' yi seçin.
10. **Talep kuralı şablonu** ' nda, **LDAP özniteliklerini talep olarak gönder** ' i seçin.
11. Bir **talep kuralı adı** belirtin. **Öznitelik deposu** Için **Active Directory Seç** ' i seçin, aşağıdaki talepleri ekleyin ve ardından **son** ' a ve **Tamam** ' a tıklayın.

    | LDAP özniteliği | Giden talep türü |
    | -------------- | ------------------- |
    | Kullanıcı Asıl Adı | userPrincipalName |
    | Soyadı | family_name |
    | Given-Name | given_name |
    | E-posta adresi | e-posta |
    | Display-Name | name |

    Bu adların, giden talep türü açılan listesinde görüntülemediğine unutmayın. Bunları el ile yazmanız gerekir. (Açılan menü aslında düzenlenebilir).

12.  Sertifika türü temelinde, karma algoritmayı ayarlamanız gerekebilir. Bağlı olan taraf güveni (B2C tanıtımı) Özellikler penceresinde **Gelişmiş** sekmesini seçin ve **güvenli karma algoritmasını** olarak değiştirin `SHA-256` ve **Tamam** ' a tıklayın.
13. Sunucu Yöneticisi ' de **Araçlar** ' ı seçin ve ardından **ADFS yönetimi** ' ni seçin.
14. Oluşturduğunuz bağlı olan taraf güvenini seçin, **Federasyon meta verilerinden Güncelleştir** ' i seçin ve ardından **Güncelleştir** ' e tıklayın.

## <a name="create-an-azure-ad-b2c-application"></a>Azure AD B2C uygulaması oluşturma

Azure AD B2C ile iletişim, B2C kiracınıza kaydolmanızı sağlayan bir uygulama aracılığıyla oluşur. Bu bölümde, daha önce yapmadıysanız bir test uygulaması oluşturmak için tamamlayabildiğiniz isteğe bağlı adımlar listelenmektedir.

[!INCLUDE [active-directory-b2c-appreg-idp](../../includes/active-directory-b2c-appreg-idp.md)]

### <a name="update-and-test-the-relying-party-file"></a>Bağlı olan taraf dosyasını güncelleştirme ve test etme

Oluşturduğunuz Kullanıcı yolculuğunu başlatan bağlı olan taraf (RP) dosyasını güncelleştirin.

1. Çalışma dizininizde *SignUpOrSignIn.xml* bir kopyasını oluşturun ve yeniden adlandırın. Örneğin, *SignUpSignInADFS.xml* olarak yeniden adlandırın.
2. Yeni dosyayı açın ve **TrustFrameworkPolicy** Için **PolicyId** özniteliğinin değerini benzersiz bir değerle güncelleştirin. Örneğin, `SignUpSignInADFS`.
3. **Publicpolicyuri** DEĞERINI ilke URI 'siyle güncelleştirin. Örneğin,`http://contoso.com/B2C_1A_signup_signin_adfs`
4. **Defaultuseryolculuney** Içindeki **referenceıd** özniteliğinin değerini, oluşturduğunuz yenı Kullanıcı yolculuğunun kimliğiyle eşleşecek şekilde güncelleştirin (Signupsignınadfs).
5. Değişikliklerinizi kaydedin, dosyayı karşıya yükleyin ve ardından listeden yeni ilkeyi seçin.
6. Oluşturduğunuz Azure AD B2C uygulamasının **Uygulama Seç** alanında seçildiğinden emin olun ve **Şimdi Çalıştır** ' a tıklayarak test edin.

