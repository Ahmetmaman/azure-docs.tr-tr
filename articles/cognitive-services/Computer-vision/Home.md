---
title: Görüntü İşleme API’si nedir? -Görüntü işleme
titlesuffix: Azure Cognitive Services
description: Görüntü İşleme hizmeti geliştiricilerin görüntü işlemeye ve bilgi döndürmeye yönelik gelişmiş algoritmalara erişmesini sağlar.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: overview
ms.date: 02/08/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: ae421ac3f87a16fc6fd2e5f3e89c13fcae50dbf8
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/15/2019
ms.locfileid: "56311639"
---
# <a name="what-is-computer-vision"></a>Görüntü İşleme nedir?

Azure'un görüntü işleme hizmeti, geliştiricilerin erişimine resimleri işleyen ve döndürmesini gelişmiş algoritmalar sağlar. Bir görüntüyü analiz etmek için görüntü yükleyebilir veya görüntü URL'si belirtebilirsiniz. Görüntü işleme algoritmaları, ilgilendiğiniz görsel özelliklere bağlı olarak birkaç farklı yolla içeriği çözümleyebilirsiniz. Örneğin, görüntü işleme, görüntü, yetişkinlere yönelik veya müstehcen içerikli veya bir resimdeki İnsan yüzlerini tümünün bulabilirsiniz belirleyebilirsiniz.

Görüntü işleme, uygulamanızda kullanarak yerel bir SDK veya REST API'sini doğrudan çağırmak kullanabilirsiniz. Bu sayfa problem görüntü işleme ile yapabilecekleriniz ele alınmaktadır.

## <a name="analyze-images-for-insight"></a>Öngörü görüntülerini analiz edin

Algılayıp, görsel özelliklerini ve özellikler hakkında bilgiler sağlamak için görüntüleri analiz edebilirsiniz. Tüm özellikler aşağıdaki tabloda tarafından sağlanan [analiz görüntü](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) API.

| Eylem | Açıklama |
| ------ | ----------- |
|**[Görsel özellikleri etiketleme](concept-tagging-images.md)**|Belirleyin ve tanınabilir nesne, oturma şey, manzara ve Eylemler binlerce kümesinden bir görüntüde, görsel özellikler etiketleyin. Etiketlerin belirsiz olduğunda veya bilinmediği API yanıtı 'bilinen bir ayar bağlamında etiketin anlamını açıklamak için ipuçları' sağlar. Etiketleme yalnızca temel konu ile sınırlı kalmayıp ortam (iç mekân veya dış mekân), mobilyalar, aletler, bitkiler, hayvanlar, aksesuarlar, araçlar ve benzer öğeleri de kapsar.|
|**[Nesneleri algılayın](concept-object-detection.md)**| Nesne algılama etiketleme için benzer, ancak uygulanan her etiket için sınırlama kutusu koordinatları API döndürür. Örneğin, bir görüntü köpek, cat ve kişi içeriyorsa, Algıla işlemi görüntüde onların koordinatları ile birlikte bu nesneleri listeler. Daha fazla görüntü nesneleri arasındaki ilişkileri işlemek için bu işlevi kullanabilirsiniz. Ayrıca, aynı etiketi görüntünün birden fazla örneği bulunduğunda bilmenizi sağlar.|
|**[Markalarını Algıla](concept-brand-detection.md)**|Resim veya video genel logoları binlerce veritabanı ticari markaları belirleyin. Örneğin, hangi markaları en popüler sosyal medyada veya medya ürün yerleşimiyle en yaygın olduğunu öğrenmek için bu özelliği kullanabilirsiniz.|
|**[Bir görüntüyü kategorilere ayırma](concept-categorizing-images.md)**|Üst/alt öğe kalıtım hiyerarşileri içeren bir [kategori sınıflandırması](Category-Taxonomy.md) kullanarak bir görüntüyü bütünüyle tanımlayın ve kategorilere ayırın. Kategoriler tek başına veya yeni etiketleme modellerimizle birlikte kullanılabilir.<br/>Şu anda, görüntülerin etiketlenmesi ve kategorilere ayrılması için yalnızca İngilizce desteklenmektedir.|
|**[Bir görüntüyü açıklama](concept-describing-images.md)**|Tam cümleler kullanarak bir görüntünün tamamı için okunabilir açıklamalar oluşturun. Görüntü İşleme algoritmaları, görüntüde tanımlanan nesneleri temel alan çeşitli açıklamalar oluşturur. Açıklamaların her biri değerlendirilir ve bir güvenilirlik puanı oluşturulur. Ardından, güvenilirlik puanı için azalan düzende sıralı bir liste döndürülür.|
|**[Yüz algılama](concept-detecting-faces.md)** |Bir görüntüdeki yüzleri algılayın ve algılanan her bir yüz için bilgiler sunun. Görüntü İşleme algılanan her bir yüz için koordinatları, dikdörtgeni, cinsiyeti ve yaşı döndürür.<br/>Görüntü İşleme, [Yüz Tanıma](/azure/cognitive-services/face/)'da bulunan işlevlerin bir alt kümesini sunar ve Yüz tanımanın yanı sıra poz algılama gibi daha ayrıntılı analiz işlemleri için Yüz Tanıma hizmetini kullanabilirsiniz.|
|**[Görüntü türünü algılama](concept-detecting-image-types.md)**|Bir görüntü ile ilgili özellikleri (görüntünün çizim olup olmaması veya küçük resim olup olmama olasılığı gibi) algılayın.|
|**[Etki alanına özgü içeriği algılama](concept-detecting-domain-content.md)**|Bir görüntüde yer alan, ünlüler ve önemli yerler gibi etki alanına özgü içerikleri algılamak ve tanımak için etki alanı modelleri kullanın. Örneğin; bir görüntüde insanlar yer alıyorsa Görüntü İşleme, görüntüde algılanan kişilerin ünlü olup olmadığını belirlemek üzere hizmetle birlikte ünlülere yönelik bir etki alanı modeli kullanabilir.|
|**[Renk düzenini algılama](concept-detecting-color-schemes.md)**|Bir görüntüdeki renk kullanımını analiz edin. Görüntü İşleme, bir görüntünün siyah-beyaz olup olmadığını belirlemenin yanı sıra renkli görüntüler için baskın renkleri ve vurgu renklerini tanıyabilir.|
|**[Küçük resim oluşturma](concept-generating-thumbnails.md)**|Görüntülere uygun küçük resimler oluşturmak üzere söz konusu görüntülerin içeriklerini analiz edin. Görüntü işleme ilk yüksek kaliteli bir küçük resim oluşturur ve ardından belirlemek için resim içindeki nesneleri analiz eder *ilgi*. Görüntü işleme, ardından ilgi alanı gereksinimlerini resmi kırpar. Oluşturulan küçük resim, ihtiyaçlarınız doğrultusunda, özgün görüntünün en boy oranından farklı bir en boy oranı kullanılarak sunulabilir.|
|**[İlgi alanı Al](concept-generating-thumbnails.md#area-of-interest)**|Çözümle koordinatlarını döndürmek için bir görüntü içeriğini *ilgi*. Bu küçük resim oluşturma için kullanılan aynı işlevi, ancak orijinal görüntünün istediğiniz şekilde çağıran uygulama değiştirebilmesi görüntü kırpma yerine, görüntü işleme, bölge sınırlama kutusu koordinatları döndürür.|


## <a name="extract-text-from-images"></a>Metni ayıklayın

Bir görüntüdeki [metinleri OCR kullanarak](concept-extracting-text-ocr.md) makine tarafından okunabilen bir karakter akışı halinde ayıklamak üzere Görüntü İşleme'yi kullanabilirsiniz. Gerekirse, OCR, tanınan metnin yönünü yatay görüntü ekseninde derece olarak düzeltir ve her bir sözcük için çerçeve koordinatlarını verir. OCR 25 dili destekler ve ayıklanan metnin dilini otomatik olarak algılar.

Ayrıca görüntülerdeki [basılı veya el yazısıyla yazılmış metinleri de algılayabilirsiniz](concept-recognizing-text.md). Görüntü İşleme; makbuz, poster, kartvizit, mektup ve beyaz tahtalar gibi farklı yüzeylere ve arka planlara sahip çeşitli nesnelerin görüntülerindeki basılı ve el yazısıyla yazılmış metinleri algılayabilir ve ayıklayabilir. Şu anda, basılı ve el yazısıyla yazılmış metinleri tanıma özelliği önizlemededir ve yalnızca İngilizce dilinde kullanılabilmektedir.  

## <a name="moderate-content-in-images"></a>Orta içeriklerin

Görüntü İşleme'yi, görüntüde yetişkinlere yönelik veya müstehcen içerik bulunup bulunmama olasılığını derecelendirerek ve her ikisi için güvenilirlik puanı oluşturarak [yetişkinlere yönelik veya müstehcen içerikleri algılamak](concept-detecting-adult-content.md) üzere kullanabilirsiniz. Yetişkinlere yönelik veya müstehcen içeriklerin algılanmasına yönelik filtre, tercihleriniz doğrultusunda bir kaydırıcı ölçeği üzerinde ayarlanabilir.

## <a name="use-containers"></a>Kapsayıcıları kullanma

[Görüntü işleme kapsayıcılarını](computer-vision-how-to-install-containers.md) verilerinize yakın standartlaştırılmış bir Docker kapsayıcısı yükleyerek yazdırılan ve el yazısı metinleri yerel olarak tanınması için.

## <a name="image-requirements"></a>Görüntü gereksinimleri

Görüntü İşleme, şu gereksinimleri karşılayan görüntüleri analiz edebilir:

- Sunulan görüntünün JPEG, PNG, GIF veya BMP biçiminde olması gerekir
- Görüntünün dosya boyutunun 4 megabayt (MB) değerini aşmaması gerekir
- Görüntünün boyutlarının 50 x 50 pikselden büyük olması gerekir
  - OCR için görüntü boyutlarının 50 x 50 ile 4200 x 4200 piksel arasında olması gerekir

## <a name="data-privacy-and-security"></a>Veri gizliliği ve güvenliği

Olarak tüm Bilişsel hizmetler görüntü işleme hizmeti kullanan geliştiricilerin Microsoft'un müşteri verilerini ilkelerinin bilmeniz gerekir. Bkz: [Bilişsel Hizmetler sayfasına](https://www.microsoft.com/trustcenter/cloudservices/cognitiveservices) daha fazla bilgi için Microsoft Trust Center.

## <a name="next-steps"></a>Sonraki adımlar

Görüntü işleme ile Hızlı Başlangıç Kılavuzu izleyerek kullanmaya başlayın:

- [Hızlı Başlangıç: Bir resmi çözümleme](quickstarts-sdk/csharp-analyze-sdk.md)
- [Hızlı Başlangıç: El yazısı metinleri ayıklamak](quickstarts-sdk/csharp-hand-text-sdk.md)
- [Hızlı Başlangıç: Küçük resim oluşturma](quickstarts-sdk/csharp-thumb-sdk.md)
