---
title: Video oynatma-Azure
description: Yer tutucu
ms.topic: conceptual
ms.date: 04/27/2020
ms.openlocfilehash: 2020d64538b2fcc846ab9a146e2fc95325abd26b
ms.sourcegitcommit: 73fb48074c4c91c3511d5bcdffd6e40854fb46e5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2021
ms.locfileid: "106063381"
---
# <a name="video-playback"></a>Video kayıttan yürütme 

## <a name="suggested-pre-reading"></a>Önerilen önceden okuma 

* [IoT Edge genel bakış üzerinde canlı video analizi](overview.md)
* [IoT Edge terminolojisinde canlı video analizi](terminology.md)
* [Medya grafiği kavramı](media-graph-concept.md)

## <a name="overview"></a>Genel Bakış  

Azure Media Services bir [varlığa](terminology.md#asset)video kaydetmek için [medya grafiklerini](media-graph-concept.md) kullanabilirsiniz. Bu belgede, Azure Media Services mevcut akış yeteneklerini kullanarak bir varlığı oynatmak için gerçekleştirmeniz gereken adımlar hakkında bilgi edinebilirsiniz.

## <a name="streaming-endpoint"></a>Akış uç noktası 

Azure Media Services, HTTP Canlı Akışı (HLS) ve MPEG-DASH gibi sektör standardı, HTTP tabanlı medya akışı protokollerini kullanarak varlığı video oynatıcılara [akıtmak](terminology.md#streaming) için kullanabilirsiniz. Kaydedilen içerikten akış biçimlerine bu medya dönüştürme işlemi, Azure Media Service hesabınızda sağlamanız gereken bir kaynak olan bir [akış uç noktası](../latest/streaming-endpoint-concept.md)tarafından işlenir.

## <a name="streaming-policy"></a>Akış ilkesi 

Azure Media Services, [içeriğinizi Media Services dinamik şifreleme Ile koruyun](../latest/drm-content-protection-concept.md) bölümünde anlatıldığı gibi, video akışlarınızın güvenliğini sağlamak için size farklı yöntemler sunar. Yüksek düzeyde, içerik koruma seçenekleri şunlardır:

* **Açık akış** – akış sırasında hiçbir şifreleme uygulanmaz.
* **Gelişmiş Şifreleme Standardı (AES-128) – kullanın** ve yalnızca kimliği doğrulanmış görüntüleyicilere videonun şifresini çözmek için anahtar teslim etmek üzere bir yöntem uygulayın.
* **Dijital Rights Management (DRM) sistemlerini kullanarak** , bu ilkeleri zorlayan cihazlara videonun kullanımını, değiştirilmesini ve teslimini denetlemek için kullanın.

İçerik koruması elde etmek için medya hizmeti hesabınızda bir [akış ilkesi](../latest/streaming-policy-concept.md) tanımlayabilir ve oluşturabilir ve bu ilkeyi tüm varlıkları akışa almak için kullanabilirsiniz (tüm akışlar güvenlik için aynı gereksinimlere sahip olduğu varsayılırsa). Önceden tanımlanmış ilkelerden herhangi birini de kullanabilirsiniz (örneğin, Predefined_ClearStreamingOnly).

## <a name="streaming-locator"></a>Akış Bulucu  

Medya hizmeti hesabınızda bir akış uç noktası başlatıldıktan ve akış ilkesi tanımlandığında, HLS veya DASH protokolleri aracılığıyla bir varlıktan kaydedilmiş medyayı akışa devam edebilirsiniz. Web-oyuncular ve mobil uygulamalar, bu HLS veya DASH akışına işaret eden bir URL 'ye gerek duyar. Bu URL 'YI [akış bulucuyu](../latest/streaming-locators-concept.md)kullanarak oluşturabilirsiniz. Bu makalede açıklandığı gibi ve [bir akış bulucu ve derleme URL 'Leri oluşturma](../latest/create-streaming-locator-build-url.md) örneğinde gösterildiği gibi, AKıŞ URL 'si akış uç noktası, akış ilkesi ve akış bulucudan oluşur.

## <a name="content-recorded-using-file-sink"></a>Dosya havuzu kullanılarak kaydedilen içerik  

Medya [grafiği dosya havuzunda](media-graph-concept.md#file-sink)açıklandığı gibi, medya grafikinizdeki bir dosya havuzunu kullanarak uç cihazının yerel dosya sistemine videoları kaydetmek için medya grafiklerini kullanabilirsiniz. Dosya havuzu [MP4](https://developer.mozilla.org/docs/Web/Media/Formats/Containers#MP4) dosyaları oluşturur ve bu tür içerikleri oynatmak için HTML5 [ &lt; video &gt; ](https://developer.mozilla.org/docs/Web/HTML/Element/video) öğesini kullanabilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

[Azure IoT Edge](../../iot-edge/index.yml)
<!--
## Next steps

[Playback recording](playback-recording-how-to.md)
-->
