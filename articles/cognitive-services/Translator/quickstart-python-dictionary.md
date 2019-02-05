---
title: "Hızlı Başlangıç: İki dilli sözlük, Python - Translator metin çevirisi API'si ile sözcük araması"
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta Python ve Translator Metin Çevirisi API'sini kullanarak belirtilen bir metnin alternatif çevirilerini ve kullanım örneklerini bulmayı öğreneceksiniz.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: quickstart
ms.date: 02/01/2019
ms.author: erhopf
ms.openlocfilehash: 4ca07416c615984b4eb6c7799ff17d9fbe43fa3b
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/04/2019
ms.locfileid: "55691479"
---
# <a name="quickstart-look-up-words-with-bilingual-dictionary-using-python"></a>Hızlı Başlangıç: Python kullanarak iki dilli sözlük ile sözcük arayın

Bu hızlı başlangıçta Python ve Translator Metin Çevirisi API'sini kullanarak belirtilen bir metnin alternatif çevirilerini ve kullanım örneklerini bulmayı öğreneceksiniz.

Bu hızlı başlangıç, Translator Metin Çevirisi kaynağına sahip bir [Azure Bilişsel Hizmetler hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) gerektirir. Bir hesabınız yoksa, abonelik anahtarı almak için [ücretsiz deneme sürümünü](https://azure.microsoft.com/try/cognitive-services/) kullanabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıç şunları gerektirir:

* Python 2.7.x veya 3.x
* Translator Metin Çevirisi için Azure abonelik anahtarı

## <a name="create-a-project-and-import-required-modules"></a>Bir proje oluşturun ve gerekli modülleri içeri aktarın

Favori IDE ortamınızda veya düzenleyicide yeni bir Python projesi oluşturun. Ardından bu kod parçacığını projenizde `dictionary-lookup.py` adlı bir dosyaya kopyalayın.

```python
# -*- coding: utf-8 -*-
import os, requests, uuid, json
```

> [!NOTE]
> Bu modülleri kullanmadıysanız, programınızı çalıştırmadan önce bunları yüklemeniz gerekir. Bu paketleri yüklemek için şunu çalıştırın: `pip install requests uuid`.

İlk açıklama Python yorumlayıcısına UTF-8 kodlaması kullanması gerektiğini bildirir. Ardından bir ortam değişkeninden abonelik anahtarınızı okumak, HTTP isteğini yapılandırmak, benzersiz bir tanımlayıcı oluşturmak ve Translator Metin Çevirisi API'si tarafından döndürülen JSON yanıtını işlemek için gereken modüller içeri aktarılır.

## <a name="set-the-subscription-key-base-url-and-path"></a>Abonelik anahtarını, temel URL’yi ve yolu ayarlayın

Bu örnek, `TRANSLATOR_TEXT_KEY` ortam değişkeninden Translator Metin Çevirisi abonelik anahtarınızı okumaya çalışır. Ortam değişkenlerini bilmiyorsanız, `subscriptionKey` öğesini dize olarak ayarlayabilir ve koşul deyimini açıklama satırı yapabilirsiniz.

Bu kodu projenize kopyalayın:

```python
# Checks to see if the Translator Text subscription key is available
# as an environment variable. If you are setting your subscription key as a
# string, then comment these lines out.
if 'TRANSLATOR_TEXT_KEY' in os.environ:
    subscriptionKey = os.environ['TRANSLATOR_TEXT_KEY']
else:
    print('Environment variable for TRANSLATOR_TEXT_KEY is not set.')
    exit()
# If you want to set your subscription key as a string, uncomment the line
# below and add your subscription key.
#subscriptionKey = 'put_your_key_here'
```

Translator metin çevirisi genel uç noktası olarak ayarlandığından `base_url`. `path`, `dictionary/lookup` rotasını ayarlar ve API sürüm 3’ü kullanmak istediğimizi belirler.

`params` kaynak ve çıkış dillerini ayarlamak için kullanılır. Bu örnekte İngilizce ve İspanyolca dillerini temsil eden `en` ve `es` değerlerini kullanıyoruz.

>[!NOTE]
> Uç noktaları, yollar ve istek parametreleri hakkında daha fazla bilgi için bkz: [Translator Text API 3.0: Sözlük Arama](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-dictionary-lookup).

```python
base_url = 'https://api.cognitive.microsofttranslator.com'
path = '/dictionary/lookup?api-version=3.0'
params = '&from=en&to=es';
constructed_url = base_url + path + params
```

## <a name="add-headers"></a>Üst bilgileri ekleme

Bir isteğin kimliğini doğrulamanın en kolay yolu, abonelik anahtarınızda bir `Ocp-Apim-Subscription-Key` üst bilgisi olarak geçirmektir. Bu örnekte bu yöntem kullanılır. Alternatif olarak, abonelik anahtarınızı bir erişim belirteciyle değiştirebilir ve isteğinizi doğrulamak için erişim belirtecini bir `Authorization` üst bilgisi olarak geçirebilirsiniz. Daha fazla bilgi için bkz. [Kimlik doğrulaması](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication).

Bu kod parçacığını projenize kopyalayın:

```python
headers = {
    'Ocp-Apim-Subscription-Key': subscriptionKey,
    'Content-type': 'application/json',
    'X-ClientTraceId': str(uuid.uuid4())
}
```

## <a name="create-a-request-to-find-alternate-translations"></a>Alternatif çevirileri bulma isteği oluşturma

Çeviri bulmak istediğiniz dizeyi (veya dizeleri) tanımlayın:

```python
# You can pass more than one object in body.
body = [{
    'text': 'Elephants'
}]
```

Ardından, `requests` modülünü kullanarak bir POST isteği oluşturacağız. Üç bağımsız değişken kabul eder: Birleştirilmiş URL, istek üst bilgileri ve istek gövdesi:

```python
request = requests.post(constructed_url, headers=headers, json=body)
response = request.json()
```

## <a name="print-the-response"></a>Yanıtı yazdırma

Son adım sonuçları yazdırmaktır. Bu kod parçacığı anahtarları sıralayarak, girintiyi ayarlayarak ve öğe ve anahtar ayırıcıları bildirerek sonuçların daha iyi görünmesini sağlar.

```python
print(json.dumps(response, sort_keys=True, indent=4, ensure_ascii=False, separators=(',', ': ')))
```

## <a name="put-it-all-together"></a>Hepsini bir araya getirin

İşte bu kadar! Translator Metin Çevirisi API'sini çağıran ve bir JSON yanıtı döndüren basit bir program oluşturdunuz. Artık programınızı çalıştırmak zamanı geldi:

```console
python dictionary-lookup.py
```

Kodunuzu bizimkiyle karşılaştırmak isterseniz, tam örnek kodu [GitHub](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-Python)’da bulabilirsiniz.

## <a name="sample-response"></a>Örnek yanıt

```json
[
    {
        "displaySource": "elephants",
        "normalizedSource": "elephants",
        "translations": [
            {
                "backTranslations": [
                    {
                        "displayText": "elephants",
                        "frequencyCount": 1207,
                        "normalizedText": "elephants",
                        "numExamples": 5
                    }
                ],
                "confidence": 1.0,
                "displayTarget": "elefantes",
                "normalizedTarget": "elefantes",
                "posTag": "NOUN",
                "prefixWord": ""
            }
        ]
    }
]
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Abonelik anahtarınızı programınıza sabit kodladıysanız, bu hızlı başlangıcı tamamladıktan sonra abonelik anahtarını kaldırdığınızdan emin olun.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [GitHub’da Python örneklerini keşfedin](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-Python)

## <a name="see-also"></a>Ayrıca bkz.

Translator metin çevirisi API'si için kullanmayı öğrenin:

* [Metin çevirme](quickstart-python-translate.md)
* [Metni başka dilde yazma](quickstart-python-transliterate.md)
* [Girişe göre dili belirleyin](quickstart-python-detect.md)
* [Desteklenen dillerin listesini alma](quickstart-python-languages.md)
* [Girişten tümce uzunluklarını belirleme](quickstart-python-sentences.md)
