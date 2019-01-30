---
title: 'Hızlı Başlangıç: Bilgi Bankası - REST, Node.js - soru-cevap Oluşturucu yanıt alın'
titlesuffix: Azure Cognitive Services
description: Bu Node.js REST tabanlı hızlı yanıt bir Bilgi Bankası, program aracılığıyla getirmenizde size kılavuzluk eder.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: quickstart
ms.date: 11/19/2018
ms.author: diberry
ms.openlocfilehash: 720d249321d20161b6a1ef19bb522ec6c2fc665e
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55216689"
---
# <a name="get-answers-to-a-question-from-a-knowledge-base-with-nodejs"></a>Node.js ile Bilgi Bankası'tan bir soru sorulara yanıtlar alın

Bu hızlı başlangıçta, program aracılığıyla yayımlanan bir soru-cevap Oluşturucu Bilgi Bankası yanıt getirmenizde size yol gösterir. Soru-Cevap Oluşturma, [veri kaynaklarından](../Concepts/data-sources-supported.md) ve SSS gibi yarı yapılandırılmış içerikten soru ve cevapları otomatik olarak ayıklar. JSON biçiminde soru API istek gövdesinde gönderilir. 

## <a name="prerequisites"></a>Önkoşullar

* [Node.js](https://nodejs.org/en/download/)
* [Visual Studio Code](https://code.visualstudio.com/)
* [Soru-Cevap Oluşturma hizmetine](../How-To/set-up-qnamaker-service-azure.md) sahip olmanız gerekir. Anahtarınızı almak için seçin **anahtarları** altında **kaynak yönetimi** Azure Panonuzda soru-cevap Oluşturucu kaynağınızın. 
* **Yayımlama** sayfa ayarları. Yayımlanan Bilgi Bankası yoksa boş bir Bilgi Bankası oluşturun, sonra da bir Bilgi Bankası içe **ayarları** sayfasında sonra yayımlayın. İndirip kullanabilirsiniz [bu temel Bilgi Bankası](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/knowledge-bases/basic-kb.tsv). 

    Yayımla Sayfası ayarları, posta yönlendirme değeri, konak değeri ve EndpointKey değeri içerir. 

    ![Yayımlama ayarları](../media/qnamaker-quickstart-get-answer/publish-settings.png)

Bu Hızlı Başlangıç için kod [ https://github.com/Azure-Samples/cognitive-services-qnamaker-nodejs ](https://github.com/Azure-Samples/cognitive-services-qnamaker-nodejs/tree/master/documentation-samples/quickstarts/get-answer) depo. 

## <a name="create-a-nodejs-file"></a>Node.js dosyasını oluşturma

Adlı yeni bir dosya oluşturun ve açın VSCode `get-answer.js`. 

## <a name="add-the-required-dependencies"></a>Gerekli bağımlılıkları ekleme

Üst kısmındaki `get-answer.js` gerekli bağımlılıkları projeye ekleyin:

[!code-nodejs[Add the required dependencies](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/get-answer/get-answer.js?range=1-4 "Add the required dependencies")]

## <a name="add-the-required-constants"></a>Gerekli sabitleri ekleme

Ardından, soru-cevap Oluşturucu erişmek için gerekli sabitleri ekleyin. Bu değerler üzerinde **Yayımla** Bilgi Bankası yayımladıktan sonra sayfa. 

[!code-nodejs[Add the required constants](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/get-answer/get-answer.js?range=6-22 "Add the required constants")]

## <a name="add-a-post-request-to-send-question-and-get-an-answer"></a>Soru gönderin ve bir yanıt almak için bir POST isteği Ekle

Aşağıdaki kod, Bilgi Bankası'na soru göndermek için soru-cevap Oluşturucu API'si için bir HTTPS isteği yapar ve yanıtı alır:

[!code-nodejs[Add a POST request to send question to knowledge base](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/get-answer/get-answer.js?range=24-49 "Add a POST request to send question to knowledge base")]

`Authorization` Üst bilginin değeri içeren dize `EndpointKey `. 

## <a name="install-the-dependencies"></a>Bağımlılıkları yükler

Komut satırından bağımlılıklarını yükleyin:

```bash
npm install request request-promise
```

## <a name="run-the-program"></a>Programı çalıştırma

Program komut satırından çalıştırın. Otomatik olarak için soru-cevap Oluşturucu API'si isteği gönderir ve ardından konsol penceresine yazdırır.

Dosyasını çalıştırın:

```bash
node get-answer.js
```

[!INCLUDE [JSON request and response](../../../../includes/cognitive-services-qnamaker-quickstart-get-answer-json.md)]

[!INCLUDE [Clean up files and knowledge base](../../../../includes/cognitive-services-qnamaker-quickstart-cleanup-resources.md)] 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Soru-Cevap Oluşturma (V4) REST API Başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff)
