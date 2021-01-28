---
title: include dosyası
description: include dosyası
services: cognitive-services
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.date: 09/01/2020
ms.topic: include
ms.custom: include file, devx-track-js, cog-serv-seo-aug-2020
ms.openlocfilehash: f4b9c84480940889b0278129952bcf2918d9c835
ms.sourcegitcommit: 2f9f306fa5224595fa5f8ec6af498a0df4de08a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2021
ms.locfileid: "98947199"
---
Node.js için Language Understanding (LUSıS) istemci kitaplıklarını kullanın:

* Uygulama oluşturun
* Makine tarafından öğrenilen bir varlık (örneğin, örnek) ile bir amaç ekleyin
* Uygulamayı eğitme ve yayımlama
* Sorgu tahmini çalışma zamanı

[Başvuru belgeleri](/javascript/api/@azure/cognitiveservices-luis-authoring/)  |   [Yazma](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/cognitiveservices/cognitiveservices-luis-authoring) ve [tahmin](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/cognitiveservices/cognitiveservices-luis-runtime) kitaplığı kaynak kodu | [Yazma](https://www.npmjs.com/package/@azure/cognitiveservices-luis-authoring) ve [tahmin](https://www.npmjs.com/package/@azure/cognitiveservices-luis-runtime) NPM | [Örnekler](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/javascript/LUIS/sdk-3x/index.js)

## <a name="prerequisites"></a>Önkoşullar

* [Node.js](https://nodejs.org)
* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/cognitive-services)
* Azure aboneliğiniz olduktan sonra, anahtarınızı ve uç noktanızı almak için Azure portal [bir Language Understanding yazma kaynağı oluşturun](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesLUISAllInOne) . Dağıtım için bekleyin ve **Kaynağa Git** düğmesine tıklayın.
    * Uygulamanızı Language Understanding yazmak üzere bağlamak için [oluşturduğunuz](../luis-how-to-azure-subscription.md#create-luis-resources-in-the-azure-portal) kaynaktaki anahtar ve uç nokta gerekir. Anahtarınızı ve uç noktanızı daha sonra hızlı başlangıçta aşağıdaki koda yapıştırabilirsiniz. Hizmeti denemek için ücretsiz fiyatlandırma katmanını ( `F0` ) kullanabilirsiniz.

## <a name="setting-up"></a>Ayarlanıyor

### <a name="create-a-new-javascript-application"></a>Yeni bir JavaScript uygulaması oluşturma

1. Konsol penceresinde, uygulamanız için yeni bir dizin oluşturun ve bu dizine taşıyın.

    ```console
    mkdir quickstart-sdk && cd quickstart-sdk
    ```

1. Bir dosya oluşturarak dizini JavaScript uygulaması olarak başlatın `package.json` .

    ```console
    npm init -y
    ```

1. JavaScript kodunuz için adlı bir dosya oluşturun `index.js` .

    ```console
    touch index.js
    ```

### <a name="install-the-npm-libraries"></a>NPM kitaplıklarını yüklerken

Uygulama dizini içinde, aşağıdaki komutlarla bağımlılıkları yükleyerek tek seferde bir satır yürütülür:

```console
npm install @azure/ms-rest-js
npm install @azure/cognitiveservices-luis-authoring
npm install @azure/cognitiveservices-luis-runtime
```

`package.json`Şöyle görünmelidir:

```json
{
  "name": "quickstart-sdk",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "@azure/cognitiveservices-luis-authoring": "^4.0.0-preview.3",
    "@azure/cognitiveservices-luis-runtime": "^5.0.0",
    "@azure/ms-rest-js": "^2.0.8"
  }
}
```

## <a name="authoring-object-model"></a>Nesne modeli yazma

Language Understanding (LUSıS) yazma istemcisi, yazma anahtarınızı içeren Azure 'da kimlik doğrulayan bir [Luisauthoringclient](/javascript/api/@azure/cognitiveservices-luis-authoring/luisauthoringclient) nesnesidir.

## <a name="code-examples-for-authoring"></a>Yazma için kod örnekleri

İstemci oluşturulduktan sonra aşağıdaki işlevlere erişmek için bu istemciyi kullanın:

* Uygulamalar- [ekleme](/javascript/api/@azure/cognitiveservices-luis-authoring/apps#add-applicationcreateobject--msrest-requestoptionsbase-), [silme](/javascript/api/@azure/cognitiveservices-luis-authoring/apps#deletemethod-string--models-appsdeletemethodoptionalparams-), [Yayımlama](/javascript/api/@azure/cognitiveservices-luis-authoring/apps#publish-string--applicationpublishobject--msrest-requestoptionsbase-)
* Örnek araslar- [Batch ile ekleme](/javascript/api/@azure/cognitiveservices-luis-authoring/examples#batch-string--string--examplelabelobject----msrest-requestoptionsbase-), [kimliğe göre silme](/javascript/api/@azure/cognitiveservices-luis-authoring/examples#deletemethod-string--string--number--msrest-requestoptionsbase-)
* Özellikler- [tümcecik listelerini](/javascript/api/@azure/cognitiveservices-luis-authoring/features#addphraselist-string--string--phraselistcreateobject--msrest-requestoptionsbase-) yönetme
* Model- [amaçları](/javascript/api/@azure/cognitiveservices-luis-authoring/model#addintent-string--string--modelcreateobject--msrest-requestoptionsbase-) ve varlıkları yönetme
* Desen yönetme [desenleri](/javascript/api/@azure/cognitiveservices-luis-authoring/pattern#addpattern-string--string--patternrulecreateobject--msrest-requestoptionsbase-)
* Eğitim- [uygulamayı](/javascript/api/@azure/cognitiveservices-luis-authoring/train#trainversion-string--string--msrest-requestoptionsbase-) ve yoklama [durumunu](/javascript/api/@azure/cognitiveservices-luis-authoring/train#getstatus-string--string--msrest-requestoptionsbase-) eğitme
* [Sürümler](/javascript/api/@azure/cognitiveservices-luis-authoring/versions) -kopyalama, dışarı aktarma ve silme ile yönetme

## <a name="prediction-object-model"></a>Tahmin nesnesi modeli

Language Understanding (LUSıS) yazma istemcisi, yazma anahtarınızı içeren Azure 'da kimlik doğrulayan bir [Luisauthoringclient](/javascript/api/@azure/cognitiveservices-luis-runtime/luisruntimeclient) nesnesidir.

## <a name="code-examples-for-prediction-runtime"></a>Tahmin çalışma zamanı için kod örnekleri

İstemci oluşturulduktan sonra aşağıdaki işlevlere erişmek için bu istemciyi kullanın:

* [Tahmin](/javascript/api/@azure/cognitiveservices-luis-runtime/predictionoperations#getslotprediction-string--string--predictionrequest--models-predictiongetslotpredictionoptionalparams-) `staging` veya `production` yuva
* [Sürüme göre tahmin](/javascript/api/@azure/cognitiveservices-luis-runtime/predictionoperations#getversionprediction-string--string--predictionrequest--models-predictiongetversionpredictionoptionalparams-)

[!INCLUDE [Bookmark links to same article](sdk-code-examples.md)]

## <a name="add-the-dependencies"></a>Bağımlılıkları ekleme

`index.js`Dosyayı tercih ettiğiniz düzenleyicide veya IDE 'de açın ve ardından aşağıdaki bağımlılıkları ekleyin.

[!code-javascript[Add NPM libraries to code file](~/cognitive-services-quickstart-code/javascript/LUIS/sdk-3x/index.js?name=Dependencies)]

## <a name="add-boilerplate-code"></a>Ortak kod ekleme

1. `quickstart`Yöntemini ve çağrısını ekleyin. Bu yöntem, kalan kodların çoğunu barındırır. Bu yöntem, dosyanın sonunda çağırılır.

    ```javascript
    const quickstart = async () => {

        // add calls here


    }
    quickstart()
        .then(result => console.log("Done"))
        .catch(err => {
            console.log(`Error: ${err}`)
            })
    ```

1. Aksi belirtilmedikçe, kalan kodu hızlı başlangıç yöntemine ekleyin.

## <a name="create-variables-for-the-app"></a>Uygulama için değişkenler oluşturma

İki değişken kümesi oluşturun: değiştirdiğiniz ilk küme, ikinci küme, kod örneğinde göründükleri gibi bırakır. 

1. Yazma anahtarınızı ve kaynak adlarınızı tutmak için değişkenler oluşturun.

    [!code-javascript[Variables you need to change](~/cognitive-services-quickstart-code/javascript/LUIS/sdk-3x/index.js?name=VariablesYouChange)]

1. Uç noktalarınızı, uygulama adınızı, sürümünüzü ve amaç adınızı tutmak için değişkenler oluşturun.

    [!code-javascript[Variables you don't need to change](~/cognitive-services-quickstart-code/javascript/LUIS/sdk-3x/index.js?name=VariablesYouDontNeedToChangeChange)]

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrulama

Anahtarınızla bir [Biliveservicescredentials](/javascript/api/@azure/ms-rest-js/apikeycredentials) nesnesi oluşturun ve bir [Luisauthoringclient](/javascript/api/@azure/cognitiveservices-luis-authoring/luisauthoringclient) nesnesi oluşturmak için bunu uç noktanızla birlikte kullanın.

[!code-javascript[Authenticate the client](~/cognitive-services-quickstart-code/javascript/LUIS/sdk-3x/index.js?name=AuthoringCreateClient)]

## <a name="create-a-luis-app"></a>LUIS uygulaması oluşturma

Bir LUSıS uygulaması, amaçlar, varlıklar ve örnek işleme dahil olmak üzere doğal dil işleme (NLP) modelini içerir.

Uygulamayı oluşturmak için bir [AppsOperation](/javascript/api/@azure/cognitiveservices-luis-authoring/apps) nesnesinin [Add](/javascript/api/@azure/cognitiveservices-luis-authoring/apps) metodunu oluşturun. Ad ve dil kültürü gerekli özelliklerdir.

[!code-javascript[Create a LUIS app](~/cognitive-services-quickstart-code/javascript/LUIS/sdk-3x/index.js?name=AuthoringCreateApplication)]


## <a name="create-intent-for-the-app"></a>Uygulama için amaç oluştur
Bir LUıN uygulamasının modelindeki birincil nesne, amaç ' dır. Amaç, Kullanıcı utterlerinin bir gruplandırması ile _hizalanır._ Bir Kullanıcı soru sorabilir veya bir bot 'tan (veya başka bir istemci uygulamasından) belirli bir _amaçlanan_ yanıtı bulmak için bir ifade oluşturabilir. Bir uçuşmaya örnek olarak, hedef şehirdeki hava durumu hakkında bilgi isteyerek ve müşteri hizmetleri için iletişim bilgilerini soruyor.

[Model.add_intent](/javascript/api/@azure/cognitiveservices-luis-authoring/model#addintent-string--string--modelcreateobject--msrest-requestoptionsbase-) yöntemini benzersiz bir amaç adı ile kullanın, ardından uygulama kimliği, sürüm kimliği ve yeni amaç adını geçirin.

`intentName`Değer, `OrderPizzaIntent` [uygulama Için değişkenlerinizi oluşturma](#create-variables-for-the-app) bölümünde değişkenlerin bir parçası olarak için sabit olarak kodlanır.

[!code-javascript[Create intent for the app](~/cognitive-services-quickstart-code/javascript/LUIS/sdk-3x/index.js?name=AddIntent)]

## <a name="create-entities-for-the-app"></a>Uygulama için varlık oluşturma

Varlıklar gerekli olmasa da, çoğu uygulama içinde bulunur. Varlık, kullanıcının amaç bilgisini almak için gerekli olan bilgileri Kullanıcı aracılığıyla ayıklar. Her biri kendi veri dönüştürme nesnesi (DTO) modelleriyle birlikte, çok sayıda [önceden oluşturulmuş](/javascript/api/@azure/cognitiveservices-luis-authoring/model#addcustomprebuiltentity-string--string--prebuiltdomainmodelcreateobject--msrest-requestoptionsbase-) ve özel varlık türü vardır.  Uygulamanıza eklenecek ortak önceden oluşturulmuş varlıklar [Number](../luis-reference-prebuilt-number.md), [datetimeV2](../luis-reference-prebuilt-datetimev2.md), [geographyV2](../luis-reference-prebuilt-geographyv2.md), [Ordinal](../luis-reference-prebuilt-ordinal.md)içerir.

Varlıkların bir amaç ile işaretlenmediğini bilmek önemlidir. Bunlar, genellikle birçok amaç için uygulanabilir. Yalnızca belirli bir amaç için örnek Kullanıcı utbotları işaretlenir.

Varlıklar için oluşturma yöntemleri [model](/javascript/api/@azure/cognitiveservices-luis-authoring/model) sınıfının bir parçasıdır. Her varlık türünün kendi veri dönüştürme nesnesi (DTO) modeli vardır.

Varlık oluşturma kodu, alt varlıklara uygulanan alt varlıklar ve özelliklerle makine öğrenimi varlığı oluşturur `Quantity` .

:::image type="content" source="../media/quickstart-sdk/machine-learned-entity.png" alt-text="' Miktar ' alt varlıklarına uygulanan alt varlıklar ve özellikler ile makine öğrenimi varlığı, oluşturulan varlığı gösteren portaldan kısmi ekran görüntüsü.":::

[!code-javascript[Create entities for the app](~/cognitive-services-quickstart-code/javascript/LUIS/sdk-3x/index.js?name=AuthoringAddEntities)]

`quickstart`Bu alt varlığa Özellikler atamak için, alt VARLıK kimliğini bulmak için yönteminin üzerine aşağıdaki yöntemi koyun.

[!code-javascript[Find subentity id](~/cognitive-services-quickstart-code/javascript/LUIS/sdk-3x/index.js?name=AuthoringSortModelObject)]

## <a name="add-example-utterance-to-intent"></a>Amaca göre örnek ekleme

Bir utterance 'in amacı ve ayıklama varlıklarını tespit etmek için, uygulama için örneklere örnek gerekir. Örneklerin belirli, tek bir amacı hedeflemesi ve tüm özel varlıkları işaretlemesi gerekir. Önceden oluşturulmuş varlıkların işaretlenmesi gerekmez.

Her örnek için tek bir nesne olan [Examplelabelobject](/javascript/api/@azure/cognitiveservices-luis-authoring/examplelabelobject) nesnelerinin bir listesini oluşturarak örnek bir parametre ekleyin. Her örnek, varlık adı ve varlık değerinin ad/değer çiftleri sözlüğüne sahip tüm varlıkları işaretlemelidir. Varlık değeri, örnek utterine 'nın metninde göründüğünden tam olarak olmalıdır.

:::image type="content" source="../media/quickstart-sdk/labeled-example-machine-learned-entity.png" alt-text="Portalda etiketlenmiş örneği gösteren kısmi ekran görüntüsü. ":::

[Örnekleri çağırın.](/javascript/api/@azure/cognitiveservices-luis-authoring/examples) uygulama kimliği, sürüm kimliği ve örnekle birlikte ekleyin.

[!code-javascript[Add example utterance to intent](~/cognitive-services-quickstart-code/javascript/LUIS/sdk-3x/index.js?name=AuthoringAddLabeledExamples)]

## <a name="train-the-app"></a>Uygulamayı eğitme

Model oluşturulduktan sonra, bu modelin bu sürümü için LUıN uygulamasının eğitililmesi gerekir. Eğitilen bir model [kapsayıcıda](../luis-container-howto.md)kullanılabilir veya hazırlama veya ürün yuvalarında [yayımlanabilir](../luis-how-to-publish-app.md) .

[Eğit. Traınversion](/javascript/api/@azure/cognitiveservices-luis-authoring/train#trainversion-string--string--msrest-requestoptionsbase-) YÖNTEMI uygulama kimliği ve sürüm kimliği gerektirir.

Bu hızlı başlangıç gibi çok küçük bir model, çok hızlı bir şekilde eğitecektir. Üretim düzeyinde uygulamalar için, uygulamanın eğitim ne zaman ne zaman veya ne zaman başarılı olduğunu anlamak için [get_Status](/javascript/api/@azure/cognitiveservices-luis-authoring/train#getstatus-string--string--msrest-requestoptionsbase-) yöntemine bir yoklama çağrısı içermesi gerekir. Yanıt, her bir nesne için ayrı bir durum içeren [Modeltrainingınfo](/javascript/api/@azure/cognitiveservices-luis-authoring/modeltraininginfo) nesnelerinin bir listesidir. Eğitimin tamamlandı olarak kabul edilmesi için tüm nesnelerin başarılı olması gerekir.

[!code-javascript[Train the app](~/cognitive-services-quickstart-code/javascript/LUIS/sdk-3x/index.js?name=TrainAppVersion)]

## <a name="publish-app-to-production-slot"></a>Uygulamayı üretim yuvasına Yayımla

[App. Publish](/javascript/api/@azure/cognitiveservices-luis-authoring/apps#publish-string--applicationpublishobject--msrest-requestoptionsbase-) metodunu kullanarak lusıs uygulamasını yayımlayın. Bu, geçerli eğitilen sürümü uç noktada belirtilen yuvaya yayımlar. İstemci uygulamanız bu uç noktayı, amaç ve varlık ayıklama amacıyla Kullanıcı utbotları göndermek için kullanır.

[!code-javascript[Publish app to production slot](~/cognitive-services-quickstart-code/javascript/LUIS/sdk-3x/index.js?name=PublishVersion)]


## <a name="authenticate-the-prediction-runtime-client"></a>Tahmin çalışma zamanı istemcisinin kimliğini doğrulama

Anahtarınızla msRest. ApiKeyCredentials nesnesini kullanın ve bir Lusıs oluşturmak için bunu uç noktanızla birlikte kullanın [. LUISRuntimeClient](/javascript/api/@azure/cognitiveservices-luis-runtime/luisruntimeclient) nesnesi.

[!INCLUDE [Caution about using authoring key](caution-authoring-key.md)]

[!code-javascript [Authenticate the prediction runtime client](~/cognitive-services-quickstart-code/javascript/LUIS/sdk-3x/index.js?name=PredictionCreateClient)]

## <a name="get-prediction-from-runtime"></a>Çalışma zamanından tahmin al

İstek tahmin çalışma zamanına oluşturmak için aşağıdaki kodu ekleyin. Kullanıcı söylenişi, [predictionRequest](/javascript/api/@azure/cognitiveservices-luis-runtime/predictionrequest) nesnesinin bir parçasıdır.

**[Luisruntimeclient. tahmine. Getslottahmine](/javascript/api/@azure/cognitiveservices-luis-runtime/predictionoperations#getslotprediction-string--string--predictionrequest--models-predictiongetslotpredictionoptionalparams-)** metodu, isteği yerine getirmek IÇIN uygulama kimliği, yuva adı ve tahmin isteği nesnesi gibi çeşitli parametrelere ihtiyaç duyuyor. Verbose gibi diğer seçenekler, tüm hedefleri gösterir ve günlük isteğe bağlıdır.

[!code-javascript [Get prediction from runtime](~/cognitive-services-quickstart-code/javascript/LUIS/sdk-3x/index.js?name=QueryPredictionEndpoint)]

[!INCLUDE [Prediction JSON response](sdk-json.md)]

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulamayı `node index.js` hızlı başlangıç dosyanızdaki komutla çalıştırın.

```console
node index.js
```
