---
title: Amaçlar-LUSıS
titleSuffix: Azure Cognitive Services
description: Tek bir amaç, kullanıcının gerçekleştirmek istediği bir görevi veya eylemi temsil eder. Bir amaç veya hedef kullanıcının utterance ifade edilen olduğundan. Kullanıcıların uygulamanızda almak istediğiniz eylemlerine karşılık gelen bir ıntents kümesi tanımlar.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 07/29/2019
ms.author: diberry
ms.openlocfilehash: bb7fa9d930f4c1ab3c241048804060e17fe5a8e4
ms.sourcegitcommit: 08d3a5827065d04a2dc62371e605d4d89cf6564f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2019
ms.locfileid: "68619926"
---
# <a name="concepts-about-intents-in-your-luis-app"></a>LUSıS uygulamanızdaki amaçlar hakkındaki kavramlar

Kullanıcı eylem gerçekleştirmek istediği ya da bir amacı bir görevi gösterir. Bir amaç veya hedef bir kullanıcının ifade olduğundan [utterance](luis-concept-utterance.md).

Kullanıcıların uygulamanızda almak istediğiniz eylemlerine karşılık gelen bir ıntents kümesi tanımlar. Örneğin, bir seyahat uygulaması, çeşitli hedefleri tanımlar:

Uygulama amaçları seyahat   |   Örnek konuşmalar   | 
------|------|
 BookFlight     |   "Bana bir uçuş RIO için sonraki hafta kitap" <br/> "Benim için RIO üzerinde 24 uçarak" <br/> "Uçak bileti Rio de Janeiro sonraki Sunday ihtiyacım"    |
 Karşılama     |   "Hi" <br/>"Hello" <br/>"Günaydın"  |
 CheckWeather | "Gibi de Boston hava nedir?" <br/> "Bu hafta için hava durumu tahminini Göster" |
 None         | "Bana bir tanımlama bilgisi tarif Al"<br>"Lakers win?" |

Tüm uygulamalar, geri dönüş amacı olan önceden tanımlanmış "[none](#none-intent-is-fallback-for-app)" hedefi ile gelir. 

## <a name="prebuilt-domains-provide-intents"></a>Önceden oluşturulmuş etki alanları hedefleri belirtin
Tanımladığınız ıntents ek olarak, önceden oluşturulmuş hedefleri önceden oluşturulmuş etki alanlarından birini kullanabilirsiniz. Daha fazla bilgi için [LUIS uygulamalarında önceden oluşturulmuş etki alanlarını](luis-how-to-use-prebuilt-domains.md) ıntents uygulamanızda kullanmak için önceden oluşturulmuş bir etki alanından özelleştirme hakkında bilgi edinmek için.

## <a name="return-all-intents-scores"></a>Tüm hedefleri puanlarını döndürür
Tek bir hedefi için bir utterance atarsınız. LUIS uç noktasında bir utterance aldığında, o utterance bir üst hedefini döndürür. Utterance için tüm hedefleri puanları istiyorsanız sağlayabilir `verbose=true` bayrağı API sorgu dizesini [uç noktası çağrısı](https://aka.ms/v1-endpoint-api-docs). 

## <a name="intent-compared-to-entity"></a>Varlığa karşılaştırıldığında hedefi
Amaç, sohbet botu için kullanıcının gerçekleştirmesi gereken ve tüm utterance üzerinde temel eylemini temsil eder. Varlık sözcük ve tümcecikleri utterance içinde yer alan temsil eder. Bir utterance hedefi Puanlama yalnızca bir üste sahip olabilir ancak birçok varlık sahip olabilir. 

<a name="how-do-intents-relate-to-entities"></a>

Kullanıcının _amacı_ , checkhava durumu () işlevine yapılan bir çağrı gibi istemci uygulamanızda bir eylem tetikleyeceğinden bir amaç oluşturun. Ardından eylemi yürütmek için gerekli parametreleri göstermek için bir varlık oluşturun. 

|Örnek hedefi   | Varlık | Örnek konuşma varlık   | 
|------------------|------------------------------|------------------------------|
| CheckWeather | {"type": "Konum", "varlık": "seattle"}<br>{"type": "builtin.datetimeV2.date","entity": "yarın", "Çözüm": "2018-05-23"} | Hava durumu, beğendiğiniz Özellikler `Seattle` `tomorrow`? |
| CheckWeather | {"type": "date_range", "varlık": "Bu hafta sonu"} | Tahmini Göster `this weekend` | 
||||

## <a name="custom-intents"></a>Özel bir ıntents

Benzer şekilde niyetli [konuşma](luis-concept-utterance.md) için tek bir hedefi karşılık gelir. Amacınız, konuşma herhangi kullanabilirsiniz [varlık](luis-concept-entity-types.md) varlıkları hedefi özgü olmadığından uygulama. 

## <a name="prebuilt-domain-intents"></a>Önceden oluşturulmuş etki alanı hedefleri

[Önceden oluşturulmuş etki alanları](luis-how-to-use-prebuilt-domains.md) konuşma amaçlarıyla sahip.  

## <a name="none-intent"></a>Amaç yok

**Hiçbiri** amacı her uygulama için önemli değildir ve sıfır utlanmaslar içermemelidir.

### <a name="none-intent-is-fallback-for-app"></a>Hedefi hiçbir uygulama için geri dönüş
**Hiçbiri** amacı olan bir genel ya da geri dönüş hedefi. LUIS uygulama etki alanında (konu alanı), önemli değildir. Konuşma öğretmek için kullanılır. **Hiçbiri** amacı, uygulamadaki toplam konuşma yüzde 20'si ile 10 arasındaki olmalıdır. Hiçbirini boş bırakmayın. 

### <a name="none-intent-helps-conversation-direction"></a>Hedefi yok konuşma yönü de yardımcı olur.
Bir utterance hiçbiri tahmin edildiğinde, hedefi ve döndürülen bu tahmin ile Sohbet botu için robot daha fazla soru veya sohbet botu geçerli seçeneklerdir kullanıcıya yönlendirmek için bir menü sağlayın. 

### <a name="no-utterances-in-none-intent-skews-predictions"></a>Hiçbir konuşma niyetini hiçbiri eğriltir Öngörüler
İçin herhangi bir konuşma eklemezseniz **hiçbiri** hedefi, LUIS etki alanına bir etki alanı amacı dışında bir utterance zorlar. LUIS utterance yanlış hedefini eğitiminde tarafından bu tahmin puanları eğme. 

### <a name="add-utterances-to-the-none-intent"></a>Konuşma hiçbiri hedefi ekleme
**Hiçbiri** hedefi oluşturulur, ancak boş bilerek. Etki alanı dışındaki bir konuşma ile doldurun. İçin iyi bir utterance **hiçbiri** tamamen uygulamanın yanı sıra sektör uygulama dışında bir şey hizmet olduğu. Örneğin, bir seyahat uygulaması için herhangi bir konuşma kullanmamalıdır **hiçbiri** faturalandırma food, barındırma, kargo, Hareket halindeki eğlence ayırmalar gibi her fırsatta seyahat etmeye ilişkili. 

Ne tür bir konuşma bırakılır kullanmışsanız hedefi? Belirli bir şey ile botunuza böyle "ne tür bir dinozor mavi tam var?" yanıt olmamalıdır Başlat Uzak bir seyahat uygulaması dışında çok belirli bir sorunuz budur. 

### <a name="none-is-a-required-intent"></a>Hiçbiri gerekli bir hedefi olan
**Hiçbiri** amacı gerekli amacı ve silinemez veya yeniden adlandırılamaz.

## <a name="negative-intentions"></a>Negatif amaçları 
Negatif ve pozitif amaçları gibi belirlemek istiyorsanız "miyim **istediğiniz** bir araba" ve "ı **yoksa** bir araba istediğiniz", iki hedefleri (bir pozitif ve negatif bir) oluşturma ve için uygun Konuşma ekleme Her. Tek bir hedefi oluşturma ve iki farklı pozitif ve negatif koşulları bir varlık olarak işaretleyin.  

## <a name="intents-and-patterns"></a>Amaçlar ve desenler

Bir normal ifade olarak kısmen veya bütün olarak tanımlanabilen örnek utsunuz varsa, bir [desen](luis-concept-patterns.md)ile eşleştirilmiş [normal ifade varlığını](luis-concept-entity-types.md#regular-expression-entity) kullanmayı düşünün. 

Bir normal ifade varlığı kullanmak, düzenin eşleşmesi için veri ayıklamasını garanti eder. Bu eşleştirme, tam bir amacı garanti eder. 

## <a name="intent-balance"></a>Intent bakiyesi
Uygulama etki alanı hedefleri konuşma dengesi her hedefi arasında olmalıdır. 10 Konuşma ile bir hedefi ve 500 Konuşma ile başka bir amacı yoktur. Bu dengeli değil. Bu durum varsa, birçok hedefleri halinde yeniden, görmek için 500 konuşma amacıyla gözden geçirin. bir [deseni](luis-concept-patterns.md). 

**Hiçbiri** hedefi bakiyeye dahil değildir. Bu hedefi % uygulamasında toplam konuşma 10 içermelidir.

## <a name="intent-limits"></a>Intent sınırları
Gözden geçirme [sınırları](luis-boundaries.md#model-boundaries) kaç hedefleri öğrenmek için bir model ekleyebilirsiniz. 

### <a name="if-you-need-more-than-the-maximum-number-of-intents"></a>Intents en fazla sayısından fazlasını gerekiyorsa 
İlk olarak, sisteminize çok fazla ıntents kullanıp kullanmadığını göz önünde bulundurun. 

### <a name="can-multiple-intents-be-combined-into-single-intent-with-entities"></a>Birden çok hedefleri tek amacı, varlıklarla birleştirilebilir 
Çok benzer bir ıntents LUIS bunları ayırt daha zor yapabilirsiniz. Intents olması için anın ancak alan, kodunuzun her yolunu yakalamak üzere gerekmeyen ana görevleri yakalamak için yeterli değiştirilen. Örneğin, bir seyahat uygulamasında ayrı ıntents BookFlight ve FlightCustomerService olabilir, ancak BookInternationalFlight ve BookDomesticFlight çok benzerdir. Bunları ayırt etmek sisteminizi gerekiyorsa, varlıklar veya diğer mantıksal yerine hedefleri kullanın. 

### <a name="dispatcher-model"></a>Dağıtıcı modeli
LUIS ve soru-cevap Oluşturucu uygulamalarla birleştirme hakkında daha fazla bilgi [gönderme modeli](luis-concept-enterprise.md#when-you-need-to-combine-several-luis-and-qna-maker-apps). 

### <a name="request-help-for-apps-with-significant-number-of-intents"></a>Önemli sayıda hedefleri olan uygulamalar için yardım iste
Intents sayısını azaltmayı ya da birden çok uygulamalarda, hedefleri bölme sizin için işe yaramazsa, desteğe başvurun. Destek Hizmetleri Azure aboneliğinize dahildir, başvurun [Azure teknik desteğine](https://azure.microsoft.com/support/options/). 



## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi edinin [varlıkları](luis-concept-entity-types.md), önemli sözcükleri hedefleri için uygun olan
* Bilgi nasıl [ekleyin ve hedefleri yönetin](luis-how-to-add-intents.md) LUIS uygulamanızda.
* Gözden geçirin, amacı [en iyi uygulamalar](luis-concept-best-practices.md)
