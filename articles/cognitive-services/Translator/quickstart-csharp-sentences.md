---
title: "Hızlı Başlangıç: Cümlesi işlediklerinde, alma C# -Translator metin çevirisi API'si"
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, .NET Core ve Translator Text API kullanarak cümle uzunluğu belirlemek öğreneceksiniz.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: quickstart
ms.date: 11/26/2018
ms.author: erhopf
ms.openlocfilehash: daa1afd2caf5d5bfa2452858501f550e4c7e387b
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55212027"
---
# <a name="quickstart-use-the-translator-text-api-to-determine-sentence-length-using-c"></a>Hızlı Başlangıç: Translator metin çevirisi API'si cümleyi uzunluğunu kullanarak belirlemek için kullanınC#

Bu hızlı başlangıçta, .NET Core ve Translator Text API kullanarak cümle uzunlukları belirlemek öğreneceksiniz.

Bu hızlı başlangıç, Translator Metin Çevirisi kaynağına sahip bir [Azure Bilişsel Hizmetler hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) gerektirir. Bir hesabınız yoksa, abonelik anahtarı almak için [ücretsiz deneme sürümünü](https://azure.microsoft.com/try/cognitive-services/) kullanabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

* [.NET SDK](https://www.microsoft.com/net/learn/dotnet/hello-world-tutorial)
* [Json.NET NuGet paketi](https://www.nuget.org/packages/Newtonsoft.Json/)
* [Visual Studio](https://visualstudio.microsoft.com/downloads/), [Visual Studio Code](https://code.visualstudio.com/download), veya en sevdiğiniz metin düzenleyiciyi
* Translator Metin Çevirisi için Azure abonelik anahtarı

## <a name="create-a-net-core-project"></a>.NET Core projesi oluşturma

Yeni bir komut istemi (veya terminal oturumu) açın ve şu komutları çalıştırın:

```console
dotnet new console -o sentences-sample
cd sentences-sample
```

İlk komut şu iki işlemi yapar. Yeni bir .NET konsol uygulaması oluşturur ve adlı bir dizin oluşturur `sentences-sample`. İkinci komut, proje dizinine değiştirir.

Ardından, Json.Net yüklemeniz gerekir. Projenizin dizinden çalıştırın:

```console
dotnet add package Newtonsoft.Json --version 11.0.2
```

## <a name="add-required-namespaces-to-your-project"></a>Gerekli ad alanları projenize ekleyin.

`dotnet new console` Daha önce çalıştırdığınız komutu tarafından oluşturulan bir projeyi dahil olmak üzere `Program.cs`. Burada uygulama kodunuza giriyorum bu dosyasıdır. Açık `Program.cs`, mevcut using deyimlerinin değiştirin. Bu deyimler, derlemek ve örnek uygulamayı çalıştırmak için gerekli tüm türlerine erişimi olduğundan emin olun.

```csharp
using System;
using System.Net.Http;
using System.Text;
using Newtonsoft.Json;
```

## <a name="create-a-function-to-determine-sentence-length"></a>Tümce uzunluğu belirlemek için bir işlev oluşturma

İçinde `Program` sınıfı, çağrılan bir işlev oluşturma `BreakSentence`. Bu sınıf BreakSentence kaynak çağırmak için kullanılan kod kapsüller ve sonucu konsola yazdırır.

```csharp
static void BreakSentence()
{
  /*
   * The code for your call to the translation service will be added to this
   * function in the next few sections.
   */
}
```

## <a name="set-the-subscription-key-host-name-and-path"></a>Abonelik anahtarı, konak adı ve yolu ayarlayın

Bu satırları ekleyin `BreakSentence` işlevi. İle birlikte olduğunu fark edeceksiniz `api-version`, giriş dili tanımlayabilirsiniz. Bu örnekte, İngilizce olarak.

```csharp
string host = "https://api.cognitive.microsofttranslator.com";
string route = "/breaksentence?api-version=3.0&language=en";
string subscriptionKey = "YOUR_SUBSCRIPTION_KEY";
```

Ardından, oluşturma ve metin içeren bir JSON nesnesi seri hale getirmek gerekir. Unutmayın, birden fazla nesne geçirebilirsiniz `body` dizisi.

```csharp
System.Object[] body = new System.Object[] { new { Text = @"How are you? I am fine. What did you do today?" } };
var requestBody = JsonConvert.SerializeObject(body);
```

## <a name="instantiate-the-client-and-make-a-request"></a>İstemci örneği oluşturun ve bir istek oluşturun

Bu satırlar örneği `HttpClient` ve `HttpRequestMessage`:

```csharp
using (var client = new HttpClient())
using (var request = new HttpRequestMessage())
{
  // In the next few sections you'll add code to construct the request.
}
```

## <a name="construct-the-request-and-print-the-response"></a>Bir isteği oluşturmak ve yanıtı yazdırma

İçinde `HttpRequestMessage` gerekir:

* HTTP metodunu bildir
* İstek URI'si oluşturmak
* İstek gövdesi (seri hale getirilmiş JSON nesnesi) Ekle
* Gerekli üst bilgileri Ekle
* Zaman uyumsuz bir istekte
* Yanıtı yazdırma

Bu kodu ekleyin `HttpRequestMessage`:

```csharp
// Set the method to POST
request.Method = HttpMethod.Post;

// Construct the full URI
request.RequestUri = new Uri(host + route);

// Add the serialized JSON object to your request
request.Content = new StringContent(requestBody, Encoding.UTF8, "application/json");

// Add the authorization header
request.Headers.Add("Ocp-Apim-Subscription-Key", subscriptionKey);

// Send request, get response
var response = client.SendAsync(request).Result;
var jsonResponse = response.Content.ReadAsStringAsync().Result;

// Print the response
Console.WriteLine(jsonResponse);
Console.WriteLine("Press any key to continue.");
```

## <a name="put-it-all-together"></a>Hepsini bir araya getirin

Son adım çağırmaktır `BreakSentence()` içinde `Main` işlevi. Bulun `static void Main(string[] args)` ve bu satırları ekleyin:

```csharp
BreakSentence();
Console.ReadLine();
```

## <a name="run-the-sample-app"></a>Örnek uygulamayı çalıştırma

İşte bu kadar örnek uygulamanızı çalıştırmak hazır olursunuz. Komut satırını (veya terminal oturumu), proje dizinine gidin ve çalıştırın:

```console
dotnet run
```

## <a name="sample-response"></a>Örnek yanıt

```json
[
    {
        "sentLen": [
            13,
            11,
            22
        ]
    }
]
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Abonelik anahtarları gibi örnek uygulamanızın kaynak kodundan olan gizli bilgilerin kaldırdığınızdan emin olun.

## <a name="next-steps"></a>Sonraki adımlar

Başka alfabeye çevirme ve dil tanımlayıcı gibi bu ve diğer hızlı başlangıçlardaki örnek kodlarla birlikte GitHub’daki diğer örnek Translator Metin Çevirisi projelerini keşfedin.

> [!div class="nextstepaction"]
> [GitHub’da C# örneklerini keşfedin](https://aka.ms/TranslatorGitHub?type=&language=c%23)

## <a name="see-also"></a>Ayrıca bkz.

* [Metin çevirme](quickstart-csharp-translate.md)
* [Metni başka dilde yazma](quickstart-csharp-transliterate.md)
* [Girişe göre dili belirleyin](quickstart-csharp-detect.md)
* [Alternatif çeviriler edinme](quickstart-csharp-dictionary.md)
* [Desteklenen dillerin listesini alma](quickstart-csharp-languages.md)
* [Girişten tümce uzunluklarını belirleme](quickstart-csharp-sentences.md)
