---
title: 'Hızlı Başlangıç: Metin betiğini dönüştürme, C# - Translator Metin Çevirisi'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, başka bir .NET Core ve Translator Text REST API kullanarak bir komut dosyasından (dönüştürme) metin alfabeye öğreneceksiniz. Bu örnekte Japonca, Latin alfabesine dönüştürülmektedir.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: quickstart
ms.date: 11/21/2018
ms.author: erhopf
ms.openlocfilehash: 4987a50f7a689b74062154b8427fd7bec8e2e8a6
ms.sourcegitcommit: 922f7a8b75e9e15a17e904cc941bdfb0f32dc153
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/27/2018
ms.locfileid: "52334319"
---
# <a name="quickstart-transliterate-text-with-the-translator-text-rest-api-c"></a>Hızlı Başlangıç: Translator Metin Çevirisi REST API’si (C#) ile metin çevirme

Bu hızlı başlangıçta, başka bir .NET Core kullanarak bir komut dosyasından (dönüştürme) metin alfabeye öğreneceksiniz (C#) ve Translator metin çevirisi REST API'si. Verilen örnekte Japonca, Latin alfabesine dönüştürülmektedir.

Bu hızlı başlangıç, Translator Metin Çevirisi kaynağına sahip bir [Azure Bilişsel Hizmetler hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) gerektirir. Bir hesabınız yoksa, abonelik anahtarı almak için [ücretsiz deneme sürümünü](https://azure.microsoft.com/try/cognitive-services/) kullanabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

* [.NET SDK](https://www.microsoft.com/net/learn/dotnet/hello-world-tutorial)
* [Json.NET NuGet paketi](https://www.nuget.org/packages/Newtonsoft.Json/)
* [Visual Studio](https://visualstudio.microsoft.com/downloads/), [Visual Studio Code](https://code.visualstudio.com/download), veya en sevdiğiniz metin düzenleyiciyi
* Konuşma hizmeti için bir Azure aboneliği anahtarı

## <a name="create-a-net-core-project"></a>.NET Core projesi oluşturma

Yeni bir komut istemi (veya terminal oturumu) açın ve şu komutları çalıştırın:

```console
dotnet new console -o transliterate-sample
cd transliterate-sample
```

İlk komut şu iki işlemi yapar. Yeni bir .NET konsol uygulaması oluşturur ve adlı bir dizin oluşturur `transliterate-sample`. İkinci komut, proje dizinine değiştirir.

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

## <a name="create-a-function-to-transliterate-text"></a>Metin alfabeye için bir işlev oluşturma

İçinde `Program` sınıfı, çağrılan bir işlev oluşturma `TransliterateText`. Bu sınıf Transliterate kaynak çağırmak için kullanılan kod kapsüller ve sonucu konsola yazdırır.

```csharp
static void TransliterateText()
{
  /*
   * The code for your call to the translation service will be added to this
   * function in the next few sections.
   */
}
```

## <a name="set-the-subscription-key-host-name-and-path"></a>Abonelik anahtarı, konak adı ve yolu ayarlayın

Bu satırları ekleyin `TransliterateText` işlevi. İle birlikte olduğunu fark edeceksiniz `api-version`, iki ek parametreler eklenmiş için `route`. Bu parametreler, giriş dili ve betikleri alfabeye ayarlamak için kullanılır. Bu örnekte, Japonca olarak ayarlanır (`jpan`) ve Latin (`latn`). Abonelik anahtarı değerini güncelleştirdiğinizden emin olun.

```csharp
string host = "https://api.cognitive.microsofttranslator.com";
string route = "/transliterate?api-version=3.0&language=ja&fromScript=jpan&toScript=latn";
string subscriptionKey = "YOUR_SUBSCRIPTION_KEY";
```

Ardından, oluşturma ve alfabeye istediğiniz metni içeren bir JSON nesnesi seri hale getirme ihtiyacımız var. Unutmayın, birden fazla nesne geçirebilirsiniz `body` dizisi.

```csharp
System.Object[] body = new System.Object[] { new { Text = @"こんにちは" } };
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

Son adım çağırmaktır `TransliterateText()` içinde `Main` işlevi. Bulun `static void Main(string[] args)` ve bu satırları ekleyin:

```csharp
TransliterateText();
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
        "script": "latn",
        "text": "konnnichiha"
    }
]
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Abonelik anahtarları gibi örnek uygulamanızın kaynak kodundan olan gizli bilgilerin kaldırdığınızdan emin olun.

## <a name="next-steps"></a>Sonraki adımlar

Çeviri ve dil tanımlayıcı gibi bu ve diğer hızlı başlangıçlardaki örnek kodlarla birlikte GitHub’daki diğer örnek Translator Metin Çevirisi projelerini keşfedin.

> [!div class="nextstepaction"]
> [GitHub’da C# örneklerini keşfedin](https://aka.ms/TranslatorGitHub?type=&language=c%23)

## <a name="see-also"></a>Ayrıca bkz.

* [Metin çevirme](quickstart-csharp-translate.md)
* [Girişe göre dili belirleyin](quickstart-csharp-detect.md)
* [Alternatif çeviriler edinme](quickstart-csharp-dictionary.md)
* [Desteklenen dillerin listesini alma](quickstart-csharp-languages.md)
* [Girişten tümce uzunluklarını belirleme](quickstart-csharp-sentences.md)
