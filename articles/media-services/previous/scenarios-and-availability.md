---
title: Microsoft Azure Media Services senaryoları ve özelliklerin veri merkezleri arasında kullanılabilirliği | Microsoft Docs
description: Bu konu başlığı altında, Microsoft Azure Media Services senaryolarına ve özelliklerle hizmetlerin veri merkezleri arasında kullanılabilirliğine genel bir bakış sağlanır.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/20/2019
ms.author: juliako
ms.openlocfilehash: fa0cf5d698bc2186928e0db19be173ec725485e8
ms.sourcegitcommit: 7d8158fcdcc25107dfda98a355bf4ee6343c0f5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80985941"
---
# <a name="scenarios-and-availability-of-media-services-features-across-datacenters"></a>Senaryolar ve Media Services özelliklerinin veri merkezleri arasında kullanılabilirliği

> [!NOTE]
> Media Services v2’ye herhangi bir yeni özellik veya işlevsellik eklenmemektedir. <br/>En son sürümü göz atın, [Medya Hizmetleri v3](https://docs.microsoft.com/azure/media-services/latest/). Ayrıca, [v2'den v3'e geçiş kılavuzuna](../latest/migrate-from-v2-to-v3.md) bakın

Microsoft Azure Media Services (AMS), çeşitli istemcilere (TV, PC ve mobil cihazlar gibi) isteğe bağlı olarak veya canlı akış halinde teslim amacıyla video ve ses içeriklerini güvenli bir şekilde karşıya yüklemenizi, depolamanızı, kodlamanızı ve paketlemenizi sağlar.

AMS, dünyanın dört bir yanındaki birden fazla veri merkezinde çalışmaktadır. Bu veri merkezleri, coğrafi bölgeler halinde gruplandırılarak uygulamalarınızı oluşturacağınız yeri seçme esnekliği tanır. [Bölgeler ve konumlarının listesini](https://azure.microsoft.com/regions/) gözden geçirebilirsiniz. 

Bu konu, içeriğinizi [canlı](#live_scenarios) veya isteğe bağlı olarak sunmak için yaygın senaryoları gösterir. Bu konu başlığında, medya özellikleri ve hizmetlerinin veri merkezleri arasında kullanılabilirliği hakkındaki ayrıntılar da sağlanır.

## <a name="overview"></a>Genel Bakış

### <a name="prerequisites"></a>Ön koşullar

Azure Media Services’i kullanmaya başlamak için aşağıdakilerin bulunması gerekir:

* Bir Azure hesabı. Bir hesabınız yoksa, yalnızca birkaç dakika içinde ücretsiz bir deneme hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com).
* Bir Azure Media Services hesabı. Daha fazla bilgi için bkz. [Hesap Oluşturma](media-services-portal-create-account.md).
* İçerik akışı yapmak istediğiniz akış uç noktasının **Çalışıyor** durumunda olması gerekir.

    AMS hesabınız oluşturulduğunda, hesabınıza **Durdurulmuş** durumda bir **varsayılan** akış uç noktası eklenir. İçerik akışını başlatmak ve dinamik paketleme ile dinamik şifrelemeden yararlanmak için akış uç noktasının **Çalışıyor** durumda olması gerekir.

### <a name="commonly-used-objects-when-developing-against-the-ams-odata-model"></a>AMS OData modeline göre geliştirirken sık kullanılan nesneler

Aşağıdaki resimde Media Services OData modeliyle geliştirirken en sık kullanılan nesnelerin bazıları gösterilmektedir.

Resmi tam boyutlu görüntülemek için tıklayın.  

<a href="./media/media-services-overview/media-services-overview-object-model.png" target="_blank"><img src="./media/media-services-overview/media-services-overview-object-model-small.png"></a> 

Modelin tamamını [buradan](https://media.windows.net/API/$metadata?api-version=2.15) görüntüleyebilirsiniz.  

## <a name="protect-content-in-storage-and-deliver-streaming-media-in-the-clear-non-encrypted"></a>Depolama alanında içeriği koruma ve akan medyayı temiz olarak (şifrelenmemiş) teslim etme

![VoD iş akışı](./media/scenarios-and-availability/scenarios-and-availability01.png)

1. Yüksek kaliteli bir medya dosyasını bir varlığa yükleyin.

    İçeriğinizi yükleme sırasında ve depolama alanında beklerken korumak için depolama şifrelemesi seçeneğini uygulamanız önerilir.
2. Uyarlamalı bit hızlı bir MP4 dosyaları grubuna kodlayın.

    İçeriğinizi beklerken korumak için çıktı varlığına depolama şifrelemesi seçeneğini uygulamanız önerilir.
3. Varlık teslim ilkesini (dinamik paketleme tarafından kullanılır) yapılandırın.

    Varlığınıza depolama şifrelemesi uygulanmışsa varlık teslim ilkesini yapılandırmanız **gerekir**.
4. Bir OnDemand bulucu oluşturarak varlığı yayımlayın.
5. Yayımlanan içeriği akışla aktarın.

Veri merkezlerinde kullanılabilirlik hakkında bilgi için [Kullanılabilirlik](#availability) bölümüne bakın.

## <a name="protect-content-in-storage-deliver-dynamically-encrypted-streaming-media"></a>Depolama alanında içeriği koruma, dinamik olarak şifrelenmiş akan medya teslim etme

![PlayReady ile koruma](./media/media-services-content-protection-overview/media-services-content-protection-with-multi-drm.png)

1. Yüksek kaliteli bir medya dosyasını bir varlığa yükleyin. Varlığa depolama şifrelemesi seçeneğini uygulayın.
2. Uyarlamalı bit hızlı bir MP4 dosyaları grubuna kodlayın. Çıktı varlığına depolama şifrelemesi seçeneğini uygulayın.
3. Kayıttan yürütme sırasında dinamik olarak şifrelenmesini istediğiniz varlık için şifreleme içerik anahtarı oluşturun.
4. İçerik anahtarı yetkilendirme ilkesini yapılandırın.
5. Varlık teslim ilkesini (dinamik paketleme ve dinamik şifreleme tarafından kullanılır) yapılandırın.
6. Bir OnDemand bulucu oluşturarak varlığı yayımlayın.
7. Yayımlanan içeriği akışla aktarın.

Veri merkezlerinde kullanılabilirlik hakkında bilgi için [Kullanılabilirlik](#availability) bölümüne bakın.

## <a name="use-media-analytics-to-derive-actionable-insights-from-your-videos"></a>Medya Analizi kullanarak videolarınızdan eyleme dönüştürülebilir öngörüler türetme

Medya Analizi, kuruluş ve işletmelerin video dosyalarından eyleme dönüştürülebilir öngörüler türetmesini kolaylaştıran bir grup konuşma ve görme bileşenidir. Daha fazla bilgi için bkz. [Azure Media Services Analizi’ne Genel Bakış](media-services-analytics-overview.md).

1. Yüksek kaliteli bir medya dosyasını bir varlığa yükleyin.
2. Videolarınızı, [Medya Analizi’ne genel bakış](media-services-analytics-overview.md) bölümünde açıklanan Medya Analizi hizmetlerinden biriyle işleyin.
3. Medya Analizi medya işlemcileri MP4 veya JSON dosyaları üretir. Medya işlemcisi bir MP4 dosyası oluşturduysa dosyayı aşamalı olarak indirebilirsiniz. Medya işlemcisi bir JSON dosyası oluşturduysa dosyayı Azure blob depolamadan indirebilirsiniz.

Veri merkezlerinde kullanılabilirlik hakkında bilgi için [Kullanılabilirlik](#availability) bölümüne bakın.

## <a name="deliver-progressive-download"></a>Aşamalı indirme teslimi

1. Yüksek kaliteli bir medya dosyasını bir varlığa yükleyin.
2. Tek bir MP4 dosyasına kodlayın.
3. Bir OnDemand veya SAS bulucu oluşturarak varlığı yayımlayın.

    SAS Bulucu kullanıyorsanız içerik, Azure blob depolama alanından indirilir. Bu durumda, başlatılmış durumda akış uç noktası gerekli değildir.
4. Aşamalı olarak içerik indirin.

## <a name="delivering-live-streaming-events"></a><a id="live_scenarios"></a>Canlı akış olayları teslimi 

1. Çeşitli canlı akış protokollerini (RTMP veya Kesintisiz Akış gibi) kullanarak canlı içerik alın.
2. (isteğe bağlı) Akışınızı, bit hızı uyarlamalı akışa kodlayın.
3. Canlı akışınızın önizlemesini görüntüleyin.
4. İçeriği yaygın akış protokolleri (örneğin MPEG DASH, Kesintisiz, HLS) aracılığıyla doğrudan müşterilerinize veya başkalarına dağıtım için bir Content Delivery Network’e (CDN) teslim edin.

    -veya-

    Alınan içeriği daha sonra akışla aktarmak üzere kaydedip depolayın (İsteğe Bağlı Video).

Canlı akış yaparken, aşağıdaki yollardan birini seçebilirsiniz:

### <a name="working-with-channels-that-receive-multi-bitrate-live-stream-from-on-premises-encoders-pass-through"></a>Şirket içi kodlayıcılardan çoklu bit hızlı canlı akış alan kanallar ile çalışma (doğrudan geçiş)

Aşağıdaki diyagramda, AMS platformunun **doğrudan geçiş** iş akışında rol oynayan başlıca parçaları gösterilmektedir.

![Canlı iş akışı](./media/scenarios-and-availability/media-services-live-streaming-current.png)

Daha fazla bilgi için bkz. [Şirket İçi Kodlayıcılardan Çoklu Bit Hızlı Canlı Akış Alan Kanallar ile Çalışma](media-services-live-streaming-with-onprem-encoders.md).

### <a name="working-with-channels-that-are-enabled-to-perform-live-encoding-with-azure-media-services"></a>Azure Media Services ile gerçek zamanlı kodlama gerçekleştirmek için etkinleştirilmiş kanallar ile çalışma

Aşağıdaki diyagramda, AMS platformunun bir Kanalın, Media Services ile kodlama gerçekleştirmek için etkinleştirildiği Canlı Akış iş akışında rol oynayan başlıca parçaları gösterilmektedir.

![Canlı iş akışı](./media/scenarios-and-availability/media-services-live-streaming-new.png)

Daha fazla bilgi için bkz. [Azure Media Services ile Gerçek Zamanlı Kodlama Gerçekleştirmek İçin Etkinleştirilmiş Kanallar ile Çalışma](media-services-manage-live-encoder-enabled-channels.md).

Veri merkezlerinde kullanılabilirlik hakkında bilgi için [Kullanılabilirlik](#availability) bölümüne bakın.

## <a name="consuming-content"></a>İçerik kullanma

Azure Media Services, şunlar dahil olmak üzere çoğu platform için zengin ve dinamik istemci oynatıcı uygulamaları oluştururken ihtiyacınız olan araçları sağlar: iOS Cihazları, Android Cihazları, Windows, Windows Phone, Xbox ve Alıcı kutuları. 

## <a name="enabling-azure-cdn"></a>Azure CDN'yi etkinleştirme

Media Services, Azure CDN ile tümleştirmeyi destekler. Azure CDN'yi etkinleştirme hakkında daha fazla bilgi için bkz. [Media Services Hesabında Akış Uç Noktalarını Yönetme](media-services-portal-manage-streaming-endpoints.md).

## <a name="scaling-a-media-services-account"></a><a id="scaling"></a>Media Services hesabını ölçeklendirme

AMS müşterileri akış uç noktalarını, medya işleme ve depolamayı kendi AMS hesaplarında ölçeklendirebilir.

* Media Services müşterileri **Standart** akış uç noktası veya **Premium** akış uç noktası seçebilir. **Standart** akış uç noktası çoğu akış iş yükü için uygundur. **Premium** akış uç noktaları ile aynı özellikleri taşır ve giden bant genişliğini otomatik olarak ölçeklendirir. 

    **Premium** akış uç noktaları, adanmış ve ölçeklenebilir bant genişliği kapasitesi sağlar; dolayısıyla gelişmiş iş yükleri için uygundur. **Premium** akış uç noktası olan müşteriler, varsayılan olarak bir akış birimi (SU) alır. Akış uç noktası, SU’lar eklenerek ölçeklendirilebilir. Her SU, uygulamaya ek bant genişliği kapasitesi sağlar. **Premium** akış uç noktalarını ölçeklendirme hakkında daha fazla bilgi için, [Akış uç noktalarını ölçeklendirme](media-services-portal-scale-streaming-endpoints.md) konusuna bakın.

* Media Services hesabı bir Ayrılmış Birim Türüyle ilişkilendirilir ve bu da medya işleme görevlerinizin ne hızda işleneceğini belirler. Aşağıdaki ayrılmış birim türleri arasından seçim yapabilirsiniz: **S1**, **S2**, veya **S3**. Örneğin, aynı kodlama işi **S2** ayrılmış birim türünü kullandığınızda **S1** türüne göre daha hızlı çalışır.

    Ayrılmış birim türünü belirtmenin yanı sıra, hesabınızı Ayrılmış **Birimler** (RUS) ile birlikte sağlamanızı belirtebilirsiniz. Sağlanan RU sayısı, verili bir hesapta eşzamanlı olarak işlenebilecek medya görevlerinin sayısını belirler.

    >[!NOTE]
    >RU, tüm medya işlemesini paralel hale getirmek için çalışır ve Azure Media Indexer’ın kullanıldığı dizin oluşturma işleri de buna dahildir. Bununla birlikte kodlamadan farklı olarak, dizin oluşturma işleri daha hızlı ayrılmış birimlerde daha hızlı işlenmez.

    Daha fazla bilgi için bkz: [Ölçek medya işleme.](media-services-portal-scale-media-processing.md)
* Media Services hesabınızı, depolama hesapları ekleyerek de ölçeklendirebilirsiniz. Her depolama hesabı 500 TB ile sınırlıdır. Depolama alanınızı varsayılan sınırlamaların ötesine genişletmek için, tek bir Media Services hesabına birden çok depolama hesabı eklemeyi seçebilirsiniz. Daha fazla bilgi için bkz. [Depolama hesaplarını yönetme](meda-services-managing-multiple-storage-accounts.md).

## <a name="availability-of-media-services-features-across-datacenters"></a><a id="availability"></a> Media Services özelliklerinin veri merkezleri arasında kullanılabilirliği

Bu bölümde, Media Services özelliklerinin veri merkezleri arasında kullanılabilirliği hakkındaki ayrıntılar sağlanır.

### <a name="ams-accounts"></a>AMS hesapları

#### <a name="availability"></a>Kullanılabilirlik

Medya Hizmetlerinin belirli bir veri merkezinde kullanılabilir olup olmadığını belirlemek için [Bölgeye göre Azure Ürünlerini](https://azure.microsoft.com/global-infrastructure/services/?products=media-services&regions=all) kullanın.

### <a name="streaming-endpoints"></a>Akış uç noktaları 

Media Services müşterileri **Standart** akış uç noktası veya **Premium** akış uç noktası seçebilir. Daha fazla bilgi için [ölçeklendirme](#scaling) bölümüne bakın.

#### <a name="availability"></a>Kullanılabilirlik

|Adı|Durum|Veri merkezleri
|---|---|---|
|Standart|GA|Tümü|
|Premium|GA|Tümü|

### <a name="live-encoding"></a>Live encoding

#### <a name="availability"></a>Kullanılabilirlik

Şu bölgeler hariç tüm veri merkezlerinde kullanılabilir: Almanya, Güney Brezilya, Hindistan Batı, Hindistan Güney ve Hindistan Orta. 

### <a name="encoding-media-processors"></a>Kodlama medya işleyicileri

AMS, isteğe bağlı iki kodlayıcı sunar: **Media Encoder Standard** ve **Media Encoder Premium İş Akışı**. Daha fazla bilgi için bkz. [Azure isteğe bağlı medya kodlayıcılarına genel bakış ve karşılaştırma](media-services-encode-asset.md). 

#### <a name="availability"></a>Kullanılabilirlik

|Medya işlemci adı|Durum|Veri merkezleri
|---|---|---|
|Media Encoder Standard|GA|Tümü|
|Media Encoder Premium İş Akışı|GA|Çin dışında tümü|

### <a name="analytics-media-processors"></a>Analiz medya işlemcileri

Medya Analizi, kuruluş ve işletmelerin video dosyalarından eyleme dönüştürülebilir öngörüler türetmesini kolaylaştıran bir grup konuşma ve görme bileşenidir. Daha fazla bilgi için bkz. [Azure Media Services Analizi’ne Genel Bakış](media-services-analytics-overview.md).

> [!NOTE]
> Bazı analitik ortam işlemcileri kullanımdan kaldırılacaktır. Emeklilik tarihleri [için, eski bileşenler](legacy-components.md) konusuna bakın.

#### <a name="availability"></a>Kullanılabilirlik

|Medya işlemci adı|Durum|Veri merkezleri
|---|---|---|
|Azure Media Face Detector|Önizleme|Tümü|
|Azure Media Indexer|GA|Tümü|
|Azure Media Motion Detector|Önizleme|Tümü|
|Azure Media OCR|Önizleme|Tümü|
|Azure Media Redactor|GA|Tümü|
|Azure Media Video Thumbnails|Önizleme|Tümü|

### <a name="protection"></a>Koruma

Microsoft Azure Media Services, medyanızı bilgisayarınızdan ayrıldığı andan başlayıp depolama, işleme ve teslim aşamaları boyunca güvenlik altına almanızı sağlar. Daha fazla bilgi için bkz. [AMS içeriğini koruma](media-services-content-protection-overview.md).

#### <a name="availability"></a>Kullanılabilirlik

|Şifreleme|Durum|Veri merkezleri|
|---|---|---| 
|Depolama|GA|Tümü|
|AES-128 anahtarları|GA|Tümü|
|Fairplay|GA|Tümü|
|PlayReady|GA|Tümü|
|Widevine|GA|Almanya, Federal Devlet ve Çin dışında tümü.

### <a name="reserved-units-rus"></a>Ayrılmış birimler (RU)

Sağlanan ayrılmış birim sayısı, verili bir hesapta eşzamanlı olarak işlenebilecek medya görevlerinin sayısını belirler. 

Daha fazla bilgi için [ölçeklendirme](#scaling) bölümüne bakın.

#### <a name="availability"></a>Kullanılabilirlik

Tüm veri merkezlerinde kullanılabilir.

### <a name="reserved-unit-ru-type"></a>Ayrılmış birim (RU) türü

Media Services hesabı bir Ayrılmış birim türüyle ilişkilendirilir ve bu da medya işleme görevlerinizin ne hızda işleneceğini belirler. Şu ayrılmış birim türlerinden birini seçebilirsiniz: S1, S2 ve S3.

Daha fazla bilgi için [ölçeklendirme](#scaling) bölümüne bakın.

#### <a name="availability"></a>Kullanılabilirlik

|RU türü adı|Durum|Veri merkezleri
|---|---|---|
|S1|GA|Tümü|
|S2|GA|Güney Brezilya ve Hindistan Batı dışında tümü|
|S3|GA|Hindistan Batı dışında tümü|

## <a name="additional-notes"></a>Ek notlar

* Widevine, Google Inc. tarafından sağlanan ve Google, Inc.'in hizmet koşullarına ve Gizlilik Politikasına tabi olan bir hizmettir.

## <a name="next-steps"></a>Sonraki adımlar

Media Services öğrenme yollarını gözden geçirin.

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

