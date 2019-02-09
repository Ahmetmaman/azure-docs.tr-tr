---
title: Azure haritalar ısı Haritası Katman Ekle | Microsoft Docs
description: Javascript haritaya ısı Haritası katman ekleme
author: rbrundritt
ms.author: richbrun
ms.date: 12/2/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 957ce60b8519ccb1e3287232f7a5459a56b25bb7
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55960624"
---
# <a name="add-a-heat-map-layer"></a>Isı haritası katmanı ekleme

Isı Haritaları, olarak da bilinen yoğunluklu haritalar'ın üzerine, veri görselleştirme bir renk aralığı kullanarak verileri yoğunluğu göstermek için kullanılan bir tür. Bunlar genellikle bir haritada "etkin nokta" verileri göstermek için kullanılırlar ve büyük işlemek için harika bir yoludur, veri kümeleri'nin üzerine olan.  Örneğin, on binlerce noktaları eşlemesi görünümündeki simgeler olarak işleme harita alanının çoğunu kapsar ve başkaları tarafından kadar veri anlayış kazanmak zorlaştıran kapsamında fazla sembol neden olur. Ancak, ısı Haritası olarak aynı bu veri kümesini görselleştirmenin noktası verileri densest ve diğer alanlarına göreli yoğunluklu nerede görmeyi kolaylaştırır. İçinde hangi ısı haritaları kullanılan birçok senaryo vardır. Bazı örnekler şunlardır;

* İçin iki veri noktaları arasındaki sıcaklık hangi anların sağladığından sıcaklık verilerini genellikle ısı Haritası olarak işlenir.
* Isı Haritası olarak gürültü algılayıcı verilerini işleme, yalnızca algılayıcı olduğu gürültü yoğunluğunu gösterir ancak bir yoldan dağıtımı Öngörüler de sağlayabilirsiniz. Ancak birçok gürültü kapsamı alanından çakışırsa, bu çakışan alan daha yüksek gürültü düzeyleri ile karşılaşabilir ve bu nedenle ısı Haritası da görünür olur mümkün olduğu herhangi bir sitede gürültü düzeyi yüksek, olmayabilir.
* Bir GPS görselleştirme her veri noktasının yoğunluğu hızına burada dayalı bir ağırlıklı yükseklik harita olarak hızını içeren izleme burada araç hızlandırmaya yönelik hızlı bir şekilde görmek için harika bir yoludur.

> [!TIP]
> Isı Haritası katmanları varsayılan olarak, bir veri kaynağındaki tüm geometriler koordinatlarını işlenir. Katman geometri özellikleri işler noktası yalnızca oluşturmak sınırlamak için `filter` katmana özelliği `['==', '$type', 'Point']`

## <a name="add-a-heat-map-layer"></a>Isı haritası katmanı ekleme

Veri kaynağı noktalarının ısı Haritası olarak işlemek için veri kaynağınızın bir örneğini geçirin `HeatMapLayer` sınıfı ve haritayı burada gösterildiği gibi ekleyin.

<br/>

<iframe height='500' scrolling='no' title='Basit ısı Haritası katman' src='//codepen.io/azuremaps/embed/gQqdQB/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/gQqdQB/'>basit ısı Haritası katman</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Bu örnekte, bir RADIUS hiç yakınlaştırma düzeyleri 10 piksel her ısı noktası var. Isı Haritası katmanı haritaya eklerken, bu örnek etiket katmanı ekler. Bu etiketleri ısı Haritası açıkça görünür olduğundan daha iyi bir kullanıcı deneyimi oluşturur. Bu örnekte veri kaynağı [USGS deprem riskleri Program](https://earthquake.usgs.gov/) ve son 30 günde gerçekleşen önemli deprem temsil eden noktaları oluşur.

## <a name="customizing-the-heat-map-layer"></a>Isı Haritası katman özelleştirme

Önceki örnekte RADIUS ve opaklık seçeneklerini ayarlayarak ısı Haritası özelleştirilmiş. Isı Haritası katman özelleştirme için çeşitli seçenekler sağlar.

* `radius`: Her veri noktası işlemek üzere bir piksel RADIUS tanımlar. RADIUS, sabit bir sayı veya bir ifade olarak ayarlanabilir. Bir ifade kullanarak (örneğin, 5 mil mesafede RADIUS) map tutarlı bir uzamsal alanı temsil görünen yakınlaştırma düzeyini temel alan RADIUS ölçeklendirmek mümkündür.
* `color`: Isı Haritası nasıl renkli belirtir. Gradyan renk paleti ısı haritaları için sık sık kullanılır, ancak basamaklı renk paletlerini ayrıca daha gibi contour veri görünümü, ısı Haritası yapmak istiyorsanız kullanışlıdır. Bu renk paletlerini en düşük renkleri en yüksek yoğunluklu değerine tanımlayın. Isı haritaları için renk değerleri belirtilen bir ifade olarak üzerinde `heatmap-density` değeri. Dizininde bir gradyan ifadesinde 0 rengi veya bir adım renk varsayılan rengi tanımlar renk alanının veri olduğu ve arka plan rengi tanımlamak için kullanılabilir. Bu değer saydam veya yarı saydam siyah ayarlamak birçok tercih eder. Renk ifadeleri örnekleri aşağıda verilmiştir;

| Gradyan Rengi ifadesi | Basamaklı rengi ifadesi | 
|---------------------------|--------------------------|
| \[<br/>&nbsp;&nbsp;&nbsp;&nbsp;'enterpolasyon'<br/>&nbsp;&nbsp;&nbsp;&nbsp;\['doğrusal'\],<br/>&nbsp;&nbsp;&nbsp;&nbsp;\['heatmap yoğunluklu'\],<br/>&nbsp;&nbsp;&nbsp;&nbsp;0, 'saydam'<br/>&nbsp;&nbsp;&nbsp;&nbsp;0,01, 'mor',<br/>&nbsp;&nbsp;&nbsp;&nbsp;0.5, '#fb00fb',<br/>&nbsp;&nbsp;&nbsp;&nbsp;1, '#00c3ff'<br/>\] | \[<br/>&nbsp;&nbsp;&nbsp;&nbsp;'adım'<br/>&nbsp;&nbsp;&nbsp;&nbsp;\['heatmap yoğunluklu'\],<br/>&nbsp;&nbsp;&nbsp;&nbsp;'saydam'<br/>&nbsp;&nbsp;&nbsp;&nbsp;0,01, 'Lacivert',<br/>&nbsp;&nbsp;&nbsp;&nbsp;0,25, 'Lacivert',<br/>&nbsp;&nbsp;&nbsp;&nbsp;0,50, 'yeşil',<br/>&nbsp;&nbsp;&nbsp;&nbsp;0.75, 'yellow',<br/>&nbsp;&nbsp;&nbsp;&nbsp;'red' 1,00<br/>\] |   

* `opacity`: Belirtir nasıl opak ya da şeffaf ısı Haritası katmanıdır.
* `intensity`: Bir çarpandan ısı Haritası genel yoğunluğunu artırmak için her veri noktasının ağırlık için geçerlidir. Veri noktaları ağırlığını küçük farklılıkları yapmak için bu yardımcı görselleştirmek daha kolay hale gelir.
* `weight`: Varsayılan olarak, tüm veri noktalarının yarısıyla 1 ağırlığa sahip, bu nedenle tüm veri noktaları eşit ağırlıklı. Ağırlık seçeneği çarpanı olarak görev yapar ve bir sayı veya bir ifade ayarlayın. Bir sayı ağırlığı ayarlanırsa, varsayalım, 2, bu nedenle yoğunluklu Katlama her veri noktası haritada iki kez yerleştirme eşdeğer olacaktır. Benzer şekilde işler ısı Haritası ağırlık seçeneği bir sayıya ayarlanması yoğunluğu seçeneğini kullanarak. Bir ifade kullanılırsa, Bununla birlikte, her veri noktasının ağırlık bazı ölçüm her veri noktasının özelliklerinde temel alabilir. Örneğin, her veri noktası Al deprem verileri, bir deprem temsil eder. Önemli bir ölçüm her deprem veri noktası varsa, büyüklük değerdir. Her zaman depremlerin gerçekleşir, ancak en düşük bir boyuta sahiptir ve hatta düşünmüştür olmayan. Her bir veri noktasına ağırlığı atamak için bir ifadede büyüklük değerini kullanarak içinde ısı Haritası daha iyi temsil edilmesini daha önemli deprem izin verir.
* Temel katman seçeneklerini; yanı sıra en düşük/en yüksek yakınlaştırma, görünür ve filtrelemek için ayrıca bir `source` veri kaynağını güncelleştirmek istiyorsanız seçeneği ve `source-layer` vektör kutucuk kaynak veri kaynağınız varsa seçeneği.

Burada farklı ısı Haritası Katman Seçenekleri test etmek için bir araçtır.

<br/>

<iframe height='700' scrolling='no' title='Isı Haritası Katman Seçenekleri' src='//codepen.io/azuremaps/embed/WYPaXr/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/WYPaXr/'>ısı Haritası Katman seçeneklerini</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

> [!TIP]
> Birbirine yakın noktaları, veri kaynağında kümelemeyi etkinleştirerek, kümelenmiş bir noktası olarak birlikte gruplandırılır. Her küme noktası sayısı ısı haritasındaki ağırlık deyim kullanılabilir ve olması işlemek zorunda noktalarının sayısını önemli ölçüde azaltabilir. Bir küme noktası sayısı depolanan bir `point_count property` aşağıda gösterildiği gibi noktası özelliği. 
> ```JavaScript
> var layer = new atlas.layer.HeatMapLayer(datasource, null, {
>    weight: ['get', 'point_count']
> });
> ```
> Kümeleme RADIUS yalnızca birkaç piksel var ise olması biraz visual fark oluşturma. Daha büyük bir RADIUS her kümesine daha fazla puan grup ve ısı Haritası performansını ancak sahip daha belirgin farklılıklar olacaktır.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan yöntemleri ve sınıfları hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [HeatMapLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarker?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [HeatMapLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.heatmaplayeroptions?view=azure-iot-typescript-latest)

Eşlenir eklemek daha fazla kod örnekleri için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Sembol katmanı Ekle](./map-add-pin.md)

