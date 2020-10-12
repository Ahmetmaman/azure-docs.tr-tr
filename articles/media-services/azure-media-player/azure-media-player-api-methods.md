---
title: Azure Media Player API yöntemleri
description: Azure Media Player API 'SI, tarayıcıda HTML5 video, Flash, Silverlight veya diğer desteklenen herhangi bir kayıttan yürütme teknolojileriyle video çalıp oynamadan, JavaScript üzerinden video ile etkileşimde bulunabilmeniz için izin verir.
author: IngridAtMicrosoft
ms.author: inhenkel
ms.service: media-services
ms.topic: reference
ms.date: 04/20/2020
ms.openlocfilehash: d1ad0cbe30ce20e61a31f0534a47fe031f6f257b
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "81727274"
---
# <a name="api"></a>API #

Azure Media Player API 'SI, tarayıcıda HTML5 video, Flash, Silverlight veya diğer desteklenen herhangi bir kayıttan yürütme teknolojileriyle video çalıp oynamadan, JavaScript üzerinden video ile etkileşimde bulunabilmeniz için izin verir.

## <a name="referencing-the-player"></a>Yürütücüye başvurma ##

API işlevlerini kullanmak için oynatıcı nesnesine erişmeniz gerekir. Yağckily, kolayca alınır. Yalnızca video etiketinizdeki bir KIMLIĞE sahip olduğundan emin olmanız gerekir. Örnek ekleme kodunun KIMLIĞI vardır `vid1` . Bir sayfada birden çok video varsa, her video etiketinin benzersiz bir KIMLIĞE sahip olduğundan emin olun.

`var myPlayer = amp('vid1');`

> [!NOTE]
> Oynatıcı henüz Data-Setup özniteliği veya başka bir yöntem aracılığıyla başlatılmamışsa, oynatıcı da başlatılır.

## <a name="wait-until-the-player-is-ready"></a>Oyuncu hazırlanana kadar bekleyin ##

Video ve API 'yi ayarlamak için gereken süre Azure Media Player, kullanılan oynatma teknolojisine bağlı olarak değişir. HTML5, Flash veya Silverlight 'tan daha hızlı yükleme yapmak için genellikle çok daha hızlıdır. Bu nedenle Player 'ın ' Ready ' işlevi, Player 'ın API 'sini gerektiren herhangi bir kodu tetiklemek için kullanılmalıdır.

```javacript
    amp("vid_1").ready(function(){
      var myPlayer = this;

      // EXAMPLE: Start playing the video.
      myPlayer.play();
    });
```

VEYA

```javacript
    var myPlayer = amp("vid_1", myOptions, function(){
        //this is the ready function and will only execute after the player is loaded
    });
```

## <a name="api-methods"></a>API yöntemleri ##

Artık hazır bir yürütücüye erişiminiz olduğuna göre videoyu denetleyebilir, değerleri alabilir veya video olaylarına yanıt verebilirsiniz. Azure Media Player API işlevi adları [HTML5 medya API](http://www.whatwg.org/specs/web-apps/current-work/multipage/the-video-element.html)'sini izlemeye çalışır. Ana fark, alıcı/ayarlayıcı işlevlerinin video özellikleri için kullanıldığı bir farktır.

```javacript
    // setting a property on a bare HTML5 video element
    myVideoElement.currentTime = 120;

    // setting a property with Azure Media Player
    myPlayer.currentTime(120);
```

## <a name="registering-for-events"></a>Olaylara kaydolma ##
Tüm olayların uygulamaya uygun bir şekilde bildirilmesi ve Ready olayının dışında yapılması için, önce oynatıcı ilk kez başlatıldıktan sonra, olayların doğrudan kaydettirilmesi gerekir.

```javacript
    var myPlayer = amp("vid_1", myOptions, function(){
        //this is the ready function and will only execute after the player is loaded
    });
    myPlayer.addEventListener(amp.eventName.error, _ampEventHandler);
    //add other event listeners
```

## <a name="next-steps"></a>Sonraki adımlar ##

<!---Some context for the following links goes here--->
- [Hızlı başlangıç Azure Media Player](azure-media-player-quickstart.md)