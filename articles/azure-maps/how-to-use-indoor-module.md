---
title: Azure Maps ınkapısı haritaları modülünü Microsoft Creator Hizmetleri (Önizleme) ile kullanma
description: Modülün JavaScript kitaplıklarını gömerek haritaları işlemek için Microsoft Azure Maps ınkapısı haritaları modülünü kullanmayı öğrenin.
author: anastasia-ms
ms.author: v-stharr
ms.date: 07/20/2020
ms.topic: how-to
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.custom: devx-track-js
ms.openlocfilehash: e527cf5fa6a7caaeaf56ea19d684dd0830d5ca8a
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "101708688"
---
# <a name="use-the-azure-maps-indoor-maps-module"></a>Azure Maps ınkapısı haritaları modülünü kullanma

> [!IMPORTANT]
> Azure haritalar Creator Hizmetleri şu anda genel önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Azure Haritalar Web SDK 'Sı, *Azure Maps ınkapısı* modülünü içerir. *Azure Maps ınkapısı* modülü, Azure Maps Oluşturucu hizmetlerinde (Önizleme) oluşturulan ınkapıharitaları oluşturmanıza olanak sağlar 

## <a name="prerequisites"></a>Önkoşullar

1. [Azure haritalar hesabı oluşturma](quick-demo-map-app.md#create-an-azure-maps-account)
2. [Oluşturucu (Önizleme) kaynağı oluşturma](how-to-manage-creator.md)
3. Birincil anahtar veya abonelik anahtarı olarak da bilinen [birincil bir abonelik anahtarı alın](quick-demo-map-app.md#get-the-primary-key-for-your-account).
4. `tilesetId` `statesetId` [Inkapılı haritalar oluşturma öğreticisini](tutorial-creator-indoor-maps.md)tamamlayarak bir ve, alın.
 Bu tanımlayıcıları Azure Maps ınkapısı haritaları modülü ile birlikte iç eşlemeleri işlemek için kullanmanız gerekir.

## <a name="embed-the-indoor-maps-module"></a>Inkapı haritaları modülünü katıştırma

*Azure Maps ınkapısı* modülünü iki şekilde yükleyebilir ve ekleyebilirsiniz.

*Azure Maps ınkapısı* modülünün genel olarak barındırılan Azure Content Delivery Network sürümünü kullanmak IÇIN, HTML dosyasının öğesinde aşağıdaki JavaScript ve stil sayfası başvurularına başvurun `<head>` :

```html
<link rel="stylesheet" href="https://atlas.microsoft.com/sdk/javascript/indoor/0.1/atlas-indoor.min.css" type="text/css"/>
<script src="https://atlas.microsoft.com/sdk/javascript/indoor/0.1/atlas-indoor.min.js"></script>
```

 Ya da *Azure Maps ınkapıl* modülünü de indirebilirsiniz. *Azure Maps ınkapısı* modülü, Azure Maps hizmetlerine erişmek için bir istemci kitaplığı içerir. *Inkapısı* modülünü yüklemek ve Web uygulamanıza yüklemek için aşağıdaki adımları izleyin.  
  
  1. [Azure-Maps-ınkapısı paketini](https://www.npmjs.com/package/azure-maps-indoor)yükler.
  
      ```powershell
      >npm install azure-maps-indoor
      ```

  2. HTML dosyasının öğesindeki *Azure Maps ınkapısı* modül JavaScript ve stil sayfasına başvurun `<head>` :

      ```html
      <link rel="stylesheet" href="node_modules/azure-maps-drawing-tools/dist/atlas-indoor.min.css" type="text/css" />
      <script src="node_modules/azure-maps-drawing-tools/dist/atlas-indoor.min.js"></script>
      ```

## <a name="instantiate-the-map-object"></a>Map nesnesinin örneğini oluşturma

İlk olarak bir *Map nesnesi* oluşturun. *Map nesnesi* , *ınkapılı Manager* nesnesinin örneğini oluşturmak için bir sonraki adımda kullanılacaktır.  Aşağıdaki kod, *Map nesnesinin* örneğini oluşturmayı gösterir:

```javascript
const subscriptionKey = "<Your Azure Maps Primary Subscription Key>";

const map = new atlas.Map("map-id", {
  //use your facility's location
  center: [-122.13203, 47.63645],
  //or, you can use bounds: [# west, # south, # east, # north] and replace # with your map's bounds
  style: "blank",
  view: 'Auto',
  authOptions: {
      authType: 'subscriptionKey',
      subscriptionKey: subscriptionKey
  },
  zoom: 19,
});
```

## <a name="instantiate-the-indoor-manager"></a>Inkapılı yöneticinin örneğini oluşturma

Kutucukların ınkapaklı tilesets ve Map stilini yüklemek için, *ınkapılı yöneticinin* örneğini oluşturmanız gerekir. *Map nesnesini* ve karşılık gelen öğesini sağlayarak *ınkapısı yöneticisinin* örneğini oluşturun `tilesetId` . [Dinamik harita](indoor-map-dynamic-styling.md)stilini desteklemek istiyorsanız, öğesini geçirmeniz gerekir `statesetId` . `statesetId`Değişken adı büyük/küçük harfe duyarlıdır. Kodunuzun aşağıdaki JavaScript gibi olması gerekir.

```javascript
const tilesetId = "<tilesetId>";
const statesetId = "<statesetId>";

const indoorManager = new atlas.indoor.IndoorManager(map, {
    tilesetId: tilesetId,
    statesetId: statesetId // Optional
});
```

Sağladığınız durum verilerinin yoklanmasını etkinleştirmek için `statesetId` ve çağrısını sağlamanız gerekir `indoorManager.setDynamicStyling(true)` . Yoklama durumu verileri dinamik özelliklerin veya *durumların* durumunu dinamik olarak güncelleştirmenize olanak tanır. Örneğin, oda gibi bir özelliğin adlı dinamik bir özelliği (*durum*) olabilir `occupancy` . Uygulamanız, görsel harita içindeki değişikliği yansıtacak şekilde herhangi bir *durum* değişikliğini yoklamak isteyebilir. Aşağıdaki kod, durum yoklamasını nasıl etkinleştireceğinizi göstermektedir:

```javascript
const tilesetId = "<tilesetId>";
const statesetId = "<statesetId>";

const indoorManager = new atlas.indoor.IndoorManager(map, {
    tilesetId: tilesetId,
    statesetId: statesetId // Optional
});

if (statesetId.length > 0) {
    indoorManager.setDynamicStyling(true);
}
```

## <a name="indoor-level-picker-control"></a>Inkapısı düzeyi seçici denetimi

 *Inkapıdüzeyi seçici* denetimi, işlenmiş haritanın düzeyini değiştirmenize izin verir. Inkapılı Yönetim *Seçicisi* denetimini *ınkapısı Yöneticisi* aracılığıyla isteğe bağlı olarak başlatabilirsiniz. Aşağıda, düzey denetim seçiciyi başlatmak için kod verilmiştir:

```javascript
const levelControl = new atlas.control.LevelControl({ position: "top-right" });
indoorManager.setOptions({ levelControl });
```

## <a name="indoor-events"></a>Inkapılı olaylar

 *Azure Maps ınkapısı* modülü *harita nesnesi* olaylarını destekler. *Harita nesnesi* olay dinleyicileri, bir düzey veya tesis değiştirildiğinde çağrılır. Bir düzey veya tesis değiştirildiğinde kodu çalıştırmak istiyorsanız, kodunuzu olay dinleyicisine koyun. Aşağıdaki kod, *eşleme nesnesine* olay dinleyicilerinin nasıl ekleneceğini gösterir.

```javascript
map.events.add("levelchanged", indoorManager, (eventData) => {

  //code that you want to run after a level has been changed
  console.log("The level has changed: ", eventData);
});

map.events.add("facilitychanged", indoorManager, (eventData) => {

  //code that you want to run after a facility has been changed
  console.log("The facility has changed: ", eventData);
});
```

`eventData`Değişken, `levelchanged` sırasıyla veya olayını çağıran düzey veya tesis hakkında bilgiler içerir `facilitychanged` . Bir düzey değiştiğinde `eventData` nesne, `facilityId` Yeni `levelNumber` ve diğer meta verileri içerir. Bir tesis değiştiğinde, `eventData` nesne yeni, `facilityId` Yeni `levelNumber` ve diğer meta verileri içerir.

## <a name="example-use-the-indoor-maps-module"></a>Örnek: ınkapımaps modülünü kullanma

Bu örnek, Web uygulamanızda *Azure Maps ınkapısı* modülünü nasıl kullanacağınızı gösterir. Örnek kapsamında sınırlı olsa da, *Azure Maps ınkapısı* modülünü kullanmaya başlamak için gerekenler hakkında temel bilgiler ele alınmaktadır. Tüm HTML kodu, bu adımların altınlardır.

1. Azure *Maps ınkapısı* modülünü yüklemek için Azure Content Delivery Network [seçeneğini](#embed-the-indoor-maps-module) kullanın.

2. Yeni bir HTML dosyası oluştur

3. HTML üstbilgisinde *Azure Maps ınkapısı* modülü JavaScript ve stil sayfası stillerine başvurun.

4. *Harita nesnesi* başlatın. *Map nesnesi* aşağıdaki seçenekleri destekler:
    - `Subscription key` Azure haritalar birincil abonelik anahtarınız.
    - `center` ınkapıeşlem merkezi konumunuz için bir enlem ve Boylam tanımlar. `center`İçin bir değer sağlamak istemiyorsanız, için bir değer girin `bounds` . Biçim şöyle görünmelidir `center` : [-122,13315, 47,63637].
    - `bounds` , tileset eşleme verilerini kapsayan en küçük dikdörtgen şekildir. `bounds`İçin bir değer ayarlamak istemiyorsanız, için bir değer ayarlayın `center` . [Tileset LISTE API](/rest/api/maps/tileset/listpreview)'sini çağırarak harita sınırlarınızı bulabilirsiniz. Tileset API 'SI, `bbox` ayrıştırılabilir ve atayabileceğiniz öğesini döndürür `bounds` . Biçim şöyle görünmelidir `bounds` : [# Batı, # Güney, # Doğu, # Kuzey].
    - `style` arka planın rengini ayarlamanıza olanak sağlar. Beyaz bir arka planı göstermek için `style` "boş" olarak tanımlayın.
    - `zoom` Haritanız için en düşük ve en yüksek yakınlaştırma düzeylerini belirtmenize olanak tanır.

5. Sonra, *ınkapılı yönetici* modülünü oluşturun. *Azure Maps ınkapısı* atayın `tilesetId` ve isteğe bağlı olarak öğesini ekleyin `statesetId` .

6. *Inkapısı düzeyi seçici denetimini örnekleyin* .

7. *Harita nesnesi* olay dinleyicileri ekleyin.  

Dosyanız artık aşağıdaki HTML 'ye benzer şekilde görünmelidir.

  ```html
  <!DOCTYPE html>
  <html lang="en">
    <head>
      <meta charset="utf-8" />
      <meta name="viewport" content="width=device-width, user-scalable=no" />
      <title>Indoor Maps App</title>
      
      <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.css" type="text/css" />
      <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/javascript/indoor/0.1/atlas-indoor.min.css" type="text/css"/>

      <script src="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.js"></script>
      <script src="https://atlas.microsoft.com/sdk/javascript/indoor/0.1/atlas-indoor.min.js"></script>
        
      <style>
        html,
        body {
          width: 100%;
          height: 100%;
          padding: 0;
          margin: 0;
        }

        #map-id {
          width: 100%;
          height: 100%;
        }
      </style>
    </head>

    <body>
      <div id="map-id"></div>
      <script>
        const subscriptionKey = "<Your Azure Maps Primary Subscription Key>";
        const tilesetId = "<your tilesetId>";
        const statesetId = "<your statesetId>";

        const map = new atlas.Map("map-id", {
          //use your facility's location
          center: [-122.13315, 47.63637],
          //or, you can use bounds: [# west, # south, # east, # north] and replace # with your Map bounds
          style: "blank",
          view: 'Auto',
          authOptions: { 
              authType: 'subscriptionKey',
              subscriptionKey: subscriptionKey
          },
          zoom: 19,
        });

        const levelControl = new atlas.control.LevelControl({
          position: "top-right",
        });

        const indoorManager = new atlas.indoor.IndoorManager(map, {
          levelControl: levelControl, //level picker
          tilesetId: tilesetId,
          statesetId: statesetId // Optional
        });

        if (statesetId.length > 0) {
          indoorManager.setDynamicStyling(true);
        }

        map.events.add("levelchanged", indoorManager, (eventData) => {
          //put code that runs after a level has been changed
          console.log("The level has changed:", eventData);
        });

        map.events.add("facilitychanged", indoorManager, (eventData) => {
          //put code that runs after a facility has been changed
          console.log("The facility has changed:", eventData);
        });
      </script>
    </body>
  </html>
  ```

Inkapısı eşlemenizi görmek için bir Web tarayıcısına yükleyin. Aşağıdaki görüntüde olduğu gibi görünmelidir. Staırwell özelliğine tıklarsanız, *düzey seçici* sağ üst köşede görünür.

  ![ınkapısı eşleme resmi](media/how-to-use-indoor-module/indoor-map-graphic.png)

[Canlı tanıtımı gör](https://azuremapscodesamples.azurewebsites.net/?sample=Creator%20indoor%20maps)

## <a name="next-steps"></a>Sonraki adımlar

*Azure Maps ınkapılı* modülüyle Ilgili API 'ler hakkında bilgi edinin:

> [!div class="nextstepaction"]
> [Çizim paketi gereksinimleri](drawing-requirements.md)

>[!div class="nextstepaction"]
> [Inkapı haritaları için Oluşturucu (Önizleme)](creator-indoor-maps.md)

Haritalarınız için daha fazla veri ekleme hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Inkapı haritaları dinamik stil oluşturma](indoor-map-dynamic-styling.md)

> [!div class="nextstepaction"]
> [Kod örnekleri](/samples/browse/?products=azure-maps)