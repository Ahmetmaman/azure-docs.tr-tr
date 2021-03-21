---
title: Yerelleştirme-Azure Active Directory B2C
description: Azure Active Directory B2C bir özel ilkenin yerelleştirme öğesini belirtin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 03/08/2021
ms.author: mimart
ms.subservice: B2C
ms.custom: b2c-support
ms.openlocfilehash: 3a5afcd8c0ef0c31353cd2369ead332675c9877f
ms.sourcegitcommit: e6de1702d3958a3bea275645eb46e4f2e0f011af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102453130"
---
# <a name="localization-element"></a>Yerelleştirme öğesi

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

**Yerelleştirme** öğesi, Kullanıcı yolculukları için ilkedeki birden çok yerel ayarı veya dili desteketmenize olanak tanır. İlkelerde yerelleştirme desteği şunları yapmanıza olanak sağlar:

- Bir ilkede desteklenen dillerin açık listesini ayarlayın ve varsayılan bir dil seçin.
- Dile özgü dizeler ve koleksiyonlar sağlayın.

```xml
<Localization Enabled="true">
  <SupportedLanguages DefaultLanguage="en" MergeBehavior="ReplaceAll">
    <SupportedLanguage>en</SupportedLanguage>
    <SupportedLanguage>es</SupportedLanguage>
  </SupportedLanguages>
  <LocalizedResources Id="api.localaccountsignup.en">
  <LocalizedResources Id="api.localaccountsignup.es">
  ...
```

**Yerelleştirme** öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Etkin | No | Olası değerler: `true` veya `false` . |

**Yerelleştirme** Öğesı aşağıdaki XML öğelerini içerir

| Öğe | Öğeleri | Description |
| ------- | ----------- | ----------- |
| SupportedLanguages | 1: n | Desteklenen dillerin listesi. |
| LocalizedResources | 0: n | Yerelleştirilmiş kaynakların listesi. |

## <a name="supportedlanguages"></a>SupportedLanguages

**Supportedlanguages** öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| DefaultLanguage | Yes | Yerelleştirilmiş kaynaklar için varsayılan olarak kullanılacak dil. |
| MergeBehavior | No | Aynı tanımlayıcıya sahip bir üst ilkede bulunan tüm ClaimType ile birlikte birleştirilmiş değerlerin sabit listesi değerleri. Temel ilkede belirtilen bir talebin üzerine yazdığınızda bu özniteliği kullanın. Olası değerler: `Append` , `Prepend` , veya `ReplaceAll` . `Append`Değer, var olan veri koleksiyonunun üst ilkede belirtilen koleksiyonun sonuna eklenmesi gerektiğini belirtir. `Prepend`Değer, var olan veri koleksiyonunun üst ilkede belirtilen koleksiyondan önce eklenmesi gerektiğini belirtir. `ReplaceAll`Değer, üst ilkede tanımlanan veri koleksiyonunun, bunun yerine geçerli ilkede tanımlanan veriler kullanılarak yoksayılacağını belirtir. |

### <a name="supportedlanguages"></a>SupportedLanguages

**Supportedlanguages** öğesi aşağıdaki öğeleri içerir:

| Öğe | Öğeleri | Description |
| ------- | ----------- | ----------- |
| SupportedLanguage | 1: n | Dilleri tanımlamak için RFC 5646-Tags başına bir dil etiketine uyan içeriği görüntüler. |

## <a name="localizedresources"></a>LocalizedResources

**Localizedresources** öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Id | Yes | Yerelleştirilmiş kaynakları benzersiz şekilde tanımlamak için kullanılan bir tanımlayıcı. |

**Localizedresources** öğesi aşağıdaki öğeleri içerir:

| Öğe | Öğeleri | Description |
| ------- | ----------- | ----------- |
| LocalizedCollections | 0: n | Çeşitli kültürlerde tüm koleksiyonları tanımlar. Bir koleksiyon çeşitli kültürler için farklı sayıda öğe ve farklı dize içerebilir. Koleksiyon örnekleri, talep türlerinde görünen numaralandırmaları içerir. Örneğin, bir ülke/bölge listesi kullanıcıya bir açılan listede gösterilir. |
| LocalizedStrings | 0: n | Çeşitli kültürlerde koleksiyonlarda görünen dizeler hariç tüm dizeleri tanımlar. |

### <a name="localizedcollections"></a>LocalizedCollections

**Localizedcollections** öğesi aşağıdaki öğeleri içerir:

| Öğe | Öğeleri | Description |
| ------- | ----------- | ----------- |
| LocalizedCollection | 1: n | Desteklenen dillerin listesi. |

#### <a name="localizedcollection"></a>LocalizedCollection

**Localizedcollection** öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| ElementType | Yes | İlke dosyasında bir ClaimType öğesine veya bir kullanıcı arabirimi öğesine başvurur. |
| ElementID | Yes | **ElementType** bir ClaimType olarak ayarlandıysa kullanılan ClaimsSchema bölümünde zaten tanımlanmış olan bir talep türüne başvuru içeren bir dize. |
| TargetCollection | Yes | Hedef koleksiyon. |

**Localizedcollection** öğesi aşağıdaki öğeleri içerir:

| Öğe | Öğeleri | Description |
| ------- | ----------- | ----------- |
| Öğe | 0: n | Kullanıcının, açılan menüdeki bir değer gibi kullanıcı arabirimindeki bir talep için seçim yapmak üzere kullanılabilir bir seçenek tanımlar. |

**Öğe** öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Metin | Yes | Bu seçenek için Kullanıcı arabiriminde kullanıcıya gösterilmesi gereken kullanıcı dostu görüntüleme dizesi. |
| Değer | Yes | Bu seçeneği belirleyerek ilişkili dize talep değeri. |
| SelectByDefault | No | Bu seçeneğin Kullanıcı arabiriminde varsayılan olarak seçilmesinin gerekip gerekmediğini gösterir. Olası değerler: true veya false. |

Aşağıdaki örnek, **Localizedcollections** öğesinin kullanımını gösterir. Biri Ingilizce ve diğeri Ispanyolca için olmak üzere iki **Localizedcollection** öğesi içerir. Her ikisi de, bir öğenin **kısıtlama** koleksiyonunu `Gender` İngilizce ve İspanyolca için bir öğe listesiyle ayarlayın.

```xml
<LocalizedResources Id="api.selfasserted.en">
 <LocalizedCollections>
   <LocalizedCollection ElementType="ClaimType" ElementId="Gender" TargetCollection="Restriction">
      <Item Text="Female" Value="F" />
      <Item Text="Male" Value="M" />
    </LocalizedCollection>
</LocalizedCollections>

<LocalizedResources Id="api.selfasserted.es">
 <LocalizedCollections>
   <LocalizedCollection ElementType="ClaimType" ElementId="Gender" TargetCollection="Restriction">
      <Item Text="Femenino" Value="F" />
      <Item Text="Masculino" Value="M" />
    </LocalizedCollection>
</LocalizedCollections>
```

### <a name="localizedstrings"></a>LocalizedStrings

**Localizedstrings** öğesi aşağıdaki öğeleri içerir:

| Öğe | Öğeleri | Description |
| ------- | ----------- | ----------- |
| LocalizedString | 1: n | Yerelleştirilmiş bir dize. |

**LocalizedString** öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| ElementType | Yes | Olası değerler: [ClaimsProvider](#claimsprovider), [ClaimType](#claimtype), [ErrorMessage](#errormessage), [getlocalizedstringstransformationclaimtype](#getlocalizedstringstransformationclaimtype), [formatlocalizedstringdönüştürme Tionclaimtype](#formatlocalizedstringtransformationclaimtype), [koşul](#predicate), [ınputvalidation](#inputvalidation)veya [uxelement](#uxelement).   | 
| ElementID | Yes | **ElementType** , veya olarak ayarlandıysa `ClaimType` , `Predicate` `InputValidation` Bu öğe claimsschema bölümünde zaten tanımlanmış olan bir talep türüne başvuru içerir. |
| StringID | Yes | **ElementType** olarak ayarlandıysa `ClaimType` , bu öğe bir talep türü özniteliğine başvuru içerir. Olası değerler: `DisplayName` , `AdminHelpText` , veya `PatternHelpText` . `DisplayName`Değer, talep görünen adını ayarlamak için kullanılır. `AdminHelpText`Değer, talep kullanıcısının yardım metni adını ayarlamak için kullanılır. `PatternHelpText`Değer, talep deseninin yardım metnini ayarlamak için kullanılır. **ElementType** olarak ayarlandıysa `UxElement` , bu öğe bir kullanıcı arabirimi öğesinin özniteliğine bir başvuru içerir. **ElementType** olarak ayarlandıysa `ErrorMessage` , bu öğe bir hata iletisinin tanımlayıcısını belirtir. Tanımlayıcıların tüm listesi için bkz. [Yerelleştirme dize kimlikleri](localization-string-ids.md) `UxElement` .|

## <a name="elementtype"></a>ElementType

Yerelleştirilmesi için bir talep türüne ait ElementType başvurusu, bir talep dönüştürmesi veya bir kullanıcı arabirimi öğesi.

| Yerelleştirilecek öğe | ElementType | ElementID |StringID |
| --------- | -------- | ----------- |----------- |
| Kimlik sağlayıcısı adı |`ClaimsProvider`| | ClaimsExchange öğesinin KIMLIĞI|
| Talep türü öznitelikleri|`ClaimType`|Talep türünün adı| Yerelleştirilecek talebin özniteliği. Olası değerler: `AdminHelpText` , `DisplayName` , `PatternHelpText` ve `UserHelpText` .|
|Hata iletisi|`ErrorMessage`||Hata iletisinin KIMLIĞI |
|Yerelleştirilmiş dizeleri talebe kopyalar|`GetLocalizedStringsTra nsformationClaimType`||Çıkış talebinin adı|
|Koşul Kullanıcı iletisi|`Predicate`|Koşulun adı| Yerelleştirilecek koşulun özniteliği. Olası değerler: `HelpText` .|
|Koşul grubu Kullanıcı iletisi|`InputValidation`|PredicateValidation öğesinin KIMLIĞI.|PredicateGroup öğesinin KIMLIĞI. Koşul grubu, ElementID 'de tanımlandığı şekilde koşul doğrulama öğesinin bir alt öğesi olmalıdır.|
|Kullanıcı arabirimi öğeleri |`UxElement` | | Yerelleştirilecek Kullanıcı arabirimi öğesinin KIMLIĞI.|
|[Görüntüleme denetimi](display-controls.md) |`DisplayControl` |Görüntü denetiminin KIMLIĞI. | Yerelleştirilecek Kullanıcı arabirimi öğesinin KIMLIĞI.|

## <a name="examples"></a>Örnekler

### <a name="claimsprovider"></a>ClaimsProvider

ClaimsProvider değeri, talep sağlayıcılarının görünen adından birini yerelleştirmek için kullanılır. 

```xml
<OrchestrationStep Order="2" Type="ClaimsExchange">
  ...
  <ClaimsExchanges>
    <ClaimsExchange Id="FacebookExchange" TechnicalProfileReferenceId="Facebook-OAUTH" />
    <ClaimsExchange Id="GoogleExchange" TechnicalProfileReferenceId="Google-OAUTH" />
    <ClaimsExchange Id="LinkedInExchange" TechnicalProfileReferenceId="LinkedIn-OAUTH" />
  </ClaimsExchanges>
</OrchestrationStep>

```

Aşağıdaki örnek, talep sağlayıcılarının görünen adının nasıl yerelleştirileceğini gösterir.

```xml
<LocalizedString ElementType="ClaimsProvider" StringId="FacebookExchange">Facebook</LocalizedString>
<LocalizedString ElementType="ClaimsProvider" StringId="GoogleExchange">Google</LocalizedString>
<LocalizedString ElementType="ClaimsProvider" StringId="LinkedInExchange">LinkedIn</LocalizedString>
```

### <a name="claimtype"></a>ClaimType

ClaimType değeri, talep özniteliklerinden birini yerelleştirmek için kullanılır. 

```xml
<ClaimType Id="email">
  <DisplayName>Email Address</DisplayName>
  <DataType>string</DataType>
  <UserHelpText>Email address that can be used to contact you.</UserHelpText>
  <UserInputType>TextBox</UserInputType>
</ClaimType>
```

Aşağıdaki örnek, e-posta talep türünün DisplayName, UserHelpText ve PatternHelpText özniteliklerinin nasıl yerelleştirileceğini gösterir.

```xml
<LocalizedString ElementType="ClaimType" ElementId="email" StringId="DisplayName">Email</LocalizedString>
<LocalizedString ElementType="ClaimType" ElementId="email" StringId="UserHelpText">Please enter your email</LocalizedString>
<LocalizedString ElementType="ClaimType" ElementId="email" StringId="PatternHelpText">Please enter a valid email address</LocalizedString>
```

### <a name="errormessage"></a>Hata

ErrorMessage değeri, sistem hatası iletilerinden birini yerelleştirmek için kullanılır. 

```xml
<TechnicalProfile Id="AAD-UserWriteUsingAlternativeSecurityId">
  <Metadata>
    <Item Key="RaiseErrorIfClaimsPrincipalAlreadyExists">true</Item>
    <Item Key="UserMessageIfClaimsPrincipalAlreadyExists">You are already registered, please press the back button and sign in instead.</Item>
  </Metadata>
  ...
</TechnicalProfile>
```

Aşağıdaki örnek, Usermessageifclaimsprincıpalalreadyexists hata iletisinin nasıl yerelleştirileceğini gösterir.


```xml
<LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfClaimsPrincipalAlreadyExists">The account you are trying to create already exists, please sign-in.</LocalizedString>
```

### <a name="formatlocalizedstringtransformationclaimtype"></a>Formatlocalizedstringdönüştürme Tionclaimtype

Formatlocalizedstringdönüştürme Tionclaimtype değeri, talepleri yerelleştirilmiş bir dizeye biçimlendirmek için kullanılır. Daha fazla bilgi için bkz. [Formatlocalizedstring taleplerini dönüştürme](string-transformations.md#formatlocalizedstring)


```xml
<ClaimsTransformation Id="SetResponseMessageForEmailAlreadyExists" TransformationMethod="FormatLocalizedString">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="email" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="stringFormatId" DataType="string" Value="ResponseMessge_EmailExists" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="responseMsg" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

Aşağıdaki örnek, Formatlocalizedstringdönüşümtionclaimtype talep dönüştürmesinin dize biçiminin nasıl yerelleştirileceğini gösterir.

```xml
<LocalizedString ElementType="FormatLocalizedStringTransformationClaimType" StringId="ResponseMessge_EmailExists">The email '{0}' is already an account in this organization. Click Next to sign in with that account.</LocalizedString>
```

### <a name="getlocalizedstringstransformationclaimtype"></a>GetLocalizedStringsTransformationClaimType

GetLocalizedStringsTransformationClaimType değeri, yerelleştirilmiş dizeleri taleplere kopyalamak için kullanılır. Daha fazla bilgi için bkz. [Getlocalizedstringstrans, talepler dönüştürmesi](string-transformations.md#getlocalizedstringstransformation)


```xml
<ClaimsTransformation Id="GetLocalizedStringsForEmail" TransformationMethod="GetLocalizedStringsTransformation">
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="subject" TransformationClaimType="email_subject" />
    <OutputClaim ClaimTypeReferenceId="message" TransformationClaimType="email_message" />
    <OutputClaim ClaimTypeReferenceId="codeIntro" TransformationClaimType="email_code" />
    <OutputClaim ClaimTypeReferenceId="signature" TransformationClaimType="email_signature" />
   </OutputClaims>
</ClaimsTransformation>
```

Aşağıdaki örnek, Getlocalizedstringstransiceclaim dönüşümünün çıkış taleplerinin nasıl yerelleştirileceğini gösterir.

```xml
<LocalizedString ElementType="GetLocalizedStringsTransformationClaimType" StringId="email_subject">Contoso account email verification code</LocalizedString>
<LocalizedString ElementType="GetLocalizedStringsTransformationClaimType" StringId="email_message">Thanks for verifying your account!</LocalizedString>
<LocalizedString ElementType="GetLocalizedStringsTransformationClaimType" StringId="email_code">Your code is</LocalizedString>
<LocalizedString ElementType="GetLocalizedStringsTransformationClaimType" StringId="email_signature">Sincerely</LocalizedString>
```

### <a name="predicate"></a>Koşulunda

Koşul değeri, [koşul](predicates.md) hatası iletilerinden birini yerelleştirmek için kullanılır. 

```xml
<Predicates>
  <Predicate Id="LengthRange" Method="IsLengthRange"  HelpText="The password must be between 6 and 64 characters.">
    <Parameters>
      <Parameter Id="Minimum">6</Parameter>
      <Parameter Id="Maximum">64</Parameter>
    </Parameters>
  </Predicate>
  <Predicate Id="Lowercase" Method="IncludesCharacters" HelpText="a lowercase letter">
    <Parameters>
      <Parameter Id="CharacterSet">a-z</Parameter>
    </Parameters>
  </Predicate>
  <Predicate Id="Uppercase" Method="IncludesCharacters" HelpText="an uppercase letter">
    <Parameters>
      <Parameter Id="CharacterSet">A-Z</Parameter>
    </Parameters>
  </Predicate>
</Predicates>
```

Aşağıdaki örnek, koşulların yardım metninin nasıl yerelleştirileceğini gösterir.

```xml
<LocalizedString ElementType="Predicate" ElementId="LengthRange" StringId="HelpText">The password must be between 6 and 64 characters.</LocalizedString>
<LocalizedString ElementType="Predicate" ElementId="Lowercase" StringId="HelpText">a lowercase letter</LocalizedString>
<LocalizedString ElementType="Predicate" ElementId="Uppercase" StringId="HelpText">an uppercase letter</LocalizedString>
```

### <a name="inputvalidation"></a>Inputvalidation

Inputvalidation değeri, [Predicatevalidation](predicates.md) grup hata iletilerinden birini yerelleştirmek için kullanılır. 

```xml
<PredicateValidations>
  <PredicateValidation Id="CustomPassword">
    <PredicateGroups>
      <PredicateGroup Id="LengthGroup">
        <PredicateReferences MatchAtLeast="1">
          <PredicateReference Id="LengthRange" />
        </PredicateReferences>
      </PredicateGroup>
      <PredicateGroup Id="CharacterClasses">
        <UserHelpText>The password must have at least 3 of the following:</UserHelpText>
        <PredicateReferences MatchAtLeast="3">
          <PredicateReference Id="Lowercase" />
          <PredicateReference Id="Uppercase" />
          <PredicateReference Id="Number" />
          <PredicateReference Id="Symbol" />
        </PredicateReferences>
      </PredicateGroup>
    </PredicateGroups>
  </PredicateValidation>
</PredicateValidations>
```

Aşağıdaki örnek, bir koşul doğrulama grubu yardım metninin yerelleştirilmesi gösterilmektedir.

```xml
<LocalizedString ElementType="InputValidation" ElementId="CustomPassword" StringId="CharacterClasses">The password must have at least 3 of the following:</LocalizedString>
```

### <a name="uxelement"></a>UxElement

UxElement değeri, Kullanıcı arabirimi öğelerinden birini yerelleştirmek için kullanılır. Aşağıdaki örnek, Continue ve Cancel düğmelerinin nasıl yerelleştirileceğini gösterir.

```xml
<LocalizedString ElementType="UxElement" StringId="button_continue">Create new account</LocalizedString>
<LocalizedString ElementType="UxElement" StringId="button_cancel">Cancel</LocalizedString>
```

### <a name="displaycontrol"></a>DisplayControl

DisplayControl değeri, [görüntüleme denetimi](display-controls.md) Kullanıcı arabirimi öğelerinden birini yerelleştirmek için kullanılır. Etkinleştirildiğinde, görüntüleme denetimi localizedStrings, **ver_but_send**, **ver_but_edit**, **Ver_but_resend** ve **ver_but_verify** gibi _ *uxelement** strıngııd 'lerden bazıları üzerinde ***önceliği** _ alır. Aşağıdaki örnek, gönder ve Doğrula düğmelerinin nasıl yerelleştirileceğini gösterir. 

```xml
<LocalizedString ElementType="DisplayControl" ElementId="emailVerificationControl" StringId="but_send_code">Send verification code</LocalizedString>
<LocalizedString ElementType="DisplayControl" ElementId="emailVerificationControl" StringId="but_verify_code">Verify code</LocalizedString>
```

Otomatik olarak onaylanan bir teknik profilin meta veri bölümünde, başvurulan ContentDefinition 'ın DataUri 'nin [sayfa düzeni sürüm](page-layout.md) 2.1.0 veya üzeri olarak ayarlanmış olması gerekir. Örnek:

```xml
<ContentDefinition Id="api.selfasserted">
  <DataUri>urn:com:microsoft:aad:b2c:elements:selfasserted:2.1.0</DataUri>
  ...
```

## <a name="next-steps"></a>Sonraki adımlar

Yerelleştirme örnekleri için aşağıdaki makalelere bakın:

- [Azure Active Directory B2C özel ilkeyle dil özelleştirmesi](language-customization.md)
- [Azure Active Directory B2C Kullanıcı akışları ile dil özelleştirmesi](language-customization.md)
