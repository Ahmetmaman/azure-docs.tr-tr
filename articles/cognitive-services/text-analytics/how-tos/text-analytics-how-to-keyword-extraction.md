---
title: Nasıl yapılır anahtar ifade ayıklama metin Analytics REST API (Azure'da Microsoft Bilişsel hizmetler) | Microsoft Docs
description: İzlenecek yol Bu öğreticide Azure üzerinde Microsoft Bilişsel hizmetler metin analizi REST API kullanarak anahtar tümcecikleri ayıklayın yapma.
services: cognitive-services
author: HeidiSteen
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: sample
ms.date: 09/12/2018
ms.author: heidist
ms.openlocfilehash: d6e3223b4f7931f250e422f1f30edcb375407c8c
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55865709"
---
# <a name="example-how-to-extract-key-phrases-in-text-analytics"></a>Örnek: Metin analizi, anahtar ifadeleri ayıklama

[Anahtar İfade Ayıklama API’si](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6), yapılandırılmamış metni değerlendirir ve her bir JSON belgesi için bir anahtar ifade listesi döndürür. 

Bir belge koleksiyonundaki ana noktaları hızlı şekilde belirlemeniz gerekiyorsa bu özellik kullanışlıdır. Örneğin, "The food was delicious and there were wonderful staff" (Yemek lezizdi ve müthiş personel hizmeti vardı) giriş metni olduğunda hizmet, "food" (yemek) ve "wonderful staff" (müthiş personel) ana konuşma noktalarını döndürür.

Şu anda Anahtar İfade Ayıklama, İngilizce, Almanca, İspanyolca ve Japonca dillerini destekler. Diğer diller önizleme aşamasındadır. Daha fazla bilgi için bkz. [Desteklenen diller](../text-analytics-supported-languages.md).

> [!TIP]
> Metin analizi de sağlar Linux tabanlı bir Docker kapsayıcı görüntüsü anahtar ifade ayıklama için böylece [yükleyin ve metin analizi kapsayıcı çalıştırın](text-analytics-how-to-install-containers.md) verilerinizi yakın.

## <a name="preparation"></a>Hazırlık

Anahtar ifade ayıklama, üzerinde çalışılacak büyük metin öbekleri girdiğinizde en iyi şekilde çalışır. Bu, küçük metin öbekleri üzerinde daha iyi performans gösteren yaklaşım analizinin tersidir. Her iki işlemden de en iyi sonuçları elde etmek için girişleri uygun şekilde yeniden yapılandırın.

JSON belgeleri kimlik, metin, dil biçiminde olmalıdır.

Belge boyutu, belge başına 5.000 karakterden küçük olmalıdır ve koleksiyon başına en fazla 1000 öğeniz olabilir. Koleksiyon, istek gövdesinde gönderilir. Aşağıdaki örnek, anahtar ifade ayıklaması için gönderebileceğiniz içeriğin bir gösterimidir.

```
    {
        "documents": [
            {
                "language": "en",
                "id": "1",
                "text": "We love this trail and make the trip every year. The views are breathtaking and well worth the hike!"
            },
            {
                "language": "en",
                "id": "2",
                "text": "Poorly marked trails! I thought we were goners. Worst hike ever."
            },
            {
                "language": "en",
                "id": "3",
                "text": "Everyone in my family liked the trail but thought it was too challenging for the less athletic among us. Not necessarily recommended for small children."
            },
            {
                "language": "en",
                "id": "4",
                "text": "It was foggy so we missed the spectacular views, but the trail was ok. Worth checking out if you are in the area."
            },                
            {
                "language": "en",
                "id": "5",
                "text": "This is my favorite trail. It has beautiful views and many places to stop and rest"
            }
        ]
    }
```    
    
## <a name="step-1-structure-the-request"></a>1. Adım: Yapı isteği

İstek tanımıyla ilgili ayrıntılara [Metin Analizi API’sini çağırma](text-analytics-how-to-call-api.md) bölümünden erişilebilir. Kolaylık olması için aşağıdaki noktalar yeniden belirtilmektedir:

+ Bir **POST** isteği oluşturun. Bu istek için API belgelerini gözden geçirin: [Anahtar ifadeleri API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6)

+ Anahtar ifade ayıklama, Azure veya bir örneklenmiş bir metin analizi kaynak kullanarak HTTP uç noktasına ayarlayın [metin analizi kapsayıcı](text-analytics-how-to-install-containers.md). `/keyPhrases` kaynağını içermelidir: `https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/keyPhrases`

+ Metin Analizi işlemlerine yönelik erişim anahtarını dahil etmek için bir istek üst bilgisi ayarlayın. Daha fazla bilgi için bkz. [Uç noktaları ve erişim anahtarlarını bulma](text-analytics-how-to-access-key.md).

+ İstek gövdesinde, bu analiz için hazırladığınız JSON belgeleri koleksiyonunu sağlayın

> [!Tip]
> İsteği yapılandırmak ve hizmete GÖNDERMEK için [Postman](text-analytics-how-to-call-api.md) kullanın veya [belgelerdeki](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6) **API testi konsolu**’nu açın.

## <a name="step-2-post-the-request"></a>2. Adım: POST isteği

İstek alındığında analiz gerçekleştirilir. Hizmet dakikada en fazla 100 istek kabul eder. Her istek maksimum 1 MB olabilir.

Hizmetin durum bilgisi olmadığını unutmayın. Hesabınızda bir veri depolanmaz. Sonuçlar hemen yanıtta döndürülür.

## <a name="step-3-view-results"></a>3. Adım: Sonuçları görüntüleme

Tüm POST istekleri, kimlikler ve algılanan özelliklerle JSON tarafından biçimlendirilmiş bir yanıt döndürür.

Hemen çıktı döndürülür. Sonuçları, JSON kabul eden bir uygulamada akışa alabilir veya çıktıyı yerel sistemde bir dosyaya kaydedebilir, sonra da verileri sıralamanıza, aramanıza ve işlemenize olanak sağlayan bir uygulamaya içeri aktarabilirsiniz.

Aşağıda, anahtar ifade ayıklaması için çıktı örneği gösterilmektedir:

```
    "documents": [
        {
            "keyPhrases": [
                "year",
                "trail",
                "trip",
                "views"
            ],
            "id": "1"
        },
        {
            "keyPhrases": [
                "marked trails",
                "Worst hike",
                "goners"
            ],
            "id": "2"
        },
        {
            "keyPhrases": [
                "trail",
                "small children",
                "family"
            ],
            "id": "3"
        },
        {
            "keyPhrases": [
                "spectacular views",
                "trail",
                "area"
            ],
            "id": "4"
        },
        {
            "keyPhrases": [
                "places",
                "beautiful views",
                "favorite trail"
            ],
            "id": "5"
        }
```

Belirtildiği gibi çözümleyici, gerekli olmayan sözcükleri bulup atar ve bir cümlenin konusu veya nesnesi olabilecek tekli terimleri veya ifadeleri tutar. 

## <a name="summary"></a>Özet

Bu makalede, Bilişsel Hizmetler’de Metin Analizi’ni kullanarak anahtar ifade ayıklaması için kavramları ve iş akışını öğrendiniz. Özet:

+ [Anahtar ifade ayıklama API’si](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6), seçili dillerde kullanılabilir.
+ İstek gövdesindeki JSON belgeleri bir kimlik, metin ve dil kodu içerir.
+ POST isteği, aboneliğiniz için geçerli olan kişiselleştirilmiş bir [erişim anahtarı ve uç nokta](text-analytics-how-to-access-key.md) kullanılarak `/keyphrases` uç noktasına yapılır.
+ Her belge kimliği için anahtar sözcüklerden ve ifadelerden oluşan yanıt çıktısı, Excel ve Power BI da dahil olmak üzere JSON kabul eden tüm uygulamalarda akışa alınabilir.

## <a name="see-also"></a>Ayrıca bkz. 

 [Metin Analizine genel bakış](../overview.md)  
 [Sık sorulan sorular (SSS)](../text-analytics-resource-faq.md)</br>
 [Metin Analizi ürün sayfası](//go.microsoft.com/fwlink/?LinkID=759712) 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Metin Analizi API’si](//westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6)
