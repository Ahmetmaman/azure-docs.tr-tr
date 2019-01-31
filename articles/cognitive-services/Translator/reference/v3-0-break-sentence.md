---
title: Translator metin çevirisi API'si BreakSentence yöntemi
titlesuffix: Azure Cognitive Services
description: Translator metin API'si BreakSentence yöntemi kullanın.
services: cognitive-services
author: Jann-Skotdal
manager: cgronlun
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: reference
ms.date: 03/29/2018
ms.author: v-jansko
ms.openlocfilehash: 2a97c55c7caa7b0b2c4aa10b01abd2714b8ace7a
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55458542"
---
# <a name="translator-text-api-30-breaksentence"></a>Translator metin çevirisi API'si 3.0: BreakSentence

Bir metin parçası cümle sınırlarındaki konumunu tanımlar.

## <a name="request-url"></a>İstek URL'si

Gönderme bir `POST` isteği:

```HTTP
https://api.cognitive.microsofttranslator.com/breaksentence?api-version=3.0
```

## <a name="request-parameters"></a>İstek parametreleri

Sorgu dizesinde geçirilen istek Parametreler şunlardır:

<table width="100%">
  <th width="20%">Sorgu parametresi</th>
  <th>Açıklama</th>
  <tr>
    <td>API sürümü</td>
    <td>*Gerekli sorgu parametresi*.<br/>İstemci tarafından istenen API sürümü. Değer olmalıdır `3.0`.</td>
  </tr>
  <tr>
    <td>language</td>
    <td>*İsteğe bağlı bir sorgu parametresi*.<br/>Dil etiketi giriş metni dili tanımlama. Bir kod belirtilmezse otomatik dil algılama uygulanır.</td>
  </tr>
  <tr>
    <td>komut dosyası</td>
    <td>*İsteğe bağlı bir sorgu parametresi*.<br/>Giriş metni kullanılan komut dosyası tanımlayan komut dosyası etiketi. Bir komut dosyası belirtilmezse, varsayılan komut dosyası dili kabul edilir.</td>
  </tr>
</table> 

İstek üst bilgileri ekleyin:

<table width="100%">
  <th width="20%">Üst bilgiler</th>
  <th>Açıklama</th>
  <tr>
    <td>_Bir yetkilendirme_<br/>_Üst bilgi_</td>
    <td>*Gerekli istek üst bilgisi*.<br/>Bkz: [kimlik doğrulaması için kullanılabilir seçenekler](./v3-0-reference.md#authentication).</td>
  </tr>
  <tr>
    <td>Content-Type</td>
    <td>*Gerekli istek üst bilgisi*.<br/>Akıştaki içerik türünü belirtir. Olası değerler şunlardır: `application/json`.</td>
  </tr>
  <tr>
    <td>İçerik uzunluğu</td>
    <td>*Gerekli istek üst bilgisi*.<br/>İstek gövdesi uzunluğu.</td>
  </tr>
  <tr>
    <td>X-ClientTraceId</td>
    <td>*İsteğe bağlı*.<br/>İstek benzersiz olarak tanımlanabilmesi için bir istemci tarafından oluşturulan GUID. İzleme kimliği adlı bir sorgu parametresi kullanarak sorgu dizesinde eklerseniz bu başlığı atlayabilirsiniz Not `ClientTraceId`.</td>
  </tr>
</table> 

## <a name="request-body"></a>İstek gövdesi

İstek gövdesinde bir JSON dizisidir. Her dizi öğesi bir dize özelliği adlı bir JSON nesnesidir `Text`. Tümce sınırları değeri için hesaplanan `Text` özelliği. Bir örnek istek gövdesini bir metin parçası olan aşağıdaki gibi görünür:

```json
[
    { "Text": "How are you? I am fine. What did you do today?" }
]
```

Aşağıdaki sınırlamalar geçerlidir:

* Dizi en fazla 100 öğeleri olabilir.
* Bir dizi öğesinin metin değeri boşluk da dahil olmak üzere 10.000 karakterden uzun olamaz.
* İstekte bulunan tüm metin alanları dahil olmak üzere 50. 000'karakterden uzun olamaz.
* Varsa `language` sorgu parametresi belirtilmediyse, ardından tüm dizi öğeleri aynı dilde olması gerekir. Aksi takdirde otomatik dil algılama, her bir dizi öğesine bağımsız olarak uygulanır.

## <a name="response-body"></a>Yanıt gövdesi

Başarılı bir yanıt için Giriş dizisinin her bir dizede tek bir sonuç ile bir JSON dizisidir. Sonuç nesnesi, aşağıdaki özellikleri içerir:

  * `sentLen`: Metin öğesinin cümleleri uzunluklarının temsil eden tamsayı dizisi. Dizinin uzunluğunu cümleler sayısıdır ve her cümle uzunluğunu değerlerdir. 

  * `detectedLanguage`: Aşağıdaki özelliklerle algılanan dilin açıklayan bir nesne:

     * `language`: Algılanan dil kodu.

     * `score`: Sonuçta güvenle gösteren bir float değeri. Sıfır ile bir puan arasındadır ve düşük puanı düşük güven gösterir.
     
    Unutmayın `detectedLanguage` özelliği varsa yalnızca sonuç nesnesinde otomatik dil algılama istendiğinde.

JSON yanıtı örneği verilmiştir:

```json
[
  {
    "sentenceLengths": [ 13, 11, 22 ]
    "detectedLanguage": {
      "language": "en",
      "score": 401
    },
  }
]
```

## <a name="response-headers"></a>Yanıt üst bilgileri

<table width="100%">
  <th width="20%">Üst bilgiler</th>
  <th>Açıklama</th>
  <tr>
    <td>X-RequestId</td>
    <td>İstek tanımlamak için hizmeti tarafından oluşturulan değeri. Sorun giderme amacıyla kullanılır.</td>
  </tr>
</table> 

## <a name="response-status-codes"></a>Yanıt durum kodları

Bir isteği döndüren olası HTTP durum kodları şunlardır: 

<table width="100%">
  <th width="20%">Durum Kodu</th>
  <th>Açıklama</th>
  <tr>
    <td>200</td>
    <td>Başarılı.</td>
  </tr>
  <tr>
    <td>400</td>
    <td>Sorgu parametrelerden biri eksik veya geçerli değil. İstek parametreleri yeniden denemeden önce düzeltin.</td>
  </tr>
  <tr>
    <td>401</td>
    <td>İsteğin kimliği doğrulanamadı. Kimlik bilgilerinin belirtilen ve geçerli olup olmadığını denetleyin.</td>
  </tr>
  <tr>
    <td>403</td>
    <td>İstek yetkili değil. Ayrıntıları hata iletisine bakın. Bu, genellikle bir deneme aboneliği ile sağlanan tüm ücretsiz çevirileri kullanılmış olan olduğunu gösterir.</td>
  </tr>
  <tr>
    <td>429</td>
    <td>Çağıran, çok fazla istek gönderiyor.</td>
  </tr>
  <tr>
    <td>500</td>
    <td>Beklenmeyen bir hata oluştu. Sorun devam ederse, raporu ile: tarih ve saat hatanın yanıt üst bilgisinden istek tanımlayıcısı `X-RequestId`ve istemci tanımlayıcısı istek üst bilgisinden `X-ClientTraceId`.</td>
  </tr>
  <tr>
    <td>503</td>
    <td>Sunucu geçici olarak kullanılamıyor. İsteği yeniden deneyin. Sorun devam ederse, raporu ile: tarih ve saat hatanın yanıt üst bilgisinden istek tanımlayıcısı `X-RequestId`ve istemci tanımlayıcısı istek üst bilgisinden `X-ClientTraceId`.</td>
  </tr>
</table> 

## <a name="examples"></a>Örnekler

Aşağıdaki örnek, tek bir cümle için cümle sınırları edinme gösterir. Cümlenin dil hizmeti tarafından otomatik olarak algılanır.

# <a name="curltabcurl"></a>[Curl](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/breaksentence?api-version=3.0" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'How are you? I am fine. What did you do today?'}]"
```

---

