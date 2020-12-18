---
title: Azure Maps uzamsal GÇ modülünü kullanma | Microsoft Azure haritaları
description: Azure Haritalar Web SDK 'Sı tarafından sunulan uzamsal GÇ modülünü nasıl kullanacağınızı öğrenin. Bu modül, geliştiricilerin uzamsal verileri Azure Maps web SDK 'sı ile tümleştirmesini kolaylaştırmak için güçlü özellikler sağlar.
author: anastasia-ms
ms.author: v-stharr
ms.date: 02/28/2020
ms.topic: how-to
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.custom: devx-track-js
ms.openlocfilehash: cd64c80acceb1542c080fc45efbce59f287d448a
ms.sourcegitcommit: 66b0caafd915544f1c658c131eaf4695daba74c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2020
ms.locfileid: "97680687"
---
# <a name="how-to-use-the-azure-maps-spatial-io-module"></a>Azure haritalar uzamsal GÇ modülünü kullanma

Azure Haritalar Web SDK 'Sı, uzamsal verileri JavaScript veya TypeScript kullanarak Azure Maps web SDK 'Sı ile tümleştiren **uzamsal GÇ modülünü** sağlar. Bu modüldeki güçlü özellikler geliştiricilerin şunları yapmasına izin verir:

- [Sık kullanılan uzamsal veri dosyalarını okuyun ve yazın](spatial-io-read-write-spatial-data.md). Desteklenen dosya biçimleri şunlardır: KML, KMZ, GPX, GeoRSS, GML, GeoJSON ve uzamsal bilgiler içeren sütunlar içeren CSV dosyaları. Well-Known metin (WKT) de destekler.
- [Open Geospatial Consortium (OGC) hizmetlerine bağlanın ve Azure Maps web SDK ile tümleştirin. Harita üzerinde katman olarak Web harita Hizmetleri (WMS) ve Web harita kutucuk Hizmetleri 'ni (WMTS) kaplama](spatial-io-add-ogc-map-layer.md).
- [Verileri bir Web Özellik hizmetinde (WFS) sorgulama](spatial-io-connect-wfs-service.md).
- [Stil bilgisi içeren karmaşık veri kümelerini kaplama ve bunların en az kod kullanılarak otomatik olarak işlemesini](spatial-io-add-simple-data-layer.md)içerir.
- [Yüksek hızlı XML ve ayrılmış dosya okuyucusu ve yazıcı sınıflarından yararlanın](spatial-io-core-operations.md).

Bu kılavuzda, bir Web uygulamasında uzamsal GÇ modülünü tümleştirme ve kullanma hakkında bilgi edineceksiniz.

Bu videoda, Azure Maps web SDK 'sında uzamsal GÇ modülüne ilişkin bir genel bakış sunulmaktadır.

</br>

> [!VIDEO https://channel9.msdn.com/Shows/Internet-of-Things-Show/Easily-integrate-spatial-data-into-the-Azure-Maps/player?format=ny]

> [!WARNING]
> Yalnızca güvendiğiniz bir kaynaktan gelen veri ve Hizmetleri, özellikle de başka bir etki alanından başvuru yapıyorsanız kullanın. Uzamsal GÇ modülü, riski en aza indirmek için gereken adımları ele alır, ancak en güvenli yaklaşım, uygulamanıza yönelik herhangi bir desteklenmeyen veriye izin vermez. 

## <a name="prerequisites"></a>Ön koşullar

Uzamsal GÇ modülünü kullanabilmeniz için önce [bir Azure haritalar hesabı](./quick-demo-map-app.md#create-an-azure-maps-account) oluşturmanız ve [hesabınız için birincil abonelik anahtarını almanız](./quick-demo-map-app.md#get-the-primary-key-for-your-account)gerekir.

## <a name="installing-the-spatial-io-module"></a>Uzamsal GÇ modülünü yükleme

Azure Maps uzamsal GÇ modülünü iki seçenekten birini kullanarak yükleyebilirsiniz:

* Azure Maps uzamsal GÇ modülü için küresel olarak barındırılan Azure CDN. Bu seçenek için, HTML dosyasının öğesinde JavaScript 'e bir başvuru eklersiniz `<head>` .

    ```html
    <script src="https://atlas.microsoft.com/sdk/javascript/spatial/0/atlas-spatial.js"></script>
    ```

* [Azure-Maps-uzamsal-GÇ](https://www.npmjs.com/package/azure-maps-spatial-io) için kaynak kodu yerel olarak yüklenebilir ve sonra uygulamanız ile barındırılabilir. Bu paket TypeScript tanımlarını da içerir. Bu seçenek için, paketini yüklemek için aşağıdaki komutu kullanın:

    ```sh
    npm install azure-maps-spatial-io
    ```

    Daha sonra, HTML belgesi öğesindeki JavaScript 'e bir başvuru ekleyin `<head>` :

    ```html
    <script src="node_modules/azure-maps-spatial-io/dist/atlas-spatial.min.js"></script>
    ```

## <a name="using-the-spatial-io-module"></a>Uzamsal GÇ modülünü kullanma

1. Yeni bir HTML dosyası oluşturun.

2. Azure Haritalar Web SDK 'sını yükleyin ve harita denetimini başlatın. Ayrıntılar için bkz. [Azure Maps harita denetim](./how-to-use-map-control.md) Kılavuzu. Bu adımla işiniz bittiğinde, HTML dosyanız şuna benzemelidir:

    ```html
    <!DOCTYPE html>
    <html>

    <head>
        <title></title>

        <meta charset="utf-8">

        <!-- Ensures that IE and Edge uses the latest version and doesn't emulate an older version -->
        <meta http-equiv="x-ua-compatible" content="IE=Edge">

        <!-- Ensures the web page looks good on all screen sizes. -->
        <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

        <!-- Add references to the Azure Maps Map control JavaScript and CSS files. -->
        <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.css" type="text/css" />
        <script src="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.js"></script>

        <script type='text/javascript'>

            var map;

            function GetMap() {
                //Initialize a map instance.
                map = new atlas.Map('myMap', {
                    view: 'Auto',

                    //Add your Azure Maps subscription key to the map SDK. Get an Azure Maps key at https://azure.com/maps
                    authOptions: {
                        authType: 'subscriptionKey',
                        subscriptionKey: '<Your Azure Maps Key>'
                    }
                });

                //Wait until the map resources are ready.
                map.events.add('ready', function() {

                    // Write your code here to make sure it runs once the map resources are ready

                });
            }
        </script>
    </head>

    <body onload="GetMap()">
        <div id="myMap"></div>
    </body>

    </html>
    ```

2. Azure Maps uzamsal GÇ modülünü yükleyin. Bu alıştırmada, Azure Maps uzamsal GÇ modülü için CDN 'yi kullanın. Aşağıdaki başvuruyu, `<head>` HTML dosyanızın öğesine ekleyin:

    ```html
    <script src="https://atlas.microsoft.com/sdk/javascript/spatial/0/atlas-spatial.js"></script>
    ```

3. Bir başlatın `datasource` ve veri kaynağını haritaya ekleyin. Bir başlatın `layer` ve veri kaynağını harita katmanına ekleyin. Ardından, hem veri kaynağını hem de katmanını işleme. Sonraki adımda tam kodu görmek için aşağı kaydırmadan önce, veri kaynağını ve katman kodu parçacıklarını yerleştirmek için en iyi yerleri düşünün. Haritayı programlı bir şekilde işleyebilmemiz için, eşleme kaynağı hazırlanana kadar beklemeniz gerektiğini hatırlayın.

    ```javascript
    var datasource, layer;
    ```

    ve

    ```javascript
    //Create a data source and add it to the map.
    datasource = new atlas.source.DataSource();
    map.sources.add(datasource);
    
    //Add a simple data layer for rendering the data.
    layer = new atlas.layer.SimpleDataLayer(datasource);
    map.layers.add(layer);
    ```

4. Hepsini birlikte koymak, HTML kodunuzun aşağıdaki kod gibi görünmesi gerekir. Bu örnek, bir URL 'den bir XML dosyasının nasıl okunacağını gösterir. Ardından, dosyanın özellik verilerini haritada yükleyin ve görüntüleyin. 

    ```html
    <!DOCTYPE html>
    <html>

    <head>
        <title>Spatial IO Module Example</title>

        <meta charset="utf-8">

        <!-- Ensures that IE and Edge uses the latest version and doesn't emulate an older version -->
        <meta http-equiv="x-ua-compatible" content="IE=Edge">

        <!-- Ensures the web page looks good on all screen sizes. -->
        <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

        <!-- Add references to the Azure Maps Map control JavaScript and CSS files. -->
        <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.css" type="text/css" />
        <script src="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.js"></script>

        <!-- Add reference to the Azure Maps Spatial IO module. -->
        <script src="https://atlas.microsoft.com/sdk/javascript/spatial/0/atlas-spatial.js"></script>

        <script type='text/javascript'>
            var map, datasource, layer;

            function GetMap() {
                //Initialize a map instance.
                map = new atlas.Map('myMap', {
                    view: 'Auto',

                    //Add your Azure Maps subscription key to the map SDK. Get an Azure Maps key at https://azure.com/maps
                    authOptions: {
                        authType: 'subscriptionKey',
                        subscriptionKey: '<Your Azure Maps Key>'
                    }
                });

                //Wait until the map resources are ready.
                map.events.add('ready', function() {

                    //Create a data source and add it to the map.
                    datasource = new atlas.source.DataSource();
                    map.sources.add(datasource);

                    //Add a simple data layer for rendering the data.
                    layer = new atlas.layer.SimpleDataLayer(datasource);
                    map.layers.add(layer);

                    //Read an XML file from a URL or pass in a raw XML string.
                    atlas.io.read('superCoolKmlFile.xml').then(r => {
                        if (r) {
                            //Add the feature data to the data source.
                            datasource.add(r);

                            //If bounding box information is known for data, set the map view to it.
                            if (r.bbox) {
                                map.setCamera({
                                    bounds: r.bbox,
                                    padding: 50
                                });
                            }
                        }
                    });
                });
            }
        </script>
    </head>

    <body onload="GetMap()">
        <div id="myMap"></div>
    </body>

    </html>
    ```

5. `<Your Azure Maps Key>`Birincil anahtarınızla değiştirmeyi unutmayın. HTML dosyanızı açın ve aşağıdaki görüntüye benzer sonuçlar görürsünüz:

    <center>

    ![Uzamsal veri örneği](./media/how-to-use-spatial-io-module/spatial-data-example.png)

    </center>

## <a name="next-steps"></a>Sonraki adımlar

Burada gösterilen özellik, uzamsal GÇ modülünde kullanılabilen birçok özellikten yalnızca biridir. Uzamsal GÇ modülündeki diğer işlevleri nasıl kullanacağınızı öğrenmek için aşağıdaki kılavuzlardan okuyun:

> [!div class="nextstepaction"]
> [Basit bir veri katmanı ekleme](spatial-io-add-simple-data-layer.md)

> [!div class="nextstepaction"]
> [Uzamsal verileri okuma ve yazma](spatial-io-read-write-spatial-data.md)

> [!div class="nextstepaction"]
> [OGC harita katmanı ekleme](spatial-io-add-ogc-map-layer.md)

> [!div class="nextstepaction"]
> [WFS hizmetine bağlanma](spatial-io-connect-wfs-service.md)

> [!div class="nextstepaction"]
> [Çekirdek işlemlerden yararlanma](spatial-io-core-operations.md)

> [!div class="nextstepaction"]
> [Desteklenen veri biçimi ayrıntıları](spatial-io-supported-data-format-details.md)

Azure Maps uzamsal GÇ belgelerine başvurun:

> [!div class="nextstepaction"]
> [Azure haritalar uzamsal GÇ paketi](/javascript/api/azure-maps-spatial-io/)