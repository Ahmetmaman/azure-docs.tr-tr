---
title: 'Hızlı Başlangıç: Cümle uzunluklarını alma, Java - Translator Metin Çevirisi API’si'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, Java ve Translator Text API kullanarak cümle uzunluğu belirlemek öğreneceksiniz.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: quickstart
ms.date: 12/03/2018
ms.author: erhopf
ms.openlocfilehash: 941467e7756faa4fd06220bafbf733f42b43e8d9
ms.sourcegitcommit: 2bb46e5b3bcadc0a21f39072b981a3d357559191
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2018
ms.locfileid: "52888588"
---
# <a name="quickstart-use-the-translator-text-api-to-determine-sentence-length-using-java"></a>Hızlı Başlangıç: Java kullanarak cümle belirlemek için Translator Text API kullanın.

Bu hızlı başlangıçta, Java ve Translator Text API kullanarak cümle uzunlukları belirlemek öğreneceksiniz.

Bu hızlı başlangıç, Translator Metin Çevirisi kaynağına sahip bir [Azure Bilişsel Hizmetler hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) gerektirir. Bir hesabınız yoksa, abonelik anahtarı almak için [ücretsiz deneme sürümünü](https://azure.microsoft.com/try/cognitive-services/) kullanabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

* [JDK 7 veya üzeri](https://www.oracle.com/technetwork/java/javase/downloads/index.html)
* [Gradle](https://gradle.org/install/)
* Translator Metin Çevirisi için Azure abonelik anahtarı

## <a name="initialize-a-project-with-gradle"></a>Gradle ile bir proje başlatın

Bu proje için çalışan bir dizine oluşturarak başlayalım. Komut satırını (veya terminal), şu komutu çalıştırın:

```console
mkdir break-sentence-sample
cd break-sentence-sample
```

Ardından, Gradle proje başlatmak için dağıtacağız. Bu komut temel yapı dosyalarını Gradle için en önemlisi de oluşturacak `build.gradle.kts`, oluşturma ve uygulamanızı yapılandırmak için çalışma zamanında kullanılır. Çalışma dizininizdeki şu komutu çalıştırın:

```console
gradle init --type basic
```

Seçmeniz istendiğinde bir **DSL**seçin **Kotlin**.

## <a name="configure-the-build-file"></a>Derleme dosyası yapılandırma

Bulun `build.gradle.kts` ve en sevdiğiniz IDE ya da metin düzenleyici ile açın. Ardından bu yapı yapılandırması içindeki kopyalayın:

```
plugins {
    java
    application
}
application {
    mainClassName = "BreakSentence"
}
repositories {
    mavenCentral()
}
dependencies {
    compile("com.squareup.okhttp:okhttp:2.5.0")
    compile("com.google.code.gson:gson:2.8.5")
}
```

Bu örnek OkHttp HTTP isteklerini ve Gson işlemek ve JSON ayrıştırmak bağımlılıkları olduğunu not edin. Derleme yapılandırmaları hakkında daha fazla bilgi edinmek istiyorsanız, bkz. [yeni Gradle derleme oluşturma](https://guides.gradle.org/creating-new-gradle-builds/).

## <a name="create-a-java-file"></a>Bir Java dosyası oluşturma

Şimdi örnek uygulamanız için bir klasör oluşturun. Çalışma dizininizden çalıştırın:

```console
mkdir -p src/main/java
```

Ardından, bu klasörde, adlı bir dosya oluşturun `BreakSentence.java`.

## <a name="import-required-libraries"></a>Gerekli kitaplıkları içeri aktarma

Açık `BreakSentence.java` ve bu içeri aktarma deyimleri:

```java
import java.io.*;
import java.net.*;
import java.util.*;
import com.google.gson.*;
import com.squareup.okhttp.*;
```


## <a name="define-variables"></a>Değişkenleri tanımlama

İlk olarak, projeniz için genel bir sınıf oluşturmanız gerekir:

```java
public class BreakSentence {
  // All project code goes here...
}
```

Bu satırları ekleyin `BreakSentence` sınıfı. İle birlikte olduğunu fark edeceksiniz `api-version`, giriş dili tanımlayabilirsiniz. Bu örnekte, İngilizce olarak.

```java
String subscriptionKey = "YOUR_SUBSCRIPTION_KEY";
String url = "https://api.cognitive.microsofttranslator.com/breaksentence?api-version=3.0&language=en";
```

## <a name="create-a-client-and-build-a-request"></a>Bir istemci oluşturup bir istek oluşturun

Bu satırı `BreakSentence` sınıfı örneğini oluşturmak için `OkHttpClient`:

```java
// Instantiates the OkHttpClient.
OkHttpClient client = new OkHttpClient();
```

Ardından, bir POST isteği oluşturalım. Metni Değiştir çekinmeyin. Metin kaçış karakterleri eklenmelidir.

```java
// This function performs a POST request.
public String Post() throws IOException {
    MediaType mediaType = MediaType.parse("application/json");
    RequestBody body = RequestBody.create(mediaType,
            "[{\n\t\"Text\": \"How are you? I am fine. What did you do today?\"\n}]");
    Request request = new Request.Builder()
            .url(url).post(body)
            .addHeader("Ocp-Apim-Subscription-Key", subscriptionKey)
            .addHeader("Content-type", "application/json").build();
    Response response = client.newCall(request).execute();
    return response.body().string();
}
```

## <a name="create-a-function-to-parse-the-response"></a>Yanıtları ayrıştırmak için bir işlev oluşturma

Bu basit işlev ayrıştırır ve Translator metin tanıma hizmetinden JSON yanıtı prettifies.

```java
// This function prettifies the json response.
public static String prettify(String json_text) {
    JsonParser parser = new JsonParser();
    JsonElement json = parser.parse(json_text);
    Gson gson = new GsonBuilder().setPrettyPrinting().create();
    return gson.toJson(json);
}
```

## <a name="put-it-all-together"></a>Hepsini bir araya getirin

Bir istekte bulunmak ve yanıt almak için son adımdır bakın. Bu satırlar, projenize ekleyin:

```java
public static void main(String[] args) {
    try {
        BreakSentence breakSentenceRequest = new BreakSentence();
        String response = breakSentenceRequest.Post();
        System.out.println(prettify(response));
    } catch (Exception e) {
        System.out.println(e);
    }
}
```

## <a name="run-the-sample-app"></a>Örnek uygulamayı çalıştırma

İşte bu kadar örnek uygulamanızı çalıştırmak hazır olursunuz. Komut satırını (veya terminal oturumu), çalışma dizininizin kök dizinine gidin ve çalıştırın:

```console
gradle build
```

## <a name="sample-response"></a>Örnek yanıt

Başarılı bir yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür:

```json
[
  {
    "detectedLanguage": {
      "language": "en",
      "score": 1.0
    },
    "sentLen": [
      13,
      11,
      22
    ]
  }
]
```

## <a name="next-steps"></a>Sonraki adımlar

Çeviri ve başka alfabeye çevirme gibi bu ve diğer hızlı başlangıçlardaki örnek kodlarla birlikte GitHub’daki diğer örnek Translator Metin Çevirisi projelerini keşfedin.

> [!div class="nextstepaction"]
> [GitHub’da Java örneklerini keşfedin](https://aka.ms/TranslatorGitHub?type=&language=java)

## <a name="see-also"></a>Ayrıca bkz.

* [Metin çevirme](quickstart-java-translate.md)
* [Metni başka dilde yazma](quickstart-java-transliterate.md)
* [Girişe göre dili belirleyin](quickstart-java-detect.md)
* [Alternatif çeviriler edinme](quickstart-java-dictionary.md)
* [Desteklenen dillerin listesini alma](quickstart-java-languages.md)
* [Girişten tümce uzunluklarını belirleme](quickstart-java-sentences.md)
