---
title: Haritaya Çokgen kalıp ekleme | Microsoft Azure haritaları
description: Microsoft Azure Maps web SDK 'sına Çokgen bir katman ekleme.
author: anastasia-ms
ms.author: v-stharr
ms.date: 10/08/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen, devx-track-js
ms.openlocfilehash: eedbbc0126adacc2a9bdc151aa6dbc27c7ba0750
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91310263"
---
# <a name="add-a-polygon-extrusion-layer-to-the-map"></a>Haritaya Çokgen kalıp ekleme

Bu makalede, Çokgen katman alanlarının `Polygon` ve `MultiPolygon` özellik geometrileri tarafından yükseltilmiş şekiller olarak işlenip nasıl kullanılacağı gösterilir. Azure Haritalar Web SDK 'Sı, [genişletilmiş GeoJSON şemasında](extend-geojson.md#circle)tanımlandığı şekilde daire geometrileri işlemesini destekler. Bu daireler haritada işlendiğinde çokgenlere dönüştürülebilir. Tüm özellik geometrileri, Atlas ile sarmalandıktan sonra kolayca güncelleştirilebilen olabilir [. Şekil](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape) sınıfı.

## <a name="use-a-polygon-extrusion-layer"></a>Çokgen katman kullan

[Çokgen kalıp katmanını](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.polygonextrusionlayer) bir veri kaynağına bağlayın. Daha sonra, bu dosyayı haritaya yükledi. Çokgen tabakası katmanı, bir `Polygon` ve `MultiPolygon` özelliklerinin ve özellikleri yükseltilmiş şekiller olarak işler. `height` `base` Çokgen katman katmanının ve özellikleri, yükseltme şeklinin zemin ve yüksekliğinden **Ölçü**cinsinden temel mesafeyi tanımlar. Aşağıdaki kod, bir çokgen oluşturmayı, bir veri kaynağına eklemeyi ve Çokgen katman sınıfını kullanarak nasıl işleneceğini gösterir.

> [!Note]
> `base`Çokgen katman katmanında tanımlanan değer, değerinden küçük veya buna eşit olmalıdır `height` .

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Yükseltilmiş Çokgen" src="https://codepen.io/azuremaps/embed/wvvBpvE?height=265&theme-id=0&default-tab=js,result&editable=true" frameborder='no' loading="lazy" allowtransparency="true" allowfullscreen="true">
Codepen 'da Azure Maps () ile kalem <a href='https://codepen.io/azuremaps/pen/wvvBpvE'>yükseltilmiş Çokgen</a> 'e bakın <a href='https://codepen.io/azuremaps'>@azuremaps</a> . <a href='https://codepen.io'>CodePen</a></iframe>


## <a name="add-data-driven-polygons"></a>Veri odaklı çokgenler ekleme

Bir choroptath eşlemesi, Çokgen katman kullanılarak oluşturulabilir. , `height` `fillColor` `Polygon` Ve özellik geometrileri içindeki istatistiksel değişkenin ölçüsünün ve yer aldığı katman özelliklerini ayarlayın `MultiPolygon` . Aşağıdaki kod örneğinde, durum ile popülasyon yoğunluğu ölçüsünün temelinde U. S ' nin yükseltilmiş bir choroptath Haritası gösterilmektedir.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Yükseltilmiş choroptath Haritası" src="https://codepen.io/azuremaps/embed/eYYYNox?height=265&theme-id=0&default-tab=result&editable=true" frameborder='no' loading="lazy" allowtransparency="true" allowfullscreen="true">
Codepen üzerinde Azure Maps () tarafından kullanılan kalem ile <a href='https://codepen.io/azuremaps/pen/eYYYNox'>harita yükseltme</a> <a href='https://codepen.io/azuremaps'>@azuremaps</a> . <a href='https://codepen.io'>CodePen</a>
</iframe>

## <a name="add-a-circle-to-the-map"></a>Haritaya daire ekleme

Azure Maps, coğrafi [olarak gösterildiği gibi](https://docs.microsoft.com/azure/azure-maps/extend-geojson#circle)daireler için bir tanım sağlayan geojson şemasının genişletilmiş bir sürümünü kullanır. Yükseltilmiş bir daire, özelliği ile bir özellik oluşturularak `point` `subType` ve bir `Circle` `Radius` yarıçapı **Ölçü**cinsinden temsil eden numaralandırılmış bir özellik oluşturarak haritada oluşturulabilir. Örneğin:

```javascript
{
    "type": "Feature",
    "geometry": {
        "type": "Point",
        "coordinates": [-105.203135, 39.664087]
    },
    "properties": {
        "subType": "Circle",
        "radius": 1000
    }
} 
```

Azure Haritalar Web SDK 'Sı, bu `Point` Özellikleri, yerleşik olarak `Polygon` bulunan özelliklere dönüştürür. Bu `Point` Özellikler, aşağıdaki kod örneğinde gösterildiği gibi Çokgen katman katmanı kullanılarak haritada oluşturulabilir.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Drone hava sahası Çokgen" src="https://codepen.io/azuremaps/embed/zYYYrxo?height=265&theme-id=0&default-tab=js,result&editable=true" frameborder='no' loading="lazy" allowtransparency="true" allowfullscreen="true">
Codepen üzerinde Azure Maps () ile kalem <a href='https://codepen.io/azuremaps/pen/zYYYrxo'>kuruta hava sahası poligonu</a> görüntüleyin <a href='https://codepen.io/azuremaps'>@azuremaps</a> . <a href='https://codepen.io'>CodePen</a>
</iframe>

## <a name="customize-a-polygon-extrusion-layer"></a>Çokgen kalıp katmanını özelleştirme

Çokgen katman katmanı çeşitli stil seçeneklerine sahiptir. İşte deneyebileceğiniz bir araç.

<br/>

<iframe height='700' scrolling='no' title='PoogBRJ' src='//codepen.io/azuremaps/embed/PoogBRJ/?height=700&theme-id=0&default-tab=result' frameborder='no' loading="lazy" allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Codepen 'da Azure Maps () tarafından oluşan kalemin <a href='https://codepen.io/azuremaps/pen/PoogBRJ/'>Poogbrj</a> bölümüne bakın <a href='https://codepen.io/azuremaps'>@azuremaps</a> . <a href='https://codepen.io'>CodePen</a>
</iframe>

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan sınıflar ve yöntemler hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Poligon](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.polygon)

> [!div class="nextstepaction"]
> [Çokgen kalıp katmanı](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.polygonextrusionlayer)

Ek kaynaklar:

> [!div class="nextstepaction"]
> [Azure haritalar GeoJSON belirtim uzantısı](extend-geojson.md#circle)
