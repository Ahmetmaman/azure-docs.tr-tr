---
title: Metin Analizi API’sini çağırma
titleSuffix: Azure Cognitive Services
description: Bu makalede, Azure bilişsel hizmetler REST API ve Postman Metin Analizi nasıl çağrlayabileceğiniz açıklanır.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 07/30/2019
ms.author: aahi
ms.openlocfilehash: 43ee7272066dbd89e7c0053d51ba039b83fb494f
ms.sourcegitcommit: 22da82c32accf97a82919bf50b9901668dc55c97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/08/2020
ms.locfileid: "94363825"
---
# <a name="how-to-call-the-text-analytics-rest-api"></a>Metin Analizi nasıl çağrılacağını REST API

**Metin Analizi API'si** çağrıları, herhangi bir dilde formülleştirmek IÇIN http post/Get çağrılardır. Bu makalede, önemli kavramları göstermek için REST ve [Postman](https://www.postman.com/downloads/) kullanırız.

Her isteğin erişim anahtarınızı ve bir HTTP uç noktasını içermesi gerekir. Uç nokta, kaydolma sırasında seçtiğiniz bölgeyi, hizmet URL 'sini ve istekte kullanılan bir kaynağı belirtir: `sentiment` , `keyphrases` , `languages` ve `entities` . 

Yönetilecek veri varlığı olmadığından Metin Analizi durum bilgisiz olduğunu hatırlayın. Metniniz karşıya yüklenir, teslim edildiğinde çözümlenir ve sonuçlar çağıran uygulamaya hemen döndürülür.

[!INCLUDE [text-analytics-api-references](../includes/text-analytics-api-references.md)]

[!INCLUDE [v3 region availability](../includes/v3-region-availability.md)]

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [cognitive-services-text-analytics-signup-requirements](../../../../includes/cognitive-services-text-analytics-signup-requirements.md)]

<a name="json-schema"></a>

## <a name="json-schema-definition"></a>JSON şema tanımı

Giriş, ham yapılandırılmamış metinde JSON olmalıdır. XML desteklenmiyor. Şema basittir ve aşağıdaki listede açıklanan öğelerden oluşur. 

Şu anda tüm Metin Analizi işlemler için aynı belgeleri gönderebilirsiniz: yaklaşım, anahtar tümceciği, dil algılama ve varlık tanımlama. (Şema, gelecekte her analiz için farklılık gösterir.)

| Öğe | Geçerli değerler | Gerekli mi? | Kullanım |
|---------|--------------|-----------|-------|
|`id` |Veri türü dizedir, ancak uygulama belge kimlikleri ' nde tam sayı olarak eğilimlidir. | Gerekli | Sistem çıktıyı yapılandırmak için sağladığınız kimlikleri kullanır. İstekteki her bir KIMLIK için dil kodları, anahtar tümceleri ve yaklaşım puanları oluşturulur.|
|`text` | Yapılandırılmamış ham metin, en fazla 5.120 karakter. | Gerekli | Dil algılama için metin herhangi bir dilde ifade edilebilir. Yaklaşım analizi, anahtar ifade ayıklama ve varlık tanımlama için, metin [desteklenen bir dilde](../language-support.md)olmalıdır. |
|`language` | 2 karakterlik [ıso 639-1](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes) [desteklenen bir dil](../language-support.md) için kod | Değişir | Yaklaşım analizi, anahtar ifade ayıklama ve varlık bağlama için gereklidir; dil algılama için isteğe bağlı. Bunu dışladığınızda bir hata yoktur, ancak analiz bu olmadan zayıflatılmalıdır. Dil kodu, sağladığınız öğesine karşılık gelmelidir `text` . |

Sınırlamalar hakkında daha fazla bilgi için bkz. [metin analizi genel bakış > veri sınırları](../overview.md#data-limits). 


```json
{
  "documents": [
    {
      "language": "en",
      "id": "1",
      "text": "Sample text to be sent to the text analytics api."
    },
    {
      "language": "en",
      "id": "2",
      "text": "It's incredibly sunny outside! I'm so happy."
    },
    {
      "language": "en",
      "id": "3",
      "text": "Pike place market is my favorite Seattle attraction."
    }
  ]
}
```


## <a name="set-up-a-request-in-postman"></a>Postman 'da istek ayarlama

Hizmet, boyutu 1 MB 'a kadar olan isteği kabul eder. Postman (veya başka bir Web API test aracı) kullanıyorsanız, uç noktasını kullanmak istediğiniz kaynağı içerecek şekilde ayarlayın ve erişim anahtarını bir istek üst bilgisine sağlayın. Her işlem için ilgili kaynağı uç noktaya eklemeniz gerekir. 

1. Postman 'da:

   + İstek türü olarak **gönderi** ' ı seçin.
   + Portal sayfasından kopyaladığınız uç noktaya yapıştırın.
   + Kaynak Ekle.

   Kaynak uç noktaları aşağıdaki gibidir (bölgeniz farklılık gösterebilir):

   + `https://westus.api.cognitive.microsoft.com/text/analytics/v3.0/sentiment`
   + `https://westus.api.cognitive.microsoft.com/text/analytics/v3.0/keyPhrases`
   + `https://westus.api.cognitive.microsoft.com/text/analytics/v3.0/languages`
   + `https://westus.api.cognitive.microsoft.com/text/analytics/v3.0/entities/recognition/general`

2. Üç istek üst bilgilerini ayarlayın:

   + `Ocp-Apim-Subscription-Key`: Azure portal adresinden alınan erişim anahtarınız.
   + `Content-Type`: Application/JSON.
   + `Accept`: Application/JSON.

   İsteğiniz, **/keyPhrases** kaynağını varsayarak aşağıdaki ekran görüntüsüne benzer şekilde görünmelidir.

   ![Uç nokta ve üst bilgilerle ekran görüntüsü iste](../media/postman-request-keyphrase-1.png)

4. **Gövde** ' ye tıklayın ve biçim için **RAW** ' ı seçin.

   ![Gövde ayarları ile ekran görüntüsü iste](../media/postman-request-body-raw.png)

5. Bazı JSON belgelerini amaçlanan analiz için geçerli bir biçimde yapıştırın. Belirli bir analiz hakkında daha fazla bilgi için aşağıdaki konulara bakın:

  + [Dil algılama](text-analytics-how-to-language-detection.md)  
  + [Anahtar ifade ayıklama](text-analytics-how-to-keyword-extraction.md)  
  + [Yaklaşım analizi](text-analytics-how-to-sentiment-analysis.md)  
  + [Varlık tanıma](text-analytics-how-to-entity-linking.md)  


6. İsteği göndermek için **Gönder** ' e tıklayın. Dakika ve saniye başına gönderebilmeniz için istek sayısı hakkında genel [bakış bölümüne bakın](../overview.md#data-limits) .

   Postman 'da, yanıt, istekte belirtilen her belge KIMLIĞI için bir öğe olan bir sonraki pencerede, tek bir JSON belgesi olarak görüntülenir.

## <a name="see-also"></a>Ayrıca bkz. 

 [Metin Analizi genel bakış](../overview.md)  
 [Sık sorulan sorular (SSS)](../text-analytics-resource-faq.md)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Dil algılama](text-analytics-how-to-language-detection.md)