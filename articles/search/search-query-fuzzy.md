---
title: Belirsiz arama
titleSuffix: Azure Cognitive Search
description: Yanlış yazılmış bir terimi veya typo 'yi otomatik olarak düzeltmek için "sizin demek istediniz" arama deneyimini uygulayın.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 03/03/2021
ms.openlocfilehash: b2f2c8497d5365104a5ffc361b791450925d0c19
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "101694793"
---
# <a name="fuzzy-search-to-correct-misspellings-and-typos"></a>Yanlış yazım ve yazım hatalarını düzeltmek için belirsiz arama

Azure Bilişsel Arama, giriş dizesinde yazım hatası ve yanlış yazılmış terimler için telafi eden bir sorgu türü olan belirsiz aramayı destekler. Bu, benzer bir bileşim olan terimleri tarayarak yapar. Aramanın yakın eşleşmeleri kapsayacak şekilde genişletilmesi, tutarsızlık yalnızca birkaç yanlış yerleştirilmiş karakter olduğunda bir yazım hatası otomatik olarak düzeltilmesinin etkisini içerir. 

## <a name="what-is-fuzzy-search"></a>Benzer arama nedir?

Benzer bir bileşim içeren koşullara uyan bir eşleşme üreten bir genişletme alıştırmada. Benzer bir arama belirtildiğinde motor, sorgudaki tüm şartlar için benzer şekilde oluşturulmuş terimlerin bir grafiğini ( [belirleyici olarak sonlu](https://en.wikipedia.org/wiki/Deterministic_finite_automaton)bir değer temelinde) oluşturur. Örneğin, sorgunuz üç terim olan "İstanbul University" içeriyorsa, sorgudaki her terim için bir grafik oluşturulur `search=university~ of~ washington~` (benzer aramada durdur-sözcük kaldırma yoktur, bu nedenle "/" bir grafik alır).

Grafik, her bir terimin 50 genişleme veya permütasyon 'ten oluşur ve her iki durumda da hem doğru hem de hatalı varyantlar yakalanıyor. Daha sonra motor, yanıtta en üstteki ilgili eşleşmeleri döndürür. 

"Üniversite" gibi bir terim için, grafikte "unversty, üniversty, University, Universe, ters" olabilir. Grafikteki olanlarla eşleşen tüm belgeler sonuçlara dahildir. Aynı sözcüğün farklı biçimlerini ("fareler" ve "fare") işlemek için metni çözümleyen diğer sorguların aksine, benzer bir sorgudaki karşılaştırmalar, metinde hiçbir dil analizi olmadan yüz değeri olarak alınır. Sözdizimi farklılıkları küçük olduğundan, anlamsal olarak farklı olan "Universe" ve "ters" eşleşir.

Tutarsızlıklar iki veya daha az düzenleme ile sınırlıysa, bir düzenlemenin eklenen, silinen, değiştirilen veya dönüştürülmüş bir karakter olduğu durumlarda eşleşme başarılı olur. Değişiklik belirten dize düzeltme algoritması, "bir sözcüğü diğer bir sözcüğe değiştirmek için gereken en az işlem sayısı (ekleme, silme, değiştirme veya iki bitişik karakterin geçiş konumları) olarak tanımlanan [Ermerau-Pavenshtein uzaklık](https://en.wikipedia.org/wiki/Damerau%E2%80%93Levenshtein_distance) ölçümdür. 

Azure Bilişsel Arama 'de:

+ Benzer sorgu tüm şartlar için geçerlidir, ancak ve kurulumlarını aracılığıyla tümcecikleri destekleyebilirsiniz. Örneğin, "Unviersty ~ of ~" Wshington ~ "," University of Washington "ile eşleşir.

+ Bir düzenlemenin varsayılan uzaklığı 2 ' dir. Değeri, `~0` genişlemesiz bir değer belirtir (yalnızca tam terim eşleşme olarak kabul edilir), ancak `~1` bir derece veya bir düzenleme için belirtebilirsiniz. 

+ Benzer bir sorgu, 50 ek PERMÜTASYONA kadar bir terim genişletebilir. Bu sınır yapılandırılamaz, ancak düzenleme mesafesini 1 ' i azaltarak genişletmeleri sayısını etkili bir şekilde azaltabilirsiniz.

+ Yanıtlar, ilgili eşleşme içeren belgelerden oluşur (50 'e kadar).

Toplu olarak, grafikler dizindeki belirteçlere göre eşleşme ölçütü olarak gönderilir. Imagine kullanabileceğiniz gibi, benzer arama doğal olarak diğer sorgu formlarından daha yavaştır. Dizininizin boyutu ve karmaşıklığı, yanıt gecikmesini saptmak için avantajların yeterince olup olmadığını belirleyebilir.

> [!NOTE]
> Benzer arama yavaş olduğundan dolayı, n-gram dizin oluşturma gibi alternatifleri, kısa karakter dizileri (bıgram ve trigram belirteçleri için iki ve üç karakter dizisi) ile araştırmaya yönelik olabilir. Dilinize ve sorgu yüzeyine bağlı olarak, n-gram size daha iyi performans sağlayabilir. Ticareti, n-gram dizin oluşturmanın çok fazla depolama alanı yoğunluğu ve çok daha büyük dizinler üretmasıdır.
>
> Yalnızca en egregious durumlarını işlemek istiyorsanız düşünebileceğiniz başka bir alternatif, bir [eş anlamlı eşleme](search-synonyms.md)olacaktır. Örneğin, "ara" öğesini "serach, serch, Sarch" veya "Al" olarak "retreive" olarak eşleme.

## <a name="indexing-for-fuzzy-search"></a>Benzer arama için dizin oluşturma

Çözümleyiciler, bir genişletme grafiği oluşturmak için sorgu işleme sırasında kullanılmaz, ancak bu, benzer arama senaryolarında yok sayıcılar göz ardı edilmelidir. Her bir sonra çözümleyiciler, eşleme işleminin yapıldığı, sorgunun ücretsiz form, filtrelenmiş arama veya girdi olarak Graf içeren benzer bir arama olup olmadığı ile ilgili belirteçleri oluşturmak için dizin oluşturma sırasında kullanılır. 

Genellikle, bir alan temelinde çözümleyiciler atarken, analiz zinciri üzerinde ince ayar yapma kararı, belirsiz arama gibi özelleştirilmiş sorgu formları yerine birincil kullanım örneğine (bir filtre veya tam metin arama) dayanır. Bu nedenle, benzer arama için belirli bir çözümleyici önerisi yoktur. 

Bununla birlikte, test sorguları, beklediğinizi eşleştirmelere neden oluşturmazsa, daha iyi sonuçlar elde edip edemediğini görmek için dizin oluşturma Çözümleyicisi 'ni bir [dil Çözümleyicisi](index-add-language-analyzers.md)olarak ayarlamayı deneyebilirsiniz. Bazı diller, özellikle sesli harften bağımsız olarak, Microsoft doğal dil işlemcileri tarafından oluşturulan esnek ve düzensiz sözcük formlarından faydalanabilir. Bazı durumlarda, doğru dil Çözümleyicisi 'nin kullanılması, bir terimin Kullanıcı tarafından belirtilen değerle uyumlu bir şekilde simgeleştirilmiş olup olmadığı konusunda bir farklılık yapabilir.

## <a name="how-to-use-fuzzy-search"></a>Benzer arama kullanma

Benzer sorgular tam Lucene sorgu söz dizimi kullanılarak oluşturulur ve bu, [Lucene sorgu ayrıştırıcısı](https://lucene.apache.org/core/6_6_1/queryparser/org/apache/lucene/queryparser/classic/package-summary.html)çağırır.

1. Sorguda () tam bir Lucene ayrıştırıcısı ayarlayın `queryType=full` .

1. İsteğe bağlı olarak, bu parametreyi kullanarak isteği belirli alanlara kapsamını ( `searchFields=<field1,field2>` ) kullanın. 

1. `~`Tüm terimin sonuna () tilde () işlecini ekleyin `search=<string>~` .

   Düzenleme mesafesini () belirtmek istiyorsanız, isteğe bağlı bir parametre, 0 ile 2 (varsayılan) arasında bir sayı ekleyin `~1` . Örneğin, "mavi ~" veya "mavi ~ 1", "mavi", "maves" ve "tutkalla" döndürür.

Azure Bilişsel Arama, dönemin ve uzaklığın yanı sıra (en fazla 2) sorgu üzerinde ayarlanacak ek parametre yoktur.

> [!NOTE]
> Sorgu işleme sırasında, benzer sorgular [sözcük temelli Analize](search-lucene-query-architecture.md#stage-2-lexical-analysis)geçmez. Sorgu girişi doğrudan sorgu ağacına eklenir ve bir terim grafiği oluşturmak için genişletilir. Gerçekleştirilen tek dönüşüm küçük harfe eşit.

## <a name="testing-fuzzy-search"></a>Benzer aramayı test etme

Basit test için, bir sorgu ifadesi üzerinden yineleme yapmak üzere [Arama Gezgini](search-explorer.md) veya [Postman](search-get-started-rest.md) önerilir. Her iki araç da etkileşimlidir. Bu, bir terimin birden fazla çeşitinden hızlıca karşılaşmanıza ve geri gelen yanıtları değerlendirebileceğiniz anlamına gelir.

Sonuçlar belirsiz olduğunda, [isabet vurgulaması](search-pagination-page-layout.md#hit-highlighting) , yanıtta eşleşmeyi belirlemenize yardımcı olabilir. 

> [!Note]
> Benzer eşleşmeleri belirlemek için isabet vurgulamanın kullanımı sınırlamalara sahiptir ve yalnızca temel benzer arama için geçerlidir. Dizininizin Puanlama profilleri varsa veya sorguyu ek sözdizimi ile katmanladıysanız, isabet vurgulama, eşleşmeyi tanımlayamayabilir. 

### <a name="example-1-fuzzy-search-with-the-exact-term"></a>Örnek 1: tam terimle belirsiz arama

Arama belgesinde bir alanda aşağıdaki dizenin bulunduğunu varsayın `"Description"` : `"Test queries with special characters, plus strings for MSFT, SQL and Java."`

"Özel" üzerinde benzer bir aramayla başlayın ve Açıklama alanına isabet vurgusu ekleyin:

```console
search=special~&highlight=Description
```

Yanıt içinde, isabet vurgulama ' i eklediğiniz için biçimlendirme, eşleşen terim olarak "özel" e uygulanır.

```output
"@search.highlights": {
    "Description": [
        "Test queries with <em>special</em> characters, plus strings for MSFT, SQL and Java."
    ]
```

İsteği yeniden deneyin, birkaç harf ("pe") alarak "özel" yazarak hatalı çalışın:

```console
search=scial~&highlight=Description
```

Şimdiye kadar, yanıtta hiçbir değişiklik yoktur. Varsayılan 2 derece uzaklığın kullanılması, "pe" iki karakteri "özel" kaynağından kaldırmak yine de söz konusu dönemde başarılı bir eşleştirmeye izin verir.

```output
"@search.highlights": {
    "Description": [
        "Test queries with <em>special</em> characters, plus strings for MSFT, SQL and Java."
    ]
```

Bir çok istek deneniyor, toplam üç silme ("özel" iken "scal") için bir son karakter alarak arama terimini daha fazla değiştirin:

```console
search=scal~&highlight=Description
```

Aynı yanıtın döndürüldüğünden, ancak artık "özel" ile eşleşme yerine "SQL" üzerinde olduğu gibi dikkat edin.

```output
        "@search.score": 0.4232868,
        "@search.highlights": {
            "Description": [
                "Mix of special characters, plus strings for MSFT, <em>SQL</em>, 2019, Linux, Java."
            ]
```

Bu genişletilmiş örneğin noktası, isabet vurgulamanın belirsiz sonuçlara yol açmak için gereken açıklık göstermektir. Her durumda, aynı belge döndürülür. Bir eşleşmeyi doğrulamak için belge kimliklerine güvenmiştiniz, "özel" ' in "SQL" olarak kaymasını kaçırmış olabilirsiniz.

## <a name="see-also"></a>Ayrıca bkz.

+ [Tam metin aramasının Azure Bilişsel Arama 'da nasıl çalıştığı (sorgu ayrıştırma mimarisi)](search-lucene-query-architecture.md)
+ [Arama Gezgini](search-explorer.md)
+ [.NET 'te sorgulama](./search-get-started-dotnet.md)
+ [REST 'te sorgulama](./search-get-started-powershell.md)