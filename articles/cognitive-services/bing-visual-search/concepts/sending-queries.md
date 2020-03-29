---
title: Bing Görsel Arama API'sine arama sorguları gönderme
titleSuffix: Azure Cognitive Services
description: Bu makalede, Bing Görsel Arama API'sine gönderilen isteklerin parametreleri ve özniteliklerinin yanı sıra yanıt nesnesi açıklanmaktadır.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: conceptual
ms.date: 01/08/2019
ms.author: aahi
ms.openlocfilehash: 5d27aa80a63232694e1c9951f98b2191ba575e74
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "75913065"
---
# <a name="sending-search-queries-to-the-bing-visual-search-api"></a>Bing Görsel Arama API'sine arama sorguları gönderme

Bu makalede, Bing Görsel Arama API'sine gönderilen isteklerin parametreleri ve özniteliklerinin yanı sıra yanıt nesnesi açıklanmaktadır. 

Bir resim hakkında üç şekilde bilgi edinebilirsiniz:

- Önceki bir çağrıdaki bir görüntüden [Bing Resim Arama API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference) uç noktalarından birine aldığınız bir öngörü belirteci kullanma.
- Resmin URL'sini gönderme.
- Görüntü yükleme (ikili biçimde).

## <a name="bing-visual-search-requests"></a>Bing Görsel Arama istekleri

Görsel Arama'ya bir resim belirteci veya URL gönderirseniz, aşağıdaki parçacık POST'un gövdesine eklemeniz gereken JSON nesnesini gösterir:

```json
{
    "imageInfo" : {
        "url" : "",
        "imageInsightsToken" : "",
        "cropArea" : {
            "top" : 0.1,
            "left" : 0.5,
            "right" : 0.9,
            "bottom" : 0.9
        }
    },
    "knowledgeRequest" : {
      "filters" : {
        "site" : ""
      }
    }
}
```

`imageInfo` nesnesi `url` veya `imageInsightsToken` alanını içermelidir ama ikisini birden içermemelidir. `url` Alanı Internet'e erişilebilen bir görüntünün URL'sine ayarlayın. Desteklenen resim boyutu üst sınırı 1 MB'tır.

`imageInsightsToken`, içgörü belirtecine ayarlanmalıdır. İçgörü belirtecini almak için, Bing Resim API'sini çağırın. Yanıt bir `Image` nesneleri listesi içerir. Her `Image` nesnesinde belirteci içeren bir `imageInsightsToken` alanı vardır.

`cropArea` alanı isteğe bağlıdır. Kırpma alanı, ilgi çeken bir bölgenin sol üst köşesini ve sağ alt köşesini belirtir. 0,0 ile 1,0 arasında değerler belirtin. Değerler bütün genişliğin veya yüksekliğin oranıdır. Örneğin, yukarıdaki örnek ilgilenilen bölge olarak resmin sağ yarısını işaretler. İçgörü isteğini ilgilenilen bölgeyle sınırlandırmak istiyorsanız bunu ekleyin.

`filters` nesnesi, benzer resim ve benzer ürün sonuçlarını belirli bir etki alanıyla sınırlandırmak için kullanabileceğiniz bir site filtresi içerir (`site` alanına bakın). Örneğin, görüntü bir Yüzey Kitabı ise, `site` 'ye `www.microsoft.com`ayarlayabilirsiniz.

Bir resmin yerel kopyasıyla ilgili içgörüler almak istiyorsanız, resmi ikili veri olarak karşıya yükleyin.

Bu seçenekleri POST’un gövdesine ekleme hakkındaki ayrıntılar için bkz. [İçerik formu türleri](#content-form-types).

### <a name="search-endpoint"></a>Arama bitiş noktası

Görsel Arama uç noktası şudur: https:\/\/api.cognitive.microsoft.com/bing/v7.0/images/visualsearch.

İstekler yalnızca HTTP POST istekleri olarak gönderilmelidir.

### <a name="query-parameters"></a>Sorgu parametreleri

Aşağıdakiler, isteğinizde belirtilmesi gereken sorgu parametreleridir. En azından sorgu parametresini `mkt` eklemeniz gerekir:

| Adı | Değer | Tür | Gerekli |
| --- | --- | --- | --- |
| <a name="cc" />cc  | Sonuçların nereden geldiğini gösteren iki karakterli ülke kodu.<br /><br /> Bu parametreyi ayarlarsanız, [Accept-Language](#acceptlanguage) üst bilgisini de belirtmelisiniz. Bing dil listesinde bulduğu ilk desteklenen dili kullanır ve dili sizin belirttiğiniz ülke koduyla birleştirerek sonuçları hangi pazardan döndüreceğini saptar. Dil listesi desteklenen bir dil içermiyorsa, Bing isteği destekleyen en yakın dili ve pazarı bulur. Öte yandan, sonuçlarda belirtilen pazar yerine toplu veya varsayılan bir pazarı da kullanılabilir.<br /><br /> Bu sorgu parametresini ve `Accept-Language` sorgu parametresini ancak birden çok dil belirttiyseniz kullanmalısınız; aksi takdirde `mkt` ve `setLang` sorgu parametrelerini kullanmanız gerekir.<br /><br /> Bu parametre ve [](#mkt)mkt&mdash; sorgu parametresi karşılıklı olarak birbirini dışlar. İkisini birlikte belirtmeyin. | Dize | Hayır       |
| <a name="mkt" />mkt   | Sonuçların geldiği pazar. <br /><br /> **NOT:** Eğer biliniyorsa, her zaman piyasayı belirtmelisiniz. Pazarın belirtilmesi Bing’in isteği yönlendirmesine, uygun ve en iyi yanıtı döndürmesine yardımcı olur.<br /><br /> Bu parametre ve [](#cc)cc&mdash; sorgu parametresi karşılıklı olarak birbirini dışlar. İkisini birlikte belirtmeyin. | Dize | Evet      |
| <a name="safesearch" />safeSearch | Yetişkinlere uygun içerik için bir filtre. Aşağıdakiler, büyük/küçük harfe duyarlı olmayan olası filtre değerleridir.<br /><ul><li>Off&mdash;Yetişkinlere yönelik metin veya resim içeren web sayfalarını döndürür.<br /><br/></li><li>Moderate&mdash;Yetişkinlere yönelik metin içeren web sayfalarını döndürür ama yetişkinlere yönelik resim içerenleri döndürmez.<br /><br/></li><li>Strict&mdash;Yetişkinlere yönelik metin veya resim içeren web sayfalarını döndürmez.</li></ul><br /> Varsayılan ayar Moderate değeridir.<br /><br /> **NOT:** İstek, Bing'in yetişkinlere yönelik içerik ilkesinin `safeSearch` parametresinde Strict ayarlanmasını gerektirdiği bir pazardan geliyorsa, Bing `safeSearch` değerini yoksayar ve Strict değerini kullanır.<br/><br/>**NOT:** Sorgu işleci `site:` kullanırsanız, `safeSearch` sorgu parametresi ne olarak ayarlanırsa ayarlanmasın yanıtın yetişkinlere uygun içerik içerme olasılığı vardır. `site:` işlecini yalnızca sitenin içeriği hakkında bilgi sahibiyseniz ve senaryonuz, yetişkinlere yönelik içeriğin mevcut olma ihtimalini destekliyorsa kullanın.  | Dize | Hayır       |
| <a name="setlang" />setLang  | Kullanıcı arabirimi dizelerinde kullanılacak dil. ISO 639-1 iki harfli dil kodunu kullanarak dili belirtin. Örneğin, Türkçe için dil kodu TR'dir. Varsayılan değer EN (İngilizce) ayarıdır.<br /><br /> İsteğe bağlı olsa da, her zaman dil belirtmelisiniz. Kullanıcı tarafından kullanıcı arabirimi dizelerinin farklı dilde görüntülenmesi istenmediği sürece, normalde `setLang` parametresini `mkt` parametresiyle aynı dile ayarlarsınız.<br /><br /> Bu parametre ve [](#acceptlanguage)Accept-Language&mdash; üst bilgisi karşılıklı olarak birbirini dışlar. İkisini birlikte belirtmeyin.<br /><br /> Kullanıcı arabirimi dizesi, kullanıcı arabiriminde etiket olarak kullanılan dizedir. JSON yanıt nesnelerinde çok az kullanıcı arabirimi dizesi vardır. Ayrıca, yanıt nesnelerinde Bing.com özelliklerine yönelik bağlantılar da belirtilen dildedir. | Dize | Hayır   |

## <a name="headers"></a>Üst bilgiler

Aşağıdakiler, isteğinizde belirtilmesi gereken üst bilgilerdir. Ve `Content-Type` `Ocp-Apim-Subscription-Key` üstbilgi yalnızca gerekli üstbilgidir, ancak `User-Agent`, `X-MSEdge-ClientID` `X-MSEdge-ClientIP`, `X-Search-Location`, ve .

| Üst bilgi | Açıklama |
| --- | --- |
| <a name="acceptlanguage" />Accept-Language  | İsteğe bağlı istek üst bilgisi.<br /><br /> Kullanıcı arabirimi dizelerinde kullanılacak virgülle sınırlanmış bir dil listesi. Liste, tercih edilme durumuna göre azalan düzende sıralanır. Beklenen biçim de içinde olmak üzere daha fazla bilgi için bkz. [RFC2616](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).<br /><br /> Bu üst bilgi ve [](#setlang)setLang&mdash; sorgu parametresi karşılıklı olarak birbirini dışlar. İkisini birlikte belirtmeyin.<br /><br /> Bu üst bilgiyi ayarlarsanız, [cc](#cc) sorgu parametresini de belirtmelisiniz. Hangi pazardan sonuç döndürüleceğini belirlemek için, Bing listeden bulduğu ilk desteklenen dili kullanır ve bunu `cc` parametresinin değeriyle birleştirir. Liste desteklenen bir dil içermiyorsa, Bing isteği destekleyen en yakın dili ve pazarı bulur ya da sonuçlar için toplu veya varsayılan bir pazar kullanır. Bing'in kullandığı pazarı belirlemek `BingAPIs-Market` için üstbilgiye bakın.<br /><br /> Ancak birden çok dil belirtirseniz bu üst bilgiyi ve `cc` sorgu parametresini kullanın. Aksi takdirde, [mkt](#mkt) ile [setLang](#setlang) sorgu parametrelerini kullanın.<br /><br /> Kullanıcı arabirimi dizesi, kullanıcı arabiriminde etiket olarak kullanılan dizedir. JSON yanıt nesnelerinde çok az kullanıcı arabirimi dizesi vardır. Yanıt nesnelerinde Bing.com özelliklerine yönelik bağlantılar da belirtilen dildedir.  |
| <a name="contenttype" />Content-Type  |     |
| <a name="market" />BingAPIs-Market    | Yanıt üst bilgisi.<br /><br /> İstek tarafından kullanılan pazar. Biçimi şöyledir: \<languageCode\>-\<countryCode\>. Örneğin, tr-TR.  |
| <a name="traceid" />BingAPIs-TraceId  | Yanıt üst bilgisi.<br /><br /> İsteğin ayrıntılarını içeren günlük girdisinin kimliği. Hata oluştuğunda, bu kimliği yakalayın. Sorunu belirleyemez ve çözemezseniz, Destek ekibine diğer bilgilerle birlikte bu kimliği de sağlayın. |
| <a name="subscriptionkey" />Ocp-Apim-Subscription-Key | Gerekli istek üst bilgisi.<br /><br /> [Bilişsel Hizmetler](https://www.microsoft.com/cognitive-services/)'de bu hizmete kaydolduğunuzda aldığınız abonelik anahtarı. |
| <a name="pragma" />Pragma |   |
| <a name="useragent" />User-Agent  | İsteğe bağlı istek üst bilgisi.<br /><br /> İsteği başlatan kullanıcı aracısı. Bing, mobil kullanıcılara iyileştirilmiş bir deneyim sağlamak için kullanıcı aracısını kullanır. İsteğe bağlı olsa da, bu üst bilgiyi her zaman belirtmeniz önerilir.<br /><br /> User-agent, yaygın olarak kullanılan tarayıcılardan gönderilen dizeyle aynı olmalıdır. Kullanıcı aracıları hakkında daha fazla bilgi için [RFC 2616'ya](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html)bakın.<br /><br /> Aşağıda örnek user-agent dizelerini bulabilirsiniz.<br /><ul><li>Windows Phone&mdash;Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)<br /><br /></li><li>Android&mdash;Mozilla/5.0 (Linux; U; Android 2.3.5; en-us; SCH-I500 Build/GINGERBREAD) AppleWebKit/533.1 (KHTML; like Gecko) Version/4.0 Mobile Safari/533.1<br /><br /></li><li>iPhone&mdash;Mozilla/5.0 (iPhone; CPU iPhone OS 6_1 like Mac OS X) AppleWebKit/536.26 (KHTML; like Gecko) Mobile/10B142 iPhone4;1 BingWeb/3.03.1428.20120423<br /><br /></li><li>PC&mdash;Mozilla/5.0 (Windows NT 6.3; WOW64; Trident/7.0; Touch; rv:11.0) like Gecko<br /><br /></li><li>iPad&mdash;Mozilla/5.0 (iPad; CPU OS 7_0 like Mac OS X) AppleWebKit/537.51.1 (KHTML, like Gecko) Version/7.0 Mobile/11A465 Safari/9537.53</li></ul>      |
| <a name="clientid" />X-MSEdge-ClientID  | İsteğe bağlı istek ve yanıt üst bilgisi.<br /><br /> Bing, kullanıcılara tüm Bing API çağrılarında tutarlı bir davranış sağlamak için bu üst bilgiyi kullanır. Bing sık sık yeni özellikler ve geliştirmeler dağıtır ve farklı dağıtımlarda trafik ataması yapmak için anahtar olarak istemci kimliğini kullanır. Bir kullanıcı için birden çok istekte aynı istemci kimliğini kullanmazsanız, Bing kullanıcıyı birden çok çakışan dağıtıma atayabilir. Birden çok çakışan dağıtıma eklenmek, tutarsız bir kullanıcı deneyimine yol açabilir. Örneğin, ikinci isteğin dağıtım ataması ilkinden farklıysa, beklenmeyen bir deneyim yaşanabilir. Ayrıca, Bing istemci kimliğini kullanarak web sonuçlarını istemci kimliğinin arama geçmişine uyarlayabilir ve bu sayede kullanıcıya daha zengin bir deneyim sağlayabilir.<br /><br /> Bing, istemci kimliği tarafından oluşturulan etkinliği analiz ederek sonuç derecelendirmelerini geliştirmeye yardımcı olması için de bu üst bilgiyi kullanabilir. İlgi geliştirmeleri Bing API'lerinin daha kaliteli sonuçlar vermesine yardımcı olur ve böylelikle API tüketicisi için daha yüksek tıklama oranları getirir.<br /><br /> **ÖNEMLİ:** İsteğe bağlı olsa da, bu üst bilgiyi gerekli olarak kabul edebilirsiniz. Aynı son kullanıcı ile cihaz bileşimi için birden çok istekte aynı istemci kimliğini kullanıldığında, 1) API tüketicisi tutarlı bir kullanıcı deneyimi elde eder ve 2) Bing API'lerinden daha kaliteli sonuçlar alındığından tıklama oranları daha yüksek olur.<br /><br /> Bu üst bilgi için geçerli olan temel kullanım kuralları şunlardır:<br /><ul><li>Cihazda uygulamanızı kullanan her kullanıcının Bing tarafından oluşturulan benzersiz bir istemci kimliği olmalıdır.<br /><br/>İsteğe bu üst bilgiyi eklemezseniz, Bing bir kimlik oluşturur ve bu kimliği X-MSEdge-ClientID yanıt üst bilgisinde döndürür. İsteğe bu üst bilgiyi EKLEMEMENİZ gereken tek durum, söz konusu cihazda kullanıcının uygulamanızı ilk kez kullanmasıdır.<br /><br/></li><li>**DİKKAT:** Bu İstemci Kimliğinin, kimliği doğrulanmış hiçbir kullanıcı hesabı bilgisine bağlanabilir olmadığından emin olmalısınız.</li><li>Cihazda uygulamanızın bu kullanıcı için yaptığı her Bing API'si isteğinde istemci kimliğini kullanın.<br /><br/></li><li>İstemci kimliğinin kalıcı olmasını sağlayın. Tarayıcı uygulamasında kimliği kalıcı hale getirmek için, tüm oturumlarda kimliğin kullanmasını sağlayacak bir kalıcı HTTP tanımlama bilgisi kullanın. Oturum tanımlama bilgisi kullanmayın. Mobil uygulamalar gibi diğer uygulamalarda, kimliği kalıcı hale getirmek için cihazın kalıcı depolamasını kullanın.<br /><br/>Kullanıcı o cihazda uygulamanızı yeniden kullandığında, kalıcı hale getirdiğiniz istemci kimliğini alın.</li></ul><br /> **NOT:** Bing yanıtları bu üst bilgiyi içerebilir veya içermeyebilir. Yanıt bu üst bilgiyi içeriyorsa, istemci kimliğini yakalayın ve o cihazda kullanıcı için bunu izleyen tüm Bing isteklerinde onu kullanın.<br /><br /> **NOT:** X-MSEdge-ClientID üst bilgisini eklerseniz, isteğe tanımlama bilgileri eklememelisiniz. |
| <a name="clientip" />X-MSEdge-ClientIP   | İsteğe bağlı istek üst bilgisi.<br /><br /> İstemci cihazının IPv4 veya IPv6 adresi. IP adresi, kullanıcının konumunu bulmak için kullanılır. Bing konum bilgisini kullanarak güvenli arama davranışını saptar.<br /><br /> **NOT:** İsteğe bağlı olsa da, bu üst bilgiyi ve X-Search-Location üst bilgisini her zaman belirtmeniz önerilir.<br /><br /> Adresi karartmayın (örneğin, son sekiz karakteri 0'la değiştirerek). Adresin karartılması, cihazın gerçek konumuna yakın olmayan bir konum sonucu verir ve bu da Bing'in hatalı sonuçlar sağlamasına yol açabilir. |
| <a name="location" />X-Search-Location   | İsteğe bağlı istek üst bilgisi.<br /><br /> İstemcinin coğrafi konumunu açıklayan noktalı virgülle sınırlanmış anahtar/değer çifti listesi. Bing konum bilgisini kullanarak güvenli arama davranışını saptar ve ilgili yerel içeriği döndürür. Anahtar/değer çiftini \<anahtar\>:\<değer\> olarak belirtin. Aşağıda, kullanıcının konumunu belirtmek için kullandığınız anahtarlar gösterilir.<br /><br /><ul><li>lat&mdash;Gerekli. İstemcinin konumunun derece cinsinden enlemi. Enlem -90,0 değerinden büyük veya bu değere eşit ve +90,0 değerinden küçük veya bu değere eşit olmalıdır. Negatif değerler güney enlemlerini ve pozitif değerler de kuzey enlemlerini gösterir.<br /><br /></li><li>long&mdash;Gerekli. İstemcinin konumunun derece cinsinden boylamı. Boylam -180,0 değerinden büyük veya bu değere eşit ve +180,0 değerinden küçük veya bu değere eşit olmalıdır. Negatif değerler batı boylamlarını ve pozitif değerler de doğu boylamlarını gösterir.<br /><br /></li><li>re&mdash;Gerekli. Koordinatların yatay doğruluğunu belirten metre cinsinden yarıçap. Cihazın konum hizmeti tarafından döndürülen değeri geçirin. Tipik değerler GPS/Wi-Fi için 22 m, baz istasyonu nirengi için 380 m ve ters IP araması için 18.000 m olabilir.<br /><br /></li><li>ts&mdash;İsteğe bağlı. İstemcinin ne zaman konumda olduğunu gösteren UTC UNIX zaman damgası. (UNIX zaman damgası 1 Ocak 1970'den başlayarak saniye sayısıdır.)<br /><br /></li><li>head&mdash;İsteğe bağlı. İstemcinin göreli seyahat yönü. Gerçek kuzeye göre saat yönünün tersine 0 ile 360 derece arasında bir seyahat yönü belirtin. Bu anahtarı ancak `sp` anahtarı sıfırdan farklı bir değer olduğunda belirtin.<br /><br /></li><li>sp&mdash;İsteğe bağlı. Saniyedeki metre cinsinden istemci cihazının seyahat ettiği yatay hız.<br /><br /></li><li>alt&mdash;İsteğe bağlı. İstemci cihazının metre cinsinden deniz seviyesinden yüksekliği.<br /><br /></li><li>are&mdash;İsteğe bağlı. Koordinatların dikey doğruluğunu belirten metre cinsinden yarıçap. Bu anahtarı ancak `alt` anahtarı belirttiğiniz durumda belirtin.<br /><br /></li></ul> **NOT:** Anahtarların çoğu isteğe bağlı olsa da, ne kadar çok bilgi sağlarsanız konum sonuçları o kadar doğru olur.<br /><br /> **NOT:** İsteğe bağlı olsa da, kullanıcının coğrafi konumunu her zaman belirtmeniz önerilir. İstemcinin IP adresi kullanıcının fiziksel konumunu doğru yansıtmıyorsa (örneğin istemci VPN kullanıyorsa), konumun belirtilmesi özellikle önemlidir. En iyi sonuçlar için, bu üstbilgi ve `X-MSEdge-ClientIP` üstbilgi içermelidir, ancak en azından, bu üstbilgi içermelidir.       |

> [!NOTE]
> [Bing Arama API kullanımı ve görüntüleme gereksinimlerinin,](../../bing-web-search/use-display-requirements.md) bu üstbilginin kullanımı da dahil olmak üzere geçerli tüm yasalara uyulmasını gerektirdiğini unutmayın. Örneğin, Avrupa gibi bazı yasama bölgelerinde kullanıcı cihazlarına izleme cihazları takmadan önce kullanıcının iznini almak gerekir.

<a name="content-form-types" />

### <a name="content-form-types"></a>İçerik formu türleri

Her istek üstbilgi içermelidir. `Content-Type` Üstbilgi şu şekilde `multipart/form-data; boundary=\<boundary string\>`ayarlanmalıdır: \<,\> sınır dizesi, form verilerinin sınırını tanımlayan benzersiz, opak bir dizedir. Örneğin, `boundary=boundary_1234-abcd`.

Görsel Arama'ya bir resim belirteci veya URL gönderirseniz, aşağıdaki parçacık POST'un gövdesine eklemeniz gereken form verilerini gösterir. Form verilerinin üstbilgiyi `Content-Disposition` içermesi ve `name` parametresini "knowledgeRequest" olarak ayarlamanız gerekir. `imageInfo` Nesne yle ilgili ayrıntılar için isteğe bakın.

```
--boundary_1234-abcd
Content-Disposition: form-data; name="knowledgeRequest"

{
    "imageInfo" : {
        "url" : "https://contoso.com/2018/05/fashion/red.jpg"
    }
}

--boundary_1234-abcd--
```

Web bağlantıları ve `enableEntityData` atıf bilgileri de dahil `true` olmak üzere, yüklediğiniz resimdeki ana varlık hakkında ayrıntılı bilgi almak için üstbilgideki özniteliği isteğe bağlı olarak ayarlayabilirsiniz. Bu alan `false` varsayılan olarak.

```
--boundary_1234-abcd
Content-Disposition: form-data; name="knowledgeRequest"

{
  "imageInfo" : {
      "url" : "https://contoso.com/2018/05/fashion/red.jpg"
  },
  "knowledgeRequest" : {
    "invokedSkillsRequestData" : {
        "enableEntityData" : "true"
    }
  }
}

--boundary_1234-abcd--
```

Yerel bir resim yüklerseniz, aşağıdaki parçacık POST'un gövdesine eklemeniz gereken form verilerini gösterir. Form verileri üstbilgi `Content-Disposition` içermelidir. `name` parametresi "image" olarak ayarlanmalıdır, `filename` parametresi ise herhangi bir dizeye ayarlanabilir. Üstbilgi, `Content-Type` yaygın olarak kullanılan herhangi bir görüntü pandomim türüne ayarlanabilir. Formun içeriği görüntünün ikili verileridir. Karşıya yükleyebileceğiniz resim boyutu üst sınırı 1 MB'tır. Genişlik veya yükseklik üst sınırı ise 1.500 pikseldir.

```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"
Content-Type: image/jpeg

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

Aşağıdaki parçacık, yüklenen bir resmin ilgi alanının nasıl belirtilen şekilde belirtilir:

```
--boundary_1234-abcd
Content-Disposition: form-data; name="knowledgeRequest"

{
    "imageInfo" : {
        "cropArea" : {
            "top" : 0.2,
            "left" : 0.3,
            "bottom" : 0.7,
            "right" : 0.6
        }
    }
}

--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="image"
Content-Type: image/jpeg


ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

### <a name="example-request"></a>Örnek istek

Aşağıdaki snippet, görüntü belirteci ve ilgi alanı geçen tam bir görüntü öngörüleri isteğini gösterir. Önceki bir aramadan /resimlere/arama'ya yapılan bilgilerden belirteçler alırsınız:

```  
POST https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch?mkt=en-us HTTP/1.1  
Content-Type: multipart/form-data; boundary=boundary_1234-abcd
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com 

--boundary_1234-abcd
Content-Disposition: form-data; name="knowledgeRequest"

{
    "imageInfo" : {
        "imageInsightsToken" : "mid_D6426898706EC7..."
        "cropArea" : {
            "top" : 0.1,
            "left" : 0.2,
            "bottom" : 0.7,
            "right" : 0.5
        }
    }
}

--boundary_1234-abcd--
```

## <a name="bing-visual-search-responses"></a>Bing Görsel Arama yanıtları


[!INCLUDE [cognitive-services-bing-url-note](../../../../includes/cognitive-services-bing-url-note.md)]

Resim için içgörüler varsa, yanıtta içgörüleri içeren bir veya birden çok `tags` bulunur. Alan, `image` giriş görüntüsü için öngörü belirteci içerir:

```json
{
  "_type" : "ImageKnowledge",
  "tags" : [
    {...},
    {...},
    {...},
    {...},
    {...}
  ],
  "image" : {
    "imageInsightsToken" : "bcid_AF8C9CA409421B..."
  }
}
```

`tags` alanı, görünen adı ve eylem (içgörü) listesini içerir. Etiketlerden birinin boş dizeye ayarlanmış bir `displayName` alanı vardır. Bu etiket resmi, görsel olarak benzer resimleri ve resimde bulunan öğelerin alışveriş kaynaklarını içeren web sayfaları gibi varsayılan içgörüleri içerir. Resmin tamamı ilgi çekici olduğundan, varsayılan öngörüler etiketi, ilgi çeken bölgeler için sınırlayıcı kutuları içermez:

```json
{
  "_type" : "ImageKnowledge",
  "tags" : [
    {
      "displayName" : "",
      "actions" : [
        {...},
        {...},
        {...},
        {...}
      ]
    },
    {...},
    {...},
    {...},
    {...}
  ],
  "image" : {
    "imageInsightsToken" : "bcid_AF8C9CA409421B..."
  }
}
```

Varsayılan öngörülerin listesi için [Varsayılan öngörüler etiketine](../default-insights-tag.md)bakın.

Kalan etiketler kullanıcının ilgisini çekebilecek diğer içgörüleri içerir. Örneğin resimde metin varsa, etiketlerden biri tanınan metnin bulunduğu TextResults içgörüsünü içerebilir. Veya Bing görüntüde bir varlığı (yani kültürel olarak iyi bilinen/popüler bir kişi, yer veya başka bir şey) tanırsa, etiketlerden biri varlığı tanımlayabilir. Görsel Arama giriş resminden türetilen çeşitli terimler (etiketler) de döndürür. Bu etiketler, kullanıcıların resimde bulunan kavramları keşfetmesini sağlar. Örneğin giriş resmi ünlü bir sporcuya aitse, etiketlerden biri Spor olabilir ve spor resimlerinin bağlantılarını içerebilir.

Her etiket içgörüyü kategorilere ayırmak için kullanabileceğiniz bir görünen adı, içgörünün geçerli olduğu ilgilenilen bölgeyi tanımlayan sınırlayıcı kutuyu, içgörüleri ve resmin bir küçük resmini içerir. Örneğin resim spor forması giymiş birine aitse, etiketlerden biri formayı sınırlayan bir sınırlayıcı kutu ile VisualSearch ve ProductVisualSearch içgörülerini içerebilir. Bir diğer etikette de konuları bakımından birbiriyle ilgili resimleri almaya yönelik /images/search API isteğinin URL'sini içeren bir ImageResults içgörüsü veya kullanıcıyı Bing.com resim arama sonuçlarına götüren Bing.com arama URL'si bulunabilir.

Varsayılan içgörü etiketleri dışındaki tüm etiketlerde, resimde ilgilenilen bölgeyi tanımlayan sınırlayıcı kutular vardır. Örneğin resimde tanınan birden çok kişi varsa, etiketler kişilerden her biri için sınırlayıcı kutular içerebilir veya resimde tanınan giyim eşyaları varsa, etiketlerde tanınan her giyim eşyası için sınırlayıcı kutular bulunabilir. Sınırlayıcı kutuları kullanarak, tıklandığında resmin ilgili bölgesinin içeriği hakkında ayrıntılar sağlayan etkin noktalar oluşturabilirsiniz. Resmin tamamını tanımlayan sınırlayıcı kutular için etkin nokta eklememelisiniz.

### <a name="text-recognition"></a>Metin tanıma

Resim hizmetin tanıdığı metinler içeriyorsa, etiketlerden biri bir TextResults içgörüsü (eylemi) içerir. Insight's `displayName` tanınan metin içerir:

```json
    {
        "image" : {
            "thumbnailUrl" : "https:\/\/tse3.mm.bing.net\/th?q=%23%23Text..."
        },
        "displayName" : "##TextRecognition",
        "boundingBox" : {
            "queryRectangle" : {
                "topLeft" : {"x" : 0, "y" : 0},
                "topRight" : {"x" : 1, "y" : 0},
                "bottomRight" : {"x" : 1, "y" : 1},
                "bottomLeft" : {"x" : 0, "y" : 1}
            },
            "displayRectangle" : {
                "topLeft" : {"x" : 0, "y" : 0},
                "topRight" : {"x" : 1, "y" : 0},
                "bottomRight" : {"x" : 1, "y" : 1},
                "bottomLeft" : {"x" : 0, "y" : 1}
            }
        },
        "actions" : [{
            "displayName" : "WALK BIKE ACROSS BRIDGE",
            "actionType" : "TextResults"
        }],
        "sources" : ["OCR"]
    }
```

Etiketin `displayName` alanı ##TextRecognition öğesini içereceğinden, UX'te bunu bir kategori başlığı olarak kullanmayın. Bu durum ## ile başlayan tüm görünen adlar için geçerlidir. Bunun yerine, eylemin görüntü adını kullanın.

Metin tanıma, kartvizitlerin üzerindeki telefon numarası ve e-posta adresi gibi kişi bilgilerini de tanıyabilir. Sınırlayıcı kutu, kartvizitte kişi bilgilerinin konumunu belirler.

```json
    {
      "image" : {
        "thumbnailUrl" : "https:\/\/tse3.mm.bing.net\/th?q=%23%23TextRecognition..."
      },
      "displayName" : "##TextRecognition",
      "boundingBox" : {
        "queryRectangle" : {
          "topLeft" : {"x" : 0.635, "y" : 0},
          "topRight" : {"x" : 0.77, "y" : 0},
          "bottomRight" : {"x" : 0.77, "y" : 0.4873333},
          "bottomLeft" : {"x" : 0.635, "y" : 0.4873333}
        },
        "displayRectangle" : {
          "topLeft" : {"x" : 0.635, "y" : 0},
          "topRight" : {"x" : 0.77, "y" : 0},
          "bottomRight" : {"x" : 0.77, "y" : 0.4873333},
          "bottomLeft" : {"x" : 0.635, "y" : 0.4873333}
        }
      },
      "actions" : [
        {
          "url" : "tel:888%20555%201212",
          "actionType" : "Uri"
        }
      ],
      "sources" : ["OCR"]
    },
    {
      "image" : {
        "thumbnailUrl" : "https:\/\/tse3.mm.bing.net\/th?q=%23%23TextRecognition..."
      },
      "displayName" : "##TextRecognition",
      "boundingBox" : {
        "queryRectangle" : {
          "topLeft" : {"x" : 0.63, "y" : 0},
          "topRight" : {"x" : 0.866, "y" : 0},
          "bottomRight" : {"x" : 0.866, "y" : 0.5553334},
          "bottomLeft" : {"x" : 0.63, "y" : 0.5553334}
        },
        "displayRectangle" : {
          "topLeft" : {"x" : 0.63, "y" : 0},
          "topRight" : {"x" : 0.866, "y" : 0},
          "bottomRight" : {"x" : 0.866, "y" : 0.5553334},
          "bottomLeft" : {"x" : 0.63, "y" : 0.5553334}
        }
      },
      "actions" : [
        {
          "url" : "mailto:someone@outlook.com",
          "actionType" : "Uri"
        }
      ],
      "sources" : ["OCR"]
    },
    {
      "image" : {
        "thumbnailUrl" : "https:\/\/tse3.mm.bing.net\/th?q=%23%23TextRecognition..."
      },
      "displayName" : "##TextRecognition",
      "boundingBox" : {
        "queryRectangle" : {
          "topLeft" : {"x" : 0, "y" : 0},
          "topRight" : {"x" : 1, "y" : 0},
          "bottomRight" : {"x" : 1, "y" : 1},
          "bottomLeft" : {"x" : 0, "y" : 1}
        },
        "displayRectangle" : {
          "topLeft" : {"x" : 0, "y" : 0},
          "topRight" : {"x" : 1, "y" : 0},
          "bottomRight" : {"x" : 1, "y" : 1},
          "bottomLeft" : {"x" : 0, "y" : 1}
        }
      },
      "actions" : [
        {
          "displayName" : "CHARLENE WHITNEY Graphic Designer 888 555 1212 someone@outlook.com www.contoso.com",
          "actionType" : "TextResults"
        }
      ],
      "sources" : ["OCR"]
    }
```

Görüntü, kültürel olarak iyi bilinen/popüler bir kişi, yer veya başka bir şey gibi tanınmış bir varlık içeriyorsa, etiketlerden biri Varlık öngörüsü içerebilir. Ve `mainEntity` `data` alanlar yalnızca `enableEntityData` `Content-Type` üstbilgideki öznitelik . `true`

```json
{
  "image" : {
    "thumbnailUrl" : "https:\/\/tse4.mm.bing.net\/th?q=Statue+of+Liberty..."
  },
  "displayName" : "Statue of Liberty",
  "boundingBox" : {
    "queryRectangle" : {
      "topLeft" : {"x" : 0.40625, "y" : 0.1757813},
      "topRight" : {"x" : 0.6171875, "y" : 0.1757813},
      "bottomRight" : {"x" : 0.6171875, "y" : 0.3867188},
      "bottomLeft" : {"x" : 0.40625, "y" : 0.3867188}
    },
    "displayRectangle" : {
      "topLeft" : {"x" : 0.40625, "y" : 0.1757813},
      "topRight" : {"x" : 0.6171875, "y" : 0.1757813},
      "bottomRight" : {"x" : 0.6171875, "y" : 0.3867188},
      "bottomLeft" : {"x" : 0.40625, "y" : 0.3867188}
    }
  },
  "actions" : [
    {
      "_type" : "ImageEntityAction",
      "webSearchUrl" : "https:\/\/www.bing.com\/search?q=Statue+of+Liberty",
      "displayName" : "Statue of Liberty",
      "actionType" : "Entity",
      "mainEntity" : {
        "name" = "Statue of liberty",
        "bingId" : "..."
      },
      "data" : {
        "id" : "https://api.cognitive.microsoft.com/api/v7/entities/...",
        "readLink": "https://www.bingapis.com/api/v7/search?q=...",
        "readLinkPingSuffix": "...",
        "contractualRules": [
          {
            "_type": "ContractualRules/LicenseAttribution",
            "targetPropertyName": "description",
            "mustBeCloseToContent": true,
            "license": {
                "name": "CC-BY-SA",
                "url": "http://creativecommons.org/licenses/by-sa/3.0/",
                "urlPingSuffix": "..."
            },
            "licenseNotice": "Text under CC-BY-SA license"
          },
          {
            "_type": "ContractualRules/LinkAttribution",
            "targetPropertyName": "description",
            "mustBeCloseToContent": true,
            "text": "Wikipedia",
            "url": "http://en.wikipedia.org/wiki/...",
            "urlPingSuffix": "..."
          }
        ],
        "webSearchUrl": "https://www.bing.com/entityexplore?q=...",
        "webSearchUrlPingSuffix": "...",
        "name": "Statue of Liberty",
        "image": {
          "thumbnailUrl": "https://tse1.mm.bing.net/th?id=...",
          "hostPageUrl": "http://upload.wikimedia.org/wikipedia/...",
          "hostPageUrlPingSuffix": "...",
          "width": 50,
          "height": 50,
          "sourceWidth": 474,
          "sourceHeight": 598
        },
        "description" : "...",
        "bingId": "..."
        }
      }
  ]
}
```

## <a name="see-also"></a>Ayrıca bkz.

- [Bing Görsel Arama API’si nedir?](../overview.md)
- [Öğretici: Görsel Arama tek sayfalık web uygulaması oluşturma](../tutorial-bing-visual-search-single-page-app.md)
