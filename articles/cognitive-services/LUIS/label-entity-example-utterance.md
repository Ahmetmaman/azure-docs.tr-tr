---
title: Etiket varlık örneği söylenişi
description: LUı portalının amaç ayrıntısı sayfasında örnek bir şekilde bir makine öğrenimi varlığını alt varlıklarla nasıl etiketleyeceğinizi öğrenin.
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 05/20/2020
ms.openlocfilehash: ffbaa2e40d5924ba61e548398e63295cf7dba2b0
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91303735"
---
# <a name="label-machine-learning-entity-in-an-example-utterance"></a>Örnek bir örnekte makine öğrenimi varlığı etiketleme

Bir varlığın bir örnek içinde etiketlenmesi, LUYA varlığın ne olduğu ve varlığın utterde görünebileceği bir örnektir.

Makine tarafından öğrenilen varlıkları ve alt varlıkları etiketleyebilir.

Normal ifade, liste veya önceden oluşturulmuş varlıkları etiketleyip bir varlık veya alt varlık oluşturun, ardından bu varlıkları varlık veya alt varlığa uygun olduğunda özellik olarak ekleyin.

## <a name="label-example-utterances-from-the-intent-detail-page"></a>Amaç ayrıntısı sayfasından etiket örneği

Utterance içindeki varlıkların örneklerini etiketlemek için, utterance 'in amacını seçin.

1. [Luo portalında](https://www.luis.ai)oturum açın ve bu yazma kaynağına atanmış uygulamaları görmek için **aboneliğinizi** ve **yazma kaynağını** seçin.
1. **Uygulamalarım** sayfasında adını seçerek uygulamanızı açın.
1. Bir varlıkla ayıklama için etiketlemek istediğiniz örnek bir amaç seçin.
1. Etiketlemek istediğiniz metni seçin ve ardından varlığı seçin.

## <a name="two-techniques-to-label-entities"></a>Varlıkların etiketlenmesi için iki teknik

Amaç ayrıntısı sayfasında iki etiketleme tekniği desteklenir.
* Varlık [paletinden](#label-with-the-entity-palette-visible) varlık veya alt varlık ' ı seçin ve ardından örnek söylenişi metin ' i seçin. Bu, şemanıza göre doğru varlık veya alt varlıkla çalıştığınızı görsel olarak doğrulayabileceğiniz için önerilen tekniktir.
* Önce örnek söylenişi metin içinde öğesini seçin. Bunu yaptığınızda [etiketleme seçeneklerinin](#how-to-label-entity-from-in-place-menu) bir açılır menüsü sunulur.

## <a name="label-with-the-entity-palette-visible"></a>Varlık paleti görünür olan etiket

[Şemanızı varlıklar ile planladıktan](luis-how-plan-your-app.md)sonra etiketleme sırasında **varlık paletini** görünür tutun. **Varlık paleti** , hangi varlıkların ayıklanacağını hatırlatır.

**Varlık paletine**erişmek için, **@** örnek araç listesinin yukarıdaki bağlam araç çubuğundan sembolünü seçin.

> [!div class="mx-imgBorder"]
> ![Amaç ayrıntıları sayfasındaki varlık paleti ekran görüntüsü.](media/label-utterances/entity-palette-from-tool-bar.png)

## <a name="how-to-label-entity-from-entity-palette"></a>Varlık paletinden Varlık etiketleme

Varlık paleti, önceki etiketleme deneyimine bir alternatif sunar. Bir varlıkla anında etiketlemek için metnin üzerine fırçanızı sağlar.

1. **@** Söylenişi tablosunun sağ üst köşesindeki simgeyi seçerek varlık paletini açın.

2. Etiketlemek istediğiniz paletten varlığı seçin. Bu eylem, görsel olarak yeni bir imlece belirtilir. İmleç, LULAR portalında hareket ettirdiği şekilde fareyi izler.

3. Örnekte, varlığı imlece _boyayın_ .

    > [!div class="mx-imgBorder"]
    > ![Ekran görüntüsü, imlece boyanmış olan varlığı gösterir.](media/label-utterances/example-1-label-machine-learned-entity-palette-label-action.png)

## <a name="adding-entity-as-a-feature-from-the-entity-palette"></a>Varlık paletindeki varlık özelliği olarak ekleme

Varlık paletinin alt bölümü, şu anda seçili varlığa özellikler eklemenize olanak tanır. Tüm mevcut varlıklar ve tümcecik listelerinden seçim yapabilir veya yeni bir tümcecik listesi oluşturabilirsiniz.

> [!div class="mx-imgBorder"]
> ![Özellik olarak varlık olan varlık paleti ekran görüntüsü](media/label-utterances/entity-palette-entity-as-a-feature.png)

## <a name="labeling-entity-roles"></a>Varlık rollerini etiketleme

Varlık rolleri, **varlık paleti**kullanılarak etiketlenir.

1. Amaç ayrıntısı sayfasında bağlam araç çubuğundan **varlık paleti** ' ni seçin.
1. Varlık paleti açıldıktan sonra varlık listesinden varlığı seçin.
1. Varlık listesinin altında var olan bir rolü seçin.
1. Örnek söylenişi metninde, metni varlık rolüyle etiketleyin.

## <a name="how-to-label-entity-from-in-place-menu"></a>Yerinde menüden Varlık etiketleme

Yerinde etiketleme, söylemenin içindeki metni hızlıca seçmenizi ve etiketlemesini sağlar. Etiketli metinden bir makine öğrenimi varlığı veya liste varlığı da oluşturabilirsiniz.

Örnek utterance 'i göz önünde bulundurun `hi, please I want a cheese pizza in 20 minutes` .

En soldaki metni seçin, ardından varlığın en sağ metnini seçin, sonra yerinde menüsünde, etiketlemek istediğiniz varlığı seçin.

> [!div class="mx-imgBorder"]
> ![Etiket tam makine öğrenimi varlığı](media/label-utterances/label-steps-in-place-menu.png)

## <a name="review-labeled-text"></a>Etiketli metni gözden geçirme

Etiketledikten sonra, örnek bir şekilde gözden geçirin ve seçilen varlık için seçili varlıkla metnin altı çizili olduğundan emin olun. Düz çizgi, metnin etiketlendiği anlamına gelir.

> [!div class="mx-imgBorder"]
> ![Tam makine öğrenimi varlığı etiketlendi](media/label-utterances/example-1-label-machine-learned-entity-complete-order-labeled.png)

## <a name="confirm-predicted-entity"></a>Tahmin edilen varlığı Onayla

Metnin yayılması etrafında noktalı çizgili bir kutu varsa, metnin tahmin _edildiğini ancak henüz etiketlenmediğini_belirtir. Tahmine bir etiketi açmak için, söylenişi satırını seçin, sonra bağlamsal araç çubuğundan **varlıkları Onayla** ' yı seçin.

## <a name="relabeling-over-existing-entities"></a>Mevcut varlıkları ele alınıyor

Zaten etiketlenmiş bir metin varsa, LUYA var olan etiketleri bölebilir veya birleştirebilir.

## <a name="labeling-for-punctuation"></a>Noktalama işaretleri için etiketleme

Noktalama için etiketlemenize gerek yoktur. Noktalama işaretlerinin söylenişi öngörülerini nasıl etkilediğini denetlemek için [uygulama ayarlarını](luis-reference-application-settings.md) kullanın.

## <a name="unlabel-entities"></a>Varlıkların etiketini kaldır

> [!NOTE]
> Yalnızca makine tarafından öğrenilen varlıkların etiketi etiketsiz olabilir. Normal ifade varlıklarını, liste varlıklarını veya önceden oluşturulmuş varlıkları etiketleyip etiketleyemiyorum.

Bir varlığın etiketini kaldırmak için varlığı seçin ve yerinde menüden **etiketi kaldır** ' ı seçin.

> [!div class="mx-imgBorder"]
> ![Etiketlemeyi kaldırma varlığını gösteren ekran görüntüsü](media/label-utterances/unlabel-entity-using-in-place-menu.png)

## <a name="automatic-labeling-for-parent-and-child-entities"></a>Üst ve alt varlıklar için otomatik etiketleme

Bir üst varlığı etiketlendirmekte olduğunuz sırada, eğitilen sürüme göre tahmin edilebilir olan herhangi bir alt varlık etiketlenecek.

Bir alt varlık için etiketleme yapıyorsanız, üst öğe otomatik olarak etiketlenir.

## <a name="automatic-labeling-for-non-machine-learned-entities"></a>Makine tarafından öğrenilen varlıkların otomatik etiketlenmesi

Makine tarafından öğrenilen varlıklar, önceden oluşturulmuş varlıklar, normal ifade varlıkları, liste varlıkları ve model. herhangi bir varlık içerir. Bunlar, LULAR tarafından otomatik olarak etiketlendirildiklerinden, kullanıcılar tarafından el ile etiketlenmesi gerekmez.

## <a name="intent-prediction-errors"></a>Amaç tahmin hataları

Amaç tahmin hatası, geçerli eğitilen uygulama verilen, amaç için tahmin edilemeyen örnek olduğunu gösterir.

[Bu hataları](luis-how-to-add-intents.md#intent-prediction-errors) amaç ayrıntısı sayfasında görüntülemeyi öğrenin.

## <a name="entity-prediction-errors"></a>Varlık tahmin hataları

Varlık tahmin hataları, tahmin edilen varlığın etiketlenmiş varlıkla eşleşmediği anlamına gelebilir. Bu, utterance 'in yanında dikkatli bir göstergeyle görselleştirilir.

> [!div class="mx-imgBorder"]
> ![Makine öğrenimi varlığı için varlık paleti](media/label-utterances/example-utterance-indicates-prediction-error.png)

## <a name="next-steps"></a>Sonraki adımlar

Uygulamanızı tahmin kalitesini artırmak için [panoyu](luis-how-to-use-dashboard.md) kullanın ve [uç nokta utlerini gözden geçirin](luis-how-to-review-endpoint-utterances.md) .
