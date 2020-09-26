---
title: GenerateAnswer API 'SI ile meta veriler-Soru-Cevap Oluşturma
titleSuffix: Azure Cognitive Services
description: Soru-Cevap Oluşturma, anahtar/değer çiftleri biçiminde meta verileri soru/yanıt çiftlerine eklemenizi sağlar. Sonuçları Kullanıcı sorgularıyla filtreleyebilir ve izleme konuşmalarında kullanılabilecek ek bilgileri saklayabilirsiniz.
services: cognitive-services
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 07/16/2020
ms.custom: devx-track-js, devx-track-csharp
ms.openlocfilehash: 3a67f16b53c2754e2ac5ae1df467aac7726f358e
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91321008"
---
# <a name="get-an-answer-with-the-generateanswer-api-and-metadata"></a>GenerateAnswer API ve meta verileri ile bir yanıt alın

Kullanıcının sorusunun tahmin edilen yanıtını almak için GenerateAnswer API 'sini kullanın. Bir Bilgi Bankası yayımladığınızda, **Yayımlama** SAYFASıNDA bu API 'nin nasıl kullanılacağına ilişkin bilgileri görebilirsiniz. API 'yi, meta veri etiketlerine göre yanıtları filtrelemek için de yapılandırabilir ve test sorgu dizesi parametresiyle uç noktadan Bilgi Bankası ' nı test edebilirsiniz.

Soru-Cevap Oluşturma, anahtar ve değer çiftleri biçiminde meta verileri, soru ve yanıt çiftlerine eklemenizi sağlar. Daha sonra bu bilgileri Kullanıcı sorgularıyla sonuçları filtrelemek ve izleme konuşmalarında kullanılabilecek ek bilgileri depolamak için kullanabilirsiniz. Daha fazla bilgi için bkz. [Bilgi Bankası](../Concepts/knowledge-base.md).

<a name="qna-entity"></a>

## <a name="store-questions-and-answers-with-a-qna-entity"></a>Soru ve yanıtları bir QnA varlığıyla depolayın

Soru-Cevap Oluşturma soruyu nasıl depolayacağınızı ve verileri nasıl yanıtlayabileceğinizi anlamak önemlidir. Aşağıdaki çizim bir QnA varlığını göstermektedir:

![Bir QnA varlığının çizimi](../media/qnamaker-how-to-metadata-usage/qna-entity.png)

Her QnA varlığının benzersiz ve kalıcı bir KIMLIĞI vardır. Belirli bir QnA varlığında güncelleştirme yapmak için KIMLIĞINI kullanabilirsiniz.

<a name="generateanswer-api"></a>

## <a name="get-answer-predictions-with-the-generateanswer-api"></a>GenerateAnswer API 'SI ile yanıt tahminlerini alın

Soru ve yanıt çiftleriyle en iyi eşleşmeyi elde etmek için, bot veya uygulamanızdaki [Generateanswer API](https://docs.microsoft.com/rest/api/cognitiveservices/qnamakerruntime/runtime/generateanswer) 'sini bir Kullanıcı sorusu ile sorgulamak için kullanırsınız.

<a name="generateanswer-endpoint"></a>

## <a name="publish-to-get-generateanswer-endpoint"></a>GenerateAnswer uç noktasını almak için Yayımla

Bilgi bankanızı [soru-cevap oluşturma portalından](https://www.qnamaker.ai)yayımladığınızda veya [API](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/publish)'Yi kullanarak, generateanswer uç noktanızın ayrıntılarını alabilirsiniz.

Uç nokta ayrıntılarınızı almak için:
1. [https://www.qnamaker.ai](https://www.qnamaker.ai) adresinde oturum açın.
1. **Bilgi tabanlarım**' da bilgi tabanınız Için **kodu görüntüle** ' yi seçin.
    ![Bilgi temellerimin ekran görüntüsü](../media/qnamaker-how-to-metadata-usage/my-knowledge-bases.png)
1. GenerateAnswer uç noktası ayrıntılarınızı alın.

    ![Uç nokta ayrıntılarının ekran görüntüsü](../media/qnamaker-how-to-metadata-usage/view-code.png)

Ayrıca, bilgi Bankalarınızın **Ayarlar** sekmesinden uç nokta ayrıntılarınızı alabilirsiniz.

<a name="generateanswer-request"></a>

## <a name="generateanswer-request-configuration"></a>GenerateAnswer istek yapılandırması

HTTP POST isteğiyle GenerateAnswer öğesini çağırın. GenerateAnswer çağrısının nasıl çağrılacağını gösteren örnek kod için bkz. [hızlı](../quickstarts/quickstart-sdk.md#generate-an-answer-from-the-knowledge-base)başlangıçlara bakın.

POST isteği şunu kullanır:

* Gerekli [URI parametreleri](https://docs.microsoft.com/rest/api/cognitiveservices/qnamakerruntime/runtime/train#uri-parameters)
* Güvenlik için gerekli üst bilgi özelliği `Authorization`
* Gerekli [gövde özellikleri](https://docs.microsoft.com/rest/api/cognitiveservices/qnamakerruntime/runtime/train#feedbackrecorddto).

GenerateAnswer URL 'SI aşağıdaki biçime sahiptir:

```
https://{QnA-Maker-endpoint}/knowledgebases/{knowledge-base-ID}/generateAnswer
```

Öğesinin HTTP üst bilgisi özelliğini, `Authorization` `EndpointKey` sonundaki boşluk ve sonra **Ayarlar** sayfasında bulunan uç nokta anahtarı ile birlikte dizenin bir değeriyle ayarlamayı unutmayın.

Örnek bir JSON gövdesi şöyle görünür:

```json
{
    "question": "qna maker and luis",
    "top": 6,
    "isTest": true,
    "scoreThreshold": 30,
    "rankerType": "" // values: QuestionOnly
    "strictFilters": [
    {
        "name": "category",
        "value": "api"
    }],
    "userId": "sd53lsY="
}
```

[Rankertype](../concepts/best-practices.md#choosing-ranker-type)hakkında daha fazla bilgi edinin.

Önceki JSON yalnızca %30 ' da veya eşik puanı üzerinde olan yanıtları istedi.

<a name="generateanswer-response"></a>

## <a name="generateanswer-response-properties"></a>GenerateAnswer yanıt özellikleri

[Yanıt](https://docs.microsoft.com/rest/api/cognitiveservices/qnamakerruntime/runtime/generateanswer#successful-query) , varsa yanıtı göstermek için ihtiyacınız olan tüm bilgileri ve bir sonraki konuşmayı IÇEREN bir JSON nesnesidir.

```json
{
    "answers": [
        {
            "score": 38.54820341616869,
            "Id": 20,
            "answer": "There is no direct integration of LUIS with QnA Maker. But, in your bot code, you can use LUIS and QnA Maker together. [View a sample bot](https://github.com/Microsoft/BotBuilder-CognitiveServices/tree/master/Node/samples/QnAMaker/QnAWithLUIS)",
            "source": "Custom Editorial",
            "questions": [
                "How can I integrate LUIS with QnA Maker?"
            ],
            "metadata": [
                {
                    "name": "category",
                    "value": "api"
                }
            ]
        }
    ]
}
```

Önceki JSON% 38,5 puanı ile yanıt verdi.

## <a name="use-qna-maker-with-a-bot-in-c"></a>C 'de bir bot ile Soru-Cevap Oluşturma kullanma #

Bot Framework, [Getanswer API 'si](https://docs.microsoft.com/dotnet/api/microsoft.bot.builder.ai.qna.qnamaker.getanswersasync?view=botbuilder-dotnet-stable#Microsoft_Bot_Builder_AI_QnA_QnAMaker_GetAnswersAsync_Microsoft_Bot_Builder_ITurnContext_Microsoft_Bot_Builder_AI_QnA_QnAMakerOptions_System_Collections_Generic_Dictionary_System_String_System_String__System_Collections_Generic_Dictionary_System_String_System_Double__)ile soru-cevap oluşturma özelliklerine erişim sağlar:

```csharp
using Microsoft.Bot.Builder.AI.QnA;
var metadata = new Microsoft.Bot.Builder.AI.QnA.Metadata();
var qnaOptions = new QnAMakerOptions();

metadata.Name = Constants.MetadataName.Intent;
metadata.Value = topIntent;
qnaOptions.StrictFilters = new Microsoft.Bot.Builder.AI.QnA.Metadata[] { metadata };
qnaOptions.Top = Constants.DefaultTop;
qnaOptions.ScoreThreshold = 0.3F;
var response = await _services.QnAServices[QnAMakerKey].GetAnswersAsync(turnContext, qnaOptions);
```

Önceki JSON yalnızca %30 ' da veya eşik puanı üzerinde olan yanıtları istedi.

## <a name="use-qna-maker-with-a-bot-in-nodejs"></a>Node.js bir bot ile Soru-Cevap Oluşturma kullanma

Bot Framework, [Getanswer API 'si](https://docs.microsoft.com/javascript/api/botbuilder-ai/qnamaker?view=botbuilder-ts-latest#generateanswer-string---undefined--number--number-)ile soru-cevap oluşturma özelliklerine erişim sağlar:

```javascript
const { QnAMaker } = require('botbuilder-ai');
this.qnaMaker = new QnAMaker(endpoint);

// Default QnAMakerOptions
var qnaMakerOptions = {
    ScoreThreshold: 0.30,
    Top: 3
};
var qnaResults = await this.qnaMaker.getAnswers(stepContext.context, qnaMakerOptions);
```

Önceki JSON yalnızca %30 ' da veya eşik puanı üzerinde olan yanıtları istedi.

<a name="metadata-example"></a>

## <a name="use-metadata-to-filter-answers-by-custom-metadata-tags"></a>Özel meta veri etiketlerine göre yanıtları filtrelemek için meta verileri kullanma

Meta veri eklemek, yanıtları bu meta veri etiketlerine göre filtrelemenize izin verir. **Görünüm seçenekleri** menüsünden meta veri sütununu ekleyin. Meta veri simgesini seçerek, meta veri çifti eklemek için bilgi bankasından meta veriler ekleyin **+** . Bu çift bir anahtar ve bir değer içerir.

![Meta veri ekleme ekran görüntüsü](../media/qnamaker-how-to-metadata-usage/add-metadata.png)

<a name="filter-results-with-strictfilters-for-metadata-tags"></a>

## <a name="filter-results-with-strictfilters-for-metadata-tags"></a>Meta veri etiketleri için en strictFilters ile Sonuçları filtrele

"Bu otel ne zaman kapatıldığında?" Kullanıcı sorusunu göz önünde bulundurun.

Sonuçlar yalnızca Restoran "PARADISE" için gerekli olduğundan, "restoran adı" meta verilerinde GenerateAnswer çağrısında bir filtre ayarlayabilirsiniz. Aşağıdaki örnek şunu gösterir:

```json
{
    "question": "When does this hotel close?",
    "top": 1,
    "strictFilters": [ { "name": "restaurant", "value": "paradise"}]
}
```

### <a name="logical-and-by-default"></a>Mantıksal ve varsayılan olarak

Sorgudaki çeşitli meta veri filtrelerini birleştirmek için, özelliğin dizisine ek meta veri filtreleri ekleyin `strictFilters` . Varsayılan olarak, değerler mantıksal olarak birleştirilir (ve). Bir mantıksal birleşim, çiftin yanıt içinde döndürülmesi için tüm filtrelerin QnA çiftleriyle eşleşmesini gerektirir.

Bu, `strictFiltersCompoundOperationType` özelliğinin değeri ile birlikte kullanılmasına eşdeğerdir `AND` .

### <a name="logical-or-using-strictfilterscompoundoperationtype-property"></a>StrictFiltersCompoundOperationType özelliğini kullanarak mantıksal veya

Birden çok meta veri filtresi birleştirilirken, yalnızca bir veya birkaç filtre eşleştirme ile ilgileniyorlarsa, `strictFiltersCompoundOperationType` özelliği değerini kullanın `OR` .

Bu, herhangi bir filtre eşleştiğinde, ancak meta verisi olmayan yanıtlar döndürmeyeceği bilgi Bankalarınızın yanıtları döndürmesini sağlar.

```json
{
    "question": "When do facilities in this hotel close?",
    "top": 1,
    "strictFilters": [
      { "name": "type","value": "restaurant"},
      { "name": "type", "value": "bar"},
      { "name": "type", "value": "poolbar"}
    ],
    "strictFiltersCompoundOperationType": "OR"
}
```

### <a name="metadata-examples-in-quickstarts"></a>Hızlı başlangıçlarda meta veri örnekleri

Meta veriler hakkında daha fazla bilgi için Soru-Cevap Oluşturma Portal Hızlı başlangıcı:
* [Yazma-meta verileri QnA çiftine ekleyin](../quickstarts/add-question-metadata-portal.md#add-metadata-to-filter-the-answers)
* [Sorgu tahmini-meta verilere göre filtre yanıtları](../quickstarts/get-answer-from-knowledge-base-using-url-tool.md)

<a name="keep-context"></a>

## <a name="use-question-and-answer-results-to-keep-conversation-context"></a>Konuşma bağlamını tutmak için soru ve yanıt sonuçlarını kullanın

GenerateAnswer yanıtı, eşleşen soru ve yanıt çiftinin karşılık gelen meta veri bilgilerini içerir. Bu bilgileri, daha sonraki konuşmalarda kullanılmak üzere önceki görüşmenin bağlamını depolamak için istemci uygulamanızda kullanabilirsiniz.

```json
{
    "answers": [
        {
            "questions": [
                "What is the closing time?"
            ],
            "answer": "10.30 PM",
            "score": 100,
            "id": 1,
            "source": "Editorial",
            "metadata": [
                {
                    "name": "restaurant",
                    "value": "paradise"
                },
                {
                    "name": "location",
                    "value": "secunderabad"
                }
            ]
        }
    ]
}
```

## <a name="match-questions-only-by-text"></a>Yalnızca soruları Eşleştir, metne göre

Varsayılan olarak, Soru-Cevap Oluşturma sorular ve yanıtlar arasında arama yapar. Yalnızca sorulardan arama yapmak istiyorsanız, yanıt oluşturmak için `RankerType=QuestionOnly` GenerateAnswer ISTEĞININ gönderi gövdesinde öğesini kullanın.

`isTest=false`Kullanarak, veya kullanarak, ile yayımlanan KB veya test KB içinde arama yapabilirsiniz `isTest=true` .

```json
{
  "question": "Hi",
  "top": 30,
  "isTest": true,
  "RankerType":"QuestionOnly"
}
```

## <a name="common-http-errors"></a>Ortak HTTP hataları

|Kod|Açıklama|
|:--|--|
|2xx|Başarılı|
|400|İsteğin parametreleri yanlış, gerekli parametreler eksik, hatalı biçimlendirilmiş veya çok büyük|
|400|İsteğin gövdesi yanlış anlamı, JSON eksik, hatalı biçimlendirilmiş veya çok büyük|
|401|Geçersiz anahtar|
|403|Yasak-doğru izinleriniz yok|
|404|KB yok|
|410|Bu API kullanım dışı ve artık kullanılamıyor|

## <a name="next-steps"></a>Sonraki adımlar

**Yayımla** sayfası, Postman veya kıvrık [bir yanıt oluşturmak](../Quickstarts/get-answer-from-knowledge-base-using-url-tool.md) için bilgi de sağlar.

> [!div class="nextstepaction"]
> [Bilgi bankanız hakkında analizler alma](../how-to/get-analytics-knowledge-base.md)
