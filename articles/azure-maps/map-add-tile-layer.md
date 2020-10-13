---
title: Haritaya kutucuk katmanı ekleme | Microsoft Azure haritaları
description: Haritalar üzerinde görüntü oluşturmayı öğrenin. Bir haritaya Hava durumu radar kaplaması içeren bir kutucuk katmanı eklemek için Azure Maps web SDK 'sını kullanan bir örneğe bakın.
author: rbrundritt
ms.author: richbrun
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen, devx-track-js
ms.openlocfilehash: 8450fbd5287c88431f21753a9c88e8603455f844
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91310433"
---
# <a name="add-a-tile-layer-to-a-map"></a>Haritaya kutucuk katmanı ekleme

Bu makalede, haritada kutucuk katmanının nasıl yer aldığı gösterilmektedir. Döşeme katmanları, Azure Maps temel harita kutucuklarının üzerine görüntü eklemenize olanak tanır. Azure haritalar döşeme sistemi hakkında daha fazla bilgi için bkz. [yakınlaştırma düzeyleri ve kutucuk Kılavuzu](zoom-levels-and-tile-grid.md).

Döşeme katmanı bir sunucudan kutucukları yükler. Bu görüntüler önceden işlenmiş ya da dinamik olarak işlenmiş olabilir. Önceden işlenmiş görüntüler, kutucuk katmanının anladığı bir adlandırma kuralını kullanarak bir sunucu üzerindeki diğer herhangi bir görüntü gibi saklanır. Dinamik olarak işlenen görüntüler görüntüleri gerçek zamanlı olarak yakın bir şekilde yüklemek için bir hizmet kullanır. Azure haritalar [Tilelayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.tilelayer) sınıfı tarafından desteklenen üç farklı kutucuk hizmeti adlandırma kuralı vardır: 

* X, Y, zoom gösterimi-X sütundur, Y, kutucuk kılavuzundaki döşemenin satır konumudur ve yakınlaştırma düzeyi yakınlaştırma düzeyine göre bir değer gösterimi.
* Quadkey gösterimi-x, y ve zoom bilgilerini tek bir dize değeri olarak birleştirir. Bu dize değeri, tek bir kutucuk için benzersiz bir tanımlayıcı haline gelir.
* Sınırlayıcı kutusu-sınırlayıcı kutu koordinatları biçiminde bir görüntü belirtin: `{west},{south},{east},{north}` . Bu biçim genellikle [Web eşleme Hizmetleri (WMS)](https://www.opengeospatial.org/standards/wms)tarafından kullanılır.

> [!TIP]
> Bir [Tilelayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.tilelayer) , haritada büyük veri kümelerini görselleştirmenin harika bir yoludur. Bir görüntüden yalnızca kutucuk katmanı oluşturulmayabilir, vektör verileri de kutucuk katmanı olarak da oluşturulabilir. Vektör verilerini kutucuk katmanı olarak işleyerek, Map Control yalnızca dosya boyutunda daha küçük olan kutucukları, temsil ettikleri vektör verilerinden yüklemek için gerekir. Bu teknik, haritada milyonlarca veri satırını işlemek için yaygın olarak kullanılır.

Döşeme katmanına geçirilen kutucuk URL 'si, bir TileJSON kaynağına yönelik bir http veya https URL 'SI ya da aşağıdaki parametreleri kullanan bir kutucuk URL şablonu olmalıdır: 

* `{x}` -X kutucuğunun konumu. Ayrıca `{y}` , ve gerektirir `{z}` .
* `{y}` -Kutucuğun Y konumu. Ayrıca `{x}` , ve gerektirir `{z}` .
* `{z}` -Kutucuğun yakınlaştırma düzeyi. Ayrıca `{x}` , ve gerektirir `{y}` .
* `{quadkey}` -Bing Haritalar kutucuk sistem adlandırma kuralına bağlı olarak, quadkey tanımlayıcısını Döşe.
* `{bbox-epsg-3857}` - `{west},{south},{east},{north}` EPSG 3857 uzamsal başvuru sisteminde biçim içeren bir sınırlayıcı kutu dizesi.
* `{subdomain}` -Belirtilmişse, alt etki alanı değerleri için bir yer tutucu `subdomain` eklenir.
* `{azMapsDomain}` -Etki alanını ve kutucuk isteklerinin kimlik doğrulamasını harita tarafından kullanılan değerlerle aynı olacak şekilde hizalamak için bir yer tutucudur.

## <a name="add-a-tile-layer"></a>Kutucuk katmanı ekleme

 Bu örnek, bir kutucuk kümesini işaret eden döşeme katmanının nasıl oluşturulacağını gösterir. Bu örnek x, y, yakınlaştırma Döşeme sistemini kullanır. Bu kutucuk katmanının kaynağı, [Iowa çevresel Mesonet 'in Iowa çevre](https://mesonet.agron.iastate.edu/ogc/)bir hava durumu radar kaplamasıyla. Radar verileri görüntülerken, ideal kullanıcılar haritada gezindikleri gibi şehirlerin etiketlerini açıkça görürler. Bu davranış, katmanın altına döşeme katmanı eklenerek uygulanabilir `labels` .

```javascript
//Create a tile layer and add it to the map below the label layer.
//Weather radar tiles from Iowa Environmental Mesonet of Iowa State University.
map.layers.add(new atlas.layer.TileLayer({
    tileUrl: 'https://mesonet.agron.iastate.edu/cache/tile.py/1.0.0/nexrad-n0q-900913/{z}/{x}/{y}.png',
    opacity: 0.8,
    tileSize: 256
}), 'labels');
```

Aşağıda, yukarıdaki işlevselliğin tamamen çalışan kod örneği verilmiştir.

<br/>

<iframe height='500' scrolling='no' title='X, Y ve Z kullanarak döşeme katmanı' src='//codepen.io/azuremaps/embed/BGEQjG/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' loading="lazy" allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Codepen 'da <a href='https://codepen.io/azuremaps/pen/BGEQjG/'>X, Y ve Z</a> Ile Azure Maps () kullanarak kalem döşeme katmanına bakın <a href='https://codepen.io/azuremaps'>@azuremaps</a> . <a href='https://codepen.io'>CodePen</a>
</iframe>

## <a name="customize-a-tile-layer"></a>Döşeme katmanını özelleştirme

Döşeme katmanı sınıfının birçok stil seçeneği vardır. İşte deneyebileceğiniz bir araç.

<br/>

<iframe height='700' scrolling='no' title='Döşeme katmanı seçenekleri' src='//codepen.io/azuremaps/embed/xQeRWX/?height=700&theme-id=0&default-tab=result' frameborder='no' loading="lazy" allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Codepen 'da Azure Maps () tarafından bulunan kalem <a href='https://codepen.io/azuremaps/pen/xQeRWX/'>kutucuğu katman seçeneklerine</a> bakın <a href='https://codepen.io/azuremaps'>@azuremaps</a> <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan sınıflar ve yöntemler hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [TileLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.tilelayer)

> [!div class="nextstepaction"]
> [TileLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.tilelayeroptions)

Haritalarınıza eklemek için daha fazla kod örneği için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Görüntü katmanı ekleme](./map-add-image-layer.md)
