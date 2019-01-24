---
title: Kimlik deneyimi çerçevesi şema Azure Active Directory B2C için JSON talep dönüştürme örnekler | Microsoft Docs
description: JSON dönüşüm örnekleri kimlik deneyimi çerçevesi şema, Azure Active Directory B2C için talep.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: e7096773f2aaa39abd965b4697f45a3b3f80f136
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54850850"
---
# <a name="json-claims-transformations"></a>JSON dönüştürmeleri talep

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makalede, Azure Active Directory (Azure AD) B2C'de kimlik deneyimi çerçevesi şema JSON talep dönüştürmeleri kullanma örnekleri sağlar. Daha fazla bilgi için [ClaimsTransformations](claimstransformations.md).

## <a name="getclaimfromjson"></a>GetClaimFromJson

Belirtilen öğeyi bir JSON veri alın.

| Öğe | TransformationClaimType | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Inputclaim | inputJson | dize | Talep dönüştürme tarafından öğesini almak için kullanılan ClaimTypes. |
| InputParameter | claimToExtract | dize | Ayıklanacak JSON öğesi adı. |
| outputClaim | extractedClaim | dize | Bu dönüşüm talep sonra üreten ClaimType çağırıldı, öğe değeri belirtilen _claimToExtract_ giriş parametresi. |

Aşağıdaki örnekte, talep dönüştürme ayıklanan `emailAddress` JSON verilerini öğesinden: `{"emailAddress": "someone@example.com", "displayName": "Someone"}`

```XML
<ClaimsTransformation Id="GetEmailClaimFromJson" TransformationMethod="GetClaimFromJson">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="customUserData" TransformationClaimType="inputJson" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="claimToExtract" DataType="string" Value="emailAddress" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="extractedEmail" TransformationClaimType="extractedClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Örnek

- Giriş talepleri:
    - **inputJson**: {"emailAddress": "someone@example.com", "displayName": "Kişi"}
- Giriş parametresi:
    - **claimToExtract**: emailAddress
- Çıkış talep: 
    - **extractedClaim**: someone@example.com


## <a name="getclaimsfromjsonarray"></a>GetClaimsFromJsonArray

Belirtilen öğelerin listesini Json verilerinizden yararlanın.

| Öğe | TransformationClaimType | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Inputclaim | jsonSourceClaim | dize | Talep dönüştürme tarafından olan talepleri almak için kullanılan ClaimTypes. |
| InputParameter | errorOnMissingClaims | boole | Taleplerinden biri eksikse bir hata harekete geçirileceğini belirtir. |
| InputParameter | includeEmptyClaims | dize | Boş talep eklenip eklenmeyeceğini belirtin. |
| InputParameter | jsonSourceKeyName | dize | Öğe anahtarı adı |
| InputParameter | jsonSourceValueName | dize | Öğe değer adı |
| outputClaim | Koleksiyon | dize, int, boolean ve datetime |Ayıklanacak talep listesi. Talep adı belirtilen birine eşit olmalıdır _jsonSourceClaim_ giriş talep. |

Aşağıdaki örnekte, talep dönüştürme aşağıdaki talep ayıklar: e-posta (dize), displayName (dize), membershipNum (int), etkin (boolean) ve doğum tarihi (TarihSaat) JSON verilerinden.

```JSON
[{"key":"email","value":"someone@example.com"}, {"key":"displayName","value":"Someone"}, {"key":"membershipNum","value":6353399}, {"key":"active","value":true}, {"key":"birthdate","value":"1980-09-23T00:00:00Z"}]
```

```XML
<ClaimsTransformation Id="GetClaimsFromJson" TransformationMethod="GetClaimsFromJsonArray">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="jsonSourceClaim" TransformationClaimType="jsonSource" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="errorOnMissingClaims" DataType="boolean" Value="false" />
    <InputParameter Id="includeEmptyClaims" DataType="boolean" Value="false" />
    <InputParameter Id="jsonSourceKeyName" DataType="string" Value="key" />
    <InputParameter Id="jsonSourceValueName" DataType="string" Value="value" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="email" />
    <OutputClaim ClaimTypeReferenceId="displayName" />
    <OutputClaim ClaimTypeReferenceId="membershipNum" />
    <OutputClaim ClaimTypeReferenceId="active" />
    <OutputClaim ClaimTypeReferenceId="birthdate" />
  </OutputClaims>
</ClaimsTransformation>
```    

- Giriş talepleri:
    - **jsonSourceClaim**: [{"key": "email", "value": "someone@example.com"}, {"anahtarını": "displayName", "value": "Kişi"}, {"anahtarını": "membershipNum", "value": 6353399}, {"anahtarını": "etkin", "value": true}, {"anahtarını": "Doğum", "value": "1980-09-23T00:0 0:00Z"}]
- Giriş parametreleri:
    - **errorOnMissingClaims**: false
    - **includeEmptyClaims**: false
    - **jsonSourceKeyName**: anahtar
    - **jsonSourceValueName**: değer
- Çıkış talep:
    - **e-posta**: "someone@example.com"
    - **displayName**: "Kişi"
    - **membershipNum**: 6353399
    - **Etkin**: true
    - **Doğum tarihi**: 1980-09-23T00:00:00Z

## <a name="getnumericclaimfromjson"></a>GetNumericClaimFromJson

Belirtilen bir sayısal (uzun) öğesi bir JSON verilerini alır.

| Öğe | TransformationClaimType | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Inputclaim | inputJson | dize | Talep dönüştürme tarafından talep almak için kullanılan ClaimTypes. |
| InputParameter | claimToExtract | dize | Ayıklanacak JSON öğesi adı. |
| outputClaim | extractedClaim | uzun | Öğesinin değeri belirtilen bu ClaimsTransformation çağrıldıktan sonra üreten ClaimType _claimToExtract_ giriş parametreleri. |

Aşağıdaki örnekte, talep dönüştürme ayıklar `id` JSON verilerini öğesinden.

```JSON
{
    "emailAddress": "someone@example.com", 
    "displayName": "Someone", 
    "id" : 6353399
}
```

```XML
<ClaimsTransformation Id="GetIdFromResponse" TransformationMethod="GetNumericClaimFromJson">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="exampleInputClaim" TransformationClaimType="inputJson" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="claimToExtract" DataType="string" Value="id" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="membershipId" TransformationClaimType="extractedClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Örnek

- Giriş talepleri:
    - **inputJson**: {"emailAddress": "someone@example.com", "displayName": "Kişi", "id": 6353399}
- Giriş parametreleri
    - **claimToExtract**: kimliği
- Çıkış talep: 
    - **extractedClaim**: 6353399

## <a name="getsinglevaluefromjsonarray"></a>GetSingleValueFromJsonArray

Bir JSON veri diziden ilk öğeyi alır.

| Öğe | TransformationClaimType | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Inputclaim | inputJsonClaim | dize | Talep dönüştürme tarafından JSON diziden öğesini almak için kullanılan ClaimTypes. |
| outputClaim | extractedClaim | dize | Bu ClaimsTransformation çağrıldıktan sonra üreten ClaimType, JSON dizideki ilk öğe. |

Aşağıdaki örnekte, talep dönüştürme JSON diziden ilk öğeyi (e-posta adresi) ayıklar `["someone@example.com", "Someone", 6353399]`.

```XML
<ClaimsTransformation Id="GetEmailFromJson" TransformationMethod="GetSingleValueFromJsonArray">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="userData" TransformationClaimType="inputJsonClaim" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="email" TransformationClaimType="extractedClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Örnek

- Giriş talepleri:
    - **inputJsonClaim**: ["someone@example.com", "Kişi", 6353399]
- Çıkış talep: 
    - **extractedClaim**: someone@example.com

## <a name="xmlstringtojsonstring"></a>XmlStringToJsonString

XML verilerini JSON biçimine dönüştürür.

| Öğe | TransformationClaimType | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Inputclaim | xml | dize | Talep dönüştürme tarafından verileri XML'den JSON biçimine dönüştürmek için kullanılan ClaimTypes. |
| outputClaim | json | dize | Bu ClaimsTransformation çağrıldıktan sonra üreten ClaimType, verileri JSON biçiminde. |

```XML
<ClaimsTransformation Id="ConvertXmlToJson" TransformationMethod="XmlStringToJsonString">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="intpuXML" TransformationClaimType="xml" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="outputJson" TransformationClaimType="json" />
  </OutputClaims>
</ClaimsTransformation>
```

Aşağıdaki örnekte, talep dönüştürme, aşağıdaki XML verilerini JSON biçimine dönüştürür.

#### <a name="example"></a>Örnek
Giriş talep:

```XML
<user>
  <name>Someone</name>
  <email>someone@example.com</email>
</user>
```

Çıkış talep:

```JSON
{
  "user": {
    "name":"Someone",
    "email":"someone@example.com"
  }
}
```

