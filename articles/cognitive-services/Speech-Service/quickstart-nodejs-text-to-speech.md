---
title: 'Hızlı Başlangıç: Metin okuma, Node.js - konuşma Hizmetleri Dönüştür'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta nasıl dönüştürme yapılacağını öğreneceksiniz metin okuma Node.js ve metin okuma REST API'sini kullanarak. Bu kılavuzda yer örnek metni konuşma sentezi işaretleme dili (SSML'yi) olarak yapılandırılmıştır. Bu, ses ve konuşma yanıtın dili seçmenize olanak sağlar.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 01/11/2019
ms.author: erhopf
ms.custom: seodec18
ms.openlocfilehash: 5e25de83ddec2884d4bd10ea85f4708d534b8e3f
ms.sourcegitcommit: 94305d8ee91f217ec98039fde2ac4326761fea22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2019
ms.locfileid: "57403252"
---
# <a name="quickstart-convert-text-to-speech-using-nodejs"></a>Hızlı Başlangıç: Dönüştürme metin okuma Node.js kullanma

Bu hızlı başlangıçta nasıl dönüştürme yapılacağını öğreneceksiniz metin okuma Node.js ve metin okuma REST API'sini kullanarak. Bu kılavuzdaki istek gövdesi olarak yapılandırılmış [konuşma sentezi işaretleme dili (SSML'yi)](speech-synthesis-markup.md), ses ve yanıtın dili seçmenize olanak tanıyan.

Bu hızlı başlangıç bir [Azure Bilişsel Hizmetler hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) konuşma hizmeti kaynağı olan. Bir hesabınız yoksa, abonelik anahtarı almak için [ücretsiz deneme sürümünü](get-started.md) kullanabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıç şunları gerektirir:

* [Node 8.12.x veya üzeri](https://nodejs.org/en/)
* [Visual Studio](https://visualstudio.microsoft.com/downloads/), [Visual Studio Code](https://code.visualstudio.com/download), veya en sevdiğiniz metin düzenleyiciyi
* Konuşma hizmeti için bir Azure aboneliği anahtarı. [Ücretsiz edinin! ](get-started.md).

## <a name="create-a-project-and-require-dependencies"></a>Proje oluşturma ve bağımlılıklar gerektirir

Sık kullandığınız IDE veya düzenleyiciyi kullanarak yeni bir Node.js projesi oluşturun. Ardından bu kod parçacığını projenizde `tts.js` adlı bir dosyaya kopyalayın.

```javascript
// Requires request and request-promise for HTTP requests
// e.g. npm install request request-promise
const rp = require('request-promise');
// Requires fs to write synthesized speech to a file
const fs = require('fs');
// Requires readline-sync to read command line inputs
const readline = require('readline-sync');
// Requires xmlbuilder to build the SSML body
const xmlbuilder = require('xmlbuilder');
```

> [!NOTE]
> Bu modülleri kullanmadıysanız, programınızı çalıştırmadan önce bunları yüklemeniz gerekir. Bu paketleri yüklemek için şunu çalıştırın: `npm install request request-promise xmlbuilder readline-sync`.

## <a name="get-an-access-token"></a>Bir erişim belirteci alma

Metin okuma REST API, kimlik doğrulaması için bir erişim belirteci gerektirir. Erişim belirteci almak için bir exchange gereklidir. Bu işlevi kullanarak bir erişim belirteci için konuşma tanıma hizmeti abonelik anahtarınızı birbiriyle değiştirir `issueToken` uç noktası.

Bu örnek, Batı ABD bölgesinde konuşma hizmeti aboneliğiniz olduğunu varsayar. Farklı bir bölgeye kullanıyorsanız, değerini güncelleştirin `uri`. Tam bir listesi için bkz [bölgeleri](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#rest-apis).

Bu kodu projenize kopyalayın:

```javascript
// Gets an access token.
function getAccessToken(subscriptionKey) {
    let options = {
        method: 'POST',
        uri: 'https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken',
        headers: {
            'Ocp-Apim-Subscription-Key': subscriptionKey
        }
    }
    return rp(options);
}
```

> [!NOTE]
> Kimlik doğrulaması hakkında daha fazla bilgi için bkz. [bir erişim belirteci ile kimlik doğrulama](https://docs.microsoft.com/azure/cognitive-services/authentication#authenticate-with-an-authentication-token).

Metin okuma API'si çağrısı ve Sentezlenen konuşma yanıt kaydetmek için işlev sonraki bölümde oluşturacağız.

## <a name="make-a-request-and-save-the-response"></a>Bir istekte bulunmak ve yanıt Kaydet

Burada, metin okuma API'si isteği oluşturun ve konuşma yanıt kaydetmek için yedekleyeceksiniz. Bu örnek, Batı ABD uç nokta kullanmakta olduğunuz varsayılır. Kaynağınız için farklı bir bölgede kayıtlı değilse, güncelleştirdiğinizden emin olun `uri`. Daha fazla bilgi için [konuşma hizmet bölgeleri](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#text-to-speech).

Ardından, istek için gerekli üst bilgileri eklemeniz gerekir. Güncelleştirdiğinizden emin olun `User-Agent` kaynak (Azure Portalı'nda bulunur) ve küme adıyla `X-Microsoft-OutputFormat` tercih edilen ses çıkış için. Çıkış biçimleri tam bir listesi için bkz. [ses çıkarır](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis#audio-outputs).

Ardından, istek gövdesi konuşma sentezi işaretleme dili (SSML'yi) kullanarak oluşturun. Bu örnek, yapısını tanımlar ve kullandığı `text` daha önce oluşturduğunuz giriş.

>[!NOTE]
> Bu örnekte `JessaRUS` ses tipi. Ses/dillerde Microsoft tam bir listesi sağlanmaktadır için bkz: [dil desteği](language-support.md).
> Markanız için benzersiz, tanınan bir ses oluşturmak istiyorsanız bkz [özel ses tipi oluşturma](how-to-customize-voice-font.md).

Son olarak, hizmete istek yapacaksınız. İstek başarılı ise ve 200 durum kodu döndürülür, konuşma yanıt olarak yazılan `TTSOutput.wav`.

```javascript
// Make sure to update User-Agent with the name of your resource.
// You can also change the voice and output formats. See:
// https://docs.microsoft.com/azure/cognitive-services/speech-service/language-support#text-to-speech
function textToSpeech(accessToken, text) {
    // Create the SSML request.
    let xml_body = xmlbuilder.create('speak')
        .att('version', '1.0')
        .att('xml:lang', 'en-us')
        .ele('voice')
        .att('xml:lang', 'en-us')
        .att('name', 'Microsoft Server Speech Text to Speech Voice (en-US, Guy24KRUS)')
        .txt(text)
        .end();
    // Convert the XML into a string to send in the TTS request.
    let body = xml_body.toString();

    let options = {
        method: 'POST',
        baseUrl: 'https://westus.tts.speech.microsoft.com/',
        url: 'cognitiveservices/v1',
        headers: {
            'Authorization': 'Bearer ' + accessToken,
            'cache-control': 'no-cache',
            'User-Agent': 'YOUR_RESOURCE_NAME',
            'X-Microsoft-OutputFormat': 'riff-24khz-16bit-mono-pcm',
            'Content-Type': 'application/ssml+xml'
        },
        body: body
    }

    let request = rp(options)
        .on('response', (response) => {
            if (response.statusCode === 200) {
                request.pipe(fs.createWriteStream('TTSOutput.wav'));
                console.log('\nYour file is ready.\n')
            }
        });
    return request;
}
```

## <a name="put-it-all-together"></a>Hepsini bir araya getirin

Neredeyse bitti. Son adım, bir zaman uyumsuz işlev oluşturmaktır. Bu işlev, abonelik anahtarınızı okuyacaksa bir ortam değişkeni, metin için Komut İstemi'nden bir belirteç almak, isteğin tamamlayın, ardından metinden konuşmaya dönüştürün ve bir .wav ses kaydetmek bekleyin.

Ortam değişkenleriyle bilginiz veya, bir abonelik anahtarı kodlanmış bir dize olarak test etmek tercih ederseniz değiştirin `process.env.SPEECH_SERVICE_KEY` abonelik anahtarınızı dize olarak.

```javascript
// Use async and await to get the token before attempting
// to convert text to speech.
async function main() {
    // Reads subscription key from env variable.
    // You can replace this with a string containing your subscription key. If
    // you prefer not to read from an env variable.
    // e.g. const subscriptionKey = "your_key_here";
    const subscriptionKey = process.env.SPEECH_SERVICE_KEY;
    if (!subscriptionKey) {
        throw new Error('Environment variable for your subscription key is not set.')
    };
    // Prompts the user to input text.
    const text = readline.question('What would you like to convert to speech? ');

    try {
        const accessToken = await getAccessToken(subscriptionKey);
        await textToSpeech(accessToken, text);
    } catch (err) {
        console.log(`Something went wrong: ${err}`);
    }
}

main()
```

## <a name="run-the-sample-app"></a>Örnek uygulamayı çalıştırma

İşte bu kadar metin okuma örnek uygulamanızı çalıştırmak hazır olursunuz. Komut satırını (veya terminal oturumu), proje dizinine gidin ve çalıştırın:

```console
node tts.js
```

İstendiğinde, ne olursa olsun metin okuma dönüştürmek istediğiniz içinde yazın. Başarılı olursa, konuşma dosyanın proje klasöründe bulunur. Yürütün, sık kullanılan media player kullanarak.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Abonelik anahtarları gibi örnek uygulamanızın kaynak kodundan olan gizli bilgilerin kaldırdığınızdan emin olun.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Github'da node.js örneklerini keşfedin](https://github.com/Azure-Samples/Cognitive-Speech-TTS/tree/master/Samples-Http/NodeJS)

## <a name="see-also"></a>Ayrıca bkz.

* [Metin Okuma API başvurusu](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis#text-to-speech-api)
* [Özel ses tipi olarak oluşturma](how-to-customize-voice-font.md)
* [Özel ses oluşturma kayıt ses örnekleri](record-custom-voice-samples.md)
