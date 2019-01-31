---
title: Translator metin çevirisi API'si alfabeye yöntemi
titlesuffix: Azure Cognitive Services
description: Translator metin API'si alfabeye yöntemi kullanın.
services: cognitive-services
author: Jann-Skotdal
manager: cgronlun
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: reference
ms.date: 03/29/2018
ms.author: v-jansko
ms.openlocfilehash: a37b069d533dc51067aba4f9d9bd87608b116ae6
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55466464"
---
# <a name="translator-text-api-30-transliterate"></a>Translator metin çevirisi API'si 3.0: Transliterate

Bir dilde başka bir komut dosyası için bir komut dosyasından dönüştürür.

## <a name="request-url"></a>İstek URL'si

Gönderme bir `POST` isteği:

```HTTP
https://api.cognitive.microsofttranslator.com/transliterate?api-version=3.0
```

## <a name="request-parameters"></a>İstek parametreleri

Sorgu dizesinde geçirilen istek Parametreler şunlardır:

<table width="100%">
  <th width="20%">Sorgu parametresi</th>
  <th>Açıklama</th>
  <tr>
    <td>API sürümü</td>
    <td>*Gerekli parametre*.<br/>İstemci tarafından istenen API sürümü. Değer olmalıdır `3.0`.</td>
  </tr>
  <tr>
    <td>language</td>
    <td>*Gerekli parametre*.<br/>Bir komut dosyasından diğerine dönüştürmek için metin dilini belirtir. Olası diller listelenmiştir `transliteration` kapsam alınan hizmet için sorgulama yaparak kendi [desteklenen diller](./v3-0-languages.md).</td>
  </tr>
  <tr>
    <td>fromScript</td>
    <td>*Gerekli parametre*.<br/>Giriş metni kullanılan betiği belirtir. Arama [desteklenen diller](./v3-0-languages.md) kullanarak `transliteration` seçili dil için giriş komut dosyalarını bulmak için kapsam.</td>
  </tr>
  <tr>
    <td>toScript</td>
    <td>*Gerekli parametre*.<br/>Çıkış betiğini belirtir. Arama [desteklenen diller](./v3-0-languages.md) kullanarak `transliteration` çıkış kodları seçilen giriş dili bileşimi için kullanılabilir ve giriş betik bulmak için kapsam.</td>
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

İstek gövdesinde bir JSON dizisidir. Her dizi öğesi bir dize özelliği adlı bir JSON nesnesidir `Text`, dönüştürülecek dize temsil eder.

```json
[
    {"Text":"こんにちは"},
    {"Text":"さようなら"}
]
```

Aşağıdaki sınırlamalar geçerlidir:

* Dizi en fazla 10 öğe olabilir.
* Bir dizi öğesinin metin değeri boşluk da dahil olmak üzere 1.000 karakterden uzun olamaz.
* İstekte bulunan tüm metin alanları dahil olmak üzere 5000 karakterden uzun olamaz.

## <a name="response-body"></a>Yanıt gövdesi

Başarılı yanıt, giriş dizideki her öğe için bir sonuç içeren bir JSON dizisidir. Sonuç nesnesi, aşağıdaki özellikleri içerir:

  * `text`: Giriş dizesi çıkış komut dosyasına dönüştürme sonucu olan bir dize.
  
  * `script`: Komut çıktısında kullanılan belirten bir dize.

JSON yanıtı örneği verilmiştir:

```json
[
    {"text":"konnnichiha","script":"Latn"},
    {"text":"sayounara","script":"Latn"}
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

Aşağıdaki örnek, iki Japonca dizenin Japonca Romanized dönüştürme gösterilmektedir.

# <a name="curltabcurl"></a>[Curl](#tab/curl)

Bu örnekte istek için JSON yükü:

```
[{"text":"こんにちは","script":"jpan"},{"text":"さようなら","script":"jpan"}]
```

Unicode karakter desteği olmayan bir komut satırı penceresinde cURL kullanıyorsanız aşağıdaki JSON yükü Al ve adlı bir dosyaya kaydedin `request.txt`. Dosyayı kaydettiğinizden emin olun `UTF-8` kodlama.

```
curl -X POST "https://api.cognitive.microsofttranslator.com/transliterate?api-version=3.0&language=ja&fromScript=Jpan&toScript=Latn" -H "X-ClientTraceId: 875030C7-5380-40B8-8A03-63DACCF69C11" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d @request.txt
```

---
