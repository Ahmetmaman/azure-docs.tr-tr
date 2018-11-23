---
title: Video Indexer pencere öğeleri uygulamalarınıza ekleyin
titlesuffix: Azure Media Services
description: Uygulamalarınıza Video Indexer pencere öğeleri eklemeyi öğrenin.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.topic: article
ms.date: 11/19/2018
ms.author: juliako
ms.openlocfilehash: a051f40cb5586cae58d8e4939f4fcee35438bf69
ms.sourcegitcommit: beb4fa5b36e1529408829603f3844e433bea46fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2018
ms.locfileid: "52292518"
---
# <a name="embed-video-indexer-widgets-into-your-applications"></a>Video Indexer pencere öğeleri uygulamalarınıza ekleyin

Bu makalede, uygulamalarınıza Video Indexer pencere öğelerini nasıl ekleyebileceğiniz gösterilmektedir. Video Indexer, uygulamanıza iki tür pencere öğesinin eklenmesini destekler: **Bilişsel İçgörüler** ve **Yürütücü**. 
## <a name="widget-types"></a>Pencere öğesi türleri

### <a name="cognitive-insights-widget"></a>Bilişsel İçgörüler pencere öğesi

**Bilişsel İçgörüler** pencere öğeleri, video dizini oluşturma işleminizden elde edilen tüm görsel içgörüleri içerir. İçgörüler pencere öğesi, aşağıdaki isteğe bağlı URL parametrelerini destekler:

|Ad|Tanım|Açıklama|
|---|---|---|
|widgets|Virgülle ayrılmış dizeler|İşlemek istediğiniz öngörüleri denetlemenize olanak sağlar. <br/>Örnek: `https://www.videoindexer.ai/embed/insights/<accountId>/<videoId>/?widgets=people,search` yalnızca kişiler ve markalarla ilgili kullanıcı arabirimi içgörülerini işler<br/>Mevcut seçenekler: people (kişiler), keywords (anahtar sözcükler), annotations (ek açıklamalar), brands (markalar), sentiments (yaklaşımlar), transcript (transkript), search (arama).<br/>version=2’de URL aracılığıyla desteklenmez<br/><br/>**Not:** **version=2** kullanılırsa **widgets** URL parametresi desteklenmez. |
|version|**Bilişsel İçgörüler** pencere öğesinin sürümleri|En son içgörüler pencere öğesi güncelleştirmelerini almak için ekleme URL'sine `?version=2` sorgu parametresini ekleyin. Örneğin, `https://www.videoindexer.ai/embed/insights/<accountId>/<videoId>/?version=2` <br/> Eski sürümü elde etmek için URL'den `version=2` parametresini kaldırın.

### <a name="player-widget"></a>Yürütücü pencere öğesi

**Yürütücü** pencere öğeleri, bit hızı uyarlamalı video akışı yapmanıza olanak tanır. Yürütücü pencere öğesi, aşağıdaki isteğe bağlı URL parametrelerini destekler:

|Ad|Tanım|Açıklama|
|---|---|---|
|t|Başlangıçtan itibaren saniye sayısı|Yürütücünün dosyayı zamanda belirtilen noktadan itibaren yürütmeye başlamasını sağlar.<br/>Örnek: t=60|
|captions|Dil kodu|Pencere öğesi yüklenirken açıklamalı alt yazıyı belirtilen dilde getirerek açıklamalı alt yazı menüsünde mevcut olmasını sağlar.<br/>Örnek: captions=en-US|
|showCaptions|Bir boole değeri|Yürütücünün etkin olan açıklamalı alt yazıları yüklemesini sağlar.<br/>Örnek: showCaptions=true|
|type||Bir ses yürütücüsü dış görünümünü etkinleştirir (video bölümü kaldırılır).<br/>Örnek: type=audio|
|autoplay|Bir boole değeri|Yürütücünün, yüklendiğinde videoyu oynatmaya başlatıp başlatmayacağını gösterir (varsayılan: true).<br/>Örnek: autoplay=false|
|language|Dil kodu|Yürütücünün dilini denetler (varsayılan: en-US)<br/>Örnek: language=de-DE|

## <a name="embedding-public-content"></a>Genel içerik ekleme

1. [Video Indexer](https://www.videoindexer.ai/) web sitesine gidip oturum açın.
2. Çalışmak istediğiniz videoya tıklayın.
3. Videonun altında görünen "ekle" düğmesine tıklayın.

    ![Pencere öğesi](./media/video-indexer-embed-widgets/video-indexer-widget01.png)

    Düğmeye tıkladığınızda ekranda kalıcı bir ekleme öğesi görünür. Burada uygulamanıza eklemek istediğiniz pencere öğesini seçebilirsiniz.
    Bir pencere öğesi seçtiğinizde (**Yürütücü** veya **Bilişsel İçgörüler**), uygulamanıza yapıştıracağınız ekleme kodu oluşturulur.
 
4. İstediğiniz pencere öğesi türünü seçin (**Bilişsel İçgörüler** veya **Yürütücü**).
5. Ekleme kodunu kopyalayın ve uygulamanıza ekleyin. 

    ![Pencere öğesi](./media/video-indexer-embed-widgets/video-indexer-widget02.png)

## <a name="embedding-private-content"></a>Özel içerik ekleme

Ekleme kodlarını açılan ekleme pencerelerinden (önceki bölümde gösterildiği gibi) yalnızca **Genel** videolar için alabilirsiniz. 

**Özel** bir video eklemek istiyorsanız **iframe**'in **src** özniteliğinde bir erişim belirteci geçirmeniz gerekir:

     https://www.videoindexer.ai/embed/[insights | player]/<accountId>/<videoId>/?accessToken=<accessToken>
    
[**İçgörüler Pencere Öğesini Al**](https://api-portal.videoindexer.ai/docs/services/operations/operations/Get-insights-widget?) API’sini kullanarak Bilişsel İçgörüler pencere öğesi içeriği alın ya da [**Video Erişim Belirtecini Al**](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Video-Access-Token?)’ı kullanarak bu belirteci yukarıda gösterildiği gibi URL’ye sorgu parametresi olarak ekleyin. Bu URL’yi **iframe**'in **src** değeri olarak belirtin.

Ekli pencere öğenizde içgörü düzenleme özellikleri sağlamak istiyorsanız (web uygulamamızda olduğu gibi) düzenleme izinlerine sahip bir erişim belirteci geçirmeniz gerekir. [**İçgörüler Pencere Öğesini Al**](https://api-portal.videoindexer.ai/docs/services/operations/operations/Get-insights-widget?) veya [**Video Erişim Belirtecini Al**](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Video-Access-Token?) ile **&allowEdit=true** parametre değerini kullanın. 

## <a name="widgets-interaction"></a>Pencere öğeleri etkileşimi

**Bilişsel İçgörüler** pencere öğesi, uygulamanızda bir video ile etkileşim kurabilir. Bu bölümde, bu etkileşimi nasıl elde sağlayabileceğiniz gösterilmektedir.

![Pencere öğesi](./media/video-indexer-embed-widgets/video-indexer-widget03.png)

### <a name="cross-origin-communications"></a>Kaynak noktalar arası iletişim

Video Indexer hizmeti, Video Indexer pencere öğelerinin diğer bileşenler ile iletişim kurmasını sağlamak için şunları yapar:

- Kaynak noktalar arası iletişim HTML5 yöntemi **postMessage**’ı kullanır ve 
- İletiyi VideoIndexer.ai kaynağında doğrular. 

Kendi yürütücü kodunuzu uygulayıp **Bilişsel İçgörüler** pencere öğeleri ile tümleştirmeye karar verirseniz VideoIndexer.ai’den gelen iletinin kaynağını doğrulamak sizin sorumluluğunuzdadır.

### <a name="embed-both-types-of-widgets-in-your-application--blog-recommended"></a>Uygulamanıza / blogunuza her iki tür pencere öğesini de ekleyin (önerilir) 

Bu bölümde, kullanıcı uygulamanızda içgörü denetimine tıkladığında yürütücünün ilgili noktaya atlamasını sağlayacak şekilde iki Video Indexer pencere öğesi arasında etkileşim sağlanması açıklanmaktadır.

    <script src="https://breakdown.blob.core.windows.net/public/vb.widgets.mediator.js"></script> 

1. **Yürütücü** pencere öğesi ekleme kodunu kopyalayın.
2. **Bilişsel İçgörüler** ekleme kodunu kopyalayın.
3. İki pencere öğesi arasındaki iletişimi işlemek için [**Aracı dosyasını**](https://breakdown.blob.core.windows.net/public/vb.widgets.mediator.js) ekleyin:

    <script src="https://breakdown.blob.core.windows.net/public/vb.widgets.mediator.js"></script>

Artık bir kullanıcı uygulamanızda içgörü denetimine tıkladığında yürütücü ilgili noktaya atlayacaktır.

Daha fazla bilgi için [bu tanıtıma](https://codepen.io/videoindexer/pen/NzJeOb) bakın.

### <a name="embed-the-cognitive-insights-widget-and-use-azure-media-player-to-play-the-content"></a>Bilişsel İçgörüler pencere öğesini ekleyip içeriği oynatmak için Azure Media Player'ı kullanma

Bu bölümde, [AMP eklentisini](https://breakdown.blob.core.windows.net/public/amp-vb.plugin.js) kullanarak **Bilişsel İçgörüler** pencere öğesi ile bir Azure Media Player örneği arasında etkileşim sağlanması açıklanmaktadır.
 
1. AMP yürütücüsü için bir Video Indexer eklentisi ekleyin.

        <script src="https://breakdown.blob.core.windows.net/public/amp-vb.plugin.js"></script>


2. Video Indexer eklentisine sahip Azure Media Player’ın örneğini oluşturun.

        // Init Source
        function initSource() {
            var tracks = [{
            kind: 'captions',
            // Here is how to load vtt from VI, you can replace it with your vtt url.
            src: this.getSubtitlesUrl("c4c1ad4c9a", "English"),
            srclang: 'en',
            label: 'English'
            }];

            myPlayer.src([
            {
                "src": "//amssamples.streaming.mediaservices.windows.net/91492735-c523-432b-ba01-faba6c2206a2/AzureMediaServicesPromo.ism/manifest",
                "type": "application/vnd.ms-sstr+xml"
            }
            ], tracks);
        }

        // Init your AMP instance
        var myPlayer = amp('vid1', { /* Options */
            "nativeControlsForTouch": false,
            autoplay: true,
            controls: true,
            width: "640",
            height: "400",
            poster: "",
            plugins: {
            videobreakedown: {}
            }
        }, function () {
            // Activate the plugin
            this.videobreakdown({
            videoId: "c4c1ad4c9a",
            syncTranscript: true,
            syncLanguage: true
            });

            // Set the source dynamically
            initSource.call(this);
        });

3. **Bilişsel İçgörüler** ekleme kodunu kopyalayın.

Artık Azure Media Player ile iletişim kurabilirsiniz.

Daha fazla bilgi için [bu tanıtıma](https://codepen.io/videoindexer/pen/rYONrO) bakın.

### <a name="embed-video-indexer-cognitive-insights-widget-and-use-your-own-player-could-be-any-player"></a>Video Indexer Bilişsel İçgörüler pencere öğesini ekleyip kendi yürütücünüzü (dilediğiniz oynatıcı olabilir) kullanma

Kendi yürütücünüzü kullanırsanız iletişimi sağlamak için yürütücünüzün ayarlanması sizin sorumluluğunuzdadır. 

1. Video oynatıcınızı ekleyin.

    Örneğin, standart bir HTML5 yürütücü ekleyebilirsiniz

        <video id="vid1" width="640" height="360" controls autoplay preload>
           <source src="//breakdown.blob.core.windows.net/public/Microsoft%20HoloLens-%20RoboRaid.mp4" type="video/mp4" /> 
           Your browser does not support the video tag.
        </video>    

2. Bilişsel İçgörüler pencere öğesini ekleyin.
3. "İleti" olayını dinleyerek yürütücünüz için iletişimi uygulayın. Örneğin:

        <script>
    
            (function(){
            // Reference your player instance
            var playerInstance = document.getElementById('vid1');
        
            function jumpTo(evt) {
              var origin = evt.origin || evt.originalEvent.origin;
        
              // Validate that event comes from the videobreakdown domain.
              if ((origin === "https://www.videobreakdown.com") && evt.data.time !== undefined){
                
                // Here you need to call your player "jumpTo" implementation
                playerInstance.currentTime = evt.data.time;
               
                // Confirm arrival to us
                if ('postMessage' in window) {
                  evt.source.postMessage({confirm: true, time: evt.data.time}, origin);
                }
              }
            }
        
            // Listen to message event
            window.addEventListener("message", jumpTo, false);
          
            }())    
        
        </script>


Daha fazla bilgi için [bu tanıtıma](https://codepen.io/videoindexer/pen/YEyPLd) bakın.

## <a name="adding-subtitles"></a>Alt yazı ekleme

Video Indexer içgörülerini kendi AMP yürütücünüze eklerseniz **GetVttUrl** yöntemini kullanarak açıklamalı alt yazılar (alt yazılar) alabilirsiniz. Ayrıca, Video Indexer AMP eklentisi **getSubtitlesUrl**’den bir javascript yöntemi de çağırabilirsiniz (daha önce gösterildiği gibi). 

## <a name="customizing-embeddable-widgets"></a>Eklenebilir pencere öğelerini özelleştirme

### <a name="cognitive-insights-widget"></a>Bilişsel içgörüler pencere öğesi
API’den veya web uygulamasından aldığınız ekleme koduna eklenen aşağıdaki URL parametresi için bir değer olarak belirtmek suretiyle istediğiniz öngörü türlerini seçebilirsiniz:

**&widgets=** \<istenen pencere öğelerinin listesi>

Olası değerler şunlardır: people (kişiler), keywords (anahtar sözcükler), sentiments (yaklaşımlar), transcript (transkript), search (arama).

Örneğin, yalnızca kişi ve arama içgörüleri içeren bir pencere öğesi eklemek istiyorsanız iframe ekleme URL’si şöyle olacaktır: https://www.videoindexer.ai/embed/insights/<accountId>/<videoId>/?widgets=people,search

Ayrıca, iframe URL'sine **&title=**<YourTitle> parametresini ekleyerek iframe penceresinin başlığını özelleştirebilirsiniz. (Html \<başlık> değerini özelleştirir).
Örneğin, iframe pencerenize "İçgörülerim" başlığını vermek istiyorsanız URL şöyle görünür: https://www.videoindexer.ai/embed/insights/<accountId>/<videoId>/?title=İçgörülerim. Bu seçeneğin yalnızca içgörülerin yeni bir pencerede açılacağı durumlarda kullanılabileceğine dikkat edin.

### <a name="player-widget"></a>Yürütücü pencere öğesi
Video Indexer yürütücüsünü eklerseniz iframe boyutunu belirterek yürütücü boyutunu seçebilirsiniz.

Örneğin:

    <iframe width="640" height="360" src="https://www.videoindexer.ai/embed/player/<accountId>/<videoId>/" frameborder="0" allowfullscreen />

Video Indexer yürütücüsünde varsayılan olarak otomatik bir şekilde oluşturulan açıklamalı alt yazılar olacaktır. Bu açıklamalı alt yazılar, video yüklenirken seçilen kaynak dili kullanarak videodan ayıklanan transkripti temel alır.

Farklı bir dilde eklemek istiyorsanız yürütücü ekleme URL’sine **&captions=< Dil | ”all” | “false” >** ekleyebilir veya mevcut tüm dillerdeki açıklamalı alt yazıları istiyorsanız değer olarak “all” kullanabilirsiniz.
Açıklamalı alt yazıların varsayılan olarak görüntülenmesini istiyorsanız **&showCaptions=true** parametresini geçirebilirsiniz.

Ekleme URL'si şuna benzeyecektir: https://www.videoindexer.ai/embed/player/<accountId>/<videoId>/?captions=italian. Açıklamalı alt yazıları devre dışı bırakmak istiyorsanız açıklamalı alt yazılar parametresi için “false” değerini geçirebilirsiniz.

Otomatik oynatma: Yürütücü varsayılan olarak videoyu oynatmaya başlar. Videonun otomatik olarak oynatılmaması için yukarıdaki ekleme URL’sine &autoplay=false parametresini geçirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Video Indexer içgörülerini görüntüleme ve düzenleme hakkında bilgi edinmek için [bu](video-indexer-view-edit.md) makaleye bakın.

Ayrıca, [Video Indexer codepen’ine](https://codepen.io/videoindexer/pen/eGxebZ) göz atın.
