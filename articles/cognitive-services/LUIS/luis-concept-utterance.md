---
title: İyi örnek utterer-LUSıS
description: İfadeler kullanıcının yaptığı ve uygulamanızın yorumlaması gereken girişlerdir. Kullanıcıların girecağı tümcecikleri toplayın. Aynı şeyi gösteren, ancak sözcük uzunluğu ve sözcük yerleşimi içinde farklı şekilde oluşturulan utterleri dahil edin.
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 05/19/2020
ms.openlocfilehash: 46004d81756809958e359c2a0b72c143599c2853
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "101706767"
---
# <a name="understand-what-good-utterances-are-for-your-luis-app"></a>LUSıS uygulamanız için nelerin iyi olduğunu anlayın

**Söyleyceler** , uygulamanızın yorumlamak için gereken kullanıcıdan gelen giriştir. LUO 'dan amaçları ve varlıkları ayıklamak için, her bir amaç için çeşitli farklı örnek türlerini yakalamak önemlidir. Etkin öğrenimi veya yeni vavaslar üzerinde eğitim almaya devam etme süreci, LUIN sağladığı makine öğrenimi zekası için gereklidir.

Kullanıcıların girebileceği düşündüklerini toplayın. Aynı şeyi gösteren, ancak çeşitli yollarla oluşturulan utterleri dahil edin:

* Utterance uzunluğu-istemci uygulamanız için kısa, orta ve uzun
* Sözcük ve tümcecik uzunluğu
* Sözcük yerleşimi-noktadan itibaren, ortadaki ve sonunda varlık
* Dilbilgisi
* Çoğullaştırma
* Kesintilerinden kaynaklanan
* Ad ve fiil seçimi
* [Noktalama](luis-reference-application-settings.md#punctuation-normalization) -doğru, yanlış ve dilbilgisi kullanımı çok iyi

## <a name="how-to-choose-varied-utterances"></a>Değişen detersliği seçme

LUSıS modelinize [örnek eklemeler ekleyerek](./luis-how-to-add-entities.md) ilk kez başladıysanız göz önünde bulundurmanız gereken bazı ilkeler aşağıda verilmiştir.

### <a name="utterances-arent-always-well-formed"></a>Utterslar her zaman iyi biçimlendirilmemiş

"" Kayıt "veya" Paris uçuş "gibi bir cümle parçası olan" benim için Istanbul için bir bilet  Kullanıcılar genellikle yazım hataları yapar. Uygulamanızı planlarken, LUO 'ya geçirmeden önce Kullanıcı girişini düzeltmek için [Bing yazım denetimi](luis-tutorial-bing-spellcheck.md) kullanıp kullanmayacağınızı düşünün.

Kullanıcı araslarını yazım denetimi yapmazsanız, LUSıS 'yi, yazım hataları ve yazım hataları içeren uttaslar üzerinde eğitmelisiniz.

### <a name="use-the-representative-language-of-the-user"></a>Kullanıcının temsili dilini kullan

Utterlere seçerken, yaygın bir terim veya tümcecik, istemci uygulamanızın tipik kullanıcısı için doğru olmayabilir. Etki alanı deneyimine sahip olmayabilir. Bir kullanıcının yalnızca uzman olmaları durumunda söyledikleri terimleri veya tümceleri kullanırken dikkatli olun.

### <a name="choose-varied-terminology-as-well-as-phrasing"></a>Değişen terminolojiyi ve ifade ' i seçin

Fark eden tümce desenleri oluşturmaya yönelik çabalar oluştursanız bile, bazı sözlük tekrarlamaya devam edersiniz.

Bu örnek aşağıdaki adımları uygulayın:

|Örnek konuşmalar|
|--|
|bir bilgisayarı nasıl edinebilirim?|
|Bir bilgisayarı nereden alabilirim?|
|Bir bilgisayar almak istiyorum, nasıl gidebilirim?|
|Bir bilgisayar ne zaman olabilir?|

Burada temel terim, *bilgisayar*, değişken değildir. Masaüstü bilgisayar, dizüstü bilgisayar, iş istasyonu veya hatta yalnızca makine gibi alternatifleri kullanın. LUU bağlamdaki Eşanlamlı sözcükleri akıllıca çıkarabilir, ancak eğitim için utumslar oluştururken bunları değiştirmek her zaman daha iyidir.

## <a name="example-utterances-in-each-intent"></a>Her amaç için örnek söylenme

Her bir amaç, en az 15 örnek bir olmalıdır. Herhangi bir örnek elde gerektirmeyen bir amaç varsa, LUO 'yı eğitemeyeceksiniz. Bir veya çok az örnek ile ilgili bir amaç varsa, Lu, amacı doğru tahmin edemeyebilir.

## <a name="add-small-groups-of-15-utterances-for-each-authoring-iteration"></a>Her yazma yinelemesi için küçük sayıda 15 utterations ekleyin

Modelin her yinelemesinde, büyük miktarlarda sayı eklemeyin. Sayıları 15 ' te ekleyin. Yeniden [eğitin](luis-how-to-train.md), [yayımlayın](luis-how-to-publish-app.md)ve [Test](luis-interactive-test.md) edin.

LUSıS, lular model yazarı tarafından dikkatle seçilmiş olan deterleri olan etkili modeller oluşturur. Çok fazla sayıda söyleyme eklemek karışıklık sunduğundan önemli değildir.

Birkaç noktadır başlamak daha iyidir, ardından doğru amaç tahmini ve varlık ayıklama için [uç nokta utslerini gözden geçirin](luis-how-to-review-endpoint-utterances.md) .

## <a name="utterance-normalization"></a>Utterance normalleştirmesi

Utterance normalleştirme, eğitim ve tahmin sırasında metin türlerinin (noktalama ve aksan gibi) etkilerini yok saymakla oluşan bir işlemdir.

Söylenişi normalleştirme ayarları varsayılan olarak kapalıdır. Bu ayarlar şunlardır:

* Sözcük formları
* İşaretlerini
* Noktalama işaretleri

Bir normalleştirme ayarı açarsanız, **Test** bölmesi, toplu iş testleri ve uç nokta sorguları, bu normalleştirme ayarı için tüm söyleyenlerdeki puanlar değişir.

LUU portalındaki bir sürümü kopyaladığınızda, sürüm ayarları yeni kopyalanmış sürüme devam eder.

Sürüm ayarlarını,, **Yönetim** bölümünde, **uygulama ayarları** sayfasında veya [Sürüm AYARLARıNı Güncelleştir API 'si](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/versions-update-application-version-settings)aracılığıyla halan portalı aracılığıyla ayarlayın. Bu normalleştirmede bu değişiklikler hakkında daha fazla bilgi [edinin.](luis-reference-application-settings.md)

### <a name="word-forms"></a>Sözcük formları

**Sözcük biçimlerinin** normalleştirilmesi, kökün ötesinde görüntülenen sözcüklerdeki farkları yoksayar.

<a name="utterance-normalization-for-diacritics-and-punctuation"></a>

### <a name="diacritics"></a>İşaretlerini

Aksanlar, metin içindeki işaretler veya işaretlerdir, örneğin:

```
İ ı Ş Ğ ş ğ ö ü
```

### <a name="punctuation-marks"></a>Noktalama işaretleri
**Noktalama işareti** , modelleriniz eğitilen ve uç nokta sorgularınız tahmin etmeden önce, noktalama işaretlerinden kaldırılacak şekilde görünür.

Noktalama, LUSıS 'de ayrı bir belirteçtir. Uçta nokta içermeyen bir nokta ile sonunda bir nokta içeren bir söylenişi iki ayrı tanüler ve iki farklı tahmin elde edebilir.

Noktalama işareti normalleştirilmezse, bazı istemci uygulamalar bu işaretlere anlam yerleştirebilir, varsayılan olarak, Lu, noktalama işaretlerini yoksayar. Her iki stilin de aynı göreli puanları döndürmesi için, örnek uttlarınızın hem noktalama işaretlerini hem de noktalama işaretlerini kullantığınızdan emin olun.

Modelin noktalama işaretlerini (noktalama yok) veya özel sözdizimiyle noktalama işaretlerini gözardı etmek daha kolay olan [desenlerdeki](luis-concept-patterns.md) noktalama işaretlerini işlediği emin olun: `I am applying for the {Job} position[.]`

Noktalama, istemci uygulamanızda belirli bir anlamı yoksa, noktalama işaretlerini normalleştirerek [noktalama işaretlerini yok saymayı](#utterance-normalization) düşünün.

### <a name="ignoring-words-and-punctuation"></a>Sözcükler ve noktalama işaretleri yoksayılıyor

Desenlerde belirli sözcükleri veya noktalama işaretlerini yoksaymak istiyorsanız köşeli ayraçın _Yoksay_ sözdizimi olan bir [desen](luis-concept-patterns.md#pattern-syntax) kullanın `[]` .

<a name="training-utterances"></a>

## <a name="training-with-all-utterances"></a>Tüm dıklarla eğitim

Eğitim genellikle belirleyici değildir: söylenişi tahmini sürümler veya uygulamalar arasında biraz farklılık gösterebilir.
[Sürüm ayarları](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/versions-update-application-version-settings) API 'sini, `UseAllTrainingData` tüm eğitim verilerini kullanacak şekilde ad/değer çiftiyle güncelleştirerek, belirleyici olmayan eğitimi kaldırabilirsiniz.

## <a name="testing-utterances"></a>Söyleyceler test etme

Geliştiriciler, bir [tahmin uç noktası](luis-how-to-azure-subscription.md) URL 'sine utser göndererek, lusıs uygulamasının gerçek trafikle test edilmesine başlamamalıdır. Bu [Söyleyime, gözden geçirme](luis-how-to-review-endpoint-utterances.md)ve varlıkların performansını geliştirmek için kullanılır. LUSıS Web sitesi test bölmesi ile gönderilen testler, uç nokta aracılığıyla gönderilmez ve bu nedenle etkin öğrenimine katkıda bulunun.

## <a name="review-utterances"></a>Detersliği gözden geçirme

Modelinize eğitilen, yayımladım ve [uç nokta](luis-glossary.md#endpoint) sorgularını aldıktan sonra, Luo tarafından önerilen noktaları [gözden geçirin](luis-how-to-review-endpoint-utterances.md) . LUO, amaç veya varlık için düşük puanları olan uç nokta dıklarını seçer.

## <a name="best-practices"></a>En iyi uygulamalar

[En iyi uygulamaları](luis-concept-best-practices.md) gözden geçirin ve bunları düzenli yazma döngünüzün bir parçası olarak uygulayın.

## <a name="label-for-word-meaning"></a>Sözcük anlamı etiketi

Sözcük seçimi veya sözcük düzenlemesi aynıysa, ancak aynı şeyi içermiyorsa, varlıkla etiketlemeyin.

Aşağıdaki söyleyde, sözcük `fair` hograf ' dır. Aynı şekilde yazılmış ancak farklı bir anlamı vardır:

|İfade|
|--|
|Bu yaz Seattle alanında ne tür bir ilçe FAIRS oluyor?|
|Seattle incelemesi için geçerli derecelendirme mi?|

Tüm olay verilerini bulmak için bir olay varlığı istediyseniz, `fair` ilk utterde sözcüğü etiketleyip ikincinin.


## <a name="next-steps"></a>Sonraki adımlar
Kullanıcı araslarını anlamak için bir LUO uygulamasını eğitme hakkında daha fazla bilgi için bkz. [örnek ekleme](./luis-how-to-add-entities.md) .