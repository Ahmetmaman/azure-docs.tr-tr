---
title: Akış uç noktaları Azure Media Services | Microsoft Docs
description: Bu makalede, akış uç noktaları nelerdir ve Azure Media Services tarafından nasıl kullanıldıkları bir açıklama sağlar.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 02/25/2019
ms.author: juliako
ms.openlocfilehash: eb7f368100269c4e47076bb6b78bafc23e7a6089
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2019
ms.locfileid: "57845612"
---
# <a name="streaming-endpoints"></a>Akış Uç Noktaları

Microsoft Azure Media Services (AMS) içinde [akış uç noktalarını](https://docs.microsoft.com/rest/api/media/streamingendpoints) varlık içeriği bir istemci Yürütücü uygulamaya doğrudan teslim eden ya da daha fazla için bir içerik teslim ağı (CDN) bir akış hizmetini temsil eder Dağıtım. Çıkış akışından bir **akış uç noktası** hizmet canlı akış ve isteğe bağlı varlığı Media Services hesabınızda olabilir. Bir Media Services hesabı oluşturduğunuzda bir **varsayılan** akış uç noktası, durdurulmuş durumda sizin için oluşturulur. Nelze odstranit **varsayılan** akış uç noktası. Ek akış uç noktaları hesap altında oluşturulabilir. 

> [!NOTE]
> Video akışını başlatmak için başlatmanız **akış uç noktası** video akışı yapmak istediğiniz. 

## <a name="naming-convention"></a>Adlandırma kuralı

Varsayılan uç nokta için: `{AccountName}-{DatacenterAbbreviation}.streaming.media.azure.net`

Tüm ek uç noktalar için: `{EndpointName}-{AccountName}-{DatacenterAbbreviation}.streaming.media.azure.net`

## <a name="types"></a>Türler  

İki **akış uç noktası** türleri: **Standart** ve **Premium**. Türü ölçek birimi sayısına göre tanımlanır (`scaleUnits`) akış uç noktası için ayırın. 

Tablo türleri açıklanmaktadır:  

|Type|Ölçek birimleri|Açıklama|
|--------|--------|--------|  
|**Standart akış uç noktası** (önerilir)|0|**Standart** türü neredeyse tüm akış senaryoları ve hedef kitle boyutları için önerilen seçenektir. **Standart** türü giden bant genişliğini otomatik olarak ölçeklendirir. <br/>Media Services ile son derece gereksinimleri yoğunlukta olan çeşitli müşteriler için teklif **Premium** akış uç noktaları kullanılabilir ölçek artırma kapasitesi en büyük internet izleyiciler için. Geniş kitlelere ve eş zamanlı görüntüleyiciler bekliyorsanız, bizimle iletişim kurun amsstreaming\@microsoft.com taşımak gereken yönergeler **Premium** türü. |
|**Premium akış uç noktası**|>0|**Premium** akış uç noktaları, adanmış ve ölçeklenebilir bant genişliği kapasitesi sağlar; dolayısıyla gelişmiş iş yükleri için uygundur. Geçmeden bir **Premium** ayarlayarak türü `scaleUnits`. `scaleUnits` 200 MB/sn'lik artışlarla satın alınabilir adanmış çıkış kapasitesi sağlar. Kullanırken **Premium** türü, her etkin birim, uygulamaya ek bant genişliği kapasitesi sağlar. |

## <a name="working-with-cdn"></a>CDN ile çalışma

Çoğu durumda, CDN etkin olması gerekir. En fazla eşzamanlılık 500 görüntüleyiciler düşük kapasitesinden ancak ardından eşzamanlılık ile CDN en iyi ölçeklenen beri CDN devre dışı bırakmak için önerilir.

> [!NOTE]
> Akış uç noktası `hostname` ve CDN'yi etkinleştirme olup olmadığını akış URL'si aynı kalır.

### <a name="detailed-explanation-of-how-caching-works"></a>Ayrıntılı açıklaması ve önbelleğe alma nasıl işler?

CDN, akış uç noktası için bir CDN gereken bant genişliği miktarını etkin olduğundan ekleme değişir, belirli bir bant genişliği değer yoktur. Çok fazla içerik, ne kadar popüler olduğunu, bit hızlarına dönüştürme ve iletişim kuralları türüne bağlıdır. CDN ne istenen yalnızca önbelleği. Video parça önbelleğe sürece bu popüler içerik doğrudan CDN'den – hizmet anlamına gelir. Canlı içerik tam aynı şeyi izlemek çok kişi genellikle olduğundan önbelleğe olasıdır. Popüler ve bazıları da değil içerikler olabilir çünkü isteğe bağlı içerik biraz zor olabilir. Milyonlarca nerede bunların hiçbiri popüler (yalnızca 1 veya 2 görüntüleyiciler haftada) olan ancak binlerce kişinin yayınlanan tüm farklı videoları sahip video varlığını varsa, CDN daha az etkili olur. Bu önbellek isabetsizliği, akış uç noktasında yük artırın.
 
Ayrıca nasıl Uyarlamalı akış works göz önünde bulundurmanız gerekir. Kendi varlık olduğundan tek tek her video parçayı önbelleğe alınır. Örneğin, belirli bir video izlenen, ilk kez kişi yalnızca birkaç saniye izlemeyi geçici olarak atlarsa vardır ve burada yalnızca kişi izlenen ile ilişkili video parçasının CDN'de önbelleğe. Uyarlamalı akış ile video 5-7 farklı bit hızlarında genellikle sahiptir. Bir kişinin tek bit hızlı izliyor ve başka bir kişiye farklı bir bit hızı izliyor, ardından bunların her ayrı olarak CDN'de önbelleğe alınır. İki kişinin aynı hızı izlerken olsa bile, farklı protokoller üzerinden akış. Her bir protokol (HLS, MPEG-DASH, kesintisiz akış) ayrı olarak önbelleğe alınır. Bu nedenle her bit hızı ve protokol önbelleğe ayrı olarak ve istenen bu video parçasının önbelleğe alınır.
 
## <a name="properties"></a>Özellikler 

Bu bölüm, akış uç noktasının özelliklerini bazıları hakkında ayrıntılar sağlar. Yeni bir akış uç noktası ve açıklamaları tüm özelliklerinin nasıl oluşturulacağını örnekleri için bkz: [akış uç noktası](https://docs.microsoft.com/rest/api/media/streamingendpoints/create). 

- `accessControl` -Bu akış uç noktası için aşağıdaki güvenlik ayarlarını yapılandırmak için kullanılır: Akamai imza üst bilgisi kimlik doğrulaması anahtarları ve bu uç noktaya bağlanmak için izin verilen IP adresleri.<br />Bu özellik zaman ayarlanabilir `cdnEnabled` false olarak ayarlanır.
- `cdnEnabled` -Bu akış uç noktası için Azure CDN tümleştirmesi etkin (varsayılan olarak devredışı) olup olmadığını gösterir. Ayarlarsanız `cdnEnabled` true olarak, aşağıdaki yapılandırmaları devre dışı: `customHostNames` ve `accessControl`.
  
    Tüm veri merkezlerini, Azure CDN tümleştirmesini desteklemiyor. Veri merkezinizde Azure CDN tümleştirmesi kullanılabilir sahip olup olmadığını denetlemek için aşağıdakileri yapın:
 
  - Ayarlamaya `cdnEnabled` true.
  - Denetlemek için döndürülen sonuç bir `HTTP Error Code 412` (PreconditionFailed), "Akış uç noktası CdnEnabled özellik CDN özelliği geçerli bölgede kullanılamıyor gibi true olarak ayarlanamaz." iletisi ile 

    Bu hatayı alırsanız veri merkezi bunu desteklemez. Başka bir veri merkezinde çalışmanız gerekir.
- `cdnProfile` - `cdnEnabled` Ayarlandığında true de geçirebilirsiniz `cdnProfile` değerleri. `cdnProfile` CDN profil adı, CDN uç noktası oluşturulacağı ' dir. Mevcut bir cdnProfile sağlamak veya yeni bir tane kullanın. Değer NULL ise ve `cdnEnabled` true, "AzureMediaStreamingPlatformCdnProfile" kullanılan varsayılan değer olan. Varsa sağlanan `cdnProfile` zaten varolan bir uç nokta altında oluşturulur. Profil mevcut değilse yeni bir profil otomatik olarak oluşturulur.
- `cdnProvider` -CDN etkinleştirildiğinde de geçirebilirsiniz `cdnProvider` değerleri. `cdnProvider` hangi sağlayıcısı kullanılacak denetler. Şu anda üç değerleri desteklenir: "StandardVerizon", "PremiumVerizon" ve "StandardAkamai". Hiçbir değer sağlanmışsa ve `cdnEnabled` true ise "StandardVerizon" (varsayılan değer olan) kullanılır.
- `crossSiteAccessPolicies` -Çeşitli istemciler için erişim ilkeleri siteler arası belirtmek için kullanılır. Daha fazla bilgi için [etki alanları arası ilke dosyası belirtimi](https://www.adobe.com/devnet/articles/crossdomain_policy_file_spec.html) ve [bir hizmet üzerinden etki alanı sınırlarında kullanılabilir hale getirme](https://msdn.microsoft.com/library/cc197955\(v=vs.95\).aspx).
- `customHostNames` -Bir akış için bir özel konak adı yönlendirilmiş trafiğini kabul edecek şekilde uç noktası yapılandırmak için kullanılır.  Bu özellik standart ve Premium akış uç noktaları için geçerlidir ve ne zaman ayarlanabilir `cdnEnabled`: false.
    
    Etki alanı sahipliğini, Media Services tarafından onaylanmalıdır. Media Services gerektirerek etki alanı adı sahipliğini doğrulayan bir `CName` Media Services hesabı kimliği olarak kullanılan etki alanına eklenmesi için bir bileşen içeren kayıt. Örneğin, "bir kayıt için akış uç noktası için bir özel konak adı olarak kullanılacak sports.contoso.com" için `<accountId>.contoso.com` Media Services doğrulama ana bilgisayar adlarından birini işaret edecek şekilde yapılandırılması gerekir. Doğrulama ana bilgisayar adı verifydns oluşur. \<mediaservices dns bölgesi >. 

    Farklı Azure bölgelerine için doğrulama kaydında kullanılacak beklenen DNS bölgeleri şunlardır:
  
  - Kuzey Amerika, Avrupa, Singapur, Hong Kong, Japonya:
      
    - `media.azure.net`
    - `verifydns.media.azure.net`
      
  - Çin'de:
        
    - `mediaservices.chinacloudapi.cn`
    - `verifydns.mediaservices.chinacloudapi.cn`
        
    Örneğin, bir `CName` "945a4c4e-28ea-45 cd-8ccb-a519f6b700ad.contoso.com" için "verifydns.media.azure.net" eşleyen kaydı kanıtlar medya Hizmetleri kimliği 945a4c4e-28ea-45cd-8ccb-a519f6b700ad bu nedenle contoso.com etki alanının sahipliğini sahip Bu hesap altında bir akış uç noktası için bir özel konak adı olarak kullanılacak contoso.com altındaki herhangi bir ad etkinleştiriliyor. Medya hizmeti kimlik değerini bulmak için Git [Azure portalında](https://portal.azure.com/) ve medya hizmeti hesabınızı seçin. **Hesap kimliği** üstte görünür sayfanın sağ.
        
    Uygun bir doğrulaması olmadan bir özel konak adı ayarlama girişimi varsa `CName` kaydı, DNS yanıtının başarısız olur ve bir süre sonra önbelleğe alınabilir. Uygun bir kayıt yerleştirildikten sonra önbelleğe alınan yanıtın yeniden doğrulanır kadar biraz sürebilir. Özel etki alanı için DNS sağlayıcıya bağlı olarak, herhangi bir yere birkaç dakika veya saat kaydı düzeltin için ele geçirebilir.
        
    Ek olarak `CName` eşleyen `<accountId>.<parent domain>` için `verifydns.<mediaservices-dns-zone>`, başka oluşturmalısınız `CName` özel ana bilgisayar adı eşleyen (örneğin, `sports.contoso.com`), medya Hizmetleri akış uç noktanın ana bilgisayar adı (örneğin, `amstest-usea.streaming.media.azure.net`).
 
    > [!NOTE]
    > Akış uç noktaları aynı veri merkezinde bulunan paylaşamaz aynı özel ana bilgisayar adı.

    Şu anda, Media Services, SSL ile özel etki alanlarını desteklemiyor. 
    
- `maxCacheAge` -Geçersiz kılmalar varsayılan max-age HTTP önbellek denetimi akış uç noktasında medya parçasının ve isteğe bağlı bildirimlerini tarafından ayarlanan başlığı. Saniye cinsinden değeri ayarlanır.
- `resourceState` -

    - Durduruldu - akış uç noktası oluşturulduktan sonra başlangıç durumu
    - Çalışır duruma başlangıç - na geçiyor
    - -Çalışan istemciler için içerik akışı için
    - Ölçeklendirme - birimleri güncellenmekte ölçek artırabilir veya azaltılabilir
    - Durdurma - durdurulmuş duruma geçiş
    - Silme - siliniyor
    
- `scaleUnits ` -200 MB/sn'lik artışlarla satın alınabilir adanmış çıkış kapasitesi sağlar. Taşımak gerekiyorsa bir **Premium** yazın, ayarlamak `scaleUnits`.

## <a name="next-steps"></a>Sonraki adımlar

Örnek [bu depodaki](https://github.com/Azure-Samples/media-services-v3-dotnet-quickstarts/blob/master/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs) varsayılan akış uç .NET ile başlatmak gösterilmektedir.

