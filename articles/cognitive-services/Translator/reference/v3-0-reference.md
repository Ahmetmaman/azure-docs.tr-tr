---
title: Translator metin çevirisi API'si V3.0 başvurusu
titlesuffix: Azure Cognitive Services
description: Translator metin API'si V3.0 için başvuru belgeleri.
services: cognitive-services
author: Jann-Skotdal
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: reference
ms.date: 03/29/2018
ms.author: v-jansko
ms.openlocfilehash: 5c952370908919deb6531e0b175063dc2657ae98
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52870412"
---
# <a name="translator-text-api-v30"></a>Translator metin çevirisi API'si v3.0

## <a name="whats-new"></a>Yenilikler

Translator metin çevirisi API'si 3 sürümünü modern bir JSON tabanlı Web API'si sağlar. Kullanılabilirliğini artırır ve performansı daha az işlemleri ve var olan özellikler birleştirerek yeni özellikler sağlar.

 * Bir dilde bir komut dosyasından başka bir komut dosyasına dönüştürmek için alfabeye.
 * Bir istekte birden çok dil için çeviri.
 * Dil algılama, çeviri ve bir istekteki alfabeye.
 * Sözlük döneminin geri çevirileri ve bağlam içinde kullanılan terimleri gösteren örnekleri bulmak için arama alternatif çevirileri için.
 * Dil algılama sonuçları daha bilgilendirici.

## <a name="base-urls"></a>Temel URL

Microsoft Translator, birden çok veri merkezi konumlarını dışında sunulur. 6'da şu anda bulunan [Azure bölgeleri](https://azure.microsoft.com/global-infrastructure/regions):

* **Americas:** Batı ABD 2 ve Batı Orta ABD 
* **Asya Pasifik:** Güneydoğu Asya ve Kore Güney
* **Avrupa:** Kuzey Avrupa ve Batı Avrupa

Microsoft Translator metin çevirisi API'si için isteğin geldiği için en yakın veri merkezi tarafından işlenen çoğu durumda isteklerdir. Veri merkezinde hata oluşması halinde, istek bölgenin dışında yönlendirilebilir.

Belirli bir veri merkezi tarafından işlenmek üzere istek zorlamak için istenen bölge uç noktası için API isteğinde genel uç noktası değiştirin:

|Açıklama|Bölge|Temel URL|
|:--|:--|:--|
|Azure|Genel|  api.cognitive.microsofttranslator.com|
|Azure|Kuzey Amerika|   API nam.cognitive.microsofttranslator.com|
|Azure|Avrupa|  API eur.cognitive.microsofttranslator.com|
|Azure|Asya Pasifik|    API apc.cognitive.microsofttranslator.com|


## <a name="authentication"></a>Kimlik Doğrulaması

Translator metin çevirisi API'si için abone veya [Bilişsel hizmetler hepsi bir arada özellikli](https://azure.microsoft.com/pricing/details/cognitive-services/) Microsoft Bilişsel hizmetler ve aboneliğinizi anahtar (Azure portalında kullanılabilir) kimliğini doğrulamak için kullanın. 

Aboneliğinizi kimliğini doğrulamak için kullanabileceğiniz üç üstbilgileri vardır. Bu tabloda sağlayan her nasıl kullanıldığını açıklar:

|Üst bilgiler|Açıklama|
|:----|:----|
|Ocp-Apim-Subscription-Key|*Gizli anahtarınızı geçirilirse Bilişsel hizmetler abonelikle kullanın*.<br/>Aboneliğinize Translator metin çevirisi API'si için Azure gizli anahtar değeridir.|
|Yetkilendirme|*Bir kimlik doğrulama belirteci geçirilirse, Bilişsel hizmetler abonelikle kullanın.*<br/>Taşıyıcı belirteç değerdir: `Bearer <token>`.|
|Ocp-Apim-abonelik-bölge|*Hepsi bir arada gizli anahtar geçirilirse, Bilişsel hizmetler hepsi bir arada abonelikle kullanın.*<br/>Değer, hepsi bir arada aboneliğin bölgedir. Hepsi bir arada aboneliğinin kullanılmadığında, bu değer isteğe bağlıdır.|

###  <a name="secret-key"></a>Gizli anahtar
İlk seçenek kullanarak kimlik doğrulaması yapmaktır `Ocp-Apim-Subscription-Key` başlığı. Eklemeniz yeterlidir `Ocp-Apim-Subscription-Key: <YOUR_SECRET_KEY>` isteğinize başlığı.

### <a name="authorization-token"></a>Yetkilendirme belirteci
Alternatif olarak, bir erişim belirteci gizli anahtarınızı değiştirebilir. Bu belirteç her isteği olarak birlikte `Authorization` başlığı. Bir yetkilendirme belirteci almak için olun bir `POST` isteği şu URL'ye:

| Ortam     | Kimlik doğrulama hizmeti URL'si                                |
|-----------------|-----------------------------------------------------------|
| Azure           | `https://api.cognitive.microsoft.com/sts/v1.0/issueToken` |

Gizli anahtar verilen bir belirteç elde etmek için örnek isteklerini şunlardır:

```
// Pass secret key using header
curl --header 'Ocp-Apim-Subscription-Key: <your-key>' --data "" 'https://api.cognitive.microsoft.com/sts/v1.0/issueToken'

// Pass secret key using query string parameter
curl --data "" 'https://api.cognitive.microsoft.com/sts/v1.0/issueToken?Subscription-Key=<your-key>'
```

Başarılı bir istek, yanıt gövdesi içinde düz metin olarak kodlanmış bir erişim belirteci döndürür. Geçerli belirteç için Translator hizmeti, yetkilendirme taşıyıcı belirteç olarak geçirilir.

```
Authorization: Bearer <Base64-access_token>
```

Bir kimlik doğrulama belirteci 10 dakika için geçerlidir. Translator API'ları için birden çok çağrı yapılırken belirtecin yeniden kullanılan olmalıdır. Programınızı uzun bir süre Translator API için istek yaparsa, ancak ardından, programınızın yeni bir erişim belirteci düzenli aralıklarla (örneğin 8 dakikada bir) istemeniz gerekir.

### <a name="all-in-one-subscription"></a>Hepsi bir arada abonelik

Son kimlik doğrulama seçeneği bir Bilişsel hizmet hepsi bir arada abonelik kullanmaktır. Bu, birden çok hizmeti isteklerinde kimlik doğrulaması için tek bir gizli anahtar kullanmanıza olanak sağlar. 

Hepsi bir arada gizli anahtar kullandığınızda, isteğinize iki kimlik doğrulama üst bilgileri içermelidir. Gizli anahtar ilk geçirir, ikinci aboneliğinizle ilişkilendirilmiş bir bölgeye belirtir. 
* `Ocp-Api-Subscription-Key`
* `Ocp-Apim-Subscription-Region`

Gizli anahtar parametresi ile sorgu dizesinde geçirdiğiniz `Subscription-Key`, sorgu parametresi bölgeyi belirtin ve `Subscription-Region`.

Taşıyıcı belirteç kullanırsanız, bölge uç noktasından belirteç almanız gerekir: `https://<your-region>.api.cognitive.microsoft.com/sts/v1.0/issueToken`.

Kullanılabilen bölgeler `australiaeast`, `brazilsouth`, `canadacentral`, `centralindia`, `centraluseuap`, `eastasia`, `eastus`, `eastus2`, `japaneast`, `northeurope`, `southcentralus`, `southeastasia`, `uksouth`, `westcentralus`, `westeurope`, `westus`, ve `westus2`.

Bölge için hepsi bir arada Text API aboneliği gereklidir.

## <a name="errors"></a>Hatalar

Standart hata yanıtı ile ad/değer çifti adlı bir JSON nesnesidir `error`. Ayrıca özelliklere sahip bir JSON nesnesi değerdir:

  * `code`Server tanımlı hata kodu.

  * `message`: Hata kullanıcı tarafından okunabilen bir gösterimini sağlar bir dize.

Örneğin, ücretsiz kotayı bitti sonra ücretsiz deneme aboneliği olan bir müşteri aşağıdaki hatayı alırsınız:

```
{
  "error": {
    "code":403001,
    "message":"The operation is not allowed because the subscription has exceeded its free quota."
    }
}
```
3 haneli HTTP durum kodu için 3 basamaklı bir sayı daha da ardından 6 basamaklı bir sayı birleştirme kategorilere ayırma hatası hata kodudur. Genel hata kodları şunlardır:

| Kod | Açıklama |
|:----|:-----|
| 400000| İstek girdilerden biri geçerli değil.|
| 400001| "Scope" parametresi geçersiz.|
| 400002| "Kategori" parametresi geçersiz.|
| 400003| Bir dil tanımlayıcısı eksik veya geçersiz.|
| 400004| ("İçin"betik") bir hedef komut tanımlayıcısı eksik veya geçersiz.|
| 400005| Eksik veya geçersiz bir giriş metni.|
| 400006| Dili ve betiği birleşimi geçerli değil.|
| 400018| ("Kimden"betik") bir kaynak betik tanımlayıcısı eksik veya geçersiz.|
| 400019| Belirtilen dil birini desteklenmiyor.|
| 400020| Giriş metin dizideki öğelerin biri geçerli değil.|
| 400021| API Sürüm parametresi eksik veya geçersiz.|
| 400023| Belirtilen dil çifti biri geçerli değil.|
| 400035| Kaynak dil ("Kimden" alanı) geçerli değil.|
| 400036| Hedef Dil ("Kime" alanı) eksik veya geçersiz.|
| 400042| Bir alanın seçenekleri belirtilen ("Seçenekler") geçerli değil.|
| 400043| İstemci izleme kimliği (ClientTraceId alan veya X-ClientTranceId başlığı) eksik veya geçersiz.|
| 400050| Giriş metni çok uzun.|
| 400064| "Çeviri" parametresi eksik veya geçersiz.|
| 400070| Hedef komut dosyaları (ToScript parametresi) sayısı, hedef dil ('parametresi) sayısı eşleşmiyor.|
| 400071| Değer TextType için geçerli değil.|
| 400072| Giriş metni dizisi çok fazla öğe var.|
| 400073| Betik parametresi geçerli değil.|
| 400074| İstek gövdesi JSON geçerli değil.|
| 400075| Dil eşleştirme ve kategori birleşimi geçerli değil.|
| 400077| En fazla istek boyutu aşıldı.|
| 400079| Gelen ve dil arasında çeviri için istenen özel sistem yok.|
| 401000| Kimlik bilgileri eksik veya geçersiz olduğu için istek yetkili değil.|
| 401015| "Konuşma tanıma API'si için sağlanan kimlik bilgileri değildir. Bu istek metin API'si için kimlik bilgilerini gerektirir. "Lütfen Translator Text API aboneliği kullanın."|
| 403000| İşleme izin verilmiyor.|
| 403001| Abonelik, ücretsiz kotasını aştığı için işleme izin verilmiyor.|
| 405000| İstenen kaynak için istek yöntemi desteklenmiyor.|
| 408001| İstenen özel çeviri sistemi henüz kullanılamıyor. Lütfen birkaç dakika sonra yeniden deneyin.|
| 415000| Content-Type üst bilgisi eksik veya geçersiz.|
| 429000, 429001 429002| İstemci çok fazla istek gönderdiği için sunucu isteği reddetti. Azaltmayı önlemek için istekleri sıklığını azaltın.|
| 500000| Beklenmeyen bir hata oluştu. Hata devam ederse, hatanın tarih/saat ile rapor istek tanımlayıcısı yanıt üst bilgisi X-RequestId ve istek üst bilgisi X-ClientTraceId alınan istemci tanımlayıcısı.|
| 503000| Hizmet geçici olarak kullanılamıyor. Lütfen yeniden deneyin. Hata devam ederse, hatanın tarih/saat ile rapor istek tanımlayıcısı yanıt üst bilgisi X-RequestId ve istek üst bilgisi X-ClientTraceId alınan istemci tanımlayıcısı.|

