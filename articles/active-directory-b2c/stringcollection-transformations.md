---
title: Özel ilkeler için StringCollection talep dönüştürme örnekleri
titleSuffix: Azure AD B2C
description: StringCollection, Azure Active Directory B2C Identity Experience Framework (ıEF) şeması için dönüşüm örnekleri talep ediyor.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 03/04/2021
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: c9104fb4598eb62ed96d0b21734053fa118b5237
ms.sourcegitcommit: dac05f662ac353c1c7c5294399fca2a99b4f89c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102120291"
---
# <a name="stringcollection-claims-transformations"></a>StringCollection talep dönüştürmeleri

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makalede, Azure Active Directory B2C (Azure AD B2C) öğesinde kimlik deneyimi çerçevesi şemasının dize koleksiyonu talep dönüştürmelerinin kullanılmasına yönelik örnekler sağlanmaktadır. Daha fazla bilgi için bkz. [Claimstransformations](claimstransformations.md).

## <a name="additemtostringcollection"></a>AddItemToStringCollection

Yeni bir benzersiz değerler stringCollection talebine bir dize talebi ekler.

| Öğe | Dönüştürme Tionclaimtype | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Inputclaim | öğe | string | Çıkış talebine eklenecek ClaimType. |
| Inputclaim |  koleksiyonu | stringCollection | Çıkış talebine eklenecek olan dize koleksiyonu. Koleksiyon öğeler içeriyorsa, talep dönüştürmesi öğeleri kopyalar ve öğeyi çıkış koleksiyonu talebinin sonuna ekler. |
| OutputClaim |  koleksiyonu | stringCollection | Bu talep dönüştürmesinin ardından üretilen ClaimType, giriş talebinde belirtilen değerle çağırılır. |

Yeni veya mevcut bir stringCollection 'a bir dize eklemek için bu talep dönüşümünü kullanın. Bu, yaygın olarak bir **AAD-Userwriteusingalternativesecurityıd** teknik profilinde kullanılır. Yeni bir sosyal hesap oluşturulmadan önce, **Createothermailsfromemail** talep dönüştürmesi ClaimType 'ı okur ve değeri **Diğer postalar** ClaimType 'a ekler.

Aşağıdaki talep dönüştürmesi, **e-posta** ClaimType 'ı **Diğer postalar** ClaimType 'a ekler.

```xml
<ClaimsTransformation Id="CreateOtherMailsFromEmail" TransformationMethod="AddItemToStringCollection">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="email" TransformationClaimType="item" />
    <InputClaim ClaimTypeReferenceId="otherMails" TransformationClaimType="collection" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="otherMails" TransformationClaimType="collection" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Örnek

- Giriş talepleri:
  - **koleksiyon**: [" someone@outlook.com "]
  - **öğe**: " admin@contoso.com "
- Çıkış talepleri:
  - **koleksiyon**: [" someone@outlook.com ", " admin@contoso.com "]

## <a name="addparametertostringcollection"></a>AddParameterToStringCollection

Yeni bir benzersiz değerler stringCollection talebine bir String parametresi ekler.

| Öğe | Dönüştürme Tionclaimtype | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Inputclaim |  koleksiyonu | stringCollection | Çıkış talebine eklenecek olan dize koleksiyonu. Koleksiyon öğeler içeriyorsa, talep dönüştürmesi öğeleri kopyalar ve öğeyi çıkış koleksiyonu talebinin sonuna ekler. |
| InputParameter | öğe | string | Çıkış talebine eklenecek değer. |
| OutputClaim |  koleksiyonu | stringCollection | Bu talep dönüştürmesinin ardından üretilen ClaimType, giriş parametresinde belirtilen değerle çağırılır. |

Yeni veya mevcut bir stringCollection 'a bir dize değeri eklemek için bu talep dönüşümünü kullanın. Aşağıdaki örnek, admin@contoso.com **Diğer postalar** talebine bir sabit e-posta adresi () ekler.

```xml
<ClaimsTransformation Id="SetCompanyEmail" TransformationMethod="AddParameterToStringCollection">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="otherMails" TransformationClaimType="collection" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="item" DataType="string" Value="admin@contoso.com" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="otherMails" TransformationClaimType="collection" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Örnek

- Giriş talepleri:
  - **koleksiyon**: [" someone@outlook.com "]
- Giriş parametreleri
  - **öğe**: " admin@contoso.com "
- Çıkış talepleri:
  - **koleksiyon**: [" someone@outlook.com ", " admin@contoso.com "]

## <a name="getsingleitemfromstringcollection"></a>Getsingleıtemfromstringcollection

Belirtilen dize koleksiyonundan ilk öğeyi alır.

| Öğe | Dönüştürme Tionclaimtype | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Inputclaim |  koleksiyonu | stringCollection | Öğeyi almak için talep dönüştürmesi tarafından kullanılan ClaimTypes. |
| OutputClaim | Extracteditıtem | string | Bu Claimstranssetting sonrasında üretilen ClaimTypes çağrılır. Koleksiyondaki ilk öğe. |

Aşağıdaki örnek, **Diğer postalar** talebini okur ve ilk öğeyi **e-posta** talebine döndürür.

```xml
<ClaimsTransformation Id="CreateEmailFromOtherMails" TransformationMethod="GetSingleItemFromStringCollection">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="otherMails" TransformationClaimType="collection" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="email" TransformationClaimType="extractedItem" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Örnek

- Giriş talepleri:
  - **koleksiyon**: [" someone@outlook.com ", " someone@contoso.com "]
- Çıkış talepleri:
  - **Extracteditıtem**: " someone@outlook.com "


## <a name="stringcollectioncontains"></a>StringCollectionContains

Bir StringCollection talep türünün bir öğesi içerip içermediğini denetler.

| Öğe | Dönüştürme Tionclaimtype | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Inputclaim | ınputclaim | stringCollection | Aranacak talep türü. |
|InputParameter|öğe|string|Aranacak değer.|
|InputParameter|ignoreCase|string|Bu karşılaştırmanın karşılaştırılan dizelerin durumunu yoksayıp saymayacağını belirtir.|
| OutputClaim | outputClaim | boolean | Bu Claimstransbir şekilde üretilen ClaimType çağırılır. Koleksiyon böyle bir dize içeriyorsa Boolean göstergesi |

Aşağıdaki örnek, `roles` StringCollection talep türünün **yönetici** değerini içerip içermediğini denetler.

```xml
<ClaimsTransformation Id="IsAdmin" TransformationMethod="StringCollectionContains">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="roles" TransformationClaimType="inputClaim"/>
  </InputClaims>
  <InputParameters>
    <InputParameter  Id="item" DataType="string" Value="Admin"/>
    <InputParameter  Id="ignoreCase" DataType="string" Value="true"/>
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="isAdmin" TransformationClaimType="outputClaim"/>
  </OutputClaims>
</ClaimsTransformation>
```

- Giriş talepleri:
    - **ınputclaim**: ["okuyucu", "yazar", "Yönetici"]
- Giriş parametreleri:
    - **öğe**: "Yönetici"
    - **IgnoreCase**: "true"
- Çıkış talepleri:
    - **Outputclaim**: "true"

## <a name="stringcollectioncontainsclaim"></a>Stringcollectioncontainsclaım

Bir StringCollection talep türünün bir talep değeri içerip içermediğini denetler.

| Öğe | Dönüştürme Tionclaimtype | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Inputclaim |  koleksiyonu | stringCollection | Aranacak talep türü. |
| Inputclaim | öğe|string| Aranacak değeri içeren talep türü.|
|InputParameter|ignoreCase|string|Bu karşılaştırmanın karşılaştırılan dizelerin durumunu yoksayıp saymayacağını belirtir.|
| OutputClaim | outputClaim | boolean | Bu Claimstransbir şekilde üretilen ClaimType çağırılır. Koleksiyon böyle bir dize içeriyorsa Boolean göstergesi |

Aşağıdaki örnek, `roles` StringCollection talep türünün talep türü değerini içerip içermediğini denetler `role` .

```xml
<ClaimsTransformation Id="HasRequiredRole" TransformationMethod="StringCollectionContainsClaim">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="roles" TransformationClaimType="collection" />
    <InputClaim ClaimTypeReferenceId="role" TransformationClaimType="item" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="ignoreCase" DataType="string" Value="true" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="hasAccess" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation> 
```

- Giriş talepleri:
    - **koleksiyon**: ["okuyucu", "yazar", "Yönetici"]
    - **öğe**: "Yönetici"
- Giriş parametreleri:
    - **IgnoreCase**: "true"
- Çıkış talepleri:
    - **Outputclaim**: "true"
