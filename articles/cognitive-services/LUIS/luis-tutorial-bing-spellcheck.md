---
title: Bing yazım denetimi API'si v7 LUIS sorgulara ekleme | Microsoft Docs
titleSuffix: Azure
description: Bing yazım denetimi API'si V7 LUIS uç nokta sorgulara ekleyerek konuşma doğru yanlış yazılan sözcükleri.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 02/27/2018
ms.author: diberry
ms.openlocfilehash: 87882052ed7faf0a7d2a665d51afb20db7ee839c
ms.sourcegitcommit: 194789f8a678be2ddca5397137005c53b666e51e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39239160"
---
# <a name="correct-misspelled-words-with-bing-spell-check"></a>Bing yazım denetimi ile doğru yanlış yazılan sözcükleri

LUIS uygulamanızı ile tümleştirebilirsiniz [Bing yazım denetimi API'si V7](https://azure.microsoft.com/services/cognitive-services/spell-check/) LUIS puanı ve utterance varlıklarının tahmin önce Konuşma yanlış yazılan sözcükleri düzeltmek için. 

## <a name="create-first-key-for-bing-spell-check-v7"></a>Bing yazım denetimi V7'için ilk tuşu oluşturma
[İlk Bing yazım denetimi API'si v7 anahtarı](https://azure.microsoft.com/try/cognitive-services/?api=spellcheck-api) ücretsizdir. 

![Ücretsiz anahtarı oluşturma](./media/luis-tutorial-bing-spellcheck/free-key.png)

< bir adı "Oluştur-subscription-key" ></a>
## <a name="create-endpoint-key"></a>Uç noktası anahtarı oluşturma
Ücretsiz anahtarınızı süresi bir uç noktası anahtarı oluşturun.

1. [Azure Portal](https://portal.azure.com)’da oturum açın. 

2. Seçin **kaynak Oluştur** sol üst köşedeki.

3. Arama kutusuna `Bing Spell Check API V7` yazın.

    ![Arama için Bing yazım denetimi API'si V7](./media/luis-tutorial-bing-spellcheck/portal-search.png)

4. Hizmeti seçin. 

5. Yasal bildirimi bilgilerini içeren sağ bilgileri bölmesi görünür. Seçin **Oluştur** abonelik oluşturma işlemini başlatmak için. 

6. Sonraki panelinde hizmet ayarlarınızı girin. Hizmet oluşturma işleminin tamamlanması için bekleyin.

    ![Hizmet ayarlarını girin](./media/luis-tutorial-bing-spellcheck/subscription-settings.png)

7. Seçin **tüm kaynakları** altında **Sık Kullanılanlar** sol tarafındaki gezinti başlığı.

8. Yeni hizmeti seçin. Türünün **Bilişsel Hizmetler** konumu ise **genel**. 

9. Ana bölmede seçin **anahtarları** yeni anahtarlarınızı görmek için.

    ![Anahtarları alın](./media/luis-tutorial-bing-spellcheck/grab-keys.png)

10. İlk anahtarınızı kopyalayın. Yalnızca iki anahtarlarından biri gerekir. 

## <a name="using-the-key-in-luis-test-panel"></a>LUIS test panelinde anahtarı kullanma
LUIS anahtarını kullanmak için iki yerde vardır. İlk bulunduğu [test paneli](luis-interactive-test.md#view-bing-spell-check-corrections-in-test-panel). Anahtar LUIS kaydedilmez ancak bunun yerine bir oturumu değişkendir. Bing yazım denetimi API'si v7 hizmet için utterance uygulamak için test paneli her istediğinizde anahtarı ayarlamanız gerekir. Bkz: [yönergeleri](luis-interactive-test.md#view-bing-spell-check-corrections-in-test-panel) anahtarı ayarlama test panelinde.

## <a name="adding-the-key-to-the-endpoint-url"></a>Anahtar uç noktası URL'ye ekleniyor
Uç nokta sorgu yazım düzeltmeleri uygulamak istediğiniz her sorgu için sorgu dizesi parametrelerinde geçirilen anahtar gerekir. LUIS çağıran bir sohbet Robotu olabilir veya LUIS API'si uç noktası doğrudan çağırabilir. Uç nokta nasıl çağrıldığında bağımsız olarak her çağrı için yazım düzeltmeleri düzgün çalışması gereken bilgileri içermelidir.

Uç nokta URL'si doğru geçirilmesi gereken birkaç değer sahiptir. Bing yazım denetimi API'si v7 bunlardan yalnızca başka bir anahtardır. Ayarlamalısınız **yazım denetimi** parametresi true ve değerini ayarlamanız gerekir **bing-yazım-onay-subscription-key** anahtar değeri için:

https://{Region}.api.cognitive.microsoft.com/luis/v2.0/Apps/{appID}?Subscription-Key={luisKey}&spellCheck=**true**& bing-yazım-onay-subscription-key =**{bingKey}**& ayrıntılı = true & timezoneOffset = 0 & q = {utterance} =

## <a name="send-misspelled-utterance-to-luis"></a>LUIS için yazılmış utterance Gönder
1. Bir web tarayıcısında önceki dize Kopyala ve Değiştir `region`, `appId`, `luisKey`, ve `bingKey` kendi değerlerinizle. Yayımlama farklıysa uç nokta bölge kullanmaya dikkat edin [bölge](luis-reference-regions.md).

2. Yanlış yazılmış utterance gibi "ne kadar mountainn?" ekleyin. İngilizce, `mountain`, biriyle `n`, yazarken yazım olduğu. 

3. LUIS için sorgu göndermeyi seçin girin.

4. LUIS yanıt için bir JSON sonucu ile `How far is the mountain?`. Bing yazım denetimi API'si v7 bir yazım hatası algıladığında `query` özgün sorgu LUIS uygulamanın JSON yanıtı içerir ve `alteredQuery` alan LUIS için gönderilen düzeltilmiş sorgu içerir.

```
{
  "query": "How far is the mountainn?",
  "alteredQuery": "How far is the mountain?",
  "topScoringIntent": {
    "intent": "Concierge",
    "score": 0.183866
  },
  "entities": []
}
```

## <a name="ignore-spelling-mistakes"></a>Yazım hatalarını yoksay
Bing yazım denetimi API'si v7 hizmetini kullanmayı istemiyorsanız, yazım hatalarını sahip ve böylece LUIS uygun yazım ve bunun yanı sıra yazım hatası edinebilirsiniz konuşma etiketleyebilirsiniz. Bu seçenek bir yazım denetleyicisi kullanmaktan daha fazla etiketleme çaba gerektirir.

## <a name="publishing-page"></a>Yayımlama Sayfası
[Yayımlama](luis-how-to-publish-app.md) sayfasına sahip bir **etkinleştirme Bing yazım denetleyicisi** onay kutusu. Anahtar oluşturun ve uç nokta URL'sini nasıl değiştiğini anlamak için bir kolaylık budur. Yine de her utterance için düzeltilen yazım sahip olmak için doğru uç noktaya parametreleri kullanmak zorunda. 

> [!div class="nextstepaction"]
> [Örnek konuşma hakkında daha fazla bilgi edinin](luis-how-to-add-example-utterances.md)
