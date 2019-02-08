---
title: Metin - Bing özel arama vurgulamak için decoration işaretçileri kullanma
titlesuffix: Azure Cognitive Services
description: Metin süslemeleri araması yanıtlarında, etkinleştirme işlemi gösterilmektedir.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: conceptual
ms.date: 09/28/2017
ms.author: maheshb
ms.openlocfilehash: 17297b651d672cee746076d563b5c410390c0fa2
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55862811"
---
# <a name="using-decoration-markers-to-highlight-text"></a>Metni vurgulayacak şekilde decoration işaretçiler kullanma

Bing, isabet vurgulama, sorgu terimleriyle işaretler destekler (veya diğer, Bing bulursa ilgili koşulları) içinde görünen dizeleri bazı cevaplara ulaştıracaktır. Örneğin, bir Web sayfasının `name`, `displayUrl`, ve `snippet` alanları sorgu terimleriyle işaretleyin.

Varsayılan olarak, Bing, görünen dizeleri işaretçilerini vurgulama içermez. İşaretçileri dahil etmenize `textDecorations` ayarlayın ve sorgu parametresi isteğinizdeki **true**. Bing, başlangıç ve sona ermeden işaretlemek için E000 ve E001 Unicode karakterleri kullanılarak sorgu terimleri işaretler. Örneğin, sorgu terimine Dinghy yelken açmaya ne dersiniz ve her iki terim alanı var, terimi isabet vurgulama karakter aşağıdaki örnekte gösterildiği gibi içine alınır:  
  
![İsabet vurgulama](./media/bing-hit-highlighting.PNG) 

Dize, kullanıcı arabirimini görüntülemeden önce Unicode karakterleri, görüntüleme biçimi için uygun olan karakterlerle değiştirirler. HTML olarak metin görüntülüyorsanız, örneğin, sorgu terimine E000 ile değiştirerek vurgulayın < b\> ve ile E001 < /b\>. Biçimlendirme uygulamak istemiyorsanız işaretlerinin dizeden kaldırın. 

Bing, Unicode karakter veya HTML etiketleri işaretçileri olarak kullanma seçeneği sunar. Kullanmak için hangi işaretçilerini belirtmek için dahil `textFormat` sorgu parametresi. Unicode karakterleriyle içeriği işaretlemek için ayarlanmış `textFormat` ham (varsayılan) ve HTML etiketlerini içerikle işaretlemek için ayarlanmış `textFormat` HTML. 
  
Varsa `textDecorations` olduğu **true**, Bing, yanıtların görünen dizeleri aşağıdaki işaretlerinin içerebilir. HTML eşdeğeri yoksa, HTML tablo hücresi boştur.

|Unicode|HTML|Açıklama
|-|-|-
|U+E000|\<b >|(İsabet vurgulama) sorgu terimine başlangıcını işaretleyen
|U+E001|\</b >|Sorgu döneminin sonunu işaretler
|U+E002|\<Ben >|İtalik içerik başlangıcını işaretleyen 
|U+E003|\</i >|İtalik içerik sonunu işaretleyen
|U+E004|\<br/>|Bir satır sonunu işaretler
|U+E005||Bir telefon numarası başlangıcını işaretleyen
|U+E006||Bir telefon numarası sonunu işaretleyen
|U+E007||Bir adresi başlangıcını işaretleyen
|U+E008||Bir adresi sonunu işaretleyen
|U+E009|\&nbsp;|Bölünemez boşluk işaretler
|U+E00C|\<strong >|Kalın içerik başlangıcını işaretleyen
|U + E00D|\</ strong >|Kalın içerik sonunu işaretleyen
|U + E00E||İçerik arka planının çevresindeki, arka plan açık başlangıcını işaretleyen
|U + E00F||İçerik arka planının çevresindeki, arka plan açık sonunu işaretleyen
|U+E010||İçerik arka planının çevresindeki, arka plan koyu başlangıcını işaretleyen
|U+E011||İçerik arka planının çevresindeki, arka plan koyu sonunu işaretleyen
|U+E012|\<DEL >|Kaldırılmış içerik başlangıcını işaretleyen
|U+E013|\</ DEL >|Kaldırılmış içerik sonunu işaretleyen
|U+E016|\<Sub >|Alt simge içerik başlangıcını işaretleyen
|U+E017|\</ sub >|Alt simge içerik sonunu işaretleyen
|U+E018|\<sup >|Simge içerik başlangıcını işaretleyen
|U+E019|\</ sup >|Simge içerik sonunu işaretleyen

Aşağıdaki örnekte gösterildiği bir `Computation` log(2) sorgu terimi için alt simge işaretçileri içeren bir yanıt. `expression` Alanında işaretçileri yalnızca şu durumlarda ' textDecoration olduğu **true**.

![Hesaplama işaretçileri](./media/bing-markers-computation.PNG) 

İstek süslemeleri istemediyseniz, ifade log10(2) olacaktır. 
  
