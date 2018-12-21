---
title: Konuşma uç noktası gözden geçirme
titleSuffix: Azure Cognitive Services
description: LUIS HTTP uç noktası üzerinden alınan ifadeleri doğrulayarak veya düzelterek LUIS'in emin olmadığı uygulama tahminlerini geliştirin. Bazı konuşmaların amacının, diğerlerinin ise varlığının doğrulanması gerekebilir.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 12/18/2018
ms.author: diberry
ms.openlocfilehash: 622aa49a8dffa933499c243bd723ce2d67a4c251
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2018
ms.locfileid: "53714909"
---
# <a name="tutorial-1-fix-unsure-predictions"></a>1. Öğretici: Emin olunmayan tahminleri düzeltme
Bu öğreticide, LUIS HTTP uç noktası üzerinden alınan ifadeleri doğrulayarak veya düzelterek LUIS'in emin olmadığı uygulama tahminlerini geliştireceksiniz. Bazı konuşmaların amaç, diğerlerinin ise varlık için doğrulanması gerekebilir. Zamanlanmış LUIS bakımınızın normal bir parçası olarak uç noktası konuşmalarını gözden geçirmeniz gerekir. 

Bu gözden geçirme işlemi, LUIS için uygulama alanınızı öğrenmenin bir diğer yoludur. LUIS, gözden geçirme listesinde görünen konuşmaları seçmiştir. Bu liste:

* Uygulamaya özeldir.
* Uygulamanın tahmin doğruluğunu geliştirmek için hazırlanmıştır. 
* Düzenli aralıklarla gözden geçirilmelidir. 

Uç nokta ifadelerini gözden geçirerek, ifadenin tahmin edilen amacını doğrular veya düzeltirsiniz. Tahmin edilmeyen veya yanlış tahmin edilen özel varlıkları da etiketlersiniz. 

**Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Mevcut öğretici uygulamasını kullanma
> * Uç nokta ifadelerini gözden geçirme
> * Tümcecik listesini güncelleştirme
> * Uygulamayı eğitme
> * Uygulama yayımlama
> * LUIS JSON yanıtını görmek için uygulamanın uç noktasını sorgulama

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="use-existing-app"></a>Mevcut uygulamayı kullanma

Son öğreticide oluşturulan **HumanResources** adlı uygulamayla devam edin. 

Önceki öğreticinin HumanResources uygulaması elinizde yoksa aşağıdaki adımları izleyin:

1.  [Uygulama JSON dosyasını](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/documentation-samples/tutorials/custom-domain-sentiment-HumanResources.json) indirip kaydedin.

2. JSON'ı yeni bir uygulamaya içeri aktarın.

3. **Yönet** bölümünde **Sürümler** sekmesinde sürümü kopyalayın ve `review` olarak adlandırın. Kopyalama, özgün sürümünüzü etkilemeden farklı LUIS özelliklerini deneyebileceğiniz ideal bir yol sunar. Sürüm adı, URL rotasının bir parçası olarak kullanıldığından ad bir URL'de geçerli olmayan herhangi bir karakter içeremez.

    Bu öğreticiyi yeni, içeri aktarılan bir uygulama olarak kullanıyorsanız, uygulamayı eğitmeniz, yayımlamanız ve sonra da bir [betikle](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/examples/demo-upload-endpoint-utterances/endpoint.js) veya tarayıcıdaki uç noktadan ifadeleri uç noktaya eklemeniz gerekir. Eklenecek konuşmalar:

   [!code-nodejs[Node.js code showing endpoint utterances to add](~/samples-luis/examples/demo-upload-endpoint-utterances/endpoint.js?range=15-26)]

    Öğretici serisi aracılığıyla uygulamanın tüm sürümlerine sahipseniz, **Review endpoint utterances** (Uç nokta ifadelerini gözden geçir) listesinin sürüme göre değişmediğini görmek sizi şaşırtabilir. Hangi sürümü etkin olarak düzenlemekte olduğunuzdan veya uç noktada uygulamanın hangi sürümünün yayımlandığından bağımsız olarak gözden geçirilecek tek bir konuşma havuzu vardır. 

## <a name="review-endpoint-utterances"></a>Uç nokta ifadelerini gözden geçirme

1. [!INCLUDE [Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

2. Sol gezintiden **Uç nokta ifadelerini gözden geçir**'i seçin. Bu liste **ApplyForJob** amacı için filtrelenmiştir. 

    [ ![Sol gezintideki uç nokta konuşmalarını gözden geçir düğmesinin ekran görüntüsü](./media/luis-tutorial-review-endpoint-utterances/review-endpoint-utterances-with-entity-view.png)](./media/luis-tutorial-review-endpoint-utterances/review-endpoint-utterances-with-entity-view.png#lightbox)

3. Etiketlenmiş varlıkları görmek için **Varlıklar görünümüne** geçin. 
    
    [ ![Varlıklar görünümü iki durumlu düğmesini vurgulandığı Uç nokta ifadelerini gözden geçir ekran görüntüsü](./media/luis-tutorial-review-endpoint-utterances/review-endpoint-utterances-with-token-view.png)](./media/luis-tutorial-review-endpoint-utterances/review-endpoint-utterances-with-token-view.png#lightbox)

    |İfade|Doğru amaç|Eksik varlıklar|
    |:--|:--|:--|
    |Doğal Dil İşleme ile ilgili bir iş arıyorum|GetJobInfo|İş - "Doğal Dil İşleme"|

    Bu ifade doğru amaçta değildir ve puanı %50'den düşüktür. **ApplyForJob** amacının 21 ifadesi varken, **GetJobInformation** amacının yedi ifadesi vardır. Uç nokta ifadesini doğru eşleştirmenin yanı sıra, **GetJobInformation** amacına daha fazla ifade de eklenmelidir. Bu, kendi başınıza tamamlamanız için bir alıştırma olarak bırakılmıştır. **Hiçbiri** amacı dışındaki her amacın kabaca aynı sayıda örnek ifadesi olmalıdır. **Hiçbiri** amacındaki ifade sayısının, uygulamadaki ifadelerin %10'u kadar olması gerekir. 

4. `I'm looking for a job with Natual Language Processing` amacı olarak, **Eşleşmiş amaç** sütununda doğru amaç olan **GetJobInformation**'ı seçin. 

    [ ![İfadeyi amaca eşleyen Uç nokta ifadelerini gözden geçir ekran görüntüsü](./media/luis-tutorial-review-endpoint-utterances/align-intent-1.png)](./media/luis-tutorial-review-endpoint-utterances/align-intent-1.png#lightbox)

5. Aynı ifadede `Natural Language Processing` varlığı keyPhrase'dir. Bunun yerine bu bir **Job** varlığı olmalıdır. `Natural Language Processing` öğesini ve ardından listede **Job** varlığını seçin.

    [ ![İfadede varlığın etiketlendiği Uç nokta ifadelerini gözden geçir ekran görüntüsü](./media/luis-tutorial-review-endpoint-utterances/label-entity.png)](./media/luis-tutorial-review-endpoint-utterances/label-entity.png#lightbox)

6. Aynı satırda, **Eşleşmiş amaca ekle** sütununda yuvarlak için alınmış onay işaretini seçin. 

    [ ![Amaçta ifade eşleşmesini son haline getirme ekran görüntüsü](./media/luis-tutorial-review-endpoint-utterances/align-utterance.png)](./media/luis-tutorial-review-endpoint-utterances/align-utterance.png#lightbox)

    Bu eylem ifadeyi **Uç nokta ifadelerini gözden geçir** ekranından **GetJobInformation** amacına taşır. Uç nokta ifadesi şimdi söz konusu amaç için örnek bir ifade olmuştur. 

7. Bu amaçtaki diğer ifadeleri gözden geçirin, ifadeleri etiketleyin ve **Eşleşmiş amaç** yanlışsa düzeltin.

8. Tüm ifadeler doğru olduğunda, her satırdaki onay kutusunu seçin ve ardından **Seçileni ekle**'yi seçerek ifadeleri doğru eşleştirin. 

    [ ![Eşleşmiş amacın kalan ifadelerine son haline getirme ekran görüntüsü](./media/luis-tutorial-review-endpoint-utterances/finalize-utterance-alignment.png)](./media/luis-tutorial-review-endpoint-utterances/finalize-utterance-alignment.png#lightbox)

9. Artık listede bu ifadeler yer almamalıdır. Başka konuşmalar da görünürse, liste boşalana kadar amaçları düzelterek ve eksik varlıkları etiketleyerek listede çalışmaya devam edin. 

10. Filtre listesinde bir sonraki amacı seçin, sonra ifadeleri düzeltmeye ve varlıkları etiketlemeye devam edin. Her amacın son adımının ifade satırında **Eşleşmiş amaca ekle**'yi seçmek veya her amacın yanındaki kutuyu işaretleyip yukarıdaki tabloda **Seçileni ekle**'yi seçmek olduğunu unutmayın.

    Filtre listesindeki tüm amaçların ve varlıkların boş bir listesi olana kadar devam edin. Bu çok küçük bir uygulamadır. Gözden geçirme işlemi yalnızca birkaç dakika sürer. 

## <a name="update-phrase-list"></a>Tümcecik listesini güncelleştirme
Tüm yeni eklenen iş adlarıyla tümcecik listesini güncel tutun. 

1. Sol gezintiden **Tümcecik listeleri**'ni seçin.

2. **Jobs** tümcecik listesini seçin.

3. `Natural Language Processing` öğesini bir değer olarak ekleyin ve ardından **Kaydet**'i seçin. 

## <a name="train"></a>Eğitim

LUIS eğitilene kadar bu değişiklikten haberdar olmaz. 

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish"></a>Yayımlama

Bu uygulamayı içeri aktardıysanız, **Yaklaşım analizi**'ni seçmelisiniz.

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-intent-and-entities-from-endpoint"></a>Uç noktadan amacı ve varlıkları alma

Düzeltilmiş ifadeye yakın bir ifade deneyin. 

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Adres çubuğundaki URL'nin sonuna gidip `Are there any natural language processing jobs in my department right now?` yazın. Son sorgu dizesi parametresi ifade **s**orgusu olan `q` öğesidir. 

  ```json
  {
    "query": "are there any natural language processing jobs in my department right now?",
    "topScoringIntent": {
      "intent": "GetJobInformation",
      "score": 0.9247605
    },
    "intents": [
      {
        "intent": "GetJobInformation",
        "score": 0.9247605
      },
      {
        "intent": "ApplyForJob",
        "score": 0.129989788
      },
      {
        "intent": "FindForm",
        "score": 0.006438211
      },
      {
        "intent": "EmployeeFeedback",
        "score": 0.00408575451
      },
      {
        "intent": "Utilities.StartOver",
        "score": 0.00194211153
      },
      {
        "intent": "None",
        "score": 0.00166400627
      },
      {
        "intent": "Utilities.Help",
        "score": 0.00118593348
      },
      {
        "intent": "MoveEmployee",
        "score": 0.0007885918
      },
      {
        "intent": "Utilities.Cancel",
        "score": 0.0006373631
      },
      {
        "intent": "Utilities.Stop",
        "score": 0.0005980781
      },
      {
        "intent": "Utilities.Confirm",
        "score": 3.719905E-05
      }
    ],
    "entities": [
      {
        "entity": "right now",
        "type": "builtin.datetimeV2.datetime",
        "startIndex": 64,
        "endIndex": 72,
        "resolution": {
          "values": [
            {
              "timex": "PRESENT_REF",
              "type": "datetime",
              "value": "2018-07-05 15:23:18"
            }
          ]
        }
      },
      {
        "entity": "natural language processing",
        "type": "Job",
        "startIndex": 14,
        "endIndex": 40,
        "score": 0.9869922
      },
      {
        "entity": "natural language processing jobs",
        "type": "builtin.keyPhrase",
        "startIndex": 14,
        "endIndex": 45
      },
      {
        "entity": "department",
        "type": "builtin.keyPhrase",
        "startIndex": 53,
        "endIndex": 62
      }
    ],
    "sentimentAnalysis": {
      "label": "positive",
      "score": 0.8251864
    }
  }
  }
  ```

  Doğru amaç yüksek puanla tahmin edilmiş ve **Job** varlığı `natural language processing` olarak algılanmıştır. 

## <a name="can-reviewing-be-replaced-by-adding-more-utterances"></a>Gözden geçirme işleminin yerini daha fazla ifade ekleme alabilir mi? 
Neden daha fazla örnek ifade eklenmediğini merak ediyor olabilirsiniz. Uç nokta ifadelerini gözden geçirmenin amacı nedir? Gerçek dünyadaki bir LUIS uygulamasında, uç nokta ifadeleri henüz alışmadığınız sözcük seçimi ve yerleşimi olan kullanıcılara aittir. Aynı sözcük seçimini ve yerleşimini daha önce kullandıysanız, özgün tahminin yüzdesi yüksek olabilir. 

## <a name="why-is-the-top-intent-on-the-utterance-list"></a>En üst amaç neden ifade listesinde yer alıyor? 
Bazı uç nokta konuşmalarının gözden geçirme listesinde yüksek bir tahmin puanı olacaktır. Bu ifadeleri yine de gözden geçirmeniz ve doğrulamanız gerekir. Bunların listede olmasının nedeni, bir sonraki yüksek amacın puanının en üst amaç puanına yakın olmasıdır. İlk iki amaç arasında yaklaşık %15 bir fark olmasını istersiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide uç noktada gönderilen LUIS'in emin olmadığı konuşmaları incelediniz. Bu konuşmalar doğrulandıktan ve örnek konuşmalar olarak doğru amaçlara taşındıktan sonra LUIS tahmin etme doğruluğunu artıracaktır.

> [!div class="nextstepaction"]
> [Desenleri kullanmayı öğrenin](luis-tutorial-pattern.md)
