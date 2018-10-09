---
title: Proje Yanıtı Arama başvurusu
titlesuffix: Azure Cognitive Services
description: Proje yanıt arama uç noktası için başvuru.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: project-answer-search
ms.topic: reference
ms.date: 04/13/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: 1149f4d5ec0a3ef55c435d0555f944329cf5b890
ms.sourcegitcommit: 0bb8db9fe3369ee90f4a5973a69c26bff43eae00
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2018
ms.locfileid: "48869610"
---
# <a name="project-answer-search-v7-reference"></a>Proje yanıt arama v7 başvurusu

Bing yanıt SearchAPI bir sorgu parametresi alan ve döndüren bir `searchResponse` ile `answerType`: `facts` veya `entities`. 

Yanıt arama API kullanan uygulamalardan sorgu parametresi önizlemek için bir URL ile uç nokta isteği gönderin.  İstek içermelidir `q=searchTerm` parametresi ve *Ocp-Apim-Subscription-Key* başlığı.   

Olgu ve arama'nın hakkındaki ayrıntıları içeren varlıkları için JSON yanıtı ayrıştırılamaz.

## <a name="endpoint"></a>Uç Nokta
Yanıt arama sonuçları istemek için aşağıdaki uç noktaya bir istek gönderin. Daha fazla özellikleri tanımlamak için başlık ve URL parametrelerini kullanın.

GET uç noktası: 
````
https://api.labs.cognitive.microsoft.com/answerSearch/v7.0/search?q=<searchTerm>&subscription-key=0123456789ABCDEF&mkt=en-us

````

İstek, HTTPS protokolünü kullanmak ve ardından sorgu parametresi içerir:
-  q =<URL> -arama nesneyi tanımlayan sorgu

İsteğinde bulunmak nasıl gösteren örnekler için bkz: [C# hızlı](c-sharp-quickstart.md) veya [Java Hızlı Başlangıç](java-quickstart.md). 

Aşağıdaki bölümler yanıt nesneleri, sorgu parametreleri ve arama sonuçlarını etkileyecek üstbilgileri hakkında teknik ayrıntılar sağlar. 
  
İstekleri içermelidir üstbilgileri hakkında daha fazla bilgi için bkz: [üstbilgileri](#headers).  
  
İstekleri içermelidir sorgu parametreleri hakkında daha fazla bilgi için bkz: [sorgu parametreleri](#query-parameters).  
  
JSON hakkında bilgi yanıt içerdiğini nesneleri için bkz. [yanıt nesneleri](#response-objects).

Maksimum sorgu URL'SİNİN uzunluğu 2.048 karakterdir. URL uzunluğu sınırı aşmadığından emin olmak için sorgu parametrelerinizin uzunluğu en fazla 1500'den az karakter olmalıdır. URL 2.048 karakterden uzunsa sunucu 404 bulunamadı hatası döndürür.  

İzin verilen kullanım ve sonuçları görüntüleme hakkında daha fazla bilgi için bkz. [kullanın ve gereksinimlerini görüntülemek](use-display-requirements.md). 

> [!NOTE]
> URL önizlemesi, diğer arama API'lerini için anlamlı olan bazı istek üst etkilemez
> - Pragma – URL önizlemesi önbellek kullanıp kullanmadığını denetim çağırana sahip değil
> - Önbellek denetimi – çağıran URL önizlemesi önbellek kullanıp kullanmadığını denetim yok
> - Kullanıcı Aracısı

> Ayrıca, bazı parametreler için URL önizleme API'sı şu anda anlamlı değildir, ancak gelecekte geliştirilmiş Genelleştirme için kullanılabilir. 
 
## <a name="headers"></a>Üst bilgiler  
İstek ve yanıt içerebilecek üst bilgiler verilmiştir.  
  
|Üst bilgi|Açıklama|  
|------------|-----------------|  
|Kabul Ediyorum|İsteğe bağlı istek üst bilgisi.<br /><br /> Varsayılan medya türü application/json şeklindedir. Yanıt kullandığını belirtmek için [JSON-LD](http://json-ld.org/), uygulama/ld + json Accept üst bilgisi ayarlayın.|  
|<a name="acceptlanguage" />Accept-Language|İsteğe bağlı istek üst bilgisi.<br /><br /> Kullanıcı arabirimi dizelerinde kullanılacak virgülle sınırlanmış bir dil listesi. Liste, tercih edilme durumuna göre azalan düzende sıralanır. Beklenen biçim de içinde olmak üzere daha fazla bilgi için bkz. [RFC2616](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).<br /><br /> Bu üst bilgi ve [setLang](#setlang) sorgu parametresi karşılıklı olarak birbirini dışlar. İkisini birlikte belirtmeyin.<br /><br /> Bu üst bilgiyi ayarlarsanız, [cc](#cc) sorgu parametresini de belirtmelisiniz. Hangi pazardan sonuç döndürüleceğini belirlemek için, Bing listeden bulduğu ilk desteklenen dili kullanır ve bunu `cc` parametresinin değeriyle birleştirir. Liste desteklenen bir dil içermiyorsa, Bing isteği destekleyen en yakın dili ve pazarı bulur ya da sonuçlar için toplu veya varsayılan bir pazar kullanır. Bing'in kullandığı pazarı saptamak için BingAPIs-Market üst bilgisine bakın.<br /><br /> Ancak birden çok dil belirtirseniz bu üst bilgiyi ve `cc` sorgu parametresini kullanın. Aksi takdirde, [mkt](#mkt) ile [setLang](#setlang) sorgu parametrelerini kullanın.<br /><br /> Kullanıcı arabirimi dizesi, kullanıcı arabiriminde etiket olarak kullanılan dizedir. JSON yanıt nesnelerinde çok az kullanıcı arabirimi dizesi vardır. Yanıt nesnelerinde Bing.com özelliklerine yönelik bağlantılar da belirtilen dildedir.|  
|<a name="market" />BingAPIs-Market|Yanıt üst bilgisi.<br /><br /> İstek tarafından kullanılan pazar. Biçimi şöyledir: \<languageCode\>-\<countryCode\>. Örneğin, tr-TR.|  
|<a name="traceid" />BingAPIs-TraceId|Yanıt üst bilgisi.<br /><br /> İsteğin ayrıntılarını içeren günlük girdisinin kimliği. Hata oluştuğunda, bu kimliği yakalayın. Sorunu belirleyemez ve çözemezseniz, Destek ekibine diğer bilgilerle birlikte bu kimliği de sağlayın.|  
|<a name="subscriptionkey" />Ocp-Apim-Subscription-Key|Gerekli istek üst bilgisi.<br /><br /> [Bilişsel Hizmetler](https://www.microsoft.com/cognitive-services/)'de bu hizmete kaydolduğunuzda aldığınız abonelik anahtarı.|  
|<a name="pragma" />Pragma|İsteğe bağlı istek üst bilgisi<br /><br /> Varsayılan olarak, Bing önbelleğe alınmış içeriği (varsa) döndürür. Bing'in önbelleğe alınmış içeriği döndürmesini önlemek için, Pragma üst bilgisini no-cache olarak ayarlayın (örneğin, Pragma: no-cache).
|<a name="useragent" />User-Agent|İsteğe bağlı istek üst bilgisi.<br /><br /> İsteği başlatan kullanıcı aracısı. Bing, mobil kullanıcılara iyileştirilmiş bir deneyim sağlamak için kullanıcı aracısını kullanır. İsteğe bağlı olsa da, bu üst bilgiyi her zaman belirtmeniz önerilir.<br /><br /> User-agent, yaygın olarak kullanılan tarayıcılardan gönderilen dizeyle aynı olmalıdır. Kullanıcı aracıları hakkında bilgi için bkz. [RFC 2616](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).<br /><br /> Aşağıda örnek user-agent dizelerini bulabilirsiniz.<br /><ul><li>Windows Phone&mdash;Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)<br /><br /></li><li>Android&mdash;Mozilla/5.0 (Linux; U; Android 2.3.5; en-us; SCH-I500 Build/GINGERBREAD) AppleWebKit/533.1 (KHTML; like Gecko) Version/4.0 Mobile Safari/533.1<br /><br /></li><li>iPhone&mdash;Mozilla/5.0 (iPhone; CPU iPhone OS 6_1 like Mac OS X) AppleWebKit/536.26 (KHTML; like Gecko) Mobile/10B142 iPhone4;1 BingWeb/3.03.1428.20120423<br /><br /></li><li>PC&mdash;Mozilla/5.0 (Windows NT 6.3; WOW64; Trident/7.0; Touch; rv:11.0) like Gecko<br /><br /></li><li>iPad&mdash;Mozilla/5.0 (iPad; CPU OS 7_0 like Mac OS X) AppleWebKit/537.51.1 (KHTML, like Gecko) Version/7.0 Mobile/11A465 Safari/9537.53</li></ul>|
|<a name="clientid" />X-MSEdge-ClientID|İsteğe bağlı istek ve yanıt üst bilgisi.<br /><br /> Bing, kullanıcılara tüm Bing API çağrılarında tutarlı bir davranış sağlamak için bu üst bilgiyi kullanır. Bing sık sık yeni özellikler ve geliştirmeler dağıtır ve farklı dağıtımlarda trafik ataması yapmak için anahtar olarak istemci kimliğini kullanır. Bir kullanıcı için birden çok istekte aynı istemci kimliğini kullanmazsanız, Bing kullanıcıyı birden çok çakışan dağıtıma atayabilir. Birden çok çakışan dağıtıma eklenmek, tutarsız bir kullanıcı deneyimine yol açabilir. Örneğin, ikinci isteğin dağıtım ataması ilkinden farklıysa, beklenmeyen bir deneyim yaşanabilir. Ayrıca, Bing istemci kimliğini kullanarak web sonuçlarını istemci kimliğinin arama geçmişine uyarlayabilir ve bu sayede kullanıcıya daha zengin bir deneyim sağlayabilir.<br /><br /> Bing, istemci kimliği tarafından oluşturulan etkinliği analiz ederek sonuç derecelendirmelerini geliştirmeye yardımcı olması için de bu üst bilgiyi kullanabilir. İlgi geliştirmeleri Bing API'lerinin daha kaliteli sonuçlar vermesine yardımcı olur ve böylelikle API tüketicisi için daha yüksek tıklama oranları getirir.<br /><br /> **ÖNEMLİ:** İsteğe bağlı olsa da, bu üst bilgiyi gerekli olarak kabul edebilirsiniz. Aynı son kullanıcı ile cihaz bileşimi için birden çok istekte aynı istemci kimliğini kullanıldığında, 1) API tüketicisi tutarlı bir kullanıcı deneyimi elde eder ve 2) Bing API'lerinden daha kaliteli sonuçlar alındığından tıklama oranları daha yüksek olur.<br /><br /> Bu üst bilgi için geçerli olan temel kullanım kuralları şunlardır:<br /><ul><li>Cihazda uygulamanızı kullanan her kullanıcının Bing tarafından oluşturulan benzersiz bir istemci kimliği olmalıdır.<br /><br/>İsteğe bu üst bilgiyi eklemezseniz, Bing bir kimlik oluşturur ve bu kimliği X-MSEdge-ClientID yanıt üst bilgisinde döndürür. İsteğe bu üst bilgiyi EKLEMEMENİZ gereken tek durum, söz konusu cihazda kullanıcının uygulamanızı ilk kez kullanmasıdır.<br /><br/></li><li>Cihazda uygulamanızın bu kullanıcı için yaptığı her Bing API'si isteğinde istemci kimliğini kullanın.<br /><br/></li><li>**DİKKAT:** bu istemci kimliği için herhangi bir authenticatable kullanıcı hesabı bilgisi değişkenlerinden değil emin olmanız gerekir.</li><br/><li>İstemci kimliğinin kalıcı olmasını sağlayın. Tarayıcı uygulamasında kimliği kalıcı hale getirmek için, tüm oturumlarda kimliğin kullanmasını sağlayacak bir kalıcı HTTP tanımlama bilgisi kullanın. Oturum tanımlama bilgisi kullanmayın. Mobil uygulamalar gibi diğer uygulamalarda, kimliği kalıcı hale getirmek için cihazın kalıcı depolamasını kullanın.<br /><br/>Kullanıcı o cihazda uygulamanızı yeniden kullandığında, kalıcı hale getirdiğiniz istemci kimliğini alın.</li></ul><br /> **NOT:** Bing yanıtları bu üst bilgiyi içerebilir veya içermeyebilir. Yanıt bu üst bilgiyi içeriyorsa, istemci kimliğini yakalayın ve o cihazda kullanıcı için bunu izleyen tüm Bing isteklerinde onu kullanın.<br /><br /> **NOT:** X-MSEdge-ClientID üst bilgisini eklerseniz, isteğe tanımlama bilgileri eklememelisiniz.|  
|<a name="clientip" />X-MSEdge-ClientIP|İsteğe bağlı istek üst bilgisi.<br /><br /> İstemci cihazının IPv4 veya IPv6 adresi. IP adresi, kullanıcının konumunu bulmak için kullanılır. Bing konum bilgisini kullanarak güvenli arama davranışını saptar.<br /><br /> **NOT:** İsteğe bağlı olsa da, bu üst bilgiyi ve X-Search-Location üst bilgisini her zaman belirtmeniz önerilir.<br /><br /> Adresi karartmayın (örneğin, son sekiz karakteri 0'la değiştirerek). Adresin karartılması, cihazın gerçek konumuna yakın olmayan bir konum sonucu verir ve bu da Bing'in hatalı sonuçlar sağlamasına yol açabilir.|  
|<a name="location" />X-Search-Location|İsteğe bağlı istek üst bilgisi.<br /><br /> İstemcinin coğrafi konumunu açıklayan noktalı virgülle sınırlanmış anahtar/değer çifti listesi. Bing konum bilgisini kullanarak güvenli arama davranışını saptar ve ilgili yerel içeriği döndürür. Anahtar/değer çiftini \<anahtar\>:\<değer\> olarak belirtin. Aşağıda, kullanıcının konumunu belirtmek için kullandığınız anahtarlar gösterilir.<br /><br /><ul><li>LAT&mdash;derece cinsinden istemcinin konumun enlem. Enlem -90,0 değerinden büyük veya bu değere eşit ve +90,0 değerinden küçük veya bu değere eşit olmalıdır. Negatif değerler güney enlemlerini ve pozitif değerler de kuzey enlemlerini gösterir.<br /><br /></li><li>uzun&mdash;derece cinsinden istemcinin konumun boylam. Boylam -180,0 değerinden büyük veya bu değere eşit ve +180,0 değerinden küçük veya bu değere eşit olmalıdır. Negatif değerler batı boylamlarını ve pozitif değerler de doğu boylamlarını gösterir.<br /><br /></li><li>RE&mdash; koordinatları yatay doğruluğunu belirten RADIUS, ölçümleri içinde. Cihazın konum hizmeti tarafından döndürülen değeri geçirin. Normalde değerler GPS/Wi-Fi için 22 m, baz istasyonu triangülasyonu için 380 m ve ters IP araması için 18.000 m'dir.<br /><br /></li><li>TS&mdash; istemci konumunda ne zaman, UTC UNIX zaman damgası. (UNIX zaman damgası 1 Ocak 1970'den başlayarak saniye sayısıdır.)<br /><br /></li><li>head&mdash;İsteğe bağlı. İstemcinin göreli seyahat yönü. Gerçek kuzeye göre saat yönünün tersine 0 ile 360 derece arasında bir seyahat yönü belirtin. Bu anahtarı ancak `sp` anahtarı sıfırdan farklı bir değer olduğunda belirtin.<br /><br /></li><li>SP&mdash; istemci cihazı dolaşan saniye başına metre olarak yatay hız (hızlı).<br /><br /></li><li>alt&mdash; metre olarak bir istemci cihazının yüksekliği.<br /><br /></li><li>are&mdash;İsteğe bağlı. Koordinatların dikey doğruluğunu belirten metre cinsinden yarıçap. Varsayılan olarak 50 kilometre RADIUS. Bu anahtarı ancak `alt` anahtarı belirttiğiniz durumda belirtin.<br /><br /></li></ul> **Not:** Bu anahtarları isteğe bağlıdır, ancak daha doğru konuma sonucu olan sağlayan daha fazla bilgi.<br /><br /> **Not:** her zaman kullanıcının coğrafi konumu belirtmek için önerilir. İstemcinin IP adresi kullanıcının fiziksel konumunu doğru yansıtmıyorsa (örneğin istemci VPN kullanıyorsa), konumun belirtilmesi özellikle önemlidir. En iyi sonuçları elde etmek için, bu üst bilgiyi ve X-MSEdge-ClientIP üst bilgisini eklemelisiniz; ama en azından bu üst bilgiyi eklemeniz gerekir.|

> [!NOTE] 
> Kullanım Koşulları'nın, bu üst bilgilerin kullanımıyla ilgili olanlar da dahil olmak üzere tüm ilgili yasalara uymayı gerektirdiğini unutmayın. Örneğin, Avrupa gibi bazı yasama bölgelerinde kullanıcı cihazlarına izleme cihazları takmadan önce kullanıcının iznini almak gerekir.
  

## <a name="query-parameters"></a>Sorgu parametreleri  
Aşağıdaki sorgu parametreleri istek içerebilir. Gerekli Parametreler için gerekli sütununa bakın. URL gereken sorgu parametrelerine kodlayın.  
  
  
|Ad|Değer|Tür|Gerekli|  
|----------|-----------|----------|--------------|  
|<a name="mkt" />mkt|Sonuçların geldiği pazar. <br /><br />Olası Pazar değerler listesi için bkz. [Pazar kodları](#market-codes).<br /><br /> **Not:** URL önizleme API'sı şu anda yalnızca tr destekler-bize pazara çıkma sürelerini ve dili.<br /><br />|Dize|Evet|  
|<a name="query" />q|Önizleme URL'si|Dize|Evet|  
|<a name="responseformat" />responseFormat|Yanıt için kullanılacak medya türü. Büyük küçük harf duyarsız olası değerler şunlardır:<br /><ul><li>JSON</li><li>JSONLD</li></ul><br /> JSON varsayılandır. JSON hakkında bilgi yanıt içerdiğini nesneleri için bkz. [yanıt nesneleri](#response-objects).<br /><br />  JsonLd belirtirseniz, yanıt gövdesi, arama sonuçlarını içeren JSON-LD nesneler içerir. JSON-LD hakkında daha fazla bilgi için bkz. [JSON-LD](http://json-ld.org/).|Dize|Hayır|  
|<a name="safesearch" />safeSearch|Yetişkinlere yönelik içeriği filtrelemek için kullanılan bir filtre. Aşağıdakiler, büyük/küçük harfe duyarlı olmayan olası filtre değerleridir.<br /><ul><li>Kapalı&mdash;yetişkinlere yönelik metin, görüntü veya video Web sayfalarının döndürür.<br /><br/></li><li>Orta&mdash;yetişkinlere yönelik metin ancak yetişkin değil görüntü veya video Web sayfalarının döndürür.<br /><br/></li><li>Katı&mdash;yetişkinlere yönelik metin, görüntü veya video Web sayfalarının döndürmüyor.</li></ul><br /> Varsayılan ayar Moderate değeridir.<br /><br /> **Not:** isteği bir pazar geliyorsa, gerektiren bu Bing'in yetişkinlere yönelik ilke `safeSearch` ayarlanır Strıct için Bing yoksayar `safeSearch` değeri ve Strıct kullanır.<br/><br/>**NOT:** `site:` sorgu işlecini kullanmanız durumunda, `safeSearch` parametresinin ayarına bakılmaksızın yanıtta yetişkinlere yönelik içerik bulunabilir. `site:` işlecini yalnızca sitenin içeriği hakkında bilgi sahibiyseniz ve senaryonuz, yetişkinlere yönelik içeriğin mevcut olma ihtimalini destekliyorsa kullanın. |Dize|Hayır|  
|<a name="setlang" />setLang|Kullanıcı arabirimi dizelerinde kullanılacak dil. Dili belirtirken ISO 639-1 2 harfi dil kodunu kullanın. Örneğin, Türkçe için dil kodu TR'dir. Varsayılan değer EN (İngilizce) ayarıdır.<br /><br /> İsteğe bağlı olsa da, her zaman dil belirtmelisiniz. Kullanıcı tarafından kullanıcı arabirimi dizelerinin farklı dilde görüntülenmesi istenmediği sürece, normalde `setLang` parametresini `mkt` parametresiyle aynı dile ayarlarsınız.<br /><br /> Bu parametre ve [Accept-Language](#acceptlanguage) üst bilgisi karşılıklı olarak birbirini dışlar. İkisini birlikte belirtmeyin.<br /><br /> Kullanıcı arabirimi dizesi, kullanıcı arabiriminde etiket olarak kullanılan dizedir. JSON yanıt nesnelerinde çok az kullanıcı arabirimi dizesi vardır. Ayrıca, yanıt nesnelerinde Bing.com özelliklerine yönelik bağlantılar da belirtilen dildedir.|Dize|Hayır| 


## <a name="response-objects"></a>Yanıt nesneleri  
Yanıt şeması ya da bir [Web] sayfasıdır veya ErrorResponse, Web araması API'si olduğu gibi. İstek başarısız olursa, en üst düzey nesnedir [ErrorResponse](#errorresponse) nesne.


|Nesne|Açıklama|  
|------------|-----------------|  
|[Web]|Önizleme özniteliklerini içeren üst düzey JSON nesnesi.|  
|[Olay]|Bilgiler içeren üst düzey JSON nesnesi.| 
|[Varlıklar|Varlık ayrıntıları içeren üst düzey JSON nesnesi.| 

  
### <a name="error"></a>Hata  
Gerçekleşen hata tanımlar.  
  
|Öğe|Açıklama|Tür|  
|-------------|-----------------|----------|  
|<a name="error-code" />Kod|Hata kategorisi tanımlar hata kodu. Olası kodlarının listesi için bkz. [hata kodları](#error-codes).|Dize|  
|<a name="error-message" />İleti|Hatanın açıklaması.|Dize|  
|<a name="error-moredetails" />moreDetails|Hata hakkında ek bilgi sağlayan bir açıklama.|Dize|  
|<a name="error-parameter" />Parametre|Sorgu parametresi hataya neden olan istek.|Dize|  
|<a name="error-subcode" />Alt|Hatayı tanımlar hata kodu. Örneğin, varsa `code` InvalidRequest, olan `subCode` ParameterInvalid veya ParameterInvalidValue olabilir. |Dize|  
|<a name="error-value" />Değer|Geçerli değildi sorgu parametrenin değeri.|Dize|  
  

### <a name="errorresponse"></a>ErrorResponse  
Başarısız istek olduğunda yanıt içeren üst düzey nesnesi.  
  
|Ad|Değer|Tür|  
|----------|-----------|----------|  
|_türü|Tür ipucu.|Dize|  
|<a name="errors" />Hataları|İsteğin neden başarısız olma nedenlerini tanımlayan hataların listesi.|[Hata](#error)|  

  
  
### <a name="license"></a>Lisans  
Altında bir metin veya resim kullanılabilir lisans tanımlar.  
  
|Ad|Değer|Tür|  
|----------|-----------|----------|  
|ad|Lisans adı.|Dize|  
|url|Kullanıcı Lisansı hakkında daha fazla bilgi edinebileceğiniz bir Web sitesi URL'si.<br /><br /> Köprü oluşturmak için adını ve URL'sini kullanın.|Dize|  
  

### <a name="licenseattribution"></a>LicenseAttribution  
Lisans attribution için sözleşmeye dayalı bir kural tanımlar.  
  
|Ad|Değer|Tür|  
|----------|-----------|----------|  
|_türü|LicenseAttribution için ayarlanmış bir tür ipucu.|Dize|  
|lisans|İçeriği altında kullanılabilir lisans.|[Lisans](#license)|  
|licenseNotice|Hedeflenen alanının yanındaki lisans. Örneğin, "Metin SA tarafından CC lisansı altında".<br /><br /> Lisans'ın adını ve URL'sini kullan `license` lisans ayrıntılarını açıklayan Web sitesi için köprü oluşturma için alan. Ardından, lisans adı değiştirin `licenseNotice` oluşturduğunuz köprü dizesiyle (örneğin, CC-tarafından-SA).|Dize|  
|mustBeCloseToContent|Kuralın uygulanacağı alan yakınlık kapatın kural içeriğini yerleştirilmelidir olup olmadığını belirleyen bir Boole değeri. Varsa **true**, içeriği yakınında içinde yerleştirilmelidir. Varsa **false**, veya bu alan mevcut değil, arayanın kararımıza içeriği yerleştirilebilir.|Boole|  
|targetPropertyName|Kuralın uygulanacağı alanın adı.|Dize|  
  

### <a name="link"></a>Bağlantı  
Köprü bileşenlerinin tanımlar.  
  
|Ad|Değer|Tür|  
|----------|-----------|----------|  
|_türü|Tür ipucu.|Dize|  
|metin|Görünen metin.|Dize|  
|url|BİR URL. URL'yi kullanın ve metin, köprü oluşturmak için görüntüler.|Dize|  
  

### <a name="linkattribution"></a>LinkAttribution  
İçin bağlantı attribution sözleşmeye dayalı bir kural tanımlar.  
  
|Ad|Değer|Tür|  
|----------|-----------|----------|  
|_türü|LinkAttribution için ayarlanmış bir tür ipucu.|Dize|  
|mustBeCloseToContent|Kuralın uygulanacağı alan yakınlık kapatın kural içeriğini yerleştirilmelidir olup olmadığını belirleyen bir Boole değeri. Varsa **true**, içeriği yakınında içinde yerleştirilmelidir. Varsa **false**, veya bu alan mevcut değil, arayanın kararımıza içeriği yerleştirilebilir.|Boole|  
|targetPropertyName|Kuralın uygulanacağı alanın adı.<br /><br /> Bir hedef belirtilmemişse atıf varlığa bir bütün olarak uygular ve varlık sunu takip görüntülenmesi gerekir. Bir hedef belirtmeyen birden çok metin ve bağlantı attribution kuralı varsa, bunları birleştirmek ve onları görüntülemek kullanarak bir "veri:" etiketi. Örneğin, "verilerden < sağlayıcısı name1\> &#124; < sağlayıcısı name2\>".|Dize|  
|metin|Attribution metin.|Dize|  
|url|Sağlayıcının Web sitesi URL'si. Kullanım `text` ve'nın köprü oluşturmak için URL.|Dize|  
  
  
### <a name="mediaattribution"></a>MediaAttribution  
Medya attribution için sözleşmeye dayalı bir kural tanımlar.  
  
|Ad|Değer|Tür|  
|----------|-----------|----------|  
|_türü|MediaAttribution için ayarlanmış bir tür ipucu.|Dize|  
|mustBeCloseToContent|Kuralın uygulanacağı alan yakınlık kapatın kural içeriğini yerleştirilmelidir olup olmadığını belirleyen bir Boole değeri. Varsa **true**, içeriği yakınında içinde yerleştirilmelidir. Varsa **false**, veya bu alan mevcut değil, arayanın kararımıza içeriği yerleştirilebilir.|Boole|  
|targetPropertyName|Kuralın uygulanacağı alanın adı.|Dize|  
|url|Medya içeriklerinin köprüsünü oluşturmak için kullandığınız URL. Örneğin, hedef görüntü varsa görüntü tıklanabilir yapmak için URL kullanın.|Dize|  
  
  
  
### <a name="organization"></a>Kuruluş  
Bir yayımcı olarak tanımlar.  
  
Bir yayımcı adının veya Web sitesi veya her ikisini sağlayabilir unutmayın.  
  
|Ad|Değer|Tür|  
|----------|-----------|----------|  
|ad|Yayımcının adı.|Dize|  
|url|Yayımcının Web sitesi URL'si.<br /><br /> Yayımcının Web sitesi sağlamayabilir unutmayın.|Dize|  
  
  

### <a name="webpage"></a>Web sayfası  
Hakkında bilgilerini tanımlayan bir önizleme Web sayfası.  
  
|Ad|Değer|Tür|  
|----------|-----------|----------|
|ad|Sayfa başlığı, mutlaka HTML Başlığı|Dize|
|url|Aslında gezinilen URL'si (istek ve ardından yeniden yönlendirmeleri)|Dize|  
|açıklama|Sayfa içeriği ve kısa açıklama|Dize|  
|isFamilyFriendly|Web dizindeki öğeler için en doğru; Bu algılama yöntemi yalnızca bir URL ve sayfa içeriği göre gerçek zamanlı öğesinden yapın|boole|
|primaryImageOfPage/contentUrl|Önizlemede dahil etmek için temsili bir görüntü URL'si|Dize| 
  
  
### <a name="querycontext"></a>QueryContext  
Bing istek için kullanılan sorgu bağlamı tanımlar.  
  
|Öğe|Açıklama|Tür|  
|-------------|-----------------|----------|  
|adultIntent|Belirtilen sorgu yetişkinlere yönelik sonuçlar olup olmadığını belirten bir Boole değeri. Değer **true** yetişkinlere yönelik sonuçlar; sorgu varsa, aksi takdirde, **false**.|Boole|  
|alterationOverrideQuery|Orijinal dizeyi kullanmak için Bing zorlamak için kullanılacak sorgu dizesi. Örneğin, sorgu dizesi ise *downwind saling*, geçersiz kılma sorgu dizesi olacaktır *+ downwind saling*. Sonuçlanan sorgu dizesini kodlayın unutmayın *% 2Bsaling + downwind*.<br /><br /> Bu alan, yalnızca özgün sorgu dizesi bir yazım hatası içeriyorsa dahildir.|Dize|  
|alteredQuery|Bing tarafından sorguyu gerçekleştirmek için kullanılan sorgu dizesi. Bing yazım hatalarını özgün sorgu dizesini içerdiği değiştirilen sorgu dizesini kullanır. Örneğin, sorgu dizesi ise `saling downwind`, değiştirilen sorgu dizesi olacaktır `sailing downwind`.<br /><br /> Bu alan, yalnızca özgün sorgu dizesi bir yazım hatası içeriyorsa dahildir.|Dize|  
|askUserForLocation|Bing doğru sonuçlar sağlamak için kullanıcının konumuna isteyip istemediğini gösteren bir Boole değeri. Kullanarak kullanıcının bulunduğu konum belirttiyseniz [X MSEdge Clientıp](#clientip) ve [X arama konumu](#location) üst bilgiler, bu alan yoksayabilirsiniz.<br /><br /> "Günün hava durumu" veya "kullanıcının konumuna doğru sonuçlar sağlamak için gereken Yakınımdaki restoranlar" gibi konumu kullanan sorgular için bu alan ayarlanır **true**.<br /><br /> ' % S'konum (örneğin, "Seattle hava") içeren konumu kullanan sorgular için bu alan kümesine **false**. Bu alan ayrıca kümesine **false** konumu "gibi en iyi satıcılar" uyumlu olmayan sorgular.|Boole|  
|originalQuery|İstekte belirtilen sorgu dizesi.|Dize|  

### <a name="identifiable"></a>Tanımlama
|Ad|Değer|Tür|  
|-------------|-----------------|----------|
|id|Bir kaynak tanımlayıcısı|Dize|
 
### <a name="rankinggroup"></a>RankingGroup
Tanımlar grubu bir arama sonuçları, aşağıdaki gibi mainline.
|Ad|Değer|Tür|  
|-------------|-----------------|----------|
|öğeler|Grup içinde görüntülemek için arama sonuçları listesi.|RankingItem|

### <a name="rankingitem"></a>RankingItem
Görüntülenecek bir arama sonucu öğesi tanımlar.
|Ad|Değer|Tür|  
|-------------|-----------------|----------|
|resultIndex|Görüntülenecek yanıtında öğenin sıfır tabanlı dizini. Bu alan öğe içermiyorsa, yanıt tüm öğeleri görüntüler. Örneğin, haber yanıt tüm haber makalelerini görüntüler.|Tamsayı|
|answerType|Görüntülenecek öğe içeren yanıtı. Örneğin, haber.<br /><br />Yanıt SearchResponse nesnesinde bulunacak türünü kullanın. Türü bir SearchResponse alan adıdır.<br /><br /> Ancak, yalnızca bu nesne değeri alanı varsa yanıt türünü kullanın. Aksi takdirde, yoksayın.|Dize|
|textualIndex|Görüntülenecek textualAnswers yanıt dizini.| İşaretsiz tamsayı|
|değer|Görüntülenecek yanıt veya öğeyi görüntülemek için bir yanıt tanımlayan kimliği. Kimliği bir yanıt tanımlıyorsa, yanıtın tüm öğeleri görüntüler.|Tanımlama|

### <a name="rankingresponse"></a>RankingResponse  
Arama sonuçları sayfası içeriği yerleştirilmesi gerektiğini ve hangi sırayla tanımlar.  
  
|Ad|Değer|  
|----------|-----------|  
|<a name="ranking-mainline" />mainline|Ana hatta da görüntülemek için arama sonuçları.|  
|<a name="ranking-pole" />kutup|Arama sonuçlarını, en çok görünen alınmasına üyelerine gösterilen (örneğin, ana hat görüntülenen ve kenar çubuğunuzu).|  
|<a name="ranking-sidebar" />Kenar Çubuğu|Kenar çubuğunda görüntülemek için arama sonuçları.| 


### <a name="searchresponse"></a>SearchResponse  
İstek başarılı olduğunda, yanıtı içeren üst düzey nesnesi tanımlar.  
  
Hizmet bir saldırı hizmet reddi şüphelenen, istek başarılı olduğunu unutmayın (HTTP durum kodudur 200 Tamam); Ancak, yanıt gövdesi boş olur.  
  
|Ad|Değer|Tür|  
|----------|-----------|----------|  
|_türü|Tür ipucu SearchResponse için ayarlanır.|Dize|  
|Web sayfası|Önizleme tanımlayan bir JSON nesnesi|dize|  
  
  
### <a name="textattribution"></a>TextAttribution  
Düz metin attribution için sözleşmeye dayalı bir kural tanımlar.  
  
|Ad|Değer|Tür|  
|----------|-----------|----------|  
|_türü|TextAttribution için ayarlanmış bir tür ipucu.|Dize|  
|metin|Attribution metin.<br /><br /> Metin atıf varlığa bir bütün olarak uygular ve varlık sunu takip görüntülenmesi gerekir. Bir hedef belirtmeyen birden çok metin veya bağlantı attribution kuralı varsa, bunları birleştirmek ve onları görüntülemek kullanarak bir "veri:" etiketi.|Dize| 


## <a name="error-codes"></a>Hata kodları

Bir isteği döndüren olası HTTP durum kodları şunlardır:  
  
|Durum Kodu|Açıklama|  
|-----------------|-----------------|  
|200|Başarılı.|  
|400|Sorgu parametrelerden biri eksik veya geçerli değil.|  
|401|Abonelik anahtarı eksik veya geçerli değil.|  
|403|Kullanıcının kimliği doğrulanır (örneğin, bunlar bir geçerli abonelik anahtarı kullanılır), ancak istenen kaynak için izniniz yok.<br /><br /> Çağıranın sorguları başına aylık kota aşılırsa Bing bu durumu döndürebilir.|  
|410|İstek HTTP yerine HTTPS protokolü kullanılır. Yalnızca desteklenen protokol https'dir.|  
|429|Çağıran ikinci kota başına sorguları aştı.|  
|500|Beklenmeyen sunucu hatası.|

İstek başarısız olursa, yanıtını içeren bir [ErrorResponse](#errorresponse) listesini içeren bir nesne [hata](#error) hata nedenini açıklayan bir nesne. Hata parametresi, ilgili olup olmadığını `parameter` alan sorun parametresi tanımlar. Ve hata için bir parametre değeri, ilişkili olup olmadığını `value` alan, geçerli olmayan bir değer tanımlar.

```json
{
  "_type": "ErrorResponse", 
  "errors": [
    {
      "code": "InvalidRequest", 
      "subCode": "ParameterMissing", 
      "message": "Required parameter is missing.", 
      "parameter": "q" 
    }
  ]
}

{
  "_type": "ErrorResponse", 
  "errors": [
    {
      "code": "InvalidAuthorization", 
      "subCode": "AuthorizationMissing", 
      "message": "Authorization is required.", 
      "moreDetails": "Subscription key is not recognized."
    }
  ]
}
```

Olası hata kodu ve alt hata kodu değerleri şunlardır.

|Kod|Alt|Açıklama
|-|-|-
|ServerError|UnexpectedError<br/>ResourceError<br/>Uygulanmadı|HTTP durum kodunu 500'dür.
|InvalidRequest|ParameterMissing<br/>ParameterInvalidValue<br/>HttpNotAllowed<br/>Engellendi|Her isteğin herhangi bir bölümü geçerli değil Bing InvalidRequest döndürür. Örneğin, bir gerekli parametre eksik veya bir parametre değeri geçerli değil.<br/><br/>Hata ParameterMissing veya ParameterInvalidValue ise, HTTP durum kodu 400 ' dir.<br/><br/>HTTPS yerine HTTP protokolünü kullanıyorsanız, Bing HttpNotAllowed döndürür ve 410 HTTP durum kodudur.
|RateLimitExceeded|Hiçbir alt kodları|/ Saniye (QPS) sorguları veya sorgu başına aylık (QPM) kota aştığında Bing RateLimitExceeded döndürür.<br/><br/>QPS aşarsanız, HTTP durum kodu 429 Bing döndürür ve QPM aşarsanız, Bing 403 döndürür.
|InvalidAuthorization|AuthorizationMissing<br/>AuthorizationRedundancy|Bing çağıran doğrulandığında Bing InvalidAuthorization döndürür. Örneğin, `Ocp-Apim-Subscription-Key` üstbilgisi eksik veya abonelik anahtarı geçerli değil.<br/><br/>Birden fazla kimlik doğrulama yöntemi belirtmek, yedeklilik meydana gelir.<br/><br/>Hata InvalidAuthorization ise, HTTP durum kodunu 401 ' dir.
|InsufficientAuthorization|AuthorizationDisabled<br/>AuthorizationExpired|Çağıran kaynağa erişmek için izinlere sahip olmadığı durumlarda Bing InsufficientAuthorization döndürür. Bu abonelik anahtarını devre dışı bırakıldı veya süresi ortaya çıkabilir. <br/><br/>Hata InsufficientAuthorization, HTTP durum kodu 403 ise.

## <a name="next-steps"></a>Sonraki adımlar
- [C# hızlı başlangıç](c-sharp-quickstart.md)
- [Java hızlı başlangıç](java-quickstart.md)
- [Düğüm hızlı başlangıç](node-quickstart.md)
- [Python hızlı başlangıç](python-quickstart.md)

